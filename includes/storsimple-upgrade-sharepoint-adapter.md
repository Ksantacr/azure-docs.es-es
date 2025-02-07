---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: b2dec95e0258933b50d4437f1cb317639b62883d
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155845"
---
### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>Actualización de SharePoint 2010 a SharePoint 2013 e instalación del Adaptador de StorSimple para SharePoint
> [!IMPORTANT]
> Los archivos migrados previamente al almacenamiento externo mediante RBS no estarán disponibles hasta que finalice la actualización y se vuelva a habilitar la característica RBS. Para limitar el impacto en el usuario, realice cualquier actualización o reinstalación durante una ventana de mantenimiento programada.
> 
> 

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>Para actualizar SharePoint 2010 a SharePoint 2013 y, a continuación, instalar el adaptador
1. En la granja de SharePoint 2010, anote la ruta de acceso de almacenamiento de blobs para los blobs externalizados y las bases de datos de contenido para las que RBS está habilitado. 
2. Instale y configure la nueva granja de SharePoint 2013. 
3. Mueva las bases de datos, aplicaciones y colecciones de sitios de la granja SharePoint 2010 a la nueva granja de SharePoint 2013. Para obtener instrucciones, vaya a [Información general del proceso de actualización a SharePoint 2013](https://technet.microsoft.com/library/cc262483.aspx).
4. Instale el adaptador de StorSimple para SharePoint en una nueva granja. Vaya a [Instalación del adaptador de StorSimple para SharePoint](#install-the-storsimple-adapter-for-sharepoint) para ver los procedimientos.
5. Con la información que anotó en el paso 1, habilite RBS para el mismo conjunto de bases de datos de contenido y proporcione la misma ruta de almacén de blobs que se usó en la instalación de SharePoint 2010. Vaya a [Configuración de RBS](#configure-rbs) para ver los procedimientos. Después de completar este paso, los archivos anteriormente externalizados deben ser accesibles desde la nueva granja. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>Actualización del adaptador de StorSimple para SharePoint
> [!IMPORTANT]
> Debe programar esta actualización para que se produzca durante una ventana de mantenimiento planeada por las razones siguientes:
> 
> * El contenido anteriormente externalizado no estará disponible hasta que se vuelva a instalar el adaptador.
> * Cualquier contenido cargado en el sitio después de desinstalar la versión anterior del adaptador de StorSimple para SharePoint, pero antes de instalar la nueva versión, se almacenará en la base de datos de contenido. Necesitará mover ese contenido al dispositivo de StorSimple después de instalar el nuevo adaptador. Puede usar Microsoft `RBS Migrate()` cmdlet de PowerShell incluido con SharePoint para migrar el contenido. Para obtener más información, vea [migrar contenido dentro o fuera de RBS](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>Para actualizar el adaptador de StorSimple para SharePoint
1. Desinstale la versión anterior del adaptador de StorSimple para SharePoint.
   
   > [!NOTE]
   > Esto deshabilitará automáticamente RBS en las bases de datos de contenido. Sin embargo, los blobs existentes permanecerán en el dispositivo de StorSimple. Dado que RBS se deshabilita y no se han migrado los blobs a las bases de datos de contenido, se producirá un error en todas las solicitudes de esos blobs. 
   > 
   > 
2. Instale el nuevo adaptador de StorSimple para SharePoint. El nuevo adaptador reconocerá automáticamente las bases de datos de contenido que se hayan habilitado o deshabilitados para RBS y usará la configuración anterior.

