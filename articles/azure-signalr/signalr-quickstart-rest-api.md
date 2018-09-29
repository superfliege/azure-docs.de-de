---
title: 'Schnellstart: REST-API des Azure SignalR-Diensts | Microsoft Docs'
description: Ein Schnellstart für die Verwendung der REST-API des Azure SignalR-Diensts.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET
ms.workload: tbd
ms.date: 06/13/2018
ms.author: zhshang
ms.openlocfilehash: 93c1198ecfba6db809228ed6dcd99c705f53926c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972758"
---
# <a name="quickstart-broadcast-real-time-messages-from-console-app"></a>Schnellstart: Senden von Nachrichten in Echtzeit aus einer Konsolen-App

Der Azure SignalR-Dienst stellt die [REST API](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md) zur Verfügung, um Kommunikationsszenarien zwischen Servern und Clients (z.B. Broadcasting) zu unterstützen. Sie können jede beliebige Programmiersprache auswählen, die die REST-API aufrufen kann. Sie können Nachrichten an alle verbundenen Clients, an einen bestimmten Client anhand seines Namens oder an eine Gruppe von Clients übermitteln.

In diesem Schnellstart erfahren Sie, wie Sie Nachrichten aus einer Befehlszeilenanwendung an verbundene Clientanwendungen in C# senden können.

## <a name="prerequisites"></a>Voraussetzungen

Dieser Schnellstart kann unter MacOS, Windows oder Linux ausgeführt werden.
* [.NET Core SDK](https://www.microsoft.com/net/download/core)
* Ein Text-Editor oder Code-Editor Ihrer Wahl.


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter <https://portal.azure.com/> mit Ihrem Azure-Konto beim Azure-Portal an.


[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Während der Dienst bereitgestellt wird, bereiten wird den Code vor. Klonen Sie die [Beispiel-App von GitHub](https://github.com/aspnet/AzureSignalR-samples.git), legen Sie die Verbindungszeichenfolge des SignalR-Diensts fest, und führen Sie die Anwendung lokal aus.

1. Öffnen Sie ein Git-Terminalfenster. Wechseln Sie in einen Ordner, in dem Sie das Beispielprojekt klonen möchten.

1. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer.

    ```bash
    git clone https://github.com/aspnet/AzureSignalR-samples.git
    ```

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Dieses Beispiel ist eine Konsolen-App, die die Verwendung des Azure SignalR-Diensts zeigt. Sie bietet zwei Modi:

- Servermodus: Verwenden Sie einfache Befehle zum Aufrufen der REST-API des Azure SignalR-Diensts.
- Clientmodus: Stellen Sie eine Verbindung mit dem Azure SignalR-Dienst her, und empfangen Sie Nachrichten vom Server.

Sie können auch herausfinden, wie Sie ein Zugriffstoken zur Authentifizierung mit dem Azure SignalR-Dienst generieren.

### <a name="build-the-executable-file"></a>Erstellen der ausführbaren Datei

Wir verwenden macOS osx.10.13-x64 als Beispiel. Sie können [Referenzmaterial](https://docs.microsoft.com/dotnet/core/rid-catalog) zum Buildvorgang auf anderen Plattformen nutzen.
```bash
cd AzureSignalR-samples/samples/Serverless/

dotnet publish -c Release -r osx.10.13-x64
```

### <a name="start-a-client"></a>Starten Sie einen Client.

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="start-a-server"></a>Starten Sie einen Server.

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless server -c "<ConnectionString>" -h <HubName>
```

## <a name="run-the-sample-without-publishing"></a>Führen Sie das Beispiel ohne Veröffentlichung aus.

Sie können auch den folgenden Befehl ausführen, um einen Server oder Client zu starten.

```bash
# Start a server
dotnet run -- server -c "<ConnectionString>" -h <HubName>

# Start a client
dotnet run -- client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="use-user-secrets-to-specify-connection-string"></a>Verwenden von „user-secrets“ zum Angeben der Verbindungszeichenfolge

Sie können `dotnet user-secrets set Azure:SignalR:ConnectionString "<ConnectionString>"` im Stammverzeichnis des Beispiels ausführen. Danach benötigen Sie die Option `-c "<ConnectionString>"` nicht mehr.

## <a name="usage"></a>Verwendung

Nachdem der Server gestartet wurde, verwenden Sie den Befehl zum Senden von Nachrichten.

```
send user <User Id>
send users <User List>
send group <Group Name>
send groups <Group List>
broadcast
```

Sie können mehrere Clients mit unterschiedlichen Clientnamen starten.


[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]
