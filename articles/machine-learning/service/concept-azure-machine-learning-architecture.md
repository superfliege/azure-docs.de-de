---
title: 'Maschinelles Lernen in der Cloud: Begriffe und Architektur'
titleSuffix: Azure Machine Learning service
description: Enthält Informationen zu Architektur, Terminologie und Konzepten des Azure Machine Learning-Diensts. Außerdem erhalten Sie Informationen zum allgemeinen Workflow für die Nutzung des Diensts und zu den Azure-Diensten, die von Azure Machine Learning Service verwendet werden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 1640b1cdb9410f33f6556667f36aafcfe575a082
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58080321"
---
# <a name="how-azure-machine-learning-service-works-architecture-and-concepts"></a>So funktioniert Azure Machine Learning Service: Architektur und Konzepte

In diesem Artikel werden die Architektur und die Konzepte für Azure Machine Learning Service beschrieben. Die wichtigsten Komponenten des Diensts und der allgemeine Workflow für seine Nutzung sind im folgenden Diagramm dargestellt:

[![Azure Machine Learning Service – Architektur und Workflow](./media/concept-azure-machine-learning-architecture/workflow.png)](./media/concept-azure-machine-learning-architecture/workflow.png#lightbox)

Für den Workflow werden in der Regel diese Schritte ausgeführt:

1. Entwickeln von Machine Learning-Trainingsskripts in **Python**
1. Erstellen und Konfigurieren eines **Computeziels**
1. **Übermitteln der Skripts** an das konfigurierte Computeziel zur Ausführung in dieser Umgebung. Während des Trainings können die Skripts **Datenspeicher** lesen oder beschreiben. Die Datensätze zur Ausführung werden zudem als **Ausführungen** im **Arbeitsbereich** gespeichert und unter **Experimente** gruppiert.
1. **Abfragen des Experiments** auf protokollierte Metriken aus den aktuellen und bereits erfolgten Ausführungen. Wenn die Metriken kein gewünschtes Ergebnis anzeigen, können Sie zu Schritt 1 zurückkehren und Ihre Skripts erneut durchlaufen.
1. Nachdem eine zufriedenstellende Ausführung gefunden wurde, können Sie das dauerhafte Modell in der **Modellregistrierung** registrieren.
1. Entwickeln eines Bewertungsskripts
1. **Erstellen eines Images** und Registrieren in der **Imageregistrierung**
1. **Bereitstellen des Images** als **Webdienst** in Azure


> [!NOTE]
> In diesem Artikel werden von Azure Machine Learning Service genutzte Begriffe und Konzepte definiert, aber keine Begriffe und Konzepte für die Azure Platform. Weitere Informationen zur Azure Platform-Terminologie finden Sie im [Microsoft Azure-Glossar](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="workspace"></a>Arbeitsbereich

Der Arbeitsbereich ist die Ressource der obersten Ebene für Azure Machine Learning Service. Er bietet einen zentralen Ort für die Arbeit mit allen Artefakten, die Sie bei der Verwendung von Azure Machine Learning Service erstellen.

Im Arbeitsbereich wird eine Liste mit Computezielen geführt, die zum Trainieren Ihres Modells verwendet werden können. Außerdem wird der Verlauf der Trainingsläufe gespeichert, z.B. Protokolle, Metriken, Ausgabe und eine Momentaufnahme Ihrer Skripts. Anhand dieser Informationen ermitteln Sie, welcher Trainingslauf das beste Modell ergibt.

Modelle werden beim Arbeitsbereich registriert. Sie verwenden ein registriertes Modell und Bewertungsskripts, um ein Image zu erstellen. Das Image kann dann in Azure Container Instances, Azure Kubernetes Service oder einem FPGA (Field-Programmable Gate Array) als REST-basierter HTTP-Endpunkt bereitgestellt werden. Außerdem kann es als Modul auf einem Azure IoT Edge-Gerät bereitgestellt werden.

Sie können mehrere Arbeitsbereiche erstellen, und jeder Arbeitsbereich kann von mehreren Benutzern gemeinsam genutzt werden. Den Zugriff auf einen freigegebenen Arbeitsbereich steuern Sie, indem Sie Benutzern die folgenden Rollen zuweisen:

* Owner (Besitzer)
* Mitwirkender
* Leser

Wenn Sie einen neuen Arbeitsbereich erstellen, werden darin automatisch mehrere Azure-Ressourcen erstellt, die vom Arbeitsbereich verwendet werden:

* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): Registriert die Docker-Container, die Sie während des Trainings und bei der Modellbereitstellung verwenden.
* [Azure-Speicherkonto](https://azure.microsoft.com/services/storage/): Wird als Standarddatenspeicher für den Arbeitsbereich verwendet.
* [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): Speichert Überwachungsinformationen zu Ihren Modellen.
* [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): Speichert Geheimnisse, die von Computezielen verwendet werden, sowie andere vertrauliche Informationen, die vom Arbeitsbereich benötigt werden.

> [!NOTE]
> Neben dem Erstellen neuer Versionen können Sie auch vorhandene Azure-Dienste verwenden.

Das folgende Diagramm enthält eine Taxonomie des Arbeitsbereichs:

[![Taxonomie des Arbeitsbereichs](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.svg)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

## <a name="experiment"></a>Experiment

Ein Experiment ist eine Gruppierung vieler Ausführungen aus einem bestimmten Skript. Es gehört immer zu einem Arbeitsbereich. Beim Übermitteln einer Ausführung geben Sie einen Experimentnamen an. Die Informationen für die Ausführung werden unter diesem Experiment gespeichert. Wenn Sie eine Ausführung übermitteln und einen nicht vorhandenen Experimentnamen angeben, wird automatisch ein neues Experiment mit diesem neu angegebenen Namen erstellt.

Ein Beispiel zum Verwenden eines Experiments finden Sie unter [Schnellstart: Erste Schritte mit Azure Machine Learning Service](quickstart-get-started.md).

## <a name="model"></a>Modell

Im einfachsten Fall ist ein Modell ein Codeabschnitt, für den eine Eingabe verwendet wird, um eine Ausgabe zu erzeugen. Die Erstellung eines Machine Learning-Modells umfasst die Auswahl eines Algorithmus, die Bereitstellung der zugehörigen Daten und die Optimierung von Hyperparametern. Das Training ist ein iterativer Prozess, bei dem ein trainiertes Modell mit den während des Trainingsprozesses erlernten Informationen erzeugt wird.

Ein Modell wird erzeugt, indem in Azure Machine Learning eine Ausführung erfolgt. Sie können auch ein Modell verwenden, das außerhalb von Azure Machine Learning trainiert wurde. Sie können ein Modell in einem Azure Machine Learning Service-Arbeitsbereich registrieren.

Azure Machine Learning Service ist frameworkunabhängig. Beim Erstellen eines Modells können Sie ein beliebiges gängiges Machine Learning-Framework verwenden, z.B. Scikit-learn, XGBoost, PyTorch, TensorFlow, Chainer und das Microsoft Cognitive Toolkit (ehemals CNTK).

Ein Beispiel zum Trainieren eines Modells finden Sie im Dokument [Schnellstart: Verwenden des Azure-Portals zum Ausführen der ersten Schritte mit Azure Machine Learning](quickstart-get-started.md).

### <a name="model-registry"></a>Modellregistrierung

Die Modellregistrierung verfolgt alle Modelle in Ihrem Azure Machine Learning Service-Arbeitsbereich nach.

Die Modelle werden anhand des Namens und der Version identifiziert. Jedes Mal, wenn Sie ein Modell mit dem gleichen Namen wie ein bereits vorhandenes Modell registrieren, geht die Registrierung davon aus, dass es sich um eine neue Version handelt. Die Version wird inkrementiert, und das neue Modell wird unter demselben Namen registriert.

Bei der Registrierung des Modells können Sie zusätzliche Metadatentags bereitstellen und diese Tags dann beim Suchen nach Modellen verwenden.

Sie können keine Modelle löschen, die von einem Image verwendet werden.

Ein Beispiel für das Registrieren eines Modells finden Sie unter [Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning Service](tutorial-train-models-with-aml.md).

## <a name="run-configuration"></a>Laufzeitkonfiguration

Eine Laufzeitkonfiguration ist ein Satz mit Anweisungen, mit denen definiert wird, wie ein Skript auf einem bestimmten Computeziel ausgeführt werden sollte. Die Konfiguration umfasst einen weiten Bereich von Verhaltensdefinitionen, z.B. die Vorgabe, ob eine vorhandene Python-Umgebung oder eine aus einer Spezifikation erstellte Conda-Umgebung verwendet werden soll.

Eine Laufzeitkonfiguration kann in einer Datei in dem Verzeichnis beständig gespeichert werden, in dem Ihr Trainingsskript enthalten ist, oder sie kann als In-Memory-Objekt erstellt und zum Übermitteln einer Ausführung verwendet werden.

Beispiele für Laufzeitkonfigurationen finden Sie unter [Auswählen und Verwenden eines Computeziels zum Trainieren Ihres Modells](how-to-set-up-training-targets.md).

## <a name="datastore"></a>Datenspeicher

Ein Datenspeicher ist eine Speicherabstraktion eines Azure-Speicherkontos. Der Datenspeicher kann entweder einen Azure-Blobcontainer oder eine Azure-Dateifreigabe als Back-End-Speicher verwenden. Jeder Arbeitsbereich verfügt über einen Standarddatenspeicher, und Sie können auch zusätzliche Datenspeicher registrieren.

Verwenden Sie die Python SDK-API oder die Azure Machine Learning-CLI, um Dateien zu speichern und aus dem Datenspeicher abzurufen.

## <a name="compute-target"></a>Computeziel

Ein Computeziel ist die Computeressource, die zum Ausführen Ihres Trainingsskripts oder zum Hosten Ihrer Dienstbereitstellung verwendet wird. Die unterstützten Computeziele lauten:

| Computeziel | Training | Bereitstellung |
| ---- |:----:|:----:|
| Ihr lokaler Computer | ✓ | &nbsp; |
| Azure Machine Learning Compute | ✓ | &nbsp; |
| Ein virtueller Linux-Computer in Azure</br>(z. B. Data Science Virtual Machine) | ✓ | &nbsp; |
| Azure Databricks | ✓ | &nbsp; |
| Azure Data Lake Analytics | ✓ | &nbsp; |
| Apache Spark für HDInsight | ✓ | &nbsp; |
| Azure Container Instances | &nbsp; | ✓ |
| Azure Kubernetes Service | &nbsp; | ✓ |
| Azure IoT Edge | &nbsp; | ✓ |
| Project Brainwave</br>(Field Programmable Gate Array) | &nbsp; | ✓ |

Computeziele sind einem Arbeitsbereich zugeordnet. Andere Computeziele als der lokale Computer werden von Benutzern des Arbeitsbereichs gemeinsam genutzt.

### <a name="managed-and-unmanaged-compute-targets"></a>Verwaltete und nicht verwaltete Computeziele

* **Verwaltet**: Computeziele, die von Azure Machine Learning Service erstellt und verwaltet werden. Diese Computeziele sind für Machine Learning-Workloads optimiert. Azure Machine Learning Compute ist das einzige verwaltete Computeziel ab dem 4. Dezember 2018. Möglicherweise werden künftig weitere verwaltete Computeziele hinzugefügt.

    Sie können Machine Learning-Compute-Instanzen direkt über den Arbeitsbereich erstellen, indem Sie das Azure-Portal, das Azure Machine Learning SDK oder die Azure CLI verwenden. Alle anderen Computeziele müssen außerhalb des Arbeitsbereichs erstellt und dann angefügt werden.

* **Nicht verwaltet**: Computeziele, die *nicht* von Azure Machine Learning Service verwaltet werden. Sie müssen sie ggf. außerhalb von Azure Machine Learning erstellen und anschließend vor der Nutzung an Ihren Arbeitsbereich anfügen. Für nicht verwaltete Computeziele sind möglicherweise weitere Schritte erforderlich, um die Leistung von Machine Learning-Workloads beizubehalten oder zu verbessern.

Informationen zum Auswählen eines Computeziels für das Training finden Sie unter [Auswählen und Verwenden eines Computeziels zum Trainieren Ihres Modells](how-to-set-up-training-targets.md).

Informationen zum Auswählen eines Computeziels für die Bereitstellung finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning Service](how-to-deploy-and-where.md).

## <a name="training-script"></a>Trainingsskript

Zum Trainieren eines Modells geben Sie das Verzeichnis an, in dem das Trainingsskript und die zugehörigen Dateien enthalten sind. Außerdem geben Sie einen Experimentnamen an, der zum Speichern der während des Trainings gesammelten Informationen verwendet wird. Beim Training wird das gesamte Verzeichnis in die Trainingsumgebung (Computeziel) kopiert und das von der Laufzeitkonfiguration angegebene Skript gestartet. Darüber hinaus wird auch eine Momentaufnahme des Verzeichnisses unter dem Experiment im Arbeitsbereich gespeichert.

Ein Beispiel finden Sie unter [Erstellen eines Arbeitsbereichs mit Python](quickstart-get-started.md).

## <a name="run"></a>Ausführen

Eine Ausführung ist ein Datensatz, der die folgenden Informationen enthält:

* Metadaten zur Ausführung (Zeitstempel, Dauer usw.)
* Metriken, die von Ihrem Skript protokolliert werden
* Ausgabedateien, die automatisch vom Experiment gesammelt oder explizit von Ihnen hochgeladen werden
* Eine Momentaufnahme des Verzeichnisses, in dem Ihre Skripts enthalten sind, vor der Ausführung

Eine Ausführung wird ausgelöst, wenn Sie ein Skript zum Trainieren eines Modells übermitteln. Eine Ausführung kann über null oder mehr untergeordnete Elemente verfügen. Die Ausführung der obersten Ebene weist also unter Umständen zwei untergeordnete Ausführungen auf, die beide jeweils selbst eine untergeordnete Ausführung aufweisen können.

Ein Beispiel zum Anzeigen von Ausführungen, die beim Trainieren eines Modells erstellt werden, finden Sie unter [Schnellstart: Erste Schritte mit Azure Machine Learning Service](quickstart-get-started.md).

## <a name="snapshot"></a>Momentaufnahme

Beim Übermitteln einer Ausführung komprimiert Azure Machine Learning das Verzeichnis, in dem das Skript als ZIP-Datei enthalten ist, und sendet es an das Computeziel. Die ZIP-Datei wird dann extrahiert, und das Skript wird ausgeführt. Azure Machine Learning speichert die ZIP-Datei im Rahmen der Ausführungsaufzeichnung zudem als Momentaufnahme. Alle Benutzer mit Zugriff auf den Arbeitsbereich können eine Ausführungsaufzeichnung durchsuchen und die Momentaufnahme herunterladen.

## <a name="activity"></a>Aktivität

Eine Aktivität stellt einen Vorgang mit langer Ausführungsdauer dar. Die folgenden Vorgänge sind Beispiele für Aktivitäten:

* Erstellen oder Löschen eines Computeziels
* Ausführen eines Skripts auf einem Computeziel

Aktivitäten können Benachrichtigungen über das SDK oder eine Webbenutzeroberfläche bereitstellen, damit Sie den Status dieser Vorgänge auf einfache Weise überwachen können.

## <a name="image"></a>Image

Images sind eine Möglichkeit für die zuverlässige Bereitstellung eines Modells mit allen Komponenten, die für die Verwendung des Modells benötigt werden. Ein Image enthält die folgenden Elemente:

* Ein Modell.
* Ein Bewertungsskript oder eine entsprechende Anwendung. Sie verwenden das Skript, um die Eingabe an das Modell zu übergeben und die Ausgabe des Modells zurückzugeben.
* Die Abhängigkeiten, die vom Modell oder dem Bewertungsskript bzw. der Bewertungsanwendung benötigt werden. Beispielsweise können Sie eine Conda-Umgebungsdatei einfügen, in der Python-Paketabhängigkeiten aufgeführt werden.

Azure Machine Learning kann zwei Arten von Images erstellen:

* **FPGA-Image**: Wird beim Bereitstellen eines FPGA (Field-Programmable Gate Array) in Azure verwendet.
* **Docker-Image**: Wird beim Bereitstellen auf anderen Computezielen als FPGA verwendet. Beispiele hierfür sind Azure Container Instances und Azure Kubernetes Service.

Ein Beispiel für die Erstellung eines Images finden Sie unter [Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances](tutorial-deploy-models-with-aml.md).

### <a name="image-registry"></a>Imageregistrierung

Die Imageregistrierung führt die Nachverfolgung von Images durch, die mit Ihren Modellen erstellt werden. Beim Erstellen des Images können Sie weitere Metadatentags bereitstellen. Metadatentags werden von der Imageregistrierung gespeichert und können von Ihnen abgefragt werden, um nach Ihrem Image zu suchen.

## <a name="deployment"></a>Bereitstellung

Eine Bereitstellung ist eine Instanziierung Ihres Images in einem Webdienst, der in der Cloud gehostet werden kann, oder in einem IoT-Modul für Bereitstellungen von integrierten Diensten.

### <a name="web-service"></a>Webdienst

Für einen bereitgestellten Webdienst können Azure Container Instances, Azure Kubernetes Service oder FPGAs verwendet werden. Sie erstellen den Dienst aus einem Image, in dem Ihr Modell, das Skript und zugeordnete Dateien enthalten sind. Das Image verfügt über einen HTTP-Endpunkt mit Lastenausgleich, der die an den Webdienst gesendeten Bewertungsanforderungen empfängt.

Azure dient Ihnen als Hilfe beim Überwachen der Bereitstellung Ihres Webdiensts, indem die Application Insight- bzw. Modelltelemetriedaten erfasst werden, wenn Sie dieses Feature aktiviert haben. Die Telemetriedaten sind nur für Sie zugänglich und werden in Ihren Application Insights- und Speicherkontoinstanzen gespeichert.

Wenn Sie die automatische Skalierung aktiviert haben, führt Azure für Ihre Bereitstellung automatisch eine Skalierung durch.

Ein Beispiel für die Bereitstellung eines Modells als Webdienst finden Sie unter [Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances](tutorial-deploy-models-with-aml.md).

### <a name="iot-module"></a>IoT-Modul

Ein bereitgestelltes IoT-Modul ist ein Docker-Container, der Ihr Modell und das zugeordnete Skript oder die Anwendung sowie alle zusätzlichen Abhängigkeiten enthält. Sie stellen diese Module mit Azure IoT Edge auf Edgegeräten bereit.

Wenn Sie die Überwachung aktiviert haben, erfasst Azure Telemetriedaten aus dem Modell im Azure IoT Edge-Modul. Die Telemetriedaten sind nur für Sie zugänglich und werden in Ihrer Speicherkontoinstanz gespeichert.

Azure IoT Edge stellt sicher, dass Ihr Modul ausgeführt wird, und überwacht das Gerät, auf dem es gehostet wird.

## <a name="pipeline"></a>Pipeline

Machine Learning-Pipelines werden zum Erstellen und Verwalten von Workflows verwendet, die Machine Learning-Phasen zusammenfügen. Eine Pipeline kann beispielsweise eine Datenaufbereitungs-, eine Modelltrainings-, eine Modellimplementierungs- und eine Rückschlussphase enthalten. Jede Phase kann mehrere Schritte umfassen, von denen wiederum jeder Schritt auf verschiedenen Computezielen unbeaufsichtigt ausgeführt werden kann.

Weitere Informationen zu Machine Learning-Pipelines für diesen Dienst finden Sie unter [Pipelines und Azure Machine Learning](concept-ml-pipelines.md).

## <a name="logging"></a>Protokollierung

Verwenden Sie beim Entwickeln Ihrer Lösung das Azure Machine Learning Python SDK in Ihrem Python-Skript, um beliebige Metriken zu protokollieren. Fragen Sie nach der Ausführung die Metriken ab, um zu ermitteln, ob von der Ausführung das Modell erzeugt wurde, das Sie bereitstellen möchten.

## <a name="next-steps"></a>Nächste Schritte

Erste Schritte mit Azure Machine Learning Service finden Sie unter:

* [Was ist Azure Machine Learning Service?](overview-what-is-azure-ml.md)
* [Schnellstart: Verwenden von Python für die ersten Schritte mit Azure Machine Learning](quickstart-get-started.md)
* [Tutorial: Trainieren eines Modells](tutorial-train-models-with-aml.md)
* [Erstellen von VMs anhand einer Resource Manager-Vorlage](how-to-create-workspace-template.md)