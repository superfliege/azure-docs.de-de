---
title: Ausführen von Azure Functions in Azure Stream Analytics-Aufträgen
description: Dieser Artikel beschreibt die Konfiguration von Azure Functions als Ausgabesenke in Stream Analytics-Aufträgen, sodass Workloads ereignisgesteuert sind.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/19/2017
ms.openlocfilehash: a8eebfa0c40caa455eb20431e5cf4acb8eeb248c
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="run-azure-functions-from-azure-stream-analytics-jobs"></a>Ausführen von Azure Functions in Azure Stream Analytics-Aufträgen 

Sie können Azure Functions in Azure Stream Analytics ausführen, indem Sie Functions als eine der Ausgabesenken für den Stream Analytics-Auftrag konfigurieren. Functions ist eine ereignisgesteuerte On-Demand-Computeumgebung, mit der Sie Code implementieren können, der durch in Azure- oder Drittanbieterdiensten auftretende Ereignisse ausgelöst wird. Aufgrund der Möglichkeit, auf Trigger zu antworten, ist Functions die ideale Ausgabe für Stream Analytics-Aufträge.

Stream Analytics ruft Functions über HTTP-Trigger auf. Der Functions-Ausgabeadapter ermöglicht Benutzern das Verbinden von Functions mit Stream Analytics, sodass die Ereignisse basierend auf Stream Analytics-Abfragen ausgelöst werden können. 

In diesem Tutorial wird veranschaulicht, wie Stream Analytics mithilfe von [Azure Functions](../azure-functions/functions-overview.md) mit [Azure Redis Cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md) verbunden wird. 

## <a name="configure-a-stream-analytics-job-to-run-a-function"></a>Konfigurieren eines Stream Analytics-Auftrags zum Ausführen einer Funktion 

Dieser Abschnitt veranschaulicht das Konfigurieren eines Stream Analytics-Auftrags zum Ausführen einer Funktion, die Daten in Azure Redis Cache schreibt. Der Stream Analytics-Auftrag liest Ereignisse aus Azure Event Hubs und führt eine Abfrage aus, die die Funktion aufruft. Diese Funktion liest Daten aus dem Stream Analytics-Auftrag und schreibt sie in Azure Redis Cache.

![Diagramm der Beziehungen zwischen den Azure-Diensten](./media/stream-analytics-with-azure-functions/image1.png)

Die folgenden Schritte sind für diese Aufgabe erforderlich:
* [Erstellen eines Stream Analytics-Auftrags mit Event Hubs als Eingabe](#create-stream-analytics-job-with-event-hub-as-input)  
* [Erstellen einer Azure Redis Cache-Instanz](#create-an-azure-redis-cache)  
* [Erstellen einer Funktion in Azure Functions, die Daten in Azure Redis Cache schreiben kann](#create-an-azure-function-that-can-write-data-to-the-redis-cache)    
* [Aktualisieren des Stream Analytics-Auftrags mit der Funktion als Ausgabe](#update-the-stream-analytic-job-with-azure-function-as-output)  
* [Überprüfen der Ergebnisse in Azure Redis Cache](#check-redis-cache-for-results)  

## <a name="create-a-stream-analytics-job-with-event-hubs-as-input"></a>Erstellen eines Stream Analytics-Auftrags mit Event Hubs als Eingabe

Führen Sie das Tutorial zur [Betrugserkennung in Echtzeit](stream-analytics-real-time-fraud-detection.md) aus, um einen Event Hub zu erstellen, die Ereignisgenerator-Anwendung zu starten und einen Stream Analytics-Auftrag zu erstellen. (Überspringen Sie die Schritte zum Erstellen der Abfrage und der Ausgabe. Sehen Sie sich stattdessen die folgenden Abschnitte zum Einrichten der Functions-Ausgabe an.)

## <a name="create-an-azure-redis-cache-instance"></a>Erstellen einer Azure Redis Cache-Instanz

1. Erstellen Sie anhand der Schritte in [Erstellen eines Caches](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache) einen Cache in Azure Redis Cache.  

2. Wählen Sie nach der Erstellung des Caches unter **Einstellungen** die Option **Zugriffsschlüssel** aus. Notieren Sie sich die **Primäre Verbindungszeichenfolge**.

   ![Screenshot der Azure Redis Cache-Verbindungszeichenfolge](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-a-function-in-azure-functions-that-can-write-data-to-azure-redis-cache"></a>Erstellen einer Funktion in Azure Functions, die Daten in Azure Redis Cache schreiben kann

1. Lesen Sie den Abschnitt [Erstellen einer Funktions-App](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) in der Dokumentation zu Functions. Dort finden Sie die Schritte zum Erstellen einer Funktions-App und einer [Funktion in Azure Functions mit Auslösung per HTTP](../azure-functions/functions-create-first-azure-function.md#create-function) mithilfe der Sprache C#.  

2. Navigieren Sie zu der Funktion **run.csx**. Aktualisieren Sie sie mit dem folgenden Code. (Ersetzen Sie unbedingt „\<your redis cache connection string goes here\>“ durch die primäre Verbindungszeichenfolge von Azure Redis Cache, die Sie im vorherigen Abschnitt abgerufen haben.)  

   ```csharp
   using System;
   using System.Net;
   using System.Threading.Tasks;
   using StackExchange.Redis;
   using Newtonsoft.Json;
   using System.Configuration;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");
    
      // Get the request body
      dynamic dataArray = await req.Content.ReadAsAsync<object>();

      // Throw an HTTP Request Entity Too Large exception when the incoming batch(dataArray) is greater than 256 KB. Make sure that the size value is consistent with the value entered in the Stream Analytics portal.

      if (dataArray.ToString().Length > 262144)
      {        
         return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
      var connection = ConnectionMultiplexer.Connect("<your redis cache connection string goes here>");
      log.Info($"Connection string.. {connection}");
    
      // Connection refers to a property that returns a ConnectionMultiplexer
      IDatabase db = connection.GetDatabase();
      log.Info($"Created database {db}");
    
      log.Info($"Message Count {dataArray.Count}");

      // Perform cache operations using the cache object. For example, the following code block adds few integral data types to the cache
      for (var i = 0; i < dataArray.Count; i++)
      {
        string time = dataArray[i].time;
        string callingnum1 = dataArray[i].callingnum1;
        string key = time + " - " + callingnum1;
        db.StringSet(key, dataArray[i].ToString());
        log.Info($"Object put in database. Key is {key} and value is {dataArray[i].ToString()}");
       
      // Simple get of data types from the cache
      string value = db.StringGet(key);
      log.Info($"Database got: {value}");
      }

      return req.CreateResponse(HttpStatusCode.OK, "Got");
    }    

   ```

   Wenn Stream Analytics die Ausnahme „HTTP-Anforderungseinheit zu groß“ von der Funktion empfängt, verringert der Dienst die Größe der an Functions gesendeten Batches. Verwenden Sie in Ihrer Funktion den folgenden Code, um zu überprüfen, ob Stream Analytics zu große Batches sendet. Stellen Sie sicher, dass die Werte für die maximal zulässige Batchanzahl und -größe in der Funktion mit den Werten übereinstimmen, die im Stream Analytics-Portal eingegeben wurden.

   ```csharp
   if (dataArray.ToString().Length > 262144)
      {        
        return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
   ```

3. Erstellen Sie in einem Text-Editor Ihrer Wahl die JSON-Datei **project.json**. Verwenden Sie den folgenden Code, und speichern Sie sie auf Ihrem lokalen Computer. Diese Datei enthält die für die C#-Funktion erforderlichen NuGet-Paketabhängigkeiten.  
   
   ```json
       {
         "frameworks": {
             "net46": {
                 "dependencies": {
                     "StackExchange.Redis":"1.1.603",
                     "Newtonsoft.Json": "9.0.1"
                 }
             }
         }
     }

   ```
 
4. Wechseln Sie zurück zum Azure-Portal. Navigieren Sie auf der Registerkarte **Plattformfeatures** zu Ihrer Funktion. Wählen Sie unter **Entwicklungstools** die Option **App Service-Editor** aus. 
 
   ![Screenshot des App Service-Editors](./media/stream-analytics-with-azure-functions/image3.png)

5. Klicken Sie im App Service-Editor mit der rechten Maustaste auf Ihr Stammverzeichnis, und laden Sie die Datei **project.json** hoch. Aktualisieren Sie nach dem erfolgreichen Upload die Seite. Jetzt sollte eine automatisch generierte Datei namens **project.lock.json** angezeigt werden. Die automatisch generierte Datei enthält Verweise auf die DLL-Dateien, die in der Datei „project.json“ angegeben werden.  

   ![Screenshot des App Service-Editors](./media/stream-analytics-with-azure-functions/image4.png)

 

## <a name="update-the-stream-analytics-job-with-the-function-as-output"></a>Aktualisieren des Stream Analytics-Auftrags mit der Funktion als Ausgabe

1. Öffnen Sie den Stream Analytics-Auftrag im Azure-Portal.  

2. Navigieren Sie zu Ihrer Funktion, und wählen Sie **Übersicht** > **Ausgaben** > **Hinzufügen** aus. Um eine neue Ausgabe hinzuzufügen, wählen Sie **Azure-Funktion** als Senkenoption aus. Der neue Functions-Ausgabeadapter wird mit folgenden Eigenschaften zur Verfügung gestellt:  

   |**Eigenschaftenname**|**Beschreibung**|
   |---|---|
   |Ausgabealias| Ein Anzeigename, der in der Auftragsabfrage verwendet wird, um auf diese Ausgabe zu verweisen. |
   |Importoption| Sie können die Funktion aus dem aktuellem Abonnement verwenden, oder Sie geben die Einstellungen manuell an, wenn sich die Funktion in einem anderen Abonnement befindet. |
   |Funktionen-App| Der Name der Funktions-App |
   |Funktion| Der Name der Funktion in Ihrer Funktions-App (Name Ihrer run.csx-Funktion)|
   |Max Batch Size|Legt die maximale Größe für jeden Ausgabebatch fest, der an die Funktion gesendet wird. Dieser Wert ist standardmäßig auf 256 KB festgelegt.|
   |Max Batch Count|Gibt die maximale Anzahl von Ereignissen in jedem Batch an, die an die Funktion gesendet wird. Der Standardwert ist 100. Diese Eigenschaft ist optional.|
   |Schlüssel|Ermöglicht die Verwendung einer Funktion aus einem anderen Abonnement. Geben Sie den Schlüsselwert für den Zugriff auf die Funktion an. Diese Eigenschaft ist optional.|

3. Geben Sie einen Namen für den Ausgabealias an. In diesem Tutorial nennen wir ihn **saop1** (Sie können einen beliebigen Namen verwenden). Geben Sie weitere Details an.  

4. Öffnen Sie den Stream Analytics-Auftrag, und aktualisieren Sie die Abfrage wie folgt. (Ersetzen Sie den Text „saop1“, wenn Sie die Ausgabesenke anders benannt haben.)  

   ```sql
    SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        INTO saop1
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
           JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum
   ```

5. Starten Sie die Anwendung „telcodatagen.exe“, indem Sie den folgenden Befehl an der Befehlszeile ausführen (verwenden Sie das Format `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]`):  
   
   **telcodatagen.exe 1000 .2 2**
    
6.  Starten des Stream Analytics-Auftrags

## <a name="check-azure-redis-cache-for-results"></a>Überprüfen der Ergebnisse in Azure Redis Cache

1. Browsen Sie zum Azure-Portal, und suchen Sie Ihre Azure Redis Cache-Instanz. Wählen Sie **Konsole** aus.  

2. Vergewissern Sie sich mithilfe von [Redis Cache-Befehlen](https://redis.io/commands), dass Ihre Daten in Redis Cache enthalten sind. (Der Befehl weist das Format „Get {Schlüssel}“ auf.) Beispiel: 

   **Get "12/19/2017 21:32:24 - 123414732"**

   Dieser Befehl sollte den Wert für den angegebenen Schlüssel ausgeben:

   ![Screenshot der Azure Redis Cache-Ausgabe](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="known-issues"></a>Bekannte Probleme

Wenn Sie im Azure-Portal versuchen, die Werte für die maximal zulässige Batchgröße oder die maximal zulässige Batchanzahl auf einen leeren Wert (Standard) zurückzusetzen, ändert sich der Wert beim Speichern in den zuvor eingegebenen Wert. Geben Sie in diesem Fall die Standardwerte für diese Felder manuell ein.

