---
title: "Übersicht über Azure Database Migration Service – Vorschau | Microsoft-Dokumentation"
description: "Übersicht über Azure Database Migration Service, ein Dienst, der die nahtlose Migration von vielen Datenbankquellen zu Azure-Datenplattformen ermöglicht."
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.topic: article
ms.date: 12/13/2017
ms.openlocfilehash: 2aae105b7454209131db79c60d74740ce97c21ce
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="what-is-the-azure-database-migration-service-preview"></a>Was ist die Vorschauversion von Azure Database Migration Service?
Azure Database Migration Service ist ein vollständig verwalteter Dienst, der die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen mit minimaler Downtime ermöglicht. Der Dienst ist derzeit in Public Preview, und bei der Entwicklung wurden folgende Schwerpunkte berücksichtigt:

- Zuverlässigkeit und Leistung
- Iteratives Hinzufügen von Quelle-Ziel-Paaren
- Kontinuierliche Investition in reibungslose Migrationen

## <a name="use-familiar-tools"></a>Einsatz bekannter Tools
Der Azure Database Migration Service integriert einige Funktionen unserer vorhandenen Tools und Dienste.  Der Dienst bietet Kunden eine umfassende, hoch verfügbare Lösung. Der Dienst verwendet den [Data Migration Assistant](http://aka.ms/dma), um Bewertungsberichte zu generieren, die Empfehlungen bezüglich der Änderungen enthalten, die vor einer Migration erforderlich sind. Sie müssen ggf. erforderliche Wartungsarbeiten durchführen. Wenn Sie bereit sind, den Migrationsprozess zu starten, führt Azure Database Migration Service alle entsprechenden Schritte aus. Sie können den Prozess starten und müssen sich nicht weiter um Ihre Migrationsprojekte kümmern, da Sie sich darauf verlassen können, dass die Migration mit den von Microsoft bestimmten bewährten Methoden erfolgt.

## <a name="regional-availability-during-public-preview"></a>Regionale Verfügbarkeit in der Public Preview-Phase
Das Public Preview-Release von Azure Database Migration Service ist zurzeit in den folgenden Regionen verfügbar:
- USA (Ost)
- USA Süd Mitte
- USA (Westen)
- Brasilien Süd
- Europa, Westen
- Nordeuropa
- Asien, Südosten
- Indien, Westen

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen einer Azure Database Migration Service-Instanz über das Azure-Portal](quickstart-create-data-migration-service-portal.md)
- [Migrieren von SQL Server zu Azure SQL-Datenbank](tutorial-sql-server-to-azure-sql.md)
- [Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service](pre-reqs.md)
- [Häufig gestellte Fragen zu Azure Database Migration Service](faq.md)
