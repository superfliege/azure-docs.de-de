---
title: Erstellen eines Jenkins-Servers auf Azure
description: "Installieren von Jenkins auf einem virtuellen Azure-Linux-Computer aus der Vorlage der Jenkins-Lösung und erstellen Sie eine Java-beispielanwendung."
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: hero-article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 7bb74f297d52fb25171817175cce64187b397c38
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-the-azure-portal"></a>Erstellen eines Jenkins-Servers für eine Azure Linux-VM im Azure-portal

Dieser Schnellstart veranschaulicht die installieren [Jenkins](https://jenkins.io) für eine Ubuntu Linux-VM mit den Tools und so konfiguriert, dass die Arbeit mit Azure-Plug-ins. Wenn Sie fertig sind, müssen Sie einen Jenkins Server in Azure erstellen eine Java-beispielanwendung aus [GitHub](https://github.com).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement
* Zugriff auf SSH auf Ihrem Computer über die Befehlszeile (z. B. der Bash-Shell oder [PuTTY](http://www.putty.org/))

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>Erstellen Sie die Jenkins VM aus der Projektmappe (Vorlage)

Öffnen der [Marketplace-Image für Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) in Ihrem Webbrowser, und wählen **erhalten von IT jetzt** aus der linken Seite der Seite. Überprüfen Sie die Preisdetails, und wählen Sie **Fortfahren**, und wählen Sie dann **erstellen** den Jenkins-Server im Azure-Portal zu konfigurieren. 
   
![Azure-Portal, Dialogfeld](./media/install-jenkins-solution-template/ap-create.png)

In der **grundlegenden Einstellungen konfigurieren** Registerkarte, "füllen" in den folgenden Feldern:

![Grundeinstellungen konfigurieren](./media/install-jenkins-solution-template/ap-basic.png)

* Verwendung **Jenkins** für **Namen**.
* Geben Sie einen **Benutzername**. Der Benutzername muss erfüllen [bestimmte Anforderungen](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).
* Wählen Sie **Kennwort** als die **Authentifizierungstyp** und ein Kennwort eingeben. Das Kennwort muss einen Großbuchstaben, eine Zahl und ein Sonderzeichen enthalten.
* Verwendung **MyJenkinsResourceGroup** für die **Ressourcengruppe**.
* Wählen Sie die **Osten USA** [Azure-Region](https://azure.microsoft.com/regions/) aus der **Speicherort** Dropdownliste aus.

Wählen Sie **OK** zu fortfahren die **Konfigurieren zusätzlicher Optionen** Registerkarte. Geben Sie einen eindeutigen Domänennamen zu identifizieren, die Jenkins-Server, und wählen Sie **OK**.

![Einrichten weiterer Optionen](./media/install-jenkins-solution-template/ap-addtional.png)  

 Nachdem die Überprüfung erfolgreich ist, wählen **OK** erneut aus der **Zusammenfassung** Registerkarte. Wählen Sie abschließend **Kauf** Jenkins VM zu erstellen. Wenn Ihr Server bereit ist, erhalten Sie eine Benachrichtigung im Azure-Portal:   

![Jenkins ist jetzt Benachrichtigung](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-to-jenkins"></a>Herstellen einer Verbindung mit Jenkins

Navigieren Sie zu Ihrem virtuellen Computer (z. B. http://jenkins2517454.eastus.cloudapp.azure.com/), in Ihrem Webbrowser. Die Jenkins-Konsole ist nicht möglich, über unsichere HTTP, damit die Anweisungen auf der Seite, um den sicheren Zugriff auf die Verwaltungskonsole Jenkins von Ihrem Computer mit einem SSH-Tunnel bereitgestellt werden.

![Entsperren von Jenkins](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

Richten Sie die Tunnel mithilfe der `ssh` Befehl auf der Seite über die Befehlszeile und Ersetzen Sie dabei `username` durch den Namen des virtuellen Computers-Admin-Benutzer, die zuvor ausgewählt werden, wenn Sie den virtuellen Computer aus der Projektmappe (Vorlage) festlegen.

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

Nachdem Sie den Tunnel gestartet haben, navigieren Sie zu http://localhost: 8080 / auf dem lokalen Computer. 

Rufen Sie initialkennwort, indem Sie den folgenden Befehl ausführen, in der Befehlszeile, während über SSH eine Verbindung mit der Jenkins VM verbunden.

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
```

Entsperren Sie das Jenkins-Dashboard zum ersten Mal mit diesem Kennwort an.

![Entsperren von Jenkins](./media/install-jenkins-solution-template/jenkins-unlock.png)

Wählen Sie **vorgeschlagenen-Plug-Ins installieren** auf der nächsten Seite, und erstellen Sie einen Jenkins-Admin-Benutzer verwendet, um Zugriff auf das Dashboard Jenkins.

![Jenkins ist bereit!](./media/install-jenkins-solution-template/jenkins-welcome.png)

Der Jenkins-Server ist nun bereit, um Code zu erstellen.

## <a name="create-your-first-job"></a>Erstellen Sie Ihre erste Arbeitsstelle

Wählen Sie **erstellen neue Aufträge** über die Konsole Jenkins benennen Sie es dann **MySampleApp** , und wählen Sie **Freestyle Projekt**, und wählen Sie dann **OK**.

![Einen neuen Auftrag erstellen](./media/install-jenkins-solution-template/jenkins-new-job.png) 

Wählen Sie die **Quellcode-Management** Registerkarte, aktivieren Sie **Git**, und geben Sie die folgende URL in **Repository-URL** Feld:`https://github.com/spring-guides/gs-spring-boot.git`

![Definieren Sie das Git-Repository](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

Wählen Sie die **erstellen** Registerkarte aus, und wählen Sie dann **Add Buildschritt**, **aufrufen Gradle-Skript**. Wählen Sie **Gradle-Wrapper verwenden**, geben Sie dann `complete` in **Wrapper Speicherort** und `build` für **Aufgaben**.

![Verwenden Sie zum Erstellen des Gradle-Wrappers](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

Wählen Sie **erweiterte...** und geben Sie dann `complete` in der **Stamm erstellen Skript** Feld. Wählen Sie **Speichern** aus.

![Legen Sie erweiterte Einstellungen in der Gradle Wrapper Buildschritt](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a>Erstellen Sie den code

Wählen Sie **erstellen jetzt** zum Kompilieren des Codes und die Beispiel-app-Paket. Wenn Ihr Build abgeschlossen ist, wählen Sie die **Arbeitsbereich** Link für das Projekt.

![Navigieren Sie zu dem Arbeitsbereich abzurufenden JAR-Datei aus dem build](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

Navigieren Sie zu `complete/build/libs` und vergewissern Sie sich die `gs-spring-boot-0.1.0.jar` vorhanden ist, um sicherzustellen, dass es sich bei Ihrer Erstellung erfolgreich war. Ihre Jenkins-Server ist jetzt bereit, um Ihre eigenen Projekte in Azure zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Azure-VMs als Jenkins-agents](jenkins-azure-vm-agents.md)
