---
title: Übersicht über Azure Data Lake Storage Gen1 | Microsoft-Dokumentation
description: Hier erfahren Sie, was Data Lake Storage Gen1 (ehemals Azure Data Lake Store) ist und welche Vorteile es im Vergleich zu anderen Datenspeichern bietet.
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: 4dff8f4ff9fc324d48391c0399677b64824493c6
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37034887"
---
# <a name="overview-of-azure-data-lake-storage-gen1"></a>Übersicht über Azure Data Lake Storage Gen1

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake-Speicher ist ein unternehmensweites riesiges Repository für Big Data-Analyseworkloads. Azure Data Lake bietet Ihnen die Möglichkeit, Daten von beliebiger Größe, Art und Erfassungsgeschwindigkeit zur Durchführung operativer und explorativer Analysen an einem einzigen Ort zu erfassen.

> [!TIP]
> Verwenden Sie den [Lernpfad für Data Lake-Speicher](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) , um den Azure Data Lake-Speicherdienst zu erkunden.
> 
> 

Auf Azure Data Lake-Speicher kann über Hadoop (verfügbar mit HDInsight-Clustern) mithilfe der WebHDFS-kompatiblen REST-APIs zugegriffen werden. Er wurde speziell für Analysen der gespeicherten Daten konzipiert und für Datenanalyseszenarien leistungsoptimiert. Er enthält standardmäßig all die Funktionen auf Unternehmensniveau, die für reale Anwendungsfälle von Unternehmen erforderlich sind – Sicherheit, Verwaltbarkeit, Skalierbarkeit, Zuverlässigkeit und Verfügbarkeit.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Azure Data Lake bietet u. a. die folgenden wichtigen Funktionen.

### <a name="built-for-hadoop"></a>Konzipiert für Hadoop
Beim Azure Data Lake-Speicher handelt es sich um ein Apache Hadoop-Dateisystem, das mit HDFS (Hadoop Distributed File System) kompatibel ist und mit dem Hadoop-Ökosystem eingesetzt werden kann.  Ihre vorhandenen HDInsight-Anwendungen oder -Dienste, die die WebHDFS-API verwenden, können problemlos in Data Lake-Speicher integriert werden. Data Lake-Speicher macht auch eine WebHDFS-kompatible REST-Schnittstelle für Anwendungen verfügbar.

In Data Lake-Speicher gespeicherte Daten können mühelos mit Hadoop-Analyseframeworks wie MapReduce oder Hive analysiert werden. Microsoft Azure HDInsight-Cluster können für den direkten Zugriff auf in Data Lake-Speicher gespeicherte Daten bereitgestellt und konfiguriert werden.

### <a name="unlimited-storage-petabyte-files"></a>Unbegrenzter Speicher, Dateigrößen bis in den Petabytebereich
Azure Data Lake-Speicher bietet unbegrenzten Speicher und eignet sich zum Speichern unterschiedlichster Daten für Analysezwecke. Es gelten keinerlei Einschränkungen für Kontogrößen, Dateigrößen oder die Menge an Daten, die in einem Data Lake gespeichert werden kann. Data Lake-Speicher unterstützt Dateigrößen vom Kilobyte- bis in den Petabytebereich und ist somit eine gute Wahl für die Speicherung von Daten jeglicher Art. Daten werden dauerhaft gespeichert, indem mehrere Kopien erstellt werden, und können für unbegrenzte Zeit im Data Lake gespeichert werden.

### <a name="performance-tuned-for-big-data-analytics"></a>Leistungsoptimiert für Big Data-Analysen
Azure Data Lake-Speicher ist für die Ausführung großer Analysesysteme ausgelegt, die zum Abfragen und Analysieren großer Datenmengen einen enormen Durchsatz erfordern. Der Data Lake verteilt Teile einer Datei auf mehrere einzelne Speicherserver. Dies verbessert den Lesedurchsatz, wenn die Datei zum Ausführen von Datenanalysen parallel gelesen wird.

