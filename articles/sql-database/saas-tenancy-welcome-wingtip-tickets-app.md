---
title: "Willkommen bei der Wingtips-App – Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Erfahren Sie etwas über Datenbank-Mandantenmodelle und über die Beispielanwendung Wingtips SaaS für Azure SQL-Datenbank in der Cloudumgebung."
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: billgib
manager: craigg
editor: MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: billgib;genemi
ms.openlocfilehash: 96e031835905057a9ab2b3ee4023b08de092dd8e
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="welcome-to-the-wingtip-tickets-sample-saas-azure-sql-database-tenancy-app"></a>Willkommen bei der Beispielmandanten-SaaS-App Wingtip Tickets für Azure SQL-Datenbank

Willkommen bei der Beispielmandanten-SaaS-Anwendung Wingtip Tickets für Azure SQL-Datenbank und den zugehörigen Tutorials Datenbankmandant bezieht sich auf den Isolationsmodus für Daten, den Ihre App Ihren Clients bietet, die für das Hosten in Ihrer Anwendung bezahlen. Zur Vereinfachung verfügt jeder Client über eine vollständige Datenbank für sich selbst, oder er teilt sich eine Datenbank mit einem anderen Client.

## <a name="wingtip-tickets-app"></a>Wingtip Tickets-App

Die Beispielanwendung Wingtip Tickets veranschaulicht die Auswirkungen unterschiedlicher Mandantenmodelle in Datenbanken im Hinblick auf den Entwurf und die Verwaltung von mehrinstanzenfähigen SaaS-Anwendungen. Die begleitenden Tutorials beschreiben diese Auswirkungen direkt. Wingtip Tickets basiert auf Azure SQL-Datenbank.

Wingtip Tickets soll verschiedene Entwurfs- und Verwaltungsszenarien behandeln, die von tatsächlichen SaaS-Clients verwendet werden. Die abgeleiteten Nutzungsmuster werden in Wingtip Tickets berücksichtigt.

Sie können die Wingtip Tickets-App in fünf Minuten in Ihrem eigenen Azure-Abonnement installieren. Die Installation umfasst das Einfügen von Beispieldaten für mehrere Mandanten. Sie können die Anwendung und Verwaltungsskripts für alle Modelle problemlos installieren, da die Installationen nicht untereinander interagieren oder sich behindern.

#### <a name="code-in-github"></a>Code in GitHub

Anwendungscode und Verwaltungsskripts sind auf GitHub verfügbar:

- Modell mit **eigenständiger App**: Repository [WingtipTicketsSaaS-StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp)
- Modell mit einer **Datenbank pro Mandant**: Repository [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant)
- Modell mit **mehrinstanzenfähigem Sharding**: Repository [WingtipTicketsSaaS-MultiTenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB)

Dieselbe Codebasis für die Wingtip Tickets-App wird für alle aufgeführten Modelle wiederverwendet. Sie können den Code von GitHub für eigene SaaS-Projekte verwenden.



## <a name="major-database-tenancy-models"></a>Wichtige Mandantenmodelle für Datenbanken

Wingtip Tickets ist eine SaaS-Anwendung für die Ankündigung von Veranstaltungen und den Ticketverkauf. Wingtip stellt Dienste bereit, die von Veranstaltern benötigt werden. Die folgenden Elemente gelten für alle Veranstaltungsorte:

- Bezahlt Sie für das Hosten in Ihrer Anwendung
- Ist ein *Mandant* in Wingtip
- Führt Veranstaltungen durch Die folgenden Ereignisse treten auf:
    - Ticketpreise
    - Ticketverkäufe
    - Kunden, die Tickets erwerben

Die App veranschaulicht zusammen mit den Verwaltungsskripts und Tutorials ein vollständiges SaaS-Szenario. Das Szenario umfasst die folgenden Aktivitäten:

- Bereitstellen von Mandanten
- Überwachen und Verwalten der Leistung
- Verwalten des Schemas
- Erstellen von mandantenübergreifenden Berichten und Analysen

Alle diese Aktivitäten werden mit der erforderlichen Skalierung bereitgestellt.



## <a name="code-samples-for-each-tenancy-model"></a>Codebeispiele für jedes Mandantenmodell

Bestimmte Anwendungsmodelle werden intensiver behandelt. Andere Implementierungen können jedoch Elemente von zwei oder mehreren Modellen kombinieren.

#### <a name="standalone-app-model"></a>Modell mit einer eigenständigen App

![Modell mit einer eigenständigen App][standalone-app-model-62s]

Dieses Modell verwendet eine Einzelmandantenanwendung. Aus diesem Grund benötigt dieses Modell nur eine Datenbank. Es werden nur die Daten für einen Mandanten gespeichert. Der Mandant ist vollständig von anderen Mandanten in der Datenbank isoliert.

