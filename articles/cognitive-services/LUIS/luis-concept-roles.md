---
title: Grundlegendes zur Verwendung von Rollen in musterbasierten Entitäten
titleSuffix: Azure Cognitive Services
description: Rollen sind benannte, kontextabhängige Untertypen einer Entität, die nur in Mustern verwendet wird. In der Äußerung „Kaufe ein Ticket von New York nach London“ sind beispielsweise sowohl „New York“ als auch „London“ Städte, die aber jeweils eine andere Bedeutung in dem Satz haben. „New York“ ist die Ausgangsstadt „London“ ist die Zielstadt.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 033e5e5e054b0a29961ad60e72b1466b51d1df0c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035197"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Entitätsrollen in Mustern sind kontextabhängige Untertypen
Rollen sind benannte, kontextabhängige Untertypen einer Entität, die nur in [Mustern](luis-concept-patterns.md) verwendet wird.

In der Äußerung `buy a ticket from New York to London` sind beispielsweise sowohl „New York“ als auch „London“ Städte, die aber jeweils eine andere Bedeutung in dem Satz haben. „New York“ ist die Ausgangsstadt „London“ ist die Zielstadt. 

Rollen benennen diese Unterschiede:

|Entität|Rolle|Zweck|
|--|--|--|
|Standort|origin|Start des Flugs|
|Standort|Ziel|Ziel des Flugs|

## <a name="how-are-roles-used-in-patterns"></a>Wie werden Rollen in Mustern verwendet?
In der Äußerungsvorlage eines Musters werden Rollen innerhalb der Äußerung verwendet: 

|Muster mit Entitätsrollen|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Rollensyntax in Mustern
Die Entität und die Rolle werden in Klammern `{}` eingeschlossen. Die Entität und die Rolle werden durch einen Doppelpunkt voneinander getrennt. 

## <a name="roles-versus-hierarchical-entities"></a>Rollen und hierarchische Entitäten
Hierarchische Entitäten bieten die gleichen Kontextinformationen wie Rollen, aber ausschließlich für Äußerungen in **Absichten**. Auf ähnliche Weise bieten Rollen die gleiche Kontextinformationen wie hierarchische Entitäten, jedoch ausschließlich in **Mustern**.

|Kontextabhängiges Lernen|Verwendet für|
|--|--|
|hierarchische Entitäten|Absichten|
|Rollen|Muster|

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie das Hinzufügen von [Rollen](luis-how-to-add-entities.md#add-role-to-pattern-based-entity) funktioniert.
