---
title: Azure Service Fabric-Ereignisanalyse mit Application Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Ereignisse unter Verwendung von Application Insights zur Überwachung und Diagnose von Azure Service Fabric-Clustern visualisiert und analysiert werden.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/04/2018
ms.author: srrengar
ms.openlocfilehash: 29adf362fdacdb793af071fa6d7bd59214536374
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207752"
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Ereignisanalyse und Visualisierung mit Application Insights

Azure Application Insights ist eine erweiterbare Plattform für die Überwachung und Diagnose von Anwendungen. Sie enthält ein leistungsfähiges Analyse- und Abfragetool, ein anpassbares Dashboard und Visualisierungen sowie weitere Optionen, z.B. automatisierte Warnungen. Sie ist die empfohlene Plattform für die Überwachung und Diagnose für Service Fabric-Anwendungen und -Dienste. Dieser Artikel geht auf folgende allgemeine Fragen ein:

* Wie kann ich mich über die Vorgänge in meiner Anwendung und in meinen Diensten informieren und Telemetriedaten sammeln?
* Wie behebe ich Probleme mit meiner Anwendung (insbesondere Probleme bei der Kommunikation zwischen Diensten)?
* Wie erhalte ich Metriken zur Leistung meiner Dienste wie Seitenladezeit, HTTP-Anforderungen und Ähnliches?

