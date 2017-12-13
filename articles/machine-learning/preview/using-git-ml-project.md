---
title: Verwenden eines Git-Repositorys mit einem Azure Machine Learning Workbench-Projekt | Microsoft-Dokumentation
description: "In diesem Artikel wird erläutert, wie ein Git-Repository in Verbindung mit einem Azure Machine Learning Workbench-Projekt verwendet wird."
services: machine-learning
author: hning86
ms.author: haining
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/18/2017
ms.openlocfilehash: f4f1112fe68bdb2a26f68b3da08fe97f9d22d3b7
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2017
---
# <a name="use-a-git-repo-with-a-machine-learning-workbench-project"></a>Verwenden eines Git-Repositorys mit einem Machine Learning Workbench-Projekt
Erfahren Sie, wie Azure Machine Learning Workbench mit Git eine Versionskontrolle bereitstellt und die Reproduzierbarkeit in Ihrem Data Science-Experiment sicherstellt. Erfahren Sie, wie Sie Ihr Projekt mit einem Git-Repository in der Cloud verknüpfen.

Machine Learning Workbench dient zur Git-Integration. Wenn Sie ein neues Projekt erstellen, wird der Projektordner automatisch als lokales Git-Repository initialisiert. Außerdem wird ein zweites, ausgeblendetes lokales Git-Repository mit einem Branch namens „AzureMLHistory/\<Projekt-GUID\>“ erstellt. Der Branch verfolgt Änderungen des Projektordners für jede Ausführung. 

Durch das Verknüpfen eines Azure Machine Learning-Projekts mit einem Git-Repository wird eine automatische Versionskontrolle lokal und remote ermöglicht. Das Git-Repository wird in Visual Studio Team Services (Team Services) gehostet. Da das Machine Learning-Projekt mit einem Git-Repository verknüpft ist, kann jeder Benutzer mit Zugriff auf das Remoterepository den aktuellen Quellcode auf einen anderen Computer herunterladen (Roaming).  

> [!NOTE]
> Team Services verfügt über eine eigene Zugriffssteuerungsliste (ACL), die vom Azure Machine Learning-Experimentieren-Dienst unabhängig ist. Der Benutzerzugriff kann zwischen einem Git-Repository und einem Machine Learning-Arbeitsbereich oder -Projekt variieren. Sie müssen den Zugriff möglicherweise verwalten. 
> 
> Unabhängig davon, ob Sie einem Teammitglied den Zugriff auf Ihr Machine Learning-Projekt auf Codeebene ermöglichen oder einfach den Arbeitsbereich freigeben möchten, müssen Sie dem Benutzer die richtigen Zugriffsberechtigungen für das Team Services-Git-Repository gewähren. 

Zum Verwalten einer Versionskontrolle mit Git können Sie den Hauptbranch verwenden oder andere Branches im Repository erstellen. Sie können auch das lokale Git-Repository verwenden und in das Remote-Git-Repository pushen, sofern dieses bereitgestellt wurde.

Dieses Diagramm zeigt die Beziehung zwischen einem Team Services-Git-Repository und einem Machine Learning-Projekt:

![Azure Machine Learning-Git](media/using-git-ml-project/aml_git.png)

Führen Sie für die Verwendung eines Git-Remoterepositorys zuerst die in den folgenden Abschnitten beschriebenen Schritte aus.

> [!NOTE]
> Azure Machine Learning unterstützt derzeit nur Git-Repositorys in Team Services-Konten. Eine Unterstützung allgemeiner Git-Repositorys wie bei GitHub ist für die Zukunft geplant.

## <a name="step-1-create-a-machine-learning-experimentation-account"></a>Schritt 1: Erstellen eines Machine Learning-Experimentieren-Kontos
Erstellen Sie ein Machine Learning-Experimentieren-Konto, und installieren Sie die Azure Machine Learning Workbench-App. Weitere Informationen finden Sie im [Schnellstart zum Installieren und Erstellen](quickstart-installation.md).

