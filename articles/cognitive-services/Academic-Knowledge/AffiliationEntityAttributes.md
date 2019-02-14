---
title: Attribute der Zuordnungsentität in der Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Zuordnungsentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 82e6a5b66342e58e62da029d617cbd1d74c28149
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864365"
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
