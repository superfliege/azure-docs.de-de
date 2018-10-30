---
title: Erstellen einer CI-/CD-Pipeline für vorhandenen Code mit Azure DevOps-Projekt | Azure DevOps Services-Tutorial
description: Das DevOps-Projekt erleichtert die ersten Schritte mit Azure. Damit können Sie Ihren eigenen Code und das GitHub-Repository verwenden, um eine App in einem Azure-Dienst Ihrer Wahl in wenigen Schritten zu starten.
services: vsts
documentationcenter: vs-devops-build
ms.author: mlearned
ms.manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.prod: devops
ms.technology: devops-cicd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 02b6823a46c94edb0ba28c7a2a8b9ae0efc44ae8
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406091"
---
# <a name="tutorial--create-a-cicd-pipeline-for-your-existing-code-with-the-azure-devops-project"></a>Tutorial: Erstellen einer CI/CD-Pipeline für vorhandenen Code mit dem Azure DevOps-Projekt

Das Azure DevOps-Projekt stellt eine vereinfachte Umgebung dar, in der Sie Ihren vorhandenen Code und Ihr Git-Repository verwenden oder eine der Beispielanwendungen wählen können, um eine Continuous Integration (CI)- und Continuous Delivery (CD)-Pipeline zu Azure zu erstellen.

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts
> * Konfigurieren des Zugriffs auf Ihr GitHub-Repository und Wahl eines Frameworks
> * Konfigurieren von Azure DevOps Services und eines Azure-Abonnements 
> * Ausführen eines Commits für Änderungen an GitHub und automatisches Bereitstellen in Azure
> * Überprüfen der Azure DevOps Services-CI/CD-Pipeline
> * Konfigurieren der Azure Application Insights-Überwachung

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.
* Zugriff auf ein GitHub- oder externes Git-Repository, das .NET, Java, PHP, Node, Python oder statischen Webcode enthält

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Mit dem Azure DevOps-Projekt wird in Azure DevOps Services eine CI/CD-Pipeline erstellt.  Sie können eine **neue Azure DevOps Services**-Organisation erstellen oder eine **bestehende Organisation** verwenden.  Außerdem werden mit dem Azure DevOps-Projekt **Azure-Ressourcen** im **Azure-Abonnement** Ihrer Wahl erstellt.

1. Melden Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie in der linken Navigationsleiste das Symbol **+ Neu** aus, und suchen Sie anschließend nach **DevOps-Projekt**.  Wählen Sie **Erstellen**.

    ![Starten von Continuous Delivery](_img/azure-devops-project-github/fullbrowser.png)

1. Wählen Sie die Option **Verwenden von eigenem Code** aus.  Wählen Sie **Weiter** aus, wenn Sie fertig sind.

## <a name="configure-access-to-your-github-repository-and-choose-a-framework"></a>Konfigurieren des Zugriffs auf Ihr GitHub-Repository und Wahl eines Frameworks

1. Wählen Sie **GitHub** aus.  Optional können Sie ein **externes Git-Repository** auswählen.  Wählen Sie Ihr **Repository** und Ihren **Branch** aus, das bzw. der Ihre Anwendung enthält.

1. Wählen Sie Ihr **Webframework** aus.  Wählen Sie **Weiter** aus, wenn Sie fertig sind.

    ![.NET Framework](_img/azure-devops-project-github/webframework.png)

1. Das Anwendungsframework, das Sie im vorherigen Schritt ausgewählt haben, bestimmt den Typ des hier verfügbaren Bereitstellungsziels für den Azure-Dienst.  Wählen Sie den **Zieldienst** Ihrer Wahl aus.  Wählen Sie **Weiter** aus, wenn Sie fertig sind.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Konfigurieren von Azure DevOps Services und eines Azure-Abonnements 

1. Erstellen Sie eine **neue** Azure DevOps Services-Organisation, oder verwenden Sie eine **bestehende** Organisation.  Wählen Sie einen **Namen** für das Azure DevOps-Projekt aus.  Wählen Sie Ihr **Azure-Abonnement**, den **Speicherort** und einen **Namen** für Ihre Anwendung aus.  Klicken Sie auf **Fertig**, wenn Sie fertig sind.

1. In wenigen Minuten wird das **Azure DevOps-Projektdashboard** im Azure-Portal geladen.  Eine Beispielanwendung wird in einem Repository in Ihrer Azure DevOps Services-Organisation eingerichtet. Ein Build wird ausgeführt, und Ihre Anwendung wird in Azure bereitgestellt.  Dieses Dashboard bietet Einblick in Ihr GitHub-**Coderepository**, Ihre **Azure DevOps Services-CI/CD-Pipeline** und Ihre **Anwendung in Azure**.  Wählen Sie auf der rechten Seite des Dashboards **Durchsuchen**, um Ihre aktive Anwendung anzuzeigen.

    ![Dashboardansicht](_img/azure-devops-project-github/dashboardnopreview.png) 
    
