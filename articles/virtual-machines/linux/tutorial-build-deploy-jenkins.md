---
title: CI/CD von Jenkins auf virtuellen Azure-Computern mit VSTS | Microsoft-Dokumentation
description: "Einrichten von Continuous Integration (CI) und Continuous Deployment (CD) einer Node.js-App mithilfe von Jenkins auf virtuellen Azure-Computern über Release Management in Visual Studio Team Services (VSTS) oder Microsoft Team Foundation Server (TFS)"
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/19/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: d5c15d6ab36a8454d1c69bac498be89b990c7e33
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-vsts"></a>Bereitstellen Ihrer App auf virtuellen Linux-Computern mithilfe von Jenkins und VSTS

Continuous Integration (CI) und Continuous Deployment (CD) stellen eine Pipeline dar, über die Sie Ihren Code erstellen, freigeben und bereitstellen können. Visual Studio Team Services (VSTS) umfasst eine vollständige Gruppe von CI/CD-Automatisierungstools mit vollem Funktionsumfang für die Bereitstellung in Azure. Jenkins ist ein gängiges serverbasiertes CI/CD-Drittanbietertool, das auch CI/CD-Automatisierung beinhaltet. Sie können beide Tools zusammen verwenden, um die Bereitstellung Ihrer Cloud-Apps oder Clouddienste anzupassen.

In diesem Tutorial verwenden Sie Jenkins zum Erstellen einer **Node.js-Web-App** und VSTS oder Team Foundation Server (TFS) zum Bereitstellen dieser App in einer [Bereitstellungsgruppe](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/), die virtuelle Linux-Computer enthält.

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Abrufen der Beispiel-App
> * Konfigurieren von Jenkins-Plug-Ins
> * Konfigurieren eines Jenkins Freestyle-Projekts für Node.js
> * Konfigurieren von Jenkins für die VSTS-Integration
> * Erstellen eines Jenkins-Dienstendpunkts
> * Erstellen einer Bereitstellungsgruppe für die virtuellen Azure-Computer
> * Erstellen einer VSTS-Releasedefinition
> * Ausführen von manuellen und durch CI ausgelösten Bereitstellungen

## <a name="before-you-begin"></a>Voraussetzungen

