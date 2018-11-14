---
title: 'Azure Data Lake Storage Gen2 Preview: Einführung'
description: Übersicht über Azure Data Lake Storage Gen2 Preview
services: storage
author: jamesbak
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: c86609ae5b993328beced468b74c7f2a1b65def4
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283613"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Einführung in Azure Data Lake Storage Gen2 Preview

Azure Data Lake Storage Gen2 (Vorschau) setzt auf [Azure Blob Storage](../blobs/storage-blobs-introduction.md) auf und bietet eine Reihe von Funktionen für die Big Data-Analyse. Er ermöglicht Ihnen die Arbeit mit Ihren Daten über das Dateisystem und den Objektspeicher. Durch die Aufnahme von Data Lake Storage Gen2 ist Azure Storage die einzige cloudbasierte Plattform mit mehreren Modi, die das Extrahieren von Analysewerten aus all Ihren Daten ermöglicht.

Data Lake Storage Gen2 stattet Azure Storage mit allen Eigenschaften aus, die im gesamten Lebenszyklus von Analysedaten benötigt werden. Hierfür werden die Funktionen unserer beiden bestehenden Speicherdienste Azure Blob Storage und Azure Data Lake Storage Gen1 zusammengeführt. Features von [Azure Data Lake Storage Gen1](../../data-lake-store/index.md), z.B. Dateisystemsemantik, Sicherheit und Skalierbarkeit auf Dateiebene, werden mit den kostengünstigen, mehrstufigen Speicherlösungen, Hochverfügbarkeits- und Notfallwiederherstellungsfunktionen von [Azure Blob Storage](../blobs/storage-blobs-introduction.md) kombiniert.

## <a name="designed-for-enterprise-big-data-analytics"></a>Für Big Data-Analysen in Großunternehmen entwickelt

Mit Data Lake Storage Gen2 wird Azure Storage zur Grundlage für den Aufbau von Enterprise Data Lakes in Azure. Data Lake Storage Gen2 wurde eigens für die Verarbeitung mehrerer Petabyte an Informationen bei gleichzeitiger Unterstützung eines Durchsatzes von Hunderten von Gigabit konzipiert und bietet Ihnen eine einfache Möglichkeit, riesige Datenmengen zu verwalten.

Ein wesentlicher Teil von Data Lake Storage Gen2 ist der neue [hierarchische Namespace](./namespace.md) für den Blob Storage-Dienst, der Objekte und Dateien für einen leistungsstarken Datenzugriff in einer Verzeichnishierarchie organisiert. Der hierarchische Namespace ermöglicht Data Lake Storage Gen2 auch die gleichzeitige Unterstützung von Objektspeicher- und Dateisystemstrukturen. Beispielsweise verwendet eine allgemeine Namenskonvention für die Objektspeicherung Schrägstriche im Namen, um eine hierarchische Ordnerstruktur nachzuahmen. Mit Data Lake Storage Gen2 wird diese Struktur Wirklichkeit. Vorgänge wie das Umbenennen oder Löschen eines Verzeichnisses werden zu einzelnen atomaren Metadatenvorgängen im Verzeichnis, anstatt alle Objekte aufzuzählen und zu verarbeiten, die das Namenspräfix des Verzeichnisses gemeinsam haben.

In der Vergangenheit mussten bei cloudbasierten Analysen Kompromisse in puncto Leistung, Verwaltung und Sicherheit eingegangen werden. Data Lake Storage Gen2 geht jeden dieser Aspekte auf folgende Weise an:

- Die **Leistung** ist optimiert, da Sie keine Daten als Voraussetzung für die Analyse kopieren oder transformieren müssen. Der hierarchische Namespace verbessert die Leistung von Verzeichnisverwaltungsvorgängen erheblich, wodurch die gesamte Auftragsverarbeitungsleistung gesteigert wird.

- Die **Verwaltung** ist einfacher, weil Sie Dateien mithilfe von Verzeichnissen und Unterverzeichnissen organisieren und bearbeiten können.

- Die **Sicherheit** ist durchsetzbar, da Sie POSIX-Berechtigungen für Ordner oder einzelne Dateien definieren können.

- **Kosteneffizienz** wird ermöglicht, da Data Lake Storage Gen2 auf kostengünstigem [Azure Blob Storage](../blobs/storage-blobs-introduction.md) aufsetzt. Durch diese zusätzlichen Features werden die Gesamtbetriebskosten für Big Data-Analysen in Azure weiter gesenkt.

## <a name="key-features-of-data-lake-storage-gen2"></a>Besondere Features von Data Lake Storage Gen2

