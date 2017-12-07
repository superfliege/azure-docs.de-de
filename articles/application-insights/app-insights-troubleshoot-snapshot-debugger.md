---
title: "Azure Application Insights Snapshot Debugger – Leitfaden zur Problembehandlung | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Azure Application Insights Snapshot Debugger."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 
ms.service: application-insights
ms.workload: 
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mbullwin
ms.openlocfilehash: 5d6a2fe4c3ee373172f5324b6c7a39e4f94f4652
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="snapshot-debugger-troubleshooting-guide"></a>Snapshot Debugger: Leitfaden zur Problembehandlung

Mit Application Insights Snapshot Debugger können Sie automatisch eine Debugmomentaufnahme von aktiven Webanwendungen erfassen. Die Momentaufnahme zeigt den Status des Quellcodes und der Variablen zu dem Zeitpunkt an, zu dem eine Ausnahme ausgelöst wurde. Wenn bei der Verwendung von Application Insights Snapshot Debugger Probleme auftreten, finden Sie in diesem Artikel eine Anleitung zur Funktionsweise des Debuggers sowie Lösungen für allgemeine Szenarien zur Problembehandlung. 

## <a name="how-does-application-insights-snapshot-debugger-work"></a>Funktionsweise von Application Insights Snapshot Debugger

Application Insights Snapshot Debugger ist Teil der Application Insights-Telemetriepipeline (eine Instanz von ITelemetryProcessor). Mit dem Snapshot Collector werden die im Code ausgelösten Ausnahmen (AppDomain.FirstChanceException) sowie die in der Application Insights Exception-Ausnahmetelemetriepipeline nachverfolgten Ausnahmen überwacht. Nach dem erfolgreichen Hinzufügen des Snapshot Collector zu Ihrem Projekt und dem Erkennen einer Ausnahme in der Application Insights-Telemetriepipeline wird ein benutzerdefiniertes Application Insights-Ereignis mit dem Namen „AppInsightsSnapshotCollectorLogs“ und „SnapshotCollectorEnabled“ in den benutzerdefinierten Daten gesendet. Gleichzeitig wird der Prozess „MinidumpUploader.exe“ gestartet, um die erfassten Momentaufnahmen-Datendateien in Application Insights hochzuladen.  Beim Starten des Prozesses „MinidumpUploader.exe“ wird das benutzerdefinierte Ereignis „UploaderStart“ gesendet. Nach den vorherigen Schritten wird der Snapshot Collector mit dem normalen Überwachungsverhalten ausgeführt.

Während der Snapshot Collector die Application Insights-Ausnahmetelemetrie überwacht, werden die Parameter (z.B. ThresholdForSnapshotting, MaximumSnapshotsRequired, MaximumCollectionPlanSize, ProblemCounterResetInterval) verwendet, die in der Konfiguration definiert sind, um zu bestimmen, wann eine Momentaufnahme erfasst wird. Wenn alle Regeln erfüllt sind, fordert der Collector eine Momentaufnahme für die nächste am selben Ort ausgelöste Ausnahme an. Parallel wird ein benutzerdefiniertes Application Insights-Ereignis mit dem Namen „AppInsightsSnapshotCollectorLogs“ und „RequestSnapshots“ gesendet. Da der Compiler Freigabecode optimiert, sind lokale Variablen in der erfassten Momentaufnahme möglicherweise nicht sichtbar. Der Snapshot Collector versucht beim Anfordern von Momentaufnahmen, die Methode zu deoptimieren, die die Ausnahme ausgelöst hat. Während dieser Zeit wird ein benutzerdefiniertes Application Insights-Ereignis mit dem Namen „AppInsightsSnapshotCollectorLogs“ und „ProductionBreakpointsDeOptimizeMethod“ in den benutzerdefinierten Daten gesendet.  Wenn die Momentaufnahme der nächsten Ausnahme erfasst wird, sind die lokalen Variablen verfügbar. Nachdem die Momentaufnahme erfasst wurde, wird der Code wieder optimiert, um die Leistung zu gewährleisten. 

