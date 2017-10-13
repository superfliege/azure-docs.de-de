---
title: Benutzer-Realmessungen in Azure Traffic Manager | Microsoft-Dokumentation
description: "Einführung in Benutzer-Realmessungen in Traffic Manager"
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
ms.openlocfilehash: a7e8ae605b6234341d9ab8b790f4c54d8627f29f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="traffic-manager-real-user-measurements-overview"></a>Übersicht über Benutzer-Realmessungen in Traffic Manager

>[!NOTE]
>Das Feature „Benutzer-Realmessungen“ in Traffic Manager befindet sich in der öffentlichen Vorschauphase (Public Preview) und ist unter Umständen nicht so verfügbar und zuverlässig wie Features in Versionen mit allgemeiner Verfügbarkeit. Das Feature wird nicht unterstützt, bietet möglicherweise eingeschränkte Funktionen und ist vielleicht nicht an allen Azure-Standorten verfügbar. Aktuelle Hinweise zur Verfügbarkeit und zum Status dieses Features finden Sie auf der Seite mit den [Azure Traffic Manager-Updates](https://azure.microsoft.com/updates/?product=traffic-manager).

Wenn Sie ein Traffic Manager-Profil einrichten, um die Leistungsroutingmethode zu verwenden, prüft der Dienst zuerst, woher die DNS-Abfragen stammen. Anschließend trifft er Routingentscheidungen, um diese Anfragen an die Azure-Region zu leiten, die die kürzeste Wartezeit bietet. Dies geschieht mithilfe der Tabellen mit den Netzwerkwartezeiten, die Traffic Manager für verschiedene Endbenutzernetzwerke verwaltet.

Mithilfe von Benutzer-Realmessungen können Sie die Netzwerkwartezeiten in Azure-Regionen anhand der von Ihren Endbenutzern verwendeten Clientanwendungen messen. Traffic Manager berücksichtigt diese Informationen anschließend zum Treffen von Routingentscheidungen. Wenn Sie Benutzer-Realmessungen einsetzen, können Sie die Genauigkeit des Routings für Anforderungen erhöhen, die von den Netzwerken mit Ihren Endbenutzern stammen. 

## <a name="how-real-user-measurements-work"></a>Funktionsweise von Benutzer-Realmessungen

Für Benutzer-Realmessungen messen Ihre Clientanwendungen die Wartezeit in Azure-Regionen, wie diese in den Endbenutzernetzwerken wahrgenommen wird, in denen diese genutzt werden. Wenn Sie beispielsweise eine Webseite haben, auf die Benutzer von verschiedenen Standorten aus zugreifen (z.B. in den nordamerikanischen Regionen), können Sie die Leistung von Benutzer-Realmessungen nutzen. Verwenden Sie dazu die Leistungsroutingmethode, um sie zur besten Azure-Region zu leiten, in der Ihre Serveranwendung gehostet wird.

Zunächst müssen Sie in Ihre Webseiten ein von Azure bereitgestelltes JavaScript (mit enthaltenem eindeutigen Schlüssel) einbetten. Sobald dies erfolgt ist, fragt das JavaScript, wann immer ein Benutzer diese Webseite besucht, Traffic Manager ab, um Informationen zu den Azure-Regionen zu erhalten, die es messen soll. Der Dienst gibt eine Reihe von Endpunkten an das Skript zurück. Dieses misst dann nacheinander diese Regionen, indem ein in diesen Azure-Regionen gehostetes 1-Pixel-Bild heruntergeladen wird. Anschließend wird die Wartezeit zwischen dem Zeitpunkt des Sendens der Anforderung und dem Empfang des ersten Bytes erfasst. Diese Messungen werden dann dem Traffic Manager-Dienst zurückgemeldet.

Im Laufe der Zeit geschieht dies viele Male und in vielen Netzwerken, was dazu führt, dass Traffic Manager genauere Informationen zu den Wartezeiten der Netzwerke erhält, in denen sich Ihre Endbenutzer befinden. Diese Informationen beginnen, in die von Traffic Manager getroffenen Routingentscheidungen einzufließen. Dies führt zu einer erhöhten Genauigkeit bei den Entscheidungen, die auf den gesendeten Benutzer-Realmessungen basieren.

Wenn Sie Benutzer-Realmessungen verwenden, wird Ihnen die Anzahl der Messungen, die an den Traffic Manager gesendet wurden, in Rechnung gestellt. Weitere Informationen zu den Preisen finden Sie auf der [Traffic Manager-Seite mit Preisdetails](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [Benutzer-Realmessungen mit Webseiten](traffic-manager-create-rum-web-pages.md) verwenden
- Informieren Sie sich über die [Funktionsweise von Traffic Manager](traffic-manager-overview.md)
- Erfahren Sie mehr zu [Mobile Center](https://docs.microsoft.com/mobile-center/)
- Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md) .
- Informationen zum [Erstellen eines Traffic Manager-Profils](traffic-manager-create-profile.md)

