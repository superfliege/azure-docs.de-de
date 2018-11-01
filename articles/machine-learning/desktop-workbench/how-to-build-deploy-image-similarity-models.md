---
title: Erstellen und Bereitstellen eines Ähnlichkeitsmodells für Bilder mithilfe des Azure Machine Learning-Pakets für maschinelles Sehen.
description: Hier erfahren Sie, wie Sie mithilfe des Azure Machine Learning-Pakets für maschinelles Sehen ein auf maschinellem Sehen basierendes Ähnlichkeitsmodell für Bilder erstellen, trainieren, testen und bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 05/20/2018
ROBOTS: NOINDEX
ms.openlocfilehash: b1b4e3e19adb38a69e16aaa98300972df3bb71a8
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093648"
---
# <a name="build-and-deploy-image-similarity-models-with-azure-machine-learning"></a>Erstellen und Bereitstellen von Ähnlichkeitsmodellen für Bilder mit Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


In diesem Artikel erfahren Sie, wie Sie das Azure Machine Learning-Paket für maschinelles Sehen (Azure Machine Learning Package for Computer Vision, AMLPCV) zum Trainieren, Testen und Bereitstellen eines Ähnlichkeitsmodells für Bilder verwenden. Eine Übersicht über dieses Paket und die ausführliche Referenzdokumentation finden Sie unter https://aka.ms/aml-packages/vision.

Eine große Anzahl von Problemen im Bereich maschinelles Sehen kann mithilfe von Bildähnlichkeiten gelöst werden. Diese Probleme beinhalten das Erstellen von Modellen, die z.B. die folgenden Fragen beantworten:
+ _Ist ein Objekt im Bild vorhanden? Beispielsweise ein „Hund“, „Auto“ oder „Schiff“ usw._
+ _Welche Augenkrankheit-Schweregradklasse zeigt der Netzhautscan des Patienten?_

Beim Erstellen und Bereitstellen dieses Modell mit AMLPCV durchlaufen Sie die folgenden Schritte:
1. Dataseterstellung
2. Bildvisualisierung und Erstellung von Anmerkungen
3. Bilderweiterung
4. DNN-Modelldefinition (Deep Neural Network)
5. Klassifizierertraining
6. Auswertung und Visualisierung
7. Webdienstbereitstellung
8. Webdienst-Auslastungstests

Das [CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/) wird als Deep Learning-Framework verwendet, das Training erfolgt lokal auf einem GPU-gestützten Computer wie der [Deep Learning Data Science-VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview), und die Bereitstellung verwendet die Azure ML Operationalization CLI.

## <a name="prerequisites"></a>Voraussetzungen

1. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

1. Die folgenden Konten und Anwendungen müssen eingerichtet und installiert werden:
   - Ein Azure Machine Learning-Experimentieren-Konto 
   - Ein Azure Machine Learning-Modellverwaltungskonto
   - Installation von Azure Machine Learning Workbench

   Wenn diese drei Aspekte noch nicht erstellt oder installiert wurden, lesen und befolgen Sie den Artikel [Azure Machine Learning: Schnellstart und Workbench-Installation](../desktop-workbench/quickstart-installation.md). 

1. Das Azure Machine Learning-Paket für maschinelles Sehen muss installiert sein. Eine Installationsanleitung für dieses Paket finden Sie unter https://aka.ms/aml-packages/vision.

## <a name="sample-data-and-notebook"></a>Beispieldaten und Notebook

### <a name="get-the-jupyter-notebook"></a>Abrufen des Jupyter Notebooks

Laden Sie das Notebook herunter, um das hier beschriebene Beispiel selbst auszuführen.

> [!div class="nextstepaction"]
> [Jupyter Notebook abrufen](https://aka.ms/aml-packages/vision/notebooks/object_detection)

### <a name="load-the-sample-data"></a>Laden der Beispieldaten

Im folgenden Beispiel wird ein Dataset verwendet, das aus 63 Bildern mit Tafelausstattung besteht. Jedes Bild ist als zugehörig zu einer von vier verschiedenen Klassen gekennzeichnet: bowl (Schüssel), cup (Tasse), cutlery (Besteck) oder plate (Platte). Die Anzahl der Bilder in diesem Beispiel ist gering, sodass dieses Beispiel schnell ausgeführt werden kann. In der Praxis sollten mindestens 100 Bilder pro Klasse bereitgestellt werden. Alle Bilder befinden sich unter *„../sample_data/imgs_recycling/“* in Unterverzeichnissen namens „bowl“, „cup“, „cutlery“ und „plate“.

![Azure Machine Learning-Dataset](media/how-to-build-deploy-image-classification-models/recycling_examples.jpg)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie in den folgenden Artikeln mehr über das Azure Machine Learning-Paket für maschinelles Sehen:

+ Erfahren Sie, wie Sie die [Genauigkeit des Modells verbessern](how-to-improve-accuracy-for-computer-vision-models.md) können.

+ Lesen Sie die [Paketübersicht](https://aka.ms/aml-packages/vision).

+ Lesen Sie die [Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) zu diesem Paket.

+ Erfahren Sie mehr über [andere Python-Pakete für Azure Machine Learning](reference-python-package-overview.md).
