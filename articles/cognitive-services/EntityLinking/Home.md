---
title: API für den Entity Linking-Analysedienst | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die API für den Entity Linking-Analysedienst für Entitätsverknüpfungsaufgaben in Cognitive Services verwenden können.
services: cognitive-services
author: DavidLiCIG
manager: wkwok
ms.service: cognitive-services
ms.component: entity-linking-intelligence
ms.topic: article
ms.date: 03/28/2016
ms.author: davl
ms.openlocfilehash: 662295c42a421fe76892e6e5aac7b7e7178d6155
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35373003"
---
# <a name="entity-linking-intelligence-service-api"></a>API für den Entity Linking-Analysedienst

Willkommen beim Microsoft Entity Linking-Analysedienst, einem Webdienst, der Entwickler bei Aufgabe rund um die Entitätsverknüpfung unterstützen soll.

## <a name="entity-linking"></a>Entitätsverknüpfung

Manchmal kann ein Wort in verschiedenen Kontexten als benannte Entität, Verb oder andere Wortform innerhalb eines Satzes verwendet werden. Ein Beispiel: Das Wort „Zeit“ kann auch als benannte Entität noch zu zwei voneinander unterscheidbaren Entitäten gehören, wie etwa zur Wochenzeitung „Die Zeit“ oder zum Titel des Buchs von Stephen Hawking: „Eine kurze Geschichte der Zeit“. Der Entity Linking-Analysedienst erkennt und identifiziert in einem Absatz innerhalb eines Dokuments jede einzelne Entität basierend auf seinem Kontext.  

Die folgende Abbildung zeigt ein Beispiel für die Entitätsverknüpfung. Insbesondere bei der Verwendung von Wikipedia erkennt der Entity Linking-Analysedienst alle im Eingabetext genannten Entitäten und verknüpft sie entsprechend der Seiten-ID mit relevanten Referenzpunkten.

 ![Beispiel für die Entitätsverknüpfung für „Mars“](./Images/EntityLinkingSample1.png)
 
## <a name="get-started"></a>Erste Schritte
 
Um sich schnell über die grundlegenden Funktionen der Entitätsverknüpfung und den Abonnementprozess zu informieren, lesen Sie bitte unser Tutorial „Erste Schritte“.
[Erste Schritte mit der API für Entitätsverknüpfung in C#](GettingStarted.md)


