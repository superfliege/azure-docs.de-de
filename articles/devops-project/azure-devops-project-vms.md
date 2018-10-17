---
title: Bereitstellen einer ASP.NET-App in Azure Virtual Machines mithilfe des Azure DevOps-Projekts | Azure DevOps Services-Tutorial
description: Das DevOps-Projekt erleichtert die ersten Schritte mit Azure. Mit dem Azure DevOps-Projekt können Sie Ihre ASP.NET-App in Azure Virtual Machines in wenigen Schritten bereitstellen.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: b05e2c2c46aa9bfa8c92d3d3c5c83d018c547b9f
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299133"
---
# <a name="tutorial--deploy-your-aspnet-app-to-azure-virtual-machines-with-the-azure-devops-project"></a>Tutorial: Bereitstellen einer ASP.NET-App in Azure Virtual Machines mithilfe des Azure DevOps-Projekts

Das Azure DevOps-Projekt stellt eine vereinfachte Umgebung dar, in der Sie Ihren vorhandenen Code und Ihr Git-Repository verwenden oder eine der Beispielanwendungen wählen können, um eine Continuous Integration- und Continuous Delivery-Pipeline (CI/CD) zu Azure zu erstellen.  Mit dem DevOps-Projekt werden Azure-Ressourcen wie ein neuer virtueller Azure-Computer automatisch erstellt, eine Releasepipeline in Azure DevOps mit einer Buildpipeline für CI erstellt und konfiguriert, eine Releasepipeline für CD eingerichtet und anschließend eine Azure Application Insights-Ressource für die Überwachung erstellt.

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts für eine ASP.NET-App
> * Konfigurieren von Azure DevOps Services und eines Azure-Abonnements 
> * Überprüfen der Azure DevOps Services-CI-Pipeline
> * Überprüfen der Azure DevOps Services-CD-Pipeline
> * Committen von Änderungen zu Azure DevOps Services und automatisches Bereitstellen in Azure
> * Konfigurieren der Azure Application Insights-Überwachung
> * Bereinigen von Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.

## <a name="create-an-azure-devops-project-for-an-aspnet-app"></a>Erstellen eines Azure DevOps-Projekts für eine ASP.NET-App

Mit dem Azure DevOps-Projekt wird in Azure eine CI/CD-Pipeline erstellt.  Sie können eine **neue Azure DevOps Services**-Organisation erstellen oder eine **bestehende Organisation** verwenden.  Ferner werden mit dem Azure DevOps-Projekt **Azure-Ressourcen** wie etwa virtuelle Computer im **Azure-Abonnement** Ihrer Wahl erstellt.

1. Melden Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie in der linken Navigationsleiste das Symbol **+ Neu** aus, und suchen Sie anschließend nach **DevOps-Projekt**.  Wählen Sie **Erstellen**.

    ![Starten von Continuous Delivery](_img/azure-devops-project-github/fullbrowser.png)

1. Wählen Sie **.NET** und dann **Weiter** aus.

1. Wählen Sie für **Anwendungsframework auswählen** die Option **ASP.NET** und anschließend **Weiter** aus. 

1. Das Anwendungsframework, das Sie im vorherigen Schritt ausgewählt haben, bestimmt den Typ des hier verfügbaren Bereitstellungsziels für den Azure-Dienst.  Wählen Sie den **virtuellen Computer** und anschließend **Weiter** aus.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Konfigurieren von Azure DevOps Services und eines Azure-Abonnements

1. Erstellen Sie eine **neue Azure DevOps Services**-Organisation, oder verwenden Sie eine **bestehende Organisation**.  Wählen Sie einen **Namen** für das Azure DevOps-Projekt.  

1. Wählen Sie Ihren **Azure-Abonnementsdienst** aus.  Optional können Sie den Link **Ändern** auswählen und anschließend weitere Konfigurationsdetails wie etwa einen anderen Speicherort für die Azure-Ressourcen eingeben.
 
1. Geben Sie den **Namen des virtuellen Computers**, den **Benutzernamen** und das **Kennwort** für Ihre neue Azure Virtual Machine-Ressource ein, und wählen Sie anschließend **Fertig** aus.

1. Es dauert einige Minuten, bis der virtuelle Azure-Computer bereit ist.  In Ihrer Azure DevOps Services-Organisation wird eine ASP.NET-Beispielanwendung in einem Repository eingerichtet. Ein Build und ein Release werden ausgeführt, und Ihre Anwendung wird im neu erstellten virtuellen Azure-Computer bereitgestellt.  

    Sobald der Prozess abgeschlossen ist, wird das **Azure DevOps-Projektdashboard** im Azure-Portal geladen.  Über **Alle Ressourcen** können Sie das **Azure DevOps-Projektdashboard** auch direkt im **Azure-Portal** laden.  

    Dieses Dashboard bietet Einblick in Ihr Azure DevOps Services-**Coderepository**, in Ihre **Azure-CI/CD-Pipeline** und in Ihre aktive **Anwendung in Azure**.    

    ![Dashboardansicht](_img/azure-devops-project-vms/dashboardnopreview.png)

