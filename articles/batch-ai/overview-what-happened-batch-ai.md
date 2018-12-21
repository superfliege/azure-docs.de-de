---
title: Was passiert mit Azure Batch AI? | Microsoft-Dokumentation
description: Erfahren Sie, was mit Azure Batch AI und der Azure Machine Learning Service-Computeoption passiert.
services: batch-ai
author: garyericson
ms.service: batch-ai
ms.topic: overview
ms.date: 12/14/2018
ms.author: garye
ms.openlocfilehash: 6803a47ae77c072eff05df65e37156c90aabf3e6
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436873"
---
# <a name="whats-happening-to-azure-batch-ai"></a>Was passiert mit Azure Batch AI?

**Der Azure Batch AI-Dienst wird im März eingestellt.** Die skalierbaren Trainings- und Bewertungsfunktionen von Batch AI sind jetzt im [Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md) verfügbar, der am 4. Dezember 2018 in die allgemeine Verfügbarkeit überging.

Der Azure Machine Learning Service umfasst neben vielen weiteren Funktionen für das maschinelle Lernen ein cloudbasiertes, verwaltetes Computeziel zum Trainieren, Bereitstellen und Bewerten von Machine Learning-Modellen. Dieses Computeziel heißt [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Beginnen Sie noch heute mit der Migration und Nutzung](#migrate). Sie können über die entsprechenden [Python SDKs](../machine-learning/service/quickstart-create-workspace-with-python.md), die Befehlszeilenschnittstelle und das [Azure-Portal](../machine-learning/service/quickstart-get-started.md) mit dem Azure Machine Learning Service interagieren.

## <a name="support-timeline"></a>Unterstützungszeitachse

| Datum | Informationen zur Unterstützung des Batch AI-Diensts |
| ---- |-----------------|
| 14.&nbsp;Dezember&nbsp;2018| Verwenden Sie Ihre vorhandenen Azure Batch AI-Abonnements wie üblich weiter. Das Registrieren **neuer Abonnements** ist allerdings nicht mehr möglich, und für diesen Dienst werden keine Investitionen mehr getätigt.|
| 31.&nbsp;März&nbsp;2019 | Nach diesem Datum funktionieren Batch AI-Abonnements nicht mehr. |

<a name="migrate"></a>
## <a name="how-do-i-migrate"></a>Wie führe ich eine Migration aus?

Um Unterbrechungen Ihrer Anwendungen zu vermeiden und von den neuesten Features zu profitieren, führen Sie vor dem 31. März 2019 folgende Schritte aus:

1. Erstellen Sie einen Azure Machine Learning Service-Arbeitsbereich, um loszulegen:
    + [Schnellstartanleitung für Python](../machine-learning/service/quickstart-create-workspace-with-python.md)
    + [Schnellstartanleitung für das Azure-Portal](../machine-learning/service/quickstart-get-started.md)

1. Richten Sie eine Instanz von [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute) für das Trainieren von Modellen ein.

1. Aktualisieren Sie Ihre Skripts, sodass diese Azure Machine Learning Compute verwenden.

## <a name="support"></a>Support

Für Bestandskunden, die zum umfassenderen [Azure Machine Learning Service](https://aka.ms/aml-docs) migrieren möchten, steht Support zur Verfügung.

Wenn der Azure Machine Learning Service Ihre Anforderungen nicht erfüllt, aber eine unterstützte Funktion im Batch AI-Dienst vorhanden ist, starten Sie eine Batch AI-Supportanfrage beim Supportteam, um Ihr Abonnement auf die Whitelist für die Verwendung von Batch AI setzen zu lassen, bis der Dienst eingestellt wird.

## <a name="next-steps"></a>Nächste Schritte

+ Lesen Sie den [Überblick über den Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Konfigurieren Sie Computeziele zum Trainieren von Modellen](../machine-learning/service/how-to-set-up-training-targets.md) mit dem Azure Machine Learning Service.

+ Lesen Sie die [Azure-Roadmap](https://azure.microsoft.com/updates/), um mehr über Updates anderer Azure-Dienste zu erfahren.