Mit dem Azure DevOps-Projekt werden automatisch ein CI-Build- und -Releasetrigger konfiguriert.  Ihr Code verbleibt in Ihrem GitHub- oder einem anderen externen Repository.  

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Ausführen eines Commits für Änderungen an GitHub und automatisches Bereitstellen in Azure 

Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit auf Ihrer Website automatisch bereitgestellt wird, mit einem Team an Ihrer App zusammenarbeiten.  Mit jeder Änderung am GitHub-Repository wird in Azure DevOps ein Build gestartet. Ferner wird durch eine Azure DevOps-CD-Pipeline eine Bereitstellung in Azure ausgeführt.

1.  Nehmen Sie eine Änderung an Ihrer Anwendung vor, und führen Sie einen **Commit** der Änderungen an Ihrem GitHub-Repository durch.
2.  In wenigen Augenblicken wird in Azure DevOps Services ein Build gestartet.  Sie können den Buildstatus mit dem Azure DevOps-Projektdashboard oder im Browser mit Ihrer Azure DevOps Services-Organisation überwachen.
3.  Nachdem der Build erfolgreich ausgeführt wurde, **aktualisieren Sie Ihre Anwendung** im Browser, um sicherzustellen, dass Ihre Änderungen angezeigt werden.

## <a name="examine-the-azure-devops-services-cicd-pipeline"></a>Überprüfen der Azure DevOps Services-CI/CD-Pipeline

Mit dem Azure DevOps-Projekt wird in Ihrer Azure DevOps Services-Organisation automatisch eine Azure DevOps Services-CI/CD-Pipeline erstellt.  Untersuchen Sie die Pipeline, und passen Sie sie bei Bedarf an.  Gehen Sie folgendermaßen vor, um sich mit den Build- und Releasepipelines von Azure DevOps Services vertraut zu machen.

1. Wählen Sie **oben** im Azure DevOps-Projektdashboard **Pipelines erstellen** aus.  Dieser Link öffnet eine Browserregisterkarte und die Azure DevOps Services-Buildpipeline für Ihr neues Projekt.

1. Verschieben Sie den Cursor rechts neben die Buildpipeline neben das Feld **Status**. Wählen Sie die **Auslassungspunkte** aus, die angezeigt werden.  Mit dieser Aktion wird ein Menü geöffnet, über das Sie verschiedene Aktivitäten starten können. So können Sie beispielsweise einen neuen Build zur Warteschlange hinzufügen, einen Build anhalten und die Buildpipeline bearbeiten.

1. Wählen Sie **Bearbeiten** aus.

1. In dieser Ansicht können Sie sich **die verschiedenen Aufgaben ansehen**, die Sie für Ihre Buildpipeline durchführen können.  Vom Build werden verschiedene Aufgaben durchgeführt. So werden beispielsweise Quellen aus dem Git-Repository abgerufen, Abhängigkeiten wiederhergestellt und für Bereitstellungen verwendete Ausgaben veröffentlicht.

1. Wählen Sie oben in der Buildpipeline den **Buildpipelinenamen**.

1. Ersetzen Sie den **Namen** Ihrer Buildpipeline durch einen aussagekräftigeren Namen.  Wählen Sie **Speichern und in Warteschlange einreihen** und dann **Speichern** aus.

1. Wählen Sie unter dem Buildpipelinenamen **Verlauf** aus.  Es wird ein Überwachungspfad mit den letzten Änderungen für den Build angezeigt.  An der Buildpipeline vorgenommene Änderungen werden von Azure DevOps Services überwacht, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus.  Mit dem Azure DevOps-Projekt wurde automatisch ein CI-Trigger erstellt, und mit jedem Commit, der für das Repository ausgeführt wird, wird ein neuer Build gestartet.  Optional können Sie Branches aus dem CI-Prozess einbeziehen oder ausschließen.

1. Wählen Sie **Aufbewahrung** aus.  Je nach Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

1. Wählen Sie **Build und Release** und anschließend **Releases** aus.  Mit dem Azure DevOps-Projekt wurde eine Azure DevOps Services-Releasepipeline zum Verwalten von Bereitstellungen für Azure erstellt.

1. Wählen Sie links im Browser die **Auslassungspunkte** neben Ihrer Releasepipeline und anschließend **Bearbeiten** aus.

1. Die Releasepipeline enthält eine **Pipeline**, die den Releaseprozess definiert.  Wählen Sie unter **Artefakte** die Option **Ablegen** aus.  Die in den vorherigen Schritten untersuchte Buildpipeline erzeugt die für das Artefakt verwendete Ausgabe. 

1. Wählen Sie die Option **Continuous Deployment-Trigger** rechts neben dem Symbol **Ablegen** aus.  Diese Releasepipeline enthält einen aktivierten CD-Trigger. Jedes Mal, wenn ein neues Buildartefakt verfügbar ist, wird von diesem CD-Trigger eine Bereitstellung ausgeführt.  Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen. 

1. Wählen Sie links im Browser die Option **Aufgaben** aus.  Bei den Aufgaben handelt es sich um die Aktivitäten, die beim Bereitstellungsprozess durchgeführt werden.  In diesem Beispiel wurde für die Bereitstellung für **Azure App Service** eine Aufgabe erstellt.

