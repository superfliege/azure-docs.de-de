---
title: Was ist ein FPGA, und was ist Project Brainwave? – Azure Machine Learning
description: Hier erfahren Sie, wie Sie Modelle und Deep Neural Networks mit FPGAs in Azure beschleunigen. Dieser Artikel enthält eine Einführung in FPGAs (Field-Programmable Gate Arrays) und erläutert, wie Azure Machine Learning bei der Bereitstellung Ihres Modells in einem Azure-FPGA künstliche Intelligenz in Echtzeit bietet.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: tedway
author: tedway
ms.reviewer: jmartens
ms.date: 9/24/2018
ms.openlocfilehash: adcd812bc63f98e4cbde949946bf32a037c6d704
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47158686"
---
# <a name="what-is-fpga-and-project-brainwave"></a>Was ist ein FPGA, und was ist Project Brainwave?

Dieser Artikel enthält eine Einführung in FPGAs (Field-Programmable Gate Arrays) und erläutert, wie Azure Machine Learning bei der Bereitstellung Ihres Modells in einem Azure-FPGA künstliche Intelligenz in Echtzeit bietet.

FPGAs enthalten ein Array von programmierbaren Logikblöcken sowie eine Hierarchie von neu konfigurierbaren Interconnects. Durch die Interconnects können diese Blöcke nach der Fertigung auf verschiedene Weise konfiguriert werden. FPGAs bieten im Vergleich zu anderen Chips eine Kombination aus Programmierbarkeit und Leistung.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGAs im Vergleich zu CPU, GPU und ASIC

