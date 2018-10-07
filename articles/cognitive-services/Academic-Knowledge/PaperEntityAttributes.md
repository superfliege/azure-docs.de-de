---
title: Attribute der Artikelentität in der Academic Knowledge-API | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Attribute, die Sie für die Artikelentität in der Academic Knowledge-API in Cognitive Services verwenden können.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: 75efba2c2ce8842f233f766876ca00844338fb25
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094710"
---
# <a name="paper-entity"></a>Artikelentität

<sub> * Die folgenden Attribute sind spezifisch für die Artikelentität. (Ty = '0') </sub>


NAME    |BESCHREIBUNG                                        |Typ       | Vorgänge
------- | ------------------------------------------------- | --------- | ----------------------------
id      |Entitäts-ID                                          |Int64      |Equals
Ti      |Titel des Artikels                                        |Zeichenfolge     |Equals,<br/>StartsWith
L       |Sprachcode des Artikels, getrennt durch „\@@@“            |Zeichenfolge     |Equals
J       |Jahr des Artikels                                         |Int32      |Equals,<br/>IsBetween
D       |Datum des Artikels                                         |Datum       |Equals,<br/>IsBetween
CC      |Anzahl der Zitate                                     |Int32      |none  
ECC     |Geschätzte Zitatanzahl                           |Int32      |none
AA.AuN  |Name des Autors                                        |Zeichenfolge     |Equals,<br/>StartsWith
AA.AuId |ID des Autors                                          |Int64      |Equals
AA.AfN  |Name der Zugehörigkeit des Autors                            |Zeichenfolge     |Equals,<br/>StartsWith
AA.AfId |ID der Zugehörigkeit des Autors                              |Int64      |Equals
AA.S    |Reihenfolge des Autors für das Papier                         |Int32      |Equals
F.FN    |Name des Forschungsbereichs                                |Zeichenfolge     |Equals,<br/>StartsWith
F.FId   |ID des Forschungsbereichs                                  |Int64      |Equals
J.JN    |Name der Publikation                                       |Zeichenfolge     |Equals,<br/>StartsWith
J.JId   |ID der Publikation                                         |Int64      |Equals
C.CN    |Name der Konferenzreihe                             |Zeichenfolge     |Equals,<br/>StartsWith
C.CId   |ID der Konferenzreihe                               |Int64      |Equals
RId     |ID der referenzierten Artikel                              |Int64[]    |Equals
W       |Wörter aus dem Titel und der Zusammenfassung des Artikels                |String[]   |Equals
E       |Erweiterte Metadaten (siehe Tabelle unten)                |Zeichenfolge     |none  
        


## <a name="extended-metadata-attributes"></a>Erweiterte Metadatenattribute ##

NAME    | BESCHREIBUNG               
--------|---------------------------    
DN      | Anzeigename des Artikels 
S       | Quellen – Liste der Internetquellen des Artikels, sortiert nach statischem Rang
S.Ty    | Quellentyp (1:HTML, 2:Text, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS)
S.U     | Quell-URL
VFN     | Vollständiger Name des Veranstaltungsorts – vollständiger Name der Publikation oder Konferenz
VSN     | Kurzname des Veranstaltungsorts – Kurzname der Publikation oder Konferenz
V       | Band – Band der Publikation
BV      | Name der Publikation
BT      | 
PB      | Abkürzungen der Publikation
I       | Ausgabe – Ausgabe der Publikation
FP      | FirstPage – erste Seite des Artikels
LP      | LastPage – letzte Seite des Artikels
DOI     | Digitaler Objektbezeichner
CC      | Zitatkontexte – Liste der IDs der referenzierten Artikel und des entsprechenden Kontexts in dem Artikel (z. B. [{123: [„braune Füchse sind für ihre Sprünge bekannt, wie in Dokument 123 referenziert“, „die faulen Hunde sind historisch irreführend, wie in Artikel 123 gezeigt“]})
IA      | Umgekehrte Zusammenfassung
IA.IndexLength| Anzahl der Elemente im Index (Anzahl der Wörter in der Zusammenfassung)
IA.InvertedIndex| Liste der Wörter in der Zusammenfassung und ihre entsprechende Position in der ursprünglichen Zusammenfassung (z. B. [{„der“:[0, 15, 30]}, {„braune“:[1]}, {„Fuchs“:[2]}])
