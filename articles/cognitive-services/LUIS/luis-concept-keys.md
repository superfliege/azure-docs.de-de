---
title: Abonnementschlüssel
titleSuffix: Language Understadning - Azure Cognitive Services
description: 'LUIS verwendet zwei Schlüssel: den kostenlosen Erstellungsschlüssel zum Erstellen Ihres Modells und den berechneten Endpunktschlüssel für das Abfragen des Vorhersageendpunkts mit Benutzeräußerungen.'
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 6816fa3705348d07eced92c64e0c7020a08d01d5
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53132379"
---
# <a name="authoring-and-query-prediction-endpoint-keys-in-luis"></a>Erstellungsschlüssel und Endpunktschlüssel für Vorhersageabfragen in LUIS
LUIS verwendet zwei Schlüssel: [Erstellungs-](#programmatic-key) und [Endpunktschlüssel](#endpoint-key). Der Erstellungsschlüssel wird automatisch erstellt, wenn Sie Ihr LUIS-Konto erstellen. Wenn Sie Ihre LUIS-App veröffentlichen möchten, müssen Sie den [Endpunktschlüssel erstellen](luis-how-to-azure-subscription.md#create-luis-endpoint-key), ihn Ihrer LUIS-App [zuweisen](luis-how-to-manage-keys.md#assign-endpoint-key) und ihn [für Endpunktabfragen verwenden](#use-endpoint-key-in-query). 

|Schlüssel|Zweck|
|--|--|
|[Erstellungsschlüssel](#programmatic-key)|Erstellen, Veröffentlichen, Verwalten von Projektmitarbeitern, Versionsverwaltung|
|[Endpunktschlüssel](#endpoint-key)| Abfragen|

Es ist wichtig, LUIS-Apps in den [Regionen](luis-reference-regions.md#publishing-regions) zu erstellen, in denen sie auch veröffentlicht und abgefragt werden sollen.

<a name="programmatic-key" ></a>
## <a name="authoring-key"></a>Erstellungsschlüssel

Ein Erstellungsschlüssel wird auch als Startschlüssel bezeichnet und automatisch erstellt, wenn Sie ein LUIS-Konto erstellen. Er ist kostenlos. Sie verfügen über einen Erstellungsschlüssel für alle Ihre LUIS-Apps, die Sie in einer [Region](luis-reference-regions.md) erstellen. Der Erstellungsschlüssel wird zum Erstellen Ihrer LUIS-App oder zum Testen von Endpunktabfragen bereitgestellt. 

Sie finden den Erstellungsschlüssel, indem Sie sich bei [LUIS](luis-reference-regions.md#luis-website) anmelden. Klicken Sie auf der Navigationsleiste rechts oben auf den Kontonamen, um die **Kontoeinstellungen** zu öffnen.

![Erstellungsschlüssel](./media/luis-concept-keys/programatic-key.png)

Wenn Sie **Produktionsendpunkte abfragen** möchten, erstellen Sie das [LUIS-Abonnement](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) in Azure. 

> [!CAUTION]
> Der Einfachheit halber verwenden viele der Beispiele den Erstellungsschlüssel, da sein [Kontingent](luis-boundaries.md#key-limits) bereits einige Endpunktabfragen umfasst.  

## <a name="endpoint-key"></a>Endpunktschlüssel
 Wenn Sie **Produktionsendpunkte abfragen** möchten, erstellen Sie im Azure-Portal einen [LUIS-Schlüssel](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/). Merken Sie sich den zum Erstellen des Schlüssels verwendeten Namen, da Sie ihn benötigen, wenn Sie den Schlüssel zur App hinzufügen.

Nach Abschluss des LUIS-Abonnementvorgangs [weisen Sie den Schlüssel](luis-how-to-manage-keys.md#assign-endpoint-key) der App zu. 

Der Endpunktschlüssel umfasst ein Kontingent an Endpunkttreffern basierend auf dem Nutzungsplan, den Sie beim Erstellen des Schlüssels angegeben haben. Weitere Informationen zu den Preisen finden Sie unter [Cognitive Services-Preise](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h).

Der Endpunktschlüssel kann für alle LUIS-Apps oder für bestimmte LUIS-Apps verwendet werden. 

Verwenden Sie die Endpunktschlüssel nicht für das Erstellen von LUIS-Apps. 

## <a name="use-endpoint-key-in-query"></a>Verwenden des Endpunktschlüssels in Abfragen
Der LUIS-Endpunkt akzeptiert zwei Arten von Abfragen, die beide den Endpunktschlüssel verwenden, jedoch an verschiedenen Positionen:

|Verb|Beispiel-URL und Schlüsselposition|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`<br><br>Wert der Abfragezeichenfolge für `subscription-key`<br><br>Ändern Sie den Wert Ihrer Endpunktabfrage für den `subscription-key` vom Erstellungsschlüssel (Startschlüssel) zum neuen Endpunktschlüssel, um das Schlüsselkontingent des LUIS-Endpunkts zu verwenden. Wenn Sie den Schlüssel erstellen und zuweisen, aber nicht den Wert in der Endpunktabfrage für „subscription-key“ ändern, nutzen Sie nicht das Kontingent Ihres Endpunktschlüssels.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`<br><br> Headerwert für `Ocp-Apim-Subscription-Key`<br><br>Ändern Sie den Wert Ihrer Endpunktabfrage für den `Ocp-Apim-Subscription-Key` vom Erstellungsschlüssel (Startschlüssel) zum neuen Endpunktschlüssel, um das Schlüsselkontingent des LUIS-Endpunkts zu verwenden. Wenn Sie den Schlüssel erstellen und zuweisen, aber nicht den Wert in der Endpunktabfrage für `Ocp-Apim-Subscription-Key` ändern, nutzen Sie nicht das Kontingent Ihres Endpunktschlüssels.|

Die App-ID, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, die in den vorherigen URLs verwendet wurde, ist die öffentliche IoT-App für die [interaktive Demo](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). 

## <a name="api-usage-of-ocp-apim-subscription-key"></a>Verwenden von Ocp-Apim-Subscription-Key über die API
Die LUIS-APIs verwenden den Header `Ocp-Apim-Subscription-Key`. Der Headername ändert sich nicht, wenn Sie andere Schlüssel oder APIs verwenden. Legen Sie den Header für Erstellungs-APIs auf den Erstellungsschlüssel fest. Wenn Sie den Endpunkt verwenden, legen Sie den Header auf den Endpunktschlüssel fest. 

Sie können keinen Endpunktschlüssel an Erstellungs-APIs übergeben. Andernfalls wird der Fehler 401 ausgelöst: Der Zugriff wurde aufgrund eines ungültigen Endpunktschlüssels verweigert. 

## <a name="key-limits"></a>Schlüsselgrenzwerte
Lesen Sie unter [Schlüsselgrenzwerte](luis-boundaries.md#key-limits) und [Azure-Regionen](luis-reference-regions.md) nach. Der Erstellungsschlüssel ist kostenlos. Er wird für die Erstellung verwendet. Der LUIS-Endpunktschlüssel verfügt über einen kostenlosen Tarif. Er muss jedoch von Ihnen auf der Seite **Veröffentlichen** erstellt und Ihrer LUIS-App zugeordnet werden. Er kann nicht für das Erstellen, sondern nur für Endpunktabfragen verwendet werden.

Veröffentlichungsregionen unterscheiden sich von Erstellungsregionen. Stellen Sie sicher, dass Sie eine App in der Erstellungsregion erstellen, die der gewünschten Veröffentlichungsregion entspricht.

## <a name="key-limit-errors"></a>Fehler für Schlüsselgrenzwerte
Beim Überschreiten Ihres Kontingents pro Sekunde erhalten Sie den HTTP-Fehler 429. Beim Überschreiten Ihres Kontingents pro Monat erhalten Sie den HTTP-Fehler 403. Sie beheben diese Fehler, indem Sie den LUIS-[Endpunktschlüssel](#endpoint-key) abrufen und den Schlüssel auf der [LUIS](luis-reference-regions.md#luis-website)-Website auf der Seite **Veröffentlichen** der App [zuweisen](luis-how-to-manage-keys.md#assign-endpoint-key).

## <a name="automating-assignment-of-the-endpoint-key"></a>Automatisieren der Zuweisung des Endpunktschlüssels

Um den Endpunktschlüssel einer LUIS-App zuzuweisen, müssen Sie die LUIS-Website wegen der richtigen Erstellungs- und Veröffentlichungs[regionen](luis-reference-regions.md) verwenden. Es gibt **keine** automatisierte Methode für diesen Vorgang, unabhängig vom Mechanismus, etwa einem Azure Resource Manager-Skript, der Azure CLI, programmgesteuert mit dem SDK oder den APIs.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [Konzepte](luis-how-to-manage-keys.md#assign-endpoint-key) im Zusammenhang mit Erstellungs- und Endpunktschlüsseln.