---
title: Informationen zu den von Media Services empfohlenen lokalen Livestreaming-Encodern – Azure | Microsoft-Dokumentation
description: Erfahren Sie etwas über die von Media Services empfohlenen lokalen Livestreaming-Encoder.
services: media-services
keywords: Codierung; Encoder; Medien
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 12/14/2018
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 4d25e4cd94fec35f31594544b619aa054a35d58d
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2019
ms.locfileid: "54302339"
---
# <a name="recommended-live-streaming-encoders"></a>Empfohlene Livestreaming-Encoder

In Media Services repräsentiert ein [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) (Kanal) eine Pipeline zum Verarbeiten von Livestreaminginhalten. Es gibt zwei Arten, auf die Live-Eingabestreams von einem LiveEvent empfangen werden können:

* Von einem lokalen Liveencoder wird ein RTMP- oder Smooth Streaming-Datenstrom (fragmentiertes MP4) mit Mehrfachbitrate an das LiveEvent gesendet, das nicht für die Livecodierung mit Media Services aktiviert ist. Die erfassten Streams durchlaufen LiveEvents ohne weitere Verarbeitung. Diese Methode wird als **Pass-Through-Methode** bezeichnet. Ein Live-Encoder kann einen Datenstrom mit Einzelbitrate an einen Pass-Through-Kanal senden, doch wird von dieser Konfiguration abgeraten, weil in diesem Fall ein Streaming mit adaptiver Bitrate auf Clientseite nicht möglich ist.

  > [!NOTE]
  > Die Verwendung der Pass-Through-Methode ist die wirtschaftlichste Form des Livestreamings.

* Ein lokaler Liveencoder sendet einen Single-Bitrate-Datenstrom an das LiveEvent, das zum Ausführen der Livecodierung mit Media Services in einem der folgenden Formate aktiviert wurde: RTMP oder Smooth Streaming (fragmentiertes MP4). Vom LiveEvent wird dann eine Livecodierung des Eingabestreams mit Einzelbitrate in einen (adaptiven) Videostream mit Mehrfachbitrate ausgeführt.

Ausführliche Informationen zur Livecodierung mit Media Services finden Sie unter [Livestreaming mit Media Services v3](live-streaming-overview.md).

## <a name="live-encoders-that-output-rtmp"></a>Liveencoder mit RTMP-Ausgabe

Media Services empfiehlt die Verwendung eines der nachfolgenden Liveencoder mit RTMP-Ausgabe. Die unterstützten URL-Schemas sind `rtmp://` und `rtmps://`.

> [!NOTE]
 > Überprüfen Sie beim Streamen per RTMP in den Firewall- und/oder Proxyeinstellungen, ob die ausgehenden TCP-Ports 1935 und 1936 geöffnet sind.<br/>
 Überprüfen Sie beim Streamen per RTMPS in den Firewall- und/oder Proxyeinstellungen, ob die ausgehenden TCP-Ports 2935 und 2936 geöffnet sind.

- Adobe Flash Media Live Encoder 3.2
- Haivision KB
- Haivision Makito X HEVC
- OBS Studio
- Switcher Studio (iOS)
- Telestream Wirecast 8.1+
- Telestream Wirecast S
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- VMIX
- xStream

## <a name="live-encoders-that-output-fragmented-mp4"></a>Liveencoder mit Ausgabe im fragmentierten MP4-Format

Media Services empfiehlt die Verwendung eines der nachfolgenden Liveencoder, der Smooth Streaming mit Mehrfachbitrate (fragmentiertes MP4) ausgibt. Die unterstützten URL-Schemas sind `http://` und `https://`.

- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live
- Envivio-4Caster C4 Gen III
- Imagine Communications Selenio MCP3
- Media Excel Hero Live und Hero 4K (UHD/HEVC)

## <a name="how-to-become-an-on-premises-encoder-partner"></a>So werden Sie Partner für lokale Encoder

Wenn Sie Azure Media Services-Partner für lokale Encoder werden, unterstützt Media Services Ihr Produkt, indem es Ihren Encoder Unternehmenskunden empfiehlt. Um Partner für lokale Encoder zu werden, müssen Sie die Kompatibilität Ihres lokalen Encoders mit Media Services bestätigen. Führen Sie hierzu die folgenden Überprüfungsschritte aus:

### <a name="pass-through-liveevent-verification"></a>Überprüfung von Pass-Through-LiveEvents

1. Erstellen Sie ein Azure Media Services-Konto bzw. melden Sie sich bei Ihrem Konto an.
2. Erstellen und starten Sie ein **Pass-Through**-LiveEvent.
3. Konfigurieren Sie Ihren Encoder so, dass er einen Livestream mit Mehrfachbitrate ausgibt.
4. Erstellen Sie ein veröffentlichtes Liveereignis.
5. Lassen Sie Ihren Liveencoder ungefähr 10 Minuten lang laufen.
6. Beenden Sie das Liveereignis.
7. Starten Sie einen Streamingendpunkt, verwenden Sie einen Player wie [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html), um das archivierte Medienobjekt zu betrachten und sicherzustellen, dass die Wiedergabe in keiner der Qualitätsstufen sichtbare Störungen aufweist (oder beobachten und validieren Sie dies alternativ über die Vorschau-URL während der Livesitzung vor Schritt 6).
8. Vermerken Sie die Medienobjekt-ID, die veröffentlichte Streaming-URL für das Livearchiv und die Einstellungen sowie die verwendete Version Ihres Liveencoders.
9. Setzen Sie jeweils nach Erstellen einer Probe den LiveEvent-Zustand zurück.
10. Wiederholen Sie die Schritte 3 bis 9 für alle Konfigurationen, die von Ihrem Encoder unterstützt werden (mit und ohne Werbesignalisierung oder Untertiteln sowie in verschiedenen Codiergeschwindigkeiten).

### <a name="live-encoding-liveevent-verification"></a>Überprüfung von LiveEvents mit Livecodierung

1. Erstellen Sie ein Azure Media Services-Konto bzw. melden Sie sich bei Ihrem Konto an.
2. Erstellen und starten Sie ein **Livecodierungs**-LiveEvent.
3. Konfigurieren Sie den Encoder für die Übertragung eines Livestreams mit Einzelbitrate.
4. Erstellen Sie ein veröffentlichtes Liveereignis.
5. Lassen Sie Ihren Liveencoder ungefähr 10 Minuten lang laufen.
6. Beenden Sie das Liveereignis.
7. Starten Sie einen Streamingendpunkt, verwenden Sie einen Player wie [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html), um das archivierte Medienobjekt zu betrachten und sicherzustellen, dass die Wiedergabe in keiner der Qualitätsstufen sichtbare Störungen aufweist (oder beobachten und validieren Sie dies alternativ über die Vorschau-URL während der Livesitzung vor Schritt 6).
8. Vermerken Sie die Medienobjekt-ID, die veröffentlichte Streaming-URL für das Livearchiv und die Einstellungen sowie die verwendete Version Ihres Liveencoders.
9. Setzen Sie jeweils nach Erstellen einer Probe den LiveEvent-Zustand zurück.
10. Wiederholen Sie die Schritte 3 bis 9 für alle Konfigurationen, die von Ihrem Encoder unterstützt werden (mit und ohne Werbesignalisierung oder Untertiteln sowie in verschiedenen Codiergeschwindigkeiten).

### <a name="longevity-verification"></a>Überprüfung der Lebensdauer

1. Erstellen Sie ein Azure Media Services-Konto bzw. melden Sie sich bei Ihrem Konto an.
2. Erstellen und starten Sie einen **Pass-Through**-Kanal.
3. Konfigurieren Sie Ihren Encoder so, dass er einen Livestream mit Mehrfachbitrate ausgibt.
4. Erstellen Sie ein veröffentlichtes Liveereignis.
5. Lassen Sie Ihren Liveencoder für mindestens eine Woche laufen.
6. Verwenden Sie einen Player wie [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html), um das Livestreaming gelegentlich zu beobachten (oder archivierte Medienobjekte), um sicherzustellen, dass die Wiedergabe keine sichtbaren Störungen aufweist.
7. Beenden Sie das Liveereignis.
8. Vermerken Sie die Medienobjekt-ID, die veröffentlichte Streaming-URL für das Livearchiv und die Einstellungen sowie die verwendete Version Ihres Liveencoders.

Senden Sie zum Abschluss eine E-Mail mit Ihren aufgezeichneten Einstellungen und Livearchivparametern als Benachrichtigung darüber, dass alle automatischen Überprüfungen bestanden wurden, unter amsstreaming@microsoft.com an Azure Media Services. Schließen Sie auch Ihre Kontaktinformationen für nachfolgende Kontaktaufnahmen ein. Setzen Sie sich bei Fragen zu dieser Vorgehensweise mit dem Azure Media Services-Team in Verbindung.

## <a name="next-steps"></a>Nächste Schritte

[Livestreaming mit Media Services v3](live-streaming-overview.md)
