---
title: "Laden von Daten in Azure-Speicherumgebungen für Analysen | Microsoft-Dokumentation"
description: Verschieben von Daten in den und aus dem Azure-Blobspeicher
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 3a4e8214043ece060cfabae8f74ebf21344777d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Laden von Daten in Speicherumgebungen für Analysen
Der Team Data Science-Prozess erfordert, dass Daten in einer Vielzahl von Speicherumgebungen erfasst oder geladen werden, um in jeder Phase des Prozesses auf die am besten geeignete Weise verarbeitet oder analysiert zu werden. Datenziele, die häufig für die Verarbeitung genutzt werden, sind u.a. Azure Blob Storage, SQL Azure-Datenbanken, SQL Server auf virtuellen Azure-Computern, HDInsight (Hadoop) und Azure Machine Learning. 

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

Dieses **Menü** bietet Links zu Themen, in denen beschrieben wird, wie Daten in diesen Zielumgebungen erfasst werden, in denen die Daten gespeichert und verarbeitet werden.

Technische und geschäftliche Anforderungen sowie der ursprüngliche Speicherort, das Format und der Umfang Ihrer Daten bestimmen die Zielumgebungen, in denen die Daten erfasst werden müssen, um Ihre Analyseziele zu erreichen. Es ist nicht ungewöhnlich für ein Szenario, dass Daten zwischen mehreren Umgebungen verschoben werden müssen, um die verschiedenen Aufgaben zu erledigen, die zum Entwickeln eines prädiktiven Modells erforderlich sind. Zu dieser Abfolge von Aufgaben kann z. B. die Untersuchung, Vorverarbeitung, Bereinigung und das Downsampling von Daten sowie das Trainieren von Modellen gehören.

