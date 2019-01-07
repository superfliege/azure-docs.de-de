---
title: CI/CD-Pipeline mit Azure DevOps Projects – Azure IoT Edge | Microsoft-Dokumentation
description: Azure DevOps Projects erleichtert die ersten Schritte mit Azure. Damit können Sie eine Azure IoT Edge-App Ihrer Wahl in einigen wenigen Schritten starten.
author: shizn
manager: ''
ms.author: xshi
ms.date: 12/04/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: ebb7e515f9d9205f364d50b3d686c68a2988f86a
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074213"
---
# <a name="create-a-cicd-pipeline-for-iot-edge-with-azure-devops-projects-preview"></a>Erstellen einer CI/CD-Pipeline für IoT Edge mit Azure DevOps Projects (Vorschauversion)

Konfigurieren Sie mit DevOps Projects Continuous Integration (CI) und Continuous Delivery (CD) für Ihre IoT Edge-Anwendung. DevOps Projects erleichtert die Erstkonfiguration einer Build- und Releasepipeline in Azure Pipelines.

Wenn Sie über kein aktives Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Mit DevOps Projects wird eine CI/CD-Pipeline in Azure DevOps erstellt. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation verwenden. Ferner werden mit DevOps Projects Azure-Ressourcen im Azure-Abonnement Ihrer Wahl erstellt.

1. Melden Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com)an.

1. Wählen Sie im linken Bereich auf der linken Navigationsleiste das Symbol **Ressource erstellen** aus, und suchen Sie anschließend nach **DevOps Projects**.  

1.  Klicken Sie auf **Erstellen**.

## <a name="select-a-sample-application-and-azure-service"></a>Auswählen einer Beispielanwendung und eines Azure-Diensts

