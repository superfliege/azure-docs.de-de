---
title: Anwendungszuordnung in Azure Application Insights | Microsoft-Dokumentation
description: Überwachen von komplexen Anwendungstopologien mit der Anwendungsübersicht
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/17/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: e89ba38b7993b8f2d17704ff592eecd3d02d303e
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53730598"
---
# <a name="application-map-triage-distributed-applications"></a>Anwendungsübersicht: Selektieren verteilter Anwendungen

Mit der Anwendungsübersicht können Sie Leistungsengpässe oder Fehlerhotspots in allen Komponenten Ihrer verteilten Anwendung erkennen. Jeder Knoten in der Übersicht repräsentiert eine Anwendungskomponente bzw. deren Abhängigkeiten und weist einen Key Performance Indicator (KPI) zur Integrität und einen Warnungsstatus auf. Für jede Komponente können Sie detailliertere Diagnosen anzeigen, z.B. für Application Insights-Ereignisse. Wenn Ihre App Azure-Dienste nutzt, können Sie auch Azure-Diagnosedaten anzeigen, z.B. SQL Database Advisor-Empfehlungen.

## <a name="what-is-a-component"></a>Was ist eine Komponente?

Komponenten sind unabhängig bereitstellbare Teile Ihrer verteilten Anwendung/Microserviceanwendung. Entwickler und Betriebsteams verfügen über Sichtbarkeit oder Zugriff für die von diesen Anwendungskomponenten generierte Telemetrie auf Codeebene. 

* Komponenten unterscheiden sich von „beobachteten“ externen Abhängigkeiten, wie SQL, EventHub usw., auf die Ihr Team/Ihre Organisation möglicherweise keinen Zugriff hat (Code oder Telemetrie).
* Komponenten können in einer beliebigen Anzahl von Server-/Rollen-/Containerinstanzen ausgeführt werden.
* Komponenten können separate Application Insights-Instrumentierungsschlüssel (sogar in verschiedenen Abonnements) oder verschiedene Rollen aufweisen, die alle an einen einzelnen Application Insights-Instrumentierungsschlüssel berichten. In der Vorschau werden die Komponenten unabhängig von der Art ihrer Einrichtung in der Übersicht angezeigt.

## <a name="composite-application-map"></a>Verbundanwendungsübersicht

Sie können die vollständige Anwendungstopologie über mehrere Ebenen verwandter Anwendungskomponenten hinweg anzeigen. Bei Komponenten kann es sich um verschiedene Application Insights-Ressourcen oder verschiedene Rollen in einer einzelnen Ressource handeln. Die App-Übersicht ermittelt Komponenten, indem sie HTTP-Abhängigkeitsaufrufe zwischen Servern verfolgt, auf denen das Application Insights SDK installiert ist. 

Diese Funktionalität beginnt mit einer progressiven Ermittlung der Komponenten. Wenn Sie die Anwendungsübersicht zum ersten Mal laden, wird eine Reihe von Abfragen ausgelöst, um die Komponenten zu ermitteln, die mit dieser Komponente in Zusammenhang stehen. Mit einer Schaltfläche in der oberen linken Ecke wird die Anzahl der Komponenten in Ihrer Anwendung aktualisiert, wenn diese ermittelt werden. 

Durch Klicken auf „Übersichtskomponenten aktualisieren“ wird die Übersicht mit allen bis zu diesem Zeitpunkt ermittelten Komponenten aktualisiert. Der Ladevorgang kann je nach Komplexität der Anwendung etwas dauern.

Wenn es sich bei allen Komponenten um Rollen in einer einzelnen Application Insights-Ressource handelt, ist dieser Ermittlungsschritt nicht erforderlich. Nach dem ersten Ladevorgang für eine solche Anwendung sind alle Komponenten vorhanden.

![Screenshot: Anwendungsübersicht](media/app-map/001.png)

Eines der wichtigsten Ziele dieser Oberfläche ist es, komplexe Topologien mit Hunderten von Komponenten zu visualisieren.

Klicken Sie auf eine beliebige Komponente, um die zugehörigen Details anzuzeigen und zur Selektierung von Leistungs- und Fehleraspekten für diese Komponente zu wechseln.

![Flyout](media/app-map/application-map-001.png)

### <a name="investigate-failures"></a>Untersuchen von Fehlern

Wählen Sie **Fehler untersuchen**, um den Bereich „Fehler“ zu starten.

![Screenshot: Schaltfläche „Fehler untersuchen“](media/app-map/investigate-failures.png)

![Screenshot: Bereich „Fehler“](media/app-map/failures.png)

### <a name="investigate-performance"></a>Untersuchen der Leistung

Wählen Sie **Leistung untersuchen** aus, um Probleme mit der Leistung zu behandeln.

![Screenshot: Schaltfläche „Leistung untersuchen“](media/app-map/investigate-performance.png)

![Screenshot: Benutzeroberfläche für Leistung](media/app-map/performance.png)

### <a name="go-to-details"></a>Zu Details wechseln

Wählen Sie **Zu Details wechseln** aus, um den Bereich für End-to-End-Transaktionen zu erkunden, in dem Informationen zur Aufrufliste angezeigt werden.

![Screenshot: Schaltfläche „Zu Details wechseln“](media/app-map/go-to-details.png)

![Screenshot: Details von End-to-End-Transaktionen](media/app-map/end-to-end-transaction.png)

