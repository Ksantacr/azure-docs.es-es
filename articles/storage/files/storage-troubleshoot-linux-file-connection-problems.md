---
title: Solución de problemas de Azure Files en Linux | Microsoft Docs
description: Solución de problemas de Azure File en Linux
services: storage
author: jeffpatt24
tags: storage
ms.service: storage
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 06b3a5110bfdea2a2067979c806701011dc16f3d
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65987688"
---
# <a name="troubleshoot-azure-files-problems-in-linux"></a>Solución de problemas de Azure File en Linux

En este artículo se enumeran los problemas habituales relacionados con Azure Files cuando se conecta desde clientes Linux. También se proporcionan posibles causas de estos problemas y sus resoluciones. 

Además de los pasos de solución de problemas de este artículo, también puede usar [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089) para asegurarse de que el cliente de Linux cumple los requisitos previos. AzFileDiagnostics automatiza la detección de la mayoría de los síntomas que se mencionan en este artículo. Le ayuda a configurar su entorno para obtener un rendimiento óptimo. También puede encontrar esta información en el [solucionador de problemas de recursos compartidos de Azure Files](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares). El solucionador de problemas proporciona pasos para ayudarle con problemas de conexión, asignación y montaje de recursos compartidos de Azure Files.

<a id="mounterror13"></a>
## <a name="mount-error13-permission-denied-when-you-mount-an-azure-file-share"></a>"Error de montaje(13): Permiso denegado" al montar un recurso compartido de archivos de Azure

### <a name="cause-1-unencrypted-communication-channel"></a>Causa 1: Canal de comunicación sin cifrar

Por seguridad, se bloquean las conexiones a recursos compartidos de archivos de Azure si no el canal de comunicación no está cifrado y si el intento de conexión no se realiza desde el mismo centro de datos donde residen los recursos compartidos de archivos de Azure. Las conexiones no cifradas del mismo centro de datos también se pueden bloquear si la configuración de [transferencia segura requerida](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) está habilitada en la cuenta de almacenamiento. Se proporciona un canal de comunicación cifrado solamente si el sistema operativo cliente del usuario admite el cifrado SMB.

