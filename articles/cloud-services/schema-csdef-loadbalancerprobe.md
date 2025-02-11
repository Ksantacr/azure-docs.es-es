---
title: Esquema LoadBalancerProbe de definición de Azure Cloud Services | Microsoft Docs
ms.custom: ''
origin.date: 04/14/2015
ms.date: 11/06/2017
ms.prod: azure
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 113374a8-8072-4994-9d99-de391a91e6ea
caps.latest.revision: 14
author: thraka
ms.author: v-yiso
manager: timlt
ms.openlocfilehash: de365de7bf93c0a612f102b3ec2b25c79d1c3d18
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60613875"
---
# <a name="azure-cloud-services-definition-loadbalancerprobe-schema"></a>Esquema LoadBalancerProbe de definición de Azure Cloud Services
El sondeo del equilibrador de carga es un sondeo de mantenimiento definido por el cliente de los puntos de conexión UDP y los puntos de conexión de las instancias de rol. El elemento `LoadBalancerProbe` no es un elemento independiente; se combina con el rol web o el rol de trabajo en un archivo de definición de servicio. Un elemento `LoadBalancerProbe` se puede usar en más de un rol.

La extensión predeterminada del archivo de definición de servicio es. csdef.

## <a name="the-function-of-a-load-balancer-probe"></a>La función de un sondeo del equilibrador de carga
Azure Load Balancer es responsable de enrutar el tráfico entrante a las instancias de rol. El equilibrador de carga determina qué instancias pueden recibir tráfico, para lo cual sondea periódicamente cada instancia con el fin de determinar el mantenimiento de dicha instancia. El equilibrador de carga sondea cada instancia varias veces por minuto. Existen dos maneras diferentes de proporcionar mantenimiento de las instancias al equilibrador de carga: una es el sondeo predeterminado del equilibrador de carga y la otra es un sondeo personalizado del equilibrador de carga, que se implementa mediante la definición del elemento LoadBalancerProbe en el archivo .csdef.

En el sondeo predeterminado del equilibrador de carga se usa el agente invitado dentro de la máquina virtual, que escucha y responde con una respuesta HTTP 200 OK solo cuando la instancia está en estado Preparada (es decir, no se encuentra en estado Ocupada, Reciclando, Deteniendo, etc.). Si el agente invitado no responde con HTTP 200 OK, Azure Load Balancer marca la instancia como sin respuesta y deja de enviar tráfico a esa instancia. Azure Load Balancer sigue haciendo ping a la instancia y, si el agente invitado responde con HTTP 200, envía de nuevo el tráfico a esa instancia. Cuando se usa un rol web, el código del sitio web se ejecuta normalmente en w3wp.exe. Esto no lo supervisa el tejido de Azure o el agente invitado, lo que significa que los errores en w3wp.exe (por ejemplo, respuestas HTTP 500) no se notifican al agente invitado y el equilibrador de carga no sabe sacar esta instancia de la rotación.

El sondeo personalizado del equilibrador de carga anula el sondeo predeterminado del agente invitado y le permite crear su propia lógica personalizada para determinar el mantenimiento de la instancia de rol. El equilibrador de carga sondea su punto de conexión periódicamente (de forma predeterminada, cada 15 segundos) y la instancia se considera en rotación si responde con un mensaje TCP ACK o HTTP 200 dentro del período de tiempo de expiración (31 segundos de forma predeterminada). Esta situación puede resultar de utilidad para implementar su propia lógica y quitar instancias de la rotación del equilibrador de carga, por ejemplo, al devolver un estado no 200 si la instancia está por encima del 90 % de CPU. Para los roles web que usan w3wp.exe, esto también significa que dispone de supervisión automática de su sitio web, dado que los errores en el código de dicho sitio devuelven un estado no 200 en el sondeo del equilibrador de carga. Si no se define un elemento LoadBalancerProbe en el archivo .csdef, se usa el comportamiento predeterminado del equilibrador de carga (que se ha descrito anteriormente).

Si usa un sondeo personalizado del equilibrador de carga, debe asegurarse de que la lógica tenga en cuenta el método RoleEnvironment.OnStop. Cuando se usa el sondeo predeterminado del equilibrador de carga, la instancia se saca de la rotación antes de la llamada al evento OnStop, pero un sondeo personalizado del equilibrador de carga puede seguir devolviendo un mensaje 200 OK durante dicho evento. Si usa el evento OnStop para limpiar la caché, detener un servicio o realizar cambios que afectan al comportamiento en tiempo de ejecución de su servicio, debe asegurarse de que la lógica del sondeo personalizado del equilibrador de carga retire la instancia de la rotación.

## <a name="basic-service-definition-schema-for-a-load-balancer-probe"></a>Esquema básico de definición de servicio para un sondeo del equilibrador de carga
 El formato básico de un archivo de definición de servicio que contiene un sondeo del equilibrador de carga es el siguiente.

```xml
<ServiceDefinition …>
   <LoadBalancerProbes>
      <LoadBalancerProbe name="<load-balancer-probe-name>" protocol="[http|tcp]" path="<uri-for-checking-health-status-of-vm>" port="<port-number>" intervalInSeconds="<interval-in-seconds>" timeoutInSeconds="<timeout-in-seconds>"/>
   </LoadBalancerProbes>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Elementos de esquema
El elemento `LoadBalancerProbes` del archivo de definición de servicio incluye los siguientes elementos:

- [Elemento LoadBalancerProbes](#LoadBalancerProbes)
- [Elemento LoadBalancerProbe](#LoadBalancerProbe)

##  <a name="LoadBalancerProbes"></a> Elemento LoadBalancerProbes
El elemento `LoadBalancerProbes` describe la colección de sondeos del equilibrador de carga. Este elemento es el elemento primario del [elemento LoadBalancerProbe](#LoadBalancerProbe). 

##  <a name="LoadBalancerProbe"></a> Elemento LoadBalancerProbe
El elemento `LoadBalancerProbe` define el sondeo de mantenimiento de un modelo. Puede definir varios sondeos del equilibrador de carga. 

En la tabla siguiente se describen los atributos del elemento `LoadBalancerProbe`:

|Atributo|Type|DESCRIPCIÓN|
| ------------------- | -------- | -----------------|
| `name`              | `string` | Necesario. El nombre del sondeo del equilibrador de carga. El nombre debe ser único.|
| `protocol`          | `string` | Necesario. Especifica el protocolo del punto de conexión. Los valores posibles son `http` o `tcp`. Si se especifica `tcp`, se es necesario recibir una confirmación para que el sondeo se realice correctamente. Si se especifica `http`, se necesita una respuesta 200 OK del URI especificado para que el sondeo se realice correctamente.|
| `path`              | `string` | El URI que se usa para solicitar el estado de mantenimiento de la máquina virtual. Se requiere `path` si `protocol` está establecido en `http`. De lo contrario, no se permite.<br /><br /> No hay ningún valor predeterminado.|
| `port`              | `integer` | Opcional. El puerto para comunicar el sondeo. Este valor es opcional para cualquier punto de conexión, ya que el mismo puerto se usará para el sondeo. También puede configurar un puerto diferente para los sondeos. Los valores posibles oscilan entre 1 y 65535, ambos inclusive.<br /><br /> El valor predeterminado lo establece el punto de conexión.|
| `intervalInSeconds` | `integer` | Opcional. El intervalo, en segundos, de la frecuencia de sondeo del punto de conexión para conocer el estado de mantenimiento. Normalmente, el intervalo es algo inferior a la mitad del período de tiempo de expiración asignado (en segundos), lo que permite dos sondeos completos antes de sacar la instancia de la rotación.<br /><br /> El valor predeterminado es 15 y el valor mínimo es 5.|
| `timeoutInSeconds`  | `integer` | Opcional. El período de tiempo de expiración, en segundos, aplicado al sondeo cuando no hay respuesta provocará que se impida que el posterior tráfico se entregue en el punto de conexión. Este valor permite sacar de la rotación los puntos de conexión de manera más rápida o lenta que los tiempos habituales usados en Azure (que son los predeterminados).<br /><br /> El valor predeterminado es 31 y el valor mínimo es 11.|

## <a name="see-also"></a>Vea también
[Cloud Service (classic) Definition Schema](schema-csdef-file.md) (Esquema de definición de servicio en la nube [clásico])