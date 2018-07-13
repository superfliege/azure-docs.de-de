---
title: Attribute der Forschungsbereichsentität in der Academic Knowledge-API | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Attribute, die Sie für die Forschungsbereichsentität in der Academic Knowledge-API in Cognitive Services verwenden können.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: a8176370f5b2f63b7741f7e892ed5a8c775f19c1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372707"
---
# <a name="field-of-study-entity"></a>Forschungsbereichsentität

<sub> * Die folgenden Attribute sind spezifisch für die Forschungsbereichsentität. (Ty = '6') </sub>

NAME    |BESCHREIBUNG                            |Typ       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
id      |Entitäts-ID                              |Int64      |Equals
FN      |Normalisierter Name des Forschungsbereichs         |Zeichenfolge     |Equals
DFN     |Anzeigename des Forschungsbereichs            |Zeichenfolge     |none
CC      |Gesamtzitatanzahl des Forschungsbereichs    |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl des Forschungsbereichs|Int32      |none
FL      |Ebene in der Hierarchie des Forschungsbereichs     |Int32      |Equals, <br/>IsBetween
FP.FN   |Name für übergeordneten Forschungsbereich             |Zeichenfolge     |Equals
FP.FId  |ID für übergeordneten Forschungsbereich               |Int64      |Equals
FC.FN   |Name für untergeordneten Forschungsbereich              |Zeichenfolge     |Equals
FC.FId  |ID für untergeordneten Forschungsbereich                |Int64      |Equals