---
title: Testen von Azure Stream Analytics-Abfragen | Microsoft-Dokumentation
description: "Informationen zum Testen von Abfragen in Stream Analytics-Aufträgen."
keywords: Testen von Abfragen, Problembehandlung bei Abfragen
documentation center: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: 2898e3404dcfa3d75e3920f9c83e4efa7201998e
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2018
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

Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
