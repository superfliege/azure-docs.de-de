---
title: Entnehmen von Datenstichproben in Azure-Blobcontainern, SQL Server und Hive-Tabellen | Microsoft Docs
description: Informationen zum Durchsuchen von Daten, die in verschiedenen Azure-Umgebungen gespeichert sind.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 80a9dfae-e3a6-4cfb-aecc-5701cfc7e39d
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: 60431578ba8b5c740fe5e1124dcdad32c3cb2ede
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344081"
---
# <a name="heading"></a>Entnehmen von Datenstichproben in Azure-Blobcontainern, SQL Server und Hive-Tabellen

In den folgenden Artikeln wird das Entnehmen von Stichproben aus Daten beschrieben, die an einem von drei verschiedenen Azure-Speicherorten gespeichert sind:

* [**Azure Blob-Containerdaten:**](sample-data-blob.md) Stichproben werden durch programmgesteuertes Herunterladen entnommen, um dann anschließend mit Python-Code Stichproben zu erstellen.
* [**SQL Server-Daten:**](sample-data-sql-server.md) Stichproben werden mithilfe von SQL und der Python-Programmiersprache erstellt. 
* [**Hive-Tabellendaten:**](sample-data-hive.md) Stichproben werden mit Hive-Abfragen erstellt.

Dieser Stichprobentask ist ein Schritt im [Team Data Science-Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

**Warum sollten Datenstrichproben entnommen werden?**

Wenn das zu analysierende Dataset groß ist, sollten Sie in der Regel eine Komprimierung der Daten durchführen, um eine geringere aber immer noch repräsentative Größe zu erhalten. Dies erleichtert das Verständnis der Daten, das Durchsuchen und die Funktionsverarbeitung. Die Funktion besteht innerhalb des Cortana-Analyseprozesses darin, schnell Prototypen der Funktionen zur Datenverarbeitung und Modelle für das maschinelle Lernen zu erstellen.

