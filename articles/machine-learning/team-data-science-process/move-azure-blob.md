---
title: Verschieben von Daten in und aus Azure Blob Storage | Microsoft-Dokumentation
description: Verschieben von Daten in und aus Azure Blob Storage
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: fa3fd5dfdd0fbdb8200b0c5c8df512caedbe735c
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441841"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Verschieben von Daten in und aus Azure Blob Storage

Der Team Data Science-Prozess erfordert, dass Daten in einer Vielzahl von Speicherumgebungen erfasst oder geladen werden, um in jeder Phase des Prozesses auf die am besten geeignete Weise verarbeitet oder analysiert zu werden.
In den folgenden Artikeln beschrieben, wie Sie mithilfe unterschiedlicher Technologien Daten in und aus Azure Blob Storage verschieben.

* [Azure Storage-Explorer](move-data-to-azure-blob-using-azure-storage-explorer.md)
* [AzCopy](move-data-to-azure-blob-using-azcopy.md)
* [Python](move-data-to-azure-blob-using-python.md)
* [SSIS](move-data-to-azure-blob-using-ssis.md)

Es hängt vom jeweiligen Szenario ab, welche Methode für Sie am besten geeignet ist. Mithilfe der Informationen im Artikel [Szenarien für erweiterte Analysen in Azure Machine Learning](plan-sample-scenarios.md) können Sie die Ressourcen ermitteln, die Sie für verschiedene, im erweiterten Analyseprozess verwendeten Data Science-Workflows benötigen.

> [!NOTE]
> Eine umfassende Einführung in Azure-Blobspeicher finden Sie unter [Erste Schritte mit Azure Blob Storage](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) und [Azure-Blobdienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

Als Alternative können Sie [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) für folgende Zwecke verwenden: 

* Erstellen und Planen einer Pipeline, die Daten aus Azure Blob Storage herunterlädt 
* Übergeben dieser Daten an einen veröffentlichten Azure Machine Learning-Webdienst 
* Abrufen der Predictive Analytics-Ergebnisse und 
* Hochladen der Ergebnisse in den Speicher 

Weitere Informationen finden Sie unter [Erstellen von Vorhersagepipelines mithilfe von Azure Data Factory und Azure Machine Learning](../../data-factory/transform-data-using-machine-learning.md).

## <a name="prerequisites"></a>Voraussetzungen
In diesem Dokument wird davon ausgegangen, dass Sie über ein Azure-Abonnement, ein Speicherkonto und den zugehörigen Speicherschlüssel für dieses Konto verfügen. Bevor Sie Daten hoch- und herunterladen können, müssen Sie den Namen Ihres Azure-Speicherkontos und den Kontoschlüssel kennen.

* Informationen zum Einrichten eines Azure-Abonnements finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Anleitungen zum Erstellen eines Speicherkontos und zum Abrufen von Konto- und Schlüsselinformationen finden Sie unter [Informationen zu Azure-Speicherkonten](../../storage/common/storage-create-storage-account.md).

