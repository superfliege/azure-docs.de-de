---
title: Attribute der Zuordnungsentität in der Academic Knowledge-API | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Attribute, die Sie für die Zuordnungsentität in der Academic Knowledge-API in Cognitive Services verwenden können.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: a0ec67cb811ca207b3d038028491da2516028f0b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372698"
---
# <a name="affiliation-entity"></a>Zuordnungsentität

<sub> * Die folgenden Attribute sind spezifisch für die Zuordnungsentität. (Ty = '5') </sub>

NAME    |BESCHREIBUNG                            |Typ       | Vorgänge
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