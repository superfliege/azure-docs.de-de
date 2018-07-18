---
title: Hinzufügen von LUIS-Daten zu Application Insights mit C# | Microsoft-Dokumentation
titleSuffix: Azure
description: Erstellen Sie mit C# einen Bot, der in eine LUIS-Anwendung und Application Insights integriert ist.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/07/2018
ms.author: v-geberr
ms.openlocfilehash: 52b6ae224b0e8da12eb4903f5100a6e5cc39704d
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "35378838"
---
# <a name="add-luis-results-to-application-insights-from-a-web-app-bot"></a>Hinzufügen von LUIS-Ergebnissen zu Application Insights von einem Web-App-Bot
In diesem Tutorial werden [Application Insights](https://azure.microsoft.com/services/application-insights/)-Telemetriedatenspeichern LUIS-Antwortinformationen hinzugefügt. Sobald Sie über diese Daten verfügen, können Sie mit der Sprache Kusto oder mit Power BI Absichten und Entitäten der Äußerung in Echtzeit analysieren, aggregieren und Berichte für sie erstellen. Diese Analyse hilft Ihnen dabei, zu ermitteln, ob Sie die Absichten und Entitäten aus Ihrer LUIS-App hinzufügen oder bearbeiten sollten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
* Hinzufügen von Application Insights zu einem Web-App-Bot
* Erfassen und Senden von LUIS-Abfrageergebnissen an Application Insights
* Abfragen von Application Insights nach den besten Absichten, Bewertungen und Äußerungen

## <a name="prerequisites"></a>Voraussetzungen

* Ihr LUIS-Web-App-Bot aus dem **[vorherigen Tutorial](luis-csharp-tutorial-build-bot-framework-sample.md)** mit Application Insights muss aktiviert sein. 
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) muss lokal auf dem Computer installiert sein.

> [!Tip]
> Wenn Sie noch kein Abonnement besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/) registrieren.

Der gesamte Code in diesem Tutorial ist über das [GitHub-Repository für LUIS-Beispiele](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/csharp) verfügbar, und jede Zeile, die zu diesem Tutorial gehört, ist mit `//LUIS Tutorial:` kommentiert. 

## <a name="review-luis-web-app-bot"></a>Überprüfen des LUIS-Web-App-Bots
In diesem Tutorial wird vorausgesetzt, dass Sie über Code wie den folgenden verfügen oder das [andere Tutorial](luis-csharp-tutorial-build-bot-framework-sample.md) abgeschlossen haben: 

   [!code-csharp[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs "Web app bot with LUIS")]

## <a name="application-insights-in-web-app-bot"></a>Application Insights im Web-App-Bot
Derzeit sammelt der Application Insights-Dienst, der im Rahmen der Erstellung des Web-App-Botdiensts hinzugefügt wurde, allgemeine Statustelemetrie für den Bot. Er sammelt keine LUIS-Antwortinformationen. Um LUIS zu analysieren und zu verbessern benötigen Sie die LUIS-Antwortinformationen.  

Um die LUIS-Antwort zu erfassen, erfordert der Web-App-Bot, dass **[Application Insights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** für das Projekt installiert und konfiguriert ist. 

## <a name="download-web-app-bot"></a>Herunterladen des Web-App-Bots
Verwenden Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/), um Application Insights für den Web-App-Bot hinzuzufügen und zu konfigurieren. Um den Web-App-Bot in Visual Studio verwenden zu können, laden Sie den Web-App-Botcode herunter.

1. Wählen Sie im Azure-Portal für den Web-App-Bot **Erstellen** aus.

    ![Auswählen von „Erstellen“ im Portal](./media/luis-tutorial-bot-csharp-appinsights/download-build-menu.png)

2. Wählen Sie **In ZIP-Datei herunterladen** aus, und warten Sie, bis die Datei vorbereitet wurde.

    ![In ZIP-Datei herunterladen](./media/luis-tutorial-bot-csharp-appinsights/download-link.png)

3. Wählen Sie im Popupfenster **In ZIP-Datei herunterladen** aus. Merken Sie sich den Speicherort auf Ihrem Computer, da Sie ihn im nächsten Abschnitt benötigen.

    ![Popupfenster „In ZIP-Datei herunterladen“](./media/luis-tutorial-bot-csharp-appinsights/download-popup.png)

## <a name="open-solution-in-visual-studio-2017"></a>Öffnen Sie die Projektmappe in Visual Studio 2017.

