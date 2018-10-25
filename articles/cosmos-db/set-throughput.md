---
title: Bereitstellen von Durchsatz für Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie bereitgestellten Durchsatz für Ihre Azure Cosmos DB-Container, -Sammlungen, -Grafiken und -Tabellen festlegen.
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: andrl
ms.openlocfilehash: 2280a3f6b2a67d392a109a5294e1509bcc804bc3
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48869923"
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers-and-database"></a>Festlegen und Abrufen des Durchsatzes für Azure Cosmos DB-Container und -Datenbank

Sie können den Durchsatz für einen Azure Cosmos DB-Container oder eine Gruppe von Containern mithilfe des Azure-Portals oder der Client-SDKs festlegen. Dieser Artikel beschreibt die Schritte zum Konfigurieren des Durchsatzes in verschiedenen Granularitäten für ein Azure Cosmos DB-Konto.

## <a name="provision-throughput-by-using-azure-portal"></a>Bereitstellen des Durchsatzes mithilfe des Azure-Portals

### <a name="provision-throughput-for-a-container-collectiongraphtable"></a>Bereitstellen des Durchsatzes für einen Container (Sammlung/Diagramm/Tabelle)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.  
2. Wählen Sie in der linken Navigationsleiste **Alle Ressourcen**, und suchen Sie nach Ihrem Azure Cosmos DB-Konto.  
3. Sie können den Durchsatz während der Erstellung eines Containers (Sammlung, Diagramm, Tabelle) konfigurieren oder den Durchsatz für einen vorhandenen Container aktualisieren.  
4. Um den Durchsatz beim Erstellen eines Containers zuzuweisen, öffnen Sie das Blatt **Daten-Explorer** und wählen **Neue Sammlung** („Neues Diagramm“, „Neue Tabelle“ für andere APIs) aus.  
5. Füllen Sie das Formular in Blatt **Sammlung hinzufügen** aus. Die Felder in diesem Blatt sind in der folgenden Tabelle beschrieben:  

   |**Einstellung**  |**Beschreibung**  |
   |---------|---------|
   |Datenbank-ID  |  Geben Sie einen eindeutigen Namen zur Identifizierung der Datenbank an. Eine Datenbank ist ein logischer Container für mindestens eine Sammlung. Datenbanknamen müssen zwischen 1 und 255 Zeichen lang sein und dürfen weder /, \\, #, ? noch nachgestellte Leerzeichen enthalten. |
   |Sammlungs-ID  | Geben Sie einen eindeutigen Namen zur Identifizierung der Sammlung an. Für Sammlungs-IDs gelten dieselben Zeichenanforderungen wie für Datenbanknamen. |
   |Speicherkapazität   | Dieser Wert gibt die Speicherkapazität der Datenbank an. Bei der Bereitstellung des Durchsatzes für eine einzelne Sammlung kann die Speicherkapazität **Fest (10 GB)** oder **Unbegrenzt** sein. Eine unbegrenzte Speicherkapazität setzt voraus, dass Sie einen Partitionsschlüssel für Ihre Daten festlegen.  |
   |Throughput   | Jede Sammlung und Datenbank kann einen Durchsatz in Anforderungseinheiten pro Sekunde haben.  Und eine Sammlung kann eine feste oder unbegrenzte Speicherkapazität haben. |

6. Nachdem Sie Werte für diese Felder eingegeben haben, wählen Sie **OK**, um die Einstellungen zu speichern.  

   ![Festlegen des Durchsatzes für eine Sammlung](./media/set-throughput/set-throughput-for-container.png)

7. Um den Durchsatz für einen vorhandenen Container zu aktualisieren, erweitern Sie Ihre Datenbank und den Container, und klicken Sie dann auf **Einstellungen**. Geben Sie im neuen Fenster den neuen Durchsatzwert ein, und wählen Sie **Speichern**.  

   ![Aktualisieren des Durchsatzes für eine Sammlung](./media/set-throughput/update-throughput-for-container.png)

