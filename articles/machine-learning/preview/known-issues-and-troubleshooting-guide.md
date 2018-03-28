---
title: Leitfaden zu bekannten Problemen und zur Problembehandlung | Microsoft-Dokumentation
description: Liste der bekannten Probleme und eine Anleitung für die Problembehandlung
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ms.openlocfilehash: 62207fa20c4660d1e828053ee73953cb68af1b9d
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="azure-machine-learning-workbench---known-issues-and-troubleshooting-guide"></a>Azure Machine Learning-Workbench – bekannte Probleme und Anleitung zur Problembehandlung 
Dieser Artikel hilft Ihnen, Fehler oder Ausfälle zu suchen und zu beheben, die bei der Verwendung der Anwendung Azure Machine Learning Workbench auftreten können. 

## <a name="find-the-workbench-build-number"></a>Ermitteln der Workbench-Buildnummer
Bei der Kommunikation mit dem Supportteam ist es wichtig, dass Sie die Buildnummer der Workbench-App angeben. Unter Windows finden Sie die Buildnummer, indem Sie auf das Menü **Hilfe** klicken und die Option **Info zu Azure ML Workbench** auswählen. Unter macOS können Sie auf das Menü **Azure ML Workbench** klicken und die Option **Info zu Azure ML Workbench** auswählen.

