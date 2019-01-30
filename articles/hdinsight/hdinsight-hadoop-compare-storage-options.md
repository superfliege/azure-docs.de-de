---
title: Vergleich der Speicheroptionen für die Verwendung mit Azure HDInsight-Clustern
description: Dieser Artikel bietet einen Überblick über die Speichertypen und deren Funktionsweise mit Azure HDInsight.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/20/2019
ms.openlocfilehash: 14db76068cc11d3f57a72e3e540a5e0da7e1c254
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853611"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Vergleich der Speicheroptionen für die Verwendung mit Azure HDInsight-Clustern

Azure HDInsight-Benutzer haben beim Erstellen von HDInsight-Clustern die Auswahl zwischen verschiedenen Speicheroptionen:

* Azure Data Lake Storage Gen2
* Azure Storage
* Azure Data Lake Storage Gen1

Dieser Artikel bietet eine Übersicht über diese verschiedenen Speichertypen und ihre besonderen Merkmale.

## <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Verwenden von Azure Data Lake Storage Gen2 mit Apache Hadoop in Azure HDInsight

Weitere Informationen zu Data Lake Storage Gen2 finden Sie in der [Einführung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md).

Azure Data Lake Storage Gen2 übernimmt Kernfeatures von Azure Data Lake Storage Gen1, z.B. ein Hadoop-kompatibles Dateisystem, Azure Active Directory sowie POSIX-basierte Zugriffssteuerungslisten (ACLs), und integriert diese in Azure Blob Storage. Diese Kombination ermöglicht es Ihnen, die Leistung von Azure Data Lake Storage Gen1 zu nutzen und gleichzeitig das Tiering und die Verwaltung des Datenlebenszyklus von Blob Storage zu verwenden.

### <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Kernfunktionen von Azure Data Lake Storage Gen2

* Hadoop-kompatibler Zugriff: Azure Data Lake Storage Gen2 ermöglicht einen mit einem Hadoop Distributed File System (HDFS) vergleichbaren Zugriff auf Daten sowie deren Verwaltung. Der ABFS-Treiber (Azure Blob Filesystem) ist in allen Apache Hadoop-Umgebungen verfügbar, einschließlich Azure HDInsight und Azure Databricks, um auf die in Data Lake Storage Gen2 gespeicherten Daten zuzugreifen.

* Obermenge von POSIX-Berechtigungen: Das Sicherheitsmodell für Data Lake Gen2 unterstützt Zugriffssteuerungslisten und POSIX-Berechtigungen sowie zusätzliche Granularität speziell für Data Lake Storage Gen2. Die Einstellungen können über Verwaltungstools oder Frameworks wie Apache Hive und Apache Spark konfiguriert werden.

* Kosteneffizienz: Data Lake Storage Gen2 bietet kostengünstige Speicherkapazität und -transaktionen. Features wie der Lebenszyklus des Azure-Blobspeichers helfen beim Senken der Kosten, indem sie die Abrechnungsraten anpassen, während die Daten den Lebenszyklus durchlaufen.

* Unterstützung von Blob Storage-Tools, -Frameworks und -Apps: Data Lake Storage Gen2 funktioniert weiterhin mit einer Vielzahl von Tools, Frameworks und Anwendungen, die es heute bereits für Blob Storage gibt.

* Optimierter Treiber: Der ABFS-Treiber ist speziell für Big Data-Analysen optimiert. Die entsprechenden REST-APIs werden über den DFS-Endpunkt (dfs.core.windows.net) bereitgestellt.

### <a name="whats-new-about-azure-data-lake-storage-gen-2"></a>Neuigkeiten zu Azure Data Lake Storage Gen2

#### <a name="managed-identities-for-secure-file-access"></a>Verwaltete Identitäten für sicheren Dateizugriff

