---
title: Filter und dynamische Azure Media Services-Manifeste | Microsoft-Dokumentation
description: In diesem Thema wird erläutert, wie Sie Filter erstellen, mit denen Ihre Kunden bestimmte Abschnitte eines Streams streamen können. Media Services erstellt dynamische Manifeste, um dieses selektive Streaming zu archivieren.
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
ms.date: 01/24/2019
ms.author: juliako
ms.openlocfilehash: 5b666551ed47852fe8653fff174589acc4bff348
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54912032"
---
# <a name="filters-and-dynamic-manifests"></a>Filter und dynamische Manifeste

Bei der Inhaltsbereitstellung für Ihre Kunden (Streaming von Liveereignissen oder Video on Demand) benötigen Ihre Kunden möglicherweise mehr Flexibilität als in der Manifestdatei für das Standardmedienobjekt beschrieben. Azure Media Services ermöglicht es Ihnen, Kontofilter und Medienobjektfilter für Ihre Inhalte zu definieren. 

Filter sind serverseitige Regeln, die Ihren Kunden Folgendes ermöglichen: 

- Wiedergabe von einzelnen Abschnitten eines Videos (anstelle des gesamten Videos) Beispiel: 

    - Verringern des Manifests, um einen Subclip eines Liveereignisses anzuzeigen („Filtern von Subclips“), oder
    - Kürzen des Starts eines Videos ("Kürzen eines Videos").

- Ausschließliche Bereitstellung der angegebenen Wiedergaben und/oder Sprachspuren, die vom Gerät für die Inhaltswiedergabe unterstützt werden („Filtern der Wiedergabe“) 
- Anpassen des Präsentationsfensters (DVR), um eine begrenzte Größe des DVR-Fensters im Player anzugeben („Anpassen des Präsentationsfensters“)

