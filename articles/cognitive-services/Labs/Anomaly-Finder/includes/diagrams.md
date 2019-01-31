---
title: Includedatei
description: Includedatei
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 79ae38db73d55021572d04f693e5cb809e9bd056
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228966"
---
Die mit dem erwarteten Wert und den standardmäßigen Ober- und Untergrenzen zurückgegebenen Daten. In der Praxis können Sie einen Parameter [sensitivity] \(Empfindlichkeit) definieren und dann (ExpectedValue + sensitivity * UpperMargin) als Obergrenze und (ExpectedValue - sensitivity * LowerMargin) als Untergrenze verwenden, um den Anomaliepunkt ihrerseits zu optimieren. Der Wert von [sensitivity] sollte größer als 1 sein. Unten finden Sie einige Diagramme für die Optimierung.

> [!NOTE]
> Die Diagramme werden nicht von der Beispielanwendung erstellt. Sie werden von einem separaten Tool mit der Beispielanwendung erstellt.

![Optimieren: Empfindlichkeit = 1,0](../media/sensitivity_1.png)
![Optimieren: Empfindlichkeit = 1,5](../media/sensitivity_1.5.png)
![Optimieren: Empfindlichkeit = 2](../media/sensitivity_2.png)
![Optimieren: Empfindlichkeit = 3,5](../media/sensitivity_3.5.png)
