---
title: "Behandeln von Problemen mit nicht normalen periodischen Aufträgen | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe von Azure Data Lake Tools für Visual Studio einen nicht normalen periodischen Auftrag debuggen."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/27/2017
ms.author: yanacai
ms.openlocfilehash: a358f94b117c12511028a875e56b5c9dba8d3382
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-troubleshoot-an-abnormal-recurring-job"></a>Behandeln von Problemen mit nicht normalen periodischen Aufträgen

In diesem Dokument erfahren Sie, wie Sie mithilfe von [Azure Data Lake Tools für Visual Studio](http://aka.ms/adltoolsvs) Probleme mit periodischen Aufträgen behandeln. Weitere Informationen zu Pipelineaufträgen und periodischen Aufträgen finden Sie [hier](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/).
Periodische Aufträge verwenden in der Regel die gleiche Abfragelogik und ähnliche Eingabedaten. Ein Beispiel wäre ein periodischer Auftrag, der jeden Montag um 8 Uhr ausgeführt wird, um die aktiven Benutzer der letzten Woche zu zählen, wobei die Skripts für diese Aufträge eine einzelne Skriptvorlage mit der Abfragelogik verwenden und als Auftragseingaben die Nutzungsdaten der Vorwoche herangezogen werden. Durch die gemeinsame Verwendung der gleichen Abfragelogik und einer ähnlichen Eingabe ergibt sich in der Regel eine ähnliche und stabile Auftragsleistung. Sollte bei einem Ihrer periodischen Aufträge plötzlich unnormales Verhalten, ein Fehler oder eine starke Verlangsamung auftreten, haben Sie folgende Möglichkeiten:

1.  Ermitteln Sie anhand der Statistikberichte für die Vorschaudurchläufe des periodischen Auftrags, was passiert ist.
2.  Vergleichen Sie die den nicht normalen Auftrag mit einem normalen Auftrag, um zu ermitteln, was sich geändert hat.

Die Ansicht für zugehörige Aufträge**** in Azure Data Lake Tools für Visual Studio trägt in beiden Fällen zu einer schnelleren Problembehandlung bei.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>Schritt 1: Suchen nach periodischen Aufträgen und Öffnen der Ansicht für zugehörige Aufträge

Um ein Problem mit einem periodischen Auftrag unter Verwendung der Ansicht für zugehörige Aufträge behandeln zu können, müssen Sie den wiederkehrenden Auftrag zunächst in Visual Studio suchen und anschließend die Ansicht für zugehörige Aufträge öffnen.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Fall 1: Sie verfügen über die URL für den periodischen Auftrag.

Unter **Tools > Data Lake > Auftragsansicht** können Sie die Auftrags-URL einfügen, um die Auftragsansicht in Visual Studio zu öffnen, und über „Zugehörige Aufträge anzeigen“ gelangen Sie zur Ansicht für zugehörige Aufträge.

![Anzeigen zugehöriger Aufträge in Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Fall 2: Sie verfügen über die Pipeline für den periodischen Auftrag, aber nicht über die URL.

In Visual Studio können Sie unter **Server-Explorer > <Ihr Data Lake Analytics-Konto> > Pipelines** den Pipelinebrowser öffnen. (Sollten Sie diesen Knoten im Server-Explorer nicht finden, laden Sie [hier](http://aka.ms/adltoolsvs) das neueste Tool herunter.) Auf der linken Seite des Pipelinebrowsers werden alle Pipelines für das ADLA-Konto aufgeführt. Sie können die Pipelines erweitern, um alle periodischen Aufträge anzuzeigen. Klicken Sie auf den Auftrag, für den ein Problem vorliegt. Daraufhin öffnet sich auf der rechten Seite die Ansicht für zugehörige Aufträge.

![Anzeigen zugehöriger Aufträge in Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

![Anzeigen zugehöriger Aufträge in Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-statistics-report"></a>Schritt 2: Analysieren des Statistikberichts

Im oberen Bereich der Ansicht für zugehörige Aufträge werden eine Zusammenfassung und ein Statistikbericht angezeigt. Hier können Sie die potenzielle Ursache für die das Problem ermitteln. 

1.  Suchen Sie als Erstes im Bericht nach dem nicht normalen Auftrag. Die X-Achse zeigt die Auftragsübermittlungszeit, anhand der Sie den nicht normalen Auftrag ermitteln können.
2.  Gehen Sie wie folgt vor, um die Statistik zu prüfen, Informationen zu der Abweichung von der Norm zu erhalten und Lösungsmöglichkeiten zu ermitteln.

![Anzeigen zugehöriger Aufträge in Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-recurring-job-to-a-normal-job"></a>Schritt 3: Vergleichen des nicht normalen periodischen Auftrags mit einem normalen Auftrag

Sie finden alle übermittelten periodischen Aufträge in der Auftragsliste im unteren Bereich der Ansicht für zugehörige Aufträge. Per Rechtsklick können Sie den nicht normalen Auftrag mit einem vorherigen normalen Auftrag vergleichen und in der Auftragsvergleichsansicht weitere Erkenntnisse gewinnen sowie nach Lösungsmöglichkeiten suchen.

![Anzeigen zugehöriger Aufträge in Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

In der Regel empfiehlt es sich, auf die großen Unterschiede zwischen den beiden Aufträgen zu achten, da die Leistungsprobleme wahrscheinlich auf sie zurückzuführen sind. Mit den unten angegebenen Schritten können Sie zudem eine ausführlichere Prüfung durchführen.

![Anzeigen zugehöriger Aufträge in Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Nächste Schritte

* [Lösen von Problemen aufgrund von Datenschiefe mithilfe von Azure Data Lake Tools für Visual Studio](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Debuggen von benutzerdefiniertem C#-Code für fehlerhafte U-SQL-Aufträge](data-lake-analytics-debug-u-sql-jobs.md)