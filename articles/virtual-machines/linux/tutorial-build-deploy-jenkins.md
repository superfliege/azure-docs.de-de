---
title: CI/CD von Jenkins auf virtuellen Azure-Computern mit Team Services | Microsoft-Dokumentation
description: "Einrichten von Continuous Integration (CI) und Continuous Deployment (CD) einer Node.js-App mithilfe von Jenkins auf virtuellen Azure-Computern über Release Management in Visual Studio Team Services oder Microsoft Team Foundation Server"
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
ms.openlocfilehash: c96aafeb05293ccdc4c30c2b828cead1dfdb157c
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2017
---
# <a name="deploy-your-app-to-linux-vms-by-using-jenkins-and-team-services"></a>Bereitstellen Ihrer App auf virtuellen Linux-Computern mithilfe von Jenkins und Team Services

Continuous Integration (CI) und Continuous Deployment (CD) bilden eine Pipeline, über die Sie Ihren Code erstellen, freigeben und bereitstellen können. Visual Studio Team Services bietet eine vollständige Gruppe von CI/CD-Automatisierungstools mit umfassenden Features für die Bereitstellung in Azure. Jenkins ist ein gängiges serverbasiertes CI/CD-Drittanbietertool, das auch CI/CD-Automatisierung beinhaltet. Sie können Team Services und Jenkins zusammen verwenden und so die Bereitstellung Ihrer Cloud-Apps oder Clouddienste anpassen.

In diesem Tutorial erstellen Sie mithilfe von Jenkins eine Node.js-Web-App. Anschließend verwenden Sie entweder Team Services oder Team Foundation Server, um sie in einer [Bereitstellungsgruppe](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) mit virtuellen Linux-Computern bereitzustellen.

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Abrufen der Beispiel-App
> * Konfigurieren von Jenkins-Plug-Ins
> * Konfigurieren eines Jenkins Freestyle-Projekts für Node.js
> * Konfigurieren von Jenkins für die Team Services-Integration
> * Erstellen eines Jenkins-Dienstendpunkts
> * Erstellen einer Bereitstellungsgruppe für die virtuellen Azure-Computer
> * Erstellen einer Team Services-Releasedefinition
> * Ausführen manueller und durch CI ausgelöster Bereitstellungen

## <a name="before-you-begin"></a>Voraussetzungen

* Sie benötigen Zugriff auf einen Jenkins-Server. Falls Sie noch keinen Jenkins-Server erstellt haben, lesen Sie unter [Erstellen eines Jenkins-Masters auf einem virtuellen Azure-Computer](https://docs.microsoft.com/en-us/azure/jenkins/install-jenkins-solution-template) weiter. 

* Melden Sie sich bei Ihrem Team Services-Konto (**https://{Ihr Konto}.visualstudio.com**) an. 
  Sie können ein [kostenloses Team Services-Konto](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308) erstellen.

  > [!NOTE]
  > Weitere Informationen finden Sie unter [Connect to Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services) (Herstellen einer Verbindung mit Team Services).

*  Sie benötigen einen virtuellen Linux-Computer für ein Bereitstellungsziel.  Weitere Informationen finden Sie unter [Erstellen und Verwalten virtueller Linux-Computer mit der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm).

*  Öffnen Sie den eingehenden Port 80 für den virtuellen Computer. Weitere Informationen finden Sie unter [Erstellen von Netzwerksicherheitsgruppen über das Azure-Portal](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="get-the-sample-app"></a>Abrufen der Beispiel-App

Für die Bereitstellung benötigen Sie eine in einem Git-Repository gespeicherte App.
Für dieses Tutorial empfiehlt sich [diese über GitHub verfügbare Beispiel-App](https://github.com/azooinmyluggage/fabrikam-node). Dieses Tutorial enthält ein Beispielskript, das verwendet wird, um Node.js und eine Anwendung zu installieren. Wenn Sie Ihr eigenes Repository verwenden möchten, sollten Sie ein ähnliches Beispiel konfigurieren.

Erstellen Sie einen Fork dieser App, und notieren Sie den Speicherort (URL) zur Verwendung in nachfolgenden Schritten in diesem Tutorial. Weitere Informationen finden Sie unter [Fork a repo](https://help.github.com/articles/fork-a-repo/) (Forken eines Repositorys).    

> [!NOTE]
> Die App wurde über [Yeoman](http://yeoman.io/learning/index.html) erstellt. Sie verwendet Express, Bower und Grunt. Darüber hinaus verfügt sie über einige npm-Pakete als Abhängigkeiten.
> Die Beispiel-App enthält zudem ein Skript, über das Nginx eingerichtet und die App bereitgestellt wird. Es wird auf den virtuellen Computern ausgeführt. Das Skript führt folgende Aktionen aus:
> 1. Es installiert Node, Nginx und PM2.
> 2. Es konfiguriert Nginx und PM2.
> 3. Es startet die Node-App.

## <a name="configure-jenkins-plug-ins"></a>Konfigurieren von Jenkins-Plug-Ins

Als Erstes müssen zwei Jenkins-Plug-Ins konfiguriert werden: **NodeJS** und **VS Team Services Continuous Deployment**.

1. Öffnen Sie Ihr Jenkins-Konto, und klicken Sie auf **Manage Jenkins** (Jenkins verwalten) aus.
2. Klicken Sie auf der Seite **Manage Jenkins** (Jenkins verwalten) auf **Manage Plugins** (Plug-Ins verwalten).
3. Filtern Sie die Liste, um das Plug-In **NodeJS** zu finden, und klicken Sie anschließend auf **Install without restart** (Ohne Neustart installieren).
    ![Hinzufügen des NodeJS-Plug-Ins zu Jenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Filtern Sie die Liste, um das Plug-In **VS Team Services Continuous Deployment** zu finden, und klicken Sie anschließend auf **Install without restart** (Ohne Neustart installieren).
5. Navigieren Sie wieder zum Jenkins-Dashboard, und klicken Sie auf **Manage Jenkins** (Jenkins verwalten).
6. Klicken Sie auf **Global Tool Configuration** (Globale Toolkonfiguration). Suchen Sie nach **NodeJS**, und klicken Sie auf **NodeJS installations** (NodeJS-Installationen).
7. Klicken Sie auf **Install automatically** (Automatisch installieren), und geben Sie einen Wert für **Name** ein.
8. Wählen Sie **Speichern** aus.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Konfigurieren eines Jenkins Freestyle-Projekts für Node.js

1. Wählen Sie **Neues Element** aus. Geben Sie einen Elementnamen ein.
2. Klicken Sie auf **Freestyle Project** (Freestyle-Projekt). Klicken Sie auf **OK**.
3. Wählen Sie auf der Registerkarte **Source Code Management** (Quellcodeverwaltung) die Option **Git** aus, und geben Sie die Repositorydetails sowie den Branch mit Ihrem App-Code ein.    
    ![Hinzufügen eines Repositorys zum Build](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. Wählen Sie auf der Registerkarte **Build Triggers** (Buildtrigger) die Option **Poll SCM** (SCM abrufen) aus, und geben Sie den Zeitplan `H/03 * * * *` ein, um alle drei Minuten Änderungen im Git-Repository abzurufen. 
5. Wählen Sie auf der Registerkarte **Build Environment** (Buildumgebung) die Option **Provide Node &amp; npm bin/folder PATH** (Knoten angeben & npm bin/PATH-Ordner) und dann den Wert für **NodeJS Installation** (NodeJS-Installation) aus. Behalten Sie für **npmrc file** (NPMRC-Datei) die Einstellung **use system default** (Systemstandard verwenden) bei.
6. Wählen Sie auf der Registerkarte **Build** die Option **Execute shell** (Shell ausführen) aus, und geben Sie den Befehl `npm install` ein, um sicherzustellen, dass alle Abhängigkeiten aktualisiert werden.


## <a name="configure-jenkins-for-team-services-integration"></a>Konfigurieren von Jenkins für die Team Services-Integration

> [!NOTE]
> Stellen sie sicher, dass das für die folgenden Schritte verwendete persönliche Zugriffstoken (Personal Access Token, PAT) die Berechtigung *Release (Lesen, Schreiben, Ausführen und Verwalten)* in Team Services enthält.
 
1.  Erstellen Sie bei Bedarf ein persönliches Zugriffstoken in Ihrem Team Services-Konto. In Jenkins sind diese Informationen für den Zugriff auf Ihr Team Services-Konto erforderlich. Speichern Sie die Tokeninformationen für die folgenden Schritte in diesem Abschnitt.
  
    Informationen zum Generieren eines Tokens finden Sie unter [Authenticate access with personal access tokens for VSTS and TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) (Authentifizieren des Zugriffs mit persönlichen Zugriffstoken für VSTS und TFS).
2. Klicken Sie auf der Registerkarte **Post-build Actions** (Postbuildaktionen) auf **Add post-build action** (Postbuildaktion hinzufügen). Klicken Sie auf **Archive the artifacts** (Artefakte archivieren).
3. Geben Sie für **Files to archive** (Dateien zur Archivierung) den Wert `**/*` ein, um alle Dateien einzuschließen.
4. Klicken Sie zum Erstellen einer weiteren Aktion auf **Add post-build action** (Postbuildaktion hinzufügen).
5. Klicken Sie auf **Trigger release in TFS/Team Services** (Release in TFS/Team Services auslösen). Geben Sie den URI für Ihr Team Services-Konto ein (beispielsweise **https://{Ihr Kontoname}.visualstudio.com**).
6. Geben Sie unter **Team Project** (Teamprojekt) einen Namen ein.
7. Wählen Sie einen Namen für die Releasedefinition aus. (Die Releasedefinition wird später in Team Services erstellt.)
8. Wählen Sie die Anmeldeinformationen für die Verbindung mit Ihrer Team Services- oder Team Foundation Server-Umgebung aus:
   - Lassen Sie bei Verwendung von Team Services das Feld **Username** (Benutzername) leer. 
   - Geben Sie bei Verwendung einer lokale Version von Team Foundation Server einen Benutzernamen und ein Kennwort ein.    
   ![Konfigurieren von Jenkins-Postbuildaktionen](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. Speichern Sie das Jenkins-Projekt.


## <a name="create-a-jenkins-service-endpoint"></a>Erstellen eines Jenkins-Dienstendpunkts

Über einen Dienstendpunkt kann eine Verbindung zwischen Team Services und Jenkins hergestellt werden.

1. Öffnen Sie in Team Services die Seite **Dienste**, öffnen Sie die Liste **Neuer Dienstendpunkt**, und wählen Sie **Jenkins** aus.
   ![Hinzufügen eines Jenkins-Endpunkts](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Geben Sie einen Namen für die Verbindung ein.
3. Geben Sie die URL Ihres Jenkins-Servers ein, und klicken Sie auf die Option **Nicht vertrauenswürdige SSL-Zertifikate annehmen**. Beispiel-URL: **http://{Ihre Jenkins-URL}.westcentralus.cloudapp.azure.com**
4. Geben Sie den Benutzernamen und das Kennwort für Ihr Jenkins-Konto ein.
5. Klicken Sie auf **Verbindung überprüfen**, um zu überprüfen, ob die Informationen korrekt sind.
6. Klicken Sie auf **OK**, um den Dienstendpunkt zu erstellen.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Erstellen einer Bereitstellungsgruppe für virtuelle Azure-Computer

Sie benötigen eine [Bereitstellungsgruppe](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) für die Registrierung des Team Services-Agents, damit die Releasedefinition auf dem virtuellen Computer bereitgestellt werden kann. Mit Bereitstellungsgruppen können Sie komfortabel logische Gruppen von Zielcomputern für die Bereitstellung definieren und den erforderlichen Agent auf den einzelnen Computern installieren.

   > [!NOTE]
   > Achten Sie bei der folgenden Prozedur darauf, die erforderlichen Komponenten zu installieren und *das Skript nicht mit sudo-Berechtigungen auszuführen*.

1. Öffnen Sie die Registerkarte **Releases** des Hubs **Build &amp; Release**, und klicken Sie auf der Registerkarte **Bereitstellungsgruppen** auf **+ Neu**.
2. Geben Sie einen Namen und eine optionale Beschreibung für die Bereitstellungsgruppe ein. Klicken Sie anschließend auf **Erstellen**.
3. Wählen Sie das Betriebssystem für den virtuellen Zielcomputer der Bereitstellung aus. Wählen Sie also beispielsweise **Ubuntu 16.04+** aus.
4. Klicken Sie auf **Verwenden Sie für die Authentifizierung ein persönliches Zugriffstoken im Skript**.
5. Wählen Sie den Link **Systemvoraussetzungen** aus. Installieren Sie die erforderlichen Komponenten für Ihr Betriebssystem.
6. Klicken Sie auf **Skript in die Zwischenablage kopieren**, um das Skript zu kopieren.
7. Melden Sie sich bei dem virtuellen Zielcomputer der Bereitstellung an, und führen Sie das Skript aus. Führen Sie das Skript nicht mit sudo-Berechtigungen aus.
8. Nach der Installation werden Sie aufgefordert, Bereitstellungsgruppenmarkierungen anzugeben. Übernehmen Sie die Standardeinstellungen.
9. Suchen Sie den neu registrierten virtuellen Computer in Team Services unter **Bereitstellungsgruppen** (in **Ziele**).

## <a name="create-a-team-services-release-definition"></a>Erstellen einer Team Services-Releasedefinition

Eine Releasedefinition gibt den Prozess an, der in Team Services zum Bereitstellen der App verwendet wird. In diesem Beispiel führen Sie ein Shellskript aus.

So erstellen Sie die Freigabedefinition in Team Services

1. Öffnen Sie die Registerkarte **Releases** des Hubs **Build &amp; Release**, und klicken Sie auf **Releasedefinition erstellen**. 
2. Wählen Sie die Vorlage **Leer** aus, indem Sie mit einem **leeren Prozess** starten.
3. Klicken Sie im Abschnitt **Artefakte** auf **+ Artefakt hinzufügen**, und wählen Sie unter **Quelltyp** die Option **Jenkins** aus. Wählen Sie Ihre Jenkins-Dienstendpunktverbindung aus. Wählen Sie dann den Jenkins-Quellauftrag aus, und klicken Sie auf **Hinzufügen**.
4. Klicken Sie auf die Auslassungspunkte neben **Umgebung 1**. Klicken Sie auf **Bereitstellungsgruppenphase hinzufügen**.
5. Wählen Sie Ihre Bereitstellungsgruppe aus.
5. Klicken Sie auf **+**, um **Bereitstellungsgruppenphase** eine Aufgabe hinzuzufügen.
6. Wählen Sie die Aufgabe **Shellskript** aus, und klicken Sie anschließend auf **Hinzufügen**. Die Aufgabe **Shellskript** gibt die Konfiguration für ein Skript an, das auf jedem Server ausgeführt wird, um Node.js zu installieren und die App zu starten.
8. Geben Sie für **Skriptpfad** Folgendes ein: **$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh**.
9. Klicken Sie auf **Erweitert**, und aktivieren Sie dann **Arbeitsverzeichnis angeben**.
10. Geben Sie für **Arbeitsverzeichnis** Folgendes ein: **$(System.DefaultWorkingDirectory)/Fabrikam-Node**.
11. Ändern Sie den Namen der Releasedefinition in den Namen, den Sie auf der Registerkarte **Post-build Actions** (Postbuildaktionen) des Builds in Jenkins angegeben haben. Dieser Name ist in Jenkins erforderlich, damit eine neue Freigabe ausgelöst werden kann, wenn die Quellartefakte aktualisiert werden.
12. Klicken Sie auf **Speichern** und dann auf **OK**, um die Releasedefinition zu speichern.

## <a name="execute-manual-and-ci-triggered-deployments"></a>Ausführen manueller und durch CI ausgelöster Bereitstellungen

1. Klicken Sie auf **+ Release** und anschließend auf **Release erstellen**.
2. Wählen Sie in der hervorgehobenen Dropdownliste den abgeschlossenen Build aus, und klicken Sie anschließend auf **Warteschlange**.
3. Klicken Sie in der Popupmeldung auf den Releaselink. Beispiel: „Freigabe **Freigabe-1** wurde erstellt.“
4. Öffnen Sie die Registerkarte **Protokolle**, um die Konsolenausgabe der Freigabe anzuzeigen.
5. Öffnen Sie im Browser die URL für einen der Server, die Sie der Bereitstellungsgruppe hinzugefügt haben. Geben Sie dazu beispielsweise **http://{IP-Adresse des Servers}** ein.
6. Wechseln Sie zum Git-Quellrepository, und ändern Sie den Inhalt der Überschrift **h1** in der Datei „app/views/index.jade“ durch geänderten Text.
7. Committen Sie die Änderung.
8. Nach einigen Minuten wird auf der Seite **Freigaben** von Team Services oder Team Foundation Server ein neu erstelltes Release angezeigt. Öffnen Sie die Freigabe, um anzuzeigen, wie die Bereitstellung durchgeführt wird. Glückwunsch!

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die Bereitstellung einer App in Azure mit Jenkins (für den Build) und Team Services (für das Release) automatisiert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen der App in Jenkins
> * Konfigurieren von Jenkins für die Team Services-Integration
> * Erstellen einer Bereitstellungsgruppe für die virtuellen Azure-Computer
> * Erstellen einer Releasedefinition zum Konfigurieren der virtuellen Computer und zum Bereitstellen der App

Im nächsten Tutorial erfahren Sie, wie Sie einen LAMP-Stapel (Linux, Apache, MySQL und PHP) bereitstellen.

> [!div class="nextstepaction"]
> [Bereitstellen des LAMP-Stapels](tutorial-lamp-stack.md)
