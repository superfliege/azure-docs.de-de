---
title: Was passiert mit Azure Batch AI? | Microsoft-Dokumentation
description: Erfahren Sie, was mit Azure Batch AI und der Azure Machine Learning Service-Computeoption passiert.
ms.service: batch-ai
services: batch-ai
ms.topic: overview
ms.author: jmartens
author: j-martens
ms.date: 2/28/2019
ms.openlocfilehash: edd6a7e5f385d766d51631d77f5889233af2469a
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57194501"
---
# <a name="whats-happening-to-azure-batch-ai"></a>Was passiert mit Azure Batch AI?

**Der Azure Batch AI-Dienst wird im März eingestellt.** Die skalierbaren Trainings- und Bewertungsfunktionen von Batch AI sind jetzt im [Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md) verfügbar, der am 4. Dezember 2018 in die allgemeine Verfügbarkeit überging.

Der Azure Machine Learning Service umfasst neben vielen weiteren Funktionen für das maschinelle Lernen ein cloudbasiertes, verwaltetes Computeziel zum Trainieren, Bereitstellen und Bewerten von Machine Learning-Modellen. Dieses Computeziel heißt [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Beginnen Sie noch heute mit der Migration und Nutzung](#migrate). Sie können über die entsprechenden [Python SDKs](../machine-learning/service/quickstart-create-workspace-with-python.md), die Befehlszeilenschnittstelle und das [Azure-Portal](../machine-learning/service/quickstart-get-started.md) mit dem Azure Machine Learning Service interagieren.

## <a name="support-timeline"></a>Unterstützungszeitachse

Sie können derzeit Ihre vorhandenen Azure Batch AI-Abonnements wie gewohnt weiter verwenden. Es sind jedoch keine **neuen Abonnements** und keine neuen Investitionen möglich.

Ab dem &nbsp;31. März 2019&nbsp; funktionieren nicht verwendete Batch AI-Abonnements nicht mehr.

## <a name="compare-to-azure-machine-learning"></a>Vergleich zu Azure Machine Learning
Azure Machine Learning Service ist ein Clouddienst, mit dem Sie Modelle für maschinelles Lernen trainieren, bereitstellen, automatisieren und verwalten und dabei von den Vorteilen der Cloud profitieren können. Allgemeine Informationen zu Azure Machine Learning Service finden Sie in [dieser Übersicht](../machine-learning/service/overview-what-is-azure-ml.md).
 

Der Entwicklungslebenszyklus für ein Modell umfasst in der Regel folgende Phasen: Datenaufbereitung, Training/Experimente und Bereitstellung. Dieser End-to-End-Zyklus kann mithilfe von Machine Learning-Pipelines orchestriert werden.

![Flussdiagramm](./media/overview-what-happened-batch-ai/lifecycle.png)


Weitere Informationen zur Funktionsweise und zu den wichtigsten Konzepten des Diensts finden Sie [hier](../machine-learning/service/concept-azure-machine-learning-architecture.md). Viele der Konzepte des Modelltrainingsworkflows ähneln den Konzepten aus Batch AI. 

Dies wird in der folgenden Zuordnung veranschaulicht:
 
|Batch AI-Dienst|  Azure Machine Learning-Dienst|
|:--:|:---:|
|Arbeitsbereich|Arbeitsbereich|
|Cluster|   Compute vom Typ `AmlCompute`|
|Dateiserver|Datenspeicher|
|Experimente|Experimente|
|Aufträge|Ausführungen (geschachtelte Ausführungen möglich)|
 
Hier noch eine andere Darstellung der gleichen Tabelle zur weiteren Veranschaulichung:
 
### <a name="batch-ai-hierarchy"></a>Batch AI-Hierarchie
![Flussdiagramm](./media/overview-what-happened-batch-ai/batchai-heirarchy.png) 
 
### <a name="azure-machine-learning-service-hierarchy"></a>Hierarchie von Azure Machine Learning Service
![Flussdiagramm](./media/overview-what-happened-batch-ai/azure-machine-learning-service-heirarchy.png) 

## <a name="platform-capabilities"></a>Plattformfunktionen
Azure Machine Learning Service bietet mehrere neue Funktionen. Hierzu zählt unter anderem ein End-to-End-Trainings-/Bereitstellungsstapel, den Sie für Ihre KI-Entwicklung verwenden können, ohne Azure-Ressourcen verwalten zu müssen. Die folgende Tabelle enthält eine Gegenüberstellung der Trainingsfeatureunterstützung für die beiden Dienste:

|Feature|Batch AI-Dienst|Azure Machine Learning-Dienst|
|-------|:-------:|:-------:|
|Wahl der VM-Größe |CPU/GPU    |CPU/GPU. Unterstützt auch FPGA für Rückschlüsse|
|KI-bereiter Cluster (Treiber, Docker usw.)|  Ja |Ja|
|Knotenvorbereitung| Ja|    Nein |
|Wahl der Betriebssystemfamilie   |Teilweise    |Nein |
|Dedizierte VMs und VMs mit niedriger Priorität  |Ja    |Ja|
|Automatische Skalierung   |Ja    |Ja (standardmäßig)|
|Wartezeit für automatische Skalierung  |Nein  |Ja|
|SSH    |Ja|   Ja|
|Einbindung auf Clusterebene |Ja (Dateifreigaben, Blobs, NFS, benutzerdefiniert)   |Ja (Einbindung oder Download Ihres Datenspeichers)|
|Verteiltes Training|  Ja |Ja|
|Ausführungsmodus|    Virtueller Computer oder Container|    Container|
|Benutzerdefiniertes Containerimage|    Ja |Ja|
|Beliebiges Toolkit    |Ja    |Ja (Python-Skript ausführen)|
|Auftragsvorbereitung|    Ja |Noch nicht|
|Bereitstellung auf Auftragsebene |Ja (Dateifreigaben, Blobs, NFS, benutzerdefiniert)   |Ja (Dateifreigaben, Blobs)|
|Auftragsüberwachung     |über „GetJob“|    über Ausführungsverlauf (umfangreichere Informationen, benutzerdefinierte Runtime zum Pushen weiterer Metriken)|
|Abrufen von Auftragsprotokollen und Dateien/Modellen |   über „ListFiles“ und Storage-APIs  |über den Artefaktdienst|
 |Unterstützung von Tensorboard   |Nein |    Ja|
|Kontingente auf VM-Familienebene |Ja    |Ja (mit Übernahme Ihrer vorherigen Kapazität)|
 
Neben den Features aus der obigen Tabelle stehen in Azure Machine Learning Service weitere Features zur Verfügung, die in Batch AI bisher nicht unterstützt wurden:

|Feature|Batch AI-Dienst|Azure Machine Learning-Dienst|
|-------|:-------:|:-------:|
|Umgebungsvorbereitung    |Nein  |Ja (Conda-Vorbereitung und Upload in ACR)|
|Hyperparameteroptimierung  |Nein |    Ja|
|Modellverwaltung   |Nein  |Ja|
|Operationalisierung/Bereitstellung| Nein   |Über AKS und ACI|
|Vorbereitung der Daten   |Nein  |Ja|
|Computeziele    |Virtuelle Azure-Computer  |Lokal, Batch AI (als „AmlCompute“), Databricks, HDInsight|
|Automatisiertes maschinelles Lernen |Nein |    Ja|
|Pipelines  |Nein  |Ja|
|Batchbewertung  |Ja    |Ja|
|Portal-/CLI-Unterstützung|    Ja |Ja|


## <a name="programming-interfaces"></a>Programmierschnittstellen

Die folgende Tabelle enthält die verschiedenen verfügbaren Programmierschnittstellen für den jeweiligen Dienst:
    
|Feature|Batch AI-Dienst|Azure Machine Learning-Dienst|
|-------|:-------:|:-------:|
|SDK    |Java, C#, Python, Node.js   |Python (konfigurations- und estimatorbasierte Ausführung für gängige Frameworks)|
|Befehlszeilenschnittstelle (CLI)    |Ja    |Noch nicht|
|Azure-Portal   |Ja    |Ja (mit Ausnahme der Auftragsübermittlung)|
|REST-API   |Ja    |Ja, aber verteilt auf Microservices|


Wenn Sie von der Batch AI-Vorschauversion auf die allgemein verfügbare Version von Azure Machine Learning Service upgraden, stehen Ihnen benutzerfreundlichere Konzepte wie Estimatoren und Datenspeicher zur Verfügung. Darüber hinaus profitieren Sie von Azure-SLAs und Kundensupport für allgemein verfügbare Versionen.

Azure Machine Learning Service bietet zudem neue Funktionen wie automatisiertes maschinelles Lernen, Hyperparameteroptimierung und ML-Pipelines, die bei den meisten umfangreichen KI-Workloads hilfreich sind. Dank der Möglichkeit, ein trainiertes Modell ohne Wechsel zu einem separaten Dienst bereitzustellen, können Sie die Data Science-Schleife von der Datenaufbereitung (mit dem Datenaufbereitungs-SDK) bis hin zur Operationalisierung und Modellüberwachung abwickeln.

<a name="migrate"></a>
## <a name="migrate"></a>Migrieren

Im Artikel [Migrating from Batch AI to Azure Machine Learning service](how-to-migrate.md) (Migrieren von Batch AI zu Azure Machine Learning Service) wird erläutert, wie Sie eine Migration ausführen und wie der von Ihnen verwendete Code mit dem Code in Azure Machine Learning Service zusammenhängt.

## <a name="get-support"></a>Support

Batch AI wird voraussichtlich am 31. März ausgemustert. Schon jetzt können für den Dienst keine neuen Abonnements mehr registriert werden (es sei denn, es wurde zuvor über den Support eine Ausnahme beantragt und genehmigt).  Sollten Sie Fragen oder Feedback in Verbindung mit der Migration zu Azure Machine Learning Service haben, [wenden Sie sich an uns](mailto:AzureBatchAITrainingPreview@service.microsoft.com).

Azure Machine Learning Service ist ein allgemein verfügbarer Dienst. Er verfügt somit über eine verbindliche SLA und bietet eine Auswahl verschiedener Supportpläne.

Die Nutzung der Azure-Infrastruktur kostet für Azure Machine Learning Service voraussichtlich gleich viel wie für den Batch AI-Dienst, da in beiden Fällen lediglich der Preis für die zugrunde liegenden Computeressourcen in Rechnung gestellt wird. Weitere Informationen finden Sie unter [Azure-Preisrechner](https://azure.microsoft.com/pricing/details/machine-learning-service/).

Die regionale Verfügbarkeit für die beiden Dienste können Sie im [Azure-Portal](https://azure.microsoft.com/global-infrastructure/services/?products=batch-ai,machine-learning-service&regions=all) anzeigen.


## <a name="next-steps"></a>Nächste Schritte

+ Lesen Sie, wie Sie eine [Migration ausführen](how-to-migrate.md) und wie der von Ihnen verwendete Code mit Code in Azure Machine Learning Service zusammenhängt.

+ Lesen Sie den [Überblick über den Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Konfigurieren Sie Computeziele zum Trainieren von Modellen](../machine-learning/service/how-to-set-up-training-targets.md) mit dem Azure Machine Learning Service.

+ Lesen Sie die [Azure-Roadmap](https://azure.microsoft.com/updates/), um mehr über Updates anderer Azure-Dienste zu erfahren.
