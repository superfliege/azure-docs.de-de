---
title: Attribute der Forschungsbereichsentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Forschungsbereichsentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: 862fd6d506d5f1ca6f7f532f80f53a29200f33db
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900426"
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