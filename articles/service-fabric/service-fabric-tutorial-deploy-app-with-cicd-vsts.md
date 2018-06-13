---
title: Bereitstellen einer Azure Service Fabric-Anwendung mit Continuous Integration (Team Services) | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Continuous Integration und Continuous Deployment mithilfe von Visual Studio Team Services für eine Service Fabric-Anwendung einrichten.  Stellen Sie eine Anwendung in einem Service Fabric-Cluster in Azure bereit.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/13/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 5b61b7f89c127b297f058082d86952f2a45d766a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
ms.locfileid: "31798708"
---
# <a name="tutorial-deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>Tutorial: Bereitstellen einer Anwendung mit CI/CD in einem Service Fabric-Cluster
Dieses Tutorial ist der vierte Teil einer Reihe und beschreibt die Einrichtung von Continuous Integration und Continuous Deployment für eine Azure Service Fabric-Anwendung mithilfe von Visual Studio Team Services.  Es ist eine vorhandene Service Fabric-Anwendung erforderlich. Die im Artikel [Erstellen einer .NET-Anwendung](service-fabric-tutorial-create-dotnet-app.md) erstellte Anwendung wird als Beispiel verwendet.

Im dritten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen der Quellcodeverwaltung zum Projekt
> * Erstellen einer Builddefinition in Team Services
> * Erstellen einer Releasedefinition in Team Services
> * Automatisches Bereitstellen und Durchführen von Upgrades einer Anwendung

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer .NET Service Fabric-Anwendung](service-fabric-tutorial-create-dotnet-app.md)
> * [Bereitstellen der Anwendung in einem Remotecluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Hinzufügen eines HTTPS-Endpunkts zu einem ASP.NET Core-Front-End-Dienst](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * Konfigurieren von CI/CD mit Visual Studio Team Services
> * [Einrichten der Überwachung und Diagnose für die Anwendung](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Installieren Sie Visual Studio 2017](https://www.visualstudio.com/) und die Workloads **Azure-Entwicklung** und **ASP.NET und Webentwicklung**.
- [Installieren Sie das Service Fabric SDK](service-fabric-get-started.md).
- Erstellen Sie einen Windows Service Fabric-Cluster in Azure, z.B. durch das [Ausführen der Schritte in diesem Tutorial](service-fabric-tutorial-create-vnet-and-windows-cluster.md).
- Erstellen Sie ein [Team Services-Konto](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="download-the-voting-sample-application"></a>Laden Sie die Beispielanwendung „Voting“ herunter.
Falls Sie die Beispielanwendung „Voting“ aus [Teil 1 dieser Tutorialreihe](service-fabric-tutorial-create-dotnet-app.md) nicht erstellt haben, können Sie sie herunterladen. Führen Sie in einem Befehlsfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Erstellen eines Veröffentlichungsprofils
Nachdem Sie [eine Anwendung erstellt](service-fabric-tutorial-create-dotnet-app.md) und [die Anwendung in Azure bereitgestellt](service-fabric-tutorial-deploy-app-to-party-cluster.md) haben, können Sie Continuous Integration einrichten.  Erstellen Sie zunächst in der Anwendung ein Veröffentlichungsprofil für den Bereitstellungsprozess, der in Team Services ausgeführt wird.  Das Veröffentlichungsprofil muss für den Cluster konfiguriert werden, den Sie zuvor erstellt haben.  Starten Sie Visual Studio, und öffnen Sie ein vorhandenes Service Fabric-Anwendungsprojekt.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Anwendung, und wählen Sie **Veröffentlichen** aus.

Wählen Sie ein Zielprofil im Anwendungsprojekt aus, das Sie für den Continuous Integration-Workflow verwenden möchten, z.B. „Cloud“.  Geben Sie den Clusterverbindungsendpunkt an.  Aktivieren Sie das Kontrollkästchen **Upgrade der Anwendung ausführen**, damit für jede Bereitstellung in Team Services ein Upgrade der Anwendung erfolgt.  Klicken Sie auf den Link **Speichern**, um die Einstellungen im Veröffentlichungsprofil zu speichern, und klicken Sie dann auf **Abbrechen**, um das Dialogfeld zu schließen.  

![Profil pushen][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a>Freigeben der Visual Studio-Projektmappe in einem neuen Git-Repository von Team Services
Geben Sie die Anwendungsquelldateien in einem Teamprojekt in Team Services frei, damit Sie Builds generieren können.  

Erstellen Sie ein neues lokales Git-Repository für das Projekt, indem Sie auf der Statusleiste in der unteren rechten Ecke von Visual Studio **Zur Quellcodeverwaltung hinzufügen** -> **Git** auswählen. 

Klicken Sie in der Ansicht **Push** in **Team Explorer** unter **Per Push in Visual Studio Team Services übertragen** auf die Schaltfläche **Git-Repository veröffentlichen**.

![Git-Repository pushen][push-git-repo]

Überprüfen Sie Ihre E-Mail-Adresse, und wählen Sie in der Dropdownliste **Team Services-Domäne** Ihr Konto aus. Geben Sie den Repositorynamen ein, und wählen Sie **Repository veröffentlichen** aus.

![Git-Repository pushen][publish-code]

Durch das Veröffentlichen des Repositorys wird in Ihrem Konto ein neues Teamprojekt mit dem gleichen Namen wie das lokale Repository erstellt. Um das Repository in einem vorhandenen Teamprojekt zu erstellen, klicken Sie neben **Repositoryname** auf **Erweitert**, und wählen Sie ein Teamprojekt aus. Sie können den Code im Web anzeigen, indem Sie **Im Web anzeigen** auswählen.

## <a name="configure-continuous-delivery-with-vsts"></a>Konfigurieren von Continuous Delivery mit VSTS
Eine Team Services-Builddefinition beschreibt einen Workflow, der aus verschiedenen Buildschritten besteht, die nacheinander ausgeführt werden. Erstellen Sie eine Builddefinition, die ein Service Fabric-Anwendungspaket und andere Elemente erzeugt, um sie in einem Service Fabric-Cluster bereitzustellen. Weitere Informationen zu [Team Services-Builddefinitionen](https://www.visualstudio.com/docs/build/define/create). 

Eine Team Services-Releasedefinition beschreibt einen Workflow, der ein Anwendungspaket in einem Cluster bereitstellt. Bei gemeinsamer Verwendung führen die Builddefinition und Releasedefinition den gesamten Workflow aus, und zwar beginnend mit den Quelldateien und endend mit einer im Cluster ausgeführten Anwendung. Erfahren Sie mehr zu Team Services- [Releasedefinitionen](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-build-definition"></a>Erstellen einer Builddefinition
Navigieren Sie in einem Webbrowser zu Ihrem neuen Teamprojekt: [https://&lt;IhrKonto&gt;.visualstudio.com/Voting/Voting%20Team/_git/Voting](https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting). 

Wählen Sie auf der Registerkarte **Build und Release** die Option **Builds** und anschließend **+ Neue Definition** aus.  Wählen Sie unter **Vorlage auswählen** die Vorlage **Azure Service Fabric-Anwendung** aus, und klicken Sie anschließend auf **Übernehmen**. 

![Auswählen der Buildvorlage][select-build-template] 

Geben Sie unter **Aufgaben** für **Agent-Warteschlange** die Zeichenfolge „Hosted VS2017“ ein. 

![Auswählen von Aufgaben][save-and-queue]

Aktivieren Sie unter **Trigger** Continuous Integration durch Festlegen von **Triggerstatus**.  Klicken Sie auf **Speichern und in Warteschlange einreihen**, um manuell einen Buildvorgang zu starten.  

![Auswählen von Triggern][save-and-queue2]

Buildvorgänge werden auch bei Push- oder Eincheckvorgängen ausgelöst. Wechseln Sie zum Überprüfen des Buildstatus zur Registerkarte **Builds**.  Nachdem Sie überprüft haben, ob der Build erfolgreich ausgeführt wird, legen Sie eine Releasedefinition fest, mit der die Anwendung in einem Cluster bereitgestellt wird. 

### <a name="create-a-release-definition"></a>Erstellen einer Releasedefinition  

Wählen Sie auf der Registerkarte **Build und Release** die Option **Releases** und anschließend **+ Neue Definition** aus.  Wählen Sie in der Liste unter **Vorlage auswählen** die Vorlage **Azure Service Fabric-Bereitstellung** aus, und klicken Sie auf **Anwenden**.  

![Auswählen der Releasevorlage][select-release-template]

Klicken Sie auf **Aufgaben**->**Umgebung 1** und anschließend auf **+Neu**, um eine neue Clusterverbindung hinzuzufügen.

![Hinzufügen der Clusterverbindung][add-cluster-connection]

Wählen Sie in der Ansicht **Add new Service Fabric Connection** (Neue Service Fabric-Verbindung hinzufügen) die Authentifizierung **Zertifikatbasiert** oder **Azure Active Directory** aus.  Nennen Sie die Verbindung „mysftestcluster“, und geben Sie als Clusterendpunkt entweder „tcp://mysftestcluster.southcentralus.cloudapp.azure.com:19000“ (oder den Endpunkt des Zielclusters Ihrer Bereitstellung) an. 

Fügen Sie bei Verwendung der zertifikatbasierten Authentifizierung den **Zertifikatfingerabdruck des Servers** für das Serverzertifikat hinzu, das bei der Erstellung des Clusters verwendet wurde.  Fügen Sie unter **Clientzertifikat** die Base64-Codierung der Clientzertifikatdatei hinzu. Die Vorgehensweise zum Ermitteln der Base64-codierten Darstellung des Zertifikats wird in der kontextbezogenen Hilfe zu diesem Feld beschrieben. Fügen Sie auch das **Kennwort** für das Zertifikat hinzu.  Sollten Sie über kein separates Clientzertifikat verfügen, können Sie auch das Cluster- oder Serverzertifikat verwenden. 

Fügen Sie bei Verwendung von Azure Active Directory-Anmeldeinformationen den **Zertifikatfingerabdruck des Servers** für das Serverzertifikat hinzu, das bei der Erstellung des Clusters verwendet wurde, und geben Sie in den Feldern **Benutzername** und **Kennwort** die Anmeldeinformationen an, die beim Herstellen der Clusterverbindung verwendet werden sollen. 

Klicken Sie auf **Hinzufügen**, um die Clusterverbindung zu speichern.

Fügen Sie als Nächstes der Pipeline ein Buildartefakt hinzu, damit die Releasedefinition die Ausgabe des Builds findet. Klicken Sie auf **Pipeline** und anschließend auf **Artefakte**->**+Hinzufügen**.  Wählen Sie unter **Quelle (Builddefinition)** die zuvor erstellte Builddefinition aus.  Klicken Sie auf **Hinzufügen**, um das Buildartefakt zu speichern.

![Hinzufügen des Artefakts][add-artifact]

Aktivieren Sie einen Continuous Deployment-Trigger, damit nach Abschluss des Buildvorgangs automatisch ein Release erstellt wird. Klicken Sie im Artefakt auf das Blitzsymbol, aktivieren Sie den Trigger, und klicken Sie anschließend auf **Speichern**, um die Releasedefinition zu speichern.

![Aktivieren des Triggers][enable-trigger]

Wählen Sie **+ Release** -> **Release erstellen** -> **Erstellen** aus, um manuell ein Release zu erstellen.  Überprüfen Sie, ob die Bereitstellung erfolgreich war und die Anwendung im Cluster ausgeführt wird.  Öffnen Sie einen Webbrowser, und navigieren Sie zu [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Beachten Sie, dass die Anwendungsversion in diesem Beispiel „1.0.0.20170616.3“ lautet. 

## <a name="commit-and-push-changes-trigger-a-release"></a>Committen und Pushen von Änderungen, Auslösen eines Release
Wir checken einige Codeänderungen in Team Services ein, um zu überprüfen, ob die Continuous Integration-Pipeline ordnungsgemäß ausgeführt wird.    

Während Sie den Code schreiben, werden Ihre Änderungen automatisch von Visual Studio nachverfolgt. Führen Sie den Commit der Änderungen in das lokale Git-Repository aus, indem Sie auf der Statusleiste unten rechts das Symbol „Ausstehende Änderungen“ (![Ausstehend][pending]) auswählen.

Fügen Sie in der Ansicht **Änderungen** in Team Explorer eine Nachricht hinzu, die die Aktualisierung beschreibt, und führen Sie den Commit der Änderungen aus.

![Commit für alle][changes]

Wählen Sie das Statusleistensymbol für nicht veröffentlichte Änderungen (![nicht unveröffentlichte Änderungen][unpublished-changes]) oder die Synchronisierungsansicht in Team Explorer aus. Wählen Sie **Push** aus, um den Code in Team Services/TFS zu aktualisieren.

![Änderungen pushen][push]

Durch das Pushen der Änderungen nach Team Services wird automatisch ein Build ausgelöst.  Nach erfolgreichem Abschluss der Builddefinition wird automatisch ein Release erstellt, und es wird ein Upgrade der Anwendung im Cluster gestartet.

Um den Buildstatus zu überprüfen, wechseln Sie in Visual Studio in **Team Explorer** zur Registerkarte **Builds**.  Nachdem Sie überprüft haben, ob der Build erfolgreich ausgeführt wird, legen Sie eine Releasedefinition fest, mit der die Anwendung in einem Cluster bereitgestellt wird.

Überprüfen Sie, ob die Bereitstellung erfolgreich war und die Anwendung im Cluster ausgeführt wird.  Öffnen Sie einen Webbrowser, und navigieren Sie zu [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Beachten Sie, dass die Anwendungsversion in diesem Beispiel 1.0.0.20170815.3 lautet.

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>Aktualisieren der Anwendung
Nehmen Sie in der Anwendung Codeänderungen vor.  Speichern und committen Sie die Änderungen, indem Sie die vorherigen Schritte ausführen.

Sobald das Upgrade der Anwendung gestartet wird, können Sie in Service Fabric Explorer den Upgradestatus beobachten:

![Service Fabric Explorer][sfx2]

Das Anwendungsupgrade kann mehrere Minuten dauern. Wenn das Upgrade abgeschlossen ist, wird die nächste Version der Anwendung ausgeführt.  In diesem Beispiel: 1.0.0.20170815.4

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Hinzufügen der Quellcodeverwaltung zum Projekt
> * Erstellen einer Builddefinition
> * Erstellen einer Releasedefinition
> * Automatisches Bereitstellen und Durchführen von Upgrades einer Anwendung

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Einrichten der Überwachung und Diagnose für die Anwendung](service-fabric-tutorial-monitoring-aspnet.md) 


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue.png
[save-and-queue2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue2.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/EnableTrigger.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
