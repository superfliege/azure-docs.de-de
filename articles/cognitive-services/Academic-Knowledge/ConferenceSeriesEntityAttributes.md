---
title: Attribute der Konferenzreihenentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Konferenzreihenentität verwenden können.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: ff71b489cce01d8d6ea29e09905d7d3ac8429e34
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55155599"
---
# <a name="conference-series-entity"></a>Konferenzreihenentität

<sub> * Die folgenden Attribute sind spezifisch für die Konferenzreihenentität. (Ty = '3') </sub>

NAME    |BESCHREIBUNG                            |Type       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
id      |Entitäts-ID                              |Int64      |Equals
CN      |Normalisierter Name der Konferenzreihe      |Zeichenfolge     |Equals
DCN     |Anzeigename der Konferenzreihe         |Zeichenfolge     |none
CC      |Gesamtzitatanzahl der Konferenzreihe         |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl der Konferenzreihe   |Int32      |none
F.FId   |Die mit der Konferenzreihe verknüpfte Entitäts-ID des Forschungsbereichs |Int64  | Equals
F.FN    |Der mit der Konferenzreihe verknüpfte Name des Forschungsbereichs  | Equals,<br/>StartsWith
