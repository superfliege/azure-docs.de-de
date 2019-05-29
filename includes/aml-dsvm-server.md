---
title: Includedatei
description: Includedatei
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 01/25/2019
ms.openlocfilehash: 02ef0d6c7c8ddc7088938d9c8ea379e3b97f3045
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158723"
---
1. [Erstellen eines Azure Machine Learning Service-Arbeitsbereichs](../articles/machine-learning/service/setup-create-workspace.md).

1. Klonen Sie das [GitHub-Repository](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Fügen Sie mit einer der folgenden Methoden dem geklonten Verzeichnis eine Datei für die Arbeitsbereichskonfiguration hinzu:

    * Wählen Sie im [Azure-Portal](https://ms.portal.azure.com) im Abschnitt **Übersicht** Ihres Arbeitsbereichs **„config.json“ herunterladen** aus. 

    ![Herunterladen von „config.json“](./media/aml-dsvm-server/download-config.png)

    * Erstellen Sie einen neuen Arbeitsbereich, indem Sie den Code im Notebook [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) in Ihrem geklonten Verzeichnis verwenden.


1. Starten Sie den Notebook-Server aus Ihrem geklonten Verzeichnis.
    
    ```shell
    jupyter notebook
    ```