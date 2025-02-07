---
title: Las definiciones de directiva de Azure supervisadas en Azure Security Center | Microsoft Docs
description: Las definiciones de directiva de Azure supervisadas en Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: c89cb1aa-74e8-4ed1-980a-02a7a25c1a2f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/15/2019
ms.author: rkarlin
ms.openlocfilehash: 9d9369afd36f64c27cd2222cab0de5912aa913de
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60909203"
---
# <a name="azure-security-policies-monitored-by-security-center"></a>Directivas de seguridad de Azure que supervisa Security Center
En este artículo se proporciona una lista de definiciones de directiva de Azure que se pueden supervisar en Azure Security Center. Para más información acerca de las directivas de seguridad, consulte [Uso de directivas de seguridad](tutorial-security-policy.md).

## <a name="available-security-policies"></a>Directivas de seguridad disponibles

Para obtener información acerca de las directivas integradas que se supervisan mediante Security Center, consulte la tabla siguiente:

| Directiva | Funcionamiento de la directiva |
| --- | --- |
|Habilitar la auditoría de los diagnósticos de registros en Azure Service Fabric y conjuntos de escalado de máquinas virtuales|Se recomienda habilitar los registros para que esté disponible para investigación después de un incidente o poner en peligro una traza de actividad.|
|Reglas de autorización de auditoría en los espacios de nombres de Event Hubs|Clientes de Azure Event Hubs no deben usar una directiva de acceso de nivel de espacio de nombres que proporciona acceso a todas las colas y temas en un espacio de nombres. Para alinear con el modelo de seguridad de privilegio mínimo, debe crear directivas de acceso en el nivel de entidad para colas y temas para proporcionar acceso a solo la entidad concreta.|
|Auditar existencia de las reglas de autorización en entidades de Event Hubs|Auditar la existencia de las reglas de autorización en entidades de Event Hubs para conceder acceso con privilegios mínimos.|
|Auditar el acceso de red sin restricciones a cuentas de almacenamiento|Audite el acceso de red no restringido en la configuración de firewall de su cuenta de almacenamiento. Configurar reglas de red para que solo las aplicaciones de redes permitidas pueden tener acceso a la cuenta de almacenamiento. Para permitir conexiones desde internet específico o los clientes locales, conceder acceso para el tráfico de redes virtuales de Azure específicas o a internet pública dirección IP intervalos.|
|Auditar el uso de reglas de RBAC personalizadas|Roles integrados, como "Propietario, Colaborador y lector" en lugar de las funciones (RBAC) del control personalizado de acceso basado en roles, que son propensas a errores de auditoría. Uso de roles personalizados se trata como una excepción y requiere revisión rigurosa y modelado de amenazas.|
|Auditar la habilitación de registros de diagnóstico en Azure Stream Analytics|Habilitación de registros de auditoría y mantenerlos para hasta un año. Esto crea los seguimientos de actividad de investigación cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Auditar la transferencia segura a cuentas de almacenamiento|Requisitos de transferencia segura en su cuenta de almacenamiento de auditoría. La transferencia segura es una opción que obliga a la cuenta de almacenamiento a aceptar solamente solicitudes de conexiones seguras (HTTPS). Uso de HTTPS garantiza la autenticación entre el servidor y el servicio. También protege los datos en tránsito frente a ataques de nivel de red, como man-in-the-middle, interceptación y secuestros de sesiones.|
|Auditoría de aprovisionamiento de un administrador de Azure Active Directory para SQL Server|Auditoría de aprovisionamiento de un administrador de Azure Active Directory (Azure AD) para SQL Server habilitar la autenticación de Azure AD. Autenticación de Azure AD admite la administración de permisos simplificada y administración centralizada de identidades de usuarios de base de datos y otros servicios de Microsoft.|
|Auditar las reglas de autorización en espacios de nombres de Service Bus|Clientes de Azure Service Bus no deben usar una directiva de acceso de nivel de espacio de nombres que proporciona acceso a todas las colas y temas en un espacio de nombres. Para alinear con el modelo de seguridad con privilegios mínimos, cree directivas de acceso en el nivel de entidad para colas y temas para proporcionar acceso a solo la entidad concreta.|
|Auditar la habilitación de registros de diagnóstico en Service Bus|Habilitación de registros de auditoría y conservarlos en un año. Esto crea los seguimientos de actividad de investigación cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Auditar la configuración de la propiedad ClusterProtectionLevel como EncryptAndSign en Service Fabric|Service Fabric proporciona tres niveles de protección para la comunicación de nodo a nodo que utiliza un certificado de clúster principal: Ninguno, sesión y EncryptAndSign. Establezca el nivel de protección para asegurarse de que todos los mensajes de nodo a nodo se cifran y se firman digitalmente.|
|Auditar el uso de Azure Active Directory para la autenticación de clientes en Service Fabric|Auditar el uso de autenticación de cliente solo a través de Azure AD en Service Fabric.|
|Auditar la habilitación de registros de diagnóstico para el servicio Search|Habilitación de registros de auditoría y mantenerlos para hasta un año. Esto crea los seguimientos de actividad de investigación cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Auditar la habilitación de sólo las conexiones seguras a la memoria caché de Azure para Redis|Habilitar las conexiones a través de SSL para la caché de Azure para Redis solo de auditoría. Uso de conexiones seguras garantiza la autenticación entre el servidor y el servicio. También protege los datos en tránsito frente a ataques de nivel de red, como man-in-the-middle, interceptación y secuestros de sesiones.|
|Auditar la habilitación de registros de diagnóstico en Logic Apps|Habilitación de registros de auditoría y mantenerlos para hasta un año. Esto crea los seguimientos de actividad de investigación cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Auditar la habilitación de registros de diagnóstico en Key Vault|Habilitación de registros de auditoría y mantenerlos para hasta un año. Esto crea los seguimientos de actividad de investigación cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Habilitación de registros de diagnóstico de centros de eventos de auditoría|Habilitación de registros de auditoría y mantenerlos para hasta un año. Esto crea los seguimientos de actividad de investigación cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Auditar la habilitación de registros de diagnóstico en Azure Data Lake Store|Habilitación de registros de auditoría y mantenerlos hasta un año. Esto crea los seguimientos de actividad de investigación cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Auditar la habilitación de registros de diagnóstico en Data Lake Analytics|Habilitación de registros de auditoría y mantenerlos para hasta un año. Esto crea los seguimientos de actividad de investigación cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Auditar el uso de las cuentas de almacenamiento clásicas|Utilice Azure Resource Manager para las cuentas de almacenamiento para proporcionar mejoras de seguridad. Entre ellas se incluyen las siguientes: <br>-Control de acceso más fuerte (RBAC)<br>-Mejor auditoría<br>: Gobierno e implementación basada en Resource Manager azure<br>-Acceso a las identidades administradas<br>-Acceso a Azure Key Vault para los secretos<br>-Autenticación basada en AD azure<br>-Compatibilidad con etiquetas y los grupos de recursos para facilitar la administración de seguridad|
|Auditar el uso de máquinas virtuales clásicas|Utilice Azure Resource Manager para las máquinas virtuales para proporcionar mejoras de seguridad.  Entre ellas se incluyen las siguientes: <br>-Control de acceso más fuerte (RBAC)<br>-Mejor auditoría<br>: Gobierno e implementación basada en Resource Manager azure<br>-Acceso a las identidades administradas<br>-Acceso a Azure Key Vault para los secretos<br>-Autenticación basada en AD azure<br>-Compatibilidad con etiquetas y los grupos de recursos para facilitar la administración de seguridad|
|Auditar la configuración de reglas de alertas de métricas en cuentas de Batch|Configuración de auditoría de reglas de alerta de métrica en cuentas de Azure Batch para habilitar la métrica necesaria.|
|Auditar la habilitación de registros de diagnóstico en cuentas de Batch|Habilitación de registros de auditoría y mantenerlos para hasta un año. Esto crea los seguimientos de actividad de investigación cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Habilitación del cifrado de las variables de automatización de la cuenta de auditoría|Es importante habilitar el cifrado de los recursos de variables de cuenta de Azure Automation al almacenar datos confidenciales.|
|Habilitación de registros de diagnóstico de App Service de auditoría|Permite auditar la habilitación de registros de diagnóstico en la aplicación. Esto crea los seguimientos de actividad de investigación cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Auditar el estado del cifrado de datos transparente|Estado de cifrado de datos transparente de auditoría para bases de datos SQL.|
|Auditar la configuración de auditoría de nivel de SQL Server|Auditar la existencia de auditoría de SQL en el nivel de servidor.|
|\[Versión preliminar]: Supervisión de la base de datos SQL sin cifrar en Azure Security Center|Azure Security Center supervisa las bases de datos tal como se recomienda o los servidores SQL Server sin cifrar.|
|\[Versión preliminar]: Supervisión de la base de datos SQL no auditada en Azure Security Center|Azure Security Center supervisa los servidores SQL Server y bases de datos que no tienen la auditoria SQL activada tal como se recomienda.|
|\[Versión preliminar]: Supervisión de las actualizaciones del sistema que faltan en Azure Security Center|Azure Security Center supervisa que faltan actualizaciones del sistema de seguridad en los servidores, tal como se recomienda.|
|\[Versión preliminar]: Auditoría de la falta del cifrado de blob de las cuentas de almacenamiento|Las cuentas de almacenamiento que no usan el cifrado de blobs de auditoría. Esto solo se aplica a tipos de recursos de almacenamiento de Microsoft, no el almacenamiento de otros proveedores. Azure Security Center supervisa el acceso de red posible just-in-time tal como se recomienda.|
|\[Versión preliminar]: Supervisar el acceso de red posible just-in-time en Azure Security Center|Azure Security Center supervisa el acceso de red posible just-in-time tal como se recomienda.|
|\[Versión preliminar]: Supervisión de una posible inclusión de aplicaciones en la lista de permitidos en Azure Security Center|Azure Security Center supervisa la configuración de la lista blanca de posibles de la aplicación.|
|\[Versión preliminar]: Supervisión del acceso de red permisivo en Azure Security Center|Azure Security Center supervisa red grupos de seguridad que tienen reglas demasiado permisiva, tal como se recomienda.|
|\[Versión preliminar]: Supervisión de los puntos vulnerables del sistema operativo en Azure Security Center|Azure Security Center supervisa los servidores que no cumplen la línea de base configurado tal como se recomienda.| 
|\[Versión preliminar]: Supervisión de la falta de Endpoint Protection en Azure Security Center|Azure Security Center supervisa los servidores que no tienen un agente de Microsoft System Center Endpoint Protection instalado tal como se recomienda.|
|\[Versión preliminar]: Supervisar sin cifrar discos de máquina virtual en Azure Security Center|Azure Security Center supervisa las máquinas virtuales que no tiene habilitado como se recomienda el cifrado de disco.|
|\[Versión preliminar]: Supervisar las vulnerabilidades de la máquina virtual en Azure Security Center|Supervisar las vulnerabilidades detectadas por la solución de evaluación de vulnerabilidades y las máquinas virtuales que no tienen una solución de evaluación de vulnerabilidades en Azure Security Center como se recomienda.|
|\[Versión preliminar]: Supervisión de la aplicación web desprotegida en Azure Security Center|Azure Security Center supervisa las aplicaciones web que carecen de protección de firewall de aplicaciones web tal como se recomienda.|
|\[Versión preliminar]: Supervisión de puntos de conexión de red desprotegidos en Azure Security Center|Azure Security Center supervisa los puntos de conexión que no tienen protección de firewall de próxima generación, tal como se recomienda la red.|
|\[Versión preliminar]: Supervisión de los resultados de evaluación de puntos vulnerables de SQL en Azure Security Center|Evaluación de vulnerabilidad de monitor resultados del análisis y recomendaciones corregir las vulnerabilidades de la base de datos.|
|\[Versión preliminar]: Auditar el número máximo de propietarios de una suscripción|Se recomienda que designe hasta tres de los propietarios de suscripciones para reducir el riesgo de vulneración por un propietario en peligro.|
|\[Versión preliminar]: Auditar el número mínimo de propietarios de una suscripción|Se recomienda designar más de un propietario de la suscripción para garantizar el acceso de administrador redundancia.|
|\[Versión preliminar]: Auditar las cuentas con permisos de propietario que no tengan MFA habilitada en una suscripción|Autenticación multifactor (MFA) debe habilitarse para todas las cuentas de suscripción que tienen permisos de propietario para evitar una vulneración de las cuentas o recursos.|
|\[Versión preliminar]: Auditar las cuentas con permisos de escritura que no tengan MFA habilitada en una suscripción|Debe habilitarse la autenticación multifactor para todas las cuentas de suscripción que tienen permisos de escritura para evitar infracciones de las cuentas o recursos.|
|\[Versión preliminar]: Auditar las cuentas con permisos de lectura que no tengan MFA habilitada en una suscripción|Debe habilitarse la autenticación multifactor para todas las cuentas de suscripción que tiene permisos para evitar infracciones de las cuentas o recursos de lectura.|
|\[Versión preliminar]: Auditar las cuentas en desuso de una suscripción que tengan permisos de propietario|Las cuentas en desuso que tienen permisos de propietario deben quitarse de la suscripción. Inicio de sesión de se han bloqueado cuentas en desuso.|
|\[Versión preliminar]: Auditar las cuentas en desuso de una suscripción|Convendría eliminar las cuentas en desuso de las suscripciones. Inicio de sesión de se han bloqueado cuentas en desuso.|
|\[Versión preliminar]: Auditar las cuentas externas de una suscripción que tengan permisos de propietario|Las cuentas externas que tienen permisos de propietario deben quitarse de la suscripción para impedir el acceso de permisos.|
|\[Versión preliminar]: Auditar las cuentas externas de una suscripción que tengan permisos de escritura|Las cuentas externas que tienen los permisos se deben quitar de la suscripción para impedir el acceso no supervisado de escritura.|
|\[Versión preliminar]: Auditar las cuentas externas de una suscripción que tengan permisos de lectura|Las cuentas externas que tienen permisos de lectura deben quitarse de la suscripción para impedir el acceso no supervisado.|




## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a configurar directivas de seguridad en Security Center. Para obtener más información sobre Security Center, consulte los siguientes artículos.

* [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md): Obtenga información sobre cómo planear y entender las consideraciones de diseño en Azure Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): Aprenda a supervisar el estado de los recursos de Azure.
* [Administración y respuesta a alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): Aprenda a administrar y responder a las alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): Aprenda cómo supervisar el estado de mantenimiento de las soluciones de sus asociados.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): Obtenga respuestas para las preguntas más frecuentes acerca del uso del servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/): Encuentre artículos de blog sobre el cumplimiento y la seguridad de Azure.

Para más información acerca de la directiva de Azure, consulte [¿qué es Azure Policy?](../governance/policy/overview.md).
