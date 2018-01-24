---
title: Serverkonzepte in Azure Database for PostgreSQL | Microsoft-Dokumentation
description: "Dieses Thema enthält Aspekte und Richtlinien für das Konfigurieren und Verwalten von Azure Database for PostgreSQL-Servern."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 12/02/2017
ms.openlocfilehash: d7eec2735e48f57500eb2ea822f0949d2ec2e585
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
# <a name="azure-database-for-postgresql-servers"></a>Azure Database for PostgreSQL-Server
Dieser Artikel beschreibt Überlegungen und Richtlinien für die Arbeit mit Azure Database for PostgreSQL-Servern.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Was ist ein Azure Database for PostgreSQL-Server?
Ein Azure Database for PostgreSQL-Server fungiert als zentraler Verwaltungspunkt für mehrere Datenbanken. Sie weist dasselbe PostgreSQL-Serverkonstrukt auf, das Sie möglicherweise aus lokalen Umgebungen kennen. Im Besonderen ist der PostgreSQL-Dienst verwaltet, bietet Leistungsgarantien und stellt Zugriff sowie Funktionen auf Serverebene bereit.

Ein Azure Database for PostgreSQL-Server weist folgende Eigenschaften auf:

- Sie wird im Rahmen eines Azure-Abonnements erstellt.
- Sie stellt die übergeordnete Ressource für Datenbanken dar.
- Sie stellt einen Namespace für Datenbanken bereit.
- Sie ist ein Container mit einer Semantik von hoher Lebensdauer. Beim Löschen eines Servers werden die enthaltenen Datenbanken gelöscht.
- Sie stellt Ressourcen in einer Region zusammen.
- Sie stellt einen Verbindungsendpunkt für den Server- und Datenbankzugriff (.postgresql.database.azure.com) bereit.
- Sie stellt den Bereich für Verwaltungsrichtlinien bereit, die auf die jeweiligen Datenbanken angewendet werden (Anmeldungen, Firewalls, Benutzer, Rollen, Konfigurationen etc.).
- Sie ist in mehreren Versionen verfügbar. Weitere Informationen finden Sie unter [Unterstützte PostgreSQL-Datenbankversionen](concepts-supported-versions.md).
- Sie kann von Benutzern erweitert werden. Weitere Informationen finden Sie im Artikel zu [PostgreSQL-Erweiterungen](concepts-extensions.md).

Sie können auf einem Azure Database for PostgreSQL-Server eine oder mehrere Datenbanken erstellen. Sie können eine Einzeldatenbank pro Server erstellen, die alle Ressourcen nutzt, oder Sie können mehrere Datenbanken erstellen, um die Ressourcen gemeinsam zu verwenden. Die Preise gelten pro Server und basieren auf der Konfiguration des Tarifs, der Computeeinheiten und des Speichers (GB). Weitere Informationen finden Sie unter [Tarife](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-postgresql-server"></a>Wie stelle ich eine Verbindung zu einem Azure Database for PostgreSQL-Server her und authentifiziere diesen?
Mithilfe der folgenden Elemente kann ein sicherer Zugriff auf Ihre Datenbank sichergestellt werden:

|||
|:--|:--|
| **Authentifizierung und Autorisierung** | Der Azure Database for PostgreSQL-Server unterstützt die systemeigene PostgreSQL-Authentifizierung. Mithilfe der Anmeldeinformationen für Serveradministrator können Sie eine Verbindung zum Server herstellen und diesen authentifizieren. |
| **Protokoll** | Der Dienst unterstützt ein nachrichtenbasiertes Protokoll, das von PostgreSQL verwendet wird. |
| **TCP/IP** | Das Protokoll wird über TCP/IP- und Unix-Domänensockets unterstützt. |
| **Firewall** | Zum Schutz Ihrer Daten verhindert eine Firewallregel jeglichen Zugriff auf Ihren Server und auf dessen Datenbanken, solange Sie nicht angeben, welche Computer zugriffsberechtigt sind. Weitere Informationen finden Sie unter [Firewallregeln für Azure Database for PostgreSQL-Server](concepts-firewall-rules.md). |

## <a name="how-do-i-manage-a-server"></a>Wie verwalte ich einen Server?
Sie können Azure Database for PostgreSQL-Server mithilfe des [Azure-Portals](https://portal.azure.com) oder der [Azure CLI](/cli/azure/postgres) verwalten.

## <a name="server-parameters"></a>Serverparameter
Die PostgreSQL-Serverparameter bestimmen die Konfiguration des Servers. In Azure Database for PostgreSQL kann die Liste der Parameter über das Azure-Portal oder die Azure CLI angezeigt und bearbeitet werden. 

Da es sich um einen verwalteten Dienst für Postgres handelt, stellen die konfigurierbaren Parameter in Azure Database for PostgreSQL eine Teilmenge der Parameter in einer lokalen Postgres-Instanz dar. (Weitere Informationen zu Postgres-Parametern finden Sie in der [PostgreSQL-Dokumentation](https://www.postgresql.org/docs/9.6/static/runtime-config.html).) Bei der Erstellung wird der Azure Database for PostgreSQL-Server mit Standardwerten für die einzelnen Parameter konfiguriert. Parameter, die einen Serverneustart oder Superuser-Zugriff erfordern, damit sie übernommen werden, können nicht vom Benutzer konfiguriert werden.


## <a name="next-steps"></a>Nächste Schritte
- Einen Überblick über den Dienst finden Sie unter [Azure Database for PostgreSQL – Überblick](overview.md).
- Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrem **Diensttarif** finden Sie unter [Diensttarife](concepts-service-tiers.md).
- Informationen zum Herstellen einer Verbindung mit dem Dienst finden Sie unter [Datenverbindungsbibliotheken für Azure Database for PostgreSQL](concepts-connection-libraries.md).
- Sie können Serverparameter über das [Azure-Portal](howto-configure-server-parameters-using-portal.md) oder die [Azure CLI](howto-configure-server-parameters-using-cli.md) anzeigen und bearbeiten.
