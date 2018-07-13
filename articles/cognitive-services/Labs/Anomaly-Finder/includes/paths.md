---
title: Includedatei
description: Includedatei
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: a806cac410eb57e59dacb42da9be954b2f962956
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35375338"
---
<a name="paths"></a>
## <a name="paths"></a>Paths

<a name="anomalydetection-post"></a>
### <a name="detect-anomaly-points-for-the-time-series-data-points-requested"></a>Erkennen von Anomaliepunkten für die angeforderten Datenpunkte aus Zeitreihen
```
POST /anomalydetection
```


#### <a name="parameters"></a>Parameter

|Typ|NAME|BESCHREIBUNG|Schema|
|---|---|---|---|
|**Text**|**body**  <br>*erforderlich*|Die Datenpunkte der Zeitreihe und ggf. der Zeitraum|[Anforderung](#request)|


#### <a name="responses"></a>Antworten

|HTTP-Code|BESCHREIBUNG|Schema|
|---|---|---|
|**200**|Erfolgreiche Ausführung|< [response](#response) > Array|
|**400**|Die JSON-Anforderung kann nicht analysiert werden.|Kein Inhalt|
|**403**|Das angegebene Zertifikat wird vom Server nicht akzeptiert.|Kein Inhalt|
|**405**|Methode unzulässig|Kein Inhalt|
|**500**|Interner Serverfehler.|Kein Inhalt|


#### <a name="consumes"></a>Nutzt

* `application/json`


#### <a name="produces"></a>Erstellt

* `application/json`


#### <a name="tags"></a>Tags

* anomalydetection



