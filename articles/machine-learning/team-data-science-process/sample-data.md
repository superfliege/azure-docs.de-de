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
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: 73b0d3992627b7787ee46e61f62717a545c5b9e2
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="heading"></a>Entnehmen von Datenstichproben in Azure-Blobcontainern, SQL Server und Hive-Tabellen
Dieses Dokument enthält Links zu Artikeln, die das Entnehmen von Stichproben aus Daten beschreiben, die an einem von drei verschiedenen Azure-Speicherorten gespeichert sind:

* **Azure Blob-Containerdaten** : Stichproben werden durch programmgesteuertes Herunterladen entnommen, um dann anschließend mit Python-Code Stichproben zu erstellen.
* **SQL Server-Daten** : Stichproben werden mithilfe von SQL und der Python-Programmiersprache erstellt. 
* **Hive-Tabellendaten** : Stichproben werden mit Hive-Abfragen erstellt.

Das nachstehende **Menü** enthält Links zu Themen, die das Entnehmen von Datenstichproben aus diesen Azure Storage-Umgebungen beschreiben. 

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

Dieser Stichprobentask ist ein Schritt im [Team Data Science-Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

**Warum sollten Datenstrichproben entnommen werden?**

Wenn das zu analysierende Dataset groß ist, sollten Sie in der Regel eine Komprimierung der Daten durchführen, um eine geringere aber immer noch repräsentative Größe zu erhalten. Dies erleichtert das Verständnis der Daten, das Durchsuchen und die Funktionsverarbeitung. Die Funktion besteht innerhalb des Cortana-Analyseprozesses darin, schnell Prototypen der Funktionen zur Datenverarbeitung und Modelle für das maschinelle Lernen zu erstellen.

