---
title: Was ist Azure Machine Learning? | Microsoft-Dokumentation
description: Es werden grundlegende Machine Learning-Konzepte für die Cloud und die Nutzungsmöglichkeiten beschrieben, und Sie erhalten Definitionen von Machine Learning-Begriffen. Enthält eine Übersicht über Azure Machine Learning. Hierbei handelt es sich um eine integrierte End-to-End-Data Science-Lösung für Data Science-Profis, mit der erweiterte Analyseanwendungen für die Cloud entwickelt und für Experimente genutzt und bereitgestellt werden.
services: machine-learning
author: mwinkle
ms.author: mwinkle
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: overview
ms.date: 09/21/2017
ms.openlocfilehash: 3e744b0e4a7ccebcdedac5a822ff717bed6b1f72
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268415"
---
# <a name="what-is-machine-learning"></a>Was ist Machine Learning?

Machine Learning ist ein Data Science-Verfahren, mit dem Computer aus vorhandenen Daten lernen können, um zukünftiges Verhalten, Ergebnisse und Trends vorherzusagen. Mithilfe von Machine Learning lernen Computer, ohne explizit programmiert worden zu sein.

Dank solcher Vorhersagen oder Prognosen aus Machine Learning können Apps und Geräte „intelligenter“ werden. Wenn Sie online einkaufen, trägt maschinelles Lernen dazu bei, dass Ihnen anhand der gekauften Produkte weitere Produkte empfohlen werden, die Ihnen gefallen könnten. Wenn Ihre Kreditkarte verwendet wird, vergleichen Machine Learning-Prozesse die Transaktion mit einer Transaktionsdatenbank und helfen bei der Betrugserkennung. Wenn ein automatischer Staubsauger ein Zimmer saugt, wird anhand von Machine Learning-Prozessen entschieden, ob die Arbeit erledigt ist.

## <a name="what-is-azure-machine-learning"></a>Was ist Azure Machine Learning?
Azure Machine Learning ist eine integrierte End-to-End-Lösung für Data Science und Advanced Analytics. Data Scientists können sie nutzen, um Daten vorzubereiten, Experimente zu entwickeln und Modelle für die Cloud bereitzustellen.

Die wichtigsten Komponenten von Azure Machine Learning sind:
- Azure Machine Learning Workbench
- Azure Machine Learning-Experimentieren-Dienst
- Azure Machine Learning-Modellverwaltungsdienst
- Microsoft Machine Learning-Bibliotheken für Apache Spark (MMLSpark Library)
- Visual Studio Code-Tools für KI

Zusammen stellen diese Anwendungen und Dienste eine Hilfe dar, mit der Sie die Entwicklung und Bereitstellung Ihrer Data Science-Projekte erheblich beschleunigen können. 

![Azure Machine Learning-Konzepte](./media/overview-what-is-azure-ml/aml-concepts.png)


## <a name="open-source-compatible"></a>Open-Source-Kompatibilität

Azure Machine Learning verfügt über eine umfassende Unterstützung von Open-Source-Technologie. Sie können mehrere zehntausend Open-Source-Python-Pakete nutzen, z.B. die folgenden Machine Learning-Frameworks:

