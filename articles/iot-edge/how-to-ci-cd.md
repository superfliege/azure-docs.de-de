---
title: Continuous Integration und Continuous Deployment – Azure IoT Edge | Microsoft-Dokumentation
description: Einrichten von Continuous Integration und Continuous Deployment – Azure IoT Edge mit Azure DevOp, Azure Pipelines
author: shizn
manager: philmea
ms.author: xshi
ms.date: 12/12/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 3c4f5d6888d581cb44702a8d76e1ebbb13845091
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53582914"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Continuous Integration und Continuous Deployment für Azure IoT Edge

Sie können DevOps mit den integrierten Azure IoT Edge-Aufgaben in Azure Pipelines oder dem [Azure IoT Edge-Plug-In für Jenkins](https://plugins.jenkins.io/azure-iot-edge) auf Ihrem Jenkins-Server ganz einfach in Ihren Azure IoT Edge-Anwendungen übernehmen. In diesem Artikel wird gezeigt, wie Sie die Features für Continuous Integration und Continuous Deployment von Azure Pipelines zum schnellen und effizienten Erstellen, Testen und Bereitstellen von Anwendungen in Azure IoT Edge verwenden können. 

In diesem Artikel wird Folgendes behandelt:
* Erstellen und Einchecken einer IoT Edge-Beispielprojektmappe
* Konfigurieren von Continuous Integration (CI) zum Erstellen der Projektmappe
* Konfigurieren von Continuous Deployment (CD) zum Bereitstellen der Projektmappe und Anzeigen von Antworten

![Diagramm – CI- und CD-Branches für Entwicklung und Produktion](./media/how-to-ci-cd/cd.png)


## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Erstellen einer Azure IoT Edge-Beispielprojektmappe mit Visual Studio Code

In diesem Abschnitt erstellen Sie eine IoT Edge-Beispielprojektmappe mit Komponententests, die als Teil des Buildprozesses ausgeführt werden können. Führen Sie die Schritte unter [Entwickeln einer IoT Edge-Projektmappe mit mehreren Modulen in Visual Studio Code](how-to-develop-multiple-modules-vscode.md) aus, bevor Sie die Anleitung in diesem Abschnitt befolgen.

1. Geben Sie in der Befehlspalette von VS Code den Befehl **Azure IoT Edge: New IoT Edge Solution** (Azure IoT Edge: Neue IoT Edge-Projektmappe) ein, und führen Sie ihn aus. Wählen Sie dann Ihren Arbeitsbereichsordner aus, geben Sie den Namen der Projektmappe ein (der Standardname ist **EdgeSolution**), und erstellen Sie ein C#-Modul (**FilterModule**) als erstes Benutzermodul in dieser Projektmappe. Sie müssen außerdem das Docker-Image-Repository für Ihr erstes Modul angeben. Das Image-Standardrepository basiert auf einer lokalen Docker-Registrierung (`localhost:5000/filtermodule`). Ändern Sie es in Azure Container Registry (`<your container registry address>/filtermodule`) oder Docker Hub für die weitere Continuous Integration.

    ![Einrichten der Azure Container Registry](./media/how-to-ci-cd/acr.png)

2. Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Sie können optional **Azure IoT Edge: IoT Edge-Modul hinzufügen** eingeben und den Befehl ausführen. Im Stammordner befinden sich ein Ordner `modules`, ein Ordner `.vscode` und eine Bereitstellungsmanifest-Vorlagendatei. Jeglicher Code von Benutzermodulen wird in Unterordnern des Ordners `modules` gespeichert. `deployment.template.json` ist die Vorlage für das Bereitstellungsmanifest. Einige der Parameter in dieser Datei werden durch Analyse von `module.json` gewonnen, die in jedem Modulordner vorhanden ist.

3. Nun ist Ihre IoT Edge-Beispielprojektmappe bereit. Das C#-Standardmodul fungiert als Nachrichten-Pipe-Modul. In der `deployment.template.json` können Sie sehen, dass diese Projektmappe zwei Module enthält. Die Nachricht wird vom `tempSensor`-Modul generiert, direkt über `FilterModule` weitergeleitet und dann an Ihren IoT-Hub gesendet.

4. Speichern Sie diese Projekte, und committen Sie sie dann in Ihr Azure Repos-Repository.
    
> [!NOTE]
> Weitere Informationen zur Verwendung von Azure Repos finden Sie unter [Freigeben von Code mit Visual Studio und Azure-Repositorys](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-azure-pipelines-for-continuous-integration"></a>Konfigurieren von Azure Pipelines für Continuous Integration
In diesem Abschnitt erstellen Sie eine Buildpipeline, die so konfiguriert ist, dass sie beim Einchecken von Änderungen an der IoT Edge-Beispielprojektmappe automatisch ausgeführt wird und Erstellungsprotokolle in Azure Pipelines anzeigt.

1. Melden Sie sich bei Ihrer Azure DevOps-Organisation (**https://dev.azure.com/{your organization}/**) an, und öffnen Sie das Projekt, in das Sie die Beispiel-App eingecheckt haben.

    ![Einchecken von Code in Azure Pipelines](./media/how-to-ci-cd/init-project.png)

1. Öffnen Sie in Azure Pipelines die Registerkarte **Builds**, und wählen Sie **+ Neue Pipeline** aus. Wenn Sie bereits über Buildpipelines verfügen, wählen Sie die Schaltfläche **+ Neu** aus. Wählen Sie dann **Neue Buildpipeline** aus.

    ![Erstellen einer neuen Buildpipeline](./media/how-to-ci-cd/add-new-build.png)

1. Wählen Sie bei der entsprechenden Aufforderung „Azure Repos“ als Ihre Quelle aus. Wählen Sie dann das Projekt, das Repository und den Branch aus, in dem sich der Code befindet. Klicken Sie auf **Weiter**.

    ![Auswählen von Azure Repos Git](./media/how-to-ci-cd/select-vsts-git.png)

    Wählen Sie im Fenster **Vorlage auswählen** die Option zum **Starten mit einem leeren Prozess** aus.

    ![Starten mit einem leeren Prozess](./media/how-to-ci-cd/start-with-empty.png)

1. Wählen Sie im Pipeline-Editor den Agent-Pool aus. 
    
    * Wenn Sie Ihre Module auf der Plattform amd64 für Linux-Container erstellen möchten, wählen Sie **Hosted Ubuntu 1604** aus.
    * Wenn Sie Ihre Module auf der Plattform amd64 für Windows-Container erstellen möchten, wählen Sie **Hosted VS2017** aus. 
    * Wenn Sie Ihre Module auf der Plattform arm32v7 für Linux-Container erstellen möchten, müssen Sie einen eigenen Build-Agent einrichten, indem Sie auf die Schaltfläche **Verwalten** klicken.
    
    ![Konfigurieren des Build-Agent-Pools](./media/how-to-ci-cd/configure-env.png)

1. Öffnen Sie unter „Agent-Auftrag“ „+“, um in der Buildpipeline drei Aufgaben hinzuzufügen. Die ersten beiden stammen von **Azure IoT Edge**. Die dritte stammt von **Buildartefakte veröffentlichen**.
    
    ![Hinzufügen von Aufgaben zur Buildpipeline](./media/how-to-ci-cd/add-tasks.png)

1. Ändern Sie in der ersten **Azure IoT Edge**-Aufgabe den **Anzeigenamen** in **Azure IoT Edge - Build module images** (Azure IoT Edge - Modulimages erstellen), und wählen Sie in der Dropdownliste **Aktion** den Eintrag **Build module images** (Modulimages erstellen) aus. Wählen Sie im Feld **.template.json file** (.template.json-Datei) die Datei **deployment.template.json** aus, die Ihre IoT Edge-Projektmappe beschreibt. Wählen Sie dann **Standardplattform** aus. Achten Sie darauf, dass Sie die gleiche Plattform wie für das IoT Edge-Gerät auswählen. Mit dieser Aufgabe werden alle Module in der Projektmappe mit der angegebenen Zielplattform erstellt. Generieren Sie auch die Datei **deployment.json**. Den Dateipfad finden Sie unter „Ausgabevariablen“. Legen Sie den Alias für diese Variable auf `edge` fest.
    
    ![Konfigurieren der Aufgabe zum Erstellen von Modulimages](./media/how-to-ci-cd/build-and-push.png)

1. Ändern Sie in der zweiten **Azure IoT Edge**-Aufgabe den **Anzeigenamen** in **Azure IoT Edge – Push module images** (Azure IoT Edge – Modulimages übertragen), und wählen Sie in der Dropdownliste **Aktion** den Eintrag **Push module images** (Modulimages übertragen) aus. Wählen Sie den Containerregistrierungstyp aus, und stellen Sie sicher, dass Sie die gleiche Registrierung in Ihrem Code konfigurieren und auswählen („module.json“). Wählen Sie im Feld **.template.json file** (.template.json-Datei) die Datei **deployment.template.json** aus, die Ihre IoT Edge-Projektmappe beschreibt. Wählen Sie dann **Standardplattform** aus. Achten Sie darauf, dass Sie die gleiche Plattform wie für die erstellten Modulimages auswählen. Mit dieser Aufgabe werden alle Modulimages in die ausgewählte Containerregistrierung übertragen. Fügen Sie die Anmeldeinformationen für die Containerregistrierung auch in der Datei **deployment.json** ein. Den Dateipfad finden Sie unter „Ausgabevariablen“. Legen Sie den Alias für diese Variable auf `edge` fest. Wenn Sie die Modulimages in mehreren Containerregistrierungen hosten können, müssen Sie diese Aufgabe duplizieren. Wählen Sie eine andere Containerregistrierung aus, und verwenden Sie **Bypass module(s)** (Modul(e) umgehen) in den erweiterten Einstellungen, um die Images auszulassen, die nicht für diese spezifische Registrierung bestimmt sind.

    ![Konfigurieren der Aufgabe zum Übertragen von Modulimages per Push](./media/how-to-ci-cd/push.png)

1. In der Aufgabe **Buildartefakte veröffentlichen** geben Sie die Bereitstellungsdatei an, die von der Buildaufgabe generiert wurde. Legen Sie **Pfad für Veröffentlichung** auf `$(edge.DEPLOYMENT_FILE_PATH)` fest.

    ![Konfigurieren der Aufgaben zum Veröffentlichen von Artefakten](./media/how-to-ci-cd/publish-build-artifacts.png)

1. Öffnen Sie die Registerkarte **Trigger**, und aktivieren Sie den Trigger **Continuous Integration**. Stellen Sie sicher, dass der Branch mit Ihrem Code enthalten ist.

    ![Aktivieren des Continuous Integration-Triggers](./media/how-to-ci-cd/configure-trigger.png)

    Speichern Sie die neue Buildpipeline mit der Schaltfläche **Speichern**.


## <a name="configure-azure-pipelines-for-continuous-deployment"></a>Konfigurieren von Azure Pipelines für Continuous Deployment
In diesem Abschnitt erstellen Sie eine Releasepipeline, die so konfiguriert ist, dass sie beim Ablegen von Artefakten durch Ihre Buildpipeline automatisch ausgeführt wird und Erstellungsprotokolle in Azure Pipelines anzeigt.

1. Wählen Sie auf der Registerkarte **Releases** die Option **+ Neue Pipeline** aus. Wenn Sie bereits über Releasepipelines verfügen, wählen Sie die Schaltfläche **+ Neu** und dann **+ Neue Releasepipeline** aus.  

    ![Hinzufügen einer Releasepipeline](./media/how-to-ci-cd/add-release-pipeline.png)

    Wählen Sie im Fenster **Vorlage auswählen** die Option **start with an Empty job** (Mit einem leeren Auftrag starten) aus.

    ![Starten mit einem leeren Auftrag](./media/how-to-ci-cd/start-with-empty-job.png)

2. Die Releasepipeline wird dann mit einer Phase initialisiert: **Phase 1**. Benennen Sie die **Phase 1** in **QA** um, und behandeln Sie sie als Testumgebung. In einer typischen Continuous Deployment-Pipeline sind in der Regel mehrere Phasen vorhanden. Sie können basierend auf Ihrem DevOps-Verfahren weitere erstellen.

    ![Erstellen der Testumgebungsphase](./media/how-to-ci-cd/QA-env.png)

3. Verknüpfen Sie das Release mit den Buildartefakten. Klicken Sie im Artefaktbereich auf **Hinzufügen**.

    ![Hinzufügen von Artefakten](./media/how-to-ci-cd/add-artifacts.png)  
    
    Wählen Sie auf der Seite **Artefakt hinzufügen** den Quellentyp **Build** aus. Wählen Sie dann das Projekt und die erstellte Buildpipeline aus. Wählen Sie anschließend **Hinzufügen**.

    ![Hinzufügen eines Buildartefakts](./media/how-to-ci-cd/add-an-artifact.png)

    Öffnen Sie einen Continuous Deployment-Trigger, sodass immer, wenn ein neuer Build verfügbar ist, ein neues Release erstellt wird.

    ![Konfigurieren des Continuous Deployment-Triggers](./media/how-to-ci-cd/add-a-trigger.png)

4. Navigieren Sie zur Phase **QA**, und konfigurieren Sie die Aufgaben in dieser Phase.

    ![Konfigurieren der QA-Aufgaben](./media/how-to-ci-cd/view-stage-tasks.png)

   Bei Bereitstellungsaufgaben ist die Plattform nicht von Bedeutung, sodass Sie im **Agent-Pool** eine der Optionen **Hosted VS2017** oder **Hosted Ubuntu 1604** (oder einen beliebigen anderen, von Ihnen selbst verwalteten Agent) auswählen können. Wählen Sie „+“ aus, und fügen Sie eine Aufgabe hinzu.

    ![Hinzufügen von Aufgaben für QA](./media/how-to-ci-cd/add-task-qa.png)

5. Navigieren Sie in der Azure IoT Edge-Aufgabe zur Dropdownliste **Aktion**, und wählen Sie **Bereitstellen für IoT Edge** aus. Wählen Sie Ihr **Azure-Abonnement** aus, und geben Sie Ihren **IoT Hub-Namen** ein. Sie können eine Bereitstellung auf einem einzelnen oder mehreren Geräten auswählen. Bei einer Bereitstellung auf **mehreren Geräten** müssen Sie die **Zielbedingung** des Geräts angeben. Die Zielbedingung ist ein Filter, der mit einem Satz von Edge-Geräten in IoT Hub übereinstimmen muss. Wenn Sie Gerätetags als Bedingung verwenden möchten, müssen Sie die Tags der entsprechenden Geräte mit einem Iot Hub-Gerätezwilling aktualisieren. Ändern Sie die **IoT Edge-Bereitstellungs-ID** in den erweiterten Einstellungen in „deploy-qa“. Angenommen, mehrere Ihrer IoT Edge-Geräte wurden als „qa“ gekennzeichnet, dann entspricht die Taskkonfiguration dem folgenden Screenshot. 

    ![Bereitstellen für QA](./media/how-to-ci-cd/deploy-to-qa.png)

    Speichern Sie die neue Releasepipeline mit der Schaltfläche **Speichern**. Wählen Sie dann **Pipeline** aus, um zur Pipeline zurückzukehren.

6. Die zweite Phase dient für Ihre Produktionsumgebung. Um die neue Phase „PROD“ hinzuzufügen, können Sie die Phase „QA“ klonen und die geklonte Phase in **PROD** umbenennen.

    ![Klonen der Phase](./media/how-to-ci-cd/clone-stage.png)

7. Konfigurieren Sie die Aufgaben für Ihre Produktionsumgebung. Angenommen, mehrere Ihrer IoT Edge-Geräte wurden als „prod“ gekennzeichnet, dann müssen Sie in den Taskkonfigurationen die Zielbedingung in „prod“ ändern und in den erweiterten Einstellungen „deploy-prod“ als Bereitstellungs-ID festlegen. Speichern Sie mit der Schaltfläche **Speichern**. Wählen Sie dann **Pipeline** aus, um zur Pipeline zurückzukehren.
    
    ![Bereitstellen für die Produktion](./media/how-to-ci-cd/deploy-to-prod.png)

7. Derzeit wird unser Buildartefakt kontinuierlich in der Phase **QA** und dann in der Phase **PROD** ausgelöst. Meistens müssen Sie jedoch einige Testfälle auf den QA-Geräten integrieren und die Bits manuell genehmigen. Später werden die Bits in der PROD-Umgebung bereitgestellt. Richten Sie wie im folgenden Screenshot eine Genehmigung in der Phase „PROD“ ein.

    1. Öffnen Sie den Einstellungsbereich **Bedingungen vor der Bereitstellung**.

        ![Öffnen von „Bedingungen vor der Bereitstellung“](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Legen Sie unter **Genehmigungen vor der Bereitstellung** die Option **Aktiviert** fest. Geben Sie dann die Hinweise für **Genehmigende Personen** ein. Speichern Sie anschließend mit der Schaltfläche **Speichern**.
    
        ![Festlegen von Bedingungen](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


8. Ihre Releasepipeline wurde nun wie im folgenden Screenshot eingerichtet.

    ![Releasepipeline](./media/how-to-ci-cd/release-pipeline.png)

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Überprüfen von CI/CD in IoT Edge mit den Build- und Releasepipelines

In diesem Abschnitt lösen Sie einen Build aus, um die Funktion der CI/CD-Pipeline anzustoßen. Anschließend überprüfen Sie, ob die Bereitstellung erfolgreich ausgeführt wird.

1. Um einen Buildauftrag auszulösen, können Sie einen Commit in das Quellcoderepository pushen oder manuell auslösen. Sie können einen Buildauftrag in Ihrer Buildpipeline auslösen, indem Sie die Schaltfläche **Warteschlange** auswählen, wie im folgenden Screenshot dargestellt.

    ![Manueller Trigger](./media/how-to-ci-cd/manual-trigger.png)

2. Wenn die Buildpipeline erfolgreich abgeschlossen wurde, wird ein Release in der Phase **QA** ausgelöst. Wenn Sie zu den Protokollen der Buildpipeline navigieren, sollte der folgende Screenshot angezeigt werden.

    ![Buildprotokolle](./media/how-to-ci-cd/build-logs.png)

3. Die erfolgreiche Bereitstellung in der Phase **QA** sollte eine Benachrichtigung an die genehmigende Person auslösen. Wenn Sie zur Releasepipeline navigieren, wird der folgende Screenshot angezeigt. 

    ![Ausstehende Genehmigung](./media/how-to-ci-cd/pending-approval.png)


4. Nachdem die genehmigende Person diese Änderung genehmigt, kann sie in **PROD** bereitgestellt werden.

    ![Bereitstellen in PROD](./media/how-to-ci-cd/approve-and-deploy-to-prod.png)


## <a name="next-steps"></a>Nächste Schritte

* Grundlegende Informationen zur IoT Edge-Bereitstellung finden Sie unter [Grundlegendes zu IoT Edge-Bereitstellungen für einzelne Geräte oder bedarfsabhängig](module-deployment-monitoring.md).
* Führen Sie die Schritte zum Erstellen, Aktualisieren oder Löschen einer Bereitstellung in [Bereitstellen und Überwachen von IoT Edge-Modulen im großen Maßstab](how-to-deploy-monitor.md) aus.
