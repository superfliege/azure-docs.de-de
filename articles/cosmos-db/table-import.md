---
title: "Importieren von Daten für die Verwendung mit der Table-API von Azure Cosmos DB | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Daten für die Verwendung mit der Table-API von Azure Cosmos DB importieren."
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: 
ms.assetid: b60743e2-0227-43ab-965a-0ae3ebacd917
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: 984510b2dae99849507953163f94ad2f925651cd
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="import-data-for-use-with-the-azure-cosmos-db-table-api"></a>Importieren von Daten für die Verwendung mit der Table-API von Azure Cosmos DB

In diesem Tutorial erfahren Sie, wie Sie Daten für die Verwendung mit der [Table-API](table-introduction.md) von Azure Cosmos DB importieren. Wenn Sie Daten in Azure Table Storage gespeichert haben, können Sie entweder das Datenmigrationstool oder AzCopy verwenden, um Ihre Daten in Azure Cosmos DB zu importieren. Nach dem Importieren der Daten stehen Ihnen die Premiumfunktionen von Azure Cosmos DB wie globale, sofort einsatzbereite Verteilung, dedizierter Durchsatz, Wartezeiten im einstelligen Millisekundenbereich im 99. Perzentil, garantierte hohe Verfügbarkeit und automatische sekundäre Indizierung zur Verfügung.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Importieren von Daten mit dem Datenmigrationstool
> * Importieren von Daten mit AzCopy

## <a name="data-migration-tool"></a>Datenmigrationstool

Eine Option für den Import bereits vorhandener Azure Table Storage-Daten ist das Migrationstool von Azure Cosmos DB. Wenn Sie Daten mit dem Tool importieren möchten, wählen Sie Azure Table Storage als Quelle und die Table-API von Azure Cosmos DB als Ziel aus, und geben Sie die übrigen vom Tool angeforderten Informationen an. Daten können gemeinsam oder mithilfe eines sequenziellen Datensatzimports exportiert werden. 

Informationen zum Definieren von Azure Table Storage als Quelle und der Table-API als Ziel finden Sie in den folgenden Abschnitten:
- [Verwenden von Azure Table Storage als Quelle für die Datenmigration](import-data.md#AzureTableSource) 
- [Exportieren in die Table-API (Massenimport)](import-data.md#tableapibulkexport)
- [Exportieren in die Table-API (sequenzieller Datensatzimport)](import-data.md#tableapiseqtarget)

## <a name="azcopy-command"></a>AzCopy-Befehl

Die andere Option zum Migrieren von Daten aus Azure Table Storage in die Table-API von Azure Cosmos DB ist das AzCopy-Befehlszeilenprogramm. Bei Verwendung von AzCopy exportieren Sie Ihre Daten zunächst in eine Manifestdatei (wie in [Exportieren von Daten aus dem Tabellenspeicher](../storage/common/storage-use-azcopy.md#export-data-from-table-storage) beschrieben) und importieren sie anschließend aus der Manifestdatei in die [Table-API von Azure Cosmos DB](../storage/common/storage-use-azcopy.md#import-data-into-table-storage).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Importieren von Daten mit dem Datenmigrationstool
> * Importieren von Daten mit AzCopy

Im nächsten Tutorial erfahren Sie, wie Sie Daten mithilfe der Table-API von Azure Cosmos DB abfragen. 

> [!div class="nextstepaction"]
>[Abfragen von Daten](../cosmos-db/tutorial-query-table.md)



