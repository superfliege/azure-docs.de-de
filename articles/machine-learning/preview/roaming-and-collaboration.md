---
title: Roaming und Zusammenarbeit in Azure Machine Learning Workbench | Microsoft-Dokumentation
description: "Liste der bekannten Probleme und eine Anleitung für die Problembehandlung"
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/05/2017
ms.openlocfilehash: 156dd1b7f928df22b3feb9e7a13396d3b53a91d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming und Zusammenarbeit in Azure Machine Learning Workbench
Dieses Dokument behandelt exemplarisch, wie Azure Machine Learning Workbench Ihnen helfen kann, auf unterschiedlichen Computern auf Ihre Projekte zuzugreifen, und die Zusammenarbeit mit Ihren Teamkollegen ermöglicht. 

Wenn Sie ein Azure Machine Learning-Projekt mit einem Link zu einem Git-Remoterepository erstellen, werden die Metadaten und Momentaufnahmen des Projekts in der Cloud gespeichert. Der Cloudlink ermöglicht Ihnen, auf einem anderen Computer auf das Projekt zuzugreifen (was „Roaming“ genannt wird). Außerdem können Sie Ihren Kollegen Zugriff gewähren und so die Zusammenarbeit ermöglichen. 

## <a name="prerequisites"></a>Voraussetzungen
Installieren Sie zuerst die Azure Machine Learning Workbench mit einem Zugriff auf ein Konto für Experimente. Weitere Details finden Sie in der [Installationsanweisung](quickstart-installation.md).

Greifen Sie zweitens auf [Visual Studio Team System](https://www.visualstudio.com) zu, und erstellen Sie ein Repository, mit dem Ihr Projekt verknüpft wird. Ausführliche Informationen zu Git finden Sie im Artikel [Verwenden von Git-Repository mit einem Azure Machine Learning Workbench-Projekt](using-git-ml-project.md).

## <a name="create-a-new-azure-machine-learning-project"></a>Erstellen eines neuen Azure Machine Learning-Projekts
Starten Sie Azure Machine Learning-Workbench, und erstellen Sie ein neues Projekt (z.B. _iris_). Geben Sie in das Textfeld **Git-Repository-URL** eine gültige VSTS-Git-Repository-URL ein. 
>[!IMPORTANT]
>Die Projekterstellung misslingt, wenn Sie keinen Lese-/Schreibzugriff auf das Git-Repository haben und das Git-Repository nicht leer ist, d. h. es bereits einen Masterbranch hat.

Führen Sie nach der Erstellung des Projekts einige Skripts darin aus. Diese Aktion commitet den Projektzustand im Branch „Ausführungsverlauf“ des Git-Repositorys. 

Wenn Sie die-Git Authentifizierung eingerichtet haben, können Sie auch explizit im Masterbranch arbeiten oder einen neuen Branch erstellen. 

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

>Beachten Sie, dass der Download misslingt, wenn es im Azure ML-Verzeichnis einen Ordner gibt, der genau den gleichen Namen wie das Projekt hat. Um dieses Problem zu umgehen, müssen Sie einstweilen den bestehenden Ordner umbenennen.


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

# Add Bob to the Experimentation Account as a Reader.
# Bob now has read access to all workspaces and projects under the Account by inheritance.
az role assignment create --assignee bob@contoso.com --role Reader --scope <experimentation account ARM ID>

# Find ARM ID of the workspace
az ml workspace show --query "id"

# Add Bob to the workspace as a Contributor.
# Bob now has read/write access to all projects under the Workspace by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <workspace ARM ID>

# find ARM ID of the project 
az ml project show --query "id"

# Add Bob to the Project as an Owner.
# Bob now has read/write access to the Project, and can add others too.
az role assignment create --assignee bob@contoso.com --role Owner --scope <project ARM ID>
```

Nach der Rollenzuweisung, direkt oder durch Vererbung, kann Bob das Projekt in der Workbench-Projektliste sehen. Die Anwendung muss möglicherweise neu gestartet werden, damit das Projekt angezeigt wird. Bob kann dann das Projekt, wie im Abschnitt [Roaming](#roaming) beschrieben, herunterladen und mit Alice zusammenarbeiten. 

Der Ausführungsverlauf für alle an einem Projekt beteiligten Benutzer wird im selben Git-Remoterepository committet. Wenn Alice nun eine Ausführung startet, kann Bob die Ausführung im Abschnitt „Ausführungsverlauf“ des Projekts in der Workbench-App sehen. Bob kann das Projekt auch mit dem Status einer beliebigen Ausführung wiederherstellen, einschließlich der von Alice gestarteten Ausführungen. 

Durch die gemeinsame Nutzung eines Git-Remoterepositorys für das Projekt können Alice und Bob auch im Masterbranch zusammenarbeiten. Bei Bedarf können sie auch persönliche Branches erstellen und für die Zusammenarbeit Pull Requests und Merges von Git verwenden. 

### <a name="using-azure-portal-to-add-users"></a>Hinzufügen von Benutzern im Azure-Portal
<a name="portal"></a>

Konten für Azure Machine Learning-Experimentieren, Arbeitsbereiche und Projekte sind Azure Resource Manager-Ressourcen. Über den Link „Zugriffssteuerung“ im [Azure-Portal](https://portal.azure.com) können Sie Rollen zuweisen. 

In der Ansicht „Alle Ressourcen“ finden Sie die Ressourcen zum Hinzufügen von Benutzern. Klicken Sie auf der Seite auf den Link „Zugriffssteuerung (IAM)“. Hinzufügen von Benutzern 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

