---
title: Verwenden von Git-Repository mit einem Azure Machine Learning Workbench-Projekt | Microsoft-Dokumentation
description: "In diesem Artikel wird erläutert, wie ein Git-Repository in Verbindung mit einem Azure Machine Learning Workbench-Projekt verwendet wird."
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: hning86
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 59b07c9834904e01256b75344ba2e6892e56438c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="using-git-repository-with-an-azure-machine-learning-workbench-project"></a>Verwenden von Git-Repository mit einem Azure Machine Learning Workbench-Projekt
Dieses Dokument enthält Informationen dazu, wie Azure Machine Learning Workbench Git verwendet, um die Reproduzierbarkeit in Ihrem Data Science-Experiment sicherzustellen. Anweisungen zum Verknüpfen Ihres Projekts mit einer Git-Repository in der Cloud sind ebenfalls enthalten.

## <a name="introduction"></a>Einführung
Azure Machine Learning Workbench ist von Grund auf mit der Git-Integration entworfen. Wenn Sie ein neues Projekt erstellen, wird der Projektordner automatisch als ein lokales Git-Repository (Repo) „mit Git initialisiert“, während ein zweites ausgeblendetes, lokales Git-Repository mit einer Verzweigung namens _AzureMLHistory/<project_GUID>_ erstellt wird, um die Veränderungen des Projektordners für jede Ausführung nachzuverfolgen. 

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


> [!TIP]
> Stellen Sie sicher, dass Sie sich mit dem Azure Active Directory-Konto (AAD) anmelden, das zum Zugriff auf die Azure Machine Learning Workbench verwendet wird. Andernfalls erhält das neue Teamprojekt möglicherweise die falsche Mandanten-ID, und Azure Machine Learning kann es möglicherweise nicht finden. In diesem Fall müssten Sie die Befehlszeilenschnittstelle verwenden und das VSTS-Token angeben.

Nachdem das Teamprojekt erstellt wurde, sind Sie für den nächsten Schritt bereit.

Navigieren Sie direkt zum gerade erstellten Teamprojekt, die URL lautet `https://<team_project_name>.visualstudio.com`.

> [!NOTE]
> Azure Machine Learning unterstützt derzeit nur leere Git-Repositorys ohne Masterbranch. Sie können das –Force-Argument in der Befehlszeilenschnittstelle verwenden, um den Masterbranch zuerst zu löschen. 

## <a name="step-3-create-a-new-azure-ml-project-with-a-remote-git-repo"></a>Schritt 3. Erstellen Sie ein neues Azure ML-Projekt mit einem Remote-Git-Repository
Starten Sie Azure ML Workbench, und erstellen Sie ein neues Projekt. Füllen Sie das Textfeld des Git-Repositorys mit der VSTS-Git-Repository-URL aus Schritt 2. In der Regel sieht diese wie folgt aus: http://<vsts_account_name>.visualstudio.com/_git/<project_name>

![Erstellen eines Azure ML-Projekts mit Git-Repository](media/using-git-ml-project/create_project_with_git_rep.png)

Jetzt wird ein neues Azure ML-Projekt mit aktivierter Remote-Git-Repository-Integration erstellt und einsatzbereit gemacht. Der Projektordner wird immer mit Git als lokalem Git-Repository initialisiert. Und die _Remote_-Git ist auf das Remote-VSTS-Git-Repository festgelegt, damit Commits in Remote-Git-Repository übertragen werden können.

## <a name="step-3a-associate-an-existing-azure-ml-project-with-a-vsts-git-repo"></a>Schritt 3.a Verknüpfen Sie ein vorhandenes Azure ML-Projekt mit einem VSTS-Git-Repository
Sie können optional auch ein Azure ML-Projekt ohne ein VSTS-Git-Repository erstellen, und sich nur auf das lokale Git-Repository für die Ausführung von Verlaufsmomentaufnahmen verlassen. Zudem können Sie später mithilfe des folgenden Befehls ein VSTS-Git-Repository diesem vorhandenen Azure ML-Projekt zuordnen:

```azurecli
# make sure you are in the project path so CLI has context of your current project
az ml project update --repo http://<vsts_account_name>.visualstudio.com/_git/<project_name
```

## <a name="step-4-capture-project-snapshot-in-git-repo"></a>Schritt 4: Erfassen einer Projektmomentaufnahme in Git-Repository
Sie können jetzt einige Testläufe im Projekt ausführen und einige Änderungen zwischen diesen machen. Hierzu können Sie entweder die Desktopanwendung oder den `az ml experiment submit`-Befehl der CLI verwenden. Weitere Informationen finden Sie unter [Classifying Iris part 1: Prepare data (Klassifizieren von Iris Teil 1: Vorbereiten von Daten)](tutorial-classifying-iris-part-1.md). Wenn eine Änderung in einer der Dateien im Projektordner gemacht wird, wird für jede Ausführung ein Commit für die Momentaufnahme des gesamten Projektordners gemacht und in das Remote-Git-Repository übertragen. Sie können Verzweigungen und Commits anzeigen, indem Sie die URL des VSTS-Git-Repositorys durchsuchen.

![Verzweigung „Ausführungsverlauf“](media/using-git-ml-project/run_history_branch.png)

## <a name="step-5-restore-a-previous-project-snapshot"></a>Schritt 5: Wiederherstellen einer früheren Projektmomentaufnahme 
Der gesamte Projektordner kann von der AML Workbench auf den Zustand einer vorherigen Momentaufnahme des Zustands eines Ausführungsverlaufsprojekts wiederhergestellt werden.
1. Klicken Sie auf **Wird ausgeführt** in der Aktivitätsliste (Stundenglassymbol).
2. Klicken Sie in der Ansicht **Ausführungsliste** auf die Ausführung, die Sie wiederherstellen möchten.
3. Klicken Sie in der Ansicht **Ausführungsdetails** auf **Wiederherstellen**.

