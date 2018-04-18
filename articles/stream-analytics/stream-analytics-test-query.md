---
title: Testen von Abfragen in Azure Stream Analytics
description: Dieser Artikel beschreibt, wie Sie Abfragen mit Beispieldatendateien in Stream Analytics-Aufträgen testen.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: fb7d936dfdc543c208ce98c588f5ad83704ff5dc
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="test-azure-stream-analytics-queries-in-the-azure-portal"></a>Testen von Azure Stream Analytics-Abfragen im Azure-Portal

Mit Azure Stream Analytics können Sie Abfragen im Azure-Portal testen, ohne einen Auftrag starten oder beenden zu müssen.

## <a name="test-the-input"></a>Testen der Eingabe

1. Um die Beispieleingabedaten zu testen, klicken Sie mit der rechten Maustaste auf eine Ihrer Eingaben und wählen dann **Beispieldaten aus Datei hochladen** aus. Derzeit können Sie nur JSON-formatierte Daten hochladen. Wenn Ihre Daten in einem anderen Format als CSV vorliegen, müssen Sie sie vor dem Hochladen in JSON konvertieren. Sie können ein Open-Source-Konvertierungstool wie [CSV to JSON Converter](http://www.convertcsv.com/csv-to-json.htm) verwenden, um Ihre Daten in JSON zu konvertieren.

    ![Testen von Abfragen im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. Nachdem das Hochladen abgeschlossen ist, klicken Sie auf **Testen**, um diese Abfrage mit den Beispieldaten zu testen, die Sie bereitgestellt haben.

    ![Testen von Beispieldaten im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

Wenn Sie die Testausgabe zur späteren Verwendung speichern möchten, wird die Ausgabe der Abfrage im Browser mit einem Link zu den Downloadergebnissen angezeigt. Sie können nun Ihre Abfrage einfach und wiederholt ändern und testen, um festzustellen, wie sich die Ausgabe ändert.

![Beispielausgabe im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

Wenn in einer Abfrage mehrere Ausgaben verwendet werden, können Sie die Ergebnisse jeder Ausgabe separat anzeigen und mühelos dazwischen umschalten.

Sobald Sie mit den im Browser angezeigten Ergebnissen zufrieden sind, können Sie Ihre Abfrage speichern, Ihren Auftrag starten und ihn Ereignisse fehlerfrei verarbeiten lassen.

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
