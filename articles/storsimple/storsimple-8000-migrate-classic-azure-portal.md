---
title: Migración de cuentas de almacenamiento y suscripciones para el servicio Administrador de dispositivos de StorSimple | Microsoft Docs
description: Obtenga información sobre cómo migrar las suscripciones y las cuentas de almacenamiento para el servicio Administrador de dispositivos de StorSimple 8000.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/14/2019
ms.author: alkohli
ms.openlocfilehash: 3cce18fa1890fc9e518294e294cc43e0e55065aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60631541"
---
# <a name="migrate-subscriptions-and-storage-accounts-associated-with-storsimple-device-manager-service"></a>Migración de suscripciones y cuentas de almacenamiento asociadas con el servicio Administrador de dispositivos de StorSimple

Puede que tenga que mover el servicio StorSimple a una inscripción o a una suscripción nueva. Estos escenarios de migración son cambios de cuenta o cambios del centro de datos. Use la tabla siguiente para saber cuáles de estos escenarios se admiten, incluidos los pasos detallados para el movimiento.

## <a name="account-changes"></a>Cambios de cuenta

| Puede mover…| Compatible| Tiempo de inactividad| Proceso de soporte técnico de Azure| Enfoque|
|-----|-----|-----|-----|-----|
| Toda una suscripción (incluidas cuentas de almacenamiento y el servicio StorSimple) a otra inscripción. | Sí       | Sin        | **Transferencia de inscripción**<br>Se usa:<li>Cuando adquiere un compromiso de Azure en virtud de un contrato nuevo.</li><li>Cuando quiere migrar todas las cuentas y suscripciones de la inscripción anterior a la nueva. Esto incluye todos los servicios de Azure de la suscripción anterior.</li> | **Paso 1: Abra una incidencia de soporte técnico de operación de Azure Enterprise.**<li>Vaya a [https://aka.ms/AzureEntSupport](https://aka.ms/AzureEntSupport).</li><li> Seleccione **Enrollment Administraiton** (Administración de inscripción) y luego seleccione **Transfer from one enrollment to a new enrollment** (Transferir de una inscripción a una inscripción nueva).<br>**Paso 2: Proporcione la información solicitada**<br>Incluya:<li>el número de inscripción de origen</li><li> el número de inscripción de destino</li><li>la fecha de vigencia de la transferencia|
| El servicio StorSimple de una cuenta existente a una inscripción nueva.    | Sí       | Sin        | **Transferencia de cuenta**<br>Uso:<li>Cuando no se desea hacer una transferencia de inscripción completa.</li><li>Cuando solo desea mover cuentas específicas a una inscripción nueva.</li>| **Paso 1: Abra una incidencia de soporte técnico de operación de Azure Enterprise.**<li>Vaya a [https://aka.ms/AzureEntSupport](https://aka.ms/AzureEntSupport).</li><li>Seleccione **Enrollment Administration** (Administración de inscripción) y luego seleccione **Transfer an EA Account to a new enrollment** (Transferir una cuenta de EA a una inscripción nueva).<br>**Paso 2: Proporcione la información solicitada**<br>Incluya:<li>el número de inscripción de origen</li><li> el número de inscripción de destino</li><li>la fecha de vigencia de la transferencia|
| El servicio StorSimple de una suscripción a otra.      | Sin         |    Sí         | Ninguno, se trata de un proceso manual|<li>Migre datos desde el dispositivo StorSimple.</li><li>Restablezca el dispositivo a los valores de fábrica. De este modo se eliminan todos los datos locales del dispositivo.</li><li>Registre el dispositivo con la suscripción nueva a un servicio Administrador de dispositivos de StorSimple.</li><li>Migre los datos de vuelta al dispositivo.|
|¿Puedo transferir a otro directorio la propiedad de una suscripción a Azure? | Sí       | Sin        | Asociación de una suscripción existente al directorio de Azure AD | Consulte [To associate an existing subscription to your Azure AD directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md) (Asociación de una suscripción existente al directorio de Azure AD) Puede tardar hasta 10 minutos para que todo se muestre correctamente.|
| Un dispositivo StorSimple de un servicio Administrador de dispositivos StorSimple a otro servicio en una región distinta.      | Sin         | Sí            | Ninguno, se trata de un proceso manual |Igual que el anterior.|
| ¿Cuenta de almacenamiento a una nueva suscripción o grupo de recursos?     | Sí        | Sin              |Migración de una cuenta de almacenamiento a otra suscripción o grupo de recursos |Después de la migración, si se actualizan las claves de acceso de la cuenta de almacenamiento, el usuario deberá configurarlas manualmente para al cuenta de almacenamiento con el servicio Administrador de dispositivos de StorSimple.|
| Cuenta de almacenamiento clásico a una cuenta de almacenamiento de Azure Resource Manager      | Sí        | Sin              |Migración del modelo clásico a Azure Resource Manager |<li>Para detalles sobre cómo migrar una cuenta de almacenamiento desde el modelo clásico a Azure Resource Manager, vaya al artículo sobre la [migración de una cuenta de almacenamiento clásico](../virtual-machines/windows/migration-classic-resource-manager-ps.md#step-62-migrate-a-storage-account).</li><li> Si las claves de acceso de la cuenta de almacenamiento se actualizan después de la migración, el usuario deberá sincronizar las claves de acceso con la cuenta de almacenamiento migrada mediante el servicio Administrador de dispositivos de StorSimple. Esto, para asegurarse de que los dispositivos de StorSimple sigan funcionando con normalidad y puedan almacenar en capas los datos principales y de copia de seguridad en Azure. Para instrucciones detalladas sobre cómo sincronizar las claves de acceso, vaya a [Flujo de trabajo de rotación](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts).</li><li> En el caso de una instancia de StorSimple Cloud Appliance, si se migra la cuenta de almacenamiento clásico pero la máquina virtual siguen en el modelo clásico, el dispositivo debería funcionar correctamente. Si se migra la máquina virtual subyacente del dispositivo en la nube, la funcionalidad de desactivación y eliminación no funcionará.</li><li> Debe crear una instancia nueva de StorSimple Cloud Appliances en Azure Portal y, luego, realizar una conmutación por error desde los dispositivos anteriores en la nube. No puede crear una instancia de StorSimple Cloud Appliance en el nuevo Azure Portal con una cuenta de almacenamiento clásico porque necesitan una cuenta de almacenamiento de Azure Resource Manager. Para más información, vaya a [Implementación y administración de una instancia de StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).</li>|

## <a name="datacenter-changes"></a>Cambios en el centro de datos

| Puede mover…| Compatible|Tiempo de inactividad| Proceso de soporte técnico de Azure| Enfoque|
|-----|-----|-----|-----|-----|
| Un servicio StorSimple de un centro de datos de Azure a otro. | Sin  | Sí |Ninguno, se trata de un proceso manual  |<li>Migre datos desde el dispositivo StorSimple.</li><li>Restablezca el dispositivo a los valores de fábrica. De este modo se eliminan todos los datos locales del dispositivo.</li><li>Registre el dispositivo con la suscripción nueva a un servicio Administrador de dispositivos de StorSimple nuevo.</li><li>Migre los datos de vuelta al dispositivo.|
| Una cuenta de StorSimple de un centro de datos de Azure a otro. | Sin  |Sí  |Ninguno, se trata de un proceso manual  | Igual que el anterior.|

## <a name="next-steps"></a>Pasos siguientes

* [Implementación del servicio Administrador de dispositivos StorSimple](storsimple-8000-manage-service.md)
* [Implementación del dispositivo StorSimple 8000 en Azure Portal](storsimple-8000-deployment-walkthrough-u2.md)
