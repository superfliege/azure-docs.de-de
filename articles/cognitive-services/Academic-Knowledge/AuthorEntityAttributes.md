---
title: Attribute der Autorentität in der Academic Knowledge-API | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Attribute, die Sie für die Autorentität in der Academic Knowledge-API in Cognitive Services verwenden können.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: afcc3257c49522a4631c4aae0e0c2b88373f9af1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372699"
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