### <a name="provision-throughput-for-a-set-of-containers-or-at-the-database-level"></a>Bereitstellen des Durchsatzes für eine Gruppe von Containern oder auf Datenbankebene

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.  
2. Wählen Sie in der linken Navigationsleiste **Alle Ressourcen**, und suchen Sie nach Ihrem Azure Cosmos DB-Konto.  
3. Sie können den Durchsatz beim Erstellen einer Datenbank konfigurieren oder für eine vorhandene Datenbank aktualisieren.  
4. Um den Durchsatz während der Erstellung einer Datenbank zuzuweisen, öffnen Sie das Blatt **Daten-Explorer** und wählen **Neue Datenbank** aus.  
5. Füllen Sie den Wert **Datenbank-ID** aus, aktivieren Sie die Option **Durchsatz bereitstellen**, und konfigurieren Sie den Durchsatzwert.  

   ![Festlegen des Durchsatzes mit neuer Datenbankoption](./media/set-throughput/set-throughput-with-new-database-option.png)

6. Um den Durchsatz für eine vorhandene Datenbank zu aktualisieren, erweitern Sie Ihre Datenbank und den Container, und klicken Sie dann auf **Skalieren**. Geben Sie im neuen Fenster den neuen Durchsatzwert ein, und wählen Sie **Speichern**.  

   ![Aktualisieren des Durchsatzes für eine Datenbank](./media/set-throughput/update-throughput-for-database.png)

### <a name="provision-throughput-for-a-set-of-containers-as-well-as-for-an-individual-container-in-a-database"></a>Bereitstellen des Durchsatzes für eine Gruppe von Containern sowie für einen einzelnen Container in einer Datenbank

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.  
2. Wählen Sie in der linken Navigationsleiste **Alle Ressourcen**, und suchen Sie nach Ihrem Azure Cosmos DB-Konto.  
3. Erstellen Sie eine Datenbank, und weisen Sie dieser einen Durchsatz zu. Öffnen Sie das Blatt **Daten-Explorer**, und wählen Sie **Neue Datenbank** aus.  
4. Füllen Sie den Wert **Datenbank-ID** aus, aktivieren Sie die Option **Durchsatz bereitstellen**, und konfigurieren Sie den Durchsatzwert.  

   ![Festlegen des Durchsatzes mit neuer Datenbankoption](./media/set-throughput/set-throughput-with-new-database-option.png)

5. Als Nächstes erstellen Sie eine Sammlung in der Datenbank, die Sie im obigen Schritt erstellt haben. Um eine Sammlung zu erstellen, klicken Sie mit der rechten Maustaste auf die Datenbank und wählen **Neue Sammlung** aus.  

6. Geben Sie im Blatt **Sammlung hinzufügen** einen Namen für die Sammlung und einen Partitionsschlüssel ein. Optional können Sie den Durchsatz für diesen spezifischen Container bereitstellen. Wenn Sie keinen Durchsatzwert zuweisen, wird der Durchsatz, der der Datenbank zugewiesen wurde, mit der Sammlung geteilt.  

   ![Optionales Festlegen des Durchsatzes für den Container](./media/set-throughput/optionally-set-throughput-for-the-container.png)

## <a name="considerations-when-provisioning-throughput"></a>Überlegungen bei der Bereitstellung des Durchsatzes

Nachfolgend finden Sie einige Überlegungen, die Ihnen helfen, sich für eine Strategie zur Reservierung von Durchsatz zu entscheiden.

### <a name="considerations-when-provisioning-throughput-at-the-database-level"></a>Überlegungen zur Bereitstellung des Durchsatzes auf Datenbankebene

Ziehen Sie in den folgenden Fällen die Bereitstellung des Durchsatzes auf Datenbankebene (d.h. für eine Gruppe von Containern) in Betracht:

* Wenn Sie über mindestens ein Dutzend Container verfügen, bei denen einige oder alle den Durchsatz gemeinsam nutzen könnten.  

* Wenn Sie von einer Einzelmandantendatenbank, die auf IaaS-gehosteten VMs oder lokalen Instanzen (z.B. NoSQL oder relationale Datenbanken) ausgeführt wird, zu Azure Cosmos DB migrieren und über viele Container verfügen.  

* Wenn Sie ungeplante Auslastungsspitzen berücksichtigen möchten, indem Sie zusammengelegten Durchsatz auf Datenbankebene verwenden.  

* Anstatt den Durchsatz auf einen einzelnen Container festzulegen, sind Sie daran interessiert, den Gesamtdurchsatz über eine Gruppe von Containern innerhalb der Datenbank zu verteilen.

### <a name="considerations-when-provisioning-throughput-at-the-container-level"></a>Überlegungen zur Bereitstellung des Durchsatzes auf Containerebene

Ziehen Sie in den folgenden Fällen die Bereitstellung des Durchsatzes für einen einzelnen Container in Betracht:

* Wenn Sie über eine geringe Anzahl von Azure Cosmos DB-Containern verfügen.  

