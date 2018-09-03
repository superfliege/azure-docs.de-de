---
title: Importieren von Cassandra-Daten in Azure Cosmos DB | Microsoft Docs
description: Hier erfahren Sie, wie Sie mit dem CQL-Kopierbefehl Cassandra-Daten in Azure Cosmos DB kopieren.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: govindk
ms.custom: mvc
ms.openlocfilehash: 55a6fec1b6ac018b4b24c0d27dcfdd5812455800
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189675"
---
# <a name="migrate-your-data-to-azure-cosmos-db-cassandra-api-account"></a>Migrieren Ihrer Daten zum Azure Cosmos DB-Konto für die Cassandra-API

Dieses Tutorial enthält Anweisungen zum Importieren von Cassandra-Daten in Azure Cosmos DB mit dem Befehl COPY der Cassandra Query Language (CQL). 

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Abrufen der Verbindungszeichenfolge
> * Importieren von Daten mit dem Befehl „cqlsh COPY“
> * Importieren mit dem Spark-Connector 

# <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie [Apache Cassandra](http://cassandra.apache.org/download/), und vergewissern Sie sich, dass *cqlsh* vorhanden ist.  

* Erhöhung des Durchsatzes: Die Dauer der Datenmigration richtet sich nach der Durchsatzmenge, die Sie für Ihre Tabellen bereitstellen. Achten Sie darauf, dass Sie den Durchsatz für größere Datenmigrationen erhöhen. Nachdem die Migration abgeschlossen ist, können Sie den Durchsatz wieder verringern, um Kosten zu sparen. Weitere Informationen zur Erhöhung des Durchsatzes im [Azure-Portal](https://portal.azure.com) finden Sie unter [Festlegen von Durchsatz für Azure Cosmos DB-Container](set-throughput.md).  

* SSL-Aktivierung: Für Azure Cosmos DB gelten strenge Sicherheitsanforderungen und -standards. Achten Sie darauf, SSL für die Interaktion mit Ihrem Konto zu aktivieren. Wenn Sie CQL mit SSH verwenden, können Sie SSL-Informationen bereitstellen. 

## <a name="get-your-connection-string"></a>Abrufen Ihrer Verbindungszeichenfolge

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) in der linken Leiste auf **Azure Cosmos DB**.

2. Wählen Sie unter **Abonnements** den Namen Ihres Kontos aus.

3. Klicken Sie auf **Verbindungszeichenfolge**. Der rechte Bereich enthält alle Informationen, die Sie zum erfolgreichen Verbinden des Kontos benötigen.

    ![Seite „Verbindungszeichenfolge“](./media/cassandra-import-data/keys.png)

## <a name="migrate-data-by-using-cqlsh-copy"></a>Migrieren von Daten mithilfe von „cqlsh COPY“

Um Cassandra-Daten für die Verwendung mit der Cassandra-API in Azure Cosmos DB zu importieren, gehen Sie wie folgt vor:

1. Melden Sie sich bei cqhsh mit den Verbindungsinformationen aus dem Portal an.
2. Kopieren Sie mit dem [CQL-Befehl „COPY“](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) lokale Daten zum Apache Cassandra API-Endpunkt. Stellen Sie sicher, dass Quelle und Ziel sich im gleichen Rechenzentrum befinden, um Latenzprobleme zu minimieren.

### <a name="steps-to-move-data-with-cqlsh"></a>Schritte zum Verschieben von Daten mit cqlsh

1. Erstellen und skalieren Sie die Tabelle:
    * Standardmäßig stellt Azure Cosmos DB eine neue Cassandra-API-Tabelle mit 1.000 Anforderungseinheiten pro Sekunde (RU/s) bereit (CQL-basierte Erstellung wird mit 400 RU/s bereitgestellt). Stellen Sie vor dem Starten der Migration mit cqlsh zunächst alle Tabellen über das [Azure-Portal](https://portal.azure.com) oder cqlsh. 

    * Erhöhen Sie im [Azure-Portal](https://portal.azure.com) den Datendurchsatz für die Tabellen vom Standarddurchsatz (400 oder 1.000 RU/s) auf 10.000 RU/s für die Dauer der Migration. Mit dem höheren Durchsatz können Sie eine Ratenbegrenzung vermeiden und Migrationen in kürzerer Zeit durchführen. Mit der stündlichen Abrechnung in Azure Cosmos DB können Sie den Durchsatz sofort nach der Migration verringern, um Kosten zu sparen.

2. Bestimmen Sie die RU-Gebühr für einen Vorgang. Hierzu können Sie ein beliebiges Azure Cosmos DB Cassandra-API-SDK verwenden. Dieses Beispiel zeigt die .NET-Version zum Abrufen von RU-Gebühren. 

    ```csharp
    var tableInsertStatement = table.Insert(sampleEntity);
    var insertResult = await tableInsertStatement.ExecuteAsync();

    foreach (string key in insertResult.Info.IncomingPayload)
            {
                byte[] valueInBytes = customPayload[key];
                string value = Encoding.UTF8.GetString(valueInBytes);
                Console.WriteLine($“CustomPayload:  {key}: {value}”);
            }
 
    ``` 

3. Ermitteln Sie die Latenz von Ihrem Computer zum Azure Cosmos DB-Dienst. Wenn Sie sich in einem Azure-Rechenzentrum befinden, sollte die Latenz in einem niedrigen einstelligen Millisekundenbereich liegen. Außerhalb des Azure-Rechenzentrums können Sie mit psping oder azurespeed.com die ungefähre Latenz zu ihrem Standort ermitteln.   

4. Berechnen Sie die geeigneten Werte für Parameter (NUMPROCESS, INGESTRATE, MAXBATCHSIZE oder MINBATCHSIZE), die eine gute Leistung ermöglichen. 

5. Führen Sie den endgültigen Migrationsbefehl aus. Bei diesem Befehl wird davon ausgegangen, dass Sie cqlsh mit den Informationen zur Verbindungszeichenfolge gestartet haben.

   ```bash
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="migrate-data-by-using-spark"></a>Migrieren von Daten mithilfe von Spark

Bei Daten, die sich in einem vorhandenen Cluster auf virtuellen Azure-Computern befinden, ist das Importieren von Daten mithilfe von Spark ebenfalls möglich. Hierfür muss Spark für die einmalige oder regelmäßige Erfassung zwischengeschaltet werden. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Abrufen der Verbindungszeichenfolge
> * Importieren von Daten mit dem CQL-Kopierbefehl
> * Importieren mit dem Spark-Connector 

Sie können jetzt mit dem Abschnitt „Konzepte“ fortfahren, um weitere Informationen zu Azure Cosmos DB zu erhalten. 

> [!div class="nextstepaction"]
>[Einstellbare Datenkonsistenzebenen in Azure Cosmos DB](../cosmos-db/consistency-levels.md)
