---
title: Testen eines Azure Stream Analytics-Auftrags mit Beispieldaten
description: Informationen zum Testen von Abfragen in Stream Analytics-Aufträgen.
keywords: In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure-Portals einen Azure Stream Analytics-Auftrag mit Beispieleingabedaten testen und Beispieldaten hochladen.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/27/2018
ms.openlocfilehash: 3dc9091934f3db8ededc13f74d2f302eccace4d6
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2018
ms.locfileid: "32312989"
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Testen einer Stream Analytics-Abfrage mit Beispieldaten

Mit Azure Stream Analytics können Sie Beispieldaten und Testabfragen in das Azure-Portal hochladen, ohne einen Auftrag starten oder beenden zu müssen.

## <a name="upload-sample-data-and-test-the-query"></a>Hochladen von Beispieldaten und Testen der Abfrage

1. Melden Sie sich beim Azure-Portal an. 

2. Suchen Sie Ihren vorhandenen Stream Analytics-Auftrag, und wählen Sie ihn aus.

3. Wählen Sie auf der Stream Analytics-Auftragsseite unter der Überschrift **Auftragstopologie** die Option **Abfrage** aus, um das Fenster des Abfrage-Editors zu öffnen. 

4. Um Ihre Abfrage mit Beispieleingabedaten zu testen, klicken Sie mit der rechten Maustaste auf eine Ihrer Eingaben.  Wählen Sie dann **Beispieldaten aus Datei hochladen** aus.

   Bei den Daten darf es sich nur um JSON-formatierte Daten handeln. Wenn Ihre Daten in einem anderen Format als CSV vorliegen, müssen Sie sie vor dem Hochladen in JSON konvertieren. Sie können ein Open-Source-Konvertierungstool wie [CSV to JSON Converter](http://www.convertcsv.com/csv-to-json.htm) verwenden, um Ihre Daten in JSON zu konvertieren.

    ![Testen von Abfragen im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. Wählen Sie nach Abschluss des Uploadvorgangs die Option **Testen** aus, um diese Abfrage mit den Beispieldaten zu testen, die Sie bereitgestellt haben.

    ![Testen von Beispieldaten im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. Wenn Sie die Testausgabe zur späteren Verwendung benötigen, wird die Ausgabe der Abfrage im Browser mit einem Link zu den Downloadergebnissen angezeigt. 

7. Ändern Sie Ihre Abfrage iterativ, und testen Sie sie erneut, um festzustellen, wie sich die Ausgabe ändert.

   ![Beispielausgabe im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   Wenn Sie in einer Abfrage mehrere Ausgaben verwenden, werden die Ergebnisse auf separaten Registerkarten angezeigt, und Sie können problemlos zwischen ihnen wechseln.

8. **Speichern** Sie Ihre Abfrage, nachdem Sie die im Browser angezeigten Ergebnisse überprüft haben. **Starten** Sie dann den Auftrag, und lassen Sie ihn die eingehenden Ereignisse verarbeiten.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
