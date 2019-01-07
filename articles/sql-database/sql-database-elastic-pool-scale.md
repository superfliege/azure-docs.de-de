---
title: Skalieren von Ressourcen für Pools für elastische Datenbanken – Azure SQL-Datenbank | Microsoft-Dokumentation
description: Diese Seite beschreibt die Skalierung von Ressourcen für Pools für elastische Datenbanken in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: 04fcb84b22e84060652b4a3743cb73e4543ee573
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52868445"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Skalieren von Ressourcen für Pools für elastische Datenbanken in Azure SQL-Datenbank

In diesem Artikel wird beschrieben, wie die für Pools für elastische Datenbanken und in einem Pool zusammengefassten Datenbanken in Azure SQL-Datenbank verfügbaren Compute- und Speicherressourcen skaliert werden können.

## <a name="vcore-based-purchasing-model-change-elastic-pool-storage-size"></a>Auf virtuellen Kernen basierendes Erwerbsmodell: Ändern der Speichergröße in einem Pool für elastische Datenbanken

- Speicher kann bis zur maximalen Speichergröße bereitgestellt werden:

  - Für Speicher vom Typ „Standard“ erhöhen bzw. verringern Sie die Größe in Schritten von 10 GB.
  - Für Speicher vom Typ „Premium“ erhöhen bzw. verringern Sie die Größe in Schritten von 250 GB.
