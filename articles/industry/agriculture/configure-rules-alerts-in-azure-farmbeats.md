---
title: Configuración de reglas y administración de alertas
description: Se describe cómo configurar reglas y administrar alertas en FarmBeats.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 02373da114b6dc4663cd3ffc9a0780ac6a7a9e23
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475910"
---
# <a name="configure-rules-and-manage-alerts"></a>Configuración de reglas y administración de alertas

Azure FarmBeats permite crear reglas basadas en la lógica de negocios y en los datos que fluyen desde los sensores y dispositivos implementados en una granja. Las reglas desencadenan alertas en el sistema cada vez que los valores del sensor cruzan un valor de umbral. Al poder ver y analizar las alertas creadas después de los valores de umbral, es posible actuar rápidamente sobre los problemas y obtener las soluciones necesarias.

## <a name="create-rule"></a>Creación de una regla

1. En la página principal, vaya a **Rules** (Reglas).
2. Seleccione **New Rule** (Nueva regla). Aparece la ventana New Rule (Nueva regla).

    ![Proyecto de FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/new-rule-1.png)

3. En **Rule Name** (Nombre de regla) y **Rule Description** (Descripción de la regla), escriba la información pertinente y, luego, seleccione una granja del menú desplegable **Select Farm** (Seleccionar granja).
4. Escriba el nombre de la granja para seleccionarla; aparece la sección **Conditions** (Condiciones) en la misma ventana.  

    ![Proyecto de FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/new-rule-condition-1.png)

5. En **Conditions** (Condiciones), escriba los valores de **Measure** (Medida), **Operator** (Operador) y **Value** (Valor).
6. Escriba el nombre de la medida en el menú desplegable **Measure** (Medida).
7. Seleccione **+Add Condition** (+ Agregar condición) para agregar más condiciones a la regla.
8. Seleccione el valor de **Severity level** (Nivel de gravedad).
9. En **Action** (Acción), active el botón de conmutación **Email enabled** (Habilitada por correo electrónico) para habilitar las alertas por correo electrónico.

    ![Proyecto de FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/new-rule-email-1.png)

10. Escriba las **direcciones de correo electrónico** a las que quiere enviar la alerta de correo electrónico, junto con la información de **Email Subject** (Asunto de correo electrónico) y **Additional Notes** (Notas adicionales).  
11. En **Rule Status** (Estado de la regla), active el botón de conmutación **Enabled** (Habilitado) para habilitar o deshabilitar la regla.
    Puede ver el número de dispositivos que resultarán afectados por la regla.
12. Para crear la regla, seleccione **Apply** (Aplicar).

## <a name="view-rule"></a>Visualización de la regla

En la página **Farm** (Granja) se muestra la lista de reglas disponibles. En **Rule Name** (Nombre de la regla), seleccione un nombre de regla. En una ventana se muestran los siguientes detalles que son aplicables a la regla seleccionada:
 - Nombre de la regla
 - Vínculo a la granja a la que está asociada la regla
 - Fecha de creación
 - Fecha de la última actualización
 - Nivel de gravedad
 - Estado de la regla
 - Lista de condiciones  
 - Número de dispositivos afectados por la regla

    ![Proyecto de FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/view-rule-1.png)

## <a name="edit-rule"></a>Editar regla

Para editar una regla, siga estos pasos:

1. En la página principal, seleccione **Rules** (Reglas) en el menú de navegación izquierdo.
   Se muestra la ventana Rules (Reglas).
2. Seleccione la regla que quiere editar.

    ![Proyecto de FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/edit-rule-action-bar-1.png)

3. Seleccione **Edit** (Editar) en la barra de acciones; se muestra la ventana **Edit Rule** (Editar regla).

    ![Proyecto de FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/edit-rule-one-1.png)

4. Cambie la información de **Rule Name** (Nombre de regla) y **Rule Description** (Descripción de la regla) y, luego, seleccione una granja del menú desplegable **Select Farm** (Seleccionar granja).
5. Escriba el nombre de la granja para seleccionarla; aparece **Conditions** (Condiciones) en la misma ventana.  
6. En **Conditions** (Conditions), edite los datos de **Measure** (Medida), **Operator** (Operador) y **Value** (Valor).
7. Escriba el nombre de la medida en el menú desplegable **Measure** (Medida).
8. Seleccione **+Add Condition** (+ Agregar condición) para agregar condiciones a las reglas o editar condiciones.

    ![Proyecto de FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/edit-rule-two-1.png)

9.  Seleccione el valor de **Severity level** (Nivel de gravedad).  
10. En **Action** (Acción), active el botón de conmutación **Email enabled** (Habilitada por correo electrónico) para habilitar las alertas por correo electrónico.
11. Edite las **direcciones de correo electrónico** a las que quiere enviar la alerta de correo electrónico, junto con la información de **Email Subject** (Asunto de correo electrónico) y **Additional Notes**(Notas adicionales).  
12. En **Rule Status** (Estado de la regla), active el botón de conmutación **Enabled** (Habilitado) para habilitar o deshabilitar la regla.
Puede ver el número de dispositivos que resultarán afectados por esta regla.
13. Seleccione **Apply** (Aplicar) para editar la regla.

## <a name="change-rule-status"></a>Cambio del estado de la regla

Para cambiar el estado de una regla, siga estos pasos:

1. En la página principal, seleccione **Rules** (Reglas) en el menú de navegación izquierdo. Se muestra la ventana Rules (Reglas).
2. Seleccione la regla cuyo estado quiere cambiar.

    ![Proyecto de FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/change-status-rule-action-bar-1.png)

3. Seleccione **Change Status** (Cambiar estado) en la barra de acciones. Aparece la ventana **Change Status** (Cambiar estado).

    ![Proyecto de FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/rule-change-status-1.png)

3. Cambie el estado de la regla con el botón de conmutación **Change Status** (Cambiar estado).
   Puede ver el número de dispositivos que resultarán afectados por la regla.
4. Seleccione **Apply** (Aplicar) para cambiar el estado de la regla.

## <a name="delete-rule"></a>Eliminar regla

Para eliminar una regla, siga estos pasos:

1. En la página principal, seleccione **Rules** (Reglas) en el menú de navegación izquierdo. Se muestra la ventana Rules (Reglas).
2. Seleccione la regla que quiere eliminar.

    ![Proyecto de FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/delete-rule-action-bar-1.png)

3. Seleccione **Delete** (Eliminar) en la barra de acciones.

    ![Proyecto de FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/delete-rule-1.png)

4. Aparece el cuadro de diálogo **Delete Rule** (Eliminar regla). Seleccione **Eliminar**.
