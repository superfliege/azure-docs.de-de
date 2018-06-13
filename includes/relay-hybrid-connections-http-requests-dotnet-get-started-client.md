---
title: Includedatei
description: Includedatei
services: service-bus-relay
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 05/02/2018
ms.author: clemensv
ms.custom: include file
ms.openlocfilehash: 4a3f38e1423db0755d8c76f8850e41173d250f43
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33904981"
---
### <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Wenn Sie die Option „Clientautorisierung erforderlich“ bei der Relay-Erstellung deaktiviert haben, können Sie mit jedem Browser Anforderungen an die URL der Hybridverbindungen senden. Für den Zugriff auf geschützte Endpunkte müssen Sie ein Token im `ServiceBusAuthorization`-Header, der hier gezeigt wird, erstellen und übergeben.

Erstellen Sie in Visual Studio ein neues Projekt vom Typ **Konsolen-App (.NET Framework)**.

### <a name="add-the-relay-nuget-package"></a>Hinzufügen des Relay-NuGet-Pakets

1. Klicken Sie mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
2. Klicken Sie auf **Durchsuchen**, und suchen Sie nach **Microsoft.Azure.Relay**. Klicken Sie in den Suchergebnissen auf **Microsoft Azure-Relay**. 
3. Klicken Sie auf **Installieren**, um die Installation abzuschließen. Schließen Sie das Dialogfeld.

### <a name="write-code-to-send-requests"></a>Schreiben von Code zum Senden von Anforderungen

1. Ersetzen Sie die vorhandenen `using`-Anweisungen am Anfang der Datei „Program.cs“ durch die folgenden `using`-Anweisungen:
   
    ```csharp
    using System;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Relay;
    ```
2. Fügen Sie der `Program`-Klasse Konstanten als Hybridverbindungsdetails hinzu. Ersetzen Sie die Platzhalter in Klammern durch die Werte, die beim Erstellen der Hybridverbindung abgerufen wurden. Verwenden Sie den vollqualifizierten Namespacenamen.
   
    ```csharp
    private const string RelayNamespace = "{RelayNamespace}.servicebus.windows.net";
    private const string ConnectionName = "{HybridConnectionName}";
    private const string KeyName = "{SASKeyName}";
    private const string Key = "{SASKey}";
    ```
3. Fügen Sie der `Program`-Klasse die folgende Methode hinzu:
   
    ```csharp
    private static async Task RunAsync()
    {
        var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                KeyName, Key);
        var uri = new Uri(string.Format("https://{0}/{1}", RelayNamespace, ConnectionName));
        var token = (await tokenProvider.GetTokenAsync(uri.AbsoluteUri, TimeSpan.FromHours(1))).TokenString;
        var client = new HttpClient();
        var request = new HttpRequestMessage()
        {
            RequestUri = uri,
            Method = HttpMethod.Get,
        };
        request.Headers.Add("ServiceBusAuthorization", token);
        var response = await client.SendAsync(request);
        Console.WriteLine(await response.Content.ReadAsStringAsync());
    }
    ```
4. Fügen Sie der `Main`-Methode in der `Program`-Klasse die folgende Codezeile hinzu.
   
    ```csharp
    RunAsync().GetAwaiter().GetResult();
    ```
   
    Die Datei „Program.cs“ sollte wie folgt aussehen:
   
    ```csharp
    using System;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Relay;
   
    namespace Client
    {
        class Program
        {
            private const string RelayNamespace = "{RelayNamespace}.servicebus.windows.net";
            private const string ConnectionName = "{HybridConnectionName}";
            private const string KeyName = "{SASKeyName}";
            private const string Key = "{SASKey}";
   
            static void Main(string[] args)
            {
                RunAsync().GetAwaiter().GetResult();
            }
   
            private static async Task RunAsync()
            {
               var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                KeyName, Key);
                var uri = new Uri(string.Format("https://{0}/{1}", RelayNamespace, ConnectionName));
                var token = (await tokenProvider.GetTokenAsync(uri.AbsoluteUri, TimeSpan.FromHours(1))).TokenString;
                var client = new HttpClient();
                var request = new HttpRequestMessage()
                {
                    RequestUri = uri,
                    Method = HttpMethod.Get,
                };
                request.Headers.Add("ServiceBusAuthorization", token);
                var response = await client.SendAsync(request);
                Console.WriteLine(await response.Content.ReadAsStringAsync());
            }
        }
    }
    ```

