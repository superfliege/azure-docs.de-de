---
title: Verwalten, registrieren, bereitstellen und überwachen von ML-Modellen
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie mit Azure Machine Learning Services Ihre Modelle bereitstellen, verwalten und überwachen, um sie kontinuierlich zu verbessern. Sie können die Modelle, die Sie mit Azure Machine Learning Services trainiert haben, auf Ihrem lokalen Computer oder aus anderen Quellen bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 1/23/2019
ms.custom: seodec18
ms.openlocfilehash: 2cd2d328d33744854bc525e5ecf1dfa3b6e4bcc8
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59275441"
---
# <a name="manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>Verwalten, Bereitstellen und Überwachen von Modellen mit Azure Machine Learning Services

In diesem Artikel lernen Sie, wie Sie mit Azure Machine Learning Services Ihre Modelle bereitstellen, verwalten und überwachen, um sie kontinuierlich zu verbessern. Sie können die Modelle, die Sie mit Azure Machine Learning trainiert haben, auf Ihrem lokalen Computer oder aus anderen Quellen bereitstellen. 

Das folgende Diagramm veranschaulicht den vollständigen Bereitstellungsworkflow: [ ![Bereitstellungsworkflow für Azure Machine Learning](media/concept-model-management-and-deployment/deployment-pipeline.png)](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

Der Bereitstellungsworkflow umfasst die folgenden Schritte:
1. **Registrieren des Modells** in einer Registrierung, die in Ihrem Azure Machine Learning Services-Arbeitsbereich gehostet wird
1. **Registrieren eines Images**, das ein Modell mit einem Bewertungsskript und Abhängigkeiten in einem portablen Container verbindet 
1. **Bereitstellen des Images** als Webdienst in der Cloud oder auf Edgegeräten
1. **Überwachen und Sammeln von Daten**
1. **Aktualisieren** Sie eine Bereitstellung für die Verwendung eines neuen Image.

Jeder Schritt kann alleine oder als Teil eines einzelnen Bereitstellungsbefehls ausgeführt werden. Darüber hinaus können Sie die Bereitstellung in einen **CI/CD-Workflow** integrieren, wie in dieser Grafik dargestellt.

[!['Zyklus von Continuous Integration und Continuous Deployment für Azure Machine Learning(media/concept-model-management-and-deployment/model-ci-cd.png)](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)

## <a name="step-1-register-model"></a>Schritt 1: Registrieren des Modells

Die Modellregistrierung ermöglicht es Ihnen, Ihre Modelle in der Azure-Cloud in Ihrem Arbeitsbereich zu speichern und zu versionieren. Die Modellregistrierung erleichtert das Organisieren und Verwalten von trainierten Modellen.
 
Registrierte Modelle werden anhand des Namens und der Version identifiziert. Jedes Mal, wenn Sie ein Modell mit dem gleichen Namen wie ein bereits vorhandenes Modell registrieren, erhöht die Registrierung die Versionsnummer. Sie können beim Registrieren auch zusätzliche Metadatentags bereitstellen, die bei der Suche nach Modellen verwendet werden können. Der Azure Machine Learning Service unterstützt Modelle, die mit Python 3 geladen werden können. 

Sie können keine Modelle löschen, die von einem Image verwendet werden.

Weitere Informationen finden Sie im Abschnitt „Registrieren eines Modells“ von [Bereitstellen von Modellen](how-to-deploy-and-where.md#registermodel).

Ein Beispiel für das Registrieren eines im Pickle-Format gespeicherten Modells, finden Sie unter [Tutorial: Trainieren eines Imageklassifizierungsmodells](tutorial-deploy-models-with-aml.md).

Informationen zur Verwendung von ONNX-Modellen finden Sie im Dokument [ONNX und Azure Machine Learning](how-to-build-deploy-onnx.md).

## <a name="step-2-register-image"></a>Schritt 2: Registrieren eines Images

Images ermöglichen eine zuverlässige Modellimplementierung, die alle Komponenten umfasst, die zum Verwenden des Modells erforderlich sind. Ein Image enthält die folgenden Elemente:

* Das Modell
* Die Bewertungs-Engine
* Die Bewertungsdatei oder -anwendung
* Alle Abhängigkeiten, die zum Bewerten des Modells erforderlich sind

Das Image kann auch SDK-Komponenten zum Protokollieren und Überwachen beinhalten. Die SDK-Protokolldaten können für die Feinabstimmung oder das erneute Training Ihres Modells verwendet werden, einschließlich zur Ein- und Ausgabe des Modells.

Azure Machine Learning unterstützt die gängigsten Frameworks – generell wird jedes Framework unterstützt, das mit „pip“ installiert werden kann.

Wenn Ihr Arbeitsbereich erstellt wurde, wurden auch andere Azure-Ressourcen erstellt, die von diesem Arbeitsbereich verwendet werden.
Alle Objekte, die zum Erstellen des Standardimages verwendet werden, werden im Azure-Speicherkonto in Ihrem Arbeitsbereich gespeichert. Beim Erstellen des Images können Sie weitere Metadatentags bereitstellen. Metadatentags werden auch von der Imageregistrierung gespeichert und können abgefragt werden, um nach Ihrem Image zu suchen.

Sie können auch benutzerdefinierte Images verwenden, die in Azure Container Registry hochgeladen und von Azure Machine Learning Service verwendet werden können.

Weitere Informationen finden Sie im Abschnitt „Konfigurieren und Registrieren eines Image“ von [Bereitstellen von Modellen](how-to-deploy-and-where.md#configureimage).

## <a name="step-3-deploy-image"></a>Schritt 3: Bereitstellen des Images

Sie können registrierte Images in der Cloud und auf Edgegeräten bereitstellen. Der Bereitstellungsprozess erstellt alle Ressourcen, mit denen Sie das Modell überwachen und automatisch skalieren sowie dafür einen Lastenausgleich vornehmen können. Der Zugriff auf die bereitgestellten Dienste kann durch eine zertifikatsbasierte Authentifizierung gesichert werden, indem die Sicherheitsressourcen beim Bereitstellen verfügbar gemacht werden. Sie können auch eine vorhandene Bereitstellung aktualisieren, um ein neueres Image zu verwenden.

Webdienstbereitstellungen sind auch durchsuchbar. So können Sie beispielsweise nach allen Bereitstellungen eines bestimmten Modells oder Images suchen.

[![IRückschlussziele(media/concept-model-management-and-deployment/inferencing-targets.png)](media/concept-model-management-and-deployment/inferencing-targets.png#lightbox)

Für Ihre Images sind die folgenden Bereitstellungsziele in der Cloud verfügbar:

* Azure Container Instances
* Azure Kubernetes Service
* Azure-FPGA-Computer
* Azure IoT Edge-Geräte

Beim Bereitstellen Ihres Dienstes wird für die Rückschlussanforderung automatisch ein Lastenausgleich vorgenommen und der Cluster skaliert, um bei Bedarf Spitzen zu verarbeiten. [Telemetriedaten zu Ihrem Dienst](how-to-enable-app-insights.md) können im Azure Application Insights-Dienst erfasst werden, der mit Ihrem Arbeitsbereich verknüpft ist.

Weitere Informationen finden Sie im Abschnitt „Bereitstellen“ von [Bereitstellen von Modellen](how-to-deploy-and-where.md#deploy).

## <a name="step-4-monitor-models-and-collect-data"></a>Schritt 4: Überwachen des Modells und Sammeln von Daten

Ein SDK für die Modellprotokollierung und Datenerfassung ist verfügbar, sodass Sie Eingabe-, Ausgabe- und andere relevante Daten Ihres Modells überwachen können. Die Daten werden als Blob im Azure-Speicherkonto für Ihren Arbeitsbereich gespeichert.

Um das SDK mit Ihrem Modell zu verwenden, importieren Sie das SDK in Ihr Bewertungsskript oder Ihre Anwendung. Dann können Sie mit dem SDK Daten protokollieren, z.B. Parameter, Ergebnisse und Eingabedetails.

Wenn Sie das Sammeln von Modelldaten aktivieren, werden bei jeder Bereitstellung des Images die zum Erfassen der Daten erforderlichen Details automatisch bereitgestellt, z.B. Anmeldeinformationen für Ihren persönlichen Blobspeicher.

> [!Important]
> Microsoft sieht Ihre gesammelten Modelldaten nicht. Die Daten werden direkt an das Azure-Speicherkonto für Ihren Arbeitsbereich gesendet.

Weitere Informationen finden Sie unter [Gewusst wie: Aktivieren der Modelldatensammlung](how-to-enable-data-collection.md).

## <a name="step-5-update-the-deployment"></a>Schritt 5: Aktualisieren der Bereitstellung

Aktualisierungen für das Modell werden nicht automatisch registriert. Ebenso wird durch die Registrierung eines neuen Images nicht automatisch die Bereitstellung aktualisiert, die aus einer früheren Version des Images erstellt wurde. Stattdessen müssen Sie das Modell manuell registrieren, das Image registrieren und dann das Modell aktualisieren. Weitere Informationen finden Sie im Abschnitt „Aktualisieren“ von [Bereitstellen von Modellen](how-to-deploy-and-where.md#update).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr darüber [wie und auf welchen Instanzen Modelle mit dem Azure Machine Learning-Dienst bereitgestellt werden können](how-to-deploy-and-where.md). Ein Bereitstellungsbeispiel finden Sie unter [Tutorial: Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances](tutorial-deploy-models-with-aml.md).

Erfahren Sie, wie Sie Clientanwendungen und -dienste erstellen, die [ein Modell verwenden, das als Webdienst bereitgestellt wird](how-to-consume-web-service.md).
