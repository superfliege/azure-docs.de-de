---
title: Includedatei
description: Includedatei
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 04/30/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 2425d9455606f5eabba4b89cfa238b7a928be30e
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285742"
---
####

Die folgenden Grenzwerte gelten für Systemthemen und benutzerdefinierte Themen von Event Grid, *nicht* für Ereignisdomänen:

| Ressource | Begrenzung |
| --- | --- |
| Benutzerdefinierte Themen pro Azure-Abonnement | 100 |
| Ereignisabonnements pro Thema | 500 |
| Veröffentlichungsrate für ein benutzerdefiniertes Thema (eingehend) | 5.000 Ereignisse pro Sekunde pro Thema |

####

Die folgenden Grenzwerte gelten ausschließlich für Ereignisdomänen:

| Ressource | Begrenzung |
| --- | --- |
| Themen pro Ereignisdomäne | 1.000 während der Public Preview-Phase |
| Ereignisabonnements pro Thema innerhalb einer Domäne | 50 während der Public Preview-Phase |
| Domänenbereich-Ereignisabonnements | 50 während der Public Preview-Phase |
| Veröffentlichungsrate für eine Ereignisdomäne (Eingang) | 5.000 Ereignisse pro Sekunde während der Public Preview-Phase |