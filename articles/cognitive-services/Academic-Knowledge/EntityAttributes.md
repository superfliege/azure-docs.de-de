---
title: Attribute der Academic Graph-Entität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Entitätsattribute, die Sie mit einer Academic Graph-Instanz in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: a203fdf6562dabb1b9d6e8ab5bb8f46ff6d5dc27
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902789"
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

