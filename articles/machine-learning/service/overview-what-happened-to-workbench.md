---
title: Was ist mit Workbench passiert?
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, was mit der Workbench-Anwendung geschehen ist, was sich beim Azure Machine Learning-Dienst geändert hat und welcher Zeitplan für die Unterstützung gilt.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: c9559e07cc70cbd7adafd75c23b9e67d45bee48a
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184304"
---
# <a name="what-is-happening-to-workbench-in-azure-machine-learning-service"></a>Was passiert mit Workbench im Azure Machine Learning-Dienst?

Die Workbench-Anwendung und einige andere frühe Features sind veraltet und wurden im Release vom 2018 ersetzt, um den Weg für eine verbesserte [Architektur](concept-azure-machine-learning-architecture.md) frei zu machen. Die Version enthält viele wichtige Updates, die durch Feedback von Kunden ausgelöst wurden, um Ihr Benutzererlebnis verbessern. Die Kernfunktionen von experimentellen Läufen bis zur Modellbereitstellung haben sich nicht verändert, aber jetzt stehen das robuste <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> und das [CLI](reference-azure-machine-learning-cli.md) zur Verfügung, um Ihre Aufgaben im maschinellen Lernen zu erreichen und Ihre Pipelines zu füllen.  

In diesem Artikel erfahren Sie, was sich geändert hat und welche Auswirkungen das auf Ihre mit Azure Machine Learning Workbench und den APIs erledigte Arbeit hat.

## <a name="what-changed"></a>Was hat sich geändert?

Die neueste Version des Azure Machine Learning-Diensts umfasst Folgendes:
+ Ein [vereinfachtes Azure-Ressourcenmodell](concept-azure-machine-learning-architecture.md)
+ Eine [neue Benutzeroberfläche im Portal](how-to-track-experiments.md) zum Verwalten Ihrer Experimente und Computeziele
+ Ein neues, umfangreicheres Python-<a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>
+ Eine neue, erweiterte [Azure CLI-Erweiterung](reference-azure-machine-learning-cli.md) für Machine Learning

