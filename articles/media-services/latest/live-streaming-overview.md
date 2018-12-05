---
title: Übersicht über das Livestreaming mit Azure Media Services | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über das Livestreaming mit Azure Media Services v3.
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
ms.date: 11/26/2018
ms.author: juliako
ms.openlocfilehash: 634563a2010562e20691abae132dc7540ef8faf2
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52632701"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Livestreaming mit Azure Media Services v3

Mit Azure Media Services können Sie Ihren Kunden Liveereignisse in der Azure Cloud anbieten. Um Liveereignisse mit Media Services zu streamen, benötigen Sie Folgendes:  

- Eine Kamera, mit der das Liveereignis aufgenommen wird.
- Ein Livevideoencoder, der Signale von der Kamera (oder einem anderen Gerät, z.B. einem Laptop) in Streams konvertiert, die an Media Services gesendet werden. Der Datenfeed kann Signale zu Werbezwecken enthalten, beispielsweise SCTE-35-Markierungen.
- Komponenten in Media Services, mit denen das Liveereignis erfasst, in einer Vorschau angezeigt, paketiert, aufgezeichnet, verschlüsselt und an Ihre Kunden übertragen oder zur weiteren Verteilung an ein CDN gesendet wird.

Dieser Artikel bietet einen detaillierten Überblick und umfasst einen Leitfaden und Diagramme der wichtigsten Komponenten für ein Livestreaming mit Media Services.

## <a name="overview-of-main-components"></a>Übersicht über die wichtigsten Komponenten

Um On-Demand- oder Livestreams mit Media Services bereitstellen zu können, benötigen Sie mindestens einen [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints). Beim Erstellen Ihres Media Services-Kontos wird dem Konto ein **Standard-StreamingEndpoint** im Zustand **Beendet** hinzugefügt. Sie müssen den StreamingEndpoint starten, von dem aus Sie Inhalt an Ihre Zuschauer streamen möchten. Sie können den standardmäßigen **StreamingEndpoint** verwenden oder einen weiteren angepassten **StreamingEndpoint** mit den gewünschten Konfigurations- und CDN-Einstellungen erstellen. Sie können mehrere StreamingEndpoint-Objekte aktivieren, die jeweils auf ein anderes CDN ausgerichtet sind und einen eindeutigen Hostnamen für die Inhaltsbereitstellung angeben. 

