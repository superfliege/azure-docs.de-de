---
title: Vergleichen der Produktoptionen für maschinelles Lernen
titleSuffix: Microsoft
description: Vergleichen Sie die Auswahl der Produkte von Microsoft zum Erstellen, Bereitstellen und Verwalten Ihrer Modelle für maschinelles Lernen. Treffen Sie die Wahl unter den verschiedenen für Ihre Lösung verfügbaren Produkten.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 1/17/2019
ms.openlocfilehash: d5660d1a4d5d2a26af57e2a2411552e3f6267379
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54389141"
---
# <a name="what-are-the-machine-learning-products-at-microsoft"></a>Welche Machine Learning-Produkte bietet Microsoft?

Microsoft bietet eine Reihe von Produkten zum Erstellen, Bereitstellen und Verwalten Ihrer Modelle für maschinelles Lernen. Vergleichen Sie diese Produkte, und wählen Sie aus, was Sie benötigen, um Ihre Lösungen für maschinelles Lernen so effektiv wie möglich zu entwickeln.


**Cloudbasierte Optionen**

In der Azure-Cloud stehen die folgenden Optionen für maschinelles Lernen zur Verfügung.

| Cloudoptionen | Funktionsbeschreibung | Gebotene Möglichkeiten |
|-|-|-|
| [Azure Machine Learning Service](#azure-machine-learning-services) | Verwalteter Clouddienst für ML  | Trainieren, Bereitstellen und Verwalten von Modellen in Azure mithilfe von Python und CLI |
| [Azure Machine Learning Studio](#azure-machine-learning-studio) | Visuelle Drag-&-Drop-Oberfläche für ML | Erstellen von, Experimentieren mit und Bereitstellen von Modellen mithilfe von vorkonfigurierten Algorithmen (Python und R)|
| [Azure Databricks](#azure-databricks) | Spark-basierte Analyseplattform | Erstellen und Bereitstellen von Modellen und Datenworkflows |
| [Azure Cognitive Services](#azure-cognitive-services) | Azure-Dienste mit vorkonfigurierten KI- und ML-Modellen | Fügen Sie Ihren Apps auf einfache Weise intelligente Features hinzu |
| [Azure Data Science Virtual Machine](#azure-data-science-virtual-machine) | Virtueller Computer mit vorinstallierten Data Science-Tools | Entwickeln von ML-Lösungen in einer vorkonfigurierten Umgebung |

**Lokale Optionen**

In der lokalen Umgebung stehen für maschinelles Lernen die folgenden Optionen zur Verfügung. Lokale Server können auch auf einem virtuellen Computer in der Cloud ausgeführt werden.

| Lokale&nbsp;Optionen | Funktionsbeschreibung | Gebotene Möglichkeiten |
|-|-|-|
| [SQL Server Machine Learning Services](#sql-server-machine-learning-services) | In SQL eingebettete Analyse-Engine | Erstellen und Bereitstellen von Modellen innerhalb von SQL Server |
| [Microsoft Machine Learning Server](#microsoft-machine-learning-server) | Eigenständiger Enterprise-Server für prädiktive Analyse | Erstellen und Bereitstellen von Modellen mit R und Python |

**Entwicklungstools**

Für maschinelles Lernen stehen die folgenden Entwicklungstools zur Verfügung.

| Entwicklungstools | Funktionsbeschreibung | Gebotene Möglichkeiten |
|-|-|-|
| [ML.NET](#mlnet) | Plattformübergreifendes Open-Source-ML-SDK | Entwickeln von ML-Lösungen für .NET-Anwendungen |
| [Windows ML](#windows-ml) | Windows 10-ML-Plattform | Auswerten von trainierten Modellen auf einem Windows 10-Gerät |







## <a name="azure-machine-learning-service"></a>Azure Machine Learning-Dienst

[Azure Machine Learning Service](overview-what-is-azure-ml.md) ist ein vollständig verwalteter Clouddienst, der zum Trainieren, Bereitstellen und Verwalten von ML-Modellen in großem Maßstab verwendet wird. Er unterstützt ohne Einschränkungen alle Open Source-Technologien. Sie können dadurch Zehntausende von verschiedenen Open Source-Paketen für Python wie TensorFlow, PyTorch und scikit-learn verwenden. Außerdem stehen umfassende Tools zur Verfügung, wie etwa [Azure-Notebooks](https://notebooks.azure.com/), [Jupyter-Notebooks](http://jupyter.org) oder die Erweiterung [Azure Machine Learning für Visual Studio Code](https://aka.ms/vscodetoolsforai), die das Untersuchen und Transformieren von Daten und das anschließende Trainieren und Bereitstellen von Modellen einfach machen. Der Azure Machine Learning Service beinhaltet Features zur Automatisierung der Modellgenerierung und zum einfachen, effizienten und präzisen Optimieren.

Verwenden Sie den Azure Machine Learning Service, um ML-Modelle mithilfe von Python und CLI im Cloudmaßstab zu trainieren, bereitzustellen und zu verwalten.

Probieren Sie die [kostenlose oder kostenpflichtige Version des Azure Machine Learning Service](http://aka.ms/AMLFree) aus.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

Mit [Azure Machine Learning Studio](../studio/what-is-ml-studio.md) steht Ihnen ein interaktiver visueller Arbeitsbereich zur Verfügung, den Sie zum schnellen und komfortablen Erstellen, Testen und Bereitstellen vorkonfigurierter Algorithmen für maschinelles Lernen verwenden können. Machine Learning Studio veröffentlicht Modelle als Webdienste, die von benutzerdefinierten Apps oder BI-Tools wie Excel problemlos genutzt werden können.
Es ist keine Programmierung erforderlich – Sie konstruieren ihr Modell für maschinelles Lernen durch Verbinden von Datasets und Analysemodulen auf einer interaktiven Zeichnungsfläche und stellen es anschließend mit ein paar Klicks bereit.

Verwenden Sie Machine Learning Studio, wenn Sie Modelle entwickeln und bereitstellen möchten, ohne Code schreiben zu müssen.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/what-is-azure-databricks) ist eine Apache Spark-basierte Analyseplattform, die für die Microsoft Azure-Clouddienstplattform optimiert ist. Databricks ist in Azure integriert, um Folgendes zu ermöglichen: Einrichtung mit nur einem Klick, optimierte Workflows und einen interaktiven Arbeitsbereich für die Zusammenarbeit von Datenspezialisten, Data Engineers und Business Analysts.
Verwenden Sie Python-, R-, Scala- und SQL-Code in webbasierten Notebooks zum Abfragen, Visualisieren und Modellieren von Daten.

Verwenden Sie Databricks, wenn Sie gemeinsam an der Erstellung von Lösungen für maschinelles Lernen auf Apache Spark arbeiten möchten.

## <a name="azure-cognitive-services"></a>Azure Cognitive Services

Bei [Azure Cognitive Services](/azure/cognitive-services/welcome) handelt es sich um eine Gruppe von APIs, mit denen Sie Apps erstellen können, für die natürliche Kommunikationsmethoden genutzt werden. Mit diesen APIs können Ihre Apps mit nur wenigen Codezeilen sehen, hören, sprechen, verstehen und Benutzeranforderungen interpretieren. Fügen Sie Ihren Apps auf einfache Weise intelligente Features hinzu, z.B.: 

- Emotions- und Stimmungserkennung
- Maschinelles Sehen und Spracherkennung
- Sprachverständnis (LUIS)
- Wissen und Suche

Verwenden Sie Cognitive Services, um Apps übergreifend für Geräte und Plattformen zu entwickeln. Die APIs werden ständig verbessert und lassen sich einfach einrichten.

## <a name="azure-data-science-virtual-machine"></a>Azure Data Science Virtual Machine

[Azure Data Science Virtual Machine](../data-science-virtual-machine/overview.md) ist eine benutzerdefinierte VM-Umgebung in der Microsoft Azure-Cloud, die speziell für Data Science konfiguriert wurde. Es hat viele beliebte Data Science und andere Tools vorinstalliert und vorkonfiguriert, damit Sie sofort intelligente Anwendungen für die erweiterte Analyse erstellen können.

Die Data Science Virtual Machine wird als Ziel für den Azure Machine Learning Service unterstützt.
Sie ist in Versionen für Windows und Linux Ubuntu verfügbar. (Azure Machine Learning Service wird unter Linux CentOS nicht unterstützt.)
Spezifische Versionsinformationen und eine Liste mit dem Lieferumfang finden Sie unter [Introduction to the Azure Data Science Virtual Machine](../data-science-virtual-machine/overview.md) (Einführung in die Azure Data Science Virtual Machine).

Verwenden Sie die Data Science-VM, wenn Sie Ihre Aufträge auf einem einzelnen Knoten ausführen oder hosten müssen. Eine anderer Grund für die Nutzung kann das zentrale Hochskalieren eines einzelnen Computers per Remotezugriff sein.

## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning Services

[SQL Server Microsoft Machine Learning Service](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) fügt SQL Server-Datenbanken statistische Analyse, Datenvisualisierung und prädiktive Analyse für relationale Daten in R und Python hinzu. R- und Python-Bibliotheken von Microsoft beinhalten fortgeschrittene Modellierungs- und ML-Algorithmen, die parallel und im großen Maßstab in SQL Server ausgeführt werden können.

Verwenden Sie SQL Server Machine Learning Services, wenn Sie integrierte KI und prädiktive Analyse von relationalen Daten in SQL Server benötigen.

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Server

[Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server) ist ein Unternehmensserver zum Hosten und Verwalten von parallelen und verteilten Workloads für R- und Python-Prozesse. Microsoft Machine Learning Server kann unter Linux, Windows, Hadoop und Apache Spark sowie in [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/) ausgeführt werden. Es stellt ein Ausführungsmodul für Lösungen bereit, die mit [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) und [MicrosoftML-Paketen](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package) erstellt wurden, und erweitert Open-Source-R und -Python um Unterstützung für Hochleistungsanalyse, statistische Analyse, maschinelles Lernen und Datasets von erheblicher Größe. Diese Funktionalität wird anhand von eigenen Paketen bereitgestellt, die mit dem Server installiert werden. Für die Entwicklung können Sie IDEs wie beispielsweise [R Tools für Visual Studio](https://www.visualstudio.com/vs/rtvs/) und [Python Tools für Visual Studio](https://www.visualstudio.com/vs/python/) verwenden.

Verwenden Sie Microsoft Machine Learning Server, wenn Sie mit R und Python erstellte Modelle auf einem Server erstellen und betriebsbereit machen müssen, oder verteilen Sie R- und Python-Training in großem Maßstab auf einem Hadoop- oder Spark-Cluster.

## <a name="mlnet"></a>ML.NET

[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/) ist ein kostenloses, plattformübergreifendes Open-Source-Framework für maschinelles Lernen, das es Ihnen ermöglicht, benutzerdefinierte Lösungen für maschinelles Lernen zu erstellen und sie in Ihre .NET-Anwendungen zu integrieren.

Verwenden Sie ML.NET, wenn Sie Lösungen für maschinelles Lernen in Ihre .NET-Anwendungen integrieren möchten.

## <a name="windows-ml"></a>Windows ML

Mit [Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/) können Sie trainierte Modelle für maschinelles Lernen in Ihren Anwendungen verwenden, wobei trainierte Modelle lokal auf Windows 10-Geräten ausgewertet werden.

Verwenden Sie Windows ML, wenn Sie trainierte Modelle für maschinelles Lernen innerhalb Ihrer Windows-Anwendungen einsetzen möchten.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu allen Entwicklungsprodukten für Künstliche Intelligenz (KI), die von Microsoft erhältlich sind, finden Sie unter [Microsoft-KI-Plattform](https://www.microsoft.com/ai)
- Schulungen zum Entwickeln von KI-Lösungen finden Sie unter [Microsoft AI School](https://aischool.microsoft.com/learning-paths)