* Wenn Sie einen garantierten Durchsatz für einen bestimmten Container unter Berücksichtigung der SLA benötigen.

<a id="set-throughput-sdk"></a>

## <a name="set-throughput-by-using-sql-api-for-net"></a>Festlegen des Durchsatzes mithilfe der SQL-API für .NET

### <a name="set-throughput-at-the-container-level"></a>Festlegen des Durchsatzes auf Containerebene
Hier sehen Sie einen Codeausschnitt zum Erstellen eines Containers mit 3.000 Anforderungseinheiten pro Sekunde für einen einzelnen Container unter Verwendung der .NET SDK der SQL-API:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

### <a name="set-throughput-for-a-set-of-containers-at-the-database-level"></a>Festlegen des Durchsatzes für eine Gruppe von Containern auf Datenbankebene

Hier sehen Sie einen Codeausschnitt zum Bereitstellen von 100.000 Anforderungseinheiten pro Sekunde für eine Gruppe von Containern unter Verwendung der .NET SDK der SQL-API:

```csharp
// Provision 100,000 RU/sec at the database level. 
// sharedCollection1 and sharedCollection2 will share the 100,000 RU/sec from the parent database
// dedicatedCollection will have its own dedicated 4,000 RU/sec, independant of the 100,000 RU/sec provisioned from the parent database
Database database = await client.CreateDatabaseAsync(new Database { Id = "myDb" }, new RequestOptions { OfferThroughput = 100000 });

DocumentCollection sharedCollection1 = new DocumentCollection();
sharedCollection1.Id = "sharedCollection1";
sharedCollection1.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, sharedCollection1, new RequestOptions())

DocumentCollection sharedCollection2 = new DocumentCollection();
sharedCollection2.Id = "sharedCollection2";
sharedCollection2.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, sharedCollection2, new RequestOptions())

DocumentCollection dedicatedCollection = new DocumentCollection();
dedicatedCollection.Id = "dedicatedCollection";
dedicatedCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, dedicatedCollection, new RequestOptions { OfferThroughput = 4000 )
```

