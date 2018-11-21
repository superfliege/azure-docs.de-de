---
title: Hochverfügbarkeitskonzepte von Azure Database for PostgreSQL
description: Dieser Artikel enthält Informationen zur Hochverfügbarkeit bei der Verwendung von Azure Database for PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 6c39cfdd3bfe244296290c4083644ea7e1b35672
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51298992"
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql"></a>Hochverfügbarkeitskonzepte von Azure Database for PostgreSQL
Der Dienst „Azure Database for PostgreSQL“ bietet eine garantiert hohes Verfügbarkeitsniveau. Die finanziell abgesicherte Vereinbarung zum Servicelevel (SLA) beträgt 99,99 % bei allgemeiner Verfügbarkeit. Es gibt praktisch keine Ausfallzeiten bei der Nutzung dieses Diensts.

## <a name="high-availability"></a>Hochverfügbarkeit
Das Hochverfügbarkeitsmodell (HA) basiert auf integrierten Failovermechanismen, die bei einer Unterbrechung auf Knotenebene greifen. Eine Unterbrechung auf Knotenebene kann aufgrund eines Hardwarefehlers oder als Reaktion auf eine Dienstbereitstellung auftreten.

Änderungen, die an einem Datenbankserver von Azure Database for PostgreSQL vorgenommen werden, treten immer im Kontext einer Transaktion auf. Änderungen werden in Azure Storage synchron aufgezeichnet, wenn die Transaktion commitet wird. Tritt eine Unterbrechung auf Knotenebene auf, erstellt der Datenbankserver automatisch einen neuen Knoten und fügt Datenspeicher an den neuen Knoten an. Alle aktiven Verbindungen werden beendet, und für alle In-flight-Transaktionen wird kein Commit ausgeführt.

## <a name="application-retry-logic-is-essential"></a>Wiederholungslogik für die Anwendung ist unerlässlich
Es ist wichtig, dass PostgreSQL-Datenbankanwendungen so erstellt werden, dass beendete Verbindungen und Transaktionsfehler erkannt und für diese Wiederholungsversuche ausgeführt werden. Wenn die Anwendung Wiederholungsversuche ausführt, wird die Verbindung der Anwendung transparent an die neu erstellte Instanz umgeleitet, die die Aufgaben der fehlerhaften Instanz übernimmt.

Intern wird in Azure ein Gateway verwendet, um die Verbindungen an die neue Instanz umzuleiten. Bei einer Unterbrechung dauert der gesamte Failovervorgang normalerweise wenige Sekunden. Da die Umleitung intern vom Gateway behandelt wird, bleibt die externe Verbindungszeichenfolge für die Clientanwendung unverändert.

## <a name="scaling-up-or-down"></a>Zentrales Hoch- oder Herunterskalieren
Wenn Azure Database for PostgreSQL zentral hoch- oder herunterskaliert wird, wird ähnlich wie beim HA-Modell eine neue Serverinstanz mit der angegebenen Größe erstellt. Der vorhandene Datenspeicher wird von der ursprünglichen Instanz getrennt und an die neue Instanz angefügt.

Während des Skalierungsvorgangs tritt eine Unterbrechung der Datenbankverbindungen auf. Die Verbindung der Clientanwendungen wird getrennt, und offene Transaktionen ohne Commit werden abgebrochen. Sobald die Clientanwendung die Verbindung wiederherstellt oder eine neue Verbindung herstellt, leitet das Gateway die Verbindung an die Instanz mit der neuen Größe weiter. 

## <a name="next-steps"></a>Nächste Schritte
- Eine Übersicht über den Dienst finden Sie unter  [Azure Database for PostgreSQL – Übersicht](overview.md).
- Eine Übersicht über Wiederholungslogik finden Sie unter [Behandeln vorübergehender Konnektivitätsfehler für Azure Database for PostgreSQL](concepts-connectivity.md).
