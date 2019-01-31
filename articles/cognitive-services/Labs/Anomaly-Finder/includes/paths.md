---
title: Includedatei
description: Includedatei
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 80503ad154a9fc4d01614ffd2816f9d5fd497fdb
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228892"
---
<a name="paths"></a>
## <a name="paths"></a>Paths

<a name="anomalydetection-post"></a>
### <a name="detect-anomaly-points-for-the-time-series-data-points-requested"></a>Erkennen von Anomaliepunkten für die angeforderten Datenpunkte aus Zeitreihen
```
POST /anomalydetection
```


#### <a name="parameters"></a>Parameter

|Type|NAME|BESCHREIBUNG|Schema|
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



