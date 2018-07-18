---
title: Bereitstellen einer ASP.NET-App in Azure Virtual Machines mithilfe des Azure DevOps-Projekts | VSTS-Tutorial
description: Das DevOps-Projekt erleichtert die ersten Schritte mit Azure. Mit dem Azure DevOps-Projekt können Sie Ihre ASP.NET-App in Azure Virtual Machines in wenigen Schritten bereitstellen.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: b5a9ce204f68d812da4dfcfebb18b79b9c70c6c9
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967395"
---
# <a name="tutorial--deploy-your-aspnet-app-to-azure-virtual-machines-with-the-azure-devops-project"></a>Tutorial: Bereitstellen einer ASP.NET-App in Azure Virtual Machines mithilfe des Azure DevOps-Projekts

Das Azure DevOps-Projekt stellt eine vereinfachte Umgebung dar, in der Sie Ihren vorhandenen Code und Ihr Git-Repository verwenden oder eine der Beispielanwendungen wählen können, um eine Continuous Integration- und Continuous Delivery-Pipeline (CI/CD) zu Azure zu erstellen.  Mit dem DevOps-Projekt werden Azure-Ressourcen wie etwa ein neuer virtueller Azure-Computer automatisch erstellt, eine Releasepipeline in VSTS mit einer Builddefinition für CI erstellt und konfiguriert, eine Releasedefinition für CD eingerichtet und anschließend eine Azure Application Insights-Ressource für die Überwachung erstellt.

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts für eine ASP.NET-App
> * Konfigurieren von VSTS und eines Azure-Abonnements 
> * Untersuchen der VSTS-CI-Builddefinition
> * Untersuchen der VSTS-CD-Releaseverwaltungsdefinition
> * Ausführen eines Commits für Änderungen an VSTS und automatisches Bereitstellen in Azure
> * Konfigurieren der Application Insights-Überwachung
> * Bereinigen von Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.

## <a name="create-an-azure-devops-project-for-an-aspnet-app"></a>Erstellen eines Azure DevOps-Projekts für eine ASP.NET-App

Mit dem Azure DevOps-Projekt wird in VSTS eine CI/CD-Pipeline erstellt.  Sie können ein **neues VSTS-Konto** erstellen oder ein **vorhandenes Konto** verwenden.  Ferner werden mit dem Azure DevOps-Projekt **Azure-Ressourcen** wie etwa virtuelle Computer im **Azure-Abonnement** Ihrer Wahl erstellt.

1. Melden Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie in der linken Navigationsleiste das Symbol **+ Neu** aus, und suchen Sie anschließend nach **DevOps-Projekt**.  Wählen Sie **Erstellen**.

    ![Starten von Continuous Delivery](_img/azure-devops-project-github/fullbrowser.png)

1. Wählen Sie **.NET** und dann **Weiter** aus.

1. Wählen Sie für **Anwendungsframework auswählen** die Option **ASP.NET** und anschließend **Weiter** aus. 

1. Das Anwendungsframework, das Sie im vorherigen Schritt ausgewählt haben, bestimmt den Typ des hier verfügbaren Bereitstellungsziels für den Azure-Dienst.  Wählen Sie den **virtuellen Computer** und anschließend **Weiter** aus.

## <a name="configure-vsts-and-an-azure-subscription"></a>Konfigurieren von VSTS und eines Azure-Abonnements

1. Erstellen Sie ein **neues** VSTS-Konto, oder wählen Sie ein **vorhandenes** Konto aus.  Wählen Sie für Ihr VSTS-Projekt einen **Namen** aus.  

1. Wählen Sie Ihr Azure-**Abonnement** aus.  Optional können Sie den Link **Ändern** auswählen und anschließend weitere Konfigurationsdetails wie etwa einen anderen Speicherort für die Azure-Ressourcen eingeben.
 
1. Geben Sie den **Namen des virtuellen Computers**, den **Benutzernamen** und das **Kennwort** für Ihre neue Azure Virtual Machine-Ressource ein, und wählen Sie anschließend **Fertig** aus.