Para más información, consulte [Requisitos previos para el montaje de un recurso compartido de archivos de Azure con Linux y el paquete cifs-utils](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-linux#prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package). 

### <a name="solution-for-cause-1"></a>Solución para la causa 1

1. Conéctese desde un cliente que admita el cifrado SMB o bien desde una máquina virtual que se encuentre en el mismo centro de datos que la cuenta de Azure Storage que se utilice para el recurso compartido de archivos de Azure.
2. Compruebe que la configuración de la [transferencia segura requerida](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) esté deshabilitada en la cuenta de almacenamiento si el cliente no admite el cifrado SMB.

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>Causa 2: Las reglas de firewall o de red virtuales están habilitadas en la cuenta de almacenamiento 

Si las reglas de red virtual (VNET) y de firewall están configuradas en la cuenta de almacenamiento, se denegará el acceso al tráfico de red a menos que la dirección IP del cliente o la red virtual tengan permitido el acceso.

### <a name="solution-for-cause-2"></a>Solución para la causa 2

Compruebe que las reglas de firewall y de red virtual están configuradas correctamente en la cuenta de almacenamiento. Para probar si las reglas de firewall o de red virtual están causando el problema, cambie temporalmente la configuración de la cuenta de almacenamiento para **Permitir el acceso desde todas las redes**. Para más información, consulte [Configuración de redes virtuales y firewalls de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-network-security).

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>"[permiso denegado] Cuota de disco superada" al intentar abrir un archivo

En Linux, recibe un mensaje de error similar al siguiente:

**\<nombre de archivo > superada la cuota de disco [permiso denegado]**

### <a name="cause"></a>Causa

Se ha alcanzado el límite superior de identificadores abiertos simultáneos permitidos para un archivo.

### <a name="solution"></a>Solución

Reduzca el número de identificadores abiertos simultáneos cerrando algunos de ellos y, después, vuelva a realizar la operación. Para más información, consulte [Lista de comprobación de rendimiento y escalabilidad de Microsoft Azure Storage](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Copia de archivos lenta en y desde Azure Files en Linux

- Si no tiene un requisito mínimo de tamaño de E/S específico, se recomienda que use 1 MiB como tamaño de E/S para disfrutar de un rendimiento óptimo.
- Si conoce el tamaño final de un archivo que amplía mediante operaciones de escritura y el software no presenta problemas de compatibilidad cuando una cola no escrita del archivo contiene ceros, establezca el tamaño de archivo con antelación en lugar de hacer que cada escritura sea una escritura de ampliación.
- Utilice el método de copia correcto:
    - Use [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) para todas las transferencias entre dos recursos compartidos de archivos.
    - Utilice [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) entre recursos compartidos de archivos y un equipo local.

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>"Error de montaje(112): El host está apagado" debido a un tiempo de espera de reconexión

Se produce un error de montaje "112" en el cliente de Linux cuando este lleva inactivo mucho tiempo. Después del tiempo de inactividad extendido, el cliente se desconecta y se agota el tiempo de espera de la conexión.  

### <a name="cause"></a>Causa

La conexión puede estar inactiva por las razones siguientes:

-   Errores de comunicación de la red que impiden el restablecimiento de una conexión TCP con el servidor cuando se usa la opción de montaje predeterminada "flexible"
-   Correcciones de reconexión recientes que no están presentes en los kernels anteriores

### <a name="solution"></a>Solución

Este problema de reconexión en el kernel de Linux se ha corregido como parte de los siguientes cambios:

- [Fix reconnect to not defer smb3 session reconnect long after socket reconnect](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93) (Corregir la reconexión para que no aplace la reconexión de la sesión de smb3 mucho después de la reconexión del socket)
- [Call echo service immediately after socket reconnect](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7) (Llamar al servicio de eco inmediatamente después de volver a conectar el socket)
- [CIFS: Fix a possible memory corruption during reconnect](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b) (CIFS: Corregir un posible daño de memoria durante la reconexión)
- [CIFS: Fix a possible double locking of mutex during reconnect (or kernel v4.9 and later)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183) (CIFS: Corregir un posible bloqueo doble de exclusión mutua durante la reconexión: para el kernel v4.9 y versiones posteriores)

Pero es posible que estos cambios todavía no se migren a todas las distribuciones de Linux. Esta corrección y otras correcciones de reconexión se realizan en los siguientes kernels de Linux conocidos: 4.4.40, 4.8.16 y 4.9.1. Puede obtener esta corrección actualizando a una de estas versiones de kernel recomendadas.

### <a name="workaround"></a>Solución alternativa

Una solución alternativa para este problema es especificar un montaje forzado. Un montaje forzado obliga al cliente a esperar hasta que se establece una conexión o hasta que se interrumpe explícitamente. Puede usarlo para evitar errores provocados por los tiempos de espera de la red. Sin embargo, esta solución alternativa puede provocar esperas indefinidas. Esté preparado para detener las conexiones según sea necesario.

Si no puede actualizar a las versiones más recientes del kernel, puede solucionar este problema manteniendo un archivo en el recurso compartido de archivos de Azure en el que se escribe cada 30 segundos o menos. Esta debe ser una operación de escritura, como volver a escribir la fecha de creación o modificación en el archivo. De lo contrario, podría obtener resultados almacenados en caché y la operación podría no desencadenar la reconexión.

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-files-by-using-smb-30"></a>"Error de montaje(115): Operación en curso" cuando monta Azure Files mediante SMB 3.0

### <a name="cause"></a>Causa

Algunas distribuciones de Linux aún no admiten características de cifrado de SMB 3.0. Los usuarios pueden recibir un mensaje de error 115 al tratar de montar Azure Files mediante SMB 3.0 porque falta una característica. SMB 3.0 con cifrado completo solo se admite cuando se usa Ubuntu 16.04 o una versión posterior.

### <a name="solution"></a>Solución

La característica de cifrado de SMB 3.0 para Linux se introdujo en el kernel 4.11. Esta característica permite montar un recurso compartido de archivos de Azure desde el entorno local o una región distinta de Azure. En el momento de la publicación, esta funcionalidad se ha usado en Ubuntu 17.04 y Ubuntu 16.10. 

Si el cliente de SMB de Linux no admite el cifrado, monte Azure Files con SMB 2.1 desde una máquina virtual Linux de Azure que se encuentre en el mismo centro de datos que el recurso de archivos. Compruebe que el ajuste [Se requiere transferencia segura]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) esté deshabilitado en la cuenta de almacenamiento. 

<a id="accessdeniedportal"></a>
## <a name="error-access-denied-when-browsing-to-an-azure-file-share-in-the-portal"></a>Error "Acceso denegado" al ir a un recurso compartido de archivos de Azure en el portal

Cuando examine un recurso compartido de archivos de Azure en el portal, puede recibir el siguiente error:

Acceso denegado  
No tiene acceso  
Parece que no tiene acceso a este contenido. Para obtener acceso, póngase en contacto con el propietario.  

### <a name="cause-1-your-user-account-does-not-have-access-to-the-storage-account"></a>Causa 1: Su cuenta de usuario no tiene acceso a la cuenta de almacenamiento

### <a name="solution-for-cause-1"></a>Solución para la causa 1

Vaya a la cuenta de almacenamiento donde se encuentra el recurso compartido de archivos de Azure, haga clic en **Control de acceso (IAM)** y compruebe que su cuenta de usuario tiene acceso a la cuenta de almacenamiento. Para más información, consulte [Protección de su cuenta de almacenamiento con el control de acceso basado en rol (RBAC)](https://docs.microsoft.com/azure/storage/common/storage-security-guide#how-to-secure-your-storage-account-with-role-based-access-control-rbac).

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>Causa 2: Las reglas de firewall o de red virtuales están habilitadas en la cuenta de almacenamiento

### <a name="solution-for-cause-2"></a>Solución para la causa 2

Compruebe que las reglas de firewall y de red virtual están configuradas correctamente en la cuenta de almacenamiento. Para probar si las reglas de firewall o de red virtual están causando el problema, cambie temporalmente la configuración de la cuenta de almacenamiento para **Permitir el acceso desde todas las redes**. Para más información, consulte [Configuración de redes virtuales y firewalls de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-network-security).

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Rendimiento lento en un recurso compartido de archivos de Azure montado en una VM de Linux

### <a name="cause"></a>Causa

Una posible causa de un rendimiento lento es que el almacenamiento en caché está deshabilitado.

### <a name="solution"></a>Solución

Para comprobar si el almacenamiento en caché está deshabilitado, busque la entrada **cache=**. 

**Cache=none** indica que el almacenamiento en caché está deshabilitado. Vuelva a montar el recurso compartido mediante el comando de montaje predeterminado o agregando explícitamente la opción **cache=strict** al comando de montaje con el fin de asegurarse de que el almacenamiento en caché predeterminado o el modo de almacenamiento en caché "strict" esté habilitado.

En algunos escenarios, la opción de montaje **serverino** puede hacer que el comando **ls** se ejecute en cada entrada de directorio. Este comportamiento da como resultado una degradación del rendimiento cuando se muestra un listado de un directorio grande. Puede comprobar las opciones de montaje en la entrada **/etc/fstab**:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=2.1,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

También puede comprobar si se usan las opciones correctas ejecutando el comando **sudo mount | grep cifs** y comprobando su salida. A continuación se incluye el resultado de ejemplo:

```
//azureuser.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=2.1,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)
```

Si no está presente la opción **cache=strict** o **serverino**, desmonte y vuelva a montar Azure Files ejecutando el comando de montaje desde la [documentación](../storage-how-to-use-files-linux.md). A continuación, vuelva a comprobar que la entrada **/etc/fstab** tiene las opciones correctas.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>Se perdieron las marcas de tiempo al copiar archivos de Windows a Linux

En las plataformas Unix y Linux, el comando **cp -p** produce un error si los archivos 1 y 2 pertenecen a distintos usuarios.

### <a name="cause"></a>Causa

La marca para exigir **f** en COPYFILE da como resultado la ejecución de **cp -p -f** en Unix. Este comando tampoco conserva la marca de tiempo del archivo que no el usuario posee.

### <a name="workaround"></a>Solución alternativa

Utilice el usuario de la cuenta de almacenamiento para copiar los archivos:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="cannot-connect-to-or-mount-an-azure-file-share"></a>No se puede conectar a un recurso compartido de archivos de Azure ni montarlo

### <a name="cause"></a>Causa

Las causas comunes de este problema son las siguientes:


- Está usando un cliente de distribución de Linux incompatible. Se recomienda usar las siguientes distribuciones de Linux para conectarse al recurso compartido de archivos de Azure:

    |   | SMB 2.1 <br>(Se monta en máquinas virtuales dentro de la misma región de Azure) | SMB 3.0 <br>(Puede montar desde el nivel local a entre regiones) |
    | --- | :---: | :---: |
    | Ubuntu Server | 14.04+ | 16.04 (o posterior) |
    | RHEL | 7 (o posterior) | 7.5+ |
    | CentOS | 7 (o posterior) |  7.5+ |
    | Debian | 8+ |   |
    | openSUSE | 13.2 (o posterior) | 42.3 (o posterior) |
    | SUSE Linux Enterprise Server | 12 | 12 SP3 (o posterior) |

- Las utilidades de CIFS (cfs-utils) no están instaladas en el cliente.
- La versión 2.1 mínima de SMB/CIFS no está instalada en el cliente.
- No se admite el cifrado SMB 3.0 en el cliente. El cifrado SMB 3.0 está disponible en Ubuntu 16.4 y versiones posteriores y en SUSE 12.3 y versiones posteriores. Otras distribuciones requieren kernel 4.11 y versiones posteriores.
- Está intentando conectarse a una cuenta de almacenamiento a través del puerto TCP 445 que no es compatible.
- Está intentando conectarse al recurso compartido de archivos de Azure desde una máquina virtual de Azure y la máquina virtual no se encuentra en la misma región que la cuenta de almacenamiento.
- Si la opción [Se requiere transferencia segura]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) está habilitada para la cuenta de almacenamiento, Azure Files solo permitirá las conexiones que usen SMB 3.0 con cifrado.

### <a name="solution"></a>Solución

Para resolver el problema, use la [herramienta de solución de problemas para los errores de montaje de Azure Files en Linux](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089). Esta herramienta:

* Le ayuda a validar el cliente que ejecuta el entorno.
* Detecta la configuración de cliente incompatible que podría provocar un error de acceso para Azure Files.
* Ofrece instrucciones prescriptivas para la corrección automática.
* Recopila los seguimientos de diagnóstico.

## <a name="ls-cannot-access-ltpathgt-inputoutput-error"></a>Es: No se puede acceder a '&lt;ruta de acceso&gt;': Error de entrada/salida

Cuando se intenta mostrar los archivos en un recurso compartido de archivos de Azure mediante el comando ls, este comando se bloquea al enumerar los archivos. Verá este error:

**Es: no se puede acceder a '&lt;ruta de acceso&gt;': Error de entrada/salida**


### <a name="solution"></a>Solución
Actualice el kernel de Linux a las siguientes versiones que tengan una solución para este problema:

- 4.4.87+
- 4.9.48+
- 4.12.11+
- Todas las versiones que sean mayores o iguales que 4.13

## <a name="cannot-create-symbolic-links---ln-failed-to-create-symbolic-link-t-operation-not-supported"></a>No se pueden crear vínculos simbólicos. En: No se pudo crear un vínculo simbólico "t": Operación no admitida

### <a name="cause"></a>Causa
De forma predeterminada, el montaje de recursos compartidos de archivos de Azure en Linux mediante CIFS no permite la compatibilidad con vínculos simbólicos (symlinks). Verá un error como este:
```
ln -s linked -n t
ln: failed to create symbolic link 't': Operation not supported
```
### <a name="solution"></a>Solución
El cliente CIFS de Linux no admite la creación de vínculos simbólicos de estilo Windows mediante el protocolo SMB 2 o 3. Actualmente, el cliente Linux admite otro estilo de vínculos simbólicos llamados [Mishall+French](https://wiki.samba.org/index.php/UNIX_Extensions#Minshall.2BFrench_symlinks) para operaciones de creación y seguimiento. Los clientes que necesitan vínculos simbólicos pueden usar la opción de montaje "mfsymlinks". Se recomienda "mfsymlinks", porque también es el formato que usan equipos Mac.

Para poder usar symlinks, agregue el siguiente código al final de su comando de montaje de CIFS:

```
,mfsymlinks
```

El comando tendrá un aspecto similar al siguiente:

```
sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino,mfsymlinks
```

A continuación, puede crear symlinks como se sugiere en la [wiki](https://wiki.samba.org/index.php/UNIX_Extensions#Storing_symlinks_on_Windows_servers).

[!INCLUDE [storage-files-condition-headers](../../../includes/storage-files-condition-headers.md)]

## <a name="need-help-contact-support"></a>¿Ayuda? Póngase en contacto con el servicio de soporte técnico.

Si sigue necesitando ayuda, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
