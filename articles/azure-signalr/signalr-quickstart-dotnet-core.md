---
title: Schnellstartanleitung zur Verwendung des Azure SignalR Service | Microsoft-Dokumentation
description: Eine Schnellstartanleitung zur Verwendung des Azure SignalR Service zum Erstellen eines Chatraums mit ASP.NET Core MVC-Apps.
services: signalr
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET
ms.workload: tbd
ms.date: 06/13/2018
ms.author: wesmc
ms.openlocfilehash: 4c34bd10768ab7acf4700b29386d3a71532490db
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38674851"
---
# <a name="quickstart-create-a-chat-room-with-signalr-service"></a>Schnellstart: Erstellen eines Chatraums per SignalR Service

Der Microsoft Azure SignalR-Dienst befindet sich derzeit in der [öffentlichen Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure SignalR Service ist ein Azure-Dienst, der Entwicklern die einfache Erstellung von Webanwendungen mit Echtzeitfunktionen ermöglicht. Dieser Dienst basiert auf [SignalR für ASP.NET Core 2.0](https://docs.microsoft.com/aspnet/core/signalr/introduction).

In diesem Artikel wird der Einstieg in den Azure SignalR Service beschrieben. In dieser Schnellstartanleitung erstellen Sie eine Chatanwendung, indem Sie eine Web-App vom Typ „ASP.NET Core MVC“ verwenden. Mit dieser App wird eine Verbindung mit Ihrer Azure SignalR Service-Ressource hergestellt, um Echtzeitupdates des Inhalts zu ermöglichen. Sie hosten die Webanwendung lokal und stellen Verbindungen mit mehreren Browserclients her. Von jedem Client aus können Inhaltsupdates per Pushvorgang an alle anderen Clients gesendet werden. 


Sie können einen beliebigen Code-Editor nutzen, um die Schritte dieser Schnellstartanleitung auszuführen. [Visual Studio Code](https://code.visualstudio.com/) ist aber eine hervorragende Option, die auf Windows-, macOS- und Linux-Plattformen verfügbar ist.

Der Code für dieses Tutorial kann aus dem [GitHub-Repository „AzureSignalR-samples“](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/ChatRoom) heruntergeladen werden.  Die Erstellung der in dieser Schnellstartanleitung verwendeten Azure-Ressourcen ist gemäß den Informationen unter [Create a SignalR Service script](scripts/signalr-cli-create-service.md) (Erstellen eines SignalR Service-Skripts) möglich.

![Schnellstart – Lokale Durchführung](media/signalr-quickstart-dotnet-core/signalr-quickstart-complete-local.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie das [.NET Core SDK](https://www.microsoft.com/net/download/windows).
* Laden Sie das GitHub-Repository [AzureSignalR-sample](https://github.com/aspnet/AzureSignalR-samples) herunter, oder klonen Sie es. 

## <a name="create-an-azure-signalr-resource"></a>Erstellen einer Azure SignalR-Ressource

[!INCLUDE [azure-signalr-create](../../includes/signalr-create.md)]

## <a name="create-an-aspnet-core-web-app"></a>Erstellen einer ASP.NET Core-Web-App

In diesem Abschnitt verwenden Sie die [.NET Core-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/dotnet/core/tools/), um ein neues ASP.NET Core MVC-Web-App-Projekt zu erstellen. Der Vorteil bei Verwendung der .NET Core-CLI gegenüber Visual Studio ist, dass sie für alle Windows-, macOS- und Linux-Plattformen verfügbar ist. 

1. Erstellen Sie einen neuen Ordner für Ihr Projekt. In dieser Schnellstartanleitung wird der Ordner *E:\Testing\chattest* verwendet.

2. Führen Sie im neuen Ordner den folgenden Befehl aus, um ein neues Projekt vom Typ „ASP.NET Core MVC-Web-App“ zu erstellen:

        dotnet new mvc


## <a name="add-secret-manager-to-the-project"></a>Hinzufügen des Geheimnis-Managers

In diesem Abschnitt fügen Sie Ihrem Projekt das [Geheimnis-Manager-Tool](https://docs.microsoft.com/aspnet/core/security/app-secrets) hinzu. Im Geheimnis-Manager-Tool werden sensible Daten für die Entwicklungsarbeit außerhalb Ihrer Projektstruktur gespeichert. Mit diesem Ansatz können Sie verhindern, dass App-Geheimnisse versehentlich im Quellcode angegeben werden.

1. Öffnen Sie die Datei mit der Endung *.csproj*. Fügen Sie das `DotNetCliToolReference`-Element hinzu, um *Microsoft.Extensions.SecretManager.Tools* einzubinden. Fügen Sie außerdem wie unten gezeigt das `UserSecretsId`-Element hinzu, und speichern Sie die Datei.

    *chattest.csproj:*

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
    <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <UserSecretsId>SignalRChatRoomEx</UserSecretsId>
    </PropertyGroup>
    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
    </ItemGroup>
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
        <DotNetCliToolReference Include="Microsoft.Extensions.SecretManager.Tools" Version="2.0.0" />
    </ItemGroup>
    </Project>    
    ```

## <a name="add-azure-signalr-to-the-web-app"></a>Hinzufügen von Azure SignalR zur Web-App

1. Fügen Sie einen Verweis auf das NuGet-Paket `Microsoft.Azure.SignalR` hinzu, indem Sie den folgenden Befehl ausführen:

        dotnet add package Microsoft.Azure.SignalR -v 1.0.0-*

2. Führen Sie den folgenden Befehl aus, um Pakete für Ihr Projekt wiederherzustellen:

        dotnet restore

3. Fügen Sie dem Geheimnis-Manager ein Geheimnis mit dem Namen *Azure__SignalR__ConnectionString* hinzu. Dieses Geheimnis ist ein hierarchischer Konfigurationswert, und ein Doppelpunkt (:) funktioniert möglicherweise nicht auf allen Plattformen. Ein doppelter Unterstrich (__), so wie von diesem Geheimnis verwendet, wird von allen Plattformen unterstützt.  Dieses Geheimnis enthält die Verbindungszeichenfolge für den Zugriff auf Ihre SignalR Service-Ressource. *Azure__SignalR__ConnectionString* ist der Standardkonfigurationsschlüssel, nach dem SignalR sucht, um eine Verbindung herzustellen. Ersetzen Sie den Wert im Befehl unten durch die Verbindungszeichenfolge für Ihre SignalR Service-Ressource.

    Dieser Befehl muss in dem Verzeichnis ausgeführt werden, in dem die *.csproj*-Datei enthalten ist.

    ```
    dotnet user-secrets set Azure__SignalR__ConnectionString "Endpoint=<Your endpoint>;AccessKey=<Your access key>;"    
    ```

    Der Geheimnis-Manager wird nur verwendet, um die Web-App zu testen, während sie lokal gehostet wird. In einem späteren Tutorial stellen Sie die Chat-Web-App dann in Azure bereit. Nachdem Sie die Web-App in Azure bereitgestellt haben, verwenden Sie eine Anwendungseinstellung, anstatt die Verbindungszeichenfolge im Geheimnis-Manager zu speichern.

4. Öffnen Sie *Startup.cs*, und aktualisieren Sie die `ConfigureServices`-Methode auf die Nutzung des Azure SignalR Service, indem Sie die `services.AddSignalR().AddAzureSignalR()`-Methode aufrufen:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        services.AddSignalR().AddAzureSignalR();
    }
    ```

    Indem kein Parameter an `AddAzureSignalR()` übergeben wird, wird in diesem Code der Standardkonfigurationsschlüssel *Azure__SignalR__ConnectionString* für die Verbindungszeichenfolge der SignalR Service-Ressource verwendet.

5. Aktualisieren Sie ebenfalls in *Startup.cs* die `Configure`-Methode, indem Sie den Aufruf von `app.UseStaticFiles()` durch den folgenden Code ersetzen und die Datei speichern.

    ```csharp
    app.UseFileServer();
    app.UseAzureSignalR(routes =>
    {
        routes.MapHub<Chat>("/chat");
    });
    ```            

### <a name="add-a-hub-class"></a>Hinzufügen einer Hubklasse

In SignalR ist ein Hub eine Kernkomponente, über die eine Reihe von Methoden verfügbar gemacht wird, die von einem Client aus aufgerufen werden können. In diesem Abschnitt definieren Sie eine Hubklasse mit zwei Methoden: 

* `Broadcast`: Mit dieser Methode wird eine Nachricht an alle Clients übertragen.
* `Echo`: Mit dieser Methode wird eine Nachricht zurück an den Aufrufer gesendet.

Für beide Methoden wird die Schnittstelle `Clients` genutzt, die über das ASP.NET Core SignalR SDK bereitgestellt wird. Mit dieser Schnittstelle haben Sie Zugriff auf alle verbundenen Clients und können Inhalt per Pushvorgang auf die Clients übertragen.

1. Fügen Sie in Ihrem Projektverzeichnis einen neuen Ordner mit dem Namen *Hub* hinzu. Fügen Sie dem neuen Ordner eine neue Hubcodedatei mit dem Namen *Chat.cs* hinzu.

2. Fügen Sie *Chat.cs* den folgenden Code hinzu, um Ihre Hubklasse zu definieren und die Datei zu speichern. 

    Aktualisieren Sie den Namespace für diese Klasse, falls Sie einen anderen Projektnamen als *chattest* verwendet haben.

    ```csharp
    using Microsoft.AspNetCore.SignalR;

    namespace chattest
    {

        public class Chat : Hub
        {
            public void BroadcastMessage(string name, string message)
            {
                Clients.All.SendAsync("broadcastMessage", name, message);
            }

            public void Echo(string name, string message)
            {
                Clients.Client(Context.ConnectionId).SendAsync("echo", name, message + " (echo from server)");
            }
        }
    }
    ```

### <a name="add-the-web-app-client-interface"></a>Hinzufügen der Web-App-Clientschnittstelle

Die Clientbenutzeroberfläche für diese Chatraum-App setzt sich aus HTML- und JavaScript-Code in einer Datei mit dem Namen *index.html* zusammen, die im Verzeichnis *wwwroot* enthalten ist.

Kopieren Sie die Datei *index.html* und die Ordner *css* und *scripts* aus dem Ordner *wwwroot* des [Repositorys mit Beispielen](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/ChatRoom/wwwroot) in den Ordner *wwwroot* Ihres Projekts.

Dies ist der Hauptcode von *index.html*: 

```javascript
var connection = new signalR.HubConnectionBuilder()
                            .withUrl('/chat')
                            .build();
bindConnectionMessage(connection);
connection.start()
    .then(function () {
        onConnected(connection);
    })
    .catch(function (error) {
        console.error(error.message);
    });
```    

Im Code in *index.html* wird `HubConnectionBuilder.build()` aufgerufen, um eine HTTP-Verbindung mit der Azure SignalR-Ressource herzustellen.

Wenn die Verbindungsherstellung erfolgreich ist, wird diese Verbindung an `bindConnectionMessage` übergeben, wo Ereignishandler für eingehende Pushvorgänge für Inhalte an den Client hinzugefügt werden. 

`HubConnection.start()` startet die Kommunikation mit dem Hub. Nachdem die Kommunikation gestartet wurde, wird `onConnected()` verwendet, um die Schaltflächen-Ereignishandler hinzuzufügen. Diese Handler nutzen die Verbindung, um für diesen Client das Übertragen von Inhaltsupdates per Pushvorgang an alle verbundenen Clients zu ermöglichen.

## <a name="add-a-development-runtime-profile"></a>Hinzufügen eines Entwicklungslaufzeit-Profils

In diesem Abschnitt fügen Sie eine Entwicklungslaufzeitumgebung für ASP.NET Core hinzu. Weitere Informationen zur Laufzeitumgebung für ASP.NET Core finden Sie unter [Arbeiten mit mehreren Umgebungen](https://docs.microsoft.com/aspnet/core/fundamentals/environments).

1. Erstellen Sie in Ihrem Projekt einen neuen Ordner mit dem Namen *Properties*.

2. Fügen Sie dem Ordner eine neue Datei mit dem Namen *launchSettings.json* hinzu, die den folgenden Inhalt aufweist, und speichern Sie die Datei.

    ```json
    {
        "profiles" : 
        {
            "ChatRoom": 
            {
                "commandName": "Project",
                "launchBrowser": true,
                "environmentVariables": 
                {
                    "ASPNETCORE_ENVIRONMENT": "Development"
                },
                "applicationUrl": "http://localhost:5000/"
            }
        }
    }
    ```


## <a name="build-and-run-the-app-locally"></a>Lokales Erstellen und Ausführen der App

1. Führen Sie den folgenden Befehl in der Befehlsshell aus, um die App per .NET Core-CLI zu erstellen:

        dotnet build

2. Führen Sie nach dem erfolgreichen Abschluss des Buildvorgangs den folgenden Befehl aus, um die Web-App lokal auszuführen:

        dotnet run

    Die App wird lokal über Port 5000 gehostet, wie in Ihrem Entwicklungslaufzeit-Profil konfiguriert:

        E:\Testing\chattest>dotnet run
        Hosting environment: Development
        Content root path: E:\Testing\chattest
        Now listening on: http://localhost:5000
        Application started. Press Ctrl+C to shut down.    

3. Starten Sie zwei Browserfenster, und navigieren Sie in jedem Browser zu `http://localhost:5000`. Sie werden aufgefordert, Ihren Namen einzugeben. Geben Sie einen Clientnamen für beide Clients ein, und testen Sie das Übertragen von Nachrichteninhalten zwischen den Clients per Pushvorgang über die Schaltfläche **Send** (Senden).

    ![Schnellstart – Lokale Durchführung](media/signalr-quickstart-dotnet-core/signalr-quickstart-complete-local.png)



## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie mit dem nächsten Tutorial fortfahren möchten, können Sie die in dieser Schnellstartanleitung erstellten Ressourcen beibehalten und im nächsten Tutorial wiederverwenden.

Wenn Sie die Schnellstart-Beispielanwendung nicht mehr benötigen, können Sie die in dieser Schnellstartanleitung erstellten Azure-Ressourcen löschen, um das Anfallen von Kosten zu vermeiden. 

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden also dauerhaft gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. Falls Sie die Ressourcen zum Hosten dieses Beispiels in einer vorhandenen Ressourcengruppe erstellt haben, die beizubehaltende Ressourcen enthält, können Sie die Ressourcen einzeln über das jeweilige Blatt löschen, statt die Ressourcengruppe zu löschen.
> 
> 

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Ressourcengruppen**.

Geben Sie im Textfeld **Nach Name filtern...** den Namen Ihrer Ressourcengruppe ein. In dieser Schnellstartanleitung wurde eine Ressourcengruppe mit dem Namen *SignalRTestResources* verwendet. Klicken Sie in Ihrer Ressourcengruppe in der Ergebnisliste auf **...** und dann auf **Ressourcengruppe löschen**.

   
![Löschen](./media/signalr-quickstart-dotnet-core/signalr-delete-resource-group.png)


Sie werden aufgefordert, das Löschen der Ressourcengruppe zu bestätigen. Geben Sie den Namen der entsprechenden Ressourcengruppe ein, und klicken Sie auf **Löschen**.
   
Daraufhin werden die Ressourcengruppe und alle darin enthaltenen Ressourcen gelöscht.



## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine neue Azure SignalR Service-Ressource erstellt und mit einer ASP.NET Core-Web-App genutzt, um Inhaltsupdates per Pushvorgang in Echtzeit auf mehrere verbundene Clients zu übertragen. Weitere Informationen zur Verwendung des Azure SignalR Service finden Sie im nächsten Tutorial, in dem es um die Authentifizierung geht.

> [!div class="nextstepaction"]
> [Azure SignalR Service-Authentifizierung](./signalr-authenticate-oauth.md)


