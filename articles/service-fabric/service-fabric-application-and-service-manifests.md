---
title: Descripción de los servicios y las aplicaciones de Service Fabric | Microsoft Docs
description: Describe cómo se utilizan los manifiestos para describir los servicios y las aplicaciones de Service Fabric.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/19/2018
ms.author: atsenthi
ms.openlocfilehash: 5e93bb3b206fbef6beb09b7aca6df0742a80ccf1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60621520"
---
# <a name="service-fabric-application-and-service-manifests"></a>Manifiestos de servicio y de aplicación de Service Fabric
En este artículo se describe cómo se definen y tienen versiones los servicios y las aplicaciones de Service Fabric mediante los archivos ApplicationManifest.xml y ServiceManifest.xml.  Para obtener más ejemplos, consulte los [ejemplos de aplicaciones y manifiesto de servicio](service-fabric-manifest-examples.md).  El esquema XML para estos archivos de manifiesto se documenta en la [documentación del esquema ServiceFabricServiceModel.xsd](service-fabric-service-model-schema.md).

> [!WARNING]
> El esquema de archivo del manifiesto XML impone el orden correcto de los elementos secundarios.  Como solución alternativa parcial, abra "C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd" en Visual Studio durante la creación o modificación de cualquiera de los manifiestos de Service Fabric. Esto le permitirá comprobar el orden de los elementos secundarios y proporciona IntelliSense.

## <a name="describe-a-service-in-servicemanifestxml"></a>Descripción de un servicio en ServiceManifest.xml
El manifiesto de servicio define mediante declaración el tipo de servicio y la versión. Especifica los metadatos de servicio, como el tipo de servicio, las propiedades de estado, las métricas de equilibrio de carga, archivos binarios del servicio y archivos de configuración.  Dicho de otro modo, describe los paquetes de código, configuración y datos que componen un paquete de servicio para admitir uno o más tipos de servicio. Un manifiesto de servicio puede contener varios paquetes de código, configuración y datos, que puede tener varias versiones independientes. Este es un manifiesto de servicio para el servicio de front-end web de ASP.NET Core de la [aplicación de ejemplo de votación](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) (y estos son [algunos ejemplos más detallados](service-fabric-manifest-examples.md)):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

**Versión** son cadenas no estructuradas y no analizadas por el sistema. Los atributos de versión se usan para dotar a cada componente de varias versiones para las actualizaciones.

**ServiceTypes** declara qué tipos de servicio admite **CodePackages** en este manifiesto. Cuando se crea una instancia de un servicio en uno de estos tipos de servicio, todos los paquetes de código declarados en este manifiesto se activan mediante la ejecución de sus puntos de entrada. Se espera que los procesos resultantes registren los tipos de servicio admitidos en tiempo de ejecución. Los tipos de servicio se declaran en el nivel de manifiesto y no en el nivel de paquete de código. De modo que, cuando hay varios paquetes de código, se activan todos cada vez que el sistema busca cualquiera de los tipos de servicio declarados.

El archivo ejecutable especificado por **EntryPoint** suele ser el host de servicio de ejecución prolongada. **SetupEntryPoint** es un punto de entrada con privilegios que se ejecuta con las mismas credenciales que Service Fabric (normalmente, la cuenta *LocalSystem* ) antes que cualquier otro punto de entrada.  La presencia de un punto de entrada de configuración independiente evita tener que ejecutar el host de servicio con privilegios elevados durante largos períodos de tiempo. El archivo ejecutable especificado por **EntryPoint** se ejecuta después de salir de **SetupEntryPoint** correctamente. Si el proceso finaliza o se bloquea alguna vez, el proceso resultante se supervisa y reinicia (comenzando de nuevo con **SetupEntryPoint**).  

Los escenarios de uso de **SetupEntryPoint** habituales se corresponden a cuando se ejecuta un archivo ejecutable antes de iniciar el servicio o cuando se realiza una operación con privilegios elevados. Por ejemplo: 

* configuración e inicialización de las variables de entorno que el ejecutable del servicio necesita. Esto no se limita tan solo a los archivos ejecutables escritos a través de los modelos de programación de Service Fabric. Por ejemplo, npm.exe necesita algunas variables de entorno configuradas para implementar una aplicación node.js.
* Configuración del control de acceso mediante la instalación de certificados de seguridad.

Para más información acerca de cómo configurar SetupEntryPoint, consulte [Configuración de la directiva para un punto de entrada del programa de instalación del servicio](service-fabric-application-runas-security.md)

**EnvironmentVariables** (sin establecer en el ejemplo anterior) proporciona una lista de variables de entorno que se establecen para este paquete de código. Las variables de entorno se pueden invalidar en `ApplicationManifest.xml` para proporcionar valores diferentes para distintas instancias de servicio. 

**DataPackage** (sin establecer en el ejemplo anterior) declara una carpeta denominada por el atributo **Nombre** que contiene datos estáticos arbitrarios que va a consumir el proceso en tiempo de ejecución.

**ConfigPackage** declara una carpeta denominada por el atributo **Nombre** que contiene un archivo *Settings.xml*. El archivo de configuración contiene secciones de configuración del par clave-valor definido por el usuario que el proceso vuelve a leer en tiempo de ejecución. Durante una actualización, si solo ha cambiado la **versión** de **ConfigPackage**, no se reiniciará el proceso en ejecución. En su lugar, una devolución de llamada notifica el proceso que los ajustes de configuración han cambiado, por lo que pueden volver a cargarse de forma dinámica. Este es un ejemplo de archivo *Settings.xml* :

