---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/16/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 87fca23cab27ec27bfc9799066c126994167f46e
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391328"
---
Los tamaños de VM optimizadas para memoria ofrecen una relación alta de memoria a CPU que es excelente para servidores de bases de datos relacionales, memorias caché de medianas a grandes y análisis en memoria. En este artículo, se proporciona información acerca del número de vCPU, discos de datos y tarjetas de interfaz de red, así como del rendimiento del almacenamiento y del ancho de banda de red para cada tamaño de esta agrupación. 

* La serie Mv2 ofrece el mayor número de vCPU (hasta 208 vCPU) y la memoria más grande (hasta 5.7 TiB) de todas las máquinas virtuales en la nube. Es ideal para bases de datos extremadamente grandes u otras aplicaciones que se benefician de un elevado número de vCPU y grandes cantidades de memoria.
 
* La serie M ofrece un recuento alto de vCPU (hasta 128 vCPU) y una gran cantidad de memoria (hasta 3,8 TiB). También es idóneo para las bases de datos extremadamente grandes u otras aplicaciones que se benefician de elevado número de vCPU y grandes cantidades de memoria.

* Serie Dv2, serie G y DSv2 o GS son ideales para aplicaciones que requieren vCPU más rápidas, mejor rendimiento del almacenamiento temporal o tienen mayor demanda de memoria. Ofrecen una combinación eficaz para muchas aplicaciones de clase empresarial.

* Serie de Dv2, una evolución de la serie D original, presenta una CPU más eficaz. La CPU de la serie Dv2 es un 35 % aproximadamente más rápida que la CPU de la serie D. Se basa en la última generación v3 de 2,4 GHz Intel Xeon® E5-2673 2,4 GHz (Haswell) o procesadores de E5-2673 v4 (Broadwell) de 2,3 GHz; y con Intel Turbo Boost Technology 2.0, puede alcanzar los 3,1 GHz. La serie Dv2 tiene las mismas configuraciones de disco y memoria que la serie D.

* Las serie Ev3 tiene el mismo procesador E5-2673 v4 (Broadwell) de 2,3 GHz en una configuración de hyperthreading. Gracia a esto, proporciona una mejor propuesta de valor para la mayoría de las cargas de trabajo de uso general y la equipara con las máquinas virtuales de propósito general de la mayoría de las demás tecnologías de nube.  Se ha ampliado la memoria (de 7 GiB/vCPU a 8 GiB/vCPU), y los límites de disco y red se han ajustado por núcleo para equipararse con el cambio a hyperthreading.  La serie Ev3 es la continuación de los tamaños de máquina virtual de memoria alta de las familias D/Dv2.

* Azure Compute ofrece tamaños de máquinas virtuales que están aislados para un tipo concreto de hardware y dedicados a un solo cliente.  Estos tamaños de máquina virtual son más adecuados para cargas de trabajo que requieren un alto grado de aislamiento de otros clientes como, por ejemplo, las cargas de trabajo que incluyen elementos como el cumplimiento normativo y los requisitos legales.  Los clientes también puede elegir subdividir aún más los recursos de estas máquinas virtuales aisladas mediante la [compatibilidad de Azure para máquinas virtuales anidadas](https://azure.microsoft.com/blog/nested-virtualization-in-azure/).  Consulte las tablas de familias de máquinas virtuales que aparecen a continuación para ver las opciones de las máquinas virtuales aisladas.

## <a name="esv3-series"></a>Serie Esv3 

ACU: 160-190 <sup>1</sup>

Premium Storage:  Compatible

Caching de Premium Storage:  Compatible

Las instancias de la serie ESv3 se basan en el procesador Intel XEON® E5-2673 v4 (Broadwell) de 2,3 GHz y pueden llegar a 3,5 GHz con la tecnología Intel Turbo Boost Technology 2.0 y el uso de Premium Storage. Las instancias de la serie Ev3 son ideales para aplicaciones empresariales de uso intensivo de memoria.


| Tamaño             | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3 | 2      | 16          | 32             | 4              | 4000 / 32 (50)                                                       | 3200 / 48                                | 2 / 1000                                   |
| Standard_E4s_v3&nbsp;<sup>2</sup> | 4      | 32          | 64             | 8              | 8000 / 64 (100)                                                      | 6400 / 96                                | 2 / 2000                                   |
| Standard_E8s_v3&nbsp;<sup>2</sup> | 8      | 64          | 128            | 16             | 16000 / 128 (200)                                                    | 12800 / 192                              | 4 / 4000                                       |
| Standard_E16s_v3&nbsp;<sup>2</sup> | 16     | 128         | 256            | 32             | 32000 / 256 (400)                                                    | 25600 / 384                              | 8 / 8000                                       |
| Standard_E20s_v3                   | 20     | 160         | 320            | 32             | 40000 / 320 (400)                                                    | 32000 / 480                              | 8 / 10 000                                       |
| Standard_E32s_v3&nbsp;<sup>2</sup> | 32     | 256         | 512            | 32             | 64000 / 512 (800)                                                    | 51200 / 768                              | 8 / 16000                             |
| Standard_E64s_v3&nbsp;<sup>2</sup> | 64     | 432         | 864            | 32             | 128000 / 1024 (1600)                                                   | 80000 / 1200                             | 8 / 30 000                             |
| Standard_E64is_v3&nbsp;<sup>3</sup> | 64     | 432         | 864            | 32             | 128000 / 1024 (1600)                                                   | 80000 / 1200                             | 8 / 30 000                             |


<sup>1</sup> Las máquinas virtuales de la serie Esv3 cuentan con la tecnología Hyper-Threading de Intel®.

<sup>2</sup> Tamaños de núcleos restringidos disponibles.

<sup>3</sup> La instancia está aislada en el hardware dedicado a un solo cliente.


## <a name="ev3-series"></a>Serie Ev3 

ACU: 160 - 190 <sup>1</sup>

Premium Storage:  No compatible

Caching de Premium Storage:  No compatible

Las instancias de la serie Ev3 se basan en el procesador Intel XEON ® E5-2673 v4 (Broadwell) de 2,3 GHz y pueden llegar a 3,5 GHz con la tecnología Intel Turbo Boost Technology 2.0. Las instancias de la serie Ev3 son ideales para aplicaciones empresariales de uso intensivo de memoria.

El almacenamiento en disco de datos se factura de forma independiente a las máquinas virtuales. Para usar discos de Premium Storage, utilice los tamaños ESv3. Los precios y los medidores de facturación para los tamaños ESv3 son los mismos que para la serie Ev3. 


| Tamaño            | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal: IOPS / MBps de lectura / MBps de escritura | Ancho de banda de red/NIC máx. |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8000                     |
| Standard_E20_v3 | 20        | 160         | 500            | 32             | 30000/469/234                                            | 8 / 10 000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30 000           |
| Standard_E64i_v3&nbsp;<sup>2,&nbsp;3</sup> | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30 000           |

<sup>1</sup> Las máquinas virtuales de la serie Ev3 cuentan con la tecnología Hyper-Threading de Intel®.

<sup>2</sup> Tamaños de núcleos restringidos disponibles.

<sup>3</sup> La instancia está aislada en el hardware dedicado a un solo cliente.


## <a name="mv2-series"></a>Serie Mv2

Premium Storage: Compatible

Caching de Premium Storage: Compatible

Acelerador de escritura: [Compatible](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

La serie Mv2 características un alto rendimiento, baja latencia, puede asignar almacenamiento local de NVMe, que se ejecutan en hyper-threading Intel® Xeon® Platinum 8180 M 2,5 GHz (Skylake de) procesador con una frecuencia de base de core todas de 2,5 GHz y una frecuencia turbo máxima de 3,8 GHz directamente. Todos los tamaños de máquina virtual de serie Mv2 pueden usar discos persistentes estándares y premium. Instancias de la serie Mv2 son tamaños de máquina virtual que proporciona un rendimiento informático sin parangón para admitir cargas de trabajo con una alta proporción de memoria a CPU que es ideal para servidores de bases de datos relacionales, memorias caché grandes y en memoria y las bases de datos de gran tamaño en memoria optimizada para memoria análisis. 

|Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M208ms_v2<sup>1, 2</sup> | 208 | 5700 | 4096 | 64 | 80000 / 800 (7040) | 40000 / 1000 | 8 / 16000 |
| Standard_M208s_v2<sup>1, 2</sup> | 208 | 2850 | 4096 | 64 | 80000 / 800 (7040) | 40000 / 1000 | 8 / 16000 |

Serie Mv2 cuentan con la tecnología Intel® Hyper-Threading  

<sup>1</sup> estas máquinas virtuales grandes requieren uno de estos sistemas operativos invitados compatibles: Windows Server 2016, Windows Server 2019, SLES 12 SP4, SLES 15.

<sup>2</sup> Mv2 serie las máquinas virtuales son la generación 2. Si usa Linux, consulte la sección siguiente para saber cómo buscar y seleccionar una imagen de SUSE Linux.

#### <a name="find-a-suse-image"></a>Buscar una imagen SUSE

Para seleccionar una imagen de SUSE Linux adecuada en el portal de Azure: 

1. En el portal de Azure, seleccione **crear un recurso** 
1. Busque "SUSE SAP" 
1. SLES para imágenes de generación 2 de SAP están disponibles como cualquier pago por uso o traiga su propia suscripción (BYOS). En los resultados de búsqueda, expanda la categoría de la imagen deseada:

    * SUSE Linux Enterprise Server (SLES) para SAP
    * SUSE Linux Enterprise Server (SLES) para SAP (BYOS)
    
1. Imágenes SUSE compatibles con la serie Mv2 tienen el prefijo con el nombre `GEN2:`. Las siguientes imágenes SUSE están disponibles para máquinas virtuales de serie Mv2:

    * GEN2: SUSE Linux Enterprise Server (SLES) 12 Service Pack 4 para las aplicaciones de SAP
    * GEN2: SUSE Linux Enterprise Server (SLES) 15 para aplicaciones de SAP
    * GEN2: SUSE Linux Enterprise Server (SLES) 12 Service Pack 4 para las aplicaciones de SAP (BYOS)
    * GEN2: SUSE Linux Enterprise Server (SLES) 15 para aplicaciones de SAP (BYOS)

#### <a name="select-a-suse-image-via-azure-cli"></a>Seleccione una imagen SUSE mediante la CLI de Azure

Para ver una lista el SLES disponibles actualmente para la imagen SAP para máquinas virtuales de serie Mv2, use el siguiente [ `az vm image list` ](https://docs.microsoft.com/cli/azure/vm/image?view=azure-cli-latest#az-vm-image-list) comando:

```azurecli
az vm image list --output table --publisher SUSE --sku gen2 --all
```

El comando da como resultado las VM de 2 generación disponibles actualmente disponibles de SUSE para máquinas virtuales de serie Mv2. 

Salida de ejemplo:

```
Offer          Publisher  Sku          Urn                                        Version
-------------  ---------  -----------  -----------------------------------------  ----------
SLES-SAP       SUSE       gen2-12-sp4  SUSE:SLES-SAP:gen2-12-sp4:2019.05.13       2019.05.13
SLES-SAP       SUSE       gen2-15      SUSE:SLES-SAP:gen2-15:2019.05.13           2019.05.13
SLES-SAP-BYOS  SUSE       gen2-12-sp4  SUSE:SLES-SAP-BYOS:gen2-12-sp4:2019.05.13  2019.05.13
SLES-SAP-BYOS  SUSE       gen2-15      SUSE:SLES-SAP-BYOS:gen2-15:2019.05.13      2019.05.13
```

## <a name="m-series"></a>Serie M 

ACU: 160-180 <sup>1</sup>

Premium Storage:  Compatible

Caching de Premium Storage:  Compatible

Acelerador de escritura:  [Compatible](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

| Tamaño            | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M8ms&nbsp;<sup>3</sup>    | 8  | 218,75 | 256  | 8  | 10000 / 100 (793)  | 5000  / 125 | 4 / 2000 |
| Standard_M16ms&nbsp;<sup>3</sup>   | 16 | 437,5  | 512  | 16 | 20000 / 200 (1587) | 10000 / 250 | 8 / 4000 |
| Standard_M32ts | 32 | 192    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M32ls | 32 | 256    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M32ms&nbsp;<sup>3</sup>   | 32 | 875    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M64s  | 64 | 1024   | 2048 | 64 | 80000 / 800 (6348)| 40000 / 1000 | 8 / 16000          |
| Standard_M64ls  | 64 | 512    | 2048 | 64 | 80000 / 800 (6348) | 40000 / 1000 | 8 / 16000 |
| Standard_M64ms&nbsp;<sup>3</sup>  | 64   | 1792 | 2048 | 64 | 80000 / 800 (6348)| 40000 / 1000 | 8 / 16000          |
| Standard_M128s&nbsp;<sup>2</sup> | 128  | 2048        | 4096  | 64 | 160000 / 1600 (12696) | 80000 / 2000                            | 8 / 30 000          |
| Standard_M128ms&nbsp;<sup>2,&nbsp;3,&nbsp;4</sup> | 128  | 3892  | 4096 | 64 | 160000 / 1600 (12696) | 80000 / 2000                            | 8 / 30 000          |
| Standard_M64   | 64  | 1024 | 7168  | 64 | 80000  / 800  (1228) | 40000 / 1000 | 8 / 16000 |
| Standard_M64m  | 64  | 1792 | 7168  | 64 | 80000  / 800  (1228) | 40000 / 1000 | 8 / 16000 |
| Standard_M128&nbsp;<sup>2  | 128 | 2048 | 14336 | 64 | 250000 / 1600 (2456) | 80000 / 2000 | 8 / 32 000 |
| Standard_M128m&nbsp;<sup>2 | 128 | 3892 | 14336 | 64 | 250000 / 1600 (2456) | 80000 / 2000 | 8 / 32 000 |



<sup>1</sup> Las VM de la serie M cuentan con la tecnología Hyper-Threading de Intel®

<sup>2</sup> Si hay más de 64 vCPU, se necesita uno de estos sistemas operativos invitados compatibles: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 y Red Hat Enterprise Linux, CentOS 7.3 u Oracle Linux 7.3 con LIS 4.2.1.

<sup>3</sup> Tamaños de núcleos restringidos disponibles.

<sup>4</sup> La instancia está aislada en el hardware dedicado a un solo cliente.
<br>

## <a name="gs-series"></a>Serie GS 

ACU: 180 - 240 <sup>1</sup>

Premium Storage:  Compatible

Caching de Premium Storage:  Compatible

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_GS1 |2 |28 |56 |8 |10000 / 100 (264) |5000 / 125 |2 / 2000 |
| Standard_GS2 |4 |56 |112 |16 |20000 / 200 (528) |10000 / 250 |2 / 4000 |
| Standard_GS3 |8 |112 |224 |32 |40000 / 400 (1056) |20000 / 500 |4 / 8000 |
| Standard_GS4&nbsp;<sup>3</sup> |16 |224 |448 |64 |80000 / 800 (2112) |40000 / 1000 |8 / 16000 |
| Standard_GS5&nbsp;<sup>2,&nbsp;3</sup> |32 |448 |896 |64 |160000 / 1600 (4224) |80000 / 2000 |8 / 20000 |

<sup>1</sup> El rendimiento de disco máx. (E/S por segundo o Mbps) posible con una VM de la serie GS puede estar limitado por el número, el tamaño y la fragmentación de los discos conectados. Para información detallada, consulte [Azure Premium Storage: Diseño de alto rendimiento](../articles/virtual-machines/windows/premium-storage-performance.md).

<sup>2</sup> La instancia está aislada en el hardware dedicado a un solo cliente.

<sup>3</sup> Tamaños de núcleos restringidos disponibles.

<br>

## <a name="g-series"></a>Serie G

ACU: 180 - 240

Premium Storage:  No compatible

Caching de Premium Storage:  No compatible

| Tamaño         | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Rendimiento máximo de almacenamiento temporal: IOPS / MBps de lectura / MBps de escritura | Discos de datos máx. / rendimiento: E/S | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000 / 93 / 46                                           | 8 / 8 x 500                       | 2 / 2000                     |
| Standard_G2  | 4         | 56          | 768            | 12000 / 187 / 93                                         | 16 / 16 x 500                       | 2 / 4000                     |
| Standard_G3  | 8         | 112         | 1536          | 24000 / 375 / 187                                        | 32 / 32 x 500                     | 4 / 8000                |
| Standard_G4  | 16        | 224         | 3072          | 48000 / 750 / 375                                        | 64 / 64 x 500                     | 8 / 16000          |
| Standard_G5&nbsp;<sup>1</sup> | 32        | 448         | 6144          | 96000 / 1500 / 750                                       | 64 / 64 x 500                     | 8 / 20000           |

<sup>1</sup> La instancia está aislada en el hardware dedicado a un solo cliente.
<br>

## <a name="dsv2-series-11-15"></a>DSv2-series 11-15

ACU: 210 - 250 <sup>1</sup>

Premium Storage:  Compatible

Caching de Premium Storage:  Compatible

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2&nbsp;<sup>3</sup> |2 |14 |28 |8 |8000 / 64 (72) |6400 / 96 |2 / 1500 |
| Standard_DS12_v2&nbsp;<sup>3</sup> |4 |28 |56 |16 |16000 / 128 (144) |12800 / 192 |4 / 3000 |
| Standard_DS13_v2&nbsp;<sup>3</sup> |8 |56 |112 |32 |32000 / 256 (288) |25600 / 384 |8 / 6000 |
| Standard_DS14_v2&nbsp;<sup>3</sup>|16 |112 |224 |64 |64000 / 512 (576) |51200 / 768 |8 / 12 000 |
| Standard_DS15_v2&nbsp;<sup>2</sup> |20 |140 |280 |64 |80000 / 640 (720) |64000 / 960 |8 / 25 000&nbsp;<sup>4</sup>

<sup>1</sup> El rendimiento de disco máx. (E/S por segundo o Mbps) posible con una VM de la serie DSv2 puede estar limitado por el número, el tamaño y la fragmentación de los discos conectados.  Para información detallada, consulte [Azure Premium Storage: Diseño de alto rendimiento](../articles/virtual-machines/windows/premium-storage-performance.md).  
<sup>2</sup> La instancia está aislada en el hardware dedicado a un solo cliente.  
<sup>3</sup> Tamaños de núcleos restringidos disponibles.  
<sup>4</sup> 25 000 Mbps con redes aceleradas. 

<br>

## <a name="dv2-series-11-15"></a>Dv2-series 11-15

ACU: 210 - 250

Premium Storage:  No compatible

Caching de Premium Storage:  No compatible

| Tamaño              | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Rendimiento máximo de almacenamiento temporal: IOPS / MBps de lectura / MBps de escritura | Discos de datos máx. / rendimiento: E/S | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000 / 93 / 46                                           | 8 / 8x500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000 / 187 / 93                                         | 16 / 16x500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000 / 375 / 187                                        | 32 / 32x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000 / 750 / 375                                        | 64 / 64x500                       | 8 / 12 000          |
| Standard_D15_v2&nbsp;<sup>1</sup> | 20        | 140         | 1000          | 60000 / 937 / 468                                        | 64 / 64x500                       | 8 / 25 000&nbsp;<sup>2</sup> |

<sup>1</sup> La instancia está aislada en el hardware dedicado a un solo cliente.  
<sup>2</sup> 25 000 Mbps con redes aceleradas. 
