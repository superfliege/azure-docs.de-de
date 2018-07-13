---
title: Entitätsattribute der Konferenzreihe in der Academic Knowledge-API | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Attribute, die Sie für die Konferenzreihenentität in der Academic Knowledge-API in Cognitive Services verwenden können.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 332736c927bdaa00334546f626a6eabb8e11d3b5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372714"
---
# <a name="conference-series-entity"></a>Konferenzreihenentität

<sub> * Die folgenden Attribute sind spezifisch für die Konferenzreihenentität. (Ty = '3') </sub>

NAME    |BESCHREIBUNG                            |Typ       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
id      |Entitäts-ID                              |Int64      |Equals
CN      |Normalisierter Name der Konferenzreihe      |Zeichenfolge     |Equals
DCN     |Anzeigename der Konferenzreihe         |Zeichenfolge     |none
CC      |Gesamtzitatanzahl der Konferenzreihe         |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl der Konferenzreihe   |Int32      |none
F.FId   |Die mit der Konferenzreihe verknüpfte Entitäts-ID des Forschungsbereichs |Int64  | Equals
F.FN    |Der mit der Konferenzreihe verknüpfte Name des Forschungsbereichs  | Equals,<br/>StartsWith