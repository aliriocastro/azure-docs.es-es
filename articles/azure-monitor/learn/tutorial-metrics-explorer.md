---
title: 'Tutorial: Creación de un gráfico de métricas en Azure Monitor'
description: Aprenda a crear su primer gráfico de métricas con el Explorador de métricas de Azure.
author: bwren
services: azure-monitor
ms.subservice: metrics
ms.topic: tutorial
ms.date: 12/16/2019
ms.author: bwren
ms.openlocfilehash: be4f026ba20ce1582f5626ec7483740831c25c1a
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77661471"
---
# <a name="tutorial-create-a-metrics-chart-in-azure-monitor"></a>Tutorial: Creación de un gráfico de métricas en Azure Monitor
El explorador de métricas es una característica de Azure Monitor de Azure Portal que permite crear gráficos a partir de valores de métricas, correlacionar visualmente las tendencias e investigar los repuntes o las caídas en los valores de las métricas. Utilice el explorador de métricas para investigar el estado y el uso de los recursos de Azure o para trazar gráficos a partir de métricas personalizadas. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Seleccionar una métrica para la que trazar un gráfico
> * Realizar agregaciones diferentes de valores de métricas
> * Modificar el intervalo de tiempo y la granularidad del gráfico

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesitará un recurso de Azure para supervisarlo. Puede usar cualquier recurso de la suscripción de Azure que admita métricas. Para determinar si un recurso es compatible con las métricas, vaya a su menú en Azure Portal y busque la opción **Métricas** en la sección **Supervisión** del menú.


## <a name="log-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="open-metrics-explorer-and-select-a-scope"></a>Apertura del explorador de métricas y selección de un ámbito
Puede abrir el explorador de métricas desde el menú de Azure Monitor o desde el menú de un recurso en Azure Portal. Las métricas de todos los recursos están disponibles independientemente de la opción que use. 

1. Seleccione **Métricas** en el menú de **Azure Monitor** o en la sección **Supervisión** del menú de un recurso.

1. Seleccione **Ámbito**, que es el recurso del cual desea ver las métricas. El ámbito aparece ya relleno al abrir el explorador de métricas desde el menú de un recurso.

    ![Selección del ámbito](media/tutorial-metrics-explorer/scope-picker.png)

2. Seleccione **Espacio de nombres** si el ámbito tiene más de uno. El espacio de nombres es una forma de organizar las métricas para que se puedan encontrar fácilmente. Por ejemplo, las cuentas de almacenamiento tienen espacios de nombres aparte para almacenar las métricas de File service, Table service, Blob service y Queue service. Muchos tipos de recursos solo tienen un espacio de nombres.

3. Seleccione una métrica de una lista de métricas disponibles para el ámbito y el espacio de nombres seleccionados.

    ![Selección de una métrica](media/tutorial-metrics-explorer/metric-picker.png)

4. Si lo desea, cambie el valor de **Agregación** de la métrica. Esto define cómo se agregarán los valores de las métricas en la granularidad de tiempo del gráfico. Por ejemplo, si la granularidad de tiempo se establece en 15 minutos y la agregación se establece en Suma, cada punto del gráfico será la suma de todos los valores recopilados en cada segmento de 15 minutos.

    ![Gráfico](media/tutorial-metrics-explorer/chart.png)

5. Use el botón **Agregar métrica** y repita estos pasos si quiere ver varias métricas trazadas en el mismo gráfico. Seleccione el botón **Nuevo gráfico** para ver varios gráficos en una vista.

## <a name="select-a-time-range-and-granularity"></a>Selección de un intervalo de tiempo y granularidad

El gráfico muestra las últimas 24 horas de los datos de métricas de forma predeterminada. Use el selector de tiempo para cambiar el **intervalo de tiempo** del gráfico o la **granularidad de tiempo** que define el intervalo de tiempo para cada punto de datos. El gráfico utiliza la agregación especificada para calcular todos los valores muestreados a lo largo de la granularidad de tiempo especificada.

![Panel para cambiar el intervalo de tiempo](media/tutorial-metrics-explorer/time-picker.png)


Use la herramienta del **pincel de tiempo** para investigar un área interesante del gráfico, como un repunte o una caída. Coloque el puntero del mouse al principio del área, haga clic y mantenga presionado el botón izquierdo, arrastre al otro lado del área y suelte el botón. El gráfico acercará el intervalo de tiempo. 

![Pincel de tiempo](media/tutorial-metrics-explorer/time-brush.png)

## <a name="apply-dimension-filters-and-splitting"></a>Aplicar filtros de dimensión y divisiones
Consulte las siguientes referencias para características avanzadas que permiten realizar análisis adicionales de las métricas e identificar posibles valores atípicos en los datos.

- El [filtrado](../platform/metrics-charts.md#apply-filters-to-charts) permite elegir qué valores de dimensión se van a incluir en el gráfico. Por ejemplo, puede que le interese mostrar las solo las solicitudes correctas al representar gráficamente una métrica de *tiempo de respuesta del servidor*. 

- Las [divisiones](../platform/metrics-charts.md#apply-splitting-to-a-chart) controlan si el gráfico va a mostrar líneas independientes por cada valor de una dimensión, o si por el contrario va a agregar los valores en una sola línea. Por ejemplo, puede ver una línea correspondiente a un tiempo de respuesta promedio de todas las instancias de servidor, o ver líneas independientes para cada servidor. 

Vea [ejemplos de gráficos](../platform/metric-chart-samples.md) que tienen filtrado y divisiones aplicados.

## <a name="advanced-chart-settings"></a>Configuración gráfico avanzada

Se puede personalizar el estilo y el título de un gráfico, así como modificar la configuración de gráfico avanzada. Cuando haya terminado de personalizarlo, puede anclarlo a un panel para guardar el trabajo. También se pueden configurar alertas de métricas. Consulte [Características avanzadas del Explorador de métricas de Azure](../platform/metrics-charts.md#lock-boundaries-of-chart-y-axis) para aprender sobre estas y otras características avanzadas del explorador de métricas de Azure Monitor.


## <a name="next-steps"></a>Pasos siguientes
Ahora que ha aprendido a trabajar con métricas en Azure Monitor, aprenda a usar métricas para enviar alertas proactivas.

> [!div class="nextstepaction"]
> [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../platform/alerts-metric.md)

