---
title: 'Consideraciones de diseño de identidad híbrida de Azure Active Directory: introducción | Microsoft Docs'
description: Información general y mapa de contenido de la guía de consideraciones de diseño de identidad híbrida
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 100509c4-0b83-4207-90c8-549ba8372cf7
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7f8dd49f3668b8f68753681123a04d21edac46c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60381490"
---
# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Consideraciones de diseño de identidad híbrida de Azure Active Directory
Los dispositivos basados en el consumidor proliferan en el mundo empresarial y las aplicaciones de software como servicio (SaaS) basadas en la nube son fáciles de adoptar. Como resultado, resulta difícil mantener el control de acceso a las aplicaciones de los usuarios entre centros de datos internos y plataformas en la nube.  

Las soluciones de identidad de Microsoft abarcan funcionalidades locales y de nube, de forma que se crea una sola identidad de usuario para la autenticación y la autorización en todos los recursos, sin importar su ubicación. Este concepto se conoce como Identidad híbrida. Existen diferentes opciones de configuración y diseño para la identidad híbrida con las soluciones de Microsoft y, en algunos casos, puede ser difícil determinar qué combinación se adaptará mejor a las necesidades de su organización. 

Esta guía de consideraciones de diseño de identidad híbrida le ayudará a entender cómo diseñar una solución de identidad híbrida que se adapte bien a las necesidades empresariales y tecnológicas de su organización.  En ella se detallan una serie de pasos y tareas que puede seguir como ayuda para diseñar una solución de identidad híbrida que satisfaga los requisitos únicos de su organización. A través de estos pasos y tareas, se presentan las tecnologías y características pertinentes que tienen a su disposición las organizaciones para satisfacer los requisitos funcionales y de calidad de servicio (por ejemplo, disponibilidad, escalabilidad, rendimiento, capacidad de administración y seguridad). 

En concreto, los objetivos de esta guía son dar respuesta a las siguientes preguntas: 

* ¿Qué preguntas debo hacerme para impulsar un diseño específico de identidad híbrida respecto a una tecnología o dominio del problema, que se adapte a mis necesidades ?
* ¿Qué secuencia de actividades debo realizar para diseñar una solución de identidad híbrida para el dominio del problema o la tecnología? 
* ¿Qué opciones de tecnología y configuración de identidad híbrida están disponibles para ayudarme con mis necesidades? ¿Cuáles son las ventajas y desventajas de esas opciones para así poder seleccionar la mejor para mi empresa?

## <a name="who-is-this-guide-intended-for"></a>¿A quién va dirigida esta guía?
 CIO, CITO, arquitectos de identidad jefe, arquitectos de empresa y arquitectos de TI responsables de diseñar una solución de identidad híbrida para organizaciones medianas o grandes.

## <a name="how-can-this-guide-help-you"></a>¿Cómo puede ayudarle esta guía?
Puede usar esta guía para aprender a diseñar una solución de identidad híbrida que sea capaz de integrar un sistema de administración de identidades basado en la nube con su solución de identidad local actual. 

En el gráfico siguiente se muestra un ejemplo de una solución de identidad híbrida que permite a los administradores de TI integrar su solución actual de Windows Server Active Directory ubicada en el entorno local con Microsoft Azure Active Directory para permitir a los usuarios usar el inicio de sesión único (SSO) en todas las aplicaciones que se encuentran en la nube y locales.

![Ejemplo](media/plan-hybrid-identity-design-considerations/hybridID-example.png)

La ilustración anterior es un ejemplo de una solución de identidad híbrida que aprovecha los servicios en la nube para integrarse con las funcionalidades locales con el fin de proporcionar una experiencia única en el proceso de autenticación del usuario final y facilitar la administración de esos recursos por parte de TI. Aunque este ejemplo puede ser un escenario común, es probable que el diseño de identidad híbrida de cada organización sea diferente al del ejemplo que se muestra en la ilustración 1, debido a que los requisitos son diferentes. 

Esta guía proporciona una serie de pasos y las tareas que puede seguir para diseñar una solución de identidad híbrida que cumpla los requisitos únicos de su organización. A través de los siguientes pasos y tareas, la guía presenta las tecnologías y característica pertinentes que están disponibles para usted para satisfacer los requisitos funcionales y de nivel de servicio de su organización.

**Suposiciones**: tiene algo de experiencia con Windows Server, Active Directory Domain Services y Azure Active Directory. En este documento, se supone que está buscando cómo estas soluciones pueden satisfacer sus necesidades de negocio por sí solas o en una solución integrada.

## <a name="design-considerations-overview"></a>Información general sobre las consideraciones de diseño
Este documento proporciona un conjunto de pasos y tareas que puede seguir para diseñar una solución de identidad híbrida que cumpla sus necesidades. Los pasos se presentan en una secuencia ordenada. Las consideraciones de diseño que aprenderá en pasos posteriores pueden llevarle a cambiar las decisiones tomadas en los pasos anteriores, debido al conflicto entre las opciones de diseño. Hemos hecho todo lo posible por avisarle de los posibles conflictos de diseño en todo el documento. 

Al final, obtendrá el diseño que mejor se adapte a sus necesidades y solo después de repetir los pasos las veces que sean necesarias para incorporar todas la consideraciones expresadas en el documento. 

| Fase de identidad híbrida | Lista de temas |
| --- | --- |
| Determinación de los requisitos de identidad |[Determinación de las necesidades empresariales](plan-hybrid-identity-design-considerations-business-needs.md)<br> [Determinación de los requisitos de sincronización de directorios](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Determinación de los requisitos de autenticación multifactor](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Definición de una estrategia de adopción de identidad híbrida](plan-hybrid-identity-design-considerations-identity-adoption-strategy.md) |
| Plan para mejorar la seguridad de los datos mediante una solución de identidad sólida |[Determinación de los requisitos de protección de datos](plan-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Determinación de los requisitos de administración de contenido](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Determinación de los requisitos de control de acceso](plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Determinación de los requisitos de respuesta a incidentes](plan-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Definición de la estrategia de protección de datos](plan-hybrid-identity-design-considerations-data-protection-strategy.md) |
| Plan para el ciclo de vida de identidad híbrida |[Determinación de las tareas de administración de identidades híbridas](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Administración de la sincronización](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Determinación de la estrategia de adopción de administración de identidad híbrida](plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |

## <a name="next-steps"></a>Pasos siguientes
[Determinación de los requisitos de identidad](plan-hybrid-identity-design-considerations-business-needs.md)

