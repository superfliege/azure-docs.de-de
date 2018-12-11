---
title: Verwenden von „mongoimport“ und „mongorestore“ mit der Azure Cosmos DB-API für MongoDB
description: Erfahren Sie, wie Sie „mongoimport“ und „mongorestore“ zum Importieren von Daten in eine API für ein MongoDB-Konto verwenden.
keywords: mongoimport, mongorestore
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 50bb34d86780dec003c63b5ff0a3884049dd47c1
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52871004"
---
# <a name="tutorial-migrate-your-data-to-azure-cosmos-db-mongodb-api-account"></a>Tutorial: Migrieren Ihrer Daten zum Azure Cosmos DB-Konto für die MongoDB-API

Dieses Tutorial enthält Anweisungen zum Migrieren von in MongoDB gespeicherten Daten zum Azure Cosmos DB-Konto für die MongoDB-API. Wenn Sie Daten aus MongoDB importieren und diese mit der SQL-API von Azure Cosmos DB verwenden möchten, sollten Sie die Daten mithilfe des [Datenmigrationstools](import-data.md) importieren.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Planen der Migration
> * Voraussetzungen für die Migration
> * Migrieren von Daten mithilfe von „mongoimport“
> * Migrieren von Daten mithilfe von „mongorestore“

Vergewissern Sie sich vor dem Migrieren von Daten zum MongoDB-API-Konto, dass MongoDB-Beispieldaten vorhanden sind. Besitzen Sie keine MongoDB-Beispieldatenbank, können Sie [MongoDB Community Server](https://www.mongodb.com/download-center) herunterladen und installieren, eine Beispieldatenbank erstellen und mithilfe von „mongoimport.exe“ bzw. „mongorestore.exe“ Beispieldaten hochladen. 

## <a name="plan-for-migration"></a>Planen der Migration

1. Erstellen und skalieren Sie Ihre Sammlungen im Voraus:
        
   * Standardmäßig stellt Azure Cosmos DB eine neue MongoDB-Sammlung mit 1.000 Anforderungseinheiten pro Sekunde (RU/s) bereit. Erstellen Sie vorab alle Ihre Sammlungen über das [Azure-Portal](https://portal.azure.com) oder über MongoDB-Treiber und -Tools, bevor Sie mit der Migration mithilfe von „mongoimport“ oder „mongorestore“ beginnen. Wenn die Größe Ihrer Daten 10 GB übersteigt, müssen Sie eine [partitionierte Sammlung](partition-data.md) mit einem entsprechenden Shard-Schlüssel erstellen. MongoDB empfiehlt, Entitätsdaten in Sammlungen zu speichern. Sie können für Entitäten vergleichbarer Größe die gleiche Sammlung verwenden und Durchsatz auf der Azure Cosmos-Datenbankebene bereitstellen.

   * Erhöhen Sie für die Dauer der Migration den Durchsatz Ihrer Sammlungen über das [Azure-Portal](https://portal.azure.com) von 1.000 RUs/s für eine Sammlung mit einer einzelnen Partition und von 2.500 RUs/s für eine Sammlung mit Shards. Mit dem höheren Durchsatz können Sie eine Ratenbegrenzung vermeiden und Migrationen in kürzerer Zeit durchführen. Sie können den Durchsatz sofort nach der Migration verringern, um Kosten zu sparen.

   * Zusätzlich zur Bereitstellung von RU/s auf Ebene der Sammlungen können Sie zudem RU/s für eine Gruppe von Sammlungen auf der übergeordneten Datenbankebene bereitstellen. Dazu müssen vorab die Datenbank und die Sammlungen erstellt sowie jeweils ein Shardschlüssel für die einzelnen Sammlungen definiert werden.

   * Sie können Sammlungen mit Shards mithilfe Ihrer bevorzugten Tools, Treiber oder SDKs erstellen. In diesem Beispiel verwenden wir die Mongo Shell zum Erstellen einer Sammlung mit Shards:

        ```bash
        db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
        ```
    
        Ergebnisse:

        ```JSON
        {
            "_t" : "ShardCollectionResponse",
            "ok" : 1,
            "collectionsharded" : "admin.people"
        }
        ```

1. Berechnen Sie die ungefähre RU-Gebühr für das Schreiben eines einzelnen Dokuments:

   a. Stellen Sie über die MongoDB-Shell eine Verbindung mit Ihrem MongoDB-API-Konto für Azure Cosmos DB her. Eine Anleitung dazu finden Sie unter [Connect a MongoDB application to Azure Cosmos DB (Verbinden einer MongoDB-Anwendung mit Azure Cosmos DB)](connect-mongodb-account.md).
    
   b. Führen Sie einen Beispieleinfügebefehl mit einem Ihrer Beispieldokumente aus der MongoDB-Shell aus:
   
      ```bash
      db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })
      ```
        
   c. Führen Sie ```db.runCommand({getLastRequestStatistics: 1})``` aus, und Sie erhalten eine Antwort ähnlich der folgenden:
     
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
        
    d. Beachten Sie die Gebühr für die Anforderung.
    
1. Ermitteln Sie die Latenz von Ihrem Computer zum Azure Cosmos DB-Clouddienst:
    
    a. Aktivieren Sie die ausführliche Protokollierung über die MongoDB-Shell mit diesem Befehl: ```setVerboseShell(true)```
    
    b. Führen Sie eine einfache Abfrage für die Datenbank aus: ```db.coll.find().limit(1)```. Sie erhalten eine Antwort ähnlich der folgenden:

       ```bash
       Fetched 1 record(s) in 100(ms)
       ```
        
1. Entfernen Sie das eingefügte Dokument vor der Migration, damit keine doppelten Dokumente vorhanden sind. Mit diesem Befehl können Sie Dokumente entfernen: ```db.coll.remove({})```

1. Berechnen Sie die ungefähren Werte für *batchSize* und *numInsertionWorkers*:

    * Berechnen Sie *batchSize*, indem Sie die Gesamtzahl der bereitgestellten RUs durch die RUs teilen, die für das Schreiben des einzelnen Dokuments in Schritt 3 verbraucht wurden.
    
    * Wenn die berechnete *batchSize* <= 24 ist, verwenden Sie diese Zahl als Wert für *batchSize*.
    
    * Wenn die berechnete *batchSize* > 24 ist, legen Sie den Wert von *batchSize* auf 24 fest.
    
    * Berechnen Sie *numInsertionWorkers* mit dieser Formel: *numInsertionWorkers = (bereitgestellter Durchsatz * Latenz in Sekunden) / (Batchgröße * Für einen einzelnen Schreibvorgang verbrauchte RUs)*.
        
    |Eigenschaft|Wert|
    |--------|-----|
    |batchSize| 24 |
    |Bereitgestellte RUs | 10000 |
    |Latency | 0,100 s |
    |Für 1 Schreibvorgang berechnete RUs | 10 RUs |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10.000 RUs x 0,1 s) / (24 x 10 RUs) = 4,1666*

1. Führen Sie den Migrationsbefehl aus. Die Optionen zum Migrieren von Daten werden in den nächsten Abschnitten beschrieben.

   ```bash
   mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```
   Oder verwenden Sie „mongorestore“ (wobei der Durchsatz für alle Sammlungen mindestens der RU-Anzahl aus den vorherigen Berechnungen entsprechen muss):
   
   ```bash
   mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
   ```

## <a name="prerequisites-for-migration"></a>Voraussetzungen für die Migration

* **Erhöhung des Durchsatzes:** Die Dauer der Datenmigration richtet sich nach dem Durchsatzwert, den Sie für eine einzelne Sammlung oder eine Gruppe von Sammlungen einrichten. Achten Sie darauf, dass Sie den Durchsatz für größere Datenmigrationen erhöhen. Nachdem die Migration abgeschlossen ist, können Sie den Durchsatz wieder verringern, um Kosten zu sparen. Weitere Informationen zur Erhöhung des Durchsatzes im [Azure-Portal](https://portal.azure.com) finden Sie unter [Leistungsstufen und Tarife in Azure Cosmos DB](performance-levels.md).

* **Aktivieren von SSL:** Für Azure Cosmos DB gelten strenge Sicherheitsanforderungen und -standards. Achten Sie darauf, SSL für die Interaktion mit Ihrem Konto zu aktivieren. Die Verfahren im restlichen Artikel verdeutlichen, wie Sie SSL für „mongoimport“ und „mongorestore“ aktivieren.

* **Erstellen von Azure Cosmos DB-Ressourcen:** Bevor Sie mit der Migration von Daten beginnen, erstellen Sie Ihre gesamten Sammlungen vorab im Azure-Portal. Wenn Sie eine Migration zu einem Azure Cosmos DB-Konto durchführen, das über Durchsatz auf Datenbankebene verfügt, sollten Sie beim Erstellen der Azure Cosmos DB-Sammlungen unbedingt einen Partitionsschlüssel angeben.

## <a name="get-your-connection-string"></a>Abrufen Ihrer Verbindungszeichenfolge 

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf der linken Seite auf den Eintrag **Azure Cosmos DB**.
1. Wählen Sie unter **Abonnements** den Namen Ihres Kontos aus.
1. Klicken Sie auf dem Blatt **Verbindungszeichenfolge** auf **Verbindungszeichenfolge**.

   Der rechte Bereich enthält alle Informationen, die Sie zum erfolgreichen Verbinden des Kontos benötigen.

   ![Blatt „Verbindungszeichenfolge“](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="migrate-data-by-using-mongoimport"></a>Migrieren von Daten mithilfe von „mongoimport“

Verwenden Sie zum Importieren von Daten in Ihr Azure Cosmos DB-Konto die folgende Vorlage. Geben Sie für *host*, *username* und *password* die Werte ein, die für Ihr Konto gelten.  

Vorlage:

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Beispiel:  

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="migrate-data-by-using-mongorestore"></a>Migrieren von Daten mithilfe von „mongorestore“

Verwenden Sie zum Wiederherstellen von Daten für Ihre API für das MongoDB-Konto die folgende Vorlage, um den Importvorgang durchzuführen. Geben Sie für *host*, *username* und *password* die Werte ein, die für Ihr Konto gelten.

Vorlage:

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Beispiel:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="next-steps"></a>Nächste Schritte

Sie können mit dem nächsten Tutorial fortfahren und sich darüber informieren, wie Sie MongoDB-Daten mithilfe von Azure Cosmos DB abfragen können. 

> [!div class="nextstepaction"]
>[Abfragen von MongoDB-Daten](../cosmos-db/tutorial-query-mongodb.md)
