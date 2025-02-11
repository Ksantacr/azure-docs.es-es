---
title: Tutorial sobre la preparación de Azure portal para la implementación de Data Box Gateway | Microsoft Docs
description: El primer tutorial para implementar Azure Data Box Gateway trata sobre la preparación de Azure Portal.
services: databox-edge-gateway
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: alkohli
ms.openlocfilehash: f9650cdb6935fb45f0c59e8a114a9ce1c8e2d809
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64686530"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-gateway"></a>Tutorial: Preparación de la implementación de Azure Data Box Gateway


Este es el primer tutorial de la serie de tutoriales de implementación necesarios para implementar completamente Azure Data Box Gateway. En este tutorial se describe cómo preparar Azure Portal para implementar recursos de Data Box Gateway. 

Para completar el proceso de instalación y configuración se necesitan privilegios de administrador. La preparación del portal dura menos de 10 minutos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un nuevo recurso
> * Descarga de la imagen del dispositivo virtual
> * Obtención de la clave de activación

## <a name="get-started"></a>Introducción

Para implementar Data Box Gateway, consulte los siguientes tutoriales en el orden indicado.

| **#** | **En este paso** | **Use estos documentos** |
| --- | --- | --- | 
| 1. |**[Preparación de Azure Portal para Data Box Gateway](data-box-gateway-deploy-prep.md)** |Cree y configure el recurso de Data Box Gateway antes de aprovisionar un dispositivo virtual de Data Box Gateway. |
| 2. |**[Aprovisionamiento de Azure Data Box Gateway en Hyper-V](data-box-gateway-deploy-provision-hyperv.md)** <br><br><br>**[Aprovisionamiento de Azure Data Box Gateway en VMware](data-box-gateway-deploy-provision-vmware.md)**|Para Hyper-V, aprovisione un dispositivo virtual de Data Box Gateway en un sistema host que ejecuta Hyper-V en Windows Server 2016 o Windows Server 2012 R2, y conéctese a él. <br><br><br> En VMware, aprovisione un dispositivo virtual de Data Box Gateway en un sistema host que ejecuta VMware ESXi 6.0, 6.5 o 6.7, y conéctese a él.<br></br> |
| 3. |**[Conexión, configuración y activación de Data Box Gateway](data-box-gateway-deploy-connect-setup-activate.md)** |Conéctese a la interfaz de usuario web local, complete la configuración del dispositivo y active el dispositivo. A continuación, puede aprovisionar los recursos compartidos de SMB.  |
| 4. |**[Transferencia de datos con Data Box Gateway](data-box-gateway-deploy-add-shares.md)** |Agregue recursos compartidos y conéctese a ellos mediante SMB o NFS. |

Ya puede empezar a configurar Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Aquí encontrará los requisitos previos para configurar su recurso de Data Box Gateway, el dispositivo Data Box Gateway y la red de centros de datos.

### <a name="for-the-data-box-gateway-resource"></a>Para el recurso Data Box Gateway

Antes de comenzar, asegúrese de que:

- El recurso de Data Box Gateway debe admitir su suscripción de Microsoft Azure. No se admiten suscripciones de pago por uso.
- Debe tener acceso de colaborador o propietario a la suscripción.
- Debe tener acceso de administrador o usuario a Graph API de Azure Active Directory. Para más información, vea [Graph API de Azure Active Directory](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).
- Tiene una cuenta de almacenamiento de Microsoft Azure con credenciales de acceso.

### <a name="for-the-data-box-gateway-device"></a>Para el dispositivo de Data Box Gateway

Antes de implementar un dispositivo virtual, asegúrese de que:

- Tiene acceso a un sistema host que ejecuta Hyper-V en Windows Server 2012 R2 o versiones posteriores o VMware (ESXi 6.0, 6.5 o 6.7) que puede usarse para aprovisionar un dispositivo.
- El sistema host puede dedicar los recursos siguientes para aprovisionar el dispositivo virtual Data Box:
  
  - Un mínimo de 4 procesadores virtuales.
  - Al menos 8 GB de RAM.
  - Una interfaz de red.
  - Un disco de sistema operativo de 250 GB.
  - Un disco virtual de 2 TB para datos del sistema.

### <a name="for-the-datacenter-network"></a>Para la red del centro de datos

Antes de comenzar, asegúrese de que:

- La red del centro de datos está configurada según los requisitos de red para el dispositivo de Data Box Gateway. Para más información, consulte [Requisitos del sistema de Data Box Gateway](data-box-gateway-system-requirements.md).

- Para que Data Box Edge funcione normalmente, debe disponer de:

    - Un ancho de banda mínimo de 10 Mbps para garantizar que el dispositivo se mantiene actualizado.
    - Un ancho de banda mínimo de 20 Mbps dedicado a carga y descarga para transferir archivos.

## <a name="create-a-new-resource"></a>Crear un nuevo recurso

