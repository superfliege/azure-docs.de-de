---
title: Was ist mit dem WebJob-Projekt passiert (verbundene Visual Studio-Dienste für Azure Storage)? | Microsoft-Dokumentation
description: Erfahren Sie, was nach dem Herstellen einer Verbindung mit einem Speicherkonto mithilfe von verbundenen Visual Studio-Diensten in einem Azure-WebJob-Projekt passiert.
services: storage
author: ghogen
manager: douge
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 673143c2bfb23bda45c4e435c3048ae61fa16902
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51256900"
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Was ist mit dem WebJob-Projekt passiert (verbundene Visual Studio-Dienste für Azure Storage)?
## <a name="references-added"></a>Verweise wurden hinzugefügt
Das Azure Storage NuGet-Paket wurde zu Ihrem Visual Studio-Projekt hinzugefügt oder aktualisiert.  
Dieses Paket fügt die folgenden .NET-Verweise hinzu:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Die Verbindungszeichenfolge für Azure Storage wurde hinzugefügt
In der Datei „App.config“ Ihres Projekts wurden die Einträge **AzureWebJobsStorage** und **AzureWebJobsDashboard** mit der Verbindungszeichenfolge und dem Schlüssel des ausgewählten Speicherkontos aktualisiert.

Weitere Informationen finden Sie unter [Dokumentationsressourcen für Azure WebJobs](https://go.microsoft.com/fwlink/?linkid=390226).

