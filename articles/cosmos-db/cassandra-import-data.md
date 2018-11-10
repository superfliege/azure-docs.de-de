---
title: Migrieren Ihrer Daten zum Azure Cosmos DB-Konto für die Cassandra-API
description: Erfahren Sie, wie Sie den CQL-Kopierbefehl und Spark verwenden, um Daten aus Apache Cassandra in die Azure Cosmos DB-Cassandra-API zu kopieren.
services: cosmos-db
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.author: govindk
ms.topic: tutorial
ms.date: 09/24/2018
ms.reviewer: sngun
ms.openlocfilehash: 56fc07c6d775ee8015ce244acb7782607bda802a
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739786"
---
# <a name="tutorial-migrate-your-data-to-azure-cosmos-db-cassandra-api-account"></a>Tutorial: Migrieren Ihrer Daten zum Azure Cosmos DB-Konto für die Cassandra-API

Dieses Tutorial enthält Anweisungen zum Migrieren von Apache Cassandra-Daten zur Azure Cosmos DB-Cassandra-API. 

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Planen der Migration
> * Voraussetzungen für die Migration
> * Migrieren von Daten mithilfe des cqlsh-Befehls COPY
> * Migrieren von Daten mithilfe von Spark 

## <a name="plan-for-migration"></a>Planen der Migration

Vor dem Migrieren von Daten zur Azure Cosmos DB-Cassandra-API sollten Sie den Durchsatz schätzen, der für Ihre Workload erforderlich ist. Im Allgemeinen wird für den Einstieg der für CRUD-Vorgänge erforderliche durchschnittliche Durchsatz empfohlen. Fügen Sie später den zusätzlich für ETL (Extrahieren, Transformieren und Laden) oder für Vorgänge mit Lastspitzen erforderlichen Durchsatz hinzu. Sie benötigen die folgenden Details für die Planung der Migration: 

* **Größe der vorhandenen Daten oder geschätzte Datengröße:** Legen Sie die Mindestanforderungen für die Datenbankgröße und den Durchsatz fest. Wenn Sie die Datengröße für eine neue Anwendung schätzen, können Sie davon ausgehen, dass die Daten gleichmäßig über die Zeilen verteilt sind und den Wert durch Multiplizieren mit der Datengröße schätzen. 

* **Erforderlicher Durchsatz:** Geben Sie die ungefähre Durchsatzrate für das Lesen (Abfragen/Abrufen) und Schreiben (Aktualisieren/Löschen/Einfügen) an. Dieser Wert ist erforderlich, um die erforderlichen Anforderungseinheiten zusammen mit der Größe der Daten mit festem Zustand zu berechnen.  

* **Abrufen des Schemas:** Stellen Sie über cqlsh eine Verbindung mit Ihrem Cassandra-Cluster her, und exportieren Sie das Schema aus Cassandra: 

  ```bash
  cqlsh [IP] "-e DESC SCHEMA" > orig_schema.cql
  ```

Nachdem Sie die Anforderungen Ihrer Workload ermittelt haben, sollten Sie anhand der gesammelten Durchsatzanforderungen ein Azure Cosmos DB-Konto, eine Datenbank und Container erstellen.  

* **Bestimmen der RU-Gebühr für einen Vorgang:** Sie können die Anforderungseinheiten (RU) mithilfe eines SDK Ihrer Wahl für die Azure Cosmos DB-Cassandra-API ermitteln. Dieses Beispiel zeigt die .NET-Version zum Abrufen von RU-Gebühren.

  ```csharp
  var tableInsertStatement = table.Insert(sampleEntity);
  var insertResult = await tableInsertStatement.ExecuteAsync();

  foreach (string key in insertResult.Info.IncomingPayload)
    {
       byte[] valueInBytes = customPayload[key];
       string value = Encoding.UTF8.GetString(valueInBytes);
       Console.WriteLine($"CustomPayload:  {key}: {value}");
    }
  ```