![Verzweigung „Ausführungsverlauf“](media/using-git-ml-project/restore_project.png)

Alternativ können Sie den folgenden Befehl aus dem Azure ML Workbench-CLI-Fenster ausführen.

```azurecli
# discover the run I want to restore snapshot from:
az ml history list -o table

# restore the snapshot from a particular run
az ml project restore --run-id <run_id>
```

Bei Ausführung dieses Befehls, wird der gesamte Projektordner mit der Momentaufnahme überschrieben, die aufgenommen wurde, als diese bestimmte Ausführung gestartet wurde. Dies bedeutet, dass Sie im aktuellen Projektordner **Alle Änderungen verlieren**. Seien Sie also besonders vorsichtig, wenn Sie diesen Befehl ausführen.

## <a name="step-6-use-the-master-branch"></a>Schritt 6: Verwenden Sie den Masterbranch
Eine Möglichkeit, mit der Sie Ihren derzeitigen Projektzustand beibehalten können, ist die Übernahme des Masterbranch durch das Git-Repository. Sie können Git direkt über die Befehlszeile (oder das Git-Clienttool Ihrer Wahl) verwenden, um den Masterbranch zu betreiben. Beispiel:

```
# make sure you are on the master branch
git checkout master

# stage all changes
git add -A

# commit all changes locally on the master branch
git commit -m 'this is my updates so far'

# push changes into the remote VSTS Git repo master branch.
git push origin master
```

Nach Schritt 5 können Sie jetzt das Projekt sicher mit einer früheren Momentaufnahme wiederherstellen, mit dem Wissen, dass Sie immer zum Commit zurückkehren können, den Sie auf dem Masterbranch ausgeführt haben.

## <a name="authentication"></a>Authentifizierung
Wenn Sie nur die Funktionen für den Ausführungsverlauf in Azure ML benötigen, um Projektmomentaufnahmen zu erstellen und wiederherzustellen, müssen Sie sich nicht um die Git-Repository-Authentifizierung kümmern. Dies wird von der Experimentieren-Dienstebene erledigt.

Wenn Sie eigene Git-Tools verwenden, um die Versionskontrolle zu verwalten, müssen Sie die Authentifizierung für das Remote-Git-Repository auf VSTS ordnungsgemäß verarbeiten. D.h., Sie müssen die Authentifizierung mit dem Git-Repository auf dem lokalen Computer einrichten, bevor Sie Git-Befehle für dieses Remote-Git-Repository ausgeben können. 

Die einfachste Möglichkeit hierzu ist die Erstellung eines SSH-Schlüsselpaars und das Hochladen des öffentlichen Schlüssels in die Sicherheitseinstellungen des Git-Repositorys.

### <a name="generate-ssh-key"></a>Generieren eines SSH-Schlüssels 
Zuerst generieren wir ein SSH-Schlüsselpaar auf Ihrem Computer.

#### <a name="on-windows"></a>Unter Windows:
Starten Sie die Git-GUI-Desktopanwendung für Windows, und klicken Sie im _Hilfe_-Menü auf _SSH-Schlüssel anzeigen_.

![SSH-Schlüssel](media/using-git-ml-project/git_gui.png)

Kopieren Sie SSH in die Zwischenablage.

#### <a name="on-macos"></a>Unter MacOS:
Kurzanleitung aus der Befehlsshell:
```
# generate the SSH key
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# start the SSH agent in the background
$ eval "$(ssh-agent -s)"

# add newly generated SSH key to the SSH agent
$ ssh-add -K ~/.ssh/id_rsa

# display the public key so you can copy it.
$ more ~/.ssh/id_rsa.pub
```
Weitere Schritte finden Sie [in diesem GitHub-Artikel](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).

### <a name="upload-public-key-to-git-repo"></a>Hochladen von öffentlichen Schlüsseln in Git-Repository
Navigieren Sie zur Startseite Ihres Visual Studio-Kontos: https://<vsts_account_name>.visualstudio.com, und melden Sie sich an. Klicken Sie dann unter Ihrem Avatar auf Sicherheit.

Fügen Sie einen öffentlichen SSH-Schlüssel hinzu, kopieren Sie den öffentlichen SSH-Schlüssel aus dem vorherigen Schritt, und geben Sie ihm einen Namen. Sie können hier mehrere Schlüssel hinzufügen.

Jetzt können Sie Git-Befehle lokal für das Remove-Repository ausgeben, und es ist keine weitere explizite Authentifizierung erforderlich.

### <a name="read-more"></a>Weitere Informationen
Befolgen Sie diese beiden Artikel (beide Ansätze können umgesetzt werden) für weitere Details zur Aktivierung von lokaler Authentifizierung des Remote-Git-Repository in VSTS.
- [Use SSH Key Authentication (Verwenden von SSH-Schlüsselauthentifizierung)](https://www.visualstudio.com/en-us/docs/git/use-ssh-keys-to-authenticate)
- [Use Git Credential Managers (Verwenden von Anmeldeinformationen für Git-Manager)](https://www.visualstudio.com/en-us/docs/git/set-up-credential-managers)


## <a name="next-steps"></a>Nächste Schritte
Informationen zum Verwenden des Team Data Science-Prozesses zum Organisieren der Projektstruktur finden Sie unter [Structure a project with TDSP (Strukturieren eines Projekts mit TDSP)](how-to-use-tdsp-in-azure-ml.md).
