---
title: Filter und dynamische Azure Media Services-Manifeste | Microsoft-Dokumentation
description: In diesem Thema wird erläutert, wie Sie Filter erstellen, mit denen Ihre Kunden bestimmte Abschnitte eines Streams streamen können. Media Services erstellt dynamische Manifeste, um dieses selektive Streaming zu erreichen.
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
ms.date: 02/19/2019
ms.author: juliako
ms.openlocfilehash: 3a496aa5dc08ac59fb51f8bf3010bd1edf1e605d
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447938"
---
# <a name="dynamic-manifests"></a>Dynamische Manifeste

Media Services bietet **dynamische Manifeste**, die auf vordefinierten Filtern basieren. Nachdem Sie Filter definiert haben (siehe [Definieren von Filtern](filters-concept.md)), können Ihre Kunden sie zum Streamen einer spezifischen Wiedergabe oder bestimmter Subclips Ihres Videos verwenden. Dazu geben sie die Filter in der Streaming-URL an. Auf Streamingprotokolle mit adaptiver Bitrate können Filter angewandt werden: Apple HTTP Live Streaming (HLS), MPEG-DASH und Smooth Streaming. 

Die folgende Tabelle zeigt einige Beispiele für URLs mit Filtern:

|Protokoll|Beispiel|
|---|---|
|HLS|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=myAssetFilter)`|


> [!NOTE]
> Dynamische Manifeste nehmen keine Änderungen am Medienobjekt und dem Standardmanifest für dieses Medienobjekt vor. Ihre Kunden können auswählen, ob sie einen Stream mit oder ohne Filter anfordern. 
> 

In diesem Thema werden Konzepte im Zusammenhang mit **dynamischen Manifesten** erläutert. Außerdem sind Beispiele für Szenarien enthalten, in denen Sie dieses Feature möglicherweise verwenden möchten.

## <a name="manifest-files-overview"></a>Übersicht über Manifestdateien

Wenn Sie ein Medienobjekt für das Streaming mit adaptiver Bitrate codieren, wird eine **Manifestdatei** (Wiedergabelistendatei) erstellt (die Datei ist textbasiert oder XML-basiert). Die **Manifestdatei** enthält Streamingmetadaten wie z. B. Typ (Audio, Video oder Text), Spurenname, Start-und Endzeit, Bitrate (Qualität), Sprachen, Präsentationsfenster (variables Fenster mit fester Dauer) und Videocodec (FourCC). Sie weist den Player zudem zum Abrufen des nächsten Fragments an, indem Informationen zu den nächsten für die Wiedergabe verfügbaren Videofragmenten und den zugehörigen Speicherorten angezeigt werden. Fragmente (oder Segmente) sind die eigentlichen „Blöcke“ von Videoinhalten.

Hier ein Beispiel für eine HLS-Manifestdatei: 

```
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(380658)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(380658)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721426)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721426)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1155246)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1155246)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2218559)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2218559)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579378)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579378)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

### <a name="get-and-examine-manifest-files"></a>Abrufen und Untersuchen von Manifestdateien

Sie geben eine Liste von Eigenschaftsbedingungen für die Filterung nach Spuren an, anhand derer die Spuren Ihres Streams (Live oder Video on Demand) in das dynamisch erstellte Manifest aufgenommen werden sollen. Um die Eigenschaften der Spuren abzurufen und zu untersuchen, müssen Sie zunächst das Smooth Streaming-Manifest laden.

