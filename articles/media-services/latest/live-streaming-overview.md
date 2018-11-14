---
title: Übersicht über Livestreaming mit Azure Media Services | Microsoft Docs
description: Dieses Thema bietet eine Übersicht über Livestreaming mit Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: c8e4e84d7ae0defdb053108dc668956062c47ea5
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50962383"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Livestreaming mit Azure Media Services v3

Beim Bereitstellen von Livestreamingereignissen mit Azure Media Services sind häufig die folgenden Komponenten beteiligt:

* Eine Kamera, mit der ein Ereignis übertragen wird.
* Ein Livevideoencoder, der Signale von der Kamera (oder einem anderen Gerät wie Laptop) in Streams konvertiert, die an den Livestreamingdienst gesendet werden. Die Signale können auch Werbeblöcke und -einblendungen gemäß dem Standard SCTE-35 enthalten. 
* Mit dem Livestreamingdienst Media Services können Sie die Inhalte erfassen, in einer Vorschau anzeigen, packen, aufzeichnen, verschlüsseln und zur weiteren Verteilung an Ihre Kunden oder ein CDN senden.

Dieser Artikel bietet einen detaillierten Überblick und enthält Diagramme der wichtigsten Komponenten für Livestreaming mit Media Services.

## <a name="overview-of-main-components"></a>Übersicht über die wichtigsten Komponenten

