---
title: Suchen der Endpunktregion mit C# in LUIS
titleSuffix: Azure Cognitive Services
description: Suchen Sie programmgesteuert mit einem Endpunktschlüssel und einer Anwendungs-ID für LUIS nach Veröffentlichungsregionen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 53c3d1abb24ae0d5b33a2a100dda07fd20ae92d1
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039631"
---
# <a name="find-endpoint-region-with-c"></a>Suchen der Endpunktregion mit C# 
Wenn Ihnen die LUIS-App-ID und die LUIS-Abonnement-ID bekannt sind, können Sie herausfinden, welche Region Sie für Endpunktabfragen verwenden müssen.

> [!NOTE] 
> Die vollständige C#-Lösung steht im [GitHub-Repository mit **LUIS-Beispielen**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/csharp/) zur Verfügung.

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

## <a name="c-class-code-to-find-region"></a>C#-Klassencode zum Suchen der Region
Die Konsolenanwendung nimmt die LUIS-App-ID und den Endpunktschlüssel entgegen und gibt alle zugeordneten Regionen zurück. Derzeit werden Endpunktschlüssel regionsspezifisch erstellt, sodass nur eine Region zurückgegeben werden sollte.

Einschließen der .NET-Bibliotheksabhängigkeiten:

[!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=1-6 "Add the dependencies")]

Fügen Sie diese benutzerdefinierte LUIS-Klasse hinzu, die zum Suchen des Bereichs erstellt wurde. Ersetzen Sie die Variablenwerte für `luisAppId` und `luisSubscriptionKey` durch Ihre eigenen Werte. Alle Regionen, die 401 zurückgeben, werden in die Debugkonsole geschrieben. 

[!code-csharp[Add the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=10-83 "Add the LUIS class")]

Dies ist ein Beispiel für das Aufrufen der benutzerdefinierten LUIS-Klasse in der Main-Methode der Konsolenanwendung:

[!code-csharp[Call the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=85-101 "Call the LUIS class")]

Wenn die Anwendung ausgeführt wird, wird in der Konsole die Region für die App-ID angezeigt.

![Screenshot der Konsolen-App mit der LUIS-Region](./media/find-region-csharp/console.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über LUIS-[Regionen](luis-reference-regions.md).
