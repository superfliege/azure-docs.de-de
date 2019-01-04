---
title: Laden von Daten in Azure-Speicherumgebungen – Team Data Science-Prozess
description: Verschieben von Daten in und aus Azure Blob Storage
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: b53ece6cd2e61fa6f6340b7bda3021ee7d9d767c
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139706"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Laden von Daten in Speicherumgebungen für Analysen

Der Team Data Science-Prozess erfordert, dass Daten in einer Vielzahl von Speicherumgebungen erfasst oder geladen werden, um in jeder Phase des Prozesses auf die am besten geeignete Weise verarbeitet oder analysiert zu werden. Datenziele, die häufig für die Verarbeitung genutzt werden, sind u.a. Azure Blob Storage, SQL Azure-Datenbanken, SQL Server auf virtuellen Azure-Computern, HDInsight (Hadoop) und Azure Machine Learning. 

In den folgenden Artikeln erfahren Sie, wie Sie Daten in verschiedenen Zielumgebungen erfassen, in denen die Daten dann gespeichert und verarbeitet werden.

* [Azure Blob Storage](move-azure-blob.md) (eingehend/ausgehend)
* [SQL Server auf virtuellem Azure-Computer](move-sql-server-virtual-machine.md) (eingehend)
* [Azure SQL-Datenbank](move-sql-azure.md) (eingehend)
* [Hive-Tabellen](move-hive-tables.md) (eingehend)
* [Partitionierte SQL-Tabellen](parallel-load-sql-partitioned-tables.md) (eingehend)
* [Lokale SQL Server-Instanz](move-sql-azure-adf.md) (ausgehend)

Technische und geschäftliche Anforderungen sowie der ursprüngliche Speicherort, das Format und der Umfang Ihrer Daten bestimmen die Zielumgebungen, in denen die Daten erfasst werden müssen, um Ihre Analyseziele zu erreichen. Es ist nicht ungewöhnlich für ein Szenario, dass Daten zwischen mehreren Umgebungen verschoben werden müssen, um die verschiedenen Aufgaben zu erledigen, die zum Entwickeln eines prädiktiven Modells erforderlich sind. Zu dieser Abfolge von Aufgaben kann z. B. die Untersuchung, Vorverarbeitung, Bereinigung und das Downsampling von Daten sowie das Trainieren von Modellen gehören.
