---
title: Agregar instrucciones condicionales a los flujos de trabajo - Azure Logic Apps | Microsoft Docs
description: Cómo crear condiciones que controlen las acciones de los flujos de trabajo en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/09/2018
ms.openlocfilehash: 9ee484971e217b0ca4dd7ad855e9e6dc3313e5d4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60684803"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Crear instrucciones condicionales que controlen las acciones de los flujo de trabajo en Azure Logic Apps

Para ejecutar acciones específicas en la aplicación lógica solo después de pasar una condición especificada, agregue una *instrucción condicional*. Esta estructura de control compara los datos de un flujo de trabajo con valores o campos concretos. A continuación, se pueden especificar las distintas acciones a ejecutar en función de si los datos cumplen la condición, o no. Las condiciones se pueden anidar unas dentro de otras.

Por ejemplo, suponga que tiene una aplicación lógica que envía demasiados correos electrónicos cuando aparecen elementos nuevos en la fuente RSS de un sitio web. Puede agregar una instrucción condicional para que se envíe correo electrónico solo cuando el elemento nuevo incluya una cadena específica. 

> [!TIP]
> Para ejecutar pasos diferentes para distintos valores concretos, use una [*instrucción switch*](../logic-apps/logic-apps-control-flow-switch-statement.md) en su lugar.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si aún no tiene una, [regístrese para obtener una cuenta de Azure gratuita](https://azure.microsoft.com/free/).

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Para seguir el ejemplo de este artículo, [cree una aplicación lógica de ejemplo](../logic-apps/quickstart-create-first-logic-app-workflow.md) con una cuenta de Outlook.com o de Office 365 Outlook.

## <a name="add-condition"></a>Incorporación de una condición

1. En <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, abra la aplicación lógica en Diseñador de aplicación lógica.

1. Agregue una condición en la ubicación que desee. 

   Para agregar una condición entre los pasos, mueva el puntero sobre la flecha en la que desee agregar la condición. Elija el **signo más** (**+**) que aparece y, luego, elija **Agregar una acción**. Por ejemplo: 

   ![Agregar acción entre pasos](./media/logic-apps-control-flow-conditional-statement/add-action.png)

   Si desea agregar una condición al final del flujo de trabajo, en la parte inferior de la aplicación lógica elija **+ Nuevo paso** > **Agregar una acción**.

1. En el cuadro de búsqueda, escriba "condición" como filtro. Seleccione esta acción: **Condición - Control**

   ![Incorporación de una condición](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

1. En el cuadro **Condición**, compile la condición. 

   1. En el cuadro izquierdo, especifique los datos o el campo que desee comparar.

      Al hacer clic en el cuadro izquierdo, aparece la lista de contenido dinámica para que pueda seleccionar las salidas de los pasos anteriores en su aplicación lógica. 
      En este ejemplo, seleccione el resumen de la fuente RSS.

      ![Defina la condición](./media/logic-apps-control-flow-conditional-statement/edit-condition.png)

   1. En el cuadro central, seleccione la operación que desee que se realice. 
   En este ejemplo, seleccione "**contiene**". 

   1. En el cuadro de la derecha, especifique el valor o campo que se va a usar como criterio. 
   En este ejemplo, especifique esta cadena: **Microsoft**

   Esta es la condición completa:

   ![Condición completa](./media/logic-apps-control-flow-conditional-statement/edit-condition-2.png)

   Para agregar otra fila a la condición, elija **agregar** > **Agregar fila**. 
   Para agregar un grupo con subcondiciones, elija **Agregar** > **Agregar grupo**. 
   Para agrupar filas existentes, seleccione las casillas de esas filas, elija el botón de puntos suspensivos (...) para cualquier fila y, a continuación, elija **Make group** (Crear grupo).

1. En **If true** e **If false**, agregue los pasos que se van a realizar en función de que la condición se cumpla o no. Por ejemplo: 

   ![Condición con las rutas "If true" e "If false"](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Puede arrastrar acciones existentes a las rutas **If true** e **If false**.

1. Guarde la aplicación lógica.

Ahora esta aplicación lógica envía correo electrónico solo cuando los nuevos elementos de la fuente RSS cumplen la condición.

## <a name="json-definition"></a>Definición JSON

Esta es la definición de código de alto nivel detrás de una instrucción condicional:

``` json
"actions": {
  "Condition": {
    "type": "If",
    "actions": {
      "Send_an_email": {
        "inputs": {},
        "runAfter": {}
    },
    "expression": {
      "and": [ 
        { 
          "contains": [ 
            "@triggerBody()?['summary']", 
            "Microsoft"
          ]
        } 
      ]
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar características y sugerencias o votar las que ya se han enviado, visite el [sitio web de comentarios de los usuarios de Azure Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* [Realización de pasos en función de los diferentes valores (instrucciones switch)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Ejecución y repetición de pasos (bucles)](../logic-apps/logic-apps-control-flow-loops.md)
* [Ejecución o combinación de pasos en paralelo (ramas)](../logic-apps/logic-apps-control-flow-branches.md)
* [Ejecución de pasos en función del estado de las acciones agrupadas (ámbitos)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
