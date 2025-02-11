---
title: Uso de sondeos de estado de Azure Load Balancer para escalar y proporcionar alta disponibilidad para el servicio
titlesuffix: Azure Load Balancer
description: Aprenda a usar los sondeos de estado para supervisar las instancias que hay detrás de Load Balancer
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/07/2019
ms.author: kumud
ms.openlocfilehash: e488a4a6438279270f3d86dafa16c45eda184059
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415704"
---
# <a name="load-balancer-health-probes"></a>Sondeos de estado de Load Balancer

Azure Load Balancer proporciona sondeos de estado para usarlos con reglas de equilibrio de carga.  La configuración de los sondeos de estado y las respuestas de sondeo determinan qué instancias del grupo de back-end recibirán los nuevos flujos. Puede usar los sondeos de mantenimiento para detectar algún error de una aplicación en una instancia de back-end. También se puede generar una respuesta personalizada para un sondeo de estado y usar el sondeo de estado para el control de flujo con el fin de administrar la carga o el tiempo de inactividad planeado. Cuando se genera un error en el sondeo de mantenimiento, Load Balancer deja de enviar flujos nuevos a la instancia con estado incorrecto respectiva.

Los sondeos de estado admiten varios protocolos. La disponibilidad de un tipo específico de sondeo de estado para admitir un protocolo específico varía según la SKU de Load Balancer.  Además, el comportamiento del servicio varía según la SKU de Load Balancer.