1. Ihre Azure IoT Edge-Module können in [C#](tutorial-csharp-module.md), [Node.js](tutorial-node-module.md), [Python](tutorial-python-module.md), [C](tutorial-c-module.md) und [Java](tutorial-java-module.md) geschrieben werden. Wählen Sie Ihre bevorzugte Sprache aus, um eine neue Anwendung zu starten. Dementsprechend können Sie zwischen **.NET**, **Node.js**, **Python**, **C**, oder **Java** auswählen. Klicken Sie dann auf **Weiter**.

    ![Sprache auswählen, um eine neue Anwendung zu erstellen](./media/how-to-devops-project/select-language.png)

2. Klicken Sie auf **Einfaches IoT (Vorschau)** und dann auf **Weiter**.

    ![Einfaches IoT-Framework auswählen](media/how-to-devops-project/select-iot.png)

3. Klicken Sie auf **IoT Edge** und dann auf **Weiter**.

    ![Dienst „IoT Edge“ auswählen](media/how-to-devops-project/select-iot-edge.png)

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Konfigurieren von Azure DevOps und eines Azure-Abonnements

1. Sie können eine neue kostenlose Azure DevOps-Organisation erstellen oder eine bestehende Organisation auswählen.

    a. Wählen Sie einen Namen für Ihr Projekt aus. 

    b. Wählen Sie Ihr Azure-Abonnement und den Standort sowie einen Namen für Ihre Anwendung aus, und klicken Sie dann auf **Fertig**.  

    ![Anwendung benennen und erstellen](media/how-to-devops-project/select-devops.png)

1. Nach wenigen Minuten wird das DevOps Projects-Dashboard im Azure-Portal angezeigt. Eine Beispiel-IoT Edge-Anwendung wird in einem Repository in Ihrer Azure DevOps-Organisation eingerichtet, ein Build wird ausgeführt, und Ihre Anwendung wird auf dem IoT Edge-Gerät bereitgestellt. Dieses Dashboard bietet Einblick in Ihr Coderepository, in Ihre CI/CD-Pipeline und in Ihre Anwendung in Azure.

    ![Anwendung im DevOps-Portal anzeigen](./media/how-to-devops-project/devops-portal.png)


## <a name="commit-code-changes-and-execute-cicd"></a>Ausführen eines Commits für Codeänderungen und Ausführen von CI/CD

Von DevOps Projects wurde ein Git-Repository in Azure Repos oder GitHub erstellt. Gehen Sie wie folgt vor, um das Repository anzuzeigen und Codeänderungen an Ihrer Anwendung vorzunehmen:

1. Wählen Sie auf der linken Seite des DevOps Projects-Dashboards den Link für Ihren **Masterbranch** aus.  
Über diesen Link wird die Ansicht des neu erstellten Git-Repositorys geöffnet.

1. Wählen Sie oben rechts im Browser die Option **Klonen** aus, um die Repository-Klon-URL anzuzeigen. Sie können Ihr Git-Repository in Visual Studio Code oder anderen bevorzugten Tools klonen. Bei den nächsten Schritten führen Sie Codeänderungen über den Webbrowser direkt am Masterbranch aus und committen sie auch dort.

    ![Git-Repository klonen](media/how-to-devops-project/clone.png)

1. Wechseln Sie links im Browser zu der Datei **modules/FilterModule/module.json**.

1. Wählen Sie **Bearbeiten** aus, und nehmen Sie unter `"tag"` eine Änderung für `"version"` vor. Sie können die Version z. B. auf `"version": "${BUILD_BUILDID}"` aktualisieren, sodass [Azure DevOps-Buildvariablen](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=vsts#build-variables) als Teil Ihres Azure IoT Edge-Modulimagetags verwendet werden.

    ![Version bearbeiten, sodass Buildvariablen akzeptiert werden](media/how-to-devops-project/update-module-json.png)

1. Wählen Sie **Commit** aus, und speichern Sie anschließend die Änderungen.

1. Navigieren Sie in Ihrem Browser zum Azure DevOps Projects-Dashboard.  Nun wird angezeigt, dass ein Build erstellt wird. Die von Ihnen vorgenommenen Änderungen werden automatisch erstellt und über eine CI/CD-Pipeline bereitgestellt.

    ![Status „Wird ausgeführt“ anzeigen](media/how-to-devops-project/ci-cd-in-progress.png)

## <a name="examine-the-cicd-pipeline"></a>Überprüfen der CI/CD-Pipeline

Im vorherigen Schritt wurde von Azure DevOps Projects automatisch eine vollständige CI/CD-Pipeline für Ihre IoT Edge-Anwendung konfiguriert. Untersuchen Sie die Pipeline, und passen Sie sie bei Bedarf an. Führen Sie die folgenden Schritte aus, um sich mit den Build- und Releasepipelines von Azure DevOps vertraut zu machen:

1. Wählen Sie oben auf dem DevOps Projects-Dashboard die Option **Buildpipelines** aus.  
Über diesen Link werden eine Browserregisterkarte und die Azure DevOps-Buildpipeline für Ihr neues Projekt geöffnet.

1. Wählen Sie **Bearbeiten** aus.

    ![Bearbeiten der Buildpipeline](media/how-to-devops-project/click-edit-button.png)

1. In diesem Bereich können Sie sich die verschiedenen Aufgaben ansehen, die Sie für Ihre Buildpipeline ausführen können. Vom Build werden verschiedene Aufgaben durchgeführt. Beispielsweise werden Quellen aus dem Git-Repository abgerufen, IoT Edge-Modulimages erstellt und gepusht sowie für Bereitstellungen verwendete Ausgaben veröffentlicht. Weitere Informationen zu Azure IoT Edge-Tasks für CI finden Sie unter [Konfigurieren von Azure Pipelines für Continuous Integration](https://docs.microsoft.com/azure/iot-edge/how-to-ci-cd#configure-azure-pipelines-for-continuous-integration).

    ![Continuous Integration-Tasks anzeigen](media/how-to-devops-project/ci.png)

1. Wählen Sie oben in der Buildpipeline den Buildpipelinenamen.

1. Ersetzen Sie den Namen Ihrer Buildpipeline durch einen aussagekräftigeren Namen, und wählen Sie **Speichern und in Warteschlange einreihen** und dann **Speichern** aus.

1. Wählen Sie unter dem Buildpipelinenamen **Verlauf** aus.   
Im Bereich **Verlauf** wird ein Überwachungsprotokoll mit den letzten Änderungen für den Build angezeigt.  An der Buildpipeline vorgenommene Änderungen werden von Azure Pipelines nachverfolgt, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus. Mit DevOps Projects wurde automatisch ein CI-Trigger erstellt, und mit jedem für das Repository ausgeführten Commit wird ein neuer Build gestartet.  Optional können Sie Branches aus dem CI-Prozess einbeziehen oder ausschließen.

1. Wählen Sie **Aufbewahrung** aus. Abhängig vom Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

1. Wählen Sie unter **Pipelines** die Option **Release** aus. DevOps Projects erstellt eine Releasepipeline zum Verwalten von Bereitstellungen in Azure IoT Edge.

    ![Releasepipeline anzeigen](media/how-to-devops-project/release-pipeline.png)

1. Wählen Sie **Bearbeiten** aus. Die Releasepipeline enthält eine Pipeline, die den Releaseprozess definiert.  

1. Wählen Sie unter **Artefakte** die Option **Ablegen** aus. Die in den vorherigen Schritten untersuchte Buildpipeline erzeugt die für das Artefakt verwendete Ausgabe. 

1. Wählen Sie neben dem Symbol **Ablegen** die Option **Continuous Deployment-Trigger** aus.  
Diese Releasepipeline enthält einen aktivierten CD-Trigger. Jedes Mal, wenn ein neues Buildartefakt verfügbar ist, wird von diesem CD-Trigger eine Bereitstellung ausgeführt. Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen.  

1. Wählen Sie auf der linken Seite **Aufgaben** aus. Bei den Aufgaben handelt es sich um die Aktivitäten, die beim Bereitstellungsprozess ausgeführt werden. In diesem Beispiel wurde für die Bereitstellung Ihres Modulimages in Azure IoT Edge ein Task erstellt. Weitere Informationen zu Azure IoT Edge-Tasks für CI finden Sie unter [Konfigurieren von Azure Pipelines für Continuous Deployment](https://docs.microsoft.com/azure/iot-edge/how-to-ci-cd#configure-azure-pipelines-for-continuous-deployment).

    ![Continuous Deployment-Tasks anzeigen](media/how-to-devops-project/dev-release.png)

1. Wählen Sie auf der rechten Seite **Releases anzeigen** aus. In dieser Ansicht wird der Verlauf von Releases angezeigt.

1. Wählen Sie neben einem Ihrer Releases die Auslassungspunkte (...) und dann die Option **Öffnen** aus.  
Sie können sich verschiedene Menüs ansehen, etwa eine Releasezusammenfassung, zugeordnete Arbeitsaufgaben und Tests.

1. Wählen Sie **Commits** aus. In dieser Ansicht werden die der jeweiligen Bereitstellung zugeordneten Codecommits angezeigt. 

1. Wählen Sie **Protokolle** aus. Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess. Sie können während und nach Bereitstellungen angezeigt werden.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die erstellte Azure App Service-Instanz und zugehörige Ressourcen löschen, wenn Sie sie nicht mehr benötigen. Verwenden Sie dazu die Funktion **Löschen** auf dem DevOps Projects-Dashboard.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen über die Tasks für Azure IoT Edge auf Azure DevOps finden Sie unter [Continuous Integration und Continuous Deployment für Azure IoT Edge](how-to-ci-cd.md).
* Grundlegende Informationen zur IoT Edge-Bereitstellung finden Sie unter [Grundlegendes zu IoT Edge-Bereitstellungen für einzelne Geräte oder bedarfsabhängig](module-deployment-monitoring.md).
* Führen Sie die Schritte zum Erstellen, Aktualisieren oder Löschen einer Bereitstellung in [Bereitstellen und Überwachen von IoT Edge-Modulen im großen Maßstab](how-to-deploy-monitor.md) aus.
