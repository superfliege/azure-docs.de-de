---
title: Definieren von Filtern in Azure Media Services
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
ms.date: 02/12/2019
ms.author: juliako
ms.openlocfilehash: 09de372ffdb48c00fde9a43c07f8f8b574462d1f
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2019
ms.locfileid: "56372935"
---
# <a name="define-account-filters-and-asset-filters"></a>Definieren von Konto- und Medienobjektfiltern  

Bei der Inhaltsbereitstellung für Ihre Kunden (Streaming von Liveereignissen oder Video on Demand) benötigen Ihre Kunden möglicherweise mehr Flexibilität als in der Manifestdatei für das Standardmedienobjekt beschrieben. Azure Media Services ermöglicht es Ihnen, Kontofilter und Medienobjektfilter für Ihre Inhalte zu definieren. 

Filter sind serverseitige Regeln, die Ihren Kunden Folgendes ermöglichen: 

- Wiedergabe von einzelnen Abschnitten eines Videos (anstelle des gesamten Videos) Beispiel: 
  - Reduzieren des Manifests, um einen Subclip eines Liveereignisses anzuzeigen („Filtern von Subclips“), oder
  - Kürzen des Starts eines Videos („Kürzen eines Videos“).
- Ausschließliche Bereitstellung der angegebenen Wiedergaben und/oder Sprachspuren, die vom Gerät für die Inhaltswiedergabe unterstützt werden („Filtern der Wiedergabe“) 
- Anpassen des Präsentationsfensters (DVR), um eine begrenzte Größe des DVR-Fensters im Player anzugeben („Anpassen des Präsentationsfensters“).

Media Services bietet [dynamische Manifeste](filters-dynamic-manifest-overview.md), die auf vordefinierten Filtern basieren. Sobald Sie Filter definiert haben, können Ihre Kunden diese in der Streaming-URL verwenden. Auf Streamingprotokolle mit adaptiver Bitrate können Filter angewandt werden: Apple HTTP Live Streaming (HLS), MPEG-DASH und Smooth Streaming.

Die folgende Tabelle zeigt einige Beispiele für URLs mit Filtern:

|Protokoll|Beispiel|
|---|---|
|HLS V4|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|HLS V3|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3,filter=myAccountFilter)`|
|MPEG DASH|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=myAssetFilter)`|

## <a name="define-filters"></a>Definieren von Filtern

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

## <a name="example"></a>Beispiel

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

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten Informationen zum programmgesteuerten Erstellen von Filtern.  

- [Erstellen von Filtern mit REST-APIs](filters-dynamic-manifest-rest-howto.md)
- [Erstellen von Filtern mit .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Erstellen von Filtern mit der CLI](filters-dynamic-manifest-cli-howto.md)