## <a name="machine-learning-msdn-forum"></a>MSDN-Forum zum Machine Learning
Wir betreiben ein MSDN-Forum, in dem Sie Fragen stellen können. Das Forum wird vom Produktteam ständig überwacht. Die Forum-URL lautet [https://aka.ms/azureml-forum](https://aka.ms/azureml-forum). 

## <a name="gather-diagnostics-information"></a>Sammeln von Diagnoseinformationen
Manchmal kann es hilfreich sein, Diagnoseinformationen bereitstellen zu können, wenn Sie um Hilfe bitten. Die Protkolldateien finden Sie hier:

### <a name="installer-log"></a>Installationsprotokoll
Sollten Sie beim Installieren auf ein Problem stoßen, können Sie die Protokolldateien des Installers in den folgenden Verzeichnissen finden:

```
# Windows:
%TEMP%\amlinstaller\logs\*

# macOS:
/tmp/amlinstaller/logs/*
```
Sie können die Inhalte dieser Verzeichnisse zippen und uns zur Diagnose zukommen lassen.

### <a name="workbench-desktop-app-log"></a>Workbench-Desktop-App-Protokoll
Wenn Sie sich nicht anmelden können oder die Workbench-Desktopanwendung abstürzt, können Sie die Protokolldateien in den folgenden Verzeichnissen überprüfen:
```
# Windows
%APPDATA%\AmlWorkbench

# macOS
~/Library/Application Support/AmlWorkbench
``` 
Sie können die Inhalte dieser Verzeichnisse zippen und uns zur Diagnose zukommen lassen.

### <a name="experiment-execution-log"></a>Experimentausführungsprotokoll
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

- Wenn Sie einen HDInsight-Cluster als Computeziel verwenden, muss ein Azure-Blob als primärer Speicher verwendet werden. Die Verwendung von Azure Data Lake Store wird nicht unterstützt.

- Umwandlungen von Textclustern werden unter Mac nicht unterstützt.

- Die RevoScalePy-Bibliothek wird nur unter Windows und Linux (in Docker-Containern) unterstützt. Unter macOS wird sie nicht unterstützt.

- Jupyter Notebooks weisen eine maximale Größe von 5 MB aus, wenn sie über die Workbench-App geöffnet werden. Sie können große Notebooks über die Befehlszeilenschnittstelle mit dem Befehl „az ml notebook start“ öffnen und die Zellenausgaben bereinigen, um die Dateigröße zu reduzieren.

## <a name="cant-update-workbench"></a>Workbench kann nicht aktualisiert werden.
Wenn ein neues Update verfügbar ist, zeigt die Startseite der Workbench-App eine Meldung an, in der Sie über das neue Update informiert werden. In der linken unteren Ecke der App wird auf dem Glockensymbol eine Updatemarkierung angezeigt. Klicken Sie auf die Markierung, und befolgen Sie die Anweisungen des Installations-Assistenten, um das Update zu installieren. 

![Bild aktualisieren](./media/known-issues-and-troubleshooting-guide/update.png)

Wenn die Benachrichtigung nicht angezeigt wird, starten Sie die App neu. Wenn die Updatebenachrichtigung nach dem Neustart noch immer nicht angezeigt wird, sind verschiedene Ursachen möglich.

### <a name="you-are-launching-workbench-from-a-pinned-shortcut-on-the-task-bar"></a>Sie starten Workbench über eine auf der Taskleiste angeheftete Verknüpfung.
Möglicherweise haben Sie das Update bereits installiert. Die angeheftete Verknüpfung verweist jedoch weiterhin auf die alten Elemente auf dem Datenträger. Sie können dies überprüfen, indem Sie zum Ordner `%localappdata%/AmlWorkbench` navigieren und überprüfen, ob die neueste Version installiert ist. Überprüfen Sie dann die Eigenschaften der angehefteten Verknüpfung, um zu sehen, worauf sie verweist. Nach der Überprüfung entfernen Sie einfach die alte Verknüpfung, starten Sie Workbench über das Startmenü, und erstellen Sie optional eine neue angeheftete fixierte Verknüpfung auf der Taskleiste.

### <a name="you-installed-workbench-using-the-install-azure-ml-workbench-link-on-a-windows-dsvm"></a>Sie haben Workbench über den Link „Azure ML Workbench installieren“ auf einem virtuellen Windows-DS-Computer installiert
Leider gibt es hierfür keine einfache Lösung. Sie müssen die folgenden Schritte zum Entfernen der installierten Elemente ausführen und das neueste Installationsprogramm herunterladen, um Workbench neu zu installieren: 
   - Entfernen des Ordners `C:\Users\<Username>\AppData\Local\amlworkbench`
   - Entfernen des Skripts `C:\dsvm\tools\setup\InstallAMLFromLocal.ps1`
   - Entfernen der Desktopverknüpfung, die das oben genannte Skript startet
   - Herunterladen des Installationsprogramms (https://aka.ms/azureml-wb-msi) und erneutes Installieren

## <a name="stuck-at-checking-experimentation-account-screen-after-logging-in"></a>Festhängen nach der Anmeldung am Bildschirm zur Überprüfung des Experimentieren-Kontos
Nach der Anmeldung kann es vorkommen, dass die Workbench-App auf einem leeren Bildschirm mit der Meldung „Experimentieren-Konto wird überprüft“ mit einem drehenden Rad stecken bleibt. Führen Sie zum Beheben dieses Problems die folgenden Schritte aus:
1. Fahren Sie die App herunter.
2. Löschen Sie die folgende Datei:
  ```
  # on Windows
  %appdata%\AmlWorkbench\AmlWb.settings

  # on macOS
  ~/Library/Application Support/AmlWorkbench/AmlWb.settings
  ```
3. Starten Sie die App neu.

## <a name="cant-delete-experimentation-account"></a>Das Experimentieren-Konto kann nicht gelöscht werden.
Sie können mit der Befehlszeilenschnittstelle ein Experimentierkonto löschen, müssen jedoch untergeordnete Arbeitsbereiche und die untergeordneten Projekte in diesen untergeordneten Arbeitsbereichen zuerst löschen. Andernfalls wird der Fehler „Ressource kann nicht gelöscht werden, bevor verschachtelte Ressourcen gelöscht wurden“ angezeigt.

```azure-cli
# delete a project
$ az ml project delete -g <resource group name> -a <experimentation account name> -w <worksapce name> -n <project name>

# delete a workspace 
$ az ml workspace delete -g <resource group name> -a <experimentation account name> -n <worksapce name>

# delete an experimentation account
$ az ml account experimentation delete -g <resource group name> -n <experimentation account name>
```

Sie können die Projekte und Arbeitsbereiche auch über die Workbench-App löschen.

## <a name="cant-open-file-if-project-is-in-onedrive"></a>Die Datei kann nicht geöffnet werden, wenn sich das Projekt in OneDrive befindet.
Wenn Sie das Windows 10 Fall Creators Update besitzen und das Projekt in einem lokalen Ordner erstellt wird, der OneDrive zugeordnet ist, können Sie möglicherweise keine Dateien in Workbench öffnen. Dies liegt an einem durch das Fall Creators Update eingeführten Fehler, der bewirkt, dass Node.js-Code in OneDrive-Ordnern zu Fehlern führt. Der Fehler wird demnächst durch Windows Update behoben, Sie sollten jedoch bis zu diesem Zeitpunkt keine Projekte in OneDrive-Ordnern erstellen.

## <a name="file-name-too-long-on-windows"></a>Dateiname unter Windows zu lang
Wenn Sie Workbench unter Windows verwenden, kann das Problem der Standardbeschränkung der Dateinamenlänge auf maximal 260 Zeichen und dadurch der Fehler „Das System kann den angegebenen Pfad nicht finden“ auftreten. Sie können die Einstellung eines Registrierungsschlüssels ändern, um viel längere Dateipfadnamen zu gestatten. Ausführliche Informationen zum Festlegen des Registrierungsschlüssels _MAX_PATH_ finden Sie in [diesem Artikel](https://msdn.microsoft.com/en-us/library/windows/desktop/aa365247%28v=vs.85%29.aspx?#maxpath).

## <a name="interrupt-cli-execution-output"></a>Unterbrechen der Ausgabe der CLI-Ausführung
Wenn Sie eine Experimentieren-Ausführung mit `az ml experiment submit` oder `az ml notebook start` starten und die Ausgabe unterbrechen möchten: 
- Verwenden Sie unter Windows die Tastenkombination STRG+UNTBR.
- Verwenden Sie unter macOS STRG+C.

Beachten Sie, dass dies nur den Ausgabestream im Fenster der Befehlszeilenschnittstelle unterbricht. Ein gerade ausgeführter Auftrag wird nicht unterbrochen. Wenn Sie einen laufenden Auftrag abbrechen möchten, verwenden Sie den Befehl `az ml experiment cancel -r <run_id> -t <target name>`.

Auf Windows-Computern mit Tastaturen, die keine UNTBR-Taste aufweisen, sind auch die Alternativen Fn+B, STRG+Fn+B oder Fn+Esc möglich. Die genaue Tastenkombination finden Sie in der Dokumentation Ihres Hardwareherstellers.

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

Falls Sie über eine kompliziertere sudo-Konfiguration verfügen, ist es ratsam, die sudo-Dokumentation für Ubuntu zu verwenden: https://help.ubuntu.com/community/Sudoers.

Der obige Fehler kann auch vorkommen, wenn Sie in Azure keine Ubuntu-basierte Linux-VM als Ausführungsziel verwenden. Wir unterstützen nur Ubuntu-basierte Linux-VMs für die Remoteausführung. 

## <a name="vm-disk-is-full"></a>VM-Datenträger ist voll
Standardmäßig erhalten Sie beim Erstellen einer neuen Linux-VM in Azure einen Datenträger für das Betriebssystem mit 30 GB. Für die Docker-Engine wird standardmäßig derselbe Datenträger zum Pullen von Images und zum Ausführen von Containern verwendet. Dies kann dazu führen, dass der Betriebssystemdatenträger schnell voll ist. In diesem Fall wird ein Fehler der Art „VM-Datenträger ist voll“ angezeigt.

Eine schnelle Lösung des Problems besteht darin, alle nicht mehr verwendeten Docker-Images zu entfernen. Mit dem folgenden Docker-Befehl wird dieser Vorgang durchgeführt. (Sie müssen natürlich per SSH auf die VM zugreifen, um den Docker-Befehl über eine Bash-Shell auszuführen.)

```
$ docker system prune -a
```

Sie können auch einen Datenträger für Daten hinzufügen und die Docker-Engine so konfigurieren, dass dieser Datenträger zum Speichern von Images verwendet wird. [Hier wird beschrieben, wie Sie einen Datenträger für Daten hinzufügen](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk). Anschließend können Sie [ändern, wo Images von Docker gespeichert werden](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169).

Sie können auch den Betriebssystemdatenträger erweitern, ohne dass es erforderlich ist, die Konfiguration der Docker-Engine zu ändern. [Hier erfahren Sie, wie Sie den Betriebssystemdatenträger erweitern](https://docs.microsoft.com/azure/virtual-machines/linux/expand-disks).

```azure-cli
# Deallocate VM (stopping will not work)
$ az vm deallocate --resource-group myResourceGroup  --name myVM

# Get VM's Disc Name
az disk list --resource-group myResourceGroup --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table

# Update Disc Size using above name
$ az disk update --resource-group myResourceGroup --name myVMdisc --size-gb 250
    
# Start VM    
$ az vm start --resource-group myResourceGroup  --name myVM
```

## <a name="sharing-c-drive-on-windows"></a>Freigeben von Laufwerk C unter Windows
Wenn die Ausführung in einem lokalen Docker-Container unter Windows erfolgt, kann die Leistung verbessert werden, indem `sharedVolumes` in der Datei `docker.compute` unter `aml_config` auf `true` festgelegt wird. Hierfür muss das Laufwerk C aber im _Tool „Docker for Windows“_ freigegeben werden. Versuchen Sie Folgendes, wenn Sie Laufwerk C nicht freigeben können:

* Überprüfen Sie die Freigabe von Laufwerk C mit dem Datei-Explorer.
* Öffnen Sie die Netzwerkadaptereinstellungen, und deinstallieren Sie die „Datei- und Druckerfreigabe für Microsoft-Netzwerke“ für vEthernet, bzw. installieren Sie sie neu.
* Öffnen Sie die Docker-Einstellungen, und geben Sie Laufwerk C von den Docker-Einstellungen aus frei.
* Änderungen des Windows-Kennworts beeinflussen die Freigabe. Öffnen Sie den Datei-Explorer, geben Sie Laufwerk C erneut frei, und geben Sie das neue Kennwort ein.
* Bei dem Versuch, Laufwerk C mit Docker freizugeben, könnten auch Firewallprobleme auftreten. Dieser [Stack Overflow-Beitrag](http://stackoverflow.com/questions/42203488/settings-to-windows-firewall-to-allow-docker-for-windows-to-share-drive/43904051) kann hilfreich sein.
* Bei der Freigabe von Laufwerk C mit Domänenanmeldeinformationen könnte die Freigabe bei der Arbeit in Netzwerken beendet werden, in denen der Domänencontroller nicht erreichbar ist (z.B. Heimnetzwerk, öffentliches WLAN usw.). Weitere Informationen finden Sie in [diesem Beitrag](https://blogs.msdn.microsoft.com/stevelasker/2016/06/14/configuring-docker-for-windows-volumes/).

Es kommt zwar zu leichten Leistungseinbußen, aber Sie können das Freigabeproblem auch vermeiden, indem Sie `sharedVolumne` in der Datei `docker.compute` auf `false` festlegen.

## <a name="wipe-clean-workbench-installation"></a>Vollständiges Löschen einer Workbench-Installation
Im Allgemeinen ist dies nicht erforderlich. Falls Sie jedoch eine Installation vollständig löschen müssen, werden dazu folgende Schritte ausgeführt:

- Unter Windows:
  - Stellen Sie zuerst sicher, dass Sie das Applet _Programme hinzufügen oder entfernen_ in der _Systemsteuerung_ zum Entfernen des Anwendungseintrags _Azure Machine Learning Workbench_ verwenden.  
  - Laden Sie dann eines der folgenden Skripts herunter, und führen Sie es aus:
    - [Skript für die Windows-Befehlszeile](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_win.cmd).
    - [Skript für Windows PowerShell](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_win.ps1). (Möglicherweise müssen Sie `Set-ExecutionPolicy Unrestricted` in einem PowerShell-Fenster mit erhöhten Rechten ausführen, bevor Sie das Skript ausführen können.)
- Unter MacOS:
  - Laden Sie einfach das [Bash-Shellskript für macOS](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_mac.sh) herunter, und führen Sie es aus.


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
