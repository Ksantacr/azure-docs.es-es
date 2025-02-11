---
title: 'Azure Site Recovery: Preguntas más frecuentes | Microsoft Docs'
description: En este artículo se analizan las preguntas más frecuentes acerca de Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: f2d64e0a081ff483be84053c442f48e7d145ca50
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66396499"
---
# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Recovery: preguntas más frecuentes (P+F)
En este artículo se resume las preguntas más frecuentes sobre Azure Site Recovery.</br>
Para consultas concretas en ASR diferentes escenarios, consulte escenario específicos preguntas más frecuentes.<br>

- [Recuperación ante desastres de máquinas virtuales de Azure en Azure](azure-to-azure-common-questions.md)
- [Recuperación ante desastres de máquinas virtuales de VMware en Azure](vmware-azure-common-questions.md)
- [Recuperación ante desastres de máquinas virtuales de Hyper-V en Azure](hyper-v-azure-common-questions.md)
 
## <a name="general"></a>General

### <a name="what-does-site-recovery-do"></a>¿Qué hace Site Recovery?
Site Recovery contribuye a su estrategia de continuidad empresarial (BCDR por su sigla en inglés) y recuperación ante desastres mediante la coordinación y la automatización de la replicación de máquinas virtuales de Azure entre regiones, máquinas virtuales locales y servidores físicos a Azure, y máquinas locales a un centro de datos secundario. [Más información](site-recovery-overview.md).

### <a name="can-i-protect-a-virtual-machine-that-has-a-docker-disk"></a>¿Puedo proteger una máquina virtual que tiene un disco de Docker?

No, se trata de un escenario no admitido.

## <a name="service-providers"></a>Proveedores de servicios

### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Soy un proveedor de servicios. ¿Site Recovery funciona para los modelos de infraestructura compartida o específica?
Sí, Site Recovery admite ambos modelos de infraestructura, dedicados y compartidos.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Para un proveedor de servicios, ¿la identidad de mi inquilino se comparte con el servicio Site Recovery?
 No. La identidad del inquilino permanece anónima. Los inquilinos no necesitan acceso al portal de Site Recovery. Solo el administrador del proveedor de servicios realiza acciones en el portal.

### <a name="will-tenant-application-data-ever-go-to-azure"></a>¿Los datos de la aplicación de mis inquilinos llegarán a Azure?
Cuando se replica entre sitios que pertenecen al proveedor de servicios, los datos de la aplicación nunca llegan a Azure. Los datos se cifran en tránsito y se replican directamente entre los sitios del proveedor de servicios.

Si está replicando a Azure, los datos de la aplicación se envían al almacenamiento de Azure, pero no al servicio Site Recovery. Los datos se cifran en tránsito y permanecen cifrados en Azure.

### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>¿Recibirán mis inquilinos una factura por los servicios de Azure?
 No. La relación de facturación de Azure se entabla directamente con el proveedor de servicios. Los proveedores de servicios son responsables de generar facturas específicas para sus inquilinos.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Si se está replicando a Azure, ¿es necesario ejecutar máquinas virtuales en Azure en todo momento?
No, los datos se replican en Azure storage en su suscripción. Al realizar una conmutación por error de prueba (obtención de detalles de recuperación ante desastres) o una conmutación por error real, Site Recovery crea automáticamente las máquinas virtuales en su suscripción.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>¿Se puede garantizar el aislamiento a nivel de inquilino al replicar a Azure?
Sí.

