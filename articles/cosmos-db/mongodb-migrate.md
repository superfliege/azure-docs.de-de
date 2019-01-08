---
title: Migrieren von MongoDB-Daten zu Azure Cosmos DB mithilfe von „mongoimport“ und „mongorestore“
description: Erfahren Sie, wie Sie „mongoimport“ und „mongorestore“ zum Importieren von Daten in Cosmos DB verwenden.
keywords: mongoimport, mongorestore
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: rimman
ms.custom: mvc
Customer intent: As a developer, I want to migrate the data from my existing MongoDB to Cosmos DB.
ms.openlocfilehash: 4cd30c7981cd6807113729292db403a80cbddef0
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793749"
---
# <a name="migrate-your-mongodb-data-to-azure-cosmos-db"></a>Migrieren Ihrer MongoDB-Daten zu Azure Cosmos DB

 Dieses Tutorial enthält Anweisungen zum Migrieren von in MongoDB gespeicherten Daten zu Azure Cosmos DB, um die API für MongoDB von Cosmos DB verwenden zu können. Wenn Sie Daten aus MongoDB importieren und diese mit der SQL-API von Azure Cosmos DB verwenden möchten, sollten Sie die Daten mithilfe des [Datenmigrationstools](import-data.md) importieren.

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten eines Migrationsplans.
> * Migrieren von Daten mithilfe von „mongoimport“.
> * Migrieren von Daten mithilfe von „mongorestore“.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Überprüfen und erfüllen Sie die folgenden Voraussetzungen, bevor Sie mit der Migration beginnen.

### <a name="plan-for-the-migration"></a>Planen der Migration

Dieser Abschnitt beschreibt, wie Sie die Datenmigration planen. Wir schätzen die RU-Gebühren, ermitteln die die Latenz zwischen Ihrem Computer und dem Clouddienst und berechnen die Batchgröße und die Anzahl der Einfügeworker.


#### <a name="pre-create-and-scale-your-collections"></a>Voraberstellen und Skalieren Ihrer Sammlungen

Erstellen Sie vorab alle Ihre Sammlungen über das [Azure-Portal](https://portal.azure.com) oder über MongoDB-Treiber und -Tools, bevor Sie mit der Migration mithilfe von „mongoimport“ oder „mongorestore“ beginnen. 

Erhöhen Sie im [Azure-Portal](https://portal.azure.com) den Durchsatz Ihrer Sammlungen für die Migration. Mit einem höheren Durchsatz können Sie eine Ratenbegrenzung vermeiden und Migrationen in kürzerer Zeit durchführen. Sie können den Durchsatz sofort nach der Migration verringern, um Kosten zu sparen.

Zusätzlich zum Bereitstellen von Durchsatz auf Sammlungsebene können Sie auch auf Datenbankebene Durchsatz für eine Reihe von Sammlungen bereitstellen, um den bereitgestellten Durchsatz zu teilen. Sie müssen die Datenbank und die Sammlungen vorab erstellen und für jede Sammlung in der gemeinsamen Durchsatzdatenbank einen Shardschlüssel definieren.

Sie können Shardsammlungen mit Ihrem bevorzugten Tool, Treiber oder SDK erstellen. In diesem Beispiel verwenden wir die Mongo Shell zum Erstellen einer Sammlung mit Shards:

```bash
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Der Befehl gibt die folgenden Ergebnisse zurück:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

#### <a name="calculate-the-approximate-ru-charge-for-a-single-document-write"></a>Berechnen der ungefähren RU-Gebühr für das Schreiben eines einzelnen Dokuments

Stellen Sie über die MongoDB-Shell eine Verbindung mit Ihrem Cosmos-Konto her, das für die Verwendung der API für MongoDB von Cosmos DB konfiguriert ist. Eine Anleitung dazu finden Sie unter [Verbinden einer MongoDB-Anwendung mit Cosmos DB](connect-mongodb-account.md).

Führen Sie dann einen Beispieleinfügebefehl mit einem Ihrer Beispieldokumente aus:
   
```bash
db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })
```
        
Führen Sie den Befehl `db.runCommand({getLastRequestStatistics: 1})`aus.

Sie erhalten eine Antwort ähnlich der folgenden Ausgabe:
     
```bash
globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "insert",
    "RequestCharge": 10,
    "RequestDurationInMilliSeconds": NumberLong(50)
}
```
        
Beachten Sie die Gebühr für die Anforderung.
    
#### <a name="determine-the-latency-from-your-machine-to-cosmos-db"></a>Ermitteln der Latenz zwischen Ihrem Computer und Cosmos DB
    
Aktivieren Sie die ausführliche Protokollierung über die MongoDB-Shell mit dem Befehl `setVerboseShell(true)`.
    
Führen eine Basisabfrage für die Datenbank mit dem Befehl `db.coll.find().limit(1)` aus.

Sie erhalten eine Antwort ähnlich der folgenden Ausgabe:

```bash
Fetched 1 record(s) in 100(ms)
```
        
Entfernen Sie vor der Migration das eingefügte Dokument, um sicherzustellen, dass keine Dokumentduplikate vorhanden sind. Sie können Dokumente mit dem Befehl `db.coll.remove({})` entfernen.

#### <a name="calculate-the-approximate-values-for-the-batchsize-and-numinsertionworkers-properties"></a>Berechnen der ungefähren Werte für die batchSize- und numInsertionWorkers-Eigenschaften

Dividieren Sie für die **batchSize**-Eigenschaft den gesamten bereitgestellten Durchsatz (RUs/Sek.) durch die RUs, die für einen einzelnen Dokumentenschreibvorgang verwendet werden, wie im Abschnitt „Ermitteln der Latenz zwischen Ihrem Computer und Cosmos DB“ erläutert wird. Wenn der berechnete Wert kleiner als oder gleich 24 ist, verwenden Sie diese berechnete Zahl als Wert der Eigenschaft. Wenn der berechnete Wert größer als 24 ist, legen Sie den Wert der Eigenschaft auf 24 fest.
    
Verwenden Sie für den Wert der Eigenschaft **numInsertionWorkers** die folgende Gleichung:

`numInsertionWorkers = (Provisioned RUs throughput * Latency in seconds) / (batchSize * Consumed RUs for a single write)`

Wir können die folgenden Werte verwenden, um einen Wert für die Eigenschaft **numInsertionWorkers** zu berechnen:

| Eigenschaft | Wert |
|--------|-----|
| **batchSize** | 24 |
| Bereitgestellte RUs | 10.000 |
| Latency | 0,100 s |
| Verbrauchte RUs | 10 RUs |
| **numInsertionWorkers** | (10.000 RUs x 0,100 s)/(24 x 10 RUs) = **4,1666** |

Führen Sie den **monogoimport**-Migrationsbefehl aus. Die Befehlsparameter werden weiter unten in diesem Artikel beschrieben.

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
```

