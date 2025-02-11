---
title: Descripción de la creación de definiciones de interfaz de usuario para aplicaciones administradas de Azure | Microsoft Docs
description: Describe cómo crear definiciones de interfaz de usuario para aplicaciones administradas de Azure
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/26/2019
ms.author: tomfitz
ms.openlocfilehash: 3d0a6d97440404904c041369a4631fdd3fb618b4
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257567"
---
# <a name="create-azure-portal-user-interface-for-your-managed-application"></a>Creación de la interfaz de usuario de Azure Portal para una aplicación administrada
En este documento se presentan los conceptos principales del archivo createUiDefinition.json. Azure Portal usa este archivo para generar la interfaz de usuario para crear una aplicación administrada.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
   "handler": "Microsoft.Compute.MultiVm",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { }
   }
}
```

CreateUiDefinition siempre contiene tres propiedades: 

* handler
* version
* parameters

Para las aplicaciones administradas, handler siempre debería ser `Microsoft.Compute.MultiVm`, y la versión más reciente admitida es `0.1.2-preview`.

El esquema de la propiedad parameters depende de la combinación de los valores de handler y version especificados. Para las aplicaciones administradas, las propiedades admitidas son `basics`, `steps` y `outputs`. Las propiedades basics y steps contienen los _elementos_, como cuadros de texto y listas desplegables, que se mostrarán en Azure Portal. La propiedad outputs se utiliza para asignar los valores de salida de los elementos especificados a los parámetros de la plantilla de implementación de Azure Resource Manager.

Se recomienda incluir `$schema`, aunque es opcional. Si se especifica, el valor de `version` debe coincidir con la versión en el identificador URI de `$schema`.

Puede usar un editor de JSON para crear la definición de interfaz de usuario, o puede usar el espacio aislado de definición de interfaz de usuario para crear y obtener una vista previa de la definición de interfaz de usuario. Para obtener más información sobre el espacio aislado, consulte [probar la interfaz del portal de Azure Managed Applications](test-createuidefinition.md).

## <a name="basics"></a>Aspectos básicos
El paso Aspectos básicos es siempre el primer paso del asistente que se genera cuando Azure Portal analiza el archivo. Además de mostrar los elementos especificados en `basics`, el portal inserta elementos para que los usuarios elijan la suscripción, el grupo de recursos y la ubicación de la implementación. Por lo general, los elementos que consultan los parámetros de toda la implementación, como el nombre de un clúster o las credenciales del administrador, deben ir en este paso.

Si el comportamiento de un elemento depende de la suscripción, el grupo de recursos o la ubicación del usuario, ese elemento no puede utilizarse en basics. Por ejemplo, **Microsoft.Compute.SizeSelector** depende de suscripción del usuario y la ubicación para determinar la lista de los tamaños disponibles. Por lo tanto, **Microsoft.Compute.SizeSelector** solo puede utilizarse en steps. Por lo general, solo los elementos del espacio de nombres **Microsoft.Common** pueden usarse en basics. Sin embargo, sí se permiten algunos elementos de otros espacios de nombres (como **Microsoft.Compute.Credentials**) que no dependen del contexto del usuario.

## <a name="steps"></a>Pasos
La propiedad steps puede contener cero o más pasos adicionales que se mostrarán después de basics, cada uno de los cuales contiene uno o más elementos. Considere la posibilidad de agregar pasos por rol o nivel de aplicación que se está implementando. Por ejemplo, agregue un paso para las entradas de los nodos principales y un paso para los nodos de trabajo de un clúster.

## <a name="outputs"></a>Salidas
Azure Portal usa la propiedad `outputs` para asignar elementos de `basics` y `steps` a los parámetros de la plantilla de implementación de Azure Resource Manager. Las claves de este diccionario son los nombres de los parámetros de plantilla, y los valores son propiedades de los objetos de salida de los elementos a los que se hace referencia.

Para definir el nombre del recurso de la aplicación administrada, debe incluir un valor llamado `applicationResourceName` en la propiedad outputs. Si no establece este valor, la aplicación asigna un GUID para el nombre. Puede incluir un cuadro de texto en la interfaz de usuario que requiera un nombre de usuario.

```json
"outputs": {
    "vmName": "[steps('appSettings').vmName]",
    "trialOrProduction": "[steps('appSettings').trialOrProd]",
    "userName": "[steps('vmCredentials').adminUsername]",
    "pwd": "[steps('vmCredentials').vmPwd.password]",
    "applicationResourceName": "[steps('appSettings').vmName]"
}
```

## <a name="functions"></a>Functions
Al igual que las funciones de plantilla en Azure Resource Manager (tanto en la sintaxis y la funcionalidad), CreateUiDefinition proporciona funciones para trabajar con entradas y salidas y características como los condicionales de los elementos.

## <a name="next-steps"></a>Pasos siguientes
El propio archivo createUiDefinition.json tiene un esquema simple. Su profundidad real procede de todos los elementos y funciones que admite. Dichos elementos se describen con más detalle en:

- [Elementos](create-uidefinition-elements.md)
- [Funciones](create-uidefinition-functions.md)

Un esquema JSON actual para createUiDefinition está disponible aquí: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.

Para un archivo de interfaz de usuario de ejemplo, vea [createUiDefinition.json](https://github.com/Azure/azure-managedapp-samples/blob/master/samples/201-managed-app-using-existing-vnet/createUiDefinition.json).
