---
title: Korrelation der Azure Application Insights-Telemetrie | Microsoft-Dokumentation
description: Korrelation der Application Insights-Telemetrie
services: application-insights
documentationcenter: .net
author: lgayhardt
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/10/2019
ms.reviewer: sergkanz
ms.author: lagayhar
ms.openlocfilehash: 8b31a85abf1c6034aaff511f23d96fae9ee64561
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2019
ms.locfileid: "54230049"
---
# <a name="telemetry-correlation-in-application-insights"></a>Telemetriekorrelation in Application Insights

In der Welt der Mikroservices müssen für jeden logischen Vorgang Einstellungen an den verschiedenen Dienstkomponenten vorgenommen werden. Jede dieser Komponenten kann separat von [Application Insights](../../azure-monitor/app/app-insights-overview.md) überwacht werden. Die Web-App-Komponente kommuniziert mit der Authentifizierungsanbieterkomponente, um die Anmeldeinformationen des Benutzers zu überprüfen, und mit der API-Komponente, um Daten zur Visualisierung abzurufen. Die API-Komponente wiederum kann Daten von anderen Diensten abfragen, Cacheanbieterkomponenten nutzen und die Abrechnungskomponente über diesen Aufruf benachrichtigen. Application Insights unterstützt die verteilte Telemetriekorrelation. Damit können Sie ermitteln, welche Komponente für Fehler oder Leistungsbeeinträchtigungen verantwortlich ist.

In diesem Artikel wird das von Application Insights verwendete Datenmodell erläutert, mit dem die von mehreren Komponenten gesendeten Telemetriedaten korreliert werden. Darin werden die Methoden und Protokolle zur Kontextpropagierung erläutert. Zudem wird auch die Implementierung der Korrelationskonzepte in verschiedenen Sprachen und auf verschiedenen Plattformen behandelt.

## <a name="telemetry-correlation-data-model"></a>Telemetriekorrelations-Datenmodell

Application Insights definiert ein [Datenmodell](../../azure-monitor/app/data-model.md) für die verteilte Telemetriekorrelation. Um die Telemetrie mit dem logischen Vorgang zu verknüpfen, ist jedes Telemetrieelement mit einem Kontextfeld namens `operation_Id` versehen. Dieser Bezeichner wird von jedem Telemetrieelement in der verteilten Ablaufverfolgung gemeinsam genutzt. Selbst bei einem Verlust von Telemetriedaten von einer einzigen Ebene können Sie dennoch Telemetriedaten zuordnen, die von anderen Komponenten gemeldet wurden.

Der verteilte logische Vorgang besteht in der Regel aus einem Satz von kleineren Vorgängen, wobei die Anforderungen von einer der Komponenten verarbeitet werden. Diese Vorgänge werden von einer [Anforderungstelemetrie](../../azure-monitor/app/data-model-request-telemetry.md) definiert. Jede Anforderungstelemetrie verfügt über eine eigene `id`, die diese eindeutig global identifiziert. Für sämtliche Telemetriedaten (Ablaufverfolgungen, Ausnahmen usw.), die dieser Anforderung zugeordnet sind, sollte die `operation_parentId` auf den Wert der Anforderungs-`id` festgelegt werden.

Alle ausgehenden Vorgänge (wie HTTP-Aufrufe an andere Komponenten) werden durch eine [Abhängigkeitstelemetrie](../../azure-monitor/app/data-model-dependency-telemetry.md) dargestellt. Abhängigkeitstelemetrien definieren zudem ihre eigene `id`, die global eindeutig ist. Anforderungsabhängigkeiten, die durch diese Anforderungstelemetrie initiiert werden, verwenden diese als `operation_parentId`.

Sie können die Ansicht des verteilten logischen Vorgangs mit `operation_Id`, `operation_parentId` und `request.id` mit `dependency.id` erstellen. Diese Felder definieren auch die Kausalitätsreihenfolge der Telemetrieaufrufe.

In Mikroserviceumgebungen können Ablaufverfolgungen von Komponenten an unterschiedliche Speicher weitergeleitet werden. Jede Komponente kann einen eigenen Instrumentierungsschlüssel in Application Insights aufweisen. Um Telemetriedaten für den logischen Vorgang abzurufen, müssen Sie Daten von jedem Speicher abfragen. Bei einer immensen Anzahl von Speichern benötigen Sie einen Anhaltspunkt, um zu wissen, wo Sie als Nächstes suchen müssen.

Zur Behebung dieses Problems definiert das Datenmodell von Application Insights zwei Felder: `request.source` und `dependency.target`. Das erste Feld identifiziert die Komponente, die die Abhängigkeitsanforderung initiiert hat, und das zweite Feld identifiziert die Komponente, die die Antwort des Abhängigkeitsaufrufs zurückgegeben hat.