1. Extrahieren Sie die Datei in einen Ordner. 

2. Öffnen Sie anschließend Visual Studio 2017 und die Projektmappendatei `Microsoft.Bot.Sample.LuisBot.sln`. Wenn die Sicherheitswarnung angezeigt wird, wählen Sie „OK“ aus.

    ![Öffnen Sie die Projektmappe in Visual Studio 2017.](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-security-warning.png)

3. Visual Studio muss der Projektmappe Abhängigkeiten hinzufügen. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **NuGet-Pakete verwalten** aus. 

    ![NuGet-Pakete verwalten](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-manage-nuget-packages.png)

4. Im NuGet-Paket-Manager wird eine Liste der installierten Pakete angezeigt. Wählen Sie auf der gelben Leiste **Wiederherstellen** aus. Warten Sie, bis der Wiederherstellungsvorgang beendet ist.

    ![Wiederherstellen der NuGet-Pakete](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-restore-packages.png)

## <a name="add-application-insights-to-the-project"></a>Hinzufügen von Application Insights zum Projekt
Installieren Sie Application Insights, und konfigurieren Sie die Anwendung in Visual Studio. 

1. Wählen Sie in Visual Studio 2017 auf der oberen Menüleiste **Projekt** und dann **Application Insights-Telemetrie hinzufügen** aus.

2. Wählen Sie im Fenster **Application Insights-Konfiguration** die Option **Kostenlos starten** aus.

    ![Beginnen mit der Konfiguration von Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configure-app-insights.png)

3. Registrieren Sie Ihre App bei Application Insights. Möglicherweise müssen Sie Ihre Anmeldeinformationen für das Azure-Portal eingeben. 

4. Visual Studio fügt Application Insights dem Projekt hinzu und zeigt dabei den Status an. 

    ![Application Insights-Status](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-adding-application-insights-to-project.png)

    Wenn der Prozess abgeschlossen ist, wird in der **Application Insights-Konfiguration** der Fortschrittstatus angezeigt. 

    ![Application Insights-Fortschrittstatus](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configured-application-insights-to-project.png)

    **Ablaufverfolgungssammlung aktivieren** ist rot, d.h. nicht aktiviert. In diesem Tutorial wird das Feature nicht verwendet. 

## <a name="build-and-resolve-errors"></a>Erstellen und Beheben von Fehlern

1. Erstellen Sie die Projektmappe, indem Sie das Menü **Erstellen** und dann **Projektmappe neu erstellen** auswählen. Warten Sie, bis der Buildvorgang abgeschlossen ist. 

2. Wenn Fehler vom Typ `CS0104` auftreten, müssen Sie sie beheben. Beheben Sie im Ordner `Controllers` in der Datei `MessagesController.cs file` die mehrdeutige Verwendung vom Typ `Activity`, indem Sie dem Aktivitätstyp den Connectortyp voranstellen. Ändern Sie dazu den Namen `Activity` in den Zeilen 22 und 36 von `Activity` in `Connector.Activity`. Erstellen Sie die Projektmappe erneut. Es sollten keine weiteren Buildfehler auftreten.

    Der vollständige Quellcode dieser Datei lautet:

    [!code-csharp[MessagesController.cs file](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/MessagesController.cs "MessagesController.cs file")]

## <a name="publish-project-to-azure"></a>Veröffentlichen des Projekts in Azure
Das Paket **Application Insights** befindet sich nun im Projekt und wurde für Ihre Anmeldeinformationen im Azure-Portal ordnungsgemäß konfiguriert. Die Änderungen am Projekt müssen zurück in Azure veröffentlicht werden.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Veröffentlichen** aus.

    ![Veröffentlichen des Projekts im Portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish.png)

2. Wählen Sie im Fenster **Veröffentlichen** die Option **Neues Profil erstellen** aus.

    ![Veröffentlichen des Projekts im Portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-1.png)

3. Wählen Sie **Profil importieren** und dann **OK** aus.

    ![Veröffentlichen des Projekts im Portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-2.png)

4. Navigieren Sie im Fenster **Datei mit Veröffentlichungseinstellungen importieren** zum Projektordner, navigieren Sie zum Ordner `PostDeployScripts`, wählen Sie die Datei aus, die mit `.PublishSettings` endet, und wählen Sie dann `Open` aus. Sie haben jetzt die Veröffentlichung für dieses Projekt konfiguriert. 

