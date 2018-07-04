---
title: Bildklassifizierung mit dem Azure Machine Learning-Paket für maschinelles Sehen und dem Team Data Science-Prozess (TDSP) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Verwendung des Team Data Science-Prozesses (TDSP) und dem Azure Machine Learning-Paket für maschinelles Sehen zur Bildklassifizierung.
services: machine-learning, team-data-science-process
documentationcenter: ''
author: xibingao
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: xibingao
ms.openlocfilehash: f9e88cfb7185845e96f287b39bebaaa24320f537
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300795"
---
# <a name="skin-cancer-image-classification-with-the-azure-machine-learning-package-for-computer-vision-and-team-data-science-process"></a>Bildklassifizierung bei Hautkrebs mit dem Azure Machine Learning-Paket für maschinelles Sehen und dem Team Data Science-Prozess

In diesem Artikel erfahren Sie, wie Sie das [Azure Machine Learning-Paket für maschinelles Sehen](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) zum Trainieren, Testen und Bereitstellen eines Modells zur *Bildklassifizierung* verwenden. Das Beispiel verwendet die TDSP-Struktur und -Vorlagen in [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation). In dieser exemplarischen Vorgehensweise wird das vollständige Beispiel bereitgestellt. Als Deep Learning-Framework wird [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) verwendet, und das Training wird auf einem GPU-Computer mit [Data Science Virtual Machine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview) durchgeführt. Die Bereitstellung erfolgt über die CLI für die Azure Machine Learning-Operationalisieurng.

Viele Anwendungen im Bereich maschinelles Sehen können als Bildklassifizierungsproblem gestaltet werden. Hierzu gehört das Erstellen von Modellen, die einfache Fragen wie „Ist ein Objekt im Bild vorhanden?“ beantworten. Dabei kann es sich bei dem Objekt z.B. um einen Hund, ein Auto oder ein Schiff handeln. Auch Antworten auf komplexere Frage sind möglich: „Welche Augenkrankheit und welchen Schweregradklasse zeigt der Netzhautscan des Patienten?“. Das Azure Machine Learning-Paket für maschinelles Sehen optimiert die Datenverarbeitung bei der Bildklassifizierung sowie die Modellierungspipeline. 

## <a name="link-to-the-github-repository"></a>Link zum GitHub-Repository
Bei diesem Artikel handelt es sich um eine Zusammenfassung zu dem Beispiel. Eine ausführlichere Dokumentation finden Sie auf der [GitHub-Website](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-walkthrough"></a>Exemplarische Vorgehensweisen für den Team Data Science-Prozess

Diese exemplarische Vorgehensweise verwendet den Lebenszyklus des [Team Data Science-Prozesses](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview). Die exemplarische Vorgehensweise umfasst die folgenden Schritte im Lebenszyklus.

### <a name="1-data-acquisitionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Datenerfassung](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
Für die Bildklassifizierung wird ein Datensatz der International Skin Imaging Collaboration (ISIC) verwendet. ISIC ist eine Partnerschaft zwischen Wissenschaft und Industrie, um die Erfassung digitaler Hautbilder zu vereinfachen und die Mortalität von Melanomen zu senken. Das [ISIC-Archiv](https://isic-archive.com/#images) enthält mehr als 13.000 Bilder von gutartigen und bösartigen Hautläsionen. Laden Sie ein Beispiel der Bilder aus dem ISIC-Archiv herunter.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Modellierung](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
Zur Modellierung gehören die folgenden Schritte.

#### <a name="dataset-creation"></a>Dataseterstellung

Generieren Sie ein Datasetobjekt mit dem Azure Machine Learning-Paket für maschinelles Sehen, indem Sie das Stammverzeichnis der Bilder auf dem lokalen Datenträger angeben. 

#### <a name="image-visualization-and-annotation"></a>Bildvisualisierung und Erstellung von Anmerkungen

Visualisieren Sie die Bilder im Datasetobjekt, und korrigieren Sie bei Bedarf die Bezeichnungen.

#### <a name="image-augmentation"></a>Bilderweiterung

Erweitern Sie ein Datasetobjekt mithilfe der Transformationen, die in der [imgaug](https://github.com/aleju/imgaug)-Bibliothek beschrieben werden.

#### <a name="dnn-model-definition"></a>DNN-Modelldefinition (Deep Neural Network)

Definieren Sie die Modellarchitektur, die im Trainingsschritt verwendet wird. Im Azure Machine Learning-Paket für maschinelles Sehen werden sechs verschiedene vorab trainierte Deep Neural Network-Modelle unterstützt: AlexNet, Resnet-18, Resnet-34, Resnet-50, Resnet-101 und Resnet-152.

#### <a name="classifier-training"></a>Klassifizierertraining

Trainieren Sie die neuronalen Netzwerke mit standardmäßigen oder benutzerdefinierten Parametern.

#### <a name="evaluation-and-visualization"></a>Auswertung und Visualisierung

Stellen Sie Funktionen bereit, mit denen Sie die Leistung des trainierten Modells anhand eines unabhängigen Testdatasets auswerten können. Zu den Auswertungsmetriken gehören Richtigkeit, Genauigkeit, Wiederholbarkeit und ROC-Kurve.

Die Schritte werden detailliert im zugehörigen Jupyter Notebook erläutert. Das Notebook stellt auch Richtlinien für die Optimierung der Parameter wie Lernrate, Mindestbatchgröße und Abbruchrate bereit, um die Modellleistung weiter zu verbessern.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Bereitstellung](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

Dieser Schritt operationalisiert das Modell, das im Modellierungsschritt erzeugt wurde. Hier werden die Voraussetzungen und das erforderliche Setup eingeführt. Außerdem wird auch die Nutzung des Webdiensts erläutert. In diesem Tutorial erfahren Sie, wie Sie Deep Learning-Modelle mit dem Azure Machine Learning-Paket für maschinelles Sehen erstellen und in Azure operationalisieren.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen finden Sie unter [Azure Machine Learning-Paket für maschinelles Sehen](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest).
- Erste Schritte werden in der Dokumentation zum [Team Data Science-Prozess](https://aka.ms/tdsp) erläutert.


## <a name="references"></a>Referenzen

* [Azure Machine Learning-Paket für maschinelles Sehen](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation)
* [Data Science Virtual Machine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

