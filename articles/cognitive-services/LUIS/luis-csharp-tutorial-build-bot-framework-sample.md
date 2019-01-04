---
title: 'Bot: C# (v3)'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Erstellen Sie mithilfe von C# einen in LUIS (Language Understanding Intelligent Service) integrierten Chatbot. Dieser Chatbot verwendet die vordefinierte HomeAutomation-Domäne, um schnell eine Botlösung zu implementieren.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 74fec29c2e70f929b64a7a64bab2a525f4facbc6
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2018
ms.locfileid: "53438726"
---
# <a name="luis-bot-in-c-with-the-bot-framework-3x-and-the-azure-web-app-bot"></a>Der LUIS-Bot in C# wird mit Bot Framework 3.x und dem Azure Web-App-Bot erstellt.

Erstellen Sie mithilfe von C# einen in LUIS (Language Understanding Intelligent Service) integrierten Chatbot. Dieser Chatbot verwendet die vordefinierte HomeAutomation-Domäne, um schnell eine Botlösung zu implementieren. Der Bot wird mit Bot Framework 3.x und dem Azure Web-App-Bot erstellt.

## <a name="prerequisite"></a>Voraussetzung

* [LUIS-App HomeAutomation](luis-get-started-create-app.md). Die Absichten aus dieser LUIS-App sind den Dialoghandlern des Bots zugeordnet. 

## <a name="luis-homeautomation-intents"></a>Absichten der LUIS-App HomeAutomation

| Absicht | Beispieläußerung | Botfunktionalität |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Turn on the lights. | Wenn die LUIS-Absicht `HomeAutomation.TurnOn` erkannt wird, ruft der Bot den Handler für den Dialog `OnIntent` auf. In diesem Dialog rufen Sie einen IoT-Dienst auf, um ein Gerät einzuschalten und dem Benutzer mitzuteilen, dass das Gerät eingeschaltet wird. |
| HomeAutomation.TurnOff | Turn off the bedroom lights. | Wenn die LUIS-Absicht `HomeAutomation.TurnOff` erkannt wird, ruft der Bot den Handler für den Dialog `OffIntent` auf. In diesem Dialog rufen Sie einen IoT-Dienst auf, um ein Gerät auszuschalten und dem Benutzer mitzuteilen, dass das Gerät ausgeschaltet wird. |

## <a name="create-a-language-understanding-bot-with-bot-service"></a>Erstellen eines Language Understanding-Bots mit Bot Service

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Menü ganz links oben die Option **Neue Ressource erstellen** aus.

    ![Erstellen einer Ressource im Azure-Portal](./media/luis-tutorial-cscharp-web-bot/bot-service-creation.png)

2. Suchen Sie im Suchfeld nach **Web-App-Bot**. 

    ![Auswählen von Web-App-Bot als Ressourcentyp](./media/luis-tutorial-cscharp-web-bot/bot-service-selection.png)

3. Klicken Sie im Fenster des Web-App-Bots auf **Erstellen**.

