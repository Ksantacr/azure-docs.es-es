---
title: Tutorial sobre conexión, configuración y activación de un dispositivo Azure Data Box Edge en Azure Portal | Microsoft Docs
description: En el tutorial para implementar Data Box Edge se proporcionan instrucciones para conectar, configurar y activar el dispositivo físico.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 3e951e44aeaa4ca8d3d06bae80a80bcb2eb1dd52
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924554"
---
# <a name="tutorial-connect-set-up-and-activate-azure-data-box-edge"></a>Tutorial: Conexión, configuración y activación de Azure Data Box Edge 

En este tutorial se describe cómo se puede conectar, configurar y activar el dispositivo Azure Data Box Edge mediante la interfaz de usuario web local.

El proceso de instalación y activación puede tardar unos 20 minutos en completarse.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Conectarse a un dispositivo físico
> * Configuración y activación del dispositivo físico

## <a name="prerequisites"></a>Requisitos previos

Antes de configurar e instalar el dispositivo Data Box Edge, asegúrese de que:

* Ha instalado el dispositivo físico como se detalla en [Instalación de Data Box Edge](data-box-edge-deploy-install.md).
* Tiene la clave de activación del servicio Data Box Edge que creó para administrar los dispositivos Data Box Edge. Para más información, consulte [Preparación de la implementación de Azure Data Box Edge](data-box-edge-deploy-prep.md).

## <a name="connect-to-the-local-web-ui-setup"></a>Conexión a la configuración de la interfaz de usuario web local 

1. Configure el adaptador Ethernet en el equipo para conectarse al dispositivo Data Box Edge con la dirección IP estática 192.168.100.5 y la subred 255.255.255.0.

2. Conecte el equipo a PUERTO 1 en el dispositivo. Use la siguiente ilustración para identificar el PUERTO 1 en el dispositivo.

    ![Placa posterior de un dispositivo cableado](./media/data-box-edge-deploy-install/backplane-cabled.png)


