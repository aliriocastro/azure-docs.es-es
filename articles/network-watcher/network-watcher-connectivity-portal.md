---
title: 'Solución de problemas de conexiones: Azure Portal'
titleSuffix: Azure Network Watcher
description: Obtenga información sobre cómo usar la funcionalidad de solución de problemas de conexiones de Azure Network Watcher con Azure Portal.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: damendo
ms.openlocfilehash: e405a91b1ea541b4ed3328fdb3bf80ca82731c93
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842859"
---
# <a name="troubleshoot-connections-with-azure-network-watcher-using-the-azure-portal"></a>Solución de problemas de conexiones con Azure Network Watcher mediante Azure Portal

> [!div class="op_single_selector"]
> - [Portal](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [CLI de Azure](network-watcher-connectivity-cli.md)
> - [API de REST de Azure](network-watcher-connectivity-rest.md)

Obtenga información sobre cómo usar la solución de problemas de conexiones para comprobar si se puede establecer una conexión TCP directa desde una máquina virtual a un punto de conexión determinado.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se da por hecho que tiene los siguientes recursos:

* Una instancia de Network Watcher en la región en la que desea solucionar los problemas de una conexión.
* Las máquinas virtuales con las cuales solucionar los problemas de las conexiones.

> [!IMPORTANT]
> La solución de problemas de conexión requiere que la VM desde la que soluciona el problema tenga la extensión de VM `AzureNetworkWatcherExtension` instalada. Para instalar la extensión en una máquina virtual Windows, consulte [Extensión de máquina virtual del agente de Azure Network Watcher para Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json), y en una máquina virtual con Linux, consulte [Extensión de máquina virtual del agente de Azure Network Watcher para Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). La extensión no es necesaria en el punto de conexión de destino.

## <a name="check-connectivity-to-a-virtual-machine"></a>Comprobación de la conectividad a una máquina virtual

En este ejemplo se comprueba la conectividad a una máquina virtual de destino a través del puerto 80.

Vaya a Network Watcher y haga clic en **Solución de problemas de conexiones**. Seleccione la máquina virtual para la que desea comprobar la conectividad. En la sección **Destino** elija **Seleccione una máquina virtual** y elija la máquina virtual correcta y el puerto para probar.

Una vez que haga clic en **Comprobar**, se comprueba la conectividad entre las máquinas virtuales en el puerto especificado. En el ejemplo, la máquina virtual de destino es inaccesible y se muestra una lista de saltos.

![Resultados de la comprobación de la conectividad de una máquina virtual][1]

## <a name="check-remote-endpoint-connectivity"></a>Comprobación de la conectividad de un punto de conexión remoto

Para comprobar la conectividad y la latencia de un punto de conexión remoto, elija el botón de radio **Especificar manualmente** en la sección **Destino**, especifique la dirección URL y el puerto y haga clic en **Comprobar**.  Se utiliza para puntos de conexión remotos como sitios web y puntos de conexión de almacenamiento.

![Resultados de la comprobación de conectividad para un sitio web][2]

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de cómo automatizar capturas de paquetes con las alertas de máquina virtual, consulte cómo [crear una captura de paquetes desencadenada por alertas](network-watcher-alert-triggered-packet-capture.md).

Para comprobar si se permite cierto tráfico hacia o desde la máquina virtual, vea cómo [consultar la Comprobación del flujo de IP](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png