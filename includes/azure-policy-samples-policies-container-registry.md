---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/02/2020
ms.author: dacoulte
ms.openlocfilehash: 57cbd8d208422dfc124bed08109250e7bdd360d5
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2020
ms.locfileid: "78262333"
---
|Nombre |Descripción |Efectos |Versión |GitHub |
|---|---|---|---|---|
|[Las instancias de Container Registry se deben cifrar con una clave administrada por el cliente (CMK)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Auditar las instancias de Container Registry que no tienen habilitado el cifrado con claves administradas por el cliente (CMK). Para más información sobre el cifrado de CMK, visite https://aka.ms/acr/CMK. |Audit, Disabled |1.0.0-preview |[Vínculo](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json)
|[Las instancias de Container Registry no deben permitir el acceso de red sin restricciones](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |Permite auditar las instancias de Container Registry que no tienen ninguna regla de red (IP o VNET) configurada y permite todo el acceso de red de forma predeterminada. Las instancias de Container Registry con al menos una regla de IP o firewall o una red virtual configurada se considerarán compatibles. Para más información acerca de las reglas de red de Container Registry, visite: https://aka.ms/acr/vnet. |Audit, Disabled |1.0.0-preview |[Vínculo](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json)
