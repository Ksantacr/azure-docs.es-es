---
title: Crear ofertas de Marketplace | Azure Marketplace
description: Creación de las ofertas de los Marketplaces de Azure y AppSource mediante Cloud Partner Portal
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pabutler
ms.openlocfilehash: cc0d8e68578500d4fa5f1b167790d60c8f7c5067
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64942179"
---
# <a name="create-azure-marketplace-and-appsource-offers"></a>Creación de las ofertas de Azure Marketplace y AppSource

Un objetivo fundamental de Cloud Partner Portal es permitir a los anunciantes crear (y, a continuación, publicar) ofertas en los Marketplace de Microsoft Azure y AppSource.  Esta operación siempre comienza con la selección del tipo de oferta desde el [menú Nueva oferta](../portal-tour/cpp-new-offer-menu.md).  En respuesta, se muestra la página **Nueva oferta** correspondiente para ese tipo de oferta.  Por ejemplo, la siguiente imagen muestra los valores predeterminados de la página **Nueva oferta** para un tipo de aplicación de Azure.

![Página predeterminada de Nueva oferta](./media/new-offer-page.png)

Hay dos selecciones de pestañas en la barra de menús horizontal que se muestra en la parte superior de esta página: 
- Pestaña **Editor**: permite escribir información y cargar recursos para la nueva instancia de oferta.  Esta pestaña se muestra de forma predeterminada.
- Pestaña **Estado**: proporciona el estado de publicación y enumera los problemas de validación y revisión. 

Al crear una oferta, use la pestaña **Editor** para especificar información sobre dicha oferta. 

## <a name="editing-operations"></a>Operaciones de edición

La barra de herramientas horizontal situada sobre el área de entrada de datos muestra los botones siguientes:

|   Botón    |   Propósito                                                          |
|   ------    |  --------                                                          |
| **Guardar**    | Guarda los cambios recientes de entrada de datos.  Debe guardar manualmente los cambios antes de salir de la página o estos se perderán. | 
| **Descartar** | Descarta los cambios de entradas de datos recientes (desde el último guardado)             |
| **Comparar** | Compara el estado de la oferta actual con la oferta publicada.  Solo se habilita después de que una oferta se ha publicado correctamente.  |
| **Publicar** | Comienza el proceso de publicación para esta oferta.                       |
| **Eliminar**  | Elimina esta oferta después de crearla, pero antes de que se haya publicado. |
|   |   |


## <a name="editing-tabs"></a>Pestañas de edición

Al crear una oferta, proporcione los datos necesarios y opcionales en todas las pestañas situadas en la columna vertical izquierda de la página **Nueva oferta**.  Se muestran controles de interfaz de usuario estándar (como cuadros de texto, menús desplegables y casillas de verificación) para la recopilación de datos.  Aunque el grupo específico de pestañas de edición depende del tipo de oferta, en la tabla siguiente se enumeran algunas de las pestañas comunes.

|      Nombre de pestaña       |   Propósito                                                            |
|      --------       |   -------                                                            |
| **Valores de la oferta**  | Recopila información sobre la identidad del anunciante y la oferta.                    |
| **SKU**            | Define las características técnicas y empresariales para cada versión de la unidad de referencia de almacén (SKU) de la oferta. |
| **Versión de prueba**      | Para los tipos de oferta que admiten esta característica opcional, define una demostración de dicha oferta.  Para más información, consulte [¿Qué es la versión de prueba?](../test-drive/what-is-test-drive.md)  |
| **Marketplace** o **Escaparate** | Recopila las cadenas de texto, documentos e imágenes que se usan para mostrar la oferta en Marketplace. |
| **Soporte técnico**         | Recopila información de contacto para la asistencia al cliente, el soporte técnico a ingeniería y el soporte en línea.  |
|  |  |

El contenido de las pestañas con el mismo nombre puede diferir entre distintos tipos de oferta.  Se proporcionan detalles específicos de estas pestañas relacionados con las ofertas en la sección "Crear oferta" de cada tipo de oferta.


## <a name="next-steps"></a>Pasos siguientes

Después de crear y guardar una oferta y antes o después de haberla publicado, puede [ver su estado](./cpp-view-status-offer.md).
