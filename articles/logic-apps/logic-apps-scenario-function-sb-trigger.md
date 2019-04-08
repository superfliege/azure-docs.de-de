---
title: Szenario – Auslösen von Logik-Apps mit Azure Functions und Azure Service Bus | Microsoft-Dokumentation
description: Erstellen von Funktionen zum Auslösen von Logik-Apps mithilfe von Azure Functions und Azure Service Bus
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jehollan, klam, LADocs
ms.topic: article
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.date: 08/25/2018
ms.openlocfilehash: 1d3c4039ae823d3797e768af5892333d4d925268
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57789940"
---
# <a name="scenario-trigger-logic-apps-with-azure-functions-and-azure-service-bus"></a>Szenario: Auslösen von Logik-Apps mit Azure Functions und Azure Service Bus

Sie können Azure Functions verwenden, um einen Trigger für Logik-Apps zu erstellen, wenn Sie einen Listener oder Aufgaben mit langer Ausführungsdauer bereitstellen müssen. Beispielsweise können Sie eine Funktion erstellen, die an einer Warteschlange lauscht und sofort eine Logik-App als Pushtrigger auslöst.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich <a href="https://azure.microsoft.com/free/" target="_blank">für ein kostenloses Azure-Konto registrieren</a>. 

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

* Bevor Sie eine Azure-Funktion erstellen können, müssen Sie eine [Funktions-App erstellen](../azure-functions/functions-create-function-app-portal.md).

## <a name="create-logic-app"></a>Erstellen einer Logik-App

In diesem Beispiel führen Sie eine Funktion für jede Logik-App aus, die ausgelöst werden muss. Erstellen Sie zunächst eine Logik-App mit einem HTTP-Anforderungstrigger. Die Funktion ruft jedes Mal, wenn eine Warteschlangennachricht empfangen wird, einen Endpunkt auf.  

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und erstellen Sie leere Logik-App. 

   Falls Sie noch nicht mit Logik-Apps gearbeitet haben, lesen Sie zunächst das Dokument [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Geben Sie im Suchfeld den Begriff „HTTP-Anforderung“. Wählen Sie in der Triggerliste den folgenden Trigger aus: **Beim Empfang einer HTTP-Anforderung**

   ![Trigger auswählen](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger.png)

1. Für den **Anforderungstrigger** können Sie optional ein JSON-Schema für die Verwendung mit der Warteschlangenmeldung eingeben. JSON-Schemas können dem Logik-App-Designer die Struktur der Eingabedaten verdeutlichen und erleichtern Ihnen die Auswahl von Ausgaben im gesamten Workflow. 

   Zum Angeben eines Schemas geben Sie dieses im Feld **JSON-Schema für Anforderungstext** ein, z.B.: 

   ![JSON-Schema angeben](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger-schema.png)

   Wenn Sie kein Schema haben, jedoch über eine Beispielnutzlast im JSON-Format verfügen, können Sie aus dieser Nutzlast ein Schema generieren.

   1. Wählen Sie im Anforderungstrigger **Beispielnutzlast zum Generieren eines Schemas verwenden**.

   1. Geben Sie unter **Geben oder fügen Sie eine JSON-Beispielnutzlast ein** Ihre Beispielnutzlast ein, und wählen Sie dann **Fertig**.
      
      ![Beispielnutzlast eingeben](./media/logic-apps-scenario-function-sb-trigger/enter-sample-payload.png)

   Diese Beispielnutzlast generiert dieses Schema, das im Trigger angezeigt wird:

   ```json
   {
      "type": "object",
      "properties": {
         "address": {
            "type": "object",
            "properties": {
               "number": {
                  "type": "integer"
               },
               "street": {
                  "type": "string"
               },
               "city": {
                  "type": "string"
               },
               "postalCode": {
                  "type": "integer"
               },
               "country": {
                  "type": "string"
               }
            }
         }
      }
   }
   ```

1. Fügen Sie alle weiteren Aktionen hinzu, die nach dem Empfang der Warteschlangenmeldung ausgeführt werden sollen. 

   Beispielsweise können Sie eine E-Mail mit dem Office 365 Outlook-Connector senden.

1. Speichern Sie Ihre Logik-App, um die Rückruf-URL für den Trigger in dieser Logik-App zu generieren. Diese URL wird **HTTP POST-URL**-Eigenschaft angezeigt.

   ![Generierte Rückruf-URL für Trigger](./media/logic-apps-scenario-function-sb-trigger/callback-URL-for-trigger.png)

## <a name="create-azure-function"></a>Azure-Funktion erstellen

Als erstellen Sie eine Funktion, die als Trigger fungiert und an der Warteschlange lauscht. 

1. Öffnen und erweitern Sie im Azure-Portal Ihre Funktionen-App, falls sie noch nicht geöffnet ist. 

1. Erweitern Sie unter den Namen Ihrer Funktionen-App **Funktionen**. Klicken Sie im Bereich **Funktionen** auf **Neue Funktion**. Diese Vorlage verwenden: **ServiceBus-Warteschlangentrigger – C#**
   
   ![Azure Functions-Portal auswählen](./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png)

1. Geben Sie einen Namen für Ihren Trigger an, und konfigurieren Sie dann die Verbindung mit der Service Bus-Warteschlange, die den `OnMessageReceive()`-Listener aus dem Azure Service Bus SDK verwendet.

1. Schreiben Sie eine einfache Funktion zum Aufrufen des zuvor erstellten Logik-App-Endpunkts, bei der die Warteschlangenmeldung als Trigger verwendet wird, z.B.: 
   
   ```CSharp
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   // Re-use instance of http clients if possible - https://docs.microsoft.com/en-us/azure/azure-functions/manage-connections
   private static HttpClient httpClient = new HttpClient();
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

       var response = httpClient.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
   }
   ```

   Dieses Beispiel verwendet den Inhaltstyp `application/json` für Nachrichten, Sie können diesen jedoch bei Bedarf ändern.

1. Zum Testen der Funktion fügen Sie mithilfe eines Tools wie dem [Service Bus-Explorer](https://github.com/paolosalvatori/ServiceBusExplorer) eine Warteschlangenmeldung hinzu. 

   Die Logik-App wird unmittelbar nach dem Empfang der Meldung durch die Funktion ausgelöst.

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](https://aka.ms/logicapps-wish).

