---
title: 'Opciones de almacenamiento para el contenedor de perfiles de FSLogix de Windows Virtual Desktop: Azure'
description: Opciones para almacenar el perfil de FSLogix de Windows Virtual Desktop en Azure Storage.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 71ba24784dee7771acbe19bf0261c7dc02478b24
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79127521"
---
# <a name="storage-options-for-fslogix-profile-containers-in-windows-virtual-desktop"></a>Opciones de almacenamiento para los contenedores de perfiles de FSLogix de Windows Virtual Desktop

Azure ofrece varias soluciones de almacenamiento que puede usar para almacenar el contenedor de perfiles de FSLogix. En este artículo se comparan las soluciones de almacenamiento que Azure ofrece para los contenedores de perfiles de usuario de FSLogix de Windows Virtual Desktop.

Windows Virtual Desktop ofrece contenedores de perfiles de FSLogix como solución recomendada para los perfiles de usuario. FSLogix está diseñado para itinerar perfiles en entornos informáticos remotos, como Windows Virtual Desktop. Al iniciar sesión, este contenedor se adjunta dinámicamente al entorno informático mediante el disco duro virtual (VHD) compatible de forma nativa y el disco duro virtual de Hyper-V (VHDX). El perfil de usuario está disponible inmediatamente y aparece en el sistema exactamente como un perfil de usuario nativo.

En las siguientes tablas se comparan las soluciones de almacenamiento que Azure Storage ofrece para los perfiles de usuario del contenedor de perfiles de FSLogix de Windows Virtual Desktop.

## <a name="azure-platform-details"></a>Detalles de la plataforma Azure

|Características|Archivos de Azure|Azure NetApp Files|Espacios de almacenamiento directo|
|--------|-----------|------------------|---------------------|
|Caso de uso|Uso general|Ultrarrendimiento o migración desde NetApp local|Multiplataforma|
|Servicio de plataforma|Sí, solución nativa de Azure|Sí, solución nativa de Azure|No, administración automática|
|Disponibilidad regional|Todas las regiones|[Seleccionar regiones](https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all)|Todas las regiones|
|Redundancia|Redundancia local/de zona/geográfica|Redundancia local|Redundancia local/de zona/geográfica|
|Niveles y rendimiento|Estándar<br>Premium<br>Hasta un máximo de 100 000 IOPS por recurso compartido con 5 GBps por recurso compartido a aproximadamente 3 ms de latencia|Estándar<br>Premium<br>Ultra<br>Hasta 320 000 (16 000) IOPS con 4,5 GBps por volumen a aproximadamente 1 ms de latencia|HDD estándar: hasta 500 IOPS por disco<br>SSD estándar: hasta 4000 IOPS por disco<br>SSD Premium: hasta 20 000 IOPS por disco<br>Se recomiendan discos Premium para Espacios de almacenamiento directo|
|Capacity|100 TiB por recurso compartido|100 TiB por volumen, hasta 12,5 PiB por suscripción|32 TiB por disco como máximo|
|Infraestructura necesaria|Tamaño mínimo del recurso compartido: 1 GiB|Grupo de capacidad mínima: 4 TiB, tamaño de volumen mínimo: 100 GiB|Dos máquinas virtuales en Azure IaaS (+ testigo de la nube) o al menos tres máquinas virtuales sin él y costos por los discos|
|Protocolos|SMB 2.1/3. y REST|NFSv3, NFSv 4.1 (versión preliminar), SMB 3.x/2.x|NFSv3, NFS v4.1, SMB 3.1|

## <a name="azure-management-details"></a>Detalles de administración de Azure

|Características|Archivos de Azure|Azure NetApp Files|Espacios de almacenamiento directo|
|--------|-----------|------------------|---------------------|
|Acceso|En la nube, locales e híbridos (Azure File Sync)|En la nube, locales (mediante Express Route)|En la nube o en el entorno local|
|Copia de seguridad|Integración de instantáneas de copia de seguridad de Azure|Instantáneas de Azure NetApp Files|Integración de instantáneas de copia de seguridad de Azure|
|Seguridad y cumplimiento normativo|[Todos los certificados admitidos en Azure](https://www.microsoft.com/trustcenter/compliance/complianceofferings)|Norma ISO completada|[Todos los certificados admitidos en Azure](https://www.microsoft.com/trustcenter/compliance/complianceofferings)|
|Integración de Azure Active Directory|[Active Directory nativo y Azure Active Directory Domain Services](https://docs.microsoft.com/azure/storage/files/storage-files-active-directory-overview)|[Azure Active Directory Domain Services y Active Directory nativo](../azure-netapp-files/azure-netapp-files-faqs.md#does-azure-netapp-files-support-azure-active-directory)|Solo compatibilidad con Active Directory nativo o Azure Active Directory Domain Services|

Una vez que haya elegido el método de almacenamiento, consulte [Precios de Windows Virtual Desktop](https://azure.microsoft.com/pricing/details/virtual-desktop/) para información sobre los planes de precios.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los contenedores de perfiles de FSLogix, los discos de perfil de usuario y otras tecnologías de perfil de usuario, consulte la tabla de [Contenedores de perfiles de FSLogix y archivos de Azure ](fslogix-containers-azure-files.md).

Si está listo para crear sus propios contenedores de perfiles de FSLogix, comience con uno de estos tutoriales:

- [Introducción a los contenedores de perfiles de FSLogix en Azure Files en Windows Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Getting-started-with-FSLogix-profile-containers-on-Azure-Files/ba-p/746477)
- [Creación de un contenedor de perfiles de FSLogix para un grupo host mediante Azure NetApp Files](create-fslogix-profile-container.md)
- Las instrucciones de [Implementar un servidor de archivos de escalabilidad horizontal de Espacios de almacenamiento directo de dos nodos para almacenar UPD en Azure](/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment/) también se aplican cuando se usa un contenedor de perfiles de FSLogix en lugar de un disco de perfil de usuario.

También puede empezar desde el principio y configurar su propia solución de Windows Virtual Desktop en [Creación de un inquilino en Windows Virtual Desktop](tenant-setup-azure-active-directory.md).
