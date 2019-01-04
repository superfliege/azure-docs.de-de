---
title: Lesereplikate in Azure Database for MySQL
description: In diesem Artikel werden Lesereplikate für Azure Database for MySQL beschrieben.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 11/13/2018
ms.openlocfilehash: 1c05203de694cde882277bf269f3ff87438f9f26
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53547727"
---
# <a name="read-replicas-in-azure-database-for-mysql"></a>Lesereplikate in Azure Database for MySQL

Mit dem Feature der Lesereplikate (öffentliche Vorschau) können Sie Daten von einem Azure Database for MySQL-Server (Masterserver) auf bis zu fünf schreibgeschützte Server (Replikate) in derselben Azure-Region replizieren. Schreibgeschützte Replikate werden mit der nativen, auf der Position der Binärprotokolldatei (binlog) basierenden Replikationstechnologie der MySQL-Engine asynchron aktualisiert. Weitere Informationen zur binlog-Replikation finden Sie unter [Binary Log File Position Based Replication Configuration Overview](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html) (Konfiguration der auf der Position der binären Protokolldatei basierenden Replikation – Übersicht).

Bei Replikaten, die im Azure Database for MySQL-Dienst erstellt werden, handelt es sich um neue Server, die genauso verwaltet werden können wie normale, eigenständige MySQL-Server. Diese Server werden mit dem gleichen Gebührensatz berechnet wie eigenständige Server.

Weitere Informationen zu Features und Problemen der MySQL-Replikation finden Sie in der [Dokumentation zur MySQL-Replikation](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).

## <a name="when-to-use-read-replicas"></a>Einsatz von Lesereplikaten

Anwendungen und Workloads mit hohen Leseanforderungen können von schreibgeschützten Replikaten genutzt werden. Lesereplikate erhöhen die Menge der verfügbaren Lesekapazitäten im Vergleich mit der Verwendung eines einzigen Servers sowohl für Lese- als auch für Schreibvorgänge. Die Leseworkloads können in den Replikaten isoliert und Schreibworkloads an den Masterserver weitergeleitet werden.

In einem häufig anzutreffenden Szenario verwenden BI- und Analyseworkloads das Lesereplikat als Datenquelle für die Berichterstellung.

## <a name="considerations-and-limitations"></a>Überlegungen und Einschränkungen

### <a name="pricing-tiers"></a>Tarife

Lesereplikate sind zurzeit nur in den Tarifen „Universell“ und „Arbeitsspeicheroptimiert“ verfügbar.

### <a name="master-server-restart"></a>Masterserverneustart

Wenn Sie in dieser Vorschauversion ein Replikat für einen Master erstellen, der keine vorhandenen Replikate hat, startet der Master zunächst neu, um sich auf die Replikation vorzubereiten. Bitte beachten Sie dies und führen Sie diese Operationen nicht zu Spitzenzeiten durch.

### <a name="stopping-replication"></a>Beenden der Replikation

Sie können die Replikation zwischen einem Master- und einem Replikatserver beenden. Beim Beenden der Replikation wird die Replikationsbeziehung zwischen dem Master- und dem Replikatserver entfernt.

Sobald die Replikation beendet wurde, wird der Replikatserver zum eigenständigen Server. Bei den Daten auf diesem eigenständigen Server handelt es sich um die Daten, die zu dem Zeitpunkt im Replikat vorhanden waren, als der Befehl zum Beenden der Replikation initiiert wurde. Der eigenständige Server wird nicht zusammen mit dem Masterserver aktualisiert. Der Server kann nicht wieder in ein Replikat umgewandelt werden.

### <a name="replicas-are-new-servers"></a>Replikate sind neue Server

Replikate werden als neue Azure Database for MySQL-Server erstellt. Vorhandene Server können nicht in Replikate umgewandelt werden.

### <a name="replica-server-configuration"></a>Replikatserverkonfiguration

Replikatserver werden unter Verwendung der gleichen Serverkonfigurationen wie beim Masterserver erstellt. Dies umfasst folgende Konfigurationen:

- Tarif
- Computegeneration
- V-Kerne
- Storage
- Aufbewahrungszeitraum der Sicherung
- Option für Sicherungsredundanz
- MySQL-Engine-Version
- Firewallregeln

Nachdem ein Replikat erstellt wurde, können Sie den Tarif (Ausnahme: Wechsel zu oder aus Basic), die Computegeneration, die virtuellen Kernen, den Speicher und die Sicherungsaufbewahrung unabhängig vom Masterserver ändern.

### <a name="master-server-configuration"></a>Masterserverkonfiguration

Wenn die Konfiguration eines Masterservers (z.B. virtuelle Kerne oder Speicher) aktualisiert wird, sollten die Konfigurationen der Replikate ebenfalls auf die gleichen oder höhere Werte aktualisiert werden. Andernfalls kann der Replikatserver bei Änderungen, die am Master vorgenommen werden, nicht mehr folgen und stürzt möglicherweise ab.

Neue Firewallregeln, die dem Masterserver nach der Erstellung eines Replikatservers hinzugefügt werden, werden nicht auf dem Replikat repliziert. Das Replikat muss ebenfalls mit dieser neuen Firewallregel aktualisiert werden.

### <a name="deleting-the-master-server"></a>Löschen des Masterservers

Wenn ein Masterserver gelöscht wird, wird die Replikation an alle Lesereplikate beendet. Diese Replikate werden zu eigenständigen Servern. Der Masterserver selbst wird gelöscht.

### <a name="user-accounts"></a>Benutzerkonten

Benutzer auf dem Masterserver werden an die Lesereplikate repliziert. Sie können nur mit denjenigen Benutzerkonten eine Verbindung mit einem Lesereplikat herstellen, die auf dem Masterserver verfügbar sind.

### <a name="other"></a>Andere

- IDs globaler Transaktionen (GTIDs) werden nicht unterstützt.
- Die Erstellung des Replikats eines Replikats wird nicht unterstützt.
- In-Memory-Tabellen können dazu führen, dass Replikate nicht mehr synchron sind. Dies ist eine Einschränkung der MySQL-Replikationstechnologie. Weitere Informationen finden Sie in der [MySQL-Referenzdokumentation](https://dev.mysql.com/doc/refman/5.7/en/replication-features-memory.html).
- Das Optimieren des Parameters [`innodb_file_per_table`](https://dev.mysql.com/doc/refman/5.7/en/innodb-multiple-tablespaces.html) auf einem Masterserver, nachdem ein Replikatserver erstellt wurde, kann dazu führen, dass das Replikat nicht mehr synchron ist. Der Replikatserver berücksichtigt die verschiedenen Tabellenbereiche nicht.
- Eine vollständige Liste aller Einschränkungen der MySQL-Replikation finden Sie in der [MySQL-Dokumentation](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Lesereplikate über das Azure-Portal erstellen und verwalten](howto-read-replicas-portal.md).
- Erfahren Sie, wie Sie [Lesereplikate über die Azure CLI erstellen und verwalten](howto-read-replicas-cli.md).
