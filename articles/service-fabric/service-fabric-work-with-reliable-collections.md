---
title: "Arbeiten mit zuverlässigen Auflistungen | Microsoft Docs"
description: "Lernen Sie die bewährten Methoden zum Arbeiten mit zuverlässigen Auflistungen."
services: service-fabric
documentationcenter: .net
author: rajak
manager: timlt
editor: 
ms.assetid: 39e0cd6b-32c4-4b97-bbcf-33dad93dcad1
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2017
ms.author: rajak
ms.openlocfilehash: f53f13e4fb83b1cd370ec673e86e5311cd93055f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="working-with-reliable-collections"></a>Arbeiten mit zuverlässigen Auflistungen
Service Fabric bietet .NET Entwickler über zuverlässige Sammlungen ein zustandsbehaftetes Programmiermodell verfügbar. Insbesondere marshallt Service Fabric zuverlässige Wörterbuch und zuverlässige Warteschlangeklassen. Wenn Sie diese Klassen verwenden, ist Ihren Zustand (für die Skalierbarkeit) partitioniert repliziert (für Verfügbarkeit) und transaktive innerhalb einer Partition (für ACID-Semantik). Wir sehen uns typisch für eine zuverlässige Dictionary-Objekt, und finden Sie, welche die tatsächlich ausführen.

```csharp

///retry:

try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) {
   await Task.Delay(100, cancellationToken); goto retry;
}
```

Alle Vorgänge auf zuverlässige Wörterbuchobjekten (mit Ausnahme von ClearAsync, die nicht rückgängig gemacht werden kann), erfordert ein ITransaction-Objekt. Dieses Objekt verfügt über zugeordnete mit ihm alle und alle Änderungen, die Sie stellen eine zuverlässige Wörterbuch und/oder zuverlässige Warteschlange Objekte innerhalb einer einzelnen Partition. Sie erhalten eine ITransaction des Objekts durch Aufrufen der Partitions StateManagers CreateTransaction-Methode.

Im obigen Code wird das ITransaction-Objekt an ein zuverlässiges Wörterbuch AddAsync-Methode übergeben. Wörterbuchmethoden, die einen Schlüssel akzeptiert dauern intern eine Reader/Writer-Sperre, die dem Schlüssel zugeordnet. Wenn die Methode den Wert des Schlüssels geändert wird, akzeptiert die Methode eine Schreibsperre für den Schlüssel, und wenn die Methode nur aus den Wert des Schlüssels liest, klicken Sie dann eine Lesesperre auf den Schlüssel erstellt. Da AddAsync des Werts des Schlüssels auf den neuen, übergebenen Wert ändert, wird der Schlüssel Schreibsperre angewendet. Also wenn 2 (oder mehr) Threads versuchen, Werte mit demselben Schlüssel gleichzeitig hinzuzufügen, ein Thread die Schreibsperre abgerufen werden, und anderen Threads blockiert. Standardmäßig werden Methoden für bis zu 4 Sekunden auf die Sperre blockiert; Klicken Sie nach 4 Sekunden lösen die Methoden eine TimeoutException. Überladungen der Methode vorhanden sein, sodass Sie einen explizites Timeout-Wert übergeben wird, wenn Sie lieber.

In der Regel ist, Schreiben Sie den Code, um eine TimeoutException reagieren, indem abfangen und den gesamten Vorgang wiederholen, (wie im obigen Code gezeigt). In meinem Code einfacher bin ich nur Task.Delay übergeben 100 Millisekunden bei jedem aufrufen. Aber in der Praxis können Sie möglicherweise besser, verwenden Sie stattdessen eine Art von exponentiellen Backoff-Verzögerung.

Sobald die Sperre abgerufen wurde, fügt AddAsync Objektverweise Schlüssel und Wert auf eine interne temporäre Wörterbuch ITransaction-Objekt zugeordnet. Dies erfolgt, um Sie mit Ihren-Besitzer-Lese-Semantik bereitstellen. D. h. nach dem Aufruf von AddAsync wird ein späteren Aufrufs von TryGetValueAsync (mit dem gleichen ITransaction-Objekt) den Wert zurück, selbst wenn die Transaktion noch nicht gespeichert haben. Als Nächstes AddAsync serialisiert Ihren Schlüssel und Wert Bytearrays-Objekte und fügt diese Bytearrays in eine Protokolldatei auf dem lokalen Knoten. Schließlich sendet AddAsync Byte-Arrays an die sekundären Replikate müssen die gleichen Schlüssel/Wert-Informationen. Obwohl die Schlüssel/Wert-Informationen in eine Protokolldatei geschrieben wurde, die Informationen nicht als Teil des Wörterbuchs, bis die Transaktion, der sie zugeordnet sind ein Commit ausgeführt wurde.

Im obigen Code führt einen Commit für der Aufruf von CommitAsync alle Vorgänge für die Transaktion. Insbesondere Commit-Informationen in die Protokolldatei auf dem lokalen Knoten angehängt, und auch den Commitdatensatz an alle sekundären Replikate gesendet wird. Sobald ein Quorum (Mehrheit) von den Replikaten beantwortet hat, gelten alle datenänderungen permanent, und alle Schlüssel, die über das Objekt ITransaction bearbeitet wurden zugeordneten Sperren werden freigegeben, damit andere Threads/Transaktionen auf den gleichen Schlüsseln und deren Werte bearbeiten können.

Wenn CommitAsync (in der Regel aufgrund einer Ausnahme, die ausgelöst wird) nicht aufgerufen wird, ruft das ITransaction-Objekt freigegeben. Wenn ein ohne Commit ITransaction-Objekt zu verwerfen, fügt Service Fabric Abort Informationen zur Protokolldatei für den lokalen Knoten, und keine Aktionen erforderlich, um ein sekundäres Replikat gesendet werden. Ein, und klicken Sie dann alle Schlüssel, die über die Transaktion bearbeitet wurden zugeordneten Sperren werden aufgehoben.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Häufige Probleme und zum vermeiden
Nun, dass Sie verstehen, wie die zuverlässige Sammlungen intern arbeiten, werfen wir einen Blick auf einige allgemeine falsch davon. Finden Sie unter den folgenden Code:

```csharp
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes,
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
```

Bei der Arbeit mit einem regulären .NET Wörterbuch können Sie dem Wörterbuch einen Schlüssel-Wert hinzufügen und ändern Sie den Wert einer Eigenschaft (z. B. LastLogin). Allerdings ist dieser Code nicht mit einem zuverlässigen Wörterbuch funktionsfähig. Denken Sie daran, aus der früheren Diskussion der Aufruf von AddAsync serialisiert die Schlüssel/Wert-Objekte in Bytearrays und speichert dann die Arrays in eine lokale Datei und sendet diese auch an die sekundären Replikate. Wenn Sie später eine Eigenschaft ändern, ändert dieser den Wert der Eigenschaft nur im Speicher; Er wirkt sich nicht auf die lokale Datei oder die Daten gesendet werden, auf die Replikate aus. Bei einem Prozessabbruch, was im Arbeitsspeicher ist sofort ausgelöst. Wenn ein neuer Prozess gestartet wird oder einem anderen Replikat das primäre Element wird, wird der alte Wert der Eigenschaft zur Verfügung steht.

Ich kann nicht genug wie einfach es ist, stellen die Art der oben gezeigten Fehler zu belasten. Und Sie werden nur erfahren Sie mehr über den Fehler bei, dass der Prozess ausfällt. Die korrekte Methode zum Schreiben des Codes besteht darin, zwei Zeilen umkehren:


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

Hier ist ein weiteres Beispiel: einen häufiger Fehler angezeigt:

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync();
   }
}
```

Erneut mit regulären .NET Wörterbücher, der obige Code funktioniert einwandfrei, und ist ein allgemeines Muster: der Entwickler verwendet einen Schlüssel, um einen Wert zu suchen. Wenn der Wert vorhanden ist, ändert der Entwickler den Wert einer Eigenschaft an. Dieser Code weist jedoch mit zuverlässigen Sammlungen das gleiche Problem wie bereits erläutert: **müssen Sie ein Objekt nicht ändern, nachdem Sie es für eine zuverlässige Sammlung erhalten haben.**

Die richtige Methode, die einen Wert in einer zuverlässigen Sammlung aktualisieren wird zum Abrufen eines Verweises auf den vorhandenen Wert und betrachten das Objekt verweist auf diesen Verweis unveränderlich. Anschließend erstellen Sie ein neues Objekt, das eine genaue Kopie des ursprünglichen Objekts ist. Jetzt, Sie ändern den Status für das neue Objekt und schreiben können das neue Objekt in der Auflistung, damit es in Bytearrays, serialisiert werden an die lokale Datei angehängt und gesendet, um die Replikate. Nach dem Commit der Änderung, aufweisen die in-Memory-Objekte, die lokale Datei und alle Replikate genau denselben Bundesstaat. Alles in Ordnung ist.

Der folgende Code zeigt die richtige Methode, die einen Wert in einer zuverlässigen Sammlung zu aktualisieren:

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync();
   }
}
```

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Definieren Sie unveränderliche Datentypen Programmierer Fehler vermeiden
Im Idealfall möchten wir den Compiler, um Fehler zu melden, wenn Sie versehentlich Code erzeugen, der Zustand eines Objekts ändert, die Sie unveränderlich berücksichtigen. Aber der C#-Compiler keinen die Möglichkeit dazu. Also, um potenzielle Programmierer Fehler zu vermeiden, wird dringend, dass Sie die Typen definieren Sie mit zuverlässigen Auflistungen verwenden, um die unveränderlichen Typen sein. Insbesondere bedeutet dies, dass Sie Core Werttypen (z. B. Zahlen [Int32, UInt64 usw.], "DateTime", Guid, TimeSpan und Like) verwenden. Und natürlich können Sie auch Zeichenfolge. Am besten vermeiden Sammlungseigenschaften als Serialisierung und Deserialisierung kann häufig kann die Leistung beeinträchtigen. Wenn Sie die Eigenschaften der Sammlung verwenden möchten, empfehlen wir jedoch hoch die Verwendung von. Unveränderliche Auflistungen der Netzwerkbibliothek ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)). Diese Bibliothek ist zum Download von http://nuget.org zur Verfügung. Es wird empfohlen, versiegeln Klassen und Felder schreibgeschützt machen, wann immer möglich ist.

