---
title: Verwenden der vorgefertigten Cortana-App aus LUIS | Microsoft-Dokumentation
description: Verwenden des persönlichen Cortana-Assistenten, einer vorgefertigten Anwendung, aus LUIS (Language Understanding Intelligent Services).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: b792d090d037ef180258a1634d4bd063c0a71b9a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35375322"
---
# <a name="cortana-prebuilt-app"></a>Vorgefertigte Cortana-App

> [!IMPORTANT]
> Wir empfehlen die Verwendung der [vorgefertigten Domänen](./luis-how-to-use-prebuilt-domains.md) anstelle der vorgefertigten Cortana-App. Verwenden Sie beispielsweise anstelle von **builtin.intent.calendar.create_calendar_entry** **Calendar.Add** aus der vorgefertigten Domäne **Calendar**.
> Die vorgefertigten Domänen bieten diese Vorteile: 
> * Sie stellen Pakete vorgefertigter und bereits trainierter Absichten und Entitäten bereit, die für ein gutes Funktionieren bei der gemeinsamen Verwendung ausgelegt sind. Sie können eine vorgefertigte Domäne direkt in Ihre App integrieren. Wenn Sie z. B. einen Fitnesstracker erstellen möchten, können Sie die Domäne **Fitness** hinzufügen und haben einen vollständigen Satz von Absichten und Entitäten zum Nachverfolgen von Fitnessaktivitäten zur Verfügung, einschließlich Absichten zum Nachverfolgen des Gewichts und zur Ernährungsplanung, zur verbleibenden Zeit oder Strecke und zum Speichern von Anmerkungen zur Fitnessaktivität.
> * Die Absichten der vorgefertigten Domänen können angepasst werden. Wenn Sie beispielsweise Hotelrezensionen eingeben möchten, können Sie die Absicht **Places.GetReviews** aus der Domäne **Places** trainieren, um Anforderungen von Hotelrezensionen zu erkennen.
> * Die vorgefertigten Domänen lassen sich erweitern. Wenn Sie beispielsweise die vorgefertigte Domäne **Places** in einem Bot verwenden möchten, der nach Restaurants sucht, und eine Absicht für das Abrufen der gebotenen Art von Küche benötigen, können Sie eine Absicht **Places.GetCuisine** erstellen und trainieren.

Nicht nur können Sie eigene Anwendungen erstellen, LUIS stellt Ihnen darüber hinaus die Absichten und Entitäten des persönlichen Assistenten Microsoft Cortana als vorgefertigte App zur Verfügung. Das Verhalten dieser öffentlich erhältlichen LUIS-App kann nicht geändert werden. Die Absichten und Entitäten in dieser Anwendung können nicht bearbeitet oder in andere LUIS-Apps integriert werden. Wenn Sie möchten, dass Ihre Clientanwendung sowohl auf diese vorgefertigte Anwendung als auch auf Ihre eigene LUIS-Anwendung hat, muss Ihre Clientanwendung auf beide LUIS-Apps verweisen.

Die vorgefertigte persönliche Assistenten-App ist in diesen Kulturen (Gebietsschemas) verfügbar: Englisch, Französisch, Italienisch, Spanisch und Chinesisch.

## <a name="get-the-endpoint-for-the-cortana-prebuilt-app"></a>Abrufen des Endpunkts für die vorgefertigte Cortana-App

Sie können mithilfe der folgenden Endpunkte auf die vorgefertigte Cortana-App zugreifen: 

| Sprache | Endpunkt|
|--------| ------------------|
| Englisch| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821|
|    Chinesisch| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c27c4af7-d44a-436f-a081-841bb834fa29|
|    Französisch| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/0355ead1-2d08-4955-ab95-e263766e8392|
|    Spanisch| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/cb2675e5-fbea-4f8b-8951-f071e9fc7b38|
|    Italienisch| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/30a0fddc-36f4-4488-b022-03de084c1633|


> [!NOTE]
> Die Endpunkt-URLs stehen außerdem über die [Apps – persönliche Assistenten-Anwendungen abrufen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c32)-API zur Verfügung.

## <a name="try-out-the-personal-assistant-app"></a>Testen der persönlichen Assistenten-App
Um den Endpunkt aufzurufen, können Sie Ihr Abonnementschlüsselargument und Ihre Abfragezeichenfolge an den Endpunkt anfügen. 

Wenn beispielsweise die Äußerung, die Sie interpretieren möchten, „Erstelle einen Termin für eine Teambesprechung“ ist, können Sie diese Äußerung an die Endpunkt-URL anhängen. 

```
https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821?subscription-key={YOUR-SUBSCRIPTION-KEY}&q=create an appointment for team meeting
```

Sie können die URL in einen Webbrowser einfügen und anstelle des Felds `{YOUR-SUBSCRIPTION-KEY}` Ihren Abonnementschlüssel einsetzen.

Im Browser können Sie sehen, dass die vorgefertigte Cortana-App `builtin.intent.calendar.create_calendar_entry` als die Absicht, `builtin.calendar.title` als den Entitätstyp und `create an appointment for team meeting` als die Äußerung identifiziert.

![Verwenden der vorgefertigten Cortana-App](./media/luis-how-to-prebuilt-cortana/luis-prebuilt-cortana-browser.png)

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Referenz zur vorgefertigten Cortana-App](./luis-reference-cortana-prebuilt.md)