1. Mit dem Azure DevOps-Projekt wird automatisch ein CI-Build- und -Releasetrigger konfiguriert, durch den Codeänderungen automatisch im Repository bereitgestellt werden.  Sie können zusätzliche Optionen in Azure DevOps konfigurieren.  Wählen Sie auf der rechten Seite des Dashboards **Durchsuchen**, um Ihre aktive Anwendung anzuzeigen.
    
## <a name="examine-the-azure-devops-services-ci-pipeline"></a>Überprüfen der Azure DevOps Services-CI-Pipeline
 
Mit dem Azure DevOps-Projekt wird in Ihrer Azure DevOps Services-Organisation automatisch eine vollständige Azure-CI/CD-Pipeline erstellt.  Sie können die Pipeline untersuchen und anpassen.  Gehen Sie wie folgt vor, um sich mit der Azure DevOps Services-Buildpipeline vertraut zu machen.

1. Wählen Sie **oben** im **Azure DevOps-Projektdashboard** **Pipelines erstellen** aus.  Dieser Link öffnet eine Browserregisterkarte und die Azure DevOps Services-Buildpipeline für Ihr neues Projekt.

1. Verschieben Sie den Cursor rechts neben die Buildpipeline neben das Feld **Status**. Wählen Sie die **Auslassungspunkte** aus, die angezeigt werden.  Mit dieser Aktion wird ein Menü geöffnet, über das Sie verschiedene Aktivitäten durchführen können. So können Sie beispielsweise **einen neuen Build zur Warteschlange hinzufügen**, **einen Build anhalten** und **die Buildpipeline bearbeiten**.

1. Wählen Sie **Bearbeiten** aus.

1. In dieser Ansicht können Sie sich **die verschiedenen Aufgaben ansehen**, die Sie für Ihre Buildpipeline durchführen können.  Vom Build werden verschiedene Aufgaben durchgeführt. So werden beispielsweise Quellen aus dem Azure DevOps Services-Git-Repository abgerufen, Abhängigkeiten wiederhergestellt und für Bereitstellungen verwendete Ausgaben veröffentlicht.

1. Wählen Sie oben in der Buildpipeline den **Buildpipelinenamen**.

1. Ersetzen Sie den **Namen** Ihrer Buildpipeline durch einen aussagekräftigeren Namen.  Wählen Sie **Speichern und in Warteschlange einreihen** und dann **Speichern** aus.

1. Wählen Sie unter dem Buildpipelinenamen **Verlauf** aus.  Es wird ein Überwachungspfad mit den letzten Änderungen für den Build angezeigt.  An der Buildpipeline vorgenommene Änderungen werden von Azure DevOps Services überwacht, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus.  Mit dem Azure DevOps-Projekt wurde automatisch ein CI-Trigger erstellt, und mit jedem Commit, der für das Repository ausgeführt wird, wird ein neuer Build gestartet.  Optional können Sie Branches aus dem CI-Prozess einbeziehen oder ausschließen.

1. Wählen Sie **Aufbewahrung** aus.  Je nach Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

## <a name="examine-the-azure-devops-services-cd-pipeline"></a>Überprüfen der Azure DevOps Services-CD-Pipeline

Mit dem Azure DevOps-Projekt werden die erforderlichen Schritte zum Bereitstellen über Ihre Azure DevOps Services-Organisation in Ihrem Azure-Abonnement automatisch erstellt und konfiguriert.  Diese Schritte umfassen die Konfiguration einer Azure-Dienstverbindung zur Authentifizierung von Azure DevOps Services für Ihr Azure-Abonnement.  Die Automatisierung erstellt auch eine Azure DevOps Services-CI-Pipeline, die wiederum die CD-Bereitstellung an den virtuellen Azure-Computer ausführt.  Gehen Sie wie folgt vor, um mehr über die Azure DevOps Services-CD-Pipeline zu erfahren.

1. Wählen Sie **Build und Release** und anschließend **Releases** aus.  Mit dem Azure DevOps-Projekt wurde eine Azure DevOps Services-Releasepipeline zum Verwalten von Bereitstellungen für Azure erstellt.

1. Wählen Sie links im Browser die **Auslassungspunkte** neben Ihrer Releasepipeline und anschließend **Bearbeiten** aus.

1. Die Releasepipeline enthält eine **Pipeline**, die den Releaseprozess definiert.  Wählen Sie unter **Artefakte** die Option **Ablegen** aus.  Die in den vorherigen Schritten untersuchte Buildpipeline erzeugt die für das Artefakt verwendete Ausgabe. 

