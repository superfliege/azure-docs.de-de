---
title: Azure Event Grid-Schemas für Media Services-Ereignisse
description: Beschreibt die Eigenschaften, die mit Azure Event Grid für Media Services-Ereignisse bereitgestellt werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: reference
ms.date: 08/17/2018
ms.author: juliako
ms.openlocfilehash: a6a6c459e170627d26aa1445f4f4dd193965fe70
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "42141588"
---
# <a name="azure-event-grid-schemas-for-media-services-events"></a>Azure Event Grid-Schemas für Media Services-Ereignisse

Dieser Artikel beschreibt die Schemas und Eigenschaften für Media Services-Ereignisse.

Eine Liste von Beispielskripts und Tutorials finden Sie unter [Media Services-Ereignisquelle](../../event-grid/event-sources.md#azure-subscriptions).

## <a name="available-event-types"></a>Verfügbare Ereignistypen

Media Services gibt die folgenden Ereignistypen aus:

| Ereignistypen | BESCHREIBUNG |
| ---------- | ----------- |
| Microsoft.Media.JobStateChange| Status der Auftragsänderungen. |
| Microsoft.Media.LiveEventConnectionRejected | Verbindungsversuch des Encoders wurde verweigert. |
| Microsoft.Media.LiveEventEncoderConnected | Encoder stellt die Verbindung mit dem Liveereignis her. |
| Microsoft.Media.LiveEventEncoderDisconnected | Verbindung des Encoders wird getrennt. |
| Microsoft.Media.LiveEventIncomingDataChunkDropped | Media-Server verwirft Datenblöcke, da es zu spät ist oder es einen überlappenden Zeitstempel gibt (Zeitstempel des neuen Datenblocks liegt vor der Endzeit des vorherigen Datenblocks). |
| Microsoft.Media.LiveEventIncomingStreamReceived | Media-Server empfängt den ersten Datenblock für jede Spur im Stream oder über die Verbindung. |
| Microsoft.Media.LiveEventIncomingStreamsOutOfSync | Media-Server hat erkannt, dass Audio- und Videostreams nicht synchron sind. Verwenden Sie dies als Warnung, da die Darstellung für den Benutzer möglicherweise nicht beeinträchtigt ist. |
| Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync | Media-Server hat erkannt, dass die beiden Videostreams von einem externen Encoder nicht mehr synchron sind. Verwenden Sie dies als Warnung, da die Darstellung für den Benutzer möglicherweise nicht beeinträchtigt ist. |
| Microsoft.Media.LiveEventIngestHeartbeat | Wird alle 20 Sekunden für jede Spur veröffentlicht, wenn ein Liveereignis ausgeführt wird. Bietet eine Zusammenfassung der Erfassungsintegrität. |
| Microsoft.Media.LiveEventTrackDiscontinuityDetected | Media-Server hat eine Diskontinuität in der eingehenden Spur erkannt. |

Es gibt zwei Kategorien für die **Liveereignisse**: Ereignisse auf Streamebene und Ereignisse auf Spurebene. 

Ereignisse auf Streamebene werden pro Stream oder Verbindung ausgelöst. Jedes Ereignis verfügt über einen Parameter `StreamId`, der die Verbindung oder den Stream identifiziert. Jeder Stream und jede Verbindung verfügt über mindestens eine Spur verschiedener Typen. Beispielsweise kann eine Verbindung von einem Encoder eine Audiospur und vier Videospuren umfassen. Die Streamereignistypen sind:

* LiveEventConnectionRejected
* LiveEventEncoderConnected
* LiveEventEncoderDisconnected

Ereignisse auf Spurebene werden pro Spur ausgelöst. Die Spurereignistypen sind:

* LiveEventIncomingDataChunkDropped
* LiveEventIncomingStreamReceived
* LiveEventIncomingStreamsOutOfSync
* LiveEventIncomingVideoStreamsOutOfSync
* LiveEventIngestHeartbeat
* LiveEventTrackDiscontinuityDetected

## <a name="jobstatechange"></a>JobStateChange

Das folgende Beispiel zeigt das Schema des **JobStateChange**-Ereignisses: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
    "eventType": "Microsoft.Media.JobStateChange",
    "eventTime": "2018-04-20T21:26:13.8978772",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "previousState": "Processing",
      "state": "Finished"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| previousState | Zeichenfolge | Der Status des Auftrags vor dem Ereignis. |
| state | Zeichenfolge | Der neue Status des Auftrags, über den in diesem Ereignis eine Benachrichtigung erfolgt. Beispiele: „Queued: The Job is awaiting resources“ (In der Warteschlange: Der Auftrag wartet auf Ressourcen) oder „Scheduled: The job is ready to start“ (Geplant: Der Auftrag ist bereit für den Start).|

Dabei kann der Auftragsstatus einer der folgenden Werte sein: *Queued* (In der Warteschlange), *Scheduled* (Geplant), *Processing* (Verarbeitung), *Finished* (Abgeschlossen), *Error* (Fehler), *Canceled* (Abgebrochen), *Canceling* (Wird abgebrochen).

## <a name="liveeventconnectionrejected"></a>LiveEventConnectionRejected

Das folgende Beispiel zeigt das Schema des **LiveEventConnectionRejected**-Ereignisses: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventConnectionRejected",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "b303db59-d5c1-47eb-927a-3650875fded1",
    "data": { 
      "StreamId":"Mystream1",
      "IngestUrl": "http://abc.ingest.isml",
      "EncoderIp": "118.238.251.xxx",
      "EncoderPort": 52859,
      "ResultCode": "MPE_INGEST_CODEC_NOT_SUPPORTED"
    },
    "dataVersion": "1.0"
  }
]
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| StreamId | Zeichenfolge | Bezeichner des Streams oder der Verbindung. Der Encoder bzw. der Kunde ist dafür verantwortlich, diese ID in die Erfassungs-URL einzufügen. |  
| IngestUrl | Zeichenfolge | Erfassungs-URL, die vom Liveereignis bereitgestellt wird. |  
| EncoderIp | Zeichenfolge | IP-Adresse des Encoders. |
| EncoderPort | Zeichenfolge | Port des Encoders, von dem dieser Stream stammt. |
| ResultCode | Zeichenfolge | Der Grund, aus dem die Verbindung abgelehnt wurde. Diese Ergebniscodes werden in der folgenden Tabelle aufgeführt. |

