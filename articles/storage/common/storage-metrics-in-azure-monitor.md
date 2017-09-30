---
title: Azure Storage-Metriken in Azure Monitor | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zu den neuen Metriken von Azure Monitor.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 09/05/2017
ms.author: fryu
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: b9fac8796b9cb36418d4494eaaf8080744321e19
ms.contentlocale: de-de
ms.lasthandoff: 09/22/2017

---

# <a name="azure-storage-metrics-in-azure-monitor-preview"></a>Azure Storage-Metriken in Azure Monitor (Vorschauversion)

Anhand von Azure Storage-Metriken können Sie Verwendungstrends analysieren, Anforderungen nachverfolgen und Probleme mit Ihrem Speicherkonto diagnostizieren.

Azure Monitor bietet einheitliche Benutzeroberflächen für die übergreifende Überwachung verschiedener Azure-Dienste. Weitere Informationen finden Sie im [Überblick über die Überwachung in Microsoft Azure](../../monitoring-and-diagnostics/monitoring-overview.md). Zur Integration von Azure Monitor sendet Azure Storage Metrikdaten an die Azure Monitor-Plattform.

## <a name="access-metrics"></a>Zugreifen auf Metriken

Azure Monitor bietet mehrere Möglichkeiten für den Metrikzugriff. Sie können über das [Azure-Portal](https://portal.azure.com), über die Azure Monitor-APIs (REST und .NET) sowie über Analyselösungen wie die Operation Management Suite und Event Hub auf Metriken zugreifen. Weitere Informationen finden Sie im [Überblick über Metriken in Microsoft Azure](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Metriken sind standardmäßig aktiviert, und es stehen Daten für die letzten 30 Tage zur Verfügung. Zur längeren Aufbewahrung können Sie Metrikdaten in einem Azure-Speicherkonto archivieren. Dies wird in den [Diagnoseeinstellungen](../../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) in Azure Monitor konfiguriert.

### <a name="access-metrics-in-the-azure-portal"></a>Zugreifen auf Metriken über das Azure-Portal

Im Azure-Portal können Sie Metriken im Zeitverlauf überwachen. Das folgende Beispiel zeigt, wie Sie **UsedCapacity** auf der Kontoebene anzeigen.

![Screenshot für den Zugriff auf Metriken über das Azure-Portal](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal.png)

Bei Metriken mit Dimensionsunterstützung müssen Sie den gewünschten Dimensionswert als Filter verwenden. Das folgende Beispiel zeigt, wie Sie **Transaktionen** auf der Kontoebene mit dem Antworttyp **Success** anzeigen.

![Screenshot für den Zugriff auf Metriken mit Dimension über das Azure-Portal](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal-with-dimension.png)

### <a name="access-metrics-with-the-rest-api"></a>Zugreifen auf Metriken über die REST-API

Azure Monitor verfügt über [REST-APIs](/rest/api/monitor/) zum Lesen von Metrikdefinition und -werten. In diesem Abschnitt erfahren Sie, wie Sie die Speichermetriken lesen. Bei allen REST-APIs wird die Ressourcen-ID verwendet. Weitere Informationen finden Sie unter [Grundlegendes zur Ressourcen-ID für Dienste in Azure Storage](#understanding-resource-id-for-services-in-storage).

Das folgende Beispiel zeigt, wie Sie [ArmClient](https://github.com/projectkudu/ARMClient) in der Befehlszeile verwenden, um das Testen mit der REST-API zu vereinfachen.

#### <a name="list-account-level-metric-definition-with-the-rest-api"></a>Auflisten der Metrikdefinition auf der Kontoebene über die REST-API

Das folgende Beispiel zeigt, wie Sie die Metrikdefinition auf der Kontoebene auflisten:

```
# Login to Azure and enter your credentials when prompted.
> armclient login

> armclient GET /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions?api-version=2017-05-01-preview

```

Wenn Sie die Metrikdefinitionen für Blob, Table, File oder Queue auflisten möchten, müssen Sie mit der API verschiedene Ressourcen-IDs für die einzelnen Dienste angeben.

Die Antwort enthält die Metrikdefinition im JSON-Format:

```Json
{
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions/UsedCapacity",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}",
      "category": "Capacity",
      "name": {
        "value": "UsedCapacity",
        "localizedValue": "Used capacity"
      },
      "isDimensionRequired": false,
      "unit": "Bytes",
      "primaryAggregationType": "Average",
      "metricAvailabilities": [
        {
          "timeGrain": "PT1M",
          "retention": "P30D"
        },
        {
          "timeGrain": "PT1H",
          "retention": "P30D"
        }
      ]
    },
    ... next metric definition
  ]
}

```

#### <a name="read-account-level-metric-values-with-the-rest-api"></a>Lesen der Metrikwerte auf der Kontoebene über die REST-API

Das folgende Beispiel zeigt, wie Sie Metrikdaten auf der Kontoebene lesen:

```
> armclient GET "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metrics?metric=Availability&api-version=2017-05-01-preview&aggregation=Average&interval=PT1H"

```

Wenn Sie im obigen Beispiel Metrikwerte für Blob, Table, File oder Queue lesen möchten, müssen Sie mit der API verschiedene Ressourcen-IDs für die einzelnen Dienste angeben.

Die folgende Antwort enthält Metrikwerte im JSON-Format:

```Json
{
  "cost": 0,
  "timespan": "2017-09-07T17:27:41Z/2017-09-07T18:27:41Z",
  "interval": "PT1H",
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/Microsoft.Insights/metrics/Availability",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Availability",
        "localizedValue": "Availability"
      },
      "unit": "Percent",
      "timeseries": [
        {
          "metadatavalues": [],
          "data": [
            {
              "timeStamp": "2017-09-07T17:27:00Z",
              "average": 100.0
            }
          ]
        }
      ]
    }
  ]
}

```

## <a name="billing-for-metrics"></a>Abrechnung für Metriken

Die Verwendung von Metriken in Azure Monitor ist derzeit kostenlos. Wenn Sie allerdings zusätzliche Lösungen zur Metrikdatenerfassung verwenden, fallen unter Umständen Kosten für diese Lösungen an. So werden etwa Gebühren für Azure Storage berechnet, wenn Sie Metrikdaten in einem Azure-Speicherkonto archivieren. Kosten fallen auch an, wenn Sie Metrikdaten für eine ausführlichere Analyse an die Operation Management Suite (OMS) streamen.

## <a name="understanding-resource-id-for-services-in-azure-storage"></a>Grundlegendes zur Ressourcen-ID für Dienste in Azure Storage

Die Ressourcen-ID ist ein eindeutiger Bezeichner einer Ressource in Azure. Wenn Sie Metrikdefinitionen oder -werte mithilfe der REST-API von Azure Monitor lesen möchten, müssen Sie die Ressourcen-ID für die gewünschte Ressource verwenden. Die Ressourcen-ID-Vorlage hat folgendes Format:

`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
`

Mit Azure Monitor bietet Storage Metriken auf der Speicherkontoebene und auf der Dienstebene. So haben Sie beispielsweise die Möglichkeit, nur Metriken für Blob Storage abzurufen. Jede Ebene hat ihre eigene Ressourcen-ID, mit der Metriken für genau diese Ebene abgerufen werden können.

### <a name="resource-id-for-a-storage-account"></a>Ressourcen-ID für ein Speicherkonto

Das Format zum Angeben der Ressourcen-ID für ein Speicherkonto sieht wie folgt aus:

`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}
`

### <a name="resource-id-for-the-storage-services"></a>Ressourcen-ID für die Speicherdienste

Das Format zum Angeben der Ressourcen-ID für die einzelnen Speicherdienste sieht wie folgt aus:

* Ressourcen-ID für den Blob-Dienst `
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default
`
* Ressourcen-ID für den Table-Dienst `
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default
`
* Ressourcen-ID für den Queue-Dienst `
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/queueServices/default
`
* Ressourcen-ID für den File-Dienst `
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/fileServices/default
`

### <a name="resource-id-in-azure-monitor-rest-api"></a>Ressourcen-ID in der REST-API von Azure Monitor

Das Muster zum Aufrufen der REST-API von Azure Monitor sieht wie folgt aus:

`
GET {resourceId}/providers/microsoft.insights/metrics?{parameters}
`

## <a name="capacity-metrics"></a>Kapazitätsmetriken

Kapazitätsmetrikwerte werden stündlich an Azure Monitor gesendet. Die Werte werden täglich aktualisiert. Das Aggregationsintervall definiert das Zeitintervall, für das Metrikwerte dargestellt werden. Das unterstützte Aggregationsintervall für alle Kapazitätsmetriken beträgt eine Stunde (PT1H).

Azure Storage stellt in Azure Monitor folgende Kapazitätsmetriken bereit:

### <a name="account-level"></a>Kontoebene

| Metrikname | Beschreibung |
| ------------------- | ----------------- |
| UsedCapacity | Die vom Speicherkonto beanspruchte Speichermenge. Bei Standardspeicherkonten ist das die Summe der von Blob, Table, File und Queue beanspruchten Kapazität. Bei Storage Premium- und Blob Storage-Konten ist es der gleiche Wert wie für „BlobCapacity“. <br/><br/> Einheit: Bytes <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 |

### <a name="blob-storage"></a>Blob Storage

| Metrikname | Beschreibung |
| ------------------- | ----------------- |
| BlobCapacity | Der gesamte im Speicherkonto beanspruchte Blobspeicher. <br/><br/> Einheit: Bytes <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 <br/> Dimension: BlobType ([Definition](#metrics-dimensions)) |
| BlobCount    | Die Anzahl von im Speicherkonto gespeicherten Blobs. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 <br/> Dimension: BlobType ([Definition](#metrics-dimensions)) |
| ContainerCount    | Die Anzahl von Containern im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 |

### <a name="table-storage"></a>Table Storage

| Metrikname | Beschreibung |
| ------------------- | ----------------- |
| TableCapacity | Der vom Speicherkonto beanspruchte Table-Speicher. <br/><br/> Einheit: Bytes <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 |
| TableCount   | Die Anzahl von Tabellen im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 |
| TableEntityCount | Die Anzahl von Tabellenentitäten im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 |

### <a name="queue-storage"></a>Queue Storage

| Metrikname | Beschreibung |
| ------------------- | ----------------- |
| QueueCapacity | Der vom Speicherkonto beanspruchte Queue-Speicher. <br/><br/> Einheit: Bytes <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 |
| QueueCount   | Die Anzahl von Warteschlangen im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 |
| QueueMessageCount | Die Anzahl nicht abgelaufener Warteschlangennachrichten im Speicherkonto. <br/><br/>Einheit: Anzahl <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 |

### <a name="file-storage"></a>File Storage

| Metrikname | Beschreibung |
| ------------------- | ----------------- |
| FileCapacity | Der vom Speicherkonto beanspruchte File-Speicher. <br/><br/> Einheit: Bytes <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 |
| FileCount   | Die Anzahl von Dateien im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 |
| FileShareCount | Die Anzahl von Dateifreigaben im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Durchschnitt <br/> Beispielwert: 1024 |

## <a name="transaction-metrics"></a>Transaktionsmetriken

Transaktionsmetriken werden im Minutentakt von Azure Storage an Azure Monitor gesendet. Alle Transaktionsmetriken stehen sowohl auf der Konto- als auch auf der Dienstebene (Blob Storage, Table Storage, Azure Files und Queue Storage) zur Verfügung. Das Aggregationsintervall definiert das Zeitintervall für die dargestellten Metrikwerte. Für alle Transaktionsmetriken werden die Aggregationsintervalle PT1H und PT1M unterstützt.

Azure Storage stellt in Azure Monitor folgende Transaktionsmetriken bereit:

| Metrikname | Beschreibung |
| ------------------- | ----------------- |
| Transaktionen | Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und nicht erfolgreiche Anforderungen sowie Anforderungen, die zu Fehlern geführt haben. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Verfügbare Dimensionen: ResponseType, GeoType, ApiName ([Definition](#metrics-dimensions))<br/> Beispielwert: 1024 |
| Eingehende Daten | Die eingehende Datenmenge. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure. <br/><br/> Einheit: Bytes <br/> Aggregationstyp: Summe <br/> Verfügbare Dimensionen: GeoType, ApiName ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| Ausgehende Daten | Die ausgehende Datenmenge. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar. <br/><br/> Einheit: Bytes <br/> Aggregationstyp: Summe <br/> Verfügbare Dimensionen: GeoType, ApiName ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| SuccessServerLatency | Die durchschnittliche Verarbeitungszeit einer erfolgreichen Anforderung durch Azure Storage. Dieser Wert beinhaltet nicht die in „SuccessE2ELatency“ angegebene Netzwerklatenz. <br/><br/> Einheit: Millisekunden <br/> Aggregationstyp: Durchschnitt <br/> Verfügbare Dimensionen: GeoType, ApiName ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| SuccessE2ELatency | Die durchschnittliche End-to-End-Wartezeit (in Millisekunden) bei erfolgreichen Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung. <br/><br/> Einheit: Millisekunden <br/> Aggregationstyp: Durchschnitt <br/> Verfügbare Dimensionen: GeoType, ApiName ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| Availability | Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Zur Berechnung der Verfügbarkeit wird der Wert der gesamten gebührenpflichtigen Anforderungen durch die Anzahl zutreffender Anforderungen geteilt. Dabei werden auch Anforderungen einbezogen, die zu unerwarteten Fehlern geführt haben. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang. <br/><br/> Einheit: Prozent <br/> Aggregationstyp: Durchschnitt <br/> Verfügbare Dimensionen: GeoType, ApiName ([Definition](#metrics-dimensions)) <br/> Beispielwert: 99,99 |

## <a name="metrics-dimensions"></a>Metrikdimensionen

Azure Storage unterstützt folgende Dimensionen für Metriken in Azure Monitor:

| Dimensionsname | Beschreibung |
| ------------------- | ----------------- |
| BlobType | Die Art des Blobs (nur für Blobmetriken). Unterstützte Werte: **BlockBlob** und **PageBlob**. Anfügeblob ist in „BlockBlob“ enthalten. |
| ResponseType | Transaktionsantworttyp. Verfügbaren Werte: <br/><br/> <li>ServerOtherError: Alle anderen serverseitigen Fehler (mit Ausnahme beschriebener Fehler). </li> <li> ServerBusyError: Authentifizierte Anforderung, die den HTTP-Statuscode 503 zurückgegeben hat. (Wird noch nicht unterstützt.) </li> <li> ServerTimeoutError: Authentifizierte Anforderung mit Timeout, die den HTTP-Statuscode 500 zurückgegeben hat. Das Timeout ist auf einen Serverfehler zurückzuführen. </li> <li> ThrottlingError: Summe clientseitiger und serverseitiger Drosselungsfehler. (Wird entfernt, sobald „ServerBusyError“ und „ClientThrottlingError“ unterstützt werden.) </li> <li> AuthorizationError: Authentifizierte Anforderung, die aufgrund eines nicht autorisierten Datenzugriffs oder aufgrund eines Autorisierungsfehlers nicht erfolgreich war. </li> <li> NetworkError: Authentifizierte Anforderung, die aufgrund von Netzwerkfehlern nicht erfolgreich war. Tritt üblicherweise auf, wenn ein Client vor Ablauf des Timeouts vorzeitig eine Verbindung trennt. </li> <li>  ClientThrottlingError: Clientseitige Drosselungsfehler. (Wird noch nicht unterstützt.) </li> <li> ClientTimeoutError: Authentifizierte Anforderung mit Timeout, die den HTTP-Statuscode 500 zurückgegeben hat. Wenn das Netzwerktimeout des Clients oder das Anforderungstimeout auf einen niedrigeren Wert festgelegt ist als vom Speicherdienst erwartet, handelt es sich um ein erwartetes Timeout. Andernfalls wird „ServerTimeoutError“ gemeldet. </li> <li> ClientOtherError: Alle anderen clientseitigen Fehler (mit Ausnahme beschriebener Fehler). </li> <li> Success: Erfolgreiche Anforderung.|
| GeoType | Transaktion aus dem primären oder sekundären Cluster. Verfügbare Werte: „Primary“ und „Secondary“. Gilt für georedundanten Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS) beim Lesen von Objekten aus dem sekundären Mandanten. |
| ApiName | Der Name des Vorgangs. Beispiel: <br/> <li>CreateContainer</li> <li>DeleteBlob</li> <li>GetBlob</li> Eine Liste mit allen Vorgangsnamen finden Sie [hier](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages#logged-operations.md). |

Bei Metriken mit Dimensionsunterstützung muss der Dimensionswert angegeben werden, um die entsprechenden Metrikwerte anzeigen zu können. Wenn Sie sich also beispielsweise erfolgreiche Antworten für **Transaktionen** ansehen möchten, müssen Sie die Dimension **ResponseType** nach **Success** filtern. Anderes Beispiel: Wenn Sie sich **BlobCount** für „Blockblob“ ansehen möchten, müssen Sie die Dimension **BlobType** nach **BlockBlob** filtern.

## <a name="service-continuity-of-legacy-metrics"></a>Dienstkontinuität von Legacymetriken

Legacymetriken stehen parallel zu den von Azure Monitor verwalteten Metriken zur Verfügung. Die Unterstützung bleibt unverändert, bis Azure Storage den Dienst für Legacymetriken einstellt. Der Einstellungsplan wird bekannt gegeben, wenn die von Azure Monitor verwalteten Metriken offiziell eingeführt werden.

## <a name="see-also"></a>Weitere Informationen

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)