```xml
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

Un **punto de conexión** del servicio Service Fabric es un ejemplo de un recurso de Service Fabric; un recurso de Service Fabric puede declararse o cambiarse sin cambiar el código compilado. El acceso a los recursos de Service Fabric especificados en el manifiesto de servicio puede controlarse a través de **SecurityGroup** en el manifiesto de aplicación. Cuando se define un recurso de punto de conexión en el manifiesto de servicio, Service Fabric asigna puertos desde el intervalo de puertos reservados de aplicación cuando un puerto no se especifica expresamente. Obtenga más información sobre la [especificación o reemplazo de los recursos de punto de conexión](service-fabric-service-manifest-resources.md).


<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application-in-applicationmanifestxml"></a>Descripción de una aplicación en ApplicationManifest.xml
Mediante declaración, el manifiesto de aplicación describe el tipo de aplicación y la versión. Especifica metadatos de composición de servicios como nombres estables, esquema de particiones, recuento de instancias/factor de replicación, directiva de seguridad y aislamiento, restricciones de ubicación, reemplazos de configuración y tipos de servicio constituyentes. También se describen los dominios de equilibrio de carga en los que se coloca la aplicación.

Por lo tanto, un manifiesto de aplicación describe elementos en el nivel de aplicación y hace referencia a uno o más manifiestos de servicio para componer un tipo de aplicación. Este es el manifiesto de aplicación para la [aplicación de ejemplo de votación](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) (y estos algunos [ejemplos más detallados](service-fabric-manifest-examples.md)):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

Al igual que los manifiestos de servicio, los atributos **Versión** son cadenas no estructuradas y no analizadas por el sistema. Los atributos de versión también se usan para dotar a cada componente de varias versiones para las actualizaciones.

**Parameters** define los parámetros utilizados en el manifiesto de aplicación. Los valores de estos parámetros pueden proporcionarse cuando se crea una instancia de la aplicación y se pueden usar para invalidar los valores de configuración del servicio o de la aplicación.  Se usa el valor por defecto del parámetro si dicho valor no se cambia durante la creación de una instancia de la aplicación. Para información sobre cómo mantener diferentes parámetros de aplicación y servicio para entornos individuales, vea [Administración de los parámetros de la aplicación en varios entornos](service-fabric-manage-multiple-environment-app-configuration.md).

**ServiceManifestImport** contiene referencias a manifiestos de servicio que componen este tipo de aplicación. Un manifiesto de aplicación puede contener varias importaciones del manifiesto de servicio y cada una puede tener varias versiones independientes. Los manifiestos de servicio importados determinan qué tipos de servicio son válidos dentro de este tipo de aplicación. Dentro de ServiceManifestImport se invalidan los valores de configuración en Settings.xml y las variables de entorno en los archivos ServiceManifest.xml. Las **directivas** (sin establecer en el ejemplo anterior) para seguridad y acceso, enlace de puntos de conexión y uso compartido de paquetes pueden establecerse en manifiestos de servicio importados.  Para obtener más información, vea [Configuración de directivas de seguridad para la aplicación](service-fabric-application-runas-security.md).

**DefaultServices** declara instancias de servicio que se crean automáticamente cada vez que se crea una instancia de una aplicación en este tipo de aplicación. Los servicios predeterminados son simplemente una comodidad y se comportan como servicios normales en todos los aspectos después de su creación. Se actualizan junto con cualquier otro servicio de la instancia de aplicación y también se pueden quitar. Un manifiesto de aplicación puede contener varios servicios predeterminados.

**Certificates** (sin establecer en el ejemplo anterior) declara los certificados que se usan para [configurar puntos de conexión HTTPS](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) o [cifrar secretos en el manifiesto de aplicación](service-fabric-application-secret-management.md).

**Policies** (sin establecer en el ejemplo anterior) describe las directivas que se establecerán en el nivel de la aplicación sobre recopilación de registros, [ejecución predeterminada](service-fabric-application-runas-security.md), [mantenimiento](service-fabric-health-introduction.md#health-policies) y [acceso de seguridad](service-fabric-application-runas-security.md).

**Principals** (sin establecer en el ejemplo anterior) describen las entidades de seguridad (usuarios o grupos) necesarias para [ejecutar servicios y asegurar recursos de servicio](service-fabric-application-runas-security.md).  Se hace referencia a las entidades de seguridad en las secciones **Policies**.





<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Pasos siguientes
- [Empaquete una aplicación](service-fabric-package-apps.md) y prepárela para la implementación.
- [Implementar y quitar aplicaciones](service-fabric-deploy-remove-applications.md).
- [Configure parameters and environment variables for different application instances](service-fabric-manage-multiple-environment-app-configuration.md) (Administración de los parámetros y las variables de entorno para varias instancias de aplicación).
- [Configuración de directivas de seguridad para la aplicación](service-fabric-application-runas-security.md).
- [Especificación de un punto de conexión HTTPS](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service).
- [Encrypt secrets in the application manifest](service-fabric-application-secret-management.md) (Cifrado de secretos en el manifiesto de aplicación).

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png



