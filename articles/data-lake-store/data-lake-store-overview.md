---
title: Übersicht über Azure Data Lake Storage Gen1 | Microsoft-Dokumentation
description: Hier erfahren Sie, was Data Lake Storage Gen1 (ehemals Azure Data Lake Store) ist und welche Vorteile es im Vergleich zu anderen Datenspeichern bietet.
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 438eab091fac103b66f0789beca0098b87ee44cd
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885654"
---
# <a name="overview-of-azure-data-lake-storage-gen1"></a>Übersicht über Azure Data Lake Storage Gen1

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake Storage Gen1 ist ein unternehmensweites Repository mit Hyperskalierung für Big Data-Analyseworkloads. Azure Data Lake bietet Ihnen die Möglichkeit, Daten von beliebiger Größe, Art und Erfassungsgeschwindigkeit zur Durchführung operativer und explorativer Analysen an einem einzigen Ort zu erfassen.

> [!TIP]
> Verwenden Sie die [Dokumentation zu Azure Data Lake Storage Gen1](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/), um den Data Lake Storage Gen1-Dienst zu erkunden.
> 
> 

Auf Data Lake Storage Gen1 kann über Hadoop (verfügbar mit HDInsight-Cluster) mithilfe der WebHDFS-kompatiblen REST-APIs zugegriffen werden. Er wurde speziell für Analysen der gespeicherten Daten konzipiert und für Datenanalyseszenarien leistungsoptimiert. Er enthält standardmäßig all die Funktionen auf Unternehmensniveau, die für reale Anwendungsfälle von Unternehmen erforderlich sind – Sicherheit, Verwaltbarkeit, Skalierbarkeit, Zuverlässigkeit und Verfügbarkeit.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

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

Data Lake Storage Gen1-Container für Daten sind im Wesentlichen Ordner und Dateien. Sie arbeiten mit den gespeicherten Daten mithilfe von SDKs, des Azure-Portals und von Azure PowerShell. Solange Sie Ihre Daten unter Verwendung dieser Schnittstellen im Speicher ablegen und die entsprechenden Container verwenden, können Sie jeden Datentyp speichern. Data Lake Storage Gen1 führt keine spezielle Verarbeitung von Daten basierend auf dem Typ der gespeicherten Daten durch.

## <a name="DataLakeStoreSecurity"></a>Schützen von Daten in Data Lake Storage Gen1
Data Lake Storage Gen1 verwendet Azure Active Directory zur Authentifizierung und Zugriffssteuerungslisten (ACLs) zum Verwalten des Zugriffs auf Ihre Daten.

| Feature | BESCHREIBUNG |
| --- | --- |
| Authentication |Zur Identitäts- und Zugriffsverwaltung für alle in Data Lake Storage Gen1 gespeicherten Daten ist Data Lake Storage Gen1 in Azure Active Directory (AAD) integriert. Dank dieser Integration nutzt Data Lake Storage Gen1 sämtliche AAD-Features, z.B. mehrstufige Authentifizierung, bedingter Zugriff, rollenbasierte Zugriffssteuerung, Überwachung der Anwendungsnutzung, Sicherheitsüberwachung und -warnungen. Data Lake Storage Gen1 unterstützt das OAuth 2.0-Protokoll für die Authentifizierung mit der REST-Schnittstelle. Weitere Informationen finden Sie unter [Authentifizierung bei Azure Data Lake Storage Gen1 mit Azure Active Directory](data-lakes-store-authentication-using-azure-active-directory.md).|
| Zugriffssteuerung |Data Lake Storage Gen1 ermöglicht eine Zugriffssteuerung durch die Unterstützung von POSIX-Berechtigungen, die vom WebHDFS-Protokoll verfügbar gemacht werden. ACLs können für Stammordner, Unterordner und einzelne Dateien aktiviert werden. Weitere Informationen zur Funktionsweise von ACLs im Kontext von Data Lake Storage Gen1 finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen1](data-lake-store-access-control.md). |
| Verschlüsselung |Data Lake Storage Gen1 bietet auch eine Verschlüsselung für Daten, die im Konto gespeichert sind. Beim Erstellen eines Data Lake Storage Gen1-Kontos geben Sie die Einstellungen für die Verschlüsselung an. Sie können auswählen, ob Ihre Daten verschlüsselt werden sollen oder nicht. Weitere Informationen finden Sie unter [Verschlüsselung von Daten in Azure Data Lake Storage Gen1](data-lake-store-encryption.md). Eine Anleitung zur Bereitstellung einer verschlüsselungsbezogenen Konfiguration finden Sie unter [Erste Schritte mit Azure Data Lake Storage Gen1 im Azure-Portal](data-lake-store-get-started-portal.md). |

