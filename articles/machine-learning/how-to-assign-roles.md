---
title: Administración de roles del área de trabajo
titleSuffix: Azure Machine Learning
description: Conozca cómo acceder a un área de trabajo de Azure Machine Learning mediante el control de acceso basado en rol (RBAC).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: larryfr
author: Blackmist
ms.date: 11/06/2019
ms.custom: seodec18
ms.openlocfilehash: 5257d9f94f6304c2a8dbea3f1648a71d0ba65e94
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77064757"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Administración del acceso a un área de trabajo de Azure Machine Learning
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, obtendrá información sobre cómo administrar el acceso a un área de trabajo de Azure Machine Learning. El [control de acceso basado en rol (RBAC)](/azure/role-based-access-control/overview) se utiliza para administrar el acceso a los recursos de Azure. Se asignan roles específicos a los usuarios de Azure Active Directory que les conceden acceso a los recursos. Azure proporciona tanto roles integrados como la capacidad de crear roles personalizados.

## <a name="default-roles"></a>Roles predeterminados

Un área de trabajo de Azure Machine Learning es un recurso de Azure. Al igual que otros recursos de Azure, cuando se crea un área de trabajo de Azure Machine Learning nueva, viene con tres roles predeterminados. Puede agregar usuarios al área de trabajo y asignarlos a uno de estos roles integrados.

| Role | Nivel de acceso |
| --- | --- |
| **Lector** | Acciones de solo lectura en el área de trabajo. Los lectores pueden enumerar y ver los recursos de un área de trabajo, pero no pueden crear ni actualizar estos recursos. |
| **Colaborador** | Ver, crear, editar o eliminar (si procede) los recursos de un área de trabajo. Por ejemplo, los colaboradores pueden crear un experimento, crear o conectar un clúster de proceso, enviar una ejecución e implementar un servicio web. |
| **Propietario** | Acceso total al área de trabajo, incluida la posibilidad de ver, crear, editar o eliminar (si procede) los recursos de un área de trabajo. Además, puede cambiar las asignaciones de roles. |

> [!IMPORTANT]
> El acceso de los roles puede tener un ámbito de varios niveles en Azure. Por ejemplo, un usuario con acceso de propietario a un área de trabajo podría no tener acceso de propietario al grupo de recursos que contiene el área de trabajo. Para más información, consulte [Cómo funciona RBAC](/azure/role-based-access-control/overview#how-rbac-works).

Para más información sobre roles integrados específicos, consulte [Roles integrados de Azure](/azure/role-based-access-control/built-in-roles).

## <a name="manage-workspace-access"></a>Administración del acceso al área de trabajo

Si es propietario de un área de trabajo, puede agregar y eliminar roles del área de trabajo. También puede asignar roles a los usuarios. Use los siguientes vínculos para descubrir cómo administrar el acceso:
- [Interfaz de usuario de Azure Portal](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [CLI de Azure](/azure/role-based-access-control/role-assignments-cli)
- [REST API](/azure/role-based-access-control/role-assignments-rest)
- [Plantillas del Administrador de recursos de Azure](/azure/role-based-access-control/role-assignments-template)

Si ha instalado la [CLI de Azure Machine Learning](reference-azure-machine-learning-cli.md), también puede usar un comando de la CLI para asignar roles a los usuarios.

```azurecli-interactive 
az ml workspace share -w <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

El campo `user` es la dirección de correo electrónico de un usuario existente en la instancia de Azure Active Directory donde se encuentra la suscripción principal del área de trabajo. Este es un ejemplo de cómo utilizar este comando:

```azurecli-interactive 
az ml workspace share -w my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Creación de un rol personalizado

Si los roles integrados no son suficientes, puede crear roles personalizados. Los roles personalizados pueden tener permisos para leer, escribir, eliminar y procesar recursos de ese área de trabajo. Puede hacer que el rol esté disponible en un nivel de área de trabajo específico, un nivel de grupo de recursos específico o un nivel de suscripción específico.

> [!NOTE]
> Debe ser un propietario del recurso en ese nivel para crear roles personalizados en ese recurso.

Para crear un rol personalizado, primero debe construir un archivo JSON de definición de rol que especifica el permiso y el ámbito del rol. El ejemplo siguiente define un rol personalizado llamado "Data Scientist" (Científico de datos) con ámbito en un nivel de área de trabajo específica:

`data_scientist_role.json`:
```json
{
    "Name": "Data Scientist",
    "IsCustom": true,
    "Description": "Can run experiment but can't create or delete compute.",
    "Actions": ["*"],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete", 
        "Microsoft.Authorization/*/write"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace_name>"
    ]
}
```

Puede cambiar el campo `AssignableScopes` para establecer el ámbito de este rol personalizado en el nivel de suscripción, el nivel de grupo de recursos o un nivel de área de trabajo específica.

Este rol personalizado puede hacer todo en el área de trabajo, excepto las siguientes acciones:

- No puede crear o actualizar un recurso de proceso.
- No puede eliminar un recurso de proceso.
- No puede agregar, eliminar o modificar asignaciones de roles.
- No puede eliminar el área de trabajo.

Para implementar este rol personalizado, use el siguiente comando de la CLI de Azure:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Después de la implementación, este rol está disponible en el área de trabajo especificada. Ahora puede agregar y asignar este rol en Azure Portal. O bien, puede asignar este rol a un usuario mediante el comando de la CLI `az ml workspace share`:

```azurecli-interactive
az ml workspace share -w my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```

Para obtener información acerca de los roles personalizados, consulte [Roles personalizados en los recursos de Azure](/azure/role-based-access-control/custom-roles).

Para más información sobre las operaciones (acciones) que se pueden usar con roles personalizados, consulte [Operaciones del proveedor de recursos](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices).


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes


### <a name="q-what-are-the-permissions-needed-to-perform-various-actions-in-the-azure-machine-learning-service"></a>Q. ¿Cuáles son los permisos necesarios para realizar varias acciones en Azure Machine Learning Service?

En la tabla siguiente se proporciona un resumen de las actividades de Azure Machine Learning y los permisos necesarios para realizarlas en el menor ámbito. Por ejemplo, si una actividad se puede realizar con un ámbito de área de trabajo (columna 4), cualquier el ámbito superior con ese permiso también funcionará automáticamente. Todas las rutas de acceso de esta tabla son **rutas de acceso relativas** a `Microsoft.MachineLearningServices/`.

| Actividad | Ámbito a nivel de supervisión | Ámbito a nivel de grupo de recursos | Ámbito a nivel de área de trabajo |
|---|---|---|---|
| Crear área de trabajo nueva | No se requiere | Propietario o colaborador | N/A (se convierte en Propietario o hereda el rol del ámbito superior después de la creación) |
| Crear un clúster de proceso | No se requiere | No se requiere | Propietario, colaborador o rol personalizado que permita: `workspaces/computes/write` |
| Crear una máquina virtual de Notebook | No se requiere | Propietario o colaborador | No es posible |
| Crear una instancia de proceso | No se requiere | No se requiere | Propietario, colaborador o rol personalizado que permita: `workspaces/computes/write` |
| Actividad de plano de datos como el envío de una ejecución, el acceso a datos, la implementación de un modelo o la publicación de una canalización | No se requiere | No se requiere | Propietario, colaborador o rol personalizado que permita: `workspaces/*/write` <br/> Tenga en cuenta que también necesita un almacén de datos registrado en el área de trabajo para permitir que MSI tenga acceso a los datos de la cuenta de almacenamiento. |


### <a name="q-how-do-i-list-all-the-custom-roles-in-my-subscription"></a>Q. ¿Cómo obtengo una lista de todos los roles personalizados en mi suscripción?

Ejecute el siguiente comando en la CLI de Azure.

```azurecli-interactive
az role definition list --subscription <sub-id> --custom-role-only true
```

### <a name="q-how-do-i-find-the-role-definition-for-a-role-in-my-subscription"></a>Q. ¿Cómo encuentro la definición de roles de un rol en mi suscripción?

Ejecute el siguiente comando en la CLI de Azure. Tenga en cuenta que `<role-name>` debe tener el mismo formato devuelto por el comando anterior.

```azurecli-interactive
az role definition list -n <role-name> --subscription <sub-id>
```

### <a name="q-how-do-i-update-a-role-definition"></a>Q. ¿Cómo actualizo una definición de roles?

Ejecute el siguiente comando en la CLI de Azure.

```azurecli-interactive
az role definition update --role-definition update_def.json --subscription <sub-id>
```

Tenga en cuenta que debe tener permisos en todo el ámbito de la nueva definición de roles. Por ejemplo, si este nuevo rol tiene un ámbito entre tres suscripciones, debe tener permisos en las tres suscripciones. 

> [!NOTE]
> Las actualizaciones de roles pueden tardar entre 15 minutos y una hora en aplicarse a todas las asignaciones de roles de ese ámbito.
### <a name="q-can-i-define-a-role-that-prevents-updating-the-workspace-edition"></a>Q. ¿Puedo definir un rol que impida la actualización de la edición del área de trabajo? 

Sí, puede definir un rol que impida la actualización de la edición del área de trabajo. Dado que la actualización del área de trabajo es una llamada PATCH en el objeto del área de trabajo, puede hacerlo colocando la siguiente acción en la matriz `"NotActions"` en la definición de JSON: 

`"Microsoft.MachineLearningServices/workspaces/write"`

### <a name="q-what-permissions-are-needed-to-perform-quota-operations-in-a-workspace"></a>Q. ¿Qué permisos son necesarios para realizar operaciones de cuota en un área de trabajo? 

Necesita permisos de nivel de suscripción para realizar cualquier operación relacionada con la cuota en el área de trabajo. Esto significa que la configuración de la cuota de nivel de suscripción o de nivel de área de trabajo para los recursos de proceso administrados solo puede producirse si tiene permisos de escritura en el ámbito de la suscripción. 


## <a name="next-steps"></a>Pasos siguientes

- [Introducción a la seguridad en la empresa](concept-enterprise-security.md)
- [Ejecución de experimentos y realización de inferencias/puntuaciones de forma segura en una red virtual](how-to-enable-virtual-network.md)
- [Tutorial: Entrenamiento de modelos](tutorial-train-models-with-aml.md)
- [Operaciones del proveedor de recursos](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices)
