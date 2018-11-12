---
title: Übersicht über Azure Database Migration Service | Microsoft-Dokumentation
description: Übersicht über Azure Database Migration Service, ein Dienst, der die nahtlose Migration von vielen Datenbankquellen zu Azure-Datenplattformen ermöglicht.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: ''
ms.reviewer: douglasl
ms.service: database-migration
ms.workload: data-services
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: 053e571b6285cd405ea17f43fec1d3ea99732070
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235580"
---
# <a name="what-is-the-azure-database-migration-service"></a>Was ist Azure Database Migration Service?
Azure Database Migration Service ist ein vollständig verwalteter Dienst, der die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen mit minimaler Ausfallzeit ermöglicht (Onlinemigrationen).

## <a name="migrate-databases-to-azure-with-familiar-tools"></a>Migrieren von Datenbanken zu Azure mit vertrauten Tools
Der Azure Database Migration Service integriert einige Funktionen unserer vorhandenen Tools und Dienste. Der Dienst bietet Kunden eine umfassende, hoch verfügbare Lösung. Der Dienst verwendet den [Data Migration Assistant](https://aka.ms/dma), um Bewertungsberichte zu generieren, die Empfehlungen bezüglich der Änderungen enthalten, die vor einer Migration erforderlich sind. Sie müssen ggf. erforderliche Wartungsarbeiten durchführen. Wenn Sie bereit sind, den Migrationsprozess zu starten, führt Azure Database Migration Service alle erforderlichen Schritte aus. Sie können den Prozess starten und müssen sich nicht weiter um Ihre Migrationsprojekte kümmern, da Sie sich darauf verlassen können, dass die Migration mit den von Microsoft bestimmten bewährten Methoden erfolgt.

> [!NOTE]
> Die Verwendung von Azure Database Migration Service zum Ausführen einer Onlinemigration erfordert das Erstellen einer Instanz auf der Grundlage des Tarifs Unternehmenskritisch (Vorschau).

## <a name="regional-availability"></a>Regionale Verfügbarkeit
Azure Database Migration Service ist zurzeit in den folgenden Regionen verfügbar:

![Regionale Verfügbarkeit von Azure Database Migration Service](media\overview\dms-regional-availability1.png)

Neueste Informationen zur regionalen Verfügbarkeit von Azure Database Migration Service finden Sie auf der globalen Azure-Infrastrukturwebsite unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/).

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen einer Azure Database Migration Service-Instanz über das Azure-Portal](quickstart-create-data-migration-service-portal.md)
- [Migrieren von SQL Server zu Azure SQL-Datenbank](tutorial-sql-server-to-azure-sql.md)
- [Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service](pre-reqs.md)
- [Häufig gestellte Fragen zu Azure Database Migration Service](faq.md)
