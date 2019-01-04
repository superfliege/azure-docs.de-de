---
title: Skalieren eines Machine Learning Studio-Webdiensts – Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Parallelität eines Azure Machine Learning Studio-Webdiensts durch Hinzufügen zusätzlicher Endpunkte erhöhen.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18, previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.author: amlstudiodocs
editor: cgronlun
keywords: Azure Machine Learning, Webdienste, Operationalisierung, Skalierung, Endpunkt, Parallelität
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.component: studio
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.openlocfilehash: 736c27f5c8ad6efb2b4e36f44998ce06d7b69248
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53256348"
---
# <a name="scaling-an-azure-machine-learning-studio-web-service-by-adding-additional-endpoints"></a>Skalieren eines Azure Machine Learning Studio-Webdiensts durch Hinzufügen zusätzlicher Endpunkte
> [!NOTE]
> Die in diesem Thema beschriebenen Verfahren beziehen sich auf einen **klassischen** Machine Learning-Webdienst. 
> 
> 

Standardmäßig ist jeder veröffentlichte Webdienst konfiguriert, 20 Anforderungen gleichzeitig zu unterstützen, wobei maximal 200 Anforderungen gleichzeitig unterstützt werden können. Azure Machine Learning optimiert die Einstellung automatisch, um die beste Leistung für Ihren Webdienst zu bieten. Der Portalwert wird ignoriert. 

Wenn Sie die API stärker als mit dem unterstützen Maximalwert von 200 gleichzeitigen Aufrufen auslasten möchten, müssen Sie mehrere Endpunkte im gleichen Webdienst erstellen. Sie können die Last dann wahllos auf alle Endpunkte verteilen.

Die Skalierung eines Webdiensts ist eine gängige Aufgabe. Gründe für die Skalierung sind eine Unterstützung von mehr als 200 gleichzeitigen Anforderungen, das Erhöhen der Verfügbarkeit mittels mehrerer Endpunkte oder das Bereitstellen getrennter Endpunkte für den Webdienst. Sie können die Skalierung erhöhen, indem Sie über das [Azure Machine Learning-Webdienstportal](https://services.azureml.net/) zusätzliche Endpunkte für den gleichen Webdienst hinzufügen.

Weitere Informationen zum Hinzufügen neuer Endpunkte finden Sie unter [Erstellen von Endpunkten](create-endpoint.md).

Bedenken Sie, dass sich eine hohe Anzahl gleichzeitiger Aufrufe nachteilig auswirken kann, wenn die API nicht mit einer entsprechend hohen Rate aufgerufen wird. Es können sporadische Timeouts und/oder Spitzen in die Latenz auftreten, wenn eine für eine hohe Auslastung konfigurierte API relativ gering ausgelastet wird.

Die synchronen APIs werden in der Regel in Situationen verwendet, in denen eine niedrige Latenz erwünscht ist. Mit Latenz ist hier der Zeitraum gemeint, der von der API benötigt wird, um eine Anforderung abzuschließen, wobei keinerlei Netzwerkverzögerungen berücksichtigt werden. Angenommen, Sie haben eine API mit einer Latenz von 50 ms (Millisekunden). Um die verfügbare Kapazität mit der Drosselungsstufe High (Hoch) und einer Höchstzahl gleichzeitiger Anrufe = 20 voll nutzen zu können, müssen Sie diese API 20 * 1000 / 50 = 400 Mal pro Sekunde aufrufen. Wenn dieses Beispiel fortgesponnen wird, ermöglicht eine Höchstzahl von 200 gleichzeitigen Aufrufen 4.000 Aufrufe der API pro Sekunde, sofern die Latenz 50 ms beträgt.

<!--Image references-->
[1]: ./media/scaling-webservice/machlearn-1.png
[2]: ./media/scaling-webservice/machlearn-2.png