### <a name="what-platforms-do-you-currently-support"></a>¿Qué plataformas se admiten actualmente?
Se admite Azure Pack, el sistema de plataforma en la nube e implementaciones basadas en System Center (2012 y superiores). [Más información](https://technet.microsoft.com/library/dn850370.aspx) acerca de la integración de Azure Pack y Site Recovery.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>¿Se admite un paquete Azure Pack sencillo e implementaciones de servidor VMM individuales?
Sí, es posible replicar máquinas virtuales de Hyper-V en Azure, o entre sitios del proveedor de servicios.  Tenga en cuenta que si se replica entre sitios del proveedor de servicios, la integración de runbooks de Azure no estará disponible.

## <a name="pricing"></a>Precios

### <a name="where-can-i-find-pricing-information"></a>¿Dónde puedo encontrar información sobre los precios?
Revisión [precios de Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) detalles.


### <a name="how-can-i-calculate-approximate-charges-during-the-use-of-site-recovery"></a>¿Cómo puedo calcular los cargos aproximados durante el uso de Site Recovery?

Puede usar el [Calculadora de precios](https://aka.ms/asr_pricing_calculator) para estimar los costos mientras usa Site Recovery.

Para la estimación detallada en los costos, ejecute la herramienta deployment planner para [VMware](https://aka.ms/siterecovery_deployment_planner) o [Hyper-V](https://aka.ms/asr-deployment-planner)y usar el [informe de estimación de costos](https://aka.ms/asr_DP_costreport).


### <a name="managed-disks-are-now-used-to-replicate-vmware-vms-and-physical-servers-do-i-incur-additional-charges-for-the-cache-storage-account-with-managed-disks"></a>Ahora se usan discos administrados para replicar máquinas virtuales de VMware y servidores físicos. ¿Incurrir en cargos adicionales para la cuenta de almacenamiento de caché con discos administrados?

No, no hay ningún cargo adicional para la caché. Cuando se replica en la cuenta de almacenamiento estándar, este almacenamiento en caché es parte de la misma cuenta de almacenamiento de destino.

### <a name="i-have-been-an-azure-site-recovery-user-for-over-a-month-do-i-still-get-the-first-31-days-free-for-every-protected-instance"></a>He sido usuario de Azure Site Recovery durante más de un mes. ¿Los treinta y un primeros días serán gratuitos para cada instancia protegida?

Sí. Ninguna instancia protegida incurre en cargos por Azure Site Recovery durante los primeros 31 días. Por ejemplo, si ha tenido 10 instancias durante los últimos 6 meses y conecta una 11ª instancia a Azure Site Recovery, no hay ningún cargo de la instancia de 11 para los primeros 31 días. Las 10 primeras instancias continúan incurriendo en cargos por Azure Site Recovery porque protegió durante más de 31 días.

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>Durante los primeros 31 días, ¿puedo incurrir en otros cargos de Azure?

Sí, aunque Site Recovery sea gratuito durante los primeros 31 días de una instancia protegida, puede incurrir en cargos por Azure Storage, transacciones de almacenamiento y transferencia de datos. Una máquina virtual recuperada también puede incurrir en cargos por proceso de Azure.


### <a name="is-there-a-cost-associated-to-perform-disaster-recovery-drillstest-failover"></a>¿Hay un costo asociado para realizar la conmutación por error el simulacros y pruebas de recuperación ante desastres?

No hay ningún costo independiente para exploraciones de recuperación ante desastres. Habrá gastos de proceso una vez creada la máquina virtual después de la conmutación por error de prueba.



## <a name="security"></a>Seguridad

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>¿Se envían mis datos de replicación al servicio de Site Recovery?
No, Site Recovery no intercepta los datos replicados ni tiene información sobre lo que se ejecuta en sus máquinas virtuales o servidores físicos.
Los datos de replicación se intercambian entre hosts locales de Hyper-V, hipervisores de VMware o servidores físicos y el Almacenamiento de Azure en el sitio secundario. Site Recovery no tiene capacidad para interceptar los datos. Únicamente se envían los metadatos necesarios para coordinar la replicación y la conmutación por error al servicio Site Recovery.  

Site Recovery está certificado según la norma ISO 27001:2013, 27018, además de HIPAA y DPA, y está completando sus evaluaciones de SOC2 y FedRAMP JAB.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Para garantizar el cumplimiento, incluso los metadatos de nuestros entornos locales deben permanecer dentro de la misma región geográfica. ¿Site Recovery puede ayudarnos?
Sí. Al crear un almacén de Site Recovery en una región, garantizamos que todos los metadatos que necesitamos para habilitar y coordinar la replicación y la conmutación por error permanezcan dentro del límite geográfico de esa región.

### <a name="does-site-recovery-encrypt-replication"></a>¿Site Recovery cifra la replicación?
Al replicar máquinas virtuales y servidores físicos entre sitios locales, se admite el cifrado en tránsito. En el caso de las máquinas virtuales y los servidores físicos que se replican en Azure, se admite tanto el cifrado en tránsito como el [cifrado en reposo (en Azure)](https://docs.microsoft.com/azure/storage/storage-service-encryption).




## <a name="disaster-recovery"></a>Recuperación ante desastres

### <a name="what-can-site-recovery-protect"></a>¿Qué se puede proteger con Site Recovery?
* **Máquinas virtuales de Azure**: Site Recovery puede replicar cualquier carga de trabajo que se ejecute en una máquina virtual de Azure compatible.
* **Máquinas virtuales de Hyper-V**: Site Recovery puede proteger cualquier carga de trabajo que se ejecute en una máquina virtual de Hyper-V.
* **Servidores físicos**: Site Recovery puede proteger servidores físicos con Windows o Linux.
* **Máquinas virtuales de VMware**: Site Recovery puede proteger cualquier carga de trabajo que se ejecute en una máquina virtual de VMware.

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>¿Qué cargas de trabajo se pueden proteger con Site Recovery?
Puede usar Site Recovery para proteger la mayoría de las cargas de trabajo que se ejecutan en una máquina virtual o un servidor físico. Site Recovery proporciona compatibilidad para la replicación con reconocimiento de aplicaciones para que estas se puedan recuperar a un estado inteligente. Se integra con aplicaciones de Microsoft como SharePoint, Exchange, Dynamics, SQL Server y Active Directory; además, colabora estrechamente con los principales proveedores, como Oracle, SAP, IBM y Red Hat. [Obtenga más información](site-recovery-workload.md) acerca de la protección de la carga de trabajo.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>¿Es posible administrar la recuperación ante desastres para las sucursales con Site Recovery?
Sí. Si usa Site Recovery para coordinar la replicación y la conmutación por error en las sucursales, disfrutará de una coordinación unificada y una vista de todas las cargas de trabajo de las sucursales desde un mismo punto. Puede ejecutar con facilidad conmutaciones por error y administrar la recuperación ante desastres de todas las sucursales desde la sede central, sin necesidad de visitar estas sucursales.


### <a name="is-disaster-recovery-supported-for-azure-vms"></a>¿Se admite la recuperación ante desastres para máquinas virtuales de Azure?

Sí, Site Recovery admite ante desastres para máquinas virtuales de Azure entre regiones de Azure. [Revise las preguntas más frecuentes](azure-to-azure-common-questions.md) sobre recuperación ante desastres de máquinas virtuales de Azure.

### <a name="is-disaster-recovery-supported-for-vmware-vms"></a>¿Se admite la recuperación ante desastres para máquinas virtuales de VMware?

Sí, Site Recovery admite la recuperación ante desastres de máquinas virtuales de VMware locales. [Revise las preguntas más frecuentes](vmware-azure-common-questions.md) para recuperación ante desastres de máquinas virtuales de VMware.

### <a name="is-disaster-recovery-supported-for-hyper-v-vms"></a>¿Se admite la recuperación ante desastres para máquinas virtuales de Hyper-V?
Sí, Site Recovery admite la recuperación ante desastres de máquinas virtuales de Hyper-V locales. [Revise las preguntas más frecuentes](hyper-v-azure-common-questions.md) para la recuperación ante desastres de máquinas virtuales de Hyper-V.

## <a name="is-disaster-recovery-supported-for-physical-servers"></a>¿Se admite la recuperación ante desastres para servidores físicos?
Sí, Site Recovery admite la recuperación ante desastres de servidores físicos locales que ejecutan Windows y Linux en Azure o en un sitio secundario. Obtenga información acerca de los requisitos para la recuperación ante desastres en [Azure](vmware-physical-azure-support-matrix.md#replicated-machines)y a[un sitio secundario](vmware-physical-secondary-support-matrix.md#replicated-vm-support).
Tenga en cuenta que los servidores físicos se ejecutan como máquinas virtuales en Azure después de la conmutación por error. Actualmente no se admite la conmutación por recuperación desde Azure a un servidor físico en el entorno local. Solo puede producir un error a una máquina virtual de VMware.





## <a name="replication"></a>Replicación

### <a name="can-i-replicate-over-a-site-to-site-vpn-to-azure"></a>¿Puedo replicar a través de una VPN de sitio a sitio en Azure?
Azure Site Recovery replica los datos a una cuenta de almacenamiento de Azure o los discos administrados, a través de un punto de conexión público. La replicación no se realiza a través de una VPN de sitio a sitio. 

### <a name="why-cant-i-replicate-over-vpn"></a>¿Por qué no puedo replicar a través de VPN?

Cuando se replica en Azure, el tráfico de replicación alcanza los extremos públicos de Azure Storage. Por lo tanto solo puede replicar a través de internet con ExpressRoute (emparejamiento público) y VPN no funciona.

### <a name="can-i-use-riverbed-steelheads-for-replication"></a>¿Puedo usar Riverbed SteelHeads para la replicación?

Nuestro socio, Riverbed, proporciona instrucciones detalladas sobre cómo trabajar con Azure Site Recovery. Revise sus [Guía de solución](https://community.riverbed.com/s/article/DOC-4627).

### <a name="can-i-use-expressroute-to-replicate-virtual-machines-to-azure"></a>¿Puedo usar ExpressRoute para replicar máquinas virtuales en Azure?
Sí, [se puede usar ExpressRoute](concepts-expressroute-with-site-recovery.md) para replicar máquinas virtuales locales en Azure.

- Azure Site Recovery replica los datos a Azure Storage a través de un punto de conexión público. Es necesario configurar el [emparejamiento público](../expressroute/expressroute-circuit-peerings.md#publicpeering) o el [emparejamiento de Microsoft](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) si se va a usar ExpressRoute para la replicación de Site Recovery.
- El emparejamiento de Microsoft es el dominio de enrutamiento recomendado para la replicación.
- Una vez que las máquinas virtuales se hayan conmutado por error a una red virtual de Azure,podrá acceder a ellas mediante la configuración [entre pares privados](../expressroute/expressroute-circuit-peerings.md#privatepeering) con la red virtual de Azure.
- La replicación no se puede realizar a través de un enrutamiento privado.
- Si va a proteger las máquinas de VMware o máquinas físicas, asegúrese de que el servidor de configuración cumpla con [requisitos de red](vmware-azure-configuration-server-requirements.md#network-requirements) para la replicación. 



### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-or-managed-disk-do-i-need"></a>¿Si se replica a Azure, qué tipo de cuenta de almacenamiento o un disco administrado necesito?

Necesita un almacenamiento LRS o GRS. Se recomienda GRS para que los datos sean resistentes si se produce una interrupción regional o si no se puede recuperar la región principal. La cuenta debe estar en la misma región que el almacén de Recovery Services. Al implementar Site Recovery en Azure Portal, Premium Storage es compatible con máquinas virtuales de VMware, máquinas virtuales de Hyper-V y con la replicación de servidores físicos. Discos administrados solo son compatibles con LRS.

### <a name="how-often-can-i-replicate-data"></a>¿Con qué frecuencia se pueden replicar los datos?
* **Hyper-V:** Las máquinas virtuales de Hyper-V se pueden replicar cada cinco minutos, o 30 segundos (excepto para el almacenamiento premium)
* **Servidores físicos de Azure las máquinas virtuales, máquinas virtuales de VMware:** en este caso no es relevante la frecuencia de replicación. La replicación es continua.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>¿Se puede ampliar la replicación desde el sitio de recuperación existente a otro tercer sitio?
No se admite la replicación extendida o encadenada. Solicite esta característica en el [foro de comentarios](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>¿Se puede hacer una replicación sin conexión la primera vez que se replique en Azure?
No es una opción admitida. Solicite esta característica en el [foro de comentarios](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-specific-disks-from-replication"></a>¿Se pueden excluir discos específicos de la replicación?
Esto es posible al replicar máquinas virtuales de VMware y máquinas virtuales de Hyper-V a Azure, mediante Azure Portal.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>¿Se pueden replicar máquinas virtuales con discos dinámicos?
Discos dinámicos se admiten al replicar máquinas virtuales de Hyper-V y al replicar máquinas virtuales de VMware y físicas en Azure. El disco del sistema operativo debe ser un disco básico.


### <a name="can-i-throttle-bandwidth-allotted-for-replication-traffic"></a>¿Puedo limitar el ancho de banda asignado para el tráfico de replicación?
Sí. Puede leer más acerca de la limitación de ancho de banda en estos artículos:

* [Capacity planning for replicating VMware VMs and physical servers](site-recovery-plan-capacity-vmware.md)
* [Capacity planning for replicating Hyper-V VMs without VMM (Planeamiento de la capacidad para replicar máquinas virtuales VMware y servidores físicos)](site-recovery-capacity-planning-for-hyper-v-replication.md)



## <a name="failover"></a>Conmutación por error
### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-vms-after-failover"></a>Si se realiza a través de Azure, ¿cómo accedo a las máquinas virtuales de Azure después de la conmutación por error?

Es posible tener acceso a las máquinas virtuales de Azure a través de una conexión segura a Internet o a través de una VPN de sitio a sitio o mediante Azure ExpressRoute. Deberá preparar algunas cosas para la conexión. [Más información](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Si se realiza una conmutación por error a Azure, ¿cómo se asegura Azure de que los datos resistan el proceso?
Azure está diseñado para la resistencia. Ya se ha diseñado para la conmutación por error a un centro de datos Azure secundaria, según el SLA de Azure Site Recovery. Si esto ocurre, nos aseguraremos de que los metadatos y los almacenes permanecen en la misma región geográfica que eligió para su almacén.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Si se replica entre dos centros de datos, ¿qué ocurre si el centro de datos principal experimenta una interrupción inesperada?
Puede desencadenar una conmutación por error no planeada desde el sitio secundario. Site Recovery no necesita conectividad desde el sitio principal para realizar la conmutación por error.

### <a name="is-failover-automatic"></a>¿La conmutación por error es automática?
La conmutación por error no es automática. Puede iniciar las conmutaciones por error con solo un clic en el portal o bien puede usar [Site Recovery PowerShell](/powershell/module/az.recoveryservices) para desencadenar una conmutación por error. La conmutación por recuperación también es una acción sencilla en el portal de Site Recovery.

Para automatizar estos procesos, puede utilizar Orchestrator u Operations Manager locales para detectar un error de la máquina virtual y desencadenar luego la conmutación por error mediante el SDK.

* [Obtenga más información](site-recovery-create-recovery-plans.md) sobre los planes de recuperación.
* [Más información](site-recovery-failover.md) acerca de la conmutación por error.
* [Más información](site-recovery-failback-azure-to-vmware.md) acerca de la conmutación por recuperación de servidores físicos y máquinas virtuales de VMware

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-fail-back-to-a-different-host"></a>Si mi host local no responde o se bloqueó, ¿conmutar por recuperación a un host diferente?
Sí, puede usar la recuperación en una ubicación alternativa para realizar la conmutación por recuperación a otro host de Azure.

* [Para máquinas virtuales de VMware](concepts-types-of-failback.md#alternate-location-recovery-alr)
* [Para máquinas virtuales de Hyper-V](hyper-v-azure-failback.md#perform-failback)

## <a name="automation"></a>Automation

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>¿Se pueden automatizar escenarios de Site Recovery con un SDK?
Sí. Puede automatizar los flujos de trabajo de Site Recovery mediante la API de Rest, PowerShell o el SDK de Azure. Escenarios admitidos actualmente para la implementación de Site Recovery con PowerShell:

* [Replicación de máquinas virtuales de Hyper-V en nubes VMM en Azure PowerShell Resource Manager](hyper-v-vmm-powershell-resource-manager.md)
* [Replicación de máquinas virtuales de Hyper-V sin VMM en Azure PowerShell Resource Manager](hyper-v-azure-powershell-resource-manager.md)
* [Replicación de máquinas virtuales de VMware en Azure con el Administrador de recursos de PowerShell](vmware-azure-disaster-recovery-powershell.md)

## <a name="componentprovider-upgrade"></a>Actualización de componente o proveedor

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-site-recovery-upgrades"></a>¿Dónde puedo encontrar los release notes/paquetes acumulativos de actualizaciones de Site Recovery

[Obtenga información sobre](site-recovery-whats-new.md) acerca de nuevas actualizaciones y [obtener información de consolidado](service-updates-how-to.md).

## <a name="next-steps"></a>Pasos siguientes
* Lea la [Información general sobre Site Recovery](site-recovery-overview.md)

