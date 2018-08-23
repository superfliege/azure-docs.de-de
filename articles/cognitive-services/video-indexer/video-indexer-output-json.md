---
title: Untersuchen der Azure Video Indexer-Ausgabe | Microsoft-Dokumentation
description: In diesem Thema wird die Video Indexer-Ausgabe untersucht.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cflower
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 7f61833d70cea3a6d176f5a08eb0db1c30e70e86
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "35378652"
---
# <a name="examine-the-video-indexer-output-produced-by-v1-api"></a>Untersuchen der von der v1-API erstellten Video Indexer-Ausgabe

> [!Note]
> Die Video Indexer V1-APIs sind jetzt veraltet und werden am 1. August 2018 entfernt. Verwenden Sie ab jetzt die Video Indexer v2-APIs, um Unterbrechungen zu vermeiden.
>
> Eine Anleitung zur Entwicklung mit den v2-APIs für Video Indexer finden Sie [hier](https://api-portal.videoindexer.ai/). 

Wenn Sie die API zum Abrufen von Aufschlüsselungen (**Get Breakdowns**) aufrufen und der Antwortstatus „OK“ lautet, erhalten Sie eine ausführliche JSON-Ausgabe als Inhalt der Antwort. Die JSON-Daten enthalten Details zu den angegebenen Erkenntnissen aus Videos (einschließlich Transkript, OCRs, Personen). Die Details enthalten Stichwörter (Themen), Gesichter, Blöcke. Jeder Block enthält Zeitbereiche, Transkriptzeilen, OCR-Zeilen, Stimmungen, Gesichter, und Vorschauminiaturen von Blöcken.

Mit der API zum Abrufen von Aufschlüsselungen **Get Breakdowns** erhalten Sie die vollständige Aufschlüsselung eines Videos als JSON-Inhalt.  
 
    GET https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/63c6d532ff HTTP/1.1
    Host: videobreakdown.azure-api.net
    Ocp-Apim-Subscription-Key: ••••••••••••••••••••••••••••••••
    
Sie können die zusammengefassten Erkenntnisse des Videos auch visuell untersuchen, indem Sie im Video Indexer-Portal die Schaltfläche für die **Wiedergabe** drücken. Weitere Informationen finden Sie unter [View and edit video insights](video-indexer-view-edit.md) (Anzeigen und Bearbeiten von Videoinformationen).

![Einblicke](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

In diesem Artikel wird der JSON-Inhalt untersucht, der von der API zum Abrufen von Aufschlüsselungen (**Get Breakdowns**) zurückgegeben wird. Das Lesen des Artikels zu [Konzepten](video-indexer-concepts.md) kann hilfreich sein.

> [!NOTE]
> Der Zeitraum bis zum Ablauf aller Zugriffstoken in Video Indexer beträgt eine Stunde.

## <a name="root-elements"></a>Stammelemente

Attribut | BESCHREIBUNG
---|---
id|Die ID dieses Videos. Beispiel: 63c6d532ff.
partition|Eine logische Partition, die der Benutzer im Upload angeben kann, um später danach suchen.
name|Der Name des Videos. Beispiel: Azure Monitor.
Beschreibung|Beschreibung des Videos. Beispiel: „John Kemnetz und Scott Hanselman zeigen Ihnen, wie Sie Azure-Überwachungsdaten mit Azure Monitor optimal nutzen“.
userName|Der Ersteller des Videos. Beispiel: Channel9 Videos.
createTime |Zeitpunkt der Erstellung. Beispiel: 2017-03-31T16:36:41.4504249+00:00.
privacyMode|Für Ihr Video kann einer der folgenden Modi verwendet werden: **Private** (Privat) oder **Public** (Öffentlich). **Public**: Das Video ist für alle Benutzer Ihres Kontos und alle Personen sichtbar, die über einen Link zum Video verfügen. **Private**: Das Video ist für alle Benutzer Ihres Kontos sichtbar.
isOwned|„True“, wenn der aktuelle Benutzer das Video besitzt. Andernfalls „false“.  
isBase|„True“, wenn die Aufschlüsselung auf einem Quellvideo basiert. „False“, wenn es sich um die Aufschlüsselung einer Wiedergabeliste handelt, die von einer anderen Aufschlüsselung abgeleitet ist.
durationInSeconds|Dauer des Videos.
summarizedInsights|Enthält ein [summarizedInsights](#summarizedInsights)-Element.
breakdowns|Kann eine oder mehrere [Aufschlüsselungen](#breakdowns) enthalten.
social|Enthält mindestens ein **social**-Element, das die Anzahl der „Gefällt mir“-Markierungen und Aufrufe des Videos angibt.

## <a name="summarizedinsights"></a>summarizedInsights

In diesem Abschnitt wird die Zusammenfassung der Erkenntnisse angezeigt.

Attribut | BESCHREIBUNG
---|---
name|Der Name des Videos. Beispiel: Azure Monitor.
shortId|Die ID des Videos. Beispiel: 63c6d532ff.
privacyMode|Für Ihre Aufschlüsselung kann einer der folgenden Modi verwendet werden: **Private** (Privat) oder **Public** (Öffentlich). **Public**: Das Video ist für alle Benutzer Ihres Kontos und alle Personen sichtbar, die über einen Link zum Video verfügen. **Private**: Das Video ist für alle Benutzer Ihres Kontos sichtbar.
duration|Enthält eine Dauer, mit der der Zeitbereich beschrieben wird, in dem eine Erkenntnis gewonnen wurde. Die Dauer wird in Sekunden angegeben.
thumbnailUrl|Die vollständige URL der Vorschauminiatur des Videos. Beispiel: „https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...“. Hinweis: Wenn das Video privat ist, enthält die URL ein Zugriffstoken für eine Stunde. Nach einer Stunde ist die URL nicht mehr gültig, und Sie müssen entweder die Aufschlüsselung mit einer neuen URL darin erneut abrufen oder „GetAccessToken“ aufrufen, um ein neues Zugriffstoken abzurufen und die vollständige URL manuell erstellen ('https://www.videoindexer.ai/api/Thumbnail/[shortId]/[ThumbnailId]?accessToken=[accessToken]').
faces|Kann ein oder mehrere [faces](#faces)-Elemente enthalten.
topics|Kann ein oder mehrere [topics](#topics)-Elemente enthalten.
sentiments|Kann ein oder mehrere [sentiments](#sentiments)-Elemente enthalten.
audioEffects| Kann ein oder mehrere [audioEffects](#audioEffects)-Elemente enthalten.
brands| Kann null oder mehr [brands](#brands)-Elemente enthalten.
Statistiken|Weitere Informationen finden Sie unter [Statistik](#Statistics)
.
### <a name="statistics"></a>Statistiken

|NAME|BESCHREIBUNG|
|---|---|
|CorrespondenceCount|Anzahl von Korrespondenzen im Video.|
|WordCount|Die Anzahl von Wörtern pro Sprecher.|
|SpeakerNumberOfFragments|Die Anzahl von Fragmenten, über die ein Sprecher im Video verfügt.|
|SpeakerLongestMonolog|Der längste Monolog des Sprechers. Falls der Sprecher bei seinem Monolog Sprechpausen einlegt, ist diese Zeit mit enthalten. Die Ruhephasen am Anfang und Ende des Monologs werden entfernt.| 
|SpeakerTalkToListenRatio|Die Berechnung basiert auf der Zeit für den Monolog des Sprechers (ohne Sprechpausen) geteilt durch die Gesamtzeit des Videos. Der Zeitwert wird auf die dritte Dezimalstelle gerundet.|

## <a name="breakdowns"></a>breakdowns

In diesem Abschnitt werden die Details der Erkenntnisse angezeigt.

Attribut | BESCHREIBUNG
---|---
id|Die Aufschlüsselungs-ID. Beispiel: 63c6d532ff.
state|Der Verarbeitungsstatus der angegebenen Aufschlüsselungs-ID. Folgende Status sind möglich: Uploaded, Processing, Processed, Failed (Hochgeladen, Verarbeitung, Verarbeitet, Fehler).
processingProgress|Der Fortschritt. Beispiel: 10%.
externalId| Sie können „externalId“ während des Uploads festlegen. Beispiel: „4f9c3500-eca7-4ab3-987e-a745017af698“. Sie können später anhand dieser externen ID nach Ihren Videos suchen.
externalUrl|Sie können „externalUrl“ während des Uploads festlegen. 
metadata|Sie können „metadata“ während des Uploads festlegen. 
insights|Kann ein oder mehrere [insights](#insights)-Elemente enthalten.
thumbnailUrl|Die vollständige URL der Vorschauminiatur des Videos. Beispiel: „https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO..“. Hinweis: Wenn das Video privat ist, enthält die URL ein Zugriffstoken für eine Stunde. Nach einer Stunde ist die URL nicht mehr gültig, und Sie müssen entweder die Aufschlüsselung mit einer neuen URL darin erneut abrufen oder „GetAccessToken“ aufrufen, um ein neues Zugriffstoken abzurufen und die vollständige URL manuell erstellen ('https://www.videoindexer.ai/api/Thumbnail/[shortId]/[ThumbnailId]?accessToken=[accessToken]').
publishedUrl|Die veröffentlichte URL. Beispiel: „https://BreakdownMedia.azureedge.net:443/d5e5232d-48e2-4fbc-9893-0ea6335da563/Azure%20Monitor%20%20Azure%20Friday.ism/manifest“.
viewToken|Das Bearertoken.
sourceLanguage|Die Quellsprache. Folgende Sprachen werden unterstützt: Chinesisch, Englisch, Französisch, Deutsch, Italienisch, Japanisch, Portugiesisch, Russisch, Spanisch.
Language|Die Sprache des Transkripts.

## <a name="insights"></a>insights

Attribut | BESCHREIBUNG 
---|---
transcriptBlocks|Kann ein oder mehrere [transcriptBlocks](#transcriptBlocks)-Elemente enthalten.
topics|Kann ein oder mehrere [topics](#topics)-Elemente enthalten.
faces|Kann ein oder mehrere [faces](#faces)-Elemente enthalten.
participants|Kann ein oder mehrere [participants](#participants)-Elemente enthalten.
contentModeration|Kann ein [contentModeration](#contentModeration)-Elemente enthalten.
audioEffectsCategories|Kann ein oder mehrere [audioEffectsCategories](#audioEffectsCategories)-Elemente enthalten.

## <a name="faces"></a>faces

### <a name="summarizedinsights"></a>summarizedInsights

Unter **summarizedInsights** angezeigte **faces**-Elemente geben eine Zusammenfassung der einzelnen Gesichter im Video an.

Attribut | BESCHREIBUNG 
---|---
id|Die ID einer Person. Beispiel: 11775.
shortId|Die Kurz-ID. Da eine Wiedergabeliste von mehreren Aufschlüsselungen abgeleitet sein kann, ist diese ID erforderlich, um herauszufinden, welche dieser Aufschlüsselungen der Ursprung der einzelnen Gesichter ist.  
name|Wenn das Gesicht erkannt wird, wird der Name der Person hinzugefügt. Beispiel: „Scott Hanselman“. Wenn das Gesicht unbekannt ist, wird „Unknown #“ (Unbekannte Person Nr.) hinzugefügt. 
Beschreibung|Wenn das Gesicht erkannt wird, wird die Beschreibung basierend auf der Bing-API-Suche ausgefüllt. Andernfalls ist die Beschreibung **NULL**.
title|Wenn das Gesicht erkannt wird, wird die Beschreibung basierend auf der Bing-API-Suche ausgefüllt. Andernfalls ist der Titel **NULL**.
thumbnailFullUrl|Die vollständige URL der Vorschauminiatur des Gesichts. Beispiel: „https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...“. Hinweis: Wenn das Video privat ist, enthält die URL ein Zugriffstoken für eine Stunde. Nach einer Stunde ist die URL nicht mehr gültig, und Sie müssen entweder die Aufschlüsselung mit einer neuen URL darin erneut abrufen oder „GetAccessToken“ aufrufen, um ein neues Zugriffstoken abzurufen und die vollständige URL manuell erstellen ('https://www.videoindexer.ai/api/Thumbnail/[shortId]/[ThumbnailId]?accessToken=[accessToken]').
appearances|Kann ein oder mehrere [appearances](#appearances)-Elemente enthalten.
seenDuration|Dauer, die das Gesicht zu sehen war (in Sekunden).
seenDurationRatio|Präsenz im Verhältnis zur Videodauer (0–1).

### <a name="breakdown-insights"></a>Erkenntnisse aus Aufschlüsselungen

Unter **breakdowns** angezeigte **faces**-Elemente beschreiben Details zu jedem Gesicht im Video.

Attribut | BESCHREIBUNG 
---|---
id|Die ID einer Person. Beispiel: 11775.
bingId|
name|Wenn das Gesicht erkannt wird, wird der Name der Person hinzugefügt. Beispiel: „Scott Hanselman“. Wenn das Gesicht unbekannt ist, wird „Unknown #“ (Unbekannte Person Nr.) hinzugefügt. 
thumbnailId|Beispiel: 616468f0-1636-4efa-94e7-262f2e575059.
Beschreibung|Wenn das Gesicht erkannt wird, wird die Beschreibung basierend auf der Bing-API-Suche ausgefüllt. Andernfalls ist die Beschreibung **NULL**.
title|Wenn das Gesicht erkannt wird, wird die Beschreibung basierend auf der Bing-API-Suche ausgefüllt. Andernfalls ist der Titel **NULL**.
imageUrl|Diese URL verweist auf ein Bild, das aus dem Quellvideo stammt.  
confidence|Die Zuverlässigkeitsbewertung (höher ist besser).
knownPersonId|Die ID einer bekannten Person (z.B. berühmte Person). Wenn eine Person nicht bekannt ist, enthält die ID Nullen. Beispiel: e3eaff5f-ee1b-4eac-80ce-ebac47aadf64.

## <a name="topics"></a>topics

### <a name="summarizedinsights"></a>summarizedInsights

Unter **summarizedInsights** angezeigte **topics**-Elemente geben eine Zusammenfassung für jedes Thema im Video an.

Attribut | BESCHREIBUNG 
---|---
name|Der Name des Themas (z.B. „Azure“). 
appearances|Kann ein oder mehrere [appearances](#appearances)-Elemente enthalten.
isTranscript|„True“ bei Vorhandensein in einem Transkript. „False“ bei Vorhandensein in einer OCR.

### <a name="breakdown-insights"></a>Erkenntnisse aus Aufschlüsselungen

Unter **breakdowns** angezeigte **topics**-Elemente beschreiben Details zu jedem Thema im Video.

|Attribut | BESCHREIBUNG |
|---|---|
|id|Eindeutige ID des Themas.|
|name|Der Name des Themas.|
|stem|Dieses Attribut wird derzeit nicht verwendet.|
|words|Dieses Attribut wird derzeit nicht verwendet.|
|rank|Relevanzbewertung (höher ist besser).|

## <a name="sentiments"></a>sentiments

Attribut | BESCHREIBUNG
---|---
sentimentKey| Derzeit werden die folgenden Stimmungen unterstützt: Positiv, Neutral, Negativ. 
appearances|Kann ein oder mehrere [appearances](#appearances)-Elemente enthalten|.
seenDurationRatio|Präsenz im Verhältnis zur Videodauer (0–1).

## <a name="audioeffects"></a>audioEffects

Attribut | BESCHREIBUNG 
---|---
audioEffectKey| Gültige Werte sind: Speech, Silence, HandClaps.
appearances|Kann ein oder mehrere [appearances](#appearances)-Elemente enthalten
seenDurationRatio|Präsenz im Verhältnis zur Videodauer (0–1).
seenDuration|Dauer des Audioeffekts (in Sekunden).

## <a name="appearances"></a>appearances

Attribut | BESCHREIBUNG 
---|---
startTime| Zeitwert.
endTime|Zeitwert.
startSeconds| Zeitwert.
endSeconds| Zeitwert.

## <a name="participants"></a>participants

Attribut | BESCHREIBUNG 
---|---
id|Die ID des Teilnehmers.
name|Der Name des Teilnehmers. Beispiel: Speaker #1 (Sprecher Nr. 1).
pictureUrl|Das **pictureUrl**-Attribut ist für die zukünftige Verwendung reserviert.

## <a name="contentmoderation"></a>contentModeration

Attribut | BESCHREIBUNG 
---|---
adultClassifierValue|Der Zuverlässigkeitsgrad für das Vorhandensein von nicht jugendfreien Inhalten im Video.
racyClassifierValue|Der Zuverlässigkeitsgrad für das Vorhandensein von freizügigen Inhalten im Video.
bannedWordsCount|Anzahl von Obszönitäten. 
bannedWordsRatio|Verhältnis von Obszönitäten zur Gesamtzahl der Wörter.
reviewRecommended|Boolescher Wert, der angibt, ob das Video manuell überprüft werden muss.
isAdult|Boolescher Wert, der angibt, ob das Video nach der manuellen Prüfung als nicht jugendfrei gilt.

## <a name="audioeffectscategories"></a>audioEffectsCategories

Attribut | BESCHREIBUNG 
---|---
type|ID der Kategorie.
key|Einer der folgenden Werte: Speech, Silence, HandClaps. 

## <a name="transcriptblocks"></a>transcriptBlocks

Attribut | BESCHREIBUNG
---|---
id|Die ID des Blocks.
lines|Kann ein oder mehrere [lines](#lines)-Elemente enthalten.
sentimentIds|Das **sentimentIds**-Attribut ist für die zukünftige Verwendung reserviert.
thumbnailIds|Das **thumbnailIds**-Attribut ist für die zukünftige Verwendung reserviert.
Stimmung|Die Stimmung im Block (0–1, negativ zu positiv).
faces|Kann ein oder mehrere [faces](#faces)-Elemente enthalten.
ocrs|Kann ein oder mehrere [ocr](#ocrs)-Elemente enthalten.
audioEffectInstances|Kann ein oder mehrere [audioEffectInstances](#audioEffectInstances)-Elemente enthalten.
scenes|Kann ein oder mehrere [scenes](#scenes)-Elemente enthalten.
annotations|Kann null oder mehr [annotations](#annotations)-Elemente enthalten.

## <a name="ocrs"></a>ocrs

Beschreibt, an welchem Punkt im Video der Textinhalt gefunden wurde. 

Attribut | BESCHREIBUNG 
---|---
timeRange|Der Zeitbereich im ursprünglichen Video.
adjustedTimeRange|„adjustedTimeRange“ ist der Zeitbereich im Verhältnis zur aktuellen Wiedergabeliste. Da Sie eine Wiedergabeliste aus verschiedenen Zeilen verschiedener Videos erstellen können, haben Sie die Möglichkeit, nur eine Zeile aus einem einstündigen Video zu verwenden, z.B. 10:00–10: 15. In diesem Fall erhalten Sie eine Wiedergabeliste mit 1 Zeile, in dem der Zeitbereich 10:00–10: 15, der adjustedTimeRange-Wert jedoch 00:00–00:15 ist.
lines|Kann ein oder mehrere [lines](#lines)-Elemente enthalten.

## <a name="lines"></a>lines

### <a name="transcriptblocks"></a>transcriptBlocks

Unter **transcriptBlocks** angezeigte **lines**-Elemente beschreiben Transkriptzeilen im Video.

Attribut | BESCHREIBUNG 
---|---
id| Die ID der Zeile.
timeRange|Der Zeitbereich im ursprünglichen Video.
adjustedTimeRange|„adjustedTimeRange“ ist der Zeitbereich im Verhältnis zur aktuellen Wiedergabeliste. Da Sie eine Wiedergabeliste aus verschiedenen Zeilen verschiedener Videos erstellen können, haben Sie die Möglichkeit, nur eine Zeile aus einem einstündigen Video zu verwenden, z.B. 10:00–10: 15. In diesem Fall erhalten Sie eine Wiedergabeliste mit 1 Zeile, in dem der Zeitbereich 10:00–10: 15, der adjustedTimeRange-Wert jedoch 00:00–00:15 ist.
participantID| Die ID des Sprechers dieser Zeile.
text| Das Transkript.
isIncluded| In Basisaufschlüsselungen immer „true“. In abgeleiteten Wiedergabelisten sind im Quellvideo enthaltene Zeilen immer auf „isIncluded=true“ festgelegt. Für alle anderen Zeilen ist „false“ festgelegt.

### <a name="ocrs"></a>ocrs

Unter **ocrs** angezeigte **lines**-Elemente beschreiben OCRs-Zeilen im Video.

Attribut | BESCHREIBUNG 
---|---
id|Die OCR-ID.
width|Dieses Attribut wird derzeit nicht verwendet.
height|Dieses Attribut wird derzeit nicht verwendet.
Language|Die OCR-Sprache.
textData|Der OCR-Text.
confidence|Die Zuverlässigkeitsbewertung (höher ist besser).

## <a name="scenes"></a>scenes

Attribut | BESCHREIBUNG 
---|---
id|Die Szenen-ID.
timeRange|Enthält ein **timeRange**-Element.
keyFrame|Die Zeit des Keyframes.
shots|Kann ein oder mehrere [shots](#shots)-Elemente enthalten.

## <a name="shots"></a>shots

Attribut | BESCHREIBUNG 
---|---
id||Die ID der Aufnahme.
timeRange|Enthält ein **timeRange**-Element.
keyFrame|Die Zeit des Keyframes.

## <a name="audioeffectinstances"></a>audioEffectInstances

Attribut | BESCHREIBUNG 
---|---
type|Der Index des Audioereignisses: Laughter = 1, HandClaps = 2, Music = 3, Speech = 4, Silence = 5
ranges|Kann ein oder mehrere [ranges](#ranges)-Elemente enthalten.

## <a name="ranges"></a>ranges

Under **audioEffectInstances** angezeigte **ranges**-Elemente beschreiben Audioeffekte in diesen Bereichen.

Attribut | BESCHREIBUNG 
---|---
timeRange|Der Zeitbereich im ursprünglichen Video.
adjustedTimeRange|„adjustedTimeRange“ ist der Zeitbereich im Verhältnis zur aktuellen Wiedergabeliste. Da Sie eine Wiedergabeliste aus verschiedenen Zeilen verschiedener Videos erstellen können, haben Sie die Möglichkeit, nur eine Zeile aus einem einstündigen Video zu verwenden, z.B. 10:00–10: 15. In diesem Fall erhalten Sie eine Wiedergabeliste mit 1 Zeile, in dem der Zeitbereich 10:00–10: 15, der adjustedTimeRange-Wert jedoch 00:00–00:15 ist.

## <a name="annotations"></a>annotations

Rückgabe-Tags basierend auf erkennbaren Objekten, Lebewesen, Landschaften, Aktionen und visuellen Mustern.

|Attribut|BESCHREIBUNG|
|---|---|
|id|Die ID der Anmerkung.|
|NAME|Der Name der Anmerkung (z.B. Person, Leichtathletik, schwarze Rahmen).|
|appearances|Kann ein oder mehrere appearances-Elemente enthalten.|

## <a name="brands"></a>brands

Markennamen von Unternehmen oder Produkten, die im Transkript der Spracherkennung bzw. in den Video-OCR-Daten erkannt werden. Die visuelle Erkennung von Marken oder die Logoerkennung sind hierin nicht enthalten.

Attribut | BESCHREIBUNG
---|---
id | Die ID einer Marke.
name | Der Name der Marke aus Bing oder eine benutzerdefinierte Marke.  
wikiId | Das Suffix der Wikipedia-URL für die Marke. Beispielsweise ist „Target_Corporation“ das Suffix von [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
wikiUrl | Die Wikipedia-URL der Marke, falls vorhanden. Beispiel: [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation)
confidence | Der Zuverlässigkeitswert der Video Indexer-Markenerkennung (0 - 1).
Beschreibung | Aus Wikipedia extrahierte Beschreibung der Marke. 
appearances | Kann ein oder mehrere appearances-Elemente enthalten.
seenDuration | Präsenz im Verhältnis zur Videodauer (0–1).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen Ihrer eigenen Aufschlüsselungen finden Sie unter [Anzeigen und Bearbeiten von Video Indexer-Auswertungen](video-indexer-view-edit.md).

Informationen dazu, wie Sie Widgets in Ihre Anwendung einbetten, finden Sie unter [Embed Video Indexer widgets into your applications](video-indexer-embed-widgets.md) (Einbetten von Video Indexer-Widgets in Ihre Anwendungen). 

## <a name="see-also"></a>Weitere Informationen

[Video Indexer-API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

[What is Video Indexer? (preview)](video-indexer-overview.md) (Was ist Video Indexer? (Vorschauversion))

