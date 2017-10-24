---
title: Kernberichte aus Verizon | Microsoft-Dokumentation
description: "Mithilfe der folgenden Berichte können Sie Verwendungsmuster für Ihr CDN anzeigen: Bandbreite, Übertragene Daten, Treffer, Cachestatus, Cachetrefferquote, Übertragene IPV4/IPV6-Daten."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 5a0d9018-8bdb-48ff-84df-23648ebcf763
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e9ee0041061296e313b3372dce13b5b86b2369c8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="core-reports-from-verizon"></a>Kernberichte aus Verizon

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Indem Sie Verizon-Kernberichte über das Verwaltungsportal für Verizon-Profile verwenden, können Sie mit den folgenden Berichten Verwendungsmuster für Ihr CDN anzeigen:

* Bandbreite
* Übertragene Daten
* Treffer
* Cachestatus
* Cachetrefferquote
* Übertragene IPV4-/IPV6-Daten

## <a name="accessing-verizon-core-reports"></a>Zugreifen auf Verizon-Kernberichte
1. Klicken Sie auf dem Blatt „CDN-Profil“ auf die Schaltfläche **Verwalten** .
   
    ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-reports/cdn-manage-btn.png)
   
    Das CDN-Verwaltungsportal wird geöffnet.
2. Zeigen Sie auf die Registerkarte **Analysen** und dann auf das Flyout **Core Reports** (Kernberichte).  Klicken Sie im Menü auf den gewünschten Bericht.
   
    ![CDN-Verwaltungsportal – Menü „Core Reports“ (Kernberichte)](./media/cdn-reports/cdn-core-reports.png)

## <a name="bandwidth"></a>Bandbreite
Der Bandbreitenbericht besteht aus einem Diagramm und einer Datentabelle, die die Bandbreitennutzung für HTTP und HTTPS über einen bestimmten Zeitraum angibt. Sie können die Bandbreitennutzung für alle CDN-POPs oder für einen bestimmten CDN-POP anzeigen. Dank dieses Berichts sehen Sie Spitzen im Datenverkehr und die Verteilung über die CDN-POPs in MBit/s.

* Wählen Sie "Alle Edgeknoten", um Datenverkehr von allen Knoten anzuzeigen, oder wählen Sie einen bestimmten Bereich/Knoten aus der Dropdownliste aus.
* Wählen Sie einen Datumsbereich, um Daten für heute/diese Woche/diesen Monat usw. anzuzeigen, oder geben Sie selbst Datumsangaben ein, und klicken Sie auf **Los**, um sicherzustellen, dass Ihre Auswahl aktualisiert wird.
* Sie können die Daten exportieren und herunterladen, indem Sie neben **Los** auf das Excel-Arbeitsblattsymbol klicken.

Der Bericht wird alle fünf Minuten aktualisiert.