1. Es dauert einige Minuten, bis der virtuelle Azure-Computer bereit ist.  In Ihrem VSTS-Konto wird eine ASP.NET-Beispielanwendung in einem Repository eingerichtet. Ein Build und ein Release werden ausgeführt, und Ihre Anwendung wird im neu erstellten virtuellen Azure-Computer bereitgestellt.  

    Sobald der Prozess abgeschlossen ist, wird das **Azure DevOps-Projektdashboard** im Azure-Portal geladen.  Über **Alle Ressourcen** können Sie das **Azure DevOps-Projektdashboard** auch direkt im **Azure-Portal** laden.  

    Dieses Dashboard bietet Einblick in Ihr VSTS-**Coderepository**, in Ihre **VSTS-CI/CD-Pipeline** und in Ihre aktive **Anwendung in Azure**.    

    ![Dashboardansicht](_img/azure-devops-project-vms/dashboardnopreview.png)

1. Mit dem Azure DevOps-Projekt wird automatisch ein CI-Build- und -Releasetrigger konfiguriert, durch den Codeänderungen automatisch im Repository bereitgestellt werden.  Sie können zusätzliche Optionen in VSTS konfigurieren.  Wählen Sie auf der rechten Seite des Dashboards **Durchsuchen**, um Ihre aktive Anwendung anzuzeigen.
    
## <a name="examine-the-vsts-ci-build-definition"></a>Untersuchen der VSTS-CI-Builddefinition

Mit dem Azure DevOps-Projekt wird in Ihrem VSTS-Konto automatisch eine vollständige VSTS-CI/CD-Pipeline erstellt.  Sie können die Pipeline untersuchen und anpassen.  Gehen Sie wie folgt vor, um sich mit der VSTS-Builddefinition vertraut zu machen.

1. Wählen Sie **oben** im **Azure DevOps-Projektdashboard** **Pipelines erstellen** aus.  Über diesen Link werden eine Browserregisterkarte und die VSTS-Builddefinition für Ihr neues Projekt geöffnet.

1. Verschieben Sie den Cursor rechts neben die Builddefinition neben das Feld **Status**. Wählen Sie die **Auslassungspunkte** aus, die angezeigt werden.  Mit dieser Aktion wird ein Menü geöffnet, über das Sie verschiedene Aktivitäten durchführen können. So können Sie beispielsweise **einen neuen Build zur Warteschlange hinzufügen**, **einen Build anhalten** und **die Builddefinition bearbeiten**.

1. Wählen Sie **Bearbeiten** aus.

1. In dieser Ansicht können Sie sich **die verschiedenen Aufgaben ansehen**, die Sie für Ihre Builddefinition durchführen können.  Vom Build werden verschiedene Aufgaben durchgeführt. So werden beispielsweise Quellen aus dem VSTS-Git-Repository abgerufen, Abhängigkeiten wiederhergestellt und für Bereitstellungen verwendete Ausgaben veröffentlicht.

1. Wählen Sie oben in der Builddefinition den **Builddefinitionsnamen** aus.

1. Ersetzen Sie den **Namen** Ihrer Builddefinition durch einen aussagekräftigeren Namen.  Wählen Sie **Speichern und in Warteschlange einreihen** und dann **Speichern** aus.

1. Wählen Sie unter dem Builddefinitionsnamen **Verlauf** aus.  Es wird ein Überwachungspfad mit den letzten Änderungen für den Build angezeigt.  An der Builddefinition vorgenommene Änderungen werden von VSTS überwacht, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus.  Mit dem Azure DevOps-Projekt wurde automatisch ein CI-Trigger erstellt, und mit jedem Commit, der für das Repository ausgeführt wird, wird ein neuer Build gestartet.  Optional können Sie Branches aus dem CI-Prozess einbeziehen oder ausschließen.

1. Wählen Sie **Aufbewahrung** aus.  Je nach Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

