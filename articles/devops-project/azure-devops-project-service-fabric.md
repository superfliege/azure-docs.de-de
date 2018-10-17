---
title: Bereitstellen einer ASP.NET Core-App in Azure Service Fabric mithilfe des Azure DevOps-Projekts | Azure DevOps Services-Tutorial
description: Das DevOps-Projekt erleichtert die ersten Schritte mit Azure. Mit dem Azure DevOps-Projekt können Sie Ihre ASP.NET Core-App mit Azure Service Fabric in wenigen Schritten bereitstellen.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 6a0539b2213b99e09021a4f90d914172eac62560
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44300394"
---
# <a name="tutorial--deploy-your-aspnet-core-app-to-azure-service-fabric-with-the-azure-devops-project"></a>Tutorial: Bereitstellen einer ASP.NET Core-App in Azure Service Fabric mithilfe des Azure DevOps-Projekts

Das Azure DevOps-Projekt stellt eine vereinfachte Umgebung dar, in der Sie Ihren vorhandenen Code und Ihr Git-Repository verwenden oder eine der Beispielanwendungen wählen können, um eine Continuous Integration- und Continuous Delivery-Pipeline (CI/CD) zu Azure zu erstellen.  Mit dem DevOps-Projekt werden automatisch Azure-Ressourcen wie Azure Service Fabric erstellt, in Azure DevOps eine Releasepipeline mit einer CI/CD-Pipeline erstellt und anschließend eine Azure Application Insights-Ressource für die Überwachung erstellt.

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts für eine ASP.NET Core-App mit Azure Service Fabric
> * Konfigurieren von Azure DevOps Services und eines Azure-Abonnements 
> * Überprüfen der CI-Pipeline
> * Überprüfen der CD-Pipeline
> * Ausführen eines Commits für Änderungen an Git und automatisches Bereitstellen in Azure
> * Bereinigen von Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.

## <a name="create-an-azure-devops-project-for-an-aspnet-core-app-and-service-fabric"></a>Erstellen eines Azure DevOps-Projekts für eine ASP.NET Core-App mit Azure Service Fabric

Mit dem Azure DevOps-Projekt wird eine CI/CD-Pipeline erstellt.  Sie können eine **neue Azure DevOps Services**-Organisation erstellen oder eine **bestehende Organisation** verwenden.  Ferner werden mit dem Azure DevOps-Projekt **Azure-Ressourcen** wie ein Azure Service Fabric-Cluster im **Azure-Abonnement** Ihrer Wahl erstellt.

1. Melden Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie in der linken Navigationsleiste das Symbol **Ressource erstellen** aus, und suchen Sie anschließend nach **DevOps-Projekt**.  Wählen Sie **Erstellen**.

    ![Starten von Continuous Delivery](_img/azure-devops-project-service-fabric/fullbrowser.png)

1. Wählen Sie **.NET** und dann **Weiter** aus.

1. Wählen Sie für **Anwendungsframework auswählen** die Option **ASP.NET Core** und anschließend **Weiter** aus.

1. Wählen Sie **Service Fabric-Cluster** und anschließend **Weiter** aus.  

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Konfigurieren von Azure DevOps Services und eines Azure-Abonnements

1. Erstellen Sie eine **neue Azure DevOps Services**-Organisation, oder verwenden Sie eine **bestehende Organisation**.  Wählen Sie einen **Namen** für das Azure DevOps-Projekt.  

1. Wählen Sie Ihr Azure-**Abonnement** aus.

1. Wählen Sie den Link **Änderung** aus, um zusätzliche Azure-Konfigurationseinstellungen anzuzeigen. Geben Sie dann die **VM-Größe des Knotens** und das **Betriebssystem** für den  **Service Fabric-Cluster** an.  Es gibt verschiedene Optionen zum Konfigurieren von Typ und Speicherort der Azure-Dienste.
 
1. Beenden Sie den Konfigurationsbereich von Azure, und klicken Sie auf **Fertig**.

1. Dieser Prozess dauert einige Minuten.  In Ihrer Azure DevOps Services-Organisation wird eine ASP.NET Core-Beispielanwendung in einem Repository eingerichtet. Ein Service Fabric-Cluster wird erstellt, eine CI/CD-Pipeline wird ausgeführt, und Ihre Anwendung wird in Azure bereitgestellt.  

    Sobald der Prozess abgeschlossen ist, wird das **Azure DevOps-Projektdashboard** im Azure-Portal geladen.  Über **Alle Ressourcen** können Sie das **Azure DevOps-Projektdashboard** auch direkt im **Azure-Portal** laden.  

    Dieses Dashboard bietet Einblick in Ihr Azure DevOps Services-**Coderepository**, in Ihre **Azure DevOps Services-CI/CD-Pipeline** und in Ihren **Service Fabric-Cluster**.  Sie können zusätzliche Optionen in Azure DevOps Services konfigurieren.  Wählen Sie auf der rechten Seite des Dashboards **Durchsuchen**, um Ihre aktive Anwendung anzuzeigen.

