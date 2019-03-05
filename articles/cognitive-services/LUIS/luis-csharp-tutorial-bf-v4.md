---
title: 'Bot: C# (v4)'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Erstellen Sie mithilfe von C# einen in LUIS (Language Understanding Intelligent Service) integrierten Chatbot. Dieser Chatbot implementiert mithilfe der Human Resources-App schnell eine Bot-Lösung. Der Bot wird mit Bot Framework, Version 4, und dem Azure Web-App-Bot erstellt.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: 028c06924e41606ba1d4e0b15fe26f2b7270db3c
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960297"
---
# <a name="tutorial-luis-bot-in-c-with-the-bot-framework-4x-and-the-azure-web-app-bot"></a>Tutorial: Der LUIS-Bot in C# wird mit Bot Framework 4.x und dem Azure Web-App-Bot erstellt.
Mithilfe von C# können Sie einen in LUIS (Language Understanding Intelligent Service) integrierten Chatbot erstellen. Diesen Bot verwendet die HomeAutomation-App, um eine Bot-Lösung zu implementieren. Der Bot wird im Azure [Web-App-Bot](https://docs.microsoft.com/azure/bot-service/) mit [Bot Framework, Version](https://github.com/Microsoft/botbuilder-js) v4, erstellt.

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Erstellen eines Web-App-Bots. In diesem Vorgang wird eine neue LUIS-App erstellt.
> * Hinzufügen einer vordefinierten Domäne zum neuen LUIS-Modell
> * Herunterladen des vom Webbotdienst erstellten Projekts
> * Lokales Starten von Bot und Emulator auf dem eigenen Computer
> * Ändern des Bot-Codes für neue LUIS-Absichten
> * Anzeigen von Äußerungsergebnissen im Bot

## <a name="prerequisites"></a>Voraussetzungen

* [Bot-Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)


## <a name="create-web-app-bot"></a>Erstellen des Web-App-Bots

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) **Neue Ressource erstellen** aus.

2. Suchen Sie im Suchfeld nach **Web-App-Bot**, und wählen Sie den Eintrag aus. Klicken Sie auf **Erstellen**.

3. Geben Sie in **Botdienst** die erforderlichen Informationen ein:

    |Einstellung|Zweck|Empfohlene Einstellung|
    |--|--|--|
    |Botname|Ressourcenname|`luis-csharp-bot-` + `<your-name>`, z.B. `luis-csharp-bot-johnsmith`|
    |Abonnement|Abonnement, in dem der Bot erstellt werden soll.|Ihr primäres Abonnement.
    |Ressourcengruppe|Logische Gruppe von Azure-Ressourcen|Erstellen einer neuen Gruppe zum Speichern aller Ressourcen, die mit diesen Bot verwendet werden; benennen Sie die Gruppe `luis-csharp-bot-resource-group`.|
    |Standort|Azure-Region: muss nicht die gleiche wie die Erstellungs- oder Veröffentlichungsregion von LUIS sein.|`westus`|
    |Tarif|Für die Grenzwerte der Dienstanforderungen und die Abrechnung verwendet.|`F0` ist der kostenlose Tarif.
    |App-Name|Der Name wird als Unterdomäne verwendet, wenn Ihr Bot in der Cloud bereitgestellt wird (z.B. personalwesenbot.azurewebsites.net).|`luis-csharp-bot-` + `<your-name>`, z.B. `luis-csharp-bot-johnsmith`|
    |Botvorlage|Einstellungen für Bot Framework – siehe nächste Tabelle|
    |LUIS-App-Speicherort|Muss mit der LUIS-Ressourcenregion übereinstimmen|`westus`|

4. Wählen Sie in den **Bot-Vorlageneinstellungen** die folgenden Werte aus, und wählen Sie dann unterhalb dieser Einstellungen die Schaltfläche **Auswählen** aus:

    |Einstellung|Zweck|Auswahl|
    |--|--|--|
    |SDK-Version|Bot-Framework-Version|**SDK v4**|
    |SDK-Sprache|Programmiersprache des Bots|**C#**|
    |Echo-/Basisbot|Bottyp|**Basisbot**|
    
