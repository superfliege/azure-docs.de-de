---
title: Verwenden von Azure Data Lake Storage (ADLS) Gen2 mit Apache Hadoop in Azure HDInsight
description: Bietet eine Übersicht über Azure Data Lake Storage Gen2 und dessen Zusammenarbeit mit Azure HDInsight.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/21/2018
ms.openlocfilehash: b7cde3a8990a51e95a8ce9ad85bca524d5669e0c
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53721120"
---
# <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Verwenden von Azure Data Lake Storage Gen2 mit Apache Hadoop in Azure HDInsight

Azure Data Lake Storage (ADLS) Gen2 übernimmt Kernfeatures von Azure Data Lake Storage Gen1 wie ein Hadoop-kompatibles Dateisystem, Azure Active Directory sowie POSIX-basierte ACLs und integriert sie in Azure Blob Storage. Diese Kombination ermöglicht es Ihnen, die Leistung von Azure Data Lake Storage Gen1 zu nutzen und gleichzeitig das Tiering und die Verwaltung des Datenlebenszyklus von Blob Storage zu verwenden.

## <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Kernfunktionen von Azure Data Lake Storage Gen2

- Hadoop-kompatibler Zugriff: Azure Data Lake Storage Gen2 ermöglicht einen mit einem Hadoop Distributed File System (HDFS) vergleichbaren Zugriff auf Daten sowie deren Verwaltung. Der ABFS-Treiber ist in allen Apache Hadoop-Umgebungen verfügbar, einschließlich Azure HDInsight und Azure Databricks, um auf die in Data Lake Storage Gen2 gespeicherten Daten zuzugreifen.

- Obermenge von POSIX-Berechtigungen: Das Sicherheitsmodell für Data Lake Gen2 unterstützt Zugriffssteuerungslisten und POSIX-Berechtigungen sowie zusätzliche Granularität speziell für Data Lake Storage Gen2. Die Einstellungen können über Verwaltungstools oder Frameworks wie Apache Hive und Apache Spark konfiguriert werden.

- Kosteneffizienz: Data Lake Storage Gen2 bietet kostengünstige Speicherkapazität und -transaktionen. Features wie der Lebenszyklus des Azure-Blobspeichers helfen beim Senken der Kosten, indem sie die Abrechnungsraten anpassen, während die Daten den Lebenszyklus durchlaufen.

- Unterstützung von Blob Storage-Tools, -Frameworks und -Apps: Data Lake Storage Gen2 funktioniert weiterhin mit einer Vielzahl von Tools, Frameworks und Anwendungen, die es heute bereits für Blob Storage gibt.

- Optimierter Treiber: Der ABFS-Treiber ist speziell für Big Data-Analysen optimiert. Die entsprechenden REST-APIs werden über den DFS-Endpunkt (dfs.core.windows.net) bereitgestellt.

## <a name="whats-new-about-azure-data-lake-storage-gen-2"></a>Neuigkeiten zu Azure Data Lake Storage Gen2

### <a name="managed-identities-for-secure-file-access"></a>Verwaltete Identitäten für sicheren Dateizugriff

Azure HDInsight verwendet verwaltete Identitäten, um den Clusterzugriff auf Dateien in Azure Data Lake Storage Gen2 zu sichern. Verwaltete Identitäten sind ein Feature von Azure Active Directory, das Azure-Diensten eine Reihe von automatisch verwalteten Anmeldeinformationen zur Verfügung stellt. Diese Anmeldeinformationen können zur Authentifizierung bei jedem Dienst verwendet werden, der die AD-Authentifizierung unterstützt. Die Verwendung verwalteter Identitäten erfordert nicht, dass Sie Anmeldeinformationen in Code- oder Konfigurationsdateien speichern.

Weitere Informationen finden Sie im unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).

### <a name="azure-blob-filesystem-abfs-driver"></a>Azure-Blobdateisystemtreiber (ABFS)

