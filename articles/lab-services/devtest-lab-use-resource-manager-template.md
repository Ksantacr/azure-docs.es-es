---
title: Visualización y uso de una plantilla de Azure Resource Manager en una máquina virtual | Microsoft Docs
description: Aprenda a usar la plantilla de Azure Resource Manager desde una máquina virtual para crear otras máquinas virtuales
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: spelluru
ms.openlocfilehash: 533770d98b146dea01e91e1249115c4b5c074b3c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62101578"
---
# <a name="create-virtual-machines-using-an-azure-resource-manager-template"></a>Creación de máquinas virtuales con una plantilla de Azure Resource Manager 

Al crear una máquina virtual (VM) en DevTest Labs a través de [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040), puede ver la plantilla de Azure Resource Manager antes de guardar la máquina virtual. La plantilla, a continuación, se puede usar como punto de partida para crear más máquinas virtuales de laboratorio con la misma configuración.

En este artículo se describen las plantillas de Resource Manager de varias VM frente a las de una sola VM y también se muestra cómo puede ver y guardar una plantilla al crear una máquina virtual.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Plantillas de Resource Manager de una sola máquina virtual o de varias
Hay dos maneras de crear las máquinas virtuales en DevTest Labs mediante una plantilla de Resource Manager: aprovisionar el recurso Microsoft.DevTestLab/labs/virtualmachines o aprovisionar el recurso Microsoft.COMPUTE/virtualmachines. Cada una se utiliza en escenarios diferentes y requiere permisos distintos.

- Las plantillas de Resource Manager que usan un tipo de recurso Microsoft.DevTestLab/labs/virtualmachines (tal y como se declara en la propiedad "resource" en la plantilla) pueden aprovisionar máquinas virtuales de laboratorio individuales. Cada máquina virtual aparecerá entonces como un único elemento en la lista de máquinas virtuales de DevTest Labs:

   ![Enumeración de máquinas virtuales como elementos únicos en la lista de máquinas virtuales de DevTest Labs](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   Este tipo de plantilla de Resource Manager se puede aprovisionar mediante el comando de PowerShell de Azure **New AzResourceGroupDeployment** o mediante el comando de CLI de Azure **Crear implementación de un grupo az**. Se necesitan permisos de administrador, por lo que los usuarios asignados a un rol de usuario de DevTest Labs no pueden realizar la implementación. 

- Las plantillas de Resource Manager que usan un tipo de recurso Microsoft.Compute/virtualmachines pueden aprovisionar varias máquinas virtuales como un único entorno en la lista de máquinas virtuales de DevTest Labs:

   ![Enumeración de máquinas virtuales como elementos únicos en la lista de máquinas virtuales de DevTest Labs](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Las máquinas virtuales en el mismo entorno pueden administrarse conjuntamente y compartir el mismo ciclo de vida. Los usuarios asignados a un rol de usuario de DevTest Labs pueden crear entornos con esas plantillas, siempre y cuando el administrador haya configurado el laboratorio de ese modo.

En el resto de este artículo se describen las plantillas de Resource Manager que utilizan Mirosoft.DevTestLab/labs/virtualmachines. Los administradores de DevTest Labs las usan para automatizar la creación de máquinas virtuales de laboratorio (por ejemplo, las VM reclamables) o la generación de imágenes maestras (por ejemplo, la factoría de imágenes).

En [Procedimientos recomendados para crear plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) se ofrecen muchas directrices y sugerencias para ayudarle a crear plantillas de Azure Resource Manager confiables y fáciles de usar.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Visualización y guardado de una plantilla de Resource Manager de una máquina virtual
1. Siga los pasos de [Creación de su primera máquina virtual en un laboratorio](tutorial-create-custom-lab.md#add-a-vm-to-the-lab) para empezar a crear una máquina virtual.
1. Escriba la información necesaria para la máquina virtual y agréguele cualquier artefacto que desee.
1. En la parte inferior de la ventana Parámetros de configuración, elija **View ARM template** (Ver plantilla ARM).

   ![Botón View ARM template (Ver plantilla ARM)](./media/devtest-lab-use-arm-template/devtestlab-lab-view-rm-template.png)
1. Copie y guarde la plantilla de Resource Manager para usarla más adelante para crear otra máquina virtual.

   ![Plantilla de Resource Manager que se debe guardar para usarla más adelante](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Después de guardar la plantilla de Resource Manager, debe actualizar la sección de parámetros de la plantilla para poder utilizarla. Puede crear un archivo parameter.json que personalice solamente los parámetros, fuera de la plantilla de Resource Manager real. 

![Personalización de los parámetros con un archivo JSON](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

La plantilla de Resource Manager ya está lista para usarse para [crear una máquina virtual](devtest-lab-create-environment-from-arm.md).

### <a name="next-steps"></a>Pasos siguientes
* Aprenda a [crear entornos de varias máquinas virtuales con plantillas de Resource Manager](devtest-lab-create-environment-from-arm.md).
* [Deploy a Resource Manager template to create a VM](devtest-lab-create-environment-from-arm.md#automate-deployment-of-environments) (Implementación de una plantilla de Resource Manager para crear una máquina virtual)
* Explore más plantillas de Resource Manager de inicio rápido para la automatización de DevTest Labs desde el [repositorio público de GitHub de DevTest Labs](https://github.com/Azure/azure-quickstart-templates).
