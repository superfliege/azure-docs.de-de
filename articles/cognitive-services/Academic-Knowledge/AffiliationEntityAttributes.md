---
title: Attribute der Zuordnungsentität in der Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Zuordnungsentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 344b26b16f74cd44982e3c93fa69295792daa9a0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190630"
---
# <a name="affiliation-entity"></a>Zuordnungsentität

<sub> * Die folgenden Attribute sind spezifisch für die Zuordnungsentität. (Ty = '5') </sub>

NAME    |BESCHREIBUNG                            |Type       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
id      |Entitäts-ID                              |Int64      |Equals
AfN     |Normalisierter Name der Zuordnung        |Zeichenfolge     |Equals
DAfN    |Anzeigename der Zuordnung       |Zeichenfolge     |none
CC      |Gesamtzitatanzahl der Konferenzinstanz der Zuordnung           |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl der Zuordnung |Int32      |none

## <a name="extended-metadata-attributes"></a>Erweiterte Metadatenattribute ##

NAME    | BESCHREIBUNG               
--------|---------------------------    
PC      |Dokumentanzahl der Zuordnung
