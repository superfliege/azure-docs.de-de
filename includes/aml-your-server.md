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
ms.openlocfilehash: 8d8b314965253dc00b39d0b068b1d6fb3e4aa471
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58395527"
---
1. Erstellen Sie anhand der Anweisungen unter [Create a Azure Machine Learning service workspace](../articles/machine-learning/service/setup-create-workspace.md#portal) (Erstellen eines Azure Machine Learning Service-Arbeitsbereichs) eine Miniconda-Umgebung und einen Arbeitsbereich, und schreiben Sie eine Konfigurationsdatei für den Arbeitsbereich (**aml_config/config.json**).

1. Klonen Sie das [GitHub-Repository](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Fügen Sie mit einer der folgenden Methoden eine Konfigurationsdatei für den Arbeitsbereich hinzu:
    * Kopieren Sie die Datei **aml_config/config.json**, die Sie anhand der Schnellstartanleitung für die Erfüllung der Voraussetzungen erstellt haben, in das geklonte Verzeichnis.
    * Erstellen Sie einen neuen Arbeitsbereich, indem Sie den in [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) enthaltenen Code verwenden.
1. Starten Sie den Notebook-Server aus Ihrem geklonten Verzeichnis.
    
    ```shell
    jupyter notebook
    ```