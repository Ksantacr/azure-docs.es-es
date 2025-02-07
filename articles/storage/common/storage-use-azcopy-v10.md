---
title: Copiar o mover datos a Azure Storage mediante AzCopy v10 | Microsoft Docs
description: AzCopy es una utilidad de línea de comandos que puede usar para copiar datos a, desde o entre cuentas de almacenamiento. En este artículo le ayuda a descargar AzCopy, conectarse a su cuenta de almacenamiento y, a continuación, transferir archivos.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: cc65d6d3f7e7dcc08ea29ecc8a299b556563135b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236300"
---
# <a name="get-started-with-azcopy"></a>Introducción a AzCopy

AzCopy es una utilidad de línea de comandos que puede usar para copiar los blobs o archivos a o desde una cuenta de almacenamiento. En este artículo le ayuda a descargar AzCopy, conectarse a su cuenta de almacenamiento y, a continuación, transferir archivos.

> [!NOTE]
> AzCopy **V10** es la versión actualmente admitida de AzCopy.
>
> Si necesita usar AzCopy **v8.1**, consulte el [usar la versión anterior de AzCopy](#previous-version) sección de este artículo.

<a id="download-and-install-azcopy" />

## <a name="download-azcopy"></a>Descarga de AzCopy

En primer lugar, descargue el archivo ejecutable AzCopy V10 a cualquier carpeta del equipo. Para mayor comodidad, considere la posibilidad de agregar la ubicación de carpeta de AzCopy a la ruta del sistema para facilitar su uso.

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

> [!NOTE]
> Si desea copiar datos desde y hacia su [de Azure Table storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) de servicio y volver a instalar [AzCopy versión 7.3](https://aka.ms/downloadazcopynet).

## <a name="run-azcopy"></a>Ejecución de AzCopy

Desde un símbolo del sistema, desplácese al directorio donde descargó el archivo.

Para ver una lista de comandos de AzCopy, escriba `azCopy`y, a continuación, presione la tecla ENTRAR.

Para obtener más información sobre un comando específico, escriba `azCopy` seguido del nombre del comando.

Por ejemplo, para obtener información sobre la `copy` comando, escriba `azcopy copy`y, a continuación, presione la tecla ENTRAR.

Antes de hacer nada significativo con AzCopy, debe decidir cómo proporcionará las credenciales de autorización para el servicio de almacenamiento.

## <a name="choose-how-youll-provide-authorization-credentials"></a>Elegir cómo deberá proporcionar las credenciales de autorización

Puede proporcionar las credenciales de autorización mediante el uso de Azure Active Directory (AD), o mediante un token de firma de acceso compartido (SAS).

Use esta tabla como guía:

| Tipo de almacenamiento | Método admitido actualmente de autorización |
|--|--|
|**Blob Storage** | Azure AD & SAS |
|**Almacenamiento de blobs (espacio de nombres de series temporales jerárquicas)** | Solo Azure AD |
|**Almacenamiento de archivos** | SAS solo |

### <a name="option-1-use-azure-ad"></a>Opción 1: Usar Azure AD

El nivel de autorización que necesita se basa en si va a cargar los archivos o simplemente descargarlos.

#### <a name="authorization-to-upload-files"></a>Autorización para cargar archivos

Compruebe que uno de estos roles se ha asignado a su identidad:

- [Colaborador de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Propietario de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Estos roles pueden asignarse a la identidad en cualquiera de estos ámbitos:

- Contenedor (sistema de archivos)
- Cuenta de almacenamiento
- Grupos de recursos
- Subscription

Para obtener información sobre cómo comprobar y asignar roles, consulte [conceder acceso a datos blob y cola de Azure con RBAC en Azure portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

No es necesario tener uno de estos roles asignados a la identidad si la identidad se agrega a la lista de control de acceso (ACL) de la carpeta o el contenedor de destino. En la ACL, su identidad necesita permiso de escritura en la carpeta de destino y permiso de ejecución en el contenedor y cada carpeta principal.

Para obtener más información, consulte [control de acceso en Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authorization-to-download-files"></a>Autorización para descargar archivos

Compruebe que uno de estos roles se ha asignado a su identidad:

- [Lector de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader)
- [Colaborador de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Propietario de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Estos roles pueden asignarse a la identidad en cualquiera de estos ámbitos:

- Contenedor (sistema de archivos)
- Cuenta de almacenamiento
- Grupos de recursos
- Subscription

Para obtener información sobre cómo comprobar y asignar roles, consulte [conceder acceso a datos blob y cola de Azure con RBAC en Azure portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

No es necesario tener uno de estos roles asignados a la identidad si la identidad se agrega a la lista de control de acceso (ACL) de la carpeta o el contenedor de destino. En la ACL, su identidad necesita permiso de lectura en la carpeta de destino y permiso de ejecución en el contenedor y cada carpeta principal.

Para obtener más información, consulte [control de acceso en Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authenticate-your-identity"></a>Autenticar la identidad

Después de haber comprobado que la identidad se ha dado el nivel de autorización necesaria, abra un símbolo del sistema, escriba el siguiente comando y, a continuación, presione la tecla ENTRAR.

```azcopy
azcopy login
```

Este comando devuelve un código de autenticación y la dirección URL de un sitio Web. Abra el sitio Web, proporcione el código y, a continuación, elija el **siguiente** botón.

![Crear un contenedor](media/storage-use-azcopy-v10/azcopy-login.png)

Aparecerá una ventana de inicio de sesión. En esa ventana, inicie sesión en su cuenta de Azure con sus credenciales de cuenta de Azure. Cuando haya iniciado sesión correctamente, puede cerrar la ventana del explorador y empezar a usar AzCopy.

### <a name="option-2-use-a-sas-token"></a>Opción 2: Uso de un token de SAS

Puede anexar un token de SAS a cada dirección URL de origen o destino que usan en los comandos de AzCopy.

Recursivamente ejecutando comandos en el ejemplo copia datos desde un directorio local en un contenedor de blobs. Un token de SAS ficticio se anexa al final de la de la dirección URL del contenedor.

```azcopy
azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1/?sv=2018-03-28&ss=bjqt&srt=sco&sp=rwddgcup&se=2019-05-01T05:01:17Z&st=2019-04-30T21:01:17Z&spr=https&sig=MGCXiyEzbtttkr3ewJIh2AR8KrghSy1DGM9ovN734bQF4%3D" --recursive=true
```

Para obtener más información sobre los tokens de SAS y cómo obtener uno, vea [mediante firmas de acceso compartido (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="transfer-files"></a>Transferencia de archivos

Después de autenticar la identidad u obtenido un token SAS, puede comenzar la transferencia de archivos.

Para obtener ejemplos de comandos, consulte cualquiera de estos artículos.

- [Transferencia de datos con AzCopy y blob storage](storage-use-azcopy-blobs.md)

- [Transferencia de datos con AzCopy y file storage](storage-use-azcopy-files.md)

- [Transferencia de datos con AzCopy y Amazon S3 depósitos](storage-use-azcopy-s3.md)

## <a name="configure-optimize-and-troubleshoot-azcopy"></a>Configurar, optimizar y solucionar problemas de AzCopy

Consulte [configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)

## <a name="use-azcopy-in-storage-explorer"></a>Uso de AzCopy en el Explorador de almacenamiento

Si desea aprovechar las ventajas de rendimiento de AzCopy, pero prefiere usar el Explorador de Storage en lugar de la línea de comandos para interactuar con los archivos, a continuación, habilite AzCopy en el Explorador de almacenamiento.

En el Explorador de Storage, elija **Preview**->**utilizar AzCopy para descarga y carga de blobs mejorado**.

![Habilitar AzCopy como un motor de transferencia en el Explorador de Storage de Azure](media/storage-use-azcopy-v10/enable-azcopy-storage-explorer.jpg)

> [!NOTE]
> No debe habilitar a esta opción si ha habilitado un espacio de nombres jerárquico en su cuenta de almacenamiento. Eso es porque el Explorador de almacenamiento utiliza automáticamente AzCopy en cuentas de almacenamiento que tienen un espacio de nombres jerárquico.  

<a id="previous-version" />

## <a name="use-the-previous-version-of-azcopy"></a>Usar la versión anterior de AzCopy

Si necesita usar la versión anterior de AzCopy (AzCopy v8.1), consulte cualquiera de los siguientes vínculos:

- [AzCopy en Windows (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy)
- [AzCopy en Linux (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux)

## <a name="next-steps"></a>Pasos siguientes

Si tiene preguntas, problemas o comentarios generales, enviarlas [en GitHub](https://github.com/Azure/azure-storage-azcopy) página.
