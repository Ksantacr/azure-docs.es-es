---
title: Opciones de transferencia de datos de Azure para grandes conjuntos de datos con poco o ningún ancho de banda de red | Microsoft Docs
description: Obtenga información sobre cómo elegir una solución de Azure para la transferencia de datos cuando ha limitado el ancho de banda de red a un valor nulo en su entorno y va a transferir grandes conjuntos de datos.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 4c4ac9489b9613b2eeaf26a3df9f4cbc664a1026
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730697"
---
# <a name="data-transfer-for-large-datasets-with-low-or-no-network-bandwidth"></a>Transferencia de datos para grandes conjuntos de datos con poco o ningún ancho de banda de red
 
En este artículo encontrará información general sobre las soluciones para la transferencia de datos cuando ha limitado el ancho de banda de red a un valor nulo en su entorno y va a transferir grandes conjuntos de datos. En el artículo también se describen las opciones recomendadas de transferencia de datos y la matriz de funcionalidades clave para este escenario.

Para obtener una visión general de todas las opciones de transferencia de datos disponibles, vaya a [Choose an Azure data transfer solution](storage-choose-data-transfer-solution.md) (Elección de una solución de transferencia de datos de Azure).

## <a name="offline-transfer-or-network-transfer"></a>Transferencia sin conexión o transferencia de red

Grandes conjuntos de datos implican que tiene a algunos TB a algunos PB de los datos. Tiene un ancho de banda de red limitado o nulo, la red es lenta o no es confiable. También:

- Está limitado por los costos de transferencia de red de sus proveedores de servicios de Internet (ISP).
- Las directivas de seguridad o de la organización no permiten las conexiones salientes cuando se trabaja con información confidencial.

En todas las instancias anteriores, use un dispositivo físico para realizar una transferencia de datos masiva de un solo uso. Elija entre dispositivos Data Box Disk, Data Box, Data Box Heavy proporcionados por Microsoft o Import/Export con sus propios discos.

Para confirmar si un dispositivo físico es la opción más adecuada, utilice la siguiente tabla. Muestra el tiempo previsto para la transferencia de datos de red, para diversos anchos de banda disponibles (suponiendo un uso del 90 %). Si está previsto que la transferencia de red sea demasiado lenta, debe usar un dispositivo físico.  

![Transferencia de red o transferencia sin conexión](media/storage-solution-large-dataset-low-network/storage-network-or-offline-transfer.png)

## <a name="recommended-options"></a>Opciones recomendadas

Las opciones disponibles en este escenario son dispositivos para la transferencia sin conexión de Azure Data Box o Azure Import/Export.

- **Familia Azure Data Box para transferencias sin conexión**: Use dispositivos Data Box suministrados por Microsoft para mover grandes cantidades de datos a Azure cuando esté limitado por el tiempo, por la disponibilidad de red o por los costos. Copie los datos locales con herramientas como Robocopy. Según el tamaño de los datos previstos para la transferencia, puede elegir entre Data Box Disk, Data Box o Data Box Heavy.
- **Azure Import/Export**: use el servicio Azure Import/Export para enviar sus propias unidades de disco y así importar de forma segura grandes cantidades de datos a Azure Blob Storage y Azure Files. También se puede usar este servicio para transferir datos desde Azure Blob Storage hasta las unidades de disco y enviarlas al sitio local.

## <a name="comparison-of-key-capabilities"></a>Comparación de funcionalidades clave

En la siguiente tabla se resumen las diferencias de las funcionalidades clave.

|                                     |    Data Box Disk      |    Data Box                                      |    Data Box Heavy              |    Import/Export                       |
|-------------------------------------|---------------------------------|--------------------------------------------------|------------------------------------------|----------------------------------------|
|    Tamaño de los datos                        |    Hasta 35 TB                 |    Hasta 80 TB por dispositivo                       |    Hasta 800 TB por dispositivo               |    Variable                            |
|    Tipo de datos                        |    Azure Blobs                  |    Azure Blobs<br>Archivos de Azure                    |    Azure Blobs<br>Archivos de Azure            |    Azure Blobs<br>Archivos de Azure          |
|    Factor de forma                      |    5 SSD por pedido             |    1 x 50 libras dispositivo de tamaño de escritorio por pedido    |    1 x ~500 libras dispositivo grande por pedido    |    Hasta 10 HDD/SSD por pedido        |
|    Tiempo de instalación inicial               |    Bajo <br>(15 min)            |    De bajo a moderado <br> (<30 min)               |    Moderado<br>(De 1 a 2 horas)               |    De moderado a difícil<br>(variable) |
|    Envío de datos a Azure               |    Sí                          |    Sí                                           |    Sí                                   |    Sí                                 |
|    Exportar datos de Azure           |    Sin                            |    No                                            |    No                                    |    Sí                                 |
|    Cifrado                       |    AES de 128 bits                  |    AES de 256 bits                                   |    AES de 256 bits                           |    AES de 128 bits                         |
|    Hardware                         |     Proporcionado por Microsoft          |    Proporcionado por Microsoft                            |    Proporcionado por Microsoft                    |    Proporcionado por el cliente                   |
|    interfaz de red                |    USB 3.1/SATA                 |    RJ 45, SFP+                                   |    RJ45, QSFP+                           |    SATA II/SATA III                    |
|    Integración de asociados              |    Algunos                         |    [Elevado](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                          |    [Elevado](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                  |    Algunos                                |
|    Envío                         |    Administrado por Microsoft            |    Administrado por Microsoft                             |    Administrado por Microsoft                     |    Administrado por el cliente                    |
| Uso cuando se mueven los datos         |Dentro de un límite de comercio|Dentro de un límite de comercio|Dentro de un límite de comercio|Traspasando fronteras geográficas; por ejemplo, de EE. UU. a la UE|
|    Precios                          |    [Precios](https://azure.microsoft.com/pricing/details/databox/disk/)                    |   [Precios](https://azure.microsoft.com/pricing/details/storage/databox/)                                      |  [Precios](https://azure.microsoft.com/pricing/details/storage/databox/heavy/)                               |   [Precios](https://azure.microsoft.com/pricing/details/storage-import-export/)                            |


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a

    - [Transferir datos con Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-quickstart-portal).
    - [Transferir datos con Data Box](https://docs.microsoft.com/azure/databox/data-box-quickstart-portal).
    - [Transferir datos con Import/Export](/azure/storage/common/storage-import-export-data-to-blobs).
