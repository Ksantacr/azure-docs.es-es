---
title: Conecte los datos de punto de comprobación a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar los datos de punto de comprobación a Centinela de Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 3229233d-400d-4971-8d76-eaa0d6591d75
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: cad1f84f611ac3214b8823bb11817ffceb3e2017
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388694"
---
# <a name="connect-your-check-point-appliance"></a>Conecte el dispositivo de punto de comprobación

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede conectar Azure Sentinel a cualquier dispositivo de punto de comprobación al guardar los archivos de registro como Syslog CEF. La integración con Azure Sentinel permite ejecutar fácilmente análisis y consultas a través de los datos del archivo de registro de punto de comprobación. Para obtener más información sobre cómo Azure Sentinel ingiere datos CEF, consulte [dispositivos conectarse CEF](connect-common-event-format.md).

> [!NOTE]
> Datos se almacenarán en la ubicación geográfica del área de trabajo en el que se ejecuta Azure Sentinel.

## <a name="step-1-connect-your-check-point-appliance-using-an-agent"></a>Paso 1: Conecte el dispositivo de punto de comprobación mediante un agente

Para conectar el equipo de punto de comprobación al Centinela de Azure, deberá implementar a un agente en un equipo dedicado (máquina virtual o local) para admitir la comunicación entre el dispositivo y Azure Sentinel. Puede implementar el agente automáticamente o de forma manual. La implementación automática solo está disponible si su máquina dedicada es una nueva máquina virtual que crea en Azure. 

También puede implementar el agente manualmente en una máquina virtual existente, en una máquina virtual en otra nube o en una máquina local.

Para ver un diagrama de red de ambas opciones, consulte [conectar orígenes de datos](connect-data-sources.md).

### <a name="deploy-the-agent-in-azure"></a>Implementar al agente de Azure

1. En el portal de Azure Sentinel, haga clic en **conectores de datos** y seleccione el tipo de dispositivo. 

