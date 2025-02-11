---
title: Conexión a servidores de Azure Analysis Services | Microsoft Docs
description: Aprenda a conectarse y obtener datos de un servidor de Analysis Services en Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 9a8863189ee9cb63d86b157c0bbebb6fd16116b0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61027990"
---
# <a name="connecting-to-servers"></a>Conexión a servidores

En este artículo se describe la conexión a un servidor mediante aplicaciones de modelado y administración de datos, como SQL Server Management Studio (SSMS) o SQL Server Data Tools (SSDT). O bien, con aplicaciones cliente de generación de informes, como Microsoft Excel, Power BI Desktop o aplicaciones personalizadas. Las conexiones a Azure Analysis Services usan HTTPS.

## <a name="client-libraries"></a>Bibliotecas de clientes

[Obtener las bibliotecas de cliente más recientes](analysis-services-data-providers.md)

Todas las conexiones a un servidor, con independencia del tipo, requieren las bibliotecas de cliente AMO, ADOMD.NET, y OLEDB actualizadas para conectarse a un servidor de Analysis Services e interaccionar con él. En el caso de SSMS, SSDT, Excel 2016 y versiones posteriores y Power BI, las bibliotecas de cliente más recientes se instalan o actualizan con versiones mensuales. Sin embargo, en algunos casos, es posible que una aplicación no tenga la más reciente. Por ejemplo, cuando las directivas retrasan las actualizaciones o las actualizaciones de Office 365 están en el canal diferido.

## <a name="server-name"></a>Nombre de servidor

Al crear un servidor de Analysis Services en Azure, especifique un nombre único y la región donde se creará el servidor. Al especificar el nombre del servidor en una conexión, el esquema de nombres del servidor es:

```
<protocol>://<region>/<servername>
```
 Donde protocol es la cadena **asazure**, region es el URI donde se creó el servidor (por ejemplo, westus.asazure.windows.net) y servername es el nombre de su servidor único dentro de la región.

### <a name="get-the-server-name"></a>Obtención del nombre del servidor

En **Azure Portal** > servidor > **Información general** > **Nombre de servidor**, copie el nombre del servidor completo. Si otros usuarios de su organización también se conectan a este servidor, podrá compartir dicho nombre con ellos. Al especificar un nombre de servidor, se debe usar la ruta de acceso completa.

![Obtención del nombre del servidor en Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

> [!NOTE]
> Es el protocolo para la región Este de EE.UU. 2 **aspaaseastus2**.

## <a name="connection-string"></a>Cadena de conexión

Al conectarse a Azure Analysis Services mediante el modelo de objetos tabular, utilice los siguientes formatos de cadena de conexión:

###### <a name="integrated-azure-active-directory-authentication"></a>Autenticación integrada de Azure Active Directory

La autenticación integrada selecciona la memoria caché de credenciales de Azure Active Directory, si está disponible. De lo contrario, se muestra la ventana de inicio de sesión de Azure.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Autenticación de Azure Active Directory con el nombre de usuario y la contraseña

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Autenticación de Windows (seguridad integrada)

Use la cuenta de Windows que ejecuta el proceso actual.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```

## <a name="connect-using-an-odc-file"></a>Conexión mediante un archivo .odc

Con versiones anteriores de Excel, los usuarios pueden conectarse a un servidor de Azure Analysis Services mediante un archivo de conexión de datos de Office (.odc). Para más información, consulte [Creación de un archivo de conexión de datos de Office (.odc)](analysis-services-odc.md).


## <a name="next-steps"></a>Pasos siguientes

[Connect with Excel](analysis-services-connect-excel.md)   (Conexión con Excel)  
[Connect with Power BI](analysis-services-connect-pbi.md)  (Conexión con Power BI)  
[Administración del servidor](analysis-services-manage.md)   

