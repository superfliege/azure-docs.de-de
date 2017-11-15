---
title: Kostenempfehlungen von Azure Advisor | Microsoft Docs
description: Nutzen Sie Azure Advisor, um die Kosten Ihrer Azure-Bereitstellungen zu optimieren.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 7b9c7037271fabd67c1ada80420ad72c340e46bb
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2017
---
# <a name="advisor-cost-recommendations"></a>Advisor-Empfehlungen zu Kosten

Advisor hilft Ihnen beim Optimieren und Senken Ihrer Gesamtausgaben für Azure, indem Ressourcen im Leerlauf bzw. zu gering ausgelastete Ressourcen ermittelt werden. Kostenempfehlungen finden Sie auf dem Ratgeberdashboard auf der Registerkarte **Kosten**.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Optimieren der Kosten für virtuelle Computer durch Ändern der Größe oder Herunterfahren von zu gering ausgelasteten Instanzen 
Obwohl bestimmte Anwendungsszenarien zielgerichtet eine geringe Auslastung bewirken, können Sie häufig Kosten sparen, indem Sie Größe und Anzahl Ihrer virtuellen Computer steuern. Der Ratgeber überwacht die Verwendung Ihrer virtuellen Computer 14 Tage lang und ermittelt virtuelle Computer mit geringer Auslastung. Virtuelle Computer, bei denen an mindestens vier Tagen die CPU-Auslastung unter 5 Prozent und die Netzwerklast unter 7 MB lag, gelten als virtuelle Computer mit geringer Auslastung.

Der Ratgeber gibt die geschätzten Kosten für die weitere Ausführung des virtuellen Computers an, sodass Sie entscheiden können, ob Sie diesen herunterfahren oder seine Größe ändern.

Wenn Sie konsequenter zu gering ausgelastete virtuelle Computer ermitteln möchten, können Sie die Regel für durchschnittliche CPU-Auslastung auf Abonnementbasis anpassen.

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a>Verwenden einer kostengünstigen Lösung, um die Leistungsziele mehrerer SQL-Datenbanken zu erreichen
Advisor ermittelt SQL Server-Instanzen, die vom Erstellen von Pools für elastische Datenbanken profitieren können. Pools für elastische Datenbanken stellen eine einfache und wirtschaftliche Lösung zum Erreichen der Leistungsziele mehrerer Datenbanken dar, die unterschiedliche Nutzungsmuster aufweisen. Unter [Was ist ein Azure-Pool für elastische Datenbanken?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/) finden Sie Informationen zu Azure-Pools für elastische Datenbanken.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Kostensenkung durch die Beseitigung nicht bereitgestellter ExpressRoute-Verbindungen
Der Ratgeber identifiziert ExpressRoute-Verbindungen, die sich für mehr als einen Monat im Anbieterstatus *Nicht bereitgestellt* befunden haben, und empfiehlt, die Verbindung zu löschen, wenn Sie nicht beabsichtigen, die Verbindung durch Ihren Dienstanbieter bereitzustellen.

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Zugreifen auf Kostenempfehlungen im Azure-Ratgeber

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie den [Ratgeber](https://aka.ms/azureadvisordashboard).

2.  Klicken Sie auf dem Ratgeberdashboard auf die Registerkarte **Kosten**.

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie weitere Informationen zu Empfehlungen des Ratgebers:
* [Einführung in Advisor](advisor-overview.md)
* [Erste Schritte](advisor-get-started.md)
* [Advisor-Empfehlungen zur Leistung](advisor-cost-recommendations.md)
* [Advisor-Empfehlungen für hohe Verfügbarkeit](advisor-cost-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-cost-recommendations.md)