> [!NOTE]
> Für die Deoptimierung muss die Application Insights-Websiteerweiterung installiert werden.

Wenn eine Momentaufnahme für eine bestimmte Ausnahme angefordert wird, startet der Snapshot Collector die Überwachung der Ausnahmebehandlungspipeline der Anwendung (AppDomain.FirstChanceException). Wenn die Ausnahme erneut auftritt, startet der Collector eine Momentaufnahme (benutzerdefiniertes Application Insights-Ereignis mit dem Namen „AppInsightsSnapshotCollectorLogs“ und „SnapshotStart“ in den benutzerdefinierten Daten). Anschließend wird eine Schattenkopie des ausgeführten Prozesses erstellt (die Seitentabelle wird dupliziert). Dies dauert normalerweise 10 bis 20 Millisekunden. Anschließend wird ein benutzerdefiniertes Application Insights-Ereignis mit dem Namen „AppInsightsSnapshotCollectorLogs“ und „SnapshotStop“ in den benutzerdefinierten Daten gesendet. Wenn der verzweigte Prozess erstellt wird, wird der gesamte ausgelagerte Speicher um dieselbe Menge wie die des ausgelagerten Speichers der ausgeführten Anwendung erhöht (der Arbeitssatz ist wesentlich kleiner). Während der Anwendungsprozess normal ausgeführt wird, wird der Speicher des Prozesses mit erstellter Schattenkopie auf dem Datenträger gesichert und in Application Insights hochgeladen. Nach dem Hochladen der Momentaufnahme wird ein benutzerdefiniertes Application Insights-Ereignis mit dem Namen „UploadSnapshotFinish“ gesendet.

## <a name="is-the-snapshot-collector-working-properly"></a>Wird der Snapshot Collector ordnungsgemäß ausgeführt?

### <a name="how-to-find-snapshot-collector-logs"></a>Suchen von Snapshot Collector-Protokollen
Snapshot Collector-Protokolle werden an Ihr Application Insights-Konto gesendet, wenn das [Snapshot Collector-NuGet-Paket](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) Version 1.1.0 oder höher installiert ist. Stellen Sie sicher, dass *ProvideAnonymousTelemetry* nicht auf „FALSE“ festgelegt ist (der Standardwert ist „TRUE“).

* Navigieren Sie im Azure-Portal zu Ihrer Application Insights-Ressource.
* Klicken Sie im Abschnitt „Übersicht“ auf *Suchen*.
* Geben Sie im Suchfeld die folgende Zeichenfolge ein:
    ```
    AppInsightsSnapshotCollectorLogs OR AppInsightsSnapshotUploaderLogs OR UploadSnapshotFinish OR UploaderStart OR UploaderStop
    ```
* Hinweis: Ändern Sie bei Bedarf den *Zeitbereich*.

![Screenshot: Suchen der Snapshot Collector-Protokolle](./media/app-insights-troubleshoot-snapshot-debugger/001.png)


### <a name="examine-snapshot-collector-logs"></a>Überprüfen von Snapshot Collector-Protokollen
Bei der Suche nach Snapshot Collector-Protokollen sollten sich „UploadSnapshotFinish“-Ereignisse im Zielzeitbereich befinden. Wenn die Schaltfläche „Debugmomentaufnahme öffnen“ zum Öffnen der Momentaufnahme nicht angezeigt wird, senden Sie eine E-Mail mit Ihrem Application Insights-Instrumentierungsschlüssel an snapshothelp@microsoft.com.

![Screenshot: Überprüfen von Snapshot Collector-Protokollen](./media/app-insights-troubleshoot-snapshot-debugger/005.png)

## <a name="i-cannot-find-a-snapshot-to-open"></a>Es wird keine Momentaufnahme zum Öffnen angezeigt
Wenn Sie das Problem mit den folgenden Schritten nicht beheben können, senden Sie eine E-Mail mit Ihrem Application Insights-Instrumentierungsschlüssel an snapshothelp@microsoft.com.

