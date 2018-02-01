---
title: Visuelles Erstellen von Data Factorys | Microsoft Docs
description: Erfahren Sie, wie Sie Azure Data Factorys visuell erstellen.
services: data-factory
documentationcenter: 
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
ms.openlocfilehash: 3e67665facba78c4ca8e2317f0323b4c5c02a49c
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2018
---
# <a name="visually-author-data-factories"></a>Visuelles Erstellen von Data Factorys
Benutzer können mit der UX-Erfahrung von Azure Data Factory Ressourcen in ihrer Data Factory visuell erstellen und bereitstellen, ohne eine einzige Codezeile schreiben zu müssen. Diese codefreie Schnittstelle ermöglicht es Ihnen, Aktivitäten mit Drag & Drop auf eine Pipelinecanvas zu ziehen, Testläufe auszuführen, iterativ zu debuggen und Ihre Pipelineausführungen zu implementieren und zu überwachen. Sie können das Tool der ADF-UX auf zwei Arten verwenden:

1. Direktes Arbeiten mit dem Data Factory-Dienst
2. Konfigurieren der VSTS Git-Integration für Zusammenarbeit, Quellcodeverwaltung oder Versionsverwaltung

## <a name="authoring-with-data-factory"></a>Erstellung mit Data Factory
Die erste Option ist das direkte Erstellen mit dem Data Factory-Modus. Dieser Ansatz unterscheidet sich insofern von der Erstellung über das VSTS-Coderepository, als es kein Repository gibt, in dem die JSON-Entitäten Ihrer Änderungen gespeichert werden, und auch keine Optimierung für die Zusammenarbeit oder Versionskontrolle erfolgt.

![Konfigurieren der Data Factory](media/author-visually/configure-data-factory.png)

Im Data Factory-Modus ist nur auf der Modus „Veröffentlichen“ verfügbar. Alle vorgenommenen Änderungen werden direkt im Data Factory-Dienst veröffentlicht.

![Data Factory-Veröffentlichung](media/author-visually/data-factory-publish.png)

