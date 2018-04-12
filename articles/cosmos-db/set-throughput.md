---
title: Bereitstellen von Durchsatz für Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie bereitgestellten Durchsatz für Ihre Azure Cosmos DB-Container, -Sammlungen, -Grafiken und -Tabellen festlegen.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: sngun
ms.openlocfilehash: 0e89b93764f51873d991524a5e226464c224b649
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Festlegen von Durchsatz für Azure Cosmos DB-Container

Sie können den Durchsatz für Ihre Azure Cosmos DB-Container im Azure-Portal oder mithilfe der Clients-SDKs festlegen. 

In der folgenden Tabelle sind die für Container verfügbaren Durchsätze aufgeführt:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Container mit nur einer Partition</strong></p></td>
            <td valign="top"><p><strong>Partitionierte Container</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimaler Durchsatz</p></td>
            <td valign="top"><p>400 Anforderungseinheiten pro Sekunde</p></td>
            <td valign="top"><p>1.000 Anforderungseinheiten pro Sekunde</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximaler Durchsatz</p></td>
            <td valign="top"><p>10.000 Anforderungseinheiten pro Sekunde</p></td>
            <td valign="top"><p>Unbegrenzt</p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>Festlegen des Durchsatzes mithilfe des Azure-Portals

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) in einem neuen Fenster.
2. Klicken Sie in der linken Leiste auf **Azure Cosmos DB**, oder klicken Sie unten auf **Alle Dienste**, scrollen Sie zu **Datenbanken**, und klicken Sie dann auf **Azure Cosmos DB**.
3. Wählen Sie Ihr Cosmos DB-Konto aus.
4. Klicken Sie im neuen Fenster im Navigationsmenü auf **Daten-Explorer**.
5. Erweitern Sie im neuen Fenster die Datenbank und den Container, und klicken Sie dann auf **Skalierungseinstellungen**.
6. Geben Sie im neuen Fenster den neuen Durchsatzwert im Feld **Durchsatz** ein, und klicken Sie dann auf **Speichern**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-net"></a>So legen Sie den Durchsatz mithilfe der SQL-API für .NET fest

Der folgende Codeausschnitt ruft den aktuellen Durchsatz ab und ändert ihn in 500 RU/s. Das vollständige Codebeispiel finden Sie auf GitHub im Projekt [CollectionManagement](https://github.com/Azure/azure-documentdb-dotnet/blob/95521ff51ade486bb899d6913880995beaff58ce/samples/code-samples/CollectionManagement/Program.cs#L188-L216).

```csharp
// Fetch the offer of the collection whose throughput needs to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 500 request units per second
offer = new OfferV2(offer, 500);

// Now persist these changes to the collection by replacing the original offer resource
await client.ReplaceOfferAsync(offer);
```

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>So legen Sie den Durchsatz mithilfe der SQL-API für Java fest

Der folgende Codeausschnitt ruft den aktuellen Durchsatz ab und ändert ihn in 500 RU/s. Ein umfassendes Codebeispiel finden Sie auf GitHub in der [OfferCrudSamples.java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java)-Datei. 

```Java
// find offer associated with this collection
Iterator < Offer > it = client.queryOffers(
    String.format("SELECT * FROM r where r.offerResourceId = '%s'", collectionResourceId), null).getQueryIterator();
assertThat(it.hasNext(), equalTo(true));

Offer offer = it.next();
assertThat(offer.getString("offerResourceId"), equalTo(collectionResourceId));
assertThat(offer.getContent().getInt("offerThroughput"), equalTo(throughput));

// update the offer
int newThroughput = 500;
offer.getContent().put("offerThroughput", newThroughput);
client.replaceOffer(offer);
```

## <a name="throughput-faq"></a>Häufig gestellte Fragen zum Durchsatz

**Kann der Durchsatz auf unter 400 RU/s festgelegt werden?**

400 RU/s ist der in Cosmos DB verfügbare minimale Durchsatz für Container mit nur einer Partition (1.000 RU/s ist der minimale Durchsatz für partitionierte Container). Die Anforderungseinheiten werden in Intervallen von 100 RU/s festgelegt, der Durchsatz kann jedoch nicht auf 100 RU/s oder einen anderen Wert unter 400 RU/s festgelegt werden. Wenn Sie nach einer kostengünstigen Methode zum Entwickeln und Testen von Cosmos DB suchen, können Sie den kostenlosen [Azure Cosmos DB-Emulator](local-emulator.md) verwenden, der lokal kostenfrei bereitgestellt werden kann. 

**Wie kann ich den Durchsatz mit der MongoDB-API festlegen?**

Es gibt keine MongoDB-API-Erweiterung zum Festlegen des Durchsatzes. Es wird empfohlen, die SQL-API zu verwenden, wie unter [So legen Sie den Durchsatz mithilfe der SQL-API für .NET fest](#set-throughput-sdk) beschrieben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung und zur weltweiten Skalierung mit Cosmos DB finden Sie unter [Partitionieren und Skalieren von Daten in Cosmos DB](partition-data.md).
