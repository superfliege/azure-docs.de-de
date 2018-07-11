---
title: Eindeutige Schlüssel in Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie, wie eindeutige Schlüssel in der Azure Cosmos DB-Datenbank verwendet werden.
services: cosmos-db
keywords: Einschränkung auf eindeutige Schlüssel, Verstoß gegen Einschränkung auf eindeutige Schlüssel
author: rafats
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: rafats
ms.openlocfilehash: d12109efbb157b1e0c15b1a4c0d005fa98c44858
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261099"
---
# <a name="unique-keys-in-azure-cosmos-db"></a>Eindeutige Schlüssel in Azure Cosmos DB

Eindeutige Schlüssel bieten Entwicklern die Möglichkeit, ihrer Datenbank eine zusätzliche Datenintegritätsebene hinzuzufügen. Durch das Erstellen eine Richtlinie für eindeutige Schlüssel beim Erstellen eines Containers wird die Eindeutigkeit von einem oder mehreren Werten pro [Partitionsschlüssel](partition-data.md) gewährleistet. Nachdem ein Container mit einer Richtlinie für eindeutige Schlüssel erstellt wurde, ist keine Erstellung neuer oder aktualisierter Elemente mit Werten möglich, die von der Einschränkung auf eindeutige Schlüssel festgelegte Werte duplizieren.   

