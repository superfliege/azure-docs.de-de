---
title: Verwenden von Indexpfaden in Azure Cosmos DB
description: Übersicht über Indexpfade in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 3/13/2019
ms.author: mjbrown
ms.openlocfilehash: d0fce763822ded374eab2f70c3f319aba0c89267
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57992833"
---
# <a name="index-paths-in-azure-cosmos-db"></a>Indexpfade in Azure Cosmos DB

Durch die Verwendung von Indexpfaden in Azure Cosmos DB können Sie wählen, ob ein bestimmter Pfad für die Indizierung ein- oder ausgeschlossen werden soll. Die Auswahl bestimmter Pfade ermöglicht eine verbesserte Schreibleistung und einen geringeren Aufwand für die Indexspeicherung in Szenarien, in denen Ihnen die Abfragemuster bekannt sind. Indexpfade beginnen mit dem Platzhalteroperator für den Stamm (`/`) und enden normalerweise mit dem Platzhalteroperator `?`. Bei diesem Muster gibt es mehrere mögliche Werte für das Präfix. Wenn Sie beispielsweise die Abfrage `SELECT * FROM Families F WHERE F.familyName = "Andersen"` bereitstellen möchten, müssen Sie einen Indexpfad für `/familyName/?` in der Indexrichtlinie des Containers einfügen.

Indexpfade können auch das Platzhalterzeichen `*`verwenden, um das Verhalten für Pfade rekursiv unter dem Präfix anzugeben. Beispielsweise kann `/payload/*` verwendet werden, um sämtliche Elemente unter der payload-Eigenschaft von der Indizierung auszuschließen.

## <a name="common-patterns-for-index-paths"></a>Häufige Muster für Indexpfade

Im Folgenden sind die allgemeinen Muster zum Angeben von Indexpfaden aufgeführt:

| **Path** | **Beschreibung/Anwendungsfall** |
| ---------- | ------- |
| /          | Der Standardpfad für die Sammlung. Rekursiv und gilt für die gesamte Dokumentstruktur.|
| /prop/?    | Zum Verarbeiten der nachfolgenden Abfragen erforderlicher Indexpfad (entsprechend mit Bereichstyp): <br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>SELECT FROM collection c WHERE c.prop > 5 <br><br>SELECT FROM collection c ORDER BY c.prop  |
| /prop/*    | Indexpfad für alle Pfade unter der angegebenen Bezeichnung. Funktioniert mit den folgenden Abfragen. <br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>SELECT FROM collection c WHERE c.prop.subprop > 5<br><br>SELECT FROM collection c WHERE c.prop.subprop.nextprop = "value"<br><br>SELECT FROM collection c ORDER BY c.prop |
| /props/[]/?| Indexpfad ist erforderlich für Iteration und JOIN-Abfragen für Arrays mit Skalaren wie ["a", "b", "c"]:<br><br>SELECT tag FROM tag IN collection.props WHERE tag = "value"<br><br>SELECT tag FROM collection c JOIN tag IN c.props WHERE tag > 5 |
| /props/[]/subprop/? | Indexpfad ist erforderlich für Iteration und JOIN-Abfragen für Arrays mit Objekten wie [{subprop: "a"}, {subprop: "b"}]:<br><br>SELECT tag FROM tag IN collection.props WHERE tag.subprop = "value"<br><br>SELECT tag FROM collection c JOIN tag IN c.props WHERE tag.subprop = "value" |
| /prop/subprop/? | Zum Verarbeiten der folgenden Abfragen erforderlicher Indexpfad (entsprechend mit Bereichstyp):<br><br>SELECT FROM collection c WHERE c.prop.subprop = "value"<br><br>SELECT FROM collection c WHERE c.prop.subprop > 5  |

Beim Festlegen von benutzerdefinierten Indexpfaden müssen Sie die Standardindizierungsregel für das gesamte Element angeben, indem Sie den speziellen Pfad `/*` verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Durchführen der Indizierung in Azure Cosmos DB finden Sie in den folgenden Artikeln:

- [Indexing in Azure Cosmos DB](index-overview.md) (Indizierung in Azure Cosmos DB)
- [Indexing policies in Azure Cosmos DB](indexing-policies.md) (Indizierungsrichtlinien in Azure Cosmos DB)
- [Index types in Azure Cosmos DB](index-types.md) (Indextypen in Azure Cosmos DB)
