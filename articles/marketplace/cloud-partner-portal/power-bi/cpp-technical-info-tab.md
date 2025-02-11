---
title: Información técnica acerca de una oferta de aplicación de Power BI | Azure Marketplace
description: Configure los campos de información técnica para una oferta de la aplicación Power BI para Microsoft AppSource Marketplace.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: pabutler
ms.openlocfilehash: 15f4e2a76724a70c15411dea767cc9bc433e4d4a
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943229"
---
# <a name="power-bi-apps-technical-info-tab"></a>Pestaña Información técnica de aplicaciones de Power BI

En el **nueva oferta** , utilice el **información técnica** tab para proporcionar el programa de instalación de Power BI dirección URL del paquete y otra información que necesita validar la nueva oferta.  La versión inicial, todas las aplicaciones de Power BI son gratuitas y están disponibles para su descarga desde AppSource. Por este motivo, no se puede definir unidades de referencia de almacén (SKU) para este tipo de oferta.

![La pestaña de información técnica](./media/technical-info-tab.png)


## <a name="technical-info-fields"></a>Campos de Información técnica 

En el **información técnica** pestaña, complete los campos descritos en la tabla siguiente. Un asterisco (*) al final de una etiqueta de campo significa que el campo es obligatorio.

|        Campo          |  DESCRIPCIÓN                                                                 |
|    ---------------    |  ----------------------------------------------------------------------------|
| **Dirección URL del instalador\***     | Power BI genera esta dirección URL cuando se publique la aplicación y pasar a producción.  Para obtener más información, consulte [publicar aplicaciones con los paneles e informes en Power BI](https://docs.microsoft.com/power-bi/service-create-distribute-apps).  |
|  **Validation instructions** (Instrucciones de validación)  |  Si lo desea, agregue instrucciones (hasta 3000 caracteres) para ayudar al equipo de validación de Microsoft configurar, conectar y probar la aplicación. Incluir opciones de configuración típica, cuentas, parámetros u otra información que puede utilizarse para probar la opción de conexión de datos. Esta información solo es visible para el equipo de validación y se usa solo con fines de validación.  |
| **¿Se ha creado esta aplicación como un paquete de contenido de Power BI?** | Actualmente, este campo se utiliza sólo internamente. Deje la configuración predeterminada de **No**. Si cambia la configuración a **Sí**, se pudo detener el proceso de publicación.  |  
|  |  |


## <a name="next-steps"></a>Pasos siguientes

En el [detalles del escaparate electrónico](./cpp-storefront-details-tab.md) pestaña, proporcione la información de marketing y legal de la aplicación.

