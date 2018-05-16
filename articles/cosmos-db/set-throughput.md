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
ms.date: 05/07/2018
ms.author: sngun
ms.openlocfilehash: bede91ed3ffc456740a0eb63ed7a15278e99ebe2
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers"></a>Festlegen und Abrufen des Durchsatzes für Azure Cosmos DB-Container

Sie können den Durchsatz für Ihre Azure Cosmos DB-Container oder eine Gruppe von Containern im Azure-Portal oder mithilfe der Client-SDKs festlegen. 

In der folgenden Tabelle sind die für Container verfügbaren Durchsätze aufgeführt:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Container mit nur einer Partition</strong></p></td>
            <td valign="top"><p><strong>Partitionierte Container</strong></p></td>
            <td valign="top"><p><strong>Gruppe von Containern</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimaler Durchsatz</p></td>
            <td valign="top"><p>400 Anforderungseinheiten pro Sekunde</p></td>
            <td valign="top"><p>1.000 Anforderungseinheiten pro Sekunde</p></td>
            <td valign="top"><p>50.000 Anforderungseinheiten pro Sekunde</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximaler Durchsatz</p></td>
            <td valign="top"><p>10.000 Anforderungseinheiten pro Sekunde</p></td>
            <td valign="top"><p>Unbegrenzt</p></td>
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
// To change the throughput for a set of containers, use the database's selflink instead of the collection's selflink
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
// To change the throughput for a set of containers, use the database's resource id instead of the collection's resource id
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

## <a id="GetLastRequestStatistics"></a>Abrufen des Durchsatzes mit dem MongoDB-API-Befehl „GetLastRequestStatistics“

Die MongoDB-API unterstützt den benutzerdefinierten Befehl *getLastRequestStatistics*, um die Anforderungsgebühren für einen bestimmten Vorgang abzurufen.

Führen Sie z.B. in der Mongo Shell den Vorgang aus, für den Sie die Anforderungsgebühr überprüfen möchten.
```
> db.sample.find()
```

Anschließend führen Sie den Befehl *getLastRequestStatistics* aus.
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

Vor diesem Hintergrund besteht eine Methode zum Abschätzen des von der Anwendung benötigten Durchsatzes darin, typische Vorgänge mit einem repräsentativen, von Ihrer Anwendung verwendeten Element auszuführen, sich dabei die berechneten Anforderungseinheiten zu notieren und anschließend die Anzahl von Vorgängen zu schätzen, die erwartungsgemäß pro Sekunde ausgeführt werden.

> [!NOTE]
> Wenn sich die Elementtypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, erfassen Sie für jeden typischen *Elementtyp* jeweils die berechneten Anforderungseinheiten des jeweiligen Vorgangs.
> 
> 

## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>Abrufen des Durchsatzes mit MongoDB-API-Portalmetriken

Die einfachste Möglichkeit, eine recht genaue Schätzung der Gebühren für Anforderungseinheiten für Ihre MongoDB-API-Datenbank zu erhalten, ist die Verwendung der Metriken im [Azure-Portal](https://portal.azure.com). Mit den Diagrammen *Anzahl von Anforderungen* und *Anforderungsgebühr* können Sie abschätzen, wie viele Anforderungseinheiten von jedem Vorgang verbraucht werden und wie viele Einheiten in Relation der Vorgänge zueinander verbraucht werden.

![MongoDB-API-Portalmetriken][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> Überschreiten von Grenzwerten für den reservierten Durchsatz in der MongoDB-API
Anwendungen, die den bereitgestellten Durchsatz für einen Container oder eine Gruppe von Containern überschreiten, werden so lange begrenzt, bis die Nutzungsrate unter den bereitgestellten Durchsatz sinkt. Wenn eine Begrenzung eintritt, beendet das Back-End die Anforderung präventiv mit einem `16500`-Fehlercode: `Too Many Requests`. Die MongoDB-API versucht standardmäßig automatisch bis zu 10-mal, eine Anforderung erneut zu senden, bevor der Fehlercode `Too Many Requests` zurückgegeben wird. Wenn Sie zu viele Fehlercodes des Typs `Too Many Requests` erhalten, sollten Sie in Betracht ziehen, das Wiederholungsverhalten in die Fehlerbehandlungsroutinen Ihrer Anwendung aufzunehmen oder den [bereitgestellten Durchsatz für den Container](set-throughput.md) zu erhöhen.

## <a name="throughput-faq"></a>Häufig gestellte Fragen zum Durchsatz

**Kann der Durchsatz auf unter 400 RU/s festgelegt werden?**

400 RU/s ist der in Cosmos DB verfügbare minimale Durchsatz für Container mit nur einer Partition (1.000 RU/s ist der minimale Durchsatz für partitionierte Container). Die Anforderungseinheiten werden in Intervallen von 100 RU/s festgelegt, der Durchsatz kann jedoch nicht auf 100 RU/s oder einen anderen Wert unter 400 RU/s festgelegt werden. Wenn Sie nach einer kostengünstigen Methode zum Entwickeln und Testen von Cosmos DB suchen, können Sie den kostenlosen [Azure Cosmos DB-Emulator](local-emulator.md) verwenden, der lokal kostenfrei bereitgestellt werden kann. 

**Wie kann ich den Durchsatz mit der MongoDB-API festlegen?**

Es gibt keine MongoDB-API-Erweiterung zum Festlegen des Durchsatzes. Es wird empfohlen, die SQL-API zu verwenden, wie unter [So legen Sie den Durchsatz mithilfe der SQL-API für .NET fest](#set-throughput-sdk) beschrieben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung und zur weltweiten Skalierung mit Cosmos DB finden Sie unter [Partitionieren und Skalieren von Daten in Cosmos DB](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png