---
title: Creación de una instancia de Azure Application Gateway mediante plantillas | Microsoft Docs
description: En esta página se ofrecen instrucciones para crear una instancia de Azure Application Gateway con la plantilla de Azure Resource Manager
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: victorh
ms.openlocfilehash: 7ff6db5acb150207f975931155386a308c48888b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66134103"
---
# <a name="create-an-application-gateway-by-using-the-azure-resource-manager-template"></a>Creación de una instancia de Application Gateway con la plantilla de Azure Resource Manager

Azure Application Gateway es un equilibrador de carga de nivel 7. Proporciona solicitudes HTTP de enrutamiento del rendimiento y conmutación por error y entre distintos servidores, independientemente de que se encuentren en la nube o en el entorno local. Application Gateway proporciona numerosas características del controlador de entrega de aplicaciones (ADC), entre las que se incluyen el equilibrio de carga HTTP, la afinidad de sesiones basada en cookies, la descarga SSL (Capa de sockets seguros), los sondeos personalizados sobre el estado, la compatibilidad con multisitio, y muchas más. Para encontrar una lista completa de las características admitidas, visite [Introducción a Application Gateway](overview.md).

Este artículo le guiará por los procesos de descarga y modificación de una [plantilla de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) desde GitHub, así como la implementación desde GitHub, PowerShell y la CLI de Azure.

Si solo va a implementar la plantilla directamente desde GitHub sin realizar ningún cambio, vaya a la implementación de una plantilla desde GitHub.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario"></a>Escenario

En este escenario:

* Creará una instancia de Application Gateway con firewall de aplicaciones web.
* Creará una red virtual denominada VirtualNetwork1 con un bloque CIDR reservado de 10.0.0.0/16.
* Creará una subred denominada Appgatewaysubnet que usa 10.0.0.0/28 como bloque CIDR.
* Instalará dos IP de back-end configuradas con anterioridad para los servidores web cuya carga de tráfico desea equilibrar. En esta plantilla de ejemplo, las IP de back-end son 10.0.1.10 y 10.0.1.11.

> [!NOTE]
> Esos son los parámetros para esta plantilla. Para personalizar la plantilla, puede cambiar las reglas, el agente de escucha, la SSL y otras opciones del archivo azuredeploy.json.

![Escenario](./media/create-vmss-template/scenario.png)

## <a name="download-and-understand-the-azure-resource-manager-template"></a>Descarga e información sobre la plantilla de Azure Resource Manager

Puede descargar la plantilla de Azure Resource Manager existente para crear una red virtual y dos subredes desde GitHub, realizar todos los cambios que desee y volver a utilizarla. Para ello, siga estos pasos:

1. Vaya al artículo sobre la [creación de instancias de Application Gateway con firewall de aplicaciones web habilitado](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).
1. Haga clic en **azuredeploy.json** y luego en **RAW**.
1. Guarde el archivo en un una carpeta local del equipo.
1. Si está familiarizado con las plantillas de Azure Resource Manager, vaya directamente al paso 7.
1. Abra el archivo que guardó y vea el contenido de **parameters** en la línea
1. Los parámetros de la plantilla de Azure Resource Manager proporcionan un marcador de posición para los valores que se pueden rellenar durante la implementación.

   | Parámetro | DESCRIPCIÓN |
   | --- | --- |
   | **subnetPrefix** |Bloque CIDR de la subred de Application Gateway. |
   | **applicationGatewaySize** | Tamaño de la instancia de Application Gateway.  WAF solo permite tamaños medianos y grandes. |
   | **backendIpaddress1** |Dirección IP del primer servidor web. |
   | **backendIpaddress2** |Dirección IP del segundo servidor web. |
   | **wafEnabled** | Configuración para determinar si WAF está habilitada.|
   | **wafMode** | Modo del firewall de aplicaciones web.  Las opciones disponibles son **prevención** o **detección**.|
   | **wafRuleSetType** | Tipo de conjunto de reglas para WAF.  Actualmente, OWASP es la única opción compatible. |
   | **wafRuleSetVersion** |Versión del conjunto de reglas. Actualmente, OWASP CRS 2.2.9 y 3.0 son las opciones admitidas. |

1. Compruebe el contenido en **resources** y observe las propiedades siguientes:

   * **type**. Tipo de recurso que creó la plantilla. En este caso, el tipo es `Microsoft.Network/applicationGateways`, que representa una puerta de enlace de aplicaciones.
   * **nombre**. Nombre del recurso. Observe el uso de `[parameters('applicationGatewayName')]`, que significa que el nombre lo proporciona el usuario o un archivo de parámetros durante la implementación.
   * **properties**. Lista de propiedades para el recurso. Esta plantilla usa la red virtual y la dirección IP pública durante la creación de la instancia de Application Gateway. Para la sintaxis y las propiedades de JSON de una puerta de enlace de aplicaciones en la plantilla, consulte [Microsoft.Network/applicationgateways](/azure/templates/microsoft.network/applicationgateways).

1. Vuelva a [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).
1. Haga clic en **azuredeploy-paremeters.json** y luego en **Sin formato**.
1. Guarde el archivo en un una carpeta local del equipo.
1. Abra el archivo que guardó y edite los valores de los parámetros. Use los siguientes valores para implementar la instancia de Application Gateway que se describe en nuestro escenario.

     ```json
     {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
             "addressPrefix": {
             "value": "10.0.0.0/16"
             },
             "subnetPrefix": {
             "value": "10.0.0.0/28"
             },
             "applicationGatewaySize": {
             "value": "WAF_Medium"
             },
             "capacity": {
             "value": 2
             },
             "backendIpAddress1": {
             "value": "10.0.1.10"
             },
             "backendIpAddress2": {
             "value": "10.0.1.11"
             },
             "wafEnabled": {
             "value": true
             },
             "wafMode": {
             "value": "Detection"
             },
             "wafRuleSetType": {
             "value": "OWASP"
             },
             "wafRuleSetVersion": {
             "value": "3.0"
             }
         }
     }
     ```

1. Guarde el archivo. Puede probar la plantilla de JSON y la plantilla de parámetros mediante las herramientas en línea de validación de JSON como [JSlint.com](https://www.jslint.com/).

## <a name="deploy-the-azure-resource-manager-template-by-using-powershell"></a>Implementación de la plantilla de Azure Resource Manager mediante PowerShell

Si es la primera vez que usa Azure PowerShell, visite: [Instalación y configuración de Azure PowerShell](/powershell/azure/overview) y siga las instrucciones para iniciar sesión en Azure y seleccionar su suscripción.

1. Inicio de sesión en PowerShell

    ```powershell
    Login-AzAccount
    ```

1. Compruebe las suscripciones para la cuenta.

    ```powershell
    Get-AzSubscription
    ```

    Se le solicita que se autentique con sus credenciales.

1. Elección de la suscripción de Azure que se va a usar.

    ```powershell
    Select-AzSubscription -Subscriptionid "GUID of subscription"
    ```

1. Si es necesario, cree un grupo de recursos mediante el cmdlet **New-AzureResourceGroup**. En el ejemplo siguiente, se crea un grupo de recursos denominado AppgatewayRG en la ubicación Este de EE. UU.

    ```powershell
    New-AzResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. Ejecute el **New AzResourceGroupDeployment** cmdlet para implementar la nueva red virtual mediante la plantilla anterior y el parámetro que descargó y modificó los archivos.
    
    ```powershell
    New-AzResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-the-azure-cli"></a>Implementación de la plantilla de Azure Resource Manager mediante la CLI de Azure

Para implementar la plantilla de Azure Resource Manager descargada mediante la CLI de Azure, siga estos pasos:

1. Si es la primera vez que usa la CLI de Azure, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli) y siga las instrucciones hasta el punto donde tiene que seleccionar su cuenta y suscripción de Azure.

1. En caso necesario, ejecute el comando `az group create` para crear un grupo de recursos, como se muestra en el siguiente fragmento de código. Observe la salida del comando. En la lista que se muestra en la salida se explican los parámetros utilizados. Para más información sobre los grupos de recursos, visite [Información general de Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    **-n (o --name)**. Nombre del nuevo grupo de recursos. En este escenario, es *appgatewayRG*.
    
    **-l (o --location)**. Región de Azure donde se crea el nuevo grupo de recursos. En este escenario, es *westus*.

1. Ejecute el cmdlet `az group deployment create` para implementar la nueva red virtual mediante los archivos de plantillas y parámetros que descargó y modificó en el paso anterior. En la lista que se muestra en la salida se explican los parámetros utilizados.

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-click-to-deploy"></a>Implementación de la plantilla de Azure Resource Manager con el método de hacer clic para implementar

Clic para implementar es otra forma de usar las plantillas de Azure Resource Manager. Se trata de una manera fácil de usar plantillas con Azure Portal.

1. Vaya a [Creación de una instancia de Application Gateway con firewall de aplicaciones web](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).

1. Haga clic en **Implementar en Azure**.

    ![Implementar en Azure](./media/create-vmss-template/deploytoazure.png)
    
1. Rellene los parámetros de la plantilla de implementación en el portal y haga clic en **Aceptar**.

    ![parameters](./media/create-vmss-template/ibiza1.png)
    
1. Seleccione **Acepto los términos y condiciones indicados anteriormente** y haga clic en **Comprar**.

1. En la hoja Implementación personalizada, haga clic en **Crear**.

## <a name="providing-certificate-data-to-resource-manager-templates"></a>Proporciona datos de certificado a las plantillas de Resource Manager

Cuando se usa SSL con una plantilla, el certificado debe proporcionarse en una cadena en base 64 en lugar de cargarse. Para convertir un archivo .pfx o .cer a una cadena en base 64, use alguno de los siguientes comandos. Los siguientes comandos convierten el certificado a una cadena en base 64 que se puede proporcionar a la plantilla. El resultado esperado es una cadena que se puede almacenar en una variable y pegarse en la plantilla.

### <a name="macos"></a>macOS
```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a> Windows
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a>Eliminación de todos los recursos

Para eliminar todos los recursos creados en este artículo, complete uno de los pasos siguientes:

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a>Pasos siguientes

Si desea configurar la descarga SSL, visite: [Configuración de una puerta de enlace de aplicaciones para la descarga SSL mediante el modelo de implementación clásica](tutorial-ssl-cli.md).

Si desea configurar una puerta de enlace de aplicaciones para usarla con un equilibrador de carga interno, consulte: [Creación de una puerta de enlace de aplicaciones con un equilibrador de carga interno (ILB)](redirect-internal-site-cli.md).

Si desea más información acerca de las opciones de equilibrio de carga en general, visite:

* [Equilibrador de carga de Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

