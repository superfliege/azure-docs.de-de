---
title: Analysieren von Verwendungsstatistiken mit Azure-CDN erweiterte HTTP-Berichte | Microsoft Docs
description: "Erfahren Sie, wie erweiterte HTTP-Berichte in Microsoft Azure-CDN zu erstellen. Diese Berichte enthalten ausführliche Informationen für CDN-Aktivität."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ef90adc1-580e-4955-8ff1-bde3f3cafc5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 2dfbc046674b2da692f30c945aee3ea25ae524eb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-usage-statistics-with-azure-cdn-advanced-http-reports"></a>Analysieren von Verwendungsstatistiken mit Azure-CDN erweiterte HTTP-Berichte
## <a name="overview"></a>Übersicht
Dieses Dokument erläutert erweiterte HTTP-Berichterstattung in Microsoft Azure-CDN. Diese Berichte enthalten ausführliche Informationen für CDN-Aktivität.

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Zugreifen auf erweiterte HTTP-Berichte
1. Klicken Sie auf dem CDN-Profil-Blatt auf den **verwalten** Schaltfläche.
   
    ![CDN-Profil Blatt Schaltfläche verwalten](./media/cdn-advanced-http-reports/cdn-manage-btn.png)
   
    Der CDN-Verwaltungsportal wird geöffnet.
2. Zeigen Sie auf die **Analytics** tab, dann zeigen Sie auf die **erweiterte HTTP-Berichte** Flyout.  Klicken Sie auf **HTTP große Plattform**.
   
    ![CDN-Verwaltungsportal – Menü Erweiterte Berichte](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)
   
    Berichtsoptionen werden angezeigt.

## <a name="geography-reports-map-based"></a>Geography-Berichte (Map-basiert)
Es gibt fünf Berichte, die nutzen Sie eine Zuordnung für die Regionen angeben, aus denen Ihre Inhalte angefordert werden. Diese Berichte sind World Map, Karte der USA, Kanada Kartentyp, Europa Zuordnung und Asien Pazifik Zuordnung.

Jede Zuordnung-basierter Bericht Ränge geografischen Entitäten (z. B. Länder, Bundesstaaten und Provinzen) gemäß den Prozentsatz der Treffer an, die von dieser Region stammen. Darüber hinaus wird eine Zuordnung bereitgestellt, können Sie die Speicherorte visuell darstellen, aus denen Ihre Inhalte angefordert werden. Es kann zu diesem Zweck jede Region nach der Menge der Anforderung, die in dieser Region erfahrenen farbcodierung. Heller schattierte Regionen zeigen niedriger Nachfrage nach Ihren Inhalt bei dunklere Regionen höhere Ebenen des Bedarfs für Ihre Inhalte angeben an.

Detaillierte Informationen Datenverkehr und die Bandbreite für jede Region wird direkt unterhalb der Zuordnung bereitgestellt. Dadurch können Sie die Gesamtanzahl der Cachetreffer, den Prozentsatz der Cachetreffer, die Gesamtmenge der Daten anzeigen (in Gigabyte) übertragen. anschließend übertragen der Prozentsatz der Daten für jede Region. Zeigt eine Beschreibung für jede dieser Metriken an. Schließlich, wenn Sie über einen Bereich (d. h., Land, Bundesland oder Kanton) zeigen, werden der Name und der Prozentsatz der Treffer an, die in der Region aufgetreten sind als QuickInfo angezeigt werden.

Eine kurze Beschreibung wird für jeden Typ von Geography Map-basierten Berichts unten bereitgestellt.

| Berichtsname | Beschreibung |
| --- | --- |
| Www-Karte |In diesem Bericht können Sie die weltweiten bei Bedarf für Ihre CDN-Inhalte anzeigen. Jedes Land sind farbcodiert, auf die World-Karte, um den Prozentsatz der Treffer an, die aus dem betreffenden Bereich stammt. |
| Karte der USA |In diesem Bericht können Sie die Nachfrage nach Ihrem CDN-Inhalte in den USA anzeigen. Jeder Zustand ist farbcodiert, auf dieser Karte den Prozentsatz der Treffer an, die von dieser Region stammen. |
| Kanada-Karte |In diesem Bericht können Sie die Nachfrage nach Ihrem CDN-Inhalte in Kanada anzeigen. Jede Kanton ist farbcodiert, auf dieser Karte den Prozentsatz der Treffer an, die von dieser Region stammen. |
| Europakarte |In diesem Bericht können Sie die Nachfrage nach Ihrem CDN-Inhalte in Europa anzuzeigen. Auf dieser Karte, um den Prozentsatz der Treffer an, die aus dem betreffenden Bereich stammt, wird jedes Land farbcodiert. |
| Asien-Pazifik-Karte |In diesem Bericht können Sie die Nachfrage nach Ihrem CDN-Inhalte in Asien anzeigen. Auf dieser Karte, um den Prozentsatz der Treffer an, die aus dem betreffenden Bereich stammt, wird jedes Land farbcodiert. |

## <a name="geography-reports-bar-charts"></a>Geography-Berichte (Balkendiagramme)
Es gibt zwei zusätzliche Berichte, die je nach Geografie statistische Informationen bieten die Städte nach oben und nach oben Ländern sind. Diese Berichte rank Städte und Ländern bzw. nach der Anzahl der Treffer an, die von dieser Regionen stammen. Nach dem Generieren von diesem Berichtstyp, Aufrufens ein Balkendiagramm die obersten 10 Städte oder Länder, die über eine bestimmte Plattform Inhalt angefordert. Diesem Balkendiagramm können Sie schnell die Regionen zu verschaffen, die die höchste Anzahl von Anforderungen für Ihre Inhalte zu generieren.

Die linke Seite des Diagramms (y-Achse) gibt an, wie viele Treffer in der angegebenen Region aufgetreten ist. Direkt unterhalb des Diagramms (x-Achse) werden soll finden Sie eine Bezeichnung für jede der obersten 10 Regionen.

### <a name="using-the-bar-charts"></a>Verwenden die Balkendiagramme
* Wenn Sie auf einen Balken zeigen, werden der Name und die Gesamtzahl der Treffer an, die in der Region aufgetreten sind als QuickInfo angezeigt.
* Die QuickInfo für den Bericht nach oben Städte identifiziert eine Stadt von Name, Bundesland/Kanton und Land Abkürzung.
* Wenn die Stadt oder die Region (z. B. Bundesland/Kanton) eine Anforderung stammt nicht ermittelt werden konnte, wird angegeben, dass sie unbekannte sind. Wenn das Land unbekannt, und klicken Sie dann auf zwei Fragezeichen (d. h.)?? ist, wird angezeigt.
* Ein Bericht kann Metriken für die "Europe" oder "Asien/Pazifik." enthalten. Statistische Informationen über alle IP-Adressen in diesen Bereichen enthalten sollen diese Elemente nicht. Stattdessen gelten nur für Anforderungen, die von IP-Adressen stammen, die auf Europa oder Asien/Pazifik statt zu einer bestimmten Stadt und Land verteilt sind.

Die Daten, mit denen das Balkendiagramm generiert wurde, können darunter angezeigt werden. Es werden Sie feststellen, übertragen die Gesamtanzahl der Cachetreffer, den Prozentsatz der Cachetreffer, die Menge der Daten (in Gigabyte), und der Prozentsatz der Daten für die oberen 250 Regionen übertragen. Zeigt eine Beschreibung für jede dieser Metriken an.

Eine kurze Beschreibung wird für beide Typen von Berichten, die weiter unten bereitgestellt.

| Berichtsname | Beschreibung |
| --- | --- |
| Top-Orte |In diesem Bericht die Rangfolge Städte entsprechend der Anzahl der Treffer an, die von stammt aus dem betreffenden Bereich bestimmt. |
| Top Ländern |In diesem Bericht die Rangfolge Ländern entsprechend der Anzahl der Treffer an, die von stammt aus dem betreffenden Bereich bestimmt. |

## <a name="daily-summary"></a>Tägliche Zusammenfassung
Die täglichen Zusammenfassungsbericht können Sie die Gesamtanzahl der Cachetreffer und über eine bestimmte Plattform auf täglicher Basis übertragenen Daten anzeigen. Diese Informationen kann verwendet werden, um CDN aktivitätsmuster schnell erkannt werden kann. Beispielsweise können in diesem Bericht Sie welche Tage erfahrenen höher oder niedriger ist als die erwartete Datenverkehr zu erkennen.

Nach dem Generieren von diesem Berichtstyp, stellt ein Balkendiagramm einen visuellen Hinweis auf die Menge des plattformspezifischen Bedarf erfahrenen täglich über den Zeitraum vom Bericht bereit. Es wird zu diesem Zweck einen Balken für jeden Tag im Bericht anzeigen. Beispielsweise als "Letzte Woche" bezeichnet den Zeitraum auswählen ein Balkendiagramm mit sieben Balken generiert. Jeder Balken wird die Gesamtzahl der Treffer an diesem Tag erfahrenen anzugeben.

Die linke Seite des Diagramms (y-Achse) gibt an, wie viele Zugriffe auf das angegebene Datum ist aufgetreten. Direkt unterhalb des Diagramms (x-Achse) werden soll, finden Sie eine Bezeichnung, der das Datum angibt (Format: JJJJ-MM-TT) für jeden Tag im Bericht enthalten.

> [!TIP]
> Wenn Sie auf einen Balken zeigen, wird die Gesamtzahl der Treffer an, die an diesem Datum aufgetreten sind als QuickInfo angezeigt.
> 
> 

Die Daten, mit denen das Balkendiagramm generiert wurde, können darunter angezeigt werden. Es finden Sie die Gesamtzahl der Treffer und die Datenmenge (in Gigabyte) für jeden Tag, die vom Bericht abgedeckt.

## <a name="by-hour"></a>Pro Stunde
Der Bericht nach Stunde können Sie die Gesamtanzahl der Cachetreffer und über eine bestimmte Plattform stündlich übertragenen Daten anzeigen. Diese Informationen kann verwendet werden, um CDN aktivitätsmuster schnell erkannt werden kann. Angenommen, können in diesem Bericht Sie die Zeiträume im Laufe des Tages zu erkennen, die höher oder niedriger ist als die erwartete Datenverkehr auftreten.

Nach dem Generieren von diesem Berichtstyp, gebe ein Balkendiagramm einen visuellen Hinweis auf die Menge des plattformspezifischen-Anforderung, die auf einer stündlichen Basis für den Zeitraum vom Bericht abgedeckt ist. Es wird zu diesem Zweck einen Balken für jede Stunde, die Werte im Bericht anzeigen. Z. B. auswählen ein 24-Stunden Zeitraum ein Balkendiagramm mit 24 Balken generiert. Jeder Balken wird die Gesamtzahl der Treffer erfahrenen während dieser Stunde angegeben.

Die linke Seite des Diagramms (y-Achse) gibt an, auf die angegebene Stunde Treffer aufgetreten ist. Direkt unterhalb des Diagramms (x-Achse) werden soll, finden Sie eine Bezeichnung, die Datum/Uhrzeit (Format: JJJJ-MM-TT HH: mm) für jede Stunde, die im Bericht enthalten. Berichtszeit wird mit 24-Stunden-Format und unter Verwendung der UTC/GMT-Zeitzone angegeben ist.

> [!TIP]
> Wenn Sie auf einen Balken zeigen, wird die Gesamtzahl der Treffer an, die während dieser Stunde wurden als QuickInfo angezeigt.
> 
> 

Die Daten, mit denen das Balkendiagramm generiert wurde, können darunter angezeigt werden. Es finden Sie die Gesamtzahl der Treffer und die Datenmenge (in Gigabyte) für jede Stunde, die vom Bericht abgedeckt.

## <a name="by-file"></a>In Datei
Der Bericht von Datei können Sie die Menge der Anforderung und den Datenverkehr über eine bestimmte Plattform für die am häufigsten angeforderten Objekte davon anzeigen. Nach dem Generieren von diesem Berichtstyp, wird ein Balkendiagramm für den angegebenen Zeitraum für die obersten 10 am häufigsten angeforderten Objekte generiert.

> [!NOTE]
> Edge-CNAME-URLs werden für die Zwecke dieses Berichts können auf ihre entsprechenden CDN-URLs konvertiert. Dies ermöglicht eine präzise Tally für die Gesamtzahl von Treffern ein Medienobjekt unabhängig vom CDN oder Edge CNAME-URL verwendet, um auf Anforderung zugeordnet.
> 
> 

Die linke Seite des Diagramms (y-Achse) gibt die Anzahl der Anforderungen für jedes Medienobjekt für den angegebenen Zeitraum an. Direkt unterhalb des Diagramms (x-Achse), eine Bezeichnung finden Sie, die den Dateinamen für jede der obersten 10 angeforderten Objekte angibt.

Die Daten, mit denen das Balkendiagramm generiert wurde, können darunter angezeigt werden. Dort finden Sie die folgende Informationen für jede der obersten 250 angeforderten Objekte: relativen Pfad, der die Gesamtzahl der Cachetreffer, den Prozentsatz der Cachetreffer, die Menge der Daten zu übertragen, (in Gigabyte), und der Prozentsatz der Daten zu übertragen.

## <a name="by-file-detail"></a>Vom Datei-Detail
Der vom Datei-Detailbericht können Sie die Menge der Anforderung und den Datenverkehr über eine bestimmte Plattform für ein bestimmtes Medienobjekt entstandenen anzeigen. Ist Sie ganz oben in diesem Bericht die Option Datei Details für ein. Diese Option bietet eine Liste Ihrer am häufigsten angeforderten Objekte für die ausgewählte Plattform. Um einen von Datei detaillierten Bericht zu generieren, müssen Sie wählen Sie die Option Datei Details für das gewünschte Medienobjekt. Nach dem ein Balkendiagramm wird angegeben, dass die Menge der täglich Anforderung, die sie für den angegebenen Zeitraum generiert.

Die linke Seite des Diagramms (y-Achse) gibt an, die Gesamtanzahl der Anforderungen erfasst, eine Anlage an einem bestimmten Tag aufgetreten ist. Direkt unterhalb des Diagramms (x-Achse) werden soll, finden Sie eine Bezeichnung, der das Datum angibt (Format: JJJJ-MM-TT) für die CDN-Anforderung für das Medienobjekt gemeldet wurde.

Die Daten, mit denen das Balkendiagramm generiert wurde, können darunter angezeigt werden. Es finden Sie die Gesamtzahl der Treffer und die Datenmenge (in Gigabyte) für jeden Tag, die vom Bericht abgedeckt.

## <a name="by-file-type"></a>Nach Dateityp
Der Bericht nach Dateityp können Sie die Menge der Anforderung und des Dateityp anfallenden Datenverkehrs anzeigen. Nach dem Generieren von diesem Berichtstyp, wird ein Ringdiagramm den Prozentsatz der Treffer generiert, die für die obersten 10 Dateitypen anzugeben.

> [!TIP]
> Wenn Sie über ein Segment in das Ringdiagramm zeigen, Medientyp Internet von diesem Dateityp als QuickInfo angezeigt.
> 
> 

Die Daten, die zum Generieren der Ringdiagramm verwendet wurde, können darunter angezeigt werden. Es findet den Dateityp Namen Erweiterung/Internet-Medien, die Gesamtanzahl der Cachetreffer, den Prozentsatz der Cachetreffer, übertragen Sie die Datenmenge (in Gigabyte), und der Prozentsatz der Daten für jede der oben 250 Dateitypen übertragen.

## <a name="by-directory"></a>Vom Verzeichnis
Der Bericht vom Verzeichnis können Sie die Menge der Anforderung und den Datenverkehr über eine bestimmte Plattform für den Inhalt aus einem bestimmten Verzeichnis entstandenen anzeigen. Nach dem Generieren von diesem Berichtstyp, wird ein Balkendiagramm die Gesamtzahl der Treffer von Inhalt in die obersten 10 Verzeichnisse generierten anzugeben.

### <a name="using-the-bar-chart"></a>Verwenden das Balkendiagramm
* Zeigen Sie auf einen Balken, um den relativen Pfad zum entsprechenden Verzeichnis anzuzeigen.
* Bei der Berechnung der Anforderung vom Verzeichnis in einen Unterordner eines Verzeichnisses gespeicherte Inhalte nicht gezählt. Diese Berechnung basiert ausschließlich auf die Anzahl der Anforderungen, die generiert wird, für den Inhalt im aktuellen Verzeichnis gespeichert.
* Edge-CNAME-URLs werden für die Zwecke dieses Berichts können auf ihre entsprechenden CDN-URLs konvertiert. Dies ermöglicht eine präzise Tally für alle Statistiken, die ein Medienobjekt unabhängig vom CDN oder Edge CNAME-URL verwendet, um auf Anforderung zugeordnet.

Die linke Seite des Diagramms (y-Achse) gibt an, die Gesamtanzahl der Anforderungen für den Inhalt in die ersten 10-Verzeichnissen gespeichert sind. Jeder Balken im Diagramm stellt ein Verzeichnis dar. Verwenden Sie die color-coding Authentifizierungsschema, um einen Balken in einem Verzeichnis im Abschnitt oben 250 vollständigen Verzeichnissen aufgeführten zuordnen.

Die Daten, mit denen das Balkendiagramm generiert wurde, können darunter angezeigt werden. Dort finden Sie die folgende Informationen für jeden der oben 250 Verzeichnisse: relativen Pfad, der die Gesamtzahl der Cachetreffer, den Prozentsatz der Cachetreffer, die Menge der Daten zu übertragen, (in Gigabyte), und der Prozentsatz der Daten zu übertragen.

## <a name="by-browser"></a>Vom Browser
Der Bericht vom Browser können Sie anzeigen, welche Browser verwendet wurden, um Inhalte angefordert werden. Nach dem Generieren von diesem Berichtstyp, Aufrufens ein Kreisdiagramms den Prozentsatz der Anforderungen von der obersten 10 Browsern verarbeitet werden.

### <a name="using-the-pie-chart"></a>Verwenden Sie das Kreisdiagramm
* Zeigen Sie auf ein Segment in das Kreisdiagramm die Namen und die Version des Browsers anzeigen.
* Für die Zwecke dieses Berichts können gilt jede eindeutige Browserversion Kombination aus einen anderen Browser.
* Das Slice mit dem Namen "Andere" gibt den Prozentsatz der Anforderungen, die von allen anderen Browsern und Versionen behandelt.

Darunter können die Daten, die verwendet wurde, generiert das Kreisdiagramm angezeigt werden. Es finden Sie die Browser Typ/Versionsnummer, die Gesamtzahl der Treffer und den Prozentsatz der Treffer für jeden der oben 250 Browser.

## <a name="by-referrer"></a>Durch Referrer
Der Bericht von Referrer können Sie die häufigste Verweise auf Inhalte für die ausgewählte Plattform anzeigen. Eine Referenz gibt an, den Hostnamen, aus dem eine Anforderung generiert wurde. Nach dem Generieren von diesem Berichtstyp, wird ein Balkendiagramm die Menge der Anforderung (d. h. Treffer) generiert, die für die obersten 10 Verweise anzugeben.

Die linke Seite des Diagramms (y-Achse) gibt an, die Gesamtanzahl der Anforderungen erfasst, ein Medienobjekt bei jeder Referrer aufgetreten ist. Jeder Balken im Diagramm stellt eine Referenz dar. Verwenden Sie die color-coding Authentifizierungsschema, um ein Balken eine Referenz im Abschnitt oben 250 Referrer aufgeführten zuordnen.

Die Daten, mit denen das Balkendiagramm generiert wurde, können darunter angezeigt werden. Es finden Sie die URL, die Gesamtzahl der Treffer und den Prozentsatz von Treffern, die von jedem der Top-250 Verweiser generiert.

## <a name="by-download"></a>Durch Herunterladen
Der Bericht durch Herunterladen kann, Download-Muster für den am häufigsten angeforderten Inhalt zu analysieren. Der Anfang des Berichts enthält ein Balkendiagramm vergleicht Downloads mit abgeschlossenen Downloads für die obersten 10 angeforderten Objekte versucht, eine. Jeder Balken sind farbcodiert, gemäß der gibt an, ob es sich um einen Versuch Download (Blau) oder einen abgeschlossenen Download (Grün) handelt.

> [!NOTE]
> Edge-CNAME-URLs werden für die Zwecke dieses Berichts können auf ihre entsprechenden CDN-URLs konvertiert. Dies ermöglicht eine präzise Tally für alle Statistiken, die ein Medienobjekt unabhängig vom CDN oder Edge CNAME-URL verwendet, um auf Anforderung zugeordnet.
> 
> 

Die linke Seite des Diagramms (y-Achse) gibt den Dateinamen für jede der obersten 10 angeforderten Objekte an. Direkt unterhalb des Diagramms (x-Achse) werden soll finden Sie Bezeichnungen, die die Gesamtanzahl der Downloads versucht/abgeschlossene angeben.

Direkt unterhalb des Diagramms Balken die folgende Informationen werden aufgeführt für die obersten 250 angeforderten Objekte: relativen Pfad (einschließlich Dateiname), die Anzahl der Fälle, in denen es vollständig heruntergeladen wurde, die Anzahl der Fälle, in denen er angefordert wurde und der Prozentsatz der Anforderungen, die in einem vollständigen Download geführt haben.

