---
title: Bereitstellen einer ASP.NET Core-App in Azure Service Fabric mithilfe des Azure DevOps-Projekts | VSTS-Tutorial
description: Das DevOps-Projekt erleichtert die ersten Schritte mit Azure. Mit dem Azure DevOps-Projekt können Sie Ihre ASP.NET Core-App mit Azure Service Fabric in wenigen Schritten bereitstellen.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 2fd1fc968eb6b61d7378dbc0efa48f2f7eb2aa54
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967326"
---
# <a name="tutorial--deploy-your-aspnet-core-app-to-azure-service-fabric-with-the-azure-devops-project"></a>Tutorial: Bereitstellen einer ASP.NET Core-App in Azure Service Fabric mithilfe des Azure DevOps-Projekts

Das Azure DevOps-Projekt stellt eine vereinfachte Umgebung dar, in der Sie Ihren vorhandenen Code und Ihr Git-Repository verwenden oder eine der Beispielanwendungen wählen können, um eine Continuous Integration (CI)- und Continuous Delivery (CD)-Pipeline zu Azure zu erstellen.  Mit dem DevOps-Projekt werden Azure-Ressourcen wie Azure Service Fabric automatisch erstellt, eine Releasepipeline in VSTS mit einer Builddefinition für CI erstellt und konfiguriert, eine Releasedefinition für CD eingerichtet und anschließend eine Azure Application Insights-Ressource für die Überwachung erstellt.

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts für eine ASP.NET Core-App mit Azure Service Fabric
> * Konfigurieren von VSTS und eines Azure-Abonnements 
> * Untersuchen der VSTS-CI-Builddefinition
> * Untersuchen der VSTS-CD-Releaseverwaltungsdefinition
> * Ausführen eines Commits für Änderungen an VSTS und automatisches Bereitstellen in Azure
> * Bereinigen von Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.

## <a name="create-an-azure-devops-project-for-an-aspnet-core-app-and-service-fabric"></a>Erstellen eines Azure DevOps-Projekts für eine ASP.NET Core-App mit Azure Service Fabric

Mit dem Azure DevOps-Projekt wird in VSTS eine CI/CD-Pipeline erstellt.  Sie können ein **neues VSTS**-Konto erstellen oder ein **vorhandenes Konto** verwenden.  Ferner werden mit dem Azure DevOps-Projekt **Azure-Ressourcen** wie ein Azure Service Fabric-Cluster im **Azure-Abonnement** Ihrer Wahl erstellt.

1. Melden Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie in der linken Navigationsleiste das Symbol **Ressource erstellen** aus, und suchen Sie anschließend nach **DevOps-Projekt**.  Wählen Sie **Erstellen**.

    ![Starten von Continuous Delivery](_img/azure-devops-project-service-fabric/fullbrowser.png)

1. Wählen Sie **.NET** und dann **Weiter** aus.

1. Wählen Sie für **Anwendungsframework auswählen** die Option **ASP.NET Core** und anschließend **Weiter** aus.

1. Wählen Sie **Service Fabric-Cluster** und anschließend **Weiter** aus.  

## <a name="configure-vsts-and-an-azure-subscription"></a>Konfigurieren von VSTS und eines Azure-Abonnements

1. Erstellen Sie ein **neues** VSTS-Konto, oder wählen Sie ein **vorhandenes** Konto aus.  Wählen Sie für Ihr VSTS-Projekt einen **Namen** aus.  

1. Wählen Sie Ihr Azure-**Abonnement** aus.

1. Wählen Sie den Link **Änderung** aus, um zusätzliche Azure-Konfigurationseinstellungen anzuzeigen. Geben Sie dann die **VM-Größe des Knotens** und das **Betriebssystem** für den  **Service Fabric-Cluster** an.  Es gibt verschiedene Optionen zum Konfigurieren von Typ und Speicherort der Azure-Dienste.
 
1. Beenden Sie den Konfigurationsbereich von Azure, und klicken Sie auf **Fertig**.

1. Dieser Prozess dauert einige Minuten.  In Ihrem VSTS-Konto wird eine ASP.NET Core-Beispielanwendung in einem Repository eingerichtet. Ein Service Fabric-Cluster wird erstellt, ein Build und ein Release werden ausgeführt, und Ihre Anwendung wird in Azure bereitgestellt.  

    Sobald der Prozess abgeschlossen ist, wird das Azure DevOps-**Projektdashboard** im Azure-Portal geladen.  Über **Alle Ressourcen** können Sie das **Azure DevOps-Projektdashboard** auch direkt im **Azure-Portal** laden.  

    Dieses Dashboard bietet Einblick in Ihr **Coderepository**, in Ihre **VSTS-CI/CD-Pipeline** und in Ihren **Service Fabric-Cluster**.  Sie können zusätzliche Optionen in VSTS konfigurieren.  Wählen Sie auf der rechten Seite des Dashboards **Durchsuchen**, um Ihre aktive Anwendung anzuzeigen.