## <a name="example"></a>Beispiel

Veranschaulichen wir uns dies anhand eines Beispiels für eine STOCK PRICES-Anwendung, die den aktuellen Marktpreis eines Aktienkurses mithilfe der externen STOCKS API angibt. Die Anwendung STOCK PRICES enthält eine `Stock page`-Seite, die vom Clientwebbrowser mit `GET /Home/Stock` geöffnet wird. Die Anwendung fragt die STOCK API mit dem HTTP-Aufruf `GET /api/stock/value` ab.

Sie können die daraus resultierenden Telemetriedaten durch Ausführung einer Abfrage analysieren:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Beachten Sie, dass in der Ergebnisansicht alle Telemetrieelemente das Stammverzeichnis `operation_Id` nutzen. Wenn ein Ajax-Aufruf über die Seite vorgenommen wird, wird die neue eindeutige ID `qJSXU` der Abhängigkeitstelemetrie zugewiesen und die Seitenansichts-ID dient als `operation_ParentId`. Die Serveranforderung verwendet im Gegenzug die Ajax-ID als `operation_ParentId` usw.

| itemType   | name                      | ID           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | Stock page                |              | STYz               | STYz         |
| dependency | GET /Home/Stock           | qJSXU        | STYz               | STYz         |
| request    | GET Home/Stock            | KqKwlrSt9PA= | qJSXU              | STYz         |
| dependency | GET /api/stock/value      | bBrf2L7mm2g= | KqKwlrSt9PA=       | STYz         |

Nachdem Sie den `GET /api/stock/value`-Aufruf an einen externen Dienst getätigt haben, sollten Sie die Identität dieses Servers herausfinden. So können Sie das Feld `dependency.target` entsprechend festlegen. Wenn der externe Dienst keine Überwachung unterstützt, wird `target` auf den Hostnamen des Diensts, z.B. `stock-prices-api.com`, festgelegt. Wenn sich dieser Dienst jedoch durch Zurückgeben eines vordefinierten HTTP-Header selbst identifiziert, enthält `target` die Dienstidentität, mit dem Application Insights verteilte Ablaufverfolgungen durch Abfrage von Telemetriedaten von diesem Dienst erstellen kann. 

## <a name="correlation-headers"></a>Korrelations-Header

