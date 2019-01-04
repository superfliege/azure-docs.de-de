---
title: Verwalten von Schlüsseln
titleSuffix: Language Understanding - Azure Cognitive Services
description: Weisen Sie nach dem Erstellen eines LUIS-Endpunktschlüssels im Azure-Portal den Schlüssel der LUIS-App zu, und rufen Sie die richtige Endpunkt-URL ab. Verwenden Sie diese Endpunkt-URL, um LUIS-Vorhersagen zu erhalten.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 0a278ded7ce290347645f345e4eee0b15972787f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53088101"
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
<a name="assign-resource"></a>


## <a name="assign-resource-in-luis-portal"></a>Zuweisen von Ressourcen im LUIS-Portal

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

### <a name="unassign-resource"></a>Aufheben der Zuweisung einer Ressource
Wenn Sie die Zuweisung des Endpunktschlüssels aufheben, wird er nicht aus Azure gelöscht. Lediglich die Verknüpfung mit LUIS wird aufgehoben. 

Wenn die Zuweisung eines Endpunktschlüssels aufgehoben oder er der App nicht zugewiesen ist, wird auf alle Anforderungen an die Endpunkt-URL ein Fehler zurückgegeben: `401 This application cannot be accessed with the current subscription`. 

### <a name="include-all-predicted-intent-scores"></a>Einschließen aller prognostizierten Absichtsbewertungen
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

### <a name="enable-bing-spell-checker"></a>Aktivieren der Bing-Rechtschreibprüfung 
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


### <a name="publishing-regions"></a>Veröffentlichungsregionen

Erfahren Sie mehr über die [Veröffentlichungsregionen](luis-reference-regions.md), einschließlich der Veröffentlichung in [Europa](luis-reference-regions.md#publishing-to-europe) und [Australien](luis-reference-regions.md#publishing-to-australia). Veröffentlichungsregionen unterscheiden sich von Erstellungsregionen. Erstellen Sie eine App in der Erstellungsregion, die der gewünschten Veröffentlichungsregion für den Abfrageendpunkt entspricht.

## <a name="assign-resource-without-luis-portal"></a>Zuweisen von Ressourcen ohne das LUIS-Portal

Für die Automatisierung, z.B. mit einer CI/CD-Pipeline, empfiehlt es sich, die Zuweisung einer LUIS-Ressource zu einer LUIS-App zu automatisieren. Die folgenden Schritte müssen dazu ausgeführt werden:

1. Rufen Sie ein Azure Resource Manager-Token von dieser [Website](https://resources.azure.com/api/token?plaintext=true) ab. Dieses Token läuft ab, daher sollten Sie es sofort verwenden. Die Anforderung gibt ein Azure Resource Manager-Token zurück.

    ![Anfordern und Empfangen eines Azure Resource Manager-Tokens](./media/luis-manage-keys/get-arm-token.png)

1. Verwenden Sie das Token zum Anfordern der LUIS-Ressourcen aus verschiedenen Abonnements über die [API zum Abrufen von LUIS-Azure-Konten](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), auf die Ihr Benutzerkonto Zugriff hat. 

    Diese POST-API erfordert folgende Einstellungen:

    |Header|Wert|
    |--|--|
    |`Authorization`|Der Wert von `Authorization` ist `Bearer {token}`. Beachten Sie, dass dem Tokenwert das Wort `Bearer` und ein Leerzeichen vorangestellt werden müssen.| 
    |`Ocp-Apim-Subscription-Key`|Ihr [Erstellungsschlüssel](luis-how-to-account-settings.md).|

    Diese API gibt ein Array von JSON-Objekten aus Ihren LUIS-Abonnements, einschließlich Abonnement-ID, Ressourcengruppe und Ressourcennamen als Kontonamen zurück. Finden Sie im Array das Element, das die LUIS-Ressource darstellt, die der LUIS-App zugewiesen werden soll. 

1. Weisen Sie das Token mit der API zum [Zuweisen eines LUIS-Azure-Kontos zu einer Anwendung](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515) der LUIS-Ressource zu. 

    Diese POST-API erfordert folgende Einstellungen:

    |Typ|Einstellung|Wert|
    |--|--|--|
    |Header|`Authorization`|Der Wert von `Authorization` ist `Bearer {token}`. Beachten Sie, dass dem Tokenwert das Wort `Bearer` und ein Leerzeichen vorangestellt werden müssen.|
    |Header|`Ocp-Apim-Subscription-Key`|Ihr [Erstellungsschlüssel](luis-how-to-account-settings.md).|
    |Header|`Content-type`|`application/json`|
    |Abfragezeichenfolge|`appid`|die LUIS-App-ID 
    |Body||{"AzureSubscriptionId":"ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup-2",<br>"AccountName": "luis-uswest-S0-2"}|

    Wenn diese API erfolgreich ist, wird der Status „201 – erstellt“ zurückgegeben. 

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie Ihren Schlüssel zum Veröffentlichen Ihrer App auf der Seite **App veröffentlichen**. Anweisungen zum Veröffentlichen finden Sie unter [Veröffentlichen von Apps](luis-how-to-publish-app.md).

Weitere Informationen zum Verständnis der Konzepte zu LUIS-Erstellungs- und -Endpunktschlüsseln finden Sie unter [Schlüssel in LUIS](luis-concept-keys.md).
