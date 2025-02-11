---
title: Configuración de PostgreSQL en una máquina virtual Linux | Microsoft Docs
description: Obtenga información acerca de cómo instalar y configurar PostgreSQL en una máquina virtual de Linux en Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 1a747363-0cc5-4ba3-9be7-084dfeb04651
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: cynthn
ms.openlocfilehash: 5fe5e819c4f1079b6eb1fa8bb19d337ecfed600d
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65955151"
---
# <a name="install-and-configure-postgresql-on-azure"></a>Instalación y configuración de PostgreSQL en Azure
PostgreSQL es una base de datos de código abierto avanzada similar a DB2 y Oracle. Incluye características preparadas para el ámbito empresarial, como el cumplimiento de ACID completo, un procesamiento transaccional confiable y un control de simultaneidad de múltiples versiones. También admite estándares como ANSI SQL y SQL/MED (incluidos los contenedores de datos externos para Oracle, MySQL, MongoDB y muchas otras). Es altamente extensible al admitir más de 12 lenguajes procedimentales, índices GIN y GiST, datos espaciales y múltiples características NoSQL para JSON o aplicaciones basadas en clave-valor.

En este artículo aprenderá a instalar y configurar PostgreSQL en una máquina virtual de Azure con Linux.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a>Instalación de PostgreSQL
> [!NOTE]
> Debe tener una máquina virtual de Azure en la que se ejecuta Linux para completar este tutorial. Para crear y configurar una máquina virtual Linux antes de continuar, consulte el [tutorial de máquinas virtuales Linux de Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

En este caso, use el puerto 1999 como puerto PostgreSQL.  

Conéctese a la máquina virtual Linux que creó mediante PuTTY. Si es la primera vez que usa una máquina virtual Linux de Azure, consulte [Utilización de SSH con Linux en Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para aprender cómo usar PuTTY para conectarse a una máquina virtual Linux.

1. Ejecute el siguiente comando para cambiar a la raíz (admin):
   
        # sudo su -
2. Algunas distribuciones tienen dependencias que se deben instalar antes de instalar PostgreSQL. Compruebe la distribución de esta lista y ejecute el comando apropiado:
   
   * Linux basado en Red Hat:
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * Linux basado en Debian:
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * Linux SUSE:
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. Descargue PostgreSQL en el directorio raíz y, a continuación, descomprima el paquete:
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    El código anterior es un ejemplo. Puede encontrar la dirección de descarga más detallada en el [Índice de /pub/source/](https://ftp.postgresql.org/pub/source/).
4. Para iniciar la compilación, ejecute estos comandos:
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. Si desea compilar todo lo que se puede compilar, incluida la documentación (páginas HTML y man) y módulos adicionales (contrib), ejecute el comando siguiente:
   
        # gmake install-world
   
    Obtendrá el siguiente mensaje de confirmación:
   
        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>Configuración de PostgreSQL
1. (Opcional) Cree un vínculo simbólico para acortar la referencia de PostgreSQL y que no incluya el número de versión:
   
        # ln -s /opt/postgresql-9.3.5 /opt/pgsql
2. Cree un directorio para la base de datos:
   
        # mkdir -p /opt/pgsql_data
3. Cree un usuario no raíz y modifique el perfil del usuario. A continuación, cambie a este nuevo usuario (denominado *postgres* en nuestro ejemplo):
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > Por motivos de seguridad, PostgreSQL utiliza un usuario no raíz para inicializar, iniciar o cerrar la base de datos.
   > 
   > 
4. Edite el archivo *bash_profile* mediante los comandos siguientes. Estas líneas se agregarán al final del archivo *bash_profile*:
   
        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF
5. Ejecute el archivo *bash_profile*:
   
        $ source .bash_profile
6. Valide la instalación con el siguiente comando:
   
        $ which psql
   
    Si la instalación se realiza correctamente, verá la siguiente respuesta:
   
        /opt/pgsql/bin/psql
7. También puede comprobar la versión de PostgreSQL:
   
        $ psql -V

8. Inicialice la base de datos:
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    Debe recibir los siguientes resultados:

![image](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>Configuración de PostgreSQL
<!--    [postgres@ test ~]$ exit -->

Ejecute los comandos siguientes:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Modifique dos variables en el archivo /etc/init.d/postgresql. El prefijo se establece en la ruta de instalación de PostgreSQL: **/opt/pgsql**. PGDATA se establece en la ruta de acceso de almacenamiento de datos de PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![image](./media/postgresql-install/no2.png)

Cambie el archivo para que sea ejecutable:

    # chmod +x /etc/init.d/postgresql

Inicie PostgreSQL:

    # /etc/init.d/postgresql start

Compruebe si el extremo de PostgreSQL está activado:

    # netstat -tunlp|grep 1999

Debería ver la siguiente salida:

![image](./media/postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Conexión a la base de datos de Postgres
Cambie de nuevo al usuario postgres:

    # su - postgres

Cree una base de datos de Postgres:

    $ createdb events

Conéctese a la base de datos events que acaba de crear:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Creación y eliminación de una tabla de Postgres
Ahora que nos hemos conectado a la base de datos, podemos crear tablas en ella.

Por ejemplo, cree una nueva tabla de Postgres de ejemplo con el siguiente comando:

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

Ahora hemos configurado una tabla de cuatro columnas con los siguientes nombres de columna y restricciones:

1. La longitud de la columna “name” está limitada por el comando VARCHAR a menos de 20 caracteres.
2. La columna “food” indica la comida que traerá cada persona. VARCHAR limita este texto a menos de 30 caracteres.
3. La columna “confirmed” registra si la persona ha confirmado la asistencia a la fiesta. Los valores aceptables son "Y" y "N".
4. Se muestra la columna “date” cuando se registren en el evento. Postgres requiere que las fechas se escriban como aaaa-mm-dd.

Verá lo siguiente si la tabla se ha creado correctamente:

![image](./media/postgresql-install/no4.png)

También puede comprobar la estructura de tabla con el comando siguiente:

![image](./media/postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Incorporación de datos a una tabla
En primer lugar, inserte información en una fila:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Debería ver este resultado:

![image](./media/postgresql-install/no6.png)

También puede agregar un par de personas más a la tabla. Estas son algunas opciones, o bien puede crear las suyas propias:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Visualización de tablas
Use el siguiente comando para mostrar una tabla:

    select * from potluck;

La salida es la siguiente:

![image](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Eliminación de datos de una tabla
Use el comando siguiente para eliminar datos de una tabla:

    delete from potluck where name=’John’;

Esto elimina toda la información de la fila "John". La salida es la siguiente:

![image](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Actualización de los datos de una tabla
Use el comando siguiente para actualizar los datos de una tabla: En esta ocasión, Sandy ha confirmado que asiste a, por lo que se cambiará la respuesta de "N" a "Y":

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a>Obtener más información sobre PostgreSQL
Ahora que ha completado la instalación de PostgreSQL en una máquina virtual Linux de Azure, puede disfrutar de su uso en Azure. Para obtener más información acerca de PostgreSQL, visite el [sitio web de PostgreSQL](https://www.postgresql.org/).

