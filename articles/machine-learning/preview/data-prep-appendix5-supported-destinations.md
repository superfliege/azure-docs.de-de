---
title: "Unterstützte Datenziele und -ausgaben bei der Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine vollständige Liste der unterstützten Ziele und Ausgaben, die für die Azure Machine Learning-Datenvorbereitung verfügbar sind."
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
ms.date: 09/07/2017
ms.openlocfilehash: bbbee61d6cd67dd437e4fbcd7260d2f378531912
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="supported-data-exports-for-this-preview"></a>Unterstützte Datenexporte für diese Vorschau 
Es können Exporte in verschiedene Formate durchgeführt werden. Bei diesen Formaten können die Zwischenergebnisse der Datenvorbereitung beibehalten werden, bevor sie in den restlichen Machine Learning-Workflow integriert werden.

## <a name="types"></a>Typen 
### <a name="csv-file"></a>CSV-Datei 
Schreiben Sie eine durch Trennzeichen getrennte Datei (Comma-Separated Value, CSV) in den Speicher.

#### <a name="options"></a>Optionen
- Zeilenenden
- Ersetzen von NULL-Werten durch
- Ersetzen von Fehlern durch 
- Trennzeichen


### <a name="parquet"></a>Parquet 
Schreiben Sie ein Dataset im PARQUET-Format in den Speicher.

Das PARQUET-Format kann in verschiedenen Formen im Speicher vorliegen. Für kleinere Datasets wird manchmal eine einzelne PARQUET-Datei verwendet. Verschiedene Python-Bibliotheken unterstützen das Lesen oder Schreiben in einzelne PARQUET-Dateien. 

Zurzeit verwendet Azure Machine Learning Workbench für das Schreiben der Parquet-Datei bei der lokalen interaktiven Verwendung die PyArrow-Python-Bibliothek. Dies bedeutet, dass als Parquet-Ausgabeformat bei der lokalen interaktiven Verwendung derzeit ausschließlich eine einzelne Parquet-Datei unterstützt wird.

Bei Ausführungen mit horizontaler Hochskalierung (in Spark) nutzt Azure Machine Learning Workbench die Lese-/Schreibfunktionen von Spark für Parquet. Das (zurzeit einzige unterstützte) Standardausgabeformat von Spark für Parquet ähnelt der Struktur eines HIVE-DataSets. Das heißt, ein Ordner enthält viele PARQUET-Dateien, die jeweils eine kleinere Partition eines größeren DataSets darstellen. 

#### <a name="caveats"></a>Einschränkungen 
Beim PARQUET-Format handelt es sich um ein relativ junges Format, das einige Inkonsistenzen bei der Implementierung in verschiedenen Bibliotheken aufweist. Beispielsweise existieren bei Spark Einschränkungen im Hinblick darauf, welche Zeichen beim Schreiben in PARQUET-Dateien in Spaltennamen verwendet werden dürfen. Bei PyArrow ist dies nicht der Fall. Die folgenden Zeichen dürfen in einem Spaltennamen nicht enthalten sein: 
- dann
- ;
- {}
- ()
- \\n
- \\t
- =

>[!NOTE]
>- Zum Sicherstellen der Kompatibilität mit Spark werden diese Zeichen in Spaltennamen beim Schreiben von Daten in Parquet stets durch „_“ (Unterstrich) ersetzt.
>- Um Konsistenz bei lokalen Ausführungen und Ausführungen mit horizontaler Hochskalierung sowie Kompatibilität mit Spark sicherzustellen, werden Spaltennamen in sämtlichen Daten, die mit der App, Python oder Spark in Parquet geschrieben werden, bereinigt. Damit bei Schreibvorgängen in Parquet die erwarteten Spaltennamen generiert werden, sollten ungültige Zeichenzusammensetzungen in Spark aus Spalten entfernt werden, bevor diese geschrieben werden.



## <a name="locations"></a>Standorte 
### <a name="local"></a>Lokal 
Lokale Festplatte oder zugeordneter Netzwerkspeicherort.

### <a name="azure-blob-storage"></a>Azure-Blobspeicher
Azure Blob Storage erfordert ein Azure-Abonnement.

