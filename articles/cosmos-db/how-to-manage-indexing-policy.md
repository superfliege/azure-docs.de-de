---
title: Verwalten von Datenbankkonten in Azure Cosmos DB
description: Verwalten von Datenbankkonten in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: 97fb5c2558d55b3f80f2e771971faa109a930c5f
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51626455"
---
# <a name="manage-indexing-in-azure-cosmos-db"></a>Verwalten der Indizierung in Azure Cosmos DB

In Azure Cosmos DB können Sie festlegen, ob ein Container automatisch alle Elemente indizieren soll. Standardmäßig werden alle Elemente in einem Azure Cosmos-Container automatisch indiziert, aber Sie können die automatische Indizierung deaktivieren. Wenn die Indizierung deaktiviert ist, kann auf die Elemente nur über seiteninterne Links oder über Abfragen mithilfe der ID des Elements (etwa der Dokument-ID) zugegriffen werden. Sie können explizit anfordern, dass Ergebnisse ohne Index bereitgestellt werden. Dabei übergeben Sie den Header **x-ms-documentdb-enable-scan** in der REST-API oder die Anforderungsoption **EnableScanInQuery** mithilfe des .NET SDK.

Ist die automatische Indizierung deaktiviert, können Sie bestimmte Dokumente weiterhin zum Index hinzufügen. Umgekehrt können Sie die automatische Indizierung aktiviert lassen und bestimmte Elemente einzeln ausschließen. Konfigurationen mit aktivierter/deaktivierter Indizierung sind hilfreich, wenn eine Teilmenge der Elemente abgefragt werden muss.  

Schreibdurchsatz und Anforderungseinheiten verhalten sich proportional zur Anzahl der zu indizierenden Werte. Die Anzahl wird durch den eingeschlossenen Satz in der Indizierungsrichtlinie angegeben. Wenn Sie bereits mit Abfragemustern vertraut sind, können Sie explizit die ein-/auszuschließende Teilmenge der Pfade auswählen, um den Schreibdurchsatz zu erhöhen.

## <a name="manage-indexing-using-azure-portal"></a>Verwalten der Indizierung über das Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Erstellen Sie ein neues Azure Cosmos-Konto, oder wählen Sie ein bereits vorhandenes Konto aus.

3. Öffnen Sie den Bereich **Daten-Explorer**.

4. Wählen Sie einen vorhandenen Container aus, erweitern Sie ihn, und ändern Sie die folgenden Werte:

   * Öffnen Sie das Fenster **Scale & Settings** (Skalierung und Einstellungen).
   * Ändern Sie die Angabe für **indexingMode** von *consistent* in *none*. Alternativ können Sie bestimmte Pfade für die Indizierung einschließen oder ausschließen.
   * Klicken Sie zum Speichern der Änderungen auf **OK**.

   ![Verwalten der Indizierung über das Azure-Portal](./media/how-to-manage-indexing/how-to-manage-indexing-portal.png)

## <a name="manage-indexing-using-azure-sdks"></a>Verwalten der Indizierung über Azure SDKs

### <a id="dotnet"></a>.NET SDK

Im folgenden Beispiel wird das explizite Einbeziehen eines Elements mithilfe des [SQL API .NET SDK](sql-api-sdk-dotnet.md) und der [RequestOptions.IndexingDirective](/dotnet/api/microsoft.azure.documents.client.requestoptions.indexingdirective)-Eigenschaft veranschaulicht.

```csharp
// To override the default behavior to exclude (or include) a document in indexing,
// use the RequestOptions.IndexingDirective property.
client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new { id = "myDocumentId", isRegistered = true },
    new RequestOptions { IndexingDirective = IndexingDirective.Include });
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Indizierung erhalten Sie in den folgenden Artikeln:

* [Indexing in Azure Cosmos DB](index-overview.md) (Indizierung in Azure Cosmos DB)
* [Indexing policy in Azure Cosmos DB](index-policy.md) (Indizierungsrichtlinie in Azure Cosmos DB)
* [Index types in Azure Cosmos DB](index-types.md) (Indextypen in Azure Cosmos DB)
* [Index paths in Azure Cosmos DB](index-paths.md) (Indexpfade in Azure Cosmos DB)