1. Wählen Sie rechts im Browser **Releases anzeigen** aus.  In dieser Ansicht wird der Verlauf von Releases angezeigt.

1. Wählen Sie die **Auslassungsp** neben einem Ihrer Releases und dann die Option **Öffnen** aus.  In dieser Ansicht finden Sie verschiedene Menüs, wie etwa eine Releasezusammenfassung, zugeordnete Arbeitsaufgaben und Tests.

1. Wählen Sie **Commits** aus.  In dieser Ansicht werden die der jeweiligen Bereitstellung zugeordneten Codecommits angezeigt. 

1. Wählen Sie **Protokolle** aus.  Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess.  Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="configure-azure-application-insights-monitoring"></a>Konfigurieren der Azure Application Insights-Überwachung

Mithilfe von Azure Application Insights können Sie die Leistung und Nutzung Ihrer Anwendung ganz einfach überwachen.  Mit dem Azure DevOps-Projekt wird für Ihre Anwendung automatisch eine Application Insights-Ressource konfiguriert.  Sie können Warnungen und Überwachungsfunktionen je nach Bedarf weiter konfigurieren.

1. Navigieren Sie im Azure-Portal zum **Azure DevOps-Projekt**.  Wählen Sie am unteren rechten Rand des Dashboards den Link zu **Application Insights** für Ihre App.

1. Das Blatt **Application Insights** wird im Azure-Portal geöffnet.  Diese Ansicht enthält Informationen zur Nutzungs-, Leistungs- und Verfügbarkeitsüberwachung für Ihre App.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. Wählen Sie **Zeitbereich** und dann **Letzte Stunde** aus.  Wählen Sie **Aktualisieren** aus, um die Ergebnisse zu filtern.  Jetzt werden alle Aktivitäten der letzten 60 Minuten angezeigt.  Wählen Sie das **x** aus, um den Zeitbereich zu verlassen.

1. Wählen Sie **Warnungen** und anschließend **+ Metrikwarnung hinzufügen** aus.  

1. Geben Sie einen **Namen** für die Warnung ein.

1. Wählen Sie die Dropdownliste für **Source Alter on** (Quellwarnung auf) aus.  Wählen Sie Ihre **App Service-Ressource** aus.
<!-- Could you please confirm if this should be "Source Alter on" instead of "Source Alert on"? -->
1. Für die Standardwarnung gilt eine **Serverantwortzeit, die größer als 1 Sekunde** ist.  Wählen Sie die Dropdownliste **Metrik** aus, um die verschiedenen Warnungsmetriken anzuzeigen.  Sie können problemlos eine Vielzahl von Warnungen konfigurieren und dadurch die Überwachungsfunktionen Ihrer App verbessern.

1. Aktivieren Sie das Kontrollkästchen für **Notify via Email owners, contributors, and readers** (Besitzer, Mitwirkende und Leser über E-Mail informieren).  Optional können Sie zusätzliche Aktionen ausführen, wenn eine Warnung beim Ausführen einer Azure-Logik-App ausgelöst wird.

1. Wählen Sie **OK** aus, um die Warnung zu erstellen.  In wenigen Augenblicken wird die Warnung als aktiv auf dem Dashboard angezeigt.  **Verlassen** Sie den Warnungsbereich, und navigieren Sie zurück zum Blatt **Application Insights**.

1. Wählen Sie **Verfügbarkeit** und anschließend **+ Test hinzufügen** aus. 

1. Geben Sie einen **Testnamen** ein, und wählen Sie dann **Erstellen** aus.  Ein einfacher Ping-Test wird erstellt, um die Verfügbarkeit Ihrer Anwendung zu überprüfen.  Nach wenigen Minuten sind die Testergebnisse verfügbar, und das Application Insights-Dashboard zeigt einen Verfügbarkeitsstatus an.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die mit dieser Schnellstartanleitung erstellte Azure App Service-Bereitstellung und die zugehörigen Ressourcen mithilfe der Funktion **Löschen** im Azure DevOps-Projektdashboard löschen, wenn Sie die Bereitstellung nicht mehr benötigen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie den CI/CD-Prozess in diesem Tutorial konfiguriert haben, wurden in Ihrem Azure DevOps-Projekt automatisch eine Build- und eine Releasepipeline erstellt. Diese Build- und Releasepipelines können Sie den Anforderungen Ihres Teams anpassen. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts
> * Konfigurieren des Zugriffs auf Ihr GitHub-Repository und Wahl eines Frameworks
> * Konfigurieren von Azure DevOps Services und eines Azure-Abonnements 
> * Ausführen eines Commits für Änderungen an GitHub und automatisches Bereitstellen in Azure
> * Überprüfen der Azure DevOps Services-CI/CD-Pipeline
> * Konfigurieren der Azure Application Insights-Überwachung

Weitere Informationen zur Azure DevOps Services-CI/CD-Pipeline finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Anpassen von CD-Prozessen](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
