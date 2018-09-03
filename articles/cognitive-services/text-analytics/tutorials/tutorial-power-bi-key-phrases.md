---
title: Übersicht über die Textanalyse mit Power BI – Azure Cognitive Services | Microsoft-Dokumentation
description: Hier erfahren Sie, wie die Textanalyse zum Extrahieren von Schlüsselbegriffen aus in Power BI gespeichertem Text verwendet wird.
services: cognitive-services
author: luiscabrer
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: tutorial
ms.date: 3/07/2018
ms.author: luisca
ms.openlocfilehash: 2cdb93d44218627efdcb0360d8cf4a4eeeca177a
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2018
ms.locfileid: "42889271"
---
# <a name="text-analytics-with-power-bi"></a>Textanalyse mit Power BI

Microsoft Power BI fasst Organisationsdaten in ansprechenden Berichten zusammen, die Sie in Ihrer Organisation verteilen und damit schnellere, umfassendere Einblicke bereitstellen können. Der Textanalysedienst ist Teil von Cognitive Services in Microsoft Azure und kann über seine Schlüsselbegriffs-API die wichtigsten Begriffe aus Text extrahieren. Zusammen bieten diese Tools Ihnen einen schnellen Überblick über die Gesprächsthemen Ihrer Kunden und ihr Empfinden darüber.

In diesem Tutorial sehen Sie, wie Power BI Desktop und die Schlüsselbegriffs-API integriert werden, um mithilfe einer benutzerdefinierten Power Query-Funktion die wichtigsten Begriffe aus Kundenfeedback zu extrahieren. Außerdem erstellen wir eine Wortwolke aus diesen Begriffen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

