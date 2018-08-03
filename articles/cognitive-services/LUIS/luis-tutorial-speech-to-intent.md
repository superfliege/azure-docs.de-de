---
title: Verwenden des Speech C# SDKs mit LUIS – Azure | Microsoft-Dokumentation
titleSuffix: Azure
description: Verwenden Sie das Speech C# SDK-Beispiel, um von LUIS Absichts- und Entitätenvorhersagen zu erhalten, wenn Sie in das Mikrofon sprechen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/26/2018
ms.author: diberry;
ms.openlocfilehash: 286efcd97c0c9ab95a8241215bc36799c486a8b6
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247714"
---
# <a name="integrate-speech-service"></a>Integrieren des Spracherkennungsdiensts
Der [Spracherkennungsdienst](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) ermöglicht Ihnen, mit einer einzigen Anforderung Audio zu empfangen und JSON-Objekte der LUIS-Vorhersage zurückgeben zu lassen.

In diesem Artikel laden Sie ein C#-Projekt herunter und verwenden es in Visual Studio, um eine Äußerung in ein Mikrofon zu sprechen und von LUIS Vorhersageinformationen zu empfangen. Das Projekt verwendet das Spracherkennungspaket [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/), das bereits als Referenz enthalten ist. 

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Websitekonto, um die Anwendung zu importieren.

## <a name="create-luis-endpoint-key"></a>Erstellen eines LUIS-Endpunktschlüssels
[Erstellen](luis-how-to-azure-subscription.md#create-luis-endpoint-key) Sie im Azure-Portal einen **Sprachverständnis**-Schlüssel (LUIS). 

## <a name="import-human-resources-luis-app"></a>Importieren der LUIS-App „Personalwesen“
Die Absichten und Äußerungen für diesen Artikel stammen aus der LUIS-App „Personalwesen“, die im Github-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) verfügbar ist. Laden Sie die Datei [HumanResources.json](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json) herunter, speichern Sie sie mit der Erweiterung *.json, und [importieren](luis-how-to-start-new-app.md#import-new-app) Sie sie in LUIS. 

Diese App enthält Absichten, Entitäten und Äußerungen aus dem Bereich „Personalwesen“. Hier sehen Sie einige einfache Beispieläußerungen:

```
Who is John Smith's manager?
Who does John Smith manage?
Where is Form 123456?
Do I have any paid time off?
```

## <a name="add-keyphrase-prebuilt-entity"></a>Hinzufügen einer vordefinierten Add KeyPhrase-Entität
Nach dem Importieren der App wählen Sie **Entitäten** und dann **Vordefinierte Entitäten verwalten** aus. Fügen Sie die **KeyPhrase**-Entität hinzu. Die KeyPhrase-Entität extrahiert wichtige fachliche Inhalte aus der Äußerung.

## <a name="train-and-publish-the-app"></a>Trainieren und Veröffentlichen der App
1. Wählen Sie in der oberen, rechten Navigationsleiste die Schaltfläche **Trainieren** aus, um die LUIS-App zu trainieren.

2. Wählen Sie **Veröffentlichen** aus, um zur Seite „Veröffentlichen“ zu wechseln. 

3. Fügen Sie am unteren Rand der Seite **Veröffentlichen** den im Abschnitt [Erstellen eines LUIS-Endpunktschlüssels](#create-luis-endpoint-key) erstellten LUIS Schlüssel hinzu.

4. Veröffentlichen Sie die LUIS-App, indem Sie die Schaltfläche **Veröffentlichen** rechts neben den Slot „Veröffentlichen“ auswählen. 

  Erfassen Sie auf der Seite **Veröffentlichen** die App-ID, die Veröffentlichungsregion und die Abonnement-ID des im Abschnitt [Erstellen eines LUIS-Endpunktschlüssels](#create-luis-endpoint-key) erstellten LUIS Schlüssels. Sie müssen den Code später in diesem Artikel entsprechend mit diesen Werten ändern. 

  Diese Werte sind alle in der Endpunkt-URL unten auf der Seite **Veröffentlichen** für den erstellten Schlüssel enthalten. 
  
  Verwenden Sie **nicht** den kostenlosen Starterschlüssel für diese Übung. Nur ein im Azure-Portal erstellter **Sprachverständnis**-Schlüssel funktioniert für diese Übung. 

  https://**REGION**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**?subscription-key=**LUISKEY**&q=

## <a name="audio-device"></a>Audiogerät
In diesem Artikel wird das Audiogerät Ihres Computers verwendet. Das kann ein Headset mit Mikrofon oder ein eingebautes Audiogerät sein. Überprüfen Sie anhand der Audioeingangspegel, ob Sie lauter sprechen sollten, als Sie es normalerweise tun würden, damit Ihre Sprache vom Audiogerät erkannt wird. 

## <a name="download-the-luis-sample-project"></a>Herunterladen des LUIS-Beispielprojekts
 Klonen Sie das Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples), oder laden Sie es herunter. Öffnen Sie das Projekt [Sprache-Absichts-Umsetzung](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-speech-intent-recognition) mit Visual Studio und stellen Sie die NuGet-Pakete wieder her. Die Visual Studio-Projektmappendatei ist .\LUIS-Samples-master\documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln.

Das Speech SDK ist bereits als Referenz enthalten. 

[![](./media/luis-tutorial-speech-to-intent/nuget-package.png "Screenshot von Visual Studio 2017 mit dem Microsoft.CognitiveServices.Speech NuGet-Paket")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>Ändern des C#-Codes
Öffnen Sie die Datei **LUIS_samples.cs**, und ändern Sie die folgenden Variablen:

|Variablenname|Zweck|
|--|--|
|luisSubscriptionKey|Entspricht dem „subscription-key“-Wert der Endpunkt-URL auf der Seite „Veröffentlichen“|
|luisRegion|Entspricht der ersten Unterdomäne der Endpunkt-URL|
|luisAppId|Entspricht der Route der Endpunkt-URL nach **apps/**|

[![](./media/luis-tutorial-speech-to-intent/change-variables.png "Screenshot von Visual Studio 2017 mit den LUIS_samples.cs-Variablen")](./media/luis-tutorial-speech-to-intent/change-variables.png#lightbox)

Der Datei sind bereits die Absichten aus „Personalwesen“ zugeordnet.

[![](./media/luis-tutorial-speech-to-intent/intents.png "Screenshot von Visual Studio 2017 mit den LUIS_samples.cs-Absichten")](./media/luis-tutorial-speech-to-intent/intents.png#lightbox)

Erstellen Sie die App, und führen Sie sie aus. 

## <a name="test-code-with-utterance"></a>Testen von Code mit Äußerungen
Wählen Sie **1** aus, und sprechen Sie „Wer ist der Vorgesetzte von John Smith“ in das Mikrofon.

```cmd
1. Speech recognition of LUIS intent.
0. Stop.
Your choice: 1
LUIS...
Say something...
ResultId:cc83cebc9d6040d5956880bcdc5f5a98 Status:Recognized IntentId:<GetEmployeeOrgChart> Recognized text:<Who is the manager of John Smith?> Recognized Json:{"DisplayText":"Who is the manager of John Smith?","Duration":25700000,"Offset":9200000,"RecognitionStatus":"Success"}. LanguageUnderstandingJson:{
  "query": "Who is the manager of John Smith?",
  "topScoringIntent": {
    "intent": "GetEmployeeOrgChart",
    "score": 0.617331
  },
  "entities": [
    {
      "entity": "manager of john smith",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 31
    }
  ]
}

Recognition done. Your Choice:

```

Die richtige Absicht, **GetEmployeeOrgChart**, wurde mit einer Zuverlässigkeit von 61 % gefunden. Die keyPhrase-Entität wurde zurückgegeben. 

Das Speech SDK gibt die gesamte LUIS-Antwort zurück. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App „HumanResources“, falls Sie sie nicht mehr benötigen. Wählen Sie dazu in der Liste rechts vom App-Namen die Auslassungspunkte (***...***) und dann **Delete** (Löschen) aus. Wählen Sie im Popupdialogfenster **Delete App?** (App löschen?) **OK** aus.

Denken Sie daran, das Verzeichnis „LUIS-Samples“ zu löschen, wenn Sie mit der Verwendung des Beispielcodes fertig sind.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Integrieren von LUIS mit einem Bot](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website