4. Geben Sie unter **Botdienst** die erforderlichen Informationen ein, und klicken Sie auf **Erstellen**. Dadurch werden der Botdienst und die LUIS-App erstellt und in Azure bereitgestellt. Wenn Sie die [Vorbereitung der Spracherkennung](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) verwenden möchten, überprüfen Sie die [Regionsanforderungen](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming), bevor Sie den Bot erstellen. 
    * Legen Sie den **App-Namen** auf den Namen Ihres Bots fest. Der Name wird als Unterdomäne verwendet, wenn Ihr Bot in der Cloud bereitgestellt wird (z.B. meinnotizbot.azurewebsites.net). <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * Wählen Sie das Abonnement, die [Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), den App Service-Plan und den [Standort](https://azure.microsoft.com/regions/) aus.
    * Wählen Sie für **Botvorlage** Folgendes aus:
        * **SDK v3**
        * **C#**
        * **Sprachverständnis**
    * Wählen Sie **LUIS App Location** (Standort der LUIS-App) aus. Dies ist die [Erstellungsregion](luis-reference-regions.md), in der die App erstellt wird.
    * Aktivieren Sie das Kontrollkästchen zur Bestätigung der rechtlichen Hinweise. Die Bestimmungen der rechtlichen Hinweise werden unterhalb des Kontrollkästchens angezeigt.

    ![Botdienst](./media/luis-tutorial-cscharp-web-bot/bot-service-setting-callout-template.png)


5. Vergewissern Sie sich, dass der Botdienst bereitgestellt wurde.
    * Klicken Sie auf „Benachrichtigungen“ (Glockensymbol im oberen Bereich des Azure-Portals). Die Benachrichtigung ändert sich von **Bereitstellung gestartet** in **Bereitstellung erfolgreich**.
    * Nachdem die Benachrichtigung in **Bereitstellung erfolgreich** geändert wurde, klicken Sie auf **Zu Ressource wechseln** für diese Benachrichtigung.

> [!Note]
> In diesem Erstellungsvorgang für den Web-App-Bot wurde auch eine neue LUIS-App für Sie erstellt. Sie wurde bereits trainiert und veröffentlicht. 

## <a name="try-the-default-bot"></a>Ausprobieren des Standardbots

Vergewissern Sie sich, dass der Bot bereitgestellt wurde, indem Sie das Kontrollkästchen **Benachrichtigungen** aktivieren. Die Benachrichtigungen ändern sich von **Die Bereitstellung wird ausgeführt** in **Bereitstellung erfolgreich**. Klicken Sie auf die Schaltfläche **Go to resource** (Zur Ressource wechseln), um die Ressourcen des Bots zu öffnen.

Klicken Sie nach der Bereitstellung des Bots auf **Test in Web Chat** (Im Webchat testen), um den Bereich für den Webchat zu öffnen. Geben Sie „hello“ im Webchat ein.

  ![Testen des Bots im Webchat](./media/luis-tutorial-cscharp-web-bot/bot-service-web-chat.png)

Der Bot antwortet mit „You have reached Greeting. You said: hello“.  Durch diese Antwort wird bestätigt, dass der Bot Ihre Nachricht empfangen und an eine selbst erstellte Standard-LUIS-App übergeben hat. Diese Standard-LUIS-App erkannte die Absicht „Greeting“. Im nächsten Schritt stellen Sie eine Verbindung des Bots mit der LUIS-App, die Sie zuvor erstellt haben, anstelle der Standard-LUIS-App her.

## <a name="connect-your-luis-app-to-the-bot"></a>Verbinden der LUIS-App mit dem Bot

Öffnen Sie **Anwendungseinstellungen**, und bearbeiten Sie das Feld **LuisAppId**, sodass es die Anwendungs-ID Ihrer LUIS-App enthält. Wenn Sie Ihre LUIS-App HomeAutomation in einer anderen Region als „USA, Westen“ erstellen, müssen Sie den **LuisAPIHostName** ebenfalls ändern. Der **LuisAPIKey** ist derzeit auf Ihren Erstellungsschlüssel festgelegt. Ändern Sie dies in Ihren Endpunktschlüssel, wenn der Datenverkehr das kostenlose Kontingent überschreitet. 

  ![Aktualisieren der LUIS-App-ID in Azure](./media/luis-tutorial-cscharp-web-bot/bot-service-app-settings.png)

> [!Note]
> Wenn Ihnen die LUIS-App-ID der [HomeAutomation-App](luis-get-started-create-app.md) nicht bekannt ist, melden Sie sich mit demselben Konto, das Sie zum Anmelden bei Azure verwenden, bei der [LUIS](luis-reference-regions.md)-Website an. 
> 1. Klicken Sie auf **Meine Apps**. 
> 2. Suchen Sie die zuvor erstellte LUIS-App, die die Absichten und Entitäten aus der Domäne HomeAutomation enthält.
> 3. Suchen Sie auf der Seite **Einstellungen** für die LUIS-App die App-ID, und kopieren Sie sie. Stellen Sie sicher, dass sie [trainiert](luis-interactive-test.md) und [veröffentlicht](luis-how-to-publish-app.md) wurde. 

    > [!WARNING]
    > If you delete your app ID or LUIS key, the bot will stop working.

## <a name="modify-the-bot-code"></a>Ändern des Botcodes

1. Klicken Sie auf **Erstellen**, und klicken Sie dann auf **Open online code editor**  (Onlinecode-Editor öffnen).

   ![Open online code editor (Onlinecode-Editor öffnen)](./media/luis-tutorial-cscharp-web-bot/bot-service-build.png)

2. Klicken Sie mit der rechten Maustaste auf `build.cmd`, und wählen Sie **Run from Console** (In Konsole ausführen) aus, um die App zu erstellen. Der Dienst führt mehrere Buildschritte automatisch für Sie aus. Der Build ist abgeschlossen, wenn „Erfolgreich abgeschlossen“ ausgegeben wird.

3. Öffnen Sie im Code-Editor `/Dialogs/BasicLuisDialog.cs`. Diese Datei enthält den folgenden Code:

   [!code-csharp[Default BasicLuisDialog.cs](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/Default_BasicLuisDialog.cs "Default BasicLuisDialog.cs")]

## <a name="change-code-to-homeautomation-intents"></a>Ändern des Codes in HomeAutomation-Absichten


1. Entfernen Sie die drei Absichtsattribute und -methoden für **Greeting**, **Cancel**, und **Help**. Diese Absichten werden in der vordefinierten HomeAutomation-Domäne nicht verwendet. Stellen Sie sicher, dass die Absichtsattribute und -methoden für **None** beibehalten werden. 

2. Fügen Sie am Anfang der Datei bei den Abhängigkeiten weitere Abhängigkeiten hinzu:

   [!code-csharp[Dependencies](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=4-5&dedent=8 "dependencies")]

3. Fügen Sie Konstanten zum Verwalten von Zeichenfolgen oben in der `BasicLuisDialog `-Klasse hinzu:

   [!code-csharp[Add Intent and Entity Constants](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=23-32&dedent=8 "Add Intent and Entity Constants")]

4. Fügen Sie den Code für die neuen Absichten von `HomeAutomation.TurnOn` und `HomeAutomation.TurnOff` in der `BasicLuisDialog `-Klasse hinzu:

   [!code-csharp[Add Intents](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=61-71&dedent=8 "Add Intents")]

5. Fügen Sie den Code zum Abrufen der von LUIS gefundenen Entitäten in der `BasicLuisDialog `-Klasse hinzu:

   [!code-csharp[Collect entities](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=34-53&dedent=8 "Collect entities")]

6. Ändern Sie die **ShowLuisResult**-Methode in der `BasicLuisDialog `-Klasse, sodass die Bewertung gerundet wird, die Entitäten gesammelt werden und die Antwortnachricht im Chatbot angezeigt wird:

   [!code-csharp[Display message in chatbot](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=73-83&dedent=8 "Display message in chatbot")]

## <a name="build-the-bot"></a>Erstellen des Bots
Klicken Sie im Code-Editor mit der rechten Maustaste auf `build.cmd`, und wählen Sie **Run from Console** (In Konsole ausführen) aus.

![Erstellen des Web-Bots ](./media/luis-tutorial-cscharp-web-bot/bot-service-build-run-from-console.png)

Die Codeansicht wird durch ein Terminalfenster ersetzt, in dem der Fortschritt und die Ergebnisse des Builds angezeigt werden.

![Erfolgreiche Erstellung des Web-Bots](./media/luis-tutorial-cscharp-web-bot/bot-service-build-success.png)

> [!TIP]
> Eine alternative Methode zum Erstellen des Bots besteht darin, den Botnamen auf der oberen blaue Leiste und dann **Open Kudu Console** (Kudu-Konsole öffnen) auszuwählen. Die Konsole wird bei **D:\home** geöffnet. 
> 
> Wechseln Sie in das Verzeichnis **D:\home\site\wwwroot**, indem Sie Folgendes eingeben: `cd site\wwwroot`
>
> Führen Sie das Buildskript aus, indem Sie Folgendes eingeben: `build.cmd`

## <a name="test-the-bot"></a>Testen des Bots

Klicken Sie im Azure-Portal auf **Test in Web Chat** (Im Webchat testen), um den Bot zu testen. Geben Sie Nachrichten wie „Turn on the lights“ oder „turn off my heater“ ein, um die hinzugefügten Absichten aufzurufen.

   ![Testen des Bots HomeAutomation im Webchat](./media/luis-tutorial-cscharp-web-bot/bot-service-chat-results.png)

> [!TIP]
> Sie können Ihre LUIS-App erneut trainieren, ohne Änderungen am Code Ihres Bots vorzunehmen. Weitere Informationen finden Sie unter [Hinzufügen von Beispieläußerungen](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) und [Trainieren und Testen der LUIS-App](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-interactive-test). 

## <a name="download-the-bot-to-debug"></a>Herunterladen des Bots zum Debuggen
Wenn Ihr Bot nicht funktioniert, laden Sie das Projekt auf Ihren lokalen Computer herunter, und fahren Sie mit dem [Debuggen](https://docs.microsoft.com/bot-framework/bot-service-debug-bot#debug-a-c-bot) fort. 

## <a name="learn-more-about-bot-framework"></a>Weitere Informationen über das Bot-Framework
Erfahren Sie mehr über das [Bot-Framework](https://dev.botframework.com/) und die SDKs der Versionen [3.x](https://github.com/Microsoft/BotBuilder) und [4.x](https://github.com/Microsoft/botbuilder-dotnet).

## <a name="next-steps"></a>Nächste Schritte

Fügen Sie die LUIS-Absichten und die Dialoge des Botdiensts zum Behandeln der Absichten **Help**, **Cancel**, und **Greeting** hinzu. Denken Sie daran, den Web-App-Bot zu trainieren, zu veröffentlichen und zu [erstellen](#build-the-bot). LUIS und der Bot sollten die gleichen Absichten aufweisen.

> [!div class="nextstepaction"]
> [Hinzufügen von Absichten](./luis-how-to-add-intents.md)
> [Aktivieren der Spracherkennung](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)


<!-- Links -->
[GitHub-BotFramework-Emulator-Download]: https://aka.ms/bot-framework-emulator
[GitHub-LUIS-Samples]: https://github.com/Microsoft/LUIS-Samples
[GitHub-LUIS-Samples-cs-hotel-bot]: https://github.com/Microsoft/LUIS-Samples/tree/master/bot-integration-samples/hotel-finder/csharp
[GitHub-LUIS-Samples-cs-hotel-bot-readme]: https://github.com/Microsoft/LUIS-Samples/blob/master/bot-integration-samples/hotel-finder/csharp/README.md
[BFPortal]: https://dev.botframework.com/
[RegisterInstructions]: https://docs.microsoft.com/bot-framework/portal-register-bot
[BotFramework]: https://docs.microsoft.com/bot-framework/
[VisualStudio]: https://www.visualstudio.com/

<!-- tested on Win10 -->
