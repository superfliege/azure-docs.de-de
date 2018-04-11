---
title: Upgrade auf Azure SQL Data Warehouse der neuesten Generation | Microsoft-Dokumentation
description: Schritte zum Durchführen eines Upgrades für Azure SQL Data Warehouse auf die Azure-Hardware- und -Speicherarchitektur der neuesten Generation.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.services: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/02/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 42b716274e655bf91f72c1b3ab207b8a5f1ccee0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="upgrade-to-latest-generation-of-azure-sql-data-warehouse-in-the-azure-portal"></a>Upgrade auf Azure SQL Data Warehouse der neuesten Generation im Azure-Portal

Verwenden Sie das Azure-Portal, um ein Upgrade für Ihre Azure SQL Data Warehouse-Instanz auf die Azure-Hardware- und -Speicherarchitektur der neuesten Generation durchzuführen. Durch ein Upgrade können Sie von höherer Leistung, größerer Skalierbarkeit und einem unbegrenzten Speicher für Columnstore-Indizes profitieren.  

## <a name="applies-to"></a>Anwendungsbereich
Dieses Upgrade gilt für Data Warehouses in der Leistungsstufe „Optimiert für Elastizität“.  In den Anweisungen werden die Schritte zum Durchführen eine Upgrades für ein Data Warehouse von der Leistungsstufe „Optimiert für Elastizität“ auf die Leistungsstufe „Optimiert für Compute“ erläutert. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="before-you-begin"></a>Voraussetzungen

1. Wenn das Data Warehouse „Optimiert für Elastizität“, für das ein Upgrade durchgeführt werden soll, angehalten wird, [setzen Sie das Data Warehouse fort](pause-and-resume-compute-portal.md).
2. Es tritt eine Ausfallzeit von einigen Minuten auf. 
3. Im Rahmen des Upgradeprozesses werden alle Sitzungen beendet und alle Verbindungen getrennt. Stellen Sie vor dem Upgrade sicher, dass Ihre Abfragen abgeschlossen wurden. Wenn Sie ein Upgrade mit laufenden Transaktionen starten, kann das Rollback sehr zeitintensiv sein. 

## <a name="start-the-upgrade"></a>Starten des Upgrades

1. Öffnen Sie Ihr Data Warehouse im Azure-Portal, und klicken Sie auf **Upgrade auf „Optimiert für Compute“ durchführen**.
2. Beachten Sie die Optionen für die Leistungsstufe „Optimiert für Compute“. Die Standardauswahl ist mit der aktuellen Stufe vor dem Upgrade vergleichbar.
3. Wählen Sie eine Leistungsstufe aus. Die Leistungsstufe „Optimiert für Compute“ ist gegenwärtig in der Vorschauphase zum halben Preis erhältlich.
4. Klicken Sie auf **Upgrade durchführen**.
5. Überprüfen Sie den Status im Azure-Portal.
6. Warten Sie, bis das Data Warehouse zu „Online“ wechselt.

## <a name="rebuild-columnstore-indexes"></a>Neuerstellen von Columnstore-Indizes

Sobald Ihr Data Warehouse online ist, können Sie Daten laden und Abfragen ausführen. Die Leistung kann jedoch zunächst langsam sein, da die Daten in einem Hintergrundprozess auf die neue Hardware migriert werden. 

Es wird empfohlen, Columnstore-Indizes zu erstellen, um eine möglichst schnelle Datenmigration zu erzielen. Lesen Sie hierfür [Neuerstellen von Columnstore-Indizes zur Verbesserung der Segmentqualität](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality). 

## <a name="next-steps"></a>Nächste Schritte
Ihr Data Warehouse ist online. Informationen zur Verwendung der neuen Leistungsfeatures finden Sie unter [Ressourcenklassen für die Workloadverwaltung](resource-classes-for-workload-management.md).
 