Azure HDInsight verwendet verwaltete Identitäten, um den Clusterzugriff auf Dateien in Azure Data Lake Storage Gen2 zu sichern. Verwaltete Identitäten sind ein Feature von Azure Active Directory, das Azure-Diensten eine Reihe von automatisch verwalteten Anmeldeinformationen zur Verfügung stellt. Diese Anmeldeinformationen können zur Authentifizierung bei jedem Dienst verwendet werden, der die AD-Authentifizierung unterstützt. Die Verwendung verwalteter Identitäten erfordert nicht, dass Sie Anmeldeinformationen in Code- oder Konfigurationsdateien speichern.

Weitere Informationen finden Sie im unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).

#### <a name="azure-blob-filesystem-abfs-driver"></a>Azure-Blobdateisystemtreiber (ABFS)

Apache Hadoop-Anwendungen erwarten nativ, dass sie Daten von lokalen Datenträgerspeichern lesen und auf diese schreiben. Ein Hadoop-Dateisystemtreiber wie ABFS ermöglicht es Hadoop-Anwendungen, mit Cloudspeicher zu arbeiten, indem reguläre Hadoop-Dateisystemvorgänge emuliert werden. Der Treiber wandelt diese anschließend in Vorgänge um, die von der eigentlichen Cloudspeicherplattform interpretiert werden können.

Bisher wurden mit dem Hadoop-Dateisystemtreiber alle Dateisystemvorgänge auf der Clientseite in Azure Storage-REST-API-Aufrufe umgewandelt und dann die REST-API aufgerufen. Diese clientseitige Konvertierung führte jedoch zu mehreren REST-API-Aufrufen für einzelne Dateisystemvorgänge, z.B. eine Dateiumbenennung. ABFS hat einen Teil der Hadoop-Dateisystemlogik von der Clientseite auf die Serverseite verschoben und die Azure Data Lake Storage Gen2-API wird jetzt parallel zur Blob-API ausgeführt. Diese Migration verbessert die Leistung, da jetzt gängige Hadoop-Dateisystemvorgänge mit einem REST-API-Aufruf ausgeführt werden können.

Weitere Informationen finden Sie unter [Azure-Blobdateisystemtreiber (ABFS): Ein dedizierter Azure Storage-Treiber für Hadoop](../storage/blobs/data-lake-storage-abfs-driver.md).

#### <a name="azure-data-lake-storage-gen-2-uri-scheme"></a>URI-Schema von Azure Data Lake Storage Gen 2

Azure Data Lake Storage Gen2 verwendet für den Zugriff auf Dateien in Azure-Speicher aus HDInsight ein neues URI-Schema:

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

> [!Note]
> In HDInsight-Clustern der Version 2.1 und 1.6 lautet der Dateiname `hadoop-examples.jar`. Wenn Dateien außerhalb von HDInsight verwendet werden, wird das ABFS-Format von den meisten Hilfsprogrammen nicht erkannt. Diese erwartet vielmehr ein einfaches Pfadformat wie `example/jars/hadoop-mapreduce-examples.jar`.

Weitere Informationen finden Sie unter [Verwenden des Azure Data Lake Storage Gen2-URI](../storage/blobs/data-lake-storage-introduction-abfs-uri.md).

## <a name="use-azure-storage"></a>Verwenden von Azure Storage

Azure Storage stellt eine robuste, universelle Speicherlösung dar, die problemlos mit HDInsight integriert werden kann. In HDInsight kann ein Blobcontainer in Azure Storage als Standarddateisystem für den Cluster verwendet werden. Über eine HDFS-Schnittstelle (Hadoop Distributed File System) können sämtliche Komponenten in HDInsight direkt mit strukturierten oder unstrukturierten Daten arbeiten, die als Blobs gespeichert sind.

> [!WARNING]  
> Beim Erstellen eines Azure Storage-Kontos stehen Ihnen mehrere Optionen zur Verfügung. Die folgende Tabelle enthält Informationen dazu, welche Optionen mit HDInsight unterstützt werden:

| Speicherkontotyp | Unterstützte Dienste | Unterstützte Leistungsstufen | Unterstützte Zugriffsebenen |
|----------------------|--------------------|-----------------------------|------------------------|
| Allgemein v2   | Blob               | Standard                    | Heiß, Kalt, Archiv*    |
| Allgemein v1   | Blob               | Standard                    | –                    |
| Blob Storage         | Blob               | Standard                    | Heiß, Kalt, Archiv*    |

Die Verwendung des Standard-Blobcontainers zum Speichern von Geschäftsdaten wird nicht empfohlen. Stattdessen empfiehlt es sich, den Standard-Blobcontainer nach jeder Verwendung zu löschen, um die Speicherkosten zu verringern. Der Standardcontainer enthält Anwendungs- und Systemprotokolle. Stellen Sie sicher, dass Sie die Protokolle abrufen, bevor Sie den Container löschen.

Die Verwendung eines Blobcontainers als Standarddateisystem für mehrere Cluster wird nicht unterstützt.
 
 > [!NOTE]  
 > Die Zugriffsebene „Archiv“ ist eine Offlineebene, die eine Abrufwartezeit von mehreren Stunden aufweist, und wird für die Verwendung mit HDInsight nicht empfohlen. Weitere Informationen finden Sie unter [Zugriffsebene „Archiv“](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

### <a name="hdinsight-storage-architecture"></a>HDInsight-Speicherarchitektur
Das folgende Diagramm bietet einen zusammenfassenden Überblick über die HDInsight-Speicherarchitektur bei der Verwendung von Azure Storage:

![Hadoop-Cluster verwenden die HDFS-API, um auf strukturierte und unstrukturierte Daten im Blobspeicher zuzugreifen und diese zu speichern.](./media/hdinsight-hadoop-compare-storage-options/HDI.WASB.Arch.png "HDInsight Storage-Architektur")

HDInsight bietet Zugang zum verteilten Dateisystem, das lokal an die Rechenknoten angefügt ist. Auf dieses Dateisystem kann über den vollständig qualifizierten URI zugegriffen werden. Zum Beispiel:

    hdfs://<namenodehost>/<path>

Zusätzlich bietet HDInsight die Möglichkeit, auf die in Azure Storage gespeicherten Daten zuzugreifen. Die Syntax ist:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

Hier sind einige Aspekte beim Verwenden eines Azure Storage-Kontos mit HDInsight-Clustern.

* **Container in Speicherkonten, die mit einem Cluster verbunden sind:** Da Kontoname und Schlüssel dem Cluster bei der Erstellung zugeordnet werden, haben Sie vollständigen Zugriff auf die Blobs in diesen Containern.

* **Öffentliche Container oder öffentliche Blobs in Speicherkonten, die NICHT mit einem Cluster verbunden sind:** Sie verfügen über Leserechte für die Blobs in den Containern.
  
  > [!NOTE]  
  > Öffentliche Container erlauben das Abrufen einer Liste aller im Container verfügbaren Blobs und der Container-Metadaten. Auf öffentliche Blobs haben Sie nur Zugriff, wenn Sie die exakte URL kennen. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Container und Blobs](../storage/blobs/storage-manage-access-to-resources.md).

* **Private Container in Speicherkonten, die NICHT mit einem Cluster verbunden sind:** Sie können nicht auf die Blobs in den Containern zugreifen, es sei denn, Sie definieren beim Senden der WebHCat-Aufträge das Speicherkonto. Dies wird weiter unten im Artikel erläutert.

Die bei der Erstellung definierten Speicherkonten und ihre Schlüssel werden in der Datei „%HADOOP_HOME%/conf/core-site.xml“ auf den Clusterknoten gespeichert. Standardmäßig verwendet HDInsight die in der Datei core-site.xml definierten Speicherkonten. Sie können diese Einstellung mit [Apache Ambari](./hdinsight-hadoop-manage-ambari.md) ändern.

