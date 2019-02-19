---
title: 'Tutorial: Erfassen von Diagnose- und Aktivitätsprotokolldaten in Azure Data Explorer ohne jeglichen Code'
description: In diesem Tutorial wird beschrieben, wie Sie Daten in Azure Data Explorer erfassen, ohne Code verwenden zu müssen, und diese Daten dann abfragen.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: tutorial
ms.date: 2/5/2019
ms.openlocfilehash: 39019c4b11d055aa8f550928bd677e4ce33d6252
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885267"
---
# <a name="tutorial-ingest-data-in-azure-data-explorer-without-one-line-of-code"></a>Tutorial: Erfassen von Daten in Azure Data Explorer ohne jeglichen Code

In diesem Tutorial wird beschrieben, wie Sie Diagnose- und Aktivitätsprotokolldaten in einem Azure Data Explorer-Cluster erfassen, ohne jeglichen Code verwenden zu müssen. Mit dieser einfachen Erfassungsmethode können Sie schnell damit beginnen, Azure Data Explorer zu Datenanalysezwecken abzufragen.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen von Tabellen und einer Erfassungszuordnung in einer Azure Data Explorer-Datenbank
> * Formatieren der erfassten Daten mit einer Updaterichtlinie
> * Erstellen eines [Event Hub](/azure/event-hubs/event-hubs-about) und Herstellen einer Verbindung mit Azure Data Explorer
> * Streamen von Daten an einen Event Hub aus [Azure Monitor-Diagnoseprotokollen](/azure/azure-monitor/platform/diagnostic-logs-overview) und [Azure Monitor-Aktivitätsprotokollen](/azure/azure-monitor/platform/activity-logs-overview)
> * Abfragen der erfassten Daten mit Azure Data Explorer

> [!NOTE]
> Erstellen aller Ressourcen an demselben Azure-Standort bzw. in derselben Region. Dies ist eine Anforderung für Azure Monitor-Diagnoseprotokolle.

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* [Schnellstart: Erstellen eines Azure Data Explorer-Clusters und einer Datenbank](create-cluster-database-portal.md). In diesem Tutorial lautet der Datenbankname *AzureMonitoring*.

## <a name="azure-monitoring-data-provider---diagnostic-and-activity-logs"></a>Datenanbieter für Azure-Überwachung: Diagnose- und Aktivitätsprotokolle

Hier können Sie die Daten anzeigen, die von den Diagnose- und Aktivitätsprotokollen der Azure-Überwachung bereitgestellt werden, und sich damit vertraut machen. Basierend auf diesen Datenschemas erstellen wir eine Erfassungspipeline.

### <a name="diagnostic-logs-example"></a>Beispiel für Diagnoseprotokolle

Bei Azure-Diagnoseprotokollen handelt es sich um Metriken, die von einem Azure-Dienst ausgegeben werden und Daten zum Betrieb dieses Diensts liefern. Daten werden mit einem Aggregationsintervall von einer Minute aggregiert. Jedes Diagnoseprotokollereignis enthält einen Datensatz. Hier ist ein Beispiel für ein Azure Data Explorer-Metrikereignisschema zur Abfragedauer angegeben:

```json
{
    "count": 14,
    "total": 0,
    "minimum": 0,
    "maximum": 0,
    "average": 0,
    "resourceId": "/SUBSCRIPTIONS/F3101802-8C4F-4E6E-819C-A3B5794D33DD/RESOURCEGROUPS/KEDAMARI/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/KEREN",
    "time": "2018-12-20T17:00:00.0000000Z",
    "metricName": "QueryDuration",
    "timeGrain": "PT1M"
}
```

### <a name="activity-logs-example"></a>Beispiel für Aktivitätsprotokoll

Azure-Aktivitätsprotokolle sind Protokolle auf Abonnementebene, die eine Sammlung mit Datensätzen enthalten. Die Protokolle liefern Erkenntnisse zu den Vorgängen, die für Ressourcen Ihres Abonnements durchgeführt wurden. Im Gegensatz zu Diagnoseprotokollen verfügt ein Aktivitätsprotokollereignis über ein Array mit Datensätzen. Zu einem späteren Zeitpunkt des Tutorials müssen wir dieses Array aufteilen. Hier ist ein Beispiel für ein Aktivitätsprotokollereignis zur Überprüfung des Zugriffs angegeben:

```json
{
    "records": [
    {
        "time": "2018-12-26T16:23:06.1090193Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Start",
        "resultSignature": "Started.",
        "durationMs": 0,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            ...
        }
    },
    {
        "time": "2018-12-26T16:23:06.3040244Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Success",
        "resultSignature": "Succeeded.OK",
        "durationMs": 194,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            "statusCode": "OK",
            "serviceRequestId": "87acdebc-945f-4c0c-b931-03050e085626"
        }
    }]
}
```

## <a name="set-up-ingestion-pipeline-in-azure-data-explorer"></a>Einrichten einer Erfassungspipeline in Azure Data Explorer 

Die Einrichtung der Azure Data Explorer-Pipeline umfasst verschiedene Schritte, z. B. die [Tabellenerstellung und Datenerfassung](/azure/data-explorer/ingest-sample-data#ingest-data). Sie können die Daten auch ändern, zuordnen und aktualisieren.

### <a name="connect-to-azure-data-explorer-web-ui"></a>Herstellen einer Verbindung mit der Azure Data Explorer-Webbenutzeroberfläche

1. Wählen Sie in der Azure Data Explorer-Datenbank *AzureMonitoring* die Option **Abfrage**, um die Azure Data Explorer-Webbenutzeroberfläche zu öffnen.

    ![Abfragen](media/ingest-data-no-code/query-database.png)

### <a name="create-target-tables"></a>Erstellen von Zieltabellen

Verwenden Sie die Azure Data Explorer-Webbenutzeroberfläche, um die Zieltabellen in der Azure Data Explorer-Datenbank zu erstellen.

#### <a name="diagnostic-logs-table"></a>Tabelle für Diagnoseprotokolle

1. Erstellen Sie in der Datenbank *AzureMonitoring* die Tabelle *DiagnosticLogsRecords*, in die die Datensätze des Diagnoseprotokolls eingefügt werden, indem Sie den Steuerungsbefehl `.create table` verwenden:

    ```kusto
    .create table DiagnosticLogsRecords (Timestamp:datetime, ResourceId:string, MetricName:string, Count:int, Total:double, Minimum:double, Maximum:double, Average:double, TimeGrain:string)
    ```

1. Wählen Sie **Ausführen**, um die Tabelle zu erstellen.

    ![Ausführen der Abfrage](media/ingest-data-no-code/run-query.png)

#### <a name="activity-logs-tables"></a>Tabellen für Aktivitätsprotokolle

Da die Struktur von Aktivitätsprotokollen nicht tabellarisch ist, müssen Sie die Daten bearbeiten und jedes Ereignis auf mindestens einen Datensatz erweitern. Die Rohdaten werden in der Zwischentabelle *ActivityLogsRawRecords* erfasst. Die Daten werden nun bearbeitet und erweitert. Die erweiterten Daten werden anschließend mit einer Updaterichtlinie in der Tabelle *ActivityLogsRecords* erfasst. Aus diesem Grund müssen Sie für die Erfassung von Aktivitätsprotokollen zwei separate Tabellen erstellen.

1. Erstellen Sie in der Datenbank *AzureMonitoring* die Tabelle *ActivityLogsRecords*, in die die Datensätze von Aktivitätsprotokollen eingefügt werden. Führen Sie die folgende Azure Data Explorer-Abfrage aus, um die Tabelle zu erstellen:

    ```kusto
    .create table ActivityLogsRecords (Timestamp:datetime, ResourceId:string, OperationName:string, Category:string, ResultType:string, ResultSignature:string, DurationMs:int, IdentityAuthorization:dynamic, IdentityClaims:dynamic, Location:string, Level:string)
    ```

1. Erstellen Sie die Zwischentabelle *ActivityLogsRawRecords* für Daten in der Datenbank *AzureMonitoring*, um die Bearbeitung der Daten zu ermöglichen:

    ```kusto
    .create table ActivityLogsRawRecords (Records:dynamic)
    ```

<!--
     ```kusto
     .alter-merge table ActivityLogsRawRecords policy retention softdelete = 0d
    <[Retention](/azure/kusto/management/retention-policy) for an intermediate data table is set at zero retention policy.
-->

### <a name="create-table-mappings"></a>Erstellen von Tabellenzuordnungen

 Da das Datenformat `json` lautet, ist eine Datenzuordnung erforderlich. Mit der `json`-Zuordnung wird jeder JSON-Pfad einem Tabellenspaltennamen zugeordnet.

#### <a name="diagnostic-logs-table-mapping"></a>Tabellenzuordnung für Diagnoseprotokolle

Verwenden Sie die folgende Abfrage, um die Daten der Tabelle zuzuordnen:

```kusto
.create table DiagnosticLogsRecords ingestion json mapping 'DiagnosticLogsRecordsMapping' '[
{"column":"Timestamp","path":"$.time"},
{"column":"ResourceId","path":"$.resourceId"},
{"column":"MetricName","path":"$.metricName"},
{"column":"Count","path":"$.count"},
{"column":"Total","path":"$.total"},
{"column":"Minimum","path":"$.minimum"},
{"column":"Maximum","path":"$.maximum"},
{"column":"Average","path":"$.average"},
{"column":"TimeGrain","path":"$.timeGrain"}]'
```

#### <a name="activity-logs-table-mapping"></a>Tabellenzuordnung für Aktivitätsprotokolle

Verwenden Sie die folgende Abfrage, um die Daten der Tabelle zuzuordnen:

```kusto
.create table ActivityLogsRawRecords ingestion json mapping 'ActivityLogsRawRecordsMapping' '[
{"column":"Records","path":"$.records"}]'
```

### <a name="create-update-policy"></a>Erstellen einer Updaterichtlinie

1. Erstellen Sie eine [Funktion](/azure/kusto/management/functions), mit der die Datensatzsammlung so erweitert wird, dass jeder Wert der Sammlung in einer separaten Zeile angeordnet wird. Verwenden Sie den Operator [`mvexpand`](/azure/kusto/query/mvexpandoperator):

    ```kusto
    .create function ActivityLogRecordsExpand() {
        ActivityLogsRawRecords
        | mvexpand events = Records
        | project
            Timestamp = todatetime(events["time"]),
            ResourceId = tostring(events["resourceId"]),
            OperationName = tostring(events["operationName"]),
            Category = tostring(events["category"]),
            ResultType = tostring(events["resultType"]),
            ResultSignature = tostring(events["resultSignature"]),
            DurationMs = toint(events["durationMs"]),
            IdentityAuthorization = events["identity.authorization"],
            IdentityClaims = events["identity.claims"],
            Location = tostring(events["location"]),
            Level = tostring(events["level"])
    }
    ```

2. Fügen Sie der Zieltabelle eine [Updaterichtlinie](/azure/kusto/concepts/updatepolicy) hinzu. Hiermit wird die Abfrage automatisch für alle neu erfassten Daten in der Zwischentabelle *ActivityLogsRawRecords* ausgeführt, und die Ergebnisse werden in der Tabelle *ActivityLogsRecords* erfasst:

    ```kusto
    .alter table ActivityLogRecords policy update @'[{"Source": "ActivityLogsRawRecords", "Query": "ActivityLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

