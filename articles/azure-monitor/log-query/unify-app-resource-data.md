---
title: Vereinigen mehrerer Azure Monitor-Application Insights-Ressourcen | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zum Verwenden einer Funktion in Azure Monitor-Protokollen zum Abfragen mehrerer Application Insights-Ressourcen und zum Visualisieren dieser Daten.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: magoedte
ms.openlocfilehash: 1dba84c686fbb873f044b4980990baa396a94c79
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237670"
---
# <a name="unify-multiple-azure-monitor-application-insights-resources"></a>Vereinigen mehrerer Azure Monitor-Application Insights-Ressourcen 
Dieser Artikel beschreibt das Abfragen und Anzeigen aller Daten in Ihren Application Insights-Anwendungsprotokollen an einem Ort, selbst wenn sie aus unterschiedlichen Azure-Abonnements stammen. Damit soll der veraltete Application Insights-Connector ersetzt werden. Die Anzahl der Application Insights-Ressourcen, die Sie in eine einzelne Abfrage einschließen können, ist auf 100 beschränkt.  

## <a name="recommended-approach-to-query-multiple-application-insights-resources"></a>Empfohlener Ansatz zum Abfragen mehrerer Application Insights-Ressourcen 
Das Auflisten mehrerer Application Insights-Ressourcen in einer Abfrage kann aufwendig und schwierig zu verwalten sein. Sie können aber stattdessen eine Funktion zum Trennen der Abfragelogik vom Gültigkeitsbereich einer Anwendung nutzen.  

Dieses Beispiel zeigt, wie Sie mehrere Application Insights-Ressourcen überwachen und die Anzahl fehlerhafter Anforderungen anhand des Anwendungsnamens visualisieren können. Bevor Sie beginnen, führen Sie diese Abfrage in dem Arbeitsbereich aus, der mit Application Insights-Ressourcen verknüpft ist, um eine Liste der verbundenen Anwendungen abzurufen: 

```
ApplicationInsights
| summarize by ApplicationName
```

Erstellen Sie mithilfe des union-Operators eine Funktion mit der Liste der Anwendungen, und speichern Sie die Abfrage in Ihrem Arbeitsbereich als Funktion mit dem Alias *applicationsScoping*.  

```
union withsource=SourceApp 
app('Contoso-app1').requests,  
app('Contoso-app2').requests, 
app('Contoso-app3').requests, 
app('Contoso-app4').requests, 
app('Contoso-app5').requests 
| parse SourceApp with * "('" applicationName "')" *  
```

>[!NOTE]
>Sie können die aufgelisteten Anwendungen jederzeit im Portal ändern. Navigieren Sie dazu zum Abfrage-Explorer in Ihrem Arbeitsbereich, und bearbeiten und speichern Sie dann die Funktion, oder verwenden Sie das PowerShell-Cmdlet `SavedSearch`. Der Befehl `withsource= SourceApp` fügt den Ergebnissen eine Spalte hinzu, die die Anwendung angibt, von der das Protokoll gesendet wurde. 
>
>Die Abfrage verwendet das Application Insights-Schema, obwohl sie im Arbeitsbereich ausgeführt wird, da die applicationsScoping-Funktion die Application Insights-Datenstruktur zurückgibt. 
>
>Die parse-Operator in diesem Beispiel ist optional. Er extrahiert den Namen der Anwendung aus der SourceApp-Eigenschaft. 

Sie können nun die applicationsScoping-Funktion in der ressourcenübergreifenden Abfrage verwenden.  

```
applicationsScoping 
| where timestamp > ago(12h)
| where success == 'False'
| parse SourceApp with * '(' applicationName ')' * 
| summarize count() by applicationName, bin(timestamp, 1h) 
| render timechart
```

Der Funktionsalias gibt die Vereinigungsmenge der Anforderungen aller definierten Anwendungen zurück. Die Abfrage filtert dann fehlgeschlagene Anforderungen heraus und visualisiert die Trends nach Anwendung sortiert.

![Beispiel für abfrageübergreifende Ergebnisse](media/unify-app-resource-data/app-insights-query-results.png)