### <a name="step-1-make-sure-your-application-is-sending-telemetry-data-and-exception-data-to-application-insights"></a>Schritt 1: Sicherstellen, dass die Anwendung Telemetrie- und Ausnahmedaten an Application Insights sendet
Navigieren Sie zu der Application Insights-Ressource, und überprüfen Sie, ob Daten von Ihrer Anwendung gesendet werden.

### <a name="step-2-make-sure-snapshot-collector-is-added-correctly-to-your-applications-application-insights-telemetry-pipeline"></a>Schritt 2: Sicherstellen, dass Snapshot Collector der Application Insights-Telemetriepipeline Ihrer Anwendung ordnungsgemäß hinzugefügt wurde
Wenn im Schritt „Suchen von Snapshot Collector-Protokollen“ Protokolle angezeigt werden, wurde der Snapshot Collector Ihrem Projekt ordnungsgemäß hinzugefügt, und Sie können diesen Schritt überspringen.

Wenn keine Snapshot Collector-Protokolle vorhanden sind, überprüfen Sie Folgendes:
* Überprüfen Sie bei klassischen ASP.NET-Anwendungen, ob die Zeile *<Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">* in der Datei *ApplicationInsights.config* vorhanden ist.

* Stellen Sie bei ASP.NET Core-Anwendungen sicher, dass den *IServiceCollection*-Diensten *ITelemetryProcessorFactory* mit *SnapshotCollectorTelemetryProcessor* hinzugefügt wurde.

* Überprüfen Sie auch, ob Sie den richtigen Instrumentierungsschlüssel in der veröffentlichten Anwendung verwenden.

* Der Snapshot Collector unterstützt nicht mehrere Instrumentierungsschlüssel innerhalb einer Anwendung. Momentaufnahmen werden an den Instrumentierungsschlüssel der ersten erkannten Ausnahme gesendet.

* Wenn Sie *InstrumentationKey* im Code manuell festlegen, aktualisieren Sie das *InstrumentationKey*-Element aus der Datei *ApplicationInsights.config*.

### <a name="step-3-make-sure-the-minidump-uploader-is-started"></a>Schritt 3: Sicherstellen, dass der Minidump-Uploader gestartet wird
Suchen Sie in den Snapshot Collector-Protokollen nach *UploaderStart*. (Geben Sie dazu „UploaderStart“ im Suchtextfeld ein.) Es sollte ein Ereignis vorhanden sein, wenn der Snapshot Collector die erste Ausnahme überwacht hat. Wenn dieses Ereignis nicht vorhanden ist, suchen Sie in anderen Protokollen nach Details. Eine Möglichkeit zum Beheben dieses Problems besteht darin, die Anwendung neu zu starten.

### <a name="step-4-make-sure-snapshot-collector-expressed-its-intent-to-collect-snapshots"></a>Schritt 4: Sicherstellen, dass der Snapshot Collector Momentaufnahmen erfasst
Suchen Sie in den Snapshot Collector-Protokollen nach *RequestSnapshots*. (Geben Sie dazu ```RequestSnapshots``` im Suchtextfeld ein.)  Wenn keine Momentaufnahme vorhanden ist, überprüfen Sie Ihre Konfiguration, z.B. auf *ThresholdForSnapshotting*. Dies gibt die Anzahl bestimmter Ausnahmen an, die auftreten können, bevor Momentaufnahmen erfasst werden.

### <a name="step-5-make-sure-that-snapshot-is-not-disabled-due-to-memory-protection"></a>Schritt 5: Sicherstellen, dass die Momentaufnahme nicht aufgrund des Speicherschutzes deaktiviert ist
Zum Schutz der Leistung Ihrer Anwendung wird eine Momentaufnahme nur bei ausreichendem Speicherpuffer erfasst. Suchen Sie in den Snapshot Collector-Protokollen nach „CannotSnapshotDueToMemoryUsage“. In den benutzerdefinierten Daten des Ereignisses ist ein detaillierter Grund angegeben. Wenn Ihre Anwendung in einer Azure-Web-App ausgeführt wird, kann die Einschränkung strikt sein. Azure Web App startet Ihre Anwendung neu, wenn bestimmte Speicherregeln erfüllt sind. Sie können versuchen, Ihren Dienstplan für Computer mit größerem Speicher zentral hochzuskalieren, um dieses Problem zu beheben.

