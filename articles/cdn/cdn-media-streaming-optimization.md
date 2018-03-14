---
title: "Medienstreamingoptimierung über das Azure Content Delivery Network"
description: "Optimieren des Streamings von Mediendateien für eine reibungslose Übermittlung"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: c953baad9ca5def916800e6abe7032b4572def5a
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="media-streaming-optimization-via-azure-cdn"></a>Medienstreamingoptimierung über das Azure Content Delivery Network 
 
Die zunehmende Verbreitung von HD-Videos im Internet stellt eine Herausforderung für die effiziente Übermittlung umfangreicher Dateien dar. Kunden erwarten bei einer Vielzahl von Netzwerken und Clients auf der ganzen Welt eine ruckelfreie Wiedergabe von Video on Demand- oder Livevideoressourcen. Für Medienstreamingdateien bedarf es daher eines schnelleren und effizienteren Übermittlungsverfahrens, um eine reibungslose und angemessene Kundenerfahrung zu gewährleisten.  

Die Übermittlung von Livestreamingmedien ist aufgrund der Dateigrößen und der Anzahl gleichzeitiger Benutzer besonders schwierig. Bei langen Verzögerungen verlieren Benutzer schnell das Interesse. Da Livestreams nicht vorab zwischengespeichert werden können und lange Wartezeiten für die Zuschauer nicht akzeptabel sind, müssen Videofragmente möglichst zeitnah übermittelt werden. 

Die Anforderungsmuster beim Streaming stellen auch einige neue Herausforderungen dar. Wenn ein beliebter Livestream oder eine neue Serie über Video on Demand (VOD) erscheint, kann es vorkommen, dass der Stream von Tausenden oder sogar von Millionen von Zuschauern gleichzeitig angefordert wird. In diesem Fall bedarf es einer intelligenten Anforderungskonsolidierung, um die Ursprungsserver nicht zu überlasten, wenn die Ressourcen noch nicht zwischengespeichert wurden.
 
Das **Azure CDN von Akamai** bietet ein Feature, das Streamingmedienobjekte effizient an Benutzer auf der ganzen Welt übermittelt. Es entlastet die Ursprungsserver und verkürzt so die Wartezeiten. Dieses Feature steht im Tarif „Akamai Standard“ zur Verfügung. 

Das **Azure CDN von Verizon** übermittelt Streamingmedien direkt mit dem Optimierungstyp „Allgemeine Webbereitstellung“.
 
## <a name="configure-an-endpoint-to-optimize-media-streaming"></a>Konfigurieren eines Endpunkts zur Optimierung von Medienstreaming
 
Sie können Ihren CDN-Endpunkt (Content Delivery Network) über das Azure-Portal für die Übertragung großer Dateien optimieren. Sie können dafür auch die REST-APIs oder eines der Client-SDKs verwenden. Die folgenden Schritte veranschaulichen die Vorgehensweise im Azure-Portal für ein **Azure CDN von Akamai**-Profil:

1. Wählen Sie zum Hinzufügen eines neuen Endpunkts auf der Seite **CDN-Profil** die Option **Endpunkt** aus.
  
    ![Neuer Endpunkt](./media/cdn-media-streaming-optimization/01_Adding.png)