Azure Cosmos DB wird mit einem Reservierungsmodell für den Durchsatz ausgeführt. Ihnen wird also der *reservierte* Durchsatz berechnet, unabhängig davon, wie viel von diesem Durchsatz aktiv *verwendet* wird. Sie können die Anzahl reservierter RUs über SDKs oder über das [Azure-Portal](https://portal.azure.com) ganz einfach zentral hoch- oder herunterskalieren, wenn sich die Auslastung, die Daten und die Nutzungsmuster Ihrer Anwendung ändern.

Jeder Container bzw. jede Gruppe von Containern ist einer `Offer`-Ressource in Azure Cosmos DB zugeordnet, die Metadaten zu dem bereitgestellten Durchsatz enthält. Sie können den reservierten Durchsatz ändern, indem Sie die entsprechende Angebotsressource für einen Container suchen und mit dem neuen Durchsatzwert aktualisieren. Hier sehen Sie einen Codeausschnitt zum Ändern des Durchsatzes eines Containers in 5.000 Anforderungseinheiten pro Sekunde mithilfe des .NET SDK. Nach dem Ändern des Durchsatzes sollten Sie alle vorhandenen Azure-Portalfenster aktualisieren, sodass sie den geänderten Durchsatz anzeigen. 

```csharp
// Fetch the resource to be updated
// For a updating throughput for a set of containers, replace the collection's self link with the database's self link
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set the throughput to 5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

Die Durchsatzänderung hat keine Auswirkungen auf die Verfügbarkeit des Containers oder der Gruppe von Containern. In der Regel wird der neue reservierte Durchsatz innerhalb von Sekunden nach der Anwendung des neuen Durchsatzes wirksam.

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

## <a name="get-the-request-charge-using-cassandra-api"></a>Abrufen der Anforderungsgebühren mithilfe der Cassandra-API 

Die Cassandra-API unterstützt eine Methode, um zusätzliche Informationen zu den Gebühren von Anforderungseinheiten für einen bestimmten Vorgang anzuzeigen. Die Gebühren für RU/s für den Einfügevorgang können beispielsweise wie folgt abgerufen werden:

```csharp
var insertResult = await tableInsertStatement.ExecuteAsync();
 foreach (string key in insertResult.Info.IncomingPayload)
        {
            byte[] valueInBytes = customPayload[key];
            string value = Encoding.UTF8.GetString(valueInBytes);
            Console.WriteLine($“CustomPayload:  {key}: {value}”);
        }
```


## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>Abrufen des Durchsatzes mit MongoDB-API-Portalmetriken

Die einfachste Möglichkeit, eine recht genaue Schätzung der Gebühren für Anforderungseinheiten für Ihre MongoDB-API-Datenbank zu erhalten, ist die Verwendung der Metriken im [Azure-Portal](https://portal.azure.com). Mit den Diagrammen *Anzahl von Anforderungen* und *Anforderungsgebühr* können Sie abschätzen, wie viele Anforderungseinheiten von jedem Vorgang verbraucht werden und wie viele Einheiten in Relation der Vorgänge zueinander verbraucht werden.

![MongoDB-API-Portalmetriken][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> Überschreiten von Grenzwerten für den reservierten Durchsatz in der MongoDB-API
Anwendungen, die den bereitgestellten Durchsatz für einen Container oder eine Gruppe von Containern überschreiten, werden so lange begrenzt, bis die Nutzungsrate unter den bereitgestellten Durchsatz sinkt. Wenn eine Begrenzung eintritt, beendet das Back-End die Anforderung mit einem `16500`-Fehlercode: `Too Many Requests`. Die MongoDB-API versucht standardmäßig automatisch bis zu 10-mal, eine Anforderung erneut zu senden, bevor der Fehlercode `Too Many Requests` zurückgegeben wird. Wenn Sie zu viele Fehlercodes des Typs `Too Many Requests` erhalten, sollten Sie in Betracht ziehen, das Wiederholungsverhalten in die Fehlerbehandlungsroutinen Ihrer Anwendung aufzunehmen oder den [bereitgestellten Durchsatz für den Container](set-throughput.md) zu erhöhen.

## <a id="GetLastRequestStatistics"></a>Abrufen der Anforderungsgebühr mit dem MongoDB-API-Befehl „GetLastRequestStatistics“

Die MongoDB-API unterstützt den benutzerdefinierten Befehl *getLastRequestStatistics*, um die Anforderungsgebühren für einen bestimmten Vorgang abzurufen.

Führen Sie z.B. in der Mongo-Shell den Vorgang aus, für den Sie die Anforderungsgebühr überprüfen möchten.
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

Eine Methode zum Abschätzen des von der Anwendung benötigten Durchsatzes besteht darin, typische Vorgänge mit einem repräsentativen, von Ihrer Anwendung verwendeten Element auszuführen, sich dabei die berechneten Anforderungseinheiten zu notieren und anschließend die Anzahl von Vorgängen zu schätzen, die erwartungsgemäß pro Sekunde ausgeführt werden.

> [!NOTE]
> Wenn sich die Elementtypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, erfassen Sie für jeden typischen *Elementtyp* jeweils die berechneten Anforderungseinheiten des jeweiligen Vorgangs.
> 
> 

## <a name="throughput-faq"></a>Häufig gestellte Fragen zum Durchsatz

**Kann der Durchsatz auf unter 400 RU/s festgelegt werden?**

400 RU/s ist der in Cosmos DB verfügbare minimale Durchsatz für Container mit nur einer Partition (1.000 RU/s ist der minimale Durchsatz für partitionierte Container). Die Anforderungseinheiten werden in Intervallen von 100 RU/s festgelegt, der Durchsatz kann jedoch nicht auf 100 RU/s oder einen anderen Wert unter 400 RU/s festgelegt werden. Wenn Sie nach einer kostengünstigen Methode zum Entwickeln und Testen von Cosmos DB suchen, können Sie den kostenlosen [Azure Cosmos DB-Emulator](local-emulator.md) verwenden, der lokal kostenfrei bereitgestellt werden kann. 

**Wie kann ich den Durchsatz mit der MongoDB-API festlegen?**

Es gibt keine MongoDB-API-Erweiterung zum Festlegen des Durchsatzes. Es wird empfohlen, die SQL-API zu verwenden, wie unter [So legen Sie den Durchsatz mithilfe der SQL-API für .NET fest](#set-throughput-sdk) beschrieben.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Einschätzung von Durchsatz und Anforderungseinheiten finden Sie unter [Anforderungseinheiten in Azure Cosmos DB](request-units.md).

* Weitere Informationen zur Bereitstellung und zur weltweiten Skalierung mit Cosmos DB finden Sie unter [Partitionieren und Skalieren von Daten in Cosmos DB](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png