## <a name="examine-the-vsts-ci-build-definition"></a>Untersuchen der VSTS-CI-Builddefinition

Mit dem Azure DevOps-Projekt wird in Ihrem VSTS-Konto automatisch eine vollständige VSTS-CI/CD-Pipeline erstellt.  Sie können die Pipeline untersuchen und anpassen.  Gehen Sie wie folgt vor, um sich mit der VSTS-Builddefinition vertraut zu machen.

1. Navigieren Sie zum **Azure DevOps-Projektdashboard**.

1. Wählen Sie **oben** im **Azure DevOps-Projektdashboard** **Pipelines erstellen** aus.  Über diesen Link werden eine Browserregisterkarte und die VSTS-Builddefinition für Ihr neues Projekt geöffnet.

1. Verschieben Sie den Cursor rechts neben die Builddefinition neben das Feld **Status**. Wählen Sie die **Auslassungspunkte** aus, die angezeigt werden.  Mit dieser Aktion wird ein Menü geöffnet, über das Sie verschiedene Aktivitäten starten können. So können Sie beispielsweise einen neuen **Build zur Warteschlange hinzufügen**, **einen Build anhalten** und  **die Builddefinition bearbeiten**.

1. Wählen Sie **Bearbeiten** aus.

1. In dieser Ansicht können Sie sich **die verschiedenen Aufgaben ansehen**, die Sie für Ihre Builddefinition durchführen können.  Vom Build werden verschiedene Aufgaben durchgeführt. So werden beispielsweise Quellen aus dem VSTS-Git-Repository abgerufen, Abhängigkeiten wiederhergestellt und für Bereitstellungen verwendete Ausgaben veröffentlicht.

1. Wählen Sie oben in der Builddefinition den **Builddefinitionsnamen** aus.

1. Ersetzen Sie den **Namen** Ihrer Builddefinition durch einen aussagekräftigeren Namen.  Wählen Sie **Speichern und in Warteschlange einreihen** und dann **Speichern** aus.

1. Wählen Sie unter dem Builddefinitionsnamen **Verlauf** aus.  Es wird ein Überwachungspfad mit den letzten Änderungen für den Build angezeigt.  An der Builddefinition vorgenommene Änderungen werden von VSTS überwacht, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus.  Mit dem Azure DevOps-Projekt wurde automatisch ein CI-Trigger erstellt, und mit jedem Commit, der für das Repository ausgeführt wird, wird ein neuer Build gestartet.  Optional können Sie Branches aus dem CI-Prozess einbeziehen oder ausschließen.

1. Wählen Sie **Aufbewahrung** aus.  Je nach Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

## <a name="examine-the-vsts-cd-release-management-definition"></a>Untersuchen der VSTS-CD-Releaseverwaltungsdefinition

Mit dem Azure DevOps-Projekt werden die erforderlichen Schritte zum Bereitstellen über Ihr VSTS-Konto in Ihrem Azure-Abonnement automatisch erstellt und konfiguriert.  Diese Schritte umfassen die Konfiguration einer Azure-Dienstverbindung zur Authentifizierung von VSTS für Ihr Azure-Abonnement.  Bei der Automatisierung wird außerdem eine VSTS-Releasedefinition erstellt. Zudem wird CD für Azure von diesem Release bereitgestellt.  Gehen Sie wie folgt vor, um mehr über die VSTS-Releasedefinition zu erfahren.

1. Wählen Sie **Build und Release** und anschließend **Releases** aus.  Mit dem Azure DevOps-Projekt wurde eine VSTS-Releasedefinition zum Verwalten von Bereitstellungen für Azure erstellt.

1. Wählen Sie links im Browser die **Auslassungspunkte** neben Ihrer Releasedefinition und anschließend **Bearbeiten** aus.

1. Die Releasedefinition enthält eine **Pipeline**, die den Releaseprozess definiert.  Wählen Sie unter **Artefakte** die Option **Ablegen** aus.  Mit der Builddefinition, die Sie in den vorherigen Schritten untersucht haben, wird die für das Artefakt verwendete Ausgabe erzeugt. 

1. Wählen Sie rechts vom Symbol **Ablegen** das **Symbol** für **Continuous Deployment-Trigger** aus.  Diese Releasedefinition verfügt über einen aktivierten CD-Trigger.  Der Trigger wird jedes Mal ausgelöst, wenn ein neues Buildartefakt verfügbar ist.  Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen. 

1. Wählen Sie rechts im Browser **Releases anzeigen** aus.  In dieser Ansicht wird ein Verlauf von Releases angezeigt.