* **Zuordnen des erforderlichen Durchsatzes:** Azure Cosmos DB kann Speicher und Durchsatz automatisch gemäß Ihren wachsenden Anforderungen skalieren. Sie können Ihre Durchsatzanforderungen mit dem [Anforderungseinheitenrechner von Azure Cosmos DB](https://www.documentdb.com/capacityplanner) schätzen. 

## <a name="prerequisites-for-migration"></a>Voraussetzungen für die Migration

* **Erstellen von Tabellen im Azure Cosmos DB-Konto für die Cassandra-API:** Bevor Sie mit der Migration von Daten beginnen, erstellen Sie vorab alle Ihre Tabellen im Azure-Portal oder mit cqlsh. Wenn Sie als Migrationsziel ein Azure Cosmos DB-Konto mit Durchsatz auf Datenbankebene verwenden, geben Sie beim Erstellen der Azure Cosmos DB-Container einen Partitionsschlüssel an.

* **Erhöhen des Durchsatzes:** Die Dauer der Datenmigration richtet sich nach der Durchsatzmenge, die Sie für Ihre Tabellen in Azure Cosmos DB bereitstellen. Erhöhen Sie den Durchsatz für die Dauer der Migration. Mit dem höheren Durchsatz können Sie eine Ratenbegrenzung vermeiden und Migrationen in kürzerer Zeit durchführen. Nachdem die Migration abgeschlossen ist, können Sie den Durchsatz wieder verringern, um Kosten zu sparen. Weitere Informationen zum Erhöhen des Durchsatzes finden Sie unter [Festlegen von Durchsatz](set-throughput.md) für Azure Cosmos DB-Container. Es wird außerdem empfohlen, ein Azure Cosmos DB-Konto in derselben Region wie die Quelldatenbank zu verwenden. 

* **Aktivieren von SSL:** Für Azure Cosmos DB gelten strenge Sicherheitsanforderungen und -standards. Achten Sie darauf, SSL für die Interaktion mit Ihrem Konto zu aktivieren. Wenn Sie CQL mit SSH verwenden, können Sie SSL-Informationen bereitstellen.

## <a name="options-to-migrate-data"></a>Optionen zum Migrieren von Daten

Mithilfe der folgenden Optionen können Sie Daten aus vorhandenen Cassandra-Workloads nach Azure Cosmos DB verschieben:

* [Mithilfe des cqlsh-Befehls COPY](#using-cqlsh-copy-command)  
* [Mit Spark](#using-spark) 

## <a name="migrate-data-using-cqlsh-copy-command"></a>Migrieren von Daten mithilfe des cqlsh-Befehls COPY

Der [CQL-Befehl COPY](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) wird verwendet, um lokale Daten in das Azure Cosmos DB-Konto für die Cassandra-API zu kopieren. Führen Sie zum Kopieren von Daten die folgenden Schritte aus:

1. Rufen Sie die Informationen zur Verbindungszeichenfolge für das Cassandra-API-Konto ab:

   * Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem Azure Cosmos DB-Konto.

   * Öffnen Sie den Bereich **Verbindungszeichenfolge**. Sie finden dort alle Informationen, die Sie benötigen, um Ihr Cassandra-API-Konto per cqlsh zu verbinden.

2. Melden Sie sich bei cqlsh mit den Verbindungsinformationen aus dem Portal an.

3. Kopieren Sie mit dem CQL-Befehl COPY lokale Daten zum Cassandra API-Konto.

   ```bash
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="migrate-data-using-spark"></a>Migrieren von Daten mithilfe von Spark 

Führen Sie die folgenden Schritte aus, um mit Spark Daten zur Azure Cosmos DB-Cassandra-API zu migrieren:

- Bereitstellen von [Azure Databricks](cassandra-spark-databricks.md) oder eines [HDInsight-Clusters](cassandra-spark-hdinsight.md) 

- Verschieben von Daten zum Cassandra-API-Zielendpunkt mit [Kopiervorgängen für Tabellen](cassandra-spark-table-copy-ops.md) 

Das Migrieren von Daten mithilfe von Spark-Aufträgen ist eine empfohlene Option, wenn sich Ihre Daten in einem vorhandenen Cluster auf virtuellen Azure-Computern oder in einer anderen Cloud befinden. Bei dieser Option muss Spark für die einmalige oder regelmäßige Erfassung zwischengeschaltet werden. Sie können diese Migration mithilfe von ExpressRoute-Verbindungen zwischen dem lokalen Standort und Azure beschleunigen. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, Ihre Daten zu Ihrem Azure Cosmos DB-Konto für die Cassandra-API zu migrieren. Sie können jetzt mit dem Abschnitt „Konzepte“ fortfahren, um weitere Informationen zu Azure Cosmos DB zu erhalten. 

> [!div class="nextstepaction"]
> [Einstellbare Datenkonsistenzebenen in Azure Cosmos DB](../cosmos-db/consistency-levels.md)


