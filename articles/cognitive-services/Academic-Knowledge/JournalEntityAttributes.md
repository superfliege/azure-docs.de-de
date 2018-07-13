---
title: Attribute der Journalentität in der Academic Knowledge-API | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Attribute, die Sie für die Journalentität in der Academic Knowledge-API in Cognitive Services verwenden können.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: e782c57b8ac57028e070c16382c53ec76666e94d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372690"
---
# <a name="journal-entity"></a>Journalentität

<sub> * Die folgenden Attribute sind spezifisch für die Journalentität. (Ty = '2') </sub>

NAME    |BESCHREIBUNG                            |Typ       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
id      |Entitäts-ID                              |Int64      |Equals
DJN     |Normalisierter Journalname                |Zeichenfolge     |none
JN      |Journalanzeigename                   |Zeichenfolge     |Equals
CC      |Gesamtzitatanzahl des Journals           |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl des Journals |Int32      |none