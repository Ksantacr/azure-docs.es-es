---
title: Configuración de la conectividad SSL para conectarse de forma segura a Azure Database for MariaDB
description: Instrucciones para configurar correctamente Azure Database for MariaDB y las aplicaciones asociadas para que utilicen correctamente conexiones SSL
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: 6de16b7264c7ae7ead06b4e131e7fa46c664cedd
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64573340"
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mariadb"></a>Configuración de la conectividad SSL en la aplicación para conectarse de forma segura a Azure Database for MariaDB
Azure Database for MariaDB permite conectar el servidor de Azure Database for MariaDB con aplicaciones cliente mediante Capa de sockets seguros (SSL). Aplicar conexiones SSL entre el servidor de base de datos y las aplicaciones cliente ayuda a proteger contra los ataques de tipo "man in the middle" mediante el cifrado del flujo de datos entre el servidor y la aplicación.

## <a name="obtain-ssl-certificate"></a>Obtención de un certificado SSL
Descargue de [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) el certificado necesario para comunicarse a través de SSL con el servidor de Azure Database for MariaDB y guarde el archivo de certificado en la unidad local (por ejemplo, en este tutorial se usa c:\ssl).
**Para Microsoft Internet Explorer y Microsoft Edge:** una vez finalizada la descarga, cambie el nombre del certificado a BaltimoreCyberTrustRoot.crt.pem.

## <a name="bind-ssl"></a>Enlace de SSL
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>Conexión al servidor mediante MySQL Workbench a través de SSL
Configure MySQL Workbench para conectarse de forma segura a través de SSL. En el cuadro de diálogo para configurar una conexión nueva, vaya a la pestaña **SSL**. En el campo **Archivo CA SSL**, escriba la ubicación del archivo **BaltimoreCyberTrustRoot.crt.pem**. 
![icono personalizado para guardar](./media/howto-configure-ssl/mysql-workbench-ssl.png) En el caso de las conexiones existentes, puede enlazar SSL haciendo clic con el botón derecho en el icono de conexión y eligiendo Editar. A continuación, navegue hasta la pestaña **SSL** y enlace el archivo de certificado.

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>Conexión al servidor mediante la CLI de MySQL a través de SSL
Otra forma de enlazar el certificado SSL es usar la interfaz de la línea de comandos de MySQL ejecutando los siguientes comandos. 

```bash
mysql.exe -h mydemoserver.mariadb.database.azure.com -u Username@mydemoserver -p --ssl-mode=REQUIRED --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

> [!NOTE]
> Es posible que, cuando utilice la interfaz de la línea de comandos de MySQL en Windows, aparezca un error `SSL connection error: Certificate signature check failed`. Si esto sucede, reemplace los parámetros `--ssl-mode=REQUIRED --ssl-ca={filepath}` por `--ssl`.

## <a name="enforcing-ssl-connections-in-azure"></a>Aplicación de conexiones SSL en Azure 
### <a name="using-the-azure-portal"></a>Uso de Azure Portal
Mediante Azure Portal, vaya al servidor de Azure Database for MariaDB y haga clic en **Seguridad de conexión**. Use el botón de alternancia para habilitar o deshabilitar la opción **Aplicar conexión SSL** y después haga clic en **Guardar**. Microsoft recomienda habilitar siempre la opción **Aplicar conexión SSL** para mejorar la seguridad.
![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Uso de la CLI de Azure
Puede habilitar o deshabilitar el parámetro **ssl-enforcement** con los valores Enabled o Disabled, respectivamente, en CLI de Azure.
```azurecli-interactive
az mariadb server update --resource-group myresource --name mydemoserver --ssl-enforcement Enabled
```

## <a name="verify-the-ssl-connection"></a>Comprobación de la conexión SSL
Ejecute el comando **status** de mysql para comprobar que se ha conectado al servidor de MariaDB mediante SSL:
```sql
status
```
Confirme que la conexión está cifrada revisando la salida, que debería mostrar:  **SSL: El cifrado en uso es AES256-SHA** 

## <a name="sample-code"></a>Código de ejemplo
Para establecer una conexión segura a Azure Database for MariaDB a través de SSL desde la aplicación, consulte los siguientes ejemplos de código:

### <a name="php"></a>PHP
```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'mydemoserver.mariadb.database.azure.com', 'myadmin@mydemoserver', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python-mysqlconnector-python"></a>Python (MySQLConnector Python)
```python
try:
    conn=mysql.connector.connect(user='myadmin@mydemoserver', 
        password='yourpassword', 
        database='quickstartdb', 
        host='mydemoserver.mariadb.database.azure.com', 
        ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
    print(err)
```
### <a name="python-pymysql"></a>Python (PyMySQL)
```python
conn = pymysql.connect(user = 'myadmin@mydemoserver', 
        password = 'yourpassword', 
        database = 'quickstartdb', 
        host = 'mydemoserver.mariadb.database.azure.com', 
        ssl = {'ssl': {'ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}})
```
### <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(
        :host     => 'mydemoserver.mariadb.database.azure.com', 
        :username => 'myadmin@mydemoserver',      
        :password => 'yourpassword',    
        :database => 'quickstartdb',
        :ssl_ca => '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'
    )
```
### <a name="golang"></a>Golang
```go
rootCertPool := x509.NewCertPool()
pem, _ := ioutil.ReadFile("/var/www/html/BaltimoreCyberTrustRoot.crt.pem")
if ok := rootCertPool.AppendCertsFromPEM(pem); !ok {
    log.Fatal("Failed to append PEM.")
}
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin@mydemoserver' , 'yourpassword', 'mydemoserver.mariadb.database.azure.com', 'quickstartdb') 
db, _ := sql.Open("mysql", connectionString)
```
### <a name="java-jdbc"></a>Java (JDBC)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'mydemoserver.mariadb.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```
### <a name="java-mariadb"></a>Java (MariaDB)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'mydemoserver.mariadb.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

### <a name="net-mysqlconnector"></a>.NET (MySqlConnector)
```csharp
var builder = new MySqlConnectionStringBuilder
{
    Server = "mydemoserver.mysql.database.azure.com",
    UserID = "myadmin@mydemoserver",
    Password = "yourpassword",
    Database = "quickstartdb",
    SslMode = MySqlSslMode.VerifyCA,
    CACertificateFile = "BaltimoreCyberTrustRoot.crt.pem",
};
using (var connection = new MySqlConnection(builder.ConnectionString))
{
    connection.Open();
}
```

<!-- 
## Next steps
Review various application connectivity options following [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md)
-->
