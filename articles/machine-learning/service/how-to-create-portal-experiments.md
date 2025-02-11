---
title: Crear y explorar los experimentos en el Portal
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo crear y administrar automatizadas experimentos de aprendizaje automático en el portal
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tsikiksr
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/02/2019
ms.openlocfilehash: a2a281fda9272fb794692becb0ca08f3cf791458
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990141"
---
# <a name="create-and-explore-automated-machine-learning-experiments-in-the-azure-portal-preview"></a>Crear y exploración automatizada experimentos de aprendizaje automático en Azure portal (versión preliminar)

 En este artículo, aprenderá a crear, ejecutar y exploración automatizada experimentos de aprendizaje automático en el portal de Azure sin una sola línea de código. Aprendizaje automático automatizadas automatiza el proceso de selección del mejor algoritmo que se usará para los datos específicos, por lo que puede generar un modelo de aprendizaje automático rápidamente. [Más información sobre aprendizaje automático automatizadas](concept-automated-ml.md).

 Si prefiere una experiencia más basada en código, también puede [configurar la máquina automatizada experimentos de Python](how-to-configure-auto-train.md) con el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Un área de trabajo de Azure Machine Learning. Consulte [crear un área de trabajo del servicio de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

## <a name="get-started"></a>Introducción

Navegue hasta el panel izquierdo del área de trabajo. Seleccione automatizada de Machine Learning en la sección de creación (versión preliminar).

![Panel de navegación de Azure portal](media/how-to-create-portal-experiments/nav-pane.png)

 Si se trata de la primera vez que se realiza cualquier experimentos con automatizada de Machine Learning, verá lo siguiente:

![Página de aterrizaje de experimento de portal de Azure](media/how-to-create-portal-experiments/landing-page.png)

En caso contrario, verá el panel de aprendizaje de máquina automatizada con una visión general de todos sus automatizadas experimentos de aprendizaje automático, incluidas aquellas creadas con el SDK. Aquí puede filtrar y explorar las ejecuciones por fecha, experimentar el nombre y estado de ejecución.

![Panel de experimento de portal de Azure](media/how-to-create-portal-experiments/dashboard.png)

## <a name="create-an-experiment"></a>Creación de un experimento

Seleccione el botón Crear experimento para rellenar el formulario siguiente.

![Crear formulario de experimento](media/how-to-create-portal-experiments/create-exp-name-compute.png)

1. Escriba el nombre del experimento.

1. Seleccione un proceso para la generación de perfiles de datos y el trabajo de aprendizaje. Una lista de los procesos existentes está disponible en la lista desplegable. Para crear un nuevo proceso, siga las instrucciones en el paso 3.

1. Seleccione el botón Crear un nuevo proceso para abrir el siguiente panel y configurar el contexto de proceso para este experimento.

    ![Crear nuevo proceso para el experimento](media/how-to-create-portal-experiments/create-new-compute.png)

    Campo|DESCRIPCIÓN
    ---|---
    Nombre del proceso| Escriba un nombre único que identifica el contexto de proceso.
    Tamaño de máquina virtual| Seleccione el tamaño de máquina virtual para la capacidad de proceso.
    Configuración adicional| *Nodo min*: Escriba el número mínimo de nodos de la capacidad de proceso. El número mínimo de nodos de proceso de Azure Machine Learning es 0. Para habilitar la generación de perfiles de datos, debe tener 1 o más nodos. <br> *Nodo max*: Escriba el número máximo de nodos para la capacidad de proceso. El valor predeterminado es 6 nodos para un proceso de Azure Machine Learning.

      Para iniciar la creación de la capacidad de proceso nuevo, seleccione **crear**. Esto puede tardar unos minutos.

      >[!NOTE]
      > El nombre de proceso indicará si es el proceso que seleccione o cree *generación de perfiles habilitado*. (Consulte 7b para obtener más detalles sobre la generación de perfiles de datos).

1. Seleccione una cuenta de almacenamiento para los datos. Versión preliminar pública solo admite cargas de archivos locales y cuentas de Azure Blob Storage.

1. Seleccione un contenedor de almacenamiento.

1. Seleccione un archivo de datos en el contenedor de almacenamiento, o cargar un archivo desde el equipo local al contenedor.

    ![Seleccione el archivo de datos para el experimento](media/how-to-create-portal-experiments/select-file.png)

1. Use las pestañas de vista previa y el perfil para configurar aún más los datos para este experimento.

    1. En la pestaña de vista previa, indicar si los datos incluyen encabezados y seleccione las características (columnas) para el uso de aprendizaje del **incluidos** cambiar los botones en cada columna de característica.

        ![Vista previa de datos](media/how-to-create-portal-experiments/data-preview.png)

    1. En la pestaña perfil, puede ver el [perfil de datos](#profile) por característica, así como la distribución, type y estadísticas de resumen (Media, mediana, max y min etc.) de cada uno.

        ![Ficha perfil de datos](media/how-to-create-portal-experiments/data-profile.png)

        >[!NOTE]
        > El siguiente mensaje de error aparecerá si el contexto de cálculo es **no** generación de perfiles habilitado: *Generación de perfiles de datos solo está disponible para destinos de proceso que se están ejecutando*.

1. Seleccione el tipo de trabajo de aprendizaje: clasificación, regresión o previsión.

1. Seleccione la columna de destino. La columna que le gustaría hacer predicciones de en.

1. Para la predicción:
    1. Seleccione la columna de hora: Esta columna contiene los datos de tiempo que se usará.
    1. Seleccione el horizonte de previsión: Indique cuántas unidades de tiempo (minutos/horas/días o semanas o meses/años) el modelo podrá predecir el futuro. Cuanto más se requiere el modelo para predecir el futuro, menos preciso será. [Aprender más acerca de previsión y previsión horizonte](https://docs.microsoft.com/azure/machine-learning/service/how-to-auto-train-forecast#configure-experiment).

1. (Opcional) Configuración avanzada: configuración adicional que puede usar para controlar mejor el trabajo de aprendizaje.

    Configuración avanzada|DESCRIPCIÓN
    ------|------
    Métrica principal| Métrica principal usado para puntuar el modelo. [Más información sobre las métricas de modelo](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-auto-train#explore-model-metrics).
    Criterios de salida| Cuando se cumple cualquiera de estos criterios, finaliza el trabajo de aprendizaje antes de finalizar la completa. <br> *Tiempo (minutos) de trabajo de entrenamiento*: ¿Cuánto tiempo para permitir la ejecución del trabajo de aprendizaje.  <br> *Número máximo de iteraciones*: Número máximo de canalizaciones (iteraciones) para probar en el trabajo de aprendizaje. El trabajo no ejecutará más el número especificado de iteraciones. <br> *Métrica de umbral de puntuación*:  Puntuación mínima de métrica para todas las canalizaciones. Esto garantiza que si tiene una métrica de destino definido que desee alcanzar, que no dedicar más tiempo en el trabajo de aprendizaje de los necesarios.
    Preprocesamiento| Seleccione esta opción para habilitar o deshabilitar el preprocesamiento realiza automatizada machine Learning. Preprocesamiento incluye la limpieza automática de los datos, preparar y transformación para generar características sintéticas. [Más información acerca de preprocesamiento](#preprocess).
    Validación| Seleccione una de las opciones de validación cruzada en el trabajo de aprendizaje. [Obtenga más información acerca de la validación cruzada](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-auto-train#cross-validation-split-options).
    Simultaneidad| Seleccione los límites de varios núcleos que le gustaría usar cuando se usa el proceso de varios núcleo.
    Algoritmo de bloqueo| Seleccione los algoritmos que desee excluir de la tarea de aprendizaje.

   ![Formulario de configuración avanzada](media/how-to-create-portal-experiments/advanced-settings.png)

> [!NOTE]
> Para obtener más información sobre los campos, haga clic en la información sobre herramientas de información.

<a name="profile"></a>

### <a name="data-profiling"></a>Datos de generación de perfiles

Puede obtener una gran variedad de estadísticas de resumen en el conjunto de datos para comprobar si el conjunto de datos está listo para el aprendizaje automático. Para las columnas no numéricas, incluyen estadísticas solo básicas, como min, max y recuento de errores. Para las columnas numéricas, también puede revisar sus momentos estadísticas y el estimado cuantiles. En concreto, nuestro perfil de datos incluye:

* **Característica**: nombre de la columna que se está resumiendo.

* **Perfil**: una visualización en línea según el tipo deducido. Por ejemplo, cadenas, booleanos y fechas tendrá recuentos de valores mientras decimales (valores numéricos) aproximan histogramas. Esto le permite obtener una descripción rápida de la distribución de los datos.

* **Tipo de distribución**: un recuento de valor en la línea de los tipos dentro de una columna. Los valores NULL son su propio tipo, por lo que es útil para detectar los valores que faltan o impares esta visualización.

* **Tipo**: el tipo inferido de la columna. Los valores posibles incluyen: cadenas, fechas, valores booleanos y decimales.

* **Min**: el valor mínimo de la columna. Aparecen las entradas en blanco para características cuyo tipo no tiene una ordenación inherente (por ejemplo, valores booleanos).

* **Max**: el valor máximo de la columna. Al igual que "min", entradas vacías aparecen para las características con tipos irrelevantes.

* **Recuento de**: el número total de entradas que faltan y que no faltan en la columna.

* **Falta el recuento no**: el número de entradas en la columna que no faltan. Tenga en cuenta que los errores y las cadenas vacías se tratan como valores, por lo que no contribuyan al "recuento que no falta".

* **Cuantiles** (en intervalos de 0,1, 1, 5, 25, 50, 75, 95, 99 y un 99,9%): los valores aproximados en cada cuantil para proporcionar una idea de la distribución de los datos. Entradas vacías aparecen para las características con tipos irrelevantes.

* **Significa**: la media aritmética de la columna. Entradas vacías aparecen para las características con tipos irrelevantes.

* **Desviación estándar**: la desviación estándar de la columna. Entradas vacías aparecen para las características con tipos irrelevantes.

* **Varianza**: la varianza de la columna. Entradas vacías aparecen para las características con tipos irrelevantes.

* **Asimetría**: el sesgo de la columna. Entradas vacías aparecen para las características con tipos irrelevantes.

* **Curtosis**: la curtosis de la columna. Entradas vacías aparecen para las características con tipos irrelevantes.

<a name="preprocess"></a>

### <a name="advanced-preprocessing"></a>Avanzada de preprocesamiento

Al configurar sus experimentos, puede habilitar la opción de configuración avanzada `Preprocess`. Si lo hace que los pasos siguientes de características y preprocesamiento de datos se realizan automáticamente.

|Preprocesamiento&nbsp;pasos| DESCRIPCIÓN |
| ------------- | ------------- |
|Se quitan las características de cardinalidad alta o sin variación|Quítelos de conjuntos de entrenamiento y validación, incluidas las características con todos los valores que faltan, el mismo valor en todas las filas o con una cardinalidad muy alta (por ejemplo, hash, identificadores o GUID).|
|Atribución de valores que faltan|Para las características numéricas, imputar con promedio de valores de la columna.<br/><br/>Para características de categorías, imputar con el valor más frecuente.|
|Generación de características adicionales|Para las características de fecha y hora: año, mes, día, día de la semana, día del año, trimestre, semana del año, hora, minuto, segundo.<br/><br/>Para las características de texto: Frecuencia de término en función de carácter trigramas, unigramas y bigramas.|
|Transformar y codificar |Las características numéricas con pocos valores únicos se transforman en características categóricas.<br/><br/>Realiza la codificación "one-hot" para cardinalidad baja categoría; para una cardinalidad alta, codificación de hash "hot" uno.|
|Inserciones de palabras|Caracterizador de texto que convierte los vectores de tokens de texto en vectores de oración mediante un modelo previamente entrenado. Vector de incrustación de cada palabra en un documento se agrega en conjunto para generar un vector de característica del documento.|
|Codificaciones de destino|Para características de categorías, se asigna cada categoría con el valor de destino promedio para los problemas de regresión y la probabilidad de la clase para cada clase de problemas de clasificación. Ponderación basada en frecuencia y k plegamiento de validación cruzan se aplica para reducir el ajuste de la asignación y el ruido de las categorías de datos dispersos.|
|Codificación de destino de texto|Entrada de texto, se usa un modelo lineal apilado con la bolsa de palabras para generar la probabilidad de cada clase.|
|Peso de evidencia (problemas)|Calcula los problemas como una medida de la correlación de las columnas de categorías para la columna de destino. Se calcula como el registro de la relación de las probabilidades de vs de su clase de la clase. Este paso genera una columna de característica numéricos por clase y elimina la necesidad de forma explícita imputar valores que faltan y tratamiento de valores atípicos.|
|Distancia del clúster|Entrena un modelo de agrupación en clústeres k-means en todas las columnas numéricas.  Salidas k nuevas características, una nueva característica numérica por clúster, que contiene la distancia de cada ejemplo para el centroide de cada clúster.|

## <a name="run-experiment-and-view-results"></a>Ejecutar el experimento y ver resultados

Para ejecutar el experimento, haga clic en Inicio. El experimento Preparando proceso tarda unos minutos.

### <a name="view-experiment-details"></a>Ver detalles del experimento

Una vez que se realiza la fase de preparación del experimento, verá la pantalla de detalles de ejecución. Esto proporciona una lista completa de los modelos creados. De forma predeterminada, el modelo que puntúa la más alta, en función de los parámetros es la parte superior de la lista. Como se trata el trabajo de aprendizaje más modelos, se agregan a la lista de iteración y el gráfico. Para obtener una comparación rápida de las métricas para los modelos generados hasta ahora, utilice el plan de iteración.

Los trabajos de entrenamiento puede tardar un tiempo para cada canalización terminen de ejecutarse.

![Panel de detalles de ejecución](media/how-to-create-portal-experiments/run-details.png)

### <a name="view-training-run-details"></a>Ver detalles de ejecución de los cursos

Explorar en profundidad en cualquiera de los modelos de salida para ver los detalles de ejecución, como los gráficos de métricas y distribución de rendimiento de entrenamiento. [Más información acerca de los gráficos](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments#understanding-automated-ml-charts).

![Detalles de iteración](media/how-to-create-portal-experiments/iteration-details.png)

## <a name="deploy-model"></a>Implementación del modelo

Una vez que tenga a mano el mejor modelo, es el momento de implementarlo como un servicio web para predecir datos nuevos.

ML automatizada le ayuda con la implementación del modelo sin escribir código:

1. Tiene varias opciones para la implementación. 
    1. Si desea implementar el mejor modelo según los criterios de las métricas establezca para el experimento, seleccione **implementar mejor modelo** desde el **detalles de ejecución** página.

        ![Botón modelo de implementación](media/how-to-create-portal-experiments/deploy-model-button.png)

    1. Si desea implementar una iteración de modelo específico, explorar en profundidad en el modelo para abrir su página de detalles específicos de la ejecución y seleccione **implementar modelo**.

        ![Botón modelo de implementación](media/how-to-create-portal-experiments/deploy-model-button2.png)

1. Primer paso es registrar el modelo en el servicio. Seleccione "Registrar el modelo" y espere a que el proceso de registro se complete.

    ![Hoja de modelo de implementación](media/how-to-create-portal-experiments/deploy-model-blade.png)

1. Una vez que se registra el modelo, podrá descargar el script de puntuación (scoring.py) y el entorno de secuencia de comandos (condaEnv.yml) que se usará durante la implementación.

1. Cuando se descargan el script de puntuación y la secuencia de comandos del entorno, vaya a la **activos** hoja del panel de navegación izquierdo y seleccione **modelos**.

    ![Modelos del panel de navegación](media/how-to-create-portal-experiments/nav-pane-models.png)

1. Seleccione el modelo que se ha registrado y seleccione "Crear imagen".

    Puede identificar el modelo por su descripción, que incluirá el identificador de ejecución, el número de iteración, en el siguiente formato: *< Run_ID > _ < Iteration_number > _modelo*

    ![Modelos: Crear imagen](media/how-to-create-portal-experiments/model-create-image.png)

1. Escriba un nombre para la imagen. 
1. Seleccione el **examinar** situado junto al cuadro "Archivo de puntuación" para cargar el archivo de puntuación (scoring.py) que descargó anteriormente.

1. Seleccione el **examinar** situado junto al cuadro "Archivo Conda" para cargar el archivo de entorno (condaEnv.yml) que descargó anteriormente.

    Puede usar su propio script de puntuación y el archivo conda, así como cargar archivos adicionales. [Más información sobre el script de puntuación](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where#script).

      >[!Important]
      > Los nombres de archivo deben tener menos de 32 caracteres y debe comenzar y terminar por caracteres alfanuméricos. Puede incluir guiones, caracteres de subrayado, puntos y caracteres alfanuméricos entre. No se permiten espacios.

    ![Crear imagen](media/how-to-create-portal-experiments/create-image.png)

1. Seleccione el botón "Crear" para iniciar la creación de imagen. Esto tardará unos minutos en completarse, una vez hecho esto, verá un mensaje en la barra superior.
1. Vaya a la pestaña "Imágenes", active la casilla situada junto a la imagen que desea implementar y seleccione "Crear implementación". [Más información acerca de las implementaciones](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where).

    Hay 2 opciones para la implementación.
     + Instancia de Azure Container Instances (ACI) - se usa más para realizar pruebas a propósito, en lugar de una implementación operativa a escala. Asegúrese de rellenar los valores para al menos un núcleo para _capacidad de reserva de CPU_y al menos un gigabyte (GB) para _capacidad de reserva de memoria_
     + Azure Kubernetes Service (AKS)): esta opción es para la implementación a escala. Debe tener un proceso en función de AKS preparado.

     ![Imágenes: Creación de una implementación](media/how-to-create-portal-experiments/images-create-deployment.png)

1. Cuando haya terminado, seleccione **Crear**. Implementar el modelo puede tardar varios minutos para cada canalización terminen de ejecutarse.

1. Eso es todo. Tiene un servicio web operational para generar predicciones.

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre aprendizaje automático automatizadas](concept-automated-ml.md) y Azure Machine Learning.
* [Obtenga información sobre cómo consumir un servicio web](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service).
