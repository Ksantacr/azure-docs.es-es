---
title: Habilitación de Enterprise State Roaming en Azure Active Directory | Microsoft Docs
description: Preguntas más frecuentes sobre la configuración Enterprise State Roaming en dispositivos de Windows. Enterprise State Roaming proporciona a los usuarios una experiencia unificada a través de sus dispositivos de Windows y reduce el tiempo necesario para configurar un nuevo dispositivo.
services: active-directory
keywords: enterprise state roaming, nube de windows, cómo habilitar enterprise state roaming
documentationcenter: ''
author: tanning
manager: daveba
editor: curtand
ms.subservice: devices
ms.assetid: f71d66fd-7f9e-45eb-9cfe-5d989870f8a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: joflore
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88e825ebc08b4bfbd65f81b7b2480ead9be314b5
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954951"
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Habilitación de Enterprise State Roaming en Azure Active Directory
Enterprise State Roaming está disponible para cualquier organización con una licencia de Azure AD Premium o Enterprise Mobility + Security (EMS). Para más información acerca de cómo obtener una suscripción de Azure AD, consulte la [página de producto de Azure AD](https://azure.microsoft.com/services/active-directory).

Al habilitar Enterprise State Roaming, a su organización se le otorgará automáticamente una licencia gratuita de uso limitado para la protección con Azure Rights Management desde Azure Information Protection. Esta suscripción gratuita se limita a cifrar y descifrar la configuración de la empresa y los datos de las aplicaciones sincronizadas por Enterprise State Roaming. Debe tener [una suscripción de pago](https://azure.microsoft.com/pricing/details/information-protection/) para utilizar todas las funcionalidades del servicio Azure Rights Management.

## <a name="to-enable-enterprise-state-roaming"></a>Para habilitar Enterprise State Roaming

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com/).

1. Seleccione **Azure Active Directory** &gt; **Dispositivos** &gt; **Enterprise State Roaming**.

1. Seleccione **Los usuarios pueden sincronizar la configuración y los datos de aplicación en distintos dispositivos**. Para más información, consulte la sección [Configuración de dispositivo](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).
  
   ![imagen de configuración de dispositivo con la etiqueta Los usuarios pueden sincronizar la configuración y los datos de aplicaciones en distintos dispositivos](./media/enterprise-state-roaming-enable/device-settings.png)
  
Para que un dispositivo Windows 10 utilice el servicio Enterprise State Roaming, el dispositivo debe autenticarse mediante una identidad de Azure AD. Para los dispositivos que están unidos a Azure AD, la identidad de inicio de sesión principal del usuario es la identidad de Azure AD, por lo que no se requiere ninguna configuración adicional. Para los dispositivos que usan una instancia de Active Directory local, los administradores de TI deben [configurar los dispositivos unidos a Azure Active Directory híbrido](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual-steps). 

## <a name="data-storage"></a>Almacenamiento de datos
Los datos de Enterprise State Roaming se hospedan en las [regiones de Azure](https://azure.microsoft.com/regions/) que mejor se alineen con el valor de país o región establecido en la instancia de Azure Active Directory. Se crean particiones de los datos de Enterprise State Roaming en función de las tres regiones geográficas principales: Estados Unidos, EMEA y APAC. Los datos móviles de Enterprise State Roaming del inquilino se encuentran ubicados de manera local con la región geográfica y no se replican entre regiones.  Por ejemplo:

Valor de país o región | sus datos se hospedan en
---------------------|-------------------------
Un país o región EMEA como Francia o Zambia | Una o más regiones de Azure en Europa 
Un país o región América del Norte, por ejemplo, Estados Unidos o Canadá | Una o más regiones de Azure en EE. UU.
Un país o región APAC como Australia o Nueva Zelanda | Una o más regiones de Azure en Asia
Regiones de Sudamérica y Antártida | Una o más regiones de Azure en EE. UU.

El valor de país o región se establece como parte del proceso de creación del directorio de AD de Azure y no se puede modificar posteriormente. Para más detalles sobre la ubicación del almacenamiento de datos, genere una incidencia con el [soporte técnico de Azure](https://azure.microsoft.com/support/options/).

## <a name="view-per-user-device-sync-status"></a>Visualización del estado de sincronización de dispositivos por usuario
Siga estos pasos para ver un informe de estado de sincronización de dispositivos por usuario.

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com/).

1. Seleccione **Azure Active Directory** &gt; **Usuarios** &gt; **Todos los usuarios**.

1. Seleccione el usuario y, a continuación, seleccione **Dispositivos**.

1. En **Mostrar**, seleccione **Dispositivos sincronizando configuración y datos de aplicación** para mostrar el estado de sincronización.
  
   ![imagen de configuración de datos de sincronización de dispositivo](./media/enterprise-state-roaming-enable/sync-status.png)
  
1. Si hay dispositivos sincronizando para este usuario, puede ver los dispositivos, como se muestra aquí.
  
   ![imagen de datos de columnas de sincronización de dispositivo](./media/enterprise-state-roaming-enable/device-status-row.png)

## <a name="data-retention"></a>Retención de datos
Los datos sincronizados en la nube de Microsoft con Enterprise State Roaming se conservan hasta que se eliminan manualmente o hasta que los datos en cuestión se determinan como obsoletos. 

### <a name="explicit-deletion"></a>Eliminación explícita
La eliminación explícita se da cuando un administrador de Azure elimina un usuario o un directorio o bien cuando solicita expresamente que se eliminen los datos.

* **Eliminación de usuarios**: cuando se elimina un usuario en Azure AD, los datos de itinerancia de su cuenta se eliminan entre 90 y 180 días después. 
* **Eliminación de directorios**: la eliminación de un directorio completo en Azure AD se realiza inmediatamente. Todos los datos de configuración asociados con el directorio se eliminan después de un período entre 90 y 180 días. 
* **Eliminación bajo demanda**: si el administrador de Azure AD desea eliminar manualmente los datos de configuración o los de un usuario concreto, puede generar una incidencia en el [Soporte técnico de Azure](https://azure.microsoft.com/support/). 

### <a name="stale-data-deletion"></a>Eliminación de datos obsoletos
Los datos a los que no se ha accedido durante un año (el período de retención) se considerarán obsoletos y se podrán eliminar de la nube de Microsoft. El periodo de retención está sujeto a cambios, pero no será inferior a 90 días. Los datos obsoletos pueden ser un conjunto específico de valores de configuración de Windows o de la aplicación o toda la configuración de un usuario. Por ejemplo:

* Si ningún dispositivo accede a una colección de configuraciones concreta (por ejemplo, una aplicación se quita del dispositivo o un grupo de configuraciones, como "Tema", está deshabilitado para todos los dispositivos de un usuario), esa colección quedará obsoleta tras el período de retención y se puede eliminar. 
* Si un usuario ha desactivado la sincronización de configuración en todos sus dispositivos, a continuación, se accederá ninguno de los datos de configuración y todos los datos de configuración para ese usuario quedarán obsoletos y podrán eliminarse tras el período de retención. 
* Si el administrador del directorio de Azure AD desactiva Enterprise State Roaming para todo el directorio, todos los usuarios de ese directorio dejarán de sincronizar la configuración y todos los datos de configuración de todos los usuarios quedarán obsoletos y podrán eliminarse tras el período de retención. 

### <a name="deleted-data-recovery"></a>Recuperación de datos eliminados
La directiva de retención de datos no es configurable. Una vez que los datos se eliminan de forma permanente, no son recuperables. Sin embargo, los datos de configuración se eliminan solo de la nube de Microsoft, no del dispositivo del usuario final. Si cualquier dispositivo más adelante vuelve a conectarse al servicio Enterprise State Roaming, la configuración se sincroniza de nuevo y se almacena en la nube de Azure.

## <a name="next-steps"></a>Pasos siguientes

* [Información general de Enterprise State Roaming](enterprise-state-roaming-overview.md)
* [Preguntas más frecuentes sobre itinerancia de datos y configuración](enterprise-state-roaming-faqs.md)
* [Configuración de MDM y directivas de grupo](enterprise-state-roaming-group-policy-settings.md)
* [Referencia de la configuración de movilidad de Windows 10](enterprise-state-roaming-windows-settings-reference.md)
* [Solución de problemas](enterprise-state-roaming-troubleshooting.md)
