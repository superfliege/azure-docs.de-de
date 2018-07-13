---
title: Leitfaden für die API-Migration von v1 zu v2 | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie zum neuesten API-Satz migrieren.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: v-geberr
ms.openlocfilehash: 060baa6578f8a31234a1a667e2d591b92c39a06f
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264283"
---
# <a name="api-v2-migration-guide"></a>API v2-Migrationshandbuch
Die [Endpunkt](https://aka.ms/v1-endpoint-api-docs)- und [Erstellungs](https://aka.ms/v1-authoring-api-docs)-APIs der Version 1 sind demnächst veraltet. In diesem Handbuch erfahren Sie, wie Sie zu den [Endpunkt](https://aka.ms/luis-endpoint-apis)- und [Erstellungs](https://aka.ms/luis-authoring-apis)-APIs der Version 2 migrieren. 

## <a name="new-azure-regions"></a>Neue Azure-Regionen
LUIS stellt neue [Regionen](https://aka.ms/LUIS-regions) für die LUIS-APIs bereit. LUIS bietet eine eigene Website für Regionsgruppen. Die Anwendung muss in derselben Region erstellt werden, die Sie abfragen möchten. Regionen werden von Anwendungen nicht automatisch migriert. Sie exportieren die App aus einer Region und importieren sie dann in eine andere, damit sie in einer neuen Region verfügbar ist.

## <a name="authoring-route-changes"></a>Erstellen von Routenänderungen
Die Route der Erstellungs-API wurde von der Route **prog** zur Route **api** geändert.


| Version | Route |
|--|--|
|1|/luis/v1.0/**prog**/apps|
|2|/luis/**api**/v2.0/apps|


## <a name="endpoint-route-changes"></a>Endpunkt-Routenänderungen
Die Endpunkt-API weist neue QueryString-Parameter sowie eine andere Antwort auf. Wenn das verbose-Flag auf TRUE festgelegt ist, werden alle Absichten unabhängig von ihrer Bewertung, zusätzlich zur höchstbewerteten Absicht topScoringIntent in einem Array mit dem Namen „intents“ zurückgegeben.

| Version | GET-Route |
|--|--|
|1|/luis/v1/application?ID={App-ID}&q={q}|
|2|/luis/v2.0/apps/{App-ID}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&Bing-Rechtschreibprüfungs-Abonnementschlüssel][&log]|


Erfolgsantwort v1-Endpunkt:
```JSON
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

Erfolgsantwort v2-Endpunkt:
```JSON
{
  "query": "forward to frank 30 dollars through HSBC",
  "topScoringIntent": {
    "intent": "give",
    "score": 0.3964121
  },
  "entities": [
    {
      "entity": "30",
      "type": "builtin.number",
      "startIndex": 17,
      "endIndex": 18,
      "resolution": {
        "value": "30"
      }
    },
    {
      "entity": "frank",
      "type": "frank",
      "startIndex": 11,
      "endIndex": 15,
      "score": 0.935219169
    },
    {
      "entity": "30 dollars",
      "type": "builtin.currency",
      "startIndex": 17,
      "endIndex": 26,
      "resolution": {
        "unit": "Dollar",
        "value": "30"
      }
    },
    {
      "entity": "hsbc",
      "type": "Bank",
      "startIndex": 36,
      "endIndex": 39,
      "resolution": {
        "values": [
          "BankeName"
        ]
      }
    }
  ]
}
```

## <a name="key-management-no-longer-in-api"></a>Schlüsselverwaltung nicht mehr in der API
Abonnementschlüssel-APIs sind veraltet und geben den Fehler 410 (nicht mehr vorhanden) zurück.

| Version | Route |
|--|--|
|1|/luis/v1.0/prog/subscriptions|
|1|/luis/v1.0/prog/subscriptions/{Abonnementschlüssel}|

Azure-[Abonnementschlüssel](luis-how-to-azure-subscription.md) werden im Azure-Portal generiert. Sie Weisen den Schlüssel auf der Seite **[Veröffentlichen](manage-keys.md)** einer LUIS-App zu. Sie müssen den tatsächlichen Schlüsselwert nicht kennen. LUIS verwendet für die Zuweisung den Namen des Abonnements. 

## <a name="new-versioning-route"></a>Neue Versionsverwaltungsroute
Die v2-Modell ist nun in einer [Version](luis-how-to-manage-versions.md) enthalten. Ein Versionsname umfasst 10 Zeichen in der Route. Die Standardversion ist 0.1.

| Version | Route |
|--|--|
|1|/luis/v1.0/**prog**/apps/{App-ID}/entities|
|2|/luis/**api**/v2.0/apps/{App-ID}/**versions**/{Versions-ID}/entities|

## <a name="metadata-renamed"></a>Metadaten umbenannt
Mehrere APIs, die LUIS Metadaten zurückgeben, weisen neue Namen auf.

| v1-Routenname | v2-Routenname |
|--|--|
|PersonalAssistantApps |assistants|
|applicationcultures|cultures|
|applicationdomains|domains|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>„Beispiel“ in „Vorschlag“ umbenannt
LUIS schlägt Äußerungen von vorhandenen [Endpunktäußerungen](label-suggested-utterances.md) vor, die das Modell verbessern können. In der vorherigen Version wurde dies als **Beispiel** bezeichnet. In der neuen Version wurde diese Bezeichnung von „Beispiel“ in **Vorschlag** geändert. Auf der LUIS-Website wird dies **[Review endpoint utterances](https://docs.microsoft.com/azure/cognitive-services/LUIS/label-suggested-utterances)** (Endpunktäußerungen überprüfen) genannt.

| Version | Route |
|--|--|
|1|/luis/v1.0/**prog**/apps/{App-ID}/entities/{Entitäts-ID}/**sample**|
|1|/luis/v1.0/**prog**/apps/{App-ID}/intents/{Absichts-ID}/**sample**|
|2|/luis/**api**/v2.0/apps/{App-ID}/**versions**/{Versions-ID}/entities/{Entitäts-ID}/**suggest**|
|2|/luis/**api**/v2.0/apps/{App-ID}/**versions**/{Versions-ID}/intents/{Absichts-ID}/**suggest**|


## <a name="create-app-from-prebuilt-domains"></a>Erstellen einer App aus vordefinierten Domänen
[Vordefinierte Domänen](luis-how-to-use-prebuilt-domains.md) stellen ein vordefiniertes Domänenmodell bereit. Mit vordefinierten Domänen können Sie Ihre LUIS-Anwendung schnell für gebräuchliche Domänen entwickeln. Diese API ermöglicht es Ihnen, eine neue App basierend auf einer vordefinierten Domäne zu erstellen. Die Antwort ist die neue App-ID.

|v2-Route|Verb|
|--|--|
|/luis/api/v2.0/apps/customprebuiltdomains  |get, post|
|/luis/api/v2.0/apps/customprebuiltdomains/{Kultur}  |get|

## <a name="importing-1x-app-into-2x"></a>Importieren einer 1.x-App in 2.x
Der JSON-Code der exportierten 1.x-App JSON enthält einige Bereiche, die Sie vor dem Importieren in [LUIS][LUIS] 2.0 ändern müssen. 

### <a name="prebuilt-entities"></a>Vordefinierte Entitäten 
Die [vordefinierten Entitäten](Pre-builtEntities.md) wurden geändert. Stellen Sie sicher, dass Sie die vordefinierten Entitäten von v2 verwenden. Dazu gehört die Verwendung von [datetimeV2](pre-builtentities.md?#use-a-prebuilt-datetimev2-entity) anstelle von datetime. 

### <a name="actions"></a>Actions
Die actions-Eigenschaft ist nicht mehr gültig. Sie sollte leer sein. 

### <a name="labeled-utterances"></a>Bezeichnete Äußerungen
In v1 konnten bezeichnete Äußerungen Leerzeichen am Anfang oder Ende des Worts oder Ausdrucks enthalten. Die Leerzeichen wurden entfernt. 

## <a name="common-reasons-for-http-response-status-codes"></a>Häufige Ursachen für HTTP-Antwortstatuscodes
Weitere Informationen finden Sie unter [LUIS-API-Antwortcodes](luis-reference-response-codes.md).

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die v2-API-Dokumentation zum Aktualisieren vorhandener REST-Aufrufe an die LUIS-[Endpunkt](https://aka.ms/luis-endpoint-apis)- und [Erstellungs](https://aka.ms/luis-authoring-apis)-APIs. 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions