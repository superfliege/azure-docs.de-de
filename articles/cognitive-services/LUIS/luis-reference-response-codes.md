---
title: LUIS-API (Language Understanding Intelligent Service) – HTTP-Antwortcodes – Azure | Microsoft-Dokumentation
titleSuffix: Azure
description: Es wird beschrieben, welche HTTP-Antwortcodes über die Erstellungs- und Endpunkt-APIs von LUIS zurückgegeben werden.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 04/16/2018
ms.author: diberry
ms.openlocfilehash: 5fd64b5fa3e3c084aee1e63c5233ccffc93917ae
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237937"
---
# <a name="luis-api-http-response-codes"></a>LUIS-API – HTTP-Antwortcodes
Die APIs für die Erstellung ([authoring](https://aka.ms/luis-authoring-apis)) und für Endpunkte ([endpoint](https://aka.ms/luis-endpoint-apis)) geben HTTP-Antwortcodes zurück. Antwortnachrichten enthalten spezifische Informationen zu einer Anforderung, aber es wird ein allgemeiner HTTP-Antwortstatuscode verwendet. 

## <a name="common-status-codes"></a>Häufige Statuscodes
In der folgenden Tabelle sind einige der häufigsten HTTP-Antwortstatuscodes für die [Erstellungs-](https://aka.ms/luis-authoring-apis) und [Endpunkt](https://aka.ms/luis-endpoint-apis)-APIs aufgeführt:

|Code|API|Erklärung|
|:--|--|--|
|400|Erstellung, Endpunkt|Die Parameter der Anforderung sind fehlerhaft, da die erforderlichen Parameter nicht vorhanden, falsch formatiert oder zu groß sind|
|400|Erstellung, Endpunkt|Der Text der Anforderung ist fehlerhaft, da der JSON-Code nicht vorhanden, falsch formatiert oder zu groß ist|
|401|Erstellen|Endpunkt-Abonnementschlüssel wurde anstelle des Erstellungsschlüssels verwendet|
|401|Erstellung, Endpunkt|Ungültig, falsch formatiert oder leerer Schlüssel|
|401|Erstellung, Endpunkt| Schlüssel stimmen nicht mit Region überein|
|401|Erstellen|Sie sind nicht der Besitzer oder Projektmitarbeiter|
|401|Erstellen|Ungültige Reihenfolge der API-Aufrufe|
|403|Erstellung, Endpunkt|Monatliche Kontingentgrenze für Schlüssel überschritten|
|409|Endpunkt|Anwendung wird noch geladen|
|410|Endpunkt|Anwendung muss neu trainiert und erneut veröffentlicht werden|
|414|Endpunkt|Abfrage überschreitet maximale Zeichenzahl|
|429|Erstellung, Endpunkt|Ratenlimit wurde überschritten (Anforderungen/Sekunde)|