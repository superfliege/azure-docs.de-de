---
title: Includedatei
description: Includedatei
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/10/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: cdb60ffb3ba3c31c336c8b74c46621792c707f74
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2018
ms.locfileid: "40046863"
---
### <a name="delete-local-resources"></a>Löschen lokaler Ressourcen

Wenn Sie die IoT Edge-Runtime und ihre Ressourcen von Ihrem Gerät löschen möchten, führen Sie die entsprechenden Befehle für Ihr Gerätebetriebssystem aus. 

#### <a name="windows"></a>Windows

Deinstallieren Sie die IoT Edge-Runtime.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Uninstall-SecurityDaemon
   ```

Sobald die IoT Edge-Runtime entfernt wird, werden die Container, die sie erstellt hat, angehalten. Sie sind jedoch weiterhin auf dem Gerät vorhanden. Zeigen Sie alle Container an.

   ```powershell
   docker ps -a
   ```

Löschen Sie die Runtimecontainer, die auf Ihrem Gerät erstellt wurden.

   ```powershell
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Löschen Sie alle weiteren Container, die ggf. in der Ausgabe von `docker ps` aufgeführt werden, indem Sie auf die Containernamen verweisen. 

#### <a name="linux"></a>Linux

Entfernen Sie die IoT Edge-Runtime.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Sobald die IoT Edge-Runtime entfernt wird, werden die Container, die sie erstellt hat, angehalten. Sie sind jedoch weiterhin auf dem Gerät vorhanden. Zeigen Sie alle Container an.

   ```bash
   sudo docker ps -a
   ```

Löschen Sie die Runtimecontainer, die auf Ihrem Gerät erstellt wurden.

   ```bash
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Löschen Sie alle weiteren Container, die ggf. in der Ausgabe von `docker ps` aufgeführt werden, indem Sie auf die Containernamen verweisen. 

Entfernen Sie die Containerruntime.

   ```bash
   sudo apt-get remove --purge moby
   ```