Apache Hadoop-Anwendungen erwarten nativ, dass sie Daten von lokalen Datenträgerspeichern lesen und auf diese schreiben. Ein Hadoop-Dateisystemtreiber wie ABFS ermöglicht es Hadoop-Anwendungen, mit Cloudspeicher zu arbeiten, indem regelmäßige Hadoop-Dateisystemoperationen für die Anwendung emuliert werden. Der Treiber wandelt dann diese Operationen in Operationen um, die von der eigentlichen Cloudspeicherplattform verstanden werden.

Bisher wurden mit dem Hadoop-Dateisystemtreiber alle Dateisystemoperationen auf der Clientseite in Azure Storage-REST-API-Aufrufe umgewandelt und dann die REST-API aufgerufen. Diese clientseitige Konvertierung führte jedoch zu mehreren REST-API-Aufrufen für einzelne Dateisystemoperationen, z. B. eine Dateiumbenennung. ABFS hat einen Teil der Hadoop-Dateisystemlogik von der Clientseite auf die Serverseite verschoben und die ADLS Gen2-API wird jetzt parallel zur Blob-API ausgeführt. Diese Migration verbessert die Leistung, da jetzt gängige Hadoop-Dateisystemoperationen mit einem REST-API-Aufruf ausgeführt werden können.

Weitere Informationen finden Sie unter [Azure-Blobdateisystemtreiber (ABFS): Ein dedizierter Azure Storage-Treiber für Apache Hadoop](../storage/data-lake-storage/abfs-driver.md).

### <a name="adls-gen-2-uri-scheme"></a>URI-Schema für ADLS Gen 2

ADLS Gen2 verwendet für den Zugriff auf Dateien in Azure-Speicher aus HDInsight ein neues URI-Schema:

`abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

Das URI-Schema bietet SSL-verschlüsselten Zugriff (Präfix `abfss://`) und unverschlüsselten Zugriff (Präfix `abfs://`). Wir empfehlen die Verwendung von `abfss`, und zwar auch für den Zugriff auf Daten, die sich in Azure in derselben Region befinden.

`<FILE_SYSTEM_NAME>` gibt den Pfad des Dateisystems in Data Lake Storage Gen2 an.

`<ACCOUNT_NAME>` gibt den Namen des Azure Storage-Kontos an. Ein vollqualifizierter Domänenname (FQDN) ist erforderlich.

`<PATH>` ist der Pfadname des Hadoop Distributed File System für die Datei oder das Verzeichnis.

Wenn keine Werte für `<FILE_SYSTEM_NAME>` und `<ACCOUNT_NAME>` angegeben sind, wird das Standarddateisystem verwendet. Für die Dateien im Standarddateisystem können Sie relative oder absolute Pfade verwenden. Auf die Datei `hadoop-mapreduce-examples.jar`, die sich in HDInsight-Clustern befindet, kann z. B. mithilfe eines der folgenden Pfade verwiesen werden:

```
abfss://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfss:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> In HDInsight-Clustern der Version 2.1 und 1.6 lautet der Dateiname `hadoop-examples.jar`. Wenn Dateien außerhalb von HDInsight verwendet werden, wird das ABFS-Format von den meisten Hilfsprogrammen nicht erkannt. Diese erwartet vielmehr ein einfaches Pfadformat wie `example/jars/hadoop-mapreduce-examples.jar`.

Weitere Informationen finden Sie unter [Verwenden des Azure Data Lake Storage Gen2-URI](../storage/data-lake-storage/introduction-abfs-uri.md).

## <a name="next-steps"></a>Nächste Schritte
- [Einführung in Azure Data Lake Storage Gen2](../storage/data-lake-storage/introduction.md)
- [Verwenden von Azure Data Lake Storage Gen2 Preview mit Azure HDInsight-Clustern](../storage/data-lake-storage/use-hdi-cluster.md)