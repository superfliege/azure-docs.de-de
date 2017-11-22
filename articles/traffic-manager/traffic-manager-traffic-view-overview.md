---
title: Datenverkehrsansicht in Azure Traffic Manager | Microsoft-Dokumentation
description: "Einführung in die Traffic Manager-Datenverkehrsansicht"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 11/11/2017
ms.author: kumud
ms.custom: 
ms.openlocfilehash: 6b4378cb293824702dd52dcdeb86619f957b83ea
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="traffic-manager-traffic-view"></a>Traffic Manager-Datenverkehrsansicht

>[!NOTE]
>Das Feature „Datenverkehrsansicht“ in Traffic Manager befindet sich in der öffentlichen Vorschauphase (Public Preview) und ist unter Umständen nicht so verfügbar und zuverlässig wie Features in Versionen mit allgemeiner Verfügbarkeit. Das Feature wird nicht unterstützt, bietet möglicherweise eingeschränkte Funktionen und ist vielleicht nicht an allen Azure-Standorten verfügbar. Aktuelle Hinweise zur Verfügbarkeit und zum Status dieses Features finden Sie auf der Seite mit den [Azure Traffic Manager-Updates](https://azure.microsoft.com/updates/?product=traffic-manager).

Traffic Manager stellt Ihnen ein Routing auf DNS-Ebene zur Verfügung, sodass Ihre Endbenutzer auf Grundlage der Routingmethode, die Sie beim Erstellen des Profils angegeben haben, an fehlerfreie Endpunkte weitergeleitet werden. Die Datenverkehrsansicht bietet Traffic Manager einen Überblick über Ihre Benutzergruppen (auf Granularitätsebene des DNS-Resolvers) und deren Datenverkehrsmuster. Wenn Sie die Datenverkehrsansicht aktivieren, werden diese Informationen verarbeitet, um Ihnen verwertbare Erkenntnisse zu liefern. 

Die Datenverkehrsansicht ermöglicht Folgendes:
- Verstehen, wo sich Ihre Benutzergruppen befinden (bis zur Granularitätsebene eines lokalen DNS-Resolvers)
- Anzeigen des Umfangs des Datenverkehrs (als DNS-Abfragen beobachtet, die von Azure Traffic Manager bearbeitet wurden), der aus diesen Regionen stammt.
- Erhalten von Einblicken hinsichtlich der repräsentativen Wartezeit dieser Benutzer.
- Detailliertes Untersuchen der spezifischen Datenverkehrsmuster jeder Benutzergruppen in Azure-Regionen, in denen Sie Endpunkte haben 

So können Sie z.B. in der Datenverkehrsansicht nachvollziehen, welche Regionen zwar ein großes Volumen an Datenverkehr haben, aber unter längeren Wartezeiten leiden. Als Nächstes können Sie diese Informationen nutzen, um Ihre Expansion in neue Azure-Regionen zu planen, damit diese Benutzer kürzere Wartezeiten haben.

## <a name="how-traffic-view-works"></a>Funktionsweise der Datenverkehrsansicht

Grundlage der Datenverkehrsansicht ist das Vergleichen der in den letzten sieben Tagen eingegangenen Abfragen durch Traffic Manager mit einem Profil, für das dieses Feature aktiviert ist. Aus den Informationen der eingegangenen Abfragen extrahiert die Datenverkehrsansicht die Quell-IP-Adresse des DNS-Resolvers, die als Darstellung des Standorts der Benutzer verwendet wird. Diese werden anschließend mit einer Granularität auf DNS-Resolverebene gruppiert, um Regionen von Benutzergruppen zu erstellen, indem die geografischen Informationen der von Traffic Manager verwalteten IP-Adressen verwendet werden. Traffic Manager sieht sich dann die Azure-Regionen an, an die die Abfrage geleitet wurde, und erstellt eine Karte des Datenverkehrsflusses der Benutzer aus diesen Regionen.  
Im nächsten Schritt korreliert Traffic Manager die Benutzergruppenregion mit der Azure-Regionskarte mithilfe der Informationen in den Tabellen mit den Netzwerkwartezeiten. Diese werden für verschiedene Endbenutzernetzwerke verwaltet, um die durchschnittliche Wartezeit für Benutzer aus diesen Regionen zu ermitteln, wenn eine Verbindung mit Azure-Regionen hergestellt wird. Alle diese Berechnungen werden dann kombiniert, und zwar pro lokaler IP-Adressebene des DNS-Resolvers, bevor sie Ihnen angezeigt werden. Sie können die Informationen auf verschiedene Weise nutzen.

>[!NOTE]
>Die in der Datenverkehrsansicht beschriebene Wartezeit ist eine repräsentative Wartezeit zwischen dem Endbenutzer und den Azure-Regionen, mit denen eine Verbindung hergestellt wurde, und nicht die Wartezeit von DNS-Lookup.

## <a name="visual-overview"></a>Visuelle Übersicht

Wenn Sie zum Abschnitt **Datenverkehrsansicht** auf Ihrer Traffic Manager-Seite navigieren, sehen Sie eine geografische Karte, die mit Erkenntnissen der Datenverkehrsansicht überlagert ist. Die Karte bietet Informationen zur Benutzergruppe und den Endpunkten für Ihr Traffic Manager-Profil.

### <a name="user-base-information"></a>Benutzergruppeninformationen

Für die lokalen DNS-Resolver, für die Standortinformationen verfügbar sind, werden diese auf der Karte angezeigt. Die Farbe des DNS-Resolvers kennzeichnet die durchschnittliche Wartezeit für Endbenutzer, die diesen DNS-Resolver für ihre Traffic Manager-Abfragen verwendet haben.

Wenn Sie auf den Standort eines DNS-Resolvers auf der Karte zeigen, wird Folgendes eingeblendet:
- die IP-Adresse des DNS-Resolvers
- das Datenverkehrsvolumen für DNS-Abfragen aus der Sicht von Traffic Manager
- die Endpunkte, an die Datenverkehr vom DNS-Resolver weitergeleitet wurde, als eine Linie zwischen dem Endpunkt und dem DNS-Resolver 
- die durchschnittliche Wartezeit von diesem Standort bis zum Endpunkt, dargestellt als Farbe der verbindenden Linie

### <a name="endpoint-information"></a>Endpunktinformationen

Die Azure-Regionen, in denen sich die Endpunkte befinden, werden als blaue Punkte auf der Karte angezeigt. Klicken Sie auf einen beliebigen Endpunkt, um die verschiedenen Standorte anzuzeigen (basierend auf dem verwendeten DNS-Resolver), von denen Datenverkehr an diesen Endpunkt weitergeleitet wurde. Die Verbindungen werden als Linie zwischen dem Endpunkt und dem Standort des DNS-Resolvers angezeigt, und die Farbe entspricht der repräsentativen Wartezeit zwischen diesen beiden. Darüber hinaus sehen Sie den Namen des Endpunkts, die Azure-Region, in der er ausgeführt wird, und das Gesamtvolumen der Anforderungen, die von diesem Traffic Manager-Profil hierhin weitergeleitet wurden.


## <a name="tabular-listing-and-raw-data-download"></a>Tabellarische Auflistung und Rohdatendownload

Sie können die Daten der Datenverkehrsansicht in einem tabellarischen Format im Azure-Portal anzeigen. Es ist ein Eintrag für jede Kombination aus DNS-Resolver-IP-Adresse und Endpunkt vorhanden, der Folgendes umfasst: den geografischen Standort des DNS-Resolvers (wenn verfügbar), den Namen der Azure-Region, in der sich der Endpunkt befindet, das Anforderungsvolumen im Zusammenhang mit diesem DNS-Resolver und die repräsentative Wartezeit im Zusammenhang mit Endbenutzern, die diesen DNS verwenden (wenn verfügbar). Sie können die Daten der Datenverkehrsansicht auch als CSV-Datei herunterladen, die dann als Teil eines gewünschten Analyseworkflows verwendet werden kann.

## <a name="billing"></a>Abrechnung

Wenn Sie die Datenverkehrsansicht verwenden, erfolgt die Abrechnung anhand der Anzahl der Datenpunkte, die zur Erstellung der gezeigten Erkenntnisse verwendet wurden. Der derzeit einzige verwendete Datenpunkt sind die Abfragen, die vom Traffic Manager-Profil empfangen werden. Weitere Informationen zu den Preisen finden Sie auf der [Traffic Manager-Seite mit Preisdetails](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [Funktionsweise von Traffic Manager](traffic-manager-overview.md)
- Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md) .
- Informationen zum [Erstellen eines Traffic Manager-Profils](traffic-manager-create-profile.md)

