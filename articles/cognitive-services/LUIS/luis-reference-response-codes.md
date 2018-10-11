---
title: LUIS-API (Language Understanding Intelligent Service) – HTTP-Antwortcodes – Azure | Microsoft-Dokumentation
titleSuffix: Azure
description: Es wird beschrieben, welche HTTP-Antwortcodes über die Erstellungs- und Endpunkt-APIs von LUIS zurückgegeben werden.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/16/2018
ms.author: diberry
ms.openlocfilehash: f005c7f13cd05ce3ca6ce494c4a2670106cb3637
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900980"
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