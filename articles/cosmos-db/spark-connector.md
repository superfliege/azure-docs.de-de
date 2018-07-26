---
title: Herstellen einer Verbindung für Apache Spark mit Azure Cosmos DB | Microsoft-Dokumentation
description: In diesem Tutorial wird der Azure Cosmos DB-Spark-Connector beschrieben, mit dem Sie für Apache Spark eine Verbindung mit Azure Cosmos DB herstellen können, um verteilte Aggregationen und Data Science-Vorgänge für das global verteilte mehrinstanzenfähige Datenbanksystem von Microsoft für die Cloud durchzuführen.
keywords: Apache Spark
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: ramkris
ms.openlocfilehash: cae66a40882231f7762af29cdeaaf658dd2aa968
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113946"
---
# <a name="accelerate-real-time-big-data-analytics-by-using-the-spark-to-azure-cosmos-db-connector"></a>Beschleunigen der Big Data-Echtzeitanalyse mit dem Spark-Connector für Azure Cosmos DB
 
Mit dem Spark-Connector für Azure Cosmos DB kann Azure Cosmos DB als Eingabe oder Ausgabe für Apache Spark-Aufträge fungieren. Durch die Verbindung von [Spark](http://spark.apache.org/) mit [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) haben Sie die Möglichkeit, plötzlich auftretende Data Science-Probleme rasch zu lösen, indem Sie Azure Cosmos DB verwenden, um Daten schnell zu speichern und abzufragen. Der Spark-Connector für Azure Cosmos DB nutzt die nativen verwalteten Azure Cosmos DB-Indizes effizient. Die Indizes aktivieren aktualisierbare Spalten, wenn Sie Analysen durchführen und Pushdown-Prädikatfilter für sich schnell ändernde global verteilte Daten verwenden. Dies kann das Internet der Dinge (IoT), Data Science und Analyseszenarien umfassen.

In diesem Video erfahren Sie mehr über den Spark-Connector für Azure Cosmos DB:

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T135/player] 

## <a name="connector-components"></a>Connectorkomponenten

Der Spark-Connector für Azure Cosmos DB verwendet die folgenden Komponenten:

