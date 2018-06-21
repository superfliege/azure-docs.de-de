---
title: Verwenden des Azure WebJobs SDK
description: Erfahren Sie mehr über das Schreiben von Code für das WebJobs SDK. Erstellen Sie ereignisgesteuerte Hintergrundverarbeitungsaufträge, die auf Daten in Azure-Diensten und Diensten von Drittanbietern zugreifen.
services: app-service\web, storage
documentationcenter: .net
author: tdykstra
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/27/2018
ms.author: tdykstra
ms.openlocfilehash: 08272ba7d828f744336723f25b482bf06b9e43dc
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234649"
---
# <a name="how-to-use-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Verwenden des WebJobs SDK für die ereignisgesteuerte Hintergrundverarbeitung

Dieser Artikel enthält Anleitungen zum Schreiben von Code für das [Azure WebJobs SDK](webjobs-sdk-get-started.md). Die Dokumentation gilt für die Versionen 2.x und 3.x, sofern nicht anders vermerkt. Die wichtigste in 3.x eingeführte Änderung ist die Verwendung von .NET Core anstelle von .NET Framework.

>[!NOTE]
> [Azure Functions](../azure-functions/functions-overview.md) basiert auf dem WebJobs SDK. Dieser Artikel enthält bei einigen Themen Links zur Azure Functions-Dokumentation. Beachten Sie die folgenden Unterschiede zwischen Functions und dem WebJobs SDK:
> * Azure Functions Version 1.x entspricht der WebJobs SDK-Version 2.x, und Azure Functions 2.x entspricht WebJobs SDK 3.x. Quellcode-Repositorys folgen der WebJobs SDK-Nummerierung, und viele verfügen über v2.x-Branches, wobei der Masterbranch aktuell 3.x-Code aufweist.
> * Der Beispielcode für Azure Functions C#-Klassenbibliotheken entspricht dem WebJobs SDK-Code, mit der Ausnahme, dass Sie in einem WebJobs SDK-Projekt kein `FunctionName`-Attribut benötigen.
> * Einige Bindungstypen, z. B. HTTP, Webhook und Event Grid (der auf HTTP basiert), werden nur in Functions unterstützt. 
> 
> Weitere Informationen finden Sie unter [Vergleich von WebJobs SDK und Azure Functions](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs). 

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie [Erste Schritte mit dem WebJobs SDK](webjobs-sdk-get-started.md) gelesen haben.

## <a name="jobhost"></a>JobHost

Das `JobHost`-Objekt ist der Laufzeitcontainer für Funktionen: Damit werden Trigger überwacht und Funktionen aufgerufen. Sie erstellen das `JobHost`-Objekt in Ihrem Code und schreiben Code zum Anpassen des entsprechenden Verhaltens.

Das ist ein entscheidender Unterschied zwischen der direkten Verwendung des WebJobs SDK und der indirekten Verwendung mithilfe von Azure Functions. In Azure Functions steuert der Dienst das `JobHost`-Objekt, und Sie können es nicht durch das Schreiben von Code anpassen. In Azure Functions können Sie das Hostverhalten über die Einstellungen in der Datei *host.json* anpassen. Bei diesen Einstellungen handelt es sich um Zeichenfolgen, nicht um Code, wodurch die Arten von Anpassungen, die Sie vornehmen können, eingeschränkt werden.

### <a name="jobhost-connection-strings"></a>JobHost-Verbindungszeichenfolgen

Bei lokaler Ausführung sucht das WebJobs SDK in der Datei *local.settings.json* nach Storage- und Service Bus-Verbindungszeichenfolgen. Und bei der Ausführung in Azure wird in der WebJobs-Umgebung danach gesucht. Wenn Sie Ihre eigenen Namen für diese Verbindungszeichenfolgen verwenden oder sie woanders speichern möchten, können Sie diese im Code festlegen, wie hier gezeigt:

```cs
static void Main(string[] args)
{
    var _storageConn = ConfigurationManager
        .ConnectionStrings["MyStorageConnection"].ConnectionString;

    var _dashboardConn = ConfigurationManager
        .ConnectionStrings["MyDashboardConnection"].ConnectionString;

    JobHostConfiguration config = new JobHostConfiguration();
    config.StorageConnectionString = _storageConn;
    config.DashboardConnectionString = _dashboardConn;
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="jobhost-development-settings"></a>JobHost-Entwicklungseinstellungen

Die `JobHostConfiguration`-Klasse verfügt über eine `UseDevelopmentSettings`-Methode, die Sie aufrufen können, um die lokale Entwicklung effizienter zu gestalten. Nachfolgend finden Sie einige der Einstellungen, die durch diese Methode geändert werden:

| Eigenschaft | Entwicklungseinstellung |
| ------------- | ------------- |
| `Tracing.ConsoleLevel` | `TraceLevel.Verbose` zum Maximieren der Protokollausgabe. |
| `Queues.MaxPollingInterval`  | Ein niedriger Wert, um sicherzustellen, dass die Warteschlangenmethoden sofort ausgelöst werden.  |
| `Singleton.ListenerLockPeriod` | 15 Sekunden, um eine schnelle iterative Entwicklung zu erleichtern. |

Im folgenden Beispiel wird die Verwendung von Entwicklungseinstellungen aufgezeigt. Legen Sie eine lokale Umgebungsvariable namens `AzureWebJobsEnv` mit dem Wert `Development` fest, damit `config.IsDevelopment` bei lokaler Ausführung den Wert `true` zurückgibt.

```cs
static void Main()
{
    config = new JobHostConfiguration();

    if (config.IsDevelopment)
    {
        config.UseDevelopmentSettings();
    }

    var host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="jobhost-servicepointmanager-settings"></a>JobHost-ServicePointManager-Einstellungen

.NET Framework enthält eine API namens [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit), die die Anzahl der gleichzeitigen Verbindungen mit einem Host steuert. Wir empfehlen Ihnen, diesen Standardwert von 2 zu erhöhen, bevor Sie Ihren WebJobs-Host starten.

Alle ausgehenden HTTP-Anforderungen, die Sie über eine Funktion mit `HttpClient` ausführen, durchlaufen den `ServicePointManager`. Sobald Sie das `DefaultConnectionLimit` erreicht haben, stellt der `ServicePointManager` Anforderungen vor dem Senden in die Warteschlange. Angenommen, Ihr `DefaultConnectionLimit` ist auf 2 festgelegt, und Ihr Code führt 1.000 HTTP-Anforderungen aus. Anfangs sind nur 2 Anforderungen für das Betriebssystem zugelassen. Die anderen 998 Anforderungen werden in die Warteschlange eingereiht, bis genügend Platz für sie vorhanden ist. Das bedeutet, dass ein Timeout für Ihren `HttpClient` auftreten kann, weil er *denkt*, dass er die Anforderung vorgenommen hat, aber die Anforderung wurde nie vom Betriebssystem auf den Zielserver gesendet. So tritt möglicherweise ein Verhalten auf, das scheinbar keinen Sinn ergibt: Ihr lokaler `HttpClient` benötigt 10 Sekunden, um eine Anforderung abzuschließen, aber Ihr Dienst gibt jede Anforderung in 200 ms zurück. 

Der Standardwert für ASP.NET-Anwendungen ist `Int32.MaxValue`. Und dies funktioniert wahrscheinlich gut, wenn WebJobs in einem App Service-Plan des Typs „Basic“ oder höher ausgeführt wird. Für WebJobs ist in der Regel die Always On-Einstellung erforderlich, und diese wird nur von einem App Service-Plan des Typs „Basic“ oder höher unterstützt. 

Bei der Ausführung des WebJobs in einem App Service-Plan des Typs „Free“ oder „Shared“ wird Ihre Anwendung vom App Service-Sandkasten (Sandbox) eingeschränkt, für den aktuell ein [Verbindungslimit von 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits) gilt. Bei einem ungebundenen Verbindungslimit in `ServicePointManager` ist es wahrscheinlicher, dass der Schwellenwert für die Sandbox-Verbindung erreicht wird und die Website heruntergefahren wird. In diesem Fall kann dies durch Festlegen von `DefaultConnectionLimit` auf einen niedrigeren Wert (wie 50 oder 100) verhindert werden und trotzdem einen ausreichenden Durchsatz ermöglichen.

Die Einstellung muss vor dem Ausführen von HTTP-Anforderungen konfiguriert werden. Aus diesem Grund sollte der WebJobs-Host nicht versuchen, die Einstellung automatisch anzupassen. Möglicherweise treten HTTP-Anforderungen auf, bevor der Host startet, und dies kann zu unerwartetem Verhalten führen. Der beste Ansatz besteht darin, den Wert sofort in Ihrer `Main`-Methode festzulegen, bevor Sie den `JobHost` initialisieren, wie im folgenden Beispiel gezeigt.

```csharp
static void Main(string[] args)
{
    // Set this immediately so that it is used by all requests.
    ServicePointManager.DefaultConnectionLimit = Int32.MaxValue;

    var host = new JobHost();
    host.RunAndBlock();
}
```

## <a name="triggers"></a>Trigger

Funktionen müssen öffentliche Methoden sein und ein Triggerattribut oder das [NoAutomaticTrigger](#manual-trigger)-Attribut aufweisen.

### <a name="automatic-trigger"></a>Automatischer Trigger

Automatische Trigger rufen eine Funktion als Reaktion auf ein Ereignis auf. Ein Beispiel für einen Trigger ist der Warteschlangentrigger im [Artikel „Erste Schritte“](webjobs-sdk-get-started.md).

### <a name="manual-trigger"></a>Manueller Trigger

Wenn Sie eine Funktion manuell auslösen möchten, verwenden Sie das `NoAutomaticTrigger`-Attribut, wie im folgenden Beispiel gezeigt:

```cs
static void Main(string[] args)
{
    JobHost host = new JobHost();
    host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
}
```

```cs
[NoAutomaticTrigger]
public static void CreateQueueMessage(
    TextWriter logger,
    string value,
    [Queue("outputqueue")] out string message)
{
    message = value;
    logger.WriteLine("Creating queue message: ", message);
}
```

## <a name="input-and-output-bindings"></a>Eingabe- und Ausgabebindungen

Eingabebindungen bieten eine deklarative Möglichkeit, Daten aus Azure oder Diensten von Drittanbietern für Ihren Code verfügbar zu machen. Ausgabebindungen bieten eine Möglichkeit, Daten zu aktualisieren. Im [Artikel „Erste Schritte“](webjobs-sdk-get-started.md) finden Sie ein Beispiel für die einzelnen Bindungen.

Sie können einen Rückgabewert einer Methode für eine Ausgabebindung nutzen, indem Sie das Attribut auf den Rückgabewert einer Methode anwenden. Ein Beispiel dafür finden Sie im Artikel über [Trigger und Bindungen](../azure-functions/functions-triggers-bindings.md#using-the-function-return-value) in Azure Functions.

## <a name="binding-types"></a>Bindungstypen

Die folgenden Trigger- und Bindungstypen sind im `Microsoft.Azure.WebJobs`-Paket enthalten:

* Blob Storage
* Queue Storage
* Table Storage

Wenn Sie andere Trigger- und Bindungstypen verwenden möchten, installieren Sie das NuGet-Paket, das diese Typen enthält, und rufen Sie eine `Use<binding>`-Methode für das `JobHostConfiguration`-Objekt auf. Wenn Sie beispielsweise einen Zeitgebertrigger verwenden möchten, installieren Sie `Microsoft.Azure.WebJobs.Extensions`, und rufen Sie `UseTimers` in der `Main`-Methode auf, wie im folgenden Beispiel gezeigt:

```cs
static void Main()
{
    config = new JobHostConfiguration();
    config.UseTimers();
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

Sie finden das für einen bestimmten Bindungstyp zu installierende Paket im Abschnitt **Pakete** des jeweiligen [Referenzartikels](#binding-reference-information) für diesen Bindungstyp für Azure Functions. Eine Ausnahme ist der Trigger- und Bindungstyp „Dateien“ (für das lokale Dateisystem), der von Azure Functions nicht unterstützt wird. Wenn Sie die Bindung „Dateien“ verwenden möchten, installieren Sie `Microsoft.Azure.WebJobs.Extensions`, und rufen Sie `UseFiles` auf.

### <a name="usecore"></a>UseCore

In dem zuvor erwähnten `Microsoft.Azure.WebJobs.Extensions`-Paket wird auch ein spezieller Bindungstyp bereitgestellt, den Sie durch Aufrufen der `UseCore`-Methode registrieren können. Mit dieser Bindung können Sie einen [ExecutionContext](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Core/ExecutionContext.cs)-Parameter in Ihrer Funktionssignatur definieren. Das Kontextobjekt bietet Ihnen Zugriff auf die Aufruf-ID, mit der Sie alle von einem bestimmten Funktionsaufruf erzeugten Protokolle korrelieren können. Hier sehen Sie ein Beispiel:

```cs
class Program
{
    static void Main()
    {
        config = new JobHostConfiguration();
        config.UseCore();
        var host = new JobHost(config);
        host.RunAndBlock();
    }
}
public class Functions
{
    public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        ExecutionContext executionContext,
        ILogger logger)
    {
        logger.LogInformation($"{message}\n{executionContext.InvocationId}");
    }
}
```

## <a name="binding-configuration"></a>Bindungskonfiguration

Viele Trigger- und Bindungstypen ermöglichen Ihnen das Konfigurieren des Verhaltens durch Festlegen von Eigenschaften in einem Konfigurationsobjekt, das Sie an den `JobHost` übergeben.

### <a name="queue-trigger-configuration"></a>Konfiguration des Warteschlangentriggers

Die Einstellungen, die Sie für den Speicherwarteschlangentrigger konfigurieren können, werden in der [„host.json“-Referenz](../azure-functions/functions-host-json.md#queues) für Azure Functions erläutert. Dem folgenden Beispiel können Sie entnehmen, wie diese in einem WebJobs SDK-Projekt festgelegt werden:

```cs
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.Queues.BatchSize = 8;
    config.Queues.NewBatchThreshold = 4;
    config.Queues.MaxDequeueCount = 4;
    config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="configuration-for-other-bindings"></a>Konfiguration für andere Bindungen

Einige Trigger- und Bindungstypen definieren ihren eigenen benutzerdefinierten Konfigurationstyp. Mit dem Dateitrigger können Sie beispielsweise den zu überwachenden Stammpfad angeben:

```cs
static void Main()
{
    config = new JobHostConfiguration();
    var filesConfig = new FilesConfiguration
    {
        RootPath = @"c:\data\import"
    };
    config.UseFiles(filesConfig);
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

## <a name="binding-expressions"></a>Bindungsausdrücke

In Attributkonstruktorparametern können Sie Ausdrücke verwenden, die in Werte aus verschiedenen Quellen aufgelöst werden. Im folgenden Code erstellt beispielsweise der Pfad für das `BlobTrigger`-Attribut einen Ausdruck mit dem Namen `filename`. Bei Verwendung für die Ausgabebindung wird `filename` in den Namen des auslösenden Blobs aufgelöst.
 
```cs
public static void CreateThumbnail(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    ILogger logger)
{
    logger.Info($"Blob trigger processing: {filename}");
    // ...
}
```

Weitere Informationen zu Bindungsausdrücken finden Sie unter [Bindungsausdrücke und Muster](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns) in der Azure Functions-Dokumentation.

### <a name="custom-binding-expressions"></a>Benutzerdefinierte Bindungsausdrücke

Mitunter möchten Sie einen Warteschlangennamen, einen Blobnamen oder Container oder einen Tabellennamen im Code angeben, anstatt ihn hart zu codieren. So möchten Sie z. B. den Warteschlangennamen für das `QueueTrigger`-Attribut in einer Konfigurationsdatei oder Umgebungsvariablen angeben.

Sie erreichen dies, indem Sie ein `NameResolver`-Objekt an das `JobHostConfiguration`-Objekt übergeben. Sie schließen Platzhalter in Konstruktorparameter von Trigger- oder Bindungsattributen ein, und Ihr `NameResolver`-Code gibt die tatsächlichen Werte an, die anstelle dieser Platzhalter verwendet werden sollen. Die Platzhalter werden durch die Umschließung mit Prozentzeichen (%) gekennzeichnet, wie im folgenden Beispiel gezeigt:
 
```cs
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Mit diesem Code können Sie eine Warteschlange mit dem Namen „logqueuetest“ in der Testumgebung und eine Warteschlange mit dem Namen „logqueueprod“ in der Produktionsumgebung verwenden. Anstelle eines hartcodierten Warteschlangennamens geben Sie den Namen eines Eintrags in der `appSettings`-Auflistung an. 

Wenn Sie keine benutzerdefinierte Namensauflösung angeben, wird ein Standard-NameResolver wirksam. Der Standardwert ruft Werte aus App-Einstellungen oder Umgebungsvariablen ab.

Ihre `NameResolver`-Klasse ruft den Warteschlangennamen aus `appSettings` ab, wie im folgenden Beispiel gezeigt:

```cs
public class CustomNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

Übergeben Sie Ihre `NameResolver`-Klasse an das `JobHost`-Objekt, wie im folgenden Beispiel gezeigt:

```cs
 static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.NameResolver = new CustomNameResolver();
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Azure Functions implementiert `INameResolver`, um Werte aus App-Einstellungen abzurufen, wie im Beispiel gezeigt. Wenn Sie das WebJobs SDK direkt verwenden, können Sie eine benutzerdefinierte Implementierung schreiben, die Ersatzwerte für Platzhalter aus einer beliebigen von Ihnen gewünschten Quelle abruft. 

## <a name="binding-at-runtime"></a>Binden zur Laufzeit

Wenn Sie vor der Verwendung eines Bindungsattributs (wie z. B. `Queue`, `Blob` oder `Table`) weitere Aufgaben in Ihrer Funktion ausführen müssen, können Sie die `IBinder`-Schnittstelle verwenden.

Im folgenden Beispiel wird aus einer Nachricht der Eingabewarteschlange eine neue Nachricht mit dem gleichen Inhalt in einer Ausgabewarteschlange erstellt. Der Name der Ausgabewarteschlangenname wird durch Code im Text der Funktion festgelegt.

```cs
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    IBinder binder)
{
    string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
    QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
    CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
    outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
}
```

Weitere Informationen finden Sie unter [Binden zur Laufzeit](../azure-functions/functions-dotnet-class-library.md#binding-at-runtime) in der Azure Functions-Dokumentation.

## <a name="binding-reference-information"></a>Referenzinformationen für Bindungen

In der Azure Functions-Dokumentation werden Referenzinformationen zu den einzelnen Bindungstypen bereitgestellt. Für die Speicherwarteschlange finden Sie beispielsweise in jedem Bindungsreferenzartikel die folgenden Informationen:

* [Pakete](../azure-functions/functions-bindings-storage-queue.md#packages---functions-1x): Hier erfahren Sie, welches Paket installiert werden muss, um Unterstützung für die Bindung in ein WebJobs SDK-Projekt zu integrieren.
* [Beispiele](../azure-functions/functions-bindings-storage-queue.md#trigger---example): Das Beispiel für die C#-Klassenbibliothek bezieht sich auf das WebJobs SDK. Lassen Sie nur das `FunctionName`-Attribut weg.
* [Attribute](../azure-functions/functions-bindings-storage-queue.md#trigger---attributes): Die für den Bindungstyp zu verwendenden Attribute.
* [Konfiguration](../azure-functions/functions-bindings-storage-queue.md#trigger---configuration): Erläuterungen der Attributeigenschaften und Konstruktorparameter.
* [Verwendung](../azure-functions/functions-bindings-storage-queue.md#trigger---usage): Hier erfahren Sie, welche Typen Sie binden können, und erhalten Informationen zur Funktionsweise der Bindung. Beispiele: Abrufalgorithmus, Verarbeitung der Warteschlange für nicht verarbeitete Nachrichten.
  
Eine Liste der Bindungsreferenzartikel finden Sie unter **Unterstützte Bindungen** im Artikel [Trigger und Bindungen](../azure-functions/functions-triggers-bindings.md#supported-bindings) für Azure Functions. Die in dieser Liste aufgeführten Bindungen „HTTP“, „Webhook“ und „Event Grid“ werden nur von Azure Functions und nicht vom WebJobs SDK unterstützt.

## <a name="disable-attribute"></a>Disable-Attribut 

Mit dem [Disable](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs)-Attribut können Sie steuern, ob eine Funktion ausgelöst werden kann. 

Wenn die App-Einstellung `Disable_TestJob` im folgenden Beispiel den Wert „1“ oder „True“ (Groß-/Kleinschreibung nicht beachten) aufweist, wird die Funktion nicht ausgeführt. In diesem Fall erstellt die Runtime die Protokollmeldung *Funktion „Functions.TestJob“ ist deaktiviert*.

```cs
[Disable("Disable_TestJob")]
public static void TestJob([QueueTrigger("testqueue2")] string message)
{
    Console.WriteLine("Function with Disable attribute executed!");
}
```

Wenn Sie im Azure-Portal die Werte einer App-Einstellung ändern, wird der WebJob neu gestartet, wobei die neue Einstellung übernommen wird.

Das Attribut kann auf Parameter-, Methoden- oder Klassenebene deklariert werden. Der Einstellungsname kann auch Bindungsausdrücke enthalten.

## <a name="timeout-attribute"></a>Timeout-Attribut

Das [Timeout](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TimeoutAttribute.cs)-Attribut bewirkt, dass eine Funktion abgebrochen wird, wenn sie nicht innerhalb einer angegebenen Zeit vollständig ausgeführt wurde. Im folgenden Beispiel wird die Funktion ohne Zeitlimit einen Tag ausgeführt. Mit Zeitlimit wird die Funktion nach 15 Sekunden abgebrochen.

```cs
[Timeout("00:00:15")]
public static async Task TimeoutJob(
    [QueueTrigger("testqueue2")] string message,
    CancellationToken token,
    TextWriter log)
{
    await log.WriteLineAsync("Job starting");
    await Task.Delay(TimeSpan.FromDays(1), token);
    await log.WriteLineAsync("Job completed");
}
```

Sie können das Timeout-Attribut auf Klassen- oder Methodenebene anwenden, und mit `JobHostConfiguration.FunctionTimeout` können Sie ein globales Zeitlimit angeben. Zeitlimits auf Klassen- oder Methodenebene setzen das globale Zeitlimit außer Kraft.

## <a name="singleton-attribute"></a>Singleton-Attribut

Mit dem [Singleton](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/SingletonAttribute.cs)-Attribut können Sie sicherstellen, dass nur eine Instanz einer Funktion ausgeführt wird, auch wenn mehrere Instanzen der Host-Web-App vorhanden sind. Dies wird durch die Implementierung einer [verteilten Sperre](#viewing-lease-blobs) erreicht.

Im folgenden Beispiel wird immer nur eine einzige Instanz der `ProcessImage`-Funktion ausgeführt:

```cs
[Singleton]
public static async Task ProcessImage([BlobTrigger("images")] Stream image)
{
     // Process the image
}
```

### <a name="singletonmodelistener"></a>SingletonMode.Listener

In einige Trigger ist die Unterstützung der Parallelitätsverwaltung integriert:

* **QueueTrigger**: Legen Sie `JobHostConfiguration.Queues.BatchSize` auf „1“ fest.
* **ServiceBusTrigger**: Legen Sie `ServiceBusConfiguration.MessageOptions.MaxConcurrentCalls` auf „1“ fest.
* **FileTrigger**: Legen Sie `FileProcessor.MaxDegreeOfParallelism` auf „1“ fest.

Sie können diese Einstellungen verwenden, um sicherzustellen, dass Ihre Funktion als Singleton auf einer einzigen Instanz ausgeführt wird. Wenn Sie sicherstellen möchten, dass nur eine Instanz der Funktion ausgeführt wird, wenn die Web-App auf mehrere Instanzen skaliert wird, wenden Sie eine Singleton-Sperre auf Listener-Ebene für die Funktion an (`[Singleton(Mode = SingletonMode.Listener)]`). Listener-Sperren werden beim Starten des JobHosts abgerufen. Wenn drei horizontal skalierte Instanzen zur selben Zeit gestartet werden, erhält nur eine der Instanzen die Sperre, und es wird nur ein Listener gestartet.

### <a name="scope-values"></a>Bereichswerte

Sie können einen **Bereichsausdruck/-wert** für das Singleton angeben, der sicherstellt, dass alle Ausführungen der Funktion in diesem Bereich serialisiert werden. Die Implementierung einer detaillierteren Sperre auf diese Weise kann eine gewisse Parallelität für Ihre Funktion ermöglichen, während andere Aufrufe entsprechend Ihren Anforderungen serialisiert werden. Im folgenden Beispiel wird beispielsweise der Bereichsausdruck an den Wert `Region` der eingehenden Nachricht gebunden. Wenn die Warteschlange 3 Nachrichten in den Regionen „East“, „East“ und „West“ enthält, werden die Nachrichten, die die Region „East“ enthalten, seriell ausgeführt, während die Nachricht mit der Region „West“ parallel dazu ausgeführt wird.

```csharp
[Singleton("{Region}")]
public static async Task ProcessWorkItem([QueueTrigger("workitems")] WorkItem workItem)
{
     // Process the work item
}

public class WorkItem
{
     public int ID { get; set; }
     public string Region { get; set; }
     public int Category { get; set; }
     public string Description { get; set; }
}
```

### <a name="singletonscopehost"></a>SingletonScope.Host

Der Standardbereich für eine Sperre ist `SingletonScope.Function`. Dies bedeutet, dass der Sperrenbereich (der Blob-Leasepfad) an den vollqualifizierten Funktionsnamen gebunden ist. Um funktionenübergreifende Sperren festzulegen, geben Sie `SingletonScope.Host` an, und verwenden Sie eine Bereichs-ID, die für alle Funktionen, die nicht gleichzeitig ausgeführt werden sollen, gleich ist. Im folgenden Beispiel wird jeweils nur eine Instanz von `AddItem` oder `RemoveItem` ausgeführt:

```charp
[Singleton("ItemsLock", SingletonScope.Host)]
public static void AddItem([QueueTrigger("add-item")] string message)
{
     // Perform the add operation
}

[Singleton("ItemsLock", SingletonScope.Host)]
public static void RemoveItem([QueueTrigger("remove-item")] string message)
{
     // Perform the remove operation
}
```

### <a name="viewing-lease-blobs"></a>Anzeigen von Lease-Blobs

Das WebJobs SDK verwendet [Azure-Blob-Leases](../storage/common/storage-concurrency.md#pessimistic-concurrency-for-blobs) im Hintergrund, um eine verteilte Sperre zu implementieren. Die von Singleton verwendeten Lease-Blobs finden Sie im `azure-webjobs-host`-Container im `AzureWebJobsStorage`-Speicherkonto unter dem Pfad „locks“ (Sperren). Beispielsweise könnte der Lease-Blob-Pfad für das erste zuvor dargestellte `ProcessImage`-Beispiel `locks/061851c758f04938a4426aa9ab3869c0/WebJobs.Functions.ProcessImage` lauten. Alle Pfade enthalten die JobHost-ID, in diesem Fall „061851c758f04938a4426aa9ab3869c0“.

## <a name="async-functions"></a>Asynchrone Funktionen

Informationen zum Codieren asynchroner Funktionen finden Sie in der Azure Functions-Dokumentation unter [Asynchrone Funktionen](../azure-functions/functions-dotnet-class-library.md#async).

## <a name="cancellation-tokens"></a>Abbruchtoken

Informationen zum Umgang mit Abbruchtoken finden Sie in der Azure Functions-Dokumentation unter [Abbruchtoken und ordnungsgemäßes Herunterfahren](../azure-functions/functions-dotnet-class-library.md#cancellation-tokens).

## <a name="multiple-instances"></a>Mehrere Instanzen

Bei der Ausführung Ihrer Web-App auf mehreren Instanzen wird ein kontinuierlicher WebJob auf allen Instanzen ausgeführt, der Trigger überwacht und Funktionen aufruft. Die verschiedenen Triggerbindungen sind so konzipiert, dass die Arbeit effizient über mehrere Instanzen hinweg geteilt wird, sodass Sie bei einer Skalierung auf mehrere Instanzen mehr Last bewältigen können.

Mit dem Warteschlangen- und Blob-Trigger wird automatisch verhindert, dass eine Funktion eine Warteschlangennachricht oder einen Blob mehrmals verarbeitet. Funktionen müssen nicht idempotenten sein.

Mit dem Zeitgebertrigger wird automatisch sichergestellt, dass jeweils nur eine Instanz des Zeitgebers ausgeführt wird, damit zu einem geplanten Zeitpunkt nicht mehrere Funktionsinstanzen ausgeführt werden.

Wenn Sie sicherstellen möchten, dass nur eine Instanz einer Funktion ausgeführt wird, auch wenn mehrere Instanzen der Host-Web-App vorhanden sind, können Sie das [Singleton](#singleton)-Attribut verwenden.
    
## <a name="filters"></a>Filter 

Funktionsfilter (Vorschauversion) bieten eine Möglichkeit zum Anpassen der WebJobs-Ausführungspipeline mit Ihrer eigenen Logik. Die Filter sind mit [ASP.NET Core-Filtern](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters) vergleichbar. Sie können als deklarative Attribute implementiert werden, die auf Ihre Funktionen oder Klassen angewendet werden. Weitere Informationen finden Sie unter [Funktionsfilter](https://github.com/Azure/azure-webjobs-sdk/wiki/Function-Filters).

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

Wir empfehlen das für ASP.NET entwickelte Protokollierungsframework, und im Artikel [Erste Schritte](webjobs-sdk-get-started.md) wird dessen Verwendung erörtert. 

### <a name="log-filtering"></a>Protokollfilterung

Jedem von einer `ILogger`-Instanz erstellten Protokoll ist eine `Category` und eine `Level` zugeordnet. [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel#Microsoft_Extensions_Logging_LogLevel) ist eine Enumeration, und der Code (eine Zahl) weist auf die relative Bedeutung hin:

|LogLevel    |Code|
|------------|---|
|Trace       | 0 |
|Debuggen       | 1 |
|Information | 2 |
|Warnung     | 3 |
|Error       | 4 |
|Kritisch    | 5 |
|Keine        | 6 |

Alle Kategorien können unabhängig voneinander nach einer bestimmten Protokollebene ([LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)) gefiltert werden. So könnten Sie beispielsweise alle Protokolle für die Verarbeitung des Blob-Triggers, aber nur `Error` und höhere Ebenen für alles andere anzeigen.

Um das Angeben von Filterregeln zu vereinfachen, bietet das WebJobs SDK den `LogCategoryFilter`, der an viele vorhandene Protokollierungsanbieter einschließlich Application Insights und Konsolenanwendungen übergeben werden kann.

Der `LogCategoryFilter` verfügt über eine `Default`-Eigenschaft mit dem Anfangswert `Information`. Dies bedeutet, dass alle Nachrichten der Ebene `Information`, `Warning`, `Error`, oder `Critical` protokolliert, aber Nachrichten der Ebene `Debug` oder `Trace` weggefiltert werden.

Die `CategoryLevels`-Eigenschaft ermöglicht Ihnen das Angeben von Protokollebenen für bestimmte Kategorien, sodass Sie die Protokollierungsausgabe optimieren können. Wird im `CategoryLevels`-Wörterbuch keine Übereinstimmung gefunden, wird der Filter bei der Entscheidung, ob die Nachricht gefiltert werden soll, auf den `Default`-Wert zurückgesetzt.

Im folgenden Beispiel wird ein Filter erstellt, der standardmäßig alle Protokolle auf der `Warning`-Ebene filtert. Die Kategorien `Function` und `Host.Results` werden auf der `Error`-Ebene gefiltert. Der `LogCategoryFilter` vergleicht die aktuelle Kategorie mit allen registrierten `CategoryLevels` und wählt die längste Übereinstimmung aus. Dies bedeutet, dass die für `Host.Triggers` registrierte `Debug`-Ebene entweder `Host.Triggers.Queue` oder `Host.Triggers.Blob` entspricht. Dadurch können Sie umfassendere Kategorien steuern, ohne jede einzelne Kategorie hinzufügen zu müssen.

```csharp
var filter = new LogCategoryFilter();
filter.DefaultLevel = LogLevel.Warning;
filter.CategoryLevels[LogCategories.Function] = LogLevel.Error;
filter.CategoryLevels[LogCategories.Results] = LogLevel.Error;
filter.CategoryLevels["Host.Triggers"] = LogLevel.Debug;

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(instrumentationKey, filter.Filter)
    .AddConsole(filter.Filter);
```

### <a name="custom-telemetry-for-application-insights"></a>Benutzerdefinierte Telemetrie für Application Insights

Intern verwendet der vom Application Insights-Anbieter für das WebJobs SDK erstellte `TelemetryClient` den [ServerTelemetryChannel](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs). Wenn der Application Insights-Endpunkt nicht verfügbar ist oder eingehende Anforderungen gedrosselt werden, [speichert dieser Kanal die Anforderungen im Dateisystem der Web-App und übermittelt sie später erneut](http://apmtips.com/blog/2015/09/03/more-telemetry-channels).

Der `TelemetryClient` wird von einer Klasse erstellt, die `ITelemetryClientFactory` implementiert. Standardmäßig handelt es sich dabei um die [DefaultTelemetryClientFactory](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/DefaultTelemetryClientFactory.cs).

Wenn Sie einen beliebigen Teil der Application Insights-Pipeline ändern möchten, können Sie Ihre eigene `ITelemetryClientFactory` angeben, und der Host verwendet Ihre Klasse, um einen `TelemetryClient` zu erstellen. Dieser Code überschreibt z. B. die `DefaultTelemetryClientFactory`, um eine Eigenschaft des `ServerTelemetryChannel` zu ändern:

```csharp
private class CustomTelemetryClientFactory : DefaultTelemetryClientFactory
{
    public CustomTelemetryClientFactory(string instrumentationKey, Func<string, LogLevel, bool> filter)
        : base(instrumentationKey, new SamplingPercentageEstimatorSettings(), filter)
    {
    }

    protected override ITelemetryChannel CreateTelemetryChannel()
    {
        ServerTelemetryChannel channel = new ServerTelemetryChannel();

        // change the default from 30 seconds to 15 seconds
        channel.MaxTelemetryBufferDelay = TimeSpan.FromSeconds(15);

        return channel;
    }
}
```

Mit dem „SamplingPercentageEstimatorSettings“-Objekt wird die [adaptive Stichprobenerstellung](https://docs.microsoft.com/azure/application-insights/app-insights-sampling#adaptive-sampling-at-your-web-server) konfiguriert. Dies bedeutet, dass Application Insights in bestimmten Szenarien mit hohem Volumen eine ausgewählte Teilmenge von Telemetriedaten an den Server sendet.

Nachdem Sie die Telemetrie-Factory erstellt haben, übergeben Sie diese an den Application Insights-Protokollierungsanbieter:

```csharp
var clientFactory = new CustomTelemetryClientFactory(instrumentationKey, filter.Filter);

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(clientFactory);
```

## <a id="nextsteps"></a> Nächste Schritte

In dieser Anleitung wurden Codeausschnitte bereitgestellt, die veranschaulichen, wie häufige Szenarien für das Arbeiten mit dem WebJobs SDK behandelt werden. Vollständige Beispiele finden Sie unter [azure-webjobs-sdk-samples](https://github.com/Azure/azure-webjobs-sdk-samples).