## <a name="examine-the-azure-devops-services-ci-pipeline"></a>Überprüfen der Azure DevOps Services-CI-Pipeline

Mit dem Azure DevOps-Projekt wird in Ihrer Azure DevOps Services-Organisation automatisch eine Azure-CI/CD-Pipeline erstellt.  Sie können die Pipeline untersuchen und anpassen.  Gehen Sie wie folgt vor, um sich mit der Azure DevOps Services-Buildpipeline vertraut zu machen.

1. Navigieren Sie zum **Azure DevOps-Projektdashboard**.

1. Wählen Sie **oben** im **Azure DevOps-Projektdashboard** **Pipelines erstellen** aus.  Dieser Link öffnet eine Browserregisterkarte und die Azure DevOps Services-Buildpipeline für Ihr neues Projekt.

1. Verschieben Sie den Cursor rechts neben die Buildpipeline neben das Feld **Status**. Wählen Sie die **Auslassungspunkte** aus, die angezeigt werden.  Mit dieser Aktion wird ein Menü geöffnet, über das Sie verschiedene Aktivitäten starten können. So können Sie beispielsweise einen **neuen Build zur Warteschlange hinzufügen**, einen **Build anhalten** und die **Buildpipeline bearbeiten**.

1. Wählen Sie **Bearbeiten** aus.

1. In dieser Ansicht können Sie sich **die verschiedenen Aufgaben ansehen**, die Sie für Ihre Buildpipeline durchführen können.  Vom Build werden verschiedene Aufgaben durchgeführt. So werden beispielsweise Quellen aus dem Azure DevOps Services-Git-Repository abgerufen, Abhängigkeiten wiederhergestellt und für Bereitstellungen verwendete Ausgaben veröffentlicht.

1. Wählen Sie oben in der Buildpipeline den **Buildpipelinenamen**. 

1. Wählen Sie unter dem Buildpipelinenamen **Verlauf** aus.  Es wird ein Überwachungspfad mit den letzten Änderungen für den Build angezeigt.  An der Buildpipeline vorgenommene Änderungen werden von Azure DevOps Services überwacht, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus.  Mit dem Azure DevOps-Projekt wurde automatisch ein CI-Trigger erstellt, und mit jedem Commit, der für das Repository ausgeführt wird, wird ein neuer Build gestartet.  Optional können Sie Branches aus dem CI-Prozess einbeziehen oder ausschließen.

1. Wählen Sie **Aufbewahrung** aus.  Je nach Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

## <a name="examine-the-azure-devops-services-cd-pipeline"></a>Überprüfen der Azure DevOps Services-CD-Pipeline

Mit dem Azure DevOps-Projekt werden die erforderlichen Schritte zum Bereitstellen über Ihre Azure DevOps Services-Organisation in Ihrem Azure-Abonnement automatisch erstellt und konfiguriert.  Diese Schritte umfassen die Konfiguration einer Azure-Dienstverbindung zur Authentifizierung von Azure DevOps Services für Ihr Azure-Abonnement.  Die Automatisierung erstellt auch eine Azure DevOps Services-Releasepipeline, die wiederum die CD-Bereitstellung an Azure ausführt.  Gehen Sie wie folgt vor, um mehr über die Azure DevOps Services-Releasepipeline zu erfahren.

1. Wählen Sie **Build und Release** und anschließend **Releases** aus.  Mit dem Azure DevOps-Projekt wurde eine Azure DevOps Services-Releasepipeline zum Verwalten von Bereitstellungen für Azure erstellt.

1. Wählen Sie links im Browser die **Auslassungspunkte** neben Ihrer Releasepipeline und anschließend **Bearbeiten** aus.

1. Die Releasepipeline enthält eine **Pipeline**, die den Releaseprozess definiert.  Wählen Sie unter **Artefakte** die Option **Ablegen** aus.  Die in den vorherigen Schritten untersuchte Buildpipeline erzeugt die für das Artefakt verwendete Ausgabe. 

1. Wählen Sie die Option **Continuous Deployment-Trigger** rechts neben dem **Symbol** **Ablegen** (das als Gewitterblitz dargestellt wird) aus.  Diese Releasepipeline verfügt über einen aktivierten CD-Trigger.  Der Trigger wird jedes Mal ausgelöst, wenn ein neues Buildartefakt verfügbar ist.  Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen. 

1. Wählen Sie rechts im Browser **Releases anzeigen** aus.  In dieser Ansicht wird der Verlauf von Releases angezeigt.