> [!div class="checklist"]
> * Microsoft Power BI Desktop. [Kostenlos herunterladen](https://powerbi.microsoft.com/get-started/).
> * Ein Microsoft Azure-Konto. [Starten Sie eine kostenlose Testversion](https://azure.microsoft.com/free/), oder [melden Sie sich an](https://portal.azure.com/).
> * Einen Zugriffsschlüssel für Textanalyse. [Registrieren Sie sich](../../cognitive-services-apis-create-account.md), und [rufen Sie dann Ihren Schlüssel ab](../how-tos/text-analytics-how-to-access-key.md).
> * Kundenkommentare. [Rufen Sie unsere Beispieldaten ab](https://aka.ms/cogsvc/ta), oder verwenden Sie Ihre eigenen.

## <a name="loading-customer-data"></a>Laden von Kundendaten

Öffnen Sie zum Einstieg Power BI Desktop, und laden Sie die CSV-Datei **FabrikamComments.csv**. Diese Datei stellt die hypothetische Aktivität eines Tages im Supportforum eines kleinen fiktiven Unternehmen dar.

> [!NOTE]
> Power BI kann Daten aus einer Vielzahl von Quellen verarbeiten, z.B. Facebook oder einer SQL-Datenbank. Weitere Informationen dazu finden Sie unter [Facebook-Integration](https://powerbi.microsoft.com/integrations/facebook/) und [SQL Server-Integration](https://powerbi.microsoft.com/integrations/sql-server/).

Suchen Sie im Hauptfenster von Power BI Desktop der Gruppe „Externe Daten“ auf dem Menüband „Start“. Wählen Sie im Dropdownmenü **Daten abrufen** in dieser Gruppe die Option **Text/CSV**.

![[Die Schaltfläche „Daten abrufen“]](../media/tutorials/power-bi/get-data-button.png)

Das Dialogfeld „Öffnen“ wird angezeigt. Navigieren Sie zum Ordner „Downloads“ bzw. zum Ordner, der die Datei mit den Beispieldaten enthält. Klicken Sie auf `FabrikamComments.csv`, dann auf die Schaltfläche **Öffnen**. Das Dialogfeld „CSV-Dateien importieren“ wird angezeigt.

![[Das Dialogfeld „CSV-Dateien importieren“]](../media/tutorials/power-bi/csv-import.png)

Im Dialogfeld „CSV-Dateien importieren“ können sicherstellen, dass Power BI Desktop den Zeichensatz, Trennzeichen, Kopfzeilen und Spaltentypen richtig erkannt hat. Alle Informationen sind korrekt, also klicken wir auf **Laden**.

Zum Anzeigen der geladenen Daten wechseln Sie mit der Schaltfläche „Datenansicht“ am linken Rand des Power BI-Arbeitsbereichs in die Datenansicht. Eine Tabelle mit unseren Daten wird geöffnet, wie in Microsoft Excel.

![[Die anfängliche Ansicht der importierten Daten]](../media/tutorials/power-bi/initial-data-view.png)

## <a name="preparing-the-data"></a>Vorbereiten der Daten

Möglicherweise müssen Sie Ihre Daten in Power BI Desktop transformieren, bevor sie vom Schlüsselbegriffsdienst verarbeitet werden können.

Unsere Daten enthalten z.B. sowohl ein `subject`-Feld als auch ein `comment`-Feld. Beim Extrahieren von Schlüsselbegriffen müssen wir den Text in beiden Feldern berücksichtigen, nicht nur in `comment`. Die Funktion „Spalten zusammenführen“ in Power BI Desktop erleichtert diese Aufgabe.

Öffnen Sie den Abfrage-Editor im Hauptfenster von Power BI Desktop, indem Sie in der Gruppe „Externe Daten“ im Menüband „Start“ auf **Abfragen bearbeiten** klicken. 

![[Die Gruppe „Externe Daten“ im Menüband „Start“]](../media/tutorials/power-bi/edit-queries.png)

Wählen Sie in der Liste der Abfragen auf der linken Seite des Fensters die Option „FabrikamComments“ aus, falls sie nicht bereits ausgewählt ist.

Wählen Sie jetzt in der Tabelle die Spalten `subject` und `comment` aus. Möglicherweise müssen Sie einen horizontalen Bildlauf durchführen, um diese Spalten anzuzeigen. Klicken Sie zuerst auf die Spaltenüberschrift `subject`, halten Sie die STRG-Taste gedrückt, und klicken Sie dann auf die Spaltenüberschrift `comment`.

![[Auswählen von Feldern, die zusammengeführt werden sollen]](../media/tutorials/power-bi/select-columns.png)

Klicken Sie in der Gruppe „Textspalten“ des Menüband „Transformation“ auf **Spalten zusammenführen**. Das Dialogfeld „Spalten zusammenführen“ wird angezeigt.

![[Zusammenführen von Feldern über das Dialogfeld „Spalten zusammenführen“]](../media/tutorials/power-bi/merge-columns.png)

Wählen Sie im Dialogfeld „Spalten zusammenführen“ die Option „Registerkarte“ als Trennlinie aus, und klicken Sie dann auf **OK.** Das war's!

Sie können auch den Filter „Leere entfernen“ verwenden, um leere Nachrichten herauszufiltern, oder die Transformation „Bereinigen“, um nicht druckbare Zeichen zu entfernen. Wenn Ihre Daten eine Spalte wie die Spalte `spamscore` in unserer Beispieldatei enthalten, können Sie Spam-Kommentare mithilfe eines Anzahlfilters überspringen.

## <a name="understanding-the-api"></a>Grundlegendes zur API

Die Schlüsselbegriffs-API kann bis zu tausend Textdokumente pro HTTP-Anforderung verarbeiten. Power BI bevorzugt jedoch die Verarbeitung einzelner Datensätze, damit unsere Aufrufe an die API jeweils nur ein Dokument enthalten. [Die API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) erfordert die folgenden Felder für jedes verarbeitete Dokument.

| | |
| - | - |
| `id`  | Ein eindeutiger Bezeichner für dieses Dokument in der Anforderung. Dieses Feld ist auch in der Antwort enthalten. So können Sie bei der Verarbeitung mehrerer Dokumente die extrahierten Schlüsselbegriffe problemlos dem Dokument zuordnen, aus dem sie stammen. Wir verarbeiten ein Dokument pro Anforderung, daher wissen wir bereits, zu welchem Dokument die Antwort gehört, und `id` kann in jeder Anforderung gleich sein.|
| `text`  | Der zu verarbeitende Text. Dieser befindet sich in der von uns erstellten Spalte `Merged`, die den kombinierten Betreffzeilen- und Kommentartext enthält. Die Schlüsselbegriffs-API erfordert, dass diese Daten höchstens ca. 5000 Zeichen umfassen.|
| `language` | Code zur Darstellung der Sprache, in der das Dokument geschrieben ist. Alle unsere Nachrichten sind auf Englisch verfasst, also können wir `en` als vordefinierten Code in die Abfrage aufnehmen.|

Wir müssen diese Felder in einem JSON-Dokument (JavaScript Object Notation) für die Übermittlung an die Schlüsselbegriffs-API kombinieren. Die Antwort auf diese Anforderung ist auch ein JSON-Dokument, das wir analysieren und die Schlüsselbegriffe auslesen müssen.

## <a name="creating-a-custom-function"></a>Erstellen einer benutzerdefinierten Funktion

Nun sind bereit zum Erstellen der benutzerdefinierte Funktion, die Power BI und Textanalyse integriert. Power BI Desktop ruft die Funktion für jede Zeile der Tabelle auf und erstellt eine neue Spalte mit den Ergebnissen.

Die Funktion erhält den zu verarbeitenden Text als Parameter. Sie konvertiert Daten in und aus der erforderlichen JSON-Datei (JavaScript Object Notation) und stellt die HTTP-Anforderung an den Endpunkt der Schlüsselbegriffs-API. Nach der Analyse der Antwort gibt die Funktion eine Zeichenfolge mit einer durch Trennzeichen getrennte Liste der extrahierten Schlüsselbegriffe zurück.

> [!NOTE]
> Benutzerdefinierte Power BI Desktop-Funktionen sind in der [Power Query-Formelsprache „M“](https://msdn.microsoft.com/library/mt211003.aspx) oder kurz „M“ geschrieben. M ist eine funktionale Programmiersprache, die auf [F#](https://docs.microsoft.com/dotnet/fsharp/) basiert. Sie müssen jedoch kein Programmierer sein, um dieses Tutorial abzuschließen; der erforderliche Code ist unten angegeben.

Sie sollten sich noch im Fenster „Abfrage-Editor“ befinden. Klicken Sie auf dem Menüband „Start“ auf **Neue Quelle** (in der Gruppe „Neue Abfrage“), und wählen Sie dann im Dropdownmenü die Option **Leere Abfrage** aus. 

Der Abfrageliste wird eine neue Abfrage mit dem Standardnamen „Query1“ hinzugefügt. Doppelklicken Sie auf diesen Eintrag, und nennen Sie ihn `KeyPhrases`.

Öffnen Sie jetzt das Fenster „Erweiterter Editor“, indem Sie in der Gruppe „Abfrage“ des Menübands „Start“ auf **Erweiterter Editor** klicken. Löschen Sie den Code, der sich bereits in diesem Fenster befindet, und fügen Sie den folgenden Code ein. 

> [!NOTE]
> In den folgenden Beispielen wird davon ausgegangen, dass der Endpunkt https://westus.api.cognitive.microsoft.com ist.  Die Textanalyse unterstützt die Erstellung eines Abonnements in 13 verschiedenen Regionen. Wenn Sie sich in einer anderen Region für den Dienst registriert haben, achten Sie darauf, den Endpunkt für die von Ihnen ausgewählte Region zu verwenden. Er sollte auf der Übersichtsseite im Azure-Portal angezeigt werden, wenn Sie Ihr Textanalyse-Abonnement auswählen.

```fsharp
// Returns key phrases from the text in a comma-separated list
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = Text.Lower(Text.Combine(jsonresp[documents]{0}[keyPhrases], ", "))
in  keyphrases
```

Fügen Sie auch Ihren Textanalyse-API-Schlüssel, den Sie aus dem Microsoft Azure-Dashboard abgerufen haben, in der Zeile ein, die mit `apikey` beginnt. (Achten Sie darauf, die Anführungszeichen vor und nach dem Schlüssel beizubehalten.) Klicken Sie anschließend auf **Fertig**.

## <a name="using-the-function"></a>Verwenden der Funktion

Jetzt können wir die benutzerdefinierte Funktion verwenden, um die Schlüsselbegriffe aus allen unseren Kundenkommentaren abzurufen und in einer neuen Spalte in der Tabelle zu speichern. 

Bleiben Sie weiterhin im Abfrage-Editor, und wechseln Sie zurück zur Abfrage „FabrikamComments“. Klicken Sie auf dem Menüband „Spalte hinzufügen“ in der Gruppe „Allgemein“ auf die Schaltfläche **Benutzerdefinierte Funktion aufrufen**.

![[Schaltfläche „Benutzerdefinierte Funktion aufrufen“]](../media/tutorials/power-bi/invoke-custom-function-button.png)<br><br>

Geben Sie im Dialogfeld „Benutzerdefinierte Funktion aufrufen“ `keyphrases` als Name der neuen Spalte ein. Wählen Sie unsere benutzerdefinierte Funktion „`KeyPhrases`“ als Funktionsabfrage. 

Im Dialogfeld wird ein neues Feld angezeigt, in dem wir auswählen müssen, welches Feld wir unserer Funktion als `text`-Parameter übergeben möchten. Wählen Sie im Dropdownmenü `Merged` aus (die Spalte, die wir zuvor durch Zusammenführen der Betreffs- und Nachrichtenfelder erstellt haben).

![[Aufrufen einer benutzerdefinierte Funktion]](../media/tutorials/power-bi/invoke-custom-function.png)

Klicken Sie abschließend auf **OK**.

Wenn alles bereit ist, ruft Power BI unsere Funktion einmal für jede Zeile in der Tabelle auf, führt die Schlüsselbegriffabfragen durch und fügt der Tabelle die neue Spalte hinzu. Zuvor müssen Sie jedoch möglicherweise Authentifizierungs- und Datenschutzeinstellungen festlegen.

## <a name="authentication-and-privacy"></a>Authentifizierung und Datenschutz

Nachdem Sie das Dialogfeld „Benutzerdefinierte Funktion aufrufen“ geschlossen haben, wird u.U. ein Banner angezeigt, der Sie auffordert, festzulegen, wie die Verbindung mit dem Schlüsselbegriff-Endpunkt hergestellt wird.

![[Banner zu Anmeldeinformationen]](../media/tutorials/power-bi/credentials-banner.png)

Klicken Sie auf **Anmeldeinformationen bearbeiten**, stellen Sie sicher, dass „Anonym“ im Dialogfeld ausgewählt ist, und klicken Sie dann auf **Verbinden**. 

> [!NOTE]
> Warum anonym? Der Textanalysedienst authentifiziert Sie über den API-Schlüssel, damit Power BI für die HTTP-Anforderung selbst keine Anmeldeinformationen angeben muss.

![[Festlegen der Authentifizierung auf „Anonym“]](../media/tutorials/power-bi/access-web-content.png)

Wenn der Banner „Anmeldeinformationen bearbeiten“ nach der Auswahl des anonymen Zugriffs weiter angezeigt wird, haben Sie möglicherweise vergessen, Ihren API-Schlüssel einzufügen. Überprüfen Sie die benutzerdefinierten Funktion `KeyPhrases` im erweiterten Editor, um sicherzustellen, dass sie vorhanden ist.

Als Nächstes kann ein Banner angezeigt werden, der Sie auffordert, Informationen zum Datenschutz Ihrer Datenquellen anzugeben. 

![[Datenschutz-Banner]](../media/tutorials/power-bi/privacy-banner.png)

Klicken Sie auf **Weiter**, und wählen Sie für jede Datenquellen im Dialogfeld die Option „Öffentlich“ aus. Klicken Sie anschließend auf **Speichern**.

![[Festlegen des Datenschutzes der Datenquelle]](../media/tutorials/power-bi/privacy-dialog.png)

## <a name="creating-the-word-cloud"></a>Erstellen der Wortwolke

Wenn Sie mit allen angezeigten Bannern fertig sind, klicken Sie auf dem Menüband „Start“ auf **Schließen und übernehmen**, um den Abfrage-Editor zu schließen.

Power BI Desktop braucht einen Moment, um die erforderlichen HTTP-Anforderungen zu stellen. Für jede Zeile in der Tabelle enthält die neue Spalte `keyphrases` die von der Schlüsselbegriffs-API im Text erkannten Schlüsselbegriffe. 

Wir verwenden diese Spalte, um eine Wortwolke zu generieren. Als erstes klicken Sie im Hauptfenster von Power BI Desktop links vom Arbeitsbereich auf die Schaltfläche „Bericht“.

> [!NOTE]
> Warum werden Schlüsselbegriffe zum Erstellen einer Wortwolke verwendet, anstatt des vollständigen Texts aller Kommentare? Die Schlüsselbegriffe enthalten die *wichtigen* Wörter aus unseren Kundenkommentaren, nicht nur die *am häufigsten verwendeten* Wörter. Außerdem ist die Wortgrößenanpassung in der Wolke dann nicht durch die häufige Verwendung eines Worts in einer relativ geringen Anzahl von Kommentaren verzerrt.

Installieren Sie das benutzerdefinierte visuelle Objekt „Wortwolke“, wenn es noch nicht installiert ist. Klicken Sie im Bereich „Visualisierungen“ rechts neben dem Arbeitsbereich auf die drei Punkte (**...** ), und wählen Sie **Import from Store** (Aus Store importieren). Suchen Sie nach „Wolke“, und klicken Sie dann neben dem visuellen Element „Wortwolke“ auf **Hinzufügen**. Power BI installiert das visuelle Element „Wortwolke“ und weist Sie auf eine erfolgreiche Installation hin.

![[Hinzufügen eines benutzerdefinierten visuellen Elements]](../media/tutorials/power-bi/add-custom-visuals.png)<br><br>

Jetzt erstellen wir unsere Wortwolke!

![[Wortwolkensymbol im Visualisierungsbereich]](../media/tutorials/power-bi/visualizations-panel.png)

Klicken Sie zunächst im Visualisierungsbereich auf das Wortwolkensymbol. Im Arbeitsbereich wird ein neuer Bericht angezeigt. Ziehen Sie das Feld `keyphrases` aus den Bereich „Felder“ in das Feld „Kategorie“ im Visualisierungsbereich. Die Wortwolke wird innerhalb des Berichts angezeigt.

Wechseln Sie nun zur Seite „Format“ des Visualisierungsbereichs. Aktivieren Sie in der Kategorie „Stoppwörter“ die Option **Standardstoppwörter**, um kurze häufige Wörter wie „von“ aus der Wolke auszuschließen. 

![[Aktivieren von Standardstoppwörtern]](../media/tutorials/power-bi/default-stop-words.png)

Deaktivieren Sie etwas weiter unten in diesem Bereich die Optionen **Text drehen** und **Titel**.

![[Aktivieren des Fokusmodus]](../media/tutorials/power-bi/word-cloud-focus-mode.png)

Klicken Sie im Bericht auf das Fokusmodus-Tool aus, um einen besseren Blick auf unsere Wortwolke zu erhalten. Das Tool erweitert die Wortwolke auf den gesamten Arbeitsbereich, wie unten dargestellt.

![[Eine Wortwolke]](../media/tutorials/power-bi/word-cloud.png)

## <a name="more-text-analytics-services"></a>Weitere Textanalysedienste

Der Textanalysedienst, welcher im Rahmen von Cognitive Services von Microsoft angeboten wird, stellt auch eine Standpunktanalyse und Sprachenerkennung bereit. Die Spracherkennung ist insbesondere nützlich, wenn nicht Ihr gesamtes Kundenfeedback in englischer Sprache verfasst ist.

Beide dieser APIs sind der Schlüsselbegriffs-API sehr ähnlich. Daher können nahezu identische benutzerdefinierte Funktionen verwendet werden, um sie in Power BI Desktop zu integrieren. Dazu erstellen Sie einfach eine leere Abfrage und fügen wie zuvor den entsprechenden Code unten in den erweiterten Editor ein. (Vergessen Sie Ihren Zugriffsschlüssel nicht!) Verwenden Sie dann wie zuvor die Funktion, um der Tabelle eine Spalte hinzuzufügen.

Die unten dargestellte Standpunktanalysefunktion gibt ein Ergebnis zurück, das angibt, wie positiv die im Text ausgedrückte Stimmung ist.

```fsharp
// Returns the sentiment score of the text, from 0.0 (least favorable) to 1.0 (most favorable)
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    sentiment   = jsonresp[documents]{0}[score]
in  sentiment
```

Hier sind zwei Versionen einer Sprachenerkennungsfunktion. Die erste gibt den ISO-Sprachencode (z.B. `en` für Englisch) zurück, während die zweite den Anzeigenamen (z. B. `English`) zurückgibt. Sie stellen möglicherweise fest, dass sich zwischen den beiden Versionen nur die letzte Zeile des Texts unterscheidet.

```fsharp
// Returns the two-letter language code (e.g. en for English) of the text
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[iso6391Name]
in  language
```
```fsharp
// Returns the name (e.g. English) of the language in which the text is written
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[name]
in  language
```

Und hier ist schließlich eine Variante der bereits vorgestellten Schlüsselbegriffsfunktion, die die Begriffe als Listenobjekt anstatt als eine einzelne Zeichenfolge mit durch Trennzeichen getrennten Begriffen zurückgibt. 

> [!NOTE]
> Das Zurückgeben einer einzelnen Zeichenfolge hat unser Wortwolkenbeispiel vereinfacht. Andererseits ist eine Liste ein flexibleres Format für das Arbeiten mit den zurückgegebenen Begriffen in Power BI. Sie können Listenobjekte in Power BI Desktop mithilfe der Gruppe „Strukturierte Spalte“ im Menüband „Transformation“ des Abfrage-Editors ändern.

```fsharp
// Returns key phrases from the text as a list object
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = jsonresp[documents]{0}[keyPhrases]
in  keyphrases
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den Textanalysedienst, die Power Query-Formelsprache „M“ oder Power BI.

> [!div class="nextstepaction"]
> [Referenz zur Textanalyse-API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)

> [!div class="nextstepaction"]
> [Referenz zu Power Query M](//msdn.microsoft.com/library/mt211003.aspx)

> [!div class="nextstepaction"]
> [Power BI-Dokumentation](//powerbi.microsoft.com/documentation/powerbi-landing-page/)