| | SKU Estándar | SKU Básico |
| --- | --- | --- |
| [Tipos de sondeo](#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Comportamiento de sondeo inactivo](#probedown) | Todos los sondeos inactivos y todos los flujos TCP continúan. | Todos los sondeos hacia abajo, todos los flujos TCP expiran. | 

> [!IMPORTANT]
> Los sondeos de estado de Load Balancer parten de la dirección IP 168.63.129.16 y no se deben bloquear para que los sondeos marquen la instancia como activa.  Para más información, consulte el apartado [Probe source IP address](#probesource) (Dirección IP de origen de sondeo).

## <a name="types"></a>Tipos de sondeo

El sondeo de estado se puede configurar para agentes de escucha mediante los tres protocolos siguientes:

- [Agentes de escucha TCP](#tcpprobe)
- [Puntos de conexión HTTP](#httpprobe)
- [Puntos de conexión HTTPS](#httpsprobe)

Los tipos de sondeos de estado disponibles varían según la SKU de Load Balancer seleccionada:

|| TCP | HTTP | HTTPS |
| --- | --- | --- | --- |
| SKU Estándar |    &#9989; |   &#9989; |   &#9989; |
| SKU Básico |   &#9989; |   &#9989; | &#10060; |

Con independencia del tipo de sondeo de estado elegido, los sondeos de estado pueden observar cualquier puerto de una instancia de back-end, incluido el puerto en el que se proporciona el servicio real.

### <a name="tcpprobe"></a>Sondeo TCP

Los sondeos TCP inician una conexión mediante la realización de un protocolo de enlace TCP abierto de tres vías con el puerto definido.  Los sondeos TCP terminan una conexión con un protocolo de enlace TCP cerrado de cuatro vías.

El intervalo de sondeo mínimo es de 5 segundos y el número mínimo de respuestas incorrectas es 2.  La duración total de todos los intervalos no puede superar los 120 segundos.

Un sondeo TCP genera un error cuando:
* El agente de escucha TCP de la instancia no responde durante el período de tiempo de expiración.  El sondeo se marca como inactivo en función del número de solicitudes de sondeo con error, que se configuraron para quedarse sin respuesta antes de marcar el sondeo como inactivo.
* El sondeo recibe un restablecimiento de TCP de la instancia.

#### <a name="resource-manager-template"></a>Plantilla de Resource Manager

```json
    {
      "name": "tcp",
      "properties": {
        "protocol": "Tcp",
        "port": 1234,
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="httpprobe"></a> <a name="httpsprobe"></a> Sondeo HTTP / HTTPS

> [!NOTE]
> El sondeo HTTPS solo está disponible para [Standard Load Balancer](load-balancer-standard-overview.md).

Los sondeos HTTP y HTTPS se basan en el sondeo TCP y emiten una solicitud HTTP GET con la ruta de acceso especificada. Ambos sondeos admiten rutas de acceso relativas para la solicitud HTTP GET. Los sondeos HTTPS son lo mismo que los sondeos de HTTP con un contenedor de Seguridad de la capa de transporte (TLS, conocida anteriormente como SSL) añadido. El sondeo de estado se marca cuando la instancia responde con un código de estado 200 de HTTP dentro del período de tiempo de espera.  De forma predeterminada, el sondeo de estado intenta comprobar cada 15 segundos el puerto de sondeo de estado configurado. El intervalo de sondeo mínimo es 5 segundos. La duración total de todos los intervalos no puede superar los 120 segundos.

Los sondeos HTTP o HTTPS también pueden ser útiles si se quiere expresar el sondeo de estado.  Implemente su propia lógica para quitar instancias de la rotación del equilibrador de carga si el puerto de sondeo es también el agente de escucha para el propio servicio. Por ejemplo, podría decidir quitar una instancia si está por encima del 90 % de la CPU y devolver un estado que no es 200. 

Si usa Cloud Services y tiene roles web que utilizan w3wp.exe, también obtiene una supervisión automática de su sitio web. Los errores en el código del sitio web devuelven un estado distinto de 200 para el sondeo del equilibrador de carga.

Un sondeo HTTP / HTTPS genera un error cuando:
* El punto de conexión de sondeo devuelve un código HTTP de respuesta distinto de 200 (por ejemplo, 403, 404 o 500). Esto marcará el sondeo de estado como inactivo de forma inmediata. 
* El punto de conexión de sondeo no responde durante un período de tiempo de espera de 31 segundos. Es posible que no se respondan varias solicitudes de sondeo antes de que el sondeo se marque como que no se está ejecutando, hasta que se haya alcanzado la suma de todos los intervalos de tiempo de espera.
* El punto de conexión de sondeo cierra la conexión mediante TCP Reset.

#### <a name="resource-manager-templates"></a>Plantillas de Resource Manager

```json
    {
      "name": "http",
      "properties": {
        "protocol": "Http",
        "port": 80,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

```json
    {
      "name": "https",
      "properties": {
        "protocol": "Https",
        "port": 443,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="guestagent"></a>Sondeo de agente invitado (solo clásico)

Los roles del servicio en la nube (roles de trabajo y roles web) usan un agente invitado para la supervisión del sondeo de forma predeterminada.  Un sondeo de agente invitado es una configuración de último recurso.  Use siempre un sondeo de estado de forma explícita con un sondeo TCP o HTTP. Un sondeo del agente invitado no es tan eficaz como LOS sondeos definidos explícitamente en los escenarios de la mayor parte de las aplicaciones.

Un sondeo del agente invitado es una comprobación del agente invitado dentro de la máquina virtual. A continuación, escucha y responde con una respuesta HTTP 200 OK solo cuando la instancia está en estado Preparado. (Otros estados son Ocupado, Reciclando o Deteniendo).

Para obtener más información, consulte [Configuring the service definition file (csdef) for health probes](https://msdn.microsoft.com/library/azure/ee758710.aspx) (Configuración del archivo de definición de servicio (csdef) para los sondeos de estado) o [Get started by creating a public load balancer for cloud services](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services) (Introducción a la creación de un equilibrador de carga público para servicios en la nube).

Si el agente invitado no responde con HTTP 200 OK, el equilibrador de carga marca la instancia como sin respuesta. y deja de enviar flujos a dicha instancia. El equilibrador de carga sigue comprobando la instancia. 

Si el agente invitado responde con un HTTP 200, el equilibrador de carga vuelve a enviar nuevos flujos a dicha instancia.

Cuando se usa un rol web, el código de sitio web normalmente se ejecuta en w3wp.exe, que no está supervisado por el agente invitado ni el tejido de Azure. Los errores en w3wp.exe (por ejemplo, las respuestas de HTTP 500) no se notifican al agente invitado. Por lo tanto, el equilibrador de carga no toma esa instancia fuera de la rotación.

<a name="health"></a>
## <a name="probehealth"></a>Sondeo de estado

Los sondeos de estado TCP, HTTP y HTTPS se consideran en buen estado y marcan la instancia del rol como en buen estado en los casos siguientes:

* El sondeo de estado es correcto después de arrancar la máquina virtual.
* Se ha obtenido el número especificado de sondeos necesarios para marcar la instancia de rol como correcta.

> [!NOTE]
> Si el sondeo de estado fluctúa, el equilibrador de carga espera más tiempo antes de devolver la instancia de rol al estado correcto. Este tiempo de espera adicional protege el usuario y la infraestructura y es una directiva intencionada.

## <a name="probe-count-and-timeout"></a>Número de sondeos y tiempo de expiración

El comportamiento de los sondeos depende de:

* El número de sondeos correctos que permiten que una instancia se marque como activa.
* El número de sondeos con error que hacen que una instancia se marque como inactiva.

Los valores de intervalo y tiempo de espera especificados determinan si una instancia se marca como activa o inactiva.

## <a name="probedown"></a>Comportamiento de sondeo inactivo

### <a name="tcp-connections"></a>Conexiones TCP

Se realizarán nuevas conexiones TCP correctamente a las restantes instancias de back-end correctas.

Si se produce un error en el sondeo de estado de una instancia de back-end, las conexiones TCP establecidas con dicho back-end continúan.

Si se produce algún error en todos los sondeos de todas las instancias de un grupo de back-end, no se enviarán flujos nuevos a dicho grupo. Standard Load Balancer permitirá que los flujos de TCP establecidos continúen.  Basic Load Balancer terminará todos los flujos de TCP existente en el grupo de back-end.
 
Load Balancer es un servicio intermedio (no termina las conexiones TCP) y el flujo siempre se produce entre el cliente y el sistema operativo invitado y la aplicación de la máquina virtual. Un grupo con todos los sondeos inactivos provocará que un front-end no responda a los intentos de apertura de una conexión TCP (SYN), ya que no hay ninguna instancia de back-end en buen estado que reciba el flujo y responda con SYN-ACK.

### <a name="udp-datagrams"></a>Datagramas UDP

Los datagramas UDP se entregarán a las instancias de back-end en buen estado.

UDP no tiene conexión y no hay ningún estado de flujo que realice el seguimiento para UDP. Si se produce un error en el sondeo de estado de la instancia de cualquier back-end, los flujos UDP existentes se pueden mover a otra instancia en buen estado del grupo de back-end.

Si se produce un error en todos los sondeos de todas las instancias de un grupo de back-end, los flujos UDP terminarán para las instancias de Basic Load Balancer y Standard Load Balancer.

<a name="source"></a>
## <a name="probesource"></a>Dirección IP de origen del sondeo

Load Balancer usa un servicio de sondeo distribuido para su modelo de mantenimiento interno. El servicio de sondeos reside en todos los host donde residan las máquinas virtuales y se puede programar a petición para generar sondeos de estado por cada configuración de cliente. El tráfico del sondeo de estado se realiza directamente entre el servicio de sondeos que genera el sondeo de estado y la máquina virtual del cliente. Todos los sondeos de Load Balancer tienen como origen la dirección IP 168.63.129.16.  Puede usar el espacio de direcciones IP dentro de una red virtual que no sea el espacio RFC1918.  El uso de una dirección IP propiedad de Microsoft reservada de forma global reduce la posibilidad de un conflicto de dirección IP con el espacio de direcciones IP que se usa dentro de la red virtual.  Esta dirección IP es la misma en todas las regiones y no cambia, y no supone un riesgo de seguridad porque solo el componente de la plataforma interna de Azure puede originar un paquete desde esta dirección IP. 

La etiqueta del servicio AzureLoadBalancer identifica esta dirección IP de origen en los [grupos de seguridad de red](../virtual-network/security-overview.md) y permite el tráfico de sondeo de estado de forma predeterminada.

Además de los sondeos de estado de equilibrador de carga, el [siguientes operaciones de usar esta dirección IP](../virtual-network/what-is-ip-address-168-63-129-16.md):

- Permite al agente de VM comunicarse con la plataforma para indicar que se encuentra en estado "Listo"
- Permite la comunicación con el servidor virtual de DNS para proporcionar resolución de nombres filtrada a los clientes que no definen servidores DNS personalizados.  Este filtro garantiza que los clientes solo pueden resolver los nombres de host de su implementación.
- Permite que la máquina virtual obtenga una dirección IP dinámica desde el servicio DHCP en Azure.

## <a name="design"></a> Instrucciones de diseño

Los sondeos de estado se usan para hacer que el servicio sea resistente y se pueda escalar. Una configuración o un patrón de diseño incorrectos pueden afectar a la disponibilidad y escalabilidad del servicio. Revise todo el documento y tenga en cuenta el impacto en su escenario es cuando esta respuesta de sondeo se marca como inactiva o activa, y cómo afecta a la disponibilidad del escenario de la aplicación.

Al diseñar el modelo de estado de la aplicación, debe sondear un puerto en una instancia de back-end que refleje el estado de esa instancia __y__ el servicio de aplicación que se va a proporcionar.  El puerto de la aplicación y el puerto de sondeo no deben ser el mismo.  En algunos escenarios, puede ser conveniente que el puerto de sondeo sea diferente al que la aplicación usa para proporcionar el servicio.  

En ocasiones, puede ser útil que la aplicación genere una respuesta de sondeo de estado no solo para detectar el estado de la aplicación, sino también para señalar directamente a Load Balancer si la instancia debe recibir flujos nuevos o no.  Puede manipular la respuesta de sondeo para permitir que la aplicación cree contrapresión y limite la entrega de los flujos nuevos a una instancia si provoca un problema en el sondeo de estado, o bien prepara el mantenimiento de la aplicación e inicia el vaciado del escenario.  Cuando se usa Standard Load Balancer, una señal [de sondeo inactivo](#probedown) siempre permitirá que los flujos TCP continúen hasta el tiempo de espera de inactividad o el cierre de la conexión. 

En el caso del equilibrio de carga de UDP, debe generar una señal de sondeo de estado personalizada desde la instancia de back-end y usar un sondeo de estado TCP, HTTP o HTTPS destinado al agente de escucha correspondiente para reflejar el estado de la aplicación de UDP.

Si se usan [reglas de equilibrio de carga de puertos de alta disponibilidad](load-balancer-ha-ports-overview.md) con [Standard Load Balancer](load-balancer-standard-overview.md), todos los puertos tienen la carga equilibrada y una sola respuesta del sondeo de estado tiene que reflejar el estado de toda la instancia.

No se debe traducir ni conectar mediante proxy un sondeo de estado a través de la instancia que recibe el sondeo de estado con otra instancia de la red virtual, porque se podrían provocar errores en cascada en el escenario.  Considere el escenario siguiente: un conjunto de aplicaciones de terceros se implementa en el grupo de back-end de un recurso de Load Balancer para proporcionar escalabilidad y redundancia para los dispositivos, y el sondeo de estado se configura para sondear un puerto que la aplicación de terceros redirige mediante un proxy o traduce a otras máquinas virtuales detrás del dispositivo.  Si se sondea el mismo puerto que se usa para traducir o redirigir mediante un proxy a las otras máquinas virtuales detrás de la aplicación, cualquier respuesta de sondeo de una sola máquina virtual detrás de la aplicación marcará el propio dispositivo como inactivo. Esta configuración puede provocar un error en cascada del escenario de aplicación completo como resultado de una instancia de back-end única detrás del dispositivo.  El desencadenador puede ser un error de sondeo intermitente que provocará que Load Balancer marque como inactivo el destino original (la instancia de la aplicación) y, a su vez, puede deshabilitar el escenario de toda la aplicación. En su lugar, sondee el estado del propio dispositivo. La selección del sondeo para determinar la señal de estado es una consideración importante para los escenarios de aplicaciones de red virtual (NVA) y debe consultar con su proveedor de aplicaciones cuál es la señal de estado adecuada para esos escenarios.

Si en las directivas de firewall no se permite la [dirección IP de origen](#probesource) del sondeo, se producirá un error en el sondeo de estado, ya que no puede acceder a la instancia.  A su vez, Load Balancer marcará la instancia como inactiva debido al error del sondeo de estado,  Esta configuración incorrecta puede producir un error en el escenario de aplicación con equilibrio de carga.

Para que el sondeo de estado de Load Balancer marque la instancia como activa, se **debe** permitir esta dirección IP en todos los [grupos de seguridad de red](../virtual-network/security-overview.md) de Azure y en las directivas de firewall locales.  De forma predeterminada, todos los grupos de seguridad de red incluyen la [etiqueta de servicio](../virtual-network/security-overview.md#service-tags) AzureLoadBalancer para permitir el tráfico de sondeo de estado.

Si quiere probar un error de un sondeo de mantenimiento o marcar como inactiva una instancia individual, puede usar un [grupo de seguridad de red](../virtual-network/security-overview.md) para bloquear de forma explícita el sondeo de mantenimiento (puerto de destino o [dirección IP de origen](#probesource)) y simular el error del sondeo.

No configure la red virtual con el intervalo de direcciones IP propiedad de Microsoft que contiene 168.63.129.16.  Esas configuraciones entrarán en conflicto con la dirección IP del sondeo de estado y pueden provocar un error en el escenario.

Si tiene varias interfaces en la máquina virtual, es preciso que se asegure de que responde al sondeo en la interfaz en que lo recibió.  Es posible que tenga que traducir la dirección de red de origen de esta dirección en la máquina virtual para cada interfaz.

No habilite [las marcas de tiempo TCP](https://tools.ietf.org/html/rfc1323).  Habilitar las marcas de tiempo TCP hará que se produzca un error en los sondeos de estado debido a que la pila TCP del sistema operativo invitado de la máquina virtual quita los paquetes TCP y, como resultado, Load Balancer marca como inactivo el punto de conexión correspondiente.  Normalmente, las marcas de tiempo TCP están habilitadas de forma predeterminada en las imágenes de máquina virtual con seguridad reforzada y se deben deshabilitar.

## <a name="monitoring"></a>Supervisión

[Standard Load Balancer](load-balancer-standard-overview.md), tanto público como interno, expone el estado del sondeo de mantenimiento por punto de conexión e instancia de back-end como métricas multidimensionales mediante Azure Monitor. Estas métricas pueden consumirlos otros servicios de Azure o aplicaciones de asociados. 

Equilibrador de carga público básico expone del sondeo de estado resumida por grupo de back-end a través de los registros de Azure Monitor.  Registros de Azure Monitor no están disponibles para equilibradores de carga básico interna.  Puede usar [registros de Azure Monitor](load-balancer-monitor-log.md) para comprobar el estado de mantenimiento del sondeo de equilibrador de carga público y el número de sondeos. El registro se puede utilizar con Power BI o con Azure Operational Insights para proporcionar estadísticas del estado de mantenimiento del equilibrador de carga.

## <a name="limitations"></a>Limitaciones

- Los sondeos HTTPS no admiten la autenticación mutua con un certificado de cliente.
- Se producirá un error en los sondeos de estado cuando se habilitan las marcas de tiempo TCP.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Load Balancer Estándar](load-balancer-standard-overview.md)
- [Empiece a crear un equilibrador de carga público en Resource Manager mediante Azure PowerShell](load-balancer-get-started-internet-arm-ps.md)
- [API REST para los sondeos de estado](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerprobes/)
- Solicite nuevas capacidades de sondeo de estado con [Uservoice de Load Balancer](https://aka.ms/lbuservoice)
