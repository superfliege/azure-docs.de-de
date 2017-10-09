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
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: ef5e058d81b64c46e8bad6f85c0bef0c69fb1512
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="azure-machine-learning-workbench---known-issues-and-troubleshooting-guide"></a>Azure Machine Learning-Workbench – bekannte Probleme und Anleitung zur Problembehandlung 
Dieser Artikel hilft Ihnen, Fehler oder Ausfälle zu suchen und zu beheben, die bei der Verwendung der Anwendung Azure Machine Learning Workbench auftreten können. 

> [!NOTE]
> Bei der Kommunikation mit dem Supportteam per E-Mail oder Forumsbeiträgen ist es hilfreich, die Nummer des Builds zur Hand zu haben. Sie können die Buildnummer der App herausfinden, indem Sie auf das **Hilfemenü** klicken. Wenn Sie auf die Buildnummer klicken, wird sie in die Zwischenablage kopiert. Sie können sie in E-Mails oder Supportforen einfügen, um das Melden von Problemen zu erleichtern.

![Überprüfen der Versionsnummer](media/known-issues-and-troubleshooting-guide/version.png)

## <a name="windows-and-mac"></a>Windows und Mac
### <a name="centos-based-azure-data-science-virtual-machine"></a>CentOS-basierte Azure Data Science Virtual Machine 
* Übermitteln von Aufträgen an CentOS-basierte Azure Data Science Virtual Machine (DSVM) wird nicht unterstützt. Sie können [Ubuntu-basierte DSVMs](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) nutzen.

### <a name="docker-error"></a>Docker-Fehler 
* Wenn bei Ausführung in einem lokalen Docker-Container die folgende Fehlermeldung angezeigt wird, können Sie den Fehler durch Ändern des Docker-DNS-Servers von „Automatic“ in „Fixed 8.8.8.8“ beheben. 
```
Get https://registry-1.docker.io/v2/: 
dial tcp: 
lookup registry-1.docker.io on [::1]:53: read udp [::1]:49385->[::1]:53: 
read: connection refused
```
![Image](media/known-issues-and-troubleshooting-guide/docker_dns.png)


### <a name="linux-vm-password"></a>Kennwort eines virtuellen Linux-Computers 
* Wenn Sie im Azure-Portal das Kennwort eines virtuellen Ubuntu-Linux-Computers ändern, könnten Sie die folgende Fehlermeldung erhalten, wenn Sie einen Auftrag darauf ausführen:
  ```
  sudo: no tty present and no askpass program specified.
  ``` 

  Die Problemumgehung besteht im Hinzufügen einer Datei (Beispiel _myDockerUsers_) in _/etc/sudoers.d_ mit folgendem Inhalt:
  ```
  <your_username> ALL = NOPASSWD: /usr/bin/docker, /usr/bin/nvidia-docker
  ```

### <a name="ssh-keys"></a>SSH-Schlüssel 
* SSH-Schlüssel werden nicht unterstützt, wenn über SSH eine Verbindung mit einem Remotecomputer oder Spark-Cluster hergestellt wird. Nur der Benutzername/Kennwort-Modus wird unterstützt.

## <a name="windows-only"></a>Nur Windows 
### <a name="sharing-c-drive-in-docker"></a>Freigeben von Laufwerk C in Docker 
* Überprüfen Sie die Freigabe von Laufwerk C mit dem Datei-Explorer.
* Öffnen Sie die Netzwerkadaptereinstellungen, und deinstallieren Sie die „Datei- und Druckerfreigabe für Microsoft-Netzwerke“ für vEthernet, bzw. installieren Sie sie neu.
* Öffnen Sie die Docker-Einstellungen, und geben Sie Laufwerk C von den Docker-Einstellungen aus frei.
* Änderungen des Windows-Kennworts beeinflussen die Freigabe. Öffnen Sie den Datei-Explorer, geben Sie Laufwerk C erneut frei, und geben Sie das neue Kennwort ein.
* Bei dem Versuch, Laufwerk C mit Docker freizugeben, könnten auch Firewallprobleme auftreten. Dieser [Stack Overflow-Beitrag](http://stackoverflow.com/questions/42203488/settings-to-windows-firewall-to-allow-docker-for-windows-to-share-drive/43904051) kann hilfreich sein.
* Bei der Freigabe von Laufwerk C mit Domänenanmeldeinformationen könnte die Freigabe bei der Arbeit in Netzwerken beendet werden, in denen der Domänencontroller nicht erreichbar ist (z.B. Heimnetzwerk, öffentliches WLAN usw.). Weitere Informationen finden Sie in [diesem Beitrag](https://blogs.msdn.microsoft.com/stevelasker/2016/06/14/configuring-docker-for-windows-volumes/).



## <a name="mac-only"></a>Nur Mac 
* Umwandlungen von Textclustern werden unter Mac nicht unterstützt.
* Die RevoScalePy-Bibliothek wird unter Mac nicht unterstützt.

## <a name="azure-machine-learning-service-limits"></a>Grenzen des Azure Machine Learning-Diensts

- Maximal zulässige Größe des Projektordners: 25MB
    >[!Note]
    >Dieser Grenzwert gilt nicht für den `.git`-, `docs`- und `outputs`-Ordner. Bei diesen Ordnernamen wird zwischen Groß- und Kleinschreibung unterschieden.

- Maximal zulässige Experimentausführungszeit: 7 Tage
- Max. Größe der nachverfolgten Datei im `outputs`-Ordner nach der Ausführung: 512MB 

>[!NOTE]
>Wenn Sie mit großen Dateien arbeiten, lesen Sie den Artikel [Persisting changes and dealing with large files](how-to-read-write-files.md) (Beibehalten von Änderungen und Umgang mit großen Dateien).

## <a name="other-issues"></a>Andere Probleme
Wenn Sie auf weitere nicht dokumentierte Probleme stoßen, senden Sie uns Feedback über _Lächeln/Stirnrunzeln senden_. 




