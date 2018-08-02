---
title: Einschränkungen in Azure-Datenbank für PostgreSQL
description: Dieser Artikel beschreibt die Einschränkungen in Azure Database for PostgreSQL, z.B. die Anzahl der Verbindungs- und Speichermoduloptionen.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/30/2018
ms.openlocfilehash: 52dfac826de86f67b3143cce49c35088547c4b39
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171771"
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Einschränkungen in Azure-Datenbank für PostgreSQL
In den folgenden Abschnitten werden die Kapazitäts- und funktionalen Beschränkungen im Datenbankdienst beschrieben.

## <a name="maximum-connections"></a>Maximale Anzahl der Verbindungen
Die folgende Tabelle enthält die maximale Anzahl von Verbindungen nach Tarif und V-Kernen: 

|**Tarif**| **vCore(s)**| **Max. Anzahl von Verbindungen** |
|---|---|---|
|Basic| 1| 50 |
|Basic| 2| 100 |
|Allgemeiner Zweck| 2| 150|
|Allgemeiner Zweck| 4| 250|
|Allgemeiner Zweck| 8| 480|
|Allgemeiner Zweck| 16| 950|
|Allgemeiner Zweck| 32| 1500|
|Arbeitsspeicheroptimiert| 2| 150|
|Arbeitsspeicheroptimiert| 4| 250|
|Arbeitsspeicheroptimiert| 8| 480|
|Arbeitsspeicheroptimiert| 16| 950|

Wenn Verbindungen den Grenzwert übersteigen, erhalten Sie möglicherweise den folgenden Fehler:
> SCHWERWIEGEND: Es sind bereits zu viele Clients vorhanden.

Das Azure-System benötigt fünf Verbindungen, um den Azure Database for PostgreSQL-Server zu überwachen. 

## <a name="functional-limitations"></a>Funktionale Beschränkungen
### <a name="scale-operations"></a>Skalierungsvorgänge
- Die dynamische Skalierung auf den oder vom Basic-Tarif aus wird zurzeit nicht unterstützt.
- Die Verringerung der Größe des Serverspeichers wird zurzeit nicht unterstützt.

### <a name="server-version-upgrades"></a>Upgrades von Serverversionen
- Die automatisierte Migration zwischen Hauptversionen von Datenbank-Engines wird derzeit nicht unterstützt.

### <a name="subscription-management"></a>Abonnementverwaltung
- Die dynamische Verschiebung von Servern zwischen Abonnement- und Ressourcengruppen wird zurzeit nicht unterstützt.

### <a name="vnet-service-endpoints"></a>VNET-Dienstendpunkte
- VNET-Dienstendpunkte werden nur für Server vom Typ „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.

### <a name="restoring-a-server"></a>Wiederherstellen eines Servers
- Wenn Sie das PITR-Feature verwenden, wird der neue Server mit den gleichen Tarifkonfigurationen erstellt wie der Server, auf dem er basiert.
- Der neue Server, der während einer Wiederherstellung erstellt wird, weist nicht die Firewallregeln auf, die auf dem ursprünglichen Server vorhanden waren. Firewallregeln müssen separat für diesen neuen Server eingerichtet werden.
- Die Wiederherstellung eines gelöschten Servers wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
- Informationen zu den [verfügbaren Funktionen in jedem Tarif](concepts-pricing-tiers.md)
- Informationen zu [unterstützten PostgreSQL-Datenbankversionen](concepts-supported-versions.md)
- Informationen zum [Sichern und Wiederherstellen eines Servers in Azure Database for PostgreSQL mithilfe des Azure-Portals](howto-restore-server-portal.md)
