---
title: Informationen zu von Azure Media Services empfohlenen Encodern | Microsoft-Dokumentation
description: Informationen zu von Media Services empfohlenen Encodern
services: media-services
keywords: Codierung; Encoder; Medien
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: d0c5536d2339470eac058250cc14e1f250b86d90
ms.sourcegitcommit: 09a2485ce249c3ec8204615ab759e3b58c81d8cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2018
---
# <a name="recommended-on-premises-encoders"></a>Empfohlene lokale Encoder
Beim Livestreaming mit Azure Media Services können Sie angeben, wie der Kanal den Eingabedatenstrom empfangen soll. Wenn Sie sich für die Verwendung eines lokalen Encoders mit einem Livecodierkanal entscheiden, sollte der Encoder einen qualitativ hochwertigen Datenstrom mit Einzelbitrate als Ausgabe übertragen. Ziehen Sie dagegen einen lokalen Encoder mit Pass-Through-Kanal vor, dann muss Ihr Encoder einen Stream mit Mehrfachbitrate als Ausgabe übertragen. Weitere Informationen finden Sie unter [Livestreaming mit lokalen Encodern, die Datenströme mit Mehrfachbitrate erstellen](media-services-live-streaming-with-onprem-encoders.md).

Azure Media Services empfiehlt die Verwendung eines der nachfolgenden Live-Encoder mit RTMP-Ausgabe:
- Adobe Flash Media Live Encoder 3.2
- Haivision Makito X HEVC
- Telestream Wirecast 8.1
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4

Azure Media Services empfiehlt die Verwendung eines der nachfolgenden Live-Encoder mit Smooth Streaming-Ausgabe mit Mehrfachbitrate:
- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live
- Envivio-4Caster C4 Gen III
- Imagine Communications Selenio MCP3
- Media Excel Hero Live

> [!NOTE]
> Ein Live-Encoder kann einen Datenstrom mit Einzelbitrate an einen Pass-Through-Kanal senden, doch wird von dieser Konfiguration abgeraten, weil in diesem Fall ein Streaming mit adaptiver Bitrate auf Clientseite nicht möglich ist.

## <a name="how-to-become-an-on-prem-encoder-partner"></a>So werden Sie Partner für lokale Encoder
Wenn Sie Azure Media Services-Partner für lokale Encoder werden, unterstützt Media Services Ihr Produkt, indem es Ihren Encoder Unternehmenskunden empfiehlt. Um Partner für lokale Encoder zu werden, müssen Sie die Kompatibilität Ihres lokalen Encoders mit Media Services bestätigen. Führen Sie hierzu die folgenden Überprüfungsschritte aus:

Pass-Through-Kanalüberprüfung
1. Erstellen Sie ein Azure Media Services-Konto bzw. melden Sie sich bei Ihrem Konto an.
2. Erstellen und starten Sie einen **Pass-Through-Kanal**.
3. Konfigurieren Sie Ihren Encoder so, dass er einen Livestream mit Mehrfachbitrate ausgibt.
4. Erstellen Sie ein veröffentlichtes Live-Ereignis.
5. Lassen Sie Ihren Live-Encoder ungefähr 10 Minuten lang laufen.
6. Beenden Sie das Live-Ereignis.
7. Vermerken Sie die Medienobjekt-ID, die veröffentlichte Streaming-URL für das Livearchiv und die Einstellungen sowie die verwendete Version Ihres Live-Encoders.
8. Setzen Sie jeweils nach Erstellen einer Probe den Kanalzustand zurück.
9. Wiederholen Sie die Schritte 3 bis 8 für alle Konfigurationen, die von Ihrem Encoder unterstützt werden (mit und ohne Werbesignalisierung oder Untertiteln sowie in verschiedenen Codiergeschwindigkeiten).

Livecodierkanal-Überprüfung
1. Erstellen Sie ein Azure Media Services-Konto bzw. melden Sie sich bei Ihrem Konto an.
2. Erstellen und starten Sie einen **Livecodierkanal**.
3. Konfigurieren Sie den Encoder für die Übertragung eines Livestreams mit Einzelbitrate.
4. Erstellen Sie ein veröffentlichtes Live-Ereignis.
5. Lassen Sie Ihren Live-Encoder ungefähr 10 Minuten lang laufen.
6. Beenden Sie das Live-Ereignis.
7. Vermerken Sie die Medienobjekt-ID, die veröffentlichte Streaming-URL für das Livearchiv und die Einstellungen sowie die verwendete Version Ihres Live-Encoders.
8. Setzen Sie jeweils nach Erstellen einer Probe den Kanalzustand zurück.
9. Wiederholen Sie die Schritte 3 bis 8 für alle Konfigurationen, die von Ihrem Encoder unterstützt werden (mit und ohne Werbesignalisierung oder Untertiteln sowie in verschiedenen Codiergeschwindigkeiten).

Überprüfung der Lebensdauer
1. Erstellen Sie ein Azure Media Services-Konto bzw. melden Sie sich bei Ihrem Konto an.
2. Erstellen und starten Sie einen **Pass-Through-Kanal**.
3. Konfigurieren Sie Ihren Encoder so, dass er einen Livestream mit Mehrfachbitrate ausgibt.
4. Erstellen Sie ein veröffentlichtes Live-Ereignis.
5. Lassen Sie Ihren Live-Encoder für mindestens eine Woche laufen.
6. Beenden Sie das Live-Ereignis.
7. Vermerken Sie die Medienobjekt-ID, die veröffentlichte Streaming-URL für das Livearchiv und die Einstellungen sowie die verwendete Version Ihres Live-Encoders.

Senden Sie Ihre vermerkten Einstellungen und die Live-Archivparameter abschließend an Media Services (per E-Mail an amsstreaming@microsoft.com). Nach Erhalt werden von Media Services Bestätigungstests mit den mit Ihrem Live-Encoder erstellten Proben durchgeführt. Setzen Sie sich bei Fragen zu dieser Vorgehensweise mit Media Services in Verbindung.