## <a name="authoring-with-vsts-git-integration"></a>Erstellung mit VSTS Git-Integration
Die Erstellung mit VSTS Git-Integration ermöglicht Quellcodeverwaltung und Zusammenarbeit beim Erstellen der Data Factory-Pipelines. Benutzer haben die Möglichkeit, eine Data Factory einem VSTS Git-Kontorepository z.B. für die Quellcodeverwaltung, Zusammenarbeit und Versionsverwaltung zuzuordnen. Ein einzelnes VSTS Git-Konto kann über mehrere Repositorys verfügen. Allerdings kann ein VSTS Git-Repository nur einer einzigen Data Factory zugeordnet werden. Wenn Sie noch nicht über ein VSTS-Konto und ein Repository verfügen, können Sie beides [hier](https://docs.microsoft.com/en-us/vsts/accounts/create-account-msa-or-work-student) erstellen.

### <a name="configure-vsts-git-repo-with-azure-data-factory"></a>Konfigurieren eines VSTS Git-Repositorys mit Azure Data Factory
Benutzer können ein VSTS Git-Repository mit einer Data Factory mit zwei Methoden konfigurieren.

#### <a name="method-1-lets-get-started-page"></a>Methode 1: Seite „Erste Schritte“

Navigieren Sie zur Seite „Erste Schritte“, und klicken Sie auf „Coderepository konfigurieren“.

![Konfigurieren des Coderepositorys](media/author-visually/configure-repo.png)

Nun wird eine Seitenleiste zum Konfigurieren der Repositoryeinstellungen angezeigt.

![Konfigurieren der Repositoryeinstellungen](media/author-visually/repo-settings.png)
* **Repositorytyp**: Visual Studio Team Services Git (zurzeit wird GitHub nicht unterstützt.)
* **Visual Studio Team Services-Konto**: Den Kontonamen finden Sie unter https://{Kontoname}.visualstudio.com. Melden Sie sich [hier](https://www.visualstudio.com/team-services/git/) bei Ihrem VSTS-Konto an, und greifen Sie auf Ihr Visual Studio-Profil zu, um Ihre Repositorys und Projekte anzuzeigen.
* **ProjectName:** Den Projektnamen finden Sie unter https://{Kontoname}.visualstudio.com/{Projektname}.
* **: RepositoryName:** Der Repositoryname. VSTS-Projekte enthalten Git-Repositorys zum Verwalten Ihres Quellcodes, wenn Ihr Projekt größer wird. Erstellen Sie ein neues Repository, oder verwenden Sie ein vorhandenes Repository, das sich bereits im Projekt befindet.
* **Vorhandene Data Factory-Ressourcen in Repository importieren**: Wenn Sie dieses Kontrollkästchen aktivieren, können Sie Ihre aktuellen Data Factory-Ressourcen, die auf der UX-Canvas erstellt wurden, in das zugehörige VSTS Git-Repository im JSON-Format importieren. Diese Aktion exportiert jede Ressource einzeln (d. h. verknüpfte Diensten und Datasets werden in separate JSONs exportiert).    Wenn Sie dieses Kontrollkästchen deaktivieren, werden die vorhandenen Ressourcen nicht in das Git-Repository importiert.

#### <a name="method-2-from-authoring-canvas"></a>Methode 2: Aus dem Dokumenterstellungsbereich

Klicken Sie im „Dokumenterstellungsbereich“ unter dem Namen Ihrer Data Factory auf das Dropdownmenü „Data Factory“. Klicken Sie dann auf „Coderepository konfigurieren“. Ähnlich wie bei **Methode 1** wird nun eine Seitenleiste zum Konfigurieren der Repositoryeinstellungen angezeigt. Informationen zu den Einstellungen finden Sie in den vorherigen Abschnitten.

![Konfigurieren von Coderepository 2](media/author-visually/configure-repo-2.png)

### <a name="version-control"></a>Versionskontrolle
Versionskontrolle (auch als Quellcodeverwaltung bezeichnet) ermöglicht Entwicklern die Zusammenarbeit an Code und das Nachverfolgen von an der Codebasis vorgenommenen Änderungen. Quellcodeverwaltung ist ein unverzichtbares Tool für Projekte, an denen mehrere Entwickler arbeiten.

Jedes VSTS Git-Repository verfügt über einen Masterbranch, sobald es einer Data Factory zugeordnet wurde. Ab diesem Zeitpunkt hat jeder Benutzer, der Zugriff auf das VSTS Git-Repository besitzt, zwei Optionen beim Vornehmen von Änderungen: „Synchronisieren“ oder „Veröffentlichen“.

![Synchronisieren/Veröffentlichen](media/author-visually/sync-publish.png)

#### <a name="sync"></a>Synchronisieren

Sobald Sie auf „Synchronisieren“ klicken, können Änderungen aus dem Masterbranch in Ihren lokalen Branch gepullt oder aus dem lokalen Branch in den Masterbranch gepusht werden.

![Synchronisieren von Änderungen](media/author-visually/sync-change.png)

#### <a name="publish"></a>Veröffentlichen
 Veröffentlichen Sie Änderungen im Masterbranch im Data Factory-Dienst.

> [!NOTE]
> Der **Masterbranch ist nicht repräsentativ für das, was im Data Factory-Dienst bereitgestellt wird.** Der Masterbranch *muss* manuell im Data Factory-Dienst veröffentlicht werden.




## <a name="expression-language"></a>Ausdruckssprache

Benutzer können beim Definieren der Eigenschaftswerte Ausdrücke angeben, indem sie die Ausdruckssprache verwenden, die von Azure Data Factory unterstützt wird. Weitere Informationen dazu, welche Ausdrücken unterstützt werden, finden Sie unter [Ausdrücke und Funktionen in Azure Data Factory](control-flow-expression-language-functions.md).

Geben Sie die Ausdrücke in Eigenschaftswerten in der UX wie folgt am.

![Ausdruckssprache](media/author-visually/expression-language.png)

## <a name="parameters"></a>Parameter
Benutzer können Parameter für Pipelines und Datasets auf der Registerkarte „Parameter“ angeben. Außerdem können sie Parameter in den Eigenschaften problemlos nutzen, indem sie auf „Dynamischen Inhalt hinzufügen“ klicken.

![Dynamischer Inhalt](media/author-visually/dynamic-content.png)

Hier können Sie einen vorhandenen Parameter verwenden oder einen neuen Parameter in Ihrem Eigenschaftswert angeben.

![Parameter](media/author-visually/parameters.png)

## <a name="feedback"></a>Feedback
Klicken Sie auf das Symbol „Feedback“, um uns (Microsoft) Ihr Feedback zu den verschiedenen Features oder zu möglichen Problemen zu übermitteln.

![Feedback](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Nächste Schritte

Im Artikel [Programmgesteuertes Überwachen und Verwalten von Pipelines](monitor-programmatically.md) erfahren Sie mehr über das Überwachen und Verwalten von Pipelines.
