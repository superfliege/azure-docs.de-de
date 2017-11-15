---
title: "Unterstützte Datenquellen für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
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
ms.openlocfilehash: db4774de28a17e022de111986f72a1f15ec32beb
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="supported-data-sources-for-azure-machine-learning-data-preparation"></a>Unterstützte Datenquellen für die Azure Machine Learning-Datenvorbereitung 
Dieser Artikel enthält eine Übersicht der zurzeit in der Azure Machine Learning-Datenvorbereitung unterstützten Datenquellen.

Folgende Datenquellen werden für dieses Release unterstützt.

## <a name="types"></a>Typen 
### <a name="directory-vs-file"></a>Verzeichnis im Vergleich zu Datei
Wählen Sie eine einzelne Datei aus, und lesen Sie sie in die Datenvorbereitung ein. Der Dateityp wird analysiert, um die Standardparameter für die Dateiverbindung auf dem nächsten Bildschirm zu bestimmen.

Wählen Sie ein Verzeichnis oder eine Gruppe von Dateien in einem Verzeichnis aus (die Dateiauswahl ermöglicht Mehrfachauswahl). Mit beiden Ansätzen werden die Dateien als einzelner Datenfluss gelesen, in dem die Dateien aneinander angefügt sind, bei Bedarf mit entfernten Headern.

Folgende Dateitypen werden unterstützt:
- Mit Trennzeichen (CSV, TSV, TXT usw.)
- Feste Breite
- Nur-Text
- JSON-Datei

### <a name="csv-file"></a>CSV-Datei
Lesen einer durch Kommas getrennten Datei aus dem Speicher

#### <a name="options"></a>Optionen
- Trennzeichen
- Kommentar
- Header
- Dezimaltrennzeichen
- Dateicodierung
- Zu überspringende Zeilen

### <a name="tsv-file"></a>TSV-Datei
Lesen einer durch Tabstopps getrennten Datei aus dem Speicher

#### <a name="options"></a>Optionen
- Kommentar
- Header
- Dateicodierung
- Zu überspringende Zeilen

### <a name="excel-xlsxlsx"></a>Excel (XLS-/XLSX-Datei)
Lesen einer Excel-Datei, jeweils ein Blatt durch Angabe von Blattname oder -nummer

#### <a name="options"></a>Optionen
- Blattname oder -nummer
- Header
- Zu überspringende Zeilen

### <a name="json-file"></a>JSON-Datei
List eine JSON-Datei aus dem Speicher. Beachten Sie, dass die Datei beim Lesen vereinfacht wird.

#### <a name="options"></a>Optionen
- Keine

### <a name="parquet"></a>Parquet
Lesen eines Parquet-DataSets (einzelne Datei oder Ordner).

Das PARQUET-Format kann in verschiedenen Formen im Speicher vorliegen. Für kleinere DataSets wird manchmal eine einzelne PARQUET-Datei verwendet. Verschiedene Python-Bibliotheken unterstützen das Lesen oder Schreiben in einzelne PARQUET-Dateien. Momentan verwendet die Azure Machine Learning-Datenvorbereitung für das Lesen von Parquet bei der lokalen interaktiven Verwendung die PyArrow-Python-Bibliothek. Es werden sowohl einzelne PARQUET-Dateien unterstützt (sofern sie als solche und nicht als Teil eines größeren DataSets geschrieben wurden) als auch Parquet-DataSets.

Ein Parquet-DataSet ist eine Sammlung mehrerer PARQUET-Dateien, die jeweils einen kleineren Abschnitt eines größeren DataSets darstellen. DataSets sind in der Regel in einem Ordner enthalten und das Parquet-Standardausgabeformat für allgemeine Plattformen wie Spark und Hive.

>[!NOTE]
>Beim Lesen von Parquet-Daten, die in einem Ordner mit mehreren PARQUET-Dateien enthalten sind, ist es am sichersten, das Verzeichnis zum Lesen auszuwählen und die Option **Parquet-DataSet** zu aktivieren. Auf diese Weise liest PyArrow den gesamten Ordner anstelle einzelner Dateien. Auf diese Weise wird sichergestellt, dass das Lesen komplizierterer Möglichkeiten der Speicherung von Parquet-Daten auf dem Datenträger (z.B. Ordnerpartitionierung) unterstützt wird.

Die Ausführung mit horizontaler Hochskalierung basiert auf den Parquet-Lesefunktionen von Spark. Dabei werden einzelne Dateien sowie Ordner auf ähnliche Weise wie bei lokal interaktiver Ausführung unterstützt.

#### <a name="options"></a>Optionen
- Parquet-DataSet. Diese Option legt fest, ob die Azure Machine Learning-Datenvorbereitung ein gegebenes Verzeichnis erweitert und versucht, jede Datei einzeln zu lesen (Modus „Nicht ausgewählt“), oder das Verzeichnis als gesamtes DataSet behandelt (Modus „Ausgewählt“). Im Modus „Ausgewählt“ legt PyArrow die beste Möglichkeit fest, die Dateien zu interpretieren.


## <a name="locations"></a>Standorte
### <a name="local"></a>Lokal
Eine lokale Festplatte oder ein zugeordneter Netzwerkspeicherort

### <a name="azure-blob-storage"></a>Azure-Blobspeicher
Azure Blob Storage (mit Anforderung eines Azure-Abonnements)

