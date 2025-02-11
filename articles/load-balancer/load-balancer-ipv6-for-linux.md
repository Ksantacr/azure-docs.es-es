---
title: Configurar DHCPv6 para VM de Linux
titlesuffix: Azure Load Balancer
description: Cómo configurar DHCPv6 para VM de Linux.
services: load-balancer
documentationcenter: na
author: KumudD
keywords: IPv6, Azure Load Balancer, pila doble, dirección ip pública, ipv6 nativo, móvil, iot
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2019
ms.author: kumud
ms.openlocfilehash: 66777ec314e95d81a4be57082f06ef16dc170186
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60516550"
---
# <a name="configure-dhcpv6-for-linux-vms"></a>Configurar DHCPv6 para VM de Linux


Algunas de las imágenes de la máquina virtual con Linux en Azure Marketplace no tienen el Protocolo de configuración dinámica de host versión 6 (DHCPv6) configurado de forma predeterminada. Para admitir IPv6, DHCPv6 debe configurarse en la distribución de sistema operativo Linux que usa. Las diversas distribuciones de Linux configuran DHCPv6 de distintos modos porque usan distintos paquetes.

> [!NOTE]
> En Azure Marketplace se han configurado previamente imágenes de SUSE Linux y CoreOS recientes con DHCPv6. No es necesario realizar ningún cambio si usa estas imágenes.

En este documento se describe cómo habilitar DHCPv6 para que su máquina virtual con Linux obtenga una dirección IPv6.

> [!WARNING]
> La edición incorrecta de archivos de configuración de red puede dar lugar a la pérdida de acceso de red a la VM. Recomendamos que pruebe los cambios de configuración en sistemas que no sean de producción. Las instrucciones de este artículo se han probado en las versiones más recientes de las imágenes Linux en Azure Marketplace. Para obtener instrucciones más detalladas, consulte la documentación de su propia versión de Linux.

## <a name="ubuntu"></a>Ubuntu

1. Edite el archivo */etc/dhcp/dhclient6.conf* y agregue la siguiente línea:

        timeout 10;

2. Edite la configuración de red de la interfaz eth0 con la siguiente configuración:

   * En **Ubuntu 12.04 y 14.04**, edite el archivo */etc/network/interfaces.d/eth0.cfg*. 
   * En **Ubuntu 16.04**, edite el archivo */etc/network/interfaces.d/50-cloud-init.cfg*.

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Renueve la dirección IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```
A partir con Ubuntu 17.10, es el mecanismo de configuración de red predeterminada [NETPLAN]( https://netplan.io).  Durante la instalación y creación de instancias, NETPLAN lee la configuración de red de los archivos de configuración de YAML en esta ubicación: / {lib,etc,run}/netplan/*.yaml.

Incluya un *dhcp6:true* instrucción para cada interfaz ethernet en la configuración.  Por ejemplo: 
  
        network:
          version: 2
          ethernets:
            eno1:
              dhcp6: true

Durante el arranque temprano, el netplan "representador de red" escribe la configuración en/ejecuta para entregar desactivar control de dispositivos para el daemon de red especificado para la información de referencia sobre NETPLAN, consulte https://netplan.io/reference.
 
## <a name="debian"></a>Debian

1. Edite el archivo */etc/dhcp/dhclient6.conf* y agregue la siguiente línea:

        timeout 10;

2. Edite el archivo */etc/network/interfaces* y agregue la configuración siguiente:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Renueve la dirección IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel-centos-and-oracle-linux"></a>RHEL, CentOS y Oracle Linux

1. Edite el archivo */etc/sysconfig/network* y agregue el parámetro siguiente:

        NETWORKING_IPV6=yes

2. Edite el archivo */etc/sysconfig/network-scripts/ifcfg-eth0* y agregue los dos parámetros siguientes:

        IPV6INIT=yes
        DHCPV6C=yes

3. Renueve la dirección IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11-and-opensuse-13"></a>SLES 11 y openSUSE 13

En Azure se han configurado previamente imágenes de SUSE Linux Enterprise Server (SLES) y openSUSE recientes con DHCPv6. No es necesario realizar ningún cambio si usa estas imágenes. Si tiene una VM basada en una imagen de SUSE anterior o personalizada, siga estos pasos:

1. Instale el paquete `dhcp-client` , en caso de ser necesario:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Edite el archivo */etc/sysconfig/network/ifcfg-eth0* y agregue el parámetro siguiente:

        DHCLIENT6_MODE='managed'

3. Renueve la dirección IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 y openSUSE Leap

En Azure se han configurado previamente imágenes de SLES y openSUSE recientes con DHCPv6. No es necesario realizar ningún cambio si usa estas imágenes. Si tiene una VM basada en una imagen de SUSE anterior o personalizada, siga estos pasos:

1. Edite el archivo */etc/sysconfig/network/ifcfg-eth0* y reemplace el parámetro `#BOOTPROTO='dhcp4'` con el valor siguiente:

        BOOTPROTO='dhcp'

2. Agregue el parámetro siguiente al archivo */etc/sysconfig/network/ifcfg-eth0*:

        DHCLIENT6_MODE='managed'

3. Renueve la dirección IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

En Azure se han configurado previamente imágenes de CoreOS recientes con DHCPv6. No es necesario realizar ningún cambio si usa estas imágenes. Si tiene una máquina virtual basada en una imagen de CoreOS anterior o personalizada, siga estos pasos:

1. Edite el archivo */etc/systemd/network/10_dhcp.network*:

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Renueve la dirección IPv6:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
