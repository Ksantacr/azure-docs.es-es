---
title: Instalación de actualizaciones en StorSimple Virtual Array | Microsoft Docs
description: Describe cómo usar la IU web de StorSimple Virtual Array para aplicar actualizaciones mediante Azure Portal y el método de revisiones
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/07/2017
ms.author: alkohli
ms.openlocfilehash: b67fcb82bdcc94d7faeceedb7420a869e6578cad
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61436467"
---
# <a name="install-update-04-on-your-storsimple-virtual-array"></a>Instalación de Update 0.4 en StorSimple Virtual Array

## <a name="overview"></a>Información general

En este artículo se describen los pasos requeridos para instalar Update 0.4 en StorSimple Virtual Array a través tanto de la IU web local como de Azure Portal. Deberá aplicar alguna actualización o revisión de software para mantener actualizada la matriz virtual de StorSimple. 

Tenga en cuenta que al instalar una actualización o revisión, se reiniciará el dispositivo. Dado que la matriz virtual de StorSimple es un dispositivo de nodo único, se interrumpirá cualquier operación de E/S que esté en curso y el dispositivo permanecerá un rato inactivo. 

Antes de aplicar una actualización, se recomienda que desconecte primero los volúmenes o recursos compartidos en el host y, luego, el dispositivo. Esto minimizará la posibilidad de daños en los datos.

> [!IMPORTANT]
> Si está ejecutando Update 0.1 o versiones de software de GA, debe utilizar el método de revisión a través de la interfaz de usuario web local para instalar Update 0.3. Si ejecuta Update 0.2, o cualquier versión posterior, se recomienda instalar las actualizaciones a través de Azure Portal.
 

## <a name="use-the-local-web-ui"></a>Uso de la interfaz de usuario web local

Se pueden seguir dos pasos con la interfaz de usuario web local:

* Descargar la actualización o la revisión
* Instalar la actualización o la revisión

### <a name="download-the-update-or-the-hotfix"></a>Descargar la actualización o la revisión

Realice los pasos siguientes para descargar la actualización de software desde el catálogo de Microsoft Update.

#### <a name="to-download-the-update-or-the-hotfix"></a>Para descargar la actualización o la revisión

1. Inicie Internet Explorer y vaya a [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com).

2. Si esta es la primera vez que utiliza el Catálogo de Microsoft Update en este equipo, haga clic en **Instalar** cuando se le solicite que instale el complemento del Catálogo de Microsoft Update.

3. En el cuadro de búsqueda del Catálogo de Microsoft Update, escriba el número de Knowledge Base correspondiente a la revisión que quiera descargar. Escriba **3216577** para Update 0.4 y, luego, haga clic en **Buscar**.
   
    Aparece la lista de revisiones, por ejemplo, **StorSimple Virtual Array Update 0.4**.
   
    ![Catálogo de búsqueda](./media/storsimple-virtual-array-install-update-04/download1.png)

4. Haga clic en **Agregar**. La actualización se agrega a la cesta.

5. Haga clic en **Ver cesta**.

6. Haga clic en **Descargar**. Especifique o **busque** una ubicación local en la que quiera que aparezcan las descargas. Las actualizaciones se descargan en la ubicación especificada y se colocan en una subcarpeta con el mismo nombre que la actualización. La carpeta también se puede copiar en un recurso compartido de red que sea accesible desde el dispositivo.

7. Abra la carpeta copiada, debería ver un archivo de paquete independiente de Microsoft Update `WindowsTH-KB3011067-x64`. Este archivo se utiliza para instalar la actualización o revisión.

### <a name="install-the-update-or-the-hotfix"></a>Instalar la actualización o la revisión

Antes de instalar la actualización o la revisión, asegúrese de que tiene la actualización o la revisión descargada de forma local en el host o que puede acceder a ella a través de un recurso compartido de red. 

Utilice este método para instalar actualizaciones en un dispositivo que ejecute las versiones de software Update 0.1 o GA. Este procedimiento tarda menos de 2 minutos en completarse. Realice los pasos siguientes para instalar la actualización o revisión.

#### <a name="to-install-the-update-or-the-hotfix"></a>Instalar la actualización o la revisión

1. En la interfaz de usuario web local, vaya a **Mantenimiento** > **Actualización de software**.
   
    ![actualizar dispositivo](./media/storsimple-virtual-array-install-update/update1m.png)

2. En **Update file path**(Ruta de acceso del archivo de actualización), escriba el nombre del archivo de actualización o de revisión. Asimismo, también puede acceder al archivo de instalación de la actualización o de la revisión si está en un recurso compartido de red. Haga clic en **Aplicar**.
   
    ![actualizar dispositivo](./media/storsimple-virtual-array-install-update/update2m.png)

3. Se mostrará una advertencia. Dado que se trata de un dispositivo de nodo único, una vez aplicada la actualización, se reiniciará el dispositivo y habrá un tiempo de inactividad. Haga clic en el icono de marca de verificación.
   
   ![actualizar dispositivo](./media/storsimple-virtual-array-install-update/update3m.png)

4. Se inicia la actualización. Una vez que el dispositivo se actualice correctamente, este se reiniciará. La interfaz de usuario local no será accesible durante este tiempo.
   
    ![actualizar dispositivo](./media/storsimple-virtual-array-install-update/update5m.png)

5. Una vez completado el reinicio, se le llevará a la página de **inicio de sesión** . Para comprobar que el software del dispositivo se ha actualizado, en la interfaz de usuario de web local, vaya a **Mantenimiento** > **Actualización de software**. La versión de software mostrada debe ser **10.0.0.0.0.10289.0** en el caso de Update 0.4.
   
   > [!NOTE]
   > Las versiones de software se muestran de forma ligeramente distinta en la interfaz de usuario web local y Azure Portal. Por ejemplo, la misma versión aparece como **10.0.0.0.0.10289** en la IU web local y como **10.0.10289.0** en Azure Portal.
   
    ![actualizar dispositivo](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-the-azure-portal"></a>Uso de Azure Portal

Si se ejecuta Update 0.2, o cualquier versión posterior, se recomienda instalar las actualizaciones mediante Azure Portal. El procedimiento del Portal requiere que el usuario examine, descargue e instale las actualizaciones. Este procedimiento tarda aproximadamente 7 minutos en completarse. Realice los pasos siguientes para instalar la actualización o revisión.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Una vez que la instalación esté completa (podrá comprobarlo cuando el estado del trabajo esté al 100 %), vaya a su servicio StorSimple Device Manager. Seleccione **Dispositivos** y haga clic en el dispositivo que desea actualizar en la lista de dispositivos conectados a este servicio. En la hoja **Configuración**, vaya a la sección **Administrar** y seleccione **Actualizaciones del dispositivo**. La versión de software que se muestra debe ser **10.0.10289.0**.


## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la [administración de la matriz virtual de StorSimple](storsimple-ova-web-ui-admin.md).