Verschiedene WebHCat-Aufträge, darunter Apache Hive, MapReduce, Apache Hadoop Streaming und Apache Pig, können eine Beschreibung von Speicherkonten und Metadaten enthalten.  (Dies funktioniert momentan nur für Pig und Speicherkonten, nicht für Metadaten.) Weitere Informationen finden Sie unter [Verwenden eines HDInsight-Clusters mit alternativen Speicherkonten und Metastores](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Blobs können für strukturierte und unstrukturierte Daten verwendet werden. In Blobcontainern werden Daten als Schlüssel-Wert-Paare gespeichert, und es gibt keine Verzeichnishierarchie. Allerdings kann im Schlüsselnamen der Schrägstrich (/) verwendet werden, damit es so aussieht, als wäre eine Datei in einer Verzeichnisstruktur gespeichert. Der Schlüssel eines Blobs kann z.B. `input/log1.txt` lauten. Tatsächlich ist kein Verzeichnis `input` vorhanden, aufgrund des Schrägstrichs (/) im Schlüsselnamen sieht es jedoch so aus, als gäbe es einen Dateipfad.

### <a id="benefits"></a>Vorteile von Azure Storage
Der Leistungsaufwand, der damit verbunden ist, dass die Computecluster und Speicherressourcen nicht an demselben Ort vorliegen, wird dadurch verringert, dass die Computecluster nahe an den Speicherkontoressourcen in der Azure-Region erstellt werden. Hier macht das Hochgeschwindigkeitsnetzwerk den Zugriff auf die Daten im Azure-Speicher für die Serverknoten effizient.

Die Speicherung von Daten im Azure-Speicher anstatt im HDFS hat mehrere Vorteile:

* **Datenwiederverwendung und -freigabe:** Die Daten im HDFS befinden sich innerhalb des Computeclusters. Nur die Anwendungen, die Zugriff auf den Rechencluster haben, können die Daten über die HDFS-API verwenden. Auf die Daten im Azure-Speicher kann entweder über die HDFS-APIs oder über die Blob Storage-REST-APIs zugegriffen werden. Somit kann eine größere Menge von Anwendungen (darunter andere HDInsight-Cluster) und Tools verwendet werden, um die Daten zu produzieren und abzurufen.
* **Datenarchivierung:** Die Speicherung von Daten im Azure-Speicher sorgt dafür, dass die HDInsight-Cluster, die für Berechnungen verwendet werden, sicher gelöscht werden können, ohne Benutzerdaten zu verlieren.
* **Datenspeicherkosten:** Die langfristige Datenspeicherung in DFS ist kostspieliger als die Datenspeicherung im Azure-Speicher, da die Kosten eines Computeclusters höher als die Kosten für Azure-Speicher sind. Da die Daten nicht für jede Erzeugung eines neues Computeclusters neu geladen werden, sparen Sie außerdem Kosten für das Laden von Daten.
* **Elastische horizontale Skalierung:** Auch wenn HDFS ein horizontal skaliertes Dateisystem bietet, wird die Skalierung durch die Anzahl der Knoten bestimmt, die Sie für Ihren Cluster erstellen. Eine Änderung der Skalierung kann weitaus schwieriger werden, als auf die flexiblen Speicherkapazitäten zu vertrauen, die Ihnen der Azure-Speicher automatisch bietet.
* **Georeplikation:** Für den Azure-Speicher kann eine Georeplikation durchgeführt werden. Obwohl Sie dadurch von geographischer Wiederherstellung und Datenredundanz profitieren, wirkt sich ein Ausfall des georeplizierten Standorts schwer auf Ihre Leistung aus und kann zusätzliche Kosten nach sich ziehen. Deshalb empfehlen wir, die Georeplikation mit Bedacht auszuwählen und nur dann anzuwenden, wenn der Wert der Daten die zusätzlichen Kosten rechtfertigt.

Bestimmte MapReduce-Aufträge und -Pakete können zu Zwischenergebnissen führen, die Sie eher nicht im Azure-Speicher speichern möchten. In diesem Fall können Sie die Dateien auch im lokalen HDFS speichern. Tatsächlich verwendet HDInsight DFS für einige dieser Zwischenergebnisse in Hive-Jobs und anderen Prozessen.

> [!NOTE]  
> Die meisten HDFS-Befehle (z.B. `ls`, `copyFromLocal` und `mkdir`) funktionieren weiterhin wie erwartet. Nur die für die native (als DFS bezeichnete) HDFS-Implementierung spezifischen Befehle wie `fschk` und `dfsadmin` zeigen in Azure Storage ein anderes Verhalten.

## <a name="use-azure-data-lake-storage-gen1"></a>Verwenden von Azure Data Lake Storage Gen1

Weitere Informationen zu Data Lake Storage Gen1 finden Sie unter [Übersicht über Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md).

Azure Data Lake Storage Gen1 ist ein unternehmensweites Repository mit Hyperskalierung für Big Data-Analyseworkloads. Azure Data Lake bietet Ihnen die Möglichkeit, Daten von beliebiger Größe, Art und Erfassungsgeschwindigkeit zur Durchführung operativer und explorativer Analysen an einem einzigen Ort zu erfassen.

Auf Data Lake Storage Gen1 kann über Hadoop (verfügbar mit einem HDInsight-Cluster) mithilfe der WebHDFS-kompatiblen REST-APIs zugegriffen werden. Dieser Speichertyp wurde speziell für Analysen der gespeicherten Daten konzipiert und ist für eine hohe Leistung in Datenanalyseszenarien optimiert. Er enthält standardmäßig all die Funktionen auf Unternehmensniveau, die für reale Anwendungsfälle von Unternehmen erforderlich sind – Sicherheit, Verwaltbarkeit, Skalierbarkeit, Zuverlässigkeit und Verfügbarkeit.

![Azure Data Lake Storage](./media/hdinsight-hadoop-compare-storage-options/data-lake-store-concept.png "HDInsight Storage-Architektur")

Data Lake Storage Gen1 bietet u.a. die folgenden wichtigen Funktionen.

### <a name="built-for-hadoop"></a>Konzipiert für Hadoop

Bei Data Lake Storage Gen1 handelt es sich um ein Apache Hadoop-Dateisystem, das mit dem HDFS (Hadoop Distributed File System) kompatibel ist und mit dem Hadoop-Ökosystem eingesetzt werden kann.  Ihre vorhandenen HDInsight-Anwendungen oder -Dienste, die die WebHDFS-API verwenden, können problemlos in Data Lake Storage Gen1 integriert werden. Data Lake Storage Gen1 macht auch eine WebHDFS-kompatible REST-Schnittstelle für Anwendungen verfügbar.

In Data Lake Storage Gen1 gespeicherte Daten können mühelos mit Hadoop-Analyseframeworks wie MapReduce oder Hive analysiert werden. Microsoft Azure HDInsight-Cluster können für den direkten Zugriff auf in Data Lake Storage Gen1 gespeicherte Daten bereitgestellt und konfiguriert werden.

### <a name="unlimited-storage-petabyte-files"></a>Unbegrenzter Speicher, Dateigrößen bis in den Petabytebereich

Data Lake Storage Gen1 bietet unbegrenzten Speicher und eignet sich zum Speichern unterschiedlichster Daten für Analysezwecke. Es gelten keinerlei Einschränkungen für Kontogrößen, Dateigrößen oder die Menge an Daten, die in einem Data Lake gespeichert werden kann. Data Lake-Speicher unterstützt Dateigrößen vom Kilobyte- bis in den Petabytebereich und ist somit eine gute Wahl für die Speicherung von Daten jeglicher Art. Daten werden dauerhaft gespeichert, indem mehrere Kopien erstellt werden, und können für unbegrenzte Zeit im Data Lake gespeichert werden.

### <a name="performance-tuned-for-big-data-analytics"></a>Leistungsoptimiert für Big Data-Analysen

Data Lake Storage Gen1 ist für die Ausführung großer Analysesysteme ausgelegt, die zum Abfragen und Analysieren großer Datenmengen einen enormen Durchsatz erfordern. Der Data Lake verteilt Teile einer Datei auf mehrere einzelne Speicherserver. Dies verbessert den Lesedurchsatz, wenn die Datei zum Ausführen von Datenanalysen parallel gelesen wird.

### <a name="enterprise-ready-highly-available-and-secure"></a>Speicher auf Unternehmensniveau: Hoch verfügbar und sicher

Data Lake Storage Gen1 bietet Verfügbarkeit und Zuverlässigkeit nach Branchenstandard. Ihre Datenassets werden dauerhaft gespeichert, indem zum Schutz vor unerwarteten Fehlern redundante Kopien erstellt werden. Unternehmen können Data Lake Storage Gen1 in ihren Lösungen als wesentlichen Bestandteil ihrer vorhandenen Datenplattform einsetzen.

Data Lake Storage Gen1 bietet außerdem Sicherheit auf Unternehmensniveau für die gespeicherten Daten. Weitere Informationen finden Sie unter [Schützen von Daten in Data Lake Storage Gen1](#DataLakeStoreSecurity).

### <a name="all-data"></a>Alle Daten

Data Lake Storage Gen1 kann beliebige Daten ohne vorherige Transformation im nativen Format speichern. Bei Data Lake Storage Gen1 ist es nicht erforderlich, vor dem Laden der Daten ein Schema zu definieren. Die Interpretation der Daten und Definition eines Schemas erfolgt zum Zeitpunkt der Analyse durch die einzelnen Analyseframeworks. Dank der Möglichkeit, Dateien beliebiger Größe und Formate zu speichern, kann Data Lake Storage Gen1 strukturierte, teilweise strukturierte und unstrukturierte Daten verwalten.

Data Lake Storage Gen1-Container für Daten sind im Wesentlichen Ordner und Dateien. Sie arbeiten mit den gespeicherten Daten über SDKs, das Azure-Portal und Azure PowerShell. Solange Sie Ihre Daten unter Verwendung dieser Schnittstellen im Speicher ablegen und die entsprechenden Container verwenden, können Sie jeden Datentyp speichern. Data Lake Storage Gen1 führt keine spezielle Verarbeitung von Daten basierend auf dem Typ der gespeicherten Daten durch.

## <a name="DataLakeStoreSecurity"></a>Schützen von Daten in Data Lake Storage Gen1
Data Lake Storage Gen1 verwendet Azure Active Directory zur Authentifizierung und Zugriffssteuerungslisten (ACLs) zum Verwalten des Zugriffs auf Ihre Daten.

| Feature | BESCHREIBUNG |
| --- | --- |
| Authentifizierung |Zur Identitäts- und Zugriffsverwaltung für alle in Data Lake Storage Gen1 gespeicherten Daten ist Data Lake Storage Gen1 in Azure Active Directory (AAD) integriert. Dank dieser Integration nutzt Data Lake Storage Gen1 sämtliche AAD-Features, z.B. mehrstufige Authentifizierung, bedingter Zugriff, rollenbasierte Zugriffssteuerung, Überwachung der Anwendungsnutzung, Sicherheitsüberwachung und -warnungen. Data Lake Storage Gen1 unterstützt das OAuth 2.0-Protokoll für die Authentifizierung mit der REST-Schnittstelle. Weitere Informationen finden Sie unter [Authentifizierung bei Azure Data Lake Storage Gen1 mit Azure Active Directory](../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md).|
| Zugriffssteuerung |Data Lake Storage Gen1 ermöglicht eine Zugriffssteuerung durch die Unterstützung von POSIX-Berechtigungen, die vom WebHDFS-Protokoll verfügbar gemacht werden. ACLs können für Stammordner, Unterordner und einzelne Dateien aktiviert werden. Weitere Informationen zur Funktionsweise von ACLs im Kontext von Data Lake Storage Gen1 finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md). |
| Verschlüsselung |Data Lake Storage Gen1 bietet auch eine Verschlüsselung für Daten, die im Konto gespeichert sind. Beim Erstellen eines Data Lake Storage Gen1-Kontos geben Sie die Einstellungen für die Verschlüsselung an. Sie können auswählen, ob Ihre Daten verschlüsselt werden sollen oder nicht. Weitere Informationen finden Sie unter [Verschlüsselung von Daten in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-encryption.md). Anweisungen zur Bereitstellung einer verschlüsselungsbezogenen Konfiguration finden Sie unter [Erste Schritte mit Azure Data Lake Storage Gen1 im Azure-Portal](../data-lake-store/data-lake-store-get-started-portal.md). |

Möchten Sie mehr über den Schutz von Daten in Data Lake Storage Gen1 erfahren? finden Sie unter folgenden Links:

* Anweisungen zum Schutz von Daten in Data Lake Storage Gen1 finden Sie unter [Schutz von Daten in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-secure-data.md).

## <a name="applications-compatible-with-data-lake-storage-gen1"></a>Mit Data Lake Storage Gen1 kompatible Anwendungen
Data Lake Storage Gen1 ist mit den meisten Open Source-Komponenten im Hadoop-Ökosystem kompatibel. Außerdem ist eine problemlose Integration in andere Azure-Dienste möglich.  Über die folgenden Links erhalten Sie weitere Informationen zur Verwendung von Data Lake Storage Gen1 mit Open Source-Komponenten und anderen Azure-Diensten.

* Eine Liste der mit Data Lake Storage Gen1 interoperablen Open Source-Anwendungen finden Sie unter [Open Source-Big Data-Anwendungen, die mit Azure Data Lake Storage Gen1 verwendet werden können](../data-lake-store/data-lake-store-compatible-oss-other-applications.md).
* Informationen dazu, wie Data Lake Storage Gen1 mit anderen Azure-Diensten verwendet werden kann, um ein breiteres Spektrum von Szenarien zu ermöglichen, finden Sie unter [Integration von Azure Data Lake Storage Gen1 in andere Azure-Dienste](../data-lake-store/data-lake-store-integrate-with-other-services.md).
* Informationen dazu, wie Data Lake Storage Gen1 beispielsweise zum Erfassen, Verarbeiten, Herunterladen und Visualisieren von Daten verwendet werden kann, finden Sie unter [Verwenden von Azure Data Lake Storage Gen1 zur Erfüllung von Big Data-Anforderungen](../data-lake-store/data-lake-store-data-scenarios.md).

## <a name="what-is-data-lake-storage-gen1-file-system-adl"></a>Was ist das Azure Data Lake Storage Gen1-Dateisystem (adl://)?
In Hadoop-Umgebungen (verfügbar mit HDInsight-Cluster) kann über das neue Dateisystem „AzureDataLakeFilesystem“ (adl://) auf Data Lake Storage Gen1 zugegriffen werden. Anwendungen und Dienste, die adl:// verwenden, können weitere Leistungsoptimierungen nutzen, die gegenwärtig nicht in WebHDFS verfügbar sind. Daher bietet Ihnen Data Lake Storage Gen1 die Flexibilität, entweder mit adl:// (empfohlene Option) die optimale Leistung zu nutzen oder vorhandenen Code beizubehalten, indem Sie die WebHDFS-API weiterhin direkt verwenden. Azure HDInsight schöpft die Möglichkeiten des „AzureDataLakeFilesystem“ voll aus, um eine optimale Leistung für Data Lake Storage Gen1 bereitzustellen.

Sie können mithilfe von `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net` auf Ihre Daten in Data Lake Storage Gen1 zugreifen. Weitere Informationen zum Zugriff auf die Daten in Data Lake Storage Gen1 finden Sie unter [Anzeigen der Eigenschaften der gespeicherten Daten](../data-lake-store/data-lake-store-get-started-portal.md#properties).



## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
* [Einführung in Azure Storage](../storage/common/storage-introduction.md)