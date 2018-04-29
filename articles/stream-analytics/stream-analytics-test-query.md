---
title: Testen von Azure Stream Analytics-Aufträgen mit Beispieldaten | Microsoft-Dokumentation
description: Informationen zum Testen von Abfragen in Stream Analytics-Aufträgen.
keywords: Testen von Aufträgen, Stichprobenerstellung für Eingaben, Hochladen von Beispieldaten
documentationcenter: ''
services: stream-analytics
author: SnehaGunda
manager: kfile
ms.assetid: ''
ms.service: stream-analytics
ms.workload: data-services
ms.date: 03/18/2018
ms.author: sngun
ms.openlocfilehash: c026a91fff5b8ef5774993b335f8d61877aa5d39
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="test-your-stream-analytics-query-with-sample-data"></a>Testen von Azure Stream Analytics-Aufträgen mit Beispieldaten

Mithilfe von Azure Stream Analytics können Sie Beispieldaten und Testabfragen in das Portal hochladen, ohne dass Sie dafür einen Auftrag starten oder beenden müssen.

## <a name="upload-sample-data-and-test-the-query"></a>Hochladen von Beispieldaten und Testen der Abfrage

1. Navigieren Sie zu einem vorhandenen Stream Analytics-Auftrag > klicken Sie auf **Abfrage**, um das Abfrage-Editor-Fenster zu öffnen. 

2. Um Ihre Abfrage mit Beispieleingabedaten zu testen, klicken Sie mit der rechten Maustaste auf eine Ihrer Eingaben, und wählen Sie anschließend **Beispieldaten aus Datei hochladen**. Derzeit können Sie nur JSON-formatierte Daten hochladen. Wenn Ihre Daten in einem anderen Format als CSV vorliegen, müssen Sie sie vor dem Hochladen in JSON konvertieren. Sie können ein Open-Source-Konvertierungstool wie [CSV to JSON Converter](http://www.convertcsv.com/csv-to-json.htm) verwenden, um Ihre Daten in JSON zu konvertieren.

    ![Testen von Abfragen im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

3. Nachdem das Hochladen abgeschlossen ist, klicken Sie auf **Testen**, um diese Abfrage mit den Beispieldaten zu testen, die Sie bereitgestellt haben.

    ![Testen von Beispieldaten im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

4. Wenn Sie die Testausgabe zur späteren Verwendung speichern möchten, wird die Ausgabe der Abfrage im Browser mit einem Link zu den Downloadergebnissen angezeigt. Sie können nun Ihre Abfrage einfach und wiederholt ändern und testen, um festzustellen, wie sich die Ausgabe ändert.

   ![Beispielausgabe im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

Wenn Sie in einer Abfrage mehrere Ausgaben verwenden, können Sie die Ergebnisse jeder Ausgabe separat anzeigen und mühelos dazwischen umschalten. Wenn Sie die im Browser angezeigten Ergebnisse überprüft haben, können Sie Ihre Abfrage speichern, Ihren Auftrag starten und ihn die Ereignisse fehlerfrei verarbeiten lassen.
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
