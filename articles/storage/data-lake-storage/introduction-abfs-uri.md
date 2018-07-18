---
title: Verwenden des Azure Data Lake Storage Gen2 (Vorschauversion)-URI
description: Verwenden des Azure Data Lake Storage Gen2 (Vorschauversion)-URI
services: storage
keywords: ''
author: jamesbak
ms.topic: article
ms.author: jamesbak
manager: jahogg
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 75edf6dc7382a8a2ece7c25edd09aeacfe1c5189
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060058"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Verwenden des Azure Data Lake Storage Gen2-URI

Der mit Azure Data Lake Storage Gen2 (Vorschauversion) kompatible [Hadoop-Dateisystemtreiber](http://www.aosabook.org/en/hdfs.html) ist unter seinem Schemabezeichner `abfs` (Azure Blob File System) bekannt. Wie andere Hadoop-Dateisystemtreiber verwendet auch der ABFS-Treiber ein URI-Format zur Angabe von Dateien und Verzeichnissen in einem Azure Data Lake Storage Gen2-fähigen Konto.

## <a name="uri-syntax"></a>URI-Syntax

Die URI-Syntax für Data Lake Storage Gen2 hängt davon ab, ob für Ihr Speicherkonto Data Lake Storage Gen2 als Standarddateisystem eingerichtet ist.

Wenn das zu adressierende Data Lake Storage Gen2-fähige Konto während der Kontoerstellung als Standarddateisystem festgelegt wurde, lautet die URI-Syntax in der Kurzschreibweise wie folgt:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Pfad**: Eine durch Schrägstriche (`/`) getrennte Darstellung der Verzeichnisstruktur.

2. **Dateiname**: Der Name der jeweiligen Datei.

Wenn das zu adressierende Data Lake Storage Gen2-fähige Konto *nicht* das Standarddateisystem ist, lautet die URI-Syntax wie folgt:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.widows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Schemabezeichner**: Das `abfs`-Protokoll wird als Schemabezeichner verwendet. Sie haben die Möglichkeit, eine Verbindung mit oder ohne SSL (Secure Socket Layer) herzustellen. Verwenden Sie `abfss` zum Herstellen einer SSL-Verbindung.

2. **Dateisystem**: Der übergeordnete Speicherort, in dem sich die Dateien und Ordner befinden. Dies entspricht Containern in Azure Storage-Blobs.

3. **Kontoname**: Der Name, der Ihrem Speicherkonto während der Erstellung gegeben wurde.

4. **Pfade**: Eine durch Schrägstriche (`/`) getrennte Darstellung der Verzeichnisstruktur.

5. **Dateiname**: Der Name der jeweiligen Datei. Beim Verweisen auf ein Verzeichnis ist dieser Parameter optional.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](use-hdi-cluster.md)