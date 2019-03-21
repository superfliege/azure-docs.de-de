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
ms.openlocfilehash: 3803bc7f1e0da01fbaa8aa11bc6e8fc5c508b5ae
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56741028"
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