## <a name="step-2-create-a-team-project-or-use-an-existing-team-project"></a>Schritt 2: Erstellen eines Teamprojekts oder Verwenden eines vorhandenes Teamprojekts
Erstellen Sie im [Azure-Portal](https://portal.azure.com/) ein neues Teamprojekt:
1. Wählen Sie **+** aus.
2. Suchen Sie nach **Teamprojekt**.
3. Geben Sie die erforderlichen Informationen ein:
    - **Name:** ein Teamname.
    - **Versionskontrolle:** Wählen Sie **Git** aus.
    - **Abonnement:** Wählen Sie ein Abonnement aus, das über ein Machine Learning-Experimentieren-Konto verfügt.
    - **Standort:** Wählen Sie idealerweise eine Region aus, die sich in der Nähe Ihrer Azure Machine Learning-Experimentieren-Ressourcen befindet.
4. Klicken Sie auf **Erstellen**. 

![Erstellen eines neuen Teamprojekts im Azure-Portal](media/using-git-ml-project/create_vsts_team.png)

Stellen Sie sicher, dass Sie sich mit demselben Azure Active Directory-Konto (Azure AD) anmelden, das Sie für den Zugriff auf die Machine Learning Workbench verwenden. Andernfalls kann das System nicht mithilfe der Azure AD-Anmeldeinformationen auf Machine Learning Workbench zugreifen. Dies gilt nicht, wenn Sie das Machine Learning-Projekt über die Befehlszeile erstellen und ein persönliches Zugriffstoken für den Zugriff auf das Git-Repository bereitstellen. Dies wird später in diesem Artikel ausführlicher erläutert.

Um direkt zum erstellten Teamprojekt zu gelangen, verwenden Sie die URL https://\<Teamprojektname\>.visualstudio.com.

## <a name="step-3-set-up-a-machine-learning-project-and-git-repo"></a>Schritt 3. Einrichten eines Machine Learning-Projekts und eines Git-Repositorys

Zum Einrichten eines Machine Learning-Projekts gibt es zwei Optionen:
- Erstellen eines Machine Learning-Projekts, das über ein Git-Remoterepository verfügt
- Zuordnen eines vorhandenen Machine Learning-Projekts zu einem Team Services-Git-Repository

### <a name="create-a-machine-learning-project-that-has-a-remote-git-repo"></a>Erstellen eines Machine Learning-Projekts, das über ein Git-Remoterepository verfügt
Öffnen Sie Machine Learning Workbench, und erstellen Sie ein neues Projekt. Geben Sie im Feld **Git-Repository** die URL des Team Services-Git-Repositorys aus Schritt 2 ein. In der Regel sieht diese wie folgt aus: https://\<Team Services-Kontoname\>.visualstudio.com/_git/\<Projektname\>.

![Erstellen eines Machine Learning-Projekts, das über ein Git-Repository verfügt](media/using-git-ml-project/create_project_with_git_rep.png)

Sie können das Projekt auch über das Azure-Befehlszeilentool (Azure CLI) erstellen. Sie können ein persönliches Zugriffstoken eingeben. Machine Learning kann mit diesem Token auf das Git-Repository zugreifen, anstatt Ihre Azure AD-Anmeldeinformationen zu verwenden:

```
# Create a new project that has a Git repo by using a personal access token.
$ az ml project create -a <Experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <Team Services personal access token>
```

> [!IMPORTANT]
> Wenn Sie die leere Projektvorlage auswählen, verfügt das ausgewählte Git-Repository möglicherweise bereits über einen Masterbranch. Machine Learning klont den Masterbranch einfach lokal. Dabei werden dem lokalen Projektordner der Ordner „aml_config“ und andere Projekt-Metadatendateien hinzugefügt. 
>
> Wenn Sie eine andere Projektvorlage auswählen, darf das Git-Repository *nicht* bereits über einen Masterbranch verfügen. Andernfalls wird eine Fehlermeldung angezeigt. Alternativ können Sie den Befehl `az ml project create` mit dem Schalter `--force` verwenden, um das Projekt zu erstellen. Dadurch werden die Dateien im ursprünglichen Masterbranch gelöscht und durch die neuen Dateien aus der ausgewählten Vorlage ersetzt.

Es wird ein neues Machine Learning-Projekt mit aktivierter Integration des Git-Remoterepositorys erstellt. Der Projektordner wird immer mit Git als lokalem Git-Repository initialisiert. Das Git-Remoterepository ist auf das Remote-Team Services-Git-Repository festgelegt, damit Commits in das Git-Remoterepository übertragen werden können.

### <a name="associate-an-existing-machine-learning-project-with-a-team-services-git-repo"></a>Zuordnen eines vorhandenen Machine Learning-Projekts zu einem Team Services-Git-Repository
Sie können ein Machine Learning-Projekt ohne ein Team Services-Git-Repository erstellen und sich für die Ausführung von Verlaufsmomentaufnahmen auf das lokale Git-Repository verlassen. Später können Sie ein Team Services-Git-Repository über den folgenden Befehl mit diesem vorhandenen Machine Learning-Projekt verknüpfen:

```azurecli
# Ensure that you are in the project path so Azure CLI has the context of your current project.
$ az ml project update --repo https://<Team Services account name>.visualstudio.com/_git/<project name>
```

> [!NOTE] 
> Sie können die Aktualisierung des Repositorys nur für ein Machine Learning-Projekt ausführen, das nicht mit einem Git-Repository verknüpft ist. Nachdem Sie ein Git-Repository mit einem Machine Learning-Projekt verknüpft haben, können Sie es außerdem nicht mehr entfernen.

## <a name="step-4-capture-a-project-snapshot-in-the-git-repo"></a>Schritt 4: Erfassen einer Projektmomentaufnahme im Git-Repository
Führen Sie einige Skriptdurchläufe im Projekt aus, und nehmen Sie zwischen diesen Durchläufen einige Änderungen vor. Hierzu können Sie entweder die Desktop-App oder den Befehl `az ml experiment submit` der Azure-Befehlszeilenschnittstelle verwenden. Weitere Informationen finden Sie im [Tutorial zum Klassifizieren von Schwertlilien](tutorial-classifying-iris-part-1.md). Wenn eine Änderung an einer Datei im Projektordner erfolgt, wird für jede Ausführung ein Commit für eine Momentaufnahme des gesamten Projektordners gemacht und unter dem Branch „AzureMLHistory/\<Projekt-GUID\>“ in das Git-Remoterepository gepusht. Um die Branches und Commits, einschließlich des Branches „AzureMLHistory/\<Projekt-GUID\>“, anzuzeigen, wechseln Sie zur Team Services-Git-Repository-URL. 

> [!NOTE] 
> Die Momentaufnahme wird nur vor einer Skriptausführung committet. Derzeit wird die Momentaufnahme nicht durch eine Datenvorbereitung oder eine Notebook-Zellenausführung ausgelöst.

![Branch des Ausführungsverlaufs](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> Es wird empfohlen, keine Git-Befehle im Verlaufsbranch auszuführen. Dies könnte den Ausführungsverlauf beeinträchtigen. Verwenden Sie stattdessen den Masterbranch, oder erstellen Sie andere Branches für eigene Git-Vorgänge.

## <a name="step-5-restore-a-previous-project-snapshot"></a>Schritt 5: Wiederherstellen einer früheren Projektmomentaufnahme 
Der gesamte Projektordner kann in Machine Learning Workbench auf den Zustand einer vorherigen Momentaufnahme des Ausführungsverlaufs wiederhergestellt werden:
1. Wählen Sie auf der Aktivitätsleiste (Sanduhrsymbol) **Ausführen** aus.
2. Wählen Sie in der Ansicht **Ausführungsliste** die Ausführung aus, die Sie wiederherstellen möchten.
3. Wählen Sie in der Ansicht **Run Detail** (Ausführungsdetails) die Option **Wiederherstellen** aus.

![Branch des Ausführungsverlaufs](media/using-git-ml-project/restore_project.png)

Optional können Sie die folgenden Befehle im Fenster der Azure-Befehlszeilenschnittstelle in Machine Learning Workbench verwenden:

```azurecli
# Discover the run I want to restore a snapshot from.
$ az ml history list -o table

# Restore the snapshot from a specific run.
$ az ml project restore --run-id <run ID>
```

Seien Sie mit diesem Befehl vorsichtig. Bei Ausführung dieses Befehls wird der gesamte Projektordner mit der Momentaufnahme überschrieben, die beim Start dieser Ausführung aufgenommen wurde. Ihr Projekt verbleibt im aktuellen Branch. Das bedeutet, dass im aktuellen Projektordner **alle Änderungen verloren gehen**.  

Möglicherweise sollten Sie Ihre Änderungen im aktuellen Branch mit Git committen, bevor Sie diesen Vorgang ausführen.

## <a name="step-6-use-the-master-branch"></a>Schritt 6: Verwenden Sie den Masterbranch
Eine Möglichkeit, mit der Sie einen versehentlichen Verlust Ihres derzeitigen Projektzustands verhindern können, ist das Committen des Projekts in den Masterbranch des Git-Repositorys (oder einen anderen selbst erstellten Branch). Sie können Git über die Befehlszeile oder das Git-Clienttool Ihrer Wahl verwenden, um am Masterbranch zu arbeiten. Beispiel:

```sh
# Check status to make sure you are on the master branch (or branch of your choice).
$ git status

# Stage all changes.
$ git add -A

# Commit all changes locally on the master branch.
$ git commit -m 'these are my updates so far'

# Push changes to the remote Team Services Git repo master branch.
$ git push origin master
```

Jetzt können Sie das Projekt auf sichere Weise in eine frühere Momentaufnahme wiederherstellen, indem Sie Schritt 5 abschließen. Sie können jederzeit zum soeben erstellten Commit im Masterbranch zurückkehren.

## <a name="authentication"></a>Authentifizierung
Wenn Sie nur die Funktionen für den Ausführungsverlauf in Machine Learning benötigen, um Projektmomentaufnahmen zu erstellen und wiederherzustellen, müssen Sie sich nicht um die Git-Repositoryauthentifizierung kümmern. Die Authentifizierung wird von der Machine Learning-Experimentieren-Dienstebene erledigt.

Wenn Sie die Versionskontrolle mithilfe eigener Git-Tools verwalten, müssen Sie die Authentifizierung für das Git-Remoterepository in Team Services verarbeiten. Das Git-Remoterepository wird in Machine Learning mithilfe des HTTPS-Protokolls dem lokalen Repository als Git-Remote hinzugefügt. Das hat zur Folge, dass Sie Benutzername und Kennwort oder ein persönliches Zugriffstoken angeben müssen, wenn Sie Git-Befehle (z.B. push oder pull) an das Remoterepository senden. Um ein persönliches Zugriffstoken in einem Team Services-Git-Repository zu erstellen, befolgen Sie die Anweisungen unter [Verwenden eines persönlichen Zugriffstokens für die Authentifizierung](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate).

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum [Verwenden des Team Data Science-Prozesses zum Organisieren der Projektstruktur](how-to-use-tdsp-in-azure-ml.md).