## <a name="examine-the-vsts-cd-release-management-definition"></a>Untersuchen der VSTS-CD-Releaseverwaltungsdefinition

Mit dem Azure DevOps-Projekt werden die erforderlichen Schritte zum Bereitstellen über Ihr VSTS-Konto in Ihrem Azure-Abonnement automatisch erstellt und konfiguriert.  Diese Schritte umfassen die Konfiguration einer Azure-Dienstverbindung zur Authentifizierung von VSTS für Ihr Azure-Abonnement.  Bei der Automatisierung wird außerdem eine VSTS-Releasedefinition erstellt, durch die für den virtuellen Azure-Computer CD bereitgestellt wird.  Gehen Sie folgendermaßen vor, um mehr über die VSTS-Releasedefinition zu erfahren.

1. Wählen Sie **Build und Release** und anschließend **Releases** aus.  Mit dem Azure DevOps-Projekt wurde eine VSTS-Releasedefinition zum Verwalten von Bereitstellungen für Azure erstellt.

1. Wählen Sie links im Browser die **Auslassungspunkte** neben Ihrer Releasedefinition und anschließend **Bearbeiten** aus.

1. Die Releasedefinition enthält eine **Pipeline**, die den Releaseprozess definiert.  Wählen Sie unter **Artefakte** die Option **Ablegen** aus.  Mit der Builddefinition, die Sie in den vorherigen Schritten untersucht haben, wird die für das Artefakt verwendete Ausgabe erzeugt. 

1. Wählen Sie die Option **Continuous Deployment-Trigger** rechts neben dem **Symbol** **Ablegen** (das als Gewitterblitz dargestellt wird) aus.  Diese Releasedefinition verfügt über einen aktivierten CD-Trigger.  Der Trigger wird jedes Mal ausgelöst, wenn ein neues Buildartefakt verfügbar ist.  Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen. 

1. Wählen Sie links im Browser **Aufgaben** und anschließend Ihre **Umgebung** aus.  

1. Bei den Aufgaben handelt es sich um die Aktivitäten, die beim Bereitstellungsprozess durchgeführt werden. Sie sind in **Phasen** gruppiert.  Für diese Releasedefinition gibt es zwei **Phasen**.  Die erste Phase enthält die Aufgabe **Bereitstellung einer Azure-Ressourcengruppe**. Mit dieser Aufgabe wird der virtuelle Computer für die Bereitstellung konfiguriert und der neue virtuelle Computer einer **VSTS-Bereitstellungsgruppe** hinzugefügt.  Mit der VM-Bereitstellungsgruppe in VSTS werden logische Gruppen von **Bereitstellungszielcomputern** verwaltet.

1. In dieser zweiten Phase wurde die Aufgabe **IIS-Web-App verwalten** zum Erstellen einer IIS-Website auf dem virtuellen Computer erstellt.  Die zweite **IIS-Web-App-Bereitstellung**-Aufgabe wurde zur Bereitstellung der Website erstellt.

1. Wählen Sie rechts im Browser **Releases anzeigen** aus.  In dieser Ansicht wird der Verlauf von Releases angezeigt.

1. Wählen Sie die **Auslassungsp** neben einem Ihrer Releases und dann die Option **Öffnen** aus.  In dieser Ansicht finden Sie verschiedene Menüs, wie etwa eine **Releasezusammenfassung**, **zugeordnete Arbeitsaufgaben** und **Tests**.

1. Wählen Sie **Commits** aus.  In dieser Ansicht werden die der jeweiligen Bereitstellung zugeordneten Codecommits angezeigt. Vergleichen Sie Releases, um die Commitunterschiede zwischen den einzelnen Bereitstellungen anzuzeigen.