### <a name="step-6-make-sure-snapshots-were-captured"></a>Schritt 6: Sicherstellen, dass Momentaufnahmen erfasst wurden
Suchen Sie in den Snapshot Collector-Protokollen nach ```RequestSnapshots```.  Wenn keine Momentaufnahmen vorhanden sind, überprüfen Sie die Konfiguration, z.B. auf ```ThresholdForSnapshotting```. Dies gibt die Anzahl bestimmter Ausnahmen an, die auftreten können, bevor Momentaufnahmen erfasst werden.

### <a name="step-7-make-sure-snapshots-are-uploaded-correctly"></a>Schritt 7: Sicherstellen, dass Momentaufnahmen ordnungsgemäß hochgeladen werden
Suchen Sie in den Snapshot Collector-Protokollen nach ```UploadSnapshotFinish```.  Wenn dieses Ereignis nicht vorhanden ist, senden Sie eine E-Mail mit Ihrem Application Insights-Instrumentierungsschlüssel an snapshothelp@microsoft.com. Wenn dieses Ereignis vorhanden ist, öffnen Sie eines der Protokolle, und kopieren Sie den Wert „SnapshotId“ in die benutzerdefinierten Daten. Suchen Sie dann nach dem Wert. Dadurch wird die Ausnahme gesucht, die der Momentaufnahme entspricht. Klicken Sie auf die Ausnahme, und öffnen Sie die Debugmomentaufnahme. (Wenn keine entsprechende Ausnahme vorhanden ist, können für die Ausnahmetelemetrie Stichproben erstellt werden. Verwenden Sie aufgrund des hohen Volumens einen anderen SnapshotId-Wert.)

![Screenshot: Suchen von SnapshotId](./media/app-insights-troubleshoot-snapshot-debugger/002.png)

![Screenshot: geöffnete Ausnahme](./media/app-insights-troubleshoot-snapshot-debugger/004b.png)

![Screenshot: Debugmomentaufnahme öffnen](./media/app-insights-troubleshoot-snapshot-debugger/003.png)

## <a name="snapshot-view-local-variables-are-not-complete"></a>Lokale Variablen in der Momentaufnahmenansicht sind nicht vollständig

Einige der lokalen Variablen sind nicht vorhanden. Wenn Ihre Anwendung Freigabecode ausführt, werden einige Variablen vom Compiler zur Optimierung entfernt. Beispiel:

```csharp
    const int a = 1; // a will be discarded by compiler and the value 1 will be inline.
    Random rand = new Random();
    int b = rand.Next() % 300; // b will be discarded and the value will be directly put to the 'FindNthPrimeNumber' call stack.
    long primeNumber = FindNthPrimeNumber(b);
```

Wenn es sich in Ihrem Fall anders verhält, kann es sich um einen Fehler handeln. Senden Sie eine E-Mail mit Ihrem Application Insights-Instrumentierungsschlüssel und dem Codeausschnitt an snapshothelp@microsoft.com.

## <a name="snapshot-view-cannot-obtain-value-of-the-local-variable-or-argument"></a>Momentaufnahmenansicht: Wert der lokalen Variable oder des Arguments kann nicht abgerufen werden
Stellen Sie sicher, dass die [Application Insights-Websiteerweiterung](https://www.siteextensions.net/packages/Microsoft.ApplicationInsights.AzureWebSites/) installiert ist. Wenn das Problem weiterhin besteht, senden Sie eine E-Mail mit Ihrem Application Insights-Instrumentierungsschlüssel an snapshothelp@microsoft.com.
