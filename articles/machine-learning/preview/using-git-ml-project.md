---
title: Verwenden von Git-Repository mit einem Azure Machine Learning Workbench-Projekt | Microsoft-Dokumentation
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
ms.openlocfilehash: 0cd447a52964578dd2348a786dd57a45ea87516e
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2017
---
# <a name="using-git-repository-with-an-azure-machine-learning-workbench-project"></a>Verwenden von Git-Repository mit einem Azure Machine Learning Workbench-Projekt
Dieses Dokument enthält Informationen dazu, wie Azure Machine Learning Workbench Git verwendet, um die Versionskontrolle bereitzustellen und die Reproduzierbarkeit in Ihrem Data Science-Experiment sicherzustellen. Anweisungen zum Verknüpfen Ihres Projekts mit einer Git-Repository in der Cloud sind ebenfalls enthalten.

## <a name="introduction"></a>Einführung
Azure Machine Learning Workbench ist von Grund auf mit der Git-Integration entworfen. Wenn Sie ein neues Projekt erstellen, wird der Projektordner automatisch als lokales Git-Repository für Git initialisiert. In der Zwischenzeit wird auch ein zweites ausgeblendetes lokales Git-Repository mit einem Branch namens _AzureMLHistory/<Projekt_GUID>_ erstellt, um Änderungen am Projektordner für die einzelnen Ausführungen nachzuverfolgen. 

Wenn Sie das Azure ML-Projekt einem Git-Repository zuordnen, das innerhalb eines Visual Studio Team Service-Projekts (VSTS) gehostet wird, wird eine automatische lokale und Remote-Versionskontrolle aktiviert. Durch diese Zuordnung kann jeder Benutzer mit Zugriff auf das Remote-Repository den aktuellen Quellcode auf einen anderen Computer herunterladen (Roaming).  

> [!NOTE]
> VSTS verfügt über eine eigene Zugriffssteuerungsliste, die unabhängig vom Dienst Azure Machine Learning-Experimentieren ist. Der Benutzerzugriff variiert zwischen einem Git-Repository und dem Azure ML-Arbeitsbereich oder -Projekt und muss ggf. verwaltet werden. Wenn Sie zusätzlich zur Freigabe des Arbeitsplatzes Ihr Azure ML-Projekt, einschließlich des Codelevelzugriffs für ein Teammitglied freigeben möchten, müssen Sie ihm/ihr explizit ordnungsgemäßen Zugriff auf das VSTS-Git-Repository gewähren. 

Mit Git ist es auch möglich, die Versionskontrolle explizit zu verwalten, indem Sie den _Masterbranch_ nutzen, oder indem Sie andere Verzweigungen für das Repository erstellen. Sie können nur das lokale Git-Repository oder das Remote-Git-Repository verwenden, falls es bereitgestellt wurde.

Dieses Diagramm zeigt die Beziehung zwischen einem VSTS-Git-Repository und einem Azure ML-Projekt:

![AML-Git](media/using-git-ml-project/aml_git.png)

Befolgen Sie diese grundlegenden Anweisungen, um ein Remote-Git-Repository zu verwenden.

> [!NOTE]
> Azure Machine Learning unterstützt derzeit nur Git-Repositorys auf VSTS-Konten. Unterstützung für allgemeine Git-Repositorys (z.B. GitHub, usw.) ist in der Zukunft geplant.

## <a name="step-1-create-an-azure-ml-experimentation-account"></a>Schritt 1: Erstellen Sie ein Azure ML-Experimentieren-Konto
Wenn das Konto noch nicht existiert, dann erstellen Sie ein Azure ML-Experimentieren-Konto und installieren Sie die Azure ML Workbench-App. Sie finden weitere Details unter [Create Azure Machine Learning preview accounts and install Azure Machine Learning Workbench (Installieren und Erstellen von Schnellstart)](quickstart-installation.md).

