---
title: 'Ejemplo de script de la CLI de Azure: restringir el tráfico web | Microsoft Docs'
description: 'Ejemplo de script de la CLI de Azure: cree una puerta de enlace de aplicaciones con un firewall de aplicaciones web y un conjunto de escalado de máquinas virtuales que use reglas OWASP para restringir el tráfico.'
services: application-gateway
documentationcenter: networking
author: vhorne
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: e9d2a8a8d47128161c10e81fe70a950f1ed81e1e
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2020
ms.locfileid: "76273376"
---
# <a name="restrict-web-traffic-using-the-azure-cli"></a>Restringir el tráfico web mediante la CLI de Azure

Este script crea una puerta de enlace de aplicaciones con un firewall de aplicaciones web que usa un conjunto de escalado de máquinas virtuales para los servidores back-end. El firewall de aplicaciones web restringe el tráfico web basado en reglas OWASP. Después de ejecutar el script, se puede probar la puerta de enlace de aplicaciones mediante la dirección IP pública.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss-waf.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Ejecute el siguiente comando para quitar el grupo de recursos, la puerta de enlace de aplicaciones y todos los recursos relacionados.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear la implementación. Cada elemento de la tabla incluye vínculos a la documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Crea una red virtual. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Crea una subred en una red virtual. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest) | Crea la dirección IP pública de la puerta de enlace de aplicaciones. |
| [az network application-gateway create](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest) | Crear una puerta de enlace de aplicaciones. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az-vmss-create) | Crea un conjunto de escalado de máquinas virtuales. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Crea una cuenta de almacenamiento. |
| [az monitor diagnostic-settings create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) | Crea una cuenta de almacenamiento. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az-network-public-ip-show) | Obtiene la dirección IP pública de la puerta de enlace de aplicaciones. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure/overview).

Se pueden encontrar más ejemplos de script de la CLI de la puerta de enlace de aplicaciones en la [documentación de Azure Application Gateway](../cli-samples.md).
