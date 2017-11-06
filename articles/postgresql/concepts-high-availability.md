---
title: "Hochverfügbarkeitskonzepte von Azure Database for PostgreSQL | Microsoft Docs"
description: "Dieses Thema biete Informationen zur Hochverfügbarkeit bei der Verwendung von Azure Database for PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/19/2017
ms.openlocfilehash: 315eb32a293eec89b274ccd017dba506241533e6
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2017
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql"></a>Hochverfügbarkeitskonzepte von Azure Database for PostgreSQL
Der Dienst „Azure Database for PostgreSQL“ bietet eine garantiert hohes Verfügbarkeitsniveau. Die finanziell abgesicherte Vereinbarung zum Servicelevel (SLA) beträgt 99,99 % bei allgemeiner Verfügbarkeit. Es gibt praktisch keine Ausfallzeiten bei der Nutzung dieses Diensts.

## <a name="high-availability"></a>Hohe Verfügbarkeit
Das Hochverfügbarkeitsmodell (HA) basiert auf integrierten Failovermechanismen, die bei einer Unterbrechung auf Knotenebene greifen. Eine Unterbrechung auf Knotenebene kann aufgrund eines Hardwarefehlers oder als Reaktion auf eine Dienstbereitstellung auftreten.

Änderungen an PostgreSQL erfolgen immer im Kontext einer Transaktion. Änderungen werden in Azure Storage synchron aufgezeichnet, wenn die Transaktion commitet wird. Tritt eine Unterbrechung auf Knotenebene auf, erstellt der Datenbankserver automatisch einen neuen Knoten und fügt Datenspeicher an den neuen Knoten an. Alle aktiven Verbindungen werden beendet, und für alle In-flight-Transaktionen wird kein Commit ausgeführt.

## <a name="application-retry-logic-is-essential"></a>Wiederholungslogik für die Anwendung ist unerlässlich
Es ist wichtig, dass PostgreSQL-Datenbankanwendungen so erstellt werden, dass beendete Verbindungen und Transaktionsfehler erkannt und für diese Wiederholungsversuche ausgeführt werden. Wenn die Anwendung Wiederholungsversuche ausführt, wird die Verbindung der Anwendung transparent an die neu erstellte Instanz umgeleitet, die die Aufgaben der fehlerhaften Instanz übernimmt.

Intern wird in Azure ein Gateway verwendet, um die Verbindungen an die neue Instanz umzuleiten. Bei einer Unterbrechung dauert der gesamte Failovervorgang normalerweise wenige Sekunden. Die externe Verbindungszeichenfolge bleibt für die Clientanwendungen gleich.

## <a name="scaling-up-or-down"></a>Zentrales Hoch- oder Herunterskalieren
Wenn Azure Database for PostgreSQL zentral hoch- oder herunterskaliert wird, wird ähnlich wie beim HA-Modell eine neue Serverinstanz mit der angegebenen Größe erstellt. Der vorhandene Datenspeicher wird von der ursprünglichen Instanz getrennt und an die neue Instanz angefügt.

Während des Skalierungsvorgangs tritt eine Unterbrechung der Datenbankverbindungen auf. Die Verbindung der Clientanwendungen wird getrennt, und offene Transaktionen ohne Commit werden abgebrochen. Sobald die Clientanwendung die Verbindung wiederherstellt oder eine neue Verbindung herstellt, leitet das Gateway die Verbindung an die Instanz mit der neuen Größe weiter. 

## <a name="next-steps"></a>Nächste Schritte
- Einen Überblick über den Dienst finden Sie unter [Azure-Datenbank für PostgreSQL – Überblick](overview.md).
