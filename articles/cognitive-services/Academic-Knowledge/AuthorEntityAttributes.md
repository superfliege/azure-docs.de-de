---
title: Attribute der Autorentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Autorentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 62e6da6d558a0494fb83115b1e307081099399d4
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900687"
---
# <a name="author-entity"></a>Autorentität
<sub> * Die folgenden Attribute sind spezifisch für die Autorentität. (Ty = '1') </sub>

NAME    |BESCHREIBUNG                            |Typ       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
id      |Entitäts-ID                              |Int64      |Equals
AuN     |Normalisierter Autorname                 |Zeichenfolge     |Equals
DAuN    |Autoranzeigename                    |Zeichenfolge     |none
CC      |Gesamtzitatanzahl des Autors            |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl des Autors  |Int32      |none
E       |Erweiterte Metadaten (siehe Tabelle „Erweiterte Metadatenattribute“)  |Zeichenfolge     |none  


## <a name="extended-metadata-attributes"></a>Erweiterte Metadatenattribute ##

NAME    | BESCHREIBUNG               
--------|---------------------------    
LKA.Afn     | Der mit dem Autoren verknüpfte Anzeigename der Zuordnung  
LKA.AfId        | Die mit dem Autoren verknüpfte Entitäts-ID der Zuordnung