## <a name="query-across-application-insights-resources-and-workspace-data"></a>Abfragen über Application Insights-Ressourcen und Arbeitsbereichsdaten 
Wenn Sie den Connector beenden und Abfragen über einen Zeitraum, der durch die Application Insights-Datenaufbewahrung (90 Tage) gekürzt wurde, ausführen müssen, führen Sie für einen mittleren Zeitraum [ressourcenübergreifende Abfragen](../../azure-monitor/log-query/cross-workspace-query.md) in dem Arbeitsbereich und für die Application Insights Ressourcen aus. Dies gilt, bis die Anwendungsdaten entsprechend der neuen Application Insights-Datenaufbewahrung gesammelt wurden. Die Abfrage erfordert einige Änderungen, da die Schemas in Application Insights und im Arbeitsbereich unterschiedlich sind. Sehen Sie sich dazu die Tabelle weiter unten in diesem Abschnitt an, in der die Schemaunterschiede hervorgehoben sind. 

Wenn der Connector z.B. am 1.11.2018 beendet wurde, während Sie Protokolle von Application Insights-Ressourcen und Anwendungsdaten im Arbeitsbereich abgefragt haben, würde Ihre Abfrage wie im folgenden Beispiel erstellt werden:

```
applicationsScoping //this brings data from Application Insights resources 
| where timestamp between (datetime("2018-11-01") .. now()) 
| where success == 'False' 
| where duration > 1000 
| union ( 
    ApplicationInsights //this is Application Insights data in Log Analytics workspace 
    | where TimeGenerated < (datetime("2018-12-01") 
    | where RequestSuccess == 'False' 
    | where RequestDuration > 1000 
    | extend duration = RequestDuration //align to Application Insights schema 
    | extend timestamp = TimeGenerated //align to Application Insights schema 
    | extend name = RequestName //align to Application Insights schema 
    | extend resultCode = ResponseCode //align to Application Insights schema 
    | project-away RequestDuration , RequestName , ResponseCode , TimeGenerated 
) 
| project timestamp , duration , name , resultCode 
```

## <a name="application-insights-and-log-analytics-workspace-schema-differences"></a>Schemaunterschiede zwischen Application Insights und Log Analytics-Arbeitsbereichen
Die folgende Tabelle zeigt die Schemaunterschiede zwischen Log Analytics und Application Insights.  

| Eigenschaften von Log Analytics-Arbeitsbereichen| Eigenschaften von Application Insights-Ressourcen|
|------------|------------| 
| AnonUserId | user_id|
| ApplicationId | appId|
| ApplicationName | appName|
| ApplicationTypeVersion | application_Version |
| AvailabilityCount | itemCount |
| AvailabilityDuration | duration |
| AvailabilityMessage | Message: |
| AvailabilityRunLocation | location |
| AvailabilityTestId | id |
| AvailabilityTestName | name |
| AvailabilityTimestamp | timestamp |
| "Browser" | client_browser |
| City | client_city |
| ClientIP | client_IP |
| Computer | cloud_RoleInstance | 
| Country | client_CountryOrRegion | 
| CustomEventCount | itemCount | 
| CustomEventDimensions | customDimensions |
| CustomEventName | name | 
| DeviceModel | client_Model | 
| DeviceType | client_Type | 
| ExceptionCount | itemCount | 
| ExceptionHandledAt | handledAt |
| ExceptionMessage | Message: | 
| ExceptionType | type |
| OperationID | operation_id |
| NameVorgang | operation_Name | 
| Betriebssystem | client_OS | 
| PageViewCount | itemCount |
| PageViewDuration | duration | 
| PageViewName | name | 
| ParentOperationID | operation_Id | 
| RequestCount | itemCount | 
| RequestDuration | duration | 
| RequestID | id | 
| RequestName | name | 
| RequestSuccess | Erfolg | 
| ResponseCode | resultCode | 
| Rolle | cloud_RoleName |
| RoleInstance | cloud_RoleInstance |
| SessionId | session_Id | 
| SourceSystem | operation_SyntheticSource |
| TelemetryType | type |
| URL | _url |
| UserAccountId | user_AccountId |

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die [Protokollsuche](../../azure-monitor/log-query/log-query-overview.md), um ausführliche Informationen zu Ihren Application Insights-Apps anzuzeigen.
