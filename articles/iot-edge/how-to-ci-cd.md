---
title: Continuous Integration und Continuous Deployment für Azure IoT Edge | Microsoft-Dokumentation
description: Übersicht über Continuous Integration und Continuous Deployment für Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 11/12/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 06dec64a55aaece4cd67ebf0485e34aa206a8936
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51633732"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Continuous Integration und Continuous Deployment für Azure IoT Edge

Sie können DevOps mit [Azure IoT Edge für Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) oder dem [Azure IoT Edge-Plug-In für Jenkins](https://plugins.jenkins.io/azure-iot-edge) ganz einfach in Ihre Azure IoT Edge-Anwendungen übernehmen. In diesem Artikel wird gezeigt, wie Sie die Features für Continuous Integration und Continuous Deployment von Azure Pipelines und Microsoft Team Foundation Server (TFS) zum schnellen und effizienten Erstellen, Testen und Bereitstellen von Anwendungen in Azure IoT Edge verwenden können. 

In diesem Artikel wird Folgendes behandelt:
* Erstellen und Einchecken einer IoT Edge-Beispielprojektmappe
* Installieren der Azure IoT Edge-Erweiterung für Ihr Azure DevOps
* Konfigurieren von Continuous Integration (CI) zum Erstellen der Projektmappe
* Konfigurieren von Continuous Deployment (CD) zum Bereitstellen der Projektmappe und Anzeigen von Antworten

Das Abschließen der Schritte in diesem Artikel dauert ca. 30 Minuten.

![CI und CD](./media/how-to-ci-cd/cd.png)


## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Erstellen einer Azure IoT Edge-Beispielprojektmappe mit Visual Studio Code

In diesem Abschnitt erstellen Sie eine IoT Edge-Beispielprojektmappe mit Komponententests, die als Teil des Buildprozesses ausgeführt werden können. Führen Sie die Schritte unter [Entwickeln einer IoT Edge-Projektmappe mit mehreren Modulen in Visual Studio Code](tutorial-multiple-modules-in-vscode.md) aus, bevor Sie die Anleitung in diesem Abschnitt befolgen.

1. Geben Sie in der Befehlspalette von VS Code den Befehl **Azure IoT Edge: New IoT Edge solution** ein, und führen Sie ihn aus. Wählen Sie dann Ihren Arbeitsbereichsordner aus, geben Sie den Namen der Projektmappe ein (der Standardname ist **EdgeSolution**), und erstellen Sie ein C#-Modul (**FilterModule**) als erstes Benutzermodul in dieser Projektmappe. Sie müssen außerdem das Docker-Image-Repository für Ihr erstes Modul angeben. Das Image-Standardrepository basiert auf einer lokalen Docker-Registrierung (`localhost:5000/filtermodule`). Sie müssen es in Azure Container Registry (`<your container registry address>/filtermodule`) oder Docker Hub für die weitere Continuous Integration ändern.

    ![Einrichten von ACR](./media/how-to-ci-cd/acr.png)

2. Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Sie können optional den Befehl **Azure IoT Edge: Add IoT Edge module** eingeben und ausführen, um weitere Module hinzuzufügen. Im Stammordner befinden sich ein Ordner `modules`, ein Ordner `.vscode` und eine Bereitstellungsmanifest-Vorlagendatei. Jeglicher Code von Benutzermodulen wird in Unterordnern des Ordners `modules` gespeichert. `deployment.template.json` ist die Vorlage für das Bereitstellungsmanifest. Einige der Parameter in dieser Datei werden durch Analyse von `module.json` gewonnen, die in jedem Modulordner vorhanden ist.

3. Nun ist Ihre IoT Edge-Beispielprojektmappe bereit. Das C#-Standardmodul fungiert als Nachrichten-Pipe-Modul. In der `deployment.template.json` können Sie sehen, dass diese Projektmappe zwei Module enthält. Die Nachricht wird vom `tempSensor`-Modul generiert, direkt über `FilterModule` weitergeleitet und dann an Ihren IoT-Hub gesendet.

4. Speichern Sie diese Projekte, und checken Sie sie dann in Ihr Azure Repos- oder TFS-Repository ein.
    
> [!NOTE]
> Weitere Informationen zur Verwendung von Azure Repos finden Sie unter [Freigeben von Code mit Visual Studio und Azure-Repositorys](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-azure-pipelines-for-continuous-integration"></a>Konfigurieren von Azure Pipelines für Continuous Integration
In diesem Abschnitt erstellen Sie eine Buildpipeline, die so konfiguriert ist, dass sie beim Einchecken von Änderungen an der IoT Edge-Beispielprojektmappe automatisch ausgeführt wird und Erstellungsprotokolle in Azure Pipelines anzeigt.

1. Melden Sie sich bei Ihrer Azure DevOps-Organisation (**https://dev.azure.com/{your organization}/**) an, und öffnen Sie das Projekt, in das Sie die Beispiel-App eingecheckt haben.

    ![Einchecken von Code](./media/how-to-ci-cd/init-project.png)

1. Besuchen Sie [Azure IoT Edge für Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) im Azure DevOps Marketplace. Klicken Sie auf die Option **Kostenlos erhalten**, und führen Sie die Schritte im Assistenten aus, um diese Erweiterung in Ihrer Azure DevOps-Organisation zu installieren oder auf Ihren TFS herunterzuladen.

    ![Installieren der Erweiterung](./media/how-to-ci-cd/install-extension.png)

1. Öffnen Sie in Azure Pipelines den Hub **Build und Release**, und wählen Sie auf der Registerkarte **Builds** die Option **+ Neue Pipeline** aus. Wenn Sie bereits über Buildpipelines verfügen, wählen Sie die Schaltfläche **+ Neu** aus.

    ![Neue Pipeline](./media/how-to-ci-cd/add-new-build.png)

1. Wenn Sie dazu aufgefordert werden, wählen Sie **Git** als Quelltyp aus. Wählen Sie dann das Projekt, das Repository und den Branch aus, in dem sich der Code befindet. Klicken Sie auf **Weiter**.

    ![Auswählen von Git](./media/how-to-ci-cd/select-vsts-git.png)

    Wählen Sie im Fenster **Vorlage auswählen** die Option zum **Starten mit einem leeren Prozess** aus.

    ![Vorlage auswählen](./media/how-to-ci-cd/start-with-empty.png)

1. Wählen Sie im Pipeline-Editor den Agent-Pool aus. 
    
    * Wenn Sie Ihre Module auf der Plattform amd64 für Linux-Container erstellen möchten, wählen Sie **Hosted Ubuntu 1604** aus.
    * Wenn Sie Ihre Module auf der Plattform amd64 für Windows-Container erstellen möchten, wählen Sie **Hosted VS2017** aus. 
    * Wenn Sie Ihre Module auf der Plattform arm32v7 für Linux-Container erstellen möchten, müssen Sie einen eigenen Build-Agent einrichten, indem Sie auf die Schaltfläche **Verwalten** klicken.
    
    ![Konfigurieren des Build-Agents](./media/how-to-ci-cd/configure-env.png)

1. Klicken Sie unter „Agent-Auftrag“ auf „+“, um in der Buildpipeline zwei Aufgaben hinzuzufügen. Die erste stammt von **Azure IoT Edge**. Die zweite stammt von **Buildartefakte veröffentlichen**.
    
    ![Hinzufügen von Aufgaben](./media/how-to-ci-cd/add-tasks.png)

1. Aktualisieren Sie in der ersten **Azure IoT Edge**-Aufgabe den **Anzeigenamen** in **Modul erstellen und pushen**, und wählen Sie in der Dropdownliste **Aktion** den Eintrag **Build and Push** (Erstellen und pushen) aus. Fügen Sie im Textfeld **Module.json-Datei** den unten angegebenen Pfad hinzu. Wählen Sie dann den **Containerregistrierungstyp** aus, und stellen Sie sicher, dass Sie die gleiche Registrierung in Ihrem Code konfigurieren und auswählen („module.json“). Mit dieser Aufgabe werden alle Module in der Projektmappe erstellt und mithilfe von Push übertragen sowie in der von Ihnen angegebenen Containerregistrierung veröffentlicht. Wenn Ihre Module in verschiedene Registrierungen übertragen werden, verfügen Sie über mehrere Aufgaben zum **Erstellen und Pushen von Modulen**. Falls sich die IoT Edge-Projektmappe nicht im Stamm des Coderepositorys befindet, können Sie in der Builddefinition **Path of Edge solution root** (Pfad zum Stamm der Edge-Projektmappe) angeben.
    
    ![Erstellen und Pushen](./media/how-to-ci-cd/build-and-push.png)

1. In der Aufgabe **Buildartefakte veröffentlichen** geben Sie die Bereitstellungsdatei an, die von der Buildaufgabe generiert wurde. Legen Sie **Pfad für Veröffentlichung** auf „config/deployment.json“ fest. Wenn Sie in der letzten Aufgabe **Path of Edge solution root** (Pfad zum Stamm der Edge-Projektmappe) festgelegt haben, müssen Sie den Stammpfad hier verknüpfen. Wenn „Path of Edge solution root“ (Pfad zum Stamm der Edge-Projektmappe) beispielsweise „./edgesolution“ lautet, muss **Pfad für Veröffentlichung** „./edgesolution/config/deployment.json“ lauten. Die Datei `deployment.json` wird bei der Erstellung generiert, daher können Sie die roten Fehlerzeilen im Textfeld problemlos ignorieren. 

    ![Veröffentlichen eines Artefakts](./media/how-to-ci-cd/publish-build-artifacts.png)

1. Öffnen Sie die Registerkarte **Trigger**, und aktivieren Sie den Trigger **Continuous Integration**. Stellen Sie sicher, dass der Branch mit Ihrem Code enthalten ist.

    ![Konfigurieren des Triggers](./media/how-to-ci-cd/configure-trigger.png)

    Speichern Sie die neue Buildpipeline. Klicken Sie auf die Schaltfläche **Save** .


## <a name="configure-azure-pipelines-for-continuous-deployment"></a>Konfigurieren von Azure Pipelines für Continuous Deployment
In diesem Abschnitt erstellen Sie eine Releasepipeline, die so konfiguriert ist, dass sie beim Ablegen von Artefakten durch Ihre Buildpipeline automatisch ausgeführt wird und Erstellungsprotokolle in Azure Pipelines anzeigt.

1. Wählen Sie auf der Registerkarte **Releases** die Option **+ Neue Pipeline** aus. Wenn Sie bereits über Releasepipelines verfügen, wählen Sie stattdessen die Schaltfläche **+ Neu** aus.  

    ![Hinzufügen einer Releasepipeline](./media/how-to-ci-cd/add-release-pipeline.png)

    Wählen Sie im Fenster **Vorlage auswählen** die Option **start with an Empty job** (Mit einem leeren Auftrag starten) aus.

    ![Starten mit einem leeren Auftrag](./media/how-to-ci-cd/start-with-empty-job.png)

2. Die Releasepipeline wird dann mit einer Phase initialisiert: **Phase 1**. Benennen Sie die **Phase 1** in **QA** um, und behandeln Sie sie als Testumgebung. In einer typischen Continuous Deployment-Pipeline sind in der Regel mehrere Phasen vorhanden. Sie können basierend auf Ihrem DevOps-Verfahren weitere erstellen.

    ![Erstellen einer Phase](./media/how-to-ci-cd/QA-env.png)

3. Verknüpfen Sie das Release mit den Buildartefakten. Klicken Sie im Artefaktbereich auf **Hinzufügen**.

    ![Hinzufügen von Artefakten](./media/how-to-ci-cd/add-artifacts.png)  
    
    Wählen Sie auf der Seite **Artefakt hinzufügen** den Quellentyp **Build** aus. Wählen Sie dann das Projekt und die erstellte Buildpipeline aus. Klicken Sie anschließend auf **Hinzufügen**.

    ![Hinzufügen eines Artefakts](./media/how-to-ci-cd/add-an-artifact.png)

    Öffnen Sie einen Continuous Deployment-Trigger, sodass immer, wenn ein neuer Build verfügbar ist, ein neues Release erstellt wird.

    ![Konfigurieren des Triggers](./media/how-to-ci-cd/add-a-trigger.png)

4. Navigieren Sie zur Phase **QA**, und konfigurieren Sie die Aufgaben in dieser Phase.

    ![Konfigurieren der QA-Aufgaben](./media/how-to-ci-cd/view-stage-tasks.png)

   Bei Bereitstellungsaufgaben ist die Plattform nicht von Bedeutung, sodass Sie im **Agent-Pool** eine der Optionen **Hosted VS2017** oder **Hosted Ubuntu 1604** (oder einen beliebigen anderen, von Ihnen selbst verwalteten Agent) auswählen können. Klicken Sie auf „+“, und fügen Sie eine Aufgabe hinzu.

    ![Hinzufügen von Aufgaben für QA](./media/how-to-ci-cd/add-task-qa.png)

5. Navigieren Sie in der Azure IoT Edge-Aufgabe zur Dropdownliste **Aktion**, und wählen Sie **Bereitstellen für IoT Edge** aus. Wählen Sie Ihr **Azure-Abonnement** aus, und geben Sie Ihren **IoT Hub-Namen** ein. Sie können eine IoT Edge-**Bereitstellungs-ID** und die **Bereitstellungspriorität** angeben. Sie können auch eine Bereitstellung auf einem einzelnen oder mehreren Geräten auswählen. Bei einer Bereitstellung auf **mehreren Geräten** müssen Sie die **Zielbedingung** des Geräts angeben. Die Zielbedingung ist ein Filter, der mit einem Satz von Edge-Geräten in IoT Hub übereinstimmen muss. Wenn Sie Gerätetags als Bedingung verwenden möchten, müssen Sie die Tags der entsprechenden Geräte mit einem Iot Hub-Gerätezwilling aktualisieren. Angenommen, mehrere Ihrer IoT Edge-Geräte wurden als „qa“ gekennzeichnet, dann entspricht die Taskkonfiguration dem folgenden Screenshot. 

    ![Bereitstellen für QA](./media/how-to-ci-cd/deploy-to-qa.png)

    Speichern Sie die neue Releasepipeline. Klicken Sie auf die Schaltfläche **Save** . Klicken Sie dann auf **Pipeline**, um zur Pipeline zurückzukehren.

6. Die zweite Phase dient für Ihre Produktionsumgebung. Um die neue Phase „PROD“ hinzuzufügen, können Sie einfach die Phase „QA“ klonen und die geklonten Phase in **PROD** umbenennen.

    ![Klonen der Phase](./media/how-to-ci-cd/clone-stage.png)

7. Konfigurieren Sie die Aufgaben für Ihre Produktionsumgebung. Angenommen, mehrere Ihrer IoT Edge-Geräte wurden als „prod“ gekennzeichnet, dann müssen Sie in den Taskkonfigurationen die Zielbedingung in „prod“ aktualisieren und als Bereitstellungs-ID „deploy-prod“ festlegen. Klicken Sie auf die Schaltfläche **Save** . Klicken Sie dann auf **Pipeline**, um zur Pipeline zurückzukehren.
    
    ![Bereitstellen für die Produktion](./media/how-to-ci-cd/deploy-to-prod.png)

7. Derzeit wird unser Buildartefakt kontinuierlich in der Phase **QA** und dann in der Phase **PROD** ausgelöst. Meistens müssen Sie jedoch einige Testfälle auf den QA-Geräten integrieren und die Bits manuell genehmigen. Später werden die Bits in der PROD-Umgebung bereitgestellt. Richten Sie eine Genehmigung in der Phase PROD wie folgt ein.

    1. Öffnen Sie den Einstellungsbereich **Bedingungen vor der Bereitstellung**.

        ![Öffnen von „Bedingungen vor der Bereitstellung“](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Legen Sie unter **Genehmigungen vor der Bereitstellung** die Option **Aktiviert** fest. Geben Sie dann die Hinweise für **Genehmigende Personen** ein. Klicken Sie anschließend auf **Speichern**.
    
        ![Festlegen von Bedingungen](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


8. Ihre Releasepipeline wurde nun wie folgt eingerichtet.

    ![Releasepipeline](./media/how-to-ci-cd/release-pipeline.png)

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Überprüfen von CI/CD in IoT Edge mit den Build- und Releasepipelines

In diesem Abschnitt lösen Sie einen Build aus, um die Funktion der CI/CD-Pipeline anzustoßen. Anschließend überprüfen Sie, ob die Bereitstellung erfolgreich ausgeführt wird.

1. Um einen Buildauftrag auszulösen, können Sie einen Commit in das Quellcoderepository pushen oder manuell auslösen. Sie können einen Buildauftrag in Ihrer Buildpipeline auslösen, indem Sie auf die Schaltfläche **Warteschlange** klicken, wie im folgenden Screenshot dargestellt.

    ![Manueller Trigger](./media/how-to-ci-cd/manual-trigger.png)

2. Wenn die Buildpipeline erfolgreich abgeschlossen wurde, wird ein Release in der Phase **QA** ausgelöst. Wenn Sie zu den Protokollen der Buildpipeline navigieren, sollten Sie Folgendes sehen.

    ![Buildprotokolle](./media/how-to-ci-cd/build-logs.png)

3. Die erfolgreiche Bereitstellung in der Phase **QA** sollte eine Benachrichtigung an die genehmigende Person auslösen. Wenn Sie zur Releasepipeline navigieren, sehen Sie Folgendes. 

    ![Ausstehende Genehmigung](./media/how-to-ci-cd/pending-approval.png)


4. Nachdem die genehmigende Person diese Änderung genehmigt, kann sie in **PROD** bereitgestellt werden.

    ![Bereitstellen in PROD](./media/how-to-ci-cd/approve-and-deploy-to-prod.png)


## <a name="next-steps"></a>Nächste Schritte

* Grundlegende Informationen zur IoT Edge-Bereitstellung finden Sie unter [Grundlegendes zu IoT Edge-Bereitstellungen für einzelne Geräte oder bedarfsabhängig](module-deployment-monitoring.md).
* Führen Sie die Schritte zum Erstellen, Aktualisieren oder Löschen einer Bereitstellung in [Bereitstellen und Überwachen von IoT Edge-Modulen im großen Maßstab](how-to-deploy-monitor.md) aus.
