---
title: Wie funktioniert der Azure Machine Learning-Dienst?
description: Enthält Informationen zu Architektur, Terminologie und Konzepten des Azure Machine Learning-Diensts. Außerdem erhalten Sie Informationen zum allgemeinen Workflow für die Nutzung des Diensts und zu den Azure-Diensten, die vom Azure Machine Learning-Dienst verwendet werden.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: haining
author: hning86
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: 3011fa85dbac2135f4d9113c6b76a8b667ee4013
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46952126"
---
# <a name="architecture-and-concepts-how-does-azure-machine-learning-service-work"></a>Architektur und Konzepte: Wie funktioniert der Azure Machine Learning-Dienst? 

In diesem Dokument werden die Architektur und Konzepte für den Azure Machine Learning-Dienst beschrieben. Im folgenden Diagramm sind die wichtigsten Komponenten des Diensts und der allgemeine Workflow für seine Nutzung dargestellt: 

[![Azure Machine Learning – Architektur und Workflow](./media/concept-azure-machine-learning-architecture/workflow.png)](./media/concept-azure-machine-learning-architecture/workflow.png#lightbox)

Für den Workflow werden in der Regel diese Schritte ausgeführt:

1. Entwickeln von Machine Learning-Trainingsskripts in __Python__
1. Erstellen und Konfigurieren eines __Computeziels__
1. __Übermitteln der Skripts__ an das konfigurierte Computeziel zur Ausführung in dieser Umgebung. Während des Trainings speichert das Computeziel Ausführungsaufzeichnungen in einem __Datenspeicher__. Darin werden die Aufzeichnungen in einem __Experiment__ gespeichert.
1. __Abfragen des Experiments__ auf protokollierte Metriken aus den aktuellen und bereits erfolgten Ausführungen. Wenn die Metriken kein gewünschtes Ergebnis anzeigen, können Sie zu Schritt 1 zurückkehren und Ihre Skripts erneut durchlaufen.
1. Nachdem eine zufriedenstellende Ausführung gefunden wurde, können Sie das dauerhafte Modell in der __Modellregistrierung__ registrieren.
1. Entwickeln eines Bewertungsskripts
1. __Erstellen eines Images__ und Registrieren in der __Imageregistrierung__ 
1. __Bereitstellen des Images__ als __Webdienst__ in Azure


[!INCLUDE [aml-preview-note](../../../includes/aml-preview-note.md)]

> [!NOTE]
> In diesem Dokument werden von Azure Machine Learning genutzte Begriffe und Konzepte definiert, aber keine Begriffe und Konzepte für die Azure Platform. Weitere Informationen zur Azure Platform-Terminologie finden Sie im [Microsoft Azure-Glossar](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="workspace"></a>Arbeitsbereich

Der Arbeitsbereich ist die Ressource der obersten Ebene für den Azure Machine Learning-Dienst. Es ist ein zentraler Ort für die Arbeit mit allen Artefakten, die Sie bei der Verwendung von Azure Machine Learning erstellen.

Im Arbeitsbereich wird eine Liste mit Computezielen geführt, die zum Trainieren Ihres Modells verwendet werden können. Außerdem wird der Verlauf der Trainingsläufe gespeichert, z.B. Protokolle, Metriken, Ausgabe und eine Momentaufnahme Ihrer Skripts. Diese Informationen werden verwendet, um zu ermitteln, welcher Trainingslauf das beste Modell ergibt.

Modelle werden unter dem Arbeitsbereich registriert. Ein registriertes Modell und Bewertungsskripts werden verwendet, um ein Image zu erstellen. Das Image kann dann in Azure Container Instances, Azure Kubernetes Service oder einem FPGA (Field-Programmable Gate Array) als REST-basierter HTTP-Endpunkt bereitgestellt werden. Außerdem kann es als Modul auf einem Azure IoT Edge-Gerät bereitgestellt werden.

Sie können mehrere Arbeitsbereiche erstellen, und jeder Arbeitsbereich kann von mehreren Benutzern gemeinsam genutzt werden. Bei einem freigegebenen Arbeitsbereich steuern Sie den Zugriff auf den Arbeitsbereich, indem Sie Benutzern die folgenden Rollen zuweisen:

* Besitzer
* Mitwirkender
* Leser

Wenn Sie einen neuen Arbeitsbereich erstellen, werden darin automatisch mehrere Azure-Ressourcen erstellt, die vom Arbeitsbereich verwendet werden:

* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): Registriert Docker-Container, die während des Trainings und beim Bereitstellen eines Modells verwendet werden.
* [Azure Storage](https://azure.microsoft.com/services/storage/): Wird als Standarddateispeicher für den Arbeitsbereich verwendet.
* [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): Speichert Überwachungsinformationen zu Ihren Modellen.
* [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): Speichert Geheimnisse, die von Computezielen verwendet werden, sowie andere vertrauliche Informationen, die vom Arbeitsbereich benötigt werden.

> [!NOTE]
> Anstatt neue Versionen zu erstellen, können Sie auch vorhandene Azure-Dienste verwenden. 

Die folgende Abbildung enthält eine Taxonomie des Arbeitsbereichs:

[![Taxonomie des Arbeitsbereichs](./media/concept-azure-machine-learning-architecture/taxonomy.png)](./media/concept-azure-machine-learning-architecture/taxonomy.png#lightbox)

## <a name="model"></a>Modell

Im einfachsten Fall ist ein Modell ein Codeabschnitt, für den eine Eingabe verwendet wird, um eine Ausgabe zu erzeugen. Die Erstellung eines Machine Learning-Modells umfasst die Auswahl eines Algorithmus, die Bereitstellung der zugehörigen Daten und die Optimierung von Hyperparametern. Das Training ist ein iterativer Prozess, bei dem ein trainiertes Modell mit den während des Trainingsprozesses erlernten Informationen erzeugt wird.

Ein Modell wird erzeugt, indem in Azure Machine Learning eine Ausführung erfolgt. Sie können auch ein Modell verwenden, das außerhalb von Azure Machine Learning trainiert wurde. Ein Modell kann unter einem Azure Machine Learning-Arbeitsbereich registriert werden.

Azure Machine Learning ist „framework-agnostisch“. Sie können beim Erstellen eines Modells ein beliebiges gängiges Machine Learning-Framework verwenden, z.B. scikit-learn, xgboost, PyTorch, TensorFlow, Chainer und CNTK.

Ein Beispiel für das Trainieren eines Modells finden Sie im Dokument [Schnellstart: Erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md).

### <a name="model-registry"></a>Modellregistrierung

Die Modellregistrierung verfolgt alle Modelle in Ihrem Azure Machine Learning-Arbeitsbereich nach. 

Die Modelle werden anhand des Namens und der Version identifiziert. Jedes Mal, wenn Sie ein Modell mit dem gleichen Namen wie ein bereits vorhandenes Modell registrieren, geht die Registrierung davon aus, dass es sich um eine neue Version handelt. Die Version wird inkrementiert, und das neue Modell wird unter dem Namen registriert.

Sie können zusätzliche Metadatentags bereitstellen, wenn Sie das Modell registrieren, und diese Tags dann beim Suchen nach Modellen verwenden.

Sie können keine Modelle löschen, die von einem Image verwendet werden.

Ein Beispiel zum Registrieren eines Modells finden Sie in der Dokumentation [Tutorial 1: Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-train-models-with-aml.md).

## <a name="image"></a>Image

Images sind eine Möglichkeit für die zuverlässige Bereitstellung eines Modells mit allen Komponenten, die für die Verwendung des Modells benötigt werden. Ein Image enthält die folgenden Elemente:

* Ein Modell.
* Ein Bewertungsskript oder eine entsprechende Anwendung. Dieses Skript wird verwendet, um die Eingabe an das Modell zu übergeben und die Ausgabe des Modells zurückzugeben.
* Abhängigkeiten, die vom Modell oder dem Bewertungsskript bzw. der Bewertungsanwendung benötigt werden.  Beispielsweise können Sie eine Conda-Umgebungsdatei einfügen, in der Python-Paketabhängigkeiten aufgeführt werden.

Es gibt zwei Arten von Images, die mit Azure Machine Learning erstellt werden können:

* FPGA-Image: Wird beim Bereitstellen eines FPGA (Field-Programmable Gate Array) in der Azure-Cloud verwendet.
* Docker-Image: Wird beim Bereitstellen auf anderen Computezielen als FPGA verwendet. Beispiele hierfür sind Azure Container Instances und Azure Kubernetes Service.

Ein Beispiel für die Erstellung eines Images finden Sie im Dokument [Deploy an image classification model in Azure Container Instances](tutorial-deploy-models-with-aml.md) (Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances).

### <a name="image-registry"></a>Imageregistrierung

Die Imageregistrierung führt die Nachverfolgung von Images durch, die mit Ihren Modellen erstellt werden. Beim Erstellen des Images können Sie weitere Metadatentags bereitstellen. Metadatentags werden von der Imageregistrierung gespeichert und können abgefragt werden, um nach Ihrem Image zu suchen.

## <a name="deployment"></a>Bereitstellung

Eine Bereitstellung ist eine Instanziierung Ihres Images in einem Webdienst, der in der Cloud gehostet werden kann, oder in einem IoT-Modul für Bereitstellungen von integrierten Diensten. 

### <a name="web-service"></a>Webdienst

Für einen bereitgestellten Webdienst können Azure Container Instances, Azure Kubernetes Service oder FPGA (Field-Programmable Gate Arrays) verwendet werden.
Der Dienst wird aus einem Image erstellt, in dem Ihr Modell, das Skript und zugeordneten Dateien enthalten sind. Das Image verfügt über einen HTTP-Endpunkt mit Lastenausgleich, der die an den Webdienst gesendeten Bewertungsanforderungen empfängt.

Azure dient Ihnen als Hilfe beim Überwachen der Bereitstellung Ihres Webdiensts, indem die Application Insight- bzw. Modelltelemetriedaten erfasst werden, wenn Sie dieses Feature aktiviert haben. Die Telemetriedaten sind nur für Sie zugänglich und werden in Ihren Application Insights- und Speicherkontoinstanzen gespeichert.

Wenn Sie die automatische Skalierung aktiviert haben, führt Azure für Ihre Bereitstellung automatisch eine Skalierung durch.

Ein Beispiel für die Bereitstellung eines Modells als Webdienst finden Sie im Dokument [Deploy an image classification model in Azure Container Instances](tutorial-deploy-models-with-aml.md) (Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances).

### <a name="iot-module"></a>IoT-Modul

Ein bereitgestelltes IoT-Modul ist ein Docker-Container, der Ihr Modell und das zugeordnete Skript oder die Anwendung sowie alle zusätzlichen Abhängigkeiten enthält. Diese Module werden mit Azure IoT Edge auf Edgegeräten bereitgestellt. 

Wenn Sie die Überwachung aktiviert haben, erfasst Azure Telemetriedaten aus dem Modell im Azure IoT Edge-Modul. Die Telemetriedaten sind nur für Sie zugänglich und werden in Ihrer Speicherkontoinstanz gespeichert.

Azure IoT Edge stellt sicher, dass Ihr Modul ausgeführt wird, und überwacht das Gerät, auf dem es gehostet wird.

## <a name="datastore"></a>Datenspeicher

Ein Datenspeicher ist eine Speicherabstraktion eines Azure Storage-Kontos. Der Datenspeicher kann entweder einen Azure-Blobcontainer oder eine Azure-Dateifreigabe als Back-End-Speicher verwenden. Jeder Arbeitsbereich verfügt über einen Standarddatenspeicher, und Sie können auch zusätzliche Datenspeicher registrieren. 

Verwenden Sie die Python SDK-API oder Azure Machine Learning CLI, um Dateien zu speichern und aus dem Datenspeicher abzurufen. 

## <a name="run"></a>Ausführen

Eine Ausführung ist ein Datensatz, der die folgenden Informationen enthält:

* Metadaten zur Ausführung (Zeitstempel, Dauer usw.)
* Metriken, die von Ihrem Skript protokolliert werden
* Ausgabedateien, die automatisch vom Experiment gesammelt oder explizit von Ihnen hochgeladen werden
* Eine Momentaufnahme des Verzeichnisses, in dem Ihre Skripts enthalten sind, vor der Ausführung

Eine Ausführung wird ausgelöst, wenn Sie ein Skript zum Trainieren eines Modells übermitteln. Eine Ausführung kann über null oder mehr untergeordnete Elemente verfügen. Die Ausführung der obersten Ebene weist also unter Umständen zwei untergeordnete Ausführungen auf, die beide jeweils selbst untergeordnete Ausführungen haben können.

Ein Beispiel für die Anzeige von Ausführungen, die von einem Trainingsmodell erzeugt werden, finden Sie im Dokument [Schnellstart: Erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md).

## <a name="experiment"></a>Experiment

Ein Experiment ist eine Gruppierung vieler Ausführungen aus einem bestimmten Skript. Es gehört immer zu einem Arbeitsbereich. Beim Übermitteln einer Ausführung geben Sie einen Experimentnamen an. Die Informationen für die Ausführung werden unter diesem Experiment gespeichert. Wenn Sie eine Ausführung übermitteln und einen nicht vorhandenen Experimentnamen angeben, wird automatisch ein neues Experiment mit diesem Namen erstellt.

Ein Beispiel für die Verwendung eines Experiments finden Sie im Dokument [Schnellstart: Erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md).

## <a name="compute-target"></a>Computeziel

Ein Computeziel ist die Computeressource, die zum Ausführen Ihres Trainingsskripts oder Hosten Ihrer Webdienstbereitstellung verwendet wird. Die unterstützten Computeziele lauten: 

* Ihr lokaler Computer
* Eine Linux-VM in Azure (z.B. die Data Science Virtual Machine)
* Azure Batch AI-Cluster
* Apache Spark für HDInsight
* Azure Container Instances
* Azure Kubernetes Service

Computeziele sind einem Arbeitsbereich zugeordnet. Andere Computeziele als der lokale Computer werden von Benutzern des Arbeitsbereichs gemeinsam genutzt.

Die meisten Computeziele können direkt über den Arbeitsbereich erstellt werden, indem das Azure-Portal, das Azure Machine Learning SDK oder die Azure CLI verwendet wird. Wenn Sie über Computeziele verfügen, die von einem anderen Prozess erstellt wurden (z.B. mit dem Azure-Portal oder der Azure CLI), können Sie sie Ihrem Arbeitsbereich hinzufügen (bzw. anfügen). Einige Computeziele müssen außerhalb des Arbeitsbereichs erstellt und dann angefügt werden.

Informationen zum Auswählen eines Computeziels für das Training finden Sie im Dokument [Auswählen und Verwenden eines Computeziels zum Trainieren Ihres Modells](how-to-set-up-training-targets.md).

Informationen zum Auswählen eines Computeziels für die Bereitstellung finden Sie im Dokument [Deploy models with the Azure Machine Learning service](how-to-deploy-and-where.md) (Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst).

## <a name="run-configuration"></a>Laufzeitkonfiguration

Eine Laufzeitkonfiguration ist ein Satz mit Anweisungen, mit denen definiert wird, wie ein Skript auf einem bestimmten Computeziel ausgeführt werden sollte. Sie umfasst einen weiten Bereich von Verhaltensdefinitionen, z.B. die Vorgabe, ob eine vorhandene Python-Umgebung oder eine aus einer Spezifikation erstellte Conda-Umgebung verwendet werden soll.

Eine Laufzeitkonfiguration kann in einer Datei in dem Verzeichnis beständig gespeichert werden, in dem Ihr Trainingsskript enthalten ist, oder als In-Memory-Objekt erstellt und zum Übermitteln einer Ausführung verwendet werden.

Beispiele für Laufzeitkonfigurationen finden Sie im Dokument [Auswählen und Verwenden eines Computeziels zum Trainieren Ihres Modells](how-to-set-up-training-targets.md).

## <a name="training-script"></a>Trainingsskript

Zum Trainieren eines Modells geben Sie das Verzeichnis an, in dem das Trainingsskript und die zugehörigen Dateien enthalten sind. Außerdem geben Sie einen Experimentnamen an, der zum Speichern der während des Trainings gesammelten Informationen verwendet wird. Beim Training wird das gesamte Verzeichnis in die Trainingsumgebung (Computeziel) kopiert und das von der Laufzeitkonfiguration angegebene Skript gestartet. Darüber hinaus wird auch eine Momentaufnahme des Verzeichnisses unter dem Experiment im Arbeitsbereich gespeichert.

Ein Beispiel für die Verwendung von Skripts zum Trainieren eines Modells finden Sie unter [Schnellstart: Erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md).

## <a name="logging"></a>Protokollierung

Verwenden Sie beim Entwickeln Ihrer Lösung das Azure Machine Learning Python SDK in Ihrem Python-Skript, um beliebige Metriken zu protokollieren. Fragen Sie nach der Ausführung die Metriken ab, um zu ermitteln, ob von der Ausführung das Modell erzeugt wurde, das Sie bereitstellen möchten. 

## <a name="snapshot"></a>Momentaufnahme

Beim Übermitteln einer Ausführung komprimiert Azure Machine Learning das Verzeichnis, in dem das Skript als ZIP-Datei enthalten ist, und sendet es an das Computeziel. Die ZIP-Datei wird dann erweitert, und das Skript wird ausgeführt. Azure Machine Learning speichert die ZIP-Datei im Rahmen der Ausführungsaufzeichnung zudem als Momentaufnahme. Alle Benutzer mit Zugriff auf den Arbeitsbereich können eine Ausführungsaufzeichnung durchsuchen und die Momentaufnahme herunterladen.

## <a name="activity"></a>Aktivität

Eine Aktivität stellt einen Vorgang mit langer Ausführungsdauer dar. Die folgenden Vorgänge sind Beispiele für Aktivitäten:

* Erstellen oder Löschen eines Computeziels
* Ausführen eines Skripts auf einem Computeziel

Aktivitäten können Benachrichtigungen über das SDK oder eine Webbenutzeroberfläche bereitstellen, damit Sie den Status dieser Vorgänge auf einfache Weise überwachen können.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die folgenden Links für den Einstieg in Azure Machine Learning:

* [Was ist der Azure Machine Learning-Dienst (Vorschauversion)?](overview-what-is-azure-ml.md)
* [Schnellstart: Erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md)
* [Tutorial 1: Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-train-models-with-aml.md)
