---
title: Roaming und Kollaboration in Azure Machine Learning Workbench | Microsoft-Dokumentation
description: Erfahren Sie, wie Roaming und Kollaboration in Azure Machine Learning Workbench eingerichtet werden.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: b587f5dcc9558ec52b85e4b53dae0e31ad475a4e
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming und Zusammenarbeit in Azure Machine Learning Workbench
Dieser Artikel beschreibt, wie Sie mit Azure Machine Learning Workbench Projekte für Roaming zwischen Computern einrichten und mit Teammitgliedern zusammenarbeiten können. 

Wenn Sie ein Azure Machine Learning-Projekt mit einem Link zu einem Git-Remoterepository erstellen, werden die Metadaten und Momentaufnahmen des Projekts in der Cloud gespeichert. Der Cloudlink ermöglicht Ihnen, auf einem anderen Computer auf das Projekt zuzugreifen („Roaming“). Sie können auch mit Teammitgliedern zusammenarbeiten, indem Sie ihnen Zugriff auf das Projekt gewähren. 

## <a name="prerequisites"></a>Voraussetzungen
1. Installieren Sie die Machine Learning Workbench-App. Stellen Sie sicher, das Sie Zugriff auf ein Azure Machine Learning-Experimentieren-Konto haben. Weitere Informationen finden Sie im [Installationshandbuch](../service/quickstart-installation.md).

