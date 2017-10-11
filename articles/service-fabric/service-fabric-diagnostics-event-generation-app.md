---
title: "Azure Service Fabric-Anwendungsebene Überwachung | Microsoft Docs"
description: "Informationen Sie zur Anwendung und Dienstereignisse und Protokolle, die zum Überwachen und Analysieren von Azure Service Fabric-Cluster verwendet."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/26/2017
ms.author: dekapur
ms.openlocfilehash: 3c472904641108b7383cd0f1416c47460f8de11a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="application-and-service-level-event-and-log-generation"></a>Anwendung und Servicelevel-Ereignis und Protokolldateien generation

## <a name="instrumenting-the-code-with-custom-events"></a>Instrumentieren den Code mit benutzerdefinierten Ereignissen

Instrumentieren des Codes ist die Grundlage für die meisten anderen Aspekte bei der Überwachung Ihrer Dienste. Instrumentation ist die einzige Möglichkeit, Sie können davon ausgehen, dass etwas falsch und diagnostizieren, was behoben werden muss. Zwar technisch möglich, einen Debugger an einen Produktionsdienst zu verbinden, ist es nicht üblich. Ausführliche Instrumentationsdaten ist deshalb wichtig.

Einige Produkte Instrumentieren automatisch Code. Obwohl diese Lösungen gut funktionieren können, ist manueller Instrumentation fast immer erforderlich. Letztlich benötigen Sie ausreichende Informationen forensisch Debuggen die Anwendung. Dieses Dokument beschreibt verschiedene Ansätze zum Instrumentieren des Codes und wann ein Vergleich mit anderen kennen von beiden Ansätzen.

## <a name="eventsource"></a>eventSource
Bei der Erstellung einer Service Fabric-Lösung aus einer Vorlage in Visual Studio ein **EventSource**-Klasse (**ServiceEventSource** oder **ActorEventSource**) wird generiert. Eine Vorlage wird erstellt, in dem Sie Ereignisse für Ihre Anwendung oder Dienst hinzufügen können. Die **EventSource** Namen **müssen** eindeutig sein und sollte aus der Vorlagenzeichenfolge "Standard" - "MyCompany" umbenannt werden&lt;Lösung&gt;-&lt;Projekt&gt;. Mehrere **EventSource** Definitionen, die den gleichen Namen verwenden bewirkt, dass ein Problem bei der Ausführung. Jedes benutzerdefinierte Ereignis muss einen eindeutigen Bezeichner verfügen. Wenn ein Bezeichner nicht eindeutig ist, ein Common Language Runtime-Fehler auftritt ist. Einige Organisationen Konfigurationsparameter im Voraus zuweisen Wertebereiche für Bezeichner zur Vermeidung von Konflikten zwischen separaten Entwicklungsteams. Weitere Informationen finden Sie unter [Vances Blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) oder [MSDN-Dokumentation](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

### <a name="using-structured-eventsource-events"></a>Verwendung von strukturierter EventSource-Ereignissen

Jedes der Ereignisse in den Codebeispielen in diesem Abschnitt werden für einen bestimmten Fall z. B. beim definiert ein Diensttyp registriert ist. Wenn Sie Nachrichten definieren, indem Sie Anwendungsfall-, Daten können mit dem Text des Fehlers verpackt werden, können Sie weitere problemlos suchen und Filter basierend auf den Namen oder die Werte der angegebenen Eigenschaften. Strukturieren der Instrumentation Ausgabe macht erfordern es einfacher, nutzen, jedoch mehr, um herauszufinden, und mehr Zeit ein neues Ereignis für jeden Verwendungsfall definieren. Bestimmte Ereignisdefinitionen können in der gesamten Anwendung freigegeben werden. Ereignis würde über viele Dienste innerhalb einer Anwendung wiederverwendet werden, z. B. eine Methode starten oder beenden. Domänenspezifische Dienst, wie ein System, möglicherweise eine **CreateOrder** -Ereignis, das einen eigenen eindeutigen des Ereignisses. Dieser Ansatz kann viele Ereignisse generieren und möglicherweise über Projektteams für Bezeichner koordiniert werden müssen. 

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The instance constructor is private to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        // The ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // The ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```

### <a name="using-eventsource-generically"></a>Verwenden von EventSource generisch

Da bestimmte Ereignisse definieren schwierig sein kann, definieren Sie viele Personen einige Ereignisse mit einem gemeinsamen Satz von Parametern, die in der Regel ihre Informationen als Zeichenfolge ausgegeben. Ein großer Teil der strukturierten Aspekt ist verloren, und es ist schwieriger zu durchsuchen und Filtern der Ergebnisse. Bei diesem Ansatz werden einige Ereignisse, die in der Regel die Protokolliergrade entsprechen definiert. Im folgenden Codeausschnitt wird eine Nachricht Debug- und Fehler definiert:

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The Instance constructor is private, to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        private const int DebugEventId = 10;
        [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
        public void Debug(string msg)
        {
            WriteEvent(DebugEventId, msg);
        }

        private const int ErrorEventId = 11;
        [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
        public void Error(string error, string msg)
        {
            WriteEvent(ErrorEventId, error, msg);
        }
```

Verwenden eine hybridlösung von strukturierten und generische Instrumentation kann auch funktionieren gut. Strukturierte Instrumentation wird verwendet, für die Berichterstattung von Fehlern und Metriken. Generische Ereignisse können für die ausführliche Protokollierung verwendet werden, die für die Problembehandlung von Engineers verarbeitet wird.

## <a name="aspnet-core-logging"></a>ASP.NET Core Protokollierung

Es ist wichtig, sorgfältig planen, wie Sie Ihren Code instrumentiert werden. Der richtige Instrumentation Plan helfen Ihnen die potenziell zur Destabilisierung der Codebasis und müssen dann den Code reinstrument vermeiden. Um das Risiko zu reduzieren, können Sie auswählen, wie ein instrumentationsbibliothek [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), Bestandteil von Microsoft ASP.NET Core. ASP.NET Core verfügt über eine [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) Schnittstelle, die Sie mit dem Anbieter Ihrer Wahl verwenden können, und minimiert gleichzeitig die Auswirkungen auf vorhandenem Code. Verwenden Sie den Code in ASP.NET Core unter Windows und Linux, und im vollständigen .NET Framework, also Codes Instrumentation standardisiert ist. Dies wird weiter unten untersucht:

### <a name="using-microsoftextensionslogging-in-service-fabric"></a>Verwenden von Microsoft.Extensions.Logging in Service Fabric

1. Fügen Sie das Microsoft.Extensions.Logging NuGet-Paket für das Projekt zu instrumentieren. Fügen Sie alle Pakete, die Anbieter zusätzlich hinzu (eines Pakets von Drittanbietern finden Sie im folgende Beispiel). Weitere Informationen finden Sie unter [Protokollierung in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Hinzufügen einer **mit** -Direktive für Microsoft.Extensions.Logging auf die Dienstdatei.
3. Definieren Sie eine private Variable innerhalb Ihrer Dienstklasse.

  ```csharp
  private ILogger _logger = null;

  ```
4. Fügen Sie im Konstruktor der Dienstklasse diesen Code hinzu:

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. Starten Sie in den Methoden Code zu instrumentieren. Hier sind einige Beispiele:

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
  // Later in the article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a>Mithilfe von anderen Anbietern Protokollierung

Einige Drittanbieter verwenden des Ansatzes im vorherigen Abschnitt beschriebenen einschließlich [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), und [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). Schließen Sie jede dieser in ASP.NET Core Protokollierung oder können Sie sie einzeln verwenden. Serilog verfügt über eine Funktion, die alle Nachrichten von einer Protokollierung zu verbessern. Diese Funktion kann nützlich, um den Dienstnamen, den Typ und die Partitionsinformationen ausgegeben wird. Um diese Funktion in der Kerninfrastruktur von ASP.NET verwenden, führen Sie folgende Schritte aus:

1. Fügen Sie die Serilog Serilog.Extensions.Logging und Serilog.Sinks.Observable NuGet-Pakete zum Projekt hinzu. Fügen Sie für das nächste Beispiel auch Serilog.Sinks.Literate hinzu. Ein besserer Ansatz ist weiter unten in diesem Artikel dargestellt.
2. Erstellen Sie in Serilog eine LoggerConfiguration und die Protokollierungsinstanz.

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. Der Dienstkonstruktor ein Serilog.ILogger-Argument hinzu, und übergeben Sie die neu erstellte Protokollierung.

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. Im Dienstkonstruktor, fügen Sie den folgenden Code, der die Eigenschaft Enrichers für erstellt die **ServiceTypeName**, **ServiceName**, **PartitionId**, und **InstanceId** Eigenschaften des Diensts. Außerdem hinzugefügt eine Eigenschaft Enricher mit der ASP.NET Core Protokollierung Factory, damit Sie Microsoft.Extensions.Logging.ILogger in Ihrem Code verwenden können.

  ```csharp
  public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
      : base(context)
  {
      PropertyEnricher[] properties = new PropertyEnricher[]
      {
          new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
          new PropertyEnricher("ServiceName", context.ServiceName),
          new PropertyEnricher("PartitionId", context.PartitionId),
          new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
      };

      serilog.ForContext(properties);

      _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
  }
  ```

5. Instrumentieren des Codes die gleiche wie bei Verwendung von ASP.NET Core ohne Serilog wurden.

  >[!NOTE]
  >Es wird empfohlen, dass Sie nicht die statische Log.Logger das vorangehende Beispiel verwenden. Service Fabric können mehrere Instanzen des gleichen Diensttyps innerhalb eines einzelnen Prozesses gehostet werden. Wenn Sie die statische Log.Logger verwenden, wird der letzte Writer, der die Eigenschaft Enrichers Werte für alle Instanzen angezeigt, die ausgeführt werden. Dies ist ein Grund, warum die _logger-Variable für eine Private Membervariable der Dienstklasse ist. Darüber hinaus müssen Sie die _logger gemeinsamer Code zur Verfügung stellen, die in Diensten verwendet werden kann.

## <a name="choosing-a-logging-provider"></a>Auswählen einer-Protokollierungsanbieter

Wenn Ihre Anwendung hohe Leistung, benötigt **EventSource** ist normalerweise ein guter Ansatz. **EventSource** *im allgemeinen* weniger Ressourcen beansprucht und bietet eine bessere Leistung als ASP.NET Core Protokollierung und keines der verfügbaren Drittanbieter-Lösungen.  Dies ist ein Problem für zahlreiche Dienste, aber wenn Sie Ihren Dienst Leistung-orientierten mit nicht **EventSource** möglicherweise die bessere Wahl sein. Jedoch folgende Vorteile des abzurufenden strukturiert Protokollierung **EventSource** erfordert eine größere Investition von engineering-Team. Wenn möglich, führen Sie einen schnellen Prototyp des einige Optionen für die Protokollierung, und wählen Sie dann das Projekt, das Ihren Anforderungen am besten entspricht.

## <a name="next-steps"></a>Nächste Schritte

Sobald Sie Ihren Protokollierungsanbieter für die zum Instrumentieren von Anwendungen und Dienste ausgewählt haben, müssen die Protokolle und Ereignisse aggregiert werden, bevor sie an einer beliebigen Plattform Analysis gesendet werden können. Erfahren Sie, [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) und [WAD](service-fabric-diagnostics-event-aggregation-wad.md) um einige der empfohlenen Optionen besser zu verstehen.
