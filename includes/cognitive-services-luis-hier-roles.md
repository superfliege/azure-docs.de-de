---
title: Includedatei
description: Includedatei
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 10/23/2018
ms.author: diberry
ms.openlocfilehash: 0ad3b360611a12b95568e9a20f13a57c93b2e603
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51208303"
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