* Mit [Azure Cosmos DB](http://documentdb.com) können Kunden sowohl den Durchsatz als auch den Speicher in beliebig vielen geografischen Regionen bereitstellen und flexibel skalieren.  

* [Apache Spark](http://spark.apache.org/) ist eine leistungsstarke Open-Source-Verarbeitungs-Engine, die für hohe Geschwindigkeit, einfache Nutzung und anspruchsvolle Analysen konzipiert wurde.  

* [Apache Spark-Cluster in Azure Databricks](https://docs.azuredatabricks.net/getting-started/index.html) werden zum Ausführen von Spark-Aufträgen im Spark-Cluster verwendet.

## <a name="connect-apache-spark-to-azure-cosmos-db"></a>Verbinden von Apache Spark mit Azure Cosmos DB

Es gibt zwei Ansätze zum Verbinden von Apache Spark mit Azure Cosmos DB:

1. Die Verwendung von [Azure Cosmos DB SQL Python SDK](https://github.com/Azure/azure-documentdb-python), einem Python-basierten Spark-Connector für Cosmos DB, der auch als „pyDocumentDB“ bezeichnet wird.  

2. Die Verwendung von [Azure Cosmos DB SQL Java SDK](https://github.com/Azure/azure-documentdb-java), einem Java-basierten Spark-Connector für Cosmos DB.


**Unterstützte Versionen**

| Komponente | Version |
|---------|-------|
|Apache Spark| 2.1.x, 2.2.x, 2.3.x |
| Scala|2.11|
| Databricks-Runtimeversion | 3.4 und höher |
| Azure Cosmos DB SQL Java SDK | 1.16.2 |

## <a name="connect-by-using-python-or-pydocumentdb-sdk"></a>Herstellen einer Verbindung mithilfe von Python oder mithilfe des pyDocumentDB SDK

Auf der folgenden Abbildung wird die Architektur der Implementierung des pyDocumentDB SDK dargestellt:

![Datenfluss von Spark zu Azure Cosmos DB über pyDocumentDB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow"></a>Datenfluss

Der Datenfluss der pyDocumentDB-Implementierung entspricht Folgendem:

* Der Spark-Masterknoten wird über pyDocumentDB mit dem Azure Cosmos DB-Gatewayknoten verbunden. Benutzer geben nur die Spark- und Azure Cosmos DB-Verbindungen an. Verbindungen mit den entsprechenden Master- und Gatewayknoten sind für den Benutzer transparent.  

* Der Gatewayknoten führt die Abfrage für Azure Cosmos DB durch, wo die Abfrage dann für die Partitionen der Sammlung auf den Datenknoten erfolgt. Die Antwort auf diese Abfragen wird zurück an den Gatewayknoten gesendet, und das Resultset wird an den Spark-Masterknoten zurückgegeben.  

* Nachfolgende Abfragen (z.B. für einen Spark-Datenrahmen) werden zur Verarbeitung an die Spark-Workerknoten gesendet.  

Die Kommunikation zwischen Spark und Azure Cosmos DB ist auf den Spark-Masterknoten und die Azure Cosmos DB-Gatewayknoten beschränkt. Wie schnell die Abfragen durchgeführt werden, hängt von der Transportschicht zwischen diesen beiden Knoten ab.

Führen Sie folgende Schritte durch, um mithilfe des pyDocumentDB SDK eine Verbindung zwischen Spark und Azure Cosmos DB herzustellen:

1. Erstellen Sie einen [Azure Databricks-Arbeitsbereich](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) und einen [Spark-Cluster](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks) (Version 4.0 der Databricks-Runtime enthält Apache Spark 2.3.0 und Scala 2.11) innerhalb dieses Arbeitsbereichs.  

2. Sobald der Cluster erstellt wurde und ausgeführt wird, navigieren Sie zu **Arbeitsbereich** > **Erstellen** > **Bibliothek**.  
3. Wählen Sie im Dialogfeld „Neue Bibliothek“ **Upload Python Egg or PyPi** (Python-EGG-Datei oder PyPi-Paket hochladen) als Quelle aus, geben Sie **pydocumentdb** als Namen an, und klicken Sie auf **Bibliothek installieren**. PyDocumentdb SDK ist bereits in den PIP-Paketen enthalten. Sie können es also in diesen suchen und anschließend installieren. 

   ![Erstellen und Anfügen von Bibliotheken](./media/spark-connector/create-library.png)

4. Wenn die Bibliothek installiert wurde, fügen Sie diese an den Cluster an, den Sie zuvor erstellt haben.  

5. Navigieren Sie anschließend zu **Arbeitsbereich** > **Erstellen** > **Notebook**.  

6. Geben Sie im Dialogfeld **Create Notebook** (Notebook erstellen) einen Anzeigenamen ein, und wählen Sie **Python** als Sprache aus. Wählen Sie aus der Dropdownliste den Cluster aus, den Sie zuvor erstellt haben, und klicken Sie auf **Erstellen**.  

7. Aufgrund der Einfachheit des Kommunikationstransports ist die Ausführung einer Abfrage von Spark zu Azure Cosmos DB per pyDocumentDB relativ unkompliziert. Als Nächstes führen Sie einige Spark-Abfragen mithilfe der Beispieldaten für Flüge durch, die im öffentlich zugänglichen Cosmos DB-Konto „doctorwho“ gehostet sind. Die HTML-Version des Notebooks ist im GitHub-Repository [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) gehostet. Laden Sie die Repositorydateien herunter, und navigieren Sie zu `\samples\Documentation_Samples\Read_Batch_PyDocumentDB.html`. Sie können das Notebook in Ihr Azure Databricks-Konto importieren und dieses ausführen. Im folgenden Abschnitt wird die Funktionalität der Codeblöcke detailliert beschrieben.

Folgende Codeausschnitte veranschaulichen, wie Sie das pyDocumentDB SDK importieren und eine Abfrage im Spark-Kontext ausführen. Wie im Codeausschnitt deutlich wird, enthält das pyDocumentDB SDK die Verbindungsparameter, die zum Herstellen einer Verbindung mit dem Azure Cosmos DB-Konto erforderlich sind. Es importiert die erforderlichen Bibliotheken und konfiguriert den Masterschlüssel und den Host, um den Azure Cosmos DB-Client (pydocumentdb.document_client) zu erstellen.


```python
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring the connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]

# Set keys to connect to Azure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)

```

Anschließend können Sie Abfragen ausführen. Der folgende Codeausschnitt stellt eine Verbindung mit der Sammlung „airports.codes“ im DoctorWho-Konto her und führt eine Abfrage zum Extrahieren der Städte mit einem Flughafen im Bundesstaat Washington aus. 

```python
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

```

Nachdem die Abfrage ausgeführt wurde, ist das Ergebnis eine query_iterable.QueryIterable-Klasse, die in eine Python-Liste konvertiert wird. Diese wird dann in einen Spark-Datenrahmen konvertiert. 

```python
# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(list(query))

# Show data
df.show()
```

## <a name="considerations-when-using-pydocumentdb-sdk"></a>Überlegungen zur Verwendung des pyDocumentDB SDK
Das Herstellen einer Verbindung zwischen Spark und Azure Cosmos DB mithilfe des pyDocumentDB SDK wird in folgenden Szenarios empfohlen:

* Sie möchten Python verwenden.  

* Sie geben ein relativ kleines Resultset von Azure Cosmos DB an Spark zurück. Beachten Sie, dass das zugrunde liegende Dataset in Azure Cosmos DB groß sein kann, und dass Sie Filter oder Prädikatfilter auf Ihre Azure Cosmos DB-Quelle anwenden.

## <a name="connect-by-using-the-java-sdk"></a>Herstellen einer Verbindung mithilfe des Java SDK

Die folgende Abbildung veranschaulicht die Implementierung der Architektur des Azure Cosmos DB SQL Java SDK und den Datenfluss zwischen den Spark-Workerknoten:

![Datenfluss im Spark-Connector für Azure Cosmos DB](./media/spark-connector/spark-connector.png)

### <a name="data-flow"></a>Datenfluss

Der Datenfluss der Implementierung des Java SDK entspricht Folgendem:

* Es wird eine Verbindung zwischen Spark-Masterknoten und Azure Cosmos DB-Gatewayknoten hergestellt, um die Partitionszuordnung zu erhalten. Benutzer geben nur die Spark- und Azure Cosmos DB-Verbindungen an. Verbindungen mit den entsprechenden Master- und Gatewayknoten sind für den Benutzer transparent.  

* Diese Informationen werden dann wieder für den Spark-Masterknoten bereitgestellt. An diesem Punkt sollten Sie die Abfrage analysieren können, um zu ermitteln, auf welche Partitionen (und ihre Standorte) Sie in Azure Cosmos DB Zugriff haben müssen.  

* Diese Informationen werden an die Spark-Workerknoten übertragen.  

* Für die Spark-Workerknoten werden direkte Verbindungen mit den Azure Cosmos DB-Partitionen hergestellt, um die Daten zu extrahieren und sie dann wieder zurück auf die Spark-Partitionen der Workerknoten zu übertragen.  

Die Kommunikation zwischen Spark und Azure Cosmos DB verläuft erheblich schneller, da die Datenverschiebung zwischen den Spark-Workerknoten und den Azure Cosmos DB-Datenknoten (Partitionen) erfolgt. In diesem Dokument senden Sie einige Twitter-Beispieldaten an ein Azure Cosmos DB-Konto und führen mithilfe dieser Daten Spark-Abfragen aus. Führen Sie folgende Schritte aus, um Twitter-Beispieldaten in Azure Cosmos DB zu schreiben:

1. Erstellen Sie ein [Azure Cosmos DB-Konto](create-sql-api-dotnet.md#create-a-database-account), und fügen Sie diesem eine [Sammlung](create-sql-api-dotnet.md#add-a-collection) hinzu.  

2. Laden Sie das Beispiel [TwitterCosmosDBFeed](https://github.com/tknandu/TwitterCosmosDBFeed) von GitHub herunter. Dieses wird zum Schreiben von Twitter-Beispieldaten in Azure Cosmos DB verwendet.  

3. Öffnen Sie eine Eingabeaufforderung, und installieren Sie Tweepy- und pyDocumentdb-Module, indem Sie folgende Befehle ausführen:

   ```bash
   pip install tweepy==3.3.0
   pip install pyDocumentDB
   ```

4. Extrahieren Sie die Inhalte des Beispiels für Twitter-Feeds, und öffnen Sie die Datei „config.py“. Aktualisieren Sie die Werte „masterKey“, „host“, „databaseId“, „collectionId“ und „preferredlocations“.  

5. Navigieren Sie zu `http://apps.twitter.com/`, und registrieren Sie das Skript für Twitter-Feeds als neue Anwendung. Nachdem Sie einen Namen und einen Anwendungstyp für Ihre App ausgewählt haben, erhalten Sie einen **Verbraucherschlüssel, ein Verbrauchergeheimnis, ein Zugriffstoken und ein Zugriffstokengeheimnis**. Kopieren Sie diese Werte, und aktualisieren Sie sie in der Datei „config.py“, um der Anwendung programmgesteuerten Zugriff auf Twitter bereitzustellen.   

6. Speichern Sie die Datei „config.py“. Öffnen Sie die Eingabeaufforderung, und führen Sie die Python-Anwendung mit folgendem Befehl aus:

   ```bash
   Python driver.py
   ```

7. Navigieren Sie zur Azure Cosmos DB-Sammlung im Portal, und überprüfen Sie, ob die Twitter-Daten in diese geschrieben wurden.

### <a name="find-and-attach-java-sdk-to-the-spark-cluster"></a>Suchen und Anfügen des Java SDK im Spark-Cluster

1. Erstellen Sie einen [Azure Databricks-Arbeitsbereich](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) und einen [Spark-Cluster](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks) (Version 4.0 der Databricks-Runtime enthält Apache Spark 2.3.0 und Scala 2.11) innerhalb dieses Arbeitsbereichs.  

2. Sobald der Cluster erstellt wurde und ausgeführt wird, navigieren Sie zu **Arbeitsbereich** > **Erstellen** > **Bibliothek**.  

3. Wählen Sie im Dialogfeld „Neue Bibliothek“ **Maven-Koordinate** als Quelle aus, geben Sie den Koordinatenwert **com.microsoft.azure:azure-cosmosdb-spark_2.3.0_2.11:1.2.0** an, und klicken Sie auf **Bibliothek erstellen**. Die Maven-Abhängigkeiten werden aufgelöst, und das Paket wird zu Ihrem Arbeitsbereich hinzugefügt. Im oben genannten Format für Maven-Koordinaten stellt 2.3.0 die Spark-Version, 2.11 die Scala-Version und 1.2.0 die Version des Azure Cosmos DB-Connectors dar. 

4. Wenn die Bibliothek installiert wurde, fügen Sie diese an den Cluster an, den Sie zuvor erstellt haben. 

In diesem Artikel wird die Verwendung des Java SDK des Spark-Connectors in folgenden Szenarios veranschaulicht:

* Beim Lesen von Twitter-Daten aus Azure Cosmos DB  

* Beim Lesen von Twitter-Daten, die an Azure Cosmos DB gestreamt werden  

* Beim Schreiben von Twitter-Daten in Azure Cosmos DB 

### <a name="read-twitter-data-from-azure-cosmos-db"></a>Beim Lesen von Twitter-Daten aus Azure Cosmos DB
 
In diesem Abschnitt führen Sie Spark-Abfragen aus, um ein Batch von Twitter-Daten aus Azure Cosmos DB zu lesen. Die HTML-Version des Notebooks ist im GitHub-Repository [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) gehostet. Laden Sie die Repositorydateien herunter, und navigieren Sie zu `\samples\Documentation_Samples\Read_Batch_Twitter_Data.html`. Sie können das Notebook in Ihr Azure Databricks-Konto importieren, die Namen des URI, des Masterschlüssels, der Datenbank und der Sammlung des Kontos aktualisieren, oder Sie können das Notebook folgendermaßen erstellen:

1. Navigieren Sie zu Ihrem Azure Databricks-Konto, und klicken Sie auf **Arbeitsbereich** > **Erstellen** > **Notebook**. 

2. Geben Sie im Dialogfeld **Create Notebook** (Notebook erstellen) einen Anzeigenamen ein, wählen Sie **Python** als Sprache aus, und wählen Sie aus der Dropdownliste den Cluster aus, den Sie zuvor erstellt haben. Klicken Sie dann auf **Erstellen**.  

3. Aktualisieren Sie die Werte für den Endpunkt, den Masterschlüssel, die Datenbank und die Sammlung, um eine Verbindung zum Konto herzustellen und Tweets mithilfe des Befehls „spark.read.format()“ zu lesen.

   ```python
   # Configuration Map
   tweetsConfig = {
   "Endpoint" : "<Your Azure Cosmos DB endpoint>",
   "Masterkey" : "<Primary key of your Azure Cosmos DB account>",
   "Database" : "<Your Azure Cosmos DB database name>",
   "Collection" : "<Your Azure Cosmos DB collection name>", 
   "preferredRegions" : "East US",
   "SamplingRatio" : "1.0",
   "schema_samplesize" : "200000",
   "query_custom" : "SELECT c.id, c.created_at, c.user.screen_name, c.user.lang, c.user.location, c.text, c.retweet_count, c.entities.hashtags, c.entities.user_mentions, c.favorited, c.source FROM c"
   }
   # Read Tweets
   tweets = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**tweetsConfig).load()
   tweets.createOrReplaceTempView("tweets")
   #tweets.cache()

   ```

4. Führen Sie die Abfrage aus, um die Anzahl der Tweets nach unterschiedlichen Hashtags aus den zwischengespeicherten Daten abzurufen. 

   ```python
   %sql
   select hashtags.text, count(distinct id) as tweets
   from (
   select 
     explode(hashtags) as hashtags,
     id
   from tweets
   ) a
   group by hashtags.text
   order by tweets desc
   limit 10
   ```

Das Java SDK unterstützt folgende Werte für die Konfigurationszuordnung: 

|Einstellung  |Beschreibung  |
|---------|---------|
|query_maxdegreeofparallelism  | Legt die Anzahl der gleichzeitigen Vorgänge fest, die auf Clientseite während der parallelen Abfrageausführung ausgeführt werden. Wenn diese Einstellung auf einen Wert festgelegt ist, der größer als 0 (null) ist, wird die Anzahl der gleichzeitigen Vorgänge auf den zugeordneten Wert festgelegt. Wenn ein kleinerer Wert als 0 festgelegt wird, wird die Anzahl der gleichzeitig auszuführenden Vorgänge automatisch vom System festgelegt. Da der Connector jede Sammlungspartition einem Executor zuordnet, hat dieser Wert keine Auswirkungen auf Lesevorgänge.        |
|query_maxbuffereditemcount     |    Legt die maximale Anzahl von Elementen fest, die auf Clientseite während der parallelen Abfrageausführung gepuffert werden können. Wenn diese Einstellung auf einen Wert festgelegt ist, der größer als 0 (null) ist, wird die Anzahl der gepufferten Elemente auf den zugeordneten Wert festgelegt. Wenn ein kleinerer Wert als 0 festgelegt wird, wird die Anzahl der zu puffernden Elemente automatisch vom System festgelegt.     |
|query_enablescan    |   Legt die Option fest, um Scans für Abfragen zu aktivieren, die nicht verarbeitet werden konnte, weil die Indizierung im angeforderten Pfad deaktiviert war.       |
|query_disableruperminuteusage  |  Deaktiviert die Kapazität für Anforderungseinheiten pro Minute, um die Abfrage zu verarbeiten, wenn die regulär bereitgestellten Anforderungseinheiten pro Sekunde ausgeschöpft sind.       |
|query_emitverbosetraces   |   Legt die Option fest, um Abfragen das Ausgeben der ausführlichen Ablaufverfolgung zu Untersuchungszwecken zu ermöglichen.      |
|query_pagesize  |   Legt die Größe der Abfrageergebnisseite für jede Abfrageanforderung fest. Verwenden Sie eine große Seite, um den Durchsatz zu optimieren, indem die Anzahl der Roundtrips zum Abrufen der Abfrageergebnisse reduziert wird.      |
|query_custom  |  Legt die Azure Cosmos DB-Abfrage fest, um die Standardabfrage beim Abrufen von Daten von Azure Cosmos DB außer Kraft zu setzen. Wenn dieser Wert bereitgestellt wird, sollten Sie beachten, dass er ebenfalls anstelle einer Abfrage mit per Push übertragenen Prädikaten verwendet wird.     |

Je nach Szenario sollten unterschiedliche Konfigurationswerte verwendet werden, um Leistung und Durchsatz zu optimieren. Beachten Sie, dass der Konfigurationsschlüssel derzeit keine Groß- und Kleinschreibung berücksichtigt und der Konfigurationswert immer eine Zeichenfolge ist.

### <a name="read-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Beim Lesen von Twitter-Daten, die an Azure Cosmos DB gestreamt werden

In diesem Abschnitt führen Sie Spark-Abfragen aus, um einen Änderungsfeed des Streamings von Twitter-Daten zu lesen. Während Sie die Abfragen in diesem Abschnitt ausführen, sollten Sie sicherstellen, dass Ihre App für Twitter-Feeds ausgeführt ist und Daten an Azure Cosmos DB überträgt. Die HTML-Version des Notebooks ist im GitHub-Repository [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) gehostet. Laden Sie die Repositorydateien herunter, und navigieren Sie zu `\samples\Documentation_Samples\Read_Stream_Twitter_Data.html`. Sie können das Notebook in Ihr Azure Databricks-Konto importieren, die Namen des URI, des Masterschlüssels, der Datenbank und der Sammlung des Kontos aktualisieren, oder Sie können das Notebook folgendermaßen erstellen:

1. Navigieren Sie zu Ihrem Azure Databricks-Konto, und klicken Sie auf **Arbeitsbereich** > **Erstellen** > **Notebook**.  

2. Geben Sie im Dialogfeld **Create Notebook** (Notebook erstellen) einen Anzeigenamen ein, wählen Sie **Scala** als Sprache aus, und wählen Sie aus der Dropdownliste den Cluster aus, den Sie zuvor erstellt haben. Klicken Sie dann auf **Erstellen**.  

3. Aktualisieren Sie die Werte für den Endpunkt, den Masterschlüssel, die Datenbank und die Sammlung, um eine Verbindung zum Konto herzustellen.

   ```scala
   // This script does the following:
   // - creates a structured stream from a Twitter feed CosmosDB collection (on top of change feed)
   // - get the count of tweets
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark.config.Config
   import org.codehaus.jackson.map.ObjectMapper
   import com.microsoft.azure.cosmosdb.spark.streaming._
   import java.time._

   val sourceConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB collection name>", 
   "ConnectionMode" -> "Gateway",
   "ChangeFeedCheckpointLocation" -> "/tmp",
   "changefeedqueryname" -> "Streaming Query from Cosmos DB Change Feed Internal Count")
   ```
4. Lesen Sie den Änderungsfeed als Stream, indem Sie den Befehl „spark.readStream.format()“ verwenden:

   ```scala
   var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()
   ```
5. Starten Sie eine Streamingabfrage in der Konsole:

   ```scala
   //**RUN THE ABOVE FIRST AND KEEP BELOW IN SEPARATE CELL
   val query = streamData.withColumn("countcol", streamData.col("id").substr(0,0)).groupBy("countcol").count().writeStream.outputMode("complete").format("console").start()
   ```

Das Java SDK unterstützt folgende Werte für die Konfigurationszuordnung:

|Einstellung  |Beschreibung  |
|---------|---------|
|readchangefeed   |  Gibt an, dass der Sammlungsinhalt vom CosmosDB-Änderungsfeed abgerufen wird. Der Standardwert ist „false“.       |
|changefeedqueryname |   Eine benutzerdefinierte Zeichenfolge zum Identifizieren der Abfrage. Der Connector verfolgt die Fortsetzungstokens für Sammlungen für unterschiedliche Änderungsfeedabfragen separat. Wenn „readchangefeedis“ auf TRUE festgelegt ist, ist dies eine erforderliche Konfiguration, deren Wert nicht leer sein darf.      |
|changefeedcheckpointlocation  |   Ein Pfad zum lokalen Speicherplatz, um Fortsetzungstokens im Fall von Knotenausfällen beizubehalten.      |
|changefeedstartfromthebeginning  |  Legt fest, ob der Änderungsfeed am Anfang (TRUE) oder am aktuellen Punkt (FALSE) beginnen soll. Standardmäßig beginnt er am aktuellen Punkt (FALSE).       |
|rollingchangefeed  |   Ein boolescher Wert, der angibt, ob der Änderungsfeed aus der letzten Abfrage stammen soll. Der Standardwert ist FALSE. Das bedeutet, dass die Änderungen vom ersten Lesen der Sammlung an gezählt werden.      |
|changefeedusenexttoken  |   Ein boolescher Wert, um die Verarbeitung von Fehlerszenarios zu unterstützen. Er wird verwendet, um anzugeben, dass das aktuelle Batch von Änderungsfeeds ordnungsgemäß verarbeitet wurde und RDD die nächsten Fortsetzungstokens verwenden soll, um das nachfolgende Batch mit Änderungen abzurufen.      |
| InferStreamSchema | Ein boolescher Wert, der angibt, ob für das Schema der Streamingdaten am Anfang des Streamings Stichproben erstellt werden sollen. Standardmäßig ist dieser Wert auf „true“ festgelegt. Wenn dieser Parameter auf TRUE festgelegt ist und das Schema der Streamingdaten geändert wird, nachdem den Daten Stichproben entnommen wurden, werden neu hinzugefügte Eigenschaften aus dem Streamingdatenrahmen gelöscht. <br/><br/> Wenn der Streamingdatenrahmen schemaunabhängig sein soll, legen Sie diesen Parameter auf FALSE fest. In diesem Modus wird der Text der Dokumente, der vom Azure Cosmos DB-Änderungsfeed gelesen wurde, in die Eigenschaft „body“ des resultierenden Streamingdatenrahmens (abgesehen von Systemeigenschaftswerten) eingebunden.
 |

### <a name="connection-settings"></a>Verbindungseinstellungen

Das Java SDK unterstützt folgende Verbindungseinstellungen:

|Einstellung  |Beschreibung  |
|---------|---------|
|connectionmode   |  Legt den Verbindungsmodus fest, den der interne Client des Dokuments verwenden soll, um mit Azure Cosmos DB zu kommunizieren. Zulässige Werte sind **DirectHttps** (Standardwert) und **Gateway**. Der Verbindungsmodus „DirectHttps“ leitet die Anforderungen direkt an die CosmosDB-Partitionen um und bietet Vorteile bezüglich der Latenz.       |
|connectionmaxpoolsize   |  Legt den Wert der Größe des Verbindungspools fest, der vom internen Client des Dokuments verwendet wird. Der Standardwert ist 100.       |
|connectionidletimeout  |  Legt den Timeoutwert für Verbindungen im Leerlauf in Sekunden fest. Der Standardwert beträgt ist 60.       |
|query_maxretryattemptsonthrottledrequests    |  Legt die maximale Anzahl von Wiederholungen fest. Dieser Wert wird im Fall eines Anforderungsfehlers aufgrund einer Ratenbegrenzung im Client verwendet. Wenn dieser nicht angegeben ist, gilt der Standardwert von 1000 Wiederholungsversuchen.       |
|query_maxretrywaittimeinseconds   |  Legt die maximale Wiederholungszeit in Sekunden fest. Der Standardwert ist 1000 Sekunden.       |

### <a name="write-twitter-data-to-azure-cosmos-db"></a>Beim Schreiben von Twitter-Daten in Azure Cosmos DB 

In diesem Abschnitt führen Sie Spark-Abfragen durch, um ein Batch von Twitter-Daten in eine neue Sammlung in derselben Datenbank zu schreiben. Die HTML-Version des Notebooks ist im GitHub-Repository [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) gehostet. Laden Sie die Repositorydateien herunter, und navigieren Sie zu `\samples\Documentation_Samples\Write_Batch_Twitter_Data.html`. Sie können das Notebook in Ihr Azure Databricks-Konto importieren, die Namen des URI, des Masterschlüssels, der Datenbank und der Sammlung des Kontos aktualisieren, oder Sie können das Notebook folgendermaßen erstellen:

1. Navigieren Sie zu Ihrem Azure Databricks-Konto, und klicken Sie auf **Arbeitsbereich** > **Erstellen** > **Notebook**.  

2. Geben Sie im Dialogfeld **Create Notebook** (Notebook erstellen) einen Anzeigenamen ein, wählen Sie **Scala** als Sprache aus, und wählen Sie aus der Dropdownliste den Cluster aus, den Sie zuvor erstellt haben. Klicken Sie dann auf **Erstellen**.  

3. Aktualisieren Sie die Werte für den Endpunkt, den Masterschlüssel, die Datenbank und die Sammlung, um eine Verbindung zur Datenbanksammlung herzustellen und Twitter-Daten zu lesen und zu schreiben.

   ```scala
   %scala
   // Import Necessary Libraries
   import org.joda.time._
   import org.joda.time.format._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.config.Config

   // Maps
   val readConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB source collection name>", 
   "preferredRegions" -> "East US",
   "SamplingRatio" -> "1.0",
   "schema_samplesize" -> "200000",
   "query_custom" -> "SELECT c.id, c.created_at, c.user.screen_name, c.user.location, c.text, c.retweet_count, c.entities.hashtags, c.entities.user_mentions, c.favorited, c.source FROM c"
   )
   val writeConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB destination collection name>", 
   "preferredRegions" -> "East US",
   "SamplingRatio" -> "1.0",
   "schema_samplesize" -> "200000"
   ) 

   // Configs
   // get read
   val readConfig = Config(readConfigMap)
   val tweets = spark.read.cosmosDB(readConfig)
   tweets.createOrReplaceTempView("tweets")
   tweets.cache()

   // get write
   val writeConfig = Config(writeConfigMap)
   ```
4. Führen Sie die Abfrage aus, um die Anzahl der Tweets nach unterschiedlichen Hashtags aus den zwischengespeicherten Daten abzurufen. 

   ```scala
   %sql
   select hashtags.text, count(distinct id) as tweets
   from (
   select 
     explode(hashtags) as hashtags,
     id
   from tweets
   ) a
   group by hashtags.text
   order by tweets desc
   limit 10
   ```

5. Erstellen Sie einen neuen Datenrahmen aus Twitter-Tags, und speichern Sie die Daten in der neuen Sammlung. Nachdem Sie folgenden Code ausgeführt haben, können Sie zurück zum Portal wechseln und überprüfen, ob die Daten in die Sammlung geschrieben wurden. 

   ```scala
   %scala
   // Import SaveMode so you can Overwrite, Append, ErrorIfExists, Ignore
   import org.apache.spark.sql.{Row, SaveMode, SparkSession}

   // Create new DataFrame of tweets tags
   val tweets_bytags = spark.sql("select '2018-06-12' as currdt, hashtags.text as hashtags, count(distinct id) as tweets from ( select explode(hashtags) as hashtags, id from tweets ) a group by hashtags.text order by tweets desc limit 10")

   // Save to Cosmos DB (using Append in this case)
   // Ensure the baseConfig contains a Read-Write Key
   // The key provided in our examples is a Read-Only Key

   tweets_bytags.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
   ```

Das Java SDK unterstützt folgende Werte für die Konfigurationszuordnung:

|Einstellung  |Beschreibung  |
|---------|---------|
| BulkImport | Ein boolescher Wert, der angibt, ob Daten mithilfe der Bibliothek „BulkExecutor“ importiert werden sollen. Standardmäßig ist dieser Wert auf „true“ festgelegt. |
|WritingBatchSize  |   Gibt die Batchgröße an, die verwendet werden soll, wenn Daten in die Azure Cosmos DB-Sammlung geschrieben werden. <br/><br/> Wenn der Parameter „BulkImport“ auf TRUE festgelegt ist, gibt der Parameter „WritingBatchSize“ die Batchgröße der Dokumente an, die als Eingabe für die importAll-API der BulkExecutor-Bibliothek bereitgestellt wurden. Dieser Wert ist standardmäßig auf 100.000 festgelegt. <br/><br/> Wenn der Parameter „BulkImport“ auf FALSE festgelegt ist, gibt der Parameter „WritingBatchSize“ die Batchgröße an, die verwendet werden soll, wenn in eine Azure Cosmos DB-Sammlung geschrieben wird. Der Connector sendet createDocument- und upsertDocument-Anforderungen asynchron im Batch. Je höher die Batchgröße festgelegt ist, desto mehr Durchsatz kann erzielt werden, solange die Clusterressourcen verfügbar sind. Sie können jedoch auch eine kleinere Batchgröße angeben, um die Rate und den Verbrauch von Anforderungseinheiten zu beschränken. Standardmäßig ist die Batchgröße für Schreibvorgänge auf 500 festgelegt.  |
|Upsert   |  Eine boolesche Wertzeichenfolge, die angibt, ob „upsertDocument“ statt „createDocument“ verwendet werden soll, wenn in eine CosmosDB-Sammlung geschrieben wird.   |
| WriteThroughputBudget |  Eine ganzzahlige Zeichenfolge, die die Anzahl der Anforderungseinheiten pro Sekunde angibt, die dem Spark-Auftrag aus dem Gesamtdurchsatz der Sammlung für die Sammelerfassung zugewiesen werden sollen. |


### <a name="write-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Schreiben von Twitter-Daten, die an Azure Cosmos DB gestreamt werden 

In diesem Abschnitt führen Sie Spark-Abfragen durch, um einen Änderungsfeed von gestreamten Twitter-Daten in eine neue Sammlung in derselben Datenbank zu schreiben. Die HTML-Version des Notebooks ist im GitHub-Repository [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) gehostet. Laden Sie die Repositorydateien herunter, und navigieren Sie zu `\samples\Documentation_Samples\Write_Stream_Twitter_Data.html`. Sie können das Notebook in Ihr Azure Databricks-Konto importieren, die Namen des URI, des Masterschlüssels, der Datenbank und der Sammlung des Kontos aktualisieren, oder Sie können das Notebook folgendermaßen erstellen:

1. Navigieren Sie zu Ihrem Azure Databricks-Konto, und klicken Sie auf **Arbeitsbereich** > **Erstellen** > **Notebook**.  

2. Geben Sie im Dialogfeld **Create Notebook** (Notebook erstellen) einen Anzeigenamen ein, wählen Sie **Scala** als Sprache aus, und wählen Sie aus der Dropdownliste den Cluster aus, den Sie zuvor erstellt haben. Klicken Sie dann auf **Erstellen**.  

3. Aktualisieren Sie die Werte für den Endpunkt, den Masterschlüssel, die Datenbank und die Sammlung, um eine Verbindung zur Datenbanksammlung herzustellen und Twitter-Daten zu lesen und zu schreiben.

   ```scala
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark.config.Config
   import com.microsoft.azure.cosmosdb.spark.streaming._

   // Configure connection to Azure Cosmos DB Change Feed (Trades)
   val ConfigMap = Map(
   // Account settings
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB source collection name>", 
   // Change feed settings
   "ReadChangeFeed" -> "true",
   "ChangeFeedStartFromTheBeginning" -> "true",
   "ChangeFeedCheckpointLocation" -> "dbfs:/cosmos-feed",
   "ChangeFeedQueryName" -> "Structured Stream Read",
   "InferStreamSchema" -> "true"
   )
   ```
4. Lesen Sie den Änderungsfeed als Stream, indem Sie den Befehl „spark.readStream.format()“ verwenden:
 
   ```scala
   // Start reading change feed of trades as a stream
   var streamdata = spark
     .readStream
     .format(classOf[CosmosDBSourceProvider].getName)
     .options(ConfigMap)
     .load()
   ```

5. Definieren Sie die Konfiguration der Zielsammlung, und starten Sie den Streamingauftrag, indem Sie die Methode „writeStream.format()“ verwenden:

   ```scala
   val sinkConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB destination collection name>", 
   "checkpointLocation" -> "streamingcheckpointlocation6",
   "WritingBatchSize" -> "100",
   "Upsert" -> "true")

   // Start the stream writer
   val streamingQueryWriter = streamdata
    .writeStream
    .format(classOf[CosmosDBSinkProvider].getName)
    .outputMode("append")
    .options(sinkConfigMap)
    .start()
 ```

Das Java SDK unterstützt folgende Werte für die Konfigurationszuordnung:

|Einstellung  |Beschreibung  |
|---------|---------|
|Upsert   |  Eine boolesche Wertzeichenfolge, die angibt, ob „upsertDocument“ statt „createDocument“ verwendet werden soll, wenn in eine CosmosDB-Sammlung geschrieben wird.   |
|checkpointlocation  |   Ein Pfad zum lokalen Speicherplatz, um Fortsetzungstokens im Fall von Knotenausfällen beizubehalten.   |
|WritingBatchSize  |  Gibt die Batchgröße an, die verwendet werden soll, wenn Daten in die Azure Cosmos DB-Sammlung geschrieben werden. Der Connector sendet createDocument- und upsertDocument-Anforderungen asynchron im Batch. Je höher die Batchgröße festgelegt ist, desto mehr Durchsatz kann erzielt werden, solange die Clusterressourcen verfügbar sind. Sie können jedoch auch eine kleinere Batchgröße angeben, um die Rate und den Verbrauch von Anforderungseinheiten zu beschränken. Standardmäßig ist die Batchgröße für Schreibvorgänge auf 500 festgelegt.  |


## <a name="considerations-when-using-java-sdk"></a>Überlegungen zur Verwendung des Java SDK

Das Herstellen einer Verbindung zwischen Spark und Azure Cosmos DB mithilfe des Java SDK wird in folgenden Szenarios empfohlen:

* Sie möchten Python und/oder Scala verwenden.  

* Sie müssen eine große Datenmenge zwischen Apache Spark und Azure Cosmos DB übertragen. Hierbei ist die Leistung von Java SDK im Vergleich zu pyDocumentDB höher. Informationen zum Unterschied bei der Abfrageleistung finden Sie im [Wiki für Abfragetestläufe](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="next-steps"></a>Nächste Schritte

Laden Sie – falls noch nicht geschehen – den Spark-Connector für Azure Cosmos DB aus dem GitHub-Repository [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) herunter, und sehen Sie sich die zusätzlichen Ressourcen im Repository an:

* [Beispiele für verteilte Aggregationen](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Beispielskripts und Notebooks](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

Es kann auch ratsam sein, die Informationen unter [Apache Spark SQL, DataFrames, and Datasets Guide](http://spark.apache.org/docs/latest/sql-programming-guide.html) (Leitfaden zu Apache Spark SQL, DataFrames und Datasets) und den Artikel [Apache Spark on Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) zu lesen.
