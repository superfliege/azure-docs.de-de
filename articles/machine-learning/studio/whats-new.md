---
title: Neuigkeiten – Azure Machine Learning Studio | Microsoft-Dokumentation
description: Neue Features, die in Azure Machine Learning Studio verfügbar sind.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: ddc716ed-2615-4806-bf27-6c9a5662a7f2
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2018
ms.openlocfilehash: 90a9a81de488f2b3b256e3164c3bfde3019d0ad3
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53253007"
---
# <a name="whats-new-in-azure-machine-learning-studio"></a>Neuerungen in Azure Machine Learning Studio

## <a name="october-2018"></a>Oktober 2018

Für das R-Sprachmodul im Modul [Execute R Script](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-r-script) wurde eine neue R-Laufzeitversion hinzugefügt: Microsoft R Open (MRO) 3.4.4. MRO 3.4.4 basiert auf der Open-Source-Version CRAN R 3.4.4 und ist deshalb mit Paketen kompatibel, für die diese Version von R verwendet werden kann. Weitere Informationen zu den unterstützten R-Paketen finden Sie im Artikel [R Packages supported by Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/r-packages-supported-by-azure-machine-learning#bkmk_List) (Von Azure Machine Learning Studio unterstützte R-Pakete).

## <a name="march-2017"></a>17. März 2017 
Dieses Release der Microsoft Azure Machine Learning-Updates enthält das folgende Feature:

* Dedizierte Kapazität für Azure Machine Learning-BES-Aufträge

    Die Machine Learning Batch-Pool-Verarbeitung verwendet den [Azure Batch-Dienst](../../batch/batch-technical-overview.md), um kundenverwaltete Skalierung für den Azure Machine Learning-Batchausführungsdienst zu ermöglichen. Die Batch Pool-Verarbeitung ermöglicht Ihnen das Erstellen von Azure Batch-Pools, an die Sie Batchaufträge senden und diese auf vorhersagbare Weise ausführen können.

    Weitere Informationen finden Sie unter [Azure Batch-Dienst für Machine Learning-Aufträge](dedicated-capacity-for-bes-jobs.md).


## <a name="august-2016"></a>August 2016 
Dieses Release der Microsoft Azure Machine Learning-Updates enthält die folgenden Features:
* Klassische Webdienste können nun im neuen [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/)-Portal verwaltet werden, das als Zentrale für die Verwaltung aller Aspekte Ihres Webdiensts fungiert.    
  * Ein Webdienst für [Nutzungsstatistik](manage-new-webservice.md) wird bereitgestellt.
  * Das Testen von Remote-Request-Aufrufen in Azure Machine Learning, die Beispieldaten verwenden, wird vereinfacht.
  * Es gibt eine neue Batch Execution Service-Testseite mit Beispieldaten und Auftragsübermittlungsverlauf.
  * Einfachere Verwaltung von Endpunkten.

## <a name="july-2016"></a>Juli 2016 
Dieses Release der Microsoft Azure Machine Learning-Updates enthält die folgenden Features:
* Webdienste werden jetzt als Azure-Ressourcen über [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) -Schnittstellen verwaltet, wodurch sich folgende Verbesserungen und Erweiterungen ergeben:
  * Es gibt neue [REST-APIs](https://msdn.microsoft.com/library/azure/Dn950030.aspx) zum Bereitstellen und Verwalten Ihrer Resource Manager-basierten Webdienste.
  * Es gibt ein neues [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/)-Portal, das als Zentrale zum Verwalten aller Aspekte Ihres Webdiensts fungiert.
* Es gibt ein neues abonnementbasiertes Bereitstellungsmodell für Webdienste für mehrere Regionen, das Resource Manager-basierte APIs verwendet, die den Resource Manager-Ressourcenanbieter für Webdienste nutzen.
* Es gibt neue [Preispläne](https://azure.microsoft.com/pricing/details/machine-learning/) und Planverwaltungsfunktionen, die den neuen Resource Manager-Ressourcenanbieter für die Abrechnung nutzen.
  * Sie können nun [Ihren Webdienst in mehreren Regionen bereitstellen](how-to-deploy-to-multiple-regions.md) , ohne für jede Region ein Abonnement erstellen zu müssen.
* Es wird ein Webdienst für [Verwendungsstatistiken](manage-new-webservice.md)bereitgestellt.
* Das Testen von Remote-Request-Aufrufen in Azure Machine Learning, die Beispieldaten verwenden, wird vereinfacht.
* Es gibt eine neue Batch Execution Service-Testseite mit Beispieldaten und Auftragsübermittlungsverlauf.

Darüber hinaus wurde Machine Learning Studio so aktualisiert, dass Sie die Bereitstellung gemäß dem neuen Webdienstmodell oder weiter gemäß dem klassischen Webdienstmodell vornehmen können. 

