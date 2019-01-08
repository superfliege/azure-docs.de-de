---
title: Was ist mit der Machine Learning Workbench geschehen?
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, was mit der Machine Learning Workbench-Anwendung geschehen ist, was sich beim Azure Machine Learning-Dienst geändert hat und welcher Zeitplan für die Unterstützung gilt.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: ff6b61874363bbc869bd509174e58640a2487f56
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811306"
---
# <a name="whats-happening-to-machine-learning-workbench-in-azure-machine-learning-service"></a>Was passiert mit der Machine Learning Workbench im Azure Machine Learning-Dienst?

Die Azure Machine Learning Workbench-Anwendung und einige andere frühe Features sind veraltet und wurden im Release vom 2018 ersetzt, um den Weg für eine verbesserte [Architektur](concept-azure-machine-learning-architecture.md) frei zu machen. Die Version enthält viele wichtige Updates, die durch Feedback von Kunden ausgelöst wurden, um Ihr Benutzererlebnis verbessern. Die Kernfunktionen von experimentellen Läufen bis zur Modellimplementierung haben sich nicht verändert,  aber jetzt stehen das robuste <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> und die [Azure CLI](reference-azure-machine-learning-cli.md) zur Verfügung, um Ihre Aufgaben im maschinellen Lernen zu erreichen und Ihre Pipelines zu füllen.  

In diesem Artikel erfahren Sie, was sich geändert hat und welche Auswirkungen das auf Ihre mit Azure Machine Learning Workbench und den APIs erledigte Arbeit hat.

## <a name="what-changed"></a>Was hat sich geändert?

Die neueste Version von Azure Machine Learning Service umfasst folgende Funktionen:
+ Ein [vereinfachtes Azure-Ressourcenmodell](concept-azure-machine-learning-architecture.md).
+ Eine [neue Portal-Benutzeroberfläche](how-to-track-experiments.md) zum Verwalten Ihrer Experimente und Computeziele.
+ Ein neues, umfangreicheres Python-<a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>.
+ Die neue, erweiterte [Azure CLI-Erweiterung](reference-azure-machine-learning-cli.md) für Machine Learning.

