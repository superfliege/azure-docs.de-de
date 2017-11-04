---
title: "Verwenden der Azure Stream Analytics-Tools für Visual Studio | Microsoft-Dokumentation"
description: "Tutorial mit ersten Schritten für die Azure Stream Analytics-Tools für Visual Studio"
keywords: Visual Studio
documentationcenter: 
services: stream-analytics
author: su-jie
manager: jhubbard
editor: cgronlun
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: sujie
ms.openlocfilehash: 8e3f1ae6739896dfd1329561dbcede38a6069546
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a>Verwenden der Azure Stream Analytics-Tools für Visual Studio
Azure Stream Analytics-Tools für Visual Studio sind jetzt allgemein verfügbar. Diese Tools bieten umfangreichere Funktionen für Stream Analytics-Benutzer beim Beheben von Problemen sowie beim Verfassen komplexer Abfragen und sogar beim lokalen Schreiben von Abfragen. Sie können auch einen Stream Analytics-Auftrag in ein Visual Studio-Projekt exportieren.

## <a name="introduction"></a>Einführung
In diesem Tutorial erfahren Sie, wie Sie mit Stream Analytics-Tools für Visual Studio Ihre Stream Analytics-Aufträge erstellen, verfassen, lokal testen, verwalten und debuggen. 

Nach Abschluss dieses Tutorials, werden Sie in der Lage sein, folgende Aufgaben auszuführen:

* Machen Sie sich mit den Stream Analytics-Tools für Visual Studio vertraut.
* Konfigurieren und Bereitstellen eines Stream Analytics-Auftrags
* Lokales Testen Ihres Auftrags mit lokalen Beispieldaten
* Verwenden der Überwachung zur Behebung von Problemen
* Exportieren vorhandener Aufträge in Projekte

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Führen Sie die Schritte bis „Erstellen eines Stream Analytics-Auftrags“ im Tutorial [Erstellen einer IoT-Lösung mithilfe von Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics) aus. 
* Installieren Sie Visual Studio 2017, Visual Studio 2015 oder Visual Studio 2013 Update 4. Die Editionen Enterprise (Ultimate/Premium), Professional und Community werden unterstützt. Die Express Edition wird nicht unterstützt. 
* Befolgen Sie die [Installationsanweisungen](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-tools-for-visual-studio-install) für die Stream Analytics-Tools für Visual Studio.

## <a name="create-a-stream-analytics-project"></a>Erstellen eines Stream Analytics-Projekts
Wählen Sie in Visual Studio **Datei** > **Neues Projekt** aus. Wählen Sie in der Vorlagenliste links **Stream Analytics** und dann **Azure Stream Analytics Application** (Azure Stream Analytics-Anwendung) aus.
Geben Sie am unteren Seitenrand den **Projektnamen**, **Speicherort** und **Lösungsnamen** wie bei anderen Projekten ein.

