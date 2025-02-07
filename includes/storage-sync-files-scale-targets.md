---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 05/05/2019
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 2614c9290bf31813d59ee753a31622bccf0682b8
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66114500"
---
| Recurso | Destino | Límite máximo |
|----------|--------------|------------|
| Servicios de sincronización de almacenamiento por región | 20 servicios de sincronización de almacenamiento | Sí |
| Grupos de sincronización por servicio de sincronización de almacenamiento | 100 grupos de sincronización | Sí |
| Servidores registrados por servicio de sincronización de almacenamiento | 99 servidores | Sí |
| Puntos de conexión en la nube por grupo de sincronización | 1 punto de conexión en la nube | Sí |
| Puntos de conexión de servidor por grupo de sincronización | 50 puntos de conexión del servidor | No |
| Puntos de conexión del servidor por servidor | 30 puntos de conexión de servidor | Sí |
| Objetos del sistema de archivos (archivos y directorios) por grupo de sincronización | 25 millones de objetos | No |
| Número máximo de objetos del sistema de archivos (archivos y directorios) en un directorio | 1 millón de objetos | Sí |
| Tamaño máximo del descriptor de seguridad de (archivos y directorios) del objeto | 64 KiB | Sí |
| Tamaño de archivo | 100 GiB | No |
| Tamaño mínimo de un archivo que se va a organizar en niveles | 64 KiB | Sí |
| Sesiones sincronizadas simultáneas | Agente de V4 y versiones posteriores: El límite varía en función de los recursos del sistema disponibles. <BR> Agente V3: Dos sesiones de ActiveSync por procesador o un máximo de ocho sesiones de ActiveSync por servidor. | Sí

> [!Note]  
> Un punto de conexión de Azure File Sync puede escalar verticalmente el tamaño de un recurso compartido de archivos de Azure. Si se alcanza el límite de tamaño del recurso compartido de archivos de Azure, la sincronización no podrá funcionar.