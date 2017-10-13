---
title: "Verfügbare unterstützte Datenziele bzw. -ausgaben für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine vollständige Liste mit unterstützten Zielen bzw. Ausgaben, die für die Azure ML-Datenvorbereitung verfügbar sind."
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
ms.openlocfilehash: 415ceba02eb3c8da3de5ab3aa6980fbe5bae2db9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="supported-data-exports-for-this-preview"></a>Unterstützte Datenexporte für diese Vorschau 
Es können Exporte in verschiedene Formate durchgeführt werden. Bei diesen Formaten können die Zwischenergebnisse der Datenvorbereitung beibehalten werden, bevor diese in den restlichen Workflows Machine Learning-Workflow integriert werden.

## <a name="types"></a>Typen 
### <a name="csv-file"></a>CSV-Datei 
Schreiben Sie eine durch Trennzeichen getrennte Datei (Comma-Separated Value, CSV) in den Speicher.

#### <a name="options"></a>Optionen
- Zeilenenden
- Ersetzen von NULL-Werten durch
- Ersetzen von Fehlern durch 
- Trennzeichen


### <a name="parquet"></a>Parquet ###
Schreiben Sie ein Dataset im PARQUET-Format in den Speicher.

Das PARQUET-Format kann in verschiedenen Formen im Speicher vorliegen. Bei kleineren Datasets wird in einigen Fällen eine einzelne PARQUET-Datei verwendet. Das Lesen von einzelnen PARQUET-Dateien bzw. Schreiben in einzelne PARQUET-Dateien wird von verschiedenen Python-Bibliotheken unterstützt. Derzeit verwendet AMLWB für das Schreiben der PARQUET-Datei bei der lokalen interaktiven Verwendung die PyArrow-Python-Bibliothek. Dies bedeutet, dass als PARQUET-Ausgabeformat bei der lokalen interaktiven Verwendung derzeit ausschließlich eine einzelne PARQUET-Datei unterstützt wird.

Bei Ausführungen mit horizontaler Hochskalierung (in Spark) nutzt AMLWB die Lese-/Schreibfunktionen von Spark für PARQUET-Dateien. Das (zurzeit einzige unterstützte) Standardausgabeformat von Spark für PARQUET-Dateien ähnelt der Struktur eines HIVE-Datasets. Das heißt, es liegt ein Ordner mit vielen PARQUET-Dateien vor, die jeweils eine kleinere Partition eines größeren Datasets darstellen. 

#### <a name="caveats"></a>Einschränkungen ####
Beim PARQUET-Format handelt es sich um ein relativ junges Format, das einige Inkonsistenzen bei der Implementierung in verschiedenen Bibliotheken aufweist. Beispielsweise existieren bei Spark Einschränkungen im Hinblick darauf, welche Zeichen beim Schreiben in PARQUET-Dateien in Spaltennamen verwendet werden dürfen, was bei PyArrow hingegen nicht der Fall ist. So dürfen Zeichen in der Zusammensetzung „,;{}()\\n\\t=“ nicht in Spaltennamen verwendet werden.

>[!NOTE]
>Zum Sicherstellen von Kompatibilität mit Spark werden diese Zeichen in Spaltennamen beim Schreiben von Daten in PARQUET-Dateien stets durch „_“ (Unterstrich) ersetzt.**

>[!NOTE]
>Um Konsistenz bei lokalen Ausführungen und Ausführungen mit horizontaler Hochskalierung sowie Kompatibilität mit Spark sicherzustellen, werden Spaltennamen in sämtlichen Daten, die mit der App Python oder Spark in Parquet-Dateien geschrieben werden, bereinigt. Damit bei Schreibvorgängen in PARQUET-Dateien die erwarteten Spaltennamen generiert werden, sollten ungültige Zeichenzusammensetzungen in Spark aus Spalten entfernt werden, bevor diese geschrieben werden.



## <a name="locations"></a>Standorte 
### <a name="local"></a>Lokal 
Lokale Festplatte oder zugeordneter Netzwerkspeicherort

### <a name="azure-blob"></a>Azure-BLOB 
Für Azure Storage (BLOB) ist ein Azure-Abonnement erforderlich.