1. En **configuración del agente de Linux Syslog**:
   - Elija **implementación automática** si desea crear un nuevo equipo que viene preinstalado con el agente de Azure Sentinel e incluye todas la necesaria de la configuración, como se describió anteriormente. Seleccione **implementación automática** y haga clic en **implementación automática del agente**. Esto le llevará a la página de compra para una máquina virtual dedicada que se conecta automáticamente al área de trabajo. La máquina virtual es un **estándar D2s v3 (2 vCPU, 8 GB de memoria)** y tiene una dirección IP pública.
     1. En el **implementación personalizada** página, proporcione sus detalles y elija un nombre de usuario y una contraseña y si acepta los términos y condiciones, la máquina virtual de compra.
      
        1. Ejecute estos comandos en el equipo de agente de Syslog para asegurarse de que todos los registros de punto de comprobación se asignará al agente de Azure centinela:
           - Si usas Syslog-ng, ejecute estos comandos (tenga en cuenta que se reinicia al agente de Syslog):
            
                SUDO bash - c "printf" filtrar f_local4_oms {facility(local4);}; \ n destino security_oms {tcp (\"127.0.0.1\" port(25226));}; \n registro {source(src); filter(f_local4_oms); destination(security_oms);}; \n\nfilter f_msg_oms {coinciden (\"punto de comprobación\" valor (\" MENSAJE\")); }; \n destino security_msg_oms {tcp (\"127.0.0.1\" port(25226));}; \n registro {source(src); filter(f_msg_oms); destination(security_msg_oms);};' > /etc/syslog-ng/security-config-omsagent.conf "

             Reiniciar el demonio Syslog: `sudo service syslog-ng restart`
           - Si usa rsyslog, ejecute estos comandos (tenga en cuenta que se reinicia al agente de Syslog):
                    
                 sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Check Point\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"
             Reiniciar el demonio Syslog: `sudo service rsyslog restart`

   - Elija **implementación Manual** si desea usar una máquina virtual existente como la máquina Linux dedicada en el que se debe instalar el agente de Azure Sentinel. 
      1. En **descargar e instalar el agente de Syslog**, seleccione **máquina virtual Linux de Azure**. 
      1. En el **máquinas virtuales** pantalla que se abre, seleccione la máquina que desea usar y haga clic en **Connect**.
      1. En la pantalla de conector en **Syslog hacia delante y configurar**, establezca si es el demonio Syslog **rsyslog.d** o **syslog-ng**. 
      1. Copie estos comandos y ejecutarlos en su dispositivo:
          - Si seleccionó rsyslog.d:
              
            1. Indicar el demonio Syslog para que escuche en instalaciones local_4 y "Punto de comprobación" y para enviar los mensajes de Syslog del agente de Azure Sentinel usando el puerto 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Check Point\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
            2. Descargue e instale el [archivo de configuración de security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.
           
            1. Reiniciar el demonio syslog `sudo service rsyslog restart`
             
          - Si ha seleccionado syslog-ng:

              1. Indicar el demonio Syslog para que escuche en instalaciones local_4 y "Punto de comprobación" y para enviar los mensajes de Syslog del agente de Azure Sentinel usando el puerto 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. Descargue e instale el [archivo de configuración de security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.

              3. Reiniciar el demonio syslog `sudo service syslog-ng restart`
      2. Reinicie al agente de Syslog con este comando: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Confirme que hay ningún error en el registro del agente, ejecute este comando: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>Implementar al agente en servidores Linux locales

Si no usa Azure, implementar manualmente el agente Azure Centinela para ejecutarse en un servidor dedicado de Linux.

1. Para crear una VM Linux dedicado, en **configuración del agente de Linux Syslog** elija **implementación Manual**.
   1. En **descargar e instalar el agente de Syslog**, seleccione **máquina Linux que no son Azure**. 
   1. En el **agente directo** pantalla que se abre, seleccione **agente para Linux** para descargar el agente o ejecute este comando para descargarlo en su equipo Linux:   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`
      1. En la pantalla de conector en **Syslog hacia delante y configurar**, establezca si es el demonio Syslog **rsyslog.d** o **syslog-ng**. 
      1. Copie estos comandos y ejecutarlos en su dispositivo:
         - Si seleccionó **rsyslog**:
           1. Indicar el demonio Syslog para que escuche en instalaciones local_4 y "Punto de comprobación" y para enviar los mensajes de Syslog del agente de Azure Sentinel usando el puerto 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Check Point\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
           2. Descargue e instale el [archivo de configuración de security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.
           3. Reiniciar el demonio syslog `sudo service rsyslog restart`
         - Si seleccionó **syslog-ng**:
            1. Indicar el demonio Syslog para que escuche en instalaciones local_4 y "Punto de comprobación" y para enviar los mensajes de Syslog del agente de Azure Sentinel usando el puerto 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. Descargue e instale el [archivo de configuración de security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.
            3. Reiniciar el demonio syslog `sudo service syslog-ng restart`
      1. Reinicie al agente de Syslog con este comando: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Confirme que hay ningún error en el registro del agente, ejecute este comando: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
 
## <a name="step-2-forward-check-point-logs-to-the-syslog-agent"></a>Paso 2: Registros de punto de comprobación de avance para el agente de Syslog

Configurar el dispositivo de punto de comprobación para reenviar los mensajes de Syslog en formato CEF a su área de trabajo de Azure a través del agente de Syslog.

1. Vaya a [exportación del registro de punto de comprobación](https://aka.ms/asi-syslog-checkpoint-forwarding).
2. Desplácese hacia abajo hasta **implementación básica** y siga las instrucciones para configurar la conexión, mediante las siguientes directrices:
   - Establecer el **puerto Syslog** a **514** o el puerto que ha establecido en el agente.
     - Reemplace el **nombre** y **dirección IP del servidor de destino** en la CLI con la dirección IP y el nombre del agente de Syslog.
     - Establezca el formato en **CEF**.
3. Si usa la versión R77.30 o R80.10, desplácese hasta **instalaciones** y siga las instrucciones para instalar un exportador de registro para su versión.
 
## <a name="step-3-validate-connectivity"></a>Paso 3: Validar conectividad

Puede tardar más de 20 minutos hasta que los registros se empiecen a aparecer en Log Analytics. 

1. Asegúrese de que utilizar la función de la derecha. La función debe ser el mismo en el dispositivo y en Azure Sentinel. Puede comprobar qué archivo de instalación que se usa en Azure Sentinel y modificarla en el archivo `security-config-omsagent.conf`. 

2. Asegúrese de que obtengan los registros para el puerto correcto en el agente de Syslog. Ejecute este comando en el equipo de agente de Syslog: `tcpdump -A -ni any  port 514 -vv` Este comando muestra los registros que se transmiten desde el dispositivo a la máquina de Syslog. Asegúrese de que se reciben los registros desde el dispositivo de origen en el puerto adecuado y la instalación correcta.

3. Asegúrese de que los registros de envío que cumplen con [RFC 5424](https://tools.ietf.org/html/rfc542).

4. En el equipo que ejecuta el agente de Syslog, asegúrese de que estos puertos 514, 25226 están abiertos y escucha, utilizando el comando `netstat -a -n:`. Para obtener más información sobre el uso de este comando, consulte [netstat(8) - página man de Linux](https://linux.die.net/man/8/netstat). Si está escuchando correctamente, verá esto:

   ![Puertos de Centinela de Azure](./media/connect-cef/ports.png) 

5. Asegúrese de que el demonio de está configurado para escuchar en el puerto 514, en el que va a enviar los registros.
    - Para rsyslog:<br>Asegúrese de que el archivo `/etc/rsyslog.conf` incluye esta configuración:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Para obtener más información, consulte [imudp: Módulo de entrada de UDP Syslog](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) y [imtcp: Módulo de entrada de Syslog TCP](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module)

   - Para syslog-ng:<br>Asegúrese de que el archivo `/etc/syslog-ng/syslog-ng.conf` incluye esta configuración:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Para obtener más información, consulte [imudp: Módulo de entrada UDP Syslog] (para obtener más información, consulte el [syslog-ng Abrir origen Edition 3.16 - Guía de administración de](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Compruebe que hay comunicación entre el demonio de Syslog y el agente. Ejecute este comando en el equipo de agente de Syslog: `tcpdump -A -ni any  port 25226 -vv` Este comando muestra los registros que se transmiten desde el dispositivo a la máquina de Syslog. Asegúrese de que los registros también se reciben en el agente.

6. Si ambos de esos comandos proporcionan resultados correctos, compruebe si Log Analytics para ver si llegan los registros. Todos los eventos que se transmite por secuencias desde estos dispositivos aparecen en formato sin procesar en Log Analytics en `CommonSecurityLog` tipo.

7. Para comprobar si hay errores o si no que llegan los registros, busque en `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Si indica que hay errores de coincidencia de formato de registro, vaya a `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` y examine el archivo `security_events.conf`y asegúrese de que los registros coinciden con el formato de expresión regular vea en este archivo.

8. Asegúrese de que el tamaño predeterminado de mensaje de Syslog se limita a 2048 bytes (2KB). Si los registros son demasiado largos, actualice el security_events.conf con este comando: `message_length_limit 4096`

4. Asegúrese de ejecutar estos comandos:
  
   - Si usas Syslog-ng, ejecute estos comandos (tenga en cuenta que se reinicia al agente de Syslog):

         sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"
        Reiniciar el demonio Syslog: `sudo service syslog-ng restart`

   - Si usa rsyslog, ejecute estos comandos (tenga en cuenta que se reinicia al agente de Syslog): 

         sudo bash -c "printf 'local4.debug @127.0.0.1:25226\n\n:msg, contains, "Check Point" @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"
     Reiniciar el demonio Syslog: `sudo service rsyslog restart`




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar dispositivos de punto de comprobación a Centinela de Azure. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).

