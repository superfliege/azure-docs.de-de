---
title: Visuelles Erstellen in Azure Data Factory | Microsoft-Dokumentation
description: Informationen zum Verwenden der visuellen Erstellung in Azure Data Factory
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/01/2018
ms.author: shlo
ms.openlocfilehash: 655a6ab2960047cde50bec2953015283ca8577f0
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214851"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Visuelles Erstellen in Azure Data Factory
Mithilfe der Azure Data Factory-Benutzeroberfläche (UX) können Sie Ressourcen für Ihre Data Factory visuell erstellen und bereitstellen, ohne Code schreiben zu müssen. Sie können Aktivitäten auf eine Pipelinecanvas ziehen, Testläufe ausführen, iterativ debuggen sowie Ihre Pipelineausführungen bereitstellen und überwachen. Die Benutzeroberfläche kann auf zwei Arten zum visuellen Erstellen verwendet werden:

- Direktes Erstellen mit dem Data Factory-Dienst
- Erstellen mit VSTS (Visual Studio Team Services) Git-Integration für Zusammenarbeit, Quellcodeverwaltung oder Versionsverwaltung

## <a name="author-directly-with-the-data-factory-service"></a>Direktes Erstellen mit dem Data Factory-Dienst
Das visuelle Erstellen mit dem Data Factory-Dienst unterscheidet sich in zwei Punkten vom visuellen Erstellen mit VSTS:

- Der Data Factory-Dienst umfasst kein Repository zum Speichern der JSON-Entitäten für Ihre Änderungen.
- Der Data Factory-Dienst ist nicht für Zusammenarbeit oder Versionskontrolle optimiert.

![Konfigurieren des Data Factory-Diensts ](media/author-visually/configure-data-factory.png)

Wenn Sie die **UX-Canvas** für die Erstellung zum direkten Erstellen mit dem Data Factory-Dienst verwenden, ist nur der Modus **Alle veröffentlichen** verfügbar. Alle vorgenommenen Änderungen werden direkt im Data Factory-Dienst veröffentlicht.

![Modus „Veröffentlichen“](media/author-visually/data-factory-publish.png)

## <a name="author-with-vsts-git-integration"></a>Erstellen mit VSTS Git-Integration
Das visuelle Erstellen mit VSTS Git-Integration unterstützt Quellcodeverwaltung und Zusammenarbeit beim Arbeiten an den Data Factory-Pipelines. Sie können eine Data Factory einem VSTS Git-Kontorepository für die Quellcodeverwaltung, Zusammenarbeit, Versionsverwaltung usw. zuordnen. Ein einzelnes VSTS Git-Konto kann über mehrere Repositorys verfügen. Allerdings kann ein VSTS Git-Repository nur einer einzigen Data Factory zugeordnet werden. Wenn Sie noch nicht über ein VSTS-Konto oder ein Repository verfügen, folgen Sie [diesen Anweisungen](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student) zum Erstellen der Ressourcen.

> [!NOTE]
> Sie können Skript- und Datendateien in einem VSTS-Git-Repository speichern. Allerdings müssen Sie die Dateien manuell in Azure Storage hochladen. Eine Data Factory-Pipeline lädt Skript- oder Datendateien, die in einem VSTS-Git-Repository gespeichert sind, nicht automatisch in Azure Storage hoch.

### <a name="configure-a-vsts-git-repository-with-azure-data-factory"></a>Konfigurieren eines VSTS Git-Repositorys mit Azure Data Factory
Es gibt zwei Methoden zum Konfigurieren eines VSTS Git-Repositorys mit einer Data Factory.

#### <a name="method1"></a> Konfigurationsmethode 1: Seite „Erste Schritte“

Wechseln Sie in Azure Data Factory zur Seite **Erste Schritte**. Wählen Sie **Coderepository konfigurieren** aus:

![Konfigurieren eines VSTS-Coderepositorys](media/author-visually/configure-repo.png)

Der Konfigurationsbereich **Repositoryeinstellungen** wird angezeigt:

![Konfigurieren der Repositoryeinstellungen](media/author-visually/repo-settings.png)

Im Bereich werden die folgenden Einstellungen für den VSTS-Coderepository angezeigt:

| Einstellung | BESCHREIBUNG | Wert |
|:--- |:--- |:--- |
| **Repositorytyp** | Der Typ des VSTS Coderepositorys.<br/>**Hinweis**: GitHub wird derzeit nicht unterstützt. | Visual Studio Team Services Git |
| **Azure Active Directory** | Ihr Name des Azure AD-Mandanten. | <your tenant name> |
| **Visual Studio Team Services-Konto** | Ihr VSTS-Kontoname. Den VSTS-Kontonamen finden Sie unter `https://{account name}.visualstudio.com`. Sie können sich [bei Ihrem VSTS-Konto anmelden](https://www.visualstudio.com/team-services/git/), um auf Ihr Visual Studio-Profil zuzugreifen und Ihre Repositorys und Projekte anzuzeigen. | <your account name> |
| **Projektname** | Ihr VSTS-Projektname. Den VSTS-Projektnamen finden Sie unter `https://{account name}.visualstudio.com/{project name}`. | <your VSTS project name> |
| **Repositoryname** | Der Name Ihres VSTS-Coderepositorys. VSTS-Projekte enthalten Git-Repositorys zum Verwalten Ihres Quellcodes, wenn Ihr Projekt größer wird. Sie können ein neues Repository erstellen oder ein vorhandenes Repository verwenden, das sich bereits in Ihrem Projekt befindet. | <your VSTS code repository name> |
| **Collaboration branch** (Kollaborationsbranch) | Ihr Branch für die VSTS-Kollaboration, der für die Veröffentlichung verwendet wird. Standardmäßig ist dies `master`. Ändern Sie diese Einstellung, falls Sie Ressourcen eines anderen Branchs veröffentlichen möchten. | <your collaboration branch name> |
| **Stammordner** | Ihr Stammordner im Branch für die VSTS-Kollaboration. | <your root folder name> |
| **Vorhandene Data Factory-Ressourcen in Repository importieren** | Gibt an, ob vorhandene Data Factory-Ressourcen aus der **UX-Canvas** für die Erstellung in ein VSTS Git-Repository importiert werden sollen. Aktivieren Sie das Kontrollkästchen, um Ihre Data Factory-Ressourcen in das zugehörige Git-Repository im JSON-Format zu importieren. Diese Aktion exportiert jede Ressource einzeln (d. h. die verknüpften Dienste und Datasets werden in separate JSONs exportiert). Ist dieses Kontrollkästchen nicht aktiviert, werden die vorhandenen Ressourcen nicht importiert. | Aktiviert (Standardeinstellung) |

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Konfigurationsmethode 2: UX-Canvas für die Erstellung
Suchen Sie in Azure Data Factory in der **UX-Canvas** für die Erstellung nach Ihrer Data Factory. Klicken Sie auf das Dropdownmenü **Data Factory**, und wählen Sie dann **Coderepository konfigurieren** aus.

Ein Konfigurationsbereich wird angezeigt. Ausführliche Informationen zu den Konfigurationseinstellungen finden Sie in den Beschreibungen zu <a href="#method1">Konfigurationsmethode 1</a>.

![Konfigurieren der Repositoryeinstellungen für Erstellung in der UX-Canvas](media/author-visually/configure-repo-2.png)

#### <a name="switch-to-a-different-git-repo"></a>Wechseln zu einem anderen Git-Repository

Um zu einem anderen Git-Repository zu wechseln, verwenden Sie das Symbol oben rechts auf der Data Factory-Übersichtseite, wie im folgenden Screenshot gezeigt. Wenn das Symbol nicht angezeigt wird, löschen Sie den lokalen Browsercache. Wählen Sie das Symbol aus, um die Zuordnung zum aktuellen Repository zu entfernen.

Nachdem Sie die Zuordnung zum aktuellen Repository entfernt haben, können Sie Ihre Git-Einstellungen zur Verwendung eines anderen Repositorys konfigurieren. Dann können Sie vorhandene Data Factory-Ressourcen in das neue Repository importieren.

![Entfernen Sie die Zuordnung zum aktuellen Git-Repository.](media/author-visually/remove-repo.png)

### <a name="use-version-control"></a>Verwenden von Versionskontrolle
Versionskontrollsysteme (auch als _Quellcodeverwaltung_ bezeichnet) ermöglichen Entwicklern die Zusammenarbeit an Code und das Nachverfolgen von an der Codebasis vorgenommenen Änderungen. Quellcodeverwaltung ist ein unverzichtbares Tool für Projekte, an denen mehrere Entwickler arbeiten.

Jedes VSTS Git-Repository, das einer Data Factory zugeordnet ist, verfügt über einen Branch für die Kollaboration. (`master` ist der Standardbranch für die Kollaboration.) Benutzer können auch Featurebranches verwenden, indem sie auf **+ Neuer Branch** klicken und in den Featurebranches Entwicklungsschritte ausführen.

![Ändern des Codes durch Synchronisieren oder Veröffentlichen](media/author-visually/sync-publish.png)

Wenn Sie mit der Featurebereitstellung in Ihrem Featurebranch fertig sind, können Sie auf **Pull Request erstellen** klicken. Sie gelangen zu VSTS GIT, wo Sie Pull Requests auslösen, Codereviews durchführen und Änderungen an Ihrem Kollaborationsbranch zusammenführen können. (`master` ist die Standardeinstellung.) Sie können nur Veröffentlichungen für den Data Factory-Dienst Ihres Kollaborationsbranchs durchführen. 

![Erstellen eines neuen Pull Requests](media/author-visually/create-pull-request.png)

#### <a name="publish-code-changes"></a>Veröffentlichen von Codeänderungen
Nachdem Sie die Änderungen des Kollaborationsbranchs zusammengeführt haben (`master` ist die Standardeinstellung), können Sie **Veröffentlichen** wählen, um Ihre Codeänderungen manuell im Branch „master“ für den Data Factory-Dienst zu veröffentlichen.

![Veröffentlichen von Änderungen für den Data Factory-Dienst](media/author-visually/publish-changes.png)

> [!IMPORTANT]
> Der Masterbranch ist nicht repräsentativ für das, was im Data Factory-Dienst bereitgestellt wird. Der Masterbranch *muss* manuell im Data Factory-Dienst veröffentlicht werden.

## <a name="use-the-expression-language"></a>Verwenden der Ausdruckssprache
Sie können Ausdrücke für Eigenschaftswerte angeben, indem sie die Ausdruckssprache verwenden, die von Azure Data Factory unterstützt wird. 

Geben Sie Ausdrücke für Eigenschaftswerte an, indem Sie **Dynamischen Inhalt hinzufügen** auswählen:

![Verwenden der Ausdruckssprache](media/author-visually/dynamic-content-1.png)

## <a name="use-functions-and-parameters"></a>Verwenden von Funktionen und Parametern

Sie können im **Ausdrucks-Generator** von Data Factory Funktionen verwenden oder Parameter für Pipelines und Datasets angeben:

Informationen zu den unterstützten Ausdrücken finden Sie unter [Ausdrücke und Funktionen in Azure Data Factory](control-flow-expression-language-functions.md).

![Dynamischen Inhalt hinzufügen](media/author-visually/dynamic-content-2.png)

## <a name="provide-feedback"></a>Feedback geben
Wählen Sie **Feedback** aus, um Kommentare zu Funktionen abzugeben oder um Microsoft Probleme mit dem Tool zu melden:

![Feedback](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Nächste Schritte
Unter [Programmgesteuertes Überwachen und Verwalten von Pipelines](monitor-programmatically.md) erfahren Sie mehr über das Überwachen und Verwalten von Pipelines.