1. Wählen Sie die Option **Continuous Deployment-Trigger** rechts neben dem **Symbol** **Ablegen** (das als Gewitterblitz dargestellt wird) aus.  Diese Releasepipeline verfügt über einen aktivierten CD-Trigger.  Der Trigger wird jedes Mal ausgelöst, wenn ein neues Buildartefakt verfügbar ist.  Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen. 

1. Wählen Sie links im Browser **Aufgaben** und anschließend Ihre **Umgebung** aus.  

1. Bei den Aufgaben handelt es sich um die Aktivitäten, die beim Bereitstellungsprozess durchgeführt werden. Sie sind in **Phasen** gruppiert.  Für diese Releasepipeline gibt es zwei **Phasen**.  Die erste Phase enthält die Aufgabe **Bereitstellung einer Azure-Ressourcengruppe**. Mit dieser Aufgabe wird der virtuelle Computer für die Bereitstellung konfiguriert und der neue virtuelle Computer einer **Azure DevOps-Bereitstellungsgruppe** hinzugefügt.  Mit der VM-Bereitstellungsgruppe in Azure DevOps werden logische Gruppen von **Bereitstellungszielcomputern** verwaltet.

1. In dieser zweiten Phase wurde die Aufgabe **IIS-Web-App verwalten** zum Erstellen einer IIS-Website auf dem virtuellen Computer erstellt.  Die zweite **IIS-Web-App-Bereitstellung**-Aufgabe wurde zur Bereitstellung der Website erstellt.

1. Wählen Sie rechts im Browser **Releases anzeigen** aus.  In dieser Ansicht wird der Verlauf von Releases angezeigt.

1. Wählen Sie die **Auslassungsp** neben einem Ihrer Releases und dann die Option **Öffnen** aus.  In dieser Ansicht finden Sie verschiedene Menüs, wie etwa eine **Releasezusammenfassung**, **zugeordnete Arbeitsaufgaben** und **Tests**.

1. Wählen Sie **Commits** aus.  In dieser Ansicht werden die der jeweiligen Bereitstellung zugeordneten Codecommits angezeigt. Vergleichen Sie Releases, um die Commitunterschiede zwischen den einzelnen Bereitstellungen anzuzeigen.

1. Wählen Sie **Protokolle** aus.  Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess.  Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="commit-changes-to-azure-devops-services-and-automatically-deploy-to-azure"></a>Committen von Änderungen zu Azure DevOps Services und automatisches Bereitstellen in Azure 

Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit auf Ihrer Website automatisch bereitgestellt wird, mit einem Team an Ihrer App zusammenarbeiten.  Jede Änderung am Azure DevOps-Services-Git-Repository startet einen Build in Azure DevOps Services, und eine Azure DevOps Services-CD-Pipeline übernimmt die Bereitstellung für Ihre Azure-VM.  Gehen Sie wie folgt vor, um einen Commit für Änderungen an Ihrem Repository auszuführen, oder verwenden Sie hierfür andere Verfahren.  Mit den Codeänderungen wird der CI/CD-Prozess initiiert. Ferner werden die neuen Änderungen an der IIS-Website automatisch auf dem virtuellen Azure-Computer bereitgestellt.

1. Wählen Sie im Azure DevOps Services-Menü den Eintrag **Code** aus, und navigieren Sie zu Ihrem Repository.

1. Navigieren Sie zum Verzeichnis **Views\Home**. Wählen Sie anschließend die **Auslassungspunkte** neben der Datei **Index.cshtml** und dann **Bearbeiten** aus.

1. Nehmen Sie eine Änderung an der Datei vor. Ändern Sie beispielsweise den Text in einem der **div-Tags**.  Wählen Sie oben rechts **Commit** aus.  Wählen Sie erneut **Commit** aus, um die Änderung mittels Push zu übertragen. 

1. Kurz danach wird **in Azure DevOps Services ein Build gestartet**. Anschließend wird zur Bereitstellung der Änderungen ein Release ausgeführt.  Überwachen Sie den **Buildstatus** mit dem DevOps-Projektdashboard oder im Browser mit Ihrer Azure DevOps Services-Organisation.

1. Nachdem das Release ausgeführt wurde, **aktualisieren Sie Ihre Anwendung** im Browser, um sicherzustellen, dass Ihre Änderungen angezeigt werden.

## <a name="configure-azure-application-insights-monitoring"></a>Konfigurieren der Application Insights-Überwachung

Mithilfe von Azure Application Insights können Sie die Leistung und Nutzung Ihrer Anwendung ganz einfach überwachen.  Mit dem Azure DevOps-Projekt wird für Ihre Anwendung automatisch eine Application Insights-Ressource konfiguriert.  Sie können Warnungen und Überwachungsfunktionen je nach Bedarf weiter konfigurieren.