![Azure Machine Learning: FPGA-Vergleich](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

|Prozessor||BESCHREIBUNG|
|---|:-------:|------|
|Anwendungsspezifische integrierte Schaltkreise|ASICs|Benutzerdefinierte Schaltkreise, z.B. TPUs (TensorFlow-Prozessoren) von Google, bieten die höchste Effizienz. Sie können nicht neu konfiguriert werden, wenn sich Ihre Anforderungen ändern.|
|Field-Programmable Gate Arrays|FPGAs|FPGAs, wie sie in Azure verfügbar sind, bieten eine nahezu an ASICs heranreichende Leistung, sind jedoch flexibel und können im Laufe der Zeit neu konfiguriert werden, um neue Logik zu implementieren.|
|Grafikprozessoren|GPUs|GPUs sind für KI-Berechnungen eine beliebte Wahl, weil sie Funktionen zur Parallelverarbeitung bieten und beim Grafikrendering schneller sind als CPUs.|
|Zentralprozessoren|CPUs|CPUs sind Allzweckprozessoren, deren Leistung für die Grafik- und Videoverarbeitung eher nicht geeignet ist.|

## <a name="project-brainwave-on-azure"></a>Project Brainwave für Azure

[Project Brainwave](https://www.microsoft.com/research/project/project-brainwave/) ist eine kostengünstige Hardwarearchitektur von Microsoft, die auf den FPGA-Geräten von Intel beruht und von Data Scientists und Entwicklern zum Beschleunigen von Echtzeit-KI-Berechnungen verwendet wird.  Diese FPGA-fähige Architektur bietet **Leistung**, **Flexibilität** und **Skalierbarkeit** und ist in Azure verfügbar.

**FPGAs ermöglichen eine geringe Latenzzeit für Echtzeit-Inferenzanforderungen.** Batchverarbeitung bedeutet, dass eine große Datenmenge erfasst und an einen Prozessor geleitet wird, um die Hardwarenutzung zu verbessern. Die Batchverarbeitung kann zu Wartezeiten führen, weil mehr Daten verarbeitet werden müssen. Gleichzeitig kann dadurch jedoch der Durchsatz verbessert werden. Bei Project Brainwave-Implementierungen von neuronalen Prozessoren ist keine Batchverarbeitung erforderlich. Daher kann die Wartezeit im Vergleich zu CPU und GPU um ein Vielfaches geringer ausfallen.

### <a name="reconfigurable-power"></a>Neu konfigurierbare Leistung
**FPGAs können für verschiedene Arten von Machine Learning-Modellen neu konfiguriert werden.** Durch diese Flexibilität ist es einfacher, die Anwendungen auf der Grundlage der optimalen numerischen Genauigkeit und des verwendeten Speichermodells zu beschleunigen.

Neue Machine Learning-Techniken werden regelmäßig entwickelt, und auch der Hardwareentwurf von Project Brainwave entwickelt sich rasant. Da FPGAs neu konfigurierbar sind, ist es möglich, mit den Anforderungen der sich schnell ändernden KI-Algorithmen Schritt zu halten.

### <a name="whats-supported-on-azure"></a>In Azure unterstützte Funktionen
**Microsoft Azure ist die weltweit größte Cloudinvestition in FPGAs.** Sie können Project Brainwave unter der Skalierungsinfrastruktur von Azure ausführen.

In Project Brainwave wird zurzeit Folgendes unterstützt:
+ Szenarien zur Bildklassifizierung und -erkennung
+ TensorFlow-Bereitstellung
+ DNNs: ResNet 50, ResNet 152, VGG-16, SSD-VGG und DenseNet-121
+ Intel FPGA-Hardware 

Mit dieser FPGA-fähigen Hardwarearchitektur werden trainierte neuronale Netze schnell und mit geringerer Wartezeit ausgeführt. Project Brainwave kann vortrainierte DNNs (Deep Neural Networks) FPGA-übergreifend parallelisieren, um Ihren Dienst horizontal hochzuskalieren. Die DNNs können als Deep Featurizer zum Transferlernen vortrainiert oder durch aktualisierte Gewichtungen optimiert werden.

### <a name="scenarios-and-applications"></a>Szenarien und Anwendungen

Project Brainwave ist in Azure Machine Learning integriert. Microsoft verwendet FPGAs für die DNN-Auswertung, die Rangfolge bei der Bing-Suche und die Beschleunigung von Software-Defined Networking (SDN), um die Wartezeit zu reduzieren und gleichzeitig CPUs für andere Aufgaben freizugeben.

Die folgenden Szenarien verwenden FPGA unter der Project Brainwave-Architektur:
+ [Automatisiertes optisches Untersuchungssystem](https://blogs.microsoft.com/ai/build-2018-project-brainwave/)

+ [Landnutzungskartierung](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/)

## <a name="deploy-to-fpgas-on-azure"></a>Bereitstellen für FPGAs in Azure

So sieht der Workflow zum Erstellen eines Bilderkennungsdiensts in Azure mithilfe von unterstützten DNNs als Featurizer für die Bereitstellung in Azure-FPGAs aus:

1. Verwenden Sie das Azure Machine Learning SDK für Python, um eine Dienstdefinition zu erstellen. Dabei handelt es sich um eine Datei, die eine Pipeline mit auf TensorFlow basierenden Diagrammen (Eingabe, Featurizer und Klassifizierung) beschreibt. Durch den Bereitstellungsbefehl werden die Definition und die Diagramme automatisch in einer ZIP-Datei komprimiert, die dann in Azure Blob Storage hochgeladen wird.  Das DNN wurde in Project Brainwave bereits zur Ausführung im FPGA bereitgestellt.

1. Registrieren Sie das Modell über das SDK anhand der ZIP-Datei in Azure Blob Storage.

1. Stellen Sie den Dienst mit dem registrierten Modell mithilfe des SDK bereit.

Sie können mit dem Bereitstellen trainierter DNN-Modelle in FPGAs in der Azure-Cloud mithilfe dieses Artikels beginnen: **[Bereitstellen eines Modells als Webdienst auf einem FPGA mit Azure Machine Learning](how-to-deploy-fpga-web-service.md)**.


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich diese Videos und Blogs an:

+ [Hyperskalierungshardware: ML skaliert über Azure und FPGA: Build 2018 (Video)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

+ [Einblicke in die FPGA-basierte konfigurierbare Cloud von Microsoft (Video)](https://channel9.msdn.com/Events/Build/2017/B8063)

+ [Project Brainwave für KI in Echtzeit: Projektstartseite](https://www.microsoft.com/research/project/project-brainwave/)
