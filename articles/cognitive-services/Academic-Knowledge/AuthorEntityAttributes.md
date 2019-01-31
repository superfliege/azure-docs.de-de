---
title: Attribute der Autorentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Autorentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 48758ac9ec8c993bbdb490229ae20fcce1fb0a49
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175178"
---
# <a name="author-entity"></a>Autorentität
<sub> * Die folgenden Attribute sind spezifisch für die Autorentität. (Ty = '1') </sub>

NAME    |BESCHREIBUNG                            |Type       | Vorgänge
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
