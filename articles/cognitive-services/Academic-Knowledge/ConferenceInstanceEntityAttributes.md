---
title: Entitätsattribute der Konferenzinstanz in der Academic Knowledge-API | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Attribute, die Sie für die Konferenzinstanzentität in der Academic Knowledge-API in Cognitive Services verwenden können.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: ef2bca4346a4666905f3dfb7bd448720f3b0ef8b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372755"
---
# <a name="conference-instance-entity"></a>Konferenzinstanzentität

<sub> * Die folgenden Attribute sind spezifisch für die Konferenzinstanzentität. (Ty = '4') </sub>

NAME    |BESCHREIBUNG                            |Typ       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
id      |Entitäts-ID                              |Int64      |Equals
CIN     |Normalisierter Name der Konferenzinstanz ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |Zeichenfolge     |Equals
DCN     |Anzeigename der Konferenzinstanz ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})       |Zeichenfolge     |none
CIL     |Speicherort der Konferenzinstanz    |Zeichenfolge     |Equals,<br/>StartsWith
CISD    |Startdatum der Konferenzinstanz  |Datum       |Equals,<br/>IsBetween
CIED    |Enddatum der Konferenzinstanz    |Datum       |Equals,<br/>IsBetween
CIARD   |Fälligkeitsdatum für die Abstraktregistrierung der Konferenzinstanz  |Datum       |Equals,<br/>IsBetween
CISDD   |Fälligkeitsdatum für die Übermittlung der Konferenzinstanz     |Datum       |Equals,<br/>IsBetween
CIFVD   |Fälligkeitsdatum der endgültigen Version der Konferenzinstanz  |Datum       |Equals,<br/>IsBetween
CINDD   |Benachrichtigungsdatum der Konferenzinstanz   |Datum       |Equals,<br/>IsBetween
CD.T    |Titel des Konferenzinstanzereignisses   |Datum       |Equals,<br/>IsBetween
CD.D    |Datum des Konferenzinstanzereignisses    |Datum       |Equals,<br/>IsBetween
PCS.CN  |Name der Konferenzreihe der Instanz |Zeichenfolge     |Equals
PCS.CId |ID der Konferenzreihe der Instanz |Int64    |Equals
CC      |Gesamtzitatanzahl der Konferenzinstanz           |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl der Konferenzinstanz |Int32      |none


## <a name="extended-metadata-attributes"></a>Erweiterte Metadatenattribute ##

NAME    | BESCHREIBUNG               
--------|---------------------------    
FN      | Vollständiger Name der Konferenzinstanz