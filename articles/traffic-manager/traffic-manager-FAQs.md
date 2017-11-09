---
title: "Azure Traffic Manager – Häufig gestellte Fragen (FAQs) | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Traffic Manager"
services: traffic-manager
documentationcenter: 
author: KumudD
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2017
ms.author: kumud
ms.openlocfilehash: eac9c3c2b7fde4ac225e17cc3b98ca5ee926c3b3
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) zu Traffic Manager

## <a name="traffic-manager-basics"></a>Traffic Manager-Grundlagen

### <a name="what-ip-address-does-traffic-manager-use"></a>Welche IP-Adresse verwendet Traffic Manager?

Traffic Manager arbeitet, wie unter [Funktionsweise von Traffic Manager](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works) erläutert, auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager).

Traffic Manager bietet daher keinen Endpunkt und keine IP-Adresse für die Verbindungsherstellung von Clients. Wenn für einen Dienst eine statische IP-Adresse benötigt wird, muss diese im Dienst und nicht in Traffic Manager konfiguriert werden.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Unterstützt Traffic Manager persistente Sitzungen?

Traffic Manager arbeitet, wie unter [Funktionsweise von Traffic Manager](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works) erläutert, auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager). Traffic Manager hat somit keinen Zugriff auf den HTTP-Datenverkehr zwischen Client und Server.

Zudem handelt es sich bei der IP-Quelladresse der von Traffic Manager empfangenen DNS-Abfrage um die IP-Adresse des rekursiven DNS-Diensts (und nicht um die IP-Adresse des Clients). Aus diesem Grund kann Traffic Manager keine individuellen Clients nachverfolgen und keine persistenten Sitzungen implementieren. Diese Einschränkung besteht bei allen DNS-basierten Systemen zur Datenverkehrsverwaltung, nicht nur bei Verwendung von Traffic Manager.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Warum tritt bei mir ein HTTP-Fehler auf, wenn ich Traffic Manager verwende?

Traffic Manager arbeitet, wie unter [Funktionsweise von Traffic Manager](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works) erläutert, auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager). HTTP-Datenverkehr zwischen Client und Server wird nicht über Traffic Manager abgewickelt. Daher müssen auftretende HTTP-Fehler auf Ihre Anwendung zurückzuführen sein. Damit der Client eine Verbindung mit der Anwendung herstellen kann, werden alle Schritte zur DNS-Auflösung durchgeführt. Hierzu gehört auch die Interaktion von Traffic Manager im Datenverkehrsfluss der Anwendung.

Konzentrieren Sie sich daher bei der weiteren Untersuchung auf die Anwendung.

