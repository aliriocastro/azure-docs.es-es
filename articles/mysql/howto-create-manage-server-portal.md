---
title: Administración de servidores mediante Azure Portal en Azure Database for MySQL
description: Aprenda a administrar un servidor de Azure Database for MySQL desde Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 3fed615a767fee2fff1700f996b2103f411db825
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74973482"
---
# <a name="manage-an-azure-database-for-mysql-server-using-the-azure-portal"></a>Administración de un servidor de Azure Database for MySQL mediante Azure Portal
En este artículo se muestra cómo administrar los servidores de Azure Database for MySQL. Entre las tareas de administración se incluyen el escalado de proceso y almacenamiento, el restablecimiento de contraseñas de administración y la visualización de detalles del servidor.

## <a name="sign-in"></a>Iniciar sesión
Inicie sesión en el [Azure Portal](https://portal.azure.com).

## <a name="create-a-server"></a>Creación de un servidor
Visite el [inicio rápido](quickstart-create-mysql-server-database-using-azure-portal.md) para obtener información sobre cómo crear un servidor de Azure Database for MySQL y empezar a trabajar con él.

## <a name="scale-compute-and-storage"></a>Escalado de proceso y almacenamiento

Tras crear el servidor, puede escalar entre los niveles De uso general y Con optimización para memoria a medida que cambien sus necesidades. También puede escalar el proceso y la memoria aumentando o reduciendo núcleos virtuales. El almacenamiento se puede escalar verticalmente (pero no reducir).

### <a name="scale-between-general-purpose-and-memory-optimized-tiers"></a>Escalado entre niveles De uso general y Con optimización de memoria

Puede escalar desde De uso general a Optimizada para memoria y viceversa. No se permite el cambio desde un plan Básico después de haber creado el servidor. 

1. Seleccione el servidor en Azure Portal. Seleccione **Plan de tarifa**, que se encuentra en la sección **Configuración**.

2. Seleccione **De uso general** o **Con optimización de memoria**, dependiendo de lo que esté escalando. 

    ![change-pricing-tier](./media/howto-create-manage-server-portal/change-pricing-tier.png)

    > [!NOTE]
    > El cambio de los niveles provoca un reinicio del servidor.

4. Seleccione **Aceptar** para guardar los cambios.


### <a name="scale-vcores-up-or-down"></a>Escalado o reducción vertical de los núcleos virtuales

1. Seleccione el servidor en Azure Portal. Seleccione **Plan de tarifa**, que se encuentra en la sección **Configuración**.

2. Cambie el valor de **vCore** moviendo el control deslizante hasta alcanzar el valor deseado.

    ![escalar proceso](./media/howto-create-manage-server-portal/scaling-compute.png)

    > [!NOTE]
    > El escalado de los núcleos virtuales provoca un reinicio del servidor.

3. Seleccione **Aceptar** para guardar los cambios.


### <a name="scale-storage-up"></a>Escalado vertical del almacenamiento

1. Seleccione el servidor en Azure Portal. Seleccione **Plan de tarifa**, que se encuentra en la sección **Configuración**.

2. Cambie el valor de **Almacenamiento** moviendo el control deslizante hacia arriba hasta alcanzar el valor que se quiera.

    ![escalar almacenamiento](./media/howto-create-manage-server-portal/scaling-storage.png)

    > [!NOTE]
    > El almacenamiento no se puede reducir verticalmente.

3. Seleccione **Aceptar** para guardar los cambios.


## <a name="update-admin-password"></a>Actualización de la contraseña del administrador
La contraseña del rol de administrador se puede cambiar en Azure Portal.

1. Seleccione el servidor en Azure Portal. En la ventana **Información general**, seleccione **Restablecer contraseña**.

   ![Información general](./media/howto-create-manage-server-portal/overview-reset-password.png)

2. Escriba la contraseña nueva y confírmela. El cuadro de texto le pedirá los requisitos de complejidad de la contraseña.

   ![reset-password](./media/howto-create-manage-server-portal/reset-password.png)

3. Seleccione **Aceptar** para guardar la nueva contraseña.


## <a name="delete-a-server"></a>Eliminación de un servidor

Puede eliminar el servidor cuando ya no lo necesite. 

1. Seleccione el servidor en Azure Portal. En la ventana **Información general**, seleccione **Eliminar**.

    ![delete](./media/howto-create-manage-server-portal/overview-delete.png)

2. Escriba el nombre del servidor en el cuadro de entrada para confirmar que es el servidor que quiere eliminar.

    ![confirmar eliminación](./media/howto-create-manage-server-portal/confirm-delete.png)

    > [!NOTE]
    > La eliminación de un servidor es irreversible.

3. Seleccione **Eliminar**.


## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre [copias de seguridad y restauración del servidor](howto-restore-server-portal.md).
- Obtenga más información sobre las opciones de [supervisión y ajuste en Azure Database for MySQL](concepts-monitoring.md).
