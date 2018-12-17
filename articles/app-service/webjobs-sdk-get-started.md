---
title: Erste Schritte mit dem WebJobs SDK – Azure
description: Einführung in das WebJobs SDK für die ereignisgesteuerte Hintergrundverarbeitung. Erfahren Sie, wie Sie auf Daten in Azure-Diensten und Diensten von Drittanbietern zugreifen.
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/27/2018
ms.author: glenga
ms.openlocfilehash: 034f182cc282f50eb3a4a1de05331f42957f49fe
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53339806"
---
# <a name="get-started-with-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Erste Schnitte mit dem Azure WebJobs SDK für die ereignisgesteuerte Hintergrundverarbeitung

In diesem Artikel erfahren Sie, wie Sie ein Azure WebJobs SDK-Projekt erstellen, es lokal ausführen und in Azure App Service bereitstellen.

Die Anweisungen gelten für [Visual Studio 2017](https://www.visualstudio.com/vs/), die gleichen Aufgaben können jedoch auch mit anderen Tools wie z. B. [Visual Studio Code](https://code.visualstudio.com/) ausgeführt werden.

## <a name="what-is-the-azure-webjobs-sdk"></a>Was ist das Azure WebJobs-SDK?

Das Azure WebJobs SDK ist ein Framework, das die Aufgabe vereinfacht, Hintergrundverarbeitungscode zu schreiben, der auf Daten in Azure-Diensten zugreift. Das SDK unterstützt eine deklarative Syntax für das Angeben von Ereignissen, die eine Funktion auslösen sollen, z. B. eine neue Nachricht, die einer Warteschlange hinzugefügt wird. Eine ähnliche deklarative Syntax steuert das Lesen und Schreiben von Daten, nachdem eine Funktion ausgelöst wurde. Dieses System von Triggern und Bindungen übernimmt die meisten einfachen Codierungsaufgaben, die mit dem Zugreifen auf Azure und Dienste von Drittanbietern verbunden sind.

### <a name="functions-triggers-and-bindings"></a>Funktionen, Trigger und Bindungen

Ein WebJobs SDK-Projekt definiert eine oder mehrere *Funktionen*. Eine Funktion ist eine Methode, die in der Methodensignatur ein Triggerattribut aufweist. Trigger geben Bedingungen für das Aufrufen einer Funktion an, und Bindungen geben an, was gelesen und geschrieben werden soll. Das Triggerattribut in der folgenden Funktion weist beispielsweise die Runtime an, die Funktion jedes Mal aufzurufen, wenn eine Warteschlangennachricht in der `items`-Warteschlange angezeigt wird. Das `Blob`-Attribut weist die Runtime an, die Warteschlangennachricht zum Lesen eines Blobs im Container *Arbeitselemente* zu verwenden. Der Inhalt der Warteschlangennachricht (im `queueTrigger`-Parameter angegeben) ist der Name des Blobs.

```cs
public static void Run(
    [QueueTrigger("items")] string myQueueItem,
    [Blob("workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

### <a name="versions-2x-and-3x"></a>Versionen 2.x und 3.x

Die Anweisungen beschreiben, wie Sie ein WebJobs SDK Version 2.x-Projekt erstellen. Die neueste Version des WebJobs SDK ist Version 3.x, doch befindet sich diese derzeit in der Vorschauphase, und in diesem Artikel sind noch keine Anweisungen für diese Version enthalten. Die wichtigste in Version 3.x eingeführte Änderung ist die Verwendung von .NET Core anstelle von .NET Framework.

### <a name="azure-functions"></a>Azure-Funktionen

[Azure Functions](../azure-functions/functions-overview.md) basiert auf dem WebJobs SDK und ist eine Option, wenn Sie das WebJobs SDK direkt verwenden müssen. Azure Functions 1.x verwendet das WebJobs SDK 2.x. Weitere Informationen finden Sie unter [Vergleich zwischen Azure Functions und dem WebJobs SDK](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs).

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird vorausgesetzt, dass Sie über [ein Azure-Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) und Erfahrungen im Umgang mit [Apps in Azure App Service](app-service-web-overview.md) verfügen. Zur Durchführung der in diesem Artikel aufgeführten Schritte ist Folgendes erforderlich:

* [Installieren Sie Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/) mit der Workload **Azure-Entwicklung**. Wenn Sie bereits über Visual Studio, jedoch nicht über diese Workload verfügen, müssen Sie die Workload hinzufügen, indem Sie **Tools > Tools und Features abrufen** auswählen.
* [Erstellen Sie eine App Service-App](app-service-web-get-started-dotnet-framework.md). Wenn Sie bereits über eine App verfügen, für die Sie einen WebJob bereitstellen können, haben Sie die Möglichkeit, diese zu verwenden, statt eine neue zu erstellen.

## <a name="create-a-project"></a>Erstellen eines Projekts

1. Wählen Sie in Visual Studio die Optionen **Datei > Neues Projekt** aus.

2. Wählen Sie **Klassischer Windows-Desktop > Konsolen-App (.NET Framework)** aus.

3. Weisen Sie dem Projekt den Namen *WebJobsSDKSample* zu, und wählen Sie dann **OK** aus.

   ![Dialogfeld "Neues Projekt"](./media/webjobs-sdk-get-started/new-project.png)

## <a name="add-webjobs-nuget-package"></a>Hinzufügen des WebJobs NuGet-Pakets

1. Installieren Sie die neueste stabile 2.x-Version des NuGet-Pakets `Microsoft.Azure.WebJobs`.
 
   Der Befehl der **Paket-Manager-Konsole** für Version 2.2.0 lautet wie folgt:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs -version 2.2.0
   ``` 

## <a name="create-the-jobhost"></a>Erstellen des JobHosts

Das `JobHost`-Objekt ist der Laufzeitcontainer für Funktionen: Damit werden Trigger überwacht und Funktionen aufgerufen. 

1. Fügen Sie in *Program.cs* eine `using`-Anweisung hinzu:

   ```cs
   using Microsoft.Azure.WebJobs;
   ```

1. Ersetzen Sie die `Main`-Methode durch den folgenden Code:

   ```cs
   static void Main()
   {
       var config = new JobHostConfiguration();
       var host = new JobHost(config);
       host.RunAndBlock();
   }
   ```

## <a name="enable-console-logging"></a>Aktivieren der Konsolenprotokollierung

Es stehen mehrere Optionen für die Protokollierung im WebJobs SDK-Projekt zur Verfügung. Die von uns empfohlene Option ist das [für ASP.NET Core entwickelte Protokollierungsframework](https://docs.microsoft.com/aspnet/core/fundamentals/logging). Dieses Framework bietet eine bessere Leistung und mehr Flexibilität bei Speichermedien und bei der Filterung. 

In diesem Abschnitt richten Sie die Konsolenprotokollierung ein, die das neue Framework verwendet.

1. Installieren Sie die neueste stabile Version der folgenden NuGet-Pakete:

   * `Microsoft.Extensions.Logging`: Das Protokollierungsframework.
   * `Microsoft.Extensions.Logging.Console`: Der Konsolen*anbieter*. Ein Anbieter sendet Protokolle an ein bestimmtes Ziel, in diesem Fall an die Konsole. 
 
   Die Befehle der **Paket-Manager-Konsole** für Version 2.0.1 lauten wie folgt:

   ```powershell
   Install-Package Microsoft.Extensions.Logging -version 2.0.1
   ``` 

   ```powershell
   Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
   ``` 

1. Fügen Sie in *Program.cs* eine `using`-Anweisung hinzu:

   ```cs
   using Microsoft.Extensions.Logging;
   ```

1. Fügen Sie in der `Main`-Methode Code zum Aktualisieren der `JobHostConfiguration` hinzu, bevor Sie den `JobHost` erstellen:
 
   ```
   config.DashboardConnectionString = "";
   var loggerFactory = new LoggerFactory();
   config.LoggerFactory = loggerFactory
       .AddConsole();
   ```

   Dieser Code nimmt die folgenden Änderungen vor:

   * Deaktiviert die [Dashboardprotokollierung](https://github.com/Azure/azure-webjobs-sdk/wiki/Queues#logs). Das Dashboard ist ein älteres Überwachungstool, und die Dashboardprotokollierung wird in Produktionsszenarien mit hohem Durchsatz nicht empfohlen.
   * Fügt den Konsolenanbieter mit der Standard[filterung](webjobs-sdk-how-to.md#log-filtering) hinzu. 

   Die `Main`-Methode sieht jetzt wie folgt aus:

   ```
   var config = new JobHostConfiguration();
   config.DashboardConnectionString = "";
   var loggerFactory = new LoggerFactory();
   config.LoggerFactory = loggerFactory
       .AddConsole();
   var host = new JobHost(config);
   host.RunAndBlock();
   ```
   
## <a name="create-a-function"></a>Erstellen einer Funktion

1. Erstellen Sie *Functions.cs* im Projektordner, und ersetzen Sie den Vorlagencode durch den folgenden Code:

   ```cs
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Host;
   using Microsoft.Extensions.Logging;

   namespace WebJobsSDKSample
   {
       public class Functions
       {
           public static void ProcessQueueMessage([QueueTrigger("queue")] string message, ILogger logger)
           {
               logger.LogInformation(message);
           }
       }
   }
   ```

   Das `QueueTrigger`-Attribut weist die Runtime an, diese Funktion aufzurufen, wenn eine neue Nachricht in die Azure Storage-Warteschlange namens `queue` geschrieben wird. Der Inhalt der Warteschlangennachricht wird für den Methodencode im `message`-Parameter bereitgestellt. Die Triggerdaten werden im Text der Methode verarbeitet. In diesem Beispiel protokolliert der Code nur die Nachricht.

   Der `message`-Parameter muss keine Zeichenfolge sein. Sie können auch eine Bindung an ein JSON-Objekt, Bytearray oder [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage)-Objekt erstellen. Weitere Informationen finden Sie unter [Verwendung des Warteschlangentriggers](../azure-functions/functions-bindings-storage-queue.md#trigger---usage). Jeder Bindungstyp (z. B. Warteschlangen, Blobs oder Tabellen) weist einen anderen Satz von Parametertypen auf, an die Sie binden können.

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Der lokal ausgeführte Azure Storage-Emulator verfügt nicht über alle Funktionen, die das WebJobs SDK benötigt. Daher erstellen Sie in diesem Abschnitt ein Speicherkonto in Azure und konfigurieren das Projekt für dessen Verwendung.

1. Öffnen Sie **Server-Explorer** in Visual Studio, und melden Sie sich in Azure an. Klicken Sie mit der rechten Maustaste auf den Knoten **Azure**, und wählen Sie dann **Verbindung mit Microsoft Azure-Abonnement herstellen** aus.

   ![Anmelden bei Azure](./media/webjobs-sdk-get-started/sign-in.png)

1. Klicken Sie in **Server-Explorer** unter dem Knoten **Azure** mit der rechten Maustaste auf **Speicher**, und wählen Sie dann **Speicherkonto erstellen** aus.

   ![Menü „Speicherkonto erstellen“](./media/webjobs-sdk-get-started/create-storage-account-menu.png)

1. Geben Sie im Dialogfeld **Speicherkonto erstellen** einen eindeutigen Namen für das Speicherkonto ein.

1. Wählen Sie die gleiche **Region**, in der Sie Ihre App Service-App erstellt haben, oder eine Region in Ihrer Nähe aus.

1. Klicken Sie auf **Erstellen**.

   ![Speicherkonto erstellen](./media/webjobs-sdk-get-started/create-storage-account.png)

1. Wählen Sie in **Server-Explorer** unter dem Knoten **Speicher** das neue Speicherkonto aus. Wählen Sie im Fenster **Eigenschaften** die Auslassungspunkte (**...**) rechts neben dem Wertfeld **Verbindungszeichenfolge** aus.

   ![Verbindungszeichenfolge, Auslassungspunkte](./media/webjobs-sdk-get-started/conn-string-ellipsis.png)

1. Kopieren Sie die Verbindungszeichenfolge, und speichern Sie diesen Wert an einem Speicherort, an dem Sie ihn problemlos wieder kopieren können.

   ![Verbindungszeichenfolge kopieren](./media/webjobs-sdk-get-started/copy-key.png)

## <a name="configure-storage-for-running-locally"></a>Konfigurieren des Speichers für die lokale Ausführung

Das WebJobs SDK sucht in der Sammlung „App-Einstellungen“ nach der Verbindungszeichenfolge für den Speicher. Bei lokaler Ausführung wird in der Datei *App.config* oder in den Umgebungsvariablen nach diesem Wert gesucht.

1. Fügen Sie in der Datei *App.config* den folgenden XML-Code unmittelbar nach dem öffnenden Tag `<configuration>` hinzu.

   ```xml
   <connectionStrings>
     <add name="AzureWebJobsStorage" connectionString="{storage connection string}" />
   </connectionStrings>
   ```

1. Ersetzen Sie *{Verbindungszeichenfolge für den Speicher}* durch die Verbindungszeichenfolge, die Sie zuvor kopiert haben.

   Sie verwenden die Verbindungszeichenfolge später erneut, wenn Sie die App Service-App in Azure konfigurieren.

## <a name="test-locally"></a>Lokales Testen

In diesem Abschnitt wird das Projekt lokal erstellt und ausgeführt, und die Funktion wird durch Erstellen einer Warteschlangennachricht ausgelöst.

1. Drücken Sie STRG+F5, um das Projekt auszuführen.

   In der Konsole wird angezeigt, dass die Runtime Ihre Funktion gefunden hat und darauf wartet, dass Warteschlangennachrichten sie auslösen.

   ```console
   Found the following functions:
   WebJobsSDKSample.Functions.ProcessQueueMessage
   info: Host.Startup[0]
         Found the following functions:
         WebJobsSDKSample.Functions.ProcessQueueMessage
   Job host started
   info: Host.Startup[0]
         Job host started
   ```

   Möglicherweise wird eine Warnmeldung zu einer `ServicePointManager`-Einstellung angezeigt. Für die Tests, die Sie mit diesem Projekt durchführen, können Sie die Warnung ignorieren. Weitere Informationen zur Warnung finden Sie unter [Verwenden des WebJobs SDK](webjobs-sdk-how-to.md#jobhost-servicepointmanager-settings).

1. Schließen Sie das Konsolenfenster.

1. Erweitern Sie in Visual Studio in **Server-Explorer** den Knoten für Ihr neues Speicherkonto, und klicken Sie dann mit der rechten Maustaste auf **Warteschlangen**. 

1. Wählen Sie **Warteschlange erstellen** aus. 

1. Geben Sie *queue* als Namen für die Warteschlange ein, und wählen Sie dann **OK** aus.

   ![Erstellen einer Warteschlange](./media/webjobs-sdk-get-started/create-queue.png)

1. Klicken Sie mit der rechten Maustaste auf den Knoten für die neue Warteschlange, und wählen Sie dann **Warteschlange anzeigen** aus.

1. Wählen Sie das Symbol **Nachricht hinzufügen** aus.

   ![Erstellen einer Warteschlange](./media/webjobs-sdk-get-started/create-queue-message.png)

1. Geben Sie im Dialogfeld **Nachricht hinzufügen** die Zeichenfolge *Hello World!* als **Nachrichtentext** ein, und wählen Sie dann **OK** aus.

   ![Erstellen einer Warteschlange](./media/webjobs-sdk-get-started/hello-world-text.png)

1. Führen Sie das Projekt erneut aus.

   Weil Sie in der `ProcessQueueMessage`-Funktion das `QueueTrigger`-Attribut verwendet haben, überwacht die Runtime des WeJobs SDK Warteschlangennachrichten beim Starten. Sie findet in der Warteschlange mit dem Namen *queue* eine neue Warteschlangennachricht und ruft die Funktion auf.

   Aufgrund des [exponentiellen Backoffs des Warteschlangenabrufs](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm) kann es bis zu zwei Minuten dauern, bis die Runtime die Nachricht findet und die Funktion aufruft. Diese Wartezeit kann durch die Ausführung im [Entwicklungsmodus](webjobs-sdk-how-to.md#jobhost-development-settings) verkürzt werden.

  Die Konsolenausgabe sieht wie folgt aus:

   ```console
   Found the following functions:
   WebJobsSDKSample.Functions.ProcessQueueMessage
   info: Host.Startup[0]
         Found the following functions:
         WebJobsSDKSample.Functions.ProcessQueueMessage
   Job host started
   info: Host.Startup[0]
         Job host started
   Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   info: Function[0]
         Hello World!
   info: Host.Results[0]
         Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   ```

1. Schließen Sie das Konsolenfenster.

## <a name="add-application-insights-logging"></a>Hinzufügen der Application Insights-Protokollierung

Wenn das Projekt in Azure ausgeführt wird, können Sie die Funktionsausführung nicht durch Anzeigen der Konsolenausgabe überwachen. Wir empfehlen daher die Überwachungslösung [Application Insights](../application-insights/app-insights-overview.md). Weitere Informationen finden Sie unter [Überwachen von Azure Functions](../azure-functions/functions-monitoring.md).

In diesem Abschnitt führen Sie die folgenden Aufgaben zum Einrichten der Application Insights-Protokollierung vor dem Bereitstellen für Azure aus:

* Stellen Sie sicher, dass Sie über eine App Service-App und eine Application Insights-Instanz verfügen, mit denen Sie arbeiten können.
* Konfigurieren Sie die App Service-App für die Verwendung der Application Insights-Instanz und des Speicherkontos, das Sie zuvor erstellt haben.
* Richten Sie das Projekt für die Protokollierung in Application Insights ein.

### <a name="create-app-service-app-and-application-insights-instance"></a>Erstellen einer App Service-App und einer Application Insights-Instanz

1. Wenn Sie noch nicht über eine App Service-App verfügen, die Sie verwenden können, [erstellen Sie eine](app-service-web-get-started-dotnet-framework.md).

1. Wenn Sie noch nicht über eine Application Insights-Ressource verfügen, die Sie verwenden können, [erstellen Sie eine](../application-insights/app-insights-create-new-resource.md). Legen Sie den **Anwendungstyp** auf **Allgemein** fest, und überspringen Sie die Abschnitte die dem Abschnitt **Kopieren des Instrumentierungsschlüssels** folgen.

1. Wenn Sie bereits über eine Application Insights-Ressource verfügen, die Sie verwenden möchten, [kopieren Sie den Instrumentierungsschlüssel](../application-insights/app-insights-create-new-resource.md#copy-the-instrumentation-key).

### <a name="configure-app-settings"></a>Konfigurieren von App-Einstellungen 

1. Erweitern Sie in Visual Studio in **Server-Explorer** den Knoten **App Service** unter **Azure**.

1. Erweitern Sie die Ressourcengruppe, in der sich Ihre App Service-App befindet, und klicken Sie dann mit der rechten Maustaste auf Ihre App Service-App.

1. Wählen Sie **Einstellungen anzeigen** aus.

1. Fügen Sie im Feld **Verbindungszeichenfolgen** den folgenden Eintrag hinzu.

   |NAME  |Verbindungszeichenfolge  |Datenbanktyp|
   |---------|---------|------|
   |AzureWebJobsStorage | {die zuvor kopierte Verbindungszeichenfolge für den Speicher}|Benutzerdefiniert|
   
1. Wenn das Feld **Anwendungseinstellungen** keinen Application Insights-Instrumentierungsschlüssel enthält, fügen Sie den zuvor kopierten hinzu. (Der Instrumentierungsschlüssel ist möglicherweise bereits vorhanden, je nachdem, wie Sie die App Service-App erstellt haben.)

   |NAME  |Wert  |
   |---------|---------|
   |APPINSIGHTS_INSTRUMENTATIONKEY | {Instrumentierungsschlüssel} |

1. Ersetzen Sie *{Instrumentierungsschlüssel}* durch den Instrumentierungsschlüssel aus der Application Insights-Ressource, die Sie verwenden.

1. Wählen Sie **Speichern**aus.

1. Fügen Sie in der Datei *App.config* den folgenden XML-Code unmittelbar nach der Auflistung der Verbindungszeichenfolgen hinzu.

   ```xml
   <appSettings>
     <add key="APPINSIGHTS_INSTRUMENTATIONKEY" value="{instrumentation key}" />
   </appSettings>
   ```

1. Ersetzen Sie *{Instrumentierungsschlüssel}* durch den Instrumentierungsschlüssel aus der Application Insights-Ressource, die Sie verwenden.

   Durch Hinzufügen dieser Daten zur Datei *App.config* können Sie die Application Insights-Verbindung testen, wenn Sie das Projekt lokal ausführen. 

1. Speichern Sie die Änderungen.

### <a name="add-application-insights-logging-provider"></a>Hinzufügen des Application Insights-Protokollierungsanbieters

1. Installieren Sie die neueste stabile 2.x-Version des NuGet-Pakets für den Application Insights-Protokollierungsanbieter: `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`.

   Der Befehl der **Paket-Manager-Konsole** für Version 2.2.0 lautet wie folgt:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
   ``` 

1. Installieren Sie die neueste stabile 4.x-Version des NuGet-Pakets für den .NET Configuration Manager: `System.Configuration.ConfigurationManager`.

   Der Befehl der **Paket-Manager-Konsole** für Version 4.4.1 lautet wie folgt:

   ```powershell
   Install-Package System.Configuration.ConfigurationManager -version 4.4.1
   ``` 

1. Öffnen Sie *Program.cs*, und fügen Sie eine `using`-Anweisung für den Configuration Manager hinzu:

   ```csharp
   using System.Configuration;
   ```

1. Ersetzen Sie den Code in der `Main`-Methode durch den folgenden Code:

   ```csharp
   using (var loggerFactory = new LoggerFactory())
   {
       var config = new JobHostConfiguration();
       var instrumentationKey =
           ConfigurationManager.AppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"];
       config.DashboardConnectionString = "";
       config.LoggerFactory = loggerFactory
           .AddApplicationInsights(instrumentationKey, null)
           .AddConsole();
       var host = new JobHost(config);
       host.RunAndBlock();
   }
   ```

   Dieser Code nimmt die folgenden Änderungen vor:

   * Fügt einen Application Insights-Protokollierungsanbieter mit Standard[filterung](webjobs-sdk-how-to.md#log-filtering) hinzu. Bei lokaler Ausführung werden jetzt alle Informationen und Protokolle einer höheren Ebene an die Konsole und an Application Insights weitergeleitet. 
   * Fügt das `LoggerFactory`-Objekt in einen `using`-Block ein, um sicherzustellen, dass die Protokollausgabe beim Beenden des Hosts geleert wird. 

## <a name="test-application-insights-logging"></a>Testen der Application Insights-Protokollierung

In diesem Abschnitt führen Sie das Projekt erneut lokal aus, um zu überprüfen, ob die Protokollierungsdaten jetzt sowohl an Application Insights als auch an die Konsole weitergeleitet werden.

1. Verwenden Sie in Visual Studio **Server-Explorer**, um eine Warteschlangennachricht auf die gleiche Weise wie [zuvor](#trigger-the-function-in-azure) zu erstellen, mit der Ausnahme, dass Sie hier *Hello App Insights!* als Nachrichtentext eingeben.

1. Führen Sie das Projekt aus.

   Das WebJobs SDK verarbeitet die Warteschlangennachricht, und die Protokolle werden im Konsolenfenster angezeigt.

1. Schließen Sie das Konsolenfenster.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und navigieren Sie zu Ihrer Application Insights-Ressource.

1. Wählen Sie **Suchen** aus.

   ![„Suchen“ auswählen](./media/webjobs-sdk-get-started/select-search.png)

1. Wenn die Nachricht *Hello App Insights!* nicht angezeigt wird, wählen Sie **Aktualisieren** in regelmäßigen Abständen mehrere Minuten lang aus. (Die Protokolle werden nicht sofort angezeigt, weil es eine Weile dauert, bis der Application Insights-Client die verarbeiteten Protokolle leert.)

   ![Protokolle in Application Insights](./media/webjobs-sdk-get-started/logs-in-ai.png)

1. Schließen Sie das Konsolenfenster.

## <a name="deploy-as-a-webjob"></a>Bereitstellen als WebJob

In diesem Abschnitt stellen Sie das Projekt als WebJob bereit. Sie stellen es für eine App Service-App bereit, die Sie [zuvor erstellt](#create-app-service-app-and-application-insights-instance) haben. Um Ihren Code während der Ausführung in Azure zu testen, lösen Sie einen Funktionsaufruf aus, indem Sie eine Warteschlangennachricht erstellen.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Als Azure-WebJob veröffentlichen** aus.

1. Wählen Sie im Dialogfeld **Azure-WebJob hinzufügen** die Option **OK** aus.

   ![Azure-WebJob hinzufügen](./media/webjobs-sdk-get-started/add-azure-webjob.png)

   Visual Studio installiert automatisch ein NuGet-Paket für die WebJob-Veröffentlichung.

1. Wählen Sie im Schritt **Profil** des Veröffentlichungs-Assistenten (Dialogfeld **Veröffentlichen**) das Ziel **Microsoft Azure App Service** aus.

   ![Dialogfeld „Veröffentlichen“](./media/webjobs-sdk-get-started/publish-dialog.png)

1. Wählen Sie im Dialogfeld **App Service** **Ihre Ressourcengruppe > Ihre App Service-App** aus, und wählen Sie dann **OK** aus.

   ![Dialogfeld „App Service“](./media/webjobs-sdk-get-started/app-service-dialog.png)

1. Wählen Sie im Schritt **Verbindung** des Assistenten die Option **Veröffentlichen** aus.

## <a name="trigger-the-function-in-azure"></a>Auslösen der Funktion in Azure

1. Stellen Sie sicher, dass das Projekt nicht lokal ausgeführt wird. (schließen Sie das Konsolenfenster, falls es noch geöffnet ist). Andernfalls verarbeitet möglicherweise die lokale Instanz als erste die von Ihnen erstellten Warteschlangennachrichten.


1. Aktualisieren Sie in Visual Studio die Seite **Warteschlange**. Die neue Nachricht wird dann nicht mehr angezeigt, weil die in Azure App Service ausgeführte Funktion sie verarbeitet hat.

   > [!TIP]
   > Verwenden Sie beim Testen in Azure den [Entwicklungsmodus](webjobs-sdk-how-to.md#jobhost-development-settings), um sicherzustellen, dass eine Warteschlangen-Triggerfunktion sofort aufgerufen wird, und vermeiden Sie Verzögerungen aufgrund des [exponentiellen Backoffs des Warteschlangenabrufs](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm).

### <a name="view-logs-in-application-insights"></a>Anzeigen von Protokollen in Application Insights

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und navigieren Sie zu Ihrer Application Insights-Ressource.

1. Wählen Sie **Suchen** aus.

1. Wenn die Nachricht *Hello Azure!* nicht angezeigt wird, wählen Sie **Aktualisieren** in regelmäßigen Abständen mehrere Minuten lang aus.

   Es werden die Protokolle der Funktion angezeigt, die in einem WebJob ausgeführt wird, einschließlich des Texts *Hello Azure!*, den Sie im vorherigen Abschnitt eingegeben haben.

## <a name="add-an-input-binding"></a>Hinzufügen einer Eingabebindung

Eingabebindungen vereinfachen Code, der Daten liest. In diesem Beispiel ist die Warteschlangennachricht ein Blobname, und Sie verwenden den Blobnamen zum Suchen und Lesen eines Blobs in Azure Storage.

1. Ersetzen Sie in *Functions.cs* die `ProcessQueueMessage`-Methode durch den folgenden Code:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
   }
   ```

   In diesem Code ist `queueTrigger` ein [Bindungsausdruck](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns), was bedeutet, dass er zur Laufzeit in einen anderen Wert aufgelöst wird.  Zur Laufzeit weist er den Inhalt der Warteschlangennachricht auf.

1. Fügen Sie ein `using`-Element hinzu:

   ```cs
   using System.IO;
   ```

1. Erstellen Sie einen Blobcontainer in Ihrem Speicherkonto.

   a. Erweitern Sie in Visual Studio in **Server-Explorer** den Knoten für Ihr Speicherkonto, klicken Sie mit der rechten Maustaste auf **Blobs**, und wählen Sie dann **BLOB-Container erstellen** aus.

   b. Geben Sie im Dialogfeld **BLOB-Container erstellen** den Text *container* als Containernamen ein, und klicken Sie dann auf **OK**.

1. Laden Sie die Datei *Program.cs* in den Blobcontainer hoch. (Diese Datei wird hier als Beispiel verwendet. Sie können eine beliebige Textdatei hochladen und eine Warteschlangennachricht mit dem Namen der Datei erstellen.)

   a. Doppelklicken Sie in **Server-Explorer** auf den Knoten für den Container, den Sie gerade erstellt haben.

   b. Wählen Sie im Fenster **Container** die Schaltfläche **Hochladen** aus.

   ![Schaltfläche „Blob hochladen“](./media/webjobs-sdk-get-started/blob-upload-button.png)

   c. Suchen Sie die Datei *Program.cs*, wählen Sie die Datei aus, und wählen Sie dann **OK** aus.

1. Erstellen Sie in der zuvor von Ihnen erstellten Warteschlange eine Warteschlangennachricht, und verwenden Sie *Program.cs* als Text der Nachricht.

   ![Warteschlangennachricht „Program.cs“](./media/webjobs-sdk-get-started/queue-msg-program-cs.png)

1. Führen Sie das Projekt aus.

   Die Warteschlangennachricht löst die Funktion aus, die dann das Blob liest und dessen Länge protokolliert. Die Konsolenausgabe sieht wie folgt aus:

   ```console
   Found the following functions:
   ConsoleApp1.Functions.ProcessQueueMessage
   Job host started
   Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=5a2ac479-de13-4f41-aae9-1361f291ff88)
   Blob name:Program.cs
   Size: 532 bytes
   Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=5a2ac479-de13-4f41-aae9-1361f291ff88)
   ```

## <a name="add-an-output-binding"></a>Hinzufügen einer Ausgabebindung

Ausgabebindungen vereinfachen Code, der Daten schreibt. In diesem Beispiel wird der vorherige Code geändert, indem eine Kopie des Blobs geschrieben wird, statt dessen Größe zu protokollieren.

1. Ersetzen Sie die `ProcessQueueMessage`-Methode durch den folgenden Code:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       [Blob("container/copy-{queueTrigger}", FileAccess.Write)] Stream outputBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
       myBlob.CopyTo(outputBlob);
   }
   ```

1. Erstellen Sie eine weitere Warteschlangennachricht mit dem Nachrichtentext *Program.cs*.

1. Führen Sie das Projekt aus.

   Die Warteschlangennachricht löst die Funktion aus, die dann das Blob liest, dessen Länge protokolliert und ein neues Blob erstellt. Die Konsolenausgabe ist identisch, aber wenn Sie zum Fenster des Blobcontainers wechseln und **Aktualisieren** auswählen, wird ein neues Blob mit dem Namen *copy-Program.cs* angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Leitfaden wurde aufgezeigt, wie Sie ein WebJobs SDK-Projekt erstellen, ausführen und bereitstellen.

Um alle Elemente aufzuzeigen, die in ein WebJobs SDK-Projekt integriert werden, mussten Sie gemäß den Anweisungen ein Projekt von Grund auf neu erstellen. Wenn Sie Ihr nächstes Projekt erstellen, sollten Sie jedoch die Verwendung der Vorlage **Azure-WebJob** in der Kategorie **Cloud** in Erwägung ziehen. Diese Vorlage erstellt ein Projekt, in dem NuGet-Pakete und der Beispielcode bereits eingerichtet sind. Beachten Sie, dass der Beispielcode möglicherweise geändert werden muss, damit das neue Protokollierungsframework verwendet wird.

Weitere Informationen finden Sie unter [Verwenden des WebJobs SDK](webjobs-sdk-how-to.md).