Im Tutorial [Hochladen, Codieren und Streamen von Dateien mit .NET](stream-files-tutorial-with-api.md) erfahren Sie, wie Sie die Streaming-URLs mit .NET erstellen (siehe Abschnitt [Erstellen von URLs](stream-files-tutorial-with-api.md#get-streaming-urls)). Wenn Sie die App ausführen, zeigt einer der URLs auf das Smooth Streaming-Manifest: `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`.<br/>Kopieren Sie die URL, und fügen Sie sie in die Adressleiste eines Browsers ein. Die Datei wird heruntergeladen. Sie können die Datei in einem Text-Editor Ihrer Wahl öffnen.

Das REST-Beispiel finden Sie unter [Hochladen, Codieren und Streamen von Dateien mit REST](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls).

### <a name="monitor-the-bitrate-of-a-video-stream"></a>Überwachen der Bitrate eines Videostreams

Sie können mithilfe der [Azure Media Player-Demoseite](http://aka.ms/amp) die Bitrate eines Videostreams überwachen. Die Demoseite zeigt auf der Registerkarte **Diagnose** Diagnoseinformationen an:

![Azure Media Player-Diagnose][amp_diagnostics]

## <a name="rendition-filtering"></a>Filtern der Wiedergabe

Sie haben die Möglichkeit, ein Medienobjekt in mehreren Codierungsprofilen (H.264 Baseline, H.264 High, AACL, AACH, Dolby Digital Plus) und mit Bitraten für unterschiedliche Qualität zu codieren. Die Profile und Bitraten des Medienobjekts werden jedoch nicht auf allen Clientgeräten unterstützt. Auf älteren Android-Geräten wird beispielsweise nur H.264 Baseline+AACL unterstützt. Wenn höhere Bitraten an ein Gerät gesendet werden, auf dem daraus kein Nutzen gezogen werden kann, werden Bandbreite und Rechenressourcen des Geräts vergeudet. Diese Geräte müssen alle angegebenen Informationen decodieren, um sie dann für die Anzeige herunterzuskalieren.

Mit dynamischen Manifesten können Sie beispielsweise Geräteprofile für Mobiltelefone, Konsolen oder HD/SD erstellen und die Spuren und die Qualität einfügen, die den einzelnen Profilen zugeordnet sein sollen.

![Beispiel für das Filtern der Wiedergabe][renditions2]

Im folgenden Beispiel wurde ein Encoder zum Codieren eines Zwischenassets in sieben ISO-MP4s-Videowiedergaben (von 180p bis 1080p) verwendet. Das codierte Medienobjekt kann dynamisch in eines der folgenden Streamingprotokolle gepackt werden: HLS, MPEG-DASH und Smooth.  Oben in der Abbildung ist das HLS-Manifest für das Medienobjekt ohne Filter dargestellt (es enthält alle sieben Wiedergaben).  Links unten ist das HLS-Manifest abgebildet, auf das der Filter "ott" angewendet wurde. Der Filter „ott“ gibt an, dass alle Bitraten unter 1 MBit/s entfernt werden. Dies führte dazu, dass zwei Qualitätsstufen in der Antwort entfernt wurden. Rechts unten ist das HLS-Manifest abgebildet, auf das der Filter „mobile“ angewandt wurde. Der Filter "mobile" gibt an, dass Wiedergaben mit einer größeren Auflösung als 720p entfernt werden. Dies führte dazu, dass zwei Wiedergaben mit 1080p gelöscht wurden.

![Filtern der Wiedergabe][renditions1]

## <a name="removing-language-tracks"></a>Entfernen von Sprachspuren
Ihre Medienobjekte können mehrere Audiosprachen enthalten, z. B. Englisch, Spanisch, Französisch usw. In der Regel verwaltet das Player-SDK die Auswahl der Standardaudiospur und verfügbare Audiospuren entsprechend der Benutzerauswahl. Es ist schwierig, solche Player-SDKs zu entwickeln. Sie erfordern verschiedene Implementierungen in gerätespezifischen Player-Frameworks. Außerdem sind die Player-APIs auf manchen Plattformen eingeschränkt und umfassen keine Funktion zur Audioauswahl, sodass Benutzer die Standardaudiospur nicht auswählen oder ändern können. Mit Filtern für Medienobjekte können Sie das Verhalten steuern, indem Sie Filter erstellen, die nur die gewünschten Audiosprachen enthalten.

![Filtern der Sprachspuren][language_filter]

## <a name="trimming-start-of-an-asset"></a>Kürzen des Starts eines Medienobjekts
Bei den meisten Live-Streaming-Ereignissen führen Operatoren vor dem eigentlichen Ereignis einige Tests durch. Sie fügen z.B. die folgende Meldung vor dem Start des Ereignisses ein: „Programm beginnt sofort“. Wenn das Programm archiviert wird, werden auch die Test- und Meldungsdaten archiviert und in die Präsentation einbezogen. Diese Informationen sollten jedoch nicht auf den Clients angezeigt werden. Mit dynamischen Manifesten können Sie einen Filter für die Startzeit erstellen und die unerwünschten Daten aus dem Manifest entfernen.

![Kürzen des Starts][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Erstellen von Subclips (Ansichten) aus einem Livearchiv
Viele Liveereignisse werden lange ausgeführt. Daher können Livearchive mehrere Ereignisse enthalten. Nach dem Ende des Liveereignisses möchten Sendeanstalten das Livearchiv möglicherweise in logische Sequenzen mit Programmstart und Programmende unterteilen. Anschließend sollen diese virtuellen Programme separat ohne Nachbearbeitung des Livearchivs veröffentlicht und keine separaten Medienobjekte erstellt werden (bei denen kein Nutzen aus den vorhandenen in den CDNs zwischengespeicherten Fragmenten gezogen wird). Beispiele für solche virtuellen Programme sind Halbzeiten eines Fußball- oder Basketballspiels, die Innings beim Baseball oder einzelne Wettbewerbe beliebiger Sportprogramme.

Mit dynamischen Manifesten können Sie Filter mit Start- und Endzeiten sowie virtuelle Ansichten für Ihr Livearchiv erstellen. 

![Filter für Subclip][subclip_filter]

Gefiltertes Medienobjekt:

![Skilaufen][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Anpassen des Präsentationsfensters (DVR)
Zum gegenwärtigen Zeitpunkt umfasst Azure Media Services Umlaufarchive, bei denen eine Dauer zwischen 5 Minuten und 25 Stunden konfiguriert werden kann. Durch Filter über Manifeste kann ein DVR-Gleitfenster über dem Archiv erstellt werden, ohne dass Medien gelöscht werden. Es gibt viele Szenarios, bei denen Sendeanstalten ein in der Größe begrenztes DVR-Fenster bereitstellen möchten, das sich am Rand des Livevideos bewegt, und gleichzeitig ein größeres Archivierungsfenster beibehalten möchten. Ein bestimmter Sender möchte eventuell die Daten verwenden, die sich außerhalb des DVR-Fensters befinden, um Clips hervorzuheben oder unterschiedliche DVR-Fenster für verschiedene Geräte bereitstellen. Auf den meisten Mobilgeräten werden beispielsweise keine großen DVR-Fenster verarbeitet. (Sie können ein 2-Minuten-Fenster für Mobilgeräte und ein 1-Stunden-Fenster für Desktopclients einrichten.)

![DVR-Fenster][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>Anpassen von LiveBackoff (Liveposition)
Mit der Filterung über Manifeste können mehrere Sekunden vom Rand eines Liveprogramms entfernt werden. Die Filterung gestattet es Sendeanstalten, die Präsentation am Vorschauveröffentlichungspunkt anzusehen und Einfügepunkte für Werbespots zu erstellen, bevor die Zuschauer den Stream empfangen (um 30 Sekunden zurückgehalten). Sendeanstalten können diese Werbespots dann an ihre Clientframeworks senden, sodass diese sie rechtzeitig empfangen und die Informationen vor der Ausstrahlung verarbeiten können.

Neben der Unterstützung für Werbespots kann mithilfe der LiveBackoff-Einstellung die Betrachterposition angepasst werden, sodass Clients anstelle der HTTP-Fehler 404 oder 412 weiterhin Fragmente empfangen, wenn sie den Rand des Livevideos erreichen.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Kombinieren mehrerer Regeln in einem Filter
Sie können mehrere Filterregeln in ein und demselben Filter kombinieren. Als Beispiel können Sie eine „Bereichsregel“ zum Entfernen von Meldungen aus einem Livearchiv definieren und zudem verfügbare Bitraten filtern. Wenn mehrere Filterregeln angewendet werden, ist das Endergebnis die Schnittmenge aller Regeln.

![mehrere Regeln][multiple-rules]

## <a name="combining-multiple-filters-filter-composition"></a>Kombinieren mehrerer Filter (Filterkomposition)

Sie können auch mehrere Filter in einer einzelnen URL kombinieren. 

Das folgende Szenario zeigt, warum es sinnvoll sein kann, Filter zu kombinieren:

1. Sie müssen Ihre Videoqualitäten für mobile Geräte wie Android oder iPAD filtern (um die Videoqualitäten einzuschränken). Zum Entfernen der unerwünschten Qualitätsstufen würden Sie einen Kontofilter erstellen, der für die Geräteprofile geeignet ist. Kontofilter können für alle Medienobjekte desselben Media Services-Konto ohne weitere Zuordnung verwendet werden. 
2. Sie möchten außerdem die Start- und Endzeit Zeit eines Medienobjekts kürzen. Zu diesem Zweck erstellen Sie einen Medienobjektfilter und legen die Start-/Endzeit fest. 
3. Sie möchten die beiden Filter kombinieren (ohne Kombination müssen Sie den Qualitätsfilter zum Kürzungsfilter hinzufügen, was die Verwendung des Filters erschweren würde).

Um Filter zu kombinieren, müssen Sie die Filternamen, durch Semikolons getrennt, zur Manifest-/Wiedergabelisten-URL hinzufügen. Angenommen, Sie verfügen über einen Filter mit dem Namen *MyMobileDevice* zum Filtern der Qualitäten und über einen weiteren Filter mit dem Namen *MyStartTime* zum Festlegen einer bestimmten Startzeit. Sie können diese Filter folgendermaßen kombinieren:

Sie können bis zu drei Filter kombinieren. 

Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) Blog.

## <a name="considerations-and-limitations"></a>Überlegungen und Einschränkungen

- Die Werte für **forceEndTimestamp**, **presentationWindowDuration** und **liveBackoffDuration** dürfen für einen VoD-Filter nicht festgelegt werden. Sie werden nur in Szenarien mit Livefiltern eingesetzt. 
- Dynamische Manifeste werden in GOP-Grenzen (Keyframes) ausgeführt, daher weisen Kürzungen GOP-Genauigkeit auf. 
- Sie können die gleichen Filternamen für Konto- und Medienobjektfilter verwenden. Medienobjektfilter haben Vorrang vor Kontofiltern und überschreiben diese.
- Wenn Sie einen Filter aktualisieren, kann es bis zu 2 Minuten dauern, bis die Regeln am Streamingendpunkt aktualisiert wurden. Wenn der Inhalt mit einigen Filtern verarbeitet (und in Proxys und CDN-Caches zwischengespeichert) wurde, können durch Aktualisieren dieser Filter Player-Fehler auftreten. Es wird empfohlen, den Cache nach dem Aktualisieren des Filters zu leeren. Wenn dies nicht möglich ist, empfiehlt sich die Verwendung eines anderen Filters.
- Kunden müssen das Manifest manuell herunterladen und den exakten Wert von „startTimestamp“ und Zeitskala analysieren.
    
    - Um die Eigenschaften der Spuren in einem Medienobjekt zu bestimmen, müssen Sie die [Manifestdatei abrufen und untersuchen](#get-and-examine-manifest-files).
    - Dies ist die Formel zum Festlegen der Zeitstempeleigenschaften für einen Medienobjektfilter: <br/>startTimestamp = &lt;Startzeit im Manifest&gt; +  &lt;erwartete Filterstartzeit in Sekunden&gt; × Zeitskala


## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten Informationen zum programmgesteuerten Erstellen von Filtern.  

- [Erstellen von Filtern mit REST-APIs](filters-dynamic-manifest-rest-howto.md)
- [Erstellen von Filtern mit .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Erstellen von Filtern mit der CLI](filters-dynamic-manifest-cli-howto.md)

[renditions1]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/filters-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/filters-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/filters-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/filters-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/filters-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/filters-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/filters-dynamic-manifest-overview/media-services-skiing.png
[amp_diagnostics]: ./media/filters-dynamic-manifest-overview/amp_diagnostics.png
