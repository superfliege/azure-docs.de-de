---
title: "Unterstützte Kombinationen der Ausführungs- und Datenumgebung für Azure Machine Learning-Datenvorbereitungen | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine vollständige Liste der unterstützten Kombinationen von unterschiedlichen Laufzeiten und Datenquellen für Azure Machine Learning-Datenvorbereitungen."
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
ms.openlocfilehash: 413bc8a0e0347498c004b93fb37f51d86ad029f5
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="supported-matrix-for-this-release"></a>Unterstützte Matrix für diese Version 
Wenn Ihr Code Daten mithilfe von Azure Machine Learning-Datenquellen oder Azure Machine Learning-Datenvorbereitungen lädt, bei denen ein Pandas- oder Spark-Datenframe abgerufen wird, werden die folgenden Kombinationen von Compute-Umgebungen für Experimente und Datenspeicherorten unterstützt:

|     |Lokale Dateien  |Azure-Blobspeicher  |SQL Server-Datenbank***  |
|---------|---------|---------|---------|---------|
|Lokales Python    |     Unterstützt    |Nicht unterstützt         | Nicht unterstützt        |         |
|Docker (Linux-VM) Python     |Nur in Projektdateien unterstützt*         | Nicht unterstützt        |        Nicht unterstützt |         |
|Docker (Linux-VM) PySpark     |Nur in Projektdateien unterstützt*     |Unterstützt         | Unterstützt**        |         |
|Azure Data Science Virtual Machine Python     |Nur in Projektdateien unterstützt*         |Nicht unterstützt         |Nicht unterstützt         |         |
|Azure Data Science Virtual Machine PySpark     | Nur in Projektdateien unterstützt*        |Nicht unterstützt         |Nicht unterstützt         |         |
|Azure HDInsight PySpark     | Nicht unterstützt        |Unterstützt         |Unterstützt**         |         |
|Azure HDInsight Python     | Nicht unterstützt        | Nicht unterstützt        | Nicht unterstützt        |         |

Azure Data Lake Store wird derzeit nicht für alle Computeziele unterstützt.

*Wenn lokale Dateipfade verwendet werden, werden Dateien innerhalb des Projekts in die Compute-Umgebung kopiert und dann dort gelesen. Dateien außerhalb des Projekts werden nicht kopiert und Pfade in der Compute-Umgebung werden nicht mehr aufgelöst. Erwägen Sie die Verwendung der Datenquellenersetzung, damit Ihr Code eine lokale Datei verwenden kann, wenn er lokal ausgeführt wird. Wechseln Sie dann zu einem Azure Storage-Blob für eine andere Laufzeitkonfiguration. Sie können auch die Unterstützung für Stichprobenentnahmen für Datenquellen verwenden, um umfangreiche Daten nur in bestimmten Laufzeitkonfigurationen zu verwalten.

**Verwendet den Maven JDBC SQL Server-Treiber 6.2.1. Sie müssen sicherstellen, dass dieses Paket (oder ein kompatibles Paket) in Ihrer Datei „spark_dependencies.yml“ für die Compute-Umgebung enthalten ist.

***Unterstützt Azure SQL-Datenbank, Azure SQL Data Warehouse oder SQL Server, sofern die Datenbank über die Compute-Umgebung erreicht werden kann. 
