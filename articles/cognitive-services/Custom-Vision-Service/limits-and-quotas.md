---
title: Preise und Grenzwerte – Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Erfahren Sie über Grenzwerte und Kontingente für Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: anroth
ms.openlocfilehash: 58109e17ed33e6af8dedf3ed8c1cc9ddf546a05e
ms.sourcegitcommit: 89b5e63945d0c325c1bf9e70ba3d9be6888da681
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57588668"
---
# <a name="pricing-and-limits"></a>Preise und Einschränkungen

Es gibt drei Schlüsselebenen für Custom Vision Service. Projektressourcen, die eingeschränkten Testabonnements entstammen, werden Ihren Anmeldeinformationen für Custom Vision (ein Azure Active Directory- oder MSA-Konto) hinzugefügt. Diese Ressourcen sollen nur für Testversionen des Diensts gültig sein, die nur von kurzer Dauer sind. Sie können sich über das Azure-Portal für ein F0- (kostenlos) oder S0-Abonnement (Standard) registrieren. Auf der entsprechenden [Cognitive Services-Preisseite](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) finden Sie Informationen zu Preisen und Transaktionen.

Konten, die im Rahmen einer frühen kostenlosen Vorschauversion erstellt wurden, bevor die Azure-Vorschauversionen (1. März 2018) veröffentlicht wurden, behalten die Kontingente für eingeschränkte Testversionen.

Es wird erwartet, dass die Anzahl der Bilder pro Projekt und die Anzahl der Tags pro Projekt für S0-Projekte mit der Zeit ansteigen.

||**Eingeschränkte Testversion**|**F0**|**S0**|
|-----|-----|-----|-----|
|Projekte|2|2|100|
|Trainingsbilder pro Projekt, Klassifizierung|5.000|5.000|50.000|
|Trainingsbilder pro Projekt, Objekterkennung|5.000|5.000|10.000|
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


