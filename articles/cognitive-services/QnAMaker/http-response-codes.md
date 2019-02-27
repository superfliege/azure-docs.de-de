---
title: API-HTTP-Antwortcodes – QnA Maker
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, welche HTTP-Antwortcodes über die QnA Maker-APIs zurückgegeben werden. Dies ist beim Beheben von Fehlern hilfreich.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/20/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 319623487f37308b5b8fe3fd107b01733825184d
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56454717"
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
|410|Diese API ist veraltet und nicht mehr verfügbar.|
