---
title: 'Azure AD Connect: Instancias de servicio de sincronización | Microsoft Docs'
description: Esta página documenta las consideraciones especiales para instancias de Azure AD.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/27/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c342eac5460d8d52422b0497b1283f367660eb3c
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66298830"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Consideraciones especiales para instancias
Azure AD Connect se utiliza habitualmente con la instancia mundial de Azure AD y Office 365. Pero también hay otras instancias y estas tienen requisitos diferentes para las direcciones URL y otras consideraciones especiales.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Germany
[Microsoft Cloud Germany](https://www.microsoft.de/cloud-deutschland) es una nube soberana operada por un administrador de datos alemán.

| Direcciones URL para abrir en el servidor proxy |
| --- |
| \*.microsoftonline.de |
| \*.windows.net |
| +Listas de revocación de certificados |

Al iniciar sesión en el inquilino de Azure AD tiene que usar una cuenta en el dominio onmicrosoft.de.

Características que actualmente no están presentes en Microsoft Cloud Germany:

* La **escritura diferida de contraseñas** está disponible en versión preliminar con Azure AD Connect versión 1.1.570.0 y posteriores.
* Otros servicios de Azure AD Premium no están disponibles.

## <a name="microsoft-azure-government"></a>Microsoft Azure Government
[Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/) es una nube para el gobierno de Estados Unidos.

Esta nube ha sido compatible con versiones anteriores de DirSync. A partir de la compilación 1.1.180 de Azure AD Connect la próxima generación de la nube es compatible. Esta generación utiliza solo puntos de conexión basados en EE. UU. y tiene una lista de direcciones URL diferentes para abrir en el servidor proxy.

| Direcciones URL para abrir en el servidor proxy |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*. windows.net (necesario para la detección automática de inquilino de Azure Government) |
| \*.gov.us.microsoftonline.com |
| +Listas de revocación de certificados |

> [!NOTE]
> A partir de Azure AD Connect versión 1.1.647.0, establecer el valor de AzureInstance en el registro ya no es necesario siempre que *. windows.net esté abierto en los servidores proxy. Sin embargo, para los clientes que no permiten la conectividad a Internet desde sus servidores de Azure AD Connect, se puede usar la siguiente configuración manual.

### <a name="manual-configuration"></a>Configuración manual

Se usan los siguientes pasos de configuración manual para asegurarse de que Azure AD Connect usa puntos de conexión de sincronización de Azure Government.

1. Inicie la instalación de Azure AD Connect.
2. Cuando vea la primera página en donde se supone que tiene que aceptar el CLUF, no continúe y deje que el Asistente para instalación se continúe ejecutando.
3. Inicie regedit y cambie la clave del registro `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` al valor `4`.
4. Vuelva al Asistente para instalación de Azure AD Connect, acepte el CLUF y continúe. Durante la instalación, asegúrese de usar el **configuración personalizada** ruta de acceso de instalación (y no la instalación rápida), a continuación, continuar con la instalación como de costumbre.

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
