---
title: Attribute der Academic Graph-Entität für die Academic Knowledge-API | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Entitätsattribute, die Sie mit einer Academic Graph-Instanz in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: f771969c3c6f05e5d2c99b1fbf593ae039ccb12c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372691"
---
# <a name="entity-attributes"></a>Entitätsattribute

Der Academic Graph ist aus sieben Entitätstypen. Alle Entitäten haben eine Entitäts-ID und einen Entitätstyp.

## <a name="common-entity-attributes"></a>Allgemeine Entitätsattribute
NAME    |BESCHREIBUNG                |Typ       | Vorgänge
------- | ------------------------- | --------- | ----------------------------
id      |Entitäts-ID                  |Int64      |Equals
Ty      |Entitätstyp                |enum   |Equals

## <a name="entity-type-enum"></a>Entitätstyp „enum“
NAME                                                            |value
----------------------------------------------------------------|-----
[Dokument](PaperEntityAttributes.md)                               |0
[Autor](AuthorEntityAttributes.md)                             |1
[Journal](JournalEntityAttributes.md)                           |2
[Konferenzreihe](JournalEntityAttributes.md)                 |3
[Konferenzinstanz](ConferenceInstanceEntityAttributes.md)    |4
[Zuordnung](AffiliationEntityAttributes.md)                   |5
[Forschungsbereich](FieldsOfStudyEntityAttributes.md)                      |6

