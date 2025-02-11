---
title: Adición o eliminación de propietarios de grupos en Azure Active Directory | Microsoft Docs
description: Obtenga información acerca de cómo agregar o quitar propietarios del grupo con Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: lizross
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd684e1bd48f877a74280b33b4df65d7baaa0fe7
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507171"
---
# <a name="add-or-remove-group-owners-in-azure-active-directory"></a>Adición o eliminación de propietarios del grupo en Azure Active Directory
Los grupos de Azure Active Directory (Azure AD) pertenecen a propietarios del grupo, quienes también lo administran. Propietarios del grupo pueden ser usuarios o entidades de servicio y son capaces de administrar el grupo incluida la pertenencia. Solo los propietarios de grupo existente o gestión de grupo de administradores pueden asignar onwers de grupo. Los propietarios del grupo no deben ser miembros del grupo.

Cuando un grupo no tiene propietario, son todavía puede administrar el grupo de administración de grupo de administradores.

## <a name="add-an-owner-to-a-group"></a>Adición de un propietario a un grupo
A continuación son las instrucciones para agregar un usuario como propietario a un grupo mediante el portal de Azure AD. Para agregar una entidad de servicio como un propietario de un grupo, siga las instrucciones para hacerlo mediante [PowerShell](https://docs.microsoft.com/powershell/module/Azuread/Add-AzureADGroupOwner?view=azureadps-2.0).

### <a name="to-add-a-group-owner"></a>Para agregar un propietario del grupo
1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador global para el directorio.

2. Seleccione **Azure Active Directory**, **Grupos** y, a continuación, el grupo para el que quiera agregar un propietario (en este ejemplo, *MDM policy - West* (Directiva MDM - Oeste)).

3. En la página **MDM policy - West Overview** (Información general de la directiva de MDM - Oeste), seleccione **Propietarios**.

    ![Página MDM policy - West Overview (Información general de la directiva MDM - Oeste), con la opción Propietarios resaltada](media/active-directory-accessmanagement-managing-group-owners/add-owners-option-overview-blade.png)

4. En la página **MDM policy - West - Owners** (Directiva MDM - Oeste - Propietarios), seleccione **Agregar propietarios**, busque y seleccione el usuario que será el nuevo propietario del grupo y, a continuación, elija **Seleccionar**.

    ![Página MDM policy - West - Owners (Directiva MDM - Oeste - Propietarios), con la opción Propietarios resaltada](media/active-directory-accessmanagement-managing-group-owners/add-owners-owners-blade.png)

    Después de seleccionar el nuevo propietario, puede actualizar la página **Propietarios** y ver el nombre agregado a la lista de propietarios.

## <a name="remove-an-owner-from-a-group"></a>Eliminación de un propietario de un grupo
Elimine un propietario de un grupo mediante Azure AD.

### <a name="to-remove-an-owner"></a>Para quitar un propietario
1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador global para el directorio.

2. Seleccione **Azure Active Directory**, **Grupos** y a continuación el grupo en el que quiera eliminar un propietario (en este ejemplo, *MDM policy - West*).

3. En la página **MDM policy - West Overview** (Información general de la directiva de MDM - Oeste), seleccione **Propietarios**.

    ![Página MDM policy - West Overview (Información general de la directiva MDM - Oeste), con la opción Propietarios resaltada](media/active-directory-accessmanagement-managing-group-owners/remove-owners-option-overview-blade.png)

4. En la página **MDM policy - West - Owners** (Directiva MDM - Oeste - Propietarios), seleccione el usuario que quiera quitar como propietario del grupo, elija **Quitar** en la página de información del usuario y seleccione **Sí** para confirmar su decisión.

    ![Página de información del usuario con la opción Quitar resaltada](media/active-directory-accessmanagement-managing-group-owners/remove-owner-info-blade.png)

    Después de quitar el propietario, puede volver a la página **Propietarios** y ver que se ha quitado el nombre de la lista de propietarios.

## <a name="next-steps"></a>Pasos siguientes
- [Administración del acceso a los recursos con grupos de Azure Active Directory](active-directory-manage-groups.md)

- [Cmdlets de Azure Active Directory para configurar las opciones de grupo](../users-groups-roles/groups-settings-cmdlets.md)

- [Uso de grupos para asignar acceso a una aplicación SaaS integrada](../users-groups-roles/groups-saasapps.md)

- [Integración de las identidades locales con Azure Active Directory](../hybrid/whatis-hybrid-identity.md)

- [Cmdlets de Azure Active Directory para configurar las opciones de grupo](../users-groups-roles/groups-settings-v2-cmdlets.md)
