---
ms.openlocfilehash: 52dfbfca5f79a7f92848ea39eddc00aa10f05ff1
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66110560"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Busque un anclaje espacial en la nube

Ser capaz de encontrar un anclaje espacial previamente cargado en la nube es una de las razones principales para usar la biblioteca espacial delimitadores de Azure. Para buscar delimitadores espacial de la nube, necesitará saber sus identificadores. Los identificadores de delimitador se pueden almacenar en el servicio de back-end de la aplicación y puede tener acceso a todos los dispositivos que pueden autenticar correctamente a ella. Para obtener un ejemplo de esta, consulte [Tutorial: Comparta delimitadores espaciales entre dispositivos](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Crear una instancia de un `AnchorLocateCriteria` de objeto, establecer los identificadores que está buscando e invocar el `CreateWatcher` método en la sesión proporcionando su `AnchorLocateCriteria`.
