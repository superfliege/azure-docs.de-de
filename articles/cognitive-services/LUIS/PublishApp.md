---
title: Veröffentlichen der LUIS-App | Microsoft-Dokumentation
description: Nach dem Erstellen und Testen Ihrer App mithilfe von Language Understanding Intelligent Service (LUIS) können Sie sie als Webdienst in Azure veröffentlichen.
services: cognitive-services
titleSuffix: Azure
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 1bd24af0498755b7cdcb170624fd8f9f3b39c85a
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341288"
---
# <a name="publish-your-trained-app"></a>Veröffentlichen der trainierten App
Wenn Sie Ihre LUIS-App erstellt und getestet haben, können Sie sie veröffentlichen. Nach der Veröffentlichung der App zeigt die Seite der Veröffentlichung alle zugeordneten HTTP-[Endpunkte](luis-glossary.md#endpoint) an. Diese Endpunkte pro [Azure-Region](luis-reference-regions.md) und pro [Schlüssel](luis-how-to-manage-keys.md) werden dann in einen beliebigen Client, Chatbot oder eine beliebige Back-End-Anwendung integriert. 

Sie können Ihre App vor einer Veröffentlichung immer [testen](interactive-test.md). 

## <a name="production-and-staging-slots"></a>Produktions- und Stagingslots
Sie können Ihre App im **Stagingslot** oder **Produktionsslot** veröffentlichen. Durch die Verwendung von zwei Slots für die Veröffentlichung können Sie zwei unterschiedliche Versionen mit veröffentlichten Endpunkten oder die gleiche Version auf zwei unterschiedlichen Endpunkten nutzen. 

<!-- TBD: what is the technical difference? log files, endpoint quota? -->

## <a name="settings-configuration-requires-publishing-model"></a>Veröffentlichungsmodell als Voraussetzung für die Einstellungskonfiguration
Führen Sie die Veröffentlichung im Endpunkt durch, nachdem Sie Änderungen an den folgenden Einstellungen vorgenommen haben. 

## <a name="external-services-settings"></a>Einstellungen externer Dienste
Zu den Einstellungen externer Dienste zählen die **[Standpunktanalyse](#enable-sentiment-analysis)** und die **[Spracherkennung](#enable-speech-priming)**.

### <a name="enable-sentiment-analysis"></a>Aktivieren der Standpunktanalyse
Wenn in den **Einstellungen für externe Dienste** das Kontrollkästchen **Enable Sentiment Analysis** (Standpunktanalyse aktivieren) aktiviert ist, kann LUIS mit der [Textanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/) integriert werden und Analysen zum Standpunkt und zu Schlüsselbegriffen durchführen. Sie müssen keine Textanalyseschlüssel bereitstellen, und es entstehen keine Abrechnungskosten für diesen Dienst in Ihrem Azure-Konto. Wenn Sie diese Einstellung einmal aktivieren, bleibt sie weiterhin gültig. 

Standpunktdaten stellen eine Bewertung zwischen 1 und 0 dar. Dabei entspricht ein positiver Standpunkt einem Wert, der näher an 1 ist, und ein negativer Standpunkt einem Wert näher 0.

Weitere Informationen zur JSON-Endpunktantwort im Zusammenhang mit der Standpunktanalyse finden Sie unter [Standpunktanalyse](luis-concept-data-extraction.md#sentiment-analysis).

### <a name="enable-speech-priming"></a>Aktivieren der Spracherkennung 
Unter den **Einstellungen externer Dienste** können Sie mithilfe des Kontrollkästchens **Enable Speech Priming** (Spracherkennung aktivieren) einen einzigen Endpunkt für eine gesprochene Äußerung aus einem Chatbot oder einer LUIS aufrufenden Anwendung festlegen und eine Prognoseantwort für LUIS erhalten. Die Spracherkennung verwendet die [SAPI](../Speech-Service/rest-apis.md), die zu Cognitive Services gehört. 

![Bild des Bestätigungsdialogfelds der Spracherkennung](./media/luis-how-to-publish-app/speech-prime-modal.png)

Nach der Aktivierung dieses Features können Sie Ihre App veröffentlichen. Beim Veröffentlichen der LUIS-App wird Ihr App-Modell an Ihren eigenen Speech-Dienst gesendet, um ihn vorzubereiten. Die Modellinformationen werden **nicht** außerhalb Ihres eigenen Diensts verwendet. 

Um die Verwendung von Spracherkennung abzuschließen, benötigen Sie die folgenden Informationen für das [Speech-SDK](../speech-service/speech-sdk-reference.md):
* Einen LUIS-Endpunktschlüssel
* Die LUIS-App-ID
* Eine Endpunktdomäne, die im Speech SDK als „Hostname“ bezeichnet wird, wie z.B. „westus.api.cognitive.microsoft.com“, wobei die erste Unterdomäne die Azure-Region ist, in der die Anwendung veröffentlicht wird

Weitere Informationen finden Sie im Beispiel für die [Sprache-Absichts-Umsetzung](http://aka.ms/speechsdk).

Wenn Ihre LUIS-App oder der Speech-Dienst gelöscht wird, werden auch Ihre Modelldaten entfernt. 

## <a name="endpoint-url-settings"></a>URL-Einstellungen des Endpunkts
Zu den URL-Einstellungen des Endpunkts zählen Unterschiede in der **[Zeitzone](#set-timezone-offset)**, **[alle vorhergesagten Absichtsbewertungen](#include-all-predicted-intent-scores)** sowie die **[ Bing-Rechtschreibprüfung](#enable-bing-spell-checker)**.

### <a name="set-timezone-offset"></a>Festlegen des Zeitzonenunterschieds 
Zur Slotwahl gehört auch die Auswahl der Zeitzone. Mit der Zeitzoneneinstellung kann LUIS jederzeit alle vordefinierten V2-Zeitwerte während der Prognose [verändern](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity), sodass die zurückgegebenen Entitätsdaten der ausgewählten Zeitzone entsprechen. 

### <a name="include-all-predicted-intent-scores"></a>Einschließen aller prognostizierten Absichtsbewertungen
Über das Kontrollkästchen **Include all predicted intent scores** (Alle prognostizierten Absichtsbewertungen einschließen) können Sie bei der Endpunktabfrageantwort die Prognosebewertung für jede Absicht einbeziehen. 

Dank dieser Einstellung kann der Chatbot oder die LUIS aufrufende Anwendung eine programmgesteuerte Entscheidung treffen, die auf den Bewertungen der zurückgegebenen Absichten beruht. Im Allgemeinen sind die obersten zwei Absichten am interessantesten. Wenn die oberste Bewertung die Absicht „None“ ist, kann Ihr Chatbot eine Nachverfolgungsfrage stellen, die endgültig zwischen der Absicht „None“ und der anderen Absicht mit hoher Bewertung entscheidet. 

Die Absichten und ihre Bewertungen sind ebenfalls in den Endpunktprotokollen enthalten. Sie können diese Protokolle [exportieren](create-new-app.md#export-app) und die Ergebnisse analysieren. 

```
{
  "query": "book a flight to Cairo",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.5223427
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.5223427
    },
    {
      "intent": "BookFlight",
      "score": 0.372391433
    }
  ],
  "entities": []
}
```

### <a name="enable-bing-spell-checker"></a>Aktivieren der Bing-Rechtschreibprüfung 
Wenn in den **URL-Einstellungen für den Endpunkt** das Kontrollkästchen **Enable Bing spell checker** (Bing-Rechtschreibprüfung aktivieren) aktiviert ist, kann LUIS falsch geschriebene Wörter vor der Vorhersage korrigieren. Dazu müssen Sie einen **[Schlüssel für die Bing Rechtschreibprüfung](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)** erstellen. Nach der Erstellung des Schlüssels werden zwei Abfragezeichenfolgenparameter zur Endpunkt-URL auf der Seite der Veröffentlichung hinzugefügt. 

Wenn Sie Ihre eigenen URLs für Ihre LUIS aufrufende Anwendung erstellen, vergewissern Sie sich, dass für den Abfragezeichenfolgenparameter Folgendes gilt: **spellCheck=true** sowie **bing-spell-check-subscription-key={YOUR_BING_KEY_HERE}**. Ersetzen Sie `{YOUR_BING_KEY_HERE}` durch den Schlüssel Ihrer Bing-Rechtschreibprüfung.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```

## <a name="publish-your-trained-app-to-an-http-endpoint"></a>Veröffentlichen der trainierten App in einem HTTP-Endpunkt
Öffnen Sie Ihre App, indem Sie auf der Seite **My Apps** (Meine Apps) auf ihren Namen und anschließend im oberen Bereich auf **Publish** (Veröffentlichen) klicken. 

![Seite der Veröffentlichung](./media/luis-how-to-publish-app/publish-to-production.png)
 
Wenn Ihre App erfolgreich veröffentlicht wurde, wird eine grüne Erfolgsmeldung am oberen Rand des Browsers angezeigt. 

* Wählen Sie, ob die Veröffentlichung in **Produktion** oder **Staging** stattfinden soll, indem Sie die jeweilige Option aus dem Dropdownmenü unter **Select Slot** (Slot auswählen) auswählen. 

## <a name="assign-key"></a>Zuweisen eines Schlüssels

Wenn Sie einen anderen als den kostenlosen Schlüssel „Starter_Key“ verwenden möchten, klicken Sie auf die Schaltfläche **Add Key** (Schlüssel hinzufügen). Dadurch wird ein Dialogfeld geöffnet, in dem Sie einen vorhandenen Endpunktschlüssel auswählen können, der der App zugewiesen wird. Weitere Informationen dazu, wie Sie Endpunktschlüssel erstellen und zu Ihrer LUIS-App hinzufügen, finden Sie unter [Verwalten Ihrer Schlüssel](luis-how-to-manage-keys.md).

Um Endpunkte und andere Schlüssel anzuzeigen, die anderen Azure-Regionen zugeordnet sind, verwenden Sie die Optionsfelder und wechseln dort die Bereiche. Jede Zeile in der Tabelle **Resources and Keys** (Ressourcen und Schlüssel) führt Azure-Ressourcen auf, die Ihrem Konto zugeordnet sind, sowie die Endpunktschlüssel, die dieser Ressource zugeordnet sind.

## <a name="endpoint-url-construction"></a>Erstellen von Endpunkt-URLs
Die Endpunkt-URL entspricht der Azure-Region, die dem Endpunktschlüssel zugeordnet ist.

Diese übersichtliche Tabelle zeigt Ihre Veröffentlichungskonfiguration in der Endpunkt-URL zusammen mit ausgewählten Routen und Werten von Abfragezeichenfolgen. Wenn Sie die Endpunkt-URLs für Ihre LUIS aufrufende Anwendung erstellen, vergewissern Sie sich, dass für den verwendeten Endpunkt die gleichen Routen und Werte von Abfragezeichenfolgen festgelegt werden – sofern Sie sie festlegen möchten.

Die URL-Route wird mit der Azure-Region und der App-ID erstellt. Wenn Sie in anderen Azure-Regionen oder mit anderen Apps veröffentlichen, kann die Endpunkt-URL durch Ändern der Azure-Region und der App-ID-Werte erstellt werden. 

* Wählen Sie den Produktionsslot aus, und klicken Sie auf die Schaltfläche **Publish** (Veröffentlichen). Wenn die Veröffentlichung erfolgreich war, verwenden Sie die angezeigte Endpunkt-URL, um auf Ihre LUIS-App zuzugreifen. 

### <a name="optional-query-string-parameters"></a>Optionale Parameter für Abfragezeichenfolgen
Die folgenden Parameter für Abfragezeichenfolgen können mit der Endpunkt-URL verwendet werden:

<!-- TBD: what about speech priming? -->

|Abfragezeichenfolge|Typ|Beispielwert|Zweck|
|--|--|--|--|
|Ausführlich|boolean|true|Schließt [alle Absichtsbewertungen](#include-all-predicted-intent-scores) für Äußerungen ein|
|timezoneOffset|Anzahl (als Einheit gelten Minuten)|60|Legt den [Zeitzonenunterschied](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) für [vordefinierte datetimeV2-Entitäten](luis-reference-prebuilt-datetimev2.md) fest|
|spellCheck|boolean|true|[Korrekte Rechtschreibung](#enable-bing-spell-checker) in der Äußerung; wird in Verbindung mit dem Parameter der Abfragezeichenfolge „bing-spell-check-subscription-key“ verwendet|
|bing-spell-check-subscription-key|Abonnement-ID||Wird zusammen mit der Abfragezeichenfolge „spellCheck“ verwendet|
|staging|boolean|false|Für die Auswahl des Staging- oder Produktionsendpunkts|
|log|boolean|true|Fügt Abfrage und Ergebnisse zum Protokoll hinzu|


## <a name="test-your-published-endpoint-in-a-browser"></a>Testen der veröffentlichten Endpunkte in einem Browser
Testen Sie Ihren veröffentlichten Endpunkt, indem Sie die URL in der Spalte **Endpoint** (Endpunkt) auswählen. Der Standardbrowser wird mit der generierten URL geöffnet. Legen Sie den URL-Parameter „&q“ auf Ihre Testabfrage fest. Fügen Sie beispielsweise `&q=Book me a flight to Boston on May 4` zu Ihrer URL hinzu, und drücken Sie dann die EINGABETASTE. Der Browser zeigt die JSON-Antwort Ihres HTTP-Endpunkts an. 

![JSON-Antwort von einem veröffentlichten HTTP-Endpunkt](./media/luis-how-to-publish-app/luis-publish-app-json-response.png)

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen, wie Sie Schlüssel zu Ihrer LUIS-App hinzufügen und wie Schlüssel Azure-Regionen zugeordnet sind, finden Sie unter [Verwalten von Schlüsseln](./luis-how-to-manage-keys.md).
* Weitere Anweisungen dazu, wie Sie Ihre veröffentlichte App in der Testkonsole testen, finden Sie unter [Trainieren und Testen Ihrer App](interactive-test.md).