Wir arbeiten an einem RFC-Vorschlag für das [Korrelations-HTTP-Protokoll](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Diese Vorschläge definieren zwei Header:

- `Request-Id` enthält die global eindeutige ID des Aufrufs.
- `Correlation-Context` enthält die Sammlung an Name/Wert-Paaren der Eigenschaften von verteilten Ablaufverfolgungen.

Der Standard definiert außerdem zwei Schemata an `Request-Id`-Generierungen, nämlich flache und hierarchische Schemata. Beim flachen Schema ist ein bekannter `Id`-Schlüssel für die Sammlung `Correlation-Context` definiert.

Application Insights definiert die [Erweiterung](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) für das Korrelations-HTTP-Protokoll. Er verwendet `Request-Context`-Name/Wert-Paare, die die vom unmittelbaren Aufrufer oder Aufgerufenen verwendete Sammlung von Eigenschaften propagieren. Das Application Insights SDK legt mithilfe dieses Header die Felder `dependency.target` und `request.source` fest.

### <a name="w3c-distributed-tracing"></a>W3C – Verteilte Ablaufverfolgung

Wir gehen über zu [W3C-Format für verteilte Ablaufverfolgung](https://w3c.github.io/trace-context/). Dieses definiert:
- `traceparent` – trägt eine global eindeutige Vorgangs-ID sowie einen eindeutigen Bezeichner des Aufrufs.
- `tracestate` – trägt einen für das Ablaufverfolgungssystem spezifischen Kontext.

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-classic-apps"></a>Aktivieren der Unterstützung für die verteilte Ablaufverfolgung von W3C für ASP.NET Classic-Apps

Diese Funktion ist in den Paketen „Microsoft.ApplicationInsights.Web“ und „Microsoft.ApplicationInsights.DependencyCollector“ verfügbar, beginnend mit Version 2.8.0-beta1.
Es ist standardmäßig **aus**, ändern Sie also zum Aktivieren `ApplicationInsights.config`:

* Fügen Sie unter `RequestTrackingTelemetryModule` das `EnableW3CHeadersExtraction`-Element mit dem Wert `true` hinzu.
* Fügen Sie unter `DependencyTrackingTelemetryModule` das `EnableW3CHeadersInjection`-Element mit dem Wert `true` hinzu.

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>Aktivieren der Unterstützung für die verteilte Ablaufverfolgung von W3C für ASP.NET Core-Apps

Diese Funktion befindet sich in „Microsoft.ApplicationInsights.AspNetCore“ mit Version 2.5.0-beta1 sowie in „Microsoft.ApplicationInsights.DependencyCollector“ Version 2.8.0-beta1.
Es ist standardmäßig **aus**, legen Sie also zum Aktivieren `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` auf `true` fest:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

#### <a name="enable-w3c-distributed-tracing-support-for-java-apps"></a>Aktivieren der Unterstützung für die verteilte Ablaufverfolgung von W3C für Java-Apps

Eingehend:

**J2EE-Apps** fügen dem `<TelemetryModules>`-Tag innerhalb der Datei „applicationinsights.xml“ Folgendes hinzu:

```xml
<Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule>
   <Param name = "W3CEnabled" value ="true"/>
   <Param name ="enableW3CBackCompat" value = "true" />
</Add>
```

**Spring Boot-Apps** fügen die folgenden Eigenschaften hinzu:

`azure.application-insights.web.enable-W3C=true`
`azure.application-insights.web.enable-W3C-backcompat-mode=true`

Ausgehend:

Fügen Sie der Datei „AI-Agent.xml“ Folgendes hinzu:

```xml
<Instrumentation>
        <BuiltIn enabled="true">
            <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
        </BuiltIn>
    </Instrumentation>
```

> [!NOTE]
> Der Abwärtskompatibilitätsmodus ist standardmäßig aktiviert, und der Parameter „enableW3CBackCompat“ ist optional und sollte nur verwendet werden, wenn Sie diese Funktion deaktivieren möchten. 

Idealerweise wäre dies der Fall, wenn alle Dienste auf neuere Versionen der SDKs aktualisiert wurden, die das W3C-Protokoll unterstützen. Es wird dringend empfohlen, so bald wie möglich auf neuere Versionen der SDKs mit W3C-Unterstützung umzustellen. 

Stellen Sie sicher, dass die **Konfigurationen für eingehende und ausgehende Vorgänge exakt gleich sind**.

## <a name="open-tracing-and-application-insights"></a>OpenTracing und Application Insights

Die [OpenTracing-Datenmodellspezifikation](https://opentracing.io/) und Application Insights-Datenmodelle sind in folgender Weise zugeordnet:

| Application Insights                  | OpenTracing                                      |
|------------------------------------   |-------------------------------------------------  |
| `Request`, `PageView`                 | `Span` mit `span.kind = server`                  |
| `Dependency`                          | `Span` mit `span.kind = client`                  |
| `Id` der `Request` und `Dependency`    | `SpanId`                                          |
| `Operation_Id`                        | `TraceId`                                         |
| `Operation_ParentId`                  | `Reference` vom Typ `ChildOf` (die übergeordnete Spanne)   |

Weitere Informationen zum Datenmodell von Application Insights finden Sie unter [Datenmodell](../../azure-monitor/app/data-model.md). 

Definitionen von OpenTracing-Konzepten finden Sie in den [Spezifikationen](https://github.com/opentracing/specification/blob/master/specification.md) und [semantischen Konventionen](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) für OpenTracing.

## <a name="telemetry-correlation-in-net"></a>Telemetriekorrelation in .NET

Im Laufe der Zeit hat .NET mehrere Möglichkeiten zur Korrelation von Telemetrie- und Diagnoseprotokollen definiert. `System.Diagnostics.CorrelationManager` ermöglicht die Nachverfolgung von [LogicalOperationStack und ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource` und die Ereignisablaufverfolgung für Windows (ETW) definieren die Methode [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger` verwendet [Protokollbereiche](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF und HTTP richten die „aktuelle“ Kontextpropagierung ein.

Allerdings wurde durch diese Methoden nicht die Unterstützung für automatische verteilte Ablaufverfolgung aktiviert. `DiagnosticsSource` ist eine Möglichkeit zur Unterstützung von automatischen computerübergreifenden Korrelationen. .NET-Bibliotheken unterstützen Diagnosequellen und ermöglichen die automatische computerübergreifende Propagierung des Korrelationskontexts durch Übertragungsmöglichkeiten wie HTTP.

Im [Leitfaden zu Aktivitäten](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) in der Diagnosequelle werden die Grundlagen von Nachverfolgungsaktivitäten erläutert. 

ASP.NET Core 2.0 unterstützt die Extraktion von HTTP-Headern und das Starten einer neuen Aktivität. 

`System.Net.HttpClient` mit Startversion `4.1.0` unterstützt die automatische Injektion der Korrelations-HTTP-Header und die Nachverfolgung des HTTP-Aufrufs als Aktivität.

Für das klassische ASP.NET ist das neue HTTP-Modul [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) verfügbar. Dieses Modul implementiert Telemetriekorrelationen mithilfe von DiagnosticsSource. Es startet Aktivitäten basierend auf den eingehenden Anforderungs-Headern. Zudem werden Telemetriedaten aus den verschiedenen Phasen der Anforderungsverarbeitung korreliert. Dies gilt selbst für Fälle, in denen jede Phase der Verarbeitung in IIS in verschiedenen Verwaltungsthreads ausgeführt wird.

Das Application Insights SDK mit der Startversion `2.4.0-beta1` verwendet DiagnosticsSource und Activity, um Telemetriedaten zu sammeln und diese mit der aktuellen Aktivität zu verknüpfen. 

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-the-java-sdk"></a>Telemetriekorrelation im Java SDK
Das [Application Insights Java SDK](../../azure-monitor/app/java-get-started.md) unterstützt ab Version `2.0.0` die automatische Korrelation von Telemetriedaten. Das SDK füllt `operation_id` automatisch für alle Telemetriedaten (Ablaufverfolgungen, Ausnahmen, benutzerdefinierte Ereignisse usw.) auf, die innerhalb des Bereichs einer Anforderung ausgegeben werden. Es sorgt auch dafür, dass die Korrelationsheader (weiter oben beschrieben) für Dienst-zu-Dienst-Aufrufe über HTTP weitergeben werden, wenn [Java SDK-Agent](../../azure-monitor/app/java-agent.md) konfiguriert ist. Hinweis: Für die Korrelationsfunktion werden nur Aufrufe unterstützt, die über Apache HTTP Client erfolgen. Wenn Sie mit Spring Rest Template oder Feign arbeiten, können beide aufgesetzt auf Apache HTTP Client verwendet werden.

Derzeit wird automatische Kontextweitergabe quer durch Messagingtechnologien (z. B. Kafka, RabbitMQ, Azure Service Bus) nicht unterstützt. Solche Szenarien können aber manuell codiert werden, indem die `trackDependency`- und die `trackRequest`-API verwendet werden, wobei Telemetriedaten zu Abhängigkeiten einer Nachricht entsprechen, die von einem Producer in die Warteschlange eingereiht wird, und die Anforderung einer Nachricht entspricht, die von einem Consumer verarbeitet wird. In diesem Fall müssen sowohl `operation_id` als auch `operation_parentId` in den Eigenschaften der Nachricht weitergegeben werden.

<a name="java-role-name"></a>
## <a name="role-name"></a>Rollenname

Gelegentlich möchten Sie möglicherweise die Art und Weise anpassen, wie Komponentennamen in der [Anwendungsübersicht](../../azure-monitor/app/app-map.md) angezeigt werden. Dazu können Sie `cloud_RoleName` manuell festlegen, indem Sie eine der folgenden Aktionen ausführen:

Bei Verwendung von Spring Boot mit dem Application Insights Spring Boot-Startprogramm besteht die einzige erforderliche Änderung darin, Ihren benutzerdefinierten Namen für die Anwendung in der Datei „application.properties“ festzulegen.

`spring.application.name=<name-of-app>`

Das Spring Boot-Startprogramm weist dem von Ihnen für die Eigenschaft „spring.application.name“ eingegebenen Wert automatisch „CloudRoleName“ zu.

Wenn Sie `WebRequestTrackingFilter` verwenden, legt der `WebAppNameContextInitializer` automatisch den Anwendungsnamen fest. Fügen Sie Folgendes zu Ihrer Konfigurationsdatei (ApplicationInsights.xml) hinzu:
```XML
<ContextInitializers>
  <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebAppNameContextInitializer" />
</ContextInitializers>
```

Über die Cloudkontextklasse:

```Java
telemetryClient.getContext().getCloud().setRole("My Component Name");
```

## <a name="next-steps"></a>Nächste Schritte

- [Schreiben benutzerdefinierter Telemetriedaten](../../azure-monitor/app/api-custom-events-metrics.md)
- [Erfahren Sie mehr über](../../azure-monitor/app/app-map.md#set-cloudrolename) das Festlegen von „cloud_RoleName“ für die andere SDKs.
- Integrieren Sie alle Komponenten Ihres Mikroservices in Application Insights. Überprüfen Sie die [unterstützten Plattformen](../../azure-monitor/app/platforms.md).
- Lesen Sie die Informationen zu den Application Insights-Typen und zum Datenmodell unter [Datenmodell](../../azure-monitor/app/data-model.md).
- Informationen zum [Erweitern und Filtern von Telemetriedaten](../../azure-monitor/app/api-filtering-sampling.md).
- [Konfigurationsreferenz für Application Insights](configuration-with-applicationinsights-config.md)

