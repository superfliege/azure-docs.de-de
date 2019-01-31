---
title: Attribute der Forschungsbereichsentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Forschungsbereichsentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: 793b35d9c6412c40a87f3f91fcd772476d57584f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55154566"
---
# <a name="field-of-study-entity"></a>Forschungsbereichsentität

<sub> * Die folgenden Attribute sind spezifisch für die Forschungsbereichsentität. (Ty = '6') </sub>

NAME    |BESCHREIBUNG                            |Type       | Vorgänge
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