> [!TIP]
> Unsere CDN informiert wird von einem HTTP-Client (d. h. Browser) Wenn ein Medienobjekt vollständig heruntergeladen wurde. Folglich berechnet, ob ein Medienobjekt gemäß Statuscodes und Bytebereichs-vollständig heruntergeladen wurde, müssen Anforderungen. Im ersten Schritt wir bei dieser Berechnung wird ein Statuscode "200 OK", ob die Anforderung führt. Wenn dies der Fall ist, untersuchen wir am bytebereichsanforderungen, um sicherzustellen, dass sie das gesamte Medienobjekt abdecken. Abschließend Vergleichen wir die Datenmenge, die auf die Größe der angeforderten Ressource. Wenn die Daten übertragen, gleich oder größer als die Dateigröße werden und die bytebereichsanforderungen eignen sich für das Medienobjekt, dann wird der Treffer als vollständige Download gezählt.
> 
> Aufgrund der interpretativen in diesem Bericht sollten Sie die folgenden Punkte bedenken, die die Konsistenz und die Genauigkeit der in diesem Bericht ändern können.
> 
> * Muster im Datenverkehr können nicht präzise vorhergesagt werden, wenn sich unterschiedlich, Benutzer-Agents Verhalten. Dies kann abgeschlossenen Download Ergebnissen führen, die größer als 100 % sind.
> * Objekte, die HTTP-progressiven Download nutzen können von diesem Bericht nicht exakt dargestellt werden. Dies liegt an der Benutzern, die an andere Positionen in einem Video Suchvorgänge.
> 
> 

## <a name="by-404-errors"></a>Von Fehlern des Typs 404
Der Bericht von 404-Fehler können Sie den Typ des Inhalts zu identifizieren, die der höchsten Anzahl an 404 Nichtgefunden-Statuscodes generiert. Der Anfang des Berichts enthält ein Balkendiagramm für die obersten 10 Medienobjekte, die für die eine 404 Nichtgefunden-Statuscode zurückgegeben wurde. Diesem Balkendiagramm vergleicht die Gesamtanzahl der Anforderungen mit Anforderungen, die für diese Ressourcen ein 404 Nichtgefunden Statuscode geführt haben. Jeder Balken sind farbcodiert. Ein gelber Balken wird verwendet, um anzugeben, dass die Anforderung einer 404 Statuscode "Nichtgefunden" geführt hat. Ein roter Strich wird verwendet, um die Gesamtanzahl der Anforderungen für das Medienobjekt anzugeben.

> [!NOTE]
> Für die Zwecke dieses Berichts ist Folgendes zu beachten:
> 
> * Treffer stellt jede Anforderung für einen Bestand unabhängig von der Statuscode.
> * Edge-CNAME-URLs werden in ihre entsprechenden CDN-URLs konvertiert. Dies ermöglicht eine präzise Tally für alle Statistiken, die ein Medienobjekt unabhängig vom CDN oder Edge CNAME-URL verwendet, um auf Anforderung zugeordnet.
> 
> 

Die linke Seite des Diagramms (y-Achse) gibt an, den Dateinamen für jede die obersten 10 angeforderten Objekte, die eine 404 Statuscode "Nichtgefunden" geführt hat. Direkt unterhalb des Diagramms (x-Achse) werden soll finden Sie Bezeichnungen, die angeben, die Gesamtanzahl der Anforderungen und die Anzahl der Anforderungen, die eine 404 Statuscode "Nichtgefunden" geführt hat.

Direkt unterhalb des Diagramms Balken die folgende Informationen werden aufgeführt für die obersten 250 angeforderten Objekte: relativen Pfad (einschließlich Dateiname), die Anzahl der Anforderungen, die eine 404 Statuscode "Nichtgefunden" geführt hat, die Gesamtzahl der Fälle, in denen das Medienobjekt angefordert wurde und der Prozentsatz der Anforderungen, die eine 404 Statuscode "Nichtgefunden" geführt hat.

## <a name="see-also"></a>Weitere Informationen:
* [Übersicht über die Azure-CDN](cdn-overview.md)
* [In Echtzeit Statistiken in Microsoft Azure-CDN](cdn-real-time-stats.md)
* [Überschreiben von Standardverhalten der HTTP-verwenden das Regelmodul](cdn-rules-engine.md)
* [Analysieren von Edge-Leistung](cdn-edge-performance.md)

