---
title: Übersicht über das maschinelle Lernen des Custom Vision Service – Azure Cognitive Services | Microsoft-Dokumentation
description: Der Custom Vision Service ist ein Dienst der Microsoft Cognitive Services, mit dem Sie auf der Azure Platform benutzerdefinierte Bildklassifizierungen erstellen können.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: overview
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: de45fc399470a806fb7456cbbe936cecf659ee7c
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37087680"
---
# <a name="what-is-the-custom-vision-service"></a>Was ist der Custom Vision Service?

Der Custom Vision Service ist ein Dienst der Microsoft Cognitive Services, mit dem Sie benutzerdefinierte Bildklassifizierungen erstellen können. Sie können damit schnell und leicht eine Bildklassifizierung erstellen, bereitstellen und verbessern. Der Custom Vision Service stellt eine REST-API und eine Weboberfläche zum Hochladen Ihrer Bilder und zum Trainieren der Klassifizierung bereit.

## <a name="what-does-custom-vision-service-do-well"></a>Für welche Zwecke ist der Custom Vision Service gut geeignet?

Der Custom Vision Service funktioniert am besten, wenn das Element, das Sie klassifizieren möchten, in Ihrem Bild hervorsticht. 

Es sind nur wenige Bilder erforderlich, um eine Klassifizierung oder ein Erkennungsmodul zu erstellen. 50 Bilder pro Klasse reichen aus, um mit einem Prototyp zu starten. Die vom Custom Vision Service verwendeten Methoden sind Unterschieden gegenüber unempfindlich, sodass Sie mit nur wenigen Daten die Prototyperstellung durchführen können. Dies bedeutet, dass der Custom Vision Service weniger gut für Szenarien geeignet ist, in denen feine Unterschiede erkannt werden sollen. Ein Beispiel hierfür sind kleinere Risse oder Dellen bei der Qualitätssicherung.

## <a name="release-notes"></a>Versionsinformationen

### <a name="may-7-2018"></a>7. Mai 2018
- Einführung der Vorschauversion des Features für die Objekterkennung für Projekte mit eingeschränkter Testversion.
- Upgrade auf 2.0-APIs
- S0-Tarif auf bis zu 250 Tags und 50.000 Bilder erweitert. 
- Umfassende Back-End-Verbesserungen an der Machine Learning-Pipeline für Bildklassifizierungsprojekte. Projekte, die nach dem 27. April 2018 trainiert wurden, profitieren von diesen Updates.
- Modellexport für ONNX zur Verwendung mit Windows ML hinzugefügt.
- Modellexport für Dockerfile hinzugefügt. Hiermit können Sie die Artefakte zum Erstellen Ihrer eigenen Windows- oder Linux-Container herunterladen, z.B. eine DockerFile, ein TensorFlow-Modell und Dienstcode. 
- Für neu trainierte Modelle, die für TensorFlow in die Domänen „General (Compact)“ (Allgemein (Kompakt)) und „Landmark (Compact)“ (Wahrzeichen (Kompakt)) exportiert werden, [betragen die Mittelwerte jetzt (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample). Dies ist der Fall, um für alle Projekte eine übergreifende Konsistenz zu erzielen. 

### <a name="march-1-2018"></a>1. März 2018
- Kostenpflichtige Vorschauversion und Onboarding im Azure-Portal. Projekte können nun an Azure-Ressourcen mit einem F0- (Free) oder S0-Tarif (Standard) angefügt werden. Einführung von Projekten mit dem S0-Tarif, für die bis zu 100 Tags und 25.000 Bilder zulässig sind. 
- Back-End-Änderungen am Machine Learning-Pipeline-/Normalisierungsparameter. Kunden erhalten hiermit beim Anpassen des Wahrscheinlichkeitsschwellenwerts eine bessere Steuerungsmöglichkeit in Bezug auf den Precision-Recall-Kompromiss. Im Rahmen dieser Änderungen wurde der Standard-Wahrscheinlichkeitsschwellenwert im CustomVision.ai-Portal auf 50% festgelegt.

### <a name="december-19-2017"></a>19. Dezember 2017

- Export für Android (TensorFlow) hinzugefügt, zusätzlich zum zuvor veröffentlichten Export für iOS (CoreML.) Dies ermöglicht den Export eines trainierten kompakten Modells für die Offlineausführung in einer Anwendung.
- Domäne „Compact“ (Kompakt) für Einzelhandel und Wahrzeichen hinzugefügt, um hierfür den Modellexport zu ermöglichen.
- Version [1.2 Trainings-API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) und [1.1 Vorhersage-API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164) veröffentlicht. Aktualisierung der API-Unterstützung des Modellexports, neuer Vorhersagevorgang, bei dem keine Bilder unter „Vorhersagen“ gespeichert werden, und Einführung von Batchvorgängen für die Trainings-API.
- Optimierungen der Benutzeroberfläche, z.B. die Möglichkeit zum Anzeigen, welche Domäne zum Trainieren einer Iteration verwendet wurde.
- Aktualisierung von [C# SDK und Beispiel](https://github.com/Microsoft/Cognitive-CustomVision-Windows).

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Klassifizierung](getting-started-build-a-classifier.md)
