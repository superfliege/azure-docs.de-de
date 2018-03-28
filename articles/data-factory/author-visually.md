---
title: Visuelles Erstellen in Azure Data Factory | Microsoft-Dokumentation
description: Informationen zum Verwenden der visuellen Erstellung in Azure Data Factory
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/9/2018
ms.author: shlo
ms.openlocfilehash: 954693ee208dc7868a5a5ad0e774c5c352036627
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
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

Wenn Sie die **UX-Canvas** für die Erstellung zum direkten Erstellen mit dem Data Factory-Dienst verwenden, ist nur der Modus **Veröffentlichen** verfügbar. Alle vorgenommenen Änderungen werden direkt im Data Factory-Dienst veröffentlicht.

![Modus „Veröffentlichen“](media/author-visually/data-factory-publish.png)

## <a name="author-with-vsts-git-integration"></a>Erstellen mit VSTS Git-Integration
Das visuelle Erstellen mit VSTS Git-Integration unterstützt Quellcodeverwaltung und Zusammenarbeit beim Arbeiten an den Data Factory-Pipelines. Sie können eine Data Factory einem VSTS Git-Kontorepository für die Quellcodeverwaltung, Zusammenarbeit, Versionsverwaltung usw. zuordnen. Ein einzelnes VSTS Git-Konto kann über mehrere Repositorys verfügen. Allerdings kann ein VSTS Git-Repository nur einer einzigen Data Factory zugeordnet werden. Wenn Sie noch nicht über ein VSTS-Konto oder ein Repository verfügen, folgen Sie [diesen Anweisungen](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student) zum Erstellen der Ressourcen.

> [!NOTE]
> Sie können Skript- und Datendateien in einem VSTS-Git-Repository speichern. Allerdings müssen Sie die Dateien manuell in Azure Storage hochladen. Eine Data Factory-Pipeline lädt Skript- oder Datendateien, die in einem VSTS-Git-Repository gespeichert sind, nicht automatisch in Azure Storage hoch.

### <a name="configure-a-vsts-git-repository-with-azure-data-factory"></a>Konfigurieren eines VSTS Git-Repositorys mit Azure Data Factory
Es gibt zwei Methoden zum Konfigurieren eines VSTS Git-Repositorys mit einer Data Factory.

<a name="method1"></a>
#### <a name="configuration-method-1-lets-get-started-page"></a>Konfigurationsmethode 1: Seite „Erste Schritte“
Wechseln Sie in Azure Data Factory zur Seite **Erste Schritte**. Wählen Sie **Coderepository konfigurieren** aus:

![Konfigurieren eines VSTS-Coderepositorys](media/author-visually/configure-repo.png)

Der Konfigurationsbereich **Repositoryeinstellungen** wird angezeigt:

![Konfigurieren der Repositoryeinstellungen](media/author-visually/repo-settings.png)

Im Bereich werden die folgenden Einstellungen für den VSTS-Coderepository angezeigt:

| Einstellung | BESCHREIBUNG | Wert |
|:--- |:--- |:--- |
| **Repositorytyp** | Der Typ des VSTS Coderepositorys.<br/>**Hinweis**: GitHub wird derzeit nicht unterstützt. | Visual Studio Team Services Git |
| **Visual Studio Team Services-Konto** | Ihr VSTS-Kontoname. Den VSTS-Kontonamen finden Sie unter `https://{account name}.visualstudio.com`. Sie können sich [bei Ihrem VSTS-Konto anmelden](https://www.visualstudio.com/team-services/git/), um auf Ihr Visual Studio-Profil zuzugreifen und Ihre Repositorys und Projekte anzuzeigen. | \<Ihr Kontoname> |
| **Projektname** | Ihr VSTS-Projektname. Den VSTS-Projektnamen finden Sie unter `https://{account name}.visualstudio.com/{project name}`. | \<Ihr VSTS-Projektname> |
| **Repositoryname** | Der Name Ihres VSTS-Coderepositorys. VSTS-Projekte enthalten Git-Repositorys zum Verwalten Ihres Quellcodes, wenn Ihr Projekt größer wird. Sie können ein neues Repository erstellen oder ein vorhandenes Repository verwenden, das sich bereits in Ihrem Projekt befindet. | \<Name Ihres VSTS-Coderepositorys> |
| **Vorhandene Data Factory-Ressourcen in Repository importieren** | Gibt an, ob vorhandene Data Factory-Ressourcen aus der **UX-Canvas** für die Erstellung in ein VSTS Git-Repository importiert werden sollen. Aktivieren Sie das Kontrollkästchen, um Ihre Data Factory-Ressourcen in das zugehörige Git-Repository im JSON-Format zu importieren. Diese Aktion exportiert jede Ressource einzeln (d. h. die verknüpften Dienste und Datasets werden in separate JSONs exportiert). Ist dieses Kontrollkästchen nicht aktiviert, werden die vorhandenen Ressourcen nicht importiert. | Aktiviert (Standardeinstellung) |

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Konfigurationsmethode 2: UX-Canvas für die Erstellung
Suchen Sie in Azure Data Factory in der **UX-Canvas** für die Erstellung nach Ihrer Data Factory. Klicken Sie auf das Dropdownmenü **Data Factory**, und wählen Sie dann **Coderepository konfigurieren** aus.

Ein Konfigurationsbereich wird angezeigt. Ausführliche Informationen zu den Konfigurationseinstellungen finden Sie in den Beschreibungen zu <a href="#method1">Konfigurationsmethode 1</a>.

![Konfigurieren der Repositoryeinstellungen für Erstellung in der UX-Canvas](media/author-visually/configure-repo-2.png)

### <a name="use-version-control"></a>Verwenden von Versionskontrolle
Versionskontrollsysteme (auch als _Quellcodeverwaltung_ bezeichnet) ermöglichen Entwicklern die Zusammenarbeit an Code und das Nachverfolgen von an der Codebasis vorgenommenen Änderungen. Quellcodeverwaltung ist ein unverzichtbares Tool für Projekte, an denen mehrere Entwickler arbeiten.

Jedes VSTS Git-Repository, das einer Data Factory zugeordnet ist, verfügt über einen Masterbranch. Wenn Sie Zugriff auf ein VSTS Git-Repository haben, können Sie den Code ändern, indem Sie **Synchronisieren** oder **Veröffentlichen** auswählen:

![Ändern des Codes durch Synchronisieren oder Veröffentlichen](media/author-visually/sync-publish.png)

#### <a name="sync-code-changes"></a>Synchronisieren von Codeänderungen
Nachdem Sie **Synchronisieren** ausgewählt haben, können Änderungen aus dem Masterbranch in Ihren lokalen Branch gepullt oder aus dem lokalen Branch in den Masterbranch gepusht werden.

![Synchronisieren von Codeänderungen](media/author-visually/sync-change.png)

#### <a name="publish-code-changes"></a>Veröffentlichen von Codeänderungen
Wählen Sie **Veröffentlichen** aus, um Ihre Codeänderungen im Masterbranch manuell im Data Factory-Dienst zu veröffentlichen.

> [!IMPORTANT]
> Der Masterbranch ist nicht repräsentativ für das, was im Data Factory-Dienst bereitgestellt wird. Der Masterbranch *muss* manuell im Data Factory-Dienst veröffentlicht werden.

## <a name="use-the-expression-language"></a>Verwenden der Ausdruckssprache
Sie können Ausdrücke für Eigenschaftswerte angeben, indem sie die Ausdruckssprache verwenden, die von Azure Data Factory unterstützt wird. Informationen zu den unterstützten Ausdrücken finden Sie unter [Ausdrücke und Funktionen in Azure Data Factory](control-flow-expression-language-functions.md).

Geben Sie Ausdrücke für Eigenschaftswerte mithilfe der **UX-Canvas** für die Erstellung an:

![Verwenden der Ausdruckssprache](media/author-visually/expression-language.png)

## <a name="specify-parameters"></a>Angeben von Parametern
Sie können Parameter für Pipelines und Datasets auf der Registerkarte **Parameter** in Azure Data Factory angeben. Sie können die Parameter in Eigenschaften problemlos verwenden, indem Sie **Dynamischen Inhalt hinzufügen** auswählen:

![Dynamischen Inhalt hinzufügen](media/author-visually/dynamic-content.png)

Sie können vorhandene Parameter verwenden oder neue Parameter für die Eigenschaftswerte angeben:

![Angeben von Parametern für Eigenschaftswerte](media/author-visually/parameters.png)

## <a name="provide-feedback"></a>Feedback geben
Wählen Sie **Feedback** aus, um Kommentare zu Funktionen abzugeben oder um Microsoft Probleme mit dem Tool zu melden:

![Feedback](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Nächste Schritte
Unter [Programmgesteuertes Überwachen und Verwalten von Pipelines](monitor-programmatically.md) erfahren Sie mehr über das Überwachen und Verwalten von Pipelines.
