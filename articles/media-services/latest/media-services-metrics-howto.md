---
title: Visualización de métricas con Azure Monitor
description: En este artículo se muestra cómo supervisar las métricas con los gráficos de Azure Portal y la CLI de Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: d331dc4eb0c6668426e1ab1a01a1dd1dcebe0c85
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795824"
---
# <a name="monitor-media-services-metrics"></a>Supervisión de las métricas de Media Services 

[Azure Monitor](../../azure-monitor/overview.md) le permite supervisar métricas y registros de diagnóstico que le ayudan a conocer el rendimiento de sus aplicaciones. Para obtener una descripción detallada de esta característica y saber por qué se recomienda usar las métricas y los registros de diagnósticos de Azure Media Services, consulte [Supervisar las métricas de Media Services y los registros de diagnóstico](media-services-metrics-diagnostic-logs.md).

Azure Monitor proporciona varias maneras de interactuar con las métricas, como la representación en gráficos en el portal, el acceso a ellas a través de la API REST o consultarlas mediante la CLI. En este artículo se muestra cómo supervisar las métricas con los gráficos de Azure Portal y la CLI de Azure.

## <a name="prerequisites"></a>Requisitos previos

- [Creación de una cuenta de Media Services](create-account-cli-how-to.md)
- Consulte [Supervisar las métricas de Media Services y los registros de diagnóstico](media-services-metrics-diagnostic-logs.md)

## <a name="view-metrics-in-azure-portal"></a>Visualización de métricas en Azure Portal

1. Inicie sesión en Azure Portal en https://portal.azure.com.
1. Vaya a la cuenta de Azure Media Services y seleccione **Métricas**.
1. Haga clic en el cuadro **RECURSO** y seleccione el recurso cuyas métricas desea supervisar. 

    Aparece la ventana **Seleccionar un recurso** a la derecha con la lista con la lista de recursos disponibles. En este caso, verá 

    * &lt;Nombre de la cuenta de Media Services&gt;
    * &lt;Nombre de la cuenta de Media Services&gt;/&lt;nombre de punto de conexión de streaming&gt;
    * &lt;nombre de la cuenta de almacenamiento&gt;

    Seleccione el recurso y pulse **Aplicar**. Para más información acerca de los recursos y métricas que se admiten, consulte [Supervisar las métricas de Media Services y los registros de diagnóstico](media-services-metrics-diagnostic-logs.md).
 
    ![Métricas](media/media-services-metrics/metrics02.png)
    
    > [!NOTE]
    > Para cambiar el recurso cuyas métricas desea supervisar, vuelva a hacer clic en el cuadro **Recurso** y repita este paso.
1. (Opcionalmente) Asigne un nombre al gráfico (para editar el nombre, pulse el icono del lápiz de la parte superior).
1. Agregue las métricas que desea ver.

    ![Métricas](media/media-services-metrics/metrics03.png)
1. Puede anclar el gráfico al panel.

## <a name="view-metrics-with-azure-cli"></a>Visualización de métricas con la CLI de Azure

Para obtener las métricas de "Egress" con la CLI, ejecute el comando de la CLI `az monitor metrics`:

```cli
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

Para obtener otras métricas, sustituya "Egress" por el nombre de la métrica que le interese.

## <a name="see-also"></a>Otras referencias

* [Métricas de Azure Monitor](../../azure-monitor/platform/data-platform.md)
* [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

## <a name="next-steps"></a>Pasos siguientes

[Registros de diagnóstico](media-services-diagnostic-logs-howto.md)
