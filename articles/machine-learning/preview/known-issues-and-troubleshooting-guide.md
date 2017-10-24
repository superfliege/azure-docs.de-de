---
title: Leitfaden zu bekannten Problemen und zur Problembehandlung | Microsoft-Dokumentation
description: "Liste der bekannten Probleme und eine Anleitung für die Problembehandlung"
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: f79910ba4fea81e88fce90dd67ba7cb4db2e8220
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-machine-learning-workbench---known-issues-and-troubleshooting-guide"></a>Azure Machine Learning-Workbench – bekannte Probleme und Anleitung zur Problembehandlung 
Dieser Artikel hilft Ihnen, Fehler oder Ausfälle zu suchen und zu beheben, die bei der Verwendung der Anwendung Azure Machine Learning Workbench auftreten können. 

> [!IMPORTANT]
> Bei der Kommunikation mit dem Supportteam ist es wichtig, dass Sie über die Buildnummer verfügen. Sie können die Buildnummer der App herausfinden, indem Sie auf das **Hilfemenü** klicken. Wenn Sie auf die Buildnummer klicken, wird sie in die Zwischenablage kopiert. Sie können sie in E-Mails oder Supportforen einfügen, um das Melden von Problemen zu erleichtern.

![Überprüfen der Versionsnummer](media/known-issues-and-troubleshooting-guide/buildno.png)

