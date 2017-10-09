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
ms.date: 09/19/2017
ms.author: kumud
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: f1ab98ab173edc794bfabdf55d950ac689a37c0a
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="traffic-manager-traffic-view"></a>Traffic Manager-Datenverkehrsansicht

>[!NOTE]
>Das Feature „Datenverkehrsansicht“ in Traffic Manager befindet sich in der öffentlichen Vorschauphase (Public Preview) und ist unter Umständen nicht so verfügbar und zuverlässig wie Features in Versionen mit allgemeiner Verfügbarkeit. Das Feature wird nicht unterstützt, bietet möglicherweise eingeschränkte Funktionen und ist vielleicht nicht an allen Azure-Standorten verfügbar. Aktuelle Hinweise zur Verfügbarkeit und zum Status dieses Features finden Sie auf der Seite mit den [Azure Traffic Manager-Updates](https://azure.microsoft.com/updates/?product=traffic-manager).

Traffic Manager stellt Ihnen ein Routing auf DNS-Ebene zur Verfügung, sodass Ihre Endbenutzer auf Grundlage der Routingmethode, die Sie bei der Erstellung des Profils eingerichtet haben, an fehlerfreie Endpunkte weitergeleitet werden. Dadurch bietet Traffic Manager einen Überblick über Ihre Benutzergruppen (auf Granularitätsebene des DNS-Resolvers) und deren Datenverkehrsmuster. Wenn Sie die Datenverkehrsansicht aktivieren, werden diese Informationen verarbeitet, um Ihnen verwertbare Erkenntnisse zu liefern. 

Die Datenverkehrsansicht ermöglicht Folgendes:
- Verstehen, wo sich Ihre Benutzergruppen befinden (bis zur Granularitätsebene eines lokalen DNS-Resolvers)
- Bestimmen des Volumens des Datenverkehrs, der aus diesen Regionen stammt
- Ermitteln der repräsentativen Wartezeit dieser Benutzer
- Detailliertes Untersuchen der spezifischen Datenverkehrsmuster jeder Benutzergruppen in Azure-Regionen, in denen Sie Endpunkte haben 

So können Sie z.B. in der Datenverkehrsansicht nachvollziehen, welche Regionen zwar ein großes Volumen an Datenverkehr haben, aber unter längeren Wartezeiten leiden. Als Nächstes können Sie diese Informationen nutzen, um Ihre Expansion in neue Azure-Regionen zu planen, damit diese Benutzer kürzere Wartezeiten haben.

## <a name="how-traffic-view-works"></a>Funktionsweise der Datenverkehrsansicht

Grundlage der Datenverkehrsansicht ist das Vergleichen der in den letzten sieben Tagen eingegangenen Abfragen durch Traffic Manager mit einem Profil, für das dieses Feature aktiviert ist. Aus diesen Informationen extrahiert Traffic Manager die Quell-IP-Adresse des DNS-Resolvers, die dann als Darstellung des Standorts der Benutzer verwendet wird. Diese werden anschließend mit einer Granularität auf DNS-Resolverebene gruppiert, um Regionen von Benutzergruppen zu erstellen, indem die geografischen Informationen der von Traffic Manager verwalteten IP-Adressen verwendet werden. Traffic Manager sieht sich dann die Azure-Regionen an, an die die Abfrage geleitet wurde, und erstellt eine Karte des Datenverkehrsflusses der Benutzer aus diesen Regionen.
Im nächsten Schritt korreliert Traffic Manager die Benutzergruppenregion mit der Azure-Regionskarte mithilfe der Informationen in den Tabellen mit den Netzwerkwartezeiten. Diese werden für verschiedene Endbenutzernetzwerke verwaltet, um die durchschnittliche Wartezeit für Benutzer aus diesen Regionen zu ermitteln, wenn eine Verbindung mit Azure-Regionen hergestellt wird. Alle diese Berechnungen werden dann tabellarisch angeordnet, und zwar pro lokaler IP-Adressebene des DNS-Resolvers, bevor sie Ihnen angezeigt werden. Sie können diese Informationen in einem Analyseworkflow Ihrer Wahl weiterverarbeiten oder als CSV-Datei herunterladen.
Wenn Sie die Datenverkehrsansicht verwenden, erfolgt die Abrechnung anhand der Anzahl der Datenpunkte, die zur Erstellung der gezeigten Erkenntnisse verwendet wurden. Der derzeit einzige verwendete Datenpunkt sind die Abfragen, die vom Traffic Manager-Profil empfangen werden. Weitere Informationen zu den Preisen finden Sie auf der [Traffic Manager-Seite mit Preisdetails](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [Funktionsweise von Traffic Manager](traffic-manager-overview.md)
- Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md) .
- Informationen zum [Erstellen eines Traffic Manager-Profils](traffic-manager-create-profile.md)


