---
title: Azure Advisor-Empfehlungen zur Leistung | Microsoft Docs
description: Nutzen Sie den Advisor, um die Leistung Ihrer Azure-Bereitstellungen zu optimieren.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 3331c795cbb1c45820d4c86d287ef57b54f0ae6b
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247641"
---
# <a name="advisor-performance-recommendations"></a>Advisor-Empfehlungen zur Leistung

Dank der Empfehlungen vom Azure Advisor zur Leistung können Sie die Geschwindigkeit und Reaktionsfähigkeit Ihrer unternehmenskritischen Anwendungen verbessern. Empfehlungen des Advisor zur Leistung erhalten Sie auf dem Advisor-Dashboard auf der Registerkarte **Leistung**.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>Reduzieren der DNS-Gültigkeitsdauer des Traffic Manager-Profils für ein schnelleres Failover auf fehlerfreie Endpunkte

In den [Gültigkeitsdauereinstellungen](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-performance-considerations) Ihres Traffic Manager-Profils können Sie angeben, wie schnell Endpunkte gewechselt werden, wenn ein bestimmter Endpunkt nicht mehr auf Abfragen reagiert. Wenn Sie die Werte der Gültigkeitsdauer reduzieren, können Clients schneller an funktionierende Endpunkte weitergeleitet werden.

Azure Advisor ermittelt Traffic Manager-Profile mit einer längeren Gültigkeitsdauer und empfiehlt, die Gültigkeitsdauer auf 20 oder 60 Sekunden zu konfigurieren, je nachdem, ob das Profil für [Schnelles Failover](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/) konfiguriert ist.

## <a name="improve-database-performance-with-sql-db-advisor"></a>Verbessern der Datenbankleistung mit SQL DB Advisor

Advisor bietet Ihnen eine einheitliche, konsolidierte Ansicht der Empfehlungen für alle Ihre Azure-Ressourcen. Azure Advisor und Azure SQL Database Advisor sind integriert, um Ihnen Empfehlungen zum Verbessern der Leistung Ihrer SQL Azure-Datenbank zu bieten. SQL Database Advisor bewertet die Leistung durch eine Analyse des Nutzungsverlaufs Ihrer SQL Azure-Datenbank. Anschließend werden Empfehlungen gegeben, die für die typische Workload der Datenbank am besten geeignet sind. 

> [!NOTE]
> Um Empfehlungen zu erhalten, muss eine Datenbank ungefähr eine Woche lang genutzt werden, und innerhalb dieser Woche müssen durchweg Aktivitäten erfolgen. SQL Database Advisor kann leichter für einheitliche Abfragemuster als für zufällige Aktivitätsspitzen optimiert werden.

Weitere Informationen zu SQL Database Advisor finden Sie unter [SQL Database Advisor](https://azure.microsoft.com/documentation/articles/sql-database-advisor/).

## <a name="improve-redis-cache-performance-and-reliability"></a>Verbessern der Leistung und Zuverlässigkeit von Redis-Cache

Der Advisor ermittelt Redis Cache-Instanzen, bei denen die Leistung ggf. durch eine hohe Auslastung von Arbeitsspeicher oder Netzwerkbandbreite, Serverlast oder große Anzahl von Clientverbindungen beeinträchtigt wird. Darüber hinaus empfiehlt der Advisor bewährte Methoden, um potenzielle Probleme zu vermeiden. Weitere Informationen zu Redis Cache-Empfehlungen finden Sie unter [Redis Cache Advisor](https://azure.microsoft.com/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>Verbessern der Leistung und Zuverlässigkeit von App Services

Azure Advisor befolgt empfohlene bewährte Methoden zum Verbessern der App Services-Leistung und Erkennen relevanter Plattformfunktionen. Beispielempfehlungen für App Services sind u.a.:
* Erkennen von Instanzen, bei denen Arbeitsspeicher- oder CPU-Ressourcen durch App-Laufzeiten ausgeschöpft sind, samt Abhilfeoptionen
* Erkennen von Instanzen, bei denen durch Zusammenstellen von Ressourcen wie Web-Apps und Datenbanken die Leistung verbessert und Kosten gesenkt werden können 

Weitere Informationen zu App Services-Empfehlungen finden Sie unter [Bewährte Methoden für Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Zugreifen auf Advisor-Empfehlungen zur Leistung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie [Advisor](https://aka.ms/azureadvisordashboard).

2.  Klicken Sie auf dem Advisor-Dashboard auf die Registerkarte **Leistung**.

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie weitere Informationen zu Empfehlungen des Advisor:

* [Einführung in Advisor](advisor-overview.md)
* [Erste Schritte mit Advisor](advisor-get-started.md)
* [Advisor-Empfehlungen zu Kosten](advisor-performance-recommendations.md)
* [Advisor-Empfehlungen für Hochverfügbarkeit](advisor-high-availability-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)

