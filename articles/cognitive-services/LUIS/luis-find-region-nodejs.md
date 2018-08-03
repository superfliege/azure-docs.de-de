---
title: Suchen von LUIS-Regionen mit Node.js in den LUIS-Einschränkungen (Language Understanding Intelligent Service) | Microsoft-Dokumentation
description: Suchen Sie programmgesteuert mit einem Endpunktschlüssel und einer Anwendungs-ID für LUIS nach Veröffentlichungsregionen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/01/2018
ms.author: diberry
ms.openlocfilehash: 66a34b5b611934ca531e7709b13e0c6d1c794206
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222510"
---
# <a name="region-can-be-determined-from-api-call"></a>Die Region kann über einen API-Aufruf ermittelt werden. 
Wenn Ihnen die LUIS-App-ID und die LUIS-Abonnement-ID bekannt sind, können Sie herausfinden, welche Region Sie für Endpunktabfragen verwenden müssen.

> [!NOTE] 
> Die vollständige Node.js-Lösung steht im [GitHub-Repository mit **LUIS-Beispielen**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/nodejs/) zur Verfügung.

## <a name="luis-endpoint-query-strategy"></a>LUIS-Endpunkt-Abfragestrategie
Für jede LUIS-Endpunktabfrage benötigen Sie Folgendes:

* Ein Endpunktschlüssel
* App-ID
* Region

Wenn die LUIS-Endpunktabfrage den richtigen Endpunkt und die richtige App-ID, aber die falsche Region verwendet, lautet der Antwortcode 401. Die 401-Anforderung wird nicht auf das Abonnementkontingent angerechnet. Aktivieren Sie diese Anforderung in eine Strategie zum Abrufen aller Regionen, um die richtige Region zu finden. Die richtige Region entspricht der einzigen Anforderung, die den Statuscode 2xx zurückgibt. 

|Antwortcode|Parameter|
|--|--|
|2xx|richtiger Endpunktschlüssel<br>richtige App-ID<br>richtige Hostregion|
|401|richtiger Endpunktschlüssel<br>richtige App-ID<br>_falsche_ Hostregion|

## <a name="nodejs-code-to-find-region"></a>Node.js-Code zum Suchen der Region
Die Konsolenanwendung nimmt die LUIS-App-ID und den Endpunktschlüssel entgegen und gibt alle zugeordneten Regionen zurück. Derzeit werden Endpunktschlüssel regionsspezifisch erstellt, sodass nur eine Region zurückgegeben werden sollte.

Einschließen der NPM-Abhängigkeiten:

[!code-javascript[Add the dependencies](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=5-6 "Add the dependencies")]

Fügen Sie Konstanten hinzu. Ersetzen Sie die Variablenwerte für `subscriptionKey` und `appId` durch Ihre eigenen Werte.  

[!code-javascript[Add constants](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=8-25 "Add constants")]

Fügen Sie die `searchRegions`-Funktion hinzu, um die Region zu suchen. Alle falschen Regionen geben den Code 401 zurück, der abgefangen und ignoriert wird.

[!code-javascript[Find region](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=27-37 "Find region")]

Aufrufen der `searchRegions`-Funktion und Zurückgeben einer einzelnen Region:

[!code-javascript[Call the function](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=39-43 "Call the function")]

Wenn die Anwendung ausgeführt wird, wird im Terminal die Region für die App-ID angezeigt.

![Screenshot der Konsolen-App mit der LUIS-Region](./media/find-region-nodejs/console.png)


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über LUIS-[Regionen](luis-reference-regions.md).