> [!NOTE]
> Während der Public Preview-Phase von Data Lake Storage Gen2 kann die Verfügbarkeit einiger der unten aufgeführten Features variieren. Sobald neue Features und Regionen in der Preview-Phase freigegeben werden, erfolgt eine entsprechende Mitteilung.
> [Registrieren](https://aka.ms/adlsgen2signup) Sie sich für die Public Preview von Data Lake Storage Gen2.  

- **Mit Hadoop kompatibler Zugriff**: Data Lake Storage Gen2 ermöglicht einen mit einem [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html) vergleichbaren Zugriff auf Daten und deren Verwaltung. Der neue [ABFS-Treiber](./abfs-driver.md) ist in allen Apache Hadoop-Umgebungen verfügbar, einschließlich [Azure HDInsight](../../hdinsight/index.yml) und [Azure Databricks](../../azure-databricks/index.yml), um auf die in Data Lake Storage Gen2 gespeicherten Daten zuzugreifen.

- **Obermenge von POSIX-Berechtigungen:** Das Sicherheitsmodell für Data Lake Gen2 unterstützt Zugriffssteuerungslisten und POSIX-Berechtigungen sowie zusätzliche Granularität speziell für Data Lake Storage Gen2. Die Einstellungen können über Verwaltungstools oder Frameworks wie Hive und Spark konfiguriert werden.

- **Kosteneffizienz:** Data Lake Storage Gen2 bietet kostengünstige Speicherkapazität und -transaktionen. Während Daten ihren gesamten Lebenszyklus durchlaufen, ändern sich Abrechnungstarife, wodurch die Kosten durch integrierte Funktionen wie [Azure Blob Storage-Lebenszyklus](../common/storage-lifecycle-managment-concepts.md) auf ein Minimum reduziert werden.

- **Unterstützung von Blob Storage-Tools, -Frameworks und -Apps**: Data Lake Storage Gen2 arbeitet weiterhin mit einer Vielzahl von Tools, Frameworks und Anwendungen, die es heute bereits für Blob Storage gibt.

- **Optimierter Treiber**: Der `abfs`-Treiber ist [speziell](./abfs-driver.md) für Big Data-Analysen optimiert. Die entsprechenden REST-APIs werden über den `dfs`-Endpunkt, `dfs.core.windows.net`, angezeigt.

## <a name="scalability"></a>Skalierbarkeit

Azure Storage ist unabhängig davon skalierbar, ob der Zugriff über Data Lake Storage Gen2- oder Blob Storage-Schnittstellen erfolgt. Die Lösung ist in der Lage, *viele Exabyte an Daten* zu speichern und zu verwalten. Diese Speichermenge steht mit einem Durchsatz zur Verfügung, der in Gigabit pro Sekunde (GBit/s) bei hohen Graden von Ein-/Ausgabevorgängen pro Sekunde (IOPS) gemessen wird. Über die reine dauerhafte Speicherung hinaus erfolgt die Verarbeitung mit nahezu konstanten Wartezeiten pro Anforderung, die auf Dienst-, Konten- und Dateiebene gemessen werden.

## <a name="cost-effectiveness"></a>Kosteneffizienz

Einer der vielen Vorteile davon, dass Data Lake Storage Gen2 auf Azure Blob Storage aufsetzt, sind die [niedrigen Kosten](https://azure.microsoft.com/pricing/details/storage) für Speicherkapazität und -transaktionen. Im Gegensatz zu anderen Cloudspeicherdiensten senkt Data Lake Storage Gen2 die Kosten, da die Daten vor der Analyse nicht verschoben oder transformiert werden müssen.

Zusätzlich verbessern Features wie der [hierarchische Namespace](./namespace.md) die Gesamtleistung vieler Analyseaufträge erheblich. Diese Leistungsverbesserung bedeutet, dass Sie weniger Rechenleistung benötigen, um die gleiche Datenmenge zu verarbeiten, was zu niedrigeren Gesamtbetriebskosten für den gesamten Analyseauftrag führt.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln werden einige der wichtigsten Konzepte von Data Lake Storage Gen2 beschrieben. Zudem wird erläutert, wie Sie Ihre Daten speichern, darauf zugreifen, sie verwalten und Einsichten daraus gewinnen können:

* [Hierarchischer Namespace](./namespace.md)
* [Erstellen eines Speicherkontos](./quickstart-create-account.md)
* [Erstellen eines HDInsight-Clusters mit Azure Data Lake Storage Gen2](./quickstart-create-connect-hdi-cluster.md)
* [Verwenden eines Azure Data Lake Storage Gen2-Kontos in Azure Databricks](./quickstart-create-databricks-account.md)