---
title: Erstellen von Ansichten zum Analysieren von Daten in Azure Log Analytics | Microsoft-Dokumentation
description: "Mit dem Ansicht-Designer in Log Analytics können Sie benutzerdefinierte Ansichten erstellen, die im Azure-Portal angezeigt werden und verschiedene Visualisierungen von Daten im Log Analytics-Arbeitsbereich enthalten. Dieser Artikel enthält eine Übersicht über den Ansicht-Designer und die Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: bwren
ms.openlocfilehash: a84f40503c1b9778c496461ebbf6864f99bd1c4b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Erstellen benutzerdefinierter Ansichten in Log Analytics mithilfe des Ansicht-Designers
Mit dem Ansicht-Designer in [Log Analytics](log-analytics-overview.md) können Sie benutzerdefinierte Ansichten im Azure-Portal mit verschiedenen Visualisierungen von Daten aus Ihrem Log Analytics-Arbeitsbereich erstellen. Dieser Artikel enthält eine Übersicht über den Ansicht-Designer und die Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten.

Weitere verfügbare Artikel für den Ansicht-Designer:

* [Referenz der Kacheln](log-analytics-view-designer-tiles.md): Referenz der Einstellungen für die einzelnen Kacheln, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind.
* [Referenz der Visualisierungskomponenten](log-analytics-view-designer-parts.md): Referenz der Einstellungen für die einzelnen Kacheln, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind.

>[!NOTE]
> Wenn Ihr Arbeitsbereich auf die [neue Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md) aktualisiert wurde, müssen die Abfragen in allen Ansichten in der [neuen Abfragesprache](https://go.microsoft.com/fwlink/?linkid=856078) geschrieben werden.  Alle Ansichten, die vor dem Upgrade des Arbeitsbereichs erstellt wurden, werden automatisch konvertiert.

## <a name="concepts"></a>Konzepte
Ansichten werden im Azure-Portal auf der Seite **Übersicht** Ihres Log Analytics-Arbeitsbereichs angezeigt.  Die Kacheln der einzelnen benutzerdefinierten Ansichten werden mit den Kacheln für die im gleichen Arbeitsbereich installierten Lösungen in alphabetischer Reihenfolge angezeigt.

![Seite „Übersicht“](media/log-analytics-view-designer/overview-page.png)

Mit dem Ansicht-Designer erstellte Ansichten enthalten die Elemente der folgenden Tabelle.

| Teil | BESCHREIBUNG |
|:--- |:--- |
| Kachel |Wird auf der Übersichtsseite Ihres Log Analytics-Arbeitsbereichs angezeigt.  Enthält ein visuelle Zusammenfassung der in der benutzerdefinierten Ansicht enthaltenen Informationen.  Verschiedene Kacheltypen bieten unterschiedliche Visualisierungen von Datensätzen.  Klicken Sie auf die Kachel, um die benutzerdefinierte Ansicht zu öffnen. |
| Benutzerdefinierte Ansicht |Wird angezeigt, wenn der Benutzer auf die Kachel klickt.  Enthält eine oder mehrere Visualisierungskomponenten. |
| Visualisierungskomponenten |Visualisierung von Daten im Log Analytics-Arbeitsbereich basierend auf einem oder mehreren [Protokollsuchvorgängen](log-analytics-log-searches.md).  Die meisten Komponenten enthalten eine Kopfzeile mit einer allgemeinen Visualisierung und eine Liste der wichtigsten Ergebnisse.  Verschiedene Komponententypen bieten unterschiedliche Visualisierungen von Datensätzen im Log Analytics-Arbeitsbereich.  Klicken Sie auf Elemente in der Komponente, um einen Protokollsuchvorgang auszuführen, der detaillierte Datensätze bereitstellt. |


## <a name="work-with-an-existing-view"></a>Verwenden einer vorhandenen Ansicht
Wenn Sie eine mit dem Ansicht-Designer erstellte Ansicht öffnen, enthält sie ein Menü mit den in der folgenden Tabelle aufgeführten Optionen.

![Menü „Übersicht“](media/log-analytics-view-designer/overview-menu.png)


| Option | BESCHREIBUNG |
|:--|:--|
| Aktualisieren   | Aktualisieren Sie die Ansicht mit den neuesten Daten. | 
| Analyse | Öffnen Sie das [Advanced Analytics-Portal](log-analytics-log-search-portals.md#advanced-analytics-portal), um Daten mit Protokollsuchvorgängen zu analysieren.(log-analytics-log-search-portals.md#advanced-analytics-portal). |
| Filter    | Legen Sie einen Zeitfilter für die in der Ansicht enthaltenen Daten fest. |
| Edit (Bearbeiten)      | Öffnen Sie die Ansicht im Ansicht-Designer, um ihre Inhalte und die Konfiguration zu bearbeiten.   |
| Klonen     | Erstellen Sie eine neue Ansicht, und öffnen Sie sie im Ansicht-Designer.  Die neue Ansicht weist den gleichen Namen wie das Original auf, allerdings wird „Kopie“ an das Ende angefügt. |


## <a name="create-a-new-view"></a>Erstellen einer neuen Ansicht
Erstellen Sie im **Ansicht-Designer** eine neue Ansicht, indem Sie im Azure-Portal im Log Analytics-Arbeitsbereich auf der Seite „Übersicht“ auf die Kachel für den Ansicht-Designer klicken.

![Kachel für den Ansicht-Designer](media/log-analytics-view-designer/view-designer-tile.png)


## <a name="working-with-view-designer"></a>Arbeiten mit dem Ansicht-Designer
Sie verwenden den Ansicht-Designer – unabhängig davon, ob Sie eine neue Ansicht erstellen oder eine vorhandene Ansicht bearbeiten.  

Der Ansicht-Designer verfügt über drei Bereiche.  Der **Entwurfsbereich** enthält die benutzerdefinierte Ansicht, die Sie erstellen oder bearbeiten.  Sie fügen Kacheln und Komponenten aus dem Bereich **Steuerung** zum Bereich **Entwurf** hinzu.  Im Bereich **Eigenschaften** werden die Eigenschaften für die Kachel oder die ausgewählte Komponente angezeigt.

![Ansicht-Designer](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurieren der Kachel für die Ansicht
Eine benutzerdefinierte Ansicht kann nur über eine einzige Kachel verfügen.  Wählen Sie die Registerkarte **Kachel** im Bereich **Steuerung** aus, um die aktuelle Kachel anzuzeigen oder eine andere auszuwählen.  Im Bereich **Eigenschaften** werden die Eigenschaften für die aktuelle Kachel angezeigt.  Konfigurieren Sie die Kacheleigenschaften entsprechend den detaillierten Informationen in der [Referenz der Kacheln](log-analytics-view-designer-tiles.md), und klicken Sie auf **Übernehmen**, um die Änderungen zu speichern.

### <a name="configure-visualization-parts"></a>Konfigurieren von Visualisierungskomponenten
Eine Ansicht kann eine beliebige Anzahl von Visualisierungskomponenten enthalten.  Wählen Sie die Registerkarte **Ansicht** und anschließend eine Visualisierungskomponente aus, die Sie der Ansicht hinzufügen möchten.  Im Bereich **Eigenschaften** werden die Eigenschaften für die ausgewählte Komponente angezeigt.  Konfigurieren Sie die Ansichtseigenschaften entsprechend den detaillierten Informationen in der [Referenz der Visualisierungskomponenten](log-analytics-view-designer-parts.md), und klicken Sie auf **Übernehmen**, um die Änderungen zu speichern.

Ansichten weisen nur eine Zeile mit Visualisierungskomponenten auf.  Sie können vorhandene Komponenten in einer Ansicht neu anordnen, indem Sie darauf klicken und sie an eine neue Position ziehen.

Sie können eine Visualisierungskomponente aus der Ansicht entfernen, indem Sie auf die Schaltfläche **X** oben rechts in der Komponente klicken.


### <a name="menu-options"></a>Menüoptionen
Für eine Ansicht im Bearbeitungsmodus stehen die in der folgenden Tabelle aufgeführten Menüoptionen zur Verfügung:

![Bearbeitungsmenü](media/log-analytics-view-designer/edit-menu.png)

| Option | BESCHREIBUNG |
|:--|:--|
| Speichern        | Speichern Sie Ihre Änderungen, und schließen Sie die Ansicht. |
| Abbrechen      | Verwerfen Sie Ihre Änderungen, und schließen Sie die Ansicht. |
| Ansicht löschen | Löschen Sie die Ansicht. |
| Export      | Exportieren Sie die Ansicht in eine [Resource Manager-Vorlage](../azure-resource-manager/resource-group-authoring-templates.md), die Sie in einen anderen Arbeitsbereich importieren können.  Der Name der Datei wird zum Namen der Ansicht mit der Erweiterung *omsview*. |
| Importieren      | Sie können eine *omsview*-Datei importieren, die Sie aus einem anderen Arbeitsbereich exportiert haben.  Dadurch wird die Konfiguration der vorhandenen Ansicht überschrieben. |
| Klonen       | Erstellen Sie eine neue Ansicht, und öffnen Sie sie im Ansicht-Designer.  Die neue Ansicht weist den gleichen Namen wie das Original auf, allerdings wird „Kopie“ an das Ende angefügt. |
| Veröffentlichen     | Exportieren Sie die Ansicht in eine JSON-Datei, die in eine [Verwaltungslösung](../operations-management-suite/operations-management-suite-solutions-resources-views.md) eingefügt werden kann.  Der Name der Datei wird zum Namen der Ansicht mit der Erweiterung *json*. Eine zweite Datei mit der Erweiterung *resjson* wird erstellt. Diese Datei enthält Werte für in der JSON-Datei definierte Ressourcen.

## <a name="next-steps"></a>Nächste Schritte
* Fügen Sie [Kacheln](log-analytics-view-designer-tiles.md) zu Ihrer benutzerdefinierten Ansicht hinzu.
* Fügen Sie [Visualisierungskomponenten](log-analytics-view-designer-parts.md) zu Ihrer benutzerdefinierten Ansicht hinzu.
