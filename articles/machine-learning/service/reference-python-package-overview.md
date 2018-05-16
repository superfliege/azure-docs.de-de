---
title: Python-Pakete für Azure Machine Learning
description: Hier finden Sie Informationen zu den Python-Paketen, die Benutzern von Azure Machine Learning zur Verfügung stehen.
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jmartens
author: j-martens
ms.date: 05/07/2018
ms.openlocfilehash: 7ff80a812ebf704315524ffb5ea518704e472429
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="python-packages-for-azure-machine-learning"></a>Python-Pakete für Azure Machine Learning

Hier finden Sie Informationen zu den proprietären Python-Paketen von Microsoft für Azure Machine Learning. Diese Bibliotheken und Funktionen können mit anderen Open-Source-Paketen oder mit Paketen von Drittanbietern kombiniert werden. Zur Verwendung der proprietären Pakete muss Ihr Python-Code allerdings für einen Dienst oder auf einem Computer ausgeführt werden, der die Interpreter bereitstellt.

Bei den Azure Machine Learning-Paketen handelt es sich um **über pip installierbare Python-Erweiterungen für Azure Machine Learning**, mit denen Data Scientists und KI-Entwickler im Handumdrehen äußerst präzise Machine Learning- und Deep Learning-Modelle für verschiedene Domänen erstellen und bereitstellen können.

<a name="amlpcv"></a>
## <a name="azure-ml-package-for-computer-vision"></a>Azure ML-Paket für maschinelles Sehen

Mit dem Azure ML-Paket für maschinelles Sehen können Sie Deep Learning-Modelle für Bildklassifizierung, Objekterkennung und Bildähnlichkeit entwickeln, optimieren und bereitstellen.

Führen Sie für dieses Paket die folgenden Schritte aus:
1. [Laden Sie das Paket herunter.](https://aka.ms/aml-packages/vision/download)
1. Lesen Sie die [Installationsanleitung](https://aka.ms/aml-packages/vision).
1. [Erstellen Sie ein Modell für maschinelles Sehen mit einem Jupyter-Notebook, und stellen Sie es bereit.](how-to-build-deploy-image-classification-models.md)
1. Lesen Sie die [Referenzdokumentation](https://aka.ms/aml-packages/vision) für das Paket.

<a name="amlpf"></a>
## <a name="azure-ml-package-for-forecasting"></a>Azure ML-Paket für Vorhersagen

Mit dem Azure ML-Paket für Vorhersagen können Sie zeitreihenbasierte Vorhersagemodelle für Finanz- und Nachfrageprognosen erstellen und bereitstellen.

Führen Sie für dieses Paket die folgenden Schritte aus:
1. [Laden Sie das Paket herunter.](https://aka.ms/aml-packages/forecasting/download)
1. Lesen Sie die [Installationsanleitung](https://aka.ms/aml-packages/forecasting).
1. [Erstellen Sie ein Modell für Vorhersagen mit einem Jupyter-Notebook, und stellen Sie es bereit.](how-to-build-deploy-forecast-models.md)
1. Lesen Sie die [Referenzdokumentation](https://aka.ms/aml-packages/forecasting) für das Paket.

<a name="amlpta"></a>
## <a name="azure-ml-package-for-text-analytics"></a>Azure ML-Paket für Textanalysen

Mit dem Azure ML-Paket für Textanalysen können Sie textbasierte Deep Learning-Modelle für Textklassifizierung, benutzerdefinierte Entitätsextraktion und Worteinbettung erstellen.

Führen Sie für dieses Paket die folgenden Schritte aus:
1. [Laden Sie das Paket herunter.](https://aka.ms/aml-packages/text/download)
1. Lesen Sie die [Installationsanleitung](https://aka.ms/aml-packages/text).
1. [Erstellen Sie ein Textklassifizierungsmodell mit einem Jupyter-Notebook, und stellen Sie es bereit.](how-to-build-deploy-text-classification-models.md)
1. Lesen Sie die [Referenzdokumentation](https://aka.ms/aml-packages/text) für das Paket.

<a name="amlrealtimeai"></a>
## <a name="amlrealtimeai-fpga-acceleration"></a>amlrealtimeai (FPGA-Beschleunigung)

Mit dem Hardwarebeschleunigungspaket von Azure Machine Learning können Data Scientists Bilder mit einer quantisierten Version von ResNet 50 ausstatten, Klassifizierungen auf der Grundlage der entsprechenden Merkmale trainieren und anschließend Modelle in einem [FPGA (Field Programmable Gate Array)](concept-accelerate-with-fpgas.md) in Azure bereitstellen, um extrem geringe Latenzstörungen zu erzielen.

Führen Sie für dieses Paket die folgenden Schritte aus:
1. [Laden Sie das Paket herunter.](https://aka.ms/aml-real-time-ai-package)
1. Lesen Sie die [Installationsanleitung](reference-fpga-package-overview.md).
1. [Stellen Sie ein Modell in einem FPGA bereit.](how-to-deploy-fpga-web-service.md)

