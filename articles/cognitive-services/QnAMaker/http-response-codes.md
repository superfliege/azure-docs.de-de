---
title: API-HTTP-Antwortcodes – QnA Maker
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, welche HTTP-Antwortcodes über die QnA Maker-APIs zurückgegeben werden.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 10/09/2018
ms.author: tulasim
ms.openlocfilehash: 96782d32817e4989c02e0ed098d7772c80aa26c8
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079667"
---
# <a name="qna-maker-api-http-response-codes"></a>QnA Maker-API – HTTP-Antwortcodes
Die APIs für die [Verwaltung](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) und Vorhersage geben HTTP-Antwortcodes zurück. Antwortnachrichten enthalten spezifische Informationen zu einer Anforderung, aber es wird ein allgemeiner HTTP-Antwortstatuscode verwendet. 

### <a name="management"></a>Verwaltung

|Code|Erklärung|
|:--|--|
|2xx|Erfolgreich|
|400|Die Parameter der Anforderung sind fehlerhaft, da die erforderlichen Parameter nicht vorhanden, falsch formatiert oder zu groß sind|
|400|Der Text der Anforderung ist fehlerhaft, da der JSON-Code nicht vorhanden, falsch formatiert oder zu groß ist|
|401|Ungültiger Schlüssel|
|403|Verboten – Sie verfügen nicht über die richtigen Berechtigungen|
|404|KB nicht vorhanden|