* Sie benötigen Zugriff auf einen Jenkins-Server. Wenn Sie noch keinen Jenkins-Server erstellt haben, finden Sie entsprechende Informationen unter [Erstellen eines Jenkins-Masters auf einem virtuellen Azure-Computer](https://docs.microsoft.com/en-us/azure/jenkins/install-jenkins-solution-template). 

* Melden Sie sich bei Ihrem VSTS-Konto an (`https://{youraccount}.visualstudio.com`). 
  Sie können ein [kostenloses VSTS-Konto](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308) einrichten.

  > [!NOTE]
  > Weitere Informationen finden Sie unter [Connect to VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services) (Herstellen einer Verbindung mit VSTS).

*  Sie benötigen einen virtuellen Linux-Computer für ein Bereitstellungsziel.  Weitere Informationen finden Sie unter [Erstellen und Verwalten virtueller Linux-Computer mit der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm).

*  Öffnen Sie den eingehenden Port 80 für den virtuellen Computer.  Weitere Informationen finden Sie unter [Erstellen von Netzwerksicherheitsgruppen über das Azure-Portal](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="get-the-sample-app"></a>Abrufen der Beispiel-App

Für die Bereitstellung benötigen Sie eine in einem Git-Repository gespeicherte App.
Für dieses Tutorial wird [diese über GitHub verfügbare Beispiel-App](https://github.com/azooinmyluggage/fabrikam-node) empfohlen.  Dieses Tutorial enthält ein Beispielskript für die Installation von Node.js und eine Anwendung.  Wenn Sie Ihr eigenes Repository verwenden möchten, sollten Sie ein ähnliches Beispiel konfigurieren.

1. Erstellen Sie einen Fork dieser App, und notieren Sie den Speicherort (URL) zur Verwendung in nachfolgenden Schritten in diesem Tutorial.  Weitere Informationen finden Sie unter [Fork a repo](https://help.github.com/articles/fork-a-repo/) (Forken eines Repositorys).    

> [!NOTE]
> Die App wurde mit [Yeoman](http://yeoman.io/learning/index.html) erstellt, sie verwendet **Express**, **Bower** und **Grunt**, und sie verfügt über einige **npm**-Pakete als Abhängigkeiten.
> Die Beispiel-App enthält zudem ein Skript, über das Nginx eingerichtet und die App bereitgestellt wird. Es wird auf den virtuellen Computern ausgeführt. Das Skript installiert Node, Nginx und PM2, konfiguriert Nginx und PM2 und startet dann die Node-App.

## <a name="configure-jenkins-plugins"></a>Konfigurieren von Jenkins-Plug-Ins

Zunächst müssen Sie zwei Jenkins-Plug-Ins für **NodeJS** und **VS Team Services Continuous Deployment** konfigurieren.

1. Öffnen Sie Ihr Jenkins-Konto, und wählen Sie **Manage Jenkins** (Jenkins verwalten) aus.
2. Wählen Sie auf der Seite **Manage Jenkins** (Jenkins verwalten) die Option **Manage Plugins** (Plug-Ins verwalten) aus.
3. Filtern Sie die Liste, um das **NodeJS**-Plug-In zu suchen. Wählen Sie dann die Option **Install without restart** (Ohne Neustart installieren) aus.
    ![Hinzufügen des NodeJS-Plug-Ins zu Jenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Filtern Sie die Liste, um das **VS Team Services Continuous Deployment**-Plug-In zu suchen. Wählen Sie dann die Option **Install without restart** (Ohne Neustart installieren) aus.
5. Navigieren Sie zurück zum Jenkins-Dashboard, und wählen Sie **Manage Jenkins** (Jenkins verwalten) aus.
6. Wählen Sie **Global Tool Configuration** (Globale Toolkonfiguration) aus.  Suchen Sie **NodeJS**, und klicken Sie auf **NodeJS installations** (NodeJS-Installationen).
7. Aktivieren Sie die Option **Install automatically** (Automatisch installieren), und geben Sie dann einen Wert für **Name** ein.
8. Klicken Sie auf **Speichern**.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Konfigurieren eines Jenkins Freestyle-Projekts für Node.js

1. Klicken Sie auf **New Item** (Neues Element).  Geben Sie unter **item name** (Elementname) einen Namen ein.
2. Wählen Sie **Freestyle Project** (Freestyle-Projekt) aus.  Klicken Sie auf **OK**.
3. Wählen Sie auf der Registerkarte **Source Code Management** (Quellcodeverwaltung) die Option **Git** aus, und geben Sie die Details des Repositorys sowie den Branch ein, der Ihren App-Code enthält.    
    ![Hinzufügen eines Repositorys zum Build](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. Wählen Sie auf der Registerkarte **Build Triggers** (Buildtrigger) die Option **Poll SCM** (SCM abrufen) aus, und geben Sie den Zeitplan `H/03 * * * *` ein, um Änderungen im Git-Repository alle drei Minuten abzurufen. 
5. Wählen Sie auf der Registerkarte **Build Environment** (Buildumgebung) die Option **Provide Node &amp; npm bin/folder PATH** (Knoten angeben & npm bin/PATH-Ordner) und dann den Wert für **NodeJS Installation** (NodeJS-Installation) aus. Lassen Sie **npmrc file** (NPMRC-Datei) auf „use system default“ (Systemstandard verwenden) festgelegt.
6. Wählen Sie auf der Registerkarte **Build** die Option **Execute shell** (Shell ausführen) aus, und geben Sie den Befehl `npm install` ein, um sicherzustellen, dass alle Abhängigkeiten aktualisiert werden.


## <a name="configure-jenkins-for-vsts-integration"></a>Konfigurieren von Jenkins für die VSTS-Integration

  > [!NOTE]
  Stellen Sie sicher, dass das für die folgenden Schritte verwendete persönliche Zugriffstoken (PAT) die **Berechtigung „Release (Lesen, Schreiben, Ausführen und Verwalten)“ in VSTS** enthält.
 
1.  Erstellen Sie ein persönliches Zugriffstoken in Ihrem VSTS-Konto, falls Sie noch über keines verfügen. In Jenkins sind diese Informationen für den Zugriff auf Ihr VSTS-Konto erforderlich.  Stellen Sie sicher, dass Sie die Tokeninformationen für die folgenden Schritte in diesem Abschnitt **speichern**.
  Informationen zum Generieren eines Tokens finden Sie unter [How do I create a personal access token for VSTS and TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) (Wie erstelle ich ein persönliches Zugriffstoken für VSTS und TFS?).
2. Klicken Sie auf der Registerkarte **Post-build Actions** (Postbuildaktionen) auf **Add post-build action** (Postbuildaktion hinzufügen). Wählen Sie **Archive the artifacts** (Artefakte archivieren) aus.
3. Geben Sie für **Files to archive** (Dateien zur Archivierung) den Wert `**/*` ein, um alle Dateien einzuschließen.
4. Um eine weitere Aktion zu erstellen, klicken Sie auf **Add post-build action** (Postbuildaktion hinzufügen).
5. Wählen Sie **Trigger release in TFS/Team Services** (Release in TFS/Team Services auslösen) aus, und geben Sie den URI für Ihr VSTS-Konto ein, z.B. `https://{your-account-name}.visualstudio.com`.
6. Geben Sie unter **Team Project** (Teamprojekt) einen Namen ein.
7. Wählen Sie einen Namen für die **release definition** (Releasedefinition) aus. (Diese Releasedefinition erstellen Sie später in VSTS.)
8. Wählen Sie die Anmeldeinformationen zum Herstellen einer Verbindung mit der VSTS- oder TFS-Umgebung aus.  Lassen Sie das Feld **Username** (Benutzername) leer, wenn Sie VSTS verwenden.
   Nehmen Sie Eingaben für **Username und Password** (Benutzername und Kennwort) vor, wenn Sie eine lokale Version von TFS verwenden.    
    ![Konfigurieren von Jenkins-Postbuildaktionen](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. **Speichern**  Sie das Jenkins-Projekt.

## <a name="create-a-jenkins-service-endpoint"></a>Erstellen eines Jenkins-Dienstendpunkts

Über einen Dienstendpunkt kann eine Verbindung zwischen VSTS und Jenkins hergestellt werden.

1. Öffnen Sie in VSTS die Seite **Dienste**, öffnen Sie die Liste **Neuer Dienstendpunkt**, und wählen Sie **Jenkins** aus.
    ![Hinzufügen eines Jenkins-Endpunkts](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Geben Sie einen Namen für die Verbindung ein.
3. Geben Sie die URL Ihres Jenkins-Servers ein, und aktivieren Sie die Option **Nicht vertrauenswürdige SSL-Zertifikate annehmen**.  Beispiel-URL: `http://{YourJenkinsURL}.westcentralus.cloudapp.azure.com`
4. Geben Sie den **Benutzernamen und das Kennwort** für Ihr Jenkins-Konto ein.
5. Wählen Sie **Verbindung überprüfen** aus, um zu überprüfen, ob die Informationen richtig sind.
6. Wählen Sie **OK** aus, um den Dienstendpunkt zu erstellen.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Erstellen einer Bereitstellungsgruppe für virtuelle Azure-Computer

Sie benötigen eine [Bereitstellungsgruppe](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) für die Registrierung des VSTS-Agents, damit die Releasedefinition auf dem virtuellen Computer bereitgestellt werden kann.  Mit Bereitstellungsgruppen lassen sich logische Gruppen von Zielcomputern für die Bereitstellung einfach definieren. Zudem wird der erforderliche Agent auf den einzelnen Computern installiert.

   > [!NOTE]
   > Stellen Sie in den folgenden Schritten sicher, dass Sie die erforderlichen Komponenten installieren und **das Skript nicht mit sudo-Berechtigungen ausführen**.

1. Öffnen Sie die Registerkarte **Releases** auf dem Hub **Build &amp; Release**. Öffnen Sie dann die Registerkarte **Bereitstellungsgruppen**, und wählen Sie **+ Neu** aus.
2. Geben Sie einen Namen und eine optionale Beschreibung für die Bereitstellungsgruppe ein.
   Wählen Sie dann **Erstellen** aus.
3. Wählen Sie das Betriebssystem für den virtuellen Zielcomputer der Bereitstellung aus.  Wählen Sie z.B. **Ubuntu 16.04+** aus.
4. Aktivieren Sie **Verwenden Sie für die Authentifizierung ein persönliches Zugriffstoken im Skript**.
5. Wählen Sie den Link **Systemvoraussetzungen** aus.  Installieren Sie die erforderlichen Komponenten für Ihr Betriebssystem.
6. Wählen Sie **Skript in die Zwischenablage kopieren** aus, um das Skript zu kopieren.
7. **Melden Sie sich** bei dem virtuellen Zielcomputer der Bereitstellung an, und **führen Sie das Skript aus**.  Führen Sie das Skript **nicht** mit sudo-Berechtigungen aus.
8. Nach der Installation werden Sie aufgefordert, Bereitstellungsgruppenmarkierungen anzugeben.  Übernehmen Sie die Standardeinstellungen.
9. Suchen Sie in VSTS den neu registrierten virtuellen Computer in **Ziele** unter **Bereitstellungsgruppen**.

## <a name="create-a-vsts-release-definition"></a>Erstellen einer VSTS-Releasedefinition

Eine Releasedefinition gibt den Vorgang an, der in VSTS zum Bereitstellen der App durchgeführt wird.  In diesem Beispiel führen Sie ein Shellskript aus.

So erstellen Sie die Releasedefinition in VSTS

1. Öffnen Sie die Registerkarte **Releases** auf dem Hub **Build &amp; Release**, und wählen Sie **Releasedefinition erstellen** aus. 
2. Wählen Sie die Vorlage **Leer** aus, indem Sie mit einem **leeren Prozess** starten.
3. Klicken Sie im Abschnitt **Artefakte** auf **+ Artefakt hinzufügen**, und wählen Sie **Jenkins** unter **Quelltyp** aus. Wählen Sie Ihre Jenkins-Dienstendpunktverbindung aus. Wählen Sie dann den Jenkins-Quellauftrag und anschließend **Hinzufügen** aus.
4.  Klicken Sie auf die Auslassungspunkte neben **Umgebung 1**.  Klicken Sie auf **Bereitstellungsgruppenphase hinzufügen**.
5.  Wählen Sie Ihre **Bereitstellungsgruppe** aus.
5. Klicken Sie auf **+**, um der **Bereitstellungsgruppenphase** eine Aufgabe hinzuzufügen.
6. Wählen Sie die Aufgabe **Shellskript** aus, und klicken Sie auf **Hinzufügen**.    
    Über die Aufgabe **Shellskript** wird die Konfiguration für ein Skript angegeben, das auf jedem Server ausgeführt wird, um Node.js zu installieren und die App zu starten. Konfigurieren Sie ihn wie folgt:
8. **Skriptpfad:**     
    `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`
9.  Klicken Sie auf **Erweitert**, und aktivieren Sie dann **Arbeitsverzeichnis angeben**.
10.  **Arbeitsverzeichnis:**`$(System.DefaultWorkingDirectory)/Fabrikam-Node`
11. Ändern Sie den Namen der Freigabedefinition in den Namen, den Sie auf der Registerkarte **Post-build Actions** (Postbuildaktionen) des Builds in Jenkins angegeben haben. Dieser Name ist in Jenkins erforderlich, damit eine neue Freigabe ausgelöst werden kann, wenn die Quellartefakte aktualisiert werden.
12. Klicken Sie auf **Speichern** und dann auf **OK**, um die Releasedefinition zu speichern.

## <a name="execute-manual-and-ci-triggered-deployments"></a>Ausführen von manuellen und durch CI ausgelösten Bereitstellungen

1. Wählen Sie **+ Freigabe** und dann **Freigabe erstellen** aus.
2. Wählen Sie den abgeschlossenen Build in der hervorgehobenen Dropdownliste und dann **Warteschlange** aus.
3. Wählen Sie den Freigabelink in der Popupmeldung aus. Beispiel: „Freigabe **Freigabe-1** wurde erstellt.“
4. Öffnen Sie die Registerkarte **Protokolle**, um die Konsolenausgabe der Freigabe anzuzeigen.
5. Öffnen Sie im Browser die URL für einen der Server, die Sie der Bereitstellungsgruppe hinzugefügt haben. Geben Sie z.B. `http://{your-server-ip-address}` ein.
6. Wechseln Sie zum Git-Quellrepository, und ändern Sie den Inhalt der Überschrift **h1** in der Datei „app/views/index.jade“ durch geänderten Text.
7. **Committen** Sie die Änderung.
8. Nach einigen Minuten wird auf der Seite **Releases** von VSTS oder TFS eine neu erstellte Freigabe angezeigt. Öffnen Sie die Freigabe, um anzuzeigen, wie die Bereitstellung durchgeführt wird. Glückwunsch!

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die Bereitstellung einer App in Azure mithilfe eines Jenkins-Builds und mit VSTS für die Freigabe automatisiert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen der App in Jenkins
> * Konfigurieren von Jenkins für die VSTS-Integration
> * Erstellen einer Bereitstellungsgruppe für die virtuellen Azure-Computer
> * Erstellen einer Freigabedefinition, über die die virtuellen Computer konfiguriert werden und die App bereitgestellt wird

Im nächsten Tutorial erfahren Sie mehr über die Bereitstellung eines LAMP-Stapels (Linux, Apache, MySQL und PHP).

> [!div class="nextstepaction"]
> [Bereitstellen des LAMP-Stapels](tutorial-lamp-stack.md)