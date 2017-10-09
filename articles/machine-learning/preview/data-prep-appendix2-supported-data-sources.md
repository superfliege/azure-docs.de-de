---
title: "Unterstützte Datenquellen für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine vollständige Liste mit unterstützten Datenquellen, die für die Azure ML-Datenvorbereitung verfügbar sind."
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
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 0a77b31e07f118155094fab4d3393ffdb37c9c6f
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="supported-data-sources-for-this-release"></a>Unterstützte Datenquellen für dieses Release 
Das folgende Dokument enthält die Liste der derzeit in der Datenvorbereitung unterstützten Datenquellen.

Die unterstützten Datenquellen für dieses Release sind im Folgenden aufgelistet.

## <a name="types"></a>Typen 
### <a name="directory-vs-file"></a>Verzeichnis im Vergleich zur Datei
Dateien/Verzeichnisse – wählen Sie eine einzelne Datei, und lesen Sie sie in die Datenvorbereitung ein; der Dateityp wird analysiert, um die Standardparameter für die Dateiverbindung auf dem nächsten Bildschirm zu bestimmen. Wählen Sie ein Verzeichnis oder einen Satz von Dateien in einem Verzeichnis (Mehrfachauswahl möglich); bei jedem Ansatz werden die Dateien als ein einzelner Datenfluss eingelesen, wobei die Dateien, aneinander angefügt werden (ggf. mit entfernten Headern).

Die unterstützten Dateitypen sind:
- Mit Trennzeichen (CSC, TSC, TXT usw.), 
- Feste Breite
- Nur Text
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
Lesen einer durch Tabs getrennten Datei aus dem Speicher

#### <a name="options"></a>Optionen
- Kommentar
- Header
- Dateicodierung
- Zu überspringende Zeilen

### <a name="excel-xlsxlsx"></a>Excel-Dateien (.xls/xlsx)
Lesen einer Excel-Datei, jeweils ein Blatt bei Angabe von Blattname oder -nummer

#### <a name="options"></a>Optionen
- Blattname/-nummer
- Header
- Zu überspringende Zeilen

### <a name="json-file"></a>JSON-Datei
Lesen einer JSON-Datei aus dem Speicher; beachten Sie, dass die Datei beim Lesen „vereinfacht“ wird

#### <a name="options"></a>Optionen
- Keine

### <a name="parquet"></a>Parquet
Lesen eines Parquet-Datasets, entweder einzelne Datei oder Ordner.

Das PARQUET-Format kann in verschiedenen Formen im Speicher vorliegen. Bei kleineren Datasets wird in einigen Fällen eine einzelne PARQUET-Datei verwendet. Das Lesen von einzelnen PARQUET-Dateien bzw. Schreiben in einzelne PARQUET-Dateien wird von verschiedenen Python-Bibliotheken unterstützt. Derzeit verwendet AMLWB für das Lesen der PARQUET-Datei bei der lokalen interaktiven Verwendung die PyArrow-Python-Bibliothek. Es werden sowohl einzelne PARQUET-Dateien unterstützt (solange sie als solche, nicht als Teil eines größeren Datasets geschrieben wurden) als auch PARQUET-Datasets. Ein PARQUET-Dataset ist eine Sammlung mehrerer PARQUET-Datei, die jeweils eine kleinere Partition eines größeren Datasets darstellen. Datasets sind in der Regel in einem Ordner enthalten und das PARQUET-Standardausgabeformat für allgemeine Plattformen wie Spark und Hive.

>[!NOTE]
>Beim Lesen von PARQUET-Daten, die in einem Ordner mit mehreren PARQUET-Dateien enthalten sind, ist es am sichersten, das Verzeichnis zum Lesen zu wählen und die Option „Parquet Dataset“ zu aktivieren. Dies veranlasst PyArrow, den gesamten Ordner statt der einzelnen Dateien zu lesen, sodass die Unterstützung für das Lesen komplizierterer Möglichkeiten der PARQUET-Speicherung auf dem Datenträger (z.B. Ordnerpartitionierung) sichergestellt ist.**

Ausführung mit horizontaler Skalierung basiert auf den PARQUET-Lesefunktionen von Spark und unterstützt einzelne Dateien sowie Ordner, auf ähnliche Weise wie lokal interaktiv.

#### <a name="options"></a>Optionen
- Parquet Dataset
  - Diese Option bestimmt, ob AMLWB ein bestimmtes Verzeichnis erweitert, und versucht, jede Datei darin einzeln zu lesen (nicht aktivierter Modus) oder das Verzeichnis als ganzen Dataset zu behandeln und PyArrow die beste Möglichkeit zum Interpretieren der Dateien herausfinden zu lassen (aktivierter Modus).


## <a name="locations"></a>Standorte
### <a name="local"></a>Lokal
Lokale Festplatte oder zugeordneter Netzwerkspeicherort

### <a name="azure-blob"></a>Azure-BLOB
Für Azure Storage (BLOB) ist ein Azure-Abonnement erforderlich.


