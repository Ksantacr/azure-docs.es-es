---
title: 'Registrar los dispositivos personales en la red de la organización: Azure Active Directory | Microsoft Docs'
description: Aprenda a registrar su dispositivo personal en la red de su organización para que pueda tener acceso a los recursos protegidos de su organización.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: lizross
ms.reviewer: jairoc
ms.custom: user-help, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 041c8bb6b4de2bbe2cbeb4c1a89e452239ae57bd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60473697"
---
# <a name="register-your-personal-device-on-your-organizations-network"></a>Registre su dispositivo personal en la red de su organización
Registre su dispositivo personal, que suele ser un teléfono o una tablet, en la red de su organización. Una vez registrado su dispositivo, podrá tener acceso a los recursos restringidos de su organización.

>[!Note]
>En este artículo se usa un dispositivo Windows con fines de demostración, pero también puede registrar dispositivos que ejecuten iOS, Android o macOS.

## <a name="what-happens-when-you-register-your-device"></a>Qué ocurre al registrar su dispositivo
Durante el registro de su dispositivo en la red de su organización, se producirán las acciones siguientes:

- Windows registra su dispositivo en la red de su organización.

- Opcionalmente, en función de las opciones de su organización, puede que se le pida configurar una verificación en dos pasos a través de [Multi-Factor Authentication](multi-factor-authentication-end-user-first-time.md) o [información de seguridad](user-help-security-info-overview.md).

- Opcionalmente, en función de las opciones de su organización, puede que se le inscriba automáticamente en la administración de dispositivos móviles, como Microsoft Intune. Para obtener más información sobre cómo inscribirse en Microsoft Intune, consulte [Enroll your device in Intune](https://docs.microsoft.com/intune-user-help/enroll-your-device-in-intune-all) (Inscribir su dispositivo en Intune).

- Seguirá el proceso de inicio de sesión, haciendo uso del nombre de usuario y la contraseña de su cuenta profesional o educativa.

## <a name="to-register-your-windows-device"></a>Para registrar su dispositivo Windows

Siga estos pasos para registrar su dispositivo personal en su red.

1. Abra **Configuración** y, a continuación, seleccione **Cuentas**.

    ![Cuentas en la pantalla Configuración](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. Seleccione **Obtener acceso a trabajo o escuela** y seleccione **Conectar** en la pantalla **Obtener acceso a trabajo o escuela**.

    ![Pantalla Obtener acceso a trabajo o escuela con la opción Conectar resaltada](./media/user-help-register-device-on-network/register-device-access-work-school-connect.png)

3. En la pantalla **Agregar una cuenta de trabajo o escuela**, escriba la dirección de correo electrónico de su cuenta profesional o educativa y seleccione **Siguiente**. Por ejemplo, alain@contoso.com.

4. Inicie sesión en su cuenta profesional o educativa y seleccione **Iniciar sesión**.

5. Complete el resto del proceso de registro, incluidas la aprobación de su solicitud de verificación de identidad (si usa la verificación en dos pasos) y la configuración de Windows Hello (si es necesario).

## <a name="to-verify-that-youre-registered"></a>Para verificar que está registrado
Puede asegurarse de que se ha registrado fijándose en su configuración.

1. Abra **Configuración** y, a continuación, seleccione **Cuentas**.

    ![Cuentas en la pantalla Configuración](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. Seleccione **Obtener acceso a trabajo o escuela** y asegúrese de que ve su cuenta profesional o educativa.

    ![Pantalla Obtener acceso a trabajo o escuela con cuenta Contoso conectada](./media/user-help-register-device-on-network/register-device-setup-verify.png)

## <a name="next-steps"></a>Pasos siguientes
Después de registrar su dispositivo personal en la red de su organización, debe poder tener acceso a la mayoría de sus recursos.

- Si su organización quiere que se una a su dispositivo de trabajo, consulte [Una su dispositivo de trabajo a la red de su organización](user-help-join-device-on-network.md).