## <a name="machine-learning-msdn-forum"></a>MSDN-Forum zum Machine Learning
Wir betreiben ein MSDN-Forum, in dem Sie Fragen stellen können. Das Forum wird vom Produktteam ständig überwacht. Die URL des Forums lautet [https://aka.ms/azureml-forum](https://aka.ms/azureml-forum). 

## <a name="gather-diagnostics-information"></a>Sammeln von Diagnoseinformationen
Manchmal kann es hilfreich sein, Diagnoseinformationen bereitstellen zu können, wenn Sie um Hilfe bitten. Die Protkolldateien finden Sie hier:

### <a name="installer"></a>Installer
Sollten Sie beim Installieren auf ein Problem stoßen, können Sie die Protokolldateien des Installers in den folgenden Verzeichnissen finden:

```
# Windows:
%TEMP%\amlinstaller\logs\*

# macOS:
/tmp/amlinstaller/logs/*
```
Sie können die Inhalte dieser Verzeichnisse zippen und uns zur Diagnose zukommen lassen.

### <a name="workbench-desktop-app"></a>Workbench-Desktopanwendung
Sollte die Workbench-Desktopanwendung abstürzen, können Sie die Protokolldateien in den folgenden Verzeichnissen finden:
```
# Windows
%APPDATA%\AmlWorkbench

# macOS
~/Library/Application Support/AmlWorkbench
``` 
Sie können die Inhalte dieser Verzeichnisse zippen und uns zur Diagnose zukommen lassen.

### <a name="experiment-execution"></a>Experimentausführung
Sollte ein bestimmtes Skript während der Übermittlung von der Desktop-App fehlschlagen, versuchen Sie es erneut über die CLI mithilfe des Befehls `az ml experiment submit`. Daraufhin sollte Ihnen eine vollständige Fehlermeldung im JSON-Format angezeigt werden, die insbesondere einen **Vorgangs-ID-Wert** enthält. Senden Sie die JSON-Datei einschließlich des **Vorgangs-ID-Wertes** an uns, und wir helfen Ihnen bei der Diagnose. 

Wenn ein bestimmtest Skript zwar erfolgreich übermittelt wird, aber die Ausführung fehlschlägt, sollte es die **Ausführungs-ID** ausdrucken, um die betreffende Ausführung zu ermitteln. Sie können die betreffenden Protokolldateien mit dem folgenden Befehl verpacken:

```azurecli
# Create a ZIP file that contains all the diagnostics information
$ az ml experiment diagnostics -r <run_id> -t <target_name>
```

Mit dem Befehl `az ml experiment diagnostics` wird im Projektstammordner die Datei `diagnostics.zip` generiert. Das ZIP-Paket enthält den gesamten Projektordner im Zustand während der Ausführung sowie zusätzliche Protokollierungsinformationen. Achten Sie vor dem Senden der Diagnosedatei darauf, dass Sie alle Informationen entfernen, die nicht für die Weitergabe bestimmt sind.

## <a name="send-us-a-frown-or-a-smile"></a>Senden Sie uns ein Stirnrunzeln (oder ein Lächeln)

Wenn Sie in Azure ML Workbench arbeiten, können Sie uns auch ein Stirnrunzeln (oder ein Lächeln) senden, indem Sie in der Anwendungsshell unten links auf das Smiley-Symbol klicken. Sie können optional Ihre E-Mail-Adresse (damit wir Kontakt mit Ihnen aufnehmen können) bzw. einen Screenshot des aktuellen Zustands hinzufügen. 

## <a name="known-service-limits"></a>Bekannte Diensteinschränkungen
- Maximal zulässige Größe des Projektordners: 25 MB.
    >[!NOTE]
    >Dieser Grenzwert gilt nicht für die Ordner `.git`, `docs` und `outputs`. Bei diesen Ordnernamen wird zwischen Groß- und Kleinschreibung unterschieden. Wenn Sie mit großen Dateien arbeiten, lesen Sie den Artikel [Persisting changes and dealing with large files](how-to-read-write-files.md) (Beibehalten von Änderungen und Umgang mit großen Dateien).

- Maximal zulässige Experimentausführungszeit: sieben Tage
- Max. Größe der nachverfolgten Datei im `outputs`-Ordner nach der Ausführung: 512MB
  - Dies bedeutet Folgendes: Wenn Ihr Skript im Ordner „Outputs“ eine Datei erzeugt, die größer als 512 MB ist, wird sie hier nicht in die Sammlung einbezogen. Wenn Sie mit großen Dateien arbeiten, lesen Sie den Artikel [Persisting changes and dealing with large files](how-to-read-write-files.md) (Beibehalten von Änderungen und Umgang mit großen Dateien).

- SSH-Schlüssel werden nicht unterstützt, wenn über SSH eine Verbindung mit einem Remotecomputer oder Spark-Cluster hergestellt wird. Derzeit wird nur der Benutzername/Kennwort-Modus unterstützt.

- Umwandlungen von Textclustern werden unter Mac nicht unterstützt.

- Die RevoScalePy-Bibliothek wird nur unter Windows oder Linux (in Docker-Containern) unterstützt. Unter macOS wird sie nicht unterstützt.

## <a name="docker-error-read-connection-refused"></a>Docker-Fehler „read: connection refused“
Bei der Ausführung für einen lokalen Docker-Container kann es vorkommen, dass der folgende Fehler angezeigt wird: 
```
Get https://registry-1.docker.io/v2/: 
dial tcp: 
lookup registry-1.docker.io on [::1]:53: read udp [::1]:49385->[::1]:53: 
read: connection refused
```

Sie können ihn beheben, indem Sie den Wert des Docker-DNS-Servers von `automatic` in den festen Wert `8.8.8.8` ändern.

## <a name="remove-vm-execution-error-no-tty-present"></a>Entfernen des VM-Ausführungsfehlers „no tty present“
Bei der Ausführung für einen Docker-Container auf einem Linux-Remotecomputer wird ggf. die folgende Fehlermeldung angezeigt:
```
sudo: no tty present and no askpass program specified.
``` 
Dies kann passieren, wenn Sie das Azure-Portal nutzen, um das Stammkennwort einer Ubuntu Linux-VM zu ändern. 

Für Azure Machine Learning Workbench benötigen sudo-Benutzer Zugriff ohne Kennwort, um die Ausführung auf Remotehosts durchführen zu können. Die einfachste Möglichkeit ist hierbei die Verwendung von _visudo_ zum Bearbeiten der folgenden Datei (Sie können die Datei erstellen, falls sie nicht vorhanden ist):

```
$ sudo visudo -f /etc/sudoers
```

>[!IMPORTANT]
>Es ist wichtig, dass Sie die Datei mit _visudo_ und nicht mit einem anderen Befehl bearbeiten. _visudo_ führt automatisch eine Syntaxprüfung für alle sudo-Konfigurationsdateien durch, und wenn Sie keine sudo-Datei mit korrekter Syntax erstellen, werden Sie unter Umständen für sudo gesperrt.

Fügen Sie am Ende der Datei die folgende Zeile ein:

```
username ALL=(ALL) NOPASSWD:ALL
```

Hierbei ist _username_ der Name, der von Azure Machine Learning Workbench für die Anmeldung an Ihrem Remotehost verwendet wird.

Die Zeile muss nach #includedir "/etc/sudoers.d" angeordnet werden, da sie sonst von einer anderen Regel außer Kraft gesetzt wird.

Falls Sie über eine kompliziertere sudo-Konfiguration verfügen, ist es ratsam, die sudo-Dokumentation für Ubuntu zu verwenden. Sie finden sie unter „https://help.ubuntu.com/community/Sudoers“.

Der obige Fehler kann auch vorkommen, wenn Sie in Azure keine Ubuntu-basierte Linux-VM als Ausführungsziel verwenden. Wir unterstützen nur Ubuntu-basierte Linux-VMs für die Remoteausführung. 

## <a name="vm-disk-is-full"></a>VM-Datenträger ist voll
Standardmäßig erhalten Sie beim Erstellen einer neuen Linux-VM in Azure einen Datenträger für das Betriebssystem mit 30 GB. Für das Docker-Modul wird standardmäßig derselbe Datenträger zum Pullen von Images und zum Ausführen von Containern verwendet. Dies kann dazu führen, dass der Betriebssystemdatenträger schnell voll ist. In diesem Fall wird ein Fehler der Art „VM-Datenträger ist voll“ angezeigt.

Eine schnelle Lösung des Problems besteht darin, alle nicht mehr verwendeten Docker-Images zu entfernen. Mit dem folgenden Docker-Befehl wird dieser Vorgang durchgeführt. (Sie müssen natürlich per SSH auf die VM zugreifen, um den Docker-Befehl über eine Bash-Shell auszuführen.)

```
$ docker system prune -a
```

Sie können auch einen Datenträger für Daten hinzufügen und das Docker-Modul so konfigurieren, dass dieser Datenträger zum Speichern von Images verwendet wird. [Hier wird beschrieben, wie Sie einen Datenträger für Daten hinzufügen](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/add-disk). Anschließend können Sie [ändern, wo Images von Docker gespeichert werden](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169).

Sie können auch den Betriebssystemdatenträger erweitern, ohne dass es erforderlich ist, die Konfiguration des Docker-Moduls zu ändern. [Hier erfahren Sie, wie Sie den Betriebssystemdatenträger erweitern](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/add-disk).

## <a name="sharing-c-drive-on-windows"></a>Freigeben von Laufwerk C unter Windows
Wenn die Ausführung in einem lokalen Docker-Container unter Windows erfolgt, kann die Leistung verbessert werden, indem `sharedVolumes` in der Datei `docker.compute` unter `aml_config` auf `true` festgelegt wird. Hierfür muss das Laufwerk C aber im _Tool „Docker for Windows“_ freigegeben werden. Versuchen Sie Folgendes, wenn Sie Laufwerk C nicht freigeben können:

* Überprüfen Sie die Freigabe von Laufwerk C mit dem Datei-Explorer.
* Öffnen Sie die Netzwerkadaptereinstellungen, und deinstallieren Sie die „Datei- und Druckerfreigabe für Microsoft-Netzwerke“ für vEthernet, bzw. installieren Sie sie neu.
* Öffnen Sie die Docker-Einstellungen, und geben Sie Laufwerk C von den Docker-Einstellungen aus frei.
* Änderungen des Windows-Kennworts beeinflussen die Freigabe. Öffnen Sie den Datei-Explorer, geben Sie Laufwerk C erneut frei, und geben Sie das neue Kennwort ein.
* Bei dem Versuch, Laufwerk C mit Docker freizugeben, könnten auch Firewallprobleme auftreten. Dieser [Stack Overflow-Beitrag](http://stackoverflow.com/questions/42203488/settings-to-windows-firewall-to-allow-docker-for-windows-to-share-drive/43904051) kann hilfreich sein.
* Bei der Freigabe von Laufwerk C mit Domänenanmeldeinformationen könnte die Freigabe bei der Arbeit in Netzwerken beendet werden, in denen der Domänencontroller nicht erreichbar ist (z.B. Heimnetzwerk, öffentliches WLAN usw.). Weitere Informationen finden Sie in [diesem Beitrag](https://blogs.msdn.microsoft.com/stevelasker/2016/06/14/configuring-docker-for-windows-volumes/).

Es kommt zwar zu leichten Leistungseinbußen, aber Sie können das Freigabeproblem auch vermeiden, indem Sie `sharedVolumne` in der Datei `docker.compute` auf `false` festlegen.

## <a name="some-useful-docker-commands"></a>Hilfreiche Docker-Befehle

Hier sind einige hilfreiche Docker-Befehle aufgeführt:

```sh
# display all running containers
$ docker ps

# dislplay all containers (running or stopped)
$ docke ps -a

# display all images
$ docker images

# show Docker logs of a container
$ docker logs <container_id>

# create a new container and launch into a bash shell
$ docker run <image_id> /bin/bash

# launch into a bash shell on a running container
$ docker exec -it <container_id> /bin/bash

# stop an running container
$ docker stop <container_id>

# delete a container
$ docker rm <container_id>

# delete an image
$ docker rmi <image_id>

# delete all unussed Docker images 
$ docker system prune -a

```