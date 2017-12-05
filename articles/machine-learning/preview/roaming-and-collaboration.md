---
title: Roaming und Zusammenarbeit in Azure Machine Learning Workbench | Microsoft-Dokumentation
description: "Liste der bekannten Probleme und eine Anleitung für die Problembehandlung"
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 50f48fb096cb907e050769a8a4159689eb25418c
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2017
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming und Zusammenarbeit in Azure Machine Learning Workbench
Dieses Dokument behandelt exemplarisch, wie Azure Machine Learning Workbench Ihnen helfen kann, auf unterschiedlichen Computern auf Ihre Projekte zuzugreifen, und die Zusammenarbeit mit Ihren Teamkollegen ermöglicht. 

Wenn Sie ein Azure Machine Learning-Projekt mit einem Link zu einem Git-Remoterepository erstellen, werden die Metadaten und Momentaufnahmen des Projekts in der Cloud gespeichert. Der Cloudlink ermöglicht Ihnen, auf einem anderen Computer auf das Projekt zuzugreifen (was „Roaming“ genannt wird). Außerdem können Sie Ihren Kollegen Zugriff gewähren und so die Zusammenarbeit ermöglichen. 

## <a name="prerequisites"></a>Voraussetzungen
Installieren Sie zuerst die Azure Machine Learning Workbench mit einem Zugriff auf ein Konto für Experimente. Weitere Details finden Sie in der [Installationsanweisung](quickstart-installation.md).

