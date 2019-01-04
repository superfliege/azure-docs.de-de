---
title: Verwenden von Azure Machine Learning Services in Azure Notebooks
description: Eine Übersicht der Beispielnotebooks für Azure Machine Learning Services, die Sie mit Azure Notebooks verwenden können.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 0dc4fc31-ae1c-422c-ac34-7b025e6651b4
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: b6d3cbd56182b8dba3738f38798efa378c6b49d0
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53254825"
---
# <a name="use-azure-machine-learning-services-in-a-notebook"></a>Verwenden von Azure Machine Learning Services in einem Notebook

Azure Notebooks ist mit der erforderlichen Umgebung für das Arbeiten mit [Azure Machine Learning Services](/azure/machine-learning/service/) vorkonfiguriert. Sie können einfach ein Beispielprojekt in Ihr Notebooks-Konto klonen, um eine Vielzahl von Machine Learning-Szenarien zu untersuchen.

## <a name="clone-the-sample-into-your-account"></a>Klonen des Beispiels in Ihr Konto

1. Melden Sie sich bei [Azure Notebooks](https://notebooks.azure.com/) an.
1. Wählen Sie **Meine Projekte** aus, um zum Projektdashboard zu navigieren.
1. Wählen Sie **Upload GitHub Repo** (GitHub-Repository hochladen, den Pfeil nach oben) aus, um das Popupfenster **Upload GitHub Repository** (GitHub-Repository hochladen) zu öffnen.
1. Geben Sie im Popupfenster `Azure/MachineLearningNotebooks` in **GitHub-Repository** ein, geben Sie in **Project Name** (Projektname) einen Namen für das Projekt an, z. B. „Azure ML Services“, geben Sie einen Bezeichner in **Project ID** (Projekt-ID) ein, deaktivieren Sie ggf. **Public** (Öffentlich), und wählen Sie dann **Import** (Importieren) aus.

    ![Importieren des Azure Machine Learning Notebook-Beispiels in Ihr Notebooks-Konto](media/azureml-import-project.png)

1. Nach einer oder zwei Minuten navigiert Azure Notebooks automatisch zum Dashboard des neuen Projekts.

## <a name="run-a-sample-notebook"></a>Ausführen eines Beispielnotebook

1. Wählen Sie **00 - configuration.ipynb** aus, um den Konfigurationsabschnitt des Notebooks zu starten, und befolgen Sie seine Anweisungen, um einen Azure Machine Learning-Arbeitsbereich zu erstellen.

    - Da Azure Notebooks bereits die erforderlichen Python-Pakete enthält, können Sie einfach den Codeausschnitt in Schritt 2 der Voraussetzungen ausführen, um die Azure ML SDK-Version zu überprüfen.

1. Wählen Sie nach dem Abschluss der Konfiguration **01.getting-started** aus, um in den Ordner zu navigieren, der dreizehn verschiedene Beispielnotebooks enthält, die allesamt selbsterklärend sind.

## <a name="next-steps"></a>Nächste Schritte

Die Dokumentation zu Azure Machine Learning Services enthält eine Vielzahl weiterer Ressourcen, um Sie durch die Arbeit mit Machine Learning Service innerhalb von Notebooks zu führen:

- [Schnellstart: Verwenden von Python für die ersten Schritte mit Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/quickstart-create-workspace-with-python)
- [Tutorial 1: Trainieren eines Bildklassifizierungsmodells mit dem Azure Machine Learning Service](https://docs.microsoft.com/azure/machine-learning/service/tutorial-train-models-with-aml)
- [Tutorial 2: Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances (ACI)](https://docs.microsoft.com/azure/machine-learning/service/tutorial-deploy-models-with-aml)
- [Tutorial: Trainieren eines Klassifizierungsmodells mit automatisiertem maschinellem Lernen im Azure Machine Learning Service](https://docs.microsoft.com/azure/machine-learning/service/tutorial-auto-train-models)

Lesen Sie außerdem die Dokumentation zum [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
