---
title: Grundlegendes zur Verwendung von Rollen in musterbasierten Entitäten
titleSuffix: Azure Cognitive Services
description: Rollen sind benannte, kontextabhängige Untertypen einer Entität, die nur in Mustern verwendet wird. In der Äußerung „Kaufe ein Ticket von New York nach London“ sind beispielsweise sowohl „New York“ als auch „London“ Städte, die aber jeweils eine andere Bedeutung in dem Satz haben. „New York“ ist die Ausgangsstadt „London“ ist die Zielstadt.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: fea70e2b1912bde94e23dbd98fa933784d2a69ac
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140314"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Entitätsrollen in Mustern sind kontextabhängige Untertypen
Rollen sind benannte, kontextabhängige Untertypen einer Entität, die nur in [Mustern](luis-concept-patterns.md) verwendet wird.

In der Äußerung `buy a ticket from New York to London` sind beispielsweise sowohl „New York“ als auch „London“ Städte, die aber jeweils eine andere Bedeutung in dem Satz haben. „New York“ ist die Ausgangsstadt „London“ ist die Zielstadt. 

Rollen benennen diese Unterschiede:

|Entität|Rolle|Zweck|
|--|--|--|
|Standort|origin|Start des Flugs|
|Standort|Ziel|Ziel des Flugs|
|Vordefinierte datetimeV2|zu|Enddatum|
|Vordefinierte datetimeV2|from|Startdatum|

## <a name="how-are-roles-used-in-patterns"></a>Wie werden Rollen in Mustern verwendet?
In der Äußerungsvorlage eines Musters werden Rollen innerhalb der Äußerung verwendet: 

|Muster mit Entitätsrollen|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Rollensyntax in Mustern
Die Entität und die Rolle werden in Klammern `{}` eingeschlossen. Die Entität und die Rolle werden durch einen Doppelpunkt voneinander getrennt. 


[!include[H2 Roles versus hierarchical entities](../../../includes/cognitive-services-luis-hier-roles.md)] 

## <a name="roles-with-prebuilt-entities"></a>Rollen mit vordefinierten Entitäten

Verwenden Sie Rollen mit vordefinierten Entitäten, um verschiedenen Instanzen der vordefinierten Entität innerhalb einer Äußerung eine Bedeutung zuzuweisen. 

### <a name="roles-with-datetimev2"></a>Rollen mit „datetimeV2“

Die vordefinierte Entität „datetimeV2“ eignet sich hervorragend, um unterschiedlichste Datums- und Uhrzeitangaben in Äußerungen zu verstehen. Sie können zusätzlich zu den standardmäßig von der vordefinierten Entität verstandenen Datumsangaben und Datumsbereiche noch weitere hinzufügen. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie das Hinzufügen von [Rollen](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity) funktioniert.
