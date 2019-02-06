---
title: Übersicht über Livestreaming mit Azure Media Services | Microsoft Docs
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
ms.date: 01/27/2019
ms.author: juliako
ms.openlocfilehash: a3e4821d9deb7ceee815d804f58d0b1ba14925b4
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103563"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Livestreaming mit Azure Media Services v3

Mit Azure Media Services können Sie Ihren Kunden Liveereignisse in der Azure Cloud anbieten. Um Liveereignisse mit Media Services zu streamen, benötigen Sie Folgendes:  

- Eine Kamera, mit der das Liveereignis aufgenommen wird.<br/>Ausrüstungsanregungen finden Sie unter [Simple and Portable Event Video Gear Setup]( https://link.medium.com/KNTtiN6IeT) (Einfache und tragbare Videoausrüstung für Veranstaltungen).
- Ein Livevideoencoder, der Signale von einer Kamera (oder einem anderen Gerät, etwa einem Laptop) in Streams konvertiert, die an Media Services gesendet werden. Der Datenfeed kann Signale zu Werbezwecken enthalten, beispielsweise SCTE-35-Markierungen.<br/>Eine Liste mit empfohlenen Livestreaming-Encodern finden Sie [hier](recommended-on-premises-live-encoders.md). Sehen Sie sich auch den folgenden Blog an: [Live streaming production with OBS](https://link.medium.com/ttuwHpaJeT) (Livestreamproduktion mit OBS).
- Komponenten in Media Services, mit denen das Liveereignis erfasst, in einer Vorschau angezeigt, paketiert, aufgezeichnet, verschlüsselt und an Ihre Kunden übertragen oder zur weiteren Verteilung an ein CDN gesendet wird.

Mit Media Services können Sie die **dynamische Paketerstellung** nutzen, mit der Sie Ihre Livestreams im [MPEG DASH-, HLS- und Smooth Streaming-Format](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) aus dem an den Dienst gesendeten Beitragsfeed als Vorschau anzeigen und übertragen können. Ihre Zuschauer können den Livestream mit einem beliebigen Player wiedergeben, der mit HLS, DASH oder Smooth Streaming kompatibel ist. Sie können den [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) in Ihren Webanwendungen oder mobilen Anwendungen nutzen, um Ihren Stream mit einem beliebigen dieser Protokolle bereitzustellen.

Mit Media Services können Sie Ihre zu übermittelnden Inhalte dynamisch (mithilfe der **dynamischen Verschlüsselung**) mit Advanced Encryption Standard (AES-128) oder einem der drei wichtigsten DRM-Systeme (Digital Rights Management, Verwaltung digitaler Rechte) verschlüsseln: Microsoft PlayReady, Google Widevine und Apple FairPlay. Media Services bietet außerdem einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen an autorisierte Clients. Weitere Informationen zum Verschlüsseln Ihrer Inhalte mit Media Services finden Sie unter [Übersicht über den Inhaltsschutz](content-protection-overview.md).

Sie können auch die dynamische Filterung anwenden, mit der Sie die Anzahl von Spuren, Formaten und Bitraten sowie die Präsentationszeitfenster steuern können, die an die Player gesendet werden. Weitere Informationen finden Sie unter [Filter und dynamische Manifeste](filters-dynamic-manifest-overview.md).

Dieser Artikel enthält eine Übersicht und Anleitungen für Livestreaming mit Media Services.

## <a name="prerequisites"></a>Voraussetzungen

Um die Livestreaming-Workflows in Media Services v3 nachvollziehen zu können, müssen Sie mit folgenden Konzepten vertraut sein: 

- [Streamingendpunkte](streaming-endpoint-concept.md)
- [Liveereignisse und Liveausgaben](live-events-outputs-concept.md)

## <a name="live-streaming-workflow"></a>Workflow für das Livestreaming

Nachfolgend werden die Schritte in einem Workflow für das Livestreaming aufgeführt:

1. Navigieren Sie zu Ihrem Media Services-Konto, und vergewissern Sie sich, dass der **Streamingendpunkt** ausgeführt wird. 
2. Erstellen Sie ein **Liveereignis**. <br/>Bei der Ereigniserstellung können Sie angeben, dass das Ereignis automatisch gestartet werden soll. Starten Sie alternativ das Ereignis, wenn Sie zum Starten des Streamings bereit sind.<br/> Wenn für den automatischen Start „true“ festgelegt ist, wird das Liveereignis direkt nach der Erstellung gestartet. Das bedeutet, dass die Abrechnung beginnt, sobald das Liveereignis startet. Sie müssen für die Liveereignisressource explizit „Stop“ aufrufen, damit keine Gebühren mehr anfallen. Weitere Informationen finden Sie im Abschnitt [LiveEvent-Zustandswerte und Abrechnung](live-event-states-billing.md).
3. Rufen Sie die Erfassungs-URLs ab, und konfigurieren Sie Ihren lokalen Encoder vor Ort, um die URL zum Senden des Beitragsfeeds zu verwenden.<br/>Siehe [Empfohlene Livestreaming-Encoder](recommended-on-premises-live-encoders.md).
4. Rufen Sie die Vorschau-URL ab und verwenden Sie sie, um sich zu vergewissern, dass die Eingabe des Encoders auch tatsächlich empfangen wird.
5. Erstellen Sie ein neues **Medienobjekt**.
6. Erstellen Sie eine **Liveausgabe**, und verwenden Sie den Namen des erstellten Medienobjekts.<br/>Die **Liveausgabe** archiviert den Datenstrom im **Medienobjekt**.
7. Erstellen Sie einen **Streaminglocator** mit den integrierten Arten von **Streamingrichtlinien**.<br/>Wenn Sie beabsichtigen, den Inhalt zu verschlüsseln, lesen Sie [Übersicht über den Inhaltsschutz](content-protection-overview.md).
8. Listen Sie die Pfade auf dem **StreamingLocator** auf, um die zu verwendenden URLs zurückzugeben (diese sind deterministisch).
9. Rufen Sie den Hostnamen für den **Streamingendpunkt** ab, von dem aus Sie streamen möchten.
10. Kombinieren Sie die URL aus Schritt 8 mit dem Hostnamen aus Schritt 9, um die vollständige URL zu erhalten.
11. Wenn Sie möchten, dass Ihr **Liveereignis** nicht mehr angesehen werden kann, müssen Sie das Streamen des Ereignisses beenden und den **Streaminglocator** löschen.

## <a name="other-important-articles"></a>Weitere wichtige Artikel

- [Empfohlene Liveencoder](recommended-on-premises-live-encoders.md)
- [Verwenden eines Cloud-DVR](live-event-cloud-dvr.md)
- [Vergleich der LiveEvent-Typen](live-event-types-comparison.md)
- [Zustandswerte und Abrechnung](live-event-states-billing.md)
- [Latenz](live-event-latency.md)

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial zu Livestreaming](stream-live-tutorial-with-api.md)
* [Hinweise zur Migration von Media Services v2 zu v3](migrate-from-v2-to-v3.md)
