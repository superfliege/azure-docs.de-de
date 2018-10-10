---
title: Serverkonzepte in Azure Database for MariaDB
description: Dieses Thema enthält Aspekte und Richtlinien für die Arbeit mit Azure Database for MariaDB-Servern.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: cf57acdcbcfa792a6c5ab62c6e8ec0589d625df7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994586"
---
# <a name="server-concepts-in-azure-database-for-mariadb"></a>Serverkonzepte in Azure Database for MariaDB
Dieser Artikel enthält Aspekte und Richtlinien für die Arbeit mit Azure Database for MariaDB-Servern.

## <a name="what-is-an-azure-database-for-mariadb-server"></a>Was ist ein Azure Database for MariaDB-Server?

Ein Azure Database for MariaDB-Server fungiert als zentraler Verwaltungspunkt für mehrere Datenbanken. Er weist dasselbe MariaDB-Serverkonstrukt auf, das Sie möglicherweise aus lokalen Umgebungen kennen. Im Besonderen ist der Azure Database for MariaDB-Dienst verwaltet, bietet Leistungsgarantien und stellt Zugriff sowie Funktionen auf Serverebene bereit.

Für einen Azure Database for MariaDB-Server gilt Folgendes:

- Er wird im Rahmen eines Azure-Abonnements erstellt.
- Er stellt die übergeordnete Ressource für Datenbanken dar.
- Er stellt einen Namespace für Datenbanken bereit.
- Er ist ein Container mit einer Semantik von hoher Lebensdauer. Beim Löschen eines Servers werden die enthaltenen Datenbanken gelöscht.
- Er stellt Ressourcen in einer Region zusammen.
- Er stellt einen Verbindungsendpunkt für den Server- und Datenbankzugriff bereit.
- Er stellt den Bereich für Verwaltungsrichtlinien bereit, die auf die jeweiligen Datenbanken angewendet werden (Anmeldungen, Firewalls, Benutzer, Rollen, Konfigurationen etc.).
- Er steht in MariaDB-Engine-Version 10.2 zur Verfügung. Weitere Informationen finden Sie unter [Unterstützte Azure Database for MariaDB-Datenbankversionen](./concepts-supported-versions.md).

Sie können in einer Instanz des Azure Database for MariaDB-Servers eine oder mehrere Datenbanken erstellen. Sie können eine Einzeldatenbank pro Server erstellen, die alle Ressourcen nutzt, oder Sie können mehrere Datenbanken erstellen, um die Ressourcen gemeinsam zu verwenden. Die Preise gelten pro Server und basieren auf der Konfiguration des Tarifs, der virtuellen Kerne und des Speichers (GB). Weitere Informationen finden Sie unter [Tarife](./concepts-pricing-tiers.md).

## <a name="how-do-i-secure-an-azure-database-for-mariadb-server"></a>Wie schütze ich einen Azure Database for MariaDB-Server?

Mithilfe der folgenden Elemente kann ein sicherer Zugriff auf Ihre Datenbank sichergestellt werden.
|||
| :--| :--|
| **Authentifizierung und Autorisierung** | Azure Database for MariaDB-Server unterstützen die systemeigene MySQL-Authentifizierung. Mithilfe der Anmeldeinformationen des Serveradministrators können Sie eine Verbindung mit dem Server herstellen und sich authentifizieren. |
| **Protokoll** | Der Dienst unterstützt ein nachrichtenbasiertes Protokoll, das von MySQL verwendet wird. |
| **TCP/IP** | Das Protokoll wird über TCP/IP- und Unix-Domänensockets unterstützt. |
| **Firewall** | Zum Schutz Ihrer Daten verhindert eine Firewallregel jeglichen Zugriff auf Ihren Datenbankserver, bis Sie angeben, welche Computer zugriffsberechtigt sind. Informationen hierzu finden Sie unter [Firewallregeln für den Azure Database for MariaDB-Server](./concepts-firewall-rules.md). |
| **SSL** | Der Dienst unterstützt die Erzwingung von SSL-Verbindungen zwischen Ihrer Anwendung und Ihrem Datenbankserver.  <!--See [Configure SSL connectivity in your application to securely connect to Azure Database for MariaDB](./howto-configure-ssl.md).--> |

## <a name="how-do-i-manage-a-server"></a>Wie verwalte ich einen Server?
Sie können Azure Database for MariaDB-Server mithilfe des Azure-Portals oder der Azure CLI verwalten.

## <a name="next-steps"></a>Nächste Schritte
- Eine Dienstübersicht finden Sie im [Überblick über Azure Database for MariaDB](./overview.md).
- Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrem **Diensttarif** finden Sie unter [Diensttarife](./concepts-pricing-tiers.md)
<!-- - For information about connecting to the service, see [Connection libraries for Azure Database for MariaDB](./concepts-connection-libraries.md). -->.