- [scikit-learn](http://scikit-learn.org/)
- [TensorFlow](https://www.tensorflow.org/)
- [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/)
- [Spark ML](https://spark.apache.org/docs/2.1.1/ml-pipeline.html)

Sie können Ihre Experimente in verwalteten Umgebungen durchführen, z.B. Docker-Containern und Spark-Clustern. Außerdem können Sie erweiterte Hardware, z.B. [GPU-fähige virtuelle Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu), verwenden, um die Ausführung zu beschleunigen.

Azure Machine Learning basiert auf der folgenden Open-Source-Technologie:

- [Jupyter Notebook](http://jupyter.org/)
- [Apache Spark](https://spark.apache.org/)
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [Python](https://www.python.org/)
- [Conda](https://conda.io/docs/)

Außerdem umfasst Azure Machine Learning die eigene Open-Source-Technologie von Microsoft, z.B. [Microsoft Machine Learning-Bibliothek für Apache Spark](https://github.com/Azure/mmlspark) und Cognitive Toolkit.

Zudem profitieren Sie von professioneller, altbewährter Lerntechnologie, die von Microsoft entworfen wurde, um Probleme größeren Umfangs zu lösen. Diese Komponenten haben sich bereits in vielen Microsoft-Produkten bewährt, z.B.:

- Windows
- Bing
- Xbox
- Office
- SQL Server

Hier sind einige Komponenten der Machine Learning-Technologie von Microsoft aufgeführt, die in Azure Machine Learning enthalten sind:

- [PROSE](https://microsoft.github.io/prose/) (PROgram Synthesis using Examples)
- [microsoftml](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)
- [revoscalepy](https://docs.microsoft.com/r-server/python-reference/revoscalepy/revoscalepy-package)

## <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench
Azure Machine Learning Workbench ist eine Desktopanwendung mit Befehlszeilentools und Unterstützung für Windows und macOS. Mit dieser Anwendung können Sie für Machine Learning-Lösungen den gesamten Data Science-Lebenszyklus verwalten:

- Datenerfassung und -vorbereitung
- Modellentwicklung und Experimentverwaltung
- Modellbereitstellung in verschiedenen Zielumgebungen

Hier sind die wichtigsten Funktionen von Azure Machine Learning Workbench aufgeführt:

- Tool für die Datenvorbereitung, das die Datentransformation anhand von Beispielen erlernen kann.
- Auf die Datenquellenabstraktion kann per Benutzeroberfläche und Python-Code zugegriffen werden.
- Python SDK zum Aufrufen von visuell erstellten Paketen für die Datenvorbereitung.
- Integrierter Jupyter-Notebook-Dienst und Clientbenutzeroberfläche.
- Experimentüberwachung und -verwaltung über Ansichten von Ausführungsverläufen.
- Rollenbasierte Zugriffssteuerung, die die Freigabe und Kollaboration per Azure Active Directory ermöglicht.
- Automatische Projektmomentaufnahmen für jede Ausführung und explizite Versionskontrolle durch native Git-Integration. 
- Integration in beliebte Python-IDEs.

Weitere Informationen finden Sie in den folgenden Artikeln:
- [Anleitung zur Datenvorbereitung](../desktop-workbench/data-prep-user-guide.md)
- [Using Git with Azure Machine Learning](../desktop-workbench/using-git-ml-project.md) (Verwenden von Git mit Azure Machine Learning)
- [Using Jupyter Notebook in Azure Machine Learning](../desktop-workbench/how-to-use-jupyter-notebooks.md) (Verwenden von Jupyter-Notebook in Azure Machine Learning)
- [Roaming und Freigabe](../desktop-workbench/roaming-and-collaboration.md)
- [Run History Guide](../desktop-workbench/how-to-use-run-history-model-metrics.md) (Leitfaden zum Ausführungsverlauf)
- [IDE Integration](../desktop-workbench/how-to-configure-your-ide.md) (IDE-Integration)

## <a name="azure-machine-learning-experimentation-service"></a>Azure Machine Learning-Experimentieren-Dienst
Mit dem Experimentieren-Dienst wird die Ausführung von Machine Learning-Experimenten verarbeitet. Außerdem verfügt er über Unterstützung für die Workbench mit Funktionen für Projektmanagement, Git-Integration, Zugriffssteuerung, Roaming und Freigabe. 

Über eine einfache Konfiguration können Sie Ihre Experimente für viele verschiedene Computeumgebungsoptionen ausführen:

- Lokal nativ
- Lokaler Docker-Container
- Docker-Container auf einem virtuellen Remotecomputer
- Horizontales Hochskalieren eines Spark-Clusters in Azure

Mit dem Experimentieren-Dienst werden virtuelle Umgebungen erstellt, um sicherzustellen, dass Ihr Skript mit reproduzierbaren Ergebnissen isoliert ausgeführt werden kann. Es werden Informationen zum Ausführungsverlauf aufgezeichnet, und der Verlauf wird grafisch dargestellt. So können Sie aus Ihren Experimentausführungen leicht das beste Modell auswählen. 

Weitere Informationen finden Sie unter [Configuring Azure Machine Learning Experimentation Service](../desktop-workbench/experimentation-service-configuration.md) (Konfigurieren des Azure Machine Learning-Experimentieren-Diensts).

## <a name="azure-machine-learning-model-management-service"></a>Azure Machine Learning-Modellverwaltungsdienst

Mit dem Modellverwaltungsdienst können Data Scientists und Entwicklerteams Vorhersagemodelle für viele unterschiedliche Umgebungen bereitstellen. Die Modellversionen und -herkunft werden von den Trainingsläufen bis hin zu den Bereitstellungen nachverfolgt. Die Modelle werden in der Cloud gespeichert, registriert und verwaltet. 

Mit einfachen CLI-Befehlen können Sie Ihr Modell, Bewertungsskripts und Abhängigkeiten in Docker-Images in Containern verpacken. Diese Images werden in Ihrer eigenen Docker-Registrierung registriert, die in Azure (Azure Container Registry) gehostet wird. Sie können zuverlässig für die folgenden Ziele bereitgestellt werden:

- Lokale Computer
- Lokale Server
- Cloud
- IoT Edge-Geräte

Eine Kubernetes-Ausführung in Azure Container Service (ACS) wird für die Cloudbereitstellung für horizontales Skalieren verwendet. Die Telemetrie der Modellausführung wird in AppInsights für die visuelle Analyse erfasst. 

Weitere Informationen zum Modellverwaltungsdienst finden Sie unter [Übersicht über die Modellverwaltung](../desktop-workbench/model-management-overview.md).


## <a name="microsoft-machine-learning-library-for-apache-spark"></a>Microsoft Machine Learning-Bibliothek für Apache Spark

[MMLSpark](https://github.com/Azure/mmlspark) (Microsoft Machine Learning-Bibliothek für Apache Spark) ist ein Open-Source-Spark-Paket mit Deep Learning- und Data Science-Tools für Apache Spark. Hiermit werden [Spark Machine Learning-Pipelines](https://spark.apache.org/docs/2.1.1/ml-pipeline.html) in das [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) und die [OpenCV](http://opencv.org/)-Bibliothek integriert. So können Sie schnell leistungsstarke, hoch skalierbare Vorhersage- und Analysemodelle für große Image- und Textdatasets erstellen. Einige Highlights:

- Einfaches Erfassen von Images aus HDFS in Spark DataFrame
- Vorverarbeiten von Imagedaten mit Transformationen aus OpenCV
- Ausstatten von Images mit Features anhand von vortrainierten Deep Neural Networks mit dem Microsoft Cognitive Toolkit 
- Verwenden von vortrainierten bidirektionalen LSTMs aus Keras zum Extrahieren von medizinischen Entitäten
- Trainieren von DNN-basierten Imageklassifizierungsmodellen auf VMs mit GPU der N-Serie in Azure
- Ausstatten von Freiform-Textdaten mit Features mit benutzerfreundlichen APIs zusätzlich zu Primitiven in SparkML über einen einzelnen Transformator
- Einfaches Trainieren von Klassifizierungs- und Regressionsmodellen durch die implizite Ausstattung von Daten mit Features
- Berechnen eines umfassenden Satzes von Auswertungsmetriken mit Metriken pro Instanz

Weitere Informationen finden Sie unter [Using MMLSpark in Azure Machine Learning](../desktop-workbench/how-to-use-mmlspark.md) (Verwenden von MMLSpark in Azure Machine Learning).

## <a name="visual-studio-code-tools-for-ai"></a>Visual Studio Code Tools for AI
Visual Studio Code-Tools für KI ist eine Erweiterung in Visual Studio Code zum Erstellen, Testen und Bereitstellen von Deep Learning- und KI-Lösungen. Die Tools enthalten viele Integrationspunkte für Azure Machine Learning, z.B.:
- Einen Ausführungsverlauf mit Anzeige der Leistung von Trainingsläufen und protokollierten Metriken
- Eine Katalogansicht, über die Benutzer neue Projekte mit dem Microsoft Cognitive Toolkit, TensorFlow und vielen anderen Deep Learning-Frameworks durchsuchen und das Bootstrapping dafür durchführen können 
- Eine Explorer-Ansicht zum Auswählen von Computezielen für Ihre auszuführenden Skripts
 

## <a name="what-are-the-machine-learning-options-from-microsoft"></a>Welche Machine Learning-Optionen gibt es bei Microsoft?
Neben Azure Machine Learning gibt es in Azure eine Vielzahl von Optionen zum Erstellen, Bereitstellen und Verwalten von Machine Learning-Modellen. [Weitere Informationen dazu finden sie hier.](../desktop-workbench/overview-more-machine-learning.md)

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Install and create Azure Machine Learning](quickstart-installation.md) (Installieren und Erstellen von Azure Machine Learning)
