---
title: Was ist ein FPGA? – Project Brainwave – Azure Machine Learning
description: Hier erfahren Sie, wie Sie Modelle und Deep Neural Networks mit FPGAs beschleunigen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/31/2018
ms.openlocfilehash: 5c8efcbb5f2e9014c9edabfc2aee37c8c29ea0b4
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "35634298"
---
# <a name="what-is-fpga-and-project-brainwave"></a>Was ist ein FPGA, und was ist Project Brainwave?

Dieser Artikel bietet eine Einführung in Field Programmable Gate Arrays (FPGAs) und erläutert, wie FPGAs in Azure Machine Learning integriert werden, um Echtzeit-KI bereitzustellen.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGAs im Vergleich zu CPU, GPU und ASIC

FPGAs enthalten eine Reihe von programmierbaren logischen Blöcken und eine Hierarchie von neu konfigurierbaren Verbindungen, die es ermöglichen, die Blöcke nach der Herstellung auf unterschiedliche Weise zu konfigurieren.

FPGAs bieten eine Kombination aus Programmierbarkeit und Leistung im Vergleich zu anderen Chips:

![Azure Machine Learning: FPGA-Vergleich](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

- **CPUs (Central Processing Units)** sind allgemeine Prozessoren. Die CPU-Leistung ist nicht ideal für Grafik- und Videoverarbeitung.
- **GPUs (Graphics Processing Units)** bieten parallele Verarbeitung und sind eine beliebte Wahl für KI-Berechnungen. Die parallele Verarbeitung mit GPUs führt zu einem schnellen Bildrendering als mit CPUs.
- **ASICs (Application-Specific Integrated Circuits)** (z.B. Google TensorFlow-Prozessoreinheiten) sind angepasste integrierte Schaltkreise. ASICs bieten zwar die höchste Effizienz, aber sie sind unflexibel.
- **FPGAs**, die z.B. in Azure verfügbar sind, stellen eine Leistung bereit, die ASICs nahe kommt, bieten jedoch die Flexibilität einer Neukonfiguration zu einem späteren Zeitpunkt.

FPGAs sind nun in jedem neuen Azure-Server verfügbar. Microsoft verwendet bereits FPGAs für die Bing-Suchrangfolge, die DNN-Auswertung (Deep Neural Network) und die SDN-Beschleunigung (Software Defined Networking). Diese FPGA-Implementierungen verringern die Latenzzeit und entlasten die CPUs für andere Aufgaben.

## <a name="project-brainwave-on-azure"></a>Project Brainwave für Azure

Project Brainwave ist eine Hardwarearchitektur, die auf den FPGA-Geräten von Intel basiert und zur Beschleunigung von KI-Berechnungen in Echtzeit verwendet wird. Mit dieser kostengünstigen FPGA-fähigen Architektur kann ein trainiertes neuronales Netzwerk so schnell wie möglich und mit geringerer Latenz ausgeführt werden. Project Brainwave kann vortrainierte DNNs FPGA-übergreifend parallelisieren, um Ihren Dienst horizontal hochzuskalieren.

- Leistung

    FPGAs ermöglichen eine geringe Latenzzeit für Echtzeit-Inferenzanforderungen. Batchverarbeitung bedeutet, dass eine Anforderung in kleinere Teile aufgeteilt und diese an einen Prozessor zum Verbessern der Hardwarenutzung übergeben werden. Batchverarbeitung ist nicht effizient und kann Latenz verursachen. Brainwave benötigt keine Batchverarbeitung, daher ist die Latenzzeit im Vergleich zu CPU und GPU 10 Mal geringer.

- Flexibilität

    FPGAs können für verschiedene Arten von Machine Learning-Modellen neu konfiguriert werden. Durch diese Flexibilität ist es einfacher, die Anwendungen auf der Grundlage der optimalen numerischen Genauigkeit und des verwendeten Speichermodells zu beschleunigen.

    Neue Machine Learning-Techniken werden regelmäßig entwickelt, und auch der Hardwareentwurf von Project Brainwave entwickelt sich rasant. Da FPGAs neu konfigurierbar sind, ist es möglich, mit den Anforderungen der sich schnell ändernden KI-Algorithmen Schritt zu halten.

- Skalieren

    Microsoft Azure ist die weltweit größte Cloudinvestition in FPGAs. Sie können Brainwave unter der Skalierungsinfrastruktur von Azure ausführen.

Eine Vorschau von Project Brainwave (mit Integration in Azure Machine Learning) ist zurzeit verfügbar. Eine eingeschränkte Vorschau ist ebenfalls verfügbar, um Project Brainwave vorzustellen, sodass Sie die Vorteile dieser Rechengeschwindigkeit in Ihren eigenen Unternehmen und Einrichtungen nutzen können.

In der aktuellen Vorschau ist Brainwave auf die TensorFlow-Bereitstellung und die auf ResNet50 basierenden neuronalen Netzwerke auf Intel FPGA-Hardware für Bildklassifizierung und -erkennung beschränkt. Es ist geplant, weitere Katalogmodelle und andere Frameworks zu unterstützen.

Die folgenden Szenarien verwenden FPGA unter der Project Brainwave-Architektur:

- Automatisiertes optisches Untersuchungssystem. Weitere Informationen finden Sie unter [Echtzeit-KI: Microsoft kündigt die Vorschau von Project Brainwave an](https://blogs.microsoft.com/ai/build-2018-project-brainwave/).
- Landnutzungskartierung. Weitere Informationen finden Sie unter [Verwenden von FPGAs für Deep Learning-Rückschlüsse zum Ausführen von Landnutzungskartierung für Luftbilder von mehreren TB Größe](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/).

## <a name="how-to-deploy-a-web-service-to-an-fpga"></a>Wie wird ein Webdienst für ein FPGA bereitgestellt?

Der allgemeine Ablauf für das Erstellen eines Bilderkennungsdiensts in Azure mit ResNet50 als Featurizer ist wie folgt:

1. ResNet50 ist bereits in Brainwave bereitgestellt. Sie können mit TensorFlow weitere Graphen (Datumseingabe, Klassifizierung usw.) erstellen und eine Pipeline (Eingabe -> Featurisieren -> Klassifizieren) mithilfe einer JSON-Dienstdefinitionsdatei definieren. Komprimieren Sie die Definition und die Graphen in einer ZIP-Datei, und laden Sie die ZIP-Datei in Azure Blob Storage hoch.
2. Registrieren Sie das Modell unter Verwendung der Azure ML-Modellverwaltungs-API mit der ZIP-Datei im Blobspeicher.
3. Stellen Sie den Dienst mit dem registrierten Modell mithilfe der Azure ML-Modellverwaltungs-API bereit.

Weitere Informationen zu diesem Vorgang finden Sie im Artikel [Bereitstellen eines Modells als Webdienst auf einem FPGA mit Azure Machine Learning](how-to-deploy-fpga-web-service.md).


## <a name="start-using-fpga"></a>Einstieg in FPGA

Hardwarebeschleunigte Azure Machine Learning-Modelle ermöglichen die Bereitstellung trainierter DNN-Modelle in FPGAs in der Azure-Cloud. Informationen zu den ersten Schritten finden Sie unter:

- [Bereitstellen eines Modells als Webdienst in einem FPGA](how-to-deploy-fpga-web-service.md)
- [Hardwarebeschleunigte Microsoft Azure Machine Learning-Modelle mit Project Brainwave](https://github.com/azure/aml-real-time-ai).

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen eines Modells als Webdienst in einem FPGA](how-to-deploy-fpga-web-service.md)

[Informationen zum Installieren des FPGA SDK](reference-fpga-package-overview.md)

[Hyperskalierungshardware: ML skaliert über Azure und FPGA: Build 2018 (Video)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

[Einblicke in die FPGA-basierte konfigurierbare Cloud von Microsoft (Video)](https://channel9.msdn.com/Events/Build/2017/B8063)

[Microsoft stellt Project Brainwave für KI in Echtzeit vor](https://www.microsoft.com/research/blog/microsoft-unveils-project-brainwave/)