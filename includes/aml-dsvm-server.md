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
ms.openlocfilehash: 12d1576c3bfbf96c0445fcd2a6f0bc37d6a68f11
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302178"
---
1. Führen Sie die Schritte unter [Schnellstart: Verwenden von Python für die ersten Schritte mit Azure Machine Learning](../articles/machine-learning/service/quickstart-create-workspace-with-python.md) durch, um einen Arbeitsbereich zu erstellen.  Wenn Sie möchten, können Sie den Abschnitt zum **Verwenden des Notebooks** überspringen.
1. Klonen Sie das [GitHub-Repository](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```
1. Fügen Sie mit einer der folgenden Methoden eine Konfigurationsdatei für den Arbeitsbereich hinzu:
    * Kopieren Sie die Datei **aml_config\config.json**, die Sie anhand der Schnellstartanleitung für die Erfüllung der Voraussetzungen erstellt haben, in das geklonte Verzeichnis.
    * Erstellen Sie einen neuen Arbeitsbereich, indem Sie den Code im Notebook [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) in Ihrem geklonten Verzeichnis verwenden.
1. Starten Sie den Notebook-Server aus Ihrem geklonten Verzeichnis.
    
    ```shell
    jupyter notebook
    ```