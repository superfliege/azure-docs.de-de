---
title: "Unterstützte Datenquellen für die Azure Machine Learning-Datenvorbereitung | Microsoft Docs"
description: "Dieses Dokument enthält eine vollständige Liste mit unterstützten Datenquellen, die für die Azure Machine Learning-Datenvorbereitung verfügbar sind."
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
ms.date: 09/12/2017
ms.openlocfilehash: 1ef4c5c33d98cfeb566e8fe23bda9e0d3f041781
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="supported-data-sources-for-this-release"></a>Unterstützte Datenquellen für dieses Release 
Das folgende Dokument enthält die Liste der zurzeit in der Azure Machine Learning-Datenvorbereitung unterstützten Datenquellen.

Die unterstützten Datenquellen für dieses Release werden im Folgenden aufgelistet.

## <a name="types"></a>Typen 
### <a name="directory-versus-file"></a>Verzeichnis oder Datei
*Dateien/Verzeichnisse*: Wählen Sie eine einzelne Datei aus, und lesen Sie sie in die Datenvorbereitung ein. Der Dateityp wird analysiert, um die Standardparameter für die Dateiverbindung auf dem nächsten Bildschirm zu bestimmen. Wählen Sie ein Verzeichnis oder eine Gruppe von Dateien in einem Verzeichnis (die Dateiauswahl ermöglicht die Mehrfachauswahl) aus. Jeder dieser Ansätze führt dazu, dass die Dateien als einzelner Datenfluss gelesenen werden, in dem die Dateien aneinander (Header bei Bedarf entfernt) angefügt sind.

Die folgenden Dateitypen sind verfügbar:
- Mit Trennzeichen (CSV-, TSV-, TXT-Datei usw.) 
- Feste Breite
- Nur-Text
- JSON-Datei

### <a name="csv-file"></a>CSV-Datei
Liest eine CSV-Datei aus dem Speicher.

#### <a name="options"></a>Optionen
- Trennzeichen
- Kommentar
- Header
- Dezimaltrennzeichen
- Dateicodierung
- Zu überspringende Zeilen

### <a name="tsv-file"></a>TSV-Datei
Liest eine TSV-Datei aus dem Speicher.

#### <a name="options"></a>Optionen
- Kommentar
- Header
- Dateicodierung
- Zu überspringende Zeilen

### <a name="excel-xlsxlsx"></a>Excel (XLS-/XLSX-Datei)
Liest eine Excel-Datei blattweise durch Angabe von Blattname oder -nummer.

#### <a name="options"></a>Optionen
- Blattname/-nummer
- Header
- Zu überspringende Zeilen

### <a name="json-file"></a>JSON-Datei
List eine JSON-Datei aus dem Speicher. Beachten Sie, dass die Datei beim Lesen „vereinfacht“ wird.

#### <a name="options"></a>Optionen
Keine

### <a name="parquet"></a>Parquet
Liest ein PARQUET-Dataset (eine einzelne Datei oder einen Ordner).

Das PARQUET-Format kann in verschiedenen Formen im Speicher vorliegen. Für kleinere Datasets wird manchmal eine einzelne PARQUET-Datei verwendet. Verschiedene Python-Bibliotheken unterstützen das Lesen oder Schreiben in eine einzelne PARQUET-Datei. Zurzeit verwendet Azure Machine Learning Workbench für das Lesen der PARQUET-Datei bei der lokalen interaktiven Verwendung die PyArrow-Python-Bibliothek. Es werden einzelne PARQUET-Dateien unterstützt (solange sie als solche und nicht als Teil eines größeren Datasets geschrieben wurden). Darüber hinaus werden PARQUET-Datasets unterstützt. 

Ein PARQUET-Dataset ist eine Sammlung mehrerer PARQUET-Dateien, die jeweils eine kleinere Partition eines größeren Datasets darstellen. Datasets sind in der Regel in einem Ordner enthalten. Sie stellen das PARQUET-Standardausgabeformat für allgemeine Plattformen wie Spark und Hive dar.

>[!NOTE]
>Beim Lesen von PARQUET-Daten, die in einem Ordner mit mehreren PARQUET-Dateien enthalten sind, ist es am sichersten, das Verzeichnis zum Lesen auszuwählen und die Option **Parquet Dataset** zu aktivieren. Auf diese Weise liest PyArrow den gesamten Ordner anstelle einzelner Dateien. Auf diese Weise wird sichergestellt, dass Unterstützung für das Lesen komplizierterer Möglichkeiten der Speicherung von PARQUET-Dateien auf dem Datenträger (z. B. Ordnerpartitionierung) vorhanden ist.

Die Ausführung mit horizontaler Skalierung basiert auf den PARQUET-Lesefunktionen von Spark und unterstützt einzelne Dateien sowie Ordner.

#### <a name="options"></a>Optionen
*Parquet Dataset*: Diese Option bestimmt, ob Azure Machine Learning-Workbench den aktivierten oder nicht aktivierten Modus verwendet. Der nicht aktivierte Modus erweitert ein bestimmtes Verzeichnis und versucht, jede Datei einzeln zu lesen. Der aktivierte Modus behandelt das Verzeichnis als ganzes Dataset und lässt PyArrow die beste Möglichkeit zum Interpretieren der Dateien herausfinden.


## <a name="locations"></a>Standorte
### <a name="local"></a>Lokal
Lokale Festplatte oder zugeordneter Netzwerkspeicherort.

### <a name="azure-blob-storage"></a>Azure-Blobspeicher
Erfordert ein Azure-Abonnement.

