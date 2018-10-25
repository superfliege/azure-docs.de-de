---
title: Verwalten und Bereitstellen von Modellen in Azure Machine Learning Services
description: Erfahren Sie, wie Sie mit Azure Machine Learning Services Ihre Modelle bereitstellen, verwalten und überwachen, um sie kontinuierlich zu verbessern. Sie können die Modelle, die Sie mit Azure Machine Learning Services trainiert haben, auf Ihrem lokalen Computer oder aus anderen Quellen bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: hjerez
ms.author: hjerez
ms.date: 09/24/2018
ms.openlocfilehash: b09ef259d73744612c41adc4fc40ea0235da9bcb
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885066"
---
# <a name="manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>Verwalten, Bereitstellen und Überwachen von Modellen mit Azure Machine Learning Services

In diesem Artikel lernen Sie, wie Sie mit Azure Machine Learning Services Ihre Modelle bereitstellen, verwalten und überwachen, um sie kontinuierlich zu verbessern. Sie können die Modelle, die Sie mit Azure Machine Learning trainiert haben, auf Ihrem lokalen Computer oder aus anderen Quellen bereitstellen. 

Die folgende Abbildung veranschaulicht den gesamten Bereitstellungsworkflow: [ ![Bereitstellungsworkflow für Azure Machine Learning](media/concept-model-management-and-deployment/deployment-pipeline.png) ](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

Der Bereitstellungsworkflow umfasst die folgenden Schritte:
1. **Registrieren des Modells** in einer Registrierung, die in Ihrem Azure Machine Learning Services-Arbeitsbereich gehostet wird
1. **Registrieren eines Images**, das ein Modell mit einem Bewertungsskript und Abhängigkeiten in einem portablen Container verbindet 
1. **Bereitstellen des Images** als Webdienst in der Cloud oder auf Edgegeräten
1. **Überwachen und Sammeln von Daten**

Jeder Schritt kann alleine oder als Teil eines einzelnen Bereitstellungsbefehls ausgeführt werden. Darüber hinaus können Sie die Bereitstellung in einen **CI/CD-Workflow** integrieren, wie in dieser Grafik dargestellt.

[ ![ Zyklus: Continuous Integration/Continuous Deployment (CI/CD) für Azure Machine Learning](media/concept-model-management-and-deployment/model-ci-cd.png) ](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)


## <a name="step-1-register-model"></a>Schritt 1: Registrieren des Modells

Die Modellregistrierung verfolgt alle Modelle in Ihrem Azure Machine Learning Services-Arbeitsbereich nach.
Die Modelle werden anhand des Namens und der Version identifiziert. Jedes Mal, wenn Sie ein Modell mit dem gleichen Namen wie ein bereits vorhandenes Modell registrieren, erhöht die Registrierung die Versionsnummer. Sie können beim Registrieren auch zusätzliche Metadatentags bereitstellen, die bei der Suche nach Modellen verwendet werden können.

Sie können keine Modelle löschen, die von einem Image verwendet werden.

## <a name="step-2-register-image"></a>Schritt 2: Registrieren eines Images

Images ermöglichen eine zuverlässige Modellimplementierung, die alle Komponenten umfasst, die zum Verwenden des Modells erforderlich sind. Ein Image enthält die folgenden Elemente:

* Das Modell
* Die Bewertungs-Engine
* Die Bewertungsdatei oder -anwendung
* Alle Abhängigkeiten, die zum Bewerten des Modells erforderlich sind

Das Image kann auch SDK-Komponenten zum Protokollieren und Überwachen beinhalten. Die SDK-Protokolldaten können für die Feinabstimmung oder das erneute Training Ihres Modells verwendet werden, einschließlich zur Ein- und Ausgabe des Modells.

Azure Machine Learning unterstützt die gängigsten Frameworks – generell wird jedes Framework unterstützt, das mit „pip“ installiert werden kann.

Wenn Ihr Arbeitsbereich erstellt wurde, wurden auch andere Azure-Ressourcen erstellt, die von diesem Arbeitsbereich verwendet werden.
Alle Objekte, die zum Erstellen des Images verwendet werden, werden im Azure-Speicherkonto in Ihrem Arbeitsbereich gespeichert. Das Image wird erstellt und in Azure Container Registry gespeichert. Sie können beim Erstellen des Images zusätzliche Metadatentags bereitstellen, die ebenfalls in der Bildregistrierung gespeichert werden. Diese können später abgefragt werden, um das Image wiederzufinden.

## <a name="step-3-deploy-image"></a>Schritt 3: Bereitstellen des Images

Sie können registrierte Images in der Cloud und auf Edgegeräten bereitstellen. Der Bereitstellungsprozess erstellt alle Ressourcen, mit denen Sie das Modell überwachen und automatisch skalieren sowie dafür einen Lastenausgleich vornehmen können. Der Zugriff auf die bereitgestellten Dienste kann durch eine zertifikatsbasierte Authentifizierung gesichert werden, indem die Sicherheitsressourcen beim Bereitstellen verfügbar gemacht werden. Sie können auch eine vorhandene Bereitstellung aktualisieren, um ein neueres Image zu verwenden.

Webdienstbereitstellungen sind auch durchsuchbar. So können Sie beispielsweise nach allen Bereitstellungen eines bestimmten Modells oder Images suchen.

[ ![Rückschlussziele](media/concept-model-management-and-deployment/inferencing-targets.png) ](media/concept-model-management-and-deployment/inferencing-targets.png#lightbox)

Für Ihre Images sind die folgenden [Bereitstellungsziele](how-to-deploy-and-where.md) in der Cloud verfügbar:

* Azure Container Instances
* Azure Kubernetes Service
* Azure-FPGA-Computer
* Azure IoT Edge-Geräte

Beim Bereitstellen Ihres Dienstes wird für die Rückschlussanforderung automatisch ein Lastenausgleich vorgenommen und der Cluster skaliert, um bei Bedarf Spitzen zu verarbeiten. [Telemetriedaten zu Ihrem Dienst](how-to-enable-app-insights.md) können im Azure Application Insights-Dienst erfasst werden, der mit Ihrem Arbeitsbereich verknüpft ist.

## <a name="step-4-monitor-models-and-collect-data"></a>Schritt 4: Überwachen des Modells und Sammeln von Daten

Ein SDK für die Modellprotokollierung und Datenerfassung ist verfügbar, sodass Sie Eingabe-, Ausgabe- und andere relevante Daten Ihres Modells überwachen können. Die Daten werden als Blob im Azure-Speicherkonto für Ihren Arbeitsbereich gespeichert.

Um das SDK mit Ihrem Modell zu verwenden, importieren Sie das SDK in Ihr Bewertungsskript oder Ihre Anwendung. Dann können Sie mit dem SDK Daten protokollieren, z.B. Parameter, Ergebnisse und Eingabedetails.

Wenn Sie das[Sammeln von Modelldaten aktivieren](how-to-enable-data-collection.md), werden bei jeder Bereitstellung des Images die zum Erfassen der Daten erforderlichen Details automatisch bereitgestellt, z.B. Anmeldeinformationen für Ihren persönlichen Blobspeicher.

> [!Important]
> Microsoft sieht Ihre gesammelten Modelldaten nicht. Die Daten werden direkt an das Azure-Speicherkonto für Ihren Arbeitsbereich gesendet.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr darüber [wie und auf welchen Instanzen Modelle mit dem Azure Machine Learning-Dienst bereitgestellt werden können](how-to-deploy-and-where.md).
