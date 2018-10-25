---
title: Attribute der Konferenzreihenentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Konferenzreihenentität verwenden können.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: f9f28afd7005d7a61aa0d2f4dba69ca598034b52
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900747"
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