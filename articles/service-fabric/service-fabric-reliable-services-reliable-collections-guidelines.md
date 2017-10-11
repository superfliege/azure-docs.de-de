---
title: "Richtlinien und Empfehlungen für die zuverlässige Sammlungen in Azure Service Fabric | Microsoft Docs"
description: "Richtlinien und Empfehlungen für die Verwendung von zuverlässigen Service Fabric-Auflistungen"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/3/2017
ms.author: mcoskun
ms.openlocfilehash: 053a7bca76362035e428fc11806b3e4f83d00946
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Richtlinien und Empfehlungen für die zuverlässige Sammlungen in Azure Service Fabric
Dieser Abschnitt enthält Richtlinien für die Verwendung von zuverlässigen Status-Manager und zuverlässige Sammlungen. Das Ziel ist, damit Benutzer häufige Fehlerquellen vermeiden können.

Die Richtlinien werden als einfache Empfehlungen, die mit den Begriffen vorangestellt organisiert *führen*, *ggf.*, *vermeiden* und *nicht*.

* Verändern sich nicht auf ein Objekt des benutzerdefinierten Typs zurückgegeben, die für Lesevorgänge (z. B. `TryPeekAsync` oder `TryGetValueAsync`). Zuverlässige Auflistungen, wie gleichzeitige Sammlungen geben einen Verweis auf die Objekte und keine Kopie zurück.
* Führen Sie Tiefenkopie das zurückgegebene Objekt eines benutzerdefinierten Typs, bevor Sie Sie ändern. Da Strukturen und integrierte Typen Übergabe nach Wert sind, müssen Sie keine tiefe Kopie davon zu tun.
* Verwenden Sie keine `TimeSpan.MaxValue` für Timeouts. Timeouts sollte verwendet werden, damit Deadlocks erkannt werden.
* Verwenden Sie keine Transaktion, nachdem sie ein Commit ausgeführt, wurde abgebrochen, oder freigegeben wurde.
* Verwenden Sie eine Enumeration nicht außerhalb der Geltungsbereich der Transaktion, in der Sie erstellt wurde.
* Erstellen Sie eine Transaktion in einer anderen Transaktion nicht `using` Anweisung, da es zu Deadlocks führen kann.
* Stellen Sie sicher, die Ihre `IComparable<TKey>` Implementierung richtig ist. Das System nimmt Abhängigkeit `IComparable<TKey>` für das Zusammenführen von Prüfpunkten und Zeilen.
* Verwenden Sie updatesperre beim Lesen eines Elements mit einer Absicht zum Aktualisieren eine bestimmten Form von Deadlocks zu vermeiden.
* Berücksichtigen Sie Ihre Elemente (z. B. TKey + TValue für zuverlässige Wörterbuch) unter 80 KB: kleinere, desto besser. Dies reduziert den großen Objektheap Nutzung sowie die Datenträger- und Netzwerkvorgänge-e/a-Anforderungen. Häufig, verringert sich doppelte Daten replizieren, wenn nur ein kleiner Teil der Wert aktualisiert wird. Üblicherweise wird dies im zuverlässige Wörterbuch zu erreichen ist um Ihre Zeilen auf mehrere Zeilen.
* Sicherung in Betracht, und Wiederherstellungsfunktionalität, um die Wiederherstellung im Notfall zu verfügen.
* Vermeiden Sie das Mischen von Einheit Vorgänge und Vorgänge für mehrere Entitäten (z. B. `GetCountAsync`, `CreateEnumerableAsync`) in der gleichen Transaktion aufgrund von den einzelnen Isolationsstufen möglich.
* Behandeln Sie InvalidOperationException. Benutzertransaktionen, die vom System für aus verschiedenen Gründen abgebrochen werden können. Beispielsweise wird Wenn zuverlässige Status-Manager seine Rolle außerhalb des primären geändert wird oder wenn eine lang andauernde Transaktion durch Abschneiden des Transaktionsprotokolls blockiert. In solchen Fällen erhalten Benutzer, die InvalidOperationException gibt an, dass die Transaktion wurde bereits beendet. Vorausgesetzt, die Beendigung der Transaktion wurde nicht vom Benutzer angefordert wird, am besten zur Behandlung dieser Ausnahme ist, verwerfen die Transaktion, überprüfen Sie, ob das Abbruchtoken, das signalisiert wurde (oder die Rolle des Replikats wurde geändert) und, wenn eine neue Transaktion und wiederholen Sie den Vorgang nicht erstellen.  

Hier sind einige Dinge zu bedenken:

* Das Standardzeitlimit beträgt vier Sekunden für die zuverlässige Auflistung-APIs. Die meisten Benutzer sollten das Standardtimeout verwenden.
* Wird das Standardabbruchtoken `CancellationToken.None` in allen zuverlässige Sammlungen-APIs.
* Der Typ des Schlüssels-Parameter (*TKey*) für eine zuverlässige Wörterbuch, ordnungsgemäß implementieren muss `GetHashCode()` und `Equals()`. Schlüssel müssen unveränderlich sein.
* Um hohe Verfügbarkeit für die zuverlässige Auflistungen zu erreichen, müssen jeden Dienst mindestens ein Ziel und die Mindestgröße des Replikatsatzes Größe von 3 festgelegt.
* Lesevorgänge auf dem sekundären darf Versionen lesen, die nicht Quorum, die ein Commit ausgeführt werden.
  Dies bedeutet, dass eine Version der Daten, die von einem einzelnen sekundären gelesen wird "false" ausgeführt werden kann.
  Liest vom primären Server sind immer stabilen: kann nie werden "false" fortgeschritten.

### <a name="next-steps"></a>Nächste Schritte
* [Arbeiten mit zuverlässigen Auflistungen](service-fabric-work-with-reliable-collections.md)
* [Transaktionen und Sperren](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* [Zuverlässige Status-Manager und Auflistung Besonderheiten](service-fabric-reliable-services-reliable-collections-internals.md)
* Verwalten von Daten
  * [Sichern und Wiederherstellen](service-fabric-reliable-services-backup-restore.md)
  * [Benachrichtigungen](service-fabric-reliable-services-notifications.md)
  * [Serialisierung und Upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Zuverlässige Zustands-Manager](service-fabric-reliable-services-configuration.md)
* Andere Benutzer
  * [Zuverlässige Services-Schnellstart](service-fabric-reliable-services-quick-start.md)
  * [Entwicklerreferenz für zuverlässige Sammlungen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