5. Veröffentlichen Sie Ihren lokalen Quellcode im Botdienst, indem Sie die Schaltfläche **Veröffentlichen** auswählen. Im Fenster **Ausgabe** wird der Status angezeigt. Der Rest des Tutorials wird im Azure-Portal abgeschlossen. Schließen Sie Visual Studio 2017. 

## <a name="open-three-browser-tabs"></a>Öffnen von drei Browserregisterkarten
Suchen Sie im Azure-Portal den Web-App-Bot, und öffnen Sie ihn. In den folgenden Schritten werden drei unterschiedliche Ansichten des Web-App-Bots verwendet. Möglicherweise ist es einfacher, im Browser drei separate Registerkarten geöffnet zu halten: 
  
>  * Testen im Webchat
>  * Erstellen/Öffnen des Onlinecode-Editors -> App Service-Editor
>  * App Service-Editor/Öffnen der Kudu-Konsole -> Diagnosekonsole

## <a name="modify-basicluisdialogcs-code"></a>Ändern des Codes in „BasicLuisDialog.cs“

1. Öffnen Sie auf der Browserregisterkarte für den **App Service-Editor** die Datei `BasicLuisDialog.cs`.

2. Fügen Sie die folgende NuGet-Abhängigkeit unter den vorhandenen `using`-Zeilen ein:

   [!code-csharp[Add using statement](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=11-12 "Add using statement")]

3. Fügen Sie die `LogToApplicationInsights`-Funktion hinzu:

   [!code-csharp[Add the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=61-92 "Add the LogToApplicationInsights function")]

    Der Application Insights-Instrumentierungsschlüssel befindet sich bereits in der Anwendungseinstellung des Web-App-Bots mit dem Namen `BotDevInsightsKey`. 

    In der letzten Zeile der Funktion werden die Daten Application Insights hinzugefügt. Der Name der Ablaufverfolgung lautet `LUIS`, ein eindeutiger Name, der sich von den anderen Telemetriedaten dieses Web-App-Bots abhebt. Alle Eigenschaften sind auch mit dem Präfix `LUIS_` versehen, damit Sie sehen können, welche Daten in diesem Tutorial hinzugefügt werden und welche Informationen vom Web-App-Bot angegeben werden.

4. Rufen Sie die `LogToApplicationInsights`-Funktion im oberen Bereich der `ShowLuisResult`-Funktion auf:

   [!code-csharp[Use the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=114-115 "Use the LogToApplicationInsights function")]

## <a name="build-web-app-bot"></a>Erstellen des Web-App-Bots
1. Erstellen Sie den Web-App-Bot auf eine von zwei Arten. Die erste Methode besteht darin, im **App Service-Editor** mit der rechten Maustaste auf `build.cmd` zu klicken und dann **Run from Console** (In Konsole ausführen) auszuwählen. Die Ausgabe der Konsole wird angezeigt und endet mit `Finished successfully.`

2. Wenn dies nicht erfolgreich abgeschlossen wird, müssen Sie die Konsole öffnen, zum Skript navigieren und es mithilfe der folgenden Schritte ausführen. Wählen Sie im **App Service-Editor** auf der blauen Leiste oben den Namen Ihres Bots und dann in der Dropdownliste **Open Kudu Console** (Kudu-Konsole öffnen) aus.

    ![Open Kudu Console (Kudu-Konsole öffnen)](./media/luis-tutorial-bot-csharp-appinsights/open-kudu-console.png)

3. Geben Sie im Konsolenfenster den folgenden Befehl ein:

    ```
    cd site\wwwroot && build.cmd
    ```

    Warten Sie, bis der Buildvorgang mit `Finished successfully.` abgeschlossen wird.

## <a name="test-the-web-app-bot"></a>Testen des Web-App-Bots

1. Zum Testen Ihres Web-App-Bots öffnen Sie das Feature **Test in Web Chat** (Im Webchat testen) im Portal. 

2. Geben Sie den Ausdruck `Coffee bar on please` ein.  

    ![Testen des Web-App-Bots im Chat](./media/luis-tutorial-bot-csharp-appinsights/test-in-web-chat.png)

3. Es sollte kein Unterschied in der Antwort des Chatbots zu erkennen sein. Die Änderung besteht im Senden von Daten an Application Insights, nicht in den Antworten des Bots. Geben Sie einige weitere Äußerungen ein, damit etwas mehr Daten in Application Insights vorliegen:

```
Please deliver a pizza
Turn off all the lights
Turn on the hall light
```

