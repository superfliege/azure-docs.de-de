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
ms.openlocfilehash: 443db1b4609e62fb7c57de417e42a2b4d0737ada
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553622"
---
Die folgenden Grenzwerte gelten für Systemthemen und benutzerdefinierte Themen von Azure Event Grid, *nicht* für Ereignisdomänen.

| Ressource | Begrenzung |
| --- | --- |
| Benutzerdefinierte Themen pro Azure-Abonnement | 100 |
| Ereignisabonnements pro Thema | 500 |
| Veröffentlichungsrate für ein benutzerdefiniertes Thema (eingehend) | 5.000 Ereignisse pro Sekunde pro Thema |

Die folgenden Grenzwerte gelten ausschließlich für Ereignisdomänen.

| Ressource | Begrenzung |
| --- | --- |
| Themen pro Ereignisdomäne | 1.000 während der Public Preview-Phase |
| Ereignisabonnements pro Thema innerhalb einer Domäne | 50 während der Public Preview-Phase |
| Domänenbereich-Ereignisabonnements | 50 während der Public Preview-Phase |
| Veröffentlichungsrate für eine Ereignisdomäne (Eingang) | 5.000 Ereignisse pro Sekunde während der Public Preview-Phase |