### <a name="enterprise-ready-highly-available-and-secure"></a>Geeignet für Unternehmen: Hoch verfügbar und sicher
Azure Data Lake-Speicher bietet Verfügbarkeit und Zuverlässigkeit nach Industriestandard. Ihre Datenassets werden dauerhaft gespeichert, indem zum Schutz vor unerwarteten Fehlern redundante Kopien erstellt werden. Unternehmen können Azure Data Lake in ihren Lösungen als wesentlichen Bestandteil ihrer vorhandenen Datenplattform einsetzen.

Data Lake-Speicher bietet außerdem Sicherheit auf Unternehmensniveau für die gespeicherten Daten. Weitere Informationen finden Sie unter [Sichern von Daten in Azure Data Lake-Speicher](#DataLakeStoreSecurity).

### <a name="all-data"></a>Alle Daten
Azure Data Lake-Speicher kann beliebige Daten ohne vorherige Transformation im systemeigenen Format speichern. Bei Data Lake-Speicher ist es nicht erforderlich, vor dem Laden der Daten ein Schema zu definieren. Die Interpretation der Daten und Definition eines Schemas erfolgt zum Zeitpunkt der Analyse durch die einzelnen Analyseframeworks. Dank der Möglichkeit, Dateien beliebiger Größe und Formate zu speichern, kann Data Lake-Speicher strukturierte, teilweise strukturierte und unstrukturierte Daten verwalten.

Azure Data Lake-Speichercontainer für Daten sind im Wesentlichen Ordner und Dateien. Sie arbeiten mit den gespeicherten Daten mithilfe von SDKs, des Azure-Portals und von Azure PowerShell. Solange Sie Ihre Daten unter Verwendung dieser Schnittstellen im Speicher ablegen und die entsprechenden Container verwenden, können Sie jeden Datentyp speichern. Data Lake-Speicher führt keine spezielle Verarbeitung von Daten basierend auf dem Typ der gespeicherten Daten durch.

## <a name="DataLakeStoreSecurity"></a>Sichern von Daten in Azure Data Lake-Speicher
Azure Data Lake-Speicher verwendet Azure Active Directory zur Authentifizierung und Zugriffssteuerungslisten (ACLs) zum Verwalten des Zugriffs auf Ihre Daten.

| Feature | BESCHREIBUNG |
| --- | --- |
| Authentifizierung |Zur Identitäts- und Zugriffsverwaltung für alle in Azure Data Lake-Speicher gespeicherten Daten ist Azure Data Lake-Speicher in Azure Active Directory (AAD) integriert. Dank dieser Integration nutzt Azure Data Lake sämtliche AAD-Funktionen, z. B. Multi-Factor Authentication, bedingter Zugriff, rollenbasierte Zugriffssteuerung, Überwachung der Anwendungsnutzung, Sicherheitsüberwachung und -warnungen usw. Azure Data Lake-Speicher unterstützt das OAuth 2.0-Protokoll für die Authentifizierung mit der REST-Schnittstelle. Weitere Informationen finden Sie unter [Authentication with Data Lake Store using Azure Active Directory](data-lakes-store-authentication-using-azure-active-directory.md) (Authentifizierung mit Data Lake Store per Azure Active Directory).|
| Zugriffssteuerung |Azure Data Lake-Speicher ermöglicht eine Zugriffssteuerung durch die Unterstützung von POSIX-Berechtigungen, die vom WebHDFS-Protokoll verfügbar gemacht werden. ACLs können für Stammordner, Unterordner und einzelne Dateien aktiviert werden. Weitere Informationen zur Funktionsweise von ACLs im Kontext von Data Lake Store finden Sie unter [Zugriffssteuerung in Data Lake Store](data-lake-store-access-control.md). |
| Verschlüsselung |Data Lake Store bietet auch eine Verschlüsselung für Daten, die im Konto gespeichert sind. Beim Erstellen eines Data Lake Store-Kontos geben Sie die Einstellungen für die Verschlüsselung an. Sie können auswählen, ob Ihre Daten verschlüsselt werden sollen oder nicht. Weitere Informationen finden Sie unter [Encryption in Data Lake Store](data-lake-store-encryption.md) (Verschlüsselung in Data Lake Store). Eine Anleitung zur Bereitstellung einer verschlüsselungsbezogenen Konfiguration finden Sie unter [Erste Schritte mit Azure Data Lake Store im Azure-Portal](data-lake-store-get-started-portal.md). |

Weitere Informationen zum Schützen von Daten im Data Lake-Speicher finden Sie unter folgenden Links:

* Anweisungen zum Sichern von Daten in Data Lake-Speicher finden Sie unter [Sichern von Daten in Azure Data Lake-Speicher](data-lake-store-secure-data.md).
* Bevorzugen Sie Videos? [dieses Video](https://mix.office.com/watch/1q2mgzh9nn5lx) über das Schützen von im Data Lake-Speicher gespeicherten Daten an.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Mit Azure Data Lake-Speicher kompatible Anwendungen
Azure Data Lake-Speicher ist mit den meisten Open Source-Komponenten des Hadoop-Systems kompatibel. Außerdem ist eine problemlose Integration in andere Azure-Dienste möglich. Das macht Data Lake-Speicher zur optimalen Lösung für Ihre Datenspeicheranforderungen. Unter den folgenden Links finden Sie weitere Informationen zur Verwendung von Data Lake-Speicher mit Open Source-Komponenten und anderen Azure-Diensten:

* Eine Liste der mit Data Lake-Speicher interoperablen Open-Source-Anwendungen finden Sie unter [Mit Azure Data Lake-Speicher kompatible Anwendungen und Dienste](data-lake-store-compatible-oss-other-applications.md) .
* Informationen dazu, wie Data Lake-Speicher mit anderen Azure-Diensten verwendet werden kann, um ein breiteres Spektrum von Szenarien zu ermöglichen, finden Sie unter [Integration in andere Azure-Dienste](data-lake-store-integrate-with-other-services.md) .
* Informationen dazu, wie Data Lake-Speicher beispielsweise zum Erfassen von Daten, zum Verarbeiten von Daten, zum Herunterladen von Daten und zum Visualisieren von Daten verwendet werden kann, finden Sie unter [Datenszenarien im Zusammenhang mit dem Azure Data Lake-Speicher](data-lake-store-data-scenarios.md) .

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Was ist das Azure Data Lake Store-Dateisystem (adl://)?
In Hadoop-Umgebungen (verfügbar mit HDInsight-Clustern) kann über das neue Dateisystem „AzureDataLakeFilesystem“ (adl://) auf Data Lake Store zugegriffen werden. Anwendungen und Dienste, die adl:// verwenden, können weitere Leistungsoptimierungen nutzen, die gegenwärtig nicht in WebHDFS verfügbar sind. Daher bietet Ihnen Data Lake-Speicher die Flexibilität, entweder mit adl:// (empfohlene Option) die optimale Leistung zu nutzen oder vorhandenen Code beizubehalten, indem Sie die WebHDFS-API weiterhin direkt verwenden. Azure HDInsight schöpft die Möglichkeiten des „AzureDataLakeFilesystem“ voll aus, um eine optimale Leistung für Data Lake-Speicher bereitzustellen.

Sie können mithilfe von `adl://<data_lake_store_name>.azuredatalakestore.net`auf Ihre Daten im Data Lake-Speicher zugreifen. Weitere Informationen zum Zugriff auf die Daten im Data Lake-Speicher finden Sie unter [Anzeigen der Eigenschaften der gespeicherten Daten](data-lake-store-get-started-portal.md#properties)

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Data Lake Storage Gen1 unter Verwendung des Azure-Portals](data-lake-store-get-started-portal.md)
* [Erste Schritte mit Azure Data Lake-Speicher mithilfe des .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)