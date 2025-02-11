---
title: Eventos programados para VM Linux en Azure| Microsoft Docs
description: Programe los eventos usando Azure Metadata Service para las máquinas virtuales Linux.
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: ''
author: ericrad
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ericrad
ms.openlocfilehash: 0831f08eaa3e8e6f6a0d3f68bc50cd927167b7ba
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507924"
---
# <a name="azure-metadata-service-scheduled-events-for-linux-vms"></a>Azure Metadata Service: Scheduled Events para máquinas virtuales Linux

Scheduled Events es un servicio de Azure Metadata Service que proporciona el tiempo de aplicación para prepararse para el mantenimiento de la máquina virtual. Da información sobre los eventos de mantenimiento próximos (por ejemplo, un reinicio) para que la aplicación pueda prepararlos y así limitar las interrupciones. Está disponible para todos los tipos de máquina virtual de Azure, incluidos IaaS y PaaS, tanto en Windows como en Linux. 

Para más información acerca de Scheduled Events en Windows, consulte [Scheduled Events para máquinas virtuales Windows](../windows/scheduled-events.md).

> [!Note] 
> Scheduled Events está disponible con carácter general en todas las regiones de Azure. Consulte el apartado sobre la [disponibilidad por región y versión](#version-and-region-availability) para obtener información sobre la versión más reciente.

## <a name="why-use-scheduled-events"></a>¿Por qué usar Scheduled Events?

Muchas aplicaciones pueden aprovechar el tiempo para preparar el mantenimiento de las máquinas virtuales. Este tiempo se puede usar para realizar tareas específicas de la aplicación con el fin de mejorar la disponibilidad, la confiabilidad y la capacidad de servicio, lo que incluye: 

- Punto de control y restauración.
- Purga de la conexión.
- Conmutación por error de la réplica principal.
- Eliminación de un grupo de equilibradores de carga.
- Registros de eventos.
- Cierre estable.

Con Scheduled Events, la aplicación puede detectar cuándo se producirá el mantenimiento y desencadenar tareas para limitar sus efectos.  

Eventos programados proporciona eventos en los casos de uso siguientes:

- [Mantenimiento iniciado por la plataforma](https://docs.microsoft.com/azure/virtual-machines/linux/maintenance-and-updates) (por ejemplo, VM reboot, migración en vivo o actualizaciones para el host de conservación de memoria)
- Hardware degradado
- Mantenimiento iniciado por el usuario (por ejemplo, el usuario reinicia o vuelve a implementar una máquina virtual)
- [Expulsión de la máquina virtual de baja prioridad](https://azure.microsoft.com/blog/low-priority-scale-sets) en escala establece

## <a name="the-basics"></a>Conceptos básicos  

  Metadata Service expone información sobre la ejecución de máquinas virtuales mediante un punto de conexión de REST accesible desde la propia máquina virtual. La información se encuentra disponible a través de una dirección IP no enrutable, de modo que no se expone fuera de la máquina virtual.

### <a name="scope"></a>Scope
Los eventos programados se entregan a:

- Virtual Machines independientes.
- Todas las máquinas virtuales en un servicio de nube.
- Todas las máquinas virtuales de un conjunto de disponibilidad.
- Todas las máquinas virtuales de un grupo de selección de ubicación de conjunto de escalado. 

Por ello, revise el campo `Resources` del evento para identificar cuáles son las máquinas virtuales que se verán afectadas.

### <a name="endpoint-discovery"></a>Detección de punto de conexión
En el caso de las máquinas virtuales con red virtual habilitada, el servicio de metadatos está disponible desde una dirección IP no enrutable estática, `169.254.169.254`. El punto de conexión completo de la versión más reciente de Scheduled Events es: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2017-11-01`

Si la máquina virtual no se crea dentro de una red virtual (la opción predeterminada para servicios en la nube y máquinas virtuales clásicas), se necesita lógica adicional para detectar la dirección IP que se va a usar. Para aprender a [detectar el punto de conexión de host](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm), consulte este ejemplo.

### <a name="version-and-region-availability"></a>Disponibilidad por región y versión
El servicio Scheduled Events tiene versiones. Las versiones son obligatorias; la actual es `2017-11-01`.

| Version | Tipo de versión | Regiones | Notas de la versión | 
| - | - | - | - | 
| 2017-11-01 | Disponibilidad general | Todo | <li> Se agregó compatibilidad para la expulsión de la máquina virtual de baja prioridad EventType 'Preempt'<br> | 
| 2017-08-01 | Disponibilidad general | Todo | <li> Se quitó el guion bajo antepuesto de los nombres de recursos en las máquinas virtuales de IaaS<br><li>Se aplicó el requisito de encabezado de metadatos para todas las solicitudes | 
| 2017-03-01 | Preview | Todo | <li>Versión inicial.


> [!NOTE] 
> Versiones preliminares de Scheduled Events admitidas {más recientes} como la versión de API. Este formato ya no es compatible y dejará de utilizarse en el futuro.

### <a name="enabling-and-disabling-scheduled-events"></a>Habilitación y deshabilitación de Scheduled Events
Scheduled Events se habilita para un servicio la primera vez que se realiza una solicitud de eventos. Debe esperar hasta dos minutos de demora en la respuesta en la primera llamada.

Scheduled Events está deshabilitado para su servicio si no realiza una solicitud durante 24 horas.

### <a name="user-initiated-maintenance"></a>Mantenimiento iniciado por el usuario
El mantenimiento de las máquinas virtuales iniciado por el usuario a través de Azure Portal, API, CLI o PowerShell, da lugar a un evento programado. Esto permite probar la lógica de preparación del mantenimiento en la aplicación, además de que la aplicación pueda prepararse para el mantenimiento iniciado por el usuario.

Si reinicia una máquina virtual, se programa un evento del tipo `Reboot`. Si vuelve a implementar una máquina virtual, se programa un evento del tipo `Redeploy`.

## <a name="use-the-api"></a>Uso de la API

### <a name="headers"></a>Encabezados
Al realizar consultas a Metadata Service, debe proporcionar el encabezado `Metadata:true` para asegurarse de que la solicitud no se haya redirigido de manera involuntaria. El encabezado `Metadata:true` es necesario para todas las solicitudes de eventos programados. Un error al incluir el encabezado en la solicitud generará una respuesta del tipo "Solicitud incorrecta" de Metadata Service.

### <a name="query-for-events"></a>Consulta de eventos
Puede consultar los eventos programados; para ello, simplemente haga la siguiente llamada:

#### <a name="bash"></a>Bash
```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

Una respuesta contiene una matriz de eventos programados. Una matriz vacía significa que actualmente no hay eventos programados.
En caso de que haya eventos programados, la respuesta contiene una matriz de eventos. 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze" | "Preempt",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a>Propiedades de evento
|Propiedad  |  DESCRIPCIÓN |
| - | - |
| EventId | Es un identificador único global del evento. <br><br> Ejemplo: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Tipo de evento | Es el impacto causado por el evento. <br><br> Valores: <br><ul><li> `Freeze`: La máquina Virtual está programada para pausarse durante unos segundos. Es posible que se suspenda la conectividad de red y CPU, pero no hay ningún impacto en la memoria o los archivos abiertos.<li>`Reboot`: la máquina virtual está programada para reiniciarse (se borrará la memoria no persistente). <li>`Redeploy`: la máquina virtual está programada para moverse a otro nodo (los discos efímeros se pierden). <li>`Preempt`: Se está eliminando la máquina Virtual de baja prioridad (discos efímeros se pierden).|
| ResourceType | Es el tipo de recurso al que este evento afecta. <br><br> Valores: <ul><li>`VirtualMachine`|
| Recursos| Es la lista de recursos a los que este evento afecta. Se garantiza que contenga máquinas de un [dominio de actualización](manage-availability.md) como máximo, pero puede no contener todas las máquinas en dicho dominio. <br><br> Ejemplo: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| EventStatus | Es el estado de este evento. <br><br> Valores: <ul><li>`Scheduled`: este evento está programado para iniciarse después de la hora especificada en la propiedad `NotBefore`.<li>`Started`: este evento se ha iniciado.</ul> Ni `Completed` ni otro estado similar se han proporcionado antes. El evento ya no vuelve cuando finaliza el evento.
| NotBefore| Hora a partir de la que puede iniciarse este evento. <br><br> Ejemplo: <br><ul><li> Lunes, 19 de septiembre de 2016, 18:29:47 GMT  |

### <a name="event-scheduling"></a>Programación de eventos
Cada evento se programa una cantidad mínima de tiempo en el futuro en función de su tipo. Este tiempo se refleja en la propiedad `NotBefore` de un evento. 

|Tipo de evento  | Aviso mínimo |
| - | - |
| Freeze| 15 minutos |
| Reiniciar | 15 minutos |
| Reimplementar | 10 minutos |
| Preferencia | 30 segundos |

### <a name="start-an-event"></a>Inicio de un evento 

Cuando se haya enterado de un evento próximo y completado la lógica para llevar a cabo un apagado estable, puede aprobar el evento pendiente mediante una llamada de `POST` a Metadata Service con `EventId`. Esta llamada indica a Azure que puede acortar el tiempo de notificación mínimo (cuando sea posible). 

A continuación se muestra el JSON de ejemplo en el cuerpo de la solicitud `POST`. La solicitud debe contener una lista de `StartRequests`. Cada `StartRequest` contiene el elemento `EventId` para el evento que desea acelerar:
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

#### <a name="bash-sample"></a>Ejemplo de Bash
```
curl -H Metadata:true -X POST -d '{"StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-11-01
```

> [!NOTE] 
> Reconocer un evento permite a este continuar para todos sus elementos `Resources`, no solo para la máquina virtual que lo reconoce. Por tanto, puede optar por elegir un líder para que coordine el reconocimiento. Este puede ser tan sencillo como la primera máquina del campo `Resources`.

## <a name="python-sample"></a>Ejemplo de Python 

En el siguiente ejemplo se realiza una consulta a Metadata Service para eventos programados y se aprueban todos los eventos pendientes:

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-11-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host " + this_host + " is scheduled for " + eventtype + " not before " + notbefore
            # Add logic for handling events here


def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)

if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a>Pasos siguientes 
- Mire [Scheduled Events on Azure Friday](https://channel9.msdn.com/Shows/Azure-Friday/Using-Azure-Scheduled-Events-to-Prepare-for-VM-Maintenance) (Scheduled Events en Azure Friday) para ver una demostración. 
- Repase los ejemplos de código de Scheduled Events en el [repositorio de GitHub Azure Instance Metadata Scheduled Events](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm) (Scheduled Events de Azure Instance Metadata).
- En [Instance Metadata Service](instance-metadata-service.md), puede obtener más información sobre las API disponibles.
- Obtenga información sobre cómo realizar [el mantenimiento planeado para máquinas virtuales Linux en Azure](planned-maintenance.md).
