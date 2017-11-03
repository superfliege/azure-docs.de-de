---
title: Upgrade von Azure Log Analytics auf die neue Protokollsuche | Microsoft-Dokumentation
description: "Die neue Log Analytics-Abfragesprache ist fast da – Sie können an der öffentlichen Vorschau teilnehmen.  Dieser Artikel beschreibt die Vorteile der neuen Sprache und die Konvertierung Ihres Arbeitsbereichs."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 1806b70ba0d34f49abfb954abebff8d29ae61291
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2017
---
# <a name="azure-log-analytics-upgrade-to-new-log-search"></a>Upgrade von Azure Log Analytics auf die neue Protokollsuche

Die neue Log Analytics-Abfragesprache ist da, und Sie müssen Ihren Arbeitsbereich aktualisieren, um sie nutzen zu können.  Sie können Ihren Arbeitsbereich entweder selbst aktualisieren oder warten, bis dieser automatisch während der Rolloutphase aktualisiert wird, die im Oktober startet und bis zum Ende des Jahres andauert.  Dieser Artikel beschreibt die Vorteile der neuen Sprache und die Konvertierung Ihres Arbeitsbereichs.  

## <a name="why-the-new-language"></a>Warum gibt es eine neue Sprache?
Wir wissen, dass jede Neuerung auch immer Arbeit und Mühen mit sich bringt, und wir ändern die Abfragesprache nicht einfach so aus Spaß.  Es gibt mehrere Gründe dafür, dass diese Änderung einen erheblichen Mehrwert für Ihre Log Analytics-Kunden bedeuten wird.

