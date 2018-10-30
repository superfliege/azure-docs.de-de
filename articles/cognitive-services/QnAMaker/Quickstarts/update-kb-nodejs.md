---
title: 'Schnellstart: Aktualisieren einer Wissensdatenbank: REST, Node.js – QnA Maker'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung wird das programmgesteuerte Aktualisieren einer vorhandenen QnA Maker-Wissensdatenbank Schritt für Schritt beschrieben.  Mit diesem JSON-Code können Sie eine Wissensdatenbank aktualisieren, indem Sie neue Datenquellen hinzufügen oder Datenquellen ändern oder löschen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: diberry
ms.openlocfilehash: 1b92589fe01f171b732c04057dc290f0f32cb31a
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647815"
---
# <a name="quickstart-update-a-knowledge-base-in-qna-maker-using-nodejs"></a>Schnellstart: Aktualisieren einer Wissensdatenbank in QnA Maker mithilfe von Node.js

In dieser Schnellstartanleitung wird das programmgesteuerte Aktualisieren einer vorhandenen QnA Maker-Wissensdatenbank Schritt für Schritt beschrieben.  Mit diesem JSON-Code können Sie eine Wissensdatenbank aktualisieren, indem Sie neue Datenquellen hinzufügen oder Datenquellen ändern oder löschen.

Diese API entspricht dem Bearbeiten und anschließenden Verwenden der Schaltfläche **Speichern und trainieren** im QnA Maker-Portal.

In dieser Schnellstartanleitung werden QnA Maker-APIs aufgerufen:
* [Update](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600): Das Modell für die Wissensdatenbank ist im JSON-Code definiert, der im Text der API-Anforderung gesendet wird. 
* [Abrufen von Vorgangsdetails](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)

## <a name="prerequisites"></a>Voraussetzungen

* [Node.js 6+](https://nodejs.org/en/download/)
* Sie benötigen einen [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md). Wählen Sie zum Abrufen des Schlüssels auf Ihrem Dashboard unter **Ressourcenverwaltung** die Option **Schlüssel** aus. 
* ID der QnA Maker-Wissensdatenbank, die in der URL des Abfragezeichenfolgenparameters „kbid“ enthalten ist, wie nachfolgend gezeigt:

    ![ID der QnA Maker-Wissensdatenbank](../media/qnamaker-quickstart-kb/qna-maker-id.png)

Falls Sie noch keine Wissensdatenbank besitzen, können Sie für diese Schnellstartanleitung eine Beispieldatenbank erstellen: [Create a new knowledge base in Python](create-new-kb-nodejs.md) (Erstellen einer neuen Wissensdatenbank in Python)

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

## <a name="create-a-knowledge-base-nodejs-file"></a>Erstellen einer Node.js-Datei für die Wissensdatenbank

Erstellen Sie eine Datei mit dem Namen `update-knowledge-base.js`.

## <a name="add-the-required-dependencies"></a>Hinzufügen der erforderlichen Abhängigkeiten

Fügen Sie oben in der Datei `update-knowledge-base.js` die folgenden Zeilen hinzu, um dem Projekt die erforderlichen Abhängigkeiten hinzuzufügen:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=1-4 "Add the dependencies")]

## <a name="add-required-constants"></a>Hinzufügen der erforderlichen Konstanten
Fügen Sie im Anschluss an die erforderlichen Abhängigkeiten die benötigten Konstanten für den Zugriff auf QnA Maker hinzu. Ersetzen Sie den Wert der Variablen `subscriptionKey` durch Ihren QnA Maker-Schlüssel. 

[!code-nodejs[Add required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=10-17 "Add required constants")]

## <a name="add-knowledge-base-id"></a>Hinzufügen der Wissensdatenbank-ID

Fügen Sie im Anschluss an die oben genannten Konstanten die Wissensdatenbank-ID ein, und fügen Sie sie dem Pfad hinzu:

[!code-nodejs[Add knowledge base ID](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=19-23 "Add knowledge base ID")]

## <a name="add-the-kb-update-model-definition"></a>Hinzufügen der Modelldefinition für das Update der Wissensdatenbank

Fügen Sie nach den Konstanten die folgende Definition für das Update der Wissensdatenbank hinzu. Die Updatedefinition enthält drei Abschnitte:

* Hinzufügen
* aktualisieren
* delete

Jeder Abschnitt kann in derselben einzelnen Anforderung an die API verwendet werden. 

[!code-nodejs[Add the KB update definition](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=25-53 "Add the KB update definition")]


## <a name="add-supporting-functions"></a>Hinzufügen von unterstützenden Funktionen

Als Nächstes fügen Sie die folgenden unterstützenden Funktionen hinzu.

1. Fügen Sie die folgende Funktion zum Ausgeben von JSON in einem lesbaren Format hinzu:

   [!code-nodejs[Add supporting functions, step 1](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=55-58 "Add supporting functions, step 1")]

2. Fügen Sie zum Verwalten der HTTP-Antwort und zum Abrufen des Status des Erstellvorgangs die folgenden Funktionen hinzu:

   [!code-nodejs[Add supporting functions, step 2](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=60-82 "Add supporting functions, step 2")]

## <a name="add-patch-request-to-update-kb"></a>Hinzufügen einer PATCH-Anforderung zum Aktualisieren der Wissensdatenbank

Fügen Sie die folgenden Funktionen hinzu, um eine HTTP PATCH-Anforderung zum Aktualisieren der Wissensdatenbank zu senden. `Ocp-Apim-Subscription-Key` ist der QnA Maker-Dienstschlüssel, der für die Authentifizierung verwendet wird.

[!code-nodejs[Add PATCH request to update KB](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=84-111 "Add PATCH request to update KB")]

Mit diesem API-Aufruf wird eine JSON-Antwort zurückgegeben, die die Vorgangs-ID enthält. Die Vorgangs-ID ist erforderlich, um den Status anzufordern, wenn der Vorgang nicht abgeschlossen ist.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-10-02T01:23:00Z",
  "lastActionTimestamp": "2018-10-02T01:23:00Z",
  "userId": "335c3841df0b42cdb00f53a49d51a89c",
  "operationId": "e7be3897-88ff-44e5-a06c-01df0e05b78c"
}
```

## <a name="add-get-request-to-determine-operation-status"></a>Hinzufügen einer GET-Anforderung zum Ermitteln des Vorgangsstatus

Überprüfen Sie den Status des Vorgangs.
    
[!code-nodejs[Add GET request to determine operation status](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=113-137 "Add GET request to determine operation status")]

Dieser API-Aufruf gibt eine JSON-Antwort zurück, die den Vorgangsstatus enthält: 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

Wiederholen Sie den Aufruf, bis er erfolgreich abgeschlossen wird oder ein Fehler auftritt: 

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-updatekb-method"></a>Hinzufügen der update_kb-Methode

Mit der folgenden Methode wird die Wissensdatenbank aktualisiert, und der Status wird wiederholt überprüft. Da die Erstellung der Wissensdatenbank einige Zeit dauern kann, müssen Sie die Aufrufe zum Überprüfen des Status wiederholen, bis der Status entweder „Erfolgreich“ oder „Fehler“ lautet.

[!code-nodejs[Add update_kb method](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=139-169 "Add update_kb method")]

## <a name="run-the-program"></a>Ausführen des Programms

Geben Sie den folgenden Befehl in einer Befehlszeile ein, um das Programm auszuführen. Die Anforderung wird an die QnA Maker-API gesendet, um die Wissensdatenbank zu aktualisieren, und dann werden die Ergebnisse alle 30 Sekunden abgefragt. Jede Antwort wird im Konsolenfenster ausgegeben.

```bash
node update-knowledge-base.js
```

Nach der Aktualisierung der Wissensdatenbank können Sie sie im QnA Maker-Portal auf der Seite [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) (Meine Wissensdatenbanken) anzeigen. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz für QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)