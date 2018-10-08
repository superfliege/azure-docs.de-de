---
title: Konfliktauflösung in Azure Cosmos DB-Multimaster
description: Dieser Artikel beschreibt die Konfliktkategorien und -auflösungsmodi wie „Letzter Schreiber gewinnt“, „Benutzerdefiniert – benutzerdefinierte Prozedur“, „Benutzerdefiniert – asynchron“ in Azure Cosmos DB-Multimaster.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 5feefdb8fe6204bc8ef42a5e65bf1e30354e0cf9
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393926"
---
# <a name="multi-master-conflict-resolution-in-azure-cosmos-db"></a>Konfliktauflösung in Azure Cosmos DB-Multimaster 

Die Multimasterfunktion von Azure Cosmos DB bietet globale Konfliktauflösungsmodi, um sicherzustellen, dass Daten in allen Regionen, in denen sie repliziert werden, konsistent sind.

## <a name="conflict-categories"></a>Konfliktkategorien

Es gibt drei Kategorien von Konflikten, die bei der Arbeit mit Azure Cosmos DB-Daten auftreten können:

* **Einfügekonflikte**: Diese Art von Konflikt tritt auf, wenn die Anwendung mindestens zwei Dokumente mit dem gleichen eindeutigen Index (z.B. „ID“) aus mindestens zwei Regionen gleichzeitig einfügt. In diesem Fall werden alle Schreibvorgänge zunächst möglicherweise erfolgreich ausgeführt, basierend auf der Konfliktauflösungsrichtlinie wird jedoch nur ein Dokument mit der ursprünglichen ID committet.

* **Ersetzungskonflikte**: Diese Art von Konflikt tritt auf, wenn die Anwendung ein einzelnes Dokument von mindestens zwei Regionen gleichzeitig aktualisiert.

* **Löschkonflikte**: Diese Art von Konflikt tritt auf, wenn die Anwendung ein Dokument aus einer Region löscht und es in mindestens einer Region aktualisiert. 

## <a name="conflict-resolution-modes"></a>Konfliktauflösungsmodi

In Azure Cosmos DB werden drei Konfliktauflösungsmodi bereitgestellt. Konfliktauflösungsmodi werden für jede Sammlung definiert.

> [!NOTE]
> SQL-API-Benutzer können zwischen drei verschiedenen Konfliktauflösungsmodi wählen. Für alle anderen API-Modelle (MongoDB, Cassandra, Graph und Tabelle) werden Konflikte mithilfe von „Letzter Schreiber gewinnt“ gelöst.

### <a name="last-writer-wins-lww"></a>Letzter Schreiber gewinnt

„Letzter Schreiber gewinnt“ ist der standardmäßige Konfliktauflösungsmodus. In diesem Modus werden Konflikte basierend auf einem numerischen Wert aufgelöst, der in einer Eigenschaft für das Dokument übergeben wird.

Der folgende Codeausschnitt zeigt beispielhaft, wie die Konfliktauflösungsrichtlinie „Letzter Schreiber gewinnt“ bei der Verwendung des .NET SDK konfiguriert wird. Der ConflictResolutionPath definiert den Pfad zur Eigenschaft, mit der der Konflikt aufgelöst wird. In diesem Beispiel ist `/userDefinedId` der Konfliktauflösungspfad und das Dokument mit dem größten `userDefinedId`-Wert wird den Konflikt immer gewinnen. Um einen Auflösungsmodus namens „Letzter Schreiber gewinnt“ zu registrieren, stellen Sie die Sammlung mit der ConflictResolutionPolicy bereit, wie unten dargestellt wird.

```csharp
DocumentCollection lwwCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
   Id = "myCollection", 
   ConflictResolutionPolicy = new ConflictResolutionPolicy
   {
      Mode = ConflictResolutionMode.LastWriterWins,
      ConflictResolutionPath = "/userDefinedId"
   }
});
```

 Der Auflösungsmodus „Letzter Schreiber gewinnt“ wird wie folgt auf unterschiedliche Datenkonfliktkategorien angewendet:

* **Einfüge- und Aktualisierungskonflikte**: Wenn bei Einfüge- oder Ersetzungsvorgängen mindestens zwei Dokumente in Konflikt stehen, gewinnt das Dokument mit den größten Wert für den Konfliktauflösungspfad (d.h. userDefinedId). Wenn mehrere Dokumente denselben numerischen Wert für den Konfliktauflösungspfad aufweisen, ist der ausgewählte Gewinner nicht deterministisch. Alle Regionen werden jedoch zu einem einzelnen Gewinner zusammengeführt.