## <a name="view-luis-entries-in-application-insights"></a>Anzeigen von LUIS-Einträgen in Application Insights
Öffnen Sie Application Insights, um die LUIS-Einträge anzuzeigen. 

1. Wählen Sie im Portal **Alle Ressourcen** aus, und filtern Sie dann nach dem Namen des Web-App-Bots. Klicken Sie auf die Ressource mit dem Typ **Application Insights**. Das Symbol für Application Insights ist eine Glühbirne. 

    ![Suchen nach Application Insights](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights.png)

2. Nachdem die Ressource geöffnet wurde, klicken Sie auf das Symbol **Suchen** (Lupensymbol) im Bereich ganz rechts. Ein neuer Bereich wird rechts angezeigt. Je nach dem Umfang der gefundenen Telemetriedaten kann es einen Moment dauern, bis der Bereich angezeigt wird. Suchen Sie nach `LUIS`. Die Liste ist nur auf LUIS-Abfrageergebnisse, die mit diesem Tutorial hinzugefügt wurden, eingeschränkt.

    ![Suchen nach Ablaufverfolgungen](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace.png)

3. Wählen Sie den oberen Eintrag aus. In einem neuen Fenster werden ausführlichere Daten angezeigt, einschließlich der benutzerdefinierten Daten für die LUIS-Abfrage ganz rechts. Zu den Daten gehören die am höchsten eingestufte Absicht und ihre Bewertung.

    ![Überprüfen eines Ablaufverfolgungselements](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace-item.png)

    Wenn Sie fertig sind, wählen Sie ganz rechts oben **X** aus, um zur Liste der Abhängigkeitselemente zurückzukehren. 


> [!Tip]
> Wenn Sie die Liste der Abhängigkeiten speichern und später zu ihr zurückkehren möchten, klicken Sie auf **...** (Mehr) und dann auf **Favorit speichern**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Abfragen von Application Insights nach Absichten, Bewertungen und Äußerungen
In Application Insights können Sie Daten mit der Sprache [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) abfragen und nach [Power BI](https://powerbi.microsoft.com) exportieren. 

1. Klicken Sie im oberen Bereich der Abhängigkeitsliste über dem Filterfeld auf **Analysen**. 

    ![Schaltfläche „Analytics“](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-analytics-button.png)

2. Es wird ein neues Fenster mit einem Abfragefenster oben und einem Datentabellenfenster darunter geöffnet. Wenn Sie bereits Datenbanken verwendet haben, ist Ihnen diese Anordnung vertraut. Die Abfrage enthält alle Elemente aus den letzten 24 Stunden, die mit dem Namen `LUIS` beginnen. Die Spalte **CustomDimensions** enthält die LUIS-Abfrageergebnisse als Name-Wert-Paare.

    ![Standardanalysebericht](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-1.png)

3. Um die besten Absichten, Bewertungen und Äußerungen abzurufen, fügen Sie Folgendes direkt über der letzten Zeile im Abfragefenster ein:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

4. Führen Sie die Abfrage aus. Scrollen Sie in der Datentabelle nach ganz rechts. Die neuen Spalten „topIntent“, „score“ und „utterance“ sind verfügbar. Klicken Sie auf die zu sortierende Spalte „topIntent“.

    ![Benutzerdefinierter Analysebericht](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-2.png)


Erfahren Sie mehr über die [Abfragesprache Kusto](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-queries), oder [exportieren Sie die Daten in Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 


## <a name="learn-more-about-bot-framework"></a>Weitere Informationen über das Bot-Framework
Erfahren Sie mehr über das [Bot-Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen, die Sie möglicherweise den Application Insights-Daten hinzufügen sollten, sind z.B. App-ID, Versions-ID, Datum der letzten Modelländerung, Datum des letzten Trainings, Datum der letzten Veröffentlichung. Diese Werte können über die Endpunkt-URL (App-ID und Versions-ID) oder über einen [Erstellungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d)-Aufruf abgerufen und dann in den Web-App-Boteinstellungen festgelegt und von dort abgerufen werden.  

Wenn Sie dasselbe Endpunktabonnement für mehrere LUIS-Apps verwenden, sollten Sie auch die Abonnement-ID und eine Eigenschaft, die besagt, dass es sich um einen gemeinsam verwendeten Schlüssel handelt, einschließen. 

> [!div class="nextstepaction"]
> [Weitere Informationen zu Beispieläußerungen](luis-how-to-add-example-utterances.md)
