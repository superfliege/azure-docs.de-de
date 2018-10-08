---
title: Verwalten von Erstellung und Endpunktschlüsseln in LUIS
titleSuffix: Azure Cognitive Services
description: Weisen Sie nach dem Erstellen eines LUIS-Endpunktschlüssels im Azure-Portal den Schlüssel der LUIS-App zu, und rufen Sie die richtige Endpunkt-URL ab. Verwenden Sie diese Endpunkt-URL, um LUIS-Vorhersagen zu erhalten.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 62081f96e2081833eb705992914899a6764bd792
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033208"
---
# <a name="add-an-azure-luis-resource-to-app"></a>Hinzufügen einer Azure LUIS-Ressource zu einer App

Weisen Sie nach dem Erstellen einer LUIS-Ressource im Azure-Portal die Ressource der LUIS-App zu, und rufen Sie die richtige Endpunkt-URL ab. Verwenden Sie diese Endpunkt-URL, um LUIS-Vorhersagen zu erhalten.

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
<a name="key-concepts"></a>
<a name="authoring-key"></a>
<a name="create-and-use-an-endpoint-key"></a>
<a name="assign-endpoint-key"></a>

## <a name="assign-resource"></a>Zuweisen einer Ressource

1. Erstellen Sie einen LUIS-Schlüssel im [Azure-Portal](https://portal.azure.com). Weitere Anweisungen finden Sie unter [Verwalten von Azure-Endpunkt-Abonnementschlüsseln](luis-how-to-azure-subscription.md).
 
2. Wählen Sie im Menü oben rechts **Verwalten** und dann **Schlüssel und Endpunkte** aus.

    [ ![Seite „Schlüssel und Endpunkte“](./media/luis-manage-keys/keys-and-endpoints.png) ](./media/luis-manage-keys/keys-and-endpoints.png#lightbox)

3. Um LUIS hinzuzufügen, wählen Sie **Ressource hinzufügen +** aus.

    ![Zuweisen einer Ressource zu Ihrer App](./media/luis-manage-keys/assign-key.png)

4. Wählen Sie im Dialogfeld, das mit der E-Mail-Adresse verknüpft ist, die Sie zur Anmeldung bei der LUIS-Website verwenden, einen Mandanten aus.  

5. Wählen Sie den **Abonnementnamen** aus, der der Azure-Ressource zugeordnet ist, die Sie hinzufügen möchten.

6. Wählen Sie den **LUIS-Ressourcennamen** aus. 

7. Wählen Sie **Ressource zuweisen** aus. 

8. Suchen Sie die neue Zeile in der Tabelle, und kopieren Sie die Endpunkt-URL. Sie ist ordnungsgemäß aufgebaut, um mithilfe einer HTTP GET-Anforderung an den LUIS-Endpunkt eine Vorhersage abzurufen. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

## <a name="unassign-resource"></a>Aufheben der Zuweisung einer Ressource
Wenn Sie die Zuweisung des Endpunktschlüssels aufheben, wird er nicht aus Azure gelöscht. Lediglich die Verknüpfung mit LUIS wird aufgehoben. 

Wenn die Zuweisung eines Endpunktschlüssels aufgehoben oder er der App nicht zugewiesen ist, wird auf alle Anforderungen an die Endpunkt-URL ein Fehler zurückgegeben: `401 This application cannot be accessed with the current subscription`. 

## <a name="include-all-predicted-intent-scores"></a>Einschließen aller prognostizierten Absichtsbewertungen
Über das Kontrollkästchen **Include all predicted intent scores** (Alle prognostizierten Absichtsbewertungen einschließen) können Sie bei der Endpunktabfrageantwort die Prognosebewertung für jede Absicht einbeziehen. 

Dank dieser Einstellung kann der Chatbot oder die LUIS aufrufende Anwendung eine programmgesteuerte Entscheidung treffen, die auf den Bewertungen der zurückgegebenen Absichten beruht. Im Allgemeinen sind die obersten zwei Absichten am interessantesten. Wenn die oberste Bewertung die Absicht „None“ ist, kann Ihr Chatbot eine Nachverfolgungsfrage stellen, die endgültig zwischen der Absicht „None“ und der anderen Absicht mit hoher Bewertung entscheidet. 

Die Absichten und ihre Bewertungen sind ebenfalls in den Endpunktprotokollen enthalten. Sie können diese Protokolle [exportieren](luis-how-to-start-new-app.md#export-app) und die Ergebnisse analysieren. 

```JSON
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

## <a name="enable-bing-spell-checker"></a>Aktivieren der Bing-Rechtschreibprüfung 
Wenn in den **URL-Einstellungen für den Endpunkt** der Ein-/Ausschalter **Bing spell checker** (Bing-Rechtschreibprüfung) aktiviert ist, kann LUIS falsch geschriebene Wörter vor der Vorhersage korrigieren. Erstellen Sie einen **[Bing-Rechtschreibprüfungs-Schlüssel](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. 

Fügen Sie den querystring-Parameter **spellCheck=true** und den **bing-spell-check-subscription-key={YOUR_BING_KEY_HERE}** hinzu. Ersetzen Sie `{YOUR_BING_KEY_HERE}` durch den Schlüssel Ihrer Bing-Rechtschreibprüfung.

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


## <a name="publishing-regions"></a>Veröffentlichungsregionen

Erfahren Sie mehr über die [Veröffentlichungsregionen](luis-reference-regions.md), einschließlich der Veröffentlichung in [Europa](luis-reference-regions.md#publishing-to-europe) und [Australien](luis-reference-regions.md#publishing-to-australia). Veröffentlichungsregionen unterscheiden sich von Erstellungsregionen. Erstellen Sie eine App in der Erstellungsregion, die der gewünschten Veröffentlichungsregion für den Abfrageendpunkt entspricht.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie Ihren Schlüssel zum Veröffentlichen Ihrer App auf der Seite **App veröffentlichen**. Anweisungen zum Veröffentlichen finden Sie unter [Veröffentlichen von Apps](luis-how-to-publish-app.md).

Weitere Informationen zum Verständnis der Konzepte zu LUIS-Erstellungs- und -Endpunktschlüsseln finden Sie unter [Schlüssel in LUIS](luis-concept-keys.md).