* **Löschkonflikt**: Bei Löschkonflikten gewinnt immer der Löschvorgang gegenüber anderen Ersetzungskonflikten, unabhängig von dem Wert des Konfliktauflösungspfads.

### <a name="custom--user-defined-procedure"></a>Benutzerdefiniert – benutzerdefinierte Prozedur

In diesem Modus steuert der Benutzer die Konfliktauflösung, indem er eine benutzerdefinierte Prozedur (User Defined Procedure, UDP) registriert. Diese UDP weist eine spezielle Signatur auf. Wenn Sie diese Option auswählen, jedoch keine UDP registrieren, oder die UDP zur Laufzeit eine Ausnahme auslöst, werden Konflikte in den Konfliktfeed geschrieben, in dem sie einzeln aufgelöst werden können.

Um einen Konfliktauflösungsmodus namens „Benutzerdefiniert – benutzerdefinierte Prozedur“ zu registrieren, stellen Sie die Sammlung mit der ConflictResolutionPolicy bereit, wie unten dargestellt wird.

```csharp
DocumentCollection udpCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
  Id = "myCollection",
  ConflictResolutionPolicy = new ConflictResolutionPolicy
  {
     Mode = ConflictResolutionMode.Custom,
     ConflictResolutionProcedure = UriFactory.CreateStoredProcedureUri("myDatabase","myCollection","myUdpStoredProcedure").ToString()
  }
});
```

Als Nächstes fügen Sie der Sammlung die benutzerdefinierte Prozedur hinzu, wie unten dargestellt wird.

```csharp
StoredProcedure myUdpStoredProc = new StoredProcedure
{
   Id = "myUdpStoredProcedure",
   Body = myUdpStoredProcText
};
await myClient.UpsertStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("myDatabase", "myCollection"), myUdpStoredProc);
```

Die bei der Sammlung registrierte gespeicherte Prozedur weist eine spezielle Signatur auf. In dem folgenden Beispiel verwendet die UDP die Eigenschaft `UserDefinedId`, um Konflikte aufzulösen. Das Dokument mit dem größten Wert gewinnt den Konflikt.

```javascript
function myUdpStoredProcedure(incomingDocument, existingDocument, isDeleteConflict, conflictingDocuments){
    var collection = getContext().getCollection();

    if (!incomingDocument) {
        if (existingDocument) {

            collection.deleteDocument(existingDocument._self, {}, function(err, responseOptions) {
                if (err) throw err;
            });
        }
    } else if (isDeleteConflict) {
        // delete always wins.
    } else {
        var documentToUse = incomingDocument;

        if (existingDocument) {
            if (documentToUse.userDefinedId < existingDocument.userDefinedId) {
                documentToUse = existingDocument;
            }
        }

        var i;
        for (i = 0; i < conflictingDocuments.length; i++) {
            if (documentToUse.userDefinedId < conflictingDocuments[i].userDefinedId) {
                documentToUse = conflictingDocuments[i];
            }
        }

        tryDelete(conflictingDocuments, incomingDocument, existingDocument, documentToUse);
    }

    function tryDelete(documents, incoming, existing, documentToInsert) {
        if (documents.length > 0) {
            collection.deleteDocument(documents[0]._self, {}, function(err, responseOptions) {
                if (err) throw err;

                documents.shift();
                tryDelete(documents, incoming, existing, documentToInsert);
            });
        } else if (existing) {
            collection.replaceDocument(existing._self, documentToInsert,
                function(err, documentCreated) {
                    if (err) throw err;
                });
        } else {
            collection.createDocument(collection.getSelfLink(), documentToInsert,
                function(err, documentCreated) {
                    if (err) throw err;
                });
        }
    }
}

```

Die Prozedur weist vier Parameter auf:

* **incomingDocument**: Gibt die in Konflikt stehende Version des Dokuments an. Der Konflikt kann ein Einfüge-, Ersetzungs- oder Löschkonflikt sein. Bei einem Löschkonflikt bezieht sich dies auf ein Image (Tombstone) ohne Inhalt.

* **existingDocument**: Gibt die Version des Dokuments an, für die ein Commit ausgeführt wurde und die den gleichen rid-Wert wie das incomingDocument aufweist. Dieser Parameter wird bei einem Einfüge- und Löschkonflikt auf NULL festgelegt.

* **isDeleteConflict**: Dieses boolesche Flag gibt an, ob das eingehende Dokument mit einem zuvor gelöschten Dokument in Konflikt steht. Wenn dieser Parameter auf „true“ festgelegt ist, ist existingDocument ebenfalls NULL.

* **conflictingDocuments**: Gibt eine Sammlung der Version aller Dokumente in der Datenbank an, für die ein Commit ausgeführt wurde und die in Konflikt mit incomingDocument bezüglich der ID-Spalte oder anderer Felder mit eindeutigem Index steht. Diese Dokumente weisen einen anderen rid-Wert als das incomingDocument auf.

