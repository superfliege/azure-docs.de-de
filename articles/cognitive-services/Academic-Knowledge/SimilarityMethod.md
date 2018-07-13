---
title: similarity-Methode in der Academic Knowledge-API | Microsoft-Dokumentation
description: Verwenden Sie die similarity-Methode, um die akademische Ähnlichkeit von zwei Zeichenfolgen in Microsoft Cognitive Services zu berechnen.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 472498d6bfe06ae4477a30f892d44e79c901acf5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372754"
---
# <a name="similarity-method"></a>similarity-Methode

Die **similarity**-REST-API wird verwendet, um die akademische Ähnlichkeit zwischen zwei Zeichenfolgen zu berechnen. 
<br>

**REST-Endpunkt:**
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?
```

## <a name="request-parameters"></a>Anforderungsparameter
Parameter        |Datentyp      |Erforderlich | BESCHREIBUNG
----------|----------|----------|------------
**s1**        |Zeichenfolge   |Ja  |Zu vergleichende Zeichenfolge*
**s2**        |Zeichenfolge   |Ja  |Zu vergleichende Zeichenfolge*
<sub> * Zu vergleichende Zeichenfolgen haben eine maximale Länge von 1 MB. </sub>
<br>
## <a name="response"></a>response
NAME | BESCHREIBUNG
--------|---------
**SimilarityScore**        |Ein Gleitkommawert, der die Kosinusähnlichkeit von s1 und s2 darstellt, wobei Werte, die näher an 1,0 liegen, ähnlicher sind und Werte, die näher an -1,0 liegen, weniger bedeuten.
<br>

## <a name="successerror-conditions"></a>Erfolgs-/Fehlerbedingungen
HTTP-Status | Grund | response
-----------|----------|--------
**200**         |Erfolgreich | Gleitkommazahl
**400**         | Unzulässige Anforderung oder Anforderung ungültig | Fehlermeldung      
**500**         |Interner Serverfehler | Fehlermeldung
**Zeitüberschreitung**     | Timeout bei der Anforderung.  | Fehlermeldung
<br>
## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>Beispiel: Berechnen der Ähnlichkeit von zwei partiellen Abstrakten
#### <a name="request"></a>Anforderung:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?s1=Using complementary priors, we derive a fast greedy algorithm that can learn deep directed belief networks one layer at a time, provided the top two layers form an undirected associative memory
&s2=Deepneural nets with a large number of parameters are very powerful machine learning systems. However, overfitting is a serious problem in such networks
```
In diesem Beispiel generieren wir das Ähnlichkeitsergebnis zwischen zwei partiellen Abstrakten mithilfe der **similarity**-API.
#### <a name="response"></a>Antwort:
```
0.520
```
#### <a name="remarks"></a>Anmerkungen:
Das Ähnlichkeitsergebnis wird durch die Bewertung der akademischen Konzepte über Worteinbettung bestimmt. In diesem Beispiel bedeutet 0,52, die zwei partielle Abstrakte zumindest ähnlich sind.
<br>