---
title: 'Identificación de escenarios de Azure Machine Learning: proceso de ciencia de datos en equipos'
description: Seleccione los escenarios adecuados para el proceso de análisis predictivo avanzado con el proceso de ciencia de datos en equipos.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 6838f4db240a0712eece7a97bc2cfe99efb87215
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61429782"
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Escenarios para análisis avanzado en Azure Machine Learning
En este artículo se describen los distintos escenarios de origen y destino de datos de ejemplo que se pueden administrar con el [proceso de ciencia de datos en equipos (TDSP)](overview.md). El TDSP proporciona un enfoque sistemático a los equipos que colaboran en la compilación de aplicaciones inteligentes. Los escenarios que se exponen aquí muestran las opciones disponibles en el flujo de trabajo de procesamiento de datos en función de las características de datos, las ubicaciones de origen y los repositorios de destino de Azure.

En la última sección se presenta el **árbol de decisión** para seleccionar los escenarios de ejemplo adecuados para los datos y el objetivo.

> [!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
> 
> 

Cada una de las secciones siguientes presenta un escenario de ejemplo. Para cada escenario, se enumera un posible flujo de ciencia de datos y análisis avanzado, así como los recursos de compatibilidad de Azure.

> [!NOTE]
> **Para todos los escenarios siguientes, debe:**
> <br/>
> 
> * [Cree una cuenta de almacenamiento](../../storage/common/storage-quickstart-create-account.md)
>   <br/>
> * [Creación de un área de trabajo de Azure Machine Learning](../studio/create-workspace.md)
> 
> 

## <a name="smalllocal"></a>Escenario \#1: Conjunto de datos tabular de tamaño pequeño a medio de archivos locales
![Archivos locales de tamaño pequeño a medio][1]

#### <a name="additional-azure-resources-none"></a>Recursos adicionales de Azure: None
1. Inicie sesión en [Azure Machine Learning Studio](https://studio.azureml.net/).
1. Cargue un conjunto de datos.
1. Cree un flujo de experimento de Azure Machine Learning comenzando con conjuntos de datos cargados.

## <a name="smalllocalprocess"></a>Escenario \#2: Conjunto de datos de tamaño pequeño a medio de archivos locales que requieren procesamiento
![Archivos locales de tamaño pequeño a medio con procesamiento][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Recursos adicionales de Azure: Azure Virtual Machine (servidor IPython Notebook)
1. Cree una máquina virtual de Azure que ejecute IPython Notebook.
1. Cargue datos en un contenedor de almacenamiento de Azure.
1. Preprocese y limpie datos en IPython Notebook obteniendo acceso desde el contenedor de almacenamiento de Azure.
1. Transforme datos para un formulario limpio y tabular.
1. Guarde los datos transformados en blobs de Azure.
1. Inicie sesión en [Azure Machine Learning Studio](https://studio.azureml.net/).
1. Lea los datos de blobs de Azure mediante el módulo [Importar datos][import-data].
1. Cree un flujo de experimento de Azure Machine Learning comenzando con conjuntos de datos introducidos.

## <a name="largelocal"></a>Escenario \#3: Conjunto de datos grande de archivos locales, con blobs de Azure como destino
![Archivos locales grandes][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Recursos adicionales de Azure: Azure Virtual Machine (servidor IPython Notebook)
1. Cree una máquina virtual de Azure que ejecute IPython Notebook.
1. Cargue datos en un contenedor de almacenamiento de Azure.
1. Preprocese y limpie datos en IPython Notebook obteniendo acceso desde blobs de Azure.
1. Transforme datos en un formulario limpio y tabular si es necesario.
1. Explore datos y cree características según sea necesario.
1. Extraiga una muestra de datos de tamaño pequeño a medio.
1. Guarde los datos de muestra en blobs de Azure.
1. Inicie sesión en [Azure Machine Learning Studio](https://studio.azureml.net/).
1. Lea los datos de blobs de Azure mediante el módulo [Importar datos][import-data].
1. Cree un flujo de experimento de Azure Machine Learning comenzando con conjuntos de datos ingeridos.

## <a name="smalllocaltodb"></a>Escenario \#4: Conjunto de datos de tamaño pequeño a medio de archivos locales, con SQL Server en una máquina virtual de Azure como destino
![Archivos locales de tamaño pequeño a medio a Base de datos SQL de Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Recursos adicionales de Azure: Azure Virtual Machine (servidor ISQL Server/IPython Notebook)
1. Cree una máquina virtual de Azure que ejecute SQL Server + IPython Notebook.
1. Cargue datos en un contenedor de almacenamiento de Azure.
1. Preprocese y limpie datos en el contenedor de almacenamiento de Azure usando IPython Notebook.
1. Transforme datos en un formulario limpio y tabular si es necesario.
1. Guarde datos en archivos locales de máquina virtual (IPython Notebook se ejecuta en una máquina virtual; las unidades locales hacen referencia a unidades de máquina virtual).
1. Cargue datos en la base de datos de SQL Server que se ejecuta en una máquina virtual de Azure.
   
   Opción \#1: Uso de SQL Server Management Studio.
   
   * Inicie sesión en la máquina virtual de SQL Server.
   * Ejecute SQL Server Management Studio.
   * Cree tablas de base de datos y de destino.
   * Use uno de los métodos de importación en bloque para cargar los datos desde archivos locales de máquina virtual.
   
   Opción \#2: Mediante IPython Notebook (no se recomienda para conjuntos de datos de tamaño medio o más grandes)
   
   <!-- -->    
   * Use la cadena de conexión ODBC para obtener acceso a SQL Server en la máquina virtual.
   * Cree tablas de base de datos y de destino.
   * Use uno de los métodos de importación en bloque para cargar los datos desde archivos locales de máquina virtual.
1. Explore datos y cree características según sea necesario. Tenga en cuenta que las características no necesitan materializarse en las tablas de base de datos. Solo tenga en cuenta la consulta necesaria para crearlas.
1. Elija un tamaño de muestra de datos, si lo necesita y/o desea.
1. Inicie sesión en [Azure Machine Learning Studio](https://studio.azureml.net/).
1. Lea los datos directamente desde SQL Server mediante el módulo [Importar datos][import-data]. Pegue la consulta necesaria que extrae los campos, crea características y toma muestras de datos si es necesario directamente en la consulta [Importar datos][import-data].
1. Cree un flujo de experimento de Azure Machine Learning comenzando con conjuntos de datos ingeridos.

## <a name="largelocaltodb"></a>Escenario \#5: Conjunto de datos grande de archivos locales, con SQL Server en una máquina virtual de Azure como destino
![Archivos locales grandes a Base de datos SQL de Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Recursos adicionales de Azure: Azure Virtual Machine (servidor ISQL Server/IPython Notebook)
1. Cree una máquina virtual de Azure que ejecute SQL Server y el servidor IPython Notebook.
1. Cargue datos en un contenedor de almacenamiento de Azure.
1. (Opcional) Preprocese y limpie los datos.
   
    a.  Preprocese y limpie datos en IPython Notebook obteniendo acceso desde Azure
   
       blobs.
   
   b.  Transforme datos en un formulario limpio y tabular si es necesario.
   
   c.  Guarde datos en archivos locales de máquina virtual (IPython Notebook se ejecuta en una máquina virtual; las unidades locales hacen referencia a unidades de máquina virtual).
1. Cargue datos en la base de datos de SQL Server que se ejecuta en una máquina virtual de Azure.
   
    a.  Inicie sesión en la máquina virtual de SQL Server.
   
   b.  Si los datos todavía no están guardados, descargue los archivos de datos desde Azure
   
       storage container to local-VM folder.
   
   c.  Ejecute SQL Server Management Studio.
   
   d.  Cree tablas de base de datos y de destino.
   
   e.  Use uno de los métodos de importación en masa para cargar los datos.
   
   f.  Si se requieren combinaciones de tablas, cree índices para acelerar dichas combinaciones.
   
   > [!NOTE]
   > Para acelerar la carga de tamaños de datos de gran volumen, es recomendable crear tablas con particiones e importar en masa los datos en paralelo. Para obtener más información, vea [Importación de datos en paralelo a tablas con particiones de SQL](parallel-load-sql-partitioned-tables.md).
   > 
   > 
1. Explore datos y cree características según sea necesario. Tenga en cuenta que las características no necesitan materializarse en las tablas de base de datos. Solo tenga en cuenta la consulta necesaria para crearlas.
1. Elija un tamaño de muestra de datos, si lo necesita y/o desea.
1. Inicie sesión en [Azure Machine Learning Studio](https://studio.azureml.net/).
1. Lea los datos directamente desde SQL Server mediante el módulo [Importar datos][import-data]. Pegue la consulta necesaria que extrae los campos, crea características y toma muestras de datos si es necesario directamente en la consulta [Importar datos][import-data].
1. Flujo de experimento de Azure Machine Learning comenzando con conjuntos de datos cargados

## <a name="largedbtodb"></a>Escenario \#6: Conjunto de datos grande en una base de datos de SQL Server local, como el destino de SQL Server en una máquina Virtual de Azure
![Base de datos SQL local a Base be datos SQL de Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Recursos adicionales de Azure: Azure Virtual Machine (servidor ISQL Server/IPython Notebook)
1. Cree una máquina virtual de Azure que ejecute SQL Server y el servidor IPython Notebook.
1. Use uno de los métodos de exportación de datos para exportar los datos desde SQL Server a archivos de volcado de memoria.
   
   > [!NOTE]
   > Si decide mover todos los datos de la base de datos local, un método alternativo (más rápido) para mover la base de datos completa a la instancia de SQL Server en Azure. Omita los pasos para exportar datos, crear la base de datos y cargar e importar datos a la base de datos de destino y siga el método alternativo.
   > 
   > 
1. Cargue archivos de volcado de memoria en el contenedor de almacenamiento de Azure.
1. Cargue los datos en una base de datos de SQL Server que se ejecute en una máquina virtual de Azure.
   
    a.  Inicie sesión en la máquina virtual de SQL Server.
   
   b.  Descargue los archivos de datos desde un contenedor de almacenamiento de Azure a la carpeta de la máquina virtual local.
   
   c.  Ejecute SQL Server Management Studio.
   
   d.  Cree tablas de base de datos y de destino.
   
   e.  Use uno de los métodos de importación en masa para cargar los datos.
   
   f.  Si se requieren combinaciones de tablas, cree índices para acelerar dichas combinaciones.
   
   > [!NOTE]
   > Para acelerar la carga de tamaños de datos de gran tamaño, cree tablas con particiones e importe en masa los datos en paralelo. Para obtener más información, vea [Importación de datos en paralelo a tablas con particiones de SQL](parallel-load-sql-partitioned-tables.md).
   > 
   > 
1. Explore datos y cree características según sea necesario. Tenga en cuenta que las características no necesitan materializarse en las tablas de base de datos. Solo tenga en cuenta la consulta necesaria para crearlas.
1. Elija un tamaño de muestra de datos, si lo necesita y/o desea.
1. Inicie sesión en [Azure Machine Learning Studio](https://studio.azureml.net/).
1. Lea los datos directamente desde SQL Server mediante el módulo [Importar datos][import-data]. Pegue la consulta necesaria que extrae los campos, crea características y toma muestras de datos si es necesario directamente en la consulta [Importar datos][import-data].
1. Flujo de experimento de Azure Machine Learning comenzando con conjuntos de datos cargados.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Método alternativo para copiar una base de datos completa desde un servidor SQL Server local a Azure SQL Database
![Desasociación de base de datos local y asociación a Base de datos SQL de Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Recursos adicionales de Azure: Azure Virtual Machine (servidor ISQL Server/IPython Notebook)
Para replicar toda la base de datos de SQL Server en la máquina virtual de SQL Server, debe copiar una base de datos desde una ubicación o  un servidor a otro, suponiendo que la base de datos se puede desconectar temporalmente. Esto se realiza en el Explorador de objetos de SQL Server Management Studio o mediante los comandos de Transact-SQL equivalentes.

1. Desasocie la base de datos en la ubicación de origen. Para más información, consulte [Desasociar una base de datos](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).
1. En el Explorador de Windows o en la ventana del símbolo del sistema de Windows, copie los archivos de base de datos desasociados o los archivos de registro a la ubicación de destino en la máquina virtual de SQL Server en Azure.
1. Asocie los archivos copiados a la instancia de SQL Server de destino. Para más información, consulte [Asociar una base de datos](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).

[Mover una base de datos mediante las opciones desasociación y asociación (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)

## <a name="largedbtohive"></a>Escenario \#7: Big Data de archivos locales, con base de datos de Hive como destino en clústeres de Azure HDInsight Hadoop
![Datos de gran tamaño en Hive de destino local][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Recursos adicionales de Azure: Clúster de Azure HDInsight Hadoop y máquina virtual de Azure (servidor IPython Notebook)
1. Cree una máquina virtual de Azure que ejecute el servidor IPython Notebook.
1. Cree un clúster de Hadoop de HDInsight de Azure.
1. (Opcional) Preprocese y limpie los datos.
   
    a.  Preprocese y limpie datos en IPython Notebook obteniendo acceso desde Azure
   
       blobs.
   
   b.  Transforme datos en un formulario limpio y tabular si es necesario.
   
   c.  Guarde datos en archivos locales de máquina virtual (IPython Notebook se ejecuta en una máquina virtual; las unidades locales hacen referencia a unidades de máquina virtual).
1. Cargue datos en el contenedor predeterminado del clúster de Hadoop que seleccionó en el paso 2.
1. Cargue datos en la base de datos de Hive en el clúster de Hadoop de HDInsight de Azure.
   
    a.  Inicie sesión en el nodo principal del clúster de Hadoop.
   
   b.  Abra la línea de comandos de Hadoop.
   
   c.  Especifique el directorio raíz de Hive mediante el comando `cd %hive_home%\bin` en la línea de comandos de Hadoop.
   
   d.  Ejecute las consultas de Hive para crear bases de datos y tablas, y cargar datos desde el almacenamiento de blobs en tablas de Hive.
   
   > [!NOTE]
   > Si los datos son grandes, los usuarios pueden crear la tabla de Hive con particiones. A continuación, los usuarios pueden usar un bucle `for` en la línea de comandos de Hadoop en el nodo principal para cargar datos en la tabla de Hive con particiones por partición.
   > 
   > 
1. Explore datos y cree características según sea necesario en la línea de comandos de Hadoop. Tenga en cuenta que las características no necesitan materializarse en las tablas de base de datos. Solo tenga en cuenta la consulta necesaria para crearlas.
   
    a.  Inicie sesión en el nodo principal del clúster de Hadoop.
   
   b.  Abra la línea de comandos de Hadoop.
   
   c.  Especifique el directorio raíz de Hive mediante el comando `cd %hive_home%\bin` en la línea de comandos de Hadoop.
   
   d.  Ejecute las consultas de Hive en línea de comandos de Hadoop en el nodo principal del clúster de Hadoop para explorar los datos y crear características según sea necesario.
1. Si lo necesita o desea, muestre los datos para ajustarlos en Azure Machine Learning Studio.
1. Inicie sesión en [Azure Machine Learning Studio](https://studio.azureml.net/).
1. Lea los datos directamente desde `Hive Queries` mediante el módulo [Importar datos][import-data]. Pegue la consulta necesaria que extrae los campos, crea características y toma muestras de datos si es necesario directamente en la consulta [Importar datos][import-data].
1. Flujo de experimento de Azure Machine Learning comenzando con conjuntos de datos cargados.

## <a name="decisiontree"></a>Árbol de decisión de selección de escenario
- - -
El diagrama siguiente resume los escenarios descritos anteriormente y las opciones de la Tecnología y procesos de análisis avanzado elegidas que le guiarán a través de los escenarios detallados. Tenga en cuenta que el procesamiento de datos, la exploración, la ingeniería de características y el muestreo pueden tener lugar en uno o varios métodos o entornos (en los entornos de origen, intermedio o de destino) y pueden continuar de forma iterativa según sea necesario. El diagrama solo sirve para ilustrar algunos de los flujos de posibles y no proporciona una enumeración exhaustiva.

![Escenarios de tutoriales de proceso de ciencia de datos de ejemplo][8]

### <a name="advanced-analytics-in-action-examples"></a>Ejemplos de análisis avanzado en acción
Para los tutoriales de Azure Machine Learning completos que emplean la Tecnología y procesos de análisis avanzado mediante conjuntos de datos públicos, consulte:

* [Proceso de ciencia de datos en equipos en acción: uso de SQL Server](sql-walkthrough.md)
* [Proceso de ciencia de datos en equipos en acción: uso de clústeres de Hadoop de HDInsight](hive-walkthrough.md)

[1]: ./media/plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
