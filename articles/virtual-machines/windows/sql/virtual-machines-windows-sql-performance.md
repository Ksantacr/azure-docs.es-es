---
title: Directrices de rendimiento para SQL Server en Azure | Microsoft Docs
description: Ofrece directrices para optimizar el rendimiento de SQL Server en máquinas virtuales de Microsoft Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a0c85092-2113-4982-b73a-4e80160bac36
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 09/26/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: c1f40c62fce61ba16dfdf289d54cd19c3739ce21
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393768"
---
# <a name="performance-guidelines-for-sql-server-in-azure-virtual-machines"></a>Directrices de rendimiento para SQL Server en Azure Virtual Machines

## <a name="overview"></a>Información general

En este tema se ofrece una guía para optimizar el rendimiento de SQL Server en una máquina virtual de Microsoft Azure. Mientras se ejecuta SQL Server en Azure Virtual Machines, se recomienda seguir usando las mismas opciones de ajuste de rendimiento de base de datos que son aplicables a SQL Server en el entorno de servidor local. Sin embargo, el rendimiento de una base de datos relacional en una nube pública depende de muchos factores como el tamaño de una máquina virtual y la configuración de los discos de datos.

En [SQL Server images provisioned in the Azure portal](quickstart-sql-vm-create-portal.md) (Imágenes de SQL Server aprovisionadas en Azure Portal), siga los procedimientos recomendados para la configuración del almacenamiento general (para obtener más información sobre cómo se configura el almacenamiento, consulte [Configuración de almacenamiento para máquinas virtuales de SQL Server](virtual-machines-windows-sql-server-storage-configuration.md)). Después del aprovisionamiento, considere la posibilidad de aplicar otras de las optimizaciones que se describen en este artículo. Elija según la carga de trabajo y realice pruebas de verificación.

> [!TIP]
> Por lo general, existe un equilibrio entre la optimización de los costos y la optimización del rendimiento. Este artículo se centra en cómo obtener el *mejor* rendimiento de SQL Server en máquinas virtuales de Azure. Si su carga de trabajo no es menos exigente, podría no necesitar todas las optimizaciones enumeradas a continuación. Tenga en cuenta sus necesidades de rendimiento, costos y patrones de carga de trabajo a medida que evalúa estas recomendaciones.

## <a name="quick-check-list"></a>Lista de comprobación rápida

La siguiente es una lista de comprobación rápida para un rendimiento óptimo de SQL Server en Azure Virtual Machines:

| Área | Optimizaciones |
| --- | --- |
| [Tamaño de VM](#vm-size-guidance) | - [DS3_v2](../sizes-general.md), o superior, para la edición SQL Enterprise.<br/><br/> - [DS2_v2](../sizes-general.md), o superior, para las ediciones SQL Standard y Web. |
| [Storage](#storage-guidance) | - Use [unidades de estado sólido prémium](../disks-types.md). Solo se recomienda el almacenamiento estándar en fases de desarrollo o pruebas.<br/><br/> - Mantenga la [cuenta de almacenamiento](../../../storage/common/storage-create-storage-account.md) y la máquina virtual con SQL Server en la misma región.<br/><br/> * Deshabilite el [almacenamiento con redundancia geográfica](../../../storage/common/storage-redundancy.md) (replicación geográfica) de Azure en la cuenta de almacenamiento. |
| [Discos](#disks-guidance) | - Use un mínimo de dos [discos P30](../disks-types.md#premium-ssd) (uno para archivos de registro y otro para archivos de datos, lo que incluye TempDB). Para cargas de trabajo que requieran aproximadamente 50.000 E/S por segundo, considere la posibilidad de usar un SSD Ultra. <br/><br/> - Evite el uso del sistema operativo o de discos temporales para el registro o almacenamiento de bases de datos.<br/><br/> - Habilite el almacenamiento en caché de lectura en los discos que hospedan los archivos de datos y los archivos de datos de TempDB.<br/><br/> - No habilite el almacenamiento en caché en los discos que hospedan el archivo de registro.  **Importante**: Detenga el servicio de SQL Server cuando se cambie la configuración de caché para un disco de VM de Azure.<br/><br/> - Seccione varios discos de datos de Azure para obtener un mayor rendimiento de E/S.<br/><br/> - Formatee con tamaños de asignación documentados. <br/><br/> - Coloque TempDB en un disco SSD local para cargas de trabajo de SQL Server críticas (después de elegir el tamaño de máquina virtual correcto). |
| [E/S](#io-guidance) |- Habilite la compresión de páginas de bases de datos.<br/><br/> - Habilite la inicialización instantánea de archivos para archivos de datos.<br/><br/> - Limite el crecimiento automático de la base de datos.<br/><br/> - Deshabilite la reducción automática de la base de datos.<br/><br/> - Mueva todas las bases de datos a discos de datos, incluidas las bases de datos del sistema.<br/><br/> - Mueva los directorios de archivos de seguimiento y registros de errores de SQL Server a discos de datos.<br/><br/> - Configure ubicaciones predeterminadas para los archivos de base de datos y de copia de seguridad.<br/><br/> - Habilite páginas bloqueadas.<br/><br/> - Aplique correcciones de rendimiento de SQL Server. |
| [Características específicas](#feature-specific-guidance) | - Realice copias de seguridad directamente en el almacenamiento de blobs. |

Para más información sobre *cómo* y *por qué* llevar a cabo estas optimizaciones, repase los detalles y las instrucciones proporcionadas en las siguientes secciones.

## <a name="vm-size-guidance"></a>Orientación sobre el tamaño de máquina virtual

En aplicaciones sensibles al rendimiento, se recomienda usar los siguientes [tamaños de máquinas virtuales](../sizes.md):

* **SQL Server Enterprise Edition**: DS3_v2 o superior
* **Ediciones de SQL Server Standard y Web**: DS2_v2 o superior

[Serie DSv2](../sizes-general.md#dsv2-series): las máquinas virtuales admiten el almacenamiento Premium, lo que se recomienda para optimizar el rendimiento. Los tamaños recomendados aquí son líneas de base, pero el tamaño real de la máquina que seleccione depende de las demandas de carga de trabajo. Las máquinas virtuales de la serie DSv2 son máquinas virtuales de uso general adecuadas para una gran variedad de cargas de trabajo, mientras que otros tamaños de máquinas están optimizados para tipos específicos de carga de trabajo. Por ejemplo, la [serie M](../sizes-memory.md#m-series) ofrece la mayor cantidad de vCPU y memoria para las mayores cargas de trabajo de SQL Server. Las [series GS](../sizes-memory.md#gs-series) y [DSv2 series 11-15](../sizes-memory.md#dsv2-series-11-15) están optimizadas para grandes requisitos de memoria. Ambas series también están disponibles en [ tamaños de núcleo limitados](../../windows/constrained-vcpu.md), lo que ahorra dinero para cargas de trabajo con menores exigencias de proceso. Las máquinas de la [serie Ls](../sizes-storage.md) están optimizadas para un alto rendimiento de disco y E/S. Es importante considerar la carga de trabajo específica de SQL Server y aplicarla a la selección de una serie y tamaño de máquina virtual.

## <a name="storage-guidance"></a>Orientación sobre el almacenamiento

Las máquinas virtuales de la serie DS (además de las series DSv2 y GS) admiten [unidades de estado sólido prémium](../disks-types.md). Se recomiendan unidades de estado sólido prémium para todas las cargas de trabajo de producción.

> [!WARNING]
> Los HDD estándar y las unidades de estado sólido (SSD) presentan ancho de banda y latencias variables, de modo que solo se recomiendan para cargas de trabajo de desarrollo o de prueba. En las cargas de trabajo de producción conviene usar SSD prémium.

Además, se recomienda crear su cuenta de almacenamiento de Azure en el mismo centro de datos que sus máquinas virtuales de SQL Server para reducir los retrasos en la transferencia. Al crear una cuenta de almacenamiento, deshabilite la replicación geográfica ya que no se garantiza el orden de escritura coherente entre varios discos. En su lugar, considere la posibilidad de configurar una tecnología de recuperación ante desastres de SQL Server entre dos centros de datos de Azure. Para obtener más información, vea [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Orientación sobre los discos

En una máquina virtual de Azure hay tres tipos de disco principales:

* **Disco del sistema operativo**: al crear una máquina virtual de Azure, la plataforma conectará al menos un disco (etiquetado como unidad **C**) a la máquina virtual como disco del sistema operativo. El disco es un VHD almacenado como blob en páginas en almacenamiento.
* **Disco temporal**: las máquinas virtuales de Azure contienen otro disco denominado disco temporal (etiquetado como unidad **D**:). Se trata de un disco en el nodo que se puede usar para el espacio de desecho.
* **Discos de datos**: También puede conectar discos adicionales a la máquina virtual como discos de datos y estos se almacenarán en el almacenamiento como blobs en páginas.

En las siguientes secciones se incluyen recomendaciones sobre cómo usar cada uno de estos discos.

### <a name="operating-system-disk"></a>Disco del sistema operativo

Un disco del sistema operativo es un VHD que se puede arrancar y montar como una versión en ejecución de un sistema operativo y está etiquetado como la unidad **C** .

La directiva del almacenamiento en caché predeterminada en el disco del sistema operativo es **Lectura/escritura**. Para aplicaciones sensibles al rendimiento, se recomienda usar discos de datos en lugar del disco del sistema operativo. Vea la sección sobre Discos de datos a continuación.

### <a name="temporary-disk"></a>Disco temporal

La unidad de almacenamiento temporal, etiquetada como la unidad **D**:, no se conserva en el almacenamiento de blobs de Azure. No almacene archivos de base de datos de usuarios ni archivos de registro de transacciones de usuarios en la unidad **D:** .

En máquinas virtuales de las series D, Dv2 y G, la unidad temporal está ubicada en discos SSD. Si la carga de trabajo implica un uso intensivo de TempDB (por ejemplo, con objetos temporales o combinaciones complejas), el almacenamiento de TempDB en la unidad **D** podría dar lugar a un mayor rendimiento de TempDB y la reducción de su latencia. Para ver un escenario de ejemplo, consulte la explicación de TempDB en la siguiente entrada de blog: [Storage Configuration Guidelines for SQL Server on Azure VM](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm) (Instrucciones de configuración del almacenamiento para SQL Server en máquinas virtuales de Azure).

En las máquinas virtuales que admiten SSD prémium (series DS, DSv2 y GS), se recomienda almacenar TempDB en un disco que admita SSD prémium y que tenga habilitado el almacenamiento en caché de lectura.

Esta recomendación tiene una sola excepción: _si se realiza un uso de TempDB intensivo en cuanto a escritura, podrá lograr un mayor rendimiento si lo almacena en la unidad **D** local, que en estos tamaños de equipos también es un disco SSD._

### <a name="data-disks"></a>Discos de datos.

* **Uso de discos de datos para archivos de datos y de registro.** : si no usa seccionamiento de discos, utilice dos discos P30 de SSD prémium, uno para los archivos de registro y otro para los archivos de datos y TempDB. Cada SSD proporciona un número de IOPS y ancho de banda (MB/s) según su tamaño, como se describe en el artículo sobre la [Selección del tipo de disco](../disks-types.md). Si usa una técnica de fragmentación de discos, como los espacios de almacenamiento, puede lograr un rendimiento óptimo si tiene dos grupos: uno para los archivos de registro y otro para los archivos de datos. Sin embargo, si planea usar instancias de clúster de conmutación por error de SQL Server (FCI), debe configurar un grupo.

   > [!TIP]
   > - Para obtener resultados de la pruebas con varias configuraciones de discos y cargas de trabajo, consulte la siguiente entrada de blog: [Storage Configuration Guidelines for SQL Server on Azure VM](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm/) (Instrucciones de configuración del almacenamiento para SQL Server en máquinas virtuales de Azure).
   > - Para obtener un rendimiento crítico en servidores con SQL Server que necesitan aproximadamente 50.000 E/S por segundo, considere la posibilidad de reemplazar 10 - discos P30 por uno SSD Ultra. Para más información, consulte la siguiente entrada de blog: [Mission critical performance with Ultra SSD](https://azure.microsoft.com/blog/mission-critical-performance-with-ultra-ssd-for-sql-server-on-azure-vm/) (Rendimiento crítico con SSD Ultra).

   > [!NOTE]
   > Al aprovisionar una máquina virtual de SQL Server en el portal, tiene la opción de modificar la configuración de almacenamiento. Según la configuración, Azure configura uno o varios discos. Varios discos se combinan en un único grupo de almacenamiento con seccionamiento. Los archivos de datos y de registro residen juntos en esta configuración. Para más información, consulte [Configuración del almacenamiento para máquinas virtuales de SQL Server](virtual-machines-windows-sql-server-storage-configuration.md).

* **Seccionamiento del disco**: para disfrutar de un mayor rendimiento, puede agregar más discos de datos y usar el seccionamiento de discos. Para determinar el número de discos de datos, debe analizar el número de IOPS y ancho de banda necesario para los archivos de registro, así como para los datos y los archivos TempDB. Observe que diferentes tamaños de máquinas virtuales tienen distintos límites en el número de IOPS y ancho de banda admitidos. Vea las tablas de IOPS por [tamaño de máquina virtual](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Use estas directrices:

  * Para Windows 8/Windows Server 2012 o posterior, use [espacios de almacenamiento](https://technet.microsoft.com/library/hh831739.aspx) con las siguientes directrices:

      1. Configure la intercalación (tamaño de franja) en 64 KB (65536 bytes) para cargas de trabajo de OLTP y 256 KB (262144 bytes) para cargas de trabajo de almacenamiento de datos para evitar que el rendimiento se vea afectado debido a la falta de alineación de las particiones. Esta característica debe establecerse con PowerShell.
      2. Establezca recuento de columnas = número de discos físicos. Use PowerShell (no la interfaz de usuario del Administrador del servidor) al configurar más de 8 discos. 

    Por ejemplo, aquí PowerShell crea un nuevo grupo de almacenamiento con el tamaño de intercalación de 64 KB y el número de columnas de 2:

    ```powershell
    $PoolCount = Get-PhysicalDisk -CanPool $True
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * Para Windows 2008 R2 o versiones anteriores, puede usar discos dinámicos (volúmenes seccionados del SO) y el tamaño de la franja siempre es 64 KB. Tenga en cuenta que esta opción es obsoleta a partir de Windows 8/Windows Server 2012. Para obtener información, vea la declaración de soporte técnico en [Servicio de disco virtual está realizando la transición a la API de administración de almacenamiento de Windows](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

  * Si usa [Espacios de almacenamiento directo (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) con [instancias del clúster de conmutación por error de SQL Server](virtual-machines-windows-portal-sql-create-failover-cluster.md), debe configurar un solo grupo. Tenga en cuenta que aunque se pueden crear diferentes volúmenes en ese único grupo, todos compartirán las mismas características, por ejemplo, la misma directiva de almacenamiento en caché.

  * Determine el número de discos asociados al grupo de almacenamiento en función de sus expectativas de carga. Tenga en cuenta que diferentes tamaños de máquina virtual permiten diferentes números de discos de datos conectados. Para obtener más información, consulte [Tamaños de máquinas virtuales](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Si no usa SSD prémium (escenarios de desarrollo y pruebas), se recomienda agregar el número máximo de discos de datos admitidos por el [tamaño de la máquina virtual](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) y usar el seccionamiento de discos.

* **Directiva de caché**: tenga en cuenta las siguientes recomendaciones para la directiva según la configuración del almacenamiento en caché.

  * Si usa discos independientes para los archivos de datos y de registro, habilite el almacenamiento en caché de lectura en los discos de datos que hospeden los archivos de datos y de datos de TempDB. Esto puede suponer una mejora del rendimiento significativa. No habilite el almacenamiento en caché en el disco que contiene el archivo de registro, ya que esto produce una pequeña disminución del rendimiento.

  * Si usa la fragmentación de discos en un solo grupo de almacenamiento, la mayoría de las cargas de trabajo se beneficiarán de la memoria caché de lectura. Si tiene grupos de almacenamiento independientes para los archivos de registro y de datos, habilite el almacenamiento en caché de lectura solo en el grupo de almacenamiento de los archivos de datos. Para ciertas cargas de trabajo de escritura intensivas, podría lograr un mejor rendimiento sin almacenamiento en caché. Esto solo se puede determinar mediante pruebas.

  * Las recomendaciones anteriores se aplican a los SSD prémium. Si no usa SSD prémium, no habilite el almacenamiento en caché en los discos de datos.

  * Para obtener instrucciones sobre la configuración del almacenamiento en caché de disco, consulte los siguientes artículos. Para el modelo de implementación clásico (ASM), consulte: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) y [Set-AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx). Para el modelo de implementación de Azure Resource Manager, consulte: [Set-AzOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk) y [Set-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdatadisk).

     > [!WARNING]
     > Detenga el servicio SQL Server al cambiar la configuración de la caché de los discos de máquina virtual de Azure para evitar la posibilidad de que se produzcan daños en la base de datos.

* **Tamaño de la unidad de asignación de NTFS**: al formatear el disco de datos, se recomienda usar un tamaño de unidad de asignación de 64 KB para los archivos de datos y de registro, además de TempDB.

* **Procedimientos recomendados de administración de discos**: al quitar un disco de datos o cambiar su tipo de caché, detenga el servicio SQL Server durante el cambio. Cuando se modifica la configuración de almacenamiento en caché en el disco del sistema operativo, Azure detiene la máquina virtual, cambia el tipo de caché y reinicia la máquina virtual. Cuando se modifica la configuración de la caché de un disco de datos, no se detiene la máquina virtual, pero el disco de datos se desconecta de la máquina virtual durante el cambio y, posteriormente, se vuelve a conectar.

  > [!WARNING]
  > De no detenerse el servicio SQL Server durante estas operaciones, podrían producirse daños en la base de datos.


## <a name="io-guidance"></a>Orientación sobre E/S

* Los mejores resultados con las unidades de estado sólido prémium se logran al establecer paralelismos entre la aplicación y las solicitudes. Las SSD prémium están diseñadas para escenarios donde la profundidad de la cola de E/S es mayor que 1, por lo que verá poco o ningún aumento del rendimiento para las solicitudes de serie de subproceso único (incluso si son intensivas de almacenamiento). Por ejemplo, esto podría afectar a los resultados de pruebas de subproceso único de herramientas de análisis de rendimiento, como SQLIO.

* Recuerde que también puede usar la [compresión de páginas de bases de datos](https://msdn.microsoft.com/library/cc280449.aspx) , ya que puede ayudarle a mejorar el rendimiento de las cargas de trabajo intensivas de E/S. Sin embargo, la compresión de datos puede aumentar el consumo de la CPU en el servidor de bases de datos.

* Considere la posibilidad de habilitar la inicialización instantánea de archivos para reducir el tiempo necesario para la asignación inicial de archivos. Para aprovechar la inicialización instantánea de archivos, se concede la cuenta de servicio de SQL Server (MSSQLSERVER) con SE_MANAGE_VOLUME_NAME y se agrega a la directiva de seguridad **Realizar tareas de mantenimiento del volumen**. Si usa una imagen de la plataforma de SQL Server para Azure, la cuenta de servicio predeterminada (NT Service\MSSQLSERVER) no se agrega a la directiva de seguridad **Realizar tareas de mantenimiento del volumen**. En otras palabras, la inicialización instantánea de archivos no se habilita en una imagen de plataforma de SQL Server Azure. Después de agregar la cuenta de servicio de SQL Server a la directiva de seguridad **Realizar tareas de mantenimiento del volumen** , reinicie el servicio de SQL Server. Puede haber consideraciones de seguridad para usar esta característica. Para obtener más información, consulte [Inicialización de archivos de base de datos](https://msdn.microsoft.com/library/ms175935.aspx).

* **crecimiento automático** se considera apenas una contingencia para el crecimiento inesperado. No administre su crecimiento de datos y registros a diario con el crecimiento automático. Si se usa el crecimiento automático, haga crecer previamente el archivo mediante el modificador Tamaño.

* Asegúrese de que la **reducción automática** está deshabilitada para evitar una sobrecarga innecesaria que puede afectar negativamente al rendimiento.

* Mueva todas las bases de datos a discos de datos, incluidas bases de datos del sistema. Para más información, consulte [Mover bases de datos del sistema](https://msdn.microsoft.com/library/ms345408.aspx).

* Mueva los directorios de archivos de seguimiento y registros de errores de SQL Server a discos de datos. Para llevar esto a cabo en el Administrador de configuración de SQL Server, haga clic con el botón secundario en la instancia de SQL Server y seleccione Propiedades. La configuración de los archivos de registro y de seguimiento de errores se puede cambiar en la pestaña **Parámetros de inicio** . El directorio de volcado se especifica en la pestaña **Opciones avanzadas** . En la siguiente captura de pantalla se muestra dónde buscar el parámetro de inicio del registro de errores.

    ![Captura de pantalla de registro de errores de SQL](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

* Configure ubicaciones predeterminadas para los archivos de base de datos y de copia de seguridad. Use las recomendaciones de este artículo y realice los cambios necesarios en la ventana Propiedades del servidor. Para obtener instrucciones, consulte [Ver o cambiar las ubicaciones predeterminadas de los archivos de datos y registro (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). En la siguiente captura de pantalla se muestra dónde realizar estos cambios.

    ![Archivos de copia de seguridad y de registro de datos de SQL](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)
* Habilite páginas bloqueadas para reducir la E/S y las actividades de paginación. Para más información, consulte [Habilitar la opción Bloquear páginas en la memoria (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

* Si está ejecutando SQL Server 2012, instale la actualización acumulativa 10 del Service Pack 1. Esta actualización contiene la corrección para el rendimiento deficiente en la E/S al ejecutar select en la instrucción de tabla temporal en SQL Server 2012. Para obtener información, consulte este [artículo de Knowledge Base](https://support.microsoft.com/kb/2958012).

* Considere la posibilidad de comprimir los archivos de datos cuando se transfieren dentro y fuera de Azure.

## <a name="feature-specific-guidance"></a>Guía específica de la característica

Algunas implementaciones pueden lograr ventajas de rendimiento adicionales mediante técnicas de configuración más avanzadas. En la siguiente lista se destacan algunas características de SQL Server que pueden ayudarle a lograr un mejor rendimiento:

### <a name="backup-to-azure-storage"></a>Copia de seguridad en almacenamiento de Azure
al realizar copias de seguridad para SQL Server que se ejecutan en máquinas virtuales de Azure, puede usar una [Copia de seguridad en URL de SQL Server](https://msdn.microsoft.com/library/dn435916.aspx). Esta característica está disponible a partir de SQL Server 2012 SP1 CU2 y se recomienda para las copias de seguridad en los discos de datos conectados. Al realizar copias de seguridad o restaurar en o desde el almacenamiento de Azure, siga las recomendaciones que se ofrecen en [Solución de problemas y prácticas recomendadas de copia de seguridad de SQL Server en URL y restauración a partir de copias de seguridad almacenadas en Azure Storage](https://msdn.microsoft.com/library/jj919149.aspx). También puede automatizar estas copias de seguridad con [Automated Backup para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

Antes de SQL Server 2012, puede usar la [Herramienta de copia de seguridad de SQL Server a Azure](https://www.microsoft.com/download/details.aspx?id=40740). Esta herramienta puede ayudar a aumentar el rendimiento de la copia de seguridad con varios destinos de franjas de copia de seguridad.

### <a name="sql-server-data-files-in-azure"></a>Archivos de datos de SQL Server en Azure

esta nueva característica ([Archivos de datos de SQL Server en Azure](https://msdn.microsoft.com/library/dn385720.aspx)) está disponible a partir de SQL Server 2014. La ejecución de SQL Server con archivos de datos en Azure muestra características de rendimiento comparables con el uso de discos de datos de Azure.

### <a name="failover-cluster-instance-and-storage-spaces"></a>Instancia de clúster de conmutación por error y espacios de almacenamiento

Si usa espacios de almacenamiento, al agregar nodos al clúster en el **confirmación** página, desactive la casilla **agregar todo el almacenamiento apto al clúster**. 

![Desactive el almacenamiento apto](media/virtual-machines-windows-sql-performance/uncheck-eligible-cluster-storage.png)

Si utiliza espacios de almacenamiento y no desactiva la casilla **Add all eligible storage to the cluster** (Agregar todo el almacenamiento apto al clúster), Windows separa los discos virtuales durante el proceso de agrupación en clústeres. Como resultado, no aparecen en el Administrador de discos ni el Explorador hasta que se quiten los espacios de almacenamiento del clúster y se vuelvan a asociar mediante PowerShell. Espacios de almacenamiento agrupa varios discos en grupos de almacenamiento. Para obtener más información, consulte el artículo sobre [espacios de almacenamiento](/windows-server/storage/storage-spaces/overview).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el almacenamiento y el rendimiento, consulte [Storage Configuration Guidelines for SQL Server on Azure VM](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm/) (Instrucciones de configuración de almacenamiento para SQL Server en máquinas virtuales de Azure).

Para ver prácticas recomendadas de seguridad, vea [Consideraciones de seguridad para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-security.md).

Revise otros artículos sobre la máquina virtual de SQL Server en [Introducción a SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md). Si tiene alguna pregunta sobre las máquinas virtuales de SQL Server, consulte las [Preguntas más frecuentes](virtual-machines-windows-sql-server-iaas-faq.md).