- **Einfach und gleichzeitig leistungsstark.** Die neue Sprache ist einfacher zu verstehen und ähnelt SQL – die Konstrukte entsprechen eher der natürlichen Sprache als bei der bisherigen Abfragesprache.
- **Umfassende Pipingfunktionen.**  Die neue Sprache bietet umfangreichere Pipingfunktionen als die alte.  Praktisch jede Ausgabe kann per Piping an einen anderen Befehl weitergegeben werden. So lassen sich komplexere Abfragen erstellen, die zuvor nicht möglich waren.
- **Feldextraktionen zur Suchzeit.**  Die neue Sprache unterstützt umfassendere zur der Laufzeit berechnete Felder als die alte Sprache.  Sie können komplexe Berechnungen für erweiterte Felder durchführen, und die berechneten Felder dann für weitere Befehle wie z.B. Joins und Aggregationen verwenden.
- **Erweiterte Joins.**  Die neue Sprache bietet im Vergleich zur alten Sprache erweiterte Joins, einschließlich der Fähigkeit, Tabellen in mehreren Feldern zu verknüpfen, innere und äußere Joins zu verwenden und erweiterte Felder zu verknüpfen.
- **Funktionen für Datum und Uhrzeit.**  Die neue Sprache bietet modernere Funktionen für Datum und Uhrzeit als die alte.
- **Intelligente Analysen**  Die neue Sprache arbeitet mit erweiterten Algorithmen, um Muster in Datasets zu bewerten und verschiedene Datasets zu vergleichen.
- **Advanced Analytics-Portal.**  Das Advanced Analytics-Portal bietet Analysefunktionen, die im Log Analytics-Portal nicht verfügbar sind, beispielsweise die Bearbeitung von mehreren Zeilen in Abfragen, zusätzliche Visualisierungen und eine erweiterte Diagnose.
- **Konsistenz mit anderen Anwendungen.**  Die neue Sprache und das Advanced Analytics-Portal werden bereits für Analysen in Application Insights verwendet.  Durch die Implementierung für Log Analytics sorgen wir für Konsistenz über die Azure-Dienste hinweg.
- **Bessere Integration in Power BI.** Abfragen in der neuen Sprache können in Power BI Desktop exportiert werden, damit Sie die umfangreichen Funktionen für die Datentransformation nutzen können.
- **Viele weitere Vorteile.** Auf der Website [Azure Log Analytics Query Language](https://docs.loganalytics.io) finden Sie alle Informationen und Tutorials für die neue Sprache.


## <a name="when-can-i-upgrade"></a>Wann kann ich das Upgrade durchführen?
Das Upgrade wird phasenweise in allen Azure-Regionen eingeführt und wird daher in einigen Regionen früher zur Verfügung stehen als in anderen.  Wenn Sie im oberen Bereich Ihres Arbeitsbereichs ein Banner sehen, das Sie auf die Upgrademöglichkeit hinweist, können Sie das Upgrade für Ihren Arbeitsbereich durchführen.

![Upgrade 1](media/log-analytics-log-search-upgrade/upgrade-01a.png)

Wenn Ihr Arbeitsbereich automatisch ein Upgrade durchführt, wird Ihnen ein Banner angezeigt, das den Upgradevorgang und eine Zusammenfassung darstellt, die angibt, ob Probleme aufgetreten sind.

 ![automatische Upgrade](media/log-analytics-log-search-upgrade/auto-upgrade.png)


## <a name="what-happens-after-the-upgrade"></a>Was geschieht nach dem Upgrade?
Die folgenden Änderungen werden an Ihrem Arbeitsbereich vorgenommen, wenn dieser konvertiert wird:

- Alle gespeicherten Suchvorgänge, Warnungsregeln und Ansichten, die Sie mit dem Ansicht-Designer erstellt haben, werden automatisch in die neue Sprache konvertiert.  Suchvorgänge, die in Projektmappen eingebunden sind, werden nicht automatisch, sondern beim nächsten Öffnen konvertiert.  
- [Mein Dashboard](log-analytics-dashboards.md) wird zugunsten des [Ansicht-Designers](log-analytics-view-designer.md) und des [Azure-Dashboards](https://docs.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards.md) nicht mehr unterstützt.  Kacheln, die Sie zu „Mein Dashboard“ hinzugefügt haben, sind schreibgeschützt, aber weiterhin verfügbar.
- Die [Power BI-Integration](log-analytics-powerbi.md) wird durch einen neuen Prozess ersetzt.  Alle vorhandenen Power BI-Zeitpläne, die Sie erstellt haben, werden deaktiviert, und Sie müssen diese mit dem neuen Prozess ersetzen.
- Antworten von [Warnungsaktionen](log-analytics-alerts-actions.md) mithilfe von Webhooks und Runbooks haben ein neues Format, dem entsprechend Sie Ihre Warnungsregeln aktualisieren sollten.
- Unter [Protokollsuche – häufig gestellte Fragen](log-analytics-log-search-faq.md) finden Sie häufig gestellte Fragen zum Upgrade.

## <a name="how-do-i-know-if-there-were-any-issues-from-the-upgrade"></a>Wie erkenne ich, ob Probleme durch das Upgrade aufgetreten sind?
Nachdem das Upgrade abgeschlossen wurde, ist der Abschnitt **Upgradezusammenfassung** in den Einstellungen für den Arbeitsbereich verfügbar.  Überprüfen Sie diesen Abschnitt für Informationen über das Upgrade und zum Anzeigen der

 ![Upgradezusammenfassung.](media/log-analytics-log-search-upgrade/upgrade-summary.png)

## <a name="how-do-i-manually-perform-the-upgrade"></a>Wie führe ich das Upgrade manuell durch?
Sie können das Upgrade für Ihren Arbeitsbereich durchführen, wenn oben im Portal das Banner angezeigt wird.  

1.  Starten Sie den Upgradeprozess, indem Sie auf das Banner **Weitere Informationen und Upgrade** klicken.

    ![Upgrade 2](media/log-analytics-log-search-upgrade/upgrade-01a.png)<br>

2.  Lesen Sie die zusätzlichen Informationen auf der Seite „Upgradeinformationen“.

    ![Upgrade 2](media/log-analytics-log-search-upgrade/upgrade-03.png)<br>

3.  Klicken Sie auf **Upgrade jetzt auszuführen**, um das Upgrade zu starten.

    ![Upgrade 4](media/log-analytics-log-search-upgrade/upgrade-04.png)<br>Ein Benachrichtigungsfeld in der oberen rechten Ecke zeigt den Status an.
    
    ![Upgrade 5](media/log-analytics-log-search-upgrade/upgrade-05.png)

4.  Fertig!  Wechseln Sie zur Seite der Protokollsuche, um sich Ihren aktualisierten Arbeitsbereich anzusehen.

    ![Upgrade 6](media/log-analytics-log-search-upgrade/upgrade-06.png)

Wenn ein Fehler auftritt, durch den das Upgrade nicht ausgeführt werden kann, stellen Sie eine Frage im [Diskussionsforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights), oder [erstellen Sie eine Supportanfrage](../azure-supportability/how-to-create-azure-support-request.md) im Azure-Portal.

## <a name="how-do-i-learn-the-new-language"></a>Wie lerne ich die neue Sprache?
Da die neue Sprache von vielen Diensten verwendet wird, haben wir eine [externe Website für die Dokumentation](https://docs.loganalytics.io/) erstellt.  Diese Website enthält Tutorials, Beispiele und eine vollständige Referenz, damit Sie schnell loslegen können. Unter [Getting Started with Queries (Erste Schritte mit Abfragen)](https://go.microsoft.com/fwlink/?linkid=856078) können Sie ein Tutorial zu der neuen Sprache durchlaufen. Unter [Log Analytics query language (Log Analytics-Abfragesprache)](https://go.microsoft.com/fwlink/?linkid=856079) finden Sie die Sprachreferenz.  

Wenn Sie die neue Sprache ausprobieren möchten, werfen Sie einen Blick auf die [Demo-Umgebung](https://portal.loganalytics.io/demo#/discover/home), die eine Reihe von Beispieldaten enthält.

Wenn Sie jedoch die bisherige Log Analytics-Abfragesprache noch nicht kennen, können Sie den Sprachkonverter verwenden, der Ihrem Arbeitsbereich im Rahmen des Upgrades hinzugefügt wird.  Geben Sie einfach die Abfrage in der alten Sprache ein, und klicken Sie dann auf **Konvertieren**, um die übersetzte Version anzuzeigen.  Sie können entweder auf die Suchschaltfläche klicken, um die Abfrage auszuführen, oder die Abfrage kopieren und an einer anderen Stelle verwenden – z.B. für eine Warnungsregel.  Sie können auch einen Blick auf unser [Cheatsheet](log-analytics-log-search-transition.md) werfen, das allgemeine Abfragen direkt über die Legacysprache vergleicht.

![Sprachkonverter](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="next-steps"></a>Nächste Schritte
- Sehen Sie sich ein [Tutorial zur neuen Sprache](https://go.microsoft.com/fwlink/?linkid=856078) an.
- Durchlaufen Sie ein [Tutorial zur Verwendung des Portals der Protokollsuche](log-analytics-log-search-log-search-portal.md) mit der neuen Abfragesprache.
- Sehen Sie sich eine Einführung in das neue [Advanced Analytics-Portal](https://go.microsoft.com/fwlink/?linkid=856587) an.
