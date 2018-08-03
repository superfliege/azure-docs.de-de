---
title: Integrieren eines Bots in LUIS mit dem Bot Builder SDK für Node.js in Azure | Microsoft-Dokumentation
description: Erstellen Sie mithilfe des Bot-Frameworks einen Bot, der in eine LUIS-Anwendung integriert ist.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/06/2018
ms.author: diberry
ms.openlocfilehash: 6d6937105b11d94138b51660dc9f3c5e682e19bc
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224074"
---
# <a name="integrate-luis-with-a-bot-using-the-bot-builder-sdk-for-nodejs"></a>Integrieren eines Bots in LUIS mit dem Bot Builder SDK für Node.js

Dieses Tutorial führt Sie durch das Erstellen eines Bots, der in eine LUIS-App integriert ist, mit dem [Bot-Framework][BotFramework].

## <a name="prerequisite"></a>Voraussetzung

Bevor Sie den Bot erstellen, befolgen Sie die Schritte in [Erstellen einer App](./luis-get-started-create-app.md), um die verwendete LUIS-App zu erstellen.

Der Bot antwortet auf Absichten aus der Domäne HomeAutomation, die in der LUIS-App enthalten sind. Für jede dieser Absichten bietet die LUIS-App eine Absicht, die ihr zugeordnet ist. Der Bot stellt einen Dialog bereit, der die von LUIS erkannte Absicht behandelt.

| Absicht | Beispieläußerung | Botfunktionalität |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Turn on the lights. | Der Bot ruft `TurnOnDialog` auf, wenn `HomeAutomation.TurnOn` erkannt wird. In diesem Dialog rufen Sie einen IoT-Dienst auf, um ein Gerät einzuschalten und dem Benutzer mitzuteilen, dass das Gerät eingeschaltet wird. |
| HomeAutomation.TurnOff | Turn off the bedroom lights. | Der Bot ruft `TurnOffDialog` auf, wenn `HomeAutomation.TurnOff` erkannt wird. In diesem Dialog rufen Sie einen IoT-Dienst auf, um ein Gerät auszuschalten und dem Benutzer mitzuteilen, dass das Gerät ausgeschaltet wird. |


## <a name="create-a-language-understanding-bot-with-bot-service"></a>Erstellen eines Language Understanding-Bots mit Bot Service

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) auf dem Menüblatt die Option **Neue Ressource erstellen** und dann **Alle anzeigen** aus.

    ![Neue Ressource erstellen](./media/luis-tutorial-node-bot/bot-service-creation.png)

2. Suchen Sie im Suchfeld nach **Web-App-Bot**. 

    ![Neue Ressource erstellen](./media/luis-tutorial-node-bot/bot-service-selection.png)

