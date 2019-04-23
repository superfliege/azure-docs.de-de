---
title: Glossar – Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Glossar für Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: anroth
ms.openlocfilehash: 3a34af77a2806ceb56e939e2b153f2e68bba61cd
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59048935"
---
# <a name="glossary-of-terms-for-custom-vision-service"></a>Glossar für Custom Vision Service

Hier finden Sie einige Begriffe, die häufig in Custom Vision Service verwendet werden:

## <a name="classifier"></a>Klassifizierung

Eine Klassifizierung ist ein Modell, das Sie mithilfe von Custom Vision Service unter Verwendung einiger Trainingsbilder erstellen. Nach Abschluss des Trainings einer neuen Klassifizierung erhalten Sie einen Auswertungsendpunkt (HTTPS), den Sie Ihrer App hinzufügen können. Jede Klassifizierung, die Sie erstellen, befindet sich in ihrem eigenen Projekt, und Sie können alle Projekte anzeigen, nachdem Sie sich angemeldet haben.

## <a name="domain"></a>Domäne

Bei der Projekterstellung wählen Sie eine Domäne für das Projekt aus. Die Domäne optimiert eine Klassifizierung für eine bestimmte Art von Objekt in Ihren Bildern. Wenn es in Ihrem Szenario also beispielsweise darum geht, Bilder von Apfelkuchen und Bilder von Karottenkuchen zu klassifizieren, wählen Sie die Domäne für Lebensmittel. Sollten Sie nicht genau wissen, welche Domäne Sie wählen sollen, wählen Sie die allgemeine Domäne.

- **Die Domäne für Nahrungsmittel.** Optimiert für Speisen, die sich beispielsweise auf der Speisekarte eines Restaurants befinden können. Die Domäne ist nicht für die Erkennung von einzelnem Obst oder Gemüse optimiert. Wenn Sie Fotos von einzelnem Obst oder Gemüse klassifizieren möchten, verwenden Sie die allgemeine Domäne.
- **Die Domäne für Wahrzeichen.** Optimiert für erkennbare Wahrzeichen (Naturdenkmäler oder künstlich geschaffene Wahrzeichen). Diese Domäne funktioniert am besten, wenn das Wahrzeichen auf dem Foto gut sichtbar ist (und sogar, wenn es leicht von einer Personengruppe im Vordergrund verdeckt wird).
- **Die Domäne für Einzelhandel.** Optimiert für die Klassifizierung von Bildern in einem Warenkatalog oder auf einer Einkaufswebsite. Verwenden Sie die Einzelhandelsdomäne, um eine hohe Präzision bei der Klassifizierung von Kleidern, Hosen, Hemden und Ähnlichem zu erzielen.
- **Die allgemeine Domäne.** Bestens geeignet für ein breites Spektrum von Bildklassifizierungsaufgaben.

Durch **kompakte Domänen** generierte Modelle können mithilfe der Iterationsfunktion exportiert werden. Sie sind für die Einschränkungen der Echtzeitklassifizierung auf Mobilgeräten optimiert. Mit einer kompakten Domäne erstellte Klassifizierungen sind möglicherweise etwas ungenauer als eine Standarddomäne mit der gleichen Menge an Trainingsdaten. Aufgrund ihrer geringen Größe können sie jedoch nahezu in Echtzeit lokal ausgeführt werden. 

## <a name="evaluation"></a>Auswertung

Nachdem Sie Ihre Klassifizierung trainiert haben, können Sie unter Verwendung des automatisch generierten HTTPS-Endpunkts ein beliebiges Bild zur Auswertung übermitteln. Ihre Klassifizierung gibt eine nach Zuverlässigkeit sortierte Gruppe vorhergesagter Tags zurück.

## <a name="iteration"></a>Iteration

Bei jedem Training oder erneuten Training Ihrer Klassifizierung wird eine neue Iteration Ihres Modells erstellt. Wir bewahren mehrere der Iterationen auf, damit Sie Ihren Fortschritt im Zeitverlauf vergleichen können. Nicht mehr benötigte Iterationen können gelöscht werden. Das Löschen einer Iteration kann allerdings nicht mehr rückgängig gemacht werden. Außerdem werden dabei auch alle spezifischen Bilder und Änderungen dieser Iteration gelöscht. 

