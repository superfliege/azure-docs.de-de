---
title: Bildklassifizierung mit dem Azure Machine Learning-Paket für maschinelles Sehen (AMLPCV) und dem Team Data Science-Prozess (TDSP) | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Verwendung von TDSP (Team Data Science-Prozess) und AMLPCV für die Bildklassifizierung.
services: machine-learning, team-data-science-process
documentationcenter: ''
author: xibingao
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 18/05/2018
ms.author: xibingao
ms.openlocfilehash: cfb49f08a245fc08ba3fe78333e801ea2d358c4a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34367942"
---
# <a name="skin-cancer-image-classification-with-azure-machine-learning-aml-package-for-computer-vision-amlpcv-and-team-data-science-process-tdsp"></a>Bildklassifizierung bei Hautkrebs mit dem Azure Machine Learning-Paket für maschinelles Sehen (AMLPCV) und dem Team Data Science-Prozess (TDSP)

## <a name="introduction"></a>Einführung

In diesem Artikel erfahren Sie, wie Sie das [Azure Machine Learning-Paket für maschinelles Sehen](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) (Azure Machine Learning Package for Computer Vision, AMLPCV) zum Trainieren, Testen und Bereitstellen eines Modells zur **Bildklassifizierung** verwenden. Das Beispiel verwendet TDSP-Struktur und -Vorlagen in [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation). Das vollständige Beispiel wird in dieser exemplarischen Vorgehensweise bereitgestellt. Als Deep Learning-Framework wird [CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/) verwendet, und das Training wird auf einem GPU-Computer wie der [Data Science VM](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview) durchgeführt. Die Bereitstellung erfolgt über die Azure ML Operationalization CLI.

Viele Anwendungen im Bereich maschinelles Sehen können als Bildklassifizierungsproblem gestaltet werden. Hierzu gehört das Erstellen von Modellen, die Fragen wie „Ist ein Objekt im Bild vorhanden?“ beantworten. (Bei dem Objekt kann es sich z.B. um einen *Hund*, ein *Auto* oder ein *Schiff* handeln.) Auch komplexere Frage sind möglich: „Welche Augenkrankheit und welchen Schweregradklasse zeigt der Netzhautscan des Patienten?“. AMLPCV optimiert die Pipeline für die Datenverarbeitung und -modellierung bei der Bildklassifizierung. 

## <a name="link-to-github-repository"></a>Link zum GitHub-Repository
Dieser Artikel bietet eine Übersicht über das Beispiel. Eine ausführlichere Dokumentation finden Sie auf der [GitHub-Website](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-tdsp-walkthrough-with-amlpcv"></a>Team Data Science-Prozess (TDSP) – exemplarische Vorgehensweise mit AMLPCV

Diese exemplarische Vorgehensweise verwendet den Lebenszyklus des [Team Data Science-Prozesses (TDSP)](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview).

Die exemplarische Vorgehensweise umfasst die folgenden Schritte im Lebenszyklus:

### <a name="1-data-acquisionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Datenabruf](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
Für den Bildklassifizierungstask wird ein ISIC-Dataset verwendet. ISIC (The International Skin Imaging Collaboration) ist eine Partnerschaft zwischen Wissenschaft und Industrie, um die Erfassung digitaler Hautbilder zu vereinfachen und die Mortalität von Melanomen zu senken. Das [ISIC-Archiv](https://isic-archive.com/#images) enthält über 13.000 Bilder von gutartigen und bösartigen Hautläsionen. Wir laden ein Beispiel der Bilder aus dem ISIC-Archiv herunter.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Modellierung](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
Zur Modellierung gehören die folgenden Schritte. 

<b>2.1. Dataseterstellung</b><br>

Um ein Datasetobjekt in AMLPCV zu generieren, geben Sie ein Stammverzeichnis für Bilder auf der lokalen Festplatte an. 

<b>2.2 Bildvisualisierung und Erstellung von Anmerkungen</b><br>

Visualisieren Sie die Bilder im Datasetobjekt, und korrigieren Sie bei Bedarf einige Bezeichnungen.

<b>2.3 Bilderweiterung</b><br>

Erweitern Sie ein Datasetobjekt mithilfe der Transformationen, die in der [imgaug](https://github.com/aleju/imgaug)-Bibliothek beschrieben werden.

<b>2.4 DNN-Modelldefinition (Deep Neural Network)</b><br>

Definieren Sie die Modellarchitektur, die im Trainingsschritt verwendet wird. In AMLPCV werden sechs verschiedene vorab trainierte Deep Neural Network-Modelle unterstützt: AlexNet, Resnet-18, Resnet-34, Resnet-50, Resnet-101 und Resnet-152.

<b>2.5 Klassifizierertraining</b><br>

Trainieren Sie die neuronalen Netzwerke mit standardmäßigen oder benutzerdefinierten Parametern.

<b>2.6 Auswertung und Visualisierung</b><br>

Dieser Schritt bietet Funktionen, mit denen Sie die Leistung des trainierten Modells anhand eines unabhängigen Testdatasets auswerten können. Zu den Auswertungsmetriken gehören Richtigkeit, Genauigkeit, Wiederholbarkeit und ROC-Kurve.

Diese Schritte werden detailliert im zugehörigen Jupyter Notebook erläutert. Wir stellen auch Richtlinien für die Optimierung der Parameter wie Lernrate, Mindestbatchgröße und Abbruchrate bereit, um die Modellleistung weiter zu verbessern.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Bereitstellung](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

Dieser Schritt operationalisiert das Modell, das im Modellierungsschritt erzeugt wurde. Hier werden die Voraussetzungen und Einrichtung der Operationalisierung eingeführt. Zum Schluss wird auch die Nutzung des Webdiensts erläutert. In diesem Tutorial erfahren Sie, wie Sie Deep Learning-Modelle mit AMLPCV erstellen und in Azure operationalisieren.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Einstieg finden Sie in der Dokumentation zum [Azure Machine Learning-Paket für maschinelles Sehen (AMLPCV)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) und zum [Team Data Science-Prozess (TDSP)](https://aka.ms/tdsp).


## <a name="references"></a>Referenzen

* [Azure Machine Learning-Paket für maschinelles Sehen (AMLPCV)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation):
* [Data Science-VM](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