Greifen Sie zweitens auf [Visual Studio Team System](https://www.visualstudio.com) zu, und erstellen Sie ein Repository, mit dem Ihr Projekt verknüpft wird. Ausführliche Informationen zu Git finden Sie im Artikel [Verwenden von Git-Repository mit einem Azure Machine Learning Workbench-Projekt](using-git-ml-project.md).

## <a name="create-a-new-azure-machine-learning-project"></a>Erstellen eines neuen Azure Machine Learning-Projekts
Starten Sie Azure Machine Learning-Workbench, und erstellen Sie ein neues Projekt (z.B. _iris_). Geben Sie in das Textfeld **Git-Repository-URL** eine gültige VSTS-Git-Repository-URL ein. 

> [!IMPORTANT]
> Wenn Sie die leere Projektvorlage auswählen, ist das in Ordnung, wenn das von Ihnen ausgewählte Git-Repository bereits über einen _Masterbranch_ verfügt. Azure ML klont den _Masterbranch_ einfach lokal und fügt den Ordner `aml_config` und andere Metadatendateien des Projekts zum lokalen Projektordner hinzu. Wenn Sie jedoch eine andere Projektvorlage auswählen, darf Ihr Git-Repository nicht bereits über einen _Masterbranch_ verfügen, da Sie ansonsten eine Fehlermeldung erhalten. Alternativ können Sie das Befehlszeilentool `az ml project create` verwenden, um das Projekt zu erstellen und den Switch `--force` bereitzustellen. Dadurch werden die Dateien auf dem ursprünglichen Masterbranch gelöscht und durch die neuen Dateien aus der ausgewählten Vorlage ersetzt.

Führen Sie nach der Erstellung des Projekts einige Skripts darin aus. Diese Aktion committet den Projektzustand im Branch „Ausführungsverlauf“ des Git-Remoterepositorys. 

> [!NOTE] 
> Nur Skriptausführungen lösen Commits für den Branch „Ausführungsverlauf“ aus. Datenvorbereitungen oder Notebook-Ausführungen lösen keine Projektmomentaufnahmen für den Branch „Ausführungsverlauf“ aus.

Wenn Sie die Git-Authentifizierung eingerichtet haben, können Sie auch explizit im Masterbranch arbeiten oder einen neuen Branch erstellen. 

Beispiel: 
```
# check current repo status
$ git status

# stage all changes in the current repo
$ git add -A

# commit changes
$ git commit -m "my commit fixes this weird bug!"

# push to remote repo.
$ git push origin master
```

## <a name="roaming"></a>Roaming
<a name="roaming"></a>

### <a name="open-azure-machine-learning-workbench-on-second-machine"></a>Öffnen von Azure Machine Learning Workbench auf einem zweiten Computer
Sobald das VSTS-Git-Repository mit Ihrem Projekt verknüpft ist, können Sie von jedem Computer aus, auf dem Azure Machine Learning Workbench ist, auf das Projekt _iris_ zugreifen. 

Um auf das Projekt „iris“ von einem anderen Computer aus zugreifen zu können, müssen Sie sich bei der App mit den gleichen Anmeldeinformationen wie beim Erstellen des Projekts anmelden. Darüber hinaus müssen Sie zum selben Konto für Experimente und Arbeitsbereich navigieren. Das Projekt _iris_ ist alphabetisch mit anderen Projekten innerhalb des Arbeitsbereichs aufgelistet. 

### <a name="download-project-on-second-machine"></a>Herunterladen des Projekts auf den zweiten Computer
Wenn der Arbeitsbereich auf dem zweiten Computer geöffnet wird, unterscheidet sich das Symbol neben dem Projekt _iris_ vom typischen Ordnersymbol. Das Downloadsymbol zeigt an, dass sich der Inhalt des Projekts in der Cloud befindet und auf den aktuellen Computer heruntergeladen werden muss. 

![Erstellen des Projekts](./media/roaming-and-collaboration/downloadable-project.png)

Durch Klicken auf das Projekt _iris_ wird eine Downloadaktion gestartet. Kurze Zeit nach Abschluss des Downloads kann das Projekt auf dem zweiten Computer aufgerufen werden. 

Unter Windows befindet es sich unter `C:\Users\<username>\Documents\AzureML`.

Bei macOS befindet es sich hier: `/home/<username>/Documents/AzureML`

In einer künftigen Version werden wir die Funktionalität so erweitern, dass Sie einen Zielordner auswählen können. 

> [!NOTE]
> Der Download misslingt, wenn es im Azure ML-Verzeichnis einen Ordner gibt, der genau den gleichen Namen wie das Projekt hat. Um dieses Problem zu umgehen, müssen Sie einstweilen den bestehenden Ordner umbenennen.


### <a name="work-on-the-downloaded-project"></a>Arbeiten im heruntergeladenen Projekt 
Das neu heruntergeladene Projekt spiegelt den Projektstand zum Zeitpunkt der letzten Ausführung im Projekt wider. Eine Momentaufnahme des Projektstands wird automatisch im Branch „Ausführungsverlauf“ im VSTS-Git-Repository committet, wenn Sie eine Ausführung übermitteln. Wir verwenden die Momentaufnahme der letzten Ausführung, wenn wir das Projekt auf dem zweiten Computer instanziieren. 
 

## <a name="collaboration"></a>Zusammenarbeit
Sie können mit Ihren Teamkollegen an Projekten zusammenarbeiten, die mit einem VSTS-Git-Repository verbunden sind. Sie können Benutzern Berechtigungen für das Konto für Experimente, den Arbeitsbereich und das Projekt zuweisen. Derzeit können Sie die Azure Resource Manager-Befehle mithilfe der Azure CLI ausführen. Sie können auch das [Azure-Portal](https://portal.azure.com) verwenden. Siehe den folgenden [Abschnitt](#portal).    

### <a name="using-command-line-to-add-users"></a>Hinzufügen von Benutzern über die Befehlszeile
Sehen wir uns ein Beispiel an. Angenommen, Alice ist die Besitzerin des Projekts „iris“ und möchte den Zugriff für Bob freigeben. 

Alice klickt auf das Menü **Datei** und wählt den Menüpunkt **Eingabeaufforderung** aus, um die für das Projekt _iris_ konfigurierte Eingabeaufforderung zu starten. Alice kann dann entscheiden, welche Zugriffsstufe sie Bob gewähren möchte, indem sie die folgenden Befehle ausführt.  

```azurecli
# Find ARM ID of the experimnetation account
az ml account experimentation show --query "id"

# Add Bob to the Experimentation Account as a Contributor.
# Bob now has read/write access to all workspaces and projects under the Account by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <experimentation account ARM ID>

# Find ARM ID of the workspace
az ml workspace show --query "id"

# Add Bob to the workspace as an Owner.
# Bob now has read/write access to all projects under the Workspace by inheritance. And he can invite or remove others.
az role assignment create --assignee bob@contoso.com --role Owner --scope <workspace ARM ID>
```

Nach der Rollenzuweisung, direkt oder durch Vererbung, kann Bob das Projekt in der Workbench-Projektliste sehen. Die Anwendung muss möglicherweise neu gestartet werden, damit das Projekt angezeigt wird. Bob kann dann das Projekt, wie im Abschnitt [Roaming](#roaming) beschrieben, herunterladen und mit Alice zusammenarbeiten. 

Der Ausführungsverlauf für alle an einem Projekt beteiligten Benutzer wird im selben Git-Remoterepository committet. Wenn Alice nun eine Ausführung startet, kann Bob die Ausführung im Abschnitt „Ausführungsverlauf“ des Projekts in der Workbench-App sehen. Bob kann das Projekt auch mit dem Status einer beliebigen Ausführung wiederherstellen, einschließlich der von Alice gestarteten Ausführungen. 

Durch die gemeinsame Nutzung eines Git-Remoterepositorys für das Projekt können Alice und Bob auch im Masterbranch zusammenarbeiten. Bei Bedarf können sie auch persönliche Branches erstellen und für die Zusammenarbeit Pull Requests und Merges von Git verwenden. 

### <a name="using-azure-portal-to-add-users"></a>Hinzufügen von Benutzern im Azure-Portal
<a name="portal"></a>

Konten für Azure Machine Learning-Experimentieren, Arbeitsbereiche und Projekte sind Azure Resource Manager-Ressourcen. Über den Link „Zugriffssteuerung“ im [Azure-Portal](https://portal.azure.com) können Sie Rollen zuweisen. 

In der Ansicht „Alle Ressourcen“ finden Sie die Ressourcen zum Hinzufügen von Benutzern. Klicken Sie auf der Seite auf den Link „Zugriffssteuerung (IAM)“. Hinzufügen von Benutzern 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Beispiel eines Workflows für die Zusammenarbeit
Zur Veranschaulichung des Ablaufs bei der Zusammenarbeit wird hier ein Beispiel durchlaufen. Die Contoso-Mitarbeiter Alice und Bob möchten mithilfe von Azure ML Workbench an einem Data Science-Projekt zusammenarbeiten. Ihre Identitäten gehören demselben Contoso Azure AD-Mandanten an.

1. Alice erstellt zunächst ein leeres Git-Repository in einem VSTS-Projekt. Dieses VSTS-Projekt sollte sich in einem unter dem Contoso AAD-Mandanten erstellten Azure-Abonnement befinden. 

2. Alice erstellt dann ein Azure ML-Konto für Experimente, einen Arbeitsbereich und ein Azure ML Workbench-Projekt auf ihrem Computer. Sie stellt die Git-Repository-URL beim Erstellen des Projekts bereit.

3. Alice beginnt mit der Arbeit an dem Projekt. Sie erstellt einige Skripts und nimmt einige Ausführungen vor. Bei jeder Ausführung wird automatisch eine Momentaufnahme des gesamten Projektordners in einen Branch für den Ausführungsverlauf des VSTS-Git-Repositorys übertragen, das von Workbench als ein Commit erstellt wurde.

4. Alice ist jetzt mit der vorgenommenen Arbeit zufrieden. Sie möchte ihre Änderung im lokalen _Masterbranch_ committen und überträgt sie in den _Masterbranch_ des VSTS-Git-Repositorys. Dazu startet sie bei geöffnetem Projekt das Eingabeaufforderungsfenster von Azure ML Workbench und gibt die folgenden Befehle aus:
    
    ```sh
    # verify the Git remote is pointing to the VSTS Git repo
    $ git remote -v

    # verify that the current branch is master
    $ git branch

    # stage all changes
    $ git add -A

    # commit changes with a comment
    $ git commit -m "this is a good milestone"

    # push the commit to the master branch of the remote Git repo in VSTS
    $ git push
    ```

5. Dann fügt Alice Bob im Arbeitsbereich als einen Mitwirkenden hinzu. Dies ist über das Azure-Portal oder mithilfe des oben gezeigten Befehls `az role assignment` möglich. Sie gewährt Bob auch Lese-/Schreibzugriff für das VSTS-Git-Repository.

6. Bob meldet sich nun auf seinem Computer bei Azure ML Workbench an. Ihm wird der Arbeitsbereich angezeigt, den Alice für ihn freigegeben hat, und das Projekt, das unter diesem Arbeitsbereich aufgeführt ist. 

7. Bob klickt auf den Projektnamen, und das Projekt wird auf seinen Computer heruntergeladen.
    
    a. Die heruntergeladenen Projektdateien sind Klone der Momentaufnahme der neuesten Ausführung, die im Ausführungsverlauf aufgezeichnet wurde. Es handelt sich nicht um den letzten Commit auf dem Masterbranch.
    
    b. Der lokale Projektordner ist auf den _Masterbranch_ mit nicht bereitgestellten Änderungen festgelegt.

8. Bob kann jetzt von Alice vorgenommene Ausführungen durchsuchen und Momentaufnahmen aller vorherigen Ausführungen wiederherstellen.

9. Bob möchte die neuesten Änderungen abrufen, die von Alice übertragen wurden, und mit der Arbeit an einem anderen Branch beginnen. Dazu öffnet er ein Eingabeaufforderungsfenster von Azure ML Workbench und führt die folgenden Befehle aus:

    ```sh
    # verify the Git remote is pointing to the VSTS Git repo
    $ git remote -v

    # verify that the current branch is master
    $ git branch

    # get the latest commit in VSTS Git master branch and overwrite current files
    $ git pull --force

    # create a new local branch named "bob" so Bob's work is done on the "bob" branch
    $ git checkout -b bob
    ```

10. Nun ändert Bob das Projekt und sendet neue Ausführungen. Die Änderungen werden auf dem Branch _bob_ ausgeführt. Auch Bobs Ausführungen sind nun für Alice sichtbar.

11. Bob ist jetzt bereit, seine Änderungen in das Git-Remoterepository zu übertragen. Um Konflikte mit dem von Alice verwendeten _Masterbranch_ zu vermeiden, entscheidet er sich für das Übertragen seiner Arbeit in einen neuen Remotebranch, der ebenfalls den Namen _bob_ hat.

    ```sh
    # verify that the current branch is "bob" and it has unstaged changes
    $ git status
    
    # stage all changes
    $ git add -A

    # commit them with a comment
    $ git commit -m "I found a cool new trick."

    # create a new branch on the remote VSTS Git repo, and push changes
    $ git push origin bob
    ```

12. Dann kann Bob Alice von dem neuen coolen Trick in seinem Code erzählen, und er erstellt eine Pullanforderung für das Git-Remoterepository vom Branch _bob_ zum _Masterbranch_. Alice kann dann die Pullanforderung im _Masterbranch_ zusammenführen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung von Git mit Azure ML Workbench: [Verwenden von Git-Repository mit einem Azure Machine Learning Workbench-Projekt](using-git-ml-project.md)