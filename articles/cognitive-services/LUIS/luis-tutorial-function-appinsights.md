---
title: Application Insights-Daten aus LUIS mit Node.js
titleSuffix: Azure Cognitive Services
description: Erstellen Sie mit Node.js einen Bot, der in eine LUIS-Anwendung und Application Insights integriert ist.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 6199e4a681f7f58ea0cf57b575afb2a63d160eee
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321953"
---
# <a name="add-luis-results-to-application-insights"></a>Hinzufügen von LUIS-Ergebnissen zu Application Insights
In diesem Tutorial werden [Application Insights](https://azure.microsoft.com/services/application-insights/)-Telemetriedatenspeichern Informationen aus LUIS-Anforderungen und -Antworten hinzugefügt. Sobald Sie über diese Daten verfügen, können Sie sie mit der Sprache Kusto oder mit Power BI abfragen, um Absichten und Entitäten einer Äußerung in Echtzeit zu analysieren, zu aggregieren und Berichte dazu zu erstellen. Diese Analyse hilft Ihnen dabei, zu ermitteln, ob Sie die Absichten und Entitäten aus Ihrer LUIS-App hinzufügen oder bearbeiten sollten.

Der Bot wird mit Bot Framework 3.x und dem Azure Web-App-Bot erstellt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
* Hinzufügen der Application Insights-Bibliothek zu einem Web-App-Bot
* Erfassen und Senden von LUIS-Abfrageergebnissen an Application Insights
* Abfragen von Application Insights nach den besten Absichten, Bewertungen und Äußerungen

## <a name="prerequisites"></a>Voraussetzungen

* Ihr LUIS-Web-App-Bot aus dem **[vorherigen Tutorial](luis-nodejs-tutorial-build-bot-framework-sample.md)** mit Application Insights muss aktiviert sein. 

> [!Tip]
> Wenn Sie noch kein Abonnement besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/) registrieren.

Der gesamte Code in diesem Tutorial ist im [GitHub-Repository für LUIS-Beispiele](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/nodejs) verfügbar, und jede Zeile, die zu diesem Tutorial gehört, ist mit `//APPINSIGHT:` kommentiert. 

## <a name="web-app-bot-with-luis"></a>Web-App-Bot mit LUIS
In diesem Tutorial wird vorausgesetzt, dass Sie über Code wie den folgenden verfügen oder das [andere Tutorial](luis-nodejs-tutorial-build-bot-framework-sample.md) abgeschlossen haben: 

   [!code-javascript[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/nodejs/app.js "Web app bot with LUIS")]

## <a name="add-application-insights-library-to-web-app-bot"></a>Hinzufügen der Application Insights-Bibliothek zum Web-App-Bot
Derzeit sammelt der Application Insights-Dienst, der in diesem Web-App-Bot verwendet wird, allgemeine Statustelemetriedaten für den Bot. Der Dienst sammelt keine Informationen aus LUIS-Anforderungen und -Antworten, die Sie überprüfen müssen, um Absichten und Entitäten zu korrigieren. 

