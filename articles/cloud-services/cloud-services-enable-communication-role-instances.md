---
title: Comunicación para roles en Cloud Services | Microsoft Docs
description: Las instancias de rol de Cloud Services pueden tener definidos puntos de conexión (http, https, tcp y udp) que se comunican con el exterior o entre otras instancias de rol.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 7008a083-acbe-4fb8-ae60-b837ef971ca1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: jeconnoc
ms.openlocfilehash: 8b521ebe869210b66ac3b3efeebda873f7c0e50b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60519424"
---
# <a name="enable-communication-for-role-instances-in-azure"></a>Habilitar la comunicación para instancias de rol en Azure
Los roles de servicio en la nube se comunican a través de conexiones internas y externas. Las conexiones externas se denominan **puntos de conexión de entrada** mientras que conexiones internas se denominan **puntos de conexión internos**. En este tema se describe cómo modificar la [definición de servicio](cloud-services-model-and-package.md#csdef) para crear puntos de conexión.

## <a name="input-endpoint"></a>Extremo de entrada
Los extremos de entrada se usan para exponer un puerto al exterior. Tiene que especificar el tipo de protocolo y el puerto del extremo que se aplicará a los puertos internos y externos del extremo. Si lo desea, puede especificar un puerto interno diferente para el extremo mediante el atributo [localPort](/previous-versions/azure/reference/gg557552(v=azure.100)#InputEndpoint) .

El extremo de entrada puede usar los siguientes protocolos: **http, https, tcp y udp**.

Para crear un punto de conexión de entrada, agregue el elemento secundario **InputEndpoint** al elemento **Endpoints** de un rol web o de trabajo.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Extremo de entrada de instancia
Los extremos de entrada de instancia son similares a los extremos de entrada, pero los primeros permiten asignar puertos específicos de acceso público a cada instancia de rol individual mediante el enrutamiento de puertos en el equilibrador de carga. Puede especificar un único puerto público o un intervalo de puertos.

Solo puede utilizar el punto de conexión de entrada de instancia como protocolo **tcp** o **udp**.

Para crear un punto de conexión de entrada de instancia, agregue el elemento secundario **InstanceInputEndpoint** al elemento **Endpoints** del rol web o de trabajo.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Punto de conexión interno
Los extremos internos están disponibles para la comunicación entre instancias. El puerto es opcional y si se omite, se asigna un puerto dinámico al extremo. Además, puede usar un intervalo de puertos. Recuerde que hay un límite de cinco extremos internos por rol.

El extremo interno puede usar los siguientes protocolos: **http, tcp, udp y any**.

Para crear un punto de conexión de entrada interno, agregue el elemento secundario **InternalEndpoint** al elemento **Endpoints** de un rol web o de trabajo.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

También puede usar un intervalo de puertos.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8999" min="8995" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Roles de trabajo en comparación con los Roles web
Hay una pequeña diferencia entre los extremos cuando se trabaja con roles web y de trabajo. El rol web debe tener como mínimo un extremo de entrada que use el protocolo **HTTP** .

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>Uso del SDK de .NET para tener acceso a un extremo
La Biblioteca administrada de Azure proporciona métodos a las instancias de rol para que puedan comunicarse en tiempo de ejecución. Mediante el código que se ejecuta en una instancia de rol, puede recuperar información sobre la existencia de otras instancias de rol y sus extremos, así como información sobre la instancia de rol actual.

> [!NOTE]
> Solo puede recuperar información sobre las instancias de rol que se ejecuten en el servicio en la nube y que definan, al menos, un extremo interno. Asimismo, no puede obtener datos sobre instancias de rol que se ejecuten en un servicio diferente.
> 
> 

Puede usar la propiedad [Instances](/previous-versions/azure/reference/ee741904(v=azure.100)) para recuperar las instancias de rol. Primero, use el elemento [CurrentRoleInstance](/previous-versions/azure/reference/ee741907(v=azure.100)) para devolver una referencia a la instancia de rol actual y, a continuación, use la propiedad [Role](/previous-versions/azure/reference/ee741918(v=azure.100)) para devolver una referencia al propio rol.

Si se conecta a una instancia de rol mediante programación a través del SDK de .NET, le será relativamente fácil obtener acceso a la información del extremo. Por ejemplo, una vez se haya conectado a un entorno de rol específico, puede obtener el puerto de un extremo en concreto con este código:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

La propiedad **Instances** devuelve una colección de objetos **RoleInstance**. Tenga en cuenta que esta colección siempre contiene la instancia actual. Si el rol no define un extremo interno, la colección incluirá la instancia actual, pero no otras instancias. El número de instancias de rol presentes en la colección siempre será 1, si resulta que no se ha definido ningún extremo interno para el rol. Si el rol define un extremo interno, sus instancias serán reconocibles en tiempo de ejecución y el número de instancias de la colección se corresponderá con el número de instancias especificadas para el rol en el archivo de configuración de servicio.

> [!NOTE]
> La Biblioteca administrada de Azure no le proporciona una manera de determinar el estado de otras instancias de rol, pero siempre puede implementar estas evaluaciones de estado si el servicio necesita esta funcionalidad. Puede usar [Diagnósticos de Azure](cloud-services-dotnet-diagnostics.md) para obtener información acerca de las instancias de rol que se estén ejecutando.
> 
> 

Para determinar el número de puerto de un extremo interno en una instancia de rol, puede usar la propiedad [InstanceEndpoints](/previous-versions/azure/reference/ee741917(v=azure.100)) para devolver un objeto Dictionary que contenga los nombres de extremo y sus direcciones IP y puertos correspondientes. La propiedad [IPEndpoint](/previous-versions/azure/reference/ee741919(v=azure.100)) devuelve la dirección IP y el puerto de un extremo específico. La propiedad **PublicIPEndpoint** devuelve el puerto de un extremo con equilibrio de carga. La parte de la dirección IP de la propiedad **PublicIPEndpoint** no se usa.

Aquí tiene un ejemplo que itera las instancias de rol.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Aquí tiene el ejemplo de un rol de trabajo que obtiene el extremo expuesto a través de la definición de servicio, y que inicia la escucha de conexiones.

> [!WARNING]
> Este código solo funcionará en un servicio implementado. Cuando se ejecutan en el Emulador de Azure Compute, los elementos de configuración de servicio que crean extremos de puerto directo (elementos**InstanceInputEndpoint** ) se ignoran.
> 
> 

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;

        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;

        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);

        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at https://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Reglas de tráfico de red para controlar la comunicación entre roles
Una vez definidos los extremos internos, puede agregar reglas de tráfico de red (basadas en los extremos que haya creado) para controlar el modo en que se comunican las instancias de rol. En el siguiente diagrama verá algunos escenarios comunes para controlar la comunicación entre roles:

![Escenarios de reglas de tráfico de red](./media/cloud-services-enable-communication-role-instances/scenarios.png "Escenarios de reglas de tráfico de red")

En el siguiente ejemplo de código verá las definiciones de rol de los roles que se mostraban en el diagrama anterior. Cada definición de rol incluye, al menos, un extremo interno definido:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [!NOTE]
> Es posible que los extremos internos de tanto puertos asignados fijos como automáticos tengan restricciones de comunicación entre roles.
> 
> 

De forma predeterminada, una vez definido el extremo interno, la comunicación puede fluir sin restricciones desde cualquier rol hasta el extremo interno de otro rol. Para restringir la comunicación, debe agregar un elemento **NetworkTrafficRules** al elemento **ServiceDefinition** que se encuentra en el archivo de definición de servicio.

### <a name="scenario-1"></a>Escenario 1.
Permitir solo el tráfico de red de **WebRole1** a **WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Escenario 2.
Permitir solo el tráfico de red de **WebRole1** a **WorkerRole1** y **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Escenario 3.
Permitir solo el tráfico de red de **WebRole1** a **WorkerRole1** y **WorkerRole1** a **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Escenario 4.
Permitir solo el tráfico de red de **WebRole1** a **WorkerRole1**, **WebRole1** a **WorkerRole2** y **WorkerRole1** a **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Puede encontrar una referencia del esquema XML de los elementos usados anteriormente [aquí](/previous-versions/azure/reference/gg557551(v=azure.100)).

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre el [modelo](cloud-services-model-and-package.md)del servicio en la nube.

