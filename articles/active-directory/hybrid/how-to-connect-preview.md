---
title: 'Azure AD Connect: Características en versión preliminar | Microsoft Docs'
description: En este tema se describen más detenidamente las características que se encuentran en la versión preliminar en Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7def733a80aea1be77825bb9069217f5f43e003
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60347811"
---
# <a name="more-details-about-features-in-preview"></a>Más detalles sobre las características de vista previa
En este tema se describe cómo usar las características que actualmente forman parte de la versión preliminar.

## <a name="group-writeback"></a>Escritura diferida de grupos
La opción para la escritura diferida de grupos en las características opcionales le permitirá escribir en diferido **Grupos de Office 365** en un bosque con Exchange instalado. Se trata de un tipo de grupo que siempre se controla en la nube. Si dispone de Exchange local, puede reescribir estos grupos en el entono local, por lo que los usuarios con un buzón de Exchange local pueden enviar y recibir correos electrónicos de ellos.

Puede encontrar más información sobre los grupos de Office 365 y cómo utilizarlos [aquí](https://aka.ms/O365g).

Este grupo de Office 365 se representará como un grupo de distribución en AD DS local. El servidor de Exchange local debe tener la actualización acumulativa 8 de Exchange 2013 (publicada en marzo de 2015) o Exchange 2016 para reconocer este nuevo tipo de grupo.

**Notas durante la vista previa**

* En la vista previa no se rellena actualmente el atributo de libreta de direcciones. Sin este atributo, el grupo no estará visible en la GAL. La manera más fácil de rellenar este atributo es usar el cmdlet de Exchange PowerShell `update-recipient`.
* Solo los bosques con el esquema de Exchange son destinos válidos para los grupos. Si no se ha detectado ningún Exchange, no se podrá habilitar la reescritura de grupos.
* Actualmente solo se admiten las implementaciones de organizaciones de Exchange de un solo bosque. Si tiene más de una organización de Exchange local, necesitará una solución de GALSync local para que estos grupos aparezcan en los demás bosques.
* La característica Reescritura de grupos no controla actualmente los grupos de seguridad ni los grupos de distribución.

> [!NOTE]
> Se necesita una suscripción a Azure AD Premium para la escritura diferida de grupos.
> 
>

## <a name="user-writeback"></a>Reescritura de usuarios
> [!IMPORTANT]
> La característica en vista previa de escritura diferida de usuario, se quitó en la actualización de agosto de 2015 a Azure AD Connect. Si la ha habilitado, debería deshabilitarla.
>
>

## <a name="next-steps"></a>Pasos siguientes
Continúe su [Instalación personalizada de Azure AD Connect](how-to-connect-install-custom.md).

Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