In Media Services sorgen [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) für das Erfassen und Verarbeiten von Livevideofeeds. Wenn Sie ein LiveEvent erstellen, wird ein Eingangsendpunkt erstellt, mit dem Sie ein Livesignal von einem Remoteencoder senden können. Der Remoteliveencoder sendet den Datenfeed an diesen Eingangsendpunkt, entweder über [RTMP](https://en.wikipedia.org/wiki/Real-Time_Messaging_Protocol) oder über das [Smooth Streaming](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming#Microsoft_Smooth_Streaming)-Protokoll (fragmentiertes MP4).  

Sobald das **LiveEvent** Daten aus dem Feed empfängt, können Sie über den zugehörigen Vorschauendpunkt (die Vorschau-URL) eine Vorschau anzeigen und den Empfang des Livestreams bestätigen, bevor Sie mit der Veröffentlichung fortfahren. Nachdem Sie sich von der Qualität des Vorschaustreams überzeugt haben, können Sie das LiveEvent verwenden, um den Livestream über einen oder mehrere (vorab erstellte) **StreamingEndpoints** zur Übertragung verfügbar zu machen. Hierzu erstellen Sie ein neues [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs)-Objekt für das **LiveEvent**. 

Das **LiveOutput**-Objekt verhält sich wie ein Tonbandgerät, das den Livestream erfasst und in einem Medienobjekt in Ihrem Media Services-Konto aufzeichnet. Der aufgezeichnete Inhalt wird in dem Ihrem Konto zugeordneten Azure Storage-Konto dauerhaft in dem Container gespeichert, der über die Medienobjektressource definiert wurde.  Mit dem **LiveOuput**-Objekt können Sie auch einige Eigenschaften des ausgehenden Livestreams steuern, z.B. wie viel vom Stream in der Archivaufzeichnung gespeichert wird (beispielsweise die Kapazität des Cloud-DVR). Das Archiv für den Datenträger ist ein zirkuläres „Archivfenster“, das nur die Menge an Inhalten umfasst, die in der Eigenschaft **archiveWindowLength** des **LiveOutput**-Objekts angegeben ist. Inhalt außerhalb dieses Fensters wird automatisch aus dem Speichercontainer entfernt und ist nicht wiederherstellbar. Sie können mehrere LiveOutput-Objekte (bis zu drei) mit unterschiedlichen Archivlängen und Einstellungen für ein LiveEvent erstellen.  

Mit Media Services können Sie die **dynamische Paketierung** nutzen, mit der Sie Ihre Livestreams im [MPEG DASH-, HLS- und Smooth Streaming-Format](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) aus dem an den Dienst gesendeten Beitragsfeed als Vorschau anzeigen und übertragen können. Ihre Zuschauer können den Livestream mit einem beliebigen Player wiedergeben, der mit HLS, DASH oder Smooth Streaming kompatibel ist. Sie können den [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) in Ihren Webanwendungen oder mobilen Anwendungen nutzen, um Ihren Stream mit einem beliebigen dieser Protokolle bereitzustellen.

Mit Media Services können Sie Ihre zu übermittelnden Inhalte mithilfe der (**dynamischen Verschlüsselung**) mit Advanced Encryption Standard (AES-128) oder einem der drei wichtigsten DRM-Systeme (Digital Rights Management, Verwaltung digitaler Rechte) verschlüsseln: Microsoft PlayReady, Google Widevine und Apple FairPlay. Media Services bietet außerdem einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen an autorisierte Clients. Weitere Informationen zum Verschlüsseln Ihrer Inhalte mit Media Services finden Sie unter [Übersicht über den Inhaltsschutz](content-protection-overview.md).

Auf Wunsch können Sie auch die dynamische Filterung anwenden, mit der Sie die Anzahl von Spuren, Formaten und Bitraten sowie die Präsentationszeitfenster steuern können, die an die Player gesendet werden. 

### <a name="new-capabilities-for-live-streaming-in-v3"></a>Neue Funktionen für das Livestreaming in v3

Mit den Media Services v3-APIs profitieren Sie von den folgenden neuen Features:

- Neue Modus für geringe Latenz. Weitere Informationen finden Sie unter [Latenz](live-event-latency.md).
- Verbesserte RTMP-Unterstützung (höhere Stabilität und bessere Unterstützung für Quellcodierer).
- Sichere RTMPS-Erfassung.<br/>Bei der Erstellung eines Liveereignisses erhalten Sie vier Erfassungs-URLs. Die vier Erfassungs-URLs sind nahezu identisch und verfügen über das gleiche Streamingtoken (AppId). Nur der Portnummernteil unterscheidet sich. Zwei der URLs dienen als primäre URL und Backup-URL für RTMPS.   
- Wenn Media Services zum Transcodieren eines Beitragsfeeds mit Einzelbitrate in einen Ausgabestream mit mehreren Bitraten verwendet wird, können Sie Liveereignisse streamen, die bis zu 24 Stunden lang sind. 

## <a name="liveevent-types"></a>LiveEvent-Typen

Für ein [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) ist einer von zwei Typen möglich: Pass-Through und Livecodierung. 

### <a name="pass-through"></a>Pass-Through

![Pass-Through](./media/live-streaming/pass-through.png)

Wenn Sie das LiveEvent vom Typ „Pass-Through“ verwenden, stützen Sie sich auf Ihren lokalen Liveencoder, um einen Videostream mit mehreren Bitraten zu erzeugen und diesen als Beitragsfeed an das LiveEvent zu senden (über RTMP oder das Protokoll für fragmentiertes MP4). Das LiveEvent leitet dann die eingehenden Videostreams ohne weitere Bearbeitung weiter. Ein LiveEvent vom Typ „Pass-Through“ ist für Liveereignisse mit langer Laufzeit oder für ein lineares 24x365-Livestreaming optimiert. Geben Sie beim Erstellen dieses LiveEvent-Typs „None“ (LiveEventEncodingType.None) an.

Sie können den Beitragsfeed mit einer Auflösung von bis zu 4K und einer Bildrate von 60 Bildern/Sekunde senden, entweder mit H.264/AVC- oder H.265/HEVC-Videocodecs und AAC-Audiocodec (AAC-LC, HE-AACv1 oder HE-AACv2).  Ausführliche Informationen finden Sie im Artikel zu [LiveEvent-Typvergleich und Einschränkungen](live-event-types-comparison.md).

> [!NOTE]
> Die Verwendung der Pass-Through-Methode ist die wirtschaftlichste Form des Livestreamings, wenn mehrere Ereignisse über einen längeren Zeitraum gestreamt werden und Sie bereits in lokale Encoder investiert haben. Preisdetails finden Sie [hier](https://azure.microsoft.com/pricing/details/media-services/) .
> 

Ein Livebeispiel finden Sie in [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live Encoding  

![Livecodierung](./media/live-streaming/live-encoding.png)

Wenn Sie die Livecodierung mit Media Services verwenden, konfigurieren Sie Ihren lokalen Liveencoder so, dass er ein Video mit einer einzelnen Bitrate als Beitragsfeed an das LiveEvent sendet (über RTMP oder das Protokoll für fragmentiertes MP4). Das LiveEvent codiert diesen eingehenden Stream mit einer einzelnen Bitrate in einen [Videostream mit mehreren Bitraten](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) und stellt ihn über Protokolle wie MPEG-DASH, HLS und Smooth Streaming für Wiedergabegeräte zur Verfügung. Geben Sie beim Erstellen dieses LiveEvent-Typs den Codierungstyp **Basic** (LiveEventEncodingType.Basic) an.

Sie können den Beitragsfeed mit einer Auflösung von bis zu 1080p und einer Bildrate von 30 Bildern/Sekunde senden, mit den H.264/AVC- oder H.265/HEVC-Videocodecs und dem AAC-Audiocodec (AAC-LC, HE-AACv1 oder HE-AACv2). Ausführliche Informationen finden Sie im Artikel zu [LiveEvent-Typvergleich und Einschränkungen](live-event-types-comparison.md).

## <a name="liveevent-types-comparison"></a>Vergleich der LiveEvent-Typen

Der folgende Artikel enthält eine Tabelle zum Vergleich der Features der zwei LiveEvent-Typen: [Vergleich](live-event-types-comparison.md).

## <a name="liveoutput"></a>LiveOutput

[LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) ermöglicht es Ihnen, die Eigenschaften des ausgehenden Livestreams zu steuern, z.B. wie viel des Streams aufgezeichnet wird (beispielsweise die Kapazität des Cloud-DVR) und ob die Zuschauer mit der Wiedergabe des Livestreams beginnen können oder nicht. Die Beziehung zwischen einem **LiveEvent** und dem zugehörigen **LiveOutput** ist ähnlich wie bei einer traditionellen Fernsehsendung, bei der ein Kanal (**LiveEvent**) einen konstanten Videodatenstrom darstellt und eine Aufzeichnung (**LiveOutput**) auf ein bestimmtes Zeitsegment (z.B. Abendnachrichten von 18:30 bis 19:00 Uhr) ausgerichtet ist. Sie können Fernsehsendungen mit einem digitalen Videorecorder (DVR) aufnehmen – die entsprechende Funktion in LiveEvents wird über die ArchiveWindowLength-Eigenschaft verwaltet. Es handelt sich um eine ISO-8601-Zeitspanne (z.B. PTHH:MM:SS), die die Kapazität des Digitalrecorders angibt. Diese kann von mindestens 3 Minuten auf bis zu maximal 25 Stunden eingestellt werden.


> [!NOTE]
> **LiveOutput** wird bei der Erstellung gestartet und beim Löschen beendet. Wenn Sie den **LiveOutput** löschen, werden das zugrunde liegende **Medienobjekt** und sein Inhalt nicht gelöscht.  

Weitere Informationen finden Sie unter [Verwenden eines Cloud-DVR](live-event-cloud-dvr.md).

## <a name="streamingendpoint"></a>StreamingEndpoint

Wenn der Stream an das **LiveEvent** übertragen wird, können Sie das Streamingereignis starten, indem Sie ein **Medienobjekt**, einen **LiveOutput** und einen **StreamingLocator** erstellen. Dadurch wird der Stream archiviert und über den [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) für die Zuschauer verfügbar gemacht.

Beim Erstellen Ihres Media Services-Kontos wird dem Konto ein Standard-Streamingendpunkt im Zustand „Beendet“ hinzugefügt. Um mit dem Streamen Ihrer Inhalte zu beginnen und die dynamische Paketerstellung und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Zustand „Wird ausgeführt“ aufweisen.

## <a name="a-idbilling-liveevent-states-and-billing"></a><a id="billing" />LiveEvent-Zustandswerte und Abrechnung

Für ein LiveEvent beginnt die Berechnung von Gebühren, sobald der Zustand in **Wird ausgeführt** geändert wird. Damit für das LiveEvent keine Gebühren mehr anfallen, müssen Sie es beenden.

Ausführliche Informationen finden Sie unter [Zustandswerte und Abrechnung](live-event-states-billing.md).

## <a name="latency"></a>Latenz

Ausführliche Informationen zur LiveEvent-Latenz finden Sie unter [Latenz](live-event-latency.md).

## <a name="live-streaming-workflow"></a>Workflow für das Livestreaming

Nachfolgend werden die Schritte in einem Workflow für das Livestreaming aufgeführt:

1. Erstellen Sie ein LiveEvent.
2. Erstellen Sie ein neues Medienobjekt.
3. Erstellen Sie einen LiveOutput, und verwenden Sie den Namen des erstellten Medienobjekts.
4. Erstellen Sie eine Streamingrichtlinie und einen Inhaltsschlüssel, wenn Sie Ihren Inhalt mit DRM verschlüsseln möchten.
5. Falls Sie kein DRM verwenden, erstellen Sie einen Streaminglocator mit den integrierten Streamingrichtlinientypen.
6. Listen Sie die Pfade für die Streamingrichtlinie auf, um die zu verwendenden URLs zurückzugeben (diese sind deterministisch).
7. Rufen Sie den Hostnamen für den Streamingendpunkt ab, von dem aus Sie streamen möchten. 
8. Kombinieren Sie die URL aus Schritt 6 mit dem Hostnamen aus Schritt 7, um die vollständige URL zu erhalten.

Weitere Informationen finden Sie in diesem [Tutorial zum Livestreaming](stream-live-tutorial-with-api.md), das auf dem [.NET Core-Beispiel für Liveereignisse](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live) basiert.

## <a name="next-steps"></a>Nächste Schritte

- [Vergleich der LiveEvent-Typen](live-event-types-comparison.md)
- [Zustandswerte und Abrechnung](live-event-states-billing.md)
- [Latenz](live-event-latency.md)