In diesem Artikel erfahren Sie, wie Sie in Application Insights Erkenntnisse gewinnen und Probleme behandeln können. Informationen zum Einrichten und Konfigurieren von AI mit Service Fabric finden Sie in [diesem Tutorial](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="monitoring-in-app-insights"></a>Überwachen in Application Insights

Application Insights bietet bereits standardmäßig umfassende Service Fabric-Funktionen. Auf der Übersichtsseite liefert AI wichtige Informationen zu Ihrem Dienst (beispielsweise zur Antwortzeit und zur Anzahl verarbeiteter Anforderungen). Wenn Sie am oberen Rand auf die Suchschaltfläche klicken, wird eine Liste mit Anforderungen angezeigt, die kürzlich in Ihrer Anwendung ausgeführt wurden. Darüber hinaus werden hier ggf. nicht erfolgreiche Anforderungen angezeigt, und Sie können die aufgetretenen Fehler diagnostizieren.

![AI-Übersicht](media/service-fabric-diagnostics-event-analysis-appinsights/ai-overview.png)

Die Liste im rechten Bereich der vorherigen Abbildung enthält hauptsächlich zwei Arten von Einträgen: Anforderungen und Ereignisse. Anforderungen sind an die API der App gerichtete Aufrufe (in diesem Fall über HTTP-Anforderungen). Ereignisse sind benutzerdefinierte Ereignisse und fungieren als Telemetrie, die Sie an einer beliebigen Stelle in Ihrem Code hinzufügen können. Weitere Informationen zur Instrumentierung Ihrer Anwendungen finden Sie unter [Application Insights-API für benutzerdefinierte Ereignisse und Metriken](../application-insights/app-insights-api-custom-events-metrics.md). Wenn Sie auf eine Anforderung klicken, werden weitere Details angezeigt, wie in der folgenden Abbildung zu sehen. Sie umfassen unter anderem Service Fabric-spezifische Daten, die im AI Service Fabric-NuGet-Paket erfasst werden. Diese Informationen können zur Problembehandlung und zur Ermittlung des Anwendungszustands herangezogen sowie in Application Insights durchsucht werden.

![AI-Anforderungsdetails](media/service-fabric-diagnostics-event-analysis-appinsights/ai-request-details.png)

Application Insights bietet eine eigene Ansicht zum Abfragen sämtlicher eingehender Daten. Klicken Sie am oberen Rand der Übersichtsseite auf „Metrik-Explorer“, um zum AI-Portal zu gelangen. Hier können Sie unter Verwendung der Kusto-Abfragesprache Abfragen für die zuvor erwähnten benutzerdefinierten Ereignisse sowie für Anforderungen, Ausnahmen, Leistungsindikatoren und weitere Metriken ausführen. Das folgende Beispiel zeigt alle Anforderungen der letzten Stunde:

![AI-Anforderungsdetails](media/service-fabric-diagnostics-event-analysis-appinsights/ai-metrics-explorer.png)

Weitere Informationen zu den Funktionen des App Insights-Portals finden Sie in der [Dokumentation zum Application Insights-Portal](../application-insights/app-insights-dashboards.md).

### <a name="configuring-ai-with-wad"></a>Konfigurieren von AI mit WAD

>[!NOTE]
>Dies gilt gegenwärtig nur für Windows-Cluster.

Es gibt zwei bevorzugte Möglichkeiten zum Senden von Daten von WAD an Azure AI. Dies erfolgt durch Hinzufügen einer AI-Senke in der WAD-Konfiguration, wie in [diesem Artikel](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md) ausführlich beschrieben.

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Hinzufügen eines AI-Instrumentierungsschlüssels beim Erstellen eines Clusters im Azure-Portal

![Hinzufügen eines AI-Schlüssels](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Wenn beim Erstellen eines Clusters die Diagnose aktiviert ist („Ein“), wird ein optionales Feld angezeigt, in dem ein Application Insights-Instrumentierungsschlüssel eingegeben werden kann. Wenn Sie Ihren AI-Schlüssel hier einfügen, wird die AI-Senke automatisch in der Resource Manager-Vorlage konfiguriert, die zum Bereitstellen Ihres Clusters verwendet wird.

#### <a name="add-the-ai-sink-to-the-resource-manager-template"></a>Hinzufügen der AI-Senke zur Resource Manager-Vorlage

Fügen Sie in „WadCfg“ der Resource Manager-Vorlage durch Einfügen der folgenden beiden Änderungen eine Senke („Sink“) hinzu:

1. Fügen Sie die Konfiguration der Senke hinzu, sobald das Deklarieren von `DiagnosticMonitorConfiguration` abgeschlossen ist:

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. Nehmen Sie die Senke in `DiagnosticMonitorConfiguration` auf, indem Sie die folgende Zeile in `DiagnosticMonitorConfiguration` von `WadCfg` hinzufügen (unmittelbar bevor `EtwProviders` deklariert werden):

    ```json
    "sinks": "applicationInsights"
    ```

In den beiden obigen Codeausschnitten wurde für die Senke der Name „applicationInsights“ verwendet. Dieser Name muss nicht verwendet werden. Solange der Name der Senke in „sinks“ eingefügt wird, können Sie eine beliebige Zeichenfolge als Name festlegen.

Protokolle aus dem Cluster werden derzeit als **Ablaufverfolgungen** in der AI-Protokollanzeige angezeigt. Da die meisten Ablaufverfolgungen der Plattform als „Information“ eingestuft werden, haben Sie auch die Möglichkeit, die Senkenkonfiguration so zu ändern, dass nur Protokolle vom Typ „Kritisch“ und „Fehler“ gesendet werden. Dies kann durch Hinzufügen von „Kanälen“ zur Senke erfolgen, wie in [diesem Artikel](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md) erläutert.

>[!NOTE]
>Wenn Sie im Portal oder in der Resource Manager-Vorlage einen falschen AI-Schlüssel verwenden, müssen Sie ihn manuell ändern und den Cluster aktualisieren und erneut bereitstellen.

### <a name="configuring-ai-with-eventflow"></a>Konfigurieren von AI mit EventFlow

Wenn Sie EventFlow zum Aggregieren von Ereignissen verwenden, müssen Sie das NuGet-Paket `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights` importieren. Der Abschnitt *outputs* von *eventFlowConfig.json* muss folgenden Code enthalten:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        "instrumentationKey": "***ADD INSTRUMENTATION KEY HERE***"
    }
]
```

Stellen Sie sicher, dass Sie die erforderlichen Änderungen in Ihren Filtern vornehmen und alle anderen Eingaben (zusammen mit den jeweiligen NuGet-Paketen) einfügen.

## <a name="aisdk"></a>AI.SDK

Es empfiehlt sich, EventFlow und WAD als Aggregationslösungen zu verwenden, da diese einen modularen Ansatz für die Diagnose und Überwachung bieten. Das heißt, wenn Sie die Ausgaben von EventFlow ändern möchten, ist keine Änderung der eigentlichen Instrumentierung, sondern lediglich eine einfache Änderung der Konfigurationsdatei erforderlich. Wenn Sie sich jedoch für die Verwendung von Application Insights entscheiden und voraussichtlich die Plattform nicht ändern werden, sollten Sie in Betracht ziehen, das neue SDK von AI einzusetzen, um Ereignisse zu aggregieren und an AI zu senden. Das bedeutet, dass Sie nicht mehr EventFlow zum Senden der Daten an AI konfigurieren müssen, sondern stattdessen das Service Fabric-NuGet-Paket von Application Insights installieren. Details zu diesem Paket finden Sie [hier](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

Unter [Application Insights-Unterstützung für Microservices und Container](https://azure.microsoft.com/blog/app-insights-microservices/) werden einige der neuen Features beschrieben, an denen gearbeitet wird (derzeit noch in der Betaversion) und mit denen Sie über umfangreichere vordefinierte Überwachungsoptionen mit AI verfügen. Dazu gehören die Abhängigkeitsnachverfolgung (wird beim Erstellen einer AppMap aller Ihrer Dienste und Anwendungen in einem Cluster und der zugehörigen Kommunikation verwendet) und eine bessere Korrelation der Ablaufverfolgungen, die von Ihren Diensten stammen (trägt zur besseren genauen Erkennung eines Problems im Workflow einer App oder eines Diensts bei).

Wenn Sie beim Entwickeln in .NET voraussichtlich einige der Service Fabric-Programmiermodelle verwenden und AI als Plattform zum Visualisieren und Analysieren von Ereignis- und Protokolldaten verwenden möchten, wird empfohlen, das AI SDK für den Workflow der Überwachung und Diagnose zu verwenden. Informationen für den Einstieg in die Verwendung von AI zum Sammeln und Anzeigen Ihrer Protokolle finden Sie [hier](../application-insights/app-insights-asp-net-more.md) und [hier](../application-insights/app-insights-asp-net-trace-logs.md).

## <a name="navigating-the-ai-resource-in-azure-portal"></a>Navigieren zu der AI-Ressource im Azure-Portal

Nachdem Sie AI als Ausgabe für die Ereignisse und Protokolle konfiguriert haben, sollten nach wenigen Minuten Informationen in der AI-Ressource angezeigt werden. Navigieren Sie zu der AI-Ressource, um das Dashboard für die AI-Ressource anzuzeigen. Klicken Sie in der AI-Taskleiste auf **Suchen**, um die zuletzt empfangenen Ablaufverfolgungen anzuzeigen und sie filtern zu können.

*Metrik-Explorer* ist ein nützliches Tool zum Erstellen von benutzerdefinierten Dashboards basierend auf den Metriken, die Ihre Anwendungen, Dienste und Cluster melden. Informationen zum Einrichten einiger benutzerdefinierter Diagramme basierend auf Ihren gesammelten Daten finden Sie unter [Untersuchen von Metriken in Application Insights](../application-insights/app-insights-metrics-explorer.md).

Durch Klicken auf **Analytics** gelangen Sie zum Application Insights Analytics-Portal, in dem Sie Ereignisse und Ablaufverfolgungen in größerem Umfang und mit mehr Optionen abfragen können. Weitere Informationen hierzu finden Sie unter [Analytics in Application Insights](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Nächste Schritte

* [Richten Sie Warnungen in AI ein](../application-insights/app-insights-alerts.md), um Benachrichtigungen zu Änderungen der Leistung oder Nutzung zu erhalten.
* Die [intelligente Erkennung in Application Insights](../application-insights/app-insights-proactive-diagnostics.md) führt eine proaktive Analyse der an AI gesendeten Telemetriedaten aus, um Sie vor potenziellen Leistungsproblemen zu warnen.