## <a name="create-an-event-hub-namespace"></a>Erstellen eines Event Hub-Namespace

Mit Azure-Diagnoseprotokollen können Metriken in ein Speicherkonto oder einen Event Hub exportiert werden. In diesem Tutorial leiten wir Metriken über einen Event Hub weiter. Sie erstellen mit den folgenden Schritten einen Event Hubs-Namespace und einen Event Hub für Diagnoseprotokolle. Bei der Azure-Überwachung wird der Event Hub *insights-operational-logs* für Aktivitätsprotokolle erstellt.

1. Erstellen Sie einen Event Hub, indem Sie im Azure-Portal eine Azure Resource Manager-Vorlage verwenden. Verwenden Sie die folgende Schaltfläche, um die Bereitstellung zu starten. Klicken Sie mit der rechten Maustaste, und wählen Sie **In neuem Fenster öffnen**, damit Sie die restlichen Schritte in diesem Artikel ausführen können. Mit der Schaltfläche **In Azure bereitstellen** gelangen Sie zum Azure-Portal.

    [![In Azure bereitstellen](media/ingest-data-no-code/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

1. Erstellen Sie einen Event Hub-Namespace und einen Event Hub für die Diagnoseprotokolle.

    ![Event Hub-Erstellung](media/ingest-data-no-code/event-hub.png)

    Füllen Sie das Formular mit den folgenden Informationen aus. Verwenden Sie für alle nicht in der folgenden Tabelle aufgeführten Einstellungen die jeweiligen Standardwerte.

    **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**
    |---|---|---|
    | Abonnement | Ihr Abonnement | Wählen Sie das Azure-Abonnement aus, das Sie für Ihren Event Hub verwenden möchten.|
    | Ressourcengruppe | *test-resource-group* | Erstellen Sie eine neue Ressourcengruppe. |
    | Standort | Wählen Sie die Region aus, die Ihre Anforderungen am besten erfüllt. | Erstellen Sie den Event Hub-Namespace an demselben Standort wie die anderen Ressourcen.
    | Namespacename | *AzureMonitoringData* | Wählen Sie einen eindeutigen Namen, der Ihren Namespace identifiziert.
    | Event Hub-Name | *DiagnosticLogsData* | Der Event Hub befindet sich unter dem Namespace, der einen eindeutigen Bereichscontainer bereitstellt. |
    | Name der Consumergruppe | *adxpipeline* | Erstellen Sie einen Consumergruppennamen. Hiermit können mehrere verarbeitende Anwendungen jeweils über eine separate Ansicht des Ereignisdatenstroms verfügen. |
    | | |

## <a name="connect-azure-monitoring-logs-to-event-hub"></a>Herstellen einer Verbindung für Protokolle der Azure-Überwachung mit einem Event Hub

### <a name="diagnostic-logs-connection-to-event-hub"></a>Verbindung von Diagnoseprotokollen mit einem Event Hub

Wählen Sie eine Ressource aus, für die Metriken exportiert werden sollen. Es gibt mehrere Ressourcentypen, die das Exportieren von Diagnoseprotokollen ermöglichen, z. B. Event Hub-Namespace, KeyVault, IoT Hub und Azure Data Explorer-Cluster. In diesem Tutorial verwenden wir den Azure Data Explorer-Cluster als unsere Ressource.

1. Wählen Sie im Azure-Portal Ihren Kusto-Cluster aus.

    ![Diagnoseeinstellungen](media/ingest-data-no-code/diagnostic-settings.png)

1. Wählen Sie im Menü auf der linken Seite die Option **Diagnoseeinstellungen**.
1. Klicken Sie auf den Link **Diagnose aktivieren**. Das Fenster **Diagnoseeinstellungen** wird geöffnet.

    ![Fenster „Diagnoseeinstellungen“](media/ingest-data-no-code/diagnostic-settings-window.png)

1. Im Bereich **Diagnoseeinstellungen**:
    1. Vergeben Sie einen Namen für Ihre Diagnoseprotokolldaten: *ADXExportedData*
    1. Aktivieren Sie das Kontrollkästchen **AllMetrics** (optional).
    1. Aktivieren Sie das Kontrollkästchen **An einen Event Hub streamen**.
    1. Klicken Sie auf **Konfigurieren**.

1. Konfigurieren Sie im Bereich **Event Hub auswählen** den Export für den von Ihnen erstellten Event Hub:
    1. Wählen Sie für **Event Hub-Namespace auswählen** in der Dropdownliste die Option *AzureMonitoringData*.
    1. Wählen Sie für **Event Hub-Name auswählen** in der Dropdownliste die Option *diagnosticlogsdata*.
    1. Wählen Sie **Event Hub-Richtlinienname auswählen** in der Dropdownliste.
    1. Klicken Sie auf **OK**.

1. Klicken Sie auf **Speichern**. Im Fenster werden der Namespace, Name und Richtlinienname für den Event Hub angezeigt.

    ![Speichern der Diagnoseeinstellungen](media/ingest-data-no-code/save-diagnostic-settings.png)

### <a name="activity-logs-connection-to-event-hub"></a>Verbindung von Aktivitätsprotokollen mit einem Event Hub

1. Wählen Sie im Azure-Portal auf der linken Seite die Option **Aktivitätsprotokoll**.
1. Das Fenster **Aktivitätsprotokoll** wird geöffnet. Klicken Sie auf **In Event Hub exportieren**.

    ![Aktivitätsprotokoll](media/ingest-data-no-code/activity-log.png)

1. Im Fenster **Aktivitätsprotokoll exportieren**:
 
    ![Exportieren des Aktivitätsprotokolls](media/ingest-data-no-code/export-activity-log.png)

    1. Wählen Sie Ihr Abonnement aus.
    1. Wählen Sie in der Dropdownliste **Regionen** die Option **Alle auswählen**.
    1. Aktivieren Sie das Kontrollkästchen **In einen Event Hub exportieren**.
    1. Klicken Sie auf **Service Bus-Namespace auswählen**, um den Bereich **Event Hub auswählen** zu öffnen.
    1. Wählen Sie im Bereich **Event Hub auswählen** in den Dropdownmenüs Folgendes aus: Ihr Abonnement, Ihren Event Hub-Namespace *AzureMonitoringData* und den Event Hub-Standardrichtliniennamen.
    1. Klicken Sie auf **OK**.
    1. Klicken Sie oben rechts im Fenster auf **Speichern**. Ein Event Hub mit dem Namen *insights-operational-logs* wird erstellt.

### <a name="see-data-flowing-to-your-event-hubs"></a>Verfolgen des Datenflusses zu Ihren Event Hubs

1. Warten Sie einige Minuten, bis die Verbindung definiert wurde und der Export der Aktivitätsprotokolle für den Event Hub abgeschlossen ist. Navigieren Sie zu Ihrem Event Hub-Namespace, um die von Ihnen erstellten Event Hubs anzuzeigen.

    ![Erstellte Event Hubs](media/ingest-data-no-code/event-hubs-created.png)

1. Verfolgen Sie den Datenfluss zu Ihrem Event Hub:

    ![Event Hub-Daten](media/ingest-data-no-code/event-hubs-data.png)

## <a name="connect-event-hub-to-azure-data-explorer"></a>Herstellen einer Verbindung für ein Event Hub mit Azure Data Explorer

### <a name="diagnostic-logs-data-connection"></a>Datenverbindung für Diagnoseprotokolle

1. Wählen Sie in Ihrem Azure Data Explorer-Cluster *kustodocs* im Menü auf der linken Seite die Option **Datenbanken**.
1. Wählen Sie im Fenster **Datenbanken** Ihren Datenbanknamen *AzureMonitoring* aus.
1. Wählen Sie im Menü auf der linken Seite die Option **Datenerfassung**.
1. Klicken Sie im Fenster **Datenerfassung** auf **+ Datenverbindung hinzufügen**.
1. Geben Sie im Fenster **Datenverbindung** die folgenden Informationen ein:

    ![Event Hub-Verbindung](media/ingest-data-no-code/event-hub-data-connection.png)

    Datenquelle:

    **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**
    |---|---|---|
    | Name der Datenverbindung | *DiagnosticsLogsConnection* | Der Name der Verbindung, die Sie im Azure-Daten-Explorer erstellen möchten.|
    | Event Hub-Namespace | *AzureMonitoringData* | Der von Ihnen zuvor ausgewählte Name, der Ihren Namespace identifiziert. |
    | Event Hub | *diagnosticlogsdata* | Der von Ihnen erstellte Event Hub. |
    | Consumergruppe | *adxpipeline* | Die Consumergruppe, die in dem von Ihnen erstellten Event Hub definiert ist. |
    | | |

    Zieltabelle:

    Es stehen zwei Routingoptionen zur Verfügung: *statisch* und *dynamisch*. In diesem Tutorial wird statisches Routing (Standardeinstellung) verwendet, für das der Tabellenname, das Dateiformat und die Zuordnung angegeben werden müssen. Lassen Sie das Kontrollkästchen **My data includes routing info** (Meine Daten enthalten Routinginformationen) daher deaktiviert.

     **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**
    |---|---|---|
    | Table | *DiagnosticLogsRecords* | Die Tabelle, die Sie in der Datenbank *AzureMonitoring* erstellt haben. |
    | Datenformat | *JSON* | Das Format in der Tabelle. |
    | Spaltenzuordnung | *DiagnosticLogsRecordsMapping* | Die Zuordnung, die Sie in der Datenbank *AzureMonitoring* erstellt haben und mit der eingehende JSON-Daten den Spaltennamen und Datentypen von *DiagnosticLogsRecords* zugeordnet werden.|
    | | |

1. Klicken Sie auf **Erstellen**  

### <a name="activity-logs-data-connection"></a>Datenverbindung für Aktivitätsprotokolle

Wiederholen Sie die Schritte im Abschnitt [Datenverbindung für Diagnoseprotokolle](#diagnostic-logs-data-connection), um eine Datenverbindung für Aktivitätsprotokolle zu erstellen.

1. Fügen Sie die folgenden Einstellungen im Fenster **Datenverbindung** ein:

    Datenquelle:

    **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**
    |---|---|---|
    | Name der Datenverbindung | *ActivityLogsConnection* | Der Name der Verbindung, die Sie im Azure-Daten-Explorer erstellen möchten.|
    | Event Hub-Namespace | *AzureMonitoringData* | Der von Ihnen zuvor ausgewählte Name, der Ihren Namespace identifiziert. |
    | Event Hub | *insights-operational-logs* | Der von Ihnen erstellte Event Hub. |
    | Consumergruppe | *$Default* | Die Standardconsumergruppe. Bei Bedarf können Sie auch eine andere Consumergruppe erstellen. |
    | | |

    Zieltabelle:

    Es stehen zwei Routingoptionen zur Verfügung: *statisch* und *dynamisch*. In diesem Tutorial wird statisches Routing (Standardeinstellung) verwendet, für das der Tabellenname, das Dateiformat und die Zuordnung angegeben werden müssen. Lassen Sie das Kontrollkästchen **My data includes routing info** (Meine Daten enthalten Routinginformationen) daher deaktiviert.

     **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**
    |---|---|---|
    | Table | *ActivityLogsRawRecords* | Die Tabelle, die Sie in der Datenbank *AzureMonitoring* erstellt haben. |
    | Datenformat | *JSON* | Das Format in der Tabelle. |
    | Spaltenzuordnung | *ActivityLogsRawRecordsMapping* | Die Zuordnung, die Sie in der Datenbank *AzureMonitoring* erstellt haben und mit der eingehende JSON-Daten den Spaltennamen und Datentypen von *ActivityLogsRawRecords* zugeordnet werden.|
    | | |

1. Klicken Sie auf **Erstellen**  

## <a name="query-the-new-tables"></a>Abfragen der neuen Tabellen

Sie verfügen über eine Pipeline, durch die Daten fließen. Da die Erfassung über den Cluster standardmäßig fünf Minuten dauert, sollten Sie den Datenfluss einige Minuten lang zulassen, bevor Sie mit dem Abfragen beginnen.

### <a name="diagnostic-logs-table-query-example"></a>Beispiel: Abfragen der Tabelle für Diagnoseprotokolle

Mit der folgenden Abfrage werden Daten zur Abfragedauer aus den Datensätzen von Azure Data Explorer-Diagnoseprotokollen analysiert:

```kusto
DiagnosticLogsRecords
| where Timestamp > ago(15m) and MetricName == 'QueryDuration'
| summarize avg(Average)
```

Abfrageergebnisse:

|   |   |
| --- | --- |
|   |  avg_Average |
|   | 00:06.156 |
| | |

### <a name="activity-logs-table-query-example"></a>Beispiel: Abfragen der Tabelle für Aktivitätsprotokolle

Mit der folgenden Abfrage werden die Daten aus den Datensätzen von Azure Data Explorer-Aktivitätsprotokollen analysiert:

```kusto
ActivityLogsRecords
| where OperationName == 'MICROSOFT.EVENTHUB/NAMESPACES/AUTHORIZATIONRULES/LISTKEYS/ACTION'
| where ResultType == 'Success'
| summarize avg(DurationMs)
```

Abfrageergebnisse:
|   |   |
| --- | --- |
|   |  avg(DurationMs) |
|   | 768.333 |
| | |

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie Sie viele weitere Abfragen für Daten schreiben können, die Sie aus Azure Data Explorer extrahieren, indem Sie den folgenden Artikel lesen:

> [!div class="nextstepaction"]
> [Schreiben von Abfragen für Azure Data Explorer](write-queries.md)