1. Wählen Sie die **Auslassungsp** neben einem Ihrer Releases und dann die Option **Öffnen** aus.  In dieser Ansicht finden Sie verschiedene Menüs, wie etwa eine **Releasezusammenfassung**, **zugeordnete Arbeitsaufgaben** und **Tests**.

1. Wählen Sie **Commits** aus.  In dieser Ansicht werden die der jeweiligen Bereitstellung zugeordneten Codecommits angezeigt. Sie können Releases vergleichen, um die Commitunterschiede zwischen den einzelnen Bereitstellungen anzuzeigen.

1. Wählen Sie **Protokolle** aus.  Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess.  Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="commit-changes-to-vsts-and-automatically-deploy-to-azure"></a>Ausführen eines Commits für Änderungen an VSTS und automatisches Bereitstellen in Azure 

 > [!NOTE]
 > Mit den folgenden Schritten wird die CI/CD-Pipeline zu Ihrer Web-App mit einer einfachen Textänderung getestet.

Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit auf Ihrer Website automatisch bereitgestellt wird, mit einem Team an Ihrer App zusammenarbeiten.  Mit jeder Änderung am VSTS-Git-Repository wird in VSTS ein Build gestartet. Darüber hinaus werden Ihre Änderungen durch eine VSTS-Releaseverwaltungsdefinition in Azure bereitgestellt.  Gehen Sie wie folgt vor, um einen Commit für Änderungen an Ihrem Repository auszuführen, oder verwenden Sie hierfür andere Verfahren.  Sie können z.B. das Git-Repository in Ihrem Lieblingstool oder Ihre IDE **klonen** und anschließend Änderungen per Push in dieses Repository übertragen.

1. Wählen Sie im VSTS-Menü den Eintrag **Code** und dann **Dateien** aus, und navigieren Sie zu Ihrem Repository.
1. Navigieren Sie zum Verzeichnis **Views\Home**. Wählen Sie anschließend die **Auslassungspunkte** neben der Datei **Index.cshtml** und dann **Bearbeiten** aus.

1. Nehmen Sie eine Änderung an der Datei vor. Ändern Sie beispielsweise den Text in einem der **div-Tags**.  Wählen Sie oben rechts **Commit** aus.  Wählen Sie erneut **Commit** aus, um die Änderung mittels Push zu übertragen. 

1. Kurz danach wird **in VSTS ein Build gestartet**. Anschließend wird zur Bereitstellung der Änderungen ein Release ausgeführt.  Sie können den **Buildstatus** mit dem DevOps-Projektdashboard oder im Browser mit Ihrem VSTS-Konto überwachen.

1. Nachdem das Release ausgeführt wurde, **aktualisieren Sie Ihre Anwendung** im Browser, um sicherzustellen, dass Ihre Änderungen angezeigt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

 > [!NOTE]
 > Mit den nachfolgenden Schritten werden Ressourcen dauerhaft gelöscht.  Verwenden Sie diese Funktion erst, nachdem Sie die Eingabeaufforderungen aufmerksam durchgelesen haben.

Beim Durchführen von Tests können Sie Ressourcen bereinigen, um zu verhindern, dass Gebühren entstehen.  Sie können den mit diesem Tutorial erstellte Service Fabric-Cluster und die zugehörigen Ressourcen mithilfe der Funktion **Löschen** im Azure DevOps-Projektdashboard löschen, wenn Sie die Bereitstellung nicht mehr benötigen.  **Seien Sie vorsichtig**, denn mit der Löschfunktion werden die im Rahmen des Azure DevOps-Projekts erstellten Daten in Azure und VSTS gelöscht. Diese Daten können anschließend nicht wiederhergestellt werden.

1. Navigieren Sie im **Azure-Portal** zum **Azure DevOps-Projekt**.
2. Wählen Sie **oben rechts** im Dashboard die Option **Löschen** aus.  Nachdem Sie die Eingabeaufforderung gelesen haben, wählen Sie **Ja** aus, um die Ressourcen **dauerhaft zu löschen**.

## <a name="next-steps"></a>Nächste Schritte

Diese Build- und Releasedefinitionen können Sie optional an die Anforderungen Ihres Teams anpassen. Sie können dieses CI/CD-Muster auch als Vorlage für Ihre anderen Projekte verwenden.  Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines Azure DevOps-Projekts für eine ASP.NET Core-App mit Azure Service Fabric
> * Konfigurieren von VSTS und eines Azure-Abonnements 
> * Untersuchen der VSTS-CI-Builddefinition
> * Untersuchen der VSTS-CD-Releaseverwaltungsdefinition
> * Ausführen eines Commits für Änderungen an VSTS und automatisches Bereitstellen in Azure
> * Bereinigen von Ressourcen

Weiter Informationen zu Service Fabric und Microservices finden Sie hier:

> [!div class="nextstepaction"]
> [Use a microservices approach for building applications (Verwenden eines Microservices-Ansatzes zum Erstellen von Anwendungen)](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
