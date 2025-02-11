---
title: Creación de una instancia de Deep Learning Data Science Virtual Machine
titleSuffix: Azure
description: Configure y cree una instancia de Deep Learning Data Science Virtual Machine en Azure para realizar análisis y aprendizaje automático.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 318df03c7c4447d051dfa396098462c0f8bbf423
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410435"
---
# <a name="provision-a-deep-learning-virtual-machine-on-azure"></a>Aprovisionamiento de Deep Learning Virtual Machine en Azure 

Deep Learning Virtual Machine (DLVM) es una variante configurada especialmente de la popular [Data Science Virtual Machine](https://aka.ms/dsvm) (DSVM) para que resulten más fáciles de usar instancias de máquinas virtuales basadas en GPU para entrenar rápidamente modelos de aprendizaje profundo. Es compatible con Windows 2016 o con DSVM de Ubuntu como base. DLVM comparte las mismas imágenes de máquina virtual principal y, por tanto, todo el conjunto de herramientas que está disponible en DSVM. 

DLVM contiene varias herramientas para AI, incluidas las ediciones de GPU de las plataformas de aprendizaje profundo populares, como Microsoft Cognitive Toolkit, TensorFlow, Keras, Caffe2, Chainer; herramientas para adquirir y procesar previamente imágenes, datos de texto, herramientas para las actividades de modelado y desarrollo de ciencia de datos como Microsoft R Server Developer Edition, Anaconda Python, Jupyter Notebook para Python y R, IDE para Python y R, bases de datos SQL y muchas otras herramientas de ciencia de datos y de aprendizaje automático. 

## <a name="create-your-deep-learning-virtual-machine"></a>Creación de una instancia de Deep Learning Virtual Machine
A continuación le indicamos los pasos para crear una instancia de Deep Learning Virtual Machine: 

1. Navegue a la lista de máquinas virtuales en [Azure Portal](https://portal.azure.com/#create/microsoft-ads.dsvm-deep-learningtoolkit
).
2. Seleccione el botón **Crear**, en la parte inferior, para acceder a un asistente.![create-dlvm](./media/dlvm-provision-wizard.PNG)
3. El asistente usado para crear la DLVM necesita **datos de entrada** para cada uno de los **cuatro pasos** que se enumeran en la parte derecha de esta ilustración. Estas son las entradas necesarias para configurar cada uno de estos pasos:

   <a name="basics"></a>   
   1. **Aspectos básicos**
      
      1. **Nombre**: nombre del servidor de ciencia de datos que está creando.
      2. **Seleccione el tipo de sistema operativo para la máquina virtual de aprendizaje profundo**: elija Windows o Linux (para Windows 2016 y DSVM con base Ubuntu Linux)
      2. **Nombre de usuario**: identificador de inicio de sesión de la cuenta del administrador.
      3. **Contraseña**: contraseña de la cuenta de administrador.
      4. **Suscripción**: Si tiene más de una suscripción, seleccione aquella en la que se creará y facturará la máquina.
      5. **Grupo de recursos**: puede crear uno nuevo o usar un grupo de recursos **vacío** existente de Azure en su suscripción.
      6. **Ubicación**: seleccione el centro de datos más adecuado. Normalmente es el centro de datos que tenga la mayoría de los datos o que esté más cercano a su ubicación física para un acceso más rápido a la red. 
      
      > [!NOTE]
      > DLVM admite todas las instancias de máquina virtual GPU de las series NC y ND. Al aprovisionar DLVM, debe elegir una de las ubicaciones de Azure que tenga GPU. Consulte la página [Productos disponibles por región](https://azure.microsoft.com/regions/services/) de Azure para ver las ubicaciones disponibles y busque **Serie NC**, **Serie NCv2**, **Serie NCv3** o **Serie ND** en **Proceso**. 

   1. **Configuración**: seleccione uno de los tamaños de máquina virtual GPU de las series NC (NC, NCv2, NCv3) o ND que satisfaga sus requisitos funcionales y las restricciones de costo. Cree una cuenta de almacenamiento para su máquina virtual.  ![dlvm-settings](./media/dlvm-provision-step-2.PNG)
   
   1. **Resumen**: Compruebe que toda la información que ha especificado es correcta.

   1. **Comprar**: haga clic en **Comprar** para iniciar el aprovisionamiento. Se proporciona un vínculo a los términos de la transacción. La máquina virtual no tiene ningún cargo adicional más allá del proceso para el tamaño del servidor que eligió en el paso **Tamaño** . 

> [!NOTE]
> El aprovisionamiento tardará entre 10 y 20 minutos. El estado del aprovisionamiento se muestra en el Portal de Azure.
> 


## <a name="how-to-access-the-deep-learning-virtual-machine"></a>Acceso a Deep Learning Virtual Machine

### <a name="windows-edition"></a>Edición de Windows
Una vez creada la máquina virtual, puede usar el escritorio remoto con las credenciales de la cuenta del administrador que configuró en la sección **Aspectos básicos** anterior. 

### <a name="linux-edition"></a>Edición de Linux

Después de crear la máquina virtual, puede iniciar sesión en ella mediante SSH. Use las credenciales de cuenta que creó en el [ **Fundamentos** ](#basics) sección del paso 3 para la interfaz de shell de texto. Para obtener más información sobre las conexiones de SSH para máquinas virtuales de Azure, consulte [instalar y configurar Escritorio remoto para conectarse a una VM Linux en Azure](/azure/virtual-machines/linux/use-remote-desktop). En un cliente de Windows, puede descargar una herramienta de cliente SSH como [Putty](https://www.putty.org). Si prefiere un escritorio gráfico (X Windows System), puede usar el reenvío de X11 en Putty o instalar el cliente X2Go. 

> [!NOTE]
> El cliente X2Go ha tenido un mejor rendimiento que el reenvío de X11 durante las pruebas. Por lo tanto, se recomienda usar el cliente X2Go para la interfaz gráfica de escritorio.
> 
> 

#### <a name="installing-and-configuring-x2go-client"></a>Instalación y configuración del cliente X2Go
La máquina virtual de aprendizaje profundo Linux ya está provista del servidor X2Go y está preparada para aceptar conexiones de cliente. Para conectarse al escritorio gráfico de la máquina virtual Linux, lleve a cabo el siguiente procedimiento en el cliente:

1. Descargue e instale el cliente X2Go para su plataforma cliente desde [aquí](https://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Ejecute el cliente X2Go y seleccione **Nueva sesión**. Se abrirá una ventana de configuración con varias pestañas. Escriba los siguientes parámetros de configuración:
   * **Pestaña Sesión**:
     * **Host**: nombre de host o dirección IP de la Linux Data Science Virtual Machine.
     * **Inicio de sesión**: nombre de usuario en la máquina virtual Linux.
     * **Puerto SSH**: déjelo en 22, el valor predeterminado.
     * **Tipo de sesión**: cambie el valor a **XFCE**. Actualmente, la máquina virtual de aprendizaje profundo Linux solo admite el escritorio XFCE.
   * **Pestaña Multimedia**: puede desactivar la compatibilidad de sonido y la impresión en el cliente si no necesita usarlas.
   * **Carpetas compartidas**: si quiere que los directorios de las máquinas cliente se monten en la VM Linux, agregue en esta pestaña los directorios de máquina cliente que quiere compartir con la VM.

Una vez que inicie sesión en la máquina virtual mediante el cliente SSH o el escritorio gráfico XFCE a través del cliente X2Go, ya podrá empezar a usar las herramientas que están instaladas y configuradas en la máquina virtual. En XFCE, puede ver accesos directos del menú de aplicaciones e iconos de escritorio para muchas de las herramientas.

Una vez creada y aprovisionada la máquina virtual, está listo para comenzar a usar las herramientas que se instalan y configuran en ella. Hay iconos del menú de inicio e iconos del escritorio para muchas de las herramientas. 