## <a name="step-2-create-a-team-project-or-use-an-existing-team-project"></a>Schritt 2: Erstellen Sie ein Teamprojekt, oder verwenden Sie ein vorhandenes Teamprojekt
Erstellen Sie im [Azure-Portal](https://portal.azure.com/) ein neues **Teamprojekt**.
1. Klicken Sie auf **+**.
2. Suchen Sie nach **„Teamprojekt“**.
3. Geben Sie die erforderlichen Informationen ein.
    - Name: Ein Teamname.
    - Versionskontrolle: **Git**
    - Abonnement: Das Dataset mit einem Azure Machine Learning-Experimentieren-Konto.
    - Ort: Idealerweise in einer Region, die sich in der Nähe Ihrer Azure Machine Learning-Experimentieren-Ressourcen befindet.
4. Klicken Sie auf **Erstellen**. 

![Erstellen Sie im Azure-Portal ein neues Teamprojekt](media/using-git-ml-project/create_vsts_team.png)

Stellen Sie sicher, dass Sie sich mit demselben Azure Active Directory-Konto (AAD) anmelden, das Sie für den Zugriff auf die Azure Machine Learning Workbench verwendet verwenden. Andernfalls kann das System nicht mit Ihren AAD-Anmeldeinformationen darauf zugreifen, sofern Sie nicht die Befehlszeile verwenden, um das Azure ML-Projekt zu erstellen und ein persönliches Zugriffstoken für den Zugriff auf das Git-Repository bereitzustellen. Dazu später weitere Informationen.

Nachdem das Teamprojekt erstellt wurde, sind Sie für den nächsten Schritt bereit.

Navigieren Sie direkt zum gerade erstellten Teamprojekt, die URL lautet `https://<team_project_name>.visualstudio.com`.

## <a name="step-3-create-a-new-azure-ml-project-with-a-remote-git-repo"></a>Schritt 3. Erstellen Sie ein neues Azure ML-Projekt mit einem Remote-Git-Repository
Starten Sie Azure ML Workbench, und erstellen Sie ein neues Projekt. Füllen Sie das Textfeld des Git-Repositorys mit der VSTS-Git-Repository-URL aus Schritt 2. Das sieht in der Regel wie folgt aus: `http://<vsts_account_name>.visualstudio.com/_git/<project_name>`

![Erstellen eines Azure ML-Projekts mit Git-Repository](media/using-git-ml-project/create_project_with_git_rep.png)

Sie können das Projekt auch über das Befehlszeilentool erstellen. Sie können ein persönliches Zugriffstoken bereitstellen. Mit diesem Token kann Azure ML in Ihrem Namen auf das Git-Repository zugreifen, anstatt sich auf Ihre AAD-Anmeldeinformationen verlassen zu müssen:

```
# create a new project with a Git repo and personal access token.
$ az ml project create -a <experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <VSTS personal access token>
```
> [!IMPORTANT]
> Wenn Sie die leere Projektvorlage auswählen, ist das in Ordnung, wenn das von Ihnen ausgewählte Git-Repository bereits über einen _Masterbranch_ verfügt. Azure ML klont den _Masterbranch_ einfach lokal und fügt den Ordner `aml_config` und andere Metadatendateien des Projekts zum lokalen Projektordner hinzu. Wenn Sie jedoch eine andere Projektvorlage auswählen, darf Ihr Git-Repository nicht bereits über einen _Masterbranch_ verfügen, da Sie ansonsten eine Fehlermeldung erhalten. Alternativ können Sie das Befehlszeilentool `az ml project create` verwenden, um das Projekt zu erstellen und den Switch `--force` bereitzustellen. Dadurch werden die Dateien auf dem ursprünglichen Masterbranch gelöscht und durch die neuen Dateien aus der ausgewählten Vorlage ersetzt.

Jetzt wird ein neues Azure ML-Projekt mit aktivierter Remote-Git-Repository-Integration erstellt und einsatzbereit gemacht. Der Projektordner wird immer mit Git als lokalem Git-Repository initialisiert. Und die _Remote_-Git ist auf das Remote-VSTS-Git-Repository festgelegt, damit Commits in Remote-Git-Repository übertragen werden können.

## <a name="step-3a-associate-an-existing-azure-ml-project-with-a-vsts-git-repo"></a>Schritt 3a: Verknüpfen Sie ein vorhandenes Azure ML-Projekt mit einem VSTS-Git-Repository
Sie können optional auch ein Azure ML-Projekt ohne ein VSTS-Git-Repository erstellen, und sich nur auf das lokale Git-Repository für die Ausführung von Verlaufsmomentaufnahmen verlassen. Zudem können Sie später mithilfe des folgenden Befehls ein VSTS-Git-Repository diesem vorhandenen Azure ML-Projekt zuordnen:

```azurecli
# make sure you are in the project path so CLI has context of your current project
$ az ml project update --repo http://<vsts_account_name>.visualstudio.com/_git/<project_name>
```

> [!NOTE] 
> Sie können den Vorgang zur Aktualisierung des Repositorys nur für ein Azure ML-Projekt ausführen, dem kein Git-Repository zugeordnet ist. Nachdem das Git-Repository zugeordnet wurde, kann es nicht entfernt werden.

## <a name="step-4-capture-project-snapshot-in-git-repo"></a>Schritt 4: Erfassen einer Projektmomentaufnahme in Git-Repository
Sie können jetzt einige Skriptdurchläufe im Projekt ausführen und zwischen diesen Durchläufen einige Änderungen vornehmen. Hierzu können Sie entweder die Desktopanwendung oder den `az ml experiment submit`-Befehl der CLI verwenden. Weitere Informationen finden Sie unter [Classifying Iris part 1: Prepare data (Klassifizieren von Iris Teil 1: Vorbereiten von Daten)](tutorial-classifying-iris-part-1.md). Wenn eine Änderung in einer der Dateien im Projektordner vorgenommen wird, wird für jede Ausführung ein Commit für die Momentaufnahme des gesamten Projektordners gemacht und unter einem Branch namens `AzureMLHistory/<Project_GUID>` in das Remote-Git-Repository übertragen. Sie können Branches und Commits anzeigen, indem Sie die URL des VSTS-Git-Repositorys durchsuchen und diesen Branch finden. 

> [!NOTE] 
> Die Momentaufnahme wird nur vor einer Skriptausführung committet. Derzeit wird die Momentaufnahme nicht durch eine Datenvorbereitung oder eine Notebook-Zellenausführung ausgelöst.

![Verzweigung „Ausführungsverlauf“](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> Es wird empfohlen, dass Sie keine Git-Befehle im Verlaufsbranch ausführen. Andernfalls könnte der Ausführungsverlauf durcheinander gebracht werden. Verwenden Sie den Masterbranch, oder erstellen Sie anstelle eigener Git-Operationen andere Branches.

## <a name="step-5-restore-a-previous-project-snapshot"></a>Schritt 5: Wiederherstellen einer früheren Projektmomentaufnahme 
Der gesamte Projektordner kann von der Azure ML Workbench auf den Zustand einer vorherigen Momentaufnahme des Zustands eines Ausführungsverlaufsprojekts wiederhergestellt werden.
1. Klicken Sie auf **Wird ausgeführt** in der Aktivitätsliste (Stundenglassymbol).
2. Klicken Sie in der Ansicht **Ausführungsliste** auf die Ausführung, die Sie wiederherstellen möchten.
3. Klicken Sie in der Ansicht **Ausführungsdetails** auf **Wiederherstellen**.

![Verzweigung „Ausführungsverlauf“](media/using-git-ml-project/restore_project.png)

Alternativ können Sie den folgenden Befehl aus dem Azure ML Workbench-CLI-Fenster ausführen.

```azurecli
# discover the run I want to restore snapshot from:
$ az ml history list -o table

# restore the snapshot from a particular run
$ az ml project restore --run-id <run_id>
```

Bei Ausführung dieses Befehls, wird der gesamte Projektordner mit der Momentaufnahme überschrieben, die aufgenommen wurde, als diese bestimmte Ausführung gestartet wurde. Aber Ihr Projekt verbleibt auf dem aktuellen Branch. Dies bedeutet, dass Sie im aktuellen Projektordner **Alle Änderungen verlieren**. Seien Sie also besonders vorsichtig, wenn Sie diesen Befehl ausführen. Möglicherweise sollten Ihre Änderungen am aktuellen Branch von Git committet werden, bevor Sie den obigen Vorgang ausführen. Weitere Informationen finden Sie unten.

## <a name="step-6-use-the-master-branch"></a>Schritt 6: Verwenden Sie den Masterbranch
Eine Möglichkeit, mit der Sie Ihren derzeitigen Projektzustand beibehalten können, ist die Übernahme des Masterbranch (oder eines anderen selbst erstellten Branch) durch das Git-Repository. Sie können Git direkt über die Befehlszeile (oder das Git-Clienttool Ihrer Wahl) verwenden, um den Masterbranch zu betreiben. Beispiel:

```sh
# check status to make sure you are on the master branch (or branch of your choice)
$ git status

# stage all changes
$ git add -A

# commit all changes locally on the master branch
$ git commit -m 'these are my updates so far'

# push changes into the remote VSTS Git repo master branch.
$ git push origin master
```

Nach Schritt 5 können Sie jetzt das Projekt sicher mit einer früheren Momentaufnahme wiederherstellen, mit dem Wissen, dass Sie immer zum Commit zurückkehren können, den Sie auf dem Masterbranch ausgeführt haben.

## <a name="authentication"></a>Authentifizierung
Wenn Sie nur die Funktionen für den Ausführungsverlauf in Azure ML benötigen, um Projektmomentaufnahmen zu erstellen und wiederherzustellen, müssen Sie sich nicht um die Git-Repository-Authentifizierung kümmern. Dies wird von der Experimentieren-Dienstebene erledigt.

Wenn Sie eigene Git-Tools verwenden, um die Versionskontrolle zu verwalten, müssen Sie die Authentifizierung für das Remote-Git-Repository auf VSTS ordnungsgemäß verarbeiten. Das Git-Remoterepository wird in Azure ML mithilfe des HTTPS-Protokoll als Git-Remote zum lokalen Repository hinzugefügt. Das hat zur Folge, dass Sie Benutzername, Kennwort oder persönliches Zugriffstoken angeben müssen, wenn Sie Git-Befehle an die Remote senden (z. B. „push“ oder „pull“). Führen Sie [diese Anweisungen](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) aus, um ein persönliches Zugriffstoken in einem VSTS Git-Repository zu erstellen.

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Verwenden des Team Data Science-Prozesses zum Organisieren der Projektstruktur finden Sie unter [Structure a project with TDSP (Strukturieren eines Projekts mit TDSP)](how-to-use-tdsp-in-azure-ml.md).
