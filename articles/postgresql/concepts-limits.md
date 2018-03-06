---
title: "Einschränkungen in Azure-Datenbank für PostgreSQL"
description: "Dieser Artikel beschreibt die Einschränkungen in Azure Database for PostgreSQL, z.B. die Anzahl der Verbindungs- und Speichermoduloptionen."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: ba05308039e9743dd207333476e61a45c0ca166a
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Einschränkungen in Azure-Datenbank für PostgreSQL
Der Azure-Datenbank für PostgreSQL-Dienst ist in der öffentlichen Vorschau verfügbar. In den folgenden Abschnitten werden die Kapazitäts- und funktionalen Beschränkungen im Datenbankdienst beschrieben.

## <a name="pricing-tier-maximums"></a>Maximalwerte der Tarife
Azure Database for PostgreSQL stellt mehrere Tarife bereit, unter denen Sie bei der Erstellung eines Servers wählen können. Weitere Informationen finden Sie unter [Tarife für Azure Database for PostgreSQL](concepts-pricing-tiers.md).  

In jedem Tarif ist eine maximale Anzahl von Verbindungen, Compute-Einheiten und Speicherkapazitäten wie folgt verfügbar: 

|Preisstufe| Computegeneration| Virtuelle Kerne| Max. Anzahl von Verbindungen |
|---|---|---|---|
|Basic| Gen 4| 1| 50 |
|Basic| Gen 4| 2| 100 |
|Basic| Gen 5| 1| 50 |
|Basic| Gen 5| 2| 100 |
|Universell| Gen 4| 2| 150|
|Universell| Gen 4| 4| 250|
|Universell| Gen 4| 8| 480|
|Universell| Gen 4| 16| 950|
|Universell| Gen 4| 32| 1500|
|Universell| Gen 5| 2| 150|
|Universell| Gen 5| 4| 250|
|Universell| Gen 5| 8| 480|
|Universell| Gen 5| 16| 950|
|Universell| Gen 5| 32| 1500|
|Arbeitsspeicheroptimiert| Gen 5| 2| 150|
|Arbeitsspeicheroptimiert| Gen 5| 4| 250|
|Arbeitsspeicheroptimiert| Gen 5| 8| 480|
|Arbeitsspeicheroptimiert| Gen 5| 16| 950|
|Arbeitsspeicheroptimiert| Gen 5| 32| 1.900|

Wenn Verbindungen den Grenzwert übersteigen, erhalten Sie möglicherweise den folgenden Fehler:
> SCHWERWIEGEND: Es sind bereits zu viele Clients vorhanden.

Das Azure-System benötigt fünf Verbindungen, um den Azure Database for PostgreSQL-Server zu überwachen. 

## <a name="functional-limitations"></a>Funktionale Beschränkungen
### <a name="scale-operations"></a>Skalierungsvorgänge
1.  Die dynamische Skalierung von Servern über verschiedene Tarife hinweg wird zurzeit nicht unterstützt. Wechsel zwischen den Tarifen „Basic“, „Universell“ und „Arbeitsspeicheroptimiert“ werden also nicht unterstützt.
2.  Die Verringerung der Größe des Serverspeichers wird zurzeit nicht unterstützt.

### <a name="server-version-upgrades"></a>Upgrades von Serverversionen
- Die automatisierte Migration zwischen Hauptversionen von Datenbank-Engines wird derzeit nicht unterstützt.

### <a name="subscription-management"></a>Abonnementverwaltung
- Die dynamische Verschiebung von Servern zwischen Abonnement- und Ressourcengruppen wird zurzeit nicht unterstützt.

### <a name="point-in-time-restore-pitr"></a>Point-in-Time-Wiederherstellung (PITR)
1.  Wenn Sie das PITR-Feature verwenden, wird der neue Server mit den gleichen Konfigurationen erstellt wie der Server, auf dem er basiert.
2.  Die Wiederherstellung eines gelöschten Servers wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
- Informationen zu den [verfügbaren Funktionen in jedem Tarif](concepts-pricing-tiers.md)
- Informationen zu [unterstützten PostgreSQL-Datenbankversionen](concepts-supported-versions.md)
- Informationen zum [Sichern und Wiederherstellen eines Servers in Azure Database for PostgreSQL mithilfe des Azure-Portals](howto-restore-server-portal.md)