1. Wählen Sie **Protokolle** aus.  Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess.  Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="commit-changes-to-vsts-and-automatically-deploy-to-azure"></a>Ausführen eines Commits für Änderungen an VSTS und automatisches Bereitstellen in Azure 

Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit an Ihrer Website automatisch bereitgestellt wird, mit einem Team an Ihrer App zusammenarbeiten.  Mit jeder Änderung am VSTS-Git-Repository wird in VSTS ein Build gestartet. Ferner wird durch eine VSTS-Releaseverwaltungsdefinition eine Bereitstellung auf Ihrem virtuellen Azure-Computer ausgeführt.  Gehen Sie wie folgt vor, um einen Commit für Änderungen an Ihrem Repository auszuführen, oder verwenden Sie hierfür andere Verfahren.  Mit den Codeänderungen wird der CI/CD-Prozess initiiert. Ferner werden die neuen Änderungen an der IIS-Website automatisch auf dem virtuellen Azure-Computer bereitgestellt.

1. Wählen Sie im VSTS-Menü den Eintrag **Code** aus, und navigieren Sie zu Ihrem Repository.

1. Navigieren Sie zum Verzeichnis **Views\Home**. Wählen Sie anschließend die **Auslassungspunkte** neben der Datei **Index.cshtml** und dann **Bearbeiten** aus.

1. Nehmen Sie eine Änderung an der Datei vor. Ändern Sie beispielsweise den Text in einem der **div-Tags**.  Wählen Sie oben rechts **Commit** aus.  Wählen Sie erneut **Commit** aus, um die Änderung mittels Push zu übertragen. 

1. Kurz danach wird **in VSTS ein Build gestartet**. Anschließend wird zur Bereitstellung der Änderungen ein Release ausgeführt.  Überwachen Sie den **Buildstatus** mit dem DevOps-Projektdashboard oder im Browser mit Ihrem VSTS-Konto.

1. Nachdem das Release ausgeführt wurde, **aktualisieren Sie Ihre Anwendung** im Browser, um sicherzustellen, dass Ihre Änderungen angezeigt werden.

## <a name="configure-azure-application-insights-monitoring"></a>Konfigurieren der Application Insights-Überwachung

Mit Azure Application Insights können Sie die Leistung und Nutzung Ihrer Anwendung ganz einfach überwachen.  Mit dem Azure DevOps-Projekt wird für Ihre Anwendung automatisch eine Application Insights-Ressource konfiguriert.  Sie können Warnungen und Überwachungsfunktionen je nach Bedarf weiter konfigurieren.

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

Beim Durchführen von Tests können Sie Ressourcen bereinigen, um zu verhindern, dass Gebühren entstehen.  Sie können den mit diesem Tutorial erstellten virtuellen Azure-Computer und die zugehörigen Ressourcen mithilfe der Funktion **Löschen** im Azure DevOps-Projektdashboard löschen, wenn Sie die Bereitstellung nicht mehr benötigen.  **Seien Sie vorsichtig**, denn mit der Löschfunktion werden die im Rahmen des Azure DevOps-Projekts erstellten Daten in Azure und VSTS gelöscht. Diese Daten können anschließend nicht wiederhergestellt werden.

1. Navigieren Sie im **Azure-Portal** zum **Azure DevOps-Projekt**.
2. Wählen Sie **oben rechts** im Dashboard die Option **Löschen** aus.  Nachdem Sie die Eingabeaufforderung gelesen haben, wählen Sie **Ja** aus, um die Ressourcen **dauerhaft zu löschen**.

## <a name="next-steps"></a>Nächste Schritte

Diese Build- und Releasedefinitionen können Sie optional an die Anforderungen Ihres Teams anpassen. Sie können dieses CI/CD-Muster auch als Vorlage für Ihre anderen Projekte verwenden.  Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts für eine ASP.NET-App
> * Konfigurieren von VSTS und eines Azure-Abonnements 
> * Untersuchen der VSTS-CI-Builddefinition
> * Untersuchen der VSTS-CD-Releaseverwaltungsdefinition
> * Ausführen eines Commits für Änderungen an VSTS und automatisches Bereitstellen in Azure
> * Konfigurieren der Application Insights-Überwachung
> * Bereinigen von Ressourcen

Weitere Informationen zur VSTS-Pipeline finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Anpassen von CD-Prozessen](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
