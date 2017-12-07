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
ms.date: 11/17/2017
ms.author: billgib
ms.openlocfilehash: 2a36df0e45af5bcce5338d04b7e1ba44221ae964
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
# <a name="the-wingtip-tickets-saas-application"></a>Die Wingtip Tickets SaaS-Anwendung

In jedem der drei Beispiele wird die gleiche *Wingtip Tickets*-Anwendung implementiert. Die App ist eine einfache SaaS-App für die Auflistung von Veranstaltungsorten, z.B. Theater oder Clubs, und den Verkauf von Eintrittskarten. Jeder Veranstaltungsort ist ein Mandant der App und umfasst spezifische Daten: Details zum Ort, eine Liste der Veranstaltungen, Kunden, Kartenbestellungen usw.  Die App veranschaulicht zusammen mit den Verwaltungsskripts und Tutorials ein vollständiges SaaS-Szenario. Hierzu gehört Folgendes: Bereitstellung von Mandanten, Überwachung und Verwaltung der Leistung, Schemaverwaltung sowie mandantenübergreifende Berichterstellung und Analyse.

## <a name="three-saas-application-patterns"></a>Drei SaaS-Anwendungsmuster

Es stehen drei Versionen der App zur Verfügung. Jede dieser Versionen steht für ein anderes Datenbankmandantenmuster in Azure SQL-Datenbank.  Das erste Beispiel verwendet eine Anwendung mit einem Mandanten und einer isolierten Datenbank für diesen einen Mandanten. Das zweite verwendet eine mehrinstanzenfähige App mit einer Datenbank pro Mandant. Das dritte verwendet eine mehrinstanzenfähige App mit mehrinstanzenfähigen Datenbanken mit Sharding.

![Drei Mandantenmuster][image-three-tenancy-patterns]

 Jedes Beispiel enthält Verwaltungsskripts und Tutorials mit verschiedenen Entwurfs- und Verwaltungsmustern, die Sie in Ihrer eigenen Anwendung verwenden können.  Jedes Beispiel lässt sich innerhalb von fünf Minuten bereitstellen.  Alle drei Beispiele können nebeneinander bereitgestellt werden, sodass Sie die Unterschiede hinsichtlich Entwurf und Verwaltung miteinander vergleichen können.

## <a name="standalone-application-pattern"></a>Muster für eigenständige Anwendung

Das Muster für eine eigenständige App verwendet eine Anwendung mit einem Mandant und einer einzelnen Datenbank für diesen Mandanten. Für jeden Mandanten wird eine App in einer separaten Azure-Ressourcengruppe bereitgestellt. Diese kann sich im Abonnement des Dienstanbieters oder im Abonnement des Mandanten befinden und wird vom Anbieter im Auftrag des Mandanten verwaltet. Dieses Muster bietet das höchste Maß an Isolierung für die Mandanten, ist aber in der Regel das teuerste, da Ressourcen nicht von mehreren Mandanten gemeinsam genutzt werden können.

Weitere Informationen finden Sie in den [Tutorials][docs-tutorials-for-wingtip-sa], den Code finden Sie auf GitHub: [.../Microsoft/WingtipTicketsSaaS-StandaloneApp][github-code-for-wingtip-sa].

## <a name="database-per-tenant-pattern"></a>Muster mit einer Datenbank pro Mandant

Das Muster mit einer Datenbank pro Mandant ist eine effektive Version für Dienstanbieter, die sowohl ihre Mandanten isolieren müssen als auch einen zentralisierten Dienst ausführen möchten, der eine kosteneffiziente Verwendung gemeinsam genutzter Ressourcen ermöglicht. Für jeden Veranstaltungsort (also jeden Mandanten) wird eine Datenbank erstellt, und alle Datenbanken werden zentral verwaltet. Datenbanken können in elastischen Pools gehostet werden, um eine kosteneffiziente und einfache Leistungsverwaltung zu ermöglichen, die von den nicht vorhersehbaren Workloadmustern der Mandanten profitiert. Eine Katalogdatenbank enthält die Zuordnung zwischen Mandanten und ihren Datenbanken. Diese Zuordnung wird mithilfe der Verwaltungsfeatures der [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md) für die Shardzuordnung verwaltet, die eine effiziente Verbindungsverwaltung für die Anwendung ermöglicht.

