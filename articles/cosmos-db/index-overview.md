---
title: Indizierung in Azure Cosmos DB
description: Erhalten Sie Informationen zur Funktionsweise der Indizierung in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: c0525cfba16fb61f8388ae4d6a693be3bb71674c
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628529"
---
# <a name="indexing-in-azure-cosmos-db"></a>Indizierung in Azure Cosmos DB

Azure Cosmos DB ist eine schemaunabhängige Datenbank und ermöglicht es Ihnen, schnell Ihre Anwendung zu durchlaufen, ohne sich mit Schema- oder Indexverwaltung befassen zu müssen. Standardmäßig indiziert Azure Cosmos DB automatisch alle Elemente in Ihrem Container, ohne dass Schema- oder Sekundärindizes von Entwicklern benötigt werden.

## <a name="items-as-trees"></a>Elemente als Strukturen

Indem Elemente in einem Container als JSON-Dokumente projiziert und als Strukturen dargestellt werden, normalisiert Azure Cosmos DB sowohl die Struktur als auch die Instanzwerte für alle Elemente in das vereinheitlichende Konzept einer  **dynamisch codierten Pfadstruktur**. In dieser Darstellung wird jede Bezeichnung in einem JSON-Dokument, das sowohl die Eigenschaftsnamen als auch deren Werte enthält, zu einem Knoten des Struktur. Die Blätter des Strukturbaums enthalten Istwerte und die Zwischenknoten enthalten die Schemainformationen. Das folgende Bild zeigt die Strukturen, die für zwei Elemente (1 und 2) in einem Container erstellt wurden:

![Strukturdarstellung für zwei verschiedene Elemente in einem Azure Cosmos-Container](./media/index-overview/indexing-as-tree.png)

Ein Pseudostammknoten wird als übergeordneter Knoten zu den eigentlichen Knoten angelegt, die den Bezeichnungen im darunter liegenden Dokument entsprechen. Die geschachtelten Datenstrukturen unterstützen die Hierarchie in der Struktur. Zwischenliegende künstliche Knoten, die mit numerischen Werten bezeichnet sind (z.B. 0, 1, ....), werden zur Darstellung von Enumerationen und Arrayindizes verwendet.

## <a name="index-paths"></a>Indexpfade

Azure Cosmos DB stellt Elemente als JSON-Dokumente und den Index als Strukturen dar. Sie können die Richtlinien für Pfade innerhalb der Struktur optimieren. Sie können Pfade in die Indizierung einschließen bzw. von dieser ausschließen. Dies kann bessere Schreibleistungen und geringeren Indexspeicherbedarf für Szenarien bieten, in denen die Abfragemuster im Voraus bekannt sind. Weitere Informationen finden Sie unter [Indexpfade](index-paths.md).

## <a name="indexing-under-the-hood"></a>Indizierung: Funktionsweise

Die Azure Cosmos-Datenbank wendet die automatische Indizierung auf die Daten an, wobei jeder Pfad in einer Struktur indiziert wird, es sei denn, Sie nehmen die Konfiguration so vor, dass bestimmte Pfade ausgeschlossen werden.

Die Azure Cosmos-Datenbank verwendet eine invertierte Indexdatenstruktur, um die Informationen zu jedem Element zu speichern und eine effiziente Darstellung der Abfrage zu ermöglichen. Die Indexstruktur ist ein Dokument, das aus der Vereinigung aller Strukturen besteht, die einzelne Elemente im Container darstellen. Die Indexstruktur wächst im Laufe der Zeit, wenn neue Elemente hinzugefügt oder bestehende Elemente im Container aktualisiert werden. Im Gegensatz zur Indexierung relationaler Datenbanken startet Azure Cosmos DB die Indizierung nicht von Grund auf neu, wenn neue Felder eingeführt und neue Elemente der bestehenden Struktur hinzugefügt werden. 

Jeder Knoten der Indexstruktur ist ein Indexeintrag, der die Bezeichnungs- und Positionswerte, die sogenannte Benennung, und die IDs der Elemente, die sogenannten Buchungen, enthält. Die Buchungen in den geschweiften Klammern (z.B. {1,2}) in der invertierten Indexzahl entsprechen den Elementen wie Document1 und Document2, die den angegebenen Bezeichungswert enthalten. Eine wichtige Schlussfolgerung für eine einheitliche Behandlung sowohl der Schemabezeichnungen als auch der Instanzwerte ist, dass alles in einen großen Index eingebunden ist. Ein Instanzwert, der sich noch in den Blättern befindet, wird nicht wiederholt, er kann in unterschiedlichen Elemente verschiedenen Rollen mit unterschiedlichen Schemabezeichnungen einnehmen, aber es ist derselbe Wert. Die folgende Abbildung zeigt die invertierte Indizierung für verschiedene Elemente:

![Funktionsweise der Indizierung, invertierter Index](./media/index-overview/inverted-index.png)

> [!NOTE]
> Der invertierte Index kann ähnlich aussehen wie die Indizierungsstrukturen, die in einer Suchmaschine in der Domäne zum Abrufen von Information verwendet werden. Mit dieser Methode ermöglicht es Ihnen Azure Cosmos DB, Ihre Datenbank unabhängig von seiner Schemastruktur nach jedem Element zu durchsuchen.

Für den normalisierten Pfad codiert der Index den Weiterleitungspfad vom Stammverzeichnis bis zum Wert zusammen mit den Typinformationen des Wertes. Der Pfad und der Wert werden codiert, um verschiedene Arten der Indizierung bereitzustellen, wie z.B. Bereich, räumliche Arten. Die Wertecodierung ist so konzipiert, dass sie einen eindeutigen Wert oder eine Zusammensetzung aus einer Reihe von Pfaden liefert.

## <a name="querying-with-indexes"></a>Abfragen mit Indizes

Der invertierte Index ermöglicht es einer Abfrage, die Dokumente, die mit dem Abfrageprädikat übereinstimmen, schnell zu identifizieren. Durch die einheitliche Behandlung von Schema- und Instanzwerten in Bezug auf Pfade ist der invertierte Index auch eine Struktur. Somit können der Index und die Ergebnisse zu einem gültigen JSON-Dokument serialisiert und bei der Rückgabe in der Strukturdarstellung selbst als Dokumente zurückgegeben werden. Diese Methode ermöglicht es, die Ergebnisse für zusätzliche Abfragen zu wiederholen. Die folgende Abbildung zeigt ein Beispiel für die Indizierung in einer Punktabfrage:  

![Beispiel für eine Punktabfrage](./media/index-overview/index-point-query.png)

Bei einer Bereichsabfrage ist GermanTax eine benutzerdefinierte Funktion, die im Rahmen der Abfrageverarbeitung ausgeführt wird. Die benutzerdefinierte Funktion ist eine beliebige registrierte Javascript-Funktion, die eine umfangreiche Programmierlogik bieten kann, die in die Abfrage integriert ist. Die folgende Abbildung zeigt ein Beispiel für die Indizierung in einer Bereichsabfrage:

![Beispiel für eine Bereichsabfrage](./media/index-overview/index-range-query.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Indizierung erhalten Sie in den folgenden Artikeln:

- [Indizierungsrichtlinie](index-policy.md)
- [Indextypen](index-types.md)
- [Indexpfade](index-paths.md)
- [Gewusst wie: Verwalten der Indizierungsrichtlinie](how-to-manage-indexing-policy.md)