---
title: Erstellen Ihrer ersten Funktion im Azure-Portal | Microsoft Docs
description: Erfahren Sie, wie Sie Ihre erste Azure-Funktion für die serverlose Ausführung mit dem Azure-Portal erstellen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/28/2018
ms.author: glenga
ms.custom: mvc, devcenter, cc996988-fb4f-47
ms.openlocfilehash: 3e04f231ad68d3da4b705d1fe78163628bed7c0a
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617716"
---
# <a name="create-your-first-function-in-the-azure-portal"></a>Erstellen Ihrer ersten Funktion im Azure-Portal

Mit Azure Functions können Sie Code in einer [serverlosen](https://azure.microsoft.com/overview/serverless-computing/) Umgebung ausführen, ohne vorher eine VM erstellen oder eine Webanwendung veröffentlichen zu müssen. In diesem Thema erfahren Sie, wie Sie Azure Functions verwenden können, um eine „Hello World“-Funktion im Azure-Portal zu erstellen.

![Erstellen einer Funktionen-App im Azure-Portal](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

> [!NOTE]
> C#-Entwickler sollten für die [Erstellung ihrer ersten Funktion Visual Studio 2017](functions-create-your-first-function-visual-studio.md) statt das Portal in Erwägung ziehen. 

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter <http://portal.azure.com> mit Ihrem Azure-Konto beim Azure-Portal an.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Sie müssen über eine Funktionen-App verfügen, die die Ausführung Ihrer Funktionen in Azure hostet. Sie können mit einer Funktionen-App Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Erstellen Sie als Nächstes in der neuen Funktionen-App eine Funktion.

## <a name="create-function"></a>Erstellen einer Funktion mit Auslösung per HTTP

1. Erweitern Sie die neue Funktionen-App, und klicken Sie anschließend auf die Schaltfläche **+** neben **Functions**.

2.  Wählen Sie auf der Seite **Get started quickly** (Schnelleinstieg) die Option **WebHook + API**, **wählen Sie eine Sprache** für die Funktion, und klicken Sie dann auf **Diese Funktion erstellen**. 
   
    ![Schnellstart von Functions im Azure-Portal](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

Eine Funktion wird in Ihrer gewählten Sprache anhand der Vorlage für eine Funktion mit Auslösung per HTTP erstellt. In diesem Thema wird zwar eine C#-Skriptfunktion im Portal gezeigt, Sie können jedoch eine Funktion in einer beliebigen [unterstützten Sprache](supported-languages.md) erstellen. 

Nun können Sie die neue Funktion ausführen, indem Sie eine HTTP-Anforderung senden.

## <a name="test-the-function"></a>Testen der Funktion

1. Klicken Sie in der neuen Funktion rechts oben auf **</> Funktions-URL abrufen**, wählen Sie **default (Function key)** (Standard (Funktionsschlüssel)) aus, und klicken Sie dann auf **Kopieren**. 

    ![Kopieren der URL der Funktion aus dem Azure-Portal](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. Fügen Sie die URL der Funktion in die Adressleiste Ihres Browsers ein. Fügen Sie den Wert der Abfragezeichenfolge `&name=<yourname>` am Ende der URL hinzu, und drücken Sie die Taste `Enter` auf Ihrer Tastatur, um die Anforderung auszuführen. Daraufhin sollte die von der Funktion zurückgegebene Antwort im Browser angezeigt werden.  

    Im folgenden Beispiel ist die Antwort im Microsoft Edge-Browser dargestellt (bei anderen Browsern wird unter Umständen auch XML-Code angezeigt):

    ![Funktionsantwort im Browser.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    Die Anfrage-URL enthält einen Schlüssel, der standardmäßig über HTTP auf Ihre Funktion zugreifen muss.   

3. Wenn Ihre Funktion ausgeführt wird, werden Nachverfolgungsinformationen in die Protokolle geschrieben. Kehren Sie zu Ihrer Funktion im Portal zurück, und klicken Sie am unteren Bildschirmrand auf den Pfeil, um **Protokolle** zu erweitern und die Nachverfolgungsausgabe der vorherigen Ausführung anzuzeigen. 

   ![Viewer der Funktionsprotokolle im Azure-Portal](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Funktionen-App mit einer einfachen Funktion mit Auslösung per HTTP erstellt.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Weitere Informationen finden Sie unter [HTTP- und Webhookbindungen in Azure Functions](functions-bindings-http-webhook.md).