Sie können auch den **monogorestore**-Befehl verwenden. Stellen Sie sicher, dass für alle Sammlungen der Durchsatz auf die Anzahl oder über den Wert der Anzahl der in den vorangegangenen Berechnungen verwendeten RUs festgelegt ist.
   
```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
```

### <a name="complete-the-prerequisites"></a>Erfüllen der Voraussetzungen

Nachdem Sie die Migration geplant haben, führen Sie die folgenden Schritte aus: 

* **Abrufen von Beispieldaten**: Stellen Sie sicher, dass Sie über einige Beispieldaten verfügen, bevor Sie die Migration starten. 

* **Erhöhen des Durchsatzes**: Die Dauer Ihrer Datenmigration hängt davon ab, wie viel Durchsatz Sie für eine einzelne Sammlung oder Datenbank bereitstellen. Achten Sie darauf, dass Sie den Durchsatz für größere Datenmigrationen erhöhen. Nachdem Sie die Migration abgeschlossen haben, können Sie den Durchsatz wieder verringern, um Kosten zu sparen. 

* **Aktivieren von SSL**:  Für Cosmos DB gelten strenge Sicherheitsanforderungen und -standards. Achten Sie darauf, SSL für die Interaktion mit Ihrem Cosmos-Konto zu aktivieren. Die Verfahren in diesem Artikel verdeutlichen, wie Sie SSL für die Befehle „mongoimport“ und „mongorestore“ aktivieren.

* **Erstellen von Cosmos DB-Ressourcen**: Bevor Sie mit der Migration beginnen, erstellen Sie zunächst vorab alle Ihre Sammlungen im Azure-Portal. Wenn Sie zu einem Cosmos-Konto migrieren, für das Durchsatz auf Datenbankebene bereitgestellt wird, stellen Sie sicher, dass Sie bei der Erstellung der Sammlungen einen Partitionsschlüssel angeben.

* **Abrufen Ihrer Verbindungszeichenfolge**: Wählen Sie im [Azure-Portal](https://portal.azure.com) den Eintrag **Azure Cosmos DB** auf der linken Seite aus. Wählen Sie unter **Abonnements** den Namen Ihres Kontos aus. Wählen Sie unter **Verbindungszeichenfolge** die Option **Verbindungszeichenfolge** aus. Auf der rechten Seite des Portals werden die Informationen angezeigt, die Sie für die Verbindung mit Ihrem Konto benötigen:

    ![Informationen zur Verbindungszeichenfolge](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="use-mongoimport"></a>Verwenden von „mongoimport“

Verwenden Sie zum Importieren von Daten in Ihr Cosmos-Konto die folgende Vorlage.

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Ersetzen Sie die Parameter \<your_hostname>, \<your_username> und \<your_password> durch die spezifischen Werte für Ihr Konto. Im folgenden Beispiel verwenden wir **sampleDB** als Wert für \<your_database> und **sampleColl** als Wert für \<your_collection>:

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="use-mongorestore"></a>Verwenden von „mongorestore“

Um Daten in Ihrem Cosmos-Konto wiederherzustellen, das mit der API für MongoDB von Cosmos DB konfiguriert wurde, verwenden Sie die folgende Vorlage, um den Import auszuführen.

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Ersetzen Sie die Parameter \<your_hostname>, \<your_username> und \<your_password> durch die spezifischen Werte für Ihr Konto. Im folgenden Beispiel verwenden wir **./dumps/dump-2016-12-07** als Wert für \<path_to_backup>:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --db mydatabase --collection mycollection --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen nicht mehr benötigen, können Sie die Ressourcengruppe, das Cosmos-Konto und alle zugehörigen Ressourcen löschen. Führen Sie die folgenden Schritte aus, um die Ressourcengruppe zu löschen:

1. Navigieren Sie zu der Ressourcengruppe, in der Sie das Cosmos-Konto erstellt haben.
1. Wählen Sie die Option **Ressourcengruppe löschen**.
1. Bestätigen Sie dann den Namen der zu löschenden Ressourcengruppe, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Daten mit der API für MongoDB von Azure Cosmos DB abfragen können. 

> [!div class="nextstepaction"]
> [Abfragen von MongoDB-Daten](../cosmos-db/tutorial-query-mongodb.md)
