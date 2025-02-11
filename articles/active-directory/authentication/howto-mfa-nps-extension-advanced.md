---
title: Configurar la extensión de NPS de Azure MFA, Azure Active Directory
description: Después de instalar la extensión NPS, siga estos pasos para la configuración avanzada como la creación de listas blancas de direcciones IP y el reemplazo de UPN.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8ac0497b13dad6795e8dc7ffaf761fe887a9953
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65988622"
---
# <a name="advanced-configuration-options-for-the-nps-extension-for-multi-factor-authentication"></a>Opciones de configuración avanzada para la extensión NPS para Multi-Factor Authentication

La extensión Servidor de directivas de redes (NPS) amplía las características de Azure Multi-Factor Authentication basadas en la nube a la infraestructura local. En este artículo se supone que ya tiene la extensión instalada y ahora desea saber cómo personalizarla conforme a sus necesidades. 

## <a name="alternate-login-id"></a>Identificador de inicio de sesión alternativo

Puesto que la extensión NPS se conecta a los directorios locales y de la nube, puede tener problemas donde los nombres principales de usuario (UPN) locales no coincidan con los nombres existentes en la nube. Para solucionar este problema, use identificadores de inicio de sesión alternativos. 

Dentro de la extensión NPS, puede designar un atributo de Active Directory para que se use en lugar del UPN para Azure Multi-Factor Authentication. Esto le permite proteger los recursos locales con la verificación en dos pasos sin modificar los UPN locales. 

Para configurar identificadores de inicio de sesión alternativos, vaya a `HKLM\SOFTWARE\Microsoft\AzureMfa` y edite los siguientes valores del Registro:

| NOMBRE | Type | Valor predeterminado | DESCRIPCIÓN |
| ---- | ---- | ------------- | ----------- |
| LDAP_ALTERNATE_LOGINID_ATTRIBUTE | string | Vacío | Designe el nombre del atributo de Active Directory que desea usar en lugar del UPN. Este atributo se utiliza como el atributo AlternateLoginId. Si este valor del Registro se establece en un [atributo de Active Directory válido](https://msdn.microsoft.com/library/ms675090.aspx) (por ejemplo, correo electrónico o displayName), a continuación, el valor del atributo se utiliza en lugar del UPN del usuario para la autenticación. Si este valor del registro está vacío o no está configurado, AlternateLoginId se deshabilita y el UPN del usuario se utiliza para la autenticación. |
| LDAP_FORCE_GLOBAL_CATALOG | boolean | False | Use esta marca para exigir el uso del catálogo global para búsquedas LDAP al buscar AlternateLoginId. Configure un controlador de dominio como catálogo global, agregue el atributo AlternateLoginId a dicho catálogo y luego habilite esta marca. <br><br> Si LDAP_LOOKUP_FORESTS se ha configurado (no vacío), **se exige que esta marca sea true**, independientemente del valor de la configuración del Registro. En este caso, la extensión NPS requiere que el catálogo global esté configurado con el atributo AlternateLoginId para cada bosque. |
| LDAP_LOOKUP_FORESTS | string | Vacío | Proporcione una lista separada por puntos y coma de bosques para buscar. Por ejemplo, *contoso.com;foobar.com*. Si se configura este valor del Registro, la extensión NPS busca iterativamente en todos los bosques en el orden en el que se muestran y devuelve el primer valor AlternateLoginId correcto. Si este valor del Registro no está configurado, la búsqueda de AlternateLoginId se limita al dominio actual.|

Para solucionar problemas con identificadores de inicio de sesión alternativos, siga los pasos recomendados para [errores de identificadores de inicio de sesión alternativos](howto-mfa-nps-extension-errors.md#alternate-login-id-errors).

## <a name="ip-exceptions"></a>Excepciones de dirección IP

Si necesita supervisar la disponibilidad del servidor, como si los equilibradores de carga comprueban qué servidores se ejecutan antes de enviar cargas de trabajo, no deseará que las solicitudes de verificación bloqueen estas comprobaciones. En su lugar, cree una lista de direcciones IP que sepa que las cuentas de servicio las utilizan y deshabilite los requisitos de Multi-Factor Authentication en esa lista.

Para configurar una dirección de IP a la lista de permitidos, vaya a `HKLM\SOFTWARE\Microsoft\AzureMfa` y configurar el valor del registro siguiente:

| NOMBRE | Type | Valor predeterminado | DESCRIPCIÓN |
| ---- | ---- | ------------- | ----------- |
| IP_WHITELIST | string | Vacío | Proporcione una lista separada por puntos y coma de direcciones IP. Incluya las direcciones IP de las máquinas donde se originan las solicitudes de servicio, como el servidor NAS/VPN. No se admiten los intervalos de direcciones IP y subredes. <br><br> Por ejemplo, *10.0.0.1;10.0.0.2;10.0.0.3*.

Cuando llega una solicitud desde una dirección IP que existe en el `IP_WHITELIST`, se omite la verificación en dos pasos. La lista de IP se compara con la dirección IP que se proporciona en el *ratNASIPAddress* atributo de la solicitud RADIUS. Si llega una solicitud RADIUS sin el atributo ratNASIPAddress, se registra la siguiente advertencia: "La lista blanca P_WHITE_LIST_WARNING::IP se ha omitido porque la dirección IP de origen falta en la solicitud RADIUS en el atributo NasIpAddress".

## <a name="next-steps"></a>Pasos siguientes

[Resolución de mensajes de error de la extensión de NPS para Azure Multi-Factor Authentication](howto-mfa-nps-extension-errors.md)