Die [Architektur](concept-azure-machine-learning-architecture.md) wurde im Hinblick auf leichte Bedienbarkeit umgestaltet. Anstelle von mehreren Azure-Ressourcen und -Konten benötigen Sie nur einen [Arbeitsbereich für den Azure Machine Learning-Dienst](concept-azure-machine-learning-architecture.md#workspace). Arbeitsbereiche können schnell im [Azure-Portal](quickstart-get-started.md) erstellt werden. Durch die Verwendung eines Arbeitsbereichs können mehrere Benutzer Computeziele für Training und Bereitstellung, Modellexperimente, Docker-Images, bereitgestellte Modelle usw. speichern.

Zwar gibt es in der aktuellen Version neue, verbesserte CLI- und SDK-Clients, die eigentliche Workbench-Desktopanwendung wurde aber als veraltet verworfen. Jetzt können Sie Ihre Experimente auf dem [Arbeitsbereich-Dashboard im Azure-Portal](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal) überwachen. Verwenden Sie das Dashboard, um Ihren Experimentverlauf abzurufen, die an Ihren Arbeitsbereich angefügten Computeziele zu verwalten, Ihre Modelle und Docker-Images zu verwalten und sogar Webdienste bereitzustellen.

## <a name="how-do-i-migrate"></a>Wie führe ich eine Migration aus?

Die meisten der in der früheren Version des Azure Machine Learning Service erstellten Artefakte sind in Ihrem eigenen lokalen Speicher oder in Cloudspeicher gespeichert. Diese Artefakte verschwinden nie. Um zu migrieren, müssen Sie die Artefakte wieder beim aktualisierten Azure Machine Learning Service registrieren. In diesem [Artikel zur Migration](how-to-migrate.md) erfahren Sie, was Sie auf welche Weise migrieren können.

<a name="timeline"></a>

## <a name="support-timeline"></a>Unterstützungszeitachse

Sie können Ihre Experimentier- und Modellverwaltungskonten für Machine Learning sowie die Machine Learning Workbench-Anwendung auch nach September 2018 weiterhin verwenden. Die Unterstützung für die folgenden Ressourcen wird in den drei bis vier Monaten nach der Veröffentlichung dieser Version schrittweise eingestellt. Informationen zur Dokumentation der alten Features finden Sie weiterhin im unteren Bereich des Inhaltsverzeichnisses im Abschnitt [Ressourcen](../desktop-workbench/tutorial-classifying-iris-part-1.md).

|Ausmusterungsphase&nbsp;|Unterstützungsdetails für frühere Features|
|:---:|----------------|
|4. Dezember 2018|Die Möglichkeit zur Erstellung von Konten für Azure Machine Learning-Experimentieren und Modellverwaltung im Azure-Portal und über die CLI. Die Möglichkeit zur Erstellung von Machine Learning Compute-Umgebungen über die CLI wurde ebenfalls eingestellt. Wenn Sie ein Konto besitzen, funktionieren die CLI und die Desktopversion der Machine Learning Workbench weiterhin in dieser Phase.|
|9. Januar 2019|Support für alle anderen Funktionen endet an diesem Datum. Beispiele hierfür sind die verbleibenden APIs und die Desktopversion der Machine Learning Workbench.|

[Beginnen Sie noch heute mit der Migration](how-to-migrate.md). Alle aktuellen Funktionen sind unter Verwendung des neuen <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>, der [CLI](reference-azure-machine-learning-cli.md) und des [Portals](quickstart-get-started.md) verfügbar.

## <a name="what-about-run-histories"></a>Wie sieht es mit Ausführungsverläufen aus?

Ausführungsverläufe bleiben noch eine Zeit lang zugänglich. Wenn Sie bereit sind, auf die aktualisierte Version des Azure Machine Learning Service umzusteigen, können Sie diese Ausführungsverläufe exportieren, wenn Sie eine Kopie aufbewahren möchten.

Ausführungsverläufe heißen in der aktuellen Version **Experimente**. Sie können die Experimente Ihres Modells sammeln und sie mithilfe des SDK, der CLI oder des Azure-Portals untersuchen.

Das Arbeitsbereich-Dashboard im Portal wird nur in den Browsern Microsoft Edge, Chrome und Firefox unterstützt:

[ ![Onlineportal](./media/overview-what-happened-to-workbench/image001.png)] (./media/overview-what-happened-to-workbench/image001.png#lightbox)


## <a name="can-i-still-prep-data"></a>Kann ich weiterhin Daten vorbereiten?

Die vorhandenen Datenvorbereitungsdateien können nicht in die neueste Version portiert werden, da wir nicht mehr über die Machine Learning Workbench verfügen. Allerdings können Sie Ihre Daten immer noch für die Modellierung vorbereiten.  

Für Datasets jeglicher Größe können Sie das [Azure Machine Learning Data Prep SDK](http://aka.ms/data-prep-sdk) verwenden, um Ihre Daten vor der Modellierung schnell vorzubereiten, indem Sie Python-Code schreiben. 

Weitere Informationen zur Verwendung des Azure Machine Learning Data Prep SDK finden Sie in [diesem Tutorial](tutorial-data-prep.md).

## <a name="will-projects-persist"></a>Bleiben Projekte dauerhaft erhalten?

Sie verlieren weder Code noch Arbeit. In der älteren Version stellen Projekte Cloudentitäten mit einem lokalen Verzeichnis dar. In der aktuellen Version fügen Sie lokale Verzeichnisse mithilfe einer lokalen Konfigurationsdatei an den Arbeitsbereich des Azure Machine Learning Service an. Hier finden Sie ein [Diagramm der aktuellen Architektur](concept-azure-machine-learning-architecture.md).

Ein großer Teil des Projektinhalts befindet sich bereits auf Ihrem lokalen Computer,  weshalb Sie lediglich eine Konfigurationsdatei im betreffenden Verzeichnis erstellen und in Ihrem Code auf sie verweisen müssen, um ihn mit Ihrem Arbeitsbereich zu verbinden. [Erfahren Sie, wie Sie vorhandene Projekte migrieren können](how-to-migrate.md#projects).

Informationen zum Einstieg in [Python mit dem Haupt-SDK](quickstart-create-workspace-with-python.md) oder zur Verwendung des [Azure-Portals](quickstart-get-started.md).

## <a name="what-about-my-registered-models-and-images"></a>Was geschieht mit meinen registrierten Modellen und Images?
 
Die Modelle, die Sie in Ihrer alten Modellregistrierung registriert haben, müssen zu Ihrem neuen Arbeitsbereich migriert werden, wenn Sie sie weiterhin verwenden möchten. Um Ihre Modell zu migrieren, [laden Sie die Modelle herunter, und registrieren Sie sie erneut](how-to-migrate.md) in Ihrem neuen Arbeitsbereich. 

Die Images, die Sie in Ihrer alten Imageregistrierung erstellt haben, müssen im neuen Arbeitsbereich neu erstellt werden, um weiterhin verwendet werden zu können. Sie können diese Images neu erstellen, indem Sie Vorgehensweisen in den Abschnitten zum [Konfigurieren und Erstellen eines Images](how-to-deploy-and-where.md#configureimage) befolgen. 

## <a name="what-about-deployed-web-services"></a>Was ist mit bereitgestellten Webdiensten?

Die Modelle, die Sie als Webdienste mithilfe Ihres Kontos für die Machine Learning-Modellverwaltung bereitgestellt haben, funktionieren weiterhin, so lange der Azure Container Service unterstützt wird. Für Machine Learning-Modellverwaltungskonten werden diese Webdienste sogar nach dem Ende der Unterstützung weiterhin funktionieren. Wenn allerdings die Unterstützung für das alte CLI endet, verlieren Sie damit zugleich die Fähigkeit, diese Webdienste zu verwalten.

In der neueren Version werden Modelle als Webdienste in Azure Container Instances oder Azure Kubernetes Service-Clustern (AKS) bereitgestellt. Darüber hinaus sind Bereitstellungen in FPGAs und an Azure IoT Edge möglich. Weitere Informationen finden Sie im Artikel [Bereitstellen: wie und wo](how-to-deploy-and-where.md). Ohne Änderung Ihrer Bewertungsdateien, Abhängigkeiten und Schemas können Sie Ihre Modelle mit dem neuen SDK oder CLI erneut bereitstellen. 

## <a name="what-about-the-old-sdk-and-cli"></a>Was ist mit dem alten SDK und der CLI geschehen?

Ja, sie werden noch bis Januar funktionieren. Siehe den vorigen Abschnitt [Zeitplan](#timeline). Wir empfehlen, neue Experimente und Modelle mit dem neuesten SDK oder der neuesten CLI zu erstellen.

Wenn Sie das neue Python SDK in der neuesten Version verwenden, können Sie mit dem Azure Machine Learning Service in beliebigen Python-Umgebungen interagieren. Informationen zur Installation des aktuellen <a href="https://aka.ms/aml-sdk" target="_blank">SDKs</a>. Sie können auch die aktualisierte [Azure Machine Learning CLI-Erweiterung](reference-azure-machine-learning-cli.md) mit dem umfassenden Satz an `az ml`-Befehlen verwenden, um mit dem Dienst in einer beliebigen Befehlszeilenumgebung zu interagieren, einschließlich der Azure Cloud Shell.

## <a name="what-about-azure-machine-learning-for-visual-studio-code"></a>Was geschieht mit Azure Machine Learning für Visual Studio Code?

In dieser aktuellen Version wurde Azure Machine Learning für Visual Studio Code erweitert und verbessert, um die zuvor genannten neuen Features zu unterstützen.

[ ![Azure Machine Learning für Visual Studio Code](./media/overview-what-happened-to-workbench/vscode.png)] (./media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>Was ist über Domänenpakete zu sagen?

Die Domänenpakete für [Maschinelles Sehen, Textanalyse und Vorhersagen](../desktop-workbench/reference-python-package-overview.md) können mit der aktuellen Version von Azure Machine Learning nicht verwendet werden. Allerdings können Sie mit dem aktuellen Azure Machine Learning Python-<a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> weiterhin Modelle für maschinelles Sehen, Textanalyse und Vorhersagen erstellen und trainieren. Informationen zum Migrieren bereits vorhandener Modelle, die mit den Paketen für maschinelles Sehen, Textanalyse und Vorhersagen erstellt wurden, erhalten Sie unter [AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte

Informationen zur [aktuellen Architektur für Azure Machine Learning Service](concept-azure-machine-learning-architecture.md). Probieren Sie einen der Schnellstarts oder eins der Tutorials aus:

* [Was ist der Azure Machine Learning Service?](overview-what-is-azure-ml.md)
* [Schnellstart: Verwenden von Python für die ersten Schritte mit Azure Machine Learning](quickstart-get-started.md)
* [Tutorial: Trainieren eines Bildklassifizierungsmodells mit dem Azure Machine Learning-Dienst](tutorial-train-models-with-aml.md)
