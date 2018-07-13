---
title: Grenzen und Kontingente für Custom Vision Service – Azure Cognitive Services | Microsoft-Dokumentation
description: Informationen zu den Grenzen und Kontingenten für den Custom Vision Service von Azure Cognitive Services
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 03/16/2018
ms.author: anroth
ms.openlocfilehash: 44666d5d7f2a51e4017c704205d21b1f6d06908c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377634"
---
# <a name="limits-and-quotas"></a>Grenzen und Kontingente

Es gibt drei Schlüsselebenen für Custom Vision Service. F0- und S0-Ressourcen können über das Azure-Portal abgerufen werden. Informationen zu den Preisen und Transaktionen finden Sie auf der [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).  Für F0-Projekte kann ein Upgrade auf S0-Projekte ausgeführt werden.

Projektressourcen, die eingeschränkten Testabonnements entstammen, werden Ihren Anmeldeinformationen für Custom Vision (ein AAD- oder ein MSA-Konto) hinzugefügt. Diese Ressourcen sollen nur für Testversionen des Diensts gültig sein, die nur von kurzer Dauer sind.  Konten, die im Rahmen einer frühen kostenlosen Vorschauversion erstellt wurden, bevor die Azure-Vorschauversionen (1. März 2018) veröffentlicht wurden, behalten die Kontingente für eingeschränkte Testversionen. 

||**Eingeschränkte Testversion**|**F0 (Azure)**|**S0 (Azure)**|
|-----|-----|-----|-----|
|Projekte|2|2|100|
|Trainingsbilder pro Projekt|5.000|5.000|50.000|
|Vorhersagen pro Monat|10.000 |10.000|Unbegrenzt|
|Tags pro Projekt|50|50|250|
|Iterationen |10|10|10|
|Mindestanzahl der gekennzeichneten Bilder pro Tag, Klassifizierung (> 50 empfohlen) |5|5|5|
|Mindestanzahl der gekennzeichneten Bilder pro Tag, Objekterkennung (> 50 empfohlen)|15|15|15|
|Dauer der Speicherung von Vorhersagebildern|30 Tage|30 Tage|30 Tage|
|[Vorhersagevorgänge](https://go.microsoft.com/fwlink/?linkid=865445) mit Speicher (Transaktionen pro Sekunde)|2|2|10|
|[Vorhersagevorgänge](https://go.microsoft.com/fwlink/?linkid=865445) ohne Speicher (Transaktionen pro Sekunde)|2|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (API-Aufrufe pro Sekunde)|2|2|10|
|[Sonstige API-Aufrufe](https://go.microsoft.com/fwlink/?linkid=865446) (Transaktionen pro Sekunde)|10|10|10|
|Maximale Bildgröße (Upload des Trainingsbilds) |6 MB|6 MB|6 MB|
|Maximale Bildgröße (Vorhersage)|4 MB|4 MB|4 MB|

Es wird erwartet, dass die Beschränkungen der *Anzahl der Bilder pro Projekt* und der *Anzahl der Tags pro Projekt* für S0-Projekte mit der Zeit ansteigen. 