Damit der Web-App-Bot LUIS-Anforderungen und -Antworten erfassen kann, muss das **[Application Insights](https://www.npmjs.com/package/applicationinsights)**-NPM-Paket für das Projekt installiert und in der **app.js**-Datei konfiguriert sein. Dann müssen die Dialoghandler für die Absicht die Informationen aus LUIS-Anforderungen und -Antworten an Application Insights senden. 

1. Wählen Sie im Azure-Portal im Web-App-Bot-Dienst im Abschnitt **Botverwaltung** die Option **Erstellen** aus. 

    ![Suchen nach Application Insights](./media/luis-tutorial-appinsights/build.png)

2. Eine neue Browserregisterkarte für den App Service-Editor wird geöffnet. Wählen Sie in der oberen Leiste den Namen der App aus, und klicken Sie dann auf **Kudu-Konsole öffnen**. 

    ![Suchen nach Application Insights](./media/luis-tutorial-appinsights/kudu-console.png)

3. Geben Sie in der Konsole folgenden Befehl ein. um Application Insights und die underscore-Pakete zu installieren:

    ```
    cd site\wwwroot && npm install applicationinsights && npm install underscore
    ```

    ![Suchen nach Application Insights](./media/luis-tutorial-appinsights/npm-install.png)

    Warten Sie, bis die Installation der Pakete abgeschlossen ist:

    ```
    luisbot@1.0.0 D:\home\site\wwwroot
    `-- applicationinsights@1.0.1 
      +-- diagnostic-channel@0.2.0 
      +-- diagnostic-channel-publishers@0.2.1 
      `-- zone.js@0.7.6 
    
    npm WARN luisbot@1.0.0 No repository field.
    luisbot@1.0.0 D:\home\site\wwwroot
    +-- botbuilder-azure@3.0.4
    | `-- azure-storage@1.4.0
    |   `-- underscore@1.4.4 
    `-- underscore@1.8.3 
    ```

    Damit sind Sie mit der Kudu-Konsole auf der Browserregisterkarte fertig.

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Erfassen und Senden von LUIS-Abfrageergebnissen an Application Insights
1. Öffnen Sie auf der Browserregisterkarte für den App Service-Editor die Datei **app.js**.

2. Fügen Sie die folgenden NPM-Bibliotheken unter den vorhandenen `requires`-Zeilen ein:

   [!code-javascript[Add NPM packages to app.js](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=12-16 "Add NPM packages to app.js")]

3. Erstellen Sie das Application Insights-Objekt, und verwenden Sie die Web-App-Bot-Anwendungseinstellung **BotDevInsightsKey**: 

   [!code-javascript[Create the Application Insights object](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=68-80 "Create the Application Insights object")]

4. Fügen Sie die Funktion **appInsightsLog** hinzu:

   [!code-javascript[Add the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=82-109 "Add the appInsightsLog function")]

    In der letzten Zeile der Funktion werden die Daten zu Application Insights hinzugefügt. Der Name des Ereignisses lautet **LUIS-results**, ein eindeutiger Name, der sich von den anderen Telemetriedaten dieses Web-App-Bots abhebt. 

5. Verwenden Sie die Funktion **appInsightsLog**. Sie fügen Sie zu jedem Absichtsdialog hinzu:

   [!code-javascript[Use the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=117-118 "Use the appInsightsLog function")]

6. Zum Testen Ihres Web-App-Bots verwenden Sie das Feature **Im Webchat testen** im Portal. Sie sollten keinen Unterschied bemerken, weil die gesamte Verarbeitung in Application Insights erfolgt und sich nicht in den Antworten des Bots widerspiegelt.

## <a name="view-luis-entries-in-application-insights"></a>Anzeigen von LUIS-Einträgen in Application Insights
Öffnen Sie Application Insights, um die LUIS-Einträge anzuzeigen. 

1. Wählen Sie im Portal **Alle Ressourcen** aus, und filtern Sie dann nach dem Namen des Web-App-Bots. Klicken Sie auf die Ressource mit dem Typ **Application Insights**. Das Symbol für Application Insights ist eine Glühbirne. 

    ![Suchen nach Application Insights](./media/luis-tutorial-appinsights/search-for-app-insights.png)



2. Nachdem die Ressource geöffnet wurde, klicken Sie auf das Symbol **Suchen** (Lupensymbol) im Bereich ganz rechts. Ein neuer Bereich wird rechts angezeigt. Je nach Umfang der gefundenen Telemetriedaten kann es einen Moment dauern, bis der Bereich angezeigt wird. Suchen Sie nach `LUIS-results`, und drücken Sie die EINGABETASTE auf der Tastatur. Die Liste ist nur auf LUIS-Abfrageergebnisse beschränkt, die mit diesem Tutorial hinzugefügt wurden.

    ![Filtern nach Abhängigkeiten](./media/luis-tutorial-appinsights/app-insights-filter.png)

3. Wählen Sie den oberen Eintrag aus. In einem neuen Fenster werden ausführlichere Daten angezeigt, einschließlich der benutzerdefinierten Daten für die LUIS-Abfrage ganz rechts. Zu den Daten gehören die am höchsten eingestufte Absicht und ihre Bewertung.

    ![Abhängigkeitsdetails](./media/luis-tutorial-appinsights/app-insights-detail.png)

    Wenn Sie fertig sind, klicken Sie ganz rechts oben auf das **X**, um zur Liste der Abhängigkeitselemente zurückzukehren. 


> [!Tip]
> Wenn Sie die Liste der Abhängigkeiten speichern und später zu ihr zurückkehren möchten, klicken Sie auf **...** (Mehr) und dann auf **Favorit speichern**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Abfragen von Application Insights nach Absichten, Bewertungen und Äußerungen
In Application Insights können Sie Daten mit der Sprache [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) abfragen und nach [Power BI](https://powerbi.microsoft.com) exportieren. 

1. Klicken Sie im oberen Bereich der Abhängigkeitsliste über dem Filterfeld auf **Analysen**. 

    ![Schaltfläche „Analytics“](./media/luis-tutorial-appinsights/analytics-button.png)

2. Es wird ein neues Fenster mit einem Abfragefenster oben und einem Datentabellenfenster darunter geöffnet. Wenn Sie bereits Datenbanken verwendet haben, ist Ihnen diese Anordnung vertraut. Die Abfrage enthält alle Elemente aus den letzten 24 Stunden, die mit dem Namen `LUIS-results` beginnen. Die Spalte **CustomDimensions** enthält die LUIS-Abfrageergebnisse als Name-Wert-Paare.

    ![Fenster „Analyseabfrage“](./media/luis-tutorial-appinsights/analytics-query-window.png)

3. Um die besten Absichten, Bewertungen und Äußerungen abzurufen, fügen Sie Folgendes direkt über der letzten Zeile im Abfragefenster ein:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_intent_intent)
    | extend score = todouble(customDimensions.LUIS_intent_score)
    | extend utterance = tostring(customDimensions.LUIS_text)
    ```

4. Führen Sie die Abfrage aus. Scrollen Sie in der Datentabelle nach ganz rechts. Die neuen Spalten „topIntent“, „score“ und „utterance“ sind verfügbar. Klicken Sie auf die Spalte „topIntent“, um die Tabelle nach dieser Spalte zu sortieren.

    ![Spalte „topIntent“ der Analyse](./media/luis-tutorial-appinsights/app-insights-top-intent.png)


Erfahren Sie mehr über die [Abfragesprache Kusto](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries), oder [exportieren Sie die Daten in Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen, die Sie möglicherweise den Application Insights-Daten hinzufügen sollten, sind z.B. App-ID, Versions-ID, Datum der letzten Modelländerung, Datum des letzten Trainings, Datum der letzten Veröffentlichung. Diese Werte können über die Endpunkt-URL (App-ID und Versions-ID) oder über einen [Erstellungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d)-Aufruf abgerufen und dann in den Web-App-Boteinstellungen festgelegt und von dort abgerufen werden.  

Wenn Sie dasselbe Endpunktabonnement für mehrere LUIS-Apps verwenden, sollten Sie auch die Abonnement-ID und eine Eigenschaft, die besagt, dass es sich um einen gemeinsam verwendeten Schlüssel handelt, einschließen. 

> [!div class="nextstepaction"]
> [Weitere Informationen zu Beispieläußerungen](luis-how-to-add-example-utterances.md)
