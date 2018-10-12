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
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: dd99218765c873120c4117a3ec1712fe0a605e66
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164586"
---
**Frage:** Soll ich eine hierarchische Entität oder ein Muster mit einer einfachen Entität mit Rollen verwenden? 

**Antwort:** Das kommt darauf an. Sowohl Muster als auch Beispieläußerungen stellen die Äußerung eines Benutzers dar und gelten speziell für eine Absicht.  

Besitzen die Äußerungen kein klares Muster, verwenden Sie hierarchische Entitäten. 

|Hierarchische Entitäten|Einfache Entität mit Rollen|
|--|--|
|Müssen Beispieläußerungen mit bezeichneten untergeordneten Elementen in Absichten enthalten.|Müssen Beispieläußerungen enthalten, **Rollen dürfen in Absichten nicht bezeichnet werden**.|
|Können in Mustern verwendet werden.|**Müssen** in Mustern verwendet werden.|
|Benötigen unter Umständen **weitere** Beispieläußerungen in der Absicht.|Benötigen unter Umständen **weniger Beispieläußerungen in der Absicht.|