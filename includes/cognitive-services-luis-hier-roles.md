---
title: Includedatei
description: Includedatei
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 10/23/2018
ms.author: diberry
ms.openlocfilehash: dee31fca841ea309128681637cc41fa0fb1e7aea
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55480513"
---
## <a name="roles-versus-hierarchical-entities"></a>Rollen und hierarchische Entitäten

Soll ich eine hierarchische Entität oder ein Muster mit einer einfachen Entität mit Rollen verwenden? 

Das kommt darauf an. Sowohl Muster als auch Beispieläußerungen stellen die Äußerung eines Benutzers dar und gelten speziell für eine Absicht.  

Besitzen die Äußerungen kein klares Muster, verwenden Sie hierarchische Entitäten. 

|Hierarchische Entitäten|Einfache Entität mit Rollen|
|--|--|
|Verfügbar in Beispieläußerungen und Mustern der Absicht|Nur in Mustern verfügbar|
|Müssen in der Absicht Beispieläußerungen mit bezeichneten untergeordneten Elementen enthalten|Rollen können in der Absicht nicht bezeichnet werden|
|Benötigen unter Umständen **weitere** Beispieläußerungen in der Absicht|Sollte **weniger** Beispieläußerungen in der Absicht benötigen|
