---
title: 'Azure SQL Data Warehouse: Anmerkungen zu dieser Version – August 2018 | Microsoft-Dokumentation'
description: Versionshinweise zu Azure SQL Data Warehouse
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/06/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 1f3b17f3163c29f9b9e1e47e14ccdbc1e37e1010
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39633658"
---
# <a name="whats-new-in-azure-sql-data-warehouse-august-2018"></a>Neuerungen in Azure SQL Data Warehouse August 2018
Azure SQL Data Warehouse wird fortlaufend verbessert. In diesem Artikel informieren wir Sie über neue Features und Änderungen, die im August 2018 eingeführt wurden.

## <a name="automatic-intelligent-insights"></a>Automatic Intelligent Insights
Microsoft hat [Automatic Intelligent Insights](https://azure.microsoft.com/blog/automatic-intelligent-insights-to-optimize-performance-with-sql-data-warehouse/) eingeführt, um die Automatisierung von Data Warehouses durch die Cloud zu ermöglichen. Somit müssen Sie Ihr Data Warehouse nicht mehr im Hinblick auf Datenschiefe und suboptimale Tabellenstatistik überwachen. SQL Data Warehouse stellt ohne Aufpreis intelligente Informationen für alle Gen2-Instanzen bereit. Durch die Integration in [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) können Sie automatisch Empfehlungen zu Best Practices erhalten, um die Leistung Ihrer aktiven Workloads zu verbessern. SQL Data Warehouse analysiert Ihre Workload und stellt Empfehlungen basierend auf Ihrer Nutzung bereit. Diese Analyse erfolgt täglich, sodass Sie die Nutzungsberichte und Empfehlungen für Verbesserungen an Ihrer Workload überwachen können.

Sie können die Empfehlungen im Azure Advisor-Portal sehen: ![Empfehlungen im Azure Advisor-Portal für Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/4e205b6d-df04-48db-8eec-d591f2592cf4.png)

Sie können einen Drilldown auf jede Kategorie ausführen, um die Empfehlungen für die jeweilige Warnung anzuzeigen: ![Details im Azure Advisor-Portal zu Empfehlungen für Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/3c42426e-6969-46e3-9025-c34c0755a302.png)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit den Grundlagen von SQL Data Warehouse vertraut gemacht haben, können Sie sich nun darüber informieren, wie Sie schnell [eine SQL Data Warehouse-Instanz erstellen][create a SQL Data Warehouse]. Falls Sie mit Azure noch nicht vertraut sind und auf neue Terminologie stoßen, ist das [Azure-Glossar][Azure glossary] sehr nützlich. Weitere SQL Data Warehouse-Ressourcen:  

* [Kundenerfolgsgeschichten]
* [Blogs]
* [Funktionsanfragen]
* [Videos]
* [Customer Advisory Team-Blogs]
* [Stack Overflow-Forum]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Customer Advisory Team-Blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Kundenerfolgsgeschichten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Funktionsanfragen]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow-Forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md