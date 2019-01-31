---
title: Attribute der Journalentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Journalentität in der Academic Knowledge-API in Cognitive Services verwenden können.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 397293c17320ef75daa5c31af76d49183b6c435c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55193827"
---
# <a name="journal-entity"></a>Journalentität

<sub> * Die folgenden Attribute sind spezifisch für die Journalentität. (Ty = '2') </sub>

NAME    |BESCHREIBUNG                            |Type       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
id      |Entitäts-ID                              |Int64      |Equals
DJN     |Normalisierter Journalname                |Zeichenfolge     |none
JN      |Journalanzeigename                   |Zeichenfolge     |Equals
CC      |Gesamtzitatanzahl des Journals           |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl des Journals |Int32      |none