2. Wählen Sie für Video on Demand-Ressourcen in der Dropdownliste **Optimiert für** die Option **Video on Demand-Medienstreaming** aus. Falls Sie eine Kombination aus Livestreaming und Video on Demand-Streaming verwenden, wählen Sie **Allgemeine Medienstreaming** aus.

    ![Streaming ausgewählt](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
Nach Erstellung des Endpunkts wird die Optimierung auf alle Dateien angewendet, die bestimmte Kriterien erfüllen. Dies wird im folgenden Abschnitt beschrieben. 
 
## <a name="media-streaming-optimizations-for-azure-cdn-from-akamai"></a>Optimierungen für das Medienstreaming für Azure CDN von Akamai
 
Die Medienstreamingoptimierung für das **Azure CDN von Akamai** empfiehlt sich für Live- oder Video on Demand-Streamingmedien, bei denen individuelle Medienfragmente übermittelt werden. Dieser Prozess unterscheidet sich vom Prozess für eine einzelne große Ressource, die mittels progressivem Download oder mithilfe von Bytebereichsanfragen übertragen wird. Informationen zu dieser Art von Medienübermittlung finden Sie unter [Optimieren großer Dateidownloads mit Azure CDN](cdn-large-file-optimization.md).

Bei den Optimierungstypen für die Übermittlung von allgemeinen Medien oder Video on Demand-Medien wird die Übermittlung der Medienobjekte mithilfe eines CDNs mit Back-End-Optimierung beschleunigt. Außerdem werden Konfigurationen für Medienobjekte verwendet, die auf Methoden basieren, die sich in der Vergangenheit bewährt haben.

### <a name="caching"></a>Caching

Wenn das **Azure CDN von Akamai** erkennt, dass es sich bei der Ressource um ein Streamingmanifest oder -fragment handelt, verwendet es andere Cachingablaufzeiten als bei der allgemeinen Webbereitstellung. (Eine vollständige Liste finden Sie in der Tabelle weiter unten.) Auch hier werden vom Ursprungsserver gesendete Header vom Typ „cache-control“ oder „expires“ berücksichtigt. Falls es sich bei der Ressource nicht um ein Medienobjekt handelt, werden beim Caching die Ablaufzeiten für die allgemeine Webbereitstellung verwendet.

Die kurze negative Zwischenspeicherungsdauer ermöglicht die Entlastung des Ursprungsservers, wenn viele Benutzer ein noch nicht vorhandenes Fragment anfordern. Ein Beispiel wäre etwa ein Livestream, dessen Pakete zum Anforderungszeitpunkt noch nicht auf dem Ursprungsserver verfügbar sind. Das längere Cachingintervall sorgt auch für eine Entlastung bei Anforderungen vom Ursprungsserver, da Videoinhalte in der Regel nicht geändert werden.
 

|   | Allgemeine Webbereitstellung | Allgemeines Medienstreaming | Video on Demand-Medienstreaming  
--- | --- | --- | ---
Caching: positiv <br> HTTP 200, 203, 300, <br> 301, 302 und 410 | 7 Tage |365 Tage | 365 Tage   
Caching: negativ <br> HTTP 204, 305, 404 <br> und 405 | Keine | 1 Sekunde | 1 Sekunde
 
### <a name="deal-with-origin-failure"></a>Behandeln eines Ausfalls des Ursprungsservers  

Bei der Übermittlung von allgemeinen Medien oder Video on Demand-Medien stehen auch ein Timeout für den Ursprungsserver sowie ein Wiederholungsprotokoll auf der Grundlage bewährter Methoden für typische Anforderungsmuster zur Verfügung. Da beispielsweise für die Übermittlung von Live- und Video on Demand-Medien die allgemeine Medienübermittlung verwendet wird, gilt angesichts des zeitkritischen Livestreamings ein erheblich kürzeres Verbindungstimeout.

Bei einem Verbindungstimeout führt das CDN eine bestimmte Anzahl von Wiederholungen aus und sendet schließlich den Fehler „504 - Gateway-Timeout“ an den Client. 

Wenn eine Datei die Bedingungen für Dateityp und -größe erfüllt, verwendet das CDN das Verhalten für Medienstreaming. Andernfalls wird die allgemeine Webbereitstellung verwendet.
   
### <a name="conditions-for-media-streaming-optimization"></a>Bedingungen für die Medienstreaming-Optimierung 

Die folgende Tabelle enthält die Kriterien für die Medienstreaming-Optimierung: 
 
Unterstützte Streamingtypen | Dateierweiterungen  
--- | ---  
Apple HLS | M3U8, M3U, M3UB, KEY, TS, AAC
Adobe HDS | F4M, F4X, DRMMETA, BOOTSTRAP, F4F,<br>Seg-Frag-URL-Struktur <br> (regulärer Ausdruck: ^(/.*)Seq(\d+)-Frag(\d+)
DASH | MPD, DASH, DIVX, ISMV, M4S, M4V, MP4, MP4V, <br> SIDX, WEBM, MP4A, M4A, ISMA
Smooth Streaming | /manifest/, /QualityLevels/Fragments/
  

 
## <a name="media-streaming-optimizations-for-azure-cdn-from-verizon"></a>Optimierungen für das Medienstreaming für Azure CDN von Verizon

Das **Azure CDN von Verizon** übermittelt Streamingmedienobjekte direkt unter Verwendung des Optimierungstyps „Allgemeine Webbereitstellung“. Einige CDN-Features tragen standardmäßig direkt zur Übermittlung von Medienobjekten bei.

### <a name="partial-cache-sharing"></a>Partielle Cachefreigabe

Mit der partiellen Cachefreigabe kann das CDN teilweise zwischengespeicherte Inhalte für neue Anforderungen bereitstellen. Wenn also beispielsweise die erste an das CDN gerichtete Anforderung zu einem Cachefehler führt, wird die Anforderung an den Ursprungsserver gesendet. Dieser unvollständige Inhalt wird in den CDN-Cache geladen, und die Daten können von anderen CDN-Anforderungen abgerufen werden. 

### <a name="cache-fill-wait-time"></a>Wartezeit für die Cacheauffüllung

 Durch die Wartezeit für die Cacheauffüllung wird erzwungen, dass der Edgeserver sämtliche nachfolgende Anforderungen für die Ressource zurückstellt, bis HTTP-Antwortheader vom Ursprungsserver eingehen. Wenn vor Ablauf der Wartezeit HTTP-Antwortheader vom Ursprungsserver eingehen, werden alle zurückgestellten Anforderungen über den wachsenden Cache bedient. Gleichzeitig wird der Cache mit Daten des Ursprungsservers gefüllt. Die Wartezeit für die Cacheauffüllung ist standardmäßig auf 3.000 Millisekunden festgelegt. 