In diesem Thema werden die [Konzepte](#concepts) beschrieben und die [Filterdefinitionen](#definitions) vorgestellt. Anschließend erhalten Sie ausführlichere Informationen zu [gängigen Szenarien](#common-scenarios). Am Ende des Artikels finden Sie Links für die programmgesteuerte Erstellung von Filtern.  

## <a name="concepts"></a>Konzepte

### <a name="dynamic-manifests"></a>Dynamische Manifeste

Media Services bietet **dynamische Manifeste**, die auf vordefinierten [Filtern](#filters) basieren. Nachdem Sie Filter definiert haben, können Ihre Kunden sie zum Streamen einer spezifischen Wiedergabe oder bestimmter Subclips Ihres Videos verwenden. Dazu geben sie die Filter in der Streaming-URL an. Auf Streamingprotokolle mit adaptiver Bitrate können Filter angewandt werden: Apple HTTP Live Streaming (HLS), MPEG-DASH und Smooth Streaming. 

Die folgende Tabelle zeigt einige Beispiele für URLs mit Filtern:

|Protokoll|Beispiel|
|---|---|
|HLS V4|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|HLS V3|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3,filter=myAccountFilter)`|
|MPEG DASH|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=myAssetFilter)`|

> [!NOTE]
> Dynamische Manifeste nehmen keine Änderungen am Medienobjekt und dem Standardmanifest für dieses Medienobjekt vor. Ihre Kunden können auswählen, ob sie einen Stream mit oder ohne Filter anfordern. 
> 
> 

### <a name="manifest-files"></a>Manifestdateien

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

## <a name="defining-filters"></a>Definieren von Filtern

Es gibt zwei Typen von Filtern für Medienobjekte: 

* [Kontofilter](https://docs.microsoft.com/rest/api/media/accountfilters) (global): Diese Filter können auf jedes Medienobjekt im Azure Media Services-Konto angewendet werden und haben die Lebensdauer des Kontos.
* [Medienobjektfilter](https://docs.microsoft.com/rest/api/media/assetfilters) (lokal): Sie können nur auf ein Medienobjekt angewendet werden, dem der Filter bei der Erstellung zugeordnet wurde, und haben die Lebensdauer des Medienobjekts. 

Die Filtertypen [Kontofilter](https://docs.microsoft.com/rest/api/media/accountfilters) und [Medienobjektfilter](https://docs.microsoft.com/rest/api/media/assetfilters) umfassen dieselben Eigenschaften zur Definition/Beschreibung des Filters. Außer beim Erstellen des **Medienobjektfilters** müssen Sie den Namen des Medienobjekts angeben, dem Sie den Filter zuordnen möchten.

Abhängig vom Szenario entscheiden Sie, welcher Filtertyp (Medienobjektfilter oder Kontofilter) besser geeignet ist. Kontofilter eignen sich für Geräteprofile (Wiedergabefilterung), Medienobjektfilter hingegen können zum Kürzen eines bestimmten Medienobjekts eingesetzt werden.

Sie verwenden die folgenden Eigenschaften zum Beschreiben der Filter. 

|NAME|BESCHREIBUNG|
|---|---|
|firstQuality|Die erste Qualitätsstufe der Bitrate für den Filter.|
|presentationTimeRange|Der Bereich der Präsentationszeit. Diese Eigenschaft wird zum Filtern der Start-/Endpunkte des Manifests, der Länge des Präsentationsfensters und der Livestartposition verwendet. <br/>Weitere Informationen finden Sie unter [PresentationTimeRange](#PresentationTimeRange).|
|tracks|Die Bedingungen zur Spurauswahl. Weitere Informationen finden Sie unter [tracks](#tracks).|

### <a name="presentationtimerange"></a>PresentationTimeRange

Verwenden Sie diese Eigenschaft mit **Medienobjektfiltern**. Es wird nicht empfohlen, die Eigenschaft mit **Kontofiltern** festzulegen.

|NAME|BESCHREIBUNG|
|---|---|
|**endTimestamp**|Die Grenze für den absoluten Endpunkt. Gilt für Video on Demand (VoD). Bei einer Livepräsentation wird diese Eigenschaft stillschweigend ignoriert und angewendet, wenn die Präsentation endet und der Stream zu VoD wird.<br/><br/>Der Wert repräsentiert einen absoluten Endpunkt des Streams. Er wird auf den nächstgelegenen GOP-Start gerundet.<br/><br/>Verwenden Sie „StartTimestamp“ und „EndTimestamp“, um die Wiedergabeliste (das Manifest) zu kürzen. Beispielsweise wird durch die Festlegung „StartTimestamp=40000000“ und „EndTimestamp = 100000000“ eine Wiedergabeliste generiert, die Medien zwischen „StartTimestamp“ und „EndTimestamp“ enthält. Wenn ein Fragment die Grenze überschreitet, wird das gesamte Fragment in das Manifest aufgenommen.<br/><br/>Siehe auch die folgende Definition für **forceEndTimestamp**.|
|**forceEndTimestamp**|Gilt für Livefilter.<br/><br/>**forceEndTimestamp** ist ein boolescher Wert. Er gibt an, ob **endTimestamp** auf einen gültigen Wert festgelegt wurde oder nicht. <br/><br/>Lautet der Wert **true**, muss **endTimestamp** angegeben werden. Erfolgt keine Angabe, wird eine fehlerhafte Anforderung zurückgegeben.<br/><br/>Wenn Sie beispielsweise einen Filter definieren möchten, der bei Minute 5 des Eingangsvideos beginnt und bis zum Ende des Streams dauert, würden Sie **forceEndTimestamp** auf „false“ festlegen und die Einstellung **endTimestamp** auslassen.|
|**liveBackoffDuration**|Gilt nur für Liveereignisse. Die Eigenschaft wird verwendet, um die Position für die Livewiedergabe zu definieren. Mit dieser Regel können Sie die Livewiedergabeposition verzögern und einen serverseitigen Puffer für Spieler erstellen. „LiveBackoffDuration“ ist relativ zur Liveposition. Der maximal zulässige Wert für „LiveBackoffDuration“ beträgt 300 Sekunden.|
|**presentationWindowDuration**|Gilt für Liveereignisse. Verwenden Sie **presentationWindowDuration**, um ein gleitendes Fenster auf die Wiedergabeliste anzuwenden. Legen Sie beispielsweise „presentationWindowDuration=1200000000“ fest, um ein gleitendes Fenster von 2 Minuten anzuwenden. Medien, die in einem 2-Minuten-Zeitraum um die Liveposition liegen, werden in die Wiedergabeliste aufgenommen. Wenn ein Fragment die Grenze überschreitet, wird das gesamte Fragment in die Wiedergabeliste aufgenommen. Die Mindestdauer für das Präsentationsfenster beträgt 60 Sekunden.|
|**startTimestamp**|Gilt für VoD oder Livestreams. Der Wert repräsentiert einen absoluten Startpunkt des Streams. Er wird auf den nächstgelegenen GOP-Start gerundet.<br/><br/>Verwenden Sie **startTimestamp** und **endTimestamp**, um die Wiedergabeliste (das Manifest) zu kürzen. Beispielsweise wird durch die Festlegung „startTimestamp=40000000“ und „endTimestamp = 100000000“ eine Wiedergabeliste generiert, die Medien zwischen „StartTimestamp“ und „EndTimestamp“ enthält. Wenn ein Fragment die Grenze überschreitet, wird das gesamte Fragment in das Manifest aufgenommen.|
|**timescale**|Gilt für VoD oder Livestreams. Die Zeitskala, die von den oben angegebenen Zeitstempeln und Zeiträumen verwendet wird. Die standardmäßige Zeitskala lautet 10000000. Es kann eine alternative Zeitskala verwendet werden. Der Standard ist 10000000 HNS (hundert Nanosekunden).|

### <a name="tracks"></a>Spuren

Sie geben eine Liste von Eigenschaftsbedingungen für die Filterung nach Spuren an (FilterTrackPropertyConditions), anhand derer die Spuren Ihres Streams (Live oder Video on Demand) in das dynamisch erstellte Manifest aufgenommen werden sollen. Die Filter werden durch einen logischen **AND**- und **OR**-Vorgang kombiniert.

Spureigenschaftsbedingungen für die Filterung beschreiben Spurtypen, Werte (siehe nachstehende Tabelle) und Vorgänge („Equal“, „NotEqual“). 

|NAME|BESCHREIBUNG|
|---|---|
|**Bitrate**|Verwenden Sie die Bitrate der Spur zur Filterung.<br/><br/>Der empfohlene Wert ist ein Bereich aus Bitraten in Bits pro Sekunde. Beispiel: 0-2427000.<br/><br/>Hinweis: Sie können einen bestimmten Wert für die Bitrate angeben, z.B. 250000 (Bits pro Sekunde), dieser Ansatz wird jedoch nicht empfohlen, weil die exakten Bitraten je nach Medienobjekt unterschiedlich sein können.|
|**FourCC**|Verwenden Sie den FourCC-Wert der Spur für die Filterung.<br/><br/>Der Wert ist das erste Element des Codecs-Formats, wie angegeben in [RFC 6381](https://tools.ietf.org/html/rfc6381). Aktuell werden die folgenden Codecs unterstützt: <br/>Für Video: avc1, hev1, hvc1<br/>Für Audio: mp4a, ec-3<br/><br/>Um die FourCC-Wert für die Spuren in einem Medienobjekt zu bestimmen, müssen Sie die [Manifestdatei abrufen und untersuchen](#get-and-examine-manifest-files).|
|**Sprache**|Verwenden Sie die Sprache der Spur zur Filterung.<br/><br/>Der Wert ist das Tag einer Sprache, die Sie einschließen möchten, wie angegeben in RFC 5646. Beispiel: „en“.|
|**Name**|Verwenden Sie den Namen der Spur zur Filterung.|
|**Typ**|Verwenden Sie den Typ der Spur zur Filterung.<br/><br/>Die folgenden Werte sind zulässig: video, audio, text|

### <a name="example"></a>Beispiel

```json
{
  "properties": {
    "presentationTimeRange": {
      "startTimestamp": 0,
      "endTimestamp": 170000000,
      "presentationWindowDuration": 9223372036854776000,
      "liveBackoffDuration": 0,
      "timescale": 10000000,
      "forceEndTimestamp": false
    },
    "firstQuality": {
      "bitrate": 128000
    },
    "tracks": [
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Audio"
          },
          {
            "property": "Language",
            "operation": "NotEqual",
            "value": "en"
          },
          {
            "property": "FourCC",
            "operation": "NotEqual",
            "value": "EC-3"
          }
        ]
      },
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Video"
          },
          {
            "property": "Bitrate",
            "operation": "Equal",
            "value": "3000000-5000000"
          }
        ]
      }
    ]
  }
}
```

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