5. Klicken Sie auf **Erstellen**. Dadurch wird der Botdienst erstellt und in Azure bereitgestellt. Im Rahmen dieses Vorgangs wird eine LUIS-App mit dem Namen `luis-csharp-bot-XXXX` erstellt. Dieser Name basiert auf den Bot- und App-Namen im vorherigen Abschnitt.

    [![Erstellen des Web-App-Bots](./media/bfv4-csharp/create-web-app-service.png)](./media/bfv4-csharp/create-web-app-service.png#lightbox)

6. Lassen Sie diese Browserregisterkarte geöffnet. Öffnen Sie für alle Schritte im LUIS-Portal eine neue Browserregisterkarte. Fahren Sie mit dem nächsten Abschnitt fort, wenn der neue Botdienst bereitgestellt wurde.

## <a name="add-prebuilt-domain-to-model"></a>Hinzufügen einer vordefinierten Domäne zum Modell
Im Rahmen der Botdienstbereitstellung wird eine neue LUIS-App mit Absichten und Beispieläußerungen erstellt. Der Bot stellt die Zuordnung der Absichten zur neuen LUIS-App für die folgenden Absichten bereit: 

|LUIS-Absichten für Basisbots|Beispieläußerung|
|--|--|
|Abbrechen|`stop`|
|Grußformel|`hello`|
|Hilfe|`help`|
|Keine|Alles, was sich außerhalb der Domäne der App befindet.|

Fügen Sie dem Modell die vordefinierte HomeAutomation-App hinzu, um Äußerungen dieser Art zu verarbeiten: `Turn off the living room lights`

1. Navigieren Sie zum [LUIS](https://www.luis.ai)-Portal, und melden Sie sich an.
2. Wählen Sie auf der Seite **Meine Apps** die Spalte **Erstellungsdatum** aus, um nach dem Erstellungsdatum der App zu sortieren. Der Azure Bot Service hat im vorherigen Abschnitt eine neue App erstellt. Der Name ist `luis-csharp-bot-`  +  `<your-name>` + 4 zufällige Zeichen.
3. Öffnen Sie die App, und wählen Sie in der oberen Navigationsleiste den Abschnitt **Erstellen** aus.
4. Wählen Sie im linken Navigationsbereich **Vordefinierte Domänen** aus.
5. Wählen Sie die Domäne **HomeAutomation** aus, indem Sie auf ihrer Karte **Domäne hinzufügen** auswählen.
6. Wählen Sie im oberen Kontextmenü **Train** (Trainieren) aus.
7. Wählen Sie im oberen Kontextmenü **Veröffentlichen** aus. 

    Die vom Azure Bot Service erstellte App verfügt nun über zwei Absichten:

    |Neue Absichten des Basisbots|Beispieläußerung|
    |--|--|
    |HomeAutomation.TurnOn|`turn the fan to high`
    |HomeAutomation.TurnOff|`turn off ac please`|

## <a name="download-the-web-app-bot"></a>Herunterladen des Web-App-Bots 
Um den Code des Web-App-Bots zu entwickeln, laden Sie den Code herunter, und verwenden Sie ihn auf Ihrem lokalen Computer. 

1. Wählen Sie im Azure-Portal, immer noch auf der Web-App-Bot-Ressource die **Anwendungseinstellungen** aus, und kopieren Sie die Werte von **botFilePath** und **botFileSecret**. Sie müssen diese später einer Umgebungsdatei hinzufügen. 

2. Wählen Sie im Azure-Portal im Abschnitt **Bot Management** (Botverwaltung) **Erstellen** aus. 

3. Wählen Sie **Bot-Quellcode herunterladen** aus. 

    [![Herunterladen des Web-App-Bot-Quellcodes für den Basisbot](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

4. Wenn der Quellcode gezippt ist, wird in einer Meldung ein Link zum Herunterladen des Codes zur Verfügung gestellt. Wählen Sie den Link aus. 

5. Speichern Sie die ZIP-Datei auf Ihrem lokalen Computer, und extrahieren Sie die Dateien. Öffnen Sie das Projekt. 

6. Öffnen Sie die bot.cs-Datei, und suchen Sie nach `_services.LuisServices`. Das ist die Stelle, an der in den Bot eingegebene Benutzeräußerungen an LUIS gesendet werden.

    ```csharp
    /// <summary>
    /// Run every turn of the conversation. Handles orchestration of messages.
    /// </summary>
    /// <param name="turnContext">Bot Turn Context.</param>
    /// <param name="cancellationToken">Task CancellationToken.</param>
    /// <returns>A <see cref="Task"/> representing the asynchronous operation.</returns>
    public async Task OnTurnAsync(ITurnContext turnContext, CancellationToken cancellationToken)
    {
        var activity = turnContext.Activity;

        if (activity.Type == ActivityTypes.Message)
        {
            // Perform a call to LUIS to retrieve results for the current activity message.
            var luisResults = await _services.LuisServices[LuisConfiguration].RecognizeAsync(turnContext, cancellationToken).ConfigureAwait(false);

            // If any entities were updated, treat as interruption.
            // For example, "no my name is tony" will manifest as an update of the name to be "tony".
            var topScoringIntent = luisResults?.GetTopScoringIntent();

            var topIntent = topScoringIntent.Value.intent;
            switch (topIntent)
            {
                case GreetingIntent:
                    await turnContext.SendActivityAsync("Hello.");
                    break;
                case HelpIntent:
                    await turnContext.SendActivityAsync("Let me try to provide some help.");
                    await turnContext.SendActivityAsync("I understand greetings, being asked for help, or being asked to cancel what I am doing.");
                    break;
                case CancelIntent:
                    await turnContext.SendActivityAsync("I have nothing to cancel.");
                    break;
                case NoneIntent:
                default:
                    // Help or no intent identified, either way, let's provide some help.
                    // to the user
                    await turnContext.SendActivityAsync("I didn't understand what you just said to me.");
                    break;
            }
        }
        else if (activity.Type == ActivityTypes.ConversationUpdate)
        {
            if (activity.MembersAdded.Any())
            {
                // Iterate over all new members added to the conversation.
                foreach (var member in activity.MembersAdded)
                {
                    // Greet anyone that was not the target (recipient) of this message.
                    // To learn more about Adaptive Cards, see https://aka.ms/msbot-adaptivecards for more details.
                    if (member.Id != activity.Recipient.Id)
                    {
                        var welcomeCard = CreateAdaptiveCardAttachment();
                        var response = CreateResponse(activity, welcomeCard);
                        await turnContext.SendActivityAsync(response).ConfigureAwait(false);
                    }
                }
            }
        }

    }
    ```

    Der Bot sendet die Äußerungen des Benutzers an LUIS und ruft die Ergebnisse ab. Die oberste Absicht bestimmt den Ablauf der Unterhaltung. 


## <a name="start-the-bot"></a>Starten des Bots
Bevor Sie Code oder Einstellungen ändern, überprüfen Sie, ob der Bot funktioniert. 

1. Öffnen Sie die Projektmappendatei in Visual Studio. 

2. Erstellen Sie eine `appsettings.json`-Datei zur Aufnahme Bot-Variablen, nach denen der Code des Bots sucht:

    ```JSON
    {
    "botFileSecret": "",
    "botFilePath": ""

    }
    ```

    Legen Sie die Werte der Variablen auf die Werte fest, die Sie aus den Anwendungseinstellungen des Azure Bot Services in Schritt 1 im Abschnitt **[Herunterladen des Web-App-Bots](#download-the-web-app-bot)** kopiert haben.

3. Starten Sie den Bot in Visual Studio. Ein Browserfenster mit der Website des Web-App-Bots wird unter der Adresse `http://localhost:3978/` geöffnet.

## <a name="start-the-emulator"></a>Starten des Emulators

1. Öffnen Sie den Bot-Emulator.

2. Wählen Sie im Bot-Emulator die *.bot-Datei im Stammverzeichnis des Projekts aus. Diese `.bot`-Datei enthält die Endpunkt-URL des Bots für Nachrichten:

    [![Bot-Emulator v4](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png)](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png#lightbox)

3. Geben Sie das Bot-Geheimnis ein, das Sie aus den Anwendungseinstellungen des Azure Bot Services in Schritt 1 im Abschnitt **[Herunterladen des Web-App-Bots](#download-the-web-app-bot)** kopiert haben. Dies ermöglicht es dem Emulator, auf die verschlüsselten Felder in der `.bot`-Datei zuzugreifen.

    ![Geheimnis in Bot-Emulator v4](../../../includes/media/cognitive-services-luis/bfv4/bot-secret.png)

4. Geben Sie im Bot-Emulator `Hello` ein, und rufen Sie die richtige Antwort für den Basisbot ab.

    [![Antwort des Basisbots im Emulator](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png)](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png#lightbox)

## <a name="modify-bot-code"></a>Ändern des Bot-Codes 

Fügen Sie in der Datei `BasicBot.cs` Code zum Verarbeiten der neuen Absichten ein. 

1. Suchen Sie oben in der Datei den Abschnitt **Unterstützte LUIS-Absichten**, und fügen Sie Konstanten für die HomeAutomation-Absichten ein:

    ```csharp
    // Supported LUIS Intents
    public const string GreetingIntent = "Greeting";
    public const string CancelIntent = "Cancel";
    public const string HelpIntent = "Help";
    public const string NoneIntent = "None";
    public const string TurnOnIntent = "HomeAutomation_TurnOn"; // new intent
    public const string TurnOffIntent = "HomeAutomation_TurnOff"; // new intent
    ```

    Beachten Sie, dass der Punkt `.` zwischen der Domäne und der Absicht aus der App des LUIS-Portals durch einen Unterstrich `_` ersetzt wird. 

2. Suchen Sie die Methode **OnTurnAsync**, die die LUIS-Vorhersage der Äußerung empfängt. Fügen Sie Code in der Switch-Anweisung hinzu, um die LUIS-Antwort für die zwei HomeAutomation-Absichten zurückzugeben. 

    ```csharp
    case TurnOnIntent:
        await turnContext.SendActivityAsync("TurnOn intent found, JSON response: " + luisResults?.Entities.ToString());
        break;
    case TurnOffIntent:
        await turnContext.SendActivityAsync("TurnOff intent found, JSON response: " + luisResults?.Entities.ToString());
        break;
    ```

    Der Bot weist nicht genau die gleiche Reaktion wie eine LUIS-REST-API-Anforderung auf, daher ist es wichtig, die Unterschiede durch Ansehen des Antwort-JSONs kennenzulernen. Die Eigenschaften für Text und Absichten sind die gleichen, aber die Eigenschaftswerte der Entitäten wurden geändert. 

    ```JSON
    {
        "$instance": {
            "HomeAutomation_Device": [
                {
                    "startIndex": 23,
                    "endIndex": 29,
                    "score": 0.9776345,
                    "text": "lights",
                    "type": "HomeAutomation.Device"
                }
            ],
            "HomeAutomation_Room": [
                {
                    "startIndex": 12,
                    "endIndex": 22,
                    "score": 0.9079433,
                    "text": "livingroom",
                    "type": "HomeAutomation.Room"
                }
            ]
        },
        "HomeAutomation_Device": [
            "lights"
        ],
        "HomeAutomation_Room": [
            "livingroom"
        ]
    }
    ```



## <a name="view-results-in-bot"></a>Anzeigen der Ergebnisse im Bot

1. Geben Sie im Bot-Emulator die Äußerung `Turn on the livingroom lights to 50%` ein.

2. Der Bot antwortet mit:

    ```JSON
    TurnOn intent found, JSON response: {"$instance":{“HomeAutomation_Device”:[{“startIndex”:23,“endIndex”:29,“score”:0.9776345,“text”:“lights”,“type”:“HomeAutomation.Device”}],“HomeAutomation_Room”:[{“startIndex”:12,“endIndex”:22,“score”:0.9079433,“text”:“livingroom”,“type”:“HomeAutomation.Room”}]},“HomeAutomation_Device”:[“lights”],“HomeAutomation_Room”:[“livingroom”]}
    ```    

## <a name="learn-more-about-bot-framework"></a>Weitere Informationen über das Bot-Framework
Azure Bot Service verwendet das Bot Framework SDK. Weitere Informationen zum SDK und zu Bot Framework:

* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4-Dokumentation
* [Bot Builder-Beispiele](https://github.com/Microsoft/botbuilder-samples)
* [Bot Builder SDK](https://docs.microsoft.com/javascript/api/botbuilder-core/?view=botbuilder-ts-latest)
* [Bot Builder-Tools](https://github.com/Microsoft/botbuilder-tools):

## <a name="next-steps"></a>Nächste Schritte

Sie haben einen Azure Bot Service erstellt, den geheimen Schlüssel des Bots und den `.bot`-Dateipfad kopiert sowie die ZIP-Datei des Codes heruntergeladen. Sie haben die vordefinierte HomeAutomation-Domäne zu der LUIS-App hinzugefügt, die im Rahmen des neuen Azure Bot Services erstellt wurde, die App dann trainiert und sie erneut veröffentlicht. Sie haben das Codeprojekt extrahiert, eine Umgebungsdatei (`.env`) erstellt und den geheimen Schlüssel des Bots sowie den `.bot`-Dateipfad festgelegt. In der bot.js-Datei haben Sie Code zum Verarbeiten der zwei neuen Absichten hinzugefügt. Dann haben Sie den Bot im Bot-Emulator getestet, um die Antwort von LUIS auf eine der neuen Absichten zu sehen. 

Sehen Sie sich weitere [Beispiele](https://github.com/Microsoft/AI) mit Konversationsbots an. 

> [!div class="nextstepaction"]
> [Erstellen einer benutzerdefinierten Domäne in LUIS](luis-quickstart-intents-only.md)