Die Ergebniscodes sind:

| Ergebniscode | BESCHREIBUNG |
| ----------- | ----------- |
| MPE_RTMP_APPID_AUTH_FAILURE | Falsche Erfassungs-URL |
| MPE_INGEST_ENCODER_CONNECTION_DENIED | Encoder-IP-Adresse ist nicht in der konfigurierten Liste zugelassener IP-Adressen enthalten |
| MPE_INGEST_RTMP_SETDATAFRAME_NOT_RECEIVED | Encoder hat keine Metadaten zum Stream übermittelt. |
| MPE_INGEST_CODEC_NOT_SUPPORTED | Der angegebene Codec wird nicht unterstützt. |
| MPE_INGEST_DESCRIPTION_INFO_NOT_RECEIVED | Ein Fragment wurde vor dem Header für den Stream empfangen. |
| MPE_INGEST_MEDIA_QUALITIES_EXCEEDED | Anzahl der angegebenen Qualitäten überschreitet die zulässige maximale Anzahl. |
| MPE_INGEST_BITRATE_AGGREGATED_EXCEEDED | Aggregierte Bitrate überschreitet das maximal zulässige Limit. |
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | Der Zeitstempel für das Video- oder Audio-FLVTag vom RTMP-Encoder ist ungültig. |

## <a name="liveeventencoderconnected"></a>LiveEventEncoderConnected

Das folgende Beispiel zeigt das Schema des **LiveEventEncoderConnected**-Ereignisses: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderConnected",
    "eventTime": "2018-08-07T23:08:09.1710643",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| StreamId | Zeichenfolge | Bezeichner des Streams oder der Verbindung. Der Encoder bzw. der Kunde ist dafür verantwortlich, diese ID in die Erfassungs-URL bereitzustellen. |
| IngestUrl | Zeichenfolge | Erfassungs-URL, die vom Liveereignis bereitgestellt wird. |
| EncoderIp | Zeichenfolge | IP-Adresse des Encoders. |
| EncoderPort | Zeichenfolge | Port des Encoders, von dem dieser Stream stammt. |

