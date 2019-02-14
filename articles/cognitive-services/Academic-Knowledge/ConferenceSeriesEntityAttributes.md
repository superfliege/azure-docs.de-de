---
title: Attribute der Konferenzreihenentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Konferenzreihenentität verwenden können.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 38b4aa4c899668a68041f042ce6981ddd8c58219
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884918"
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
