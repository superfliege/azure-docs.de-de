---
title: Was ist der Azure Machine Learning-Dienst?
description: 'Es werden grundlegende Machine Learning-Konzepte für die Cloud und die Nutzungsmöglichkeiten beschrieben, und Sie erhalten Definitionen von Machine Learning-Begriffen. Übersicht über Azure Machine Learning: Hierbei handelt es sich um eine integrierte End-to-End-Lösung, mit der professionelle Data Scientists erweiterte Analyseanwendungen für die Cloud entwickeln und bereitstellen und Experimente für diese durchführen.'
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 09/24/2018
ms.openlocfilehash: 1dac11b8ad71a936b33742b52c95ac998176baf7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244939"
---
# <a name="what-is-azure-machine-learning-service-preview"></a>Was ist der Azure Machine Learning-Dienst (Vorschauversion)?

Der Azure Machine Learning-Dienst (Vorschauversion) ist ein Clouddienst, den Sie zum Entwickeln und Bereitstellen von Machine Learning-Modellen verwenden können. Mithilfe des Azure Machine Learning-Diensts können Sie Ihre Modelle während des Erstell-, Trainings- und Bereitstellungsprozesses überwachen und sämtliche Funktionen der Cloud zur Verwaltung nutzen.

## <a name="what-is-machine-learning"></a>Was ist Machine Learning?

Machine Learning ist ein Data Science-Verfahren, mit dem Computer aus vorhandenen Daten lernen können, um zukünftiges Verhalten, Ergebnisse und Trends vorherzusagen. Mithilfe von Machine Learning lernen Computer, ohne explizit programmiert worden zu sein.

Dank solcher Vorhersagen oder Prognosen aus Machine Learning können Apps und Geräte „intelligenter“ werden. Wenn Sie beispielsweise online einkaufen, trägt maschinelles Lernen dazu bei, dass Ihnen anhand der gekauften Produkte weitere Produkte empfohlen werden, die Ihnen gefallen könnten. Ein weiteres Beispiel ist die Nutzung Ihrer Kreditkarte. Dabei wird mithilfe von maschinellem Lernen die Transaktion mit einer Transaktionsdatenbank verglichen, wodurch Betrugsfälle erkannt werden können. Auch wenn ein automatischer Staubsauger ein Zimmer saugt, wird mit maschinellem Lernen entschieden, ob die Arbeit erledigt ist.

## <a name="what-is-azure-machine-learning-service"></a>Was ist der Azure Machine Learning-Dienst?

Der Azure Machine Learning-Dienst umfasst eine cloudbasierte Umgebung, auf die Sie zum Entwickeln, Trainieren, Testen, Bereitstellen, Verwalten und Nachverfolgen von Machine Learning-Modellen zurückgreifen können.

