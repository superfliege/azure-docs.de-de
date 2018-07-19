---
title: Häufig gestellte Fragen zur Azure Machine Learning 2017-Vorschauversion | Microsoft-Dokumentation
description: Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Funktionen der Azure Machine Learning-Vorschauversion.
services: machine-learning
author: serinakaye
ms.author: serinak
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/30/2017
ms.openlocfilehash: 94a1f3bbba83e8e71cf9440b5ded0784f4616c99
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38674154"
---
# <a name="azure-machine-learning-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure Machine Learning

Azure Machine Learning ist ein vollständig verwalteter Azure-Dienst, mit dem Sie Machine Learning- und AI-Modelle erstellen, testen, verwalten und bereitstellen können. Unsere Dienste und die als Download verfügbare Anwendung bieten einen Code-First-Ansatz, bei dem die Cloud, lokale Ressourcen und Edge genutzt werden, um Leistung, Geschwindigkeit und Flexibilität für Trainings-, Bereitstellungs-, Verwaltungs- und Überwachungsmodelle sicherzustellen. Alternativ bietet Azure Machine Learning Studio eine browserbasierte, visuelle Drag & Drop-Erstellungsumgebung, in der keine Codierung erforderlich ist. 

## <a name="general-product-questions"></a>Allgemeine Fragen zum Produkt

**Welche anderen Azure-Dienste sind erforderlich?**

Azure Blob Storage und Azure Container Registry werden von Azure Machine Learning verwendet. Darüber hinaus müssen Sie Computeressourcen wie eine Data Science-VM oder einen HDInsight-Cluster bereitstellen. Computing und Hosting sind auch beim Bereitstellen von Webdiensten wie [Azure Container Service](https://docs.microsoft.com/azure/aks) erforderlich.

**In welcher Beziehung steht Azure Machine Learning zu Microsoft Machine Learning-Diensten in SQL Server 2017?**   

Machine Learning-Dienste in SQL Server 2017 ist eine erweiterbare, skalierbare Plattform zum Integrieren von Machine Learning-Aufgaben in Datenbankworkflows. Er eignet sich hervorragend für Szenarien, in denen eine lokale Lösung benötigt wird, wenn beispielsweise das Verschieben von Daten zu kostspielig oder nicht praktikabel ist. Unsere Cloud- oder Hybrid-Workloads hingegen eignen sich ausgezeichnet für unsere neuen Azure-Dienste. 

**Werden sowohl Python als auch R unterstützt? Und was ist mit anderen Programmiersprachen wie C++?**

Derzeit wird ausschließlich Python unterstützt. Wir arbeiten an der R-Integration, deren Verfügbarkeit in Kürze zu erwarten ist. 

**In welcher Beziehung steht Azure Machine Learning zu Microsoft Machine Learning für Spark?**

MMLSpark bietet umfassende Learning- und Data Science-Tools für Apache Spark, wobei der Schwerpunkt auf Produktivität, unkompliziertem Experimentieren und modernsten Algorithmen liegt. MMLSpark bietet die Integration von Spark Machine Learning-Pipelines mit dem Microsoft Cognitive Toolkit und OpenCV. Sie können leistungsstarke, hochgradig skalierbare Prognose- und Analysemodelle für Bild- und Textdaten erstellen. MMLSpark ist mit einer Open-Source-Lizenz erhältlich und als Gruppe von verwendbaren Modellen und Algorithmen in AML Workbench enthalten. Wenn Sie weitere Informationen zu MMLSpark wünschen, sehen Sie unsere Produktdokumentation ein. 

**Welche Versionen von Spark werden von den neuen Tools und Diensten unterstützt?**

Workbench enthält und unterstützt derzeit MMLSpark, Version 0.8; diese ist kompatibel mit Apache Spark 2.1. Sie haben auch die Möglichkeit, ein GPU-fähiges Docker-Image von MMLSpark 0.8 auf virtuellen Linux-Computern zu verwenden.

## <a name="experimentation-service"></a>ML-Experimentieren-Dienst

**Was ist der Azure Machine Learning-Experimentieren-Dienst?**

Der Experimentieren-Dienst ist ein verwalteter Azure-Dienst, der Machine Learning-Experimente auf einem neuen Niveau möglich macht. Experimente können sowohl lokal als auch in der Cloud erstellt werden. Erstellen Sie sie zunächst auf einem Desktop, und skalieren Sie sie dann auf virtuellen Computern oder in Spark-Clustern. Azure-VMs mit der neuesten GPU-Technologie ermöglichen Ihnen ein schnelles und effektives Deep Learning. Wir haben zudem eine umfassende Integration mit Git eingeschlossen, sodass Sie sich für Codeverfolgung, Konfiguration und Zusammenarbeit problemlos in vorhandene Workflows einklinken können. 

**Wie werden für ML-Experimentieren Gebühren berechnet?**

Die ersten beiden Ihrem Azure Machine Learning-Experimentieren-Dienst zugeordneten Benutzer sind kostenlos. Zusätzlichen Benutzern werden Gebühren gemäß dem Public Preview-Satz von 50 $/Monat in Rechnung gestellt. Weitere Informationen zu Preisen und Abrechnung finden Sie auf unserer Seite zu Preisen.

**Werden Gebühren entsprechend der Anzahl der von mir ausgeführten Experimente berechnet?**

Nein, mit ML-Experimentieren können Sie so viele Experimente wie nötig ausführen, und Gebühren werden lediglich entsprechend der Anzahl der Benutzer berechnet. Compute-Ressourcen für ML-Experimentieren werden separat in Rechnung gestellt. Wir empfehlen Ihnen, eine Reihe von Experimenten auszuführen, sodass Sie das bestgeeignete Modell für Ihre Lösung ermitteln können.   

**Welche spezifischen Arten von Compute- und Speicherressourcen kann ich verwenden?**

ML-Experimentieren kann Ihre Experimente auf lokalen Computern (direkt oder Docker-basiert), [Azure Virtual-VMs](https://azure.microsoft.com/services/virtual-machines/) und [HDInsight](https://azure.microsoft.com/services/hdinsight/) ausführen. Der Dienst greift zudem auf ein [Azure Storage](https://azure.microsoft.com/services/storage/)-Konto zu, um Ausführungsausgaben zu speichern, und er kann ein [Visual Studio Team Services](https://azure.microsoft.com/services/visual-studio-team-services/)-Konto für Versionskontrolle und Git-Speicherung nutzen. Beachten Sie, dass Ihnen verbrauchte Compute- und Speicherressourcen separat berechnet werden; hierbei gelten die jeweiligen Preise.  


## <a name="model-management"></a>Modellverwaltung

**Was ist die Azure Machine Learning-Modellverwaltung?**

Die Azure Machine Learning-Modellverwaltung ist ein verwalteter Azure-Dienst, anhand dessen Datenspezialisten und Entwicklerteams Prognosemodelle in einer Vielzahl von Umgebungen zuverlässig bereitstellen können. Git-Repositorys und Docker-Container bieten Nachverfolgbarkeit und Wiederholbarkeit. Modelle können in der Cloud, lokal oder in Edge zuverlässig bereitgestellt werden. Nach der Übernahme in die Produktion können Sie die Modellleistung verwalten und bei einer Leistungsverschlechterung proaktiv ein Neutraining ausführen. Sie können Modelle auf lokalen Computern, auf [Azure VMs](https://azure.microsoft.com/services/virtual-machines/), Spark auf [HDInsight](https://azure.microsoft.com/services/hdinsight/) oder Kubernetes-orchestrierten [Azure Container Service](https://azure.microsoft.com/services/container-service/)-Clustern bereitstellen.  

**Was ist ein „Modell“?**

Ein Modell ist die Ausgabe ausgeführter Experimente, die für die Modellverwaltung höher gestuft wurde. Ein Modell, das im Hosting-Konto registriert ist, wird auf Ihren Plan angerechnet, u.a. auch Modelle, die während des erneuten Trainings oder der Versionsiteration aktualisiert wurden.

**Was ist ein „verwaltetes Modell“?**

Ein Modell ist die Ausgabe eines Trainingsprozesses und die Anwendung eines Machine Learning-Algorithmus auf Trainingsdaten. Mit der Modellverwaltung können Sie Modelle als Webdienste bereitstellen, verschiedene Versionen Ihrer Modelle verwalten und deren Leistung und Metriken überwachen. „Verwaltete“ Modelle wurden bei einem Azure Machine Learning-Modellverwaltungskonto registriert. Betrachten Sie beispielsweise ein Szenario, in dem Sie versuchen, Umsätze zu prognostizieren. Während der Experimentierphase generieren Sie eine Vielzahl von Modellen und verwenden dabei unterschiedlich Datasets oder Algorithmen. Sie haben vier Modelle mit unterschiedlichem Genauigkeitsgrad generiert. Sie registrieren jedoch nur das Modell mit der höchsten Genauigkeit. Das registrierte Modell wird zu Ihrem ersten verwalteten Modell.
 
**Was ist eine „Bereitstellung“?**

Mit der Modellverwaltung können Sie Modelle als gepackte Webdienstcontainer in Azure bereitstellen. Diese Webdienste können mithilfe von REST-APIs aufgerufen werden. Jeder Webdienst zählt als eine Bereitstellung, und die Gesamtzahl der aktiven Bereitstellungen wird auf Ihren Plan angerechnet. Kehren wir zum Umsatzprognosebeispiel zurück. Wenn Sie das Modell mit der besten Leistung bereitstellen, wird Ihr Plan um eine Bereitstellung erhöht. Wenn Sie eine andere Version neu trainieren und bereitstellen, verfügen Sie über zwei Bereitstellungen. Wenn Sie feststellen, dass das neuere Modell besser ist und das ursprüngliche Modell löschen, wird Ihre Bereitstellungsanzahl um eine Bereitstellung reduziert.  

**Welche spezifischen Compute-Ressourcen sind für meine Bereitstellungen verfügbar?** 

Die Modellverwaltung kann Ihre Bereitstellungen als Docker-Container ausführen, die bei [Azure Container Service](https://azure.microsoft.com/services/container-service/) registriert sind, als [Azure-VMs](https://azure.microsoft.com/services/virtual-machines/) oder auf lokalen Computern. Weitere Bereitstellungsziele sind in Kürze verfügbar. Beachten Sie, dass Ihnen verbrauchte Compute- Ressourcen separat berechnet werden; hierbei gelten die jeweiligen Preise.     

**Kann ich mit der Azure Machine Learning-Modellverwaltung Modelle bereitstellen, die mit anderen Tools als ML-Experimentieren erstellt wurden?**

Die besten Ergebnisse erzielen Sie, wenn Sie mit ML-Experimentieren erstellte Modelle bereitstellen. Sie können jedoch auch Modelle bereitstellen, die Sie mit anderen Frameworks und Tools erstellt haben. Wir unterstützen eine Vielzahl von Modellen, u.a. MMLSpark, TensorFlow, Microsoft Cognitive Toolkit, scikit-learn, Keras usw. 

**Kann ich meine eigenen Azure-Ressourcen verwenden?**

Ja, ML-Experimentieren und die ML-Modellverwaltung wirken mit verschiedenen Azure-Datenspeichern, Azure-Compute-Workloads und anderen Azure-Diensten zusammen. Einzelheiten zu den erforderlichen Azure-Diensten finden Sie in unserer technischen Dokumentation.

**Unterstützen Sie sowohl lokale Bereitstellungsszenarien als auch Szenarien mit Bereitstellung in der Cloud?**

Ja. Wir unterstützen lokale und Cloud-Bereitstellungsszenarien über Docker-Container. Lokale Ausführungsziele: Docker-Bereitstellungen mit einem einzigen Knoten, [Microsoft SQL Server mit ML-Diensten](https://docs.microsoft.com/sql/advanced-analytics/r/r-services), Hadoop und Spark. Zudem unterstützen wir Cloudbereitstellungen über Docker. Hierzu zählen Folgende: gruppierte Bereitstellungen über Azure Container Service und Kubernetes, HDInsight, oder Spark-Cluster. Edge-Szenarien werden über Docker-Container und Azure IOT Edge unterstützt. 

**Kann ich ein Docker-Image ausführen, das unter Verwendung der Azure Machine Learning-CLI auf einem anderen Host erstellt wurde?**

Ja. Sie können das Image als Webdienst auf einem beliebigen Docker-Host verwenden, sofern der Host über ausreichende Ressourcen zum Hosten des Docker-Image verfügt.

**Wird das erneute Training von bereitgestellten Modellen unterstützt?**

Ja, können Sie mehrere Versionen desselben Modells bereitstellen. Die Modellverwaltung unterstützt Dienstaktualisierungen für alle aktualisierten Modelle und Images.

## <a name="workbench"></a>Workbench

**Was ist die Azure Machine Learning Workbench?**

Der Azure Machine Learning-Workbench ist eine unterstützende Anwendung, die für professionelle Datenanalysten entwickelt wurde. Verfügbar für Windows und Mac, bietet die Machine Learning Workbench Übersichts-, Verwaltungs- und Steuerungsfunktionen für Machine Learning-Lösungen. Die Machine Learning Workbench bietet Zugriff auf modernste AI-Frameworks von Microsoft und aus der Open-Source-Community. Wir haben die gängigsten Data Science-Toolkits eingeschlossen, einschließlich von TensorFlow, Microsoft Cognitive Toolkit, Spark ML, scikit-learn und anderen. Darüber hinaus haben wir auch die Integration mit populären Data Science-IDEs wie Jupyter-Notebooks, PyCharm und Visual Studio Code ermöglicht. Die Machine Learning Workbench verfügt über integrierte Datenvorbereitungsfunktionen, mit denen schnell Stichproben von Daten genommen und Daten rasch verstanden und vorbereitet werden können, ganz gleich, ob es sich um strukturierte oder unstrukturierte Daten handelt. Unser neues Datenvorbereitungstool [PROSE](https://microsoft.github.io/prose/) baut auf modernsten Technologien von Microsoft Research auf.  

**Ist die Workbench eine IDE?**

Nein. Die Machine Learning Workbench wurde als Begleitprodukt für beliebte IDEs wie Jupyter Notebooks, Visual Studio Code und PyCharm konzipiert, stellt jedoch keine IDE mit vollem Funktionsumfang dar. Die Machine Learning Workbench bietet einige grundlegende Textbearbeitungsfunktionen. Debugging, Intellisense und andere häufig verwendete IDE-Funktionen hingegen werden nicht unterstützt. Wir empfehlen, die jeweils bevorzugte IDE zum Entwickeln, Bearbeiten und Debuggen von Code zu verwenden. Möglicherweise möchten Sie auch die [Visual Studio Code-Tools für KI](https://www.visualstudio.com/downloads/ai-tools-vscode) ausprobieren.

**Werden für die Verwendung der Azure Machine Learning Workbench Gebühren berechnet?**

Nein. Die Azure Machine Learning Workbench ist eine kostenlose Anwendung. Sie können Sie entsprechend den jeweiligen Anforderungen auf beliebig viele Computer und für beliebig viele Benutzer herunterladen. Zum Verwenden der Azure Machine Learning Workbench müssen Sie über ein ML-Experimentieren-Konto verfügen. zu erstellen und zu verwalten.  

**Werden Befehlszeilenfunktionen unterstützt?**

Ja, Azure Machine Learning bietet eine komplette CLI-Schnittstelle. Die Machine Learning-CLI wird standardmäßig zusammen mit der Azure Machine Learning Workbench installiert. Sie wird auch als Bestandteil des virtuellen Linux Data Science-Computers unter Azure bereitgestellt und in die [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) integriert.


**Kann ich Jupyter-Notebooks mit der Workbench verwenden?**

Ja. Sie können Jupyter-Notebooks in der Workbench verwenden, wobei die Workbench die Client-Hostanwendung darstellt, ebenso wie Sie einen Browser als Client verwenden würden. 

**Welche Jupyter-Notebook-Kernels werden unterstützt?**

Die in die Workbench eingeschlossene aktuelle Version von Jupyter startet einen Python 3-Kernel sowie einen zusätzlichen Kernel für jede „runconfig“-Datei im Ordner „aml_config“. Unterstützte Konfigurationen:
- Lokales Python
- Python in lokalem Docker-Container oder Docker-Remotecontainer

## <a name="data-formats-and-capabilities"></a>Datenformate und -funktionen

**Welche Dateiformate werden derzeit für die Datenerfassung in der Workbench unterstützt?**

Die Datenvorbereitungstools in der Workbench unterstützen derzeit die Erfassung aus den folgenden Formaten: 
- Durch Trennzeichen getrennte Dateien wie CSV, TSV, usw.  
- Dateien mit fester Breite
- Nur-Text-Dateien
- Excel-Dateien (.xls/xlsx)
- JSON-Dateien
- Parquet-Dateien 
- Benutzerdefinierte Dateien (Skripts) Wenn Ihre Lösung die Datenerfassung aus zusätzlichen Quellen erfordert, kann Python-Code zu folgenden Zwecken verwendet werden... 

**Welche Datenspeicherorte werden derzeit unterstützt?**

Für die öffentliche Vorschau unterstützt die Workbench die Datenerfassung aus: 
- Lokale Festplatte oder zugeordneter Netzwerkspeicherort
- Azure-Blob oder Azure Storage (hierfür ist ein Azure-Abonnement erforderlich)
- Azure SQL Server
- Microsoft SQL Server


**Welche Arten von Daten-Wrangling, -Vorbereitung und -Transformationen sind verfügbar?**

Für die öffentliche Vorschau unterstützt die Workbench das Ableiten von Spalten anhand eines Beispiels, das Teilen von Spalten anhand eines Beispiels, Text-Clustering, das Behandeln fehlender Werte und vieles mehr.  Die Workbench unterstützt auch die Datentypkonvertierung, Datenaggregation (COUNT, MEAN, VARIANCE usw.) und komplexe Datenverknüpfungen. Eine vollständige Liste der unterstützten Funktionen finden Sie in der Produktdokumentation. 

**Gelten Einschränkungen hinsichtlich der Datengröße, die von Azure Machine Learning Workbench, ML-Experimentieren oder Modellverwaltung erzwungen werden?**

Nein, für die neuen Dienste gelten keinerlei Dateneinschränkungen. Es gibt jedoch Einschränkungen durch die jeweilige Umgebung, in der Sie Datenvorbereitung, Modelltraining, Experimente und Bereitstellung ausführen. Wenn Sie beispielsweise eine lokale Umgebung als Ziel für das Training festlegen, sind Sie durch den verfügbaren Speicherplatz auf der Festplatte beschränkt. Ist das Ziel hingegen HDInsight, sind Sie durch die jeweils zugeordnete Größe oder Compute-Einschränkungen beschränkt. 

## <a name="algorithms-and-libraries"></a>Algorithmen und Bibliotheken

**Welche Algorithmen werden in der Azure Machine Learning Workbench unterstützt?**

Unsere Vorschauprodukte und -dienste stützen sich auf die besten Ressourcen der Open-Source-Community. Wir unterstützen eine breite Palette von Algorithmen und Bibliotheken, einschließlich von TensorFlow, scikit-learn, Apache Spark und des Microsoft Cognitive Toolkit. Darüber hinaus schließt die Azure Machine Learning Workbench das [Microsoft revoscalepy](https://docs.microsoft.com/sql/advanced-analytics/python/what-is-revoscalepy)-Paket ein.

**In welcher Beziehung steht Azure Machine Learning zum Microsoft Cognitive Toolkit?**

Das [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) ist eines der vielen Frameworks, das von unseren neuen Tools und Diensten unterstützt wird. Das Cognitive Toolkit ist ein vereinheitlichtes Deep-Learning-Toolkit, mit dem Sie populäre Machine Learning-Modelle verwenden und miteinander kombinieren können, u.a. Deep Neural Networks für die Feedweiterleitung, Convolutional Nets, Sequence-to-Sequence und Recurrent Networks. Weitere Informationen zum Microsoft Cognitive Toolkit erhalten Sie in unserer [Produktdokumentation](https://docs.microsoft.com/cognitive-toolkit/). 