2. Greifen Sie auf [Visual Studio Team Services](https://www.visualstudio.com) (Team Services) zu, und erstellen Sie dann ein Repository, mit dem Ihr Projekt verknüpft werden soll. Weitere Informationen finden Sie unter [Verwenden eines Git-Repositorys mit einem Machine Learning Workbench-Projekt](using-git-ml-project.md).

## <a name="create-a-new-machine-learning-project"></a>Erstellen von Machine Learning-Projekten
Öffnen Sie Azure Machine Learning-Workbench, und erstellen Sie ein neues Projekt (z.B. ein Projekt namens „iris“). Geben Sie im Feld **Visualstudio.com GIT Repository URL** eine gültige URL für ein Team Services-Git-Repository ein. 

> [!IMPORTANT]
> Wenn Sie die leere Projektvorlage auswählen, verfügt das ausgewählte Git-Repository möglicherweise bereits über einen Masterbranch. Machine Learning klont den Masterbranch einfach lokal. Dabei werden dem lokalen Projektordner der Ordner „aml_config“ und andere Projekt-Metadatendateien hinzugefügt. 
>
> Wenn Sie eine andere Projektvorlage auswählen, darf das Git-Repository *nicht* bereits über einen Masterbranch verfügen. Andernfalls wird eine Fehlermeldung angezeigt. Alternativ können Sie den Befehl `az ml project create` mit dem Schalter `--force` verwenden, um das Projekt zu erstellen. Dadurch werden die Dateien im ursprünglichen Masterbranch gelöscht und durch die neuen Dateien aus der ausgewählten Vorlage ersetzt.

Führen Sie nach dem Erstellen des Projekts einige Skripts darin aus. Diese Aktion committet den Projektzustand im Branch „Ausführungsverlauf“ des Git-Repositorys. 

> [!NOTE] 
> Nur Skriptausführungen lösen Commits für den Branch „Ausführungsverlauf“ aus. Datenvorbereitungen und Notebookausführungen lösen keine Projektmomentaufnahmen für den Branch „Ausführungsverlauf“ aus.

Wenn Sie die Git-Authentifizierung eingerichtet haben, können Sie auch im Masterbranch arbeiten. Sie können stattdessen aber auch einen neuen Branch erstellen. 

Beispiel: 
```
# Check current repo status.
$ git status

# Stage all changes in the current repo.
$ git add -A

# Commit changes.
$ git commit -m "my commit fixes this weird bug!"

# Push to the remote repo.
$ git push origin master
```

## <a name="roaming"></a>Roaming
<a name="roaming"></a>

### <a name="open-machine-learning-workbench-on-a-second-computer"></a>Öffnen von Machine Learning Workbench auf einem zweiten Computer
Nachdem das Team Services-Git-Repository mit Ihrem Projekt verknüpft ist, können Sie von jedem Computer, auf dem Azure Machine Learning Workbench installiert wurde, auf das Projekt „iris“ zugreifen. 

Um auf einem anderen Computer auf das Projekt „iris“ zugreifen zu können, müssen Sie sich bei der App mit denselben Anmeldeinformationen wie beim Erstellen des Projekts anmelden. Sie müssen auch beim selben Machine Learning Experimentieren-Konto und -Arbeitsbereich angemeldet sein. Das Projekt „iris“ wird alphabetisch mit den anderen Projekten im Arbeitsbereich aufgelistet. 

### <a name="download-the-project-on-a-second-computer"></a>Herunterladen des Projekts auf einen zweiten Computer
Wenn der Arbeitsbereich auf dem zweiten Computer geöffnet ist, unterscheidet sich das Symbol neben dem Projekt „iris“ vom typischen Ordnersymbol. Das Downloadsymbol zeigt an, dass sich der Inhalt des Projekts in der Cloud befindet und auf den aktuellen Computer heruntergeladen werden kann. 

![Projekt erstellen](./media/roaming-and-collaboration/downloadable-project.png)

Wählen Sie das Projekt „iris“ aus, um einen Download zu starten. Nach Abschluss des Downloads kann das Projekt auf dem zweiten Computer aufgerufen werden. 

Unter Windows befindet sich das Projekt unter „C:\Users\\<Benutzername\>\Documents\AzureML“.

Unter macOS befindet sich das Projekt unter „/home/\<Benutzername\>/Documents/AzureML“.

Für eine zukünftige Version ist geplant, die Funktionalität zu erweitern, sodass Sie einen Zielordner auswählen können. 

> [!NOTE]
> Wenn es im Machine Learning-Verzeichnis einen Ordner gibt, der genau den gleichen Namen wie das Projekt hat, misslingt der Download. Um dieses Problem zu umgehen, benennen Sie den vorhandenen Ordner vorübergehend um.


### <a name="work-on-the-downloaded-project"></a>Arbeiten im heruntergeladenen Projekt 
Das neu heruntergeladene Projekt spiegelt den Projektstand zum Zeitpunkt der letzten Ausführung im Projekt wider. Eine Momentaufnahme des Projektstands wird automatisch im Branch „Ausführungsverlauf“ im Team Services-Git-Repository committet, wenn Sie eine Ausführung übermitteln. Die mit der letzten Ausführung verknüpfte Momentaufnahme wird zum Instanziieren des Projekts auf dem zweiten Computer verwendet. 
 

## <a name="collaboration"></a>Zusammenarbeit
Sie können mit Teammitgliedern an Projekten zusammenarbeiten, die mit einem Team Services-Git-Repository verknüpft sind. Sie können Benutzern Berechtigungen für das Machine Learning-Experimentieren-Konto, den Arbeitsbereich und das Projekt zuweisen. Derzeit können Sie Azure Resource Manager-Befehle über die Azure-Befehlszeilenschnittstelle ausführen. Sie können auch das [Azure-Portal](https://portal.azure.com) verwenden. Weitere Informationen finden Sie unter [Verwenden des Azure-Portals zum Hinzufügen von Benutzern](#portal).    

### <a name="use-the-command-line-to-add-users"></a>Hinzufügen von Benutzern über die Befehlszeile
Beispiel: Alice ist die Besitzerin des Projekts „iris“. Alice möchte den Zugriff auf das Projekt für Bob freigeben. 

Alice wählt das Menü **Datei** und dann die Menüoption **Eingabeaufforderung** aus. Das Projekt „iris“ wird im Eingabeaufforderungsfenster geöffnet. Alice kann dann entscheiden, welche Zugriffsebene sie Bob erteilen möchte. Sie gewährt Berechtigungen, indem sie die folgenden Befehle ausführt:  

```azurecli
# Find the Resource Manager ID of the Experimentation account.
az ml account experimentation show --query "id"

# Add Bob to the Experimentation account as a Contributor.
# Bob now has read/write access to all workspaces and projects under the account by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <Experimentation account Resource Manager ID>

# Find the Resource Manager ID of the workspace.
az ml workspace show --query "id"

# Add Bob to the workspace as an Owner.
# Bob now has read/write access to all projects under the workspace by inheritance. Bob can invite or remove other users.
az role assignment create --assignee bob@contoso.com --role Owner --scope <workspace Resource Manager ID>
```

Nach der Rollenzuweisung (direkt oder durch Vererbung) kann Bob das Projekt in der Machine Learning Workbench-Projektliste sehen. Bob muss die Anwendung möglicherweise neu starten, damit das Projekt angezeigt wird. Bob kann dann das Projekt wie im Abschnitt [Roaming](#roaming) beschrieben herunterladen und mit Alice zusammenarbeiten. 

Der Ausführungsverlauf für alle an einem Projekt beteiligten Benutzer wird im selben Git-Remoterepository committet. Wenn Alice nun eine Ausführung startet, kann Bob die Ausführung im Abschnitt „Ausführungsverlauf“ des Projekts in der Machine Learning Workbench-App sehen. Bob kann das Projekt auch mit dem Status einer beliebigen Ausführung wiederherstellen – einschließlich der von Alice gestarteten Ausführungen. 

Durch Freigeben eines Git-Remoterepositorys für das Projekt können Alice und Bob auch im Masterbranch zusammenarbeiten. Bei Bedarf können sie auch persönliche Branches erstellen und für die Zusammenarbeit Pull-Anforderungen und Merges von Git verwenden. 

### <a name="use-the-azure-portal-to-add-users"></a>Hinzufügen von Benutzern im Azure-Portal
<a name="portal"></a>

Machine Learning-Experimentieren-Konten, -Arbeitsbereiche und -Projekte sind Azure Resource Manager-Ressourcen. Über den Link **Zugriffssteuerung** im [Azure-Portal](https://portal.azure.com) können Sie Rollen zuweisen. 

Sie finden die Ressource, der Sie Benutzer hinzufügen möchten, in der Ansicht **Alle Ressourcen**. Wählen Sie den Link **Zugriffssteuerung (IAM)** und dann **Benutzer hinzufügen** aus. 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Beispiel eines Workflows für die Zusammenarbeit
Zur Veranschaulichung des Kollaborationsworkflows wird hier ein Beispiel durchlaufen. Die Contoso-Mitarbeiter Alice und Bob möchten mithilfe von Machine Learning Workbench an einem Data Science-Projekt zusammenarbeiten. Ihre Identitäten gehören demselben Contoso Azure Active Directory-Mandanten (Azure AD) an. Alice und Bob führen die folgenden Schritte aus:

1. Alice erstellt ein leeres Git-Repository in einem Team Services-Projekt. Das Team Services-Projekt muss sich in einem Azure-Abonnement befinden, das unter dem Contoso Azure AD-Mandanten erstellt wurde. 

2. Alice erstellt dann ein Machine Learning-Experimentieren-Konto, einen Arbeitsbereich und ein Machine Learning Workbench-Projekt auf ihrem Computer. Beim Erstellen des Projekts gibt sie die Team Services-Git-Repository-URL an.

3. Alice beginnt mit der Arbeit an dem Projekt. Sie erstellt einige Skripts und nimmt einige Ausführungen vor. Bei jeder Ausführung wird automatisch eine Momentaufnahme des gesamten Projektordners als Commit an einen Branch „Ausführungsverlauf“ im Team Services-Git-Repository übertragen, der von Machine Learning Workbench erstellt wird.

4. Alice ist mit der vorgenommenen Arbeit zufrieden. Sie möchte ihre Änderung im lokalen Masterbranch committen und dann per Push in den Masterbranch des Team Services-Git-Repositorys übertragen. Während das Projekt in Machine Learning-Workbench geöffnet ist, öffnet Sie das Eingabeaufforderungsfenster und gibt diese Befehle ein:
    
    ```sh
    # Verify that the Git remote is pointing to the Team Services Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Stage all changes.
    $ git add -A

    # Commit changes with a comment.
    $ git commit -m "this is a good milestone"

    # Push the commit to the master branch of the remote Git repo in Team Services.
    $ git push
    ```

5. Alice fügt Bob dem Arbeitsbereich als einen Mitwirkenden hinzu. Dies erreicht sie im Azure-Portal oder mithilfe des Befehls `az role assignment`, wie weiter oben erläutert. Alice gewährt Bob außerdem Berechtigungen für den Lese-/Schreibzugriff auf das Team Services-Git-Repository.

6. Bob meldet sich auf seinem Computer bei Machine Learning Workbench an. Er kann den Arbeitsbereich sehen, den Alice für ihn freigegeben hat. Er kann das Projekt „iris“ sehen, das unter diesem Arbeitsbereich aufgeführt wird. 

7. Bob wählt den Projektnamen aus. Das Projekt wird auf seinen Computer heruntergeladen.
    * Die heruntergeladenen Projektdateien sind eine Kopie der Momentaufnahme der neuesten Ausführung, die im Ausführungsverlauf aufgezeichnet wurde. Es handelt sich nicht um den letzten Commit auf dem Masterbranch.
    * Der lokale Projektordner ist auf den Masterbranch mit den nicht bereitgestellten Änderungen festgelegt.

8. Bob kann Ausführungen von Alice durchsuchen. Er kann Momentaufnahmen aller früheren Ausführungen wiederherstellen.

9. Bob möchte die neuesten von Alice gepushten Änderungen erhalten und dann in einem anderen Branch arbeiten. Dazu öffnet er in Machine Learning Workbench ein Eingabeaufforderungsfenster und führt die folgenden Befehle aus:

    ```sh
    # Verify that the Git remote is pointing to the Team Services Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Get the latest commit in the Team Services Git master branch and overwrite current files.
    $ git pull --force

    # Create a new local branch named "bob" so that Bob's work is done in the "bob" branch
    $ git checkout -b bob
    ```

10. Bob ändert das Projekt und sendet neue Ausführungen. Die Änderungen werden im Branch „bob“ vorgenommen. Bobs Ausführungen werden für Alice ebenfalls sichtbar.

11. Bob kann seine Änderungen in das Git-Remoterepository übertragen. Um Konflikte mit dem von Alice verwendeten Masterbranch zu vermeiden, pusht Bob seine Arbeit in einen neuen Remotebranch, der ebenfalls den Namen „bob“ hat.

    ```sh
    # Verify that the current branch is "bob," and that it has unstaged changes.
    $ git status
    
    # Stage all changes.
    $ git add -A

    # Commit the changes with a comment.
    $ git commit -m "I found a cool new trick."

    # Create a new branch on the remote Team Services Git repo, and then push the changes.
    $ git push origin bob
    ```

12. Um Alice von dem neuen Trick in seinem Code zu erzählen, erstellt Bob eine Pullanforderung für das Git-Remoterepository vom Branch „bob“ zum Masterbranch. Alice kann dann die Pullanforderung im Masterbranch zusammenführen.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zum [Verwenden eines Git-Repositorys mit einem Machine Learning Workbench-Projekt](using-git-ml-project.md).