Möchten Sie mehr über den Schutz von Daten in Data Lake Storage Gen1 erfahren? finden Sie unter folgenden Links:

* Anweisungen zum Schutz von Daten in Data Lake Storage Gen1 finden Sie unter [Schutz von Daten in Azure Data Lake Storage Gen1](data-lake-store-secure-data.md).
* Bevorzugen Sie Videos? Sehen Sie sich [dieses Video](https://mix.office.com/watch/1q2mgzh9nn5lx) zum Schutz von in Data Lake Storage Gen1 gespeicherten Daten an.

## <a name="applications-compatible-with-data-lake-storage-gen1"></a>Mit Data Lake Storage Gen1 kompatible Anwendungen
Data Lake Storage Gen1 ist mit den meisten Open Source-Komponenten des Hadoop-Ökosystems kompatibel. Außerdem ist eine problemlose Integration in andere Azure-Dienste möglich. Das macht Data Lake Storage Gen1 zur optimalen Lösung für Ihre Datenspeicheranforderungen. Unter den folgenden Links finden Sie weitere Informationen zur Verwendung von Data Lake Storage Gen1 mit Open Source-Komponenten und anderen Azure-Diensten:

* Eine Liste der mit Data Lake Storage Gen1 interoperablen Open Source-Anwendungen finden Sie unter [Mit Azure Data Lake Storage Gen1 kompatible, Open Source-basierte Big Data-Anwendungen](data-lake-store-compatible-oss-other-applications.md).
* Informationen dazu, wie Data Lake Storage Gen1 mit anderen Azure-Diensten verwendet werden kann, um ein breiteres Spektrum von Szenarien zu ermöglichen, finden Sie unter [Integration von Azure Data Lake Storage Gen1 in andere Azure-Dienste](data-lake-store-integrate-with-other-services.md).
* Informationen dazu, wie Data Lake Storage Gen1 beispielsweise zum Erfassen, Verarbeiten, Herunterladen und Visualisieren von Daten verwendet werden kann, finden Sie unter [Verwenden von Azure Data Lake Storage Gen1 zur Erfüllung von Big Data-Anforderungen](data-lake-store-data-scenarios.md).

## <a name="what-is-data-lake-storage-gen1-file-system-adl"></a>Was ist das Azure Data Lake Storage Gen1-Dateisystem (adl://)?
In Hadoop-Umgebungen (verfügbar mit HDInsight-Cluster) kann über das neue Dateisystem „AzureDataLakeFilesystem“ (adl://) auf Data Lake Storage Gen1 zugegriffen werden. Anwendungen und Dienste, die adl:// verwenden, können weitere Leistungsoptimierungen nutzen, die gegenwärtig nicht in WebHDFS verfügbar sind. Daher bietet Ihnen Data Lake Storage Gen1 die Flexibilität, entweder mit adl:// (empfohlene Option) die optimale Leistung zu nutzen oder vorhandenen Code beizubehalten, indem Sie die WebHDFS-API weiterhin direkt verwenden. Azure HDInsight schöpft die Möglichkeiten des „AzureDataLakeFilesystem“ voll aus, um eine optimale Leistung für Data Lake Storage Gen1 bereitzustellen.

Sie können mithilfe von `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net` auf Ihre Daten in Data Lake Storage Gen1 zugreifen. Weitere Informationen zum Zugriff auf die Daten in Data Lake Storage Gen1 finden Sie unter [Anzeigen der Eigenschaften der gespeicherten Daten](data-lake-store-get-started-portal.md#properties).

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Data Lake Storage Gen1 unter Verwendung des Azure-Portals](data-lake-store-get-started-portal.md)
* [Erste Schritte mit Azure Data Lake Storage Gen1 mit dem .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Verwenden von Azure HDInsight mit Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)