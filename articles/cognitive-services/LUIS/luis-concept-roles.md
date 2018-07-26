---
title: Grundlegendes zur Verwendung von Rollen in musterbasierten Entitäten – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Rollen in einer musterbasierten Entität verwendet werden, um einen kontextabhängigen Entitätsuntertyp zu benennen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: diberry
ms.openlocfilehash: d2692cdce9da7428bd7b30c4feaf7347792618f5
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222702"
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

```
buy a ticket from {Location:origin} to {Location:destination}
```

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
