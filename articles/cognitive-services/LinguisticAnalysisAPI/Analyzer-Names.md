---
title: Benennungsstruktur im Analysetool in der API für die linguistische Analyse | Microsoft-Dokumentation
description: Erfahren Sie, wie die API für die linguistische Analyse ihr Benennungsstruktur für Analysetools verwendet, um Flexibilität und Genauigkeit zu ermöglichen.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 03/23/2016
ms.author: lesun
ms.openlocfilehash: 2729b7126e82862660fc8e1a995cc87ae996ea03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372987"
---
# <a name="analyzer-names"></a>Analysetoolnamen

Wir verwenden eine etwas komplizierte Benennungsstruktur für Analysetools, um sowohl Flexibilität bei den Analysetools als auch Genauigkeit beim Verstehen der Bedeutung der Benennung zu ermöglichen.
Namen für Analysetools bestehen aus vier Teilen: eine ID, eine Variante, eine Spezifikation und eine Implementierung.
Die Rolle der einzelnen Komponenten wird unten definiert.

## <a name="id"></a>ID
Zunächst hat ein Analysetool eine eindeutige ID: eine GUID.
Diese GUIDs sollten sich relativ selten ändern, sind aber die einzige Möglichkeit, ein bestimmtes Analysetool eindeutig zu beschreiben.

## <a name="kind"></a>Variante
Als Nächstes ist jedes Analysetool eine **Variante**.
Diese definiert ganz allgemein die Art der zurückgegebenen Analyse und sollte die Datenstruktur, die zur Darstellung dieser Analyse verwendet wird, eindeutig definieren.
Derzeit gibt es drei verschiedene Varianten:
 - [Token](Sentences-and-Tokens.md)
 - [POS-Tags](Pos-Tagging.md)
 - [Konstituentenstruktur](constituency-parsing.md)

## <a name="specification"></a>Spezifikation
Innerhalb einer bestimmten Variante können sich jedoch verschiedene Experten uneinig darüber sein, wie ein bestimmtes Phänomen analysiert werden soll.
Im Gegensatz zu Programmiersprachen gibt es keine klare und genaue Definition, wie dies geschehen soll.

Stellen Sie sich zum Beispiel vor, wir würden versuchen, die Token im englischen Satz „He didn't go.“ zu finden.
Beachten Sie insbesondere die Zeichenfolge „didn‘t“.
Eine mögliche Interpretation ist, die Zeichenfolge in zwei Token aufzuteilen: „did“ und „not“.
Der alternative Satz „He did not go“ würde dann denselben Satz an Token enthalten.
Eine andere Möglichkeit wäre es, dass sie in die Token „did“ und „n't“ aufgeteilt werden könnte.
Letzteres Token wird normalerweise nicht als Wort betrachtet, aber dieser Ansatz enthält mehr Informationen über die Oberflächenzeichenfolge, was manchmal nützlich sein kann.
Oder diese Kontraktion kann als einzelnes Wort betrachtet werden.

Egal, für welche Möglichkeit Sie sich entscheiden, sollten Sie diese konsistent beibehalten.
Dies ist genau die Rolle einer **Spezifikation**: zu entscheiden, was eine korrekte Darstellung werden soll.

Ausgaben von Analysetools können nur ordnungsgemäß mit Daten verglichen werden, die der gleichen Spezifikation entsprechen.

## <a name="implementation"></a>Implementierung

Oft gibt es mehrere Modelle, die versuchen, die gleichen Ergebnisse zu erzielen, aber mit unterschiedlichen Leistungsmerkmalen.
Ein Modell ist vielleicht schneller, dafür aber weniger genau; ein anderes könnte einen anderen Kompromiss eingehen.

Die Teil der **Implementierung** eines Analysetoolnamens wird verwendet, um diese Art von Informationen zu identifizieren, sodass der Benutzer das für seine Bedürfnisse am besten geeignete Analysetool auswählen kann.
