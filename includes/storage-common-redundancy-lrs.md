---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 02/12/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 124f5c01b7718f729094de1c02391946ff50cef4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113791"
---
El almacenamiento con redundancia local (LRS) proporciona una durabilidad mínima del 99,999999999 % (once nueves) de los objetos en un año determinado. LRS ofrece esta durabilidad de objeto mediante la replicación de los datos en una unidad de escalado de almacenamiento. Un centro de datos, ubicado en la región en la que creó su cuenta de almacenamiento, hospeda la unidad de escalado de almacenamiento. Una solicitud de escritura a una cuenta de almacenamiento de LRS se devuelve correctamente solo después de que los datos se escriben en todas las réplicas. Cada réplica reside en dominios de error y dominios de actualización independientes dentro de una unidad de escalado de almacenamiento.

Una unidad de escalado de almacenamiento es una colección de estanterías de nodos de almacenamiento. Un dominio de error (FD) es un grupo de nodos que representan una unidad física de error. Piense en un dominio de error como nodos que pertenecen al mismo bastidor físico. Un dominio de actualización (UD) es un grupo de nodos que se actualizan en conjunto durante el proceso de actualización de un servicio (implementación). Las réplicas se distribuyen entre dominios de actualización y dominios de error dentro de una unidad de escalado de almacenamiento. Esta arquitectura garantiza que los datos están disponibles si un error de hardware afecta a único bastidor o cuando los nodos se actualizan durante una actualización de servicio.

LRS es la opción de replicación de costo más bajo y ofrece la menor durabilidad en comparación con otras opciones. Si se produce un desastre en el nivel de centro de datos (por ejemplo, un incendio o una inundación), es posible que todas las réplicas se pierdan o que no se puedan recuperar. Para mitigar este riesgo, Microsoft recomienda el uso del almacenamiento con redundancia de zona (ZRS) o del almacenamiento con redundancia geográfica (GRS).

* Si la aplicación almacena los datos que se pueden reconstruir fácilmente si se produce una pérdida de datos, puede optar por LRS.
* Algunas aplicaciones están restringidas a replicar datos solo dentro de un país o región debido a requisitos de regulación de datos. En algunos casos, las regiones emparejadas en los que los datos se replican para cuentas GRS pueden estar en otro país o región. Para más información sobre las regiones emparejadas, consulte [Regiones de Azure](https://azure.microsoft.com/regions/).
