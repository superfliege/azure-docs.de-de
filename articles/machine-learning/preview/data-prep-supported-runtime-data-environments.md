---
title: "Unterstützte Kombinationen der Ausführungs- und Datenumgebung für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine vollständige Liste der unterstützten Kombinationen von unterschiedlichen Laufzeiten und Datenquellen für die Azure Machine Learning (ML)-Datenvorbereitung."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 02e0ca96bff7781c242b4c5e965b229065e71725
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="supported-matrix-for-this-release"></a>Unterstützte Matrix für diese Version 
Wenn Ihr Code Daten mithilfe von Datenquellen oder mit Datenvorbereitungen lädt, bei dem ein Pandas- oder Spark-Datenframe abgerufen wird, werden die folgenden Kombinationen von Compute-Umgebungen für Experimente und Datenspeicherorten unterstützt:

|     |Lokale Dateien  |Azure-BLOB  |SQL Server-Datenbank***  |
|---------|---------|---------|---------|---------|
|Lokales Python    |     Unterstützt    |Nicht unterstützt         | Nicht unterstützt        |         |
|Docker (Linux-VM) Python     |Nur in Projektdateien unterstützt*         | Nicht unterstützt        |        Nicht unterstützt |         |
|Docker (Linux-VM) PySpark     |Nur in Projektdateien unterstützt*     |Unterstützt         | Unterstützt**        |         |
|Azure Data Science Virtual Machine Python     |Nur in Projektdateien unterstützt*         |Nicht unterstützt         |Nicht unterstützt         |         |
|Azure Data Science Virtual Machine PySpark     | Nur in Projektdateien unterstützt*        |Nicht unterstützt         |Nicht unterstützt         |         |
|Azure HDInsight PySpark     | Nicht unterstützt        |Unterstützt         |Unterstützt**         |         |
|Azure HDInsight Python     | Nicht unterstützt        | Nicht unterstützt        | Nicht unterstützt        |         |

Azure Data Lake Store wird derzeit nicht für alle Computeziele unterstützt.

*Wenn lokale Dateipfade verwendet werden, werden Dateien innerhalb des Projekts in die Compute-Umgebung kopiert und dann dort gelesen. Dateien außerhalb des Projekts werden nicht kopiert und Pfade in der Compute-Umgebung werden nicht mehr aufgelöst. Eventuell sollten Sie die Ersetzung von Datenquellen verwenden, damit Ihr Code eine lokale Datei bei lokaler Ausführung verwenden und für eine andere Laufzeitkonfiguration anschließend zu Azure Storage Blob wechseln kann. Sie können auch die Unterstützung für Stichprobenentnahmen für Datenquellen verwenden, um umfangreiche Daten nur in bestimmten Laufzeitkonfigurationen zu verwalten.

**Verwendet den Maven JDBC SQL Server-Treiber 6.2.1. Sie müssen sicherstellen, dass dieses Paket (oder ein kompatibles Paket) in Ihrer Datei „spark_dependencies.yml“ für die Compute-Umgebung enthalten ist.

***Unterstützt Azure SQL-Datenbank, Azure SQL Data Warehouse oder Microsoft SQL Server, sofern die Datenbank über die Compute-Umgebung erreicht werden kann. 