## <a name="liveeventencoderdisconnected"></a>LiveEventEncoderDisconnected

Das folgende Beispiel zeigt das Schema des **LiveEventEncoderDisconnected**-Ereignisses: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderDisconnected",
    "eventTime": "2018-08-07T23:08:09.1710872",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "resultCode": "S_OK"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| StreamId | Zeichenfolge | Bezeichner des Streams oder der Verbindung. Der Encoder bzw. der Kunde ist dafür verantwortlich, diese ID in die Erfassungs-URL einzufügen. |  
| IngestUrl | Zeichenfolge | Erfassungs-URL, die vom Liveereignis bereitgestellt wird. |  
| EncoderIp | Zeichenfolge | IP-Adresse des Encoders. |
| EncoderPort | Zeichenfolge | Port des Encoders, von dem dieser Stream stammt. |
| ResultCode | Zeichenfolge | Der Grund für die Trennung der Verbindung mit dem Encoder. Die Trennung kann ordnungsgemäß oder verursacht durch einen Fehler erfolgen. Diese Ergebniscodes werden in der folgenden Tabelle aufgeführt. |

Die Fehlerergebniscodes sind:

| Ergebniscode | BESCHREIBUNG |
| ----------- | ----------- |
| MPE_RTMP_SESSION_IDLE_TIMEOUT | RTMP-Sitzungstimeout nach einem Leerlauf mit dem zulässigen Zeitlimit. |
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | Der Zeitstempel für das Video- oder Audio-FLVTag vom RTMP-Encoder ist ungültig. |
| MPE_CAPACITY_LIMIT_REACHED | Der Encoder sendet die Daten zu schnell. |
| Unbekannte Fehlercodes | Diese Fehlercodes können von zu wenig Arbeitsspeicher bis zu doppelten Einträgen in der Hashzuordnung reichen. |

Ergebniscodes für ordnungsgemäße Trennungen sind:

| Ergebniscode | BESCHREIBUNG |
| ----------- | ----------- |
| S_OK | Der Encoder hat die Verbindung erfolgreich getrennt. |
| MPE_CLIENT_TERMINATED_SESSION | Encoder getrennt (RTMP). |
| MPE_CLIENT_DISCONNECTED | Encoder getrennt (FMP4). |
| MPI_REST_API_CHANNEL_RESET | Befehl zum Zurücksetzen des Kanals wurde empfangen. |
| MPI_REST_API_CHANNEL_STOP | Befehl zum Beenden des Kanals wurde empfangen. |
| MPI_REST_API_CHANNEL_STOP | Kanal wird gerade gewartet. |
| MPI_STREAM_HIT_EOF | EOF-Stream wird vom Encoder gesendet. |

## <a name="liveeventincomingdatachunkdropped"></a>LiveEventIncomingDataChunkDropped

Das folgende Beispiel zeigt das Schema des **LiveEventIncomingDataChunkDropped**-Ereignisses: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingDataChunkDropped",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "03da9c10-fde7-48e1-80d8-49936f2c3e7d",
    "data": { 
      "TrackType": "Video",
      "TrackName": "Video",
      "Bitrate": 300000,
      "Timestamp": 36656620000,
      "Timescale": 10000000,
      "ResultCode": "FragmentDrop_OverlapTimestamp"
    },
    "dataVersion": "1.0"
  }
]
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| TrackType | Zeichenfolge | Spurtyp (Audio/Video) |
| TrackName | Zeichenfolge | Name der Spur |
| Bitrate | integer | Bitrate der Spur |
| Zeitstempel | Zeichenfolge | Zeitstempel des gelöschten Datenblocks |
| Zeitskala | Zeichenfolge | Zeitskala des Zeitstempels |
| ResultCode | Zeichenfolge | Grund für das Löschen des Datenblocks. **FragmentDrop_OverlapTimestamp** oder **FragmentDrop_NonIncreasingTimestamp**. |

## <a name="liveeventincomingstreamreceived"></a>LiveEventIncomingStreamReceived