> [!NOTE]
> Eindeutige Schlüssel werden von den aktuellen Versionen der [.NET](sql-api-sdk-dotnet.md)- und [.NET Core](sql-api-sdk-dotnet-core.md)-SQL SDKs sowie der [MongoDB-API](mongodb-feature-support.md#unique-indexes) unterstützt. Die Tabellen-API und die Graph-API unterstützen derzeit keine eindeutigen Schlüssel. 
> 
>

## <a name="use-case"></a>Anwendungsfall

Sehen wir uns als Beispiel an, wie eine Benutzerdatenbank, die mit einer [sozialen Anwendung](use-cases.md#web-and-mobile-applications) verknüpft ist, von einer Richtlinie für eindeutige Schlüssel für E-Mail-Adressen profitieren kann. Indem Sie die E-Mail-Adresse des Benutzers zu einem eindeutigen Schlüssel machen, stellen Sie sicher, dass in jedem Datensatz eine eindeutige E-Mail-Adresse vorhanden ist und keine neuen Datensätze mit doppelten E-Mail-Adressen erstellt werden können. 

Wenn Sie Benutzer mehrere Datensätze mit derselben E-Mail-Adresse erstellen können sollen, nicht jedoch mit derselben Kombination aus Vorname, Nachname und E-Mail-Adresse, können Sie der Richtlinie für eindeutige Schlüssel andere Pfade hinzufügen. Anstatt eines eindeutigen Schlüssels, der nur auf einer E-Mail-Adresse basiert, können Sie also einen eindeutigen Schlüssel erstellen, der eine Kombination aus Vorname, Nachname und E-Mail ist. In diesem Fall ist jede eindeutige Kombination der drei Pfade zulässig, sodass die Datenbank Elemente mit den folgenden Pfadwerten enthalten kann. Jeder dieser Datensätze entspricht der Richtlinie für eindeutige Schlüssel.  

**Zulässige Werte für die eindeutigen Schlüssel „firstName“, „lastName“ und „email“**

|Vorname|Nachname|E-Mail-Adresse|
|---|---|---|
|Helga|Kohler|gaby@contoso.com |
|Helga|Kohler|gaby@fabrikam.com|
|Simon|Kohler|gaby@fabrikam.com|
|    |Kohler|gaby@fabrikam.com|
|    |       |gaby@fabraikam.com|

Beim Versuch, einen anderen Datensatz mit einer der in der obigen Tabelle aufgeführten Kombinationen einzufügen, würden Sie die Fehlermeldung erhalten, dass die Einschränkung auf eindeutige Schlüssel nicht erfüllt ist. Azure CosmosDB gibt einen Fehler wie „Ressource mit der angegebenen ID oder dem angegebenen Namen ist bereits vorhanden“ oder „Ressource mit der angegebenen ID, dem angegebenen Namen oder dem eindeutigen Index ist bereits vorhanden“ zurück. 

## <a name="using-unique-keys"></a>Verwendung eindeutiger Schlüssel

Eindeutige Schlüssel müssen beim Erstellen des Containers definiert werden, und der eindeutige Schlüssel ist auf den Partitionsschlüssel beschränkt. Um das vorherige Beispiel weiterzuführen, könnten Sie die Datensätze aus der Tabelle in jeder Partition duplizieren, wenn Sie basierend auf der Postleitzahl partitionieren.

Sie können vorhandene Container nicht so aktualisieren, dass sie eindeutige Schlüssel verwenden.

Nachdem ein Container mit einer Richtlinie für eindeutige Schlüssel erstellt wurde, kann die Richtlinie nicht geändert werden, es sei denn, Sie erstellen den Container neu. Wenn Sie für vorhandene Daten eindeutige Schlüssel implementieren möchten, erstellen Sie den neuen Container und verwenden dann das entsprechende Datenmigrationstool, um die Daten in den neuen Container zu verschieben. Verwenden Sie für SQL-Container das [Datenmigrationstool](import-data.md). Für MongoDB-Container verwenden Sie [„mongoimport.exe“ oder „mongorestore.exe“](mongodb-migrate.md).

Jeder eindeutige Schlüssel kann höchstens 16 Pfadwerte (z. B. „/firstName“, „/lastName“, „/address/zipCode“ usw.) enthalten. 

Jede Richtlinie für eindeutige Schlüssel darf höchstens zehn Einschränkungen auf eindeutige Schlüssel oder Kombinationen enthalten, und die kombinierten Pfade für alle Eigenschaften eindeutiger Indizes dürfen 60 Zeichen nicht überschreiten. Das vorherige Beispiel, in dem Vorname, Nachname und E-Mail-Adresse verwendet werden, ist also nur eine Einschränkung und verwendet drei der 16 verfügbaren möglichen Pfade. 

Gebühren für Anforderungseinheiten für das Erstellen, Aktualisieren und Löschen eines Elements sind geringfügig höher, wenn eine Richtlinie für eindeutige Schlüssel für den Container gilt. 

Eindeutige Schlüssel mit geringer Dichte werden nicht unterstützt. Wenn Werte für einige eindeutige Pfade fehlen, werden sie als spezieller NULL-Wert behandelt, für den die Eindeutigkeitseinschränkung gilt.

## <a name="sql-api-sample"></a>SQL-API-Beispiel

Im folgenden Codebeispiel wird die Erstellung eines neuen SQL-Containers mit zwei Einschränkungen auf eindeutige Schlüssel veranschaulicht. Die erste Einschränkung ist die firstName- lastName-, email-Einschränkung, die im vorangehenden Beispiel beschrieben wurde. Die zweite Einschränkung betrifft die Adresse und Postleitzahl des Benutzers. Eine JSON-Beispieldatei, die die Pfade in dieser Richtlinie für eindeutige Schlüssel verwendet, folgt dem Codebeispiel. 

```csharp
// Create a collection with two separate UniqueKeys, one compound key for /firstName, /lastName,
// and /email, and another for /address/zipCode.
private static async Task CreateCollectionIfNotExistsAsync(string dataBase, string collection)
{
    try
    {
        await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(dataBase, collection));
    }
    catch (DocumentClientException e)
    {
        if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
        {
            DocumentCollection myCollection = new DocumentCollection();
            myCollection.Id = collection;
            myCollection.PartitionKey.Paths.Add("/pk");
            myCollection.UniqueKeyPolicy = new UniqueKeyPolicy
            {
                UniqueKeys =
                new Collection<UniqueKey>
                {
                    new UniqueKey { Paths = new Collection<string> { "/firstName" , "/lastName" , "/email" }}
                    new UniqueKey { Paths = new Collection<string> { "/address/zipcode" } },
          }
            };
            await client.CreateDocumentCollectionAsync(
                UriFactory.CreateDatabaseUri(dataBase),
                myCollection,
                new RequestOptions { OfferThroughput = 2500 });
        }
        else
        {
            throw;
        }
    }
```

JSON-Beispieldokument.

```json
{
    "id": "1",
    "pk": "1234",
    "firstName": "Gaby",
    "lastName": "Duperre",
    "email": "gaby@contoso.com",
    "address": 
        {            
            "line1": "100 Some Street",
            "line2": "Unit 1",
            "city": "Seattle",
            "state": "WA",
            "zipcode": 98012
        }
    
}
```
> [!NOTE]
> Beachten Sie, dass beim Namen des eindeutigen Schlüssels Groß-/Kleinschreibung unterschieden wird. Wie im Beispiel oben gezeigt, ist der eindeutige Name für „address“ (Adresse) und „zipcode“ (Postleitzahl) festgelegt. Wenn Ihre Daten „ZipCode“ enthalten, wird „null“ in den eindeutigen Schlüssel eingefügt, da „zipcode“ nicht gleich „ZipCode“ ist. Und wegen dieser Groß-/Kleinschreibung können alle anderen Datensätze mit „ZipCode“ nicht eingefügt werden, da eine doppelte „Null“ gegen die Einschränkung auf eindeutige Schlüssel verstößt.

## <a name="mongodb-api-sample"></a>Beispiel für MongoDB-API

Im folgenden Befehlsbeispiel wird die Erstellung eines eindeutigen Index für die Felder „firstName“, „lastName“ und „email“ der Auflistung des Benutzers für die MongoDB-API veranschaulicht. Dadurch wird die Eindeutigkeit für eine Kombination aus allen drei Feldern für alle Dokumente in der Auflistung sichergestellt. Für MongoDB-API-Auflistungen wird der eindeutige Index nach dem Erstellen, aber vor dem Auffüllen der Auflistung erstellt.

```
db.users.createIndex( { firstName: 1, lastName: 1, email: 1 }, { unique: true } )
```
## <a name="configure-unique-keys-by-using-azure-portal"></a>Konfigurieren eindeutiger Schlüssel im Azure-Portal

In den obigen Abschnitten finden Sie Codebeispiele, die zeigen, wie Sie Einschränkung auf eindeutige Schlüssel definieren können, wenn eine Sammlung mit der SQL-API oder MongoDB-API erstellt wird. Es ist jedoch auch möglich, eindeutige Schlüssel zu definieren, wenn Sie eine Sammlung auf der Webbenutzeroberfläche im Azure-Portal erstellen. 

- Navigieren Sie in Ihrem Cosmos DB-Konto zum **Daten-Explorer**.
- Klicken Sie auf **Neue Sammlung**.
- Im Abschnitt „Eindeutige Schlüssel“ können Sie die gewünschten Einschränkungen auf eindeutige Schlüssel hinzufügen, indem Sie auf **Eindeutigen Schlüssel hinzufügen** klicken.

![Definieren eindeutiger Schlüssel im Daten-Explorer](./media/unique-keys/unique-keys-azure-portal.png)

- Wenn Sie eine Einschränkung auf eindeutige Schlüssel für den Pfad „lastName“ erstellen möchten, fügen Sie `/lastName` hinzu.
- Wenn Sie eine Einschränkung auf eindeutige Schlüssel für die Kombination aus „lastName“ und „firstName“ erstellen möchten, fügen Sie `/lastName,/firstName` hinzu.

Wenn Sie fertig sind, klicken Sie auf **OK**, um die Sammlung zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie eindeutige Schlüssel für Elemente in einer Datenbank erstellt werden. Wenn Sie zum ersten Mal einen Container erstellen, lesen Sie [Partitionierung von Daten in Azure Cosmos DB](partition-data.md), da eindeutige Schlüssel und Partitionsschlüssel voneinander abhängig sind. 