Si ya tiene un recurso de Data Box Gateway para administrar los dispositivos virtuales, omita este paso y vaya a [Obtención de la clave de activación](#get-the-activation-key).

Siga estos pasos en Azure Portal para crear un recurso de Data Box Gateway.

1. Use sus credenciales de Microsoft Azure para iniciar sesión en:

    - Azure Portal en esta dirección URL: [https://portal.azure.com](https://portal.azure.com).
    - O bien, el portal de Azure Government en esta dirección URL: [https://portal.azure.us](https://portal.azure.us). Para más información, vaya a [Connect to Azure Government using the portal](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-portal) (Conexión a Azure Government mediante el portal).

2. En el menú izquierdo, seleccione **+ Crear un recurso**. Busque **Data Box Edge/Data Box Gateway**. Seleccione Data Box Edge/Data Box Gateway. Seleccione **Crear**.
3. Seleccione la suscripción que desea usar para el dispositivo Data Box Gateway. Seleccione la región donde quiere implementar el recurso Data Box Gateway. En esta versión, están disponibles las regiones de Este de EE. UU., Sudeste Asiático y Europa Occidental. Elija la ubicación más cercana a la región geográfica donde quiera implementar el dispositivo. En la opción **Data Box Gateway**, seleccione **Crear**.

    ![Busque el servicio Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-edge-sku.png)

4. En la pestaña **Datos básicos**, escriba o seleccione los siguientes **detalles del proyecto**.
    
    |Configuración  |Valor  |
    |---------|---------|
    |Subscription    |Este valor se rellena automáticamente según la selección anterior. La suscripción está vinculada a la cuenta de facturación. |
    |Grupos de recursos  |Cree un nuevo grupo o seleccione uno existente.<br>Más información sobre los [grupos de recursos de Azure](../azure-resource-manager/resource-group-overview.md).     |

5. Escriba o seleccione los siguientes **detalles de la instancia**.

    |Configuración  |Valor  |
    |---------|---------|
    |NOMBRE   | Nombre descriptivo que identifique el recurso.<br>El nombre tiene entre 2 y 50 caracteres que contiene letras, números y guiones.<br> El nombre comienza y termina con una letra o un número.        |   
    |Region     |En esta versión, están disponibles las regiones de Este de EE. UU., Sudeste Asiático y Europa Occidental para implementar el recurso. En cuanto a Azure Government, todas las regiones que aparecen en la lista de [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/) están disponibles. <br> Elija la ubicación más cercana a la región geográfica donde quiera implementar el dispositivo.|
    
    ![Creación de un recurso de Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-resource.png)
    
6. Seleccione **Revisar + crear**.
 
7. En la pestaña **Revisar y crear**, revise la información de **Detalles de precios**, **Términos de uso** y los detalles de su recurso. Seleccione **Crear**.

    ![Revisión de los detalles del recurso de Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-resource1.png)

Se tarda unos minutos en crear el recurso. Cuando el recurso se haya creado e implementado correctamente, recibirá una notificación. Haga clic en **Go to resource** (Ir al recurso).

![Revisión de los detalles del recurso de Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-resource2.png)

## <a name="download-the-virtual-device-image"></a>Descarga de la imagen del dispositivo virtual

Una vez creado el recurso de Data Box Gateway, descargue la imagen correspondiente al dispositivo virtual para aprovisionar un dispositivo virtual en el sistema host. Las imágenes de dispositivo virtual son específicas de un sistema operativo.

> [!IMPORTANT]
> El software que se ejecuta en Data Box Gateway solo puede usarse con el recurso Data Box Gateway.

Siga estos pasos en [Azure Portal](https://portal.azure.com/) para descargar una imagen de dispositivo virtual.

1. En el recurso que ha creado, seleccione **Información general**. Si ya tiene un recurso de Azure Data Box Gateway, selecciónelo y vaya a **Información general**. Seleccione **Instalación del dispositivo**.

    ![Nuevo recurso de Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

2. En el icono **Descargar imagen**, seleccione la imagen de dispositivo virtual correspondiente al sistema operativo del servidor host que se usa para aprovisionar la máquina virtual. Los archivos de imagen tienen un tamaño aproximado de 5,6 GB.
   
   * [VHD para Hyper-V en Windows Server 2012 R2 y versiones posteriores](https://aka.ms/dbe-vhdx-2012).
   * [VMDK para VMWare ESXi 6.0, 6.5 o 6.7](https://aka.ms/dbe-vmdk).

    ![Descarga de la imagen de dispositivo virtual de Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-download-image.png)

5. Descargue y descomprima el archivo en una unidad local y tome nota de dónde se encuentra el archivo sin comprimir.


## <a name="get-the-activation-key"></a>Obtención de la clave de activación

Cuando el recurso de Data Box Gateway esté en funcionamiento, deberá obtener la clave de activación. Esta clave se utiliza para activar y conectar el dispositivo de Data Box Gateway con el recurso. Puede obtener esta clave ahora mientras está en Azure Portal.

1. Seleccione el recurso que ha creado y, a continuación, seleccione **Información general**. En **Instalación del dispositivo**, vaya al icono **Configurar y activar**.

    ![Icono Configurar y activar](media/data-box-gateway-deploy-prep/data-box-gateway-configure-activate.png)

2. Haga clic en **Generar clave** para crear una nueva clave de activación. Seleccione el icono de copia para copiar la clave y guárdela para su uso posterior.

    ![Obtención de la clave de activación](media/data-box-gateway-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - La clave de activación expira tres días después de generarla.
> - Si la clave ha expirado, genere una nueva. La clave antigua no es válida.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Gateway, como:

> [!div class="checklist"]
> * Crear un nuevo recurso
> * Descarga de la imagen del dispositivo virtual
> * Obtención de la clave de activación

Pase al siguiente tutorial para aprender cómo aprovisionar una máquina virtual para Data Box Gateway. Según el sistema operativo host, consulte las instrucciones detalladas en:

> [!div class="nextstepaction"]
> [Aprovisionamiento de Data Box Gateway en Hyper-V](./data-box-gateway-deploy-provision-hyperv.md)

OR

> [!div class="nextstepaction"]
> [Aprovisionamiento de Data Box Gateway en VMware](./data-box-gateway-deploy-provision-vmware.md)