![Erstellung eines neuen Projekts](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

Das Projekt **Toll** wird im **Projektmappen-Explorer** generiert.

![Projekt „Toll“ im Projektmappen-Explorer](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-the-correct-subscription"></a>Auswählen des richtigen Abonnements
1. Wählen Sie in Visual Studio im Menü **Ansicht** den **Server-Explorer** aus.

2. Melden Sie sich bei Ihrem Azure-Konto an. 

## <a name="define-input-sources"></a>Definieren von Eingabequellen
1. Erweitern Sie im **Projektmappen-Explorer** den Knoten **Eingaben**, und benennen Sie **Input.json** in **EntryStream.json** um. Doppelklicken Sie auf **EntryStream.json**.

2. Geben Sie **EntryStream** für **Eingabealias** ein. Beachten Sie, dass der Eingabealias im Abfrageskript verwendet wird.

3. Wählen Sie als **Quelltyp** die Option **Datenstrom** aus.

4. Wählen Sie für **Quelle** die Option **Event Hub** aus.

5. Wählen Sie für **Service Bus-Namespace** in der Dropdownliste die Option **TollData** aus.

6. Wählen Sie für **Event Hub-Name** die Option **entry** aus.

7. Wählen Sie für **Event Hub-Richtlinienname** die Option **RootManageSharedAccessKey** (Standardwert) aus.

8. Wählen Sie für **Ereignisserialisierungsformat** die Option **JSON** und für **Codierung** die Option **UTF8** aus.
   
   Ihre Einstellungen sehen folgendermaßen aus:
   
   ![Eingabeeinstellungen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
   
9. Wählen Sie unten auf der Seite **Speichern** aus, um den Assistenten zu beenden. Sie können jetzt eine andere Eingabequelle zum Erstellen des Ausgangsdatenstroms hinzufügen. Klicken Sie mit der rechten Maustaste auf den Knoten **Eingaben**, und wählen Sie **Neues Element**.
   
   ![Neues Element](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
   
10. Wählen Sie im Popupfenster **Stream Analytics Input** (Stream Analytics-Eingabe) aus, und ändern Sie den **Namen** in **ExitStream.json**. Wählen Sie **Hinzufügen**.
   
    ![Neues Element hinzufügen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
   
11. Doppelklicken Sie im Projekt auf **ExitStream.json**, und führen Sie dieselben Schritte wie im Eingabedatenstrom aus, um die Felder auszufüllen. Achten Sie darauf, dass Sie für **Event Hub-Name** den Text **exit** eingeben, wie im folgenden Screenshot dargestellt:
   
    ![ExitStream-Einstellungen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)
   
   Sie haben jetzt zwei Eingabedatenströme definiert:
   
   ![Zwei Eingabedatenströme](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
   
   Als Nächstes fügen Sie die Referenzdateneingabe für die Blobdatei hinzu, in der die Fahrzeugregistrierungsdaten enthalten sind.
   
12. Klicken Sie mit der rechten Maustaste auf den Knoten **Eingaben** des Projekts, und führen Sie dann denselben Prozess wie für die Datenstromeingaben aus. Wählen Sie für **Quelltyp** den Text **Referenzdaten** und für **Eingabealias** die Option **Registrierung** aus.
   
    ![Registrierungseinstellungen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)
   
13. Wählen Sie das **Speicherkonto** aus, das die Option mit **TollData** enthält. Der Name des Containers ist **TollData** und das **Pfadmuster** lautet **registration.json**. Für diesen Dateinamen wird die Groß-/Kleinschreibung berücksichtigt, und er sollte nur Kleinbuchstaben enthalten.

14. Wählen Sie **Speichern** aus, um den Assistenten zu beenden.

Jetzt sind alle Eingaben definiert.

## <a name="define-output"></a>Definieren der Ausgabe
1. Erweitern Sie im **Projektmappen-Explorer** den Knoten **Eingaben**, und doppelklicken Sie auf **Output.json**.

2. Geben Sie für **Ausgabealias** den Text **output** ein. Wählen Sie für **Senke** die Option **SQL-Datenbank** aus.

3. Geben Sie für den Namen der **Datenbank** den Text **TollDataDB** ein.

4. Geben Sie für **Benutzername** den Text **tolladmin** ein. Geben Sie für **Kennwort** den Text **123toll!** ein. Geben Sie für **Tabelle** den Text **TollDataRefJoin** ein.

5. Wählen Sie **Speichern** aus.

   ![Ausgabeeinstellungen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="stream-analytics-query"></a>Stream Analytics-Abfrage
In diesem Tutorial wird versucht, einige geschäftliche Fragen in Bezug auf Mautdaten zu beantworten. Wir haben Abfragen erstellt, die in Stream Analytics verwendet werden können, um relevante Antworten zu liefern. Bevor wir mit dem ersten Stream Analytics-Auftrag beginnen, sehen wir uns ein einfaches Szenario und die Abfragesyntax an.

### <a name="introduction-to-stream-analytics-query-language"></a>Einführung in die Stream Analytics-Abfragesprache
Angenommen, Sie müssen die Fahrzeuge zählen, die ein Mauthäuschen passieren. Da es sich um einen kontinuierlichen Datenstrom von Ereignissen handelt, müssen Sie einen bestimmten Zeitraum definieren. Wir formulieren die Frage also neu: „Wie viele Fahrzeuge passieren ein Mauthäuschen jeweils innerhalb von drei Minuten?“ Diese Messung wird für gewöhnlich als „Rollierende Anzahl“ bezeichnet.

Betrachten wir die Beantwortung dieser Frage durch die Stream Analytics-Abfrage:

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

Wie Sie sehen können, verwendet Stream Analytics eine Abfragesprache, die SQL ähnelt. Es fügt einige Erweiterungen zur Angabe zeitbezogener Aspekte der Abfrage hinzu.

Weitere Informationen finden Sie in den Artikeln zu [Zeitmanagement](https://msdn.microsoft.com/library/azure/mt582045.aspx)- und [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx)-Konstrukten, die in der Abfrage von MSDN verwendet werden.

Da Sie nun Ihre erste Stream Analytics-Abfrage geschrieben haben, testen Sie sie mithilfe der Beispieldatendateien, die in Ihrem TollApp-Ordner unter dem folgendem Pfad vorhanden sind:

**..\TollApp\TollApp\Data**

Dieser Ordner enthält die folgenden Dateien:

* Entry.json
* Exit.json
* registration.json

## <a name="question-number-of-vehicles-entering-a-toll-booth"></a>Frage: Wie viele Fahrzeuge passieren ein Mauthäuschen?
Doppelklicken Sie im Projekt auf **Script.asaql**, um das Skript im Editor zu öffnen. Fügen Sie das Skript aus dem vorherigen Abschnitt in den Editor ein. Der Abfrage-Editor unterstützt IntelliSense, farbige Syntaxmarkierung und einen Fehlermarker.

![Abfrage-Editor](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a>Lokales Testen von Stream Analytics-Abfragen
Sie können zunächst die Abfrage kompilieren, um festzustellen, ob ein Syntaxfehler aufgetreten ist.

1. Um diese Abfrage anhand von Beispieldaten zu überprüfen, verwenden Sie lokale Beispieldaten, indem Sie mit der rechten Maustaste auf die Eingabe klicken und **Lokale Eingabe hinzufügen** auswählen.
   
   ![Lokale Eingabe hinzufügen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
   
2. Wählen Sie im Popupfenster die Beispieldaten in Ihrem lokalen Pfad aus. Wählen Sie **Speichern** aus.
   
   ![Lokale Eingabe hinzufügen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
   
   Eine Datei namens **local_EntryStream.json** wird automatisch Ihrem Ordner für Eingaben hinzugefügt.
   
   ![Dateiliste für lokalen Eingabeordner](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
   
3. Wählen Sie im Abfrage-Editor die Option **Lokal ausführen** aus. Oder drücken Sie F5.
   
   ![Lokal ausführen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)
   
   Sie können den Ausgabepfad über die Konsolenausgabe finden. Drücken Sie eine beliebige Taste, um den Ergebnisordner zu öffnen.
   
   ![Lokaler Testlauf](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)
   
4. Überprüfen Sie die Ergebnisse im lokalen Ordner.
   
   ![Ergebnis des lokalen Ordners](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-03.png)
   
   
### <a name="sample-input"></a>Beispieleingabe
Sie können auch Stichproben von Eingabedaten aus Eingabequellen nehmen und in der lokalen Datei speichern. Klicken Sie mit der rechten Maustaste auf die Eingabekonfigurationsdatei, und wählen Sie **Beispieldaten** aus. 

![Beispieldaten](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

Beachten Sie, dass Sie derzeit nur Stichproben für Event Hubs oder IoT Hubs nehmen können. Andere Eingabequellen werden nicht unterstützt. Geben Sie im Dialogfeld den lokalen Pfad ein, der zum Speichern von Beispieldaten verwendet wird. Wählen Sie **Beispiel** aus.

![Beispieldatenkonfiguration](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
Sie können den Status im Fenster **Ausgabe** anzeigen. 

![Beispieldatenausgabe](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-to-azure"></a>Übermitteln einer Stream Analytics-Abfrage an Azure
1. Wählen Sie im **Abfrage-Editor** im Skript-Editor die Option **An Azure übermitteln** aus.

   ![An Azure übermitteln](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. Wählen Sie die Option **Neuen Azure Stream Analytics-Auftrag erstellen**. Geben Sie für **Auftragsname** den Text **TollApp** ein. Wählen Sie in der Dropdownliste das richtige **Abonnement** aus. Klicken Sie auf **Submit** (Senden).

   ![Auftrag übermitteln](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-the-job"></a>Starten des Auftrags
Jetzt wird Ihr Auftrag erstellt und die Auftragsansicht automatisch geöffnet. 
1. Wählen Sie zum Starten des Auftrags die Schaltfläche mit dem grünen Pfeil aus.

   ![Schaltfläche „Auftrag starten“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. Wählen Sie die Standardeinstellung und dann **Start** aus.
 
   ![Auftrag starten](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

   Sie sehen, dass der Status des Auftrags in **Ausführen** geändert wurde, und Eingabe-/Ausgabeereignisse vorhanden sind.

   ![Auftragszusammenfassung und Metriken](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-results-in-visual-studio"></a>Überprüfen Sie die Ergebnisse in Visual Studio
1. Öffnen Sie Visual Studio Server-Explorer, und klicken Sie mit der rechten Maustaste auf die Tabelle **TollDataRefJoin** .

2. Wählen Sie die Option **Tabellendaten anzeigen**, um die Ausgabe des Auftrags anzuzeigen.
   
   ![Tabellendaten anzeigen](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)
   

### <a name="view-job-metrics"></a>Anzeigen von Auftragsmetriken
Einige grundlegende Auftragsstatistiken werden in **Auftragsmetriken** angezeigt. 

![Auftragsmetriken](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-the-job-in-server-explorer"></a>Auflisten des Auftrags im Server-Explorer
Wählen Sie im **Server-Explorer** die Option **Stream Analytics-Aufträge** und dann **Aktualisieren** aus. Ihr Auftrag wird unter **Stream Analytics-Aufträge** angezeigt.

![Auftragsliste](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-the-job-view"></a>Öffnen der Auftragsansicht
Erweitern Sie den Auftragsknoten, und doppelklicken Sie auf den Knoten **Auftragsansicht**, um eine Auftragsansicht zu öffnen.

![Die Auftragsansicht](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-to-a-project"></a>Exportieren eines vorhandenen Auftrags in ein Projekt
Es gibt zwei Möglichkeiten zum Exportieren eines vorhandenen Auftrags in ein Projekt.
* Klicken Sie in **Server-Explorer** unter **Stream Analytics-Aufträge** mit der rechten Maustaste auf den Auftragsknoten. Wählen Sie **In neues Stream Analytics-Projekt exportieren** aus.
   
   ![In neues Stream Analytics-Projekt exportieren](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)
   
   Das erstellte Projekt wird im **Projektmappen-Explorer** angezeigt.
   
    ![Projektmappen-Explorer-Auftrag](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
   
* Wählen Sie in der Auftragsansicht **Projekt generieren** aus.
   
   ![Projekt generieren](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)
   
## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen
 
* Lokale Tests funktionieren nicht, wenn Ihre Abfrage geografische Funktionen enthält.
* Beim Hinzufügen oder Ändern von JavaScript-UDF ist keine Editor-Unterstützung verfügbar.
* Lokale Tests unterstützen das Speichern der Ausgabe im JSON-Format nicht. 
* Für Power BI- und ADLS-Ausgaben ist keine Unterstützung verfügbar.



## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics: Betrugserkennung in Echtzeit](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referenz zur Stream Analytics-Abfragesprache)
* [Azure Stream Analytics management REST API reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) (Referenz zur Azure Stream Analytics-Verwaltungs-REST-API)