1. Navigieren Sie im **Azure-Portal** zum **Azure DevOps-Projekt**.  Wählen Sie am unteren rechten Rand des Dashboards den Link zu **Application Insights** für Ihre App aus.

1. Das Blatt **Application Insights** wird im Azure-Portal geöffnet.  Diese Ansicht enthält Informationen zur Nutzungs-, Leistungs- und Verfügbarkeitsüberwachung für Ihre App.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. Wählen Sie **Zeitbereich** und dann **Letzte Stunde** aus.  Wählen Sie **Aktualisieren** aus, um die Ergebnisse zu filtern.  Jetzt werden alle Aktivitäten der letzten 60 Minuten angezeigt.  Wählen Sie das **x** aus, um den Zeitbereich zu verlassen.

1. Am oberen Rand des Dashboards finden Sie **Warnungen** und verschiedene andere nützliche Links.  Wählen Sie **Warnungen** und anschließend **+ Metrikwarnung hinzufügen** aus.

1. Geben Sie einen **Namen** für die Warnung ein.

1. Für die Standardwarnung gilt eine **Serverantwortzeit, die größer als 1 Sekunde** ist.  Wählen Sie die Dropdownliste **Metrik** aus, um die verschiedenen Warnungsmetriken anzuzeigen.  Sie können beispielsweise **Ausführungszeit für ASP.NET-Anforderung** für eine ASP.NET-App konfigurieren.  Sie können problemlos eine Vielzahl von Warnungen konfigurieren und dadurch die Überwachungsfunktionen Ihrer App verbessern.

1. Aktivieren Sie das Kontrollkästchen für **Notify via Email owners, contributors, and readers** (Besitzer, Mitwirkende und Leser über E-Mail informieren).  Optional können Sie zusätzliche Aktionen ausführen, wenn eine Warnung beim Ausführen einer Azure-Logik-App ausgelöst wird.

1. Wählen Sie **OK** aus, um die Warnung zu erstellen.  In wenigen Augenblicken wird die Warnung als aktiv auf dem Dashboard angezeigt.  **Verlassen** Sie den Warnungsbereich, und navigieren Sie zurück zum Blatt **Application Insights**.

1. Wählen Sie **Verfügbarkeit** und anschließend **+ Test hinzufügen** aus. 

1. Geben Sie einen **Testnamen** ein, und wählen Sie dann **Erstellen** aus.  Damit wird ein einfacher Ping-Test zur Überprüfung der Verfügbarkeit Ihrer Anwendung erstellt.  Nach wenigen Minuten sind die Testergebnisse verfügbar, und das Application Insights-Dashboard zeigt einen Verfügbarkeitsstatus an.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

 > [!NOTE]
 > Mit den nachfolgenden Schritten werden Ressourcen dauerhaft gelöscht.  Verwenden Sie diese Funktion erst, nachdem Sie die Eingabeaufforderungen aufmerksam durchgelesen haben.

Beim Durchführen von Tests können Sie Ressourcen bereinigen, um zu verhindern, dass Gebühren entstehen.  Sie können den mit diesem Tutorial erstellten virtuellen Azure-Computer und die zugehörigen Ressourcen mithilfe der Funktion **Löschen** im Azure DevOps-Projektdashboard löschen, wenn Sie die Bereitstellung nicht mehr benötigen.  **Seien Sie vorsichtig**, denn mit der Löschfunktion werden die im Rahmen des Azure DevOps-Projekts erstellten Daten in Azure und Azure DevOps gelöscht. Diese Daten können anschließend nicht wiederhergestellt werden.

1. Navigieren Sie im **Azure-Portal** zum **Azure DevOps-Projekt**.
2. Wählen Sie **oben rechts** im Dashboard die Option **Löschen** aus.  Nachdem Sie die Eingabeaufforderung gelesen haben, wählen Sie **Ja** aus, um die Ressourcen **dauerhaft zu löschen**.

## <a name="next-steps"></a>Nächste Schritte

Diese Build- und Releasepipelines können Sie optional an die Anforderungen Ihres Teams anpassen. Sie können dieses CI/CD-Muster auch als Vorlage für Ihre anderen Projekte verwenden.  Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts für eine ASP.NET-App
> * Konfigurieren von Azure DevOps Services und eines Azure-Abonnements 
> * Überprüfen der Azure DevOps Services-CI-Pipeline
> * Überprüfen der Azure DevOps Services-CD-Pipeline
> * Committen von Änderungen zu Azure DevOps Services und automatisches Bereitstellen in Azure
> * Konfigurieren der Azure Application Insights-Überwachung
> * Bereinigen von Ressourcen

Weitere Informationen zur CI/CD-Pipeline finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Anpassen von CD-Prozessen](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
