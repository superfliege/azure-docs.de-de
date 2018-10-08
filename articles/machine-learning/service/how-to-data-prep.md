---
title: Vorbereiten von Daten mit dem Machine Learning Data Prep SDK für Python – Azure
description: Erfahren Sie, wie Sie mit dem Azure Machine Learning Data Prep SDK für Python Daten in verschiedenen Formaten laden, zur besseren Nutzbarkeit umwandeln und diese Daten in einen Speicherort schreiben, damit diese durch Ihre Modelle aufgerufen werden können.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 673b8ddf3ed07507cbce4b54e9b8a488cb3ac689
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166604"
---
# <a name="prepare-data-for-modeling-with-azure-machine-learning"></a>Vorbereiten von Daten zum Modellieren mit Azure Machine Learning
 
Die Vorbereitung der Daten ist ein wichtiger Bestandteil eines Machine Learning-Workflows. Ihre Modelle sind präziser und effizienter, wenn diese über Zugriff auf bereinigte Daten in einem Format verfügen, das einfacher zu verwenden ist. 

Sie können Ihre Daten in Python mit dem [Azure Machine Learning Data Prep SDK](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py) vorbereiten. 

## <a name="data-preparation-pipeline"></a>Datenaufbereitungspipeline

Die wichtigsten Schritte der Datenaufbereitung sind:

1. [Laden von Daten](how-to-load-data.md), ggf. in verschiedenen Formaten
2. [Transformieren](how-to-transform-data.md) in eine besser nutzbare Struktur
3. [Schreiben der Daten](how-to-write-data.md) in einen Speicherort, auf den Ihre Modelle zugreifen können

![Datenvorbereitungsprozess](./media/concept-data-preparation/data-prep-process.png)

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich ein [Beispielnotebook](https://github.com/Microsoft/PendletonDocs/blob/master/Scenarios/GettingStarted/getting-started.ipynb) zur Datenvorbereitung mit dem Azure Machine Learning Data Prep SDK an.