3. Geben Sie auf dem Blatt **Bot Service** die erforderlichen Informationen an, und wählen Sie **Erstellen** aus. Dadurch werden der Botdienst und die LUIS-App erstellt und in Azure bereitgestellt. Wenn Sie die [Vorbereitung der Spracherkennung](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) verwenden möchten, überprüfen Sie die [Regionsanforderungen](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming), bevor Sie den Bot erstellen. 
    * Legen Sie den **App-Namen** auf den Namen Ihres Bots fest. Der Name wird als Unterdomäne verwendet, wenn Ihr Bot in der Cloud bereitgestellt wird (z.B. meinnotizbot.azurewebsites.net). <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * Wählen Sie das Abonnement, die [Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), den App Service-Plan und den [Standort](https://azure.microsoft.com/regions/) aus.
    * Wählen Sie die Vorlage **Language Understanding (Node.js)** im Feld **Bot template** (Botvorlage) aus.
    * Wählen Sie **LUIS App Location** (Standort der LUIS-App) aus. Dies ist die [Erstellungsregion][LUIS], in der die App erstellt wird.
    * Aktivieren Sie das Kontrollkästchen zur Bestätigung der rechtlichen Hinweise. Die Bestimmungen der rechtlichen Hinweise werden unterhalb des Kontrollkästchens angezeigt.

    ![Blatt „Bot Service“](./media/luis-tutorial-node-bot/bot-service-setting-callout-template.png)


4. Vergewissern Sie sich, dass der Botdienst bereitgestellt wurde.
    * Wählen Sie „Benachrichtigungen“ (Glockensymbol im oberen Bereich des Azure-Portals) aus. Die Benachrichtigung ändert sich von **Bereitstellung gestartet** in **Bereitstellung erfolgreich**.
    * Nachdem die Benachrichtigung in **Bereitstellung erfolgreich** geändert wurde, wählen Sie **Zu Ressource wechseln** für diese Benachrichtigung aus.

## <a name="try-the-default-bot"></a>Ausprobieren des Standardbots

Vergewissern Sie sich, dass der Bot bereitgestellt wurde, indem Sie das Kontrollkästchen **Benachrichtigungen** aktivieren. Die Benachrichtigungen ändern sich von **Die Bereitstellung wird ausgeführt** in **Bereitstellung erfolgreich**. Wählen Sie die Schaltfläche **Go to resource** (Zur Ressource wechseln) aus, um das Ressourcenblatt des Bots zu öffnen.

<!-- this step isn't supposed to be necessary -->
## <a name="install-npm-resources"></a>Installieren von NPM-Ressourcen
Installieren Sie NPM-Pakete mit den folgenden Schritten:

1. Wählen Sie im Bereich **Bot Management** (Botverwaltung) des Web-App-Bots **Erstellen** aus. 

2. Eine neues zweites Browserfenster wird geöffnet. Wählen Sie **Open online code editor** (Onlinecode-Editor öffnen) aus.

3. Wählen Sie in der oberen Navigationsleiste den Namen des Web-App-Bots `homeautomationluisbot` aus. 

4. Wählen Sie in der Dropdownliste **Open Kudu Console** (Kudu-Konsole öffnen) aus.

5. Ein neues Browserfenster wird geöffnet. Geben Sie in der Konsole den folgenden Befehl ein:

    ```
    cd site\wwwroot && npm install
    ```

    Warten Sie, bis der Installationsvorgang abgeschlossen ist. Kehren Sie zum ersten Browserfenster zurück. 

## <a name="test-in-web-chat"></a>Testen im Webchat
Wählen Sie nach der Registrierung des Bots **Test in Web Chat** (Im Webchat testen) aus, um den Bereich für den Webchat zu öffnen. Geben Sie „hello“ im Webchat ein.

  ![Testen des Bots im Webchat](./media/luis-tutorial-node-bot/bot-service-web-chat.png)

Der Bot antwortet mit „You have reached Greeting. You said: hello“. Dadurch wird bestätigt, dass der Bot Ihre Nachricht empfangen und an eine selbst erstellte Standard-LUIS-App übergeben hat. Diese Standard-LUIS-App erkannte die Absicht „Greeting“. Im nächsten Schritt stellen Sie eine Verbindung des Bots mit der LUIS-App, die Sie zuvor erstellt haben, anstelle der Standard-LUIS-App her.

## <a name="connect-your-luis-app-to-the-bot"></a>Verbinden der LUIS-App mit dem Bot

Öffnen Sie im ersten Browserfenster **Anwendungseinstellungen**, und bearbeiten Sie das Feld **LuisAppId**, sodass es die Anwendungs-ID Ihrer LUIS-App enthält.

  ![Aktualisieren der LUIS-App-ID in Azure](./media/luis-tutorial-node-bot/bot-service-app-id.png)

Wenn Sie keine LUIS-App-ID haben, melden Sie sich mit demselben Konto, das Sie zum Anmelden bei Azure verwenden, bei der [LUIS](luis-reference-regions.md)-Website an. Wählen Sie **Meine Apps** aus. 

1. Suchen Sie die zuvor erstellte LUIS-App, die die Absichten und Entitäten aus der Domäne HomeAutomation enthält.

2. Suchen Sie auf der Seite **Einstellungen** für die LUIS-App die App-ID, und kopieren Sie sie.

3. Wenn Sie die App noch nicht trainiert haben, wählen Sie die Schaltfläche **Trainieren** in der rechten oberen Ecke aus, um Ihre App zu trainieren.

4. Wenn Sie die App noch nicht veröffentlicht haben, wählen Sie auf der oberen Navigationsleiste **Veröffentlichen** aus, um die Seite **Veröffentlichen** zu öffnen. Wählen Sie den Produktionsslot und dann die Schaltfläche **Publish** (Veröffentlichen) aus.

## <a name="modify-the-bot-code"></a>Ändern des Botcodes

Wechseln Sie zum zweiten Browserfenster, wenn es noch geöffnet ist, oder verwendet Sie das erste Browserfenster. Wählen Sie **Erstellen** und dann **Open online code editor** (Onlinecode-Editor öffnen) aus.

   ![Open online code editor (Onlinecode-Editor öffnen)](./media/luis-tutorial-node-bot/bot-service-build.png)

Öffnen Sie im Code-Editor `app.js`. Diese Datei enthält den folgenden Code:

```javascript
/*-----------------------------------------------------------------------------
A simple Language Understanding (LUIS) bot for the Microsoft Bot Framework. 
-----------------------------------------------------------------------------*/

var restify = require('restify');
var builder = require('botbuilder');
var botbuilder_azure = require("botbuilder-azure");

// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});
  
// Create chat connector for communicating with the Bot Framework Service
var connector = new builder.ChatConnector({
    appId: process.env.MicrosoftAppId,
    appPassword: process.env.MicrosoftAppPassword,
    openIdMetadata: process.env.BotOpenIdMetadata 
});

// Listen for messages from users 
server.post('/api/messages', connector.listen());

/*----------------------------------------------------------------------------------------
* Bot Storage: This is a great spot to register the private state storage for your bot. 
* We provide adapters for Azure Table, CosmosDb, SQL Azure, or you can implement your own!
* For samples and documentation, see: https://github.com/Microsoft/BotBuilder-Azure
* ---------------------------------------------------------------------------------------- */

var tableName = 'botdata';
var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

// Create your bot with a function to receive messages from the user
// This default message handler is invoked if the user's utterance doesn't
// match any intents handled by other dialogs.
var bot = new builder.UniversalBot(connector, function (session, args) {
    session.send('You reached the default message handler. You said \'%s\'.', session.message.text);
});

bot.set('storage', tableStorage);

// Make sure you add code to validate these fields
var luisAppId = process.env.LuisAppId;
var luisAPIKey = process.env.LuisAPIKey;
var luisAPIHostName = process.env.LuisAPIHostName || 'westus.api.cognitive.microsoft.com';

const LuisModelUrl = 'https://' + luisAPIHostName + '/luis/v2.0/apps/' + luisAppId + '?subscription-key=' + luisAPIKey;

// Create a recognizer that gets intents from LUIS, and add it to the bot
var recognizer = new builder.LuisRecognizer(LuisModelUrl);
bot.recognizer(recognizer);

// Add a dialog for each intent that the LUIS app recognizes.
// See https://docs.microsoft.com/bot-framework/nodejs/bot-builder-nodejs-recognize-intent-luis 
bot.dialog('GreetingDialog',
    (session) => {
        session.send('You reached the Greeting intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Greeting'
})

bot.dialog('HelpDialog',
    (session) => {
        session.send('You reached the Help intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Help'
})

bot.dialog('CancelDialog',
    (session) => {
        session.send('You reached the Cancel intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Cancel'
})
```

Die vorhandenen Absichten in „App.js“ werden ignoriert. Sie können diese unverändert lassen. 

## <a name="add-a-dialog-that-matches-homeautomationturnon"></a>Hinzufügen eines Dialogs, der mit HomeAutomation.TurnOn übereinstimmt

Kopieren Sie den folgenden Code, und fügen Sie ihn `app.js` hinzu.

```javascript
bot.dialog('TurnOn',
    (session) => {
        session.send('You reached the TurnOn intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'HomeAutomation.TurnOn'
})
```

Die Option [matches][matches] unter [triggerAction][triggerAction] für den Dialog gibt den Namen der Absicht an. Die Erkennung wird jedes Mal ausgeführt, wenn der Bot eine Äußerung vom Benutzer erhält. Wenn der erkannte Absicht mit der höchsten Bewertung mit einer `triggerAction` für den Dialog übereinstimmt, ruft der Bot diesen Dialog auf.

## <a name="add-a-dialog-that-matches-homeautomationturnoff"></a>Hinzufügen eines Dialogs, der mit HomeAutomation.TurnOff übereinstimmt

Kopieren Sie den folgenden Code, und fügen Sie ihn `app.js` hinzu.

```javascript
bot.dialog('TurnOff',
    (session) => {
        session.send('You reached the TurnOff intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'HomeAutomation.TurnOff'
})
```
## <a name="test-the-bot"></a>Testen des Bots

Wählen Sie im Azure-Portal **Test in Web Chat** (Im Webchat testen) aus, um den Bot zu testen. Geben Sie Nachrichten wie „Turn on the lights“ oder „turn off my heater“ ein, um die hinzugefügten Absichten aufzurufen.
   ![Testen des Bots HomeAutomation im Webchat](./media/luis-tutorial-node-bot/bot-service-chat-results.png)

> [!TIP]
> Wenn Sie feststellen, dass Ihr Bot nicht immer die richtige Absicht bzw. die richtigen Entitäten erkennt, verbessern Sie die Leistung Ihrer LUIS-App, indem Sie sie mit weiteren Beispieläußerungen trainieren. Sie können Ihre LUIS-App erneut trainieren, ohne Änderungen am Code Ihres Bots vorzunehmen. Weitere Informationen finden Sie unter [Hinzufügen von Beispieläußerungen](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) und [Trainieren und Testen der LUIS-App](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-interactive-test).

## <a name="learn-more-about-bot-framework"></a>Weitere Informationen über das Bot-Framework
Erfahren Sie mehr über das [Bot-Framework](https://dev.botframework.com/) und die SDKs der Versionen [3.x](https://github.com/Microsoft/BotBuilder) und [4.x](https://github.com/Microsoft/botbuilder-js).

## <a name="next-steps"></a>Nächste Schritte

<!-- From trying the bot, you can see that the recognizer can trigger interruption of the currently active dialog. Allowing and handling interruptions is a flexible design that accounts for what users really do. Learn more about the various actions you can associate with a recognized intent.--> Sie können versuchen, der LUIS-App weitere Absichten hinzuzufügen, z.B. „Help“, „Cancel“ und „Greeting“. Fügen Sie dann Dialoge für die neuen Absichten hinzu, und testen Sie sie mit dem Bot. 

<!-- 
> [!NOTE] 
> The default LUIS app that the template created contains example utterances for Cancel, Greeting, and Help intents. In the list of apps, find the app that begins with the name specified in **App name** in the **Bot Service** blade when you created the Bot Service. -->

> [!div class="nextstepaction"]
> [Hinzufügen von Absichten](./luis-how-to-add-intents.md)
> [Aktivieren der Spracherkennung](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)


[intentDialog]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html

[intentDialog_matches]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html#matches 

[NotesSample]: https://github.com/Microsoft/BotFramework-Samples/tree/master/docs-samples/Node/basics-naturalLanguage

[triggerAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction

[confirmPrompt]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#confirmprompt

[waterfall]: bot-builder-nodejs-dialog-manage-conversation-flow.md#manage-conversation-flow-with-a-waterfall

[session_userData]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.session.html#userdata

[EntityRecognizer_findEntity]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.entityrecognizer.html#findentity

[matches]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#matches

[LUISAzureDocs]: https://docs.microsoft.com/azure/cognitive-services/LUIS/Home

[Dialog]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html

[IntentRecognizerSetOptions]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.iintentrecognizersetoptions.html

[LuisRecognizer]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.luisrecognizer

[LUISConcepts]: https://docs.botframework.com/node/builder/guides/understanding-natural-language/

[DisambiguationSample]: https://github.com/Microsoft/BotBuilder/tree/master/Node/examples/feature-onDisambiguateRoute

[IDisambiguateRouteHandler]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.idisambiguateroutehandler.html

[RegExpRecognizer]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.regexprecognizer.html

[AlarmBot]: https://github.com/Microsoft/BotBuilder/blob/master/Node/examples/basics-naturalLanguage/app.js

[LUISBotSample]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-LUIS

[UniversalBot]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.universalbot.html


<!-- Old Links -->
[Github-BotFramework-Emulator-Download]: https://aka.ms/bot-framework-emulator
[Github-LUIS-Samples]: https://github.com/Microsoft/LUIS-Samples
[Github-LUIS-Samples-node-hotel-bot]: https://github.com/Microsoft/LUIS-Samples/tree/master/bot-integration-samples/hotel-finder/nodejs
[NodeJs]: https://nodejs.org/
[BFPortal]: https://dev.botframework.com/
[RegisterInstructions]: https://docs.microsoft.com/bot-framework/portal-register-bot
[BotFramework]: https://docs.microsoft.com/bot-framework/
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions

