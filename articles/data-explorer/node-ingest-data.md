---
title: 'Schnellstart: Erfassen von Daten mit der Azure-Daten-Explorer-Bibliothek für Node'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Node.js Daten in Azure Data Explorer erfassen (laden).
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 10/25/2018
ms.openlocfilehash: fa322ee685d09717ac5b98398d4d1d61de2be1e9
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706625"
---
# <a name="quickstart-ingest-data-using-the-azure-data-explorer-node-library"></a>Schnellstart: Erfassen von Daten mit der Azure-Daten-Explorer-Bibliothek für Node

Azure-Daten-Explorer ist ein schneller und hochgradig skalierbarer Dienst zur Untersuchung von Daten (Protokoll- und Telemetriedaten). Azure Data Explorer bietet zwei Clientbibliotheken für Node: eine [Erfassungsbibliothek](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-ingest) und eine [Datenbibliothek](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-data). Mit diesen Bibliotheken können Sie über Ihren Code Daten in einem Cluster erfassen (laden) und Daten abfragen. In diesem Schnellstart erstellen Sie zunächst eine Tabelle und eine Datenzuordnung in einem Testcluster. Anschließend stellen Sie die Erfassung im Cluster in eine Warteschlange und überprüfen die Ergebnisse.

Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Zusätzlich zu einem Azure-Abonnement benötigen Sie zum Abschließen dieses Schnellstarts Folgendes:

* [Einen Testcluster und eine Testdatenbank](create-cluster-database-portal.md)

* [Node.js](https://nodejs.org/en/download/) auf dem Entwicklungscomputer

## <a name="install-the-data-and-ingest-libraries"></a>Installieren der Daten- und der Erfassungsbibliothek

Installieren Sie *azure-kusto-ingest* und *azure-kusto-data*.

```bash
npm i --save azure-kusto-ingest azure-kusto-data
```

## <a name="add-import-statements-and-constants"></a>Hinzufügen von Importanweisungen und Konstanten

Importieren Sie Klassen aus den Bibliotheken.

```javascript
const KustoClient = require('azure-kusto-data').KustoClient;
const KustoIngestClient = require('azure-kusto-ingest').KustoIngestClient;
const KustoConnectionStringBuilder = require('azure-kusto-ingest').KustoConnectionStringBuilder;
```

Für die Authentifizierung von Anwendungen verwendet Azure Data Explorer Ihre Azure Active Directory-Mandanten-ID. Informationen zum Ermitteln Ihrer Mandanten-ID finden Sie unter [Find your Office 365 tenant ID](https://docs.microsoft.com/onedrive/find-your-office-365-tenant-id) (Ermitteln Ihrer Office 365-Mandanten-ID).

Legen Sie vor dem Ausführen dieses Codes die Werte für `authorityId`, `kustoUri`, `kustoIngestUri` und `kustoDatabase` fest.

```javascript
const authorityId = "<TenantId>";
const kustoUri = "https://<ClusterName>.<Region>.kusto.windows.net:443/";
const kustoIngestUri = "https://ingest-<ClusterName>.<Region>.kusto.windows.net:443/"
const kustoDatabase  = "<DatabaseName>"
```

Erstellen Sie nun die Verbindungszeichenfolge. In diesem Beispiel wird die Geräteauthentifizierung zum Zugreifen auf den Cluster verwendet. Sie können auch das Azure Active Directory-Anwendungszertifikat, den Anwendungsschlüssel und den Benutzer mit dem zugehörigen Kennwort verwenden.

Sie erstellen die Zieltabelle und die Zuordnung in einem späteren Schritt.

```javascript
const kcsbIngest = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoIngestUri, authorityId);

const kcsbData = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoUri, authorityId);

const destTable = "StormEvents";
const destTableMapping = "StormEvents_CSV_Mapping";
```

## <a name="set-source-file-information"></a>Festlegen der Informationen zur Quelldatei

Importieren Sie zusätzliche Klassen, und legen Sie Konstanten für die Datenquellendatei fest. In diesem Beispiel wird eine Beispieldatei verwendet, die in Azure Blob Storage gehostet wird. Das **StormEvents**-Beispieldataset enthält wetterbezogene Daten der [National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/).

```javascript
from azure.storage.blob import BlockBlobService
from azure.kusto.ingest import KustoIngestClient, IngestionProperties, FileDescriptor, BlobDescriptor, DataFormat, ReportLevel, ReportMethod

const container = "samplefiles";
const account = "kustosamplefiles";
const sas = "?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D"
const filePath = "StormEvents.csv";
const fileSize = 64158321    # in bytes

const blobPath = `https://${account}.blob.core.windows.net/${container}/${filePath}${sas}";
```

## <a name="create-a-table-on-your-test-cluster"></a>Erstellen einer Tabelle im Testcluster

Erstellen Sie eine Tabelle, die dem Schema der Daten in der Datei `StormEvents.csv` entspricht. Wenn dieser Code ausgeführt wird, gibt er eine Meldung wie die folgende zurück: *Verwenden Sie zum Anmelden einen Webbrowser, um die Seite https://microsoft.com/devicelogin zu öffnen, und geben Sie den Code XXXXXXXXX ein, um sich zu authentifizieren*. Befolgen Sie die Schritte für die Anmeldung, und kehren Sie dann zurück, um den Codeblock auszuführen. Für nachfolgende Codeblöcke, die eine Verbindung herstellen, müssen Sie sich wieder anmelden.

```javascript
const kustoClient = new KustoClient(kcsbData);
const createTableCommand = ".create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)"

kustoClient.executeMgmt(kustoDatabase, createTableCommand, (err, results) => {
    console.log(result.primaryResults[0]);
});
```

## <a name="define-ingestion-mapping"></a>Definieren der Erfassungszuordnung

Ordnen Sie eingehende JSON-Daten den beim Erstellen der Tabelle verwendeten Spaltennamen und Datentypen zu.

```javascript
const createMappingCommand = `.create table StormEvents ingestion csv mapping ${destTableMapping} '[{\"Name\":\"StartTime\",\"datatype\":\"datetime\",\"Ordinal\":0}, {\"Name\":\"EndTime\",\"datatype\":\"datetime\",\"Ordinal\":1},{\"Name\":\"EpisodeId\",\"datatype\":\"int\",\"Ordinal\":2},{\"Name\":\"EventId\",\"datatype\":\"int\",\"Ordinal\":3},{\"Name\":\"State\",\"datatype\":\"string\",\"Ordinal\":4},{\"Name\":\"EventType\",\"datatype\":\"string\",\"Ordinal\":5},{\"Name\":\"InjuriesDirect\",\"datatype\":\"int\",\"Ordinal\":6},{\"Name\":\"InjuriesIndirect\",\"datatype\":\"int\",\"Ordinal\":7},{\"Name\":\"DeathsDirect\",\"datatype\":\"int\",\"Ordinal\":8},{\"Name\":\"DeathsIndirect\",\"datatype\":\"int\",\"Ordinal\":9},{\"Name\":\"DamageProperty\",\"datatype\":\"int\",\"Ordinal\":10},{\"Name\":\"DamageCrops\",\"datatype\":\"int\",\"Ordinal\":11},{\"Name\":\"Source\",\"datatype\":\"string\",\"Ordinal\":12},{\"Name\":\"BeginLocation\",\"datatype\":\"string\",\"Ordinal\":13},{\"Name\":\"EndLocation\",\"datatype\":\"string\",\"Ordinal\":14},{\"Name\":\"BeginLat\",\"datatype\":\"real\",\"Ordinal\":16},{\"Name\":\"BeginLon\",\"datatype\":\"real\",\"Ordinal\":17},{\"Name\":\"EndLat\",\"datatype\":\"real\",\"Ordinal\":18},{\"Name\":\"EndLon\",\"datatype\":\"real\",\"Ordinal\":19},{\"Name\":\"EpisodeNarrative\",\"datatype\":\"string\",\"Ordinal\":20},{\"Name\":\"EventNarrative\",\"datatype\":\"string\",\"Ordinal\":21},{\"Name\":\"StormSummary\",\"datatype\":\"dynamic\",\"Ordinal\":22}]'`;

kustoClient.executeMgmt(kustoDatabase, createMappingCommand, (err, results) => {
    console.log(result.primaryResults[0]);
});
```

## <a name="queue-a-message-for-ingestion"></a>Stellen einer Nachricht in eine Warteschlange für die Erfassung

Stellen Sie eine Nachricht zum Abrufen von Daten aus Blob Storage in eine Warteschlange, und erfassen Sie diese Daten im Azure-Daten-Explorer.

```javascript
const { DataFormat, JsonColumnMapping } =  require("azure-kusto-ingest").IngestionPropertiesEnums;
const { BlobDescriptor } = require("azure-kusto-ingest").Descriptors;
const ingestClient = new KustoIngestClient(kcsbIngest);


// All ingestion properties are documented here: https://docs.microsoft.com/azure/kusto/management/data-ingest#ingestion-properties
const ingestionProps  = new IngestionProperties(kustoDatabase, destTable, DataFormat.csv, null,destTableMapping, {'ignoreFirstRecord': 'true'});
const blobDesc = new BlobDescriptor(blobPath, 10);
ingestClient.ingestFromBlob(blobDesc,ingestionProps, (err) => {
    if (err) throw new Error(err);
});
```

## <a name="validate-that-table-contains-data"></a>Überprüfen, ob die Tabelle Daten enthält

Überprüfen Sie, ob die Daten in der Tabelle erfasst wurden. Warten Sie fünf bis zehn Minuten, bis die Warteschlange die Erfassung geplant und die Daten in den Azure-Daten-Explorer geladen hat. Führen Sie dann den folgenden Code aus, um die Anzahl von Datensätzen in der Tabelle `StormEvents` zu erhalten.

```javascript
const query = "StormEvents | count";

kustoClient.execute(kustoDatabse, query, (err, results) => {
    if (err) throw new Error(err);  
    console.log(results.primaryResults[0].toString());
});
```

## <a name="run-troubleshooting-queries"></a>Ausführen von Abfragen zur Problembehandlung

Melden Sie sich bei [https://dataexplorer.azure.com](https://dataexplorer.azure.com) an, und stellen Sie eine Verbindung mit Ihrem Cluster her. Führen Sie den folgenden Befehl in Ihrer Datenbank aus, um festzustellen, ob in den letzten vier Stunden Erfassungsfehler aufgetreten sind. Ersetzen Sie den Namen der Datenbank vor dem Ausführen.
    
```Kusto
.show ingestion failures
| where FailedOn > ago(4h) and Database == "<DatabaseName>"
```

Führen Sie den folgenden Befehl aus, um den Status aller Erfassungsvorgänge in den letzten vier Stunden anzuzeigen. Ersetzen Sie den Namen der Datenbank vor dem Ausführen.

```Kusto
.show operations
| where StartedOn > ago(4h) and Database == "<DatabaseName>" and Operation == "DataIngestPull"
| summarize arg_max(LastUpdatedOn, *) by OperationId
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie unsere anderen Schnellstarts und Tutorials durchgehen möchten, behalten Sie die erstellten Ressourcen bei. Wenn dies nicht der Fall ist, führen Sie den folgenden Befehl in der Datenbank aus, um die Tabelle `StormEvents` zu bereinigen.

```Kusto
.drop table StormEvents
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Write queries](write-queries.md) (Schreiben von Abfragen)