Der vom Browser des Clients gesendete HTTP-Hostheader ist die häufigste Ursache für Probleme. Konfigurieren Sie die Anwendung so, dass der richtige Hostheader für den verwendeten Domänennamen akzeptiert wird. Informationen zu Endpunkten, die den Azure App Service nutzen, finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-App in Azure App Services, der Traffic Manager verwendet](../app-service/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>Wie wirkt sich die Verwendung von Traffic Manager auf die Leistung aus?

Traffic Manager arbeitet, wie unter [Funktionsweise von Traffic Manager](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works) erläutert, auf der DNS-Ebene. Da Clients eine direkte Verbindung mit Ihren Endpunkten herstellen, hat die Verwendung von Traffic Manager keinerlei Auswirkungen auf die Leistung, sobald die Verbindung hergestellt wurde.

Da sich Traffic Manager mit Anwendungen auf der DNS-Ebene integriert, muss ein zusätzlicher DNS-Lookup erfolgen, der in die DNS-Auflösungskette eingefügt werden muss. Die DNS-Auflösungszeit wird durch Traffic Manager nur minimal beeinträchtigt. Traffic Manager verwendet ein globales Netzwerk von Namenservern sowie [Anycastnetzwerke](https://en.wikipedia.org/wiki/Anycast), um sicherzustellen, dass DNS-Abfragen immer an den nächstgelegenen verfügbaren Namenserver weitergeleitet werden. Dank der Zwischenspeicherung von DNS-Antworten wirkt sich die zusätzliche, durch die Verwendung von Traffic Manager bedingte DNS-Latenz lediglich auf einen Bruchteil der Sitzungen aus.

Mit der leistungsorientierten Methode wird der Datenverkehr an den nächstgelegenen verfügbaren Endpunkt geleitet. Das führt dazu, dass die mit dieser Methode verbundenen Auswirkungen auf die Gesamtleistung minimal sind. Eine höhere DNS-Latenz muss durch eine geringere Netzwerklatenz für den Endpunkt ausgeglichen werden.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Welche Anwendungsprotokolle kann ich mit Traffic Manager verwenden?

Traffic Manager arbeitet, wie unter [Funktionsweise von Traffic Manager](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works) erläutert, auf der DNS-Ebene. Nach Abschluss des DNS-Lookups stellen Clients eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager). Daher kann für diese Verbindung ein beliebiges Anwendungsprotokoll verwendet werden. Bei Auswahl von TCP als Überwachungsprotokoll kann die Endpunkt-Systemüberwachung durch Traffic Manager ohne Anwendungsprotokolle erfolgen. Wenn Sie wünschen, dass die Integrität mithilfe eines Anwendungsprotokolls überprüft wird, muss der Endpunkt entweder auf HTTP oder HTTPS GET-Anforderungen reagieren können.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Kann ich Traffic Manager mit einem reinen Domänennamen verwenden?

Nein. Die DNS-Standards lassen keine Koexistenz von CNAME-Einträgen und anderen DNS-Einträgen mit demselben Namen zu. Die Spitze (oder der Stamm) einer DNS-Zone enthält immer zwei bereits vorhandene DNS-Einträge: den SOA-Eintrag und den autoritativen NS-Eintrag. Das bedeutet, dass im Zonenspitze kein CNAME-Eintrag erstellt werden kann, ohne gegen die DNS-Standards zu verstoßen.

Traffic Manager benötigt einen DNS-CNAME-Eintrag zum Zuordnen des Vanity-DNS-Namens. „www.contoso.com“ wird beispielsweise dem DNS-Namen des Traffic Manager-Profils „contoso.trafficmanager.net“ zugeordnet. Darüber hinaus gibt das Traffic Manager-Profil selbst einen zweiten DNS-CNAME-Eintrag zurück, um anzugeben, mit welchem Endpunkt der Client eine Verbindung herstellen soll.

Um dieses Problem zu umgehen, wird empfohlen, eine HTTP-Umleitung zu verwenden, um Datenverkehr von einem reinen Domänennamen an eine andere URL umzuleiten, die Traffic Manager verwenden kann. So kann beispielsweise die reine Domäne „contoso.com“ Benutzer an den CNAME „www.contoso.com“ weiterleiten, der auf den DNS-Namen von Traffic Manager verweist.

Die uneingeschränkte Unterstützung von Naked-Domänen in Traffic Manager ist Teil unseres Feature-Backlogs. Sie können die Unterstützung für diese Funktionsanforderung registrieren, indem Sie [auf unserer Community-Feedbackwebsite dafür abstimmen](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

### <a name="does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries"></a>Berücksichtigt Traffic Manager beim Verarbeiten von DNS-Abfragen die Clientsubnetzadresse? 
Ja. Zusätzlich zur empfangenen IP-Quelladresse der DNS-Abfrage (normalerweise die IP-Adresse des DNS-Resolvers) berücksichtigt Traffic Manager beim Durchführen von Suchvorgängen für geografische und leistungsbezogene Routingmethoden auch die Clientsubnetzadresse, sofern sie vom Resolver, der die Anforderung im Namen des Endbenutzers durchführt, in die Abfrage eingebunden wird.  
Insbesondere [RFC 7871 – Client Subnet in DNS Queries](https://tools.ietf.org/html/rfc7871), über den ein [Extension Mechanism for DNS (EDNS0)](https://tools.ietf.org/html/rfc2671) bereitgestellt wird, der die Clientsubnetzadresse von unterstützenden Resolvern weitergeben kann.

### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>Was ist DNS TTL, und wie wirkt DNS TTL sich auf meine Benutzer aus?

Wenn eine DNS-Abfrage bei Traffic Manager eingeht, legt er in der Antwort einen als Gültigkeitsdauer (Time To Live, TTL) bezeichneten Wert fest. Dieser Wert gibt Downstream-DNS-Resolvern in der Einheit Sekunden an, wie lange diese Antwort zwischengespeichert werden soll. Es ist zwar nicht garantiert, dass die DNS-Resolver dieses Ergebnis zwischenspeichern, doch das Zwischenspeichern ermöglicht ihnen, auf etwaige weiteren Abfragen aus dem Cache heraus zu antworten, anstatt zu Traffic Manager-DNS-Servern zu gehen. Dies wirkt sich wie folgt auf die Antworten aus:
- Eine höhere Gültigkeitsdauer (TTL) reduziert die Anzahl der bei den Traffic Manager-DNS-Servern eingehenden Abfragen, was die Kosten für einen Kunden verringern kann, da die Anzahl der verarbeiteten Abfragen eine abrechenbare Nutzung ist.
- Eine höhere Gültigkeitsdauer (TTL) kann möglicherweise den Zeitaufwand für eine DNS-Suche reduzieren.
- Eine höhere Gültigkeitsdauer (TTL) bedeutet auch, dass Ihre Daten nicht die aktuellen Integritätsinformationen wiedergeben, die Traffic Manager mithilfe seiner Test-Agents ermittelt hat.

### <a name="how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses"></a>Wie hoch oder niedrig kann ich die Gültigkeitsdauer (TTL) für Traffic Manager-Antworten festlegen?

Sie können die DNS-TTL pro Profil auf minimal 0 und maximal 2.147.483.647 Sekunden festlegen (der mit [RFC-1035](https://www.ietf.org/rfc/rfc1035.txt ) kompatible Maximalbereich). Der TTL-Wert 0 bedeutet, dass Downstream-DNS-Resolver Abfrageantworten nicht zwischenspeichern und alle Abfragen zur Auflösung an den Traffic Manager-DNS-Server gesendet werden.

## <a name="traffic-manager-geographic-traffic-routing-method"></a>Geografische Methode für das Datenverkehrsrouting in Traffic Manager

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Was sind einige Anwendungsfälle, in denen geografisches Routing nützlich ist? 
Der geografische Routingtyp kann in jedem Szenario verwendet werden, in dem ein Azure-Kunde seine Benutzer auf der Grundlage geografischer Regionen unterscheiden muss. Beispielsweise können Sie mithilfe des geografischen Routings für Benutzer aus bestimmten Regionen eine andere Benutzeroberfläche anzeigen als für Benutzer aus anderen Regionen. Ein weiteres Beispiel wäre die Einhaltung ortsbezogener Datenhoheitsmandate, für die es erforderlich ist, dass Benutzer aus einer bestimmten Region nur von Endpunkten in der betreffenden Region bedient werden.

### <a name="what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Welche Bereiche werden von Traffic Manager für das geografische Routing unterstützt? 
Die Landes/Regionshierarchie, die von Traffic Manager verwendet wird, finden Sie [hier](traffic-manager-geographic-regions.md). Diese Seite wird jeweils auf dem neuesten Änderungsstatus gehalten, jedoch können Sie die gleichen Informationen auch programmgesteuert mithilfe der [Azure Traffic Manager-REST-API](https://docs.microsoft.com/rest/api/trafficmanager/) abrufen. 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Wie bestimmt Traffic Manager, woher die Abfrage eines Benutzers stammt? 
Traffic Manager untersucht die IP-Quelladresse der Abfrage (dabei handelt es sich mit größter Wahrscheinlichkeit um einen lokalen DNS-Resolver, der die Abfrage im Auftrag des Benutzers ausführt) und verwendet eine interne Zuordnung von IPs zu Regionen, um den Standort zu bestimmen. Diese Zuordnung wird fortlaufend aktualisiert, um Änderungen im Internet Rechnung zu tragen. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case"></a>Ist garantiert, dass Traffic Manager immer den genauen geografischen Standort des Benutzers ermitteln kann?
Nein, Traffic Manager kann aus den folgenden Gründen nicht garantieren, dass die geografische Region, die aus der Quell-IP-Adresse einer DNS-Abfrage abgeleitet wird, immer dem Benutzerstandort entspricht: 

- 1. Wie in der vorherigen Frage beschrieben wurde, ist die erkannte Quell-IP-Adresse die eines DNS-Resolvers, der das Lookup im Namen des Benutzers durchführt. Wenngleich der geografische Standort des DNS-Resolvers ein guter Anhaltspunkt für den geografischen Standort des Benutzers ist, kann er je nach Speicherort des DNS-Resolverdients und dem vom Kunden verwendeten spezifischen DNS-Resolverdienst abweichen. Hier ein Beispiel: Ein in Malaysia ansässiger Kunde könnte in den Geräteeinstellungen einen DNS-Resolverdienst angeben, dessen zugehöriger DNS-Server in Singapur möglicherweise ausgewählt wird, um die Abfrageauflösungen für diesen Benutzer/Dienst durchzuführen. In diesem Fall kann Traffic Manager nur die IP-Adresse des Resolvers sehen, die dem Standort in Singapur entspricht. Siehe hierzu auch die Frage zur Unterstützung von Clientsubnetzadressen auf dieser Seite.

- 2. Traffic Manager verwendet eine interne Zuordnung zur Übersetzung der IP-Adressen in geografische Regionen. Wenngleich diese Zuordnung regelmäßig überprüft und aktualisiert wird, um ihre Genauigkeit zu erhöhen und der ständigen Weiterentwicklung des Internets Rechnung zu tragen, besteht dennoch die Möglichkeit, dass unsere Informationen den geografischen Standort nicht für alle IP-Adressen genau abbilden.


###  <a name="does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing"></a>Muss sich ein Endpunkt physisch in der Region befinden, für die er für geografisches Routen konfiguriert ist? 
Nein, der Standort des Endpunkts bringt keine Einschränkungen hinsichtlich der ihm zuzuordnenden Regionen mit sich. Beispielsweise können einem Endpunkt in der Azure-Region „US-Central“ alle Benutzer aus Indien zugeleitet werden.

### <a name="can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing"></a>Kann ich geografische Regionen Endpunkten in einem Profil zuordnen, das nicht für geografisches Routing konfiguriert ist? 

Ja, falls die Routingmethode eines Profils nicht geografisch ist, können Sie die [Azure Traffic Manager-REST-API](https://docs.microsoft.com/rest/api/trafficmanager/) verwenden, um Endpunkten im betreffenden Profil geografische Regionen zuzuweisen. Diese Konfiguration wird für Profile von nicht geografischen Routingtypen ignoriert. Wenn Sie ein derartiges Profil zu einem späteren Zeitpunkt auf den geografischen Routingtyp umstellen, kann Traffic Manager diese Zuordnungen verwenden.


### <a name="why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic"></a>Warum tritt ein Fehler auf, wenn ich versuche, die Routingmethode eines vorhandenen Profils in geografisch zu ändern?

Allen Endpunkten in einem Profil mit geografischem Routing muss mindestens eine geografische Region zugeordnet sein. Um ein vorhandenes Profil in den geografischen Routingtyp zu konvertieren, müssen Sie zuerst mithilfe der [Azure Traffic Manager-REST-API](https://docs.microsoft.com/rest/api/trafficmanager/) allen seinen Endpunkten geografische Regionen zuordnen, bevor Sie den Routingtyp in geografisch ändern. Wenn Sie das Portal verwenden, löschen Sie zuerst die Endpunkte, ändern Sie die Routingmethode des Profils in geografisch und fügen Sie anschließend alle Endpunkte zusammen mit ihrer geografischen Regionszuordnung hinzu. 


###  <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Warum wird es Kunden dringend empfohlen, in einem Profil mit aktiviertem geografischem Routing geschachtelte Profile anstelle von Endpunkten zu erstellen? 

Beim geografischen Routingtyp kann eine Region nur einem Endpunkt innerhalb eines Profils zugeordnet sein. Wenn der betreffende Endpunkt kein geschachtelter Typ mit zugeordnetem untergeordnetem Profil ist, fährt Traffic Manager auch bei nicht intaktem Integritätsstatus des betreffenden Endpunkts fort, Verkehr an den Endpunkt zu senden, da die Alternative, gar keinen Verkehr zu senden, nicht besser ist. Traffic Manager führt auch dann kein Failover zu einem anderen Endpunkt aus, wenn die zugeordnete Region dem nicht mehr intakten Endpunkt „übergeordnet“ ist (z.B. erfolgt kein Failover auf einen anderen Endpunkt mit der Region „Europa“, wenn ein Endpunkt mit der Region „Spanien“ nicht mehr intakt ist). Dies erfolgt, um sicherzustellen, dass Traffic Manager die geografischen Grenzen beachtet, die Kunden in ihren Profilen konfiguriert haben. Um den Vorzug des Failovers auf einen anderen Endpunkt nutzen zu können, wenn die Integrität eines Endpunkts nicht mehr intakt ist, wird empfohlen, dass geografische Regionen geschachtelten Profilen, die mehrere Endpunkte enthalten, anstelle von einzelnen Endpunkten zugeordnet werden. Wenn ein Endpunkt innerhalb des untergeordneten Profils ausfällt, kann auf diese Weise ein Failover des Datenverkehrs auf einen anderen Endpunkt innerhalb des gleichen geschachtelten untergeordneten Profils erfolgen.

### <a name="are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type"></a>Gibt es Einschränkungen hinsichtlich der API-Version, die diesen Routingtyp unterstützt?

Ja, nur die API-Version 2017-03-01 und höhere Versionen unterstützen den geografischen Routingtyp. Ältere API-Versionen können nicht zum Erstellen von Profilen vom geografischen Routingtyp oder zum Zuweisen von geografischen Regionen zu Endpunkten verwendet werden. Wenn eine ältere API-Version zum Abrufen von Profilen aus einem Azure-Abonnement verwendet wird, werden keine Profile mit dem geografischen Routingtyp zurückgegeben. Darüber hinaus wird bei Verwendung älterer API-Versionen für zurückgegebene Profile, die Endpunkte mit einer geografischen Regionszuweisung aufweisen, die geografische Regionszuweisung nicht angezeigt.

## <a name="real-user-measurements"></a>Benutzer-Realmessungen

>[!NOTE]
>Das Feature „Benutzer-Realmessungen“ in Traffic Manager befindet sich in der öffentlichen Vorschauphase (Public Preview) und ist unter Umständen nicht so verfügbar und zuverlässig wie Features in Versionen mit allgemeiner Verfügbarkeit. Das Feature wird nicht unterstützt, bietet möglicherweise eingeschränkte Funktionen und ist vielleicht nicht an allen Azure-Standorten verfügbar. Aktuelle Hinweise zur Verfügbarkeit und zum Status dieses Features finden Sie auf der Seite mit den [Azure Traffic Manager-Updates](https://azure.microsoft.com/updates/?product=traffic-manager).

### <a name="what-are-the-benefits-of-using-real-user-measurements"></a>Welche Vorteile hat die Verwendung von Benutzer-Realmessungen?
Wenn Sie die Routingmethode „Leistung“ verwenden, wählt Traffic Manager für Ihre Endbenutzer die beste Azure-Region aus, mit der eine Verbindung hergestellt werden kann. Hierzu werden die Quell-IP und das EDNS-Clientsubnetz (falls übergeben) untersucht und mit den Intelligence-Daten zur Netzwerklatenz des Diensts abgeglichen. Mithilfe von Benutzer-Realmessungen wird dies für Ihre Endbenutzer verbessert, indem deren Erfahrungen zu dieser Latenztabelle einen Beitrag leisten. Außerdem wird sichergestellt, dass diese Tabelle in adäquater Weise die Endbenutzernetzwerke widerspiegelt, aus denen die Endbenutzer Verbindungen mit Azure herstellen. Dies führt zu einer höheren Genauigkeit beim Routing für Ihre Endbenutzer.

### <a name="can-i-use-real-user-measurements-with-non-azure-regions"></a>Kann ich Benutzer-Realmessungen auch für andere Regionen als Azure-Regionen verwenden?
Bei Benutzer-Realmessungen wird nur die Latenz bei der Erreichung von Azure-Regionen gemessen und gemeldet. Wenn Sie das leistungsbasierte Routing für Endpunkte verwenden, die in anderen Regionen als Azure-Regionen gehostet werden, können Sie trotzdem von diesem Feature profitieren. Sie verfügen so nämlich über mehr Latenzinformationen zur repräsentativen Azure-Region, die Sie für die Zuordnung zu diesem Endpunkt ausgewählt haben.

### <a name="which-routing-method-benefits-from-real-user-measurements"></a>Welche Routingmethode profitiert von Benutzer-Realmessungen?
Die zusätzlichen Informationen, die mit Benutzer-Realmessungen ermittelt werden, gelten nur für Profile, für die die Routingmethode „Leistung“ verwendet wird. Beachten Sie, dass der Link für „Benutzer-Realmessungen“ für alle Profile verfügbar ist, wenn Sie das Azure-Portal zum Anzeigen verwenden.

### <a name="do-i-need-to-enable-real-user-measurements-each-profile-separately"></a>Muss ich Benutzer-Realmessungen für jedes Profil separat aktivieren?
Nein. Sie müssen sie nur einmal pro Abonnement aktivieren. Alle gemessenen und gemeldeten Latenzinformationen sind dann für alle Profile verfügbar.

### <a name="how-do-i-turn-off-real-user-measurements-for-my-subscription"></a>Wie deaktiviere ich Benutzer-Realmessungen für mein Abonnement?
Sie können die Berechnung von Gebühren für Benutzer-Realmessungen beenden, wenn Sie aufhören, über Ihre Clientanwendung Latenzmessungen zu sammeln und einzusenden. Wenn JavaScript-Code für die Messung beispielsweise in Webseiten eingebettet ist, können Sie die Nutzung dieses Features beenden, indem Sie den JavaScript-Code entfernen oder das Aufrufen beim Rendern der Seite deaktivieren.
Eine weitere Möglichkeit zum Deaktivieren von Benutzer-Realmessungen ist das Löschen Ihres Schlüssels. Wenn Sie die Deaktivierung durchführen, werden alle Messungen verworfen, die über diesen Schlüssel an Traffic Manager gesendet werden.

### <a name="can-i-use-real-user-measurements-with-client-applications-other-than-web-pages"></a>Kann ich Benutzer-Realmessungen mit anderen Anwendungen als Webseiten nutzen?
Ja. Benutzer-Realmessungen sind so konzipiert, dass Daten erfasst werden können, die über unterschiedliche Arten von Endbenutzerclients gesammelt werden. Diese häufig gestellten Fragen werden aktualisiert, wenn neue Arten von Clientanwendungen unterstützt werden.

### <a name="how-many-measurements-are-made-each-time-my-real-user-measurements-enabled-web-page-is-rendered"></a>Wie viele Messungen werden jeweils durchgeführt, wenn meine Webseite mit aktivierten Benutzer-Realmessungen gerendert wird?
Wenn Benutzer-Realmessungen mit dem angegebenen JavaScript-Code für Messungen verwendet werden, führt jeder Renderingvorgang der Seite dazu, dass sechs Messungen durchgeführt werden. Diese werden dann an den Traffic Manager-Dienst gemeldet. Beachten Sie, dass Ihnen dieses Feature basierend auf der Anzahl von Messungen berechnet wird, die an den Traffic Manager-Dienst gemeldet werden. Wenn der Benutzer beispielsweise von Ihrer Webseite weg navigiert, während die Messungen durchgeführt werden, und dies vor dem Melden der Ergebnisse geschieht, werden diese Messungen nicht für die Abrechnung herangezogen.

### <a name="is-there-a-delay-before-real-user-measurements-script-runs-in-my-webpage"></a>Kommt es zu einer Verzögerung, bevor das Skript für Benutzer-Realmessungen auf meiner Webseite ausgeführt wird?
Nein. Vor dem Aufrufen des Skripts kommt es nicht zu einer programmierten Verzögerung.

### <a name="can-i-use-configure-real-user-measurements-with-only-the-azure-regions-i-want-to-measure"></a>Kann ich Benutzer-Realmessungen nur für die Azure-Regionen verwenden, für die ich Messungen durchführen möchte?
Nein. Bei jedem Aufruf wird mit dem Skript für Benutzer-Realmessungen eine Gruppe von sechs Azure-Regionen gemessen, die vom Dienst bestimmt wird. Diese Gruppe ändert sich zwischen den einzelnen Aufrufen, und wenn eine große Zahl dieser Aufrufe erfolgt, werden durch die Messungen unterschiedliche Azure-Regionen abgedeckt.

### <a name="can-i-limit-the-number-of-measurements-made-to-a-specific-number"></a>Kann ich die Anzahl von durchgeführten Messungen auf einen bestimmten Wert beschränken?
Der JavaScript-Code für die Messung ist in Ihre Webseite eingebettet, und Sie haben die vollständige Kontrolle darüber, wann Sie den Vorgang starten und beenden. Solange der Traffic Manager-Dienst eine Anforderung zum Messen einer Liste mit Azure-Regionen erhält, wird eine Gruppe von Regionen zurückgegeben. Beachten Sie auch, dass Ihnen während der Vorschauphase keine Messungen, die an Traffic Manager gemeldet werden, berechnet werden.

### <a name="can-i-see-the-measurements-taken-by-my-client-application-as-part-of-real-user-measurements"></a>Kann ich die Messungen, die von meiner Clientanwendung durchgeführt werden, als Teil der Benutzer-Realmessungen anzeigen?
Da die Messlogik über Ihre Clientanwendung ausgeführt wird, haben Sie die vollständige Kontrolle über die Abläufe, also auch über die Anzeige der Latenzmessungen. Traffic Manager stellt keine Aggregatansicht der Messungen bereit, die über den mit Ihrem Abonnement verknüpften Schlüssel eingehen.

### <a name="can-i-modify-the-measurement-script-provided-by-traffic-manager"></a>Kann ich das von Traffic Manager bereitgestellte Skript für die Messungen ändern?
Sie können zwar steuern, was auf Ihrer Webseite eingebettet ist, aber wir raten dringend davon ab, Änderungen am Skript für die Messungen vorzunehmen. So ist sichergestellt, dass die Latenzen richtig gemessen und gemeldet werden.

### <a name="will-it-be-possible-for-others-to-see-the-key-i-use-with-real-user-measurements"></a>Können andere Personen den Schlüssel sehen, den ich für Benutzer-Realmessungen verwende?
Wenn Sie das Skript für die Messungen in eine Webseite einbetten, können andere Personen das Skript und Ihren Schlüssel für Benutzer-Realmessungen (Real User Measurements, RUM) anzeigen. Folgender Hinweis ist hierbei wichtig: Dieser Schlüssel unterscheidet sich von der Abonnement-ID und wird von Traffic Manager nur für diesen Zweck generiert. Wenn Ihr RUM-Schlüssel anderen Personen bekannt ist, bedeutet dies also nicht, dass die Sicherheit Ihres Azure-Kontos kompromittiert ist.

### <a name="can-others-abuse-my-rum-key"></a>Können andere Personen meinen RUM-Schlüssel für schädliche Zwecke missbrauchen?
Es ist zwar möglich, dass Ihr Schlüssel von anderen Personen genutzt wird, um falsche Informationen an Azure zu senden. Durch einige falsche Messergebnisse ändert sich aber nicht das Routing, da diese zusammen mit allen anderen Messungen, die wir erhalten, berücksichtigt werden. Falls Sie Ihre Schlüssel ändern müssen, können Sie einen Schlüssel neu generieren. Der alte Schlüssel wird dann verworfen.

###  <a name="do-i-need-to-put-the-measurement-javascript-in-all-my-web-pages"></a>Muss ich den JavaScript-Code für die Messungen auf allen meinen Webseiten einfügen?
Je höher die Anzahl von Messungen ist, desto größer ist der Nutzen von Benutzer-Realmessungen. Es ist aber Ihre Entscheidung, ob Sie den Code auf allen Webseiten oder nur auf ausgewählten Webseiten einfügen möchten. Wir raten Ihnen, den Code zuerst auf Ihrer meistbesuchten Seite einzufügen, auf der Benutzer normalerweise mindestens fünf Sekunden lang verweilen.

### <a name="can-information-about-my-end-users-be-identified-by-traffic-manager-if-i-use-real-user-measurements"></a>Können Informationen zu meinen Endbenutzern von Traffic Manager identifiziert werden, wenn ich Benutzer-Realmessungen verwende?
Wenn der bereitgestellte JavaScript-Code für Messungen verwendet wird, sind für Traffic Manager die Client-IP-Adresse des Endbenutzers und die Quell-IP-Adresse der genutzten lokalen DNS-Auflösung sichtbar. Die Client-IP-Adresse wird von Traffic Manager erst verwendet, nachdem diese abgeschnitten wurde. Auf diese Weise ist es nicht möglich, den jeweiligen Endbenutzer zu identifizieren, der die Messungen gesendet hat. 

### <a name="does-the-webpage-measuring-real-user-measurements-need-to-be-using-traffic-manager-for-routing"></a>Muss für die Webseite, für die Benutzer-Realmessungen durchgeführt werden, Traffic Manager zu Routingzwecken eingesetzt werden?
Nein. Die Verwendung von Traffic Manager ist nicht erforderlich. Der Routingbereich von Traffic Manager ist vom Bereich für die Benutzer-Realmessungen getrennt, und es ist zwar hilfreich, wenn beide unter derselben Webeigenschaft angeordnet sind, aber dies muss nicht zwingend der Fall sein.

### <a name="do-i-need-to-host-any-service-on-azure-regions-to-use-with-real-user-measurements"></a>Muss ich für die Verwendung mit Benutzer-Realmessungen einen Dienst in den Azure-Regionen hosten?
Nein. Es ist nicht erforderlich, serverseitige Komponenten in Azure zu hosten, damit Benutzer-Realmessungen funktionieren. Das Bild mit dem einzelnen Pixel, das vom JavaScript für die Messungen heruntergeladen wird, und der Dienst, der in den verschiedenen Azure-Regionen ausgeführt wird, wird von Azure gehostet und verwaltet. 

### <a name="will-my-azure-bandwidth-usage-increase-when-i-use-real-user-measurements"></a>Erhöht sich meine Azure-Bandbreitenauslastung, wenn ich Benutzer-Realmessungen verwende?
Wie schon in der vorherigen Antwort erwähnt: Die serverseitigen Komponenten von Benutzer-Realmessungen befinden sich im Besitz von Azure und werden auch von Azure verwaltet. Dies bedeutet, dass sich Ihre Azure-Bandbreitenauslastung nicht erhöht, wenn Sie Benutzer-Realmessungen verwenden. Beachten Sie, dass dies nicht für die Bandbreitenauslastung außerhalb der abgerechneten Gebühren von Azure gilt. Wir verringern die Bandbreite, indem wir ein Bild mit nur einem Pixel herunterladen, um die Latenz einer Azure-Region zu messen. 

## <a name="traffic-view"></a>Datenverkehrsansicht

>[!NOTE]
>Das Feature „Datenverkehrsansicht“ in Traffic Manager befindet sich in der öffentlichen Vorschauphase (Public Preview) und ist unter Umständen nicht so verfügbar und zuverlässig wie Features in Versionen mit allgemeiner Verfügbarkeit. Das Feature wird nicht unterstützt, bietet möglicherweise eingeschränkte Funktionen und ist vielleicht nicht an allen Azure-Standorten verfügbar. Aktuelle Hinweise zur Verfügbarkeit und zum Status dieses Features finden Sie auf der Seite mit den [Azure Traffic Manager-Updates](https://azure.microsoft.com/updates/?product=traffic-manager).

### <a name="what-does-traffic-view-do"></a>Wozu dient die Datenverkehrsansicht?
Die Datenverkehrsansicht ist ein Feature von Traffic Manager, mit dem Sie mehr Informationen zu Ihren Benutzern und zur Benutzerfreundlichkeit erhalten. Mithilfe der Abfragen von Traffic Manager und den Tabellen mit den Intelligence-Daten zur Netzwerklatenz, die vom Dienst gepflegt werden, werden folgende Informationen für Sie bereitgestellt:
- Die Regionen, aus denen Ihre Benutzer eine Verbindung mit Ihren Endpunkten in Azure herstellen.
- Die Menge an Benutzern, die aus diesen Regionen Verbindungen herstellen.
- Die Azure-Regionen, an die die Benutzer weitergeleitet werden.
- Die Latenzdaten der Benutzer für diese Azure-Regionen.

Diese Informationen können Sie im Portal in einer Tabellenansicht nutzen und außerdem als Rohdaten herunterladen.

### <a name="how-can-i-benefit-from-using-traffic-view"></a>Welche Vorteile ergeben sich für mich durch die Verwendung der Datenverkehrsansicht?

Die Datenverkehrsansicht ermöglicht Ihnen eine Gesamtübersicht über den Datenverkehr, der bei Ihren Traffic Manager-Profilen eingeht. Sie dient vor allem dem besseren Verständnis, von wo aus Ihre Benutzer Verbindungen herstellen und – was genauso wichtig ist – welche durchschnittliche Latenz dabei zu beobachten ist. Sie können diese Informationen dann nutzen, um Bereiche zu identifizieren, die Ihre besondere Aufmerksamkeit erfordern. Beispielsweise können Sie Ihre Azure-Abdeckung auf eine Region erweitern, in der die Latenz für diese Benutzer geringer ist. Eine weiterer Vorteil bei Verwendung der Datenverkehrsansicht ist die Verfolgung von Datenverkehrsmustern für verschiedene Regionen. Diese Informationen können hilfreich für das Treffen der Entscheidung sein, ob Sie die Anstrengungen für diese Regionen erhöhen oder verringern sollten.

### <a name="how-is-traffic-view-different-from-the-traffic-manager-metrics-available-through-azure-monitor"></a>Inwiefern unterscheidet sich die Datenverkehrsansicht von den Traffic Manager-Metriken, die über Azure Monitor verfügbar sind?

Azure Monitor kann genutzt werden, um den Datenverkehr, der von Ihrem Profil und dessen Endpunkten eingeht, auf Aggregatebene zu verstehen. Außerdem können Sie den Integritätsstatus der Endpunkte nachverfolgen, indem Sie die Ergebnisse der Integritätsprüfung verfügbar machen. Wenn Ihre Anforderungen höher sind und Sie verstehen möchten, wie sich die Verbindungsherstellung mit Azure auf regionaler Ebene für Benutzer gestaltet, können Sie dies mithilfe der Datenverkehrsansicht erreichen.

### <a name="does-traffic-view-use-edns-client-subnet-information"></a>Werden für die Datenverkehrsansicht EDNS-Clientsubnetz-Informationen verwendet?

Von der Datenverkehrsansicht werden beim Erstellen der Ausgabe keine EDNS-Clientsubnetz-Informationen verwendet. Sie nutzt die IP-Adresse des lokalen DNS-Resolvers Ihrer Benutzer, um die Gruppierung durchzuführen.

### <a name="how-many-days-of-data-does-traffic-view-use"></a>Wie viele Tage mit Daten werden von der Datenverkehrsansicht genutzt?

Die Datenverkehrsansicht erstellt ihre Ausgabe, indem die Daten der letzten sieben Tage vor dem Tag verwendet werden, an dem Sie die Informationen anzeigen. Hierbei handelt es sich um ein bewegliches Fenster, und es werden jeweils die neuesten Daten verwendet, wenn Sie das Feature nutzen.

### <a name="how-does-traffic-view-handle-external-endpoints"></a>Wie werden externe Endpunkte von der Datenverkehrsansicht behandelt?

Bei Verwendung von externen Endpunkten, die außerhalb von Azure-Regionen gehostet werden, in einem Traffic Manager-Profil können Sie die Zuordnung zu einer Azure-Region wählen, die dann quasi als Proxy für die Latenzmerkmale dient (dies ist erforderlich, wenn Sie die Routingmethode „Leistung“ verwenden). Wenn diese Zuordnung zu einer Azure-Region vorhanden ist, werden beim Erstellen der Ausgabe für die Datenverkehrsansicht die Latenzmetriken dieser Azure-Region genutzt. Falls keine Azure-Region angegeben ist, sind die Latenzinformationen in den Daten für diese externen Endpunkte leer.

### <a name="do-i-need-to-enable-traffic-view-for-each-profile-in-my-subscription"></a>Muss ich die Datenverkehrsansicht für jedes Profil meines Abonnements aktivieren?
Während der Vorschauphase wird die Datenverkehrsansicht auf Abonnementebene aktiviert und ist für alle Traffic Manager-Profile des Abonnements verfügbar.

### <a name="how-can-i-turn-off-traffic-view"></a>Wie kann ich die Datenverkehrsansicht deaktivieren?
Während der Vorschauphase ist es erforderlich, dass Sie ein Supportticket erstellen, um die Datenverkehrsansicht für Ihr Abonnement zu deaktivieren.

### <a name="how-does-traffic-view-billing-work"></a>Wie funktioniert die Abrechnung für die Datenverkehrsansicht?

Die Preise für die Datenverkehrsansicht basieren auf der Anzahl von Datenpunkten, die zum Erstellen der Ausgabe verwendet werden. Derzeit werden als Datentyp nur die Abfragen unterstützt, die von Ihrem Profil empfangen werden. Außerdem wird Ihnen nur die Verarbeitung berechnet, die durchgeführt wurde, als Sie die Datenverkehrsansicht aktiviert hatten. Dies bedeutet Folgendes: Wenn Sie die Datenverkehrsansicht während eines Monats einige Zeit aktivieren und dann wieder deaktivieren, werden nur die Datenpunkte, die bei aktiviertem Feature verarbeitet wurden, zur Abrechnung herangezogen.
Während der Vorschauphase werden Ihnen für die Nutzung der Datenverkehrsansicht keine Gebühren berechnet.

## <a name="traffic-manager-endpoints"></a>Traffic Manager-Endpunkte

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Kann ich Traffic Manager mit Endpunkten mehrerer Abonnements verwenden?

Die Verwendung von Endpunkten aus mehreren Abonnements ist mit Azure-Web-Apps nicht möglich. Für Azure-Web-Apps dürfen mit Web-Apps verwendete, benutzerdefinierte Domänennamen nur innerhalb eines einzelnen Abonnements genutzt werden. Es ist nicht möglich, Web-Apps aus mehreren Abonnements mit dem gleichen Domänennamen zu verwenden.

Für andere Endpunkttypen kann Traffic Manager mit Endpunkten aus mehreren Abonnements verwendet werden. In Resource Manager können Endpunkte aus jedem Abonnement zu Traffic Manager hinzugefügt werden, solange die Person, die das Traffic Manager-Profil konfiguriert, über Lesezugriff für den Endpunkt verfügt. Diese Berechtigungen können über die [rollenbasierte Zugriffssteuerung (RBAC) von Azure Resource Manager](../active-directory/role-based-access-control-configure.md)gewährt werden.


### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Kann ich Traffic Manager mit „Stagingslots“ des Clouddiensts verwenden?

Ja. Stagingslots des Clouddiensts können in Traffic Manager als externe Endpunkte konfiguriert werden. Integritätsprüfungen werden weiterhin mit der Rate von Azure-Endpunkten berechnet. Änderungen des zugrunde liegenden Diensts werden nicht automatisch übernommen, da ein Endpunkt vom Typ „Extern“ verwendet wird. Mit externen Endpunkten kann Traffic Manager nicht erkennen, wenn der Clouddienst beendet oder gelöscht wird. Aus diesem Grund wird die Abrechnung für Integritätsprüfungen von Traffic Manager fortgesetzt, bis der Endpunkt deaktiviert oder gelöscht wird.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Unterstützt Traffic Manager IPv6-Endpunkte?

Traffic Manager stellt derzeit keine IPv6-adressierbaren Nameserver zur Verfügung. Traffic Manager kann jedoch weiterhin von IPv6-Clients verwendet werden, die IPv6-Endpunkte verbinden. Ein Client erstellt keine DNS-Anfragen direkt an Traffic Manager. Stattdessen verwendet der Client einen rekursiven DNS-Dienst. Ein reiner IPv6-Client sendet Anforderungen an den rekursiven DNS-Dienst über IPv6. Der rekursive Dienst sollte anschließend in der Lage sein, die Traffic Manager-Namensserver per IPv4 zu kontaktieren.

Traffic Manager antwortet mit dem DNS-Namen des Endpunkts. Ein IPv6-Endpunkt kann unterstützt werden, indem ein DNS AAAA-Eintrag konfiguriert wird, mit dem der DNS-Name des Endpunkts auf die IPv6-Adresse verwiesen wird. Traffic Manager-Integritätsprüfungen unterstützen nur IPv4-Adressen. Der Dienst muss einen IPv4-Endpunkt auf dem gleichen DNS-Namen verfügbar machen.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Kann ich Traffic Manager mit mehr als einer Web-App in derselben Region verwenden?

Normalerweise wird Traffic Manager verwendet, um Datenverkehr an Anwendungen zu leiten, die in verschiedenen Regionen bereitgestellt wurden. Traffic Manager kann aber auch eingesetzt werden, wenn eine Anwendung über mehr als eine Bereitstellung in derselben Region verfügt. Die Traffic Manager-Azure-Endpunkte lassen es nicht zu, dass demselben Traffic Manager-Profil mehr als ein Web-App-Endpunkt für dieselbe Azure-Region hinzugefügt wird.

### <a name="how-do-i-move-my-traffic-manager-profiles-azure-endpoints-to-a-different-resource-group"></a>Wie werden die Azure-Endpunkte meines Traffic Manager-Profils in eine andere Ressourcengruppe verschoben?

Azure-Endpunkte, die einem Traffic Manager-Profil zugeordnet sind, werden über ihre Ressourcen-IDs nachverfolgt. Wenn eine Azure-Ressource, die als Endpunkt (z. B. öffentliche IP-Adresse, klassischer Clouddienst, WebApp oder anderes Traffic Manager-Profil, das in geschachtelte Weise verwendet wird) verwendet wird, in eine andere Ressourcengruppe verschoben wird, ändert sich ihre Ressourcen-ID. In diesem Szenario müssen Sie das Traffic Manager-Profil derzeit aktualisieren, indem Sie zuerst die Endpunkte löschen und sie dann wieder zum Profil hinzufügen. 

##  <a name="traffic-manager-endpoint-monitoring"></a>Traffic Manager-Endpunktüberwachung

### <a name="is-traffic-manager-resilient-to-azure-region-failures"></a>Ist Traffic Manager gegenüber Ausfällen von Azure-Regionen resistent?

Traffic Manager ist eine Schlüsselkomponente der Bereitstellung von hoch verfügbaren Anwendungen in Azure.
Um hohe Verfügbarkeit bereitzustellen, muss Traffic Manager ein außergewöhnlich hohes Maß an Verfügbarkeit aufweisen und resistent gegenüber Regionsausfällen sein.

Traffic Manager-Komponenten sind standardmäßig gegenüber einem vollständigen Ausfall jeder Azure-Region resistent. Diese Resilienz gilt für alle Traffic Manager-Komponenten: DNS-Namensserver, API, Speicherebene und Endpunkt-Überwachungsdienst.

Im unwahrscheinlichen Fall eines Ausfalls einer gesamten Azure-Region wird erwartet, dass Traffic Manager weiterhin normal funktioniert. Bei Anwendungen, die in mehreren Azure-Regionen bereitgestellt wurden, kann darauf vertraut werden, dass Traffic Manager Datenverkehr an eine verfügbare Instanz der Anwendung leitet.

### <a name="how-does-the-choice-of-resource-group-location-affect-traffic-manager"></a>Wie wirkt sich die Wahl des Ressourcengruppenstandorts auf Traffic Manager aus?

Traffic Manager ist ein einzelner globaler Dienst. Es ist kein regionaler Dienst. Die Wahl des Ressourcengruppenstandorts spielt für Traffic Manager-Profile, die in der Ressourcengruppe bereitgestellt werden, keine Rolle.

Für Azure Resource Manager ist es erforderlich, dass für alle Ressourcengruppen ein Standort angegeben wird, mit dem der Standardstandort für die in dieser Ressourcengruppe bereitgestellten Ressourcen bestimmt wird. Wenn Sie ein Traffic Manager-Profil erstellen, wird es in einer Ressourcengruppe erstellt. Für alle Traffic Manager-Profile wird als Standort **global** verwendet, wobei der Standardwert der Ressourcengruppe überschrieben wird.

### <a name="how-do-i-determine-the-current-health-of-each-endpoint"></a>Wie kann ich den aktuellen Integritätsstatus jedes Endpunkts bestimmen?

Der aktuelle Überwachungsstatus jedes Endpunkts sowie das Gesamtprofil werden im Azure-Portal angezeigt. Diese Informationen sind auch über die [REST-API](https://msdn.microsoft.com/library/azure/mt163667.aspx) von Traffic Manager, [PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt125941.aspx) und die [plattformübergreifende Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md) verfügbar.

Azure stellt keine alten Informationen über vergangene Endpunktintegrität bzw. die Möglichkeit zum Auslösen von Alarmen bei Änderungen der Endpunktintegrität bereit.

### <a name="can-i-monitor-https-endpoints"></a>Kann ich HTTPS-Endpunkte überwachen?

Ja. Traffic Manager unterstützt Tests per HTTPS. Konfigurieren Sie in der Überwachungskonfiguration **HTTPS** als Protokoll.

Traffic Manager kann keine Zertifikatüberprüfung bereitstellen. Dazu zählt Folgendes:

* Serverseitige Zertifikate werden nicht überprüft.
* Serverseitige SNI-Zertifikate werden nicht unterstützt.
* Clientzertifikate werden nicht unterstützt.

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>Kann ich Traffic Manager auch dann verwenden, wenn meine Anwendung HTTP oder HTTPS nicht unterstützt?

Ja. Wenn Sie TCP als Überwachungsprotokoll angeben, kann Traffic Manager eine TCP-Verbindung initiieren und auf eine Antwort vom Endpunkt warten. Wenn der Endpunkt auf die Verbindungsanforderung innerhalb des Zeitlimits mit einer Antwort zum Herstellen der Verbindung antwortet, wird dieser Endpunkt als fehlerfrei markiert.

### <a name="what-specific-responses-are-required-from-the-endpoint-when-using-tcp-monitoring"></a>Welche bestimmten Antworten vom Endpunkt sind bei Verwendung der TCP-Überwachung erforderlich?

Bei Verwendung der TCP-Überwachung startet Traffic Manager durch Senden einer SYN-Anforderung an den Endpunkt am angegebenen Port ein Drei-Wege-TCP-Handshake. Er wartet dann eine bestimmte Zeit (wie in den Timeouteinstellungen angegeben) auf eine Antwort vom Endpunkt. Wenn der Endpunkt innerhalb des in den Überwachungseinstellungen angegeben Timeoutzeitraums auf die SYN-Anforderung mit einer SYN-ACK-Antwort reagiert, wird dieser Endpunkt als fehlerfrei angesehen. Bei Empfang der SYN-ACK-Antwort setzt Traffic Manager die Verbindung mit einer RST-Antwort zurück.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>Wie schnell verschiebt Traffic Manager meine Benutzer von einem fehlerhaften Endpunkt weg?

Traffic Manager bietet mehrere Einstellungen, mit denen Sie wie folgt das Failoververhalten Ihres Traffic Manager-Profils steuern können:
- Sie können angeben, dass Traffic Manager die Endpunkte häufiger prüft, indem Sie das Testintervall auf 10 Sekunden festlegen. Dadurch wird sichergestellt, dass jeder fehlerhafte Endpunkt so bald wie möglich erkannt werden kann. 
- Sie können einen Timeoutwert für die Anforderung einer Integritätsprüfung festlegen (der Minimalwert sind 5 Sekunden).
- Sie können angeben, wie viele Fehler auftreten können, bevor der Endpunkt als fehlerhaft markiert wird. Dieser Wert kann niedriger als 0 (null) sein. In diesem Fall wird der Endpunkt als fehlerhaft anzeigt, sobald die erste Integritätsprüfung negativ ausfällt. Jedoch kann ein Mindestwert von 0 für die zulässige Anzahl von Fehlern dazu führen, dass Endpunkte aufgrund vorübergehender, zum Zeitpunkt der Überprüfung auftretender Probleme aus der Rotation herausgenommen werden.
- Sie können für die DNS-Antwort eine minimale Gültigkeitsdauer (TTL) von 0 angeben. Dies bedeutet, dass die DNS-Resolver die Antwort nicht zwischenspeichern können, und jede neue Abfrage eine Antwort erhält, die die aktuellen Integritätsinformationen wiedergibt, über die Traffic Manager verfügt.

Mithilfe dieser Einstellungen kann Traffic Manager Failover in weniger als 10 Sekunden bereitstellen, nachdem ein Endpunkt fehlerhaft ist und eine DNS-Abfrage des entsprechenden Profils erfolgt.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Wie kann ich verschiedene Überwachungseinstellungen für verschiedene Endpunkte in einem Profil angeben?

Traffic Manager-Überwachungseinstellungen werden pro Profil festgelegt. Wenn Sie nur für einen Endpunkt eine andere Überwachungseinstellung verwenden müssen, kann dieser Endpunkt als [geschachteltes Profil](traffic-manager-nested-profiles.md) konfiguriert werden, dessen Überwachungseinstellungen sich vom übergeordneten Profil unterscheiden.

### <a name="what-host-header-do-endpoint-health-checks-use"></a>Welcher Hostheader wird für die Integritätsprüfungen für Endpunkte verwendet?

Traffic Manager verwendet Hostheader in HTTP- und HTTPS-Integritätsprüfungen. Der von Traffic Manager verwendete Hostheader ist der Name des Endpunktziels, das im Profil konfiguriert wurde. Der im Hostheader verwendete Wert kann nicht separat von der Zieleigenschaft („target“) angegeben werden.

### <a name="what-are-the-ip-addresses-from-which-the-health-checks-originate"></a>Wie lauten die IP-Adressen, von denen die Integritätsüberprüfungen stammen?

Die folgende Liste enthält die IP-Adressen, aus denen Traffic Manager-Integritätsprüfungen stammen können. Sie können anhand dieser Liste sicherstellen, dass eingehende Verbindungen von diesen IP-Adressen an den Endpunkten zur Überprüfung des Integritätsstatus zugelassen sind.

* 40.68.30.66
* 40.68.31.178
* 137.135.80.149
* 137.135.82.249
* 23.96.236.252
* 65.52.217.19
* 40.87.147.10
* 40.87.151.34
* 13.75.124.254
* 13.75.127.63
* 52.172.155.168
* 52.172.158.37
* 104.215.91.84
* 13.75.153.124
* 13.84.222.37
* 23.101.191.199
* 23.96.213.12
* 137.135.46.163
* 137.135.47.215
* 191.232.208.52
* 191.232.214.62
* 13.75.152.253
* 104.41.187.209
* 104.41.190.203
* 52.173.90.107
* 52.173.250.232
* 104.45.149.110
* 40.114.5.197
* 52.240.151.125
* 52.240.144.45
* 13.65.95.152
* 13.65.92.252
* 40.78.67.110
* 104.42.192.195

### <a name="how-many-health-checks-to-my-endpoint-can-i-expect-from-traffic-manager"></a>Wie viele Integritätsprüfungen meines Endpunkts kann ich von Traffic Manager erwarten?

Die Anzahl der Traffic Manager-Integritätsprüfungen Ihres Endpunkts hängt von Folgendem ab:
- dem Wert, den Sie für das Überwachungsintervall festgelegt haben (ein kleineres Intervall bedeutet, dass in einem bestimmten Zeitraum mehr Anforderungen auf dem Endpunkt eingehen).
- der Anzahl der Standorte, von denen die Integritätsprüfungen ausgehen (die IP-Adressen, von denen aus diese Überprüfungen durchgeführt werden können, sind in der vorherigen FAQ aufgelistet).

## <a name="traffic-manager-nested-profiles"></a>Geschachtelte Profile in Traffic Manager

### <a name="how-do-i-configure-nested-profiles"></a>Wie konfiguriere ich geschachtelte Profile?

Geschachtelte Traffic Manager-Profile können sowohl mit Azure Resource Manager als auch mit den klassischen Azure-REST-APIs, Azure PowerShell-Cmdlets und plattformübergreifenden Befehlen der Azure-Befehlszeilenschnittstelle konfiguriert werden. Sie werden auch im neuen Azure-Portal unterstützt. Über das klassische Portal werden sie jedoch nicht unterstützt.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Wie viele Schachtelungsebenen werden in Traffic Manager unterstützt?

Sie können Profile bis zu 10 Ebenen schachteln. Schleifen sind nicht zulässig.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Kann ich andere Endpunkttypen mit geschachtelten untergeordneten Profilen im gleichen Traffic Manager-Profil kombinieren?

Ja. Es gibt keine Einschränkungen hinsichtlich der Kombination von Endpunkten verschiedener Typen innerhalb eines Profils.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Wie wird das Abrechnungsmodell auf geschachtelte Profile angewendet?

Die Verwendung geschachtelter Profile wirkt sich nicht negativ auf die Preise aus.

Die Abrechnung für Traffic Manager besteht aus zwei Komponenten: Integritätsprüfungen der Endpunkte und Millionen von DNS-Abfragen.

* Überprüfungen der Endpunktintegrität: Wenn ein untergeordnetes Profil als Endpunkt in einem übergeordneten Profil konfiguriert wird, fallen keine Gebühren an. Die Überwachung von Endpunkten im untergeordneten Profil wird in gewohnter Weise abgerechnet.
* DNS-Abfragen: Jede Abfrage wird nur einmal gezählt. Die Abfrage eines übergeordneten Profils, die einen Endpunkt aus einem untergeordneten Profil zurückgibt, wird nur im übergeordneten Profil gezählt.

Ausführliche Informationen finden Sie auf der Seite [Traffic Manager Preise](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Wirken sich geschachtelte Profile auf die Leistung aus?

Nein. Durch geschachtelte Profile entstehen keine Leistungseinbußen.

Die Traffic Manager-Namenserver durchlaufen bei der Verarbeitung der einzelnen DNS-Abfragen intern die Profilhierarchie. Eine DNS-Abfrage an ein übergeordnetes Profil kann eine DNS-Antwort mit einem Endpunkt aus einem untergeordneten Profil erhalten. Unabhängig davon, ob Sie ein einzelnes Profil oder geschachtelte Profile verwenden, wird nur ein einziger CNAME-Eintrag verwendet. Es gibt keine Notwendigkeit, für jedes Profil in der Hierarchie einen CNAME-Eintrag zu erstellen.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Wie berechnet Traffic Manager die Integrität eines geschachtelten Endpunkts in einem übergeordneten Profil?

Das übergeordnete Profil führt die Integritätsprüfung nicht direkt am untergeordneten Profil durch. Stattdessen wird mit der Integrität der Endpunkte des untergeordneten Profils die Gesamtintegrität des untergeordneten Profils berechnet. Diese Informationen werden in der Hierarchie der geschachtelten Profile nach oben weitergegeben, um die Integrität des geschachtelten Endpunkts zu bestimmen. Das übergeordnete Profil verwendet diese aggregierte Integrität, um festzulegen, ob der Datenverkehr an das untergeordnete Profil weitergeleitet wird.

Die folgende Tabelle beschreibt das Verhalten von Traffic Manager-Integritätsprüfungen für einen geschachtelten Endpunkt.

| Überwachungsstatus von untergeordneten Profilen | Überwachungsstatus von übergeordneten Endpunkten | Hinweise |
| --- | --- | --- |
| Deaktiviert. Das untergeordnete Profil wurde deaktiviert. |Stopped |Der Status des übergeordneten Endpunkts ist "Beendet", nicht "Deaktiviert". Der Status "Deaktiviert" zeigt lediglich an, dass Sie den Endpunkt im übergeordneten Profil deaktiviert haben. |
| Heruntergestuft. Der Status mindestens eines Endpunkts wurde heruntergestuft. |Online: Die Anzahl von Onlineendpunkten im untergeordneten Profil ist mindestens gleich dem Wert von MinChildEndpoints.<BR>CheckingEndpoint: Die Anzahl von Online- plus CheckingEndpoint-Endpunkten im untergeordneten Profil ist mindestens gleich dem Wert von MinChildEndpoints.<BR>Heruntergestuft: andere Fälle. |Der Datenverkehr wird an einen Endpunkt mit dem Status „CheckingEndpoint“ weitergeleitet. Wenn MinChildEndpoints zu hoch festgelegt wurde, wird der Endpunkt immer heruntergestuft. |
| Online. Mindestens ein untergeordneter Profilendpunkt weist den Status „Online“ auf. Kein Endpunkt weist den Status „Heruntergestuft“ auf. |Siehe oben. | |
| CheckingEndpoints. Mindestens ein untergeordneter Profilendpunkt weist den Status „CheckingEndpoint“ auf. Kein Endpunkt weist den Status „Online“ oder „Heruntergestuft“ auf. |Wie oben. | |
| Inaktiv. Alle untergeordneten Profilendpunkte sind „Deaktiviert“ oder „Beendet“, oder dieses Profil weist keine Endpunkte auf. |Beendet | |

## <a name="next-steps"></a>Nächste Schritte:
- Informieren Sie sich ausführlicher über [Endpunktüberwachung und automatisches Failover](../traffic-manager/traffic-manager-monitoring.md)von Traffic Manager.
- Informieren Sie sich ausführlicher über [Traffic Manager-Routingmethoden](../traffic-manager/traffic-manager-routing-methods.md).