3. Abra una ventana del explorador y acceda a la interfaz de usuario web local del dispositivo en https://192.168.100.10.  
    Esta acción puede tardar unos minutos en completarse después de que haya activado el dispositivo. 

    Ve un error o advertencia que indica que hay un problema con el certificado de seguridad del sitio web. 
   
    ![Mensaje de error del certificado de seguridad del sitio web](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

4. Seleccione **Continue to this webpage**(Continuar a esta página web).  
    Estos pasos pueden variar en función del explorador que se use.

5. Inicie sesión en la interfaz de usuario web del dispositivo. La contraseña predeterminada es *Password1*. 
   
    ![Página de inicio de sesión del dispositivo Data Box Edge](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

6. En el aviso, cambie la contraseña del administrador de dispositivos.  
    Recuerde que la nueva contraseña debe tener entre 8 y 16 caracteres. Esta debe contener tres de los siguientes caracteres: mayúsculas, minúsculas, números y caracteres especiales.

Ahora está en el panel del dispositivo.

## <a name="set-up-and-activate-the-physical-device"></a>Configuración y activación del dispositivo físico
 
El panel muestra los distintos valores necesarios para configurar y registrar el dispositivo físico en el servicio Data Box Edge. Las opciones **Nombre de dispositivo**, **Configuración de red**, **Configuración de proxy web** y **Configuración horaria** son opcionales. Los únicos valores obligatorios son **Configuración de la nube**.
   
![Página "Panel" de la interfaz de usuario web local](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

1. En el panel izquierdo, seleccione **Nombre de dispositivo**y escriba un nombre descriptivo para el dispositivo.  
    Dicho nombre debe tener entre 1 y 15 caracteres, así como letras, números y guiones.

    ![Página de "Nombre de dispositivo" de la interfaz de usuario web local](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

2. (Opcional) En el panel izquierdo, seleccione **Configuración de red** y configure los distintos valores.  
    En el dispositivo físico hay seis interfaces de red. PUERTO 1 y PUERTO 2 son interfaces de red de 1 Gbps. PUERTO 3, PUERTO 4, PUERTO 5 y PUERTO 6 son interfaces de red de 25 Gbps que también pueden actuar como interfaces de red de 10 Gbps. PUERTO 1 se configura automáticamente como puerto solo de administración y PUERTO 2 a PUERTO 6 son los puertos de datos. La página **Configuración de red** es tal como se muestra a continuación.
    
    ![Página "Configuración de red" de la interfaz de usuario web local](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Al configurar la red, tenga en cuenta lo siguiente:

   - Si DHCP está habilitado en el entorno, las interfaces de red se configuran automáticamente. Se asignan automáticamente una dirección IP, la subred, la puerta de enlace y DNS.
   - Si DHCP no está habilitado, puede asignar direcciones IP estáticas si es necesario.
   - Puede configurar la interfaz de red como IPv4.

     >[!NOTE] 
     > Se recomienda no cambiar la dirección IP local de la interfaz de red de estática a DHCP, a menos que se tenga otra dirección IP para conectarse al dispositivo. Si se usara una interfaz de red y cambiara a DHCP, no habría forma de determinar la dirección DHCP. Si desea cambiar a una dirección DHCP, espere a que el dispositivo se haya registrado con el servicio y, después, realice el cambio. Después, puede ver las direcciones IP de todos los adaptadores de las **propiedades del dispositivo** de su servicio en Azure Portal.

3. (Opcional) En el panel izquierdo, seleccione **Configuración de proxy web** y, después, configure el servidor proxy web. Aunque la configuración del proxy web es opcional, si usa un proxy web, solo puede configurarlo aquí.
   
   ![Página "Configuración de proxy web" de la interfaz de usuario web local](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   En la página **Configuración de proxy web** haga lo siguiente:
   
    a. En el cuadro **URL de proxy web** , escriba la dirección URL en este formato: `http://host-IP address or FQDN:Port number`. No se admiten direcciones URL HTTPS.

   b. En **Autenticación**, seleccione **No** o **NTLM**.

   c. Si usa autenticación, especifique el nombre de usuario y la contraseña.

   d. Para validar y aplicar la configuración del proxy web configurados, seleccione **Aplicar configuración**.

4. (Opcional) En el panel izquierdo, seleccione **Configuración horaria** y, después, configure la zona horaria y los servidores NTP principal y secundario del dispositivo.  
    Se requieren servidores NTP, ya que el dispositivo debe sincronizar la hora para que pueda autenticarse con los proveedores de servicios en la nube.
       
    En la página **Configuración horaria** haga lo siguiente:
    
    1. En la lista desplegable **Zona horaria**, seleccione la correspondiente a la ubicación geográfica en que se va a implementar el dispositivo.
        La zona horaria predeterminada para el dispositivo es la hora del Pacífico (PST). El dispositivo usará esta zona horaria para todas las operaciones programadas.

    2. En el cuadro **Servidor NTP principal**, especifique el servidor principal del dispositivo o acepte el valor predeterminado de time.windows.com.  
        Asegúrese de que la red permite que el tráfico NTP pase del centro de datos a Internet.

    3. Si lo desea, en el cuadro **Servidor NTP secundario**, escriba un servidor secundario para el dispositivo.

    4. Seleccione **Aplicar configuración** para validar y aplicar la configuración horaria.

        ![Página "Configuración horaria" de la interfaz de usuario web local](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)

5. (Opcional) En el panel izquierdo, seleccione **Configuración de almacenamiento** para configurar la resistencia de almacenamiento en el dispositivo. Esta funcionalidad actualmente está en su versión preliminar. De manera predeterminada, el almacenamiento en el dispositivo no es resistente y hay pérdida de datos si se produce un error en un disco de datos del dispositivo. Cuando se habilita la opción resistente, se vuelve a configurar el almacenamiento en el dispositivo y el dispositivo puede resistir el error de un disco de datos sin pérdida de datos. La configuración del almacenamiento como resistente reducirá la capacidad utilizable del dispositivo.

    > [!IMPORTANT] 
    > La resistencia solo se puede configurar antes de activar el dispositivo. 

    ![Página "Configuración de almacenamiento" de la interfaz de usuario web local](./media/data-box-edge-deploy-connect-setup-activate/storage-settings.png)

6. En el panel izquierdo, seleccione **Configuración de la nube**  y active el dispositivo con el servicio Data Box Edge en Azure Portal.
    
    1. En el cuadro **Clave de activación**, escriba la que obtuvo en [Obtención de la clave de activación](data-box-edge-deploy-prep.md#get-the-activation-key) para Data Box Edge.
    2. Seleccione **Aplicar**.
       
        ![Página "Configuración de nube" de la interfaz de usuario web local](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)

    3. En primer lugar, se activa el dispositivo. A continuación, se analiza para comprobar si hay actualizaciones críticas y, en caso de que haya alguna disponible, se aplica automáticamente. Verá una notificación al respecto.

        El cuadro de diálogo también tiene una clave de recuperación que debe copiar y guardar en una ubicación segura. Esta clave se usa para recuperar los datos en caso de que el dispositivo no pueda iniciarse.

        ![Página "Configuración de nube" actualizada de la interfaz de usuario web local](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)

    4. Es posible que deba esperar varios minutos después de que la actualización se complete correctamente. La página se actualiza para indicar que el dispositivo se ha activado correctamente.

        ![Página "Configuración de nube" actualizada de la interfaz de usuario web local](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-8.png)

La instalación del dispositivo está completa. Ahora puede agregar recursos compartidos en el dispositivo.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * A conectarse a un dispositivo físico
> * Configuración y activación del dispositivo físico

Para aprender a transferir datos con Data Box Edge, consulte:

> [!div class="nextstepaction"]
> [Transferencia de datos con Data Box Edge](./data-box-edge-deploy-add-shares.md).
