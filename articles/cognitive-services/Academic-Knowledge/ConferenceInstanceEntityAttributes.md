---
title: Attribute der Konferenzinstanzentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Konferenzinstanzentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 6111ad00044943f12b2e098c4fd07ffb40185799
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902411"
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