In Media Services sind [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) für die Verarbeitung von Livestreaminginhalten zuständig. Ein LiveEvent (Liveereignis) stellt einen Eingabeendpunkt (Erfassungs-URL) bereit, den Sie dann einem lokalen Liveencoder bereitstellen. Das LiveEvent empfängt vom Liveencoder Liveeingabestreams im RTMP- oder Smooth Streaming-Format und stellt diese zum Streamen über einen oder mehrere [StreamingEndpoints](https://docs.microsoft.com/rest/api/media/streamingendpoints) (Streamingendpunkte) zur Verfügung. Ein [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) (Liveausgabe) ermöglicht Ihnen die Steuerung der Veröffentlichung, Aufzeichnung und DVR-Fenstereinstellungen des Livestreams. Zudem stellt das LiveEvent einen Vorschauendpunkt (Vorschau-URL) bereit, mit dem Sie eine Vorschau des Streams anzeigen und prüfen können, bevor Sie ihn weiter verarbeiten und übermitteln. 

Media Services bietet eine **dynamische Paketerstellung**, die Ihnen das Anzeigen einer Vorschau und Senden Ihrer Inhalte in den folgenden Streamingformaten ermöglicht: MPEG DASH, HLS und Smooth Streaming. Dabei ist es nicht erforderlich, für diese Streamingformate eine erneute Paketerstellung durchzuführen. Die Wiedergabe ist mit allen Playern möglich, die mit HLS, DASH oder Smooth kompatibel sind. Sie können auch [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) verwenden, um Ihren Stream zu testen.

Mit Media Services können Sie Ihre zu übermittelnden Inhalte mithilfe der (**dynamischen Verschlüsselung**) mit Advanced Encryption Standard (AES-128) oder einem der drei wichtigsten DRM-Systeme (Digital Rights Management, Verwaltung digitaler Rechte) verschlüsseln: Microsoft PlayReady, Google Widevine und Apple FairPlay. Media Services bietet auch einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen (PlayReady, Widevine und FairPlay) an autorisierte Clients.

Auf Wunsch können Sie auch die **dynamische Filterung** anwenden, mit der Sie die Anzahl der Spuren, Formate und Bitraten steuern können, die an die Player gesendet werden. Media Services unterstützt auch das Einfügen von Werbeeinblendungen.

### <a name="new-live-encoding-improvements"></a>Neue Verbesserungen bei der Livecodierung

Die folgenden neuen Verbesserungen wurden in der neuesten Version vorgenommen.

- Neuer Modus mit geringer Latenz für den Livebetrieb (10 Sekunden für den End-to-End-Vorgang).
- Verbesserte RTMP-Unterstützung (höhere Stabilität und bessere Unterstützung für Quellcodierer).
- Sichere RTMPS-Erfassung.

    Bei der Erstellung eines Liveereignisses erhalten Sie jetzt vier Erfassungs-URLs. Die vier Erfassungs-URLs sind nahezu identisch und verfügen über das gleiche Streamingtoken (AppId). Nur der Portnummernteil unterscheidet sich. Zwei der URLs dienen als primäre URL und Backup-URL für RTMPS.   
- Unterstützung der 24-Stunden-Transcodierung. 
- Verbesserte Unterstützung für Werbeeinblendungen in RTMP über SCTE35.

## <a name="liveevent-types"></a>LiveEvent-Typen

Für ein [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) ist einer von zwei Typen möglich: Livecodierung und Pass-Through. 

### <a name="live-encoding-with-media-services"></a>Livecodierung mit Media Services

![Livecodierung](./media/live-streaming/live-encoding.png)

Ein lokaler Liveencoder sendet einen Stream mit Einzelbitrate an das LiveEvent, das zum Ausführen einer Livecodierung mit Media Services in einem der folgenden Protokolle aktiviert ist: RTMP oder Smooth Streaming (fragmentiertes MP4). Vom LiveEvent wird dann eine Livecodierung des Eingabestreams mit Einzelbitrate in einen (adaptiven) Videostream mit Mehrfachbitrate ausgeführt. Auf Anforderung wird der Datenstrom den Kunden von Media Services bereitgestellt.

Geben Sie beim Erstellen dieses LiveEvent-Typs **Basic** (LiveEventEncodingType.Basic) ein.

### <a name="pass-through"></a>Pass-Through

![Pass-Through](./media/live-streaming/pass-through.png)

Pass-Through ist optimiert für lange Livestreams oder die lineare Livecodierung rund um die Uhr mithilfe eines lokalen Liveencoders. Vom lokalen Liveencoder wird **RTMP** oder **Smooth Streaming** (fragmentiertes MP4) mit Mehrfachbitrate an das für die **Pass-Through**-Übertragung konfigurierte LiveEvent gesendet. Bei der **Pass-Through**-Übertragung durchlaufen die erfassten Streams die **LiveEvents** ohne weitere Verarbeitung. 

LiveEvents mit Pass-Through können eine Auflösung von bis zu 4K unterstützen. Bei Verwendung des Smooth Streaming-Erfassungsprotokolls wird HEVC Pass-Through unterstützt. 

Geben Sie beim Erstellen dieses LiveEvent-Typs **None** (LiveEventEncodingType.None) ein.

> [!NOTE]
> Die Verwendung der Pass-Through-Methode ist die wirtschaftlichste Form des Livestreamings, wenn mehrere Ereignisse über einen längeren Zeitraum gestreamt werden und Sie bereits in lokale Encoder investiert haben. Preisdetails finden Sie [hier](https://azure.microsoft.com/pricing/details/media-services/) .
> 

## <a name="liveevent-types-comparison"></a>Vergleich der LiveEvent-Typen 

In der folgenden Tabelle werden die Features der beiden LiveEvent-Typen verglichen.

| Feature | LiveEvent „Pass-Through“ | LiveEvent „Basic“ |
| --- | --- | --- |
| Die Single-Bitrate-Eingabe wird in mehreren Bitraten in der Cloud codiert. |Nein  |JA |
| Maximale Auflösung, Anzahl der Ebenen |4Kp30  |720p, 6 Ebenen, 30 fps |
| Eingabeprotokolle |RTMP, Smooth Streaming |RTMP, Smooth Streaming |
| Preis |Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/media-services/) , und klicken Sie auf die Registerkarte „Live-Video“. |Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/media-services/) |
| Maximale Laufzeit |Rund um die Uhr |Rund um die Uhr |
| Unterstützung für das Einfügen von Slates |Nein  |JA |
| Unterstützung für Werbeeinblendungen über API|Nein  |JA |
| Unterstützung für Werbeeinblendungen über SCTE-35 Inband|JA |JA |
| Pass-Through-CEA-608/708-Untertitel |JA |JA |
| Möglichkeit zum Wiederherstellen nach kurzen Unterbrechungen im Beitrag |JA |Nein (LiveEvent beginnt Slating nach 6 Sekunden ohne Eingabedaten)|
| Unterstützung für nicht einheitliche Eingabe-GOPs |JA |Nein, Eingabe muss aus festen zweisekündigen GOPs bestehen |
| Unterstützung für Eingaben mit variable Bildwiederholrate |JA |Nein, Eingabe muss eine feste Bildfrequenz aufweisen.<br/>Kleinere Abweichungen, beispielsweise bei Szenen mit viel Bewegung, werden toleriert. Der Encoder kann aber nicht bis auf 10 Frames pro Sekunde zurückfallen. |
| Automatische Abschaltung des LiveEvents, wenn der Eingabefeed verloren geht |Nein  |Nach 12 Stunden, wenn kein LiveOutput erfolgt |

## <a name="liveevent-states"></a>LiveEvent-Status 

Der aktuelle Zustand eines LiveEvents. Mögliche Werte sind:

|Zustand|BESCHREIBUNG|
|---|---|
|**Beendet**| Dies ist der anfängliche Status des LiveEvents nach seiner Erstellung (es sei denn, im Portal wurde automatisches Starten gewählt). In diesem Status werden keine Gebühren berechnet. In diesem Zustand können die LiveEvent-Eigenschaften aktualisiert werden, aber Streaming ist nicht zulässig.|
|**Wird gestartet**| Das LiveEvent wird gestartet. In diesem Status werden keine Gebühren berechnet. In diesem Status sind weder Updates noch Streaming zulässig. Wenn ein Fehler auftritt, wird das LiveEvent in den Zustand „Stopped“ (Angehalten) zurückgesetzt.|
|**Wird ausgeführt**| Das LiveEvent kann Livestreams verarbeiten. Die Nutzung wird jetzt berechnet. Sie müssen das LiveEvent beenden, um zu verhindern, dass weitere Gebühren anfallen.|
|**Wird beendet**| Das LiveEvent wird beendet. In diesem Übergangszustand erfolgt keine Berechnung. In diesem Status sind weder Updates noch Streaming zulässig.|
|**Wird gelöscht**.| Das LiveEvent wird gelöscht. In diesem Übergangszustand erfolgt keine Berechnung. In diesem Status sind weder Updates noch Streaming zulässig.|

## <a name="liveoutput"></a>LiveOutput

Ein [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) (Liveausgabe) ermöglicht Ihnen die Steuerung der Veröffentlichung, Aufzeichnung und DVR-Fenstereinstellungen des Livestreams. Die Beziehung zwischen LiveEvent und LiveOutput ähnelt herkömmlichen Medien, bei denen ein Kanal (LiveEvent) einen konstanten Stream von Inhalten aufweist und ein Programm (LiveOutput) auf ein zeitlich festgelegtes Ereignis in diesem LiveEvent ausgerichtet ist.
Mithilfe der Eigenschaft **ArchiveWindowLength** können Sie die Anzahl der Stunden festlegen, für die Sie den aufgezeichneten Inhalt für den LiveOutput beibehalten möchten. **ArchiveWindowLength** ist eine gemäß ISO-8601 definierte Zeitraumlänge des Archivfensters (Digitaler Videorekorder bzw. DVR). Dieser Wert kann von mindestens 5 Minuten bis zu einem Höchstwert von 25 Stunden eingestellt werden. 

**ArchiveWindowLength** bestimmt außerdem die maximale Häufigkeit, mit der Clients von der aktuellen Liveposition aus rückwärts suchen können. LiveOutputs können über den angegebenen Zeitraum laufen. Inhalte, die über das Zeitfenster hinausgehen, werden jedoch fortlaufend verworfen. Der Wert dieser Eigenschaft legt außerdem fest, auf welche Länge Clientmanifeste anwachsen können.

Jeder LiveOutput ist mit einem [Medienobjekt](https://docs.microsoft.com/rest/api/media/assets) verknüpft und zeichnet Daten in einen Container in Azure Storage auf, der mit dem Media Services-Konto verbunden ist. Zum Veröffentlichen des LiveOutputs müssen Sie einen [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) für das zugehörige Medienobjekt erstellen. Mithilfe dieses Locators können Sie eine Streaming-URL erstellen, die Sie Ihren Kunden bereitstellen können.

Ein LiveEvent unterstützt bis zu drei gleichzeitig ausgeführte LiveOutputs, sodass Sie mehrere Archive desselben eingehenden Datenstroms erstellen können. Auf diese Weise können Sie verschiedene Teile eines Ereignisses nach Bedarf veröffentlichen und archivieren. Beispiel: Ihre Geschäftsanforderung besteht darin, rund um die Uhr einen linearen Livefeed zu senden. Sie möchten jedoch den ganzen Tag über „Aufzeichnungen“ von Programmen erstellen, die Sie Ihren Kunden auf Abruf zur Nachbetrachtung anbieten können.  Für dieses Szenario erstellen Sie zunächst einen primären LiveOutput mit einem kurzen Archivfenster von maximal einer Stunde, in dem sich die Kunden dem primären Livestream zuschalten können. Sie erstellen einen StreamingLocator für diesen LiveOutput und veröffentlichen ihn als „Live“-Feed in Ihrer Anwendung oder auf Ihrer Website.  Während der Feed aktiv ist, können Sie programmgesteuert einen zweiten gleichzeitigen LiveOutput am Anfang einer Sendung erstellen (oder 5 Minuten früher, um Material zum späteren Schneiden bereitzustellen). Dieser zweite LiveOutput kann 5 Minuten nach Programm- oder Ereignisende beendet werden. Sie können dann einen neuen StreamingLocator erstellen, um dieses Programm als Medienobjekt auf Abruf im Katalog Ihrer Anwendung zu veröffentlichen.  Sie können diesen Vorgang für andere Programmgrenzen oder Highlights, die Sie sofort auf Abruf freischalten möchten, mehrfach wiederholen, während der "Live"-Feed des ersten LiveOutputs weiterhin den linearen Feed sendet.  Darüber hinaus können Sie die Unterstützung des dynamischen Filters nutzen, um Anfang und Ende des Archivs aus dem LiveOutput, den Sie für "Überlappungssicherheit" zwischen Programmen eingeführt haben, zu schneiden und einen genaueren Start und ein genaueres Ende des Inhalts zu erreichen. Archivierte Inhalte können auch an eine [Transformation](https://docs.microsoft.com/rest/api/media/transforms) zur Codierung oder framegenauen Erstellung von Subclips in mehrere Ausgabeformate für die Nutzung als Syndikation für andere Dienste übergeben werden.

## <a name="streamingendpoint"></a>StreamingEndpoint

Wenn der Stream an das LiveEvent übertragen wird, können Sie das Streamingereignis starten, indem Sie ein Medienobjekt, einen LiveOutput und StreamingLocator erstellen. Dadurch wird der Stream archiviert und über den [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) für die Zuschauer verfügbar gemacht.

Beim Erstellen Ihres Media Services-Kontos wird dem Konto ein Standard-Streamingendpunkt im Zustand „Beendet“ hinzugefügt. Um mit dem Streamen Ihrer Inhalte zu beginnen und die dynamische Paketerstellung und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Zustand „Wird ausgeführt“ aufweisen.

## <a name="billing"></a>Abrechnung

Für ein LiveEvent beginnt die Berechnung von Gebühren, sobald der Zustand in „Wird ausgeführt“ geändert wird. Damit für das LiveEvent keine Gebühren mehr anfallen, müssen Sie es beenden.

> [!NOTE]
> Wenn **LiveEventEncodingType** für Ihr [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) auf „Basic“ festgelegt ist, schaltet der Media Services jedes LiveEvent, das sich noch im Zustand „Wird ausgeführt“ befindet, 12 Stunden nachdem der Eingangsfeed verloren gegangen ist, automatisch ab, und es erfolgen keine LiveOutputs. Allerdings wird Ihnen dennoch die Zeit in Rechnung gestellt, die das LiveEvent den Zustand „Wird ausgeführt“ hatte.
>

In der folgenden Tabelle ist die Zuordnung der LiveEvent-Zustände zu den Abrechnungsmodi aufgeführt.

| LiveEvent-Zustand | Werden Gebühren berechnet? |
| --- | --- |
| Wird gestartet |Nein (Übergangsstatus) |
| Wird ausgeführt |JA |
| Wird beendet |Nein (Übergangsstatus) |
| Beendet |Nein  |

## <a name="next-steps"></a>Nächste Schritte

[Tutorial zu Livestreaming](stream-live-tutorial-with-api.md)