1. Wählen Sie die **Auslassungsp** neben einem Ihrer Releases und dann die Option **Öffnen** aus.  In dieser Ansicht finden Sie verschiedene Menüs, wie etwa eine **Releasezusammenfassung**, **zugeordnete Arbeitsaufgaben** und **Tests**.

1. Wählen Sie **Commits** aus.  In dieser Ansicht werden die der jeweiligen Bereitstellung zugeordneten Codecommits angezeigt. Sie können Releases vergleichen, um die Commitunterschiede zwischen den einzelnen Bereitstellungen anzuzeigen.

1. Wählen Sie **Protokolle** aus.  Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess.  Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="commit-changes-to-git-and-automatically-deploy-to-azure"></a>Ausführen eines Commits für Änderungen an Git und automatisches Bereitstellen in Azure 

 > [!NOTE]
 > Mit den folgenden Schritten wird die CI/CD-Pipeline mit einer einfachen Textänderung an Ihrer Web-App getestet.

Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit auf Ihrer Website automatisch bereitgestellt wird, mit einem Team an Ihrer App zusammenarbeiten.  Durch jede Änderung an einem Git-Repository wird ein Build gestartet, und in Release stellt Ihre Änderungen in Azure bereit.  Gehen Sie wie folgt vor, um einen Commit für Änderungen an Ihrem Repository auszuführen, oder verwenden Sie hierfür andere Verfahren.  Sie können z.B. das Git-Repository in Ihrem Lieblingstool oder Ihre IDE **klonen** und anschließend Änderungen per Push in dieses Repository übertragen.

1. Wählen Sie im Azure DevOps Services-Menü den Eintrag **Code** und **Dateien** aus, und navigieren Sie zu Ihrem Repository.
1. Navigieren Sie zum Verzeichnis **Views\Home**. Wählen Sie anschließend die **Auslassungspunkte** neben der Datei **Index.cshtml** und dann **Bearbeiten** aus.

1. Nehmen Sie eine Änderung an der Datei vor. Ändern Sie beispielsweise den Text in einem der **div-Tags**.  Wählen Sie oben rechts **Commit** aus.  Wählen Sie erneut **Commit** aus, um die Änderung mittels Push zu übertragen. 

1. Kurz danach wird **ein Build gestartet**. Anschließend wird zur Bereitstellung der Änderungen ein Release ausgeführt.  Sie können den **Buildstatus** mit dem DevOps-Projektdashboard oder im Browser mit der Azure DevOps Services-Echtzeitprotokollierung überwachen.

1. Nachdem das Release ausgeführt wurde, **aktualisieren Sie Ihre Anwendung** im Browser, um sicherzustellen, dass Ihre Änderungen angezeigt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

 > [!NOTE]
 > Mit den nachfolgenden Schritten werden Ressourcen dauerhaft gelöscht.  Verwenden Sie diese Funktion erst, nachdem Sie die Eingabeaufforderungen aufmerksam durchgelesen haben.

Beim Durchführen von Tests können Sie Ressourcen bereinigen, um zu verhindern, dass Gebühren entstehen.  Sie können den mit diesem Tutorial erstellte Service Fabric-Cluster und die zugehörigen Ressourcen mithilfe der Funktion **Löschen** im Azure DevOps-Projektdashboard löschen, wenn Sie die Bereitstellung nicht mehr benötigen.  **Seien Sie vorsichtig**, denn mit der Löschfunktion werden die im Rahmen des Azure DevOps-Projekts erstellten Daten in Azure und Azure DevOps Services gelöscht. Diese Daten können anschließend nicht wiederhergestellt werden.

1. Navigieren Sie im **Azure-Portal** zum **Azure DevOps-Projekt**.
2. Wählen Sie **oben rechts** im Dashboard die Option **Löschen** aus.  Nachdem Sie die Eingabeaufforderung gelesen haben, wählen Sie **Ja** aus, um die Ressourcen **dauerhaft zu löschen**.

## <a name="next-steps"></a>Nächste Schritte

Diese CI/CD-Pipeline können Sie optional an die Anforderungen Ihres Teams anpassen. Sie können dieses CI/CD-Muster auch als Vorlage für Ihre anderen Projekte verwenden.  Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts für eine ASP.NET Core-App mit Azure Service Fabric
> * Konfigurieren von Azure DevOps Services und eines Azure-Abonnements 
> * Überprüfen der CI-Pipeline
> * Überprüfen der CD-Pipeline
> * Ausführen eines Commits für Änderungen an Git und automatisches Bereitstellen in Azure
> * Bereinigen von Ressourcen

Weiter Informationen zu Service Fabric und Microservices finden Sie hier:

> [!div class="nextstepaction"]
> [Use a microservices approach for building applications (Verwenden eines Microservices-Ansatzes zum Erstellen von Anwendungen)](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