Weitere Informationen finden Sie in den [Tutorials][docs-tutorials-for-wingtip-dpt], den Code finden Sie auf GitHub: [.../Microsoft/WingtipTicketsSaaS-DbPerTenant][github-code-for-wingtip-dpt].

## <a name="sharded-multi-tenant-database-pattern"></a>Muster für mehrinstanzenfähige Datenbanken mit Sharding

Mehrinstanzenfähige Datenbanken sind eine effektive Möglichkeit für Dienstanbieter, die die Kosten pro Mandant senken möchten und für die ein geringeres Maß an Isolation für die Mandanten kein Problem darstellt. Mit diesem Muster lässt sich eine große Anzahl von Mandanten mit einer einzigen Datenbank verwalten, wodurch die Kosten pro Mandant sinken. Durch Sharding (horizontales Partitionieren) der Mandanten über mehrere Datenbanken hinweg ist eine nahezu unbegrenzte Skalierung möglich.  Auch hier werden die Mandanten den Datenbanken über eine Katalogdatenbank zugeordnet.  

Dieses Muster ermöglicht auch ein hybrides Modell, in dem Sie durch Verwenden einer einzigen Datenbank für mehrere Mandanten die Kosten optimieren oder Ihre Anwendung mit einem einzigen Mandanten pro Datenbank für die Isolierung optimieren können. Diese Entscheidung kann für jeden Mandanten einzeln getroffen werden, entweder während der Bereitstellung des Mandanten oder später – ohne Auswirkungen auf die Anwendung.

Weitere Informationen finden Sie in den [Tutorials][docs-tutorials-for-wingtip-mt], den Code finden Sie auf GitHub: [.../Microsoft/WingtipTicketsSaaS-MultiTenantDb][github-code-for-wingtip-mt].

## <a name="next-steps"></a>Nächste Schritte

#### <a name="conceptual-descriptions"></a>Beschreibungen der Konzepte

- Eine genauere Erläuterung der Mandantenmuster für Anwendungen finden Sie unter [Mandantenmuster für mehrinstanzenfähige SaaS-Datenbanken][saas-tenancy-app-design-patterns-md].

#### <a name="tutorials-and-code"></a>Tutorials und Code

- Eigenständige App:
    - [Tutorials für eigenständige Apps][docs-tutorials-for-wingtip-sa].
    - [Code für eigenständige Apps auf GitHub][github-code-for-wingtip-sa].

- Eine Datenbank pro Mandant:
    - [Tutorials für eine Datenbank pro Mandant][docs-tutorials-for-wingtip-dpt].
    - [Code für eine Datenbank pro Mandant auf GitHub][github-code-for-wingtip-dpt].

- Mehrinstanzenfähige Datenbank mit Shards:
    - [Tutorials für mehrinstanzenfähige Datenbanken mit Shards][docs-tutorials-for-wingtip-mt].
    - [Code für mehrinstanzenfähige Datenbanken mit Shards auf GitHub][github-code-for-wingtip-mt].



<!-- Image references. -->

[image-three-tenancy-patterns]: media/saas-tenancy-welcome-wingtip-tickets-app/three-tenancy-patterns.png "Drei Mandantenmuster"

<!-- Docs.ms.com references. -->

[saas-tenancy-app-design-patterns-md]: saas-tenancy-app-design-patterns.md

<!-- WWWeb http references. -->

[docs-tutorials-for-wingtip-sa]: https://aka.ms/wingtipticketssaas-sa
[github-code-for-wingtip-sa]: https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp

[docs-tutorials-for-wingtip-dpt]: https://aka.ms/wingtipticketssaas-dpt
[github-code-for-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant

[docs-tutorials-for-wingtip-mt]: https://aka.ms/wingtipticketssaas-mt
[github-code-for-wingtip-mt]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb

