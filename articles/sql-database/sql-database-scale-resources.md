---
title: Azure SQL-Datenbank – Skalieren von Ressourcen | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Ihre Datenbank skalieren, indem Sie zugeordnete Ressourcen hinzufügen oder entfernen.
services: sql-database
author: jovanpop-msft
ms.reviewer: carlrab
ms.service: sql-database
ms.topic: conceptual
ms.date: 07/07/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: f55ce511f6ba90c27e149ac90bbd2c8aa0b3c742
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "37923438"
---
# <a name="scale-database-resources"></a>Skalieren von Datenbankressourcen

Mit Azure SQL-Datenbank können Sie Ihrer Datenbank dynamisch weitere Ressourcen hinzufügen, ohne dass längere Ausfallzeiten erforderlich sind.

## <a name="overview"></a>Übersicht

Wenn die Nachfrage nach Ihrer App von einer Handvoll von Geräten und Kunden in den Millionenbereich steigt, kann Azure SQL-Datenbank im laufenden Betrieb mit minimaler Ausfallzeit skaliert werden. Die Skalierbarkeit ist eines der wichtigsten Merkmale von PaaS, mit dem Sie Ihrem Dienst bei Bedarf dynamisch weitere Ressourcen hinzufügen können. Mit Azure SQL-Datenbank können Sie Ressourcen (CPU-Leistung, Arbeitsspeicher, E/A-Durchsatz und Speicher), die Ihren Datenbanken zugeordnet sind, leicht ändern.  
Sie können Leistungsprobleme beheben, die aufgrund der vermehrten Nutzung Ihrer Anwendung auftreten und sich per Indizierung oder mit Methoden zum Umschreiben von Abfragen nicht beseitigen lassen. Durch das Hinzufügen von weiteren Ressourcen können Sie schnell reagieren, wenn Ihre Datenbank die derzeitigen Ressourcenlimits erreicht und eine höhere Leistung benötigt, um die eingehende Workload verarbeiten zu können. Mit Azure SQL-Datenbank können Sie die Ressourcen auch zentral herunterskalieren, wenn sie nicht benötigt werden, um die Kosten zu senken.
Sie müssen sich nicht mit dem Kauf von Hardware und der Änderung der zugrunde liegenden Infrastruktur befassen. Sie können die Datenbank skalieren, indem Sie einfach im Azure-Portal einen Schieberegler verwenden.

![Skalieren der Datenbankleistung](media/sql-database-scalability/scale-performance.svg)

Azure SQL-Datenbank verfügt über ein [DTU-basiertes Kaufmodell](sql-database-service-tiers-dtu.md) sowie das [V-Kern-basierte Kaufmodell (Vorschauversion)](sql-database-service-tiers-vcore.md). 
-   Das [DTU-basierte Kaufmodell](sql-database-service-tiers-dtu.md) bietet zur Unterstützung von einfachen bis hin zu komplexen Datenbankworkloads eine Mischung aus Compute-, Arbeitsspeicher- und E/A-Ressourcen auf drei Dienstebenen: Basic, Standard und Premium. Leistungsstufen auf den einzelnen Ebenen bieten unterschiedliche Ressourcenzusammenstellungen, durch zusätzliche Speicherressourcen ergänzt werden können.
-   Beim [V-Kern-basierten Kaufmodell](sql-database-service-tiers-vcore.md) (Vorschauversion) können Sie die Anzahl virtueller Kerne, die Arbeitsspeichermenge sowie Menge und Geschwindigkeit des Speichers auswählen.
Sie können zu geringen monatlichen Kosten Ihre erste App in einer kleinen Einzeldatenbank erstellen und dann manuell oder programmgesteuert jederzeit die Dienstebene (Tarif) ändern, um die Anforderungen Ihrer Lösung zu erfüllen. Die Leistungsanpassung ist möglich, ohne dass es für die App oder für Ihre Kunden zu Ausfallzeiten kommt. Dank der dynamischen Skalierbarkeit kann Ihre Datenbank in transparenter Form auf sich schnell ändernde Ressourcenanforderungen reagieren, und Sie zahlen nur für die Ressourcen, die Sie jeweils benötigen.


> [!NOTE]
> Dynamische Skalierbarkeit ist nicht dasselbe wie automatische Skalierung. Bei der automatischen Skalierung wird ein Dienst automatisch auf der Grundlage von Kriterien skaliert. Die dynamische Skalierbarkeit ermöglicht dagegen eine manuelle Skalierung ohne Ausfallzeiten.
>


Eine einzelne Azure SQL-Datenbank unterstützt die manuelle dynamische Skalierbarkeit, aber keine automatischen Skalierung. Ein höheres Maß an *Automatisierung* lässt sich bei Bedarf mithilfe von Pools für elastische Datenbanken erzielen, die die gemeinsame Nutzung eines Ressourcenpools auf der Grundlage individueller Datenbankanforderungen ermöglichen.
Es gibt allerdings auch Skripts, die Sie bei der Automatisierung der Skalierbarkeit für eine einzelne Azure SQL-Datenbank unterstützen. Ein Beispiel finden Sie unter [Überwachen und Skalieren einer einzelnen SQL­-Datenbank mit PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).


Alle drei Arten von Azure SQL-Datenbank verfügen über Funktionen zum dynamischen Skalieren Ihrer Datenbanken:
-   In einer [Azure SQL-Einzeldatenbank](sql-database-single-database-scale.md) können Sie entweder [DTU](sql-database-dtu-resource-limits-single-databases.md)- oder [V-Kern](sql-database-vcore-resource-limits-single-databases.md)-Modelle nutzen, um die maximale Menge von Ressourcen zu definieren, die den einzelnen Datenbanken zugewiesen werden.
-   Für eine [verwaltete Azure SQL-Instanz](sql-database-managed-instance.md) wird der [V-Kern](/azure/sql-database/sql-database-managed-instance#vcore-based-purchasing-model-preview)-Modus verwendet, und Sie können die maximale Anzahl von CPU-Kernen und den maximalen Speicher für Ihre Instanz definieren. Alle Datenbanken innerhalb der Instanz nutzen die der Instanz zugeordneten Ressourcen gemeinsam.
-   Mit [Pools für elastische Azure SQL-Datenbanken](sql-database-elastic-pool-scale.md) können Sie das maximale Ressourcenlimit pro Datenbankgruppe im Pool definieren.

## <a name="alternative-scale-methods"></a>Alternative Skalierungsmethoden
Die Skalierung von Ressourcen ist die einfachste und effektivste Möglichkeit, die Leistung Ihrer Datenbank zu verbessern, ohne entweder den Datenbank- oder Anwendungscode zu ändern.
In einigen Fällen kann es sein, dass auch die höchsten Leistungsstufen und -optimierungen zu einer erfolgreichen und kostengünstigen Verarbeitung Ihrer Workload führen. In diesen Fällen haben Sie weitere Möglichkeiten zum Skalieren Ihrer Datenbank:
-   Die [horizontale Leseskalierung](sql-database-read-scale-out.md) ist ein verfügbares Feature, bei dem Sie ein schreibgeschütztes Replikat Ihrer Daten erhalten, über das Sie anspruchsvolle schreibgeschützte Abfragen, z.B. Berichte, ausführen können. Mit dem schreibgeschützten Replikat wird Ihre schreibgeschützte Workload verarbeitet, ohne dass sich Auswirkungen auf die Ressourcenverwendung in Ihrer primären Datenbank ergeben.
-   Das [Datenbank-Sharding](sql-database-elastic-scale-introduction.md) umfasst eine Reihe von Verfahren, mit denen Sie Ihre Daten in mehrere Datenbanken aufteilen und unabhängig voneinander skalieren können.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zur Verbesserung der Datenbankleistung durch das Ändern des Datenbankcodes finden Sie unter [Suchen und Anwenden von Empfehlungen zur Leistung](sql-database-advisor-portal.md).
- Informationen zur Optimierung Ihrer Datenbank mit integrierter Datenbank-Intelligence finden Sie unter [Automatische Optimierung](sql-database-automatic-tuning.md).
- Informationen zur horizontalen Leseskalierung im Azure SQL-Datenbank-Dienst finden Sie unter [Verwenden von schreibgeschützten Replikaten für den Lastenausgleich schreibgeschützter Abfrageworkloads](sql-database-read-scale-out.md).
- Informationen zum Datenbank-Sharding finden Sie unter [Horizontales Hochskalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md).