## <a name="precision"></a>Precision

Wenn Sie ein Bild klassifizieren, wie hoch ist die Wahrscheinlichkeit, dass Ihre Klassifizierung das Bild richtig klassifiziert? Wie viel Prozent aller Bilder, mit denen die Klassifizierung (Hunde und Ponys) trainiert wurde, hat das Modell korrekt erkannt? Bei 99 korrekten Tags und 100 Bildern ergibt sich eine Genauigkeit von 99 Prozent.

## <a name="predictions"></a>Vorhersagen

Wenn Ihre Klassifizierung neue zu klassifizierende Bilder akzeptiert, speichert sie die Bilder für Sie. Mit diesen Bildern können Sie die Genauigkeit Ihrer Klassifizierung verbessern, indem Sie falsch vorhergesagte Bilder mit den korrekten Tags versehen. Außerdem können Sie Ihre Klassifizierung mithilfe dieser neuen Bilder neu trainieren.

## <a name="recall"></a>Recall (Trefferquote)

Wie viele der Bilder, die korrekt klassifiziert werden sollten, wurden von Ihrer Klassifizierung korrekt identifiziert? Eine Trefferquote von 100 liegt beispielsweise vor, wenn in den Bildern, die zum Trainieren der Klassifizierung verwendet wurden, 38 Hundebilder enthalten waren und von der Klassifizierung 38 Hunde gefunden wurden.

## <a name="settings"></a>Einstellungen

Es gibt zwei Arten von Einstellungen: Einstellungen auf Projektebene und Einstellungen auf Benutzerebene.

- Einstellungen auf Projektebene:
  
  Einstellungen auf Projektebene gelten für ein Projekt oder eine Klassifizierung. Hierzu gehören folgende Berechtigungen:

   - Projektdomäne
   - Projektname
   - Projektbeschreibung
   - Syntax:
      - Anzahl von Trainingsbildern
      - Anzahl erstellter Tags
      - Anzahl erstellter Iterationen

- Einstellungen auf Benutzerebene: 
   - Abonnementschlüssel: einer für das Training, einer für die Auswertung/Vorhersage
   - Syntax:
      - Anzahl erstellter Projekte
      - Anzahl ausgeführter Aufrufe der Auswertungs-/Vorhersage-API

## <a name="tags"></a>Tags

Verwenden Sie Tags, um die Objekte in Ihren Trainingsbilder zu benennen. Wenn Sie eine Klassifizierung zur Identifizierung von Hunden und Ponys erstellen, versehen Sie Hundebilder mit dem Tag „Hund“, Ponybilder mit dem Tag „Pony“ und Bilder, auf denen sowohl ein Hund als auch ein Pony zu sehen ist, mit den Tags „Hund“ und „Pony“.

## <a name="training-image"></a>Trainingsbild

Zur Erstellung einer Klassifizierung mit hoher Genauigkeit benötigt Custom Vision Service mehrere Trainingsbilder. Ein Trainingsbild ist ein Foto des Bilds, das von Custom Vision Service klassifiziert werden soll. Wenn Sie also beispielsweise Orangen klassifizieren möchten, müssen Sie mehrere Bilder von Orangen in Custom Vision Service hochladen, damit der Dienst eine Klassifizierung für die Erkennung von Orangen erstellen kann. Wir empfehlen mindestens 30 Bilder pro Tag.

## <a name="workspace"></a>Arbeitsbereich

Ihr Arbeitsbereich enthält alle Ihre Trainingsbilder und spiegelt alle Änderungen aus Ihrer aktuellen Iteration wider (beispielsweise entfernte oder hinzugefügte Bilder). Wenn Sie Ihre Klassifizierung trainieren, erstellen Sie eine neue Iteration Ihrer Klassifizierung unter Verwendung der Bilder aus Ihrem Arbeitsbereich.