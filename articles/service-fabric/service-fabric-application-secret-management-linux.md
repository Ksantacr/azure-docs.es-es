---
title: Configuración de certificado de cifrado y cifrado de secretos en clústeres Linux de Azure Service Fabric | Microsoft Docs
description: Obtenga información sobre cómo configurar un certificado de cifrado y cifrado de secretos en clústeres Linux.
services: service-fabric
documentationcenter: .net
author: shsha
manager: ''
editor: ''
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2019
ms.author: shsha
ms.openlocfilehash: 9589d6ea69a2293d592a9e63f2b726f1a620bb9e
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126995"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-linux-clusters"></a>Configuración de un certificado de cifrado y cifrado de secretos en clústeres Linux
En este artículo se muestra cómo configurar un certificado de cifrado y usarlo para cifrar secretos en clústeres Linux. Para los clústeres Windows, consulte [Configuración de un certificado de cifrado y cifrado de secretos en clústeres Windows][secret-management-windows-specific-link].

## <a name="obtain-a-data-encipherment-certificate"></a>Obtención de un certificado de cifrado de datos
Un certificado de cifrado de datos se utiliza estrictamente para cifrar y descifrar los [parámetros][parameters-link] de un archivo Settings.xml del servicio y las [variables de entorno][environment-variables-link] de un archivo ServiceManifest.xml del servicio. No se usa para la autenticación o la firma de texto cifrado. El certificado debe cumplir los siguientes requisitos:

* El certificado debe contener una clave privada.
* El uso de claves de certificado debe incluir el cifrado de datos (10), y no debe incluir la autenticación de servidor o la autenticación de cliente.

  Por ejemplo, se pueden usar los siguientes comandos para generar el certificado necesario con OpenSSL:
  
  ```console
  user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
  user@linux:~$ cat TestCert.prv >> TestCert.pem
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Instalación del certificado en el clúster
Este certificado debe instalarse en cada nodo del clúster en `/var/lib/sfcerts`. La cuenta de usuario en la que el servicio se está ejecutando (sfuser de forma predeterminada) **debe tener acceso de lectura** al certificado instalado (es decir, `/var/lib/sfcerts/TestCert.pem` en este ejemplo).

## <a name="encrypt-secrets"></a>Cifrado de secretos
El siguiente fragmento de código puede utilizarse para cifrar un secreto. Este fragmento de código solo cifra el valor; **no** firma el texto cifrado. Para producir texto cifrado para los valores de secreto, **debe usar** el mismo certificado de cifrado que está instalado en el clúster.

```console
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt -o plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```
La cadena codificada en base64 resultante (encrypted.txt) contiene tanto el texto cifrado del secreto, así como la información sobre el certificado que se usó para cifrarlo. Puede comprobar su validez descifrándolo con OpenSSL.
```console
user@linux:$ cat encrypted.txt | base64 -d | openssl smime -decrypt -inform der -inkey TestCert.prv
```

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo [especificar secretos cifrados en una aplicación.][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application
