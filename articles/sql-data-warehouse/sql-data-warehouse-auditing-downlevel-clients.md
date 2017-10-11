---
title: "SQL Data Warehouse – Unterstützung von kompatiblen Clients für die Datenüberwachung | Microsoft Docs"
description: "Sie erhalten Informationen zur Unterstützung von kompatiblen Clients in SQL Data Warehouse für die Datenüberwachung."
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: dfe29ff3-dfeb-4309-83c0-c1a300f4f44e
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: a7ea6141285a0098339f1e071af2592dd4535c12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>SQL Data Warehouse – Unterstützung von kompatiblen Clients für Überwachung und dynamische Datenmaskierung
[Überwachung](sql-data-warehouse-auditing-overview.md) kann mit SQL-Clients genutzt werden, die die TDS-Umleitung unterstützen.

Jeder Client, der TDS 7.4 implementiert, sollte auch die Umleitung unterstützen. Zu den Ausnahmen gehören JDBC 4.0, in dem die Umleitungsfunktion nicht vollständig unterstützt wird, und Tedious for Node.JSS, in dem die Umleitungsfunktion nicht implementiert wurde.

Für "Vorgängerversionsclients", d. h. Clients, die TDS 7.3 und ältere Versionen unterstützen, sollte der vollqualifizierte Domänenname des Servers in der Verbindungszeichenfolge geändert werden:

Ursprünglicher vollqualifizierter Domänenname des Servers in der Verbindungszeichenfolge: <*Servername*>.database.windows.net

Geänderter vollqualifizierter Domänenname des Servers in der Verbindungszeichenfolge: <*Servername*>.database.**secure**.windows.net

Eine unvollständige Liste der "Vorgängerversionsclients":

* .NET 4.0 und ältere Versionen
* ODBC 10.0 und ältere Versionen
* JDBC (JDBC unterstützt zwar TDS 7.4, aber die TDS-Umleitungsfunktion wird nicht vollständig unterstützt)
* Tedious (for Node.JS)

**Anmerkung:** Die oben genannte Änderung des vollqualifizierten Domänennamens des Servers kann sich auch als nützlich erweisen bei der Anwendung einer Richtlinie für die Überwachung auf SQL Server-Ebene, ohne dass ein Konfigurationsschritt in jeder Datenbank ausgeführt werden muss (temporäre Minderung).     

