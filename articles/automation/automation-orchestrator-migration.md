---
title: Migración de Orchestrator a Azure Automation
description: Describe cómo migrar runbooks y paquetes de integración desde System Center Orchestrator a Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 528b961ca07ec86ad502ee1b589772e354564a3d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75421694"
---
# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Migración de Orchestrator a Azure Automation (beta)
Los runbooks de [System Center Orchestrator](https://technet.microsoft.com/library/hh237242.aspx) se basan en actividades provenientes de paquetes de integración escritos específicamente para Orchestrator, mientras que los runbooks de Azure Automation se basan en Windows PowerShell.  [Runbooks gráficos](automation-runbook-types.md#graphical-runbooks) de Azure Automation tienen una apariencia similar a los runbooks de Orchestrator, con sus actividades que representan los cmdlets de PowerShell, runbooks secundarios y recursos.

El [kit de herramientas para migración de System Center Orchestrator](https://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) incluye herramientas que le ayudan a convertir runbooks de Orchestrator a Azure Automation.  Además de convertir los runbooks, debe convertir los paquetes de integración con las actividades usadas por los runbooks en módulos de integración con cmdlets de Windows PowerShell.  

A continuación se muestra el proceso básico para convertir Runbooks de Orchestrator en Runbooks de Azure Automation.  Cada uno de estos pasos se describe en detalle en las secciones que aparecen a continuación.

1. Descargue el [kit de herramientas para migración de System Center Orchestrator](https://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) , que contiene las herramientas y los módulos analizados en este artículo.
2. Importe el [Módulo de actividades estándar](#standard-activities-module) a Azure Automation.  Esto incluye versiones convertidas de las actividades estándar de Orchestrator que los runbooks convertidos pueden usar.
3. Importe los [Módulos de integración de System Center Orchestrator](#system-center-orchestrator-integration-modules) en Azure Automation para los paquetes de integración usados por los runbooks que acceden a System Center.
4. Convierta los paquetes de integración personalizados y de terceros con el [Convertidor de paquetes de integración](#integration-pack-converter) e instálelos en Azure Automation.
5. Convierta los runbooks de Orchestrator con el [Convertidor de runbooks](#runbook-converter) e instálelos en Azure Automation.
6. Cree manualmente los recursos de Orchestrator necesarios en Azure Automation, ya que el Convertidor de runbooks no convierte estos recursos.
7. Configure un [Hybrid Runbook Worker](#hybrid-runbook-worker) en su centro de datos local para ejecutar los runbooks convertidos que accederán a recursos locales.

## <a name="service-management-automation"></a>Service Management Automation
[Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx) (SMA) almacena y ejecuta runbooks en su centro de datos local como Orchestrator y usa los mismos módulos de integración que Azure Automation. El [Convertidor de runbooks](#runbook-converter) convierte los runbooks de Orchestrator en runbooks gráficos, a pesar de que no son compatibles con SMA.  Aún así puede instalar el [módulo de actividades estándar](#standard-activities-module) y los [módulos de integración de System Center Orchestrator](#system-center-orchestrator-integration-modules) en SMA, pero tendrá que [reescribir los runbooks](https://technet.microsoft.com/library/dn469262.aspx) manualmente.

## <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker
Los runbooks de Orchestrator se almacenan en un servidor de base de datos y se ejecutan en servidor de runbooks, ambos en su centro de datos local.  Los runbooks de Azure Automation se almacenan en la nube de Azure y se pueden ejecutar en su centro de datos local, con un [trabajo híbrido de runbook](automation-hybrid-runbook-worker.md).  Es de esta manera que normalmente ejecutará los runbooks convertidos desde Orchestrator, dado que están diseñados para ejecutarse en servidores locales.

## <a name="integration-pack-converter"></a>Convertidor de paquetes de integración
El Convertidor de paquetes de integración convierte paquetes de integración creados con [Orchestrator Integration Tool (OIT)](https://technet.microsoft.com/library/hh855853.aspx) en módulos de integración basados en Windows PowerShell que se pueden importar a Azure Automation o a Service Management Automation.  

Cuando ejecuta el convertir de paquetes de integración, aparece un asistente que le permitirá seleccionar un archivo de paquete de instalación (.oip).  A continuación, el asistente enumera las actividades incluidas en ese paquete de integración y le permite seleccionar las que se migrarán.  Al completar el asistente, se crea un módulo de integración que incluye un cmdlet correspondiente para cada una de las actividades del paquete de integración original.

### <a name="parameters"></a>Parámetros
Las propiedades de una actividad en el paquete de integración se convierten en parámetros del cmdlet correspondiente en el módulo de integración.  Los cmdlets de Windows PowerShell tienen un conjunto de [parámetros comunes](https://technet.microsoft.com/library/hh847884.aspx) que se pueden usar con todos los cmdlets.  Por ejemplo, el parámetro -Verbose hace que un cmdlet genere información detallada acerca de su operación.  Ningún cmdlet puede tener un parámetro con el mismo nombre que un parámetro común.  Si una actividad sí tiene una propiedad con el mismo nombre que un parámetro común, el asistente le solicitará que cambie el nombre del parámetro.

### <a name="monitor-activities"></a>Actividad de supervisión
Los runbooks de supervisión en Orchestrator comienzan con una [actividad de supervisión](https://technet.microsoft.com/library/hh403827.aspx) y se ejecutan de manera constante, mientras esperan que un evento determinado los invoque.  Azure Automation no admite los runbooks de supervisión, por lo que no se convertirá ninguna actividad de supervisión del paquete de integración.  En su lugar, se crea un cmdlet de marcador de posición en el módulo de integración para la actividad de supervisión.  Este cmdlet no tiene ninguna funcionalidad, sino que permite la instalación de cualquier runbook convertir que lo use.  Este runbook no se podrá ejecutar en Azure Automation, pero sí se podrá instalar para que el usuario pueda modificarlo.

### <a name="integration-packs-that-cannot-be-converted"></a>Paquetes de integración que no se pueden convertir
Mediante el Convertidor de paquetes de integración no se pueden convertir los paquetes de integración que no se han creado con OIT. También hay algunos módulos de integración proporcionados por Microsoft que actualmente no se pueden convertir con esta herramienta.  Las versiones convertidas de estos paquetes de integración se [proporcionan para descarga](#system-center-orchestrator-integration-modules) para que se pueden instalar en Azure Automation o Service Management Automation.

## <a name="standard-activities-module"></a>Módulo de actividades estándar
Orchestrator incluye un conjunto de [actividades estándar](https://technet.microsoft.com/library/hh403832.aspx) que no están incluidas en un paquete de integración, pero que son usadas por muchos runbooks.  El módulo Actividades estándar es un módulo de integración que incluye un cmdlet equivalente para cada una de estas actividades.  Debe instalar este módulo de integración en Azure Automation antes de importar cualquier runbook convertido que usa una actividad estándar.

Además de admitir los runbooks convertidos, es posible que un usuario familiarizado con Orchestrator pueda usar los cmdlets del módulo de actividades estándar para crear nuevos runbooks en Azure Automation.  A pesar de que la funcionalidad de todas las actividades estándar se puede realizar con cmdlets, es posible que funcionen de manera diferente.  Los cmdlets del módulo de actividades estándar convertidas funcionarán de la misma manera que sus actividades correspondientes y usarán los mismos parámetros.  Esto puede ayudar al autor del runbook de Orchestrator existente en su transición a runbooks de Azure Automation.

## <a name="system-center-orchestrator-integration-modules"></a>Módulos de integración de System Center Orchestrator
Microsoft proporciona [paquetes de integración](https://technet.microsoft.com/library/hh295851.aspx) para crear runbooks a fin de automatizar componentes de System Center y otros productos.  Algunos de estos paquetes de integración se basan en OIT pero actualmente no se pueden convertir en módulos de integración debido a problemas conocidos.  [Módulos de integración de System Center Orchestrator](https://www.microsoft.com/download/details.aspx?id=49555) incluyen las versiones convertidas de estos paquetes de integración que se pueden importar a Azure Automation y Service Management Automation.  

Mediante la versión RTM de esta herramienta, se publicarán las versiones actualizadas de los paquetes de integración basados en OIT que se pueden convertir con el Convertidor de paquetes de integración.  También se proporcionarán instrucciones para ayudarle a convertir runbooks mediante actividades de los paquetes de integración que no se basan en OIT.

## <a name="runbook-converter"></a>Convertidor de runbooks
Esta herramienta convertirá runbooks de Orchestrator en [runbooks gráficos](automation-runbook-types.md#graphical-runbooks) que se pueden importar a Azure Automation.  

El Convertidor de runbooks se implementa como un módulo de PowerShell con un cmdlet denominado **ConvertFrom-SCORunbook** que realiza la conversión.  Al instalar la herramienta, creará un acceso directo a una sesión de PowerShell que carga el cmdlet.   

A continuación, se proporciona el proceso básico para convertir un runbook de Orchestrator e importarlo a Azure Automation.  En las secciones siguientes se proporcionan más detalles sobre el uso de la herramienta y el trabajo con runbooks convertidos.

1. Exporte uno o varios runbooks desde Orchestrator.
2. Obtenga los módulos de integración para todas las actividades del runbook.
3. Convierta los runbooks de Orchestrator en el archivo exportado.
4. Revise la información de los registros para validar la conversión y determinar las tareas manuales necesarias.
5. Importe los runbooks convertidos a Azure Automation.
6. Cree todos los recursos necesarios en Azure Automation.
7. Edite el runbook en Azure Automation para modificar las actividades necesarias.

### <a name="using-runbook-converter"></a>Uso del Convertidor de runbooks
La sintaxis de **ConvertFrom-SCORunbook** es la siguiente:

```powershell
ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string>
```

* RunbookPath: ruta de acceso al archivo de exportación que contiene los runbooks que se convertirá.
* Module: lista delimitada por comas de los módulos de integración que contienen las actividades de los runbooks.
* OutputFolder: ruta de acceso a la carpeta para crear runbooks gráficos convertidos.

El siguiente comando de ejemplo convierte los runbooks en un archivo de exportación llamado **MyRunbooks.ois_export**.  Estos runbooks usan los paquetes de integración de Active Directory y Data Protection Manager.

```powershell
ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks"
```

### <a name="log-files"></a>Archivos de registro
El Convertidor de runbooks creará los siguientes archivos de registro en la misma ubicación que el runbook convertido.  Si los archivos ya existen, se sobrescribirán con la información de la última conversión.

| Archivo | Contenido |
|:--- |:--- |
| Runbook Converter - Progress.log |Pasos detallados de la conversión, incluidas información de cada actividad convertida correctamente y advertencias para cada actividad no convertida. |
| Runbook Converter - Summary.log |Resumen de la última conversión, incluidas todas las advertencias y realizar tareas de seguimiento que debe realizar, como la creación de una variable necesaria para el runbook convertido. |

### <a name="exporting-runbooks-from-orchestrator"></a>Exportación de runbooks desde Orchestrator
El Convertidor de runbooks funciona con un archivo de exportación de Orchestrator que contiene uno o más runbooks.  Creará un runbook de Azure Automation correspondiente para cada runbook de Orchestrator en el archivo de exportación.  

Para exportar un runbook de Orchestrator, haga clic con el botón derecho en el nombre del runbook en Runbook Designer y seleccione **Exportar**.  Para exportar todos los runbooks a una carpeta, haga clic con el botón derecho en el nombre de la carpeta y seleccione **Exportar**.

### <a name="runbook-activities"></a>Actividades de runbook
El Convertidor de runbooks convierte cada actividad del runbook de Orchestrator a una actividad correspondiente en Azure Automation.  Para las actividades que no se puede convertir, se crea una actividad de marcador de posición en el runbook con el texto de la advertencia.  Después de importar el runbook convertido a Azure Automation, debe reemplazar cualquiera de estas actividades por actividades válidas que realizan la funcionalidad requerida.

Las actividades de Orchestrator en el [Módulo de actividades estándar](#standard-activities-module) se convertirán.  Sin embargo, hay algunas actividades de Orchestrator estándar que no están en este módulo y no se convierten.  Por ejemplo, **Enviar evento de plataforma** no tiene ningún equivalente de Azure Automation, ya que es un evento específico de Orchestrator.

[Actividad de supervisión](https://technet.microsoft.com/library/hh403827.aspx) no se convierten porque no hay ningún equivalente para ellas en Azure Automation.  La excepción son las actividades de supervisión en [paquetes de integración convertidos](#integration-pack-converter) , que se convertirán en la actividad de marcador de posición.

Todas las actividades de un [paquete de integración convertido](#integration-pack-converter) se convertirán si proporciona la ruta de acceso al módulo de integración con el parámetro **modules** .  Para los paquetes de integración de System Center, puede usar los [Módulos de integración de System Center Orchestrator](#system-center-orchestrator-integration-modules).

### <a name="orchestrator-resources"></a>Recursos de Orchestrator
El Convertidor de runbooks solo convierte runbooks, no otros recursos de Orchestrator como contadores, variables o conexiones.  Los contadores no se admiten en Azure Automation.  Las variables y conexiones se admiten, pero es necesario crearlas manualmente.  Los archivos de registro le indicarán si el runbook requiere estos recursos y especifica los recursos correspondientes que se deben crear en Azure Automation para que el runbook convertido funcione correctamente.

Por ejemplo, un runbook puede usar una variable para rellenar un determinado valor en una actividad.  El runbook convertido convertirá la actividad y especificará un recurso de variable en Azure Automation con el mismo nombre que la variable de Orchestrator.  Esto se anotará en el archivo **Runbook Converter - Summary.log** , creado después de la conversión.  Deberá crear manualmente este recurso de variable en Azure Automation antes de usar el runbook.

### <a name="input-parameters"></a>Parámetros de entrada
Los runbooks de Orchestrator aceptan parámetros de entrada con la actividad **Inicializar datos** .  Si el runbook que se convierte incluye esta actividad, se crea un [parámetro de entrada](automation-graphical-authoring-intro.md#runbook-input-and-output) en el runbook de Azure Automation para cada parámetro de la actividad.  Se crea una actividad [Control de Script de flujo de trabajo](automation-graphical-authoring-intro.md#activities) en el runbook convertido que recupera y devuelve cada parámetro.  Las actividades del runbook que usan un parámetro de entrada hacen referencia a la salida de esta actividad.

La razón por la que se usa esta estrategia es para reflejar mejor la funcionalidad del runbook de Orchestrator.  Las actividades de los nuevos runbooks gráficos deben hacer referencia directamente a parámetros de entrada que usan un origen de datos de entrada de Runbook.

### <a name="invoke-runbook-activity"></a>Actividad Invocar Runbook
Los runbooks de Orchestrator inician otros runbooks con la actividad **Invocar Runbook** . Si el runbook que se convierte incluye esta actividad y la opción **Esperar la finalización** está seleccionada, se crea una actividad de runbook para ella en el runbook convertido.  Si la opción **Esperar la finalización** no está seleccionada, se crea una actividad del script de flujo de trabajo que usa **Start-AzureAutomationRunbook** para iniciar el runbook.  Después de importar el runbook convertido a Azure Automation, debe modificar esta actividad con la información especificada en la actividad.

## <a name="related-articles"></a>Artículos relacionados
* [System Center 2012: Orchestrator](https://technet.microsoft.com/library/hh237242.aspx)
* [Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx)
* [Trabajo híbrido de runbook](automation-hybrid-runbook-worker.md)
* [Actividades estándar de Orchestrator](https://technet.microsoft.com/library/hh403832.aspx)
* [Descargue el kit de herramientas para migración de System Center Orchestrator](https://www.microsoft.com/en-us/download/details.aspx?id=47323)

