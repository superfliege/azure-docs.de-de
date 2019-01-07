---
title: Azure SQL Data Warehouse Gen2 unterstützt niedrigere Computetarife | Microsoft-Dokumentation
description: Azure SQL Data Warehouse Gen2 unterstützt jetzt niedrigere Computetarife | Microsoft-Dokumentation
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 12/03/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: d3c42c13e58156013167b8570a8845ce9de08b3b
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52884886"
---
# <a name="azure-sql-data-warehouse-gen2-support-for-lower-compute-tiers"></a>Azure SQL Data Warehouse Gen2-Unterstützung für niedrigere Computetarife

Microsoft fügt niedrigere Computetarife für das extrem schnelle Azure SQL Data Warehouse Gen2 hinzu, sodass Sie die Einstiegskosten für ein Data Warehouse zur Verarbeitung anspruchsvoller Abfragen verringern können. Kunden profitieren von den führenden Azure SQL Data Warehouse-Features für Leistung, Flexibilität und Sicherheit, die bei 100 cDWU (Compute Data Warehouse-Einheiten) beginnen und in Minutenschnelle auf 30.000 cDWU skaliert werden können. Kunden können die Vorteile der Gen2-Leistung und -Flexibilität bei niedrigeren Computetarifen nutzen. 

Durch die Beseitigung des Einstiegspunkts für das Data Warehousing der nächsten Generation öffnet Microsoft die Türen für wertorientierte Kunden, die alle Vorteile eines sicheren, leistungsstarken Data Warehouse nutzen möchten, ohne erraten zu müssen, welche Testumgebung für sie am besten geeignet ist.  Anstelle des aktuellen Einstiegspunkts von 500 cDWU können Kunden mit nur 100 cDWU beginnen.  SQL Data Warehouse Gen2 bietet weiterhin Unterstützung für Vorgänge zum Anhalten und Fortsetzen und geht im Hinblick auf Computevorgänge weit über einfache Flexibilität hinaus.  Gen2 unterstützt zudem eine unbegrenzte Columnstore-Speicherkapazität und 2,5-mal mehr Arbeitsspeicher pro Abfrage, bis zu 128 gleichzeitige Abfragen sowie adaptive Cachefeatures. So erleben Kunden im Durchschnitt die fünffache Leistung im Vergleich zu derselben Data Warehouse-Einheit in Gen 1 zum selben Preis.  Georedundante Sicherungen mit integrierter Datenschutzgarantie gehören in Gen2 zum Standard. Azure SQL Data Warehouse Gen2 kann bei Bedarf sofort skaliert werden.

Die Bereitstellung von oder die Skalierung auf die Gen 2-Instanzen des niedrigeren Tarifs werden im Portal zurzeit nicht unterstützt. Wir arbeiten daran, diese Funktion zu aktivieren. Senden Sie in der Zwischenzeit ein [Ticket](sql-data-warehouse-get-started-create-support-ticket.md), wenn Sie diesen neuen Tarif nutzen möchten.

## <a name="getting-started-with-azure-sql-data-warehouse-gen2"></a>Erste Schritte mit Azure SQL Data Warehouse Gen2 

Kunden können wählen, ob sie eine neue Gen2-Instanz bereitstellen oder eine vorhandene Gen1-Data Warehouse-Instanz aktualisieren möchten, um von der Flexibilität und Leistung des Data Warehousing der nächsten Generation zu profitieren. 

Probieren Sie den [für Compute optimierten Azure SQL Data Warehouse Gen2-Tarif](https://azure.microsoft.com/services/sql-data-warehouse/?v=17.44) aus.
Führen Sie ein Upgrade des [für Compute optimierten Azure SQL Warehouse Gen1-Tarifs auf Gen2](https://docs.microsoft.com/azure/sql-data-warehouse/upgrade-to-latest-generation) durch. Sehen Sie sich Azure SQL Data Warehouse Gen2 in diesem [Video zu Microsoft Mechanics](https://www.youtube.com/watch?v=Ap8I3UZonzI&feature=youtu.be) in Aktion an.


## <a name="supported-regions-for-lower-compute-tiers"></a>Unterstützte Regionen für niedrigere Computetarife

- USA, Osten 1 
- USA (Ost 2)
- Europa, Westen
- Nordeuropa
- USA, Westen 2
- Asien, Südosten
- USA, Süden-Mitte
- USA (Mitte) 
- Asien, Osten
- Japan, Osten
- Indien, Mitte
- Australien (Osten)
- Kanada, Mitte
- Japan, Westen 
- Kanada, Mitte

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen](upgrade-to-latest-generation.md) zum Optimieren der Leistung durch ein Upgrade von SQL Data Warehouse 
