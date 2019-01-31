---
title: Attribute der Academic Graph-Entität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Entitätsattribute, die Sie mit einer Academic Graph-Instanz in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: f227cc03578adcfbf73fec3ae8941045e8352513
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55182998"
---
# <a name="entity-attributes"></a>Entitätsattribute

Der Academic Graph ist aus sieben Entitätstypen. Alle Entitäten haben eine Entitäts-ID und einen Entitätstyp.

## <a name="common-entity-attributes"></a>Allgemeine Entitätsattribute
NAME    |BESCHREIBUNG                |Type       | Vorgänge
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