Die [Architektur](concept-azure-machine-learning-architecture.md) wurde im Hinblick auf leichte Bedienbarkeit umgestaltet. Anstelle von mehreren Azure-Ressourcen und -Konten benötigen Sie nur einen [Arbeitsbereich für den Azure Machine Learning-Dienst](concept-azure-machine-learning-architecture.md#workspace).  Arbeitsbereiche können schnell im [Azure-Portal](quickstart-get-started.md) erstellt werden.  Ein Arbeitsbereich kann von vielen Benutzern verwendet werden, um Computeziele für Training und Bereitstellung, Modellexperimente, Docker-Images, bereitgestellte Modelle usw. zu speichern.

Zwar gibt es in der aktuellen Version neue, verbesserte CLI- und SDK-Clients, die eigentliche Workbench-Desktopanwendung wurde aber als veraltet verworfen. Jetzt können Sie Ihre Experimente auf dem [Arbeitsbereich-Dashboard im Azure-Webportal](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal) überwachen. Verwenden Sie das Dashboard, um Ihren Experimentverlauf abzurufen, die an Ihren Arbeitsbereich angefügten Computeziele zu verwalten, Ihre Modelle und Docker-Images zu verwalten und sogar Webdienste bereitzustellen.

## <a name="how-do-i-migrate"></a>Wie führe ich eine Migration aus?

Die meisten der in der früheren Version des Azure Machine Learning Services erstellten Artefakte sind in Ihrem eigenen lokalen Speicher oder in Cloudspeicher gespeichert. Diese Artefakte verschwinden nie. Um zu migrieren, müssen Sie die Artefakte wieder beim aktualisierten Azure Machine Learning Service registrieren. In diesem [Artikel zur Migration](how-to-migrate.md) erfahren Sie, was Sie auf welche Weise migrieren können.

<a name="timeline"></a>

## <a name="support-timeline"></a>Unterstützungszeitachse

Ihre Experimentier- und Modellverwaltungskonten sowie die Workbench-Anwendung können Sie auch nach September 2018 noch eine Weile weiter verwenden. Die Unterstützung für die folgenden Ressourcen wird in den 3 bis 4 Monaten nach der Veröffentlichung dieser Version schrittweise eingestellt. Informationen zur Dokumentation der alten Features finden Sie weiterhin im unteren Bereich des Inhaltsverzeichnisses im Abschnitt [Ressourcen](../desktop-workbench/tutorial-classifying-iris-part-1.md).

|Ausmusterungsphase&nbsp;|Unterstützungsdetails für frühere Features|
|:---:|----------------|
|4. Dezember 2018|Die Möglichkeit zur Erstellung eines _Kontos für Azure Machine Learning-Experimentieren_ und eines _Modellverwaltungskontos_ im Azure-Portal und über die CLI. Die Möglichkeit zur Erstellung von ML Compute-Umgebungen über die CLI wurde ebenfalls eingestellt. Wenn Sie ein Konto besitzen, funktionieren die CLI und die Desktop-Workbench weiterhin in dieser Phase.|
|9. Januar 2019|Die Unterstützung für alle anderen Komponenten, einschließlich der restlichen APIs und der Desktop-Workbench, wird an diesem Datum eingestellt.|

[Beginnen Sie noch heute mit der Migration](how-to-migrate.md). Alle aktuellen Funktionen sind im neuen <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>, dem [CLI](reference-azure-machine-learning-cli.md) und im [Portal](quickstart-get-started.md) verfügbar.

## <a name="what-about-run-histories"></a>Wie sieht es mit Ausführungsverläufen aus?

Ausführungsverläufe bleiben noch eine Zeit lang zugänglich. Wenn Sie bereit sind, auf die aktualisierte Version des Azure Machine Learning Services umzusteigen, können Sie diese Ausführungsverläufe exportieren, wenn Sie eine Kopie aufbewahren möchten.

Ausführungsverläufe heißen in der aktuellen Version jetzt _Experimente_. Sie können die Experimente Ihres Modells sammeln und sie mithilfe des SDKs, des CLIs oder des Webportals untersuchen.

Das Arbeitsbereich-Dashboard im Portal wird nur in den Browsern Edge, Chrome und Firefox unterstützt.

[ ![Onlineportal](./media/overview-what-happened-to-workbench/image001.png) ] (./media/overview-what-happened-to-workbench/image001.png#lightbox)


## <a name="can-i-still-prep-data"></a>Kann ich weiterhin Daten vorbereiten?

Die vorhandenen Datenvorbereitungsdateien können nicht in die neueste Version portiert werden, da wir nicht mehr über die Workbench verfügen. Allerdings können Sie Ihre Daten immer noch für die Modellierung vorbereiten.  

Für kleinere Datasets können Sie das <a href="https://aka.ms/aml-sdk" target="_blank">Azure Machine Learning Data Prep SDK</a> verwenden, um Ihre Daten vor der Modellierung schnell vorzubereiten. 

Sie können das gleiche <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> für größere Datasets oder Azure Databricks verwenden, um große Datasets vorzubereiten. 

## <a name="will-projects-persist"></a>Bleiben Projekte dauerhaft erhalten?

Sie verlieren weder Code noch Arbeit. In der älteren Version stellen Projekte Cloudentitäten mit einem lokalen Verzeichnis dar. In der aktuellen Version fügen Sie lokale Verzeichnisse mithilfe einer lokalen Konfigurationsdatei an den Arbeitsbereich des Azure Machine Learning-Diensts an. [Diagramm der aktuellen Architektur](concept-azure-machine-learning-architecture.md).

Da sich ein großer Teil des Projektinhalts bereits auf Ihrem lokalen Computer befindet, brauchen Sie lediglich eine Konfigurationsdatei im betreffenden Verzeichnis zu erstellen und in Ihrem Code auf sie zu verweisen, um ihn mit Ihrem Arbeitsbereich zu verbinden. [Erfahren Sie, wie Sie vorhandene Projekte migrieren können.](how-to-migrate.md#projects)

Informationen zum Einstieg [in Python mit dem Haupt-SDK](quickstart-get-started.md).

## <a name="what-about-my-registers-models-and-images"></a>Was geschieht mit meinen registrierten Modellen und Images?
 
Die Modelle, die Sie in Ihrer alten Modellregistrierung registriert haben, müssen zu Ihrem neuen Arbeitsbereich migriert werden, wenn Sie sie weiterhin verwenden möchten. Sie können dies tun, indem Sie [die Modelle herunterladen und sie in Ihrem neuen Arbeitsbereich erneut registrieren](how-to-migrate.md). 

Die Images, die Sie in Ihrer alten Imageregistrierung erstellt haben, müssen im neuen Arbeitsbereich neu erstellt werden, um weiterhin verwendet werden zu können. Die entsprechende Vorgehensweise wird in den Abschnitten zum [Konfigurieren und Erstellen eines Images](how-to-deploy-and-where.md#configureimage) beschrieben. 

## <a name="what-about-deployed-web-services"></a>Was ist mit bereitgestellten Webdiensten?

Die Modelle, die Sie als Webdienste mithilfe Ihres Kontos zur Modellverwaltung bereitgestellt haben, funktionieren weiterhin, so lange der Azure Container Service (ACS) unterstützt wird. Für Modellverwaltungskonten werden diese Webdienste sogar nach dem Ende der Unterstützung weiterhin funktionieren. Wenn allerdings die Unterstützung für das alte CLI endet, verlieren Sie damit zugleich die Fähigkeit, diese Webdienste zu verwalten.

In der neueren Version werden Modelle als Webdienste in Azure Container Instances (ACI) oder Azure Kubernetes Service-Clustern (AKS) bereitgestellt. Darüber hinaus sind Bereitstellungen in FPGAs und am IoT-Edge möglich. Weitere Informationen finden Sie im Dokument [Bereitstellen von Modellen mit dem Azure Machine Learning Service](how-to-deploy-and-where.md). Ohne Änderung Ihrer Bewertungsdateien, Abhängigkeiten und Schemas können Sie Ihre Modelle mit dem neuen SDK oder CLI erneut bereitstellen. 

## <a name="what-about-the-old-sdk--cli"></a>Was ist mit dem alten SDK und CLI geschehen?

Sie werden noch bis Januar funktionieren (Informationen dazu finden Sie auf der [Zeitachse](#timeline) oben). Wir empfehlen, neue Experimente und Modelle mit dem neuesten SDK und/oder CLI zu erstellen.

In der neuesten Version ermöglicht Ihnen das neue Python-SDK die Interaktion mit dem Azure Machine Learning Service in beliebigen Python-Umgebungen. Informationen zur Installation des aktuellen <a href="https://aka.ms/aml-sdk" target="_blank">SDKs</a>.  Sie können die [aktualisierte Azure CLI-Erweiterung für maschinelles Lernen](reference-azure-machine-learning-cli.md) außerdem mit dem umfassenden Satz an `az ml`-Befehlen verwenden, um mit dem Dienst in einer beliebigen Befehlszeilenumgebung zu interagieren, einschließlich der Cloud Shell im Azure-Portal.

## <a name="what-about-azure-machine-learning-for-visual-studio-code"></a>Was geschieht mit Azure Machine Learning für Visual Studio Code?

In dieser aktuellen Version wurde Azure Machine Learning für Visual Studio (VS) Code erweitert und verbessert, um die oben genannten neuen Features zu unterstützen.

[ ![Azure Machine Learning für Visual Studio Code](./media/overview-what-happened-to-workbench/vscode.png) ] (./media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>Was ist über Domänenpakete zu sagen?

Die Domänenpakete für [Maschinelles Sehen, Textanalyse und Vorhersagen](../desktop-workbench/reference-python-package-overview.md) können mit der aktuellen Version von Azure Machine Learning nicht verwendet werden. Allerdings können Sie mit dem aktuellen Azure Machine Learning Python-<a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> weiterhin Modelle für maschinelles Sehen, Textanalyse und Vorhersagen erstellen und trainieren. Informationen zum Migrieren bereits vorhandener Modelle, die mit den Paketen für maschinelles Sehen, Textanalyse und Vorhersagen erstellt wurden, erhalten Sie von uns unter [AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [die aktuelle Architektur für den Azure Machine Learning Service](concept-azure-machine-learning-architecture.md), und probieren Sie einen der Schnellstarts oder eins der Tutorials aus:

* [Was ist der Azure Machine Learning-Dienst (Vorschauversion)?](overview-what-is-azure-ml.md)
* [Schnellstart: Verwenden von Python für die ersten Schritte mit Azure Machine Learning](quickstart-get-started.md)
* [Tutorial: Trainieren eines Bildklassifizierungsmodells mit dem Azure Machine Learning-Dienst](tutorial-train-models-with-aml.md)
