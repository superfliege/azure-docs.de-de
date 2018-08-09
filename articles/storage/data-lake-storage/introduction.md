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
ms.openlocfilehash: 51f38cf7ade01b58ad5ce7925af5546d1a4f1a0c
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525381"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Einführung in Azure Data Lake Storage Gen2 Preview

Azure Data Lake Storage Gen2 Preview setzt auf [Azure Blob Storage](../blobs/storage-blobs-introduction.md) auf und bietet eine Reihe von Funktionen für die Big Data-Analyse. Die Lösung ermöglicht Ihnen eine Schnittstelle zu Ihren Daten sowohl über das Dateisystem und aus auch den Objektspeicher. Damit ist Data Lake Storage Gen2 der einzige cloudbasierte multimodale Speicherdienst, der es Ihnen ermöglicht, aus all Ihren Daten wertvolle Analysen zu gewinnen.

Data Lake Storage Gen2 bietet alle Eigenschaften, die im gesamten Lebenszyklus von Analysedaten benötigt werden. Dies ist das Ergebnis der Zusammenführung der Möglichkeiten unserer beiden bestehenden Speicherdienste. Funktionen von [Azure Data Lake Storage Gen1](../../data-lake-store/index.md), wie z.B. Dateisystemsemantik, Sicherheit und Skalierbarkeit auf Dateiebene, werden mit den kostengünstigen, mehrstufigen Speicherlösungen, Hochverfügbarkeits- und Notfallwiederherstellungsfunktionen und einem umfassenden Ökosystem aus SDKs und Tools von [Azure Blob Storage](../blobs/storage-blobs-introduction.md) kombiniert. In Data Lake Storage Gen2 bleiben alle Vorzüge des Objektspeichers erhalten, während gleichzeitig die Vorteile einer für Analyseworkloads optimierten Dateisystemschnittstelle zum Tragen kommen.

## <a name="designed-for-enterprise-big-data-analytics"></a>Für Big Data-Analysen in Großunternehmen entwickelt

Data Lake Storage Gen2 ist der grundlegende Speicherdienst zum Aufbau von Enterprise Data Lakes (EDL) in Azure. Data Lake Storage Gen2 wurde eigens für die Verarbeitung mehrerer Petabytes an Informationen bei gleichzeitiger Unterstützung eines Durchsatzes von Hunderten von Gigabit konzipiert und bietet Ihnen eine einfache Möglichkeit, riesige Datenmengen zu verwalten.

Ein wesentliches Merkmal von Data Lake Storage Gen2 ist die Hinzufügung eines [hierarchischen Namespace](./namespace.md) zum Blob Storage-Dienst, der Objekte und Dateien für einen leistungsstarken Datenzugriff in einer Verzeichnishierarchie organisiert. Der hierarchische Namespace ermöglicht Data Lake Storage Gen2 auch die gleichzeitige Unterstützung von Objektspeicher- und Dateisystemstrukturen. Beispielsweise verwendet eine allgemeine Namenskonvention für die Objektspeicherung Schrägstriche im Namen, um eine hierarchische Ordnerstruktur nachzuahmen. Mit Data Lake Storage Gen2 wird diese Struktur Wirklichkeit. Vorgänge wie das Umbenennen oder Löschen eines Verzeichnisses werden zu einzelnen atomaren Metadatenvorgängen im Verzeichnis, anstatt alle Objekte aufzuzählen und zu verarbeiten, die das Namenspräfix des Verzeichnisses gemeinsam haben.

In der Vergangenheit mussten bei cloudbasierten Analysen Kompromisse in puncto Leistung, Verwaltung und Sicherheit eingegangen werden. Data Lake Storage Gen2 geht jeden dieser Aspekte auf folgende Weise an:

- Die **Leistung** ist optimiert, da Sie keine Daten als Voraussetzung für die Analyse kopieren oder transformieren müssen. Der hierarchische Namespace verbessert die Leistung von Verzeichnisverwaltungsvorgängen erheblich, wodurch die gesamte Auftragsverarbeitungsleistung gesteigert wird.

- Die **Verwaltung** ist einfacher, weil Sie Dateien mithilfe von Verzeichnissen und Unterverzeichnissen organisieren und bearbeiten können.

- **Kosteneffizienz** wird ermöglicht, da Data Lake Storage Gen2 auf kostengünstigem [Azure Blob Storage](../blobs/storage-blobs-introduction.md) aufsetzt. Durch diese zusätzlichen Features werden die Gesamtbetriebskosten für Big Data-Analysen in Azure weiter gesenkt.

## <a name="key-features-of-data-lake-storage-gen2"></a>Besondere Features von Data Lake Storage Gen2

> [!NOTE]
> Während der Public Preview-Phase von Data Lake Storage Gen2 kann die Verfügbarkeit einiger der unten aufgeführten Features variieren. Sobald neue Features und Regionen in der Preview-Phase freigegeben werden, erfolgt eine entsprechende Mitteilung.
> [Registrieren](https://aka.ms/adlsgen2signup) Sie sich für die Public Preview von Data Lake Storage Gen2.  

- **Mit Hadoop kompatibler Zugriff**: Data Lake Storage Gen2 ermöglicht einen mit einem [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html) vergleichbaren Zugriff auf Daten und deren Verwaltung. Der neue [ABFS-Treiber](./abfs-driver.md) ist in allen Apache Hadoop-Umgebungen verfügbar, einschließlich [Azure HDInsight](../../hdinsight/index.yml) und [Azure Databricks](../../azure-databricks/index.yml), um auf die in Data Lake Storage Gen2 gespeicherten Daten zuzugreifen.

- **Datenzugriff mit Unterstützung mehrerer Protokolle und Modi**: Data Lake Storage Gen2 gilt als **multimodaler** Speicherdienst, da er sowohl Objektspeicher- als auch Dateisystemschnittstellen zu den gleichen Daten **zur gleichen Zeit** bereitstellt. Dies wird durch die Bereitstellung mehrerer Protokollendpunkte erreicht, die auf die gleichen Daten zugreifen können. 

    Im Gegensatz zu anderen Analyselösungen müssen die in Data Lake Storage Gen2 gespeicherten Daten nicht verschoben oder transformiert werden, damit Sie eine Vielzahl von Analysetools ausführen können. Sie können auf Daten über herkömmliche [Blob Storage-APIs](../blobs/storage-blobs-introduction.md) zugreifen (z.B. Daten über [Event Hubs Capture](../../event-hubs/event-hubs-capture-enable-through-portal.md) erfassen) und diese Daten gleichzeitig mit HDInsight oder Azure Databricks verarbeiten. 

- **Kosteneffizienz**: Data Lake Storage Gen2 bietet kostengünstige Speicherkapazität und -transaktionen. Während Daten ihren gesamten Lebenszyklus durchlaufen, ändern sich Abrechnungstarife, wodurch die Kosten durch integrierte Funktionen wie [Azure Blob Storage-Lebenszyklus](../common/storage-lifecycle-managment-concepts.md) auf ein Minimum reduziert werden.

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
* [Erstellen Sie ein Speicherkonto](./quickstart-create-account.md)
* [Erstellen eines HDInsight-Clusters mit Azure Data Lake Storage Gen2](./quickstart-create-connect-hdi-cluster.md)
* [Verwenden eines Azure Data Lake Storage Gen2-Kontos in Azure Databricks](./quickstart-create-databricks-account.md) 