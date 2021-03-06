---
title: 'Listas de exclusión de Firewall de aplicaciones web en Azure Front Door: Azure Portal'
description: En este artículo se proporciona información sobre la configuración de las listas de exclusión en Azure Front con Azure Portal.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 02/25/2020
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 6ed382e88700e4ecd7f8de20a2c8da7ed3c13566
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2020
ms.locfileid: "77921795"
---
# <a name="web-application-firewall-waf-with-front-door-service-exclusion-lists"></a>Listas de exclusión de Firewall de aplicaciones web (WAF) con Front Door Service 

A veces, el Firewall de aplicaciones web (WAF) podría bloquear una solicitud que quiere permitir para su aplicación. Por ejemplo, Active Directory inserta tokens que se usan para la autenticación. Estos tokens pueden contener caracteres especiales que podrían desencadenar un falso positivo de las reglas de WAF. Las listas de exclusión del WAF le permiten omitir determinados atributos de solicitud de una evaluación del WAF.  Se puede configurar una lista de exclusión con [PowserShell](https://docs.microsoft.com/powershell/module/az.frontdoor/New-AzFrontDoorWafManagedRuleExclusionObject?view=azps-3.5.0), la [CLI de Azure](https://docs.microsoft.com/cli/azure/ext/front-door/network/front-door/waf-policy/managed-rules/exclusion?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-managed-rules-exclusion-add), [API REST](https://docs.microsoft.com/rest/api/frontdoorservice/webapplicationfirewall/policies/createorupdate) o Azure Portal. En el ejemplo siguiente se muestra la configuración de Azure Portal. 
## <a name="configure-exclusion-lists-using-the-azure-portal"></a>Configuración de las listas de exclusión mediante Azure Portal
Es posible acceder a **Administrar las exclusiones** desde el portal de WAF en **Reglas administradas**.

![Administrar las exclusiones](../media/waf-front-door-exclusion/exclusion1.png)
![Administrar exclusion_add](../media/waf-front-door-exclusion/exclusion2.png)

 Una lista de exclusión de ejemplo: ![Administrar exclusion_define](../media/waf-front-door-exclusion/exclusion3.png)

En este ejemplo se excluye el valor en el campo del encabezado *user*. Una solicitud válida puede incluir el campo *user* que contiene una cadena que desencadena una regla de inyección de código SQL. En este caso, puede excluir el parámetro *user* para que la regla de WAF no evalúe nada en el campo.

Los siguientes atributos se pueden agregar a las listas de exclusión por su nombre. Los valores de los campos que se usan no se evalúan con las reglas de WAF, pero sí se evalúan sus nombres. Las listas de exclusión quitan la inspección del valor del campo.

* Nombre del encabezado de la solicitud
* Nombre de la cookie de la solicitud
* Nombre de los argumentos de la cadena de consulta
* Nombre de los argumentos de publicación del cuerpo de la solicitud

Puede especificar una coincidencia exacta con un atributo de cadena de consulta, una cookie, el cuerpo o el encabezado de una solicitud.  Si lo desea, también puede especificar coincidencias parciales. Los operadores siguientes son los criterios de coincidencia admitidos:

- **Equals** (es igual a):  este operador se usa para una coincidencia exacta. Por ejemplo, para seleccionar el encabezado denominado **bearerToken**, utilice el operador de igualdad con el selector definido como **bearerToken**.
- **Starts with** (empieza por): este operador coincide con todos los campos que comienzan por el valor del selector especificado.
- **Ends with** (termina en):  este operador coincide con todos los campos de solicitud que terminan con el valor del selector especificado.
- **Contains** (contiene): este operador coincide con todos los campos de solicitud que contienen el valor del selector especificado.
- **Equals any** (es igual a cualquiera): este operador coincide con todos los campos de solicitud. * es el valor del selector.

Los nombres de encabezado y cookies no distinguen mayúsculas de minúsculas.

Puede aplicar la lista de exclusión a todas las reglas del conjunto de reglas administradas, a las reglas de un grupo de reglas específico o a una sola regla, tal como se muestra en el ejemplo anterior. 

## <a name="next-steps"></a>Pasos siguientes

Después de configurar el WAF, aprenda a ver los registros del WAF. Para más información, consulte el artículo sobre [diagnósticos de Front Door](../afds/waf-front-door-monitor.md).
