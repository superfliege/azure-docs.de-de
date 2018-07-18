---
title: Includedatei
description: Includedatei
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: df7326cb8e671d0f71924e813a1354dfef1e20c7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35375298"
---
Die mit dem erwarteten Wert und den standardmäßigen Ober- und Untergrenzen zurückgegebenen Daten. In der Praxis können Sie einen Parameter [sensitivity] (Empfindlichkeit) definieren und dann (ExpectedValue + sensitivity * UpperMargin) als Obergrenze und (ExpectedValue - sensitivity * LowerMargin) als Untergrenze verwenden, um den Anomaliepunkt ihrerseits zu optimieren. Der Wert von [sensitivity] sollte größer als 1 sein. Unten finden Sie einige Diagramme für die Optimierung.

> [!NOTE]
> Die Diagramme werden nicht von der Beispielanwendung erstellt. Sie werden von einem separaten Tool mit der Beispielanwendung erstellt.

![Optimieren: Empfindlichkeit = 1,0](../media/sensitivity_1.png)
![Optimieren: Empfindlichkeit = 1,5](../media/sensitivity_1.5.png)
![Optimieren: Empfindlichkeit = 2](../media/sensitivity_2.png)
![Optimieren: Empfindlichkeit = 3,5](../media/sensitivity_3.5.png)