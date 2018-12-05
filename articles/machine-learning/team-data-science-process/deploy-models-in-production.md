---
title: Bereitstellen von Modellen in der Produktion – Azure Machine Learning | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Modelle in der Produktion bereitstellen, damit sie eine aktive Rolle bei geschäftlichen Entscheidungen spielen können.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/30/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 5b1614f92f7633e008f4f7176723002dc7730b15
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495971"
---
# <a name="deploy-models-in-production"></a>Bereitstellen von Modellen in der Produktion

Bei einer Produktionsbereitstellung kann ein Modell eine aktive Rolle in einem Unternehmen spielen. Vorhersagen aus einem bereitgestellten Modell können für Geschäftsentscheidungen verwendet werden.

## <a name="production-platforms"></a>Produktionsplattformen

Es gibt verschiedene Ansätze und Plattformen für das Einführen von Modellen in die Produktion. Hier sind einige Optionen angegeben:

- [Bereitstellen von Modellen im Azure Machine Learning Service](../service/how-to-deploy-and-where.md)
- [Bereitstellung eines Modells in SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>[!NOTE]
>Vor der Bereitstellung muss sichergestellt werden, dass bei der Bewertung durch das Modell ausreichend geringe Latenzen für eine Verwendung in der Produktion auftreten.
>

>[!NOTE]
>Informationen zur Bereitstellung mit Azure Machine Learning Studio finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts](../studio/publish-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>A/B-Tests

Wenn sich mehrere Modelle in der Produktion befinden, kann es nützlich sein, [A/B-Tests](https://en.wikipedia.org/wiki/A/B_testing) zum Vergleichen der Leistung der Modelle durchzuführen. 
 
## <a name="next-steps"></a>Nächste Schritte

Exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für **bestimmte Szenarien** gezeigt werden, sind ebenfalls verfügbar. Sie sind im Artikel [Exemplarische Vorgehensweisen](walkthroughs.md) aufgeführt und mit Miniaturansichtsbeschreibungen verlinkt. Sie zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden. 