- Der Speicher für einen Pool für elastische Datenbanken kann durch Erhöhen oder Verringern der Maximalgröße bereitgestellt werden.
- Der Preis für den Speicher für einen Pool für elastische Datenbanken errechnet sich aus der Menge an bereitgestelltem Speicher multipliziert mit dem Speichereinheitenpreis für den Diensttarif. Ausführliche Informationen zu den Preisen für zusätzlichen Speicherplatz siehe [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Unter bestimmten Umständen müssen Sie ggf. eine Datenbank verkleinern, um ungenutzten Speicherplatz freizugeben. Weitere Informationen finden Sie unter [Verwalten von Dateispeicherplatz in Azure SQL-Datenbank](sql-database-file-space-management.md).

## <a name="vcore-based-purchasing-model-change-elastic-pool-compute-resources-vcores"></a>Auf virtuellen Kernen basierendes Erwerbsmodell: Ändern von Computeressourcen in einem Pool für elastische Datenbanken (virtuelle Kerne)

Sie können die Computegröße für einen Pool für elastische Datenbanken basierend auf den Ressourcenanforderungen im [Azure-Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), in [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), in der [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update) oder in der [REST-API](https://docs.microsoft.com/rest/api/sql/elasticpools/update) erhöhen oder verringern.

- Beim erneuten Skalieren von virtuellen Kernen in einem Pool für elastische Datenbanken werden Datenbankverbindungen vorübergehend getrennt. Dieses Verhalten ist auch beim erneuten Skalieren von DTUs für eine einzelne Datenbank zu beobachten. Ausführliche Informationen zu Dauer und Auswirkungen der Trennung von Verbindungen für eine Datenbank bei Vorgängen zur erneuten Skalierung finden Sie unter [Auf DTUs basierendes Kaufmodell: Ändern von Computeressourcen (DTUs)](sql-database-single-database-scale.md#dtu-based-purchasing-model-change-compute-resources-dtus).
- Die Dauer der Neuskalierung von virtuellen Kernen in einem Pool kann von der Gesamtmenge des Speicherplatzes abhängen, die von allen Datenbanken im Pool verwendet wird. Im Allgemeinen beträgt die Neuskalierungs-Latenzzeit durchschnittlich höchstens 90 Minuten pro 100 GB. Wenn beispielsweise der gesamte, von allen Datenbanken im Pool verwendete Speicherplatz 200 GB beträgt, beträgt die erwartete Neuskalierungs-Latenzzeit höchstens drei Stunden. In einigen Fällen kann die Neuskalierungs-Latenzzeit in der Ebene „Standard“ oder „Basic“ unabhängig von der Menge des verwendeten Speicherplatzes weniger als fünf Minuten betragen.
- Änderungen der minimalen oder maximalen Anzahl von virtuellen Kernen pro Datenbank dauern in der Regel höchstens fünf Minuten.
- Beim Herabstufen von virtuellen Kernen in Pools muss die verwendete Speichermenge im Pool kleiner sein als die maximal zulässige Größe der Zieldienstebene und der virtuellen Kerne im Pool.

## <a name="dtu-based-purchasing-model-change-elastic-pool-storage-size"></a>DTU-basiertes Erwerbsmodell: Ändern der Speichergröße in einem Pool für elastische Datenbanken

- Der eDTU-Preis für einen Pool für elastische Datenbanken enthält eine bestimmte Menge Speicher ohne zusätzliche Kosten. Zusätzlicher Speicher über die inbegriffene Speichermenge hinaus kann gegen zusätzliche Gebühren bis zur Obergrenze in Inkrementen von 250 GB bis zu 1 TB und dann in Inkrementen von 256 GB über 1 TB hinaus bereitgestellt werden. Informationen zu enthaltenen Speichermengen und Maximalgrößen finden Sie unter [Pool für elastische Datenbanken: Speichergrößen und Computegrößen](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
- Zusätzlicher Speicher für einen Pool für elastische Datenbanken kann durch Erhöhen der maximalen Größe mithilfe von [Azure-Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update) oder [REST-API](https://docs.microsoft.com/rest/api/sql/elasticpools/update) bereitgestellt werden.
- Der Preis für zusätzlichen Speicher für einen Pool für elastische Datenbanken errechnet sich aus der Menge an zusätzlich bereitgestelltem Speicher multipliziert mit dem Einheitenpreis für zusätzlichen Speicher für die Dienstebene. Ausführliche Informationen zu den Preisen für zusätzlichen Speicherplatz siehe [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Unter bestimmten Umständen müssen Sie ggf. eine Datenbank verkleinern, um ungenutzten Speicherplatz freizugeben. Weitere Informationen finden Sie unter [Verwalten von Dateispeicherplatz in Azure SQL-Datenbank](sql-database-file-space-management.md).

## <a name="dtu-based-purchasing-model-change-elastic-pool-compute-resources-edtus"></a>DTU-basiertes Erwerbsmodell: Ändern von Computeressourcen in einem Pool für elastische Datenbanken (eDTUs)

Sie können die verfügbaren Ressourcen für einen Pool für elastische Datenbanken basierend auf den Anforderungen der Ressource mit [Azure-Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update) oder [ REST-API](https://docs.microsoft.com/rest/api/sql/elasticpools/update) herauf- oder herabsetzen.

- Beim Neuskalieren von Pool-eDTUs werden Datenbankverbindungen vorübergehend getrennt. Dieses Verhalten ist auch beim erneuten Skalieren von DTUs für eine einzelne Datenbank zu beobachten. Ausführliche Informationen zu Dauer und Auswirkungen der Trennung von Verbindungen für eine Datenbank bei Vorgängen zur erneuten Skalierung finden Sie unter [Auf DTUs basierendes Kaufmodell: Ändern von Computeressourcen (DTUs)](sql-database-single-database-scale.md#dtu-based-purchasing-model-change-compute-resources-dtus).
- Die Dauer der Neuskalierung von Pool-eDTUs kann von der Gesamtmenge des Speicherplatzes abhängen, die von allen Datenbanken im Pool verwendet wird. Im Allgemeinen beträgt die Neuskalierungs-Latenzzeit durchschnittlich höchstens 90 Minuten pro 100 GB. Wenn beispielsweise der gesamte, von allen Datenbanken im Pool verwendete Speicherplatz 200 GB beträgt, beträgt die erwartete Neuskalierungs-Latenzzeit höchstens drei Stunden. In einigen Fällen kann die Neuskalierungs-Latenzzeit in der Ebene „Standard“ oder „Basic“ unabhängig von der Menge des verwendeten Speicherplatzes weniger als fünf Minuten betragen.
- Änderungen der minimalen oder maximalen Anzahl von eDTUs pro Datenbank werden in der Regel in höchstens fünf Minuten durchgeführt.
- Beim Herabstufen von eDTUs für einen Pool für elastische Datenbanken muss die verwendete Poolspeichermenge kleiner als die maximal zulässige Größe der Zieldienstebenen- und Pool-eDTUs sein.
- Beim erneuten Skalieren von eDTUs für einen Pool für elastische Datenbanken fallen zusätzliche Speicherkosten an, wenn (1) die maximale Speichergröße des Pools vom Zielpool unterstützt wird und (2) die maximale Speichergröße die enthaltene Speichermenge für den Zielpool überschreitet. Wenn z.B. ein 100-eDTU-Standard-Pool mit einer maximalen Größe von 100 GB zu einem 50-eDTU-Standard-Pool herabgestuft wird, fallen zusätzliche Speicherkosten an, da der Zielpool eine maximale Größe von 100 GB unterstützt, und die enthaltene Speichermenge nur 50 GB beträgt. Daher beträgt die zusätzliche Speichermenge 100 GB – 50 GB = 50 GB. Ausführliche Informationen zu den Preisen für zusätzlichen Speicherplatz siehe [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/). Wenn die tatsächlich verwendete Speichermenge kleiner als die enthaltene Speichermenge ist, können Sie diese zusätzlichen Kosten vermeiden, indem Sie die maximale Datenbankgröße auf die enthaltene Menge reduzieren.

## <a name="next-steps"></a>Nächste Schritte

Allgemeine Ressourcenlimits finden Sie unter [Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells in SQL-Datenbank – Pools für elastische Datenbanken](sql-database-vcore-resource-limits-elastic-pools.md) und unter [Ressourcenlimits des auf DTUs basierenden Kaufmodells in SQL-Datenbank – Pools für elastische Datenbanken](sql-database-dtu-resource-limits-elastic-pools.md).