Welche UserInfo veranschaulicht, wie einen unveränderlichen Typ, der Nutzen der bereits erwähnten Empfehlungen zu definieren.

```csharp

[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;

   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }

   [DataMember]
   public readonly String Email;

   // Ideally, this would be a readonly field but it can't be because OnDeserialized
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
```

Der Element-ID-Typ ist ein unveränderlicher Typ auch, wie hier gezeigt:

```csharp

[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
```

## <a name="schema-versioning-upgrades"></a>Versionsverwaltung des Schemas (Upgrades)
Intern serialisieren zuverlässige Sammlungen Objekte mithilfe. NET "DataContractSerializer". Die serialisierte Objekte auf dem primären Replikat lokalem Datenträger beibehalten werden und auch an die sekundären Replikate gesendet werden. Bei fortschreitender Ihr Dienst ist es wahrscheinlich ist, sollten Sie die Art der Daten (Schema) zu ändern, die Ihr Dienst erfordert. Sie müssen die versionsverwaltung Ihrer Daten mit großer Umsicht annähern. Zunächst müssen Sie immer zum Deserialisieren von alter Daten sein. Dies bedeutet insbesondere, deserialisierungscodes muss unbegrenzt abwärts kompatibel sein: 333 der Version des Codes Dienst muss in der Lage, Daten platziert in einer zuverlässigen Sammlung von Version 1 der Dienstcode 5 Jahre vor bearbeitet werden.

Darüber hinaus ist Dienstcode aktualisierten einer upgradedomäne zu einem Zeitpunkt. Während eines Upgrades müssen Sie daher zwei unterschiedliche Versionen des Code Ihres Diensts, die gleichzeitig ausgeführt werden. Sie müssen vermeiden, dass die neue Version des Codes Service das neue Schema verwenden, wie alte Versionen von Code Ihres Diensts möglicherweise nicht das neue Schema zu behandeln. Wenn möglich, sollten Sie jede Version des Diensts bei aufwärtskompatibel mit Version 1 werden entwerfen. Insbesondere bedeutet dies, dass der Code Ihres Diensts V1 können ignoriert alle Schemaelemente, die explizit nicht verarbeiten kann. Allerdings muss er sein um alle Daten, denen sie explizit nicht bekannt ist und einfach Zurückschreiben bei der Aktualisierung ein Wörterbuchschlüssel oder Wert zu speichern.

> [!WARNING]
> Während Sie das Schema eines Schlüssels ändern können, müssen Sie sicherstellen, dass Hashcode des Schlüssels und Equals-Algorithmen stabil sind. Wenn Sie ändern, wie eine der folgenden Algorithmen funktionieren, werden Sie nicht den Schlüssel im Wörterbuch zuverlässige jemals erneut zu suchen können.
>
>

Alternativ können Sie ausführen, in der Regel als "ein 2-Phasen-Upgrade" bezeichnet wird. Mit einem 2-Phasen-Upgrade upgrade Ihren Dienst V1, V2: V2 enthält den Code, der weiß, wie für den Umgang mit dem neuen Schema ändern, aber dieser Code nicht ausgeführt werden. Wenn der Code V2 V1 Daten liest, darauf arbeitet und schreibt V1-Daten. Klicken Sie dann nach das Upgrade alle upgradedomänen abgeschlossen ist, können Sie irgendeine der ausgeführten V2-Instanzen zu signalisieren, dass das Upgrade abgeschlossen ist. (Eine Möglichkeit zum Signal sieht eine konfigurationsaktualisierung Rollout; Dies wird dadurch eine 2-Phasen-Upgrade wird). Jetzt können die V2-Instanzen V1 Daten lesen, konvertieren Sie ihn in V2-Daten verarbeiten und schreibt sie als V2-Daten. Wenn andere Instanzen V2-Daten gelesen, sie ist nicht erforderlich, die sie konvertieren, nur verarbeiten, V2 Daten ausgeschrieben werden.

## <a name="next-steps"></a>Nächste Schritte
Zum Erstellen von Vorwärts kompatiblen Datenverträgen finden Sie unter [aufwärtskompatible Datenverträge](https://msdn.microsoft.com/library/ms731083.aspx).

Bewährte Methoden zur versionsverwaltung von Datenverträgen finden Sie unter [Datenvertragsversionsverwaltung](https://msdn.microsoft.com/library/ms731138.aspx).

Gewusst wie: Implementieren Sie fehlertolerante Datenverträge Version finden Sie unter [versionstolerante Serialisierungsrückrufe](https://msdn.microsoft.com/library/ms733734.aspx).

Um zu erfahren, wie eine Datenstruktur bereitstellen, die über mehrere Versionen hinweg zusammenarbeiten können, finden Sie unter [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