![Bericht zur Bandbreite](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Übertragene Daten
Dieser Bericht besteht aus einem Diagramm und einer Datentabelle, die den Datenverkehr für HTTP und HTTPS über einen bestimmten Zeitraum angibt. Sie können den Datenverkehr für alle CDN-POPs oder für ein bestimmtes POP anzeigen. Dank dieses Berichts sehen Sie Spitzen im Datenverkehr und die Verteilung über die CDN-POPs in GB.

* Wählen Sie "Alle Edgeknoten", um Datenverkehr von allen Knoten anzuzeigen, oder wählen Sie einen bestimmten Bereich/Knoten aus der Dropdownliste aus.
* Wählen Sie einen Datumsbereich, um Daten für heute/diese Woche/diesen Monat usw. anzuzeigen, oder geben Sie selbst Datumsangaben ein, und klicken Sie auf **Los**, um sicherzustellen, dass Ihre Auswahl aktualisiert wird.
* Sie können die Daten exportieren und herunterladen, indem Sie neben **Los** auf das Excel-Arbeitsblattsymbol klicken.

Der Bericht wird alle fünf Minuten aktualisiert.

![Bericht zu übertragenen Daten](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Treffer (Statuscodes)
Dieser Bericht zeigt die Verteilung von Anforderungsstatuscodes für Ihre Inhalte. Jede Inhaltsanforderung generiert einen HTTP-Statuscode. Der Statuscode beschreibt, wie die Anforderung von Edge-POPs behandelt wurde. Beispielsweise weist ein 2xx-Statuscode darauf hin, dass die Anforderung erfolgreich für einen Client verarbeitet wurde, während ein 4xx-Statuscode auf einen Fehler hinweist. Weitere Informationen zu HTTP-Statuscodes finden Sie unter [List of HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) (Liste der HTTP-Statuscodes).

* Wählen Sie einen Datumsbereich, um Daten für heute/diese Woche/diesen Monat usw. anzuzeigen, oder geben Sie selbst Datumsangaben ein, und klicken Sie auf **Los**, um sicherzustellen, dass Ihre Auswahl aktualisiert wird.
* Sie können die Daten exportieren und herunterladen, indem Sie neben **Los** auf das Excel-Arbeitsblattsymbol klicken.

![Bericht zu Treffern](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Cachestatus
Dieser Bericht zeigt die Verteilung der Cachetreffer und Cachefehler für eine Clientanforderung an. Da die schnellste Leistung durch Cachetreffer erzielt wird, können Sie die Geschwindigkeit der Datenübermittlung optimieren, indem Sie Cachefehler und abgelaufene Cachetreffer auf ein Minimum reduzieren. Cachefehler können durch Konfigurieren des Ursprungsservers verringert werden, um die Zuweisung von „no-cache“-Antwortheadern zu vermeiden. Ferner können Sie das Zwischenspeichern von Abfragezeichenfolgen unterbinden, wo es nicht unbedingt notwendig ist, und Antwortcodes vermeiden, die nicht zwischenspeicherbar sind. Abgelaufene Cachetreffer können vermieden werden, indem Sie den Wert „max-age“ einer Ressource so hoch wie möglich setzen, um die Anzahl der Anforderungen an den Ursprungsserver zu minimieren.

![Bericht zum Cachestatus](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Im Folgenden sind die wichtigsten Cachestatusangaben aufgeführt:
* TCP_HIT: Von Edge bereitgestellt. Das Objekt befand sich im Cache und hatte den Wert „max-age“ nicht überschritten.
* TCP_MISS: Vom Ursprung bereitgestellt. Das Objekt befand sich nicht im Cache, und die Antwort ging zurück an den Ursprung.
* TCP_EXPIRED _MISS: Vom Ursprung bereitgestellt nach erneuter Überprüfung beim Ursprung. Das Objekt befand sich im Cache, hatte jedoch den Wert „max-age“ überschritten. Eine erneute Überprüfung beim Ursprung führte zum Ersetzen des Cacheobjekts durch eine neue Antwort vom Ursprung.
* TCP_EXPIRED _HIT: Von Edge bereitgestellt nach erneuter Überprüfung beim Ursprung. Das Objekt befand sich im Cache, hatte jedoch den Wert "max-age" überschritten. Eine erneute Überprüfung beim ursprünglichen Server führte dazu, dass das Cacheobjekt nicht geändert wurde.
* Wählen Sie einen Datumsbereich, um Daten für heute/diese Woche/diesen Monat usw. anzuzeigen, oder geben Sie selbst Datumsangaben ein, und klicken Sie auf **Los**, um sicherzustellen, dass Ihre Auswahl aktualisiert wird.
* Sie können die Daten exportieren und herunterladen, indem Sie neben **Los** auf das Excel-Arbeitsblattsymbol klicken.

### <a name="full-list-of-cache-statuses"></a>Vollständige Liste der Cachestatusangaben
* TCP_HIT: Dieser Status wird gemeldet, wenn eine Anforderung direkt vom POP für den Client bereitgestellt wird. Eine Ressource wird sofort von einem POP bereitgestellt, wenn sie auf dem dem Client nächstgelegenen POP zwischengespeichert ist und über eine entsprechende Gültigkeitsdauer verfügt. Die Gültigkeitsdauer (TTL) wird durch die folgenden Antwortheader bestimmt:
  
  * Cache-Control: s-maxage
  * Cache-Control: max-age
  * Expires
* TCP_MISS: Dieser Status gibt an, dass auf dem POP, der dem Client am nächsten liegt, keine zwischengespeicherte Version des angeforderten Medienobjekts gefunden wurde. Die Ressource wird entweder von einem Ursprungsserver oder von einem Ursprungsschutzserver angefordert. Wenn der Ursprungsserver oder der Ursprungsschutzserver eine Ressource zurückgibt, wird sie für den Client bereitgestellt und sowohl auf dem Client als auch auf dem Edgeserver zwischengespeichert. Andernfalls wird ein anderer Statuscode als 200 zurückgegeben (z.B. „403 Verboten“ oder „404 Nicht gefunden“).
* TCP_EXPIRED _HIT: Dieser Status wird gemeldet, wenn eine Anforderung, die an eine Ressource mit abgelaufener TTL gerichtet ist direkt vom POP für den Client bereitgestellt wurde. Beispiel: „max-age“ der Ressource ist abgelaufen. 
  
    Eine abgelaufene Anforderung führt in der Regel zu einer Anforderung zur erneuten Überprüfung an den ursprünglichen Server. Damit TCP_EXPIRED _HIT auftritt, muss der Ursprungsserver angeben, dass keine neuere Version der Ressource vorhanden ist. Diese Situation führt normalerweise zu einer Aktualisierung der Header „Cache-Control“ und „Expires“ der Ressource.
* TCP_EXPIRED _MISS: Dieser Status wird gemeldet, wenn eine neuere Version eines abgelaufenen zwischengespeicherten Medienobjekts vom POP für den Client bereitgestellt wird. Dieser Status tritt auf, wenn die Gültigkeitsdauer für eine zwischengespeicherte Ressource abgelaufen ist (z. B. „max-age“ abgelaufen) und der Ursprungsserver eine neuere Version dieser Ressource zurückgibt. Diese neue Version der Ressource wird für den Client und nicht für die zwischengespeicherte Version bereitgestellt. Darüber hinaus wird sie auf dem Edgeserver und auf dem Client zwischengespeichert.
* CONFIG_NOCACHE: Dieser Status gibt an, dass eine kundenspezifische Konfiguration auf unserem Edge-POP die Zwischenspeicherung des Medienobjekts verhindert.
* NONE: Dieser Status gibt an, dass keine Überprüfung der Aktualität des Cache-Inhalts durchgeführt wurde.
* TCP_ CLIENT_REFRESH _MISS: Dieser Status wird gemeldet, wenn ein HTTP-Client (z.B. ein Browser) einen Edge-POP zwingt, eine neue Version einer veralteten Ressource vom Ursprungsserver abzurufen.
  
    Standardmäßig verhindern unsere Server, dass ein HTTP-Client unsere Edgeserver zwingt, eine neue Version des Medienobjekts vom Ursprungsserver abzurufen.
* TCP_ PARTIAL_HIT: Dieser Status wird gemeldet, wenn eine Bytebereichsanforderung zu einem Treffer für ein teilweise zwischengespeichertes Medienobjekt führt. Der angeforderte Bytebereich wird sofort vom POP an den Client übermittelt.
* UNCACHEABLE: Dieser Status wird gemeldet, wenn die Header "Cache-Control" und "Expires" eines Medienobjekts darauf hinweisen, dass dieses nicht auf einem POP oder vom HTTP-Client zwischengespeichert werden soll. Diese Anforderungstypen werden vom Ursprungsserver bereitgestellt.

## <a name="cache-hit-ratio"></a>Cachetrefferquote
Dieser Bericht zeigt den Prozentsatz der zwischengespeicherten Anforderungen an, die direkt aus dem Cache bedient wurden.

Die Details finden Sie im folgenden Bericht:

* Der angeforderte Inhalt wurde auf dem POP zwischengespeichert, der der anfordernden Person am nächsten war.
* Die Anforderung wurde direkt vom Edgeserver am Rand unseres Netzwerks aus verarbeitet.
* Für die Anforderung war keine erneute Überprüfung beim ursprünglichen Server erforderlich.

Folgende Elemente sind im Bericht nicht enthalten:

* Anforderungen, die aufgrund von Länderfilteroptionen verweigert werden.
* Anforderungen für Medienobjekte, deren Header darauf hinweisen, dass sie nicht zwischengespeichert werden sollen. Beispielsweise verhindern die Header „Cache-Control: private“, „Cache-Control: no-cache“ oder „Pragma: no-cache“, dass eine Ressource zwischengespeichert wird.
* Bytebereichsanforderungen für teilweise zwischengespeicherte Inhalte.

Die Formel lautet: (TCP_ HIT/(TCP_ HIT+TCP_MISS))*100

* Wählen Sie einen Datumsbereich, um Daten für heute/diese Woche/diesen Monat usw. anzuzeigen, oder geben Sie selbst Datumsangaben ein, und klicken Sie auf **Los**, um sicherzustellen, dass Ihre Auswahl aktualisiert wird.
* Sie können die Daten exportieren und herunterladen, indem Sie neben **Los** auf das Excel-Arbeitsblattsymbol klicken.

![Bericht zur Cachetrefferquote](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>Übertragene IPV4-/IPV6-Daten
Dieser Bericht zeigt die Verteilung des Datenverkehrs in IPV4 und IPV6.

![Übertragene IPV4-/IPV6-Daten](./media/cdn-reports/cdn-ipv4-ipv6.png)

* Wählen Sie "Datumsbereich", um Daten für heute/diese Woche/diesen Monat usw. anzuzeigen, oder geben Sie selbst Datumsangaben ein.
* Klicken Sie dann auf **Los**, um sicherzustellen, dass Ihre Auswahl aktualisiert wird.

## <a name="considerations"></a>Überlegungen
Berichte können nur über die letzten 18 Monate generiert werden.