### <a name="view-in-analytics"></a>In Analytics anzeigen

Klicken Sie auf **In Analytics anzeigen**, um Ihre Anwendungsdaten abzufragen und eingehender zu untersuchen.

![Screenshot: Schaltfläche „In Analytics anzeigen“](media/app-map/view-in-analytics.png)

![Screenshot: Analytics-Bereich](media/app-map/analytics.png)

### <a name="alerts"></a>Alerts

Wählen Sie **Warnungen** aus, um aktive Warnungen und die zugrunde liegenden Regeln anzuzeigen, die zur Ausführung der Warnungen führen.

![Screenshot: Schaltfläche „Warnungen“](media/app-map/alerts.png)

![Screenshot: Analytics-Bereich](media/app-map/alerts-view.png)

## <a name="set-cloudrolename"></a>Festlegen von „cloud_RoleName“

Die Anwendungsübersicht verwendet die Eigenschaft `cloud_RoleName`, um die Komponenten in der Zuordnung zu identifizieren. Das Application Insights SDK versieht die von Komponenten ausgegebenen Telemetriedaten automatisch mit der Eigenschaft `cloud_RoleName`. So fügt das SDK der Eigenschaft `cloud_RoleName` beispielsweise einen Websitenamen oder einen Dienstrollennamen hinzu. Manchmal soll der Standardwert jedoch möglicherweise überschrieben werden. So können Sie „cloud_RoleName“ überschreiben und den Inhalt der Anwendungsübersicht ändern:

### <a name="net"></a>.NET

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                //set custom role name here
                telemetry.Context.Cloud.RoleName = "RoleName";
            }
        }
    }
}
```

**Laden des Initialisierers**

In "ApplicationInsights.config":

```xml
    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="CustomInitializer.Telemetry.MyTelemetryInitializer, CustomInitializer"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>
```

Alternativ können Sie den Initialisierer im Code instanziieren (beispielsweise in „Global.aspx.cs“):

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
    }
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();
appInsights.defaultClient.context.tags["ai.cloud.role"] = "your role name";
appInsights.defaultClient.context.tags["ai.cloud.roleInstance"] = "your role instance";
```

### <a name="alternate-method-for-nodejs"></a>Alternative Methode für Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();

appInsights.defaultClient.addTelemetryProcessor(envelope => {
    envelope.tags["ai.cloud.role"] = "your role name";
    envelope.tags["ai.cloud.roleInstance"] = "your role instance"
});
```

### <a name="java"></a>Java

Bei Verwendung von Spring Boot mit dem Application Insights Spring Boot-Startprogramm besteht die einzige erforderliche Änderung darin, Ihren benutzerdefinierten Namen für die Anwendung in der Datei „application.properties“ festzulegen.

`spring.application.name=<name-of-app>`

Das Spring Boot-Startprogramm weist dem von Ihnen für die Eigenschaft „spring.application.name“ eingegebenen Wert automatisch „cloudRoleName“ zu.

Weitere Informationen zur Java-Korrelation und zum Konfigurieren von „cloudRoleName“ für SpringBoot-fremde Anwendungen finden Sie in [diesem Abschnitt](https://docs.microsoft.com/azure/application-insights/application-insights-correlation#role-name) zur Korrelation.

### <a name="clientbrowser-side-javascript"></a>Client-/Browserseitiger JavaScript-Code

```javascript
appInsights.queue.push(() => {
appInsights.context.addTelemetryInitializer((envelope) => {
  envelope.tags["ai.cloud.role"] = "your role name";
  envelope.tags["ai.cloud.roleInstance"] = "your role instance";
});
});
```

Weitere Informationen zum Überschreiben der Eigenschaft „cloud_RoleName“ mit Telemetrieinitialisierern finden Sie unter [Add properties: ITelemetryInitializer](api-filtering-sampling.md#add-properties-itelemetryinitializer) (Hinzufügen von Eigenschaften: ITelemetryInitializer).

## <a name="troubleshooting"></a>Problembehandlung

Bei Problemen mit der Anwendungsübersicht können Sie Folgendes versuchen:

1. Vergewissern Sie sich, dass Sie ein offiziell unterstütztes SDK verwenden. Nicht unterstützte SDKs oder Community-SDKs unterstützen möglicherweise keine Korrelation.

    Eine Liste mit unterstützten SDKs finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).

2. Upgraden Sie alle Komponenten auf die neueste SDK-Version.

3. Falls Sie Azure Functions mit C# verwenden, führen Sie ein Upgrade auf [Functions V2](https://docs.microsoft.com/azure/azure-functions/functions-versions) durch.

4. Vergewissern Sie sich, dass [cloud_RoleName](app-map.md#Set-cloud-RoleName) ordnungsgemäß konfiguriert ist.

5. Sollte eine Abhängigkeit fehlen, stellen Sie sicher, dass sie sich in der Liste [automatisch erfasster Abhängigkeiten](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies) befindet. Andernfalls können Sie sie auch manuell mit einem [TrackDependency-Aufruf](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency) nachverfolgen.

## <a name="portal-feedback"></a>Feedback zum Portal
Verwenden Sie für Feedback die Feedbackoption.

![MapLink-1 (Abbildung)](./media/app-map/14-updated.png)

## <a name="next-steps"></a>Nächste Schritte

* [Telemetriekorrelation in Application Insights](https://docs.microsoft.com/azure/application-insights/application-insights-correlation)