Die benutzerdefinierte Prozedur verfügt über Vollzugriff auf den Cosmos DB-Partitionsschlüssel und kann alle Speichervorgänge durchführen, um Konflikte aufzulösen. Wenn die benutzerdefinierte Prozedur die in Konflikt stehende Version nicht committet, löscht das System den Konflikt, und das existingDocument verbleibt im committeten Zustand. Wenn bei der benutzerdefinierten Prozedur ein Fehler auftritt oder diese nicht vorhanden ist, fügt Azure Cosmos DB den Konflikt zum schreibgeschützten Konfliktfeed hinzu, in dem dieser asynchron verarbeitet werden kann, wie unter [Benutzerdefiniert – asynchron]() gezeigt wird. 

### <a name="custom--asynchronous"></a>Benutzerdefiniert – asynchron  

In diesem Modus kann Azure Cosmos DB alle Konflikte (Einfügungen, Ersetzungen und Löschungen) von Commits ausschließen und diese beim schreibgeschützten Konfliktfeed registrieren, damit sie von der Anwendung des Benutzers verzögert aufgelöst werden. Die Anwendung kann Konfliktauflösungen asynchron ausführen und eine Logik anwenden oder auf eine externe Quelle, Anwendung oder einen Dienst verweisen, um den Konflikt aufzulösen.

Um einen Konfliktauflösungsmodus namens „Benutzerdefiniert – asynchron“ zu registrieren, stellen Sie die Sammlung mit der ConflictResolutionPolicy bereit, wie unten dargestellt wird.

```csharp
DocumentCollection manualCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
    Id = "myCollection",
    ConflictResolutionPolicy = new ConflictResolutionPolicy
    {
        Mode = ConflictResolutionMode.Custom
    }
});
```

Um Konflikte im Konfliktfeed zu lesen und zu verarbeiten, implementieren Sie den nachfolgend gezeigten Code. Für die im Konfliktfeed gespeicherten Daten fallen Speicherkosten an. Daher wird empfohlen, die im Konfliktfeed gespeicherten Daten zu löschen, nachdem sie verarbeitet wurden.

```csharp
FeedResponse<Conflict> response = await myClient.ReadConflictFeedAsync(myCollectionUri);
  foreach (Conflict conflict in response)
  {
      switch(conflict.OperationKind)
      {
         case OperationKind.Create:
         //Process Insert Conflicts.
         …
         case OperationKind.Replace:
         //Process Replace Conflicts
         …
         case OperationKind.Delete:
         //Process Delete Conflicts
         …
      }
  //Optionally delete the conflict after processed
  await myClient.DeleteConflictAsync(conflict.SelfLink);
  }
```

> [!NOTE]
> Der Konfliktfeed enthält keinen Listener zum Senden von Benachrichtigungen für die Downstreamverarbeitung wie den Änderungsfeed in Cosmos DB. Sie müssen die Logik implementieren, um den Konfliktfeed abzufragen und zu bestimmen, ob Konflikte vorhanden sind.

## <a name="code-samples"></a>Codebeispiele

Im Folgenden finden Sie Beispielanwendungen, die die Konfliktauflösung für die aufgeführten APIs veranschaulichen. Jedes Beispiel generiert Konflikte in einem Container und veranschaulicht anschließend, wie bei jedem unterstützten Konfliktlösungsmodus Konflikte aufgelöst werden.

|API-Modell  | SDK |Beispiel |
|---------|---------|---------|
|SQL-API    | .NET    |[azure-cosmos-db-sql-dotnet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-sql-dotnet-multi-master)  |
|SQL-API    | Knoten    |[azure-cosmos-js/samples/MultiRegionWrite/](https://github.com/Azure/azure-cosmos-js/tree/master/samples/MultiRegionWrite)  |
|SQL-API    | Java    |[azure-cosmos-db-sql-java-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-sql-java-multi-master)  |
|MongoDB  | .NET    |[azure-cosmos-db-mongodb-dotnet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-multi-master)   |
|Tabellen-API  | .NET    |[azure-cosmos-db-table-dotnet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-multi-master)       |
|Gremlin-API | .NET | [azure-cosmos-db-gremlin-dontnet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-gremlin-dontnet-multi-master)|

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Konfliktkategorien und Konfliktauflösungsmodi für Azure Cosmos DB kennengelernt. Sehen Sie sich als Nächstes folgende Artikel an:

* [Verwendung von Azure Cosmos DB-Multimaster mit Open Source-basierten NoSQL-Datenbanken](multi-master-oss-nosql.md)