Sie können dieses Modell verwenden, wenn Sie Instanzen Ihrer App an viele verschiedene Clients verkaufen, die diese jeweils eigenständig ausführen. Der Client ist dann der einzige Mandant. Auch wenn in der Datenbank nur Daten für einen einzigen Client gespeichert werden, werden jedoch Daten für viele Kunden des Clients gespeichert.

#### <a name="database-per-tenant"></a>Datenbank pro Mandant

![Modell mit einer Datenbank pro Mandant][database-per-tenant-model-35d]

Dieses Modell umfasst mehrere Mandanten in der Instanz der Anwendung. Für jeden neuen Mandanten wird jedoch eine andere Datenbank für die Verwendung nur durch den neuen Mandanten zugeordnet.

Dieses Modell bietet vollständige Datenbankisolation für jeden Mandanten. Der Azure SQL-Datenbank-Dienst bietet alle Voraussetzungen für dieses Modell.

- [Einführung zum Beispiel einer mehrinstanzenfähigen SQL-Datenbank-SaaS-App][saas-dbpertenant-wingtip-app-overview-15d] enthält weitere Informationen.

#### <a name="sharded-multi-tenant-databases-the-hybrid"></a>Mehrinstanzenfähige Datenbanken mit Shards – die Hybridlösung

![Modell mit mehrinstanzenfähigen Datenbanken mit Shards – die Hybridlösung][sharded-multitenantdb-model-hybrid-79m]

Dieses Modell umfasst mehrere Mandanten in der Instanz der Anwendung. Dieses Modell verfügt ebenfalls über mehrere Mandanten in einigen oder allen zugehörigen Datenbanken. Dieses Modell eignet sich für die verschiedenen Dienstebenen, sodass Clients mehr bezahlen können, wenn sie Wert auf eine vollständige Datenbankisolation legen.

Das Schema jeder Datenbank umfasst eine Mandanten-ID. Die Mandanten-ID ist auch in Datenbanken enthalten, die nur einen einzigen Mandanten speichern.

- [Einführung zum Beispiel einer mehrinstanzenfähigen SQL-Datenbank-SaaS-App][saas-multitenantdb-get-started-deploy-89i]



## <a name="tutorials-for-each-tenancy-model"></a>Tutorials für jedes Mandantenmodell

Jedes Mandantenmodell wird folgendermaßen beschrieben:

- Eine Reihe von Tutorialartikeln
- Der Quellcode ist in einem GitHub-Repository gespeichert, das ausschließlich für das Modell verwendet wird:
    - Der Code für die Wingtip Tickets-Anwendung
    - Der Skriptcode für Verwaltungsszenarien

#### <a name="tutorials-for-management-scenarios"></a>Tutorials für Verwaltungsszenarien

Die Tutorialartikel für jedes Modell behandeln die folgenden Verwaltungsszenarien:

- Bereitstellen von Mandanten
- Überwachen und Verwalten der Leistung
- Verwalten des Schemas
- Erstellen von mandantenübergreifenden Berichten und Analysen
- Wiederherstellen eines Mandanten zu einem früheren Zeitpunkt
- Notfallwiederherstellung



## <a name="next-steps"></a>Nächste Schritte

- [Einführung zum Beispiel einer mehrinstanzenfähigen SQL-Datenbank-SaaS-App][saas-dbpertenant-wingtip-app-overview-15d] enthält weitere Informationen.

- [Mandantenmuster für mehrinstanzenfähige SaaS-Datenbanken][multi-tenant-saas-database-tenancy-patterns-60p]



<!-- Image references. -->

[standalone-app-model-62s]: media/saas-tenancy-welcome-wingtip-tickets-app/model-standalone-app.png "Modell mit einer eigenständigen App"

[database-per-tenant-model-35d]: media/saas-tenancy-welcome-wingtip-tickets-app/model-database-per-tenant.png "Modell mit einer Datenbank pro Mandant"

[sharded-multitenantdb-model-hybrid-79m]: media/saas-tenancy-welcome-wingtip-tickets-app/model-sharded-multitenantdb-hybrid.png "Modell mit mehrinstanzenfähigen Datenbanken mit Shards – die Hybridlösung"



<!-- Article references. -->

[saas-dbpertenant-wingtip-app-overview-15d]: saas-dbpertenant-wingtip-app-overview.md

[multi-tenant-saas-database-tenancy-patterns-60p]: saas-tenancy-app-design-patterns.md

[saas-multitenantdb-get-started-deploy-89i]: saas-multitenantdb-get-started-deploy.md