[ ![Workflow des Azure Machine Learning-Diensts](./media/overview-what-is-azure-ml/aml.png) ] (./media/overview-what-is-azure-ml/aml.png#lightbox)

Der Azure Machine Learning-Dienst unterstützt ohne Einschränkungen alle Open Source-Technologien. Sie können dadurch Zehntausende von verschiedenen Open Source-Paketen für Python in Kombination mit Komponenten für maschinelles Lernen wie TensorFlow und scikit-learn verwenden.
Umfassende Tools wie [Jupyter Notebook](http://jupyter.org) oder [Visual Studio Code Tools for AI](https://visualstudio.microsoft.com/downloads/ai-tools-vscode/) stellen eine einfache Möglichkeit dar, Daten auf interaktive Weise zu untersuchen und zu transformieren und anschließend Modelle zu entwickeln und zu testen.
Zudem umfasst der Azure Machine Learning-Dienst Features zum [Automatisieren der Modellgenerierung und -optimierung](tutorial-auto-train-models.md), wodurch Sie auf einfache und effiziente Weise genaue Modelle erstellen können.

Mithilfe des Azure Machine Learning-Diensts können Sie auf Ihrem lokalen Computer mit dem Training beginnen und eine horizontale Hochskalierung auf die Cloud durchführen. Durch die native Unterstützung für [Azure Batch AI](https://azure.microsoft.com/services/batch-ai/) und [Dienste für die erweiterte Hyperparameteroptimierung](how-to-tune-hyperparameters.md) können Sie mithilfe der Cloud schneller bessere Modelle erstellen. 

Wenn Sie über das richtige Modell verfügen, können Sie es ganz einfach in einem Container wie Docker bereitstellen. Dies vereinfacht die Bereitstellung von [Azure Container Instances](how-to-deploy-to-aci.md) oder [Azure Kubernetes Service](how-to-deploy-to-aks.md). Sie können den Container aber auch lokal oder in der Cloud in Ihren Bereitstellungen verwenden.
Sie können die bereitgestellten Modelle verwalten und mehrere Ausführungen nachverfolgen, um die bestmögliche Lösung zu finden.

[!INCLUDE [aml-preview-note](../../../includes/aml-preview-note.md)]

## <a name="what-can-i-do-with-azure-machine-learning-service"></a>Wofür kann ich den Azure Machine Learning-Dienst verwenden?

Der Azure Machine Learning-Dienst kann automatisch ein Modell generieren und optimieren.
Ein Beispiel finden Sie unter [Tutorial: Automatically train a classification model with Azure Automated Machine Learning (Tutorial: Automatisches Trainieren eines Klassifizierungsmodells mithilfe von Azure Automated Machine Learning)](tutorial-auto-train-models.md).

Stattdessen können Sie aber auch selbst mit dem Azure Machine Learning <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> für Python und mit Open Source-Paketen für Python sehr genaue Machine Learning- und Deep Learning-Modelle in einem Arbeitsbereich für den Azure Machine Learning-Dienst erstellen und trainieren.
Sie können aus mehreren Komponenten für maschinelles Lernen auswählen, die in Open Source-Paketen für Python wie den folgenden enthalten sind:

- <a href="http://scikit-learn.org/stable/" target="_blank">scikit-learn</a>
- <a href="https://www.tensorflow.org" target="_blank">TensorFlow</a>
- <a href="https://pytorch.org" target="_blank">PyTorch</a>
- <a href="https://www.microsoft.com/en-us/cognitive-toolkit/" target="_blank">CNTK</a>
- <a href="http://mxnet.io" target="_blank">MXNet</a>

Sobald Sie über ein Modell verfügen, können Sie es verwenden, um einen Container (z.B. Docker) zu erstellen, der zuerst auf lokaler Ebene zu Testzwecken und anschließend als Produktionswebdienst in [Azure Container Instances](how-to-deploy-to-aci.md) oder [Azure Kubernetes Service](how-to-deploy-to-aks.md) bereitgestellt werden kann.

Die bereitgestellten Modelle lassen sich dann über das [Azure-Portal](https://portal.azure.com/) oder die [Azure Machine Learning-CLI-Erweiterung](reference-azure-machine-learning-cli.md) verwalten.
Sie können Modellmetriken bewerten, neue Versionen des Modells erneut trainieren und bereitstellen und gleichzeitig die Modellexperimente nachverfolgen.

Nachfolgend finden Sie in den [nächsten Schritten](#next-steps) Informationen zur Einführung in den Azure Machine Learning-Dienst.

## <a name="how-is-azure-machine-learning-service-different-from-studio"></a>Inwiefern unterscheidet sich der Azure Machine Learning-Dienst von Azure Machine Learning Studio?

Azure Machine Learning Studio ist ein visueller Arbeitsbereich mit Drag & Drop-Funktionalität, der sich für die Zusammenarbeit mit anderen Entwicklern und das Erstellen, Testen und Bereitstellen von Lösungen für maschinelles Lernen eignet. Dafür muss kein Code geschrieben werden. Diese Umgebung verwendet vorgefertigte und vorkonfigurierte Algorithmen für maschinelles Lernen und Module für die Verarbeitung von Daten.

Greifen Sie auf Azure Machine Learning Studio zurück, wenn Sie auf schnelle und einfache Weise mit Machine Learning-Modellen experimentieren möchten und die integrierten Algorithmen für maschinelles Lernen für Ihre Lösungen ausreichend sind.

Verwenden Sie den Machine Learning-Dienst, wenn Sie in einer Python-Umgebung arbeiten und mehr Kontrolle über Ihre Algorithmen für maschinelles Lernen wünschen oder Open Source-Bibliotheken für maschinelles Lernen verwenden möchten.

> [!NOTE]
> Modelle, die in Azure Machine Learning Studio erstellt werden, können nicht vom Azure Machine Learning-Dienst bereitgestellt und verwaltet werden.

## <a name="free-trial"></a>Kostenlose Testversion
Wenn Sie kein Abonnent sind, können Sie [ein kostenloses Azure-Konto eröffnen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Sie erhalten dann eine Gutschrift, die Sie für Azure-Dienste einlösen können. Wenn das Guthaben aufgebraucht ist, können Sie das Konto behalten und weiterhin [kostenlose Azure-Dienste](https://azure.microsoft.com/free/) nutzen. Ihre Kreditkarte wird nur dann belastet, wenn Sie Ihre Einstellungen explizit ändern und mit der Berechnung von Gebühren einverstanden sind. Alternativ können Sie Ihre [Vorteile für MSDN-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können.

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie wie im Artikel [Use Azure Portal to get started](quickstart-get-started.md) (Verwenden des Azure-Portals für die ersten Schritte) beschrieben einen Arbeitsbereich für den Machine Learning-Dienst.
 
- Befolgen Sie die in dem Tutorial [Train an image classification model with Azure Machine Learning (Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning)](tutorial-train-models-with-aml.md) beschriebenen Schritte, um Modelle mit dem Azure Machine Learning-Dienst zu trainieren und bereitzustellen.

- Weitere Informationen dazu, wie Sie Azure Machine Learning gestatten, Ihr Modell automatisch zu generieren und zu optimieren finden Sie in dem Tutorial [Automatically train a classification model with Azure Automated Machine Learning (Automatisches Trainieren eines Klassifizierungsmodells mithilfe von Azure Automated Machine Learning)](tutorial-auto-train-models.md).

- Erfahren Sie mehr über [Machine Learning-Pipelines](/azure/machine-learning/service/concept-ml-pipelines) zum Erstellen, Optimieren und Verwalten Ihrer Machine Learning-Szenarien.

- Eine ausführlichere Beschreibung mit dem Fokus auf die betreffende Technik finden Sie unter [Azure Machine Learning service architecture and concepts (Architektur und Konzepte des Azure Machine Learning-Diensts)](concept-azure-machine-learning-architecture.md).

- Weitere Informationen zu anderen Microsoft-Produkten für maschinelles Lernen finden Sie unter [Andere Microsoft-Produkte für maschinelles Lernen](./overview-more-machine-learning.md).


<!-- 

An intro to AML or an end-to-end quickstart video could go here.

In this 9-minute video, learn how you can benefit your app. You'll learn about key features and what a typical workflow looks like. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0-3 minutes covers key features and use-cases.
+ 3-4 minutes covers service provisioning. 
+ 4-6 minutes covers Import Data wizard used to create an index using the built-in real estate dataset.

-->