Das folgende Beispiel zeigt das Schema des **LiveEventIncomingStreamReceived**-Ereignisses: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamReceived",
    "eventTime": "2018-08-07T23:08:10.5069288Z",
    "id": "7f939a08-320c-47e7-8250-43dcfc04ab4d",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml/Streams(15864-stream0)15864-stream0",
      "trackType": "video",
      "trackName": "video",
      "bitrate": 2962000,
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "timestamp": "15336831655032322",
      "duration": "20000000",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| TrackType | Zeichenfolge | Spurtyp (Audio/Video) |
| TrackName | Zeichenfolge | Name der Spur (entweder vom Encoder oder (bei RTMP) vom Server im Format *SpurTyp_Bitrate* generiert). |
| Bitrate | integer | Bitrate der Spur |
| IngestUrl | Zeichenfolge | Erfassungs-URL, die vom Liveereignis bereitgestellt wird. |
| EncoderIp | Zeichenfolge  | IP-Adresse des Encoders. |
| EncoderPort | Zeichenfolge | Port des Encoders, von dem dieser Stream stammt. |
| Zeitstempel | Zeichenfolge | Erster Zeitstempel des empfangenen Datenblocks |
| Zeitskala | Zeichenfolge | Zeitskala für die Darstellung des Zeitstempels |

## <a name="liveeventincomingstreamsoutofsync"></a>LiveEventIncomingStreamsOutOfSync

Das folgende Beispiel zeigt das Schema des **LiveEventIncomingStreamsOutOfSync**-Ereignisses: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamsOutOfSync",
    "eventTime": "2018-08-10T02:26:20.6269183Z",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "minLastTimestamp": "319996",
      "typeOfStreamWithMinLastTimestamp": "Audio",
      "maxLastTimestamp": "366000",
      "typeOfStreamWithMaxLastTimestamp": "Video"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| MinLastTimestamp | Zeichenfolge | Minimum der letzten Zeitstempel für alle Spuren (Audio oder Video). |
| TypeOfTrackWithMinLastTimestamp | Zeichenfolge | Der Typ der Spur (Audio oder Video) mit dem niedrigsten letzten Zeitstempel. |
| MaxLastTimestamp | Zeichenfolge | Maximum aller Zeitstempel in allen Spuren (Audio oder Video). |
| TypeOfTrackWithMaxLastTimestamp | Zeichenfolge | Der Typ der Spur (Audio oder Video) mit dem höchsten letzten Zeitstempel. |

## <a name="liveeventincomingvideostreamsoutofsync"></a>LiveEventIncomingVideoStreamsOutOfSync

Das folgende Beispiel zeigt das Schema des **LiveEventIncomingVideoStreamsOutOfSync**-Ereignisses: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/LiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "6dd4d862-d442-40a0-b9f3-fc14bcf6d750",
    "data": {
      "FirstTimestamp": "2162058216",
      "FirstDuration": "2000",
      "SecondTimestamp": "2162057216",
      "SecondDuration": "2000"
    },
    "dataVersion": "1.0"
  }
]
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| FirstTimestamp | Zeichenfolge | Zeitstempel, der für eine der Spuren/Qualitätsstufen vom Typ Video empfangen wurde. |
| FirstDuration | Zeichenfolge | Dauer des Datenblocks mit dem ersten Zeitstempel. |
| SecondTimestamp | Zeichenfolge  | Zeitstempel, der für einige andere Spuren/Qualitätsstufen vom Typ Video empfangen wurde. |
| SecondDuration | Zeichenfolge | Dauer des Datenblocks mit dem zweiten Zeitstempel. |

## <a name="liveeventingestheartbeat"></a>LiveEventIngestHeartbeat

Das folgende Beispiel zeigt das Schema des **LiveEventIngestHeartbeat**-Ereignisses: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIngestHeartbeat",
    "eventTime": "2018-08-07T23:17:57.4610506",
    "id": "7f450938-491f-41e1-b06f-c6cd3965d786",
    "data": {
      "trackType": "audio",
      "trackName": "audio",
      "bitrate": 160000,
      "incomingBitrate": 155903,
      "lastTimestamp": "15336837535253637",
      "timescale": "10000000",
      "overlapCount": 0,
      "discontinuityCount": 0,
      "nonincreasingCount": 0,
      "unexpectedBitrate": false,
      "state": "Running",
      "healthy": true
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| TrackType | Zeichenfolge | Spurtyp (Audio/Video) |
| TrackName | Zeichenfolge | Name der Spur (entweder vom Encoder oder (bei RTMP) vom Server im Format *SpurTyp_Bitrate* generiert). |
| Bitrate | integer | Bitrate der Spur |
| IncomingBitrate | integer | Berechnete Bitrate basierend auf den Datenblöcken vom Encoder. |
| LastTimestamp | Zeichenfolge | Neuester Zeitstempel, der für eine Spur in den letzten 20 Sekunden empfangen wurde. |
| Zeitskala | Zeichenfolge | Zeitskala für die Darstellung der Zeitstempel. |
| OverlapCount | integer | Anzahl der Datenblöcke mit überlappenden Zeitstempeln in den letzten 20 Sekunden. |
| DiscontinuityCount | integer | Anzahl von Diskontinuitäten in den letzten 20 Sekunden. |
| NonIncreasingCount | integer | Anzahl der Datenblöcke mit Zeitstempeln in der Vergangenheit, die in den letzten 20 Sekunden empfangen wurden. |
| UnexpectedBitrate | bool | Die erwarteten und tatsächlichen Bitraten in den letzten 20 Sekunden unterscheiden sich um mehr als das maximal zulässige Limit. TRUE, wenn (und nur wenn) IncomingBitrate >= 2 * Bitrate OR IncomingBitrate <= Bitrate / 2 OR IncomingBitrate = 0 gilt. |
| State (Zustand) | Zeichenfolge | Zustand des Liveereignisses. |
| Healthy | bool | Gibt an, ob die Erfassung hinsichtlich Anzahl und Flags fehlerfrei ist. Healthy ist TRUE, wenn OverlapCount = 0 && DiscontinuityCount = 0 && NonIncreasingCount = 0 && UnexpectedBitrate = false gilt. |

## <a name="liveeventtrackdiscontinuitydetected"></a>LiveEventTrackDiscontinuityDetected

Das folgende Beispiel zeigt das Schema des **LiveEventTrackDiscontinuityDetected**-Ereignisses: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventTrackDiscontinuityDetected",
    "eventTime": "2018-08-07T23:18:06.1270405Z",
    "id": "5f4c510d-5be7-4bef-baf0-64b828be9c9b",
    "data": {
      "trackName": "video",
      "previousTimestamp": "15336837615032322",
      "trackType": "video",
      "bitrate": 2962000,
      "newTimestamp": "15336837619774273",
      "discontinuityGap": "575284",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| TrackType | Zeichenfolge | Spurtyp (Audio/Video) |
| TrackName | Zeichenfolge | Name der Spur (entweder vom Encoder oder (bei RTMP) vom Server im Format *SpurTyp_Bitrate* generiert). |
| Bitrate | integer | Bitrate der Spur |
| PreviousTimestamp | Zeichenfolge | Zeitstempel des vorherigen Fragments. |
| NewTimestamp | Zeichenfolge | Zeitstempel des aktuellen Fragments. |
| DiscontinuityGap | Zeichenfolge | Lücke zwischen den beiden obigen Zeitstempeln. |
| Zeitskala | Zeichenfolge | Zeitskala für die Darstellung von Zeitstempel und Diskontinuitätslücke. |

## <a name="common-event-properties"></a>Allgemeine Ereigniseigenschaften

Ein Ereignis weist die folgenden Daten auf oberster Ebene aus:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| Thema | Zeichenfolge | Das EventGrid-Thema. Diese Eigenschaft enthält die Ressourcen-ID für das Media Services-Konto. |
| subject | Zeichenfolge | Ressourcenpfad für den Media Services-Kanal unter dem Media Services-Konto. Durch Verketten von topic und subject erhalten Sie die Ressourcen-ID für den Auftrag. |
| eventType | Zeichenfolge | Einer der registrierten Ereignistypen für die Ereignisquelle. Beispiel: “Microsoft.Media.JobStateChange“. |
| eventTime | Zeichenfolge | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | Zeichenfolge | Eindeutiger Bezeichner für das Ereignis. |
| data | object | Media Services-Ereignisdaten. |
| dataVersion | Zeichenfolge | Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion. |
| metadataVersion | Zeichenfolge | Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt. |

## <a name="next-steps"></a>Nächste Schritte

[Registrieren für Statusänderungsereignisse von Aufträgen](job-state-events-cli-how-to.md)