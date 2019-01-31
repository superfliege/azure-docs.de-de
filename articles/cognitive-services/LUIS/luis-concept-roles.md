---
title: Rollen für Entitäten
titleSuffix: Azure Cognitive Services
description: Rollen sind benannte, kontextabhängige Untertypen einer Entität, die nur in Mustern verwendet wird. In der Äußerung `buy a ticket from New York to London` sind beispielsweise sowohl „New York“ als auch „London“ Städte, die aber jeweils eine andere Bedeutung in dem Satz haben. „New York“ ist die Ausgangsstadt „London“ ist die Zielstadt.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: diberry
ms.openlocfilehash: f5768012a03629190a65dd86d004dc842d99912e
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55215945"
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


[!INCLUDE [H2 Roles versus hierarchical entities](../../../includes/cognitive-services-luis-hier-roles.md)] 

## <a name="example-role-for-entities"></a>Beispielrolle für Entitäten

Eine Rolle ist lediglich eine kontextbezogen erlernte Platzierung einer Entität innerhalb einer Äußerung. Sie ist am effektivsten, wenn die Äußerung mehrere Vorkommen des Entitätstyps enthält. Das einfachste Beispiel für einen Entitätstyp ist die Unterscheidung zwischen einem Ziel- und einem Ursprungsort. Der Ort kann in verschiedensten Entitätstypen verwendet werden. 

Ein möglicher Anwendungsfall wäre etwa die Versetzung eines Mitarbeiters in eine andere Abteilung, wobei jede Abteilung ein Element in einer Liste ist. Beispiel:  

`Move [PersonName] from [Department:from] to [Department:to]`. 

In der zurückgegebenen Vorhersage werden beide Abteilungsentitäten in der JSON-Antwort zurückgegeben, und jede enthält den Rollennamen. 

## <a name="roles-with-prebuilt-entities"></a>Rollen mit vordefinierten Entitäten

Verwenden Sie Rollen mit vordefinierten Entitäten, um verschiedenen Instanzen der vordefinierten Entität innerhalb einer Äußerung eine Bedeutung zuzuweisen. 

### <a name="roles-with-datetimev2"></a>Rollen mit „datetimeV2“

Die vordefinierte Entität „datetimeV2“ eignet sich hervorragend, um unterschiedlichste Datums- und Uhrzeitangaben in Äußerungen zu verstehen. Sie können zusätzlich zu den standardmäßig von der vordefinierten Entität verstandenen Datumsangaben und Datumsbereiche noch weitere hinzufügen. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie das Hinzufügen von [Rollen](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity) funktioniert.
