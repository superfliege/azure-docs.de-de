---
title: Includedatei
description: Includedatei
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: 73d40bfb5a7e691cead5a84be70398e9cbf6656a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53262771"
---
## <a name="run-the-web-application"></a>Ausführen der Webanwendung

1. Es ist eine Single-Page-Beispielwebanwendung verfügbar, die in GitHub gehostet wird. Öffnen Sie Ihren Browser mit [https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat/](https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat/).

    > [!NOTE]
    > Die HTML-Quelldatei befindet sich unter [/docs/demo/chat/index.html](https://github.com/Azure-Samples/signalr-service-quickstart-serverless-chat/blob/master/docs/demo/chat/index.html).

1. Wenn Sie zur Eingabe der Basis-URL für die Funktions-App aufgefordert werden, geben Sie *http://localhost:7071* ein.

1. Geben Sie einen Benutzernamen ein, wenn Sie dazu aufgefordert werden.

1. Die Webanwendung ruft die Funktion *GetSignalRInfo* in der Funktions-App auf, um die Verbindungsinformationen zum Herstellen einer Verbindung mit dem Azure SignalR-Dienst abzurufen. Nachdem die Verbindung hergestellt wurde, wird das Eingabefeld für Chatnachrichten angezeigt.

1. Geben Sie eine Nachricht ein, und drücken Sie die EINGABETASTE. Die Anwendung sendet die Nachricht an die *SendMessage*-Funktion in der Azure Functions-App, die dann die SignalR-Ausgabebindung verwendet, um die Nachricht an alle verbundenen Clients zu senden. Wenn alles ordnungsgemäß funktioniert, sollte die Nachricht in der Anwendung angezeigt werden.

    ![Ausführen der Anwendung](../media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-run-application.png)

1. Öffnen Sie eine andere Instanz der Webanwendung in einem anderen Browserfenster. Sie können erkennen, dass alle gesendeten Nachrichten in allen Instanzen der Anwendung angezeigt werden.