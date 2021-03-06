---
title: Recopilar datos en los modelos de producción
titleSuffix: Azure Machine Learning
description: Aprenda a recopilar datos de modelos de entrada de Azure Machine Learning en Azure Blob Storage.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: laobri
ms.author: copeters
author: lostmygithubaccount
ms.date: 11/12/2019
ms.custom: seodec18
ms.openlocfilehash: 3c481a2e12d83e865025cd90e59e0eba572ad9a5
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75771400"
---
# <a name="collect-data-for-models-in-production"></a>Recopilar datos de modelos en producción

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

>[!IMPORTANT]
> El SDK de supervisión de Azure Machine Learning se retirará pronto. Sin embargo, aún es adecuado para los desarrolladores que usan actualmente el SDK para supervisar el desfase de datos en los modelos. No obstante, para los nuevos clientes, se recomienda usar la [supervisión de datos simplificada con Application Insights](https://docs.microsoft.com/azure/machine-learning/how-to-enable-app-insights).

En este artículo se muestra cómo recopilar datos de modelos de entrada de Azure Machine Learning. También se muestra cómo implementar los datos de entrada en un clúster de Azure Kubernetes Service (AKS) y cómo almacenar los datos de salida en Azure Blob Storage.

Una vez que la colección está habilitada, los datos que recopile le ayudarán a:

* [Supervisar el desfase de datos](how-to-monitor-data-drift.md) a medida que los datos de producción entran en el modelo.

* Tomar mejores decisiones sobre cuándo volver a entrenar u optimizar el modelo.

* Volver a entrenar el modelo con los datos recopilados.

## <a name="what-is-collected-and-where-it-goes"></a>¿Qué información se recopila y a dónde va?

Se pueden recopilar los siguientes datos:

* Datos de entrada de modelo de los servicios web implementados en un clúster de AKS. El audio de voz, las imágenes y el vídeo *no* se recopilan.
  
* Predicciones de modelo con datos de entrada de producción.

>[!NOTE]
> La agregación y el cálculo previos en estos datos no forman parte actualmente del servicio de recopilación.

La salida se guarda en el almacenamiento de blobs. Dado que los datos se agregan al almacenamiento de blobs, puede elegir su herramienta favorita para ejecutar el análisis.

La ruta de acceso a los datos de salida en el blob sigue esta sintaxis:

```
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<designation>/<year>/<month>/<day>/data.csv
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```

>[!NOTE]
> En las versiones del SDK de Azure Machine Learning para Python anteriores a la versión 0.1.0 A16, el argumento `designation` se denomina `identifier`. Si desarrolló el código con una versión anterior, debe actualizarlo en consecuencia.

## <a name="prerequisites"></a>Prerequisites

- Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLFree) antes de empezar.

- Un área de trabajo de Azure Machine Learning, un directorio local que contenga los scripts y tener instalado el SDK de Azure Machine Learning para Python. Para información sobre cómo instalarlos, consulte [Configurar un entorno de desarrollo para Azure Machine Learning](how-to-configure-environment.md).

- Necesita un modelo de aprendizaje automático entrenado para implementarlo en AKS. Si no tiene uno, consulte el tutorial sobre el [entrenamiento de los modelos de clasificación de imágenes](tutorial-train-models-with-aml.md).

- Necesita un clúster de AKS. Para información sobre cómo crear uno e implementarlo, consulte [Implementación de modelos con Azure Machine Learning](how-to-deploy-and-where.md).

- [Configure el entorno](how-to-configure-environment.md) e instale el [SDK de supervisión de Azure Machine Learning](https://aka.ms/aml-monitoring-sdk).

## <a name="enable-data-collection"></a>Habilitación de recolección de datos

Puede habilitar la recopilación de datos independientemente del modelo que implemente mediante Azure Machine Learning u otras herramientas.

Para habilitar la recopilación de datos, debe hacer lo siguiente:

1. Abrir el archivo de puntuación.

1. Agregue el [código siguiente](https://aka.ms/aml-monitoring-sdk) en la parte superior del archivo:

   ```python 
   from azureml.monitoring import ModelDataCollector
   ```

1. Declare las variables de recopilación de datos en su función `init`:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector("best_model", designation="inputs", feature_names=["feat1", "feat2", "feat3". "feat4", "feat5", "feat6"])
    prediction_dc = ModelDataCollector("best_model", designation="predictions", feature_names=["prediction1", "prediction2"])
    ```

    *CorrelationId* es un parámetro opcional. No es necesario usarlo si el modelo no lo necesita. El uso de *CorrelationId* le ayuda en la asignación con otros datos, como *LoanNumber* o *CustomerId*.
    
    El parámetro *Identifier* se usa posteriormente para compilar la estructura de carpetas en el blob. Se puede usar para diferenciar los datos sin procesar de los datos procesados.

1. Agregue las líneas de código siguientes en la función `run(input_df)`:

    ```python
    data = np.array(data)
    result = model.predict(data)
    inputs_dc.collect(data) #this call is saving our input data into Azure Blob
    prediction_dc.collect(result) #this call is saving our input data into Azure Blob
    ```

1. La recopilación de datos *no* se establece automáticamente en **true** cuando se implementa un servicio en AKS. Actualice el archivo de configuración, como en el ejemplo siguiente:

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True)
    ```

    También puede habilitar Application Insights para la supervisión de servicios si modifica esta configuración:

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True, enable_app_insights=True)
    ```

1. Para crear una imagen e implementar el modelo de aprendizaje automático, consulte [Implementación de modelos con Azure Machine Learning](how-to-deploy-and-where.md).

Si ya tiene un servicio con las dependencias instaladas en el archivo de entorno y el archivo de puntuación, habilite la recopilación de datos mediante estos pasos:

1. Vaya a [Azure Machine Learning](https://ml.azure.com).

1. Abra el área de trabajo.

1. Seleccione **Deployments** > **Select service** > **Edit** (Implementaciones > Seleccionar servicio > Editar).

   ![Edición del servicio](././media/how-to-enable-data-collection/EditService.PNG)

1. En **Configuración avanzada**, seleccione **Habilitar recopilación de datos de modelos**.

    [![Selección de la recopilación de datos](./media/how-to-enable-data-collection/CheckDataCollection.png)](././media/how-to-enable-data-collection/CheckDataCollection.png#lightbox)

   También puede elegir **Enable AppInsights diagnostics** (Habilitar diagnósticos de AppInsights) para realizar un seguimiento del estado de su servicio.

1. Seleccione **Update** (Actualizar) para aplicar el cambio.

## <a name="disable-data-collection"></a>Deshabilitar la recopilación de datos

Puede dejar de recopilar datos en cualquier momento. Use código Python o Azure Machine Learning Studio para deshabilitar la recopilación de datos.

### <a name="option-1---disable-data-collection-in-azure-machine-learning"></a>Opción 1: Deshabilitar la recopilación de datos en Azure Machine Learning

1. Inicie sesión en [Azure Machine Learning](https://ml.azure.com).

1. Abra el área de trabajo.

1. Seleccione **Deployments** > **Select service** > **Edit** (Implementaciones > Seleccionar servicio > Editar).

   [![Selección del icono de edición](././media/how-to-enable-data-collection/EditService.PNG)](./././media/how-to-enable-data-collection/EditService.PNG#lightbox)

1. En **Advanced Settings** (Configuración avanzada), desactive **Enable Model data collection** (Habilitar recopilación de datos de modelo).

    [![Desactivación de la casilla de recopilación de datos](./media/how-to-enable-data-collection/UncheckDataCollection.png)](././media/how-to-enable-data-collection/UncheckDataCollection.png#lightbox)

1. Seleccione **Actualizar** para aplicar el cambio.

También puede acceder a esta configuración en el área de trabajo de [Azure Machine Learning](https://ml.azure.com).

### <a name="option-2---use-python-to-disable-data-collection"></a>Opción 2: Usar Python para deshabilitar la recopilación de datos

  ```python 
  ## replace <service_name> with the name of the web service
  <service_name>.update(collect_model_data=False)
  ```

## <a name="validate-and-analyze-your-data"></a>Validación y análisis de los datos

Puede elegir la herramienta que prefiera para analizar los datos recopilados en el almacenamiento de blobs.

### <a name="quickly-access-your-blob-data"></a>Acceso rápido a los datos del blob

1. Inicie sesión en [Azure Machine Learning](https://ml.azure.com).

1. Abra el área de trabajo.

1. Seleccione **Storage**.

    [![Selección de la opción de almacenamiento](./media/how-to-enable-data-collection/StorageLocation.png)](././media/how-to-enable-data-collection/StorageLocation.png#lightbox)

1. Siga la ruta de acceso a los datos de salida del blob con esta sintaxis:

   ```
   /modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<designation>/<year>/<month>/<day>/data.csv
   # example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
   ```

### <a name="analyze-model-data-using-power-bi"></a>Análisis de los datos del modelo mediante Power BI

1. Descargue y abra [Power BI Desktop](https://www.powerbi.com).

1. Seleccione **Obtener datos** y, luego, [**Azure Blob Storage**](https://docs.microsoft.com/power-bi/desktop-data-sources).

    [![Configuración de blobs en Power BI](./media/how-to-enable-data-collection/PBIBlob.png)](././media/how-to-enable-data-collection/PBIBlob.png#lightbox)

1. Agregue el nombre de la cuenta de almacenamiento y escriba la clave de almacenamiento. Para encontrar esta información, seleccione **Configuración** > **Claves de acceso** en el blob.

1. Seleccione el contenedor **datos del modelo** y elija **Editar**.

    [![Navegador de Power BI](./media/how-to-enable-data-collection/pbiNavigator.png)](././media/how-to-enable-data-collection/pbiNavigator.png#lightbox)

1. En el editor de consultas, haga clic en la columna **Nombre** y agregue su cuenta de almacenamiento.

1. Especifique la ruta de acceso del modelo en el filtro. Si quiere mirar únicamente en los archivos de un determinado año o mes, expanda la ruta de acceso de filtro. Por ejemplo, para mirar solo en los datos de marzo, use esta ruta de filtro:

   /modeldata/\<subscriptionid>/\<resourcegroupname>/\<workspacename>/\<webservicename>/\<modelname>/\<modelversion>/\<designation>/\<year>/3

1. Filtre los datos pertinentes por los valores de **Nombre**. Si almacenó predicciones y entradas, tendrá que crear una consulta para cada una.

1. Seleccione las flechas dobles hacia abajo junto al encabezado de columna **Contenido** para combinar los archivos.

    [![Contenido de Power BI](./media/how-to-enable-data-collection/pbiContent.png)](././media/how-to-enable-data-collection/pbiContent.png#lightbox)

1. Seleccione **Aceptar**. Los datos se cargan previamente.

    [![Combinación de archivos en Power BI](./media/how-to-enable-data-collection/pbiCombine.png)](././media/how-to-enable-data-collection/pbiCombine.png#lightbox)

1. Seleccione **Close and Apply** (Cerrar y aplicar).

1. Si ha agregado entradas y predicciones, las tablas se ordenan automáticamente por los valores **RequestId**.

1. Comience a crear los informes personalizados con los datos del modelo.

### <a name="analyze-model-data-using-azure-databricks"></a>Análisis de datos del modelo mediante Azure Databricks

1. Cree un [área de trabajo de Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

1. Acceda al área de trabajo de Databricks.

1. En ese área, seleccione **Upload Data** (Cargar datos).

    [![Selección de la opción de carga de datos de Databricks](./media/how-to-enable-data-collection/dbupload.png)](././media/how-to-enable-data-collection/dbupload.png#lightbox)

1. Seleccione **Create New Table** (Crear nueva tabla) y seleccione **Other Data Sources** > **Azure Blob Storage** > **Create Table in Notebook** (Otros orígenes de datos > Azure Blob Storage > Crear tabla en Notebook).

    [![Creación de tablas de Databricks](./media/how-to-enable-data-collection/dbtable.PNG)](././media/how-to-enable-data-collection/dbtable.PNG#lightbox)

1. Actualice la ubicación de los datos. Este es un ejemplo:

    ```
    file_location = "wasbs://mycontainer@storageaccountname.blob.core.windows.net/modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/*/*/data.csv" 
    file_type = "csv"
    ```

    [![Configuración de Databricks](./media/how-to-enable-data-collection/dbsetup.png)](././media/how-to-enable-data-collection/dbsetup.png#lightbox)

1. Siga los pasos de la plantilla para ver y analizar los datos.
