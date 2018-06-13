---
title: Verwenden von Ambari-Ansichten für die Arbeit mit Hive in HDInsight (Hadoop) – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Hive-Ansicht im Webbrowser zum Übermitteln von Hive-Abfragen verwenden. Die Hive-Ansicht ist Teil der Ambari-Webbenutzeroberfläche, die mit dem Linux-basierten HDInsight-Cluster bereitgestellt wird.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: larryfr
ms.openlocfilehash: 6c07e9a45cbfbc5e6ed6787277dcfa1bf4cf4b2b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34201491"
---
# <a name="use-ambari-hive-view-with-hadoop-in-hdinsight"></a>Verwenden der Ambari-Hive-Ansicht mit Hadoop in HDInsight

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel wird das folgende Thema erläutert: Ausführen von Hive-Abfragen mithilfe der Ambari-Hive-Ansicht. In der Hive-Ansicht können Sie Hive-Abfragen direkt in Ihrem Webbrowser erstellen, optimieren und ausführen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Linux-basierter Hadoop-Cluster in HDInsight, Version 3.4 oder höher.

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Aus einem Webbrowser

## <a name="run-a-hive-query"></a>Ausführen einer Hive-Abfrage

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

2. Wählen Sie Ihren HDInsight-Cluster, und wählen Sie dann im Abschnitt **Quicklinks** die Option **Ambari-Ansichten**.

    ![Abschnitt „Quicklinks“ des Portals](./media/apache-hadoop-use-hive-ambari-view/quicklinks.png)

    Wenn Sie aufgefordert werden, sich zu authentifizieren, verwenden Sie den Kontonamen und das Kennwort der Clusteranmeldung (standardmäßig `admin`), die Sie bei der Erstellung des Clusters angegeben haben.

3. Wählen Sie aus der Liste der Ansichten die __Hive-Ansicht__ aus.

    ![Die ausgewählte Hive-Ansicht](./media/apache-hadoop-use-hive-ambari-view/select-hive-view.png)

    Die Seite „Hive-Ansicht“ ähnelt der folgenden Abbildung:

    ![Abbildung des Arbeitsblatts „Abfragen“ für die Hive-Ansicht](./media/apache-hadoop-use-hive-ambari-view/ambari-hive-view.png)

4. Kopieren Sie folgende HiveQL-Anweisungen aus der Registerkarte __Query__ (Abfrage) in das Arbeitsblatt:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS loglevel, COUNT(*) AS count FROM log4jLogs 
        WHERE t4 = '[ERROR]' 
        GROUP BY t4;
    ```

    Diese Anweisungen führen die folgenden Aktionen aus:

   * `DROP TABLE`: Löscht Tabelle und Datendatei, falls die Tabelle bereits existiert.

   * `CREATE EXTERNAL TABLE`: Erstellt eine neue „externe“ Tabelle in Hive.
   Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben an ihrem ursprünglichen Speicherort.

   * `ROW FORMAT`: Gibt an, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.

   * `STORED AS TEXTFILE LOCATION`: Zeigt den Speicherort für die Daten an, und dass sie als Text gespeichert sind.

   * `SELECT`: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte „t4“ den Wert „[ERROR]“ enthält.

    > [!IMPORTANT]
    > Belassen Sie die Auswahl der __Database__ (Datenbank) bei __default__ (Standard). Die Beispiele in diesem Dokument verwenden die Standarddatenbank, die in HDInsight enthalten ist.

5. Klicken Sie auf die Schaltfläche **Execute** (Ausführen) unter dem Arbeitsblatt, um die Abfrage zu starten. Die Schaltfläche wird nun orange angezeigt, und der Text ändert sich in **Stop** (Beenden).

6. Nach Abschluss der Abfrage werden in der Registerkarte **Results** (Ergebnisse) die Ergebnisse des Vorgangs angezeigt. Der folgende Text ist das Ergebnis der Abfrage:

        loglevel       count
        [ERROR]        3

    Sie können auf der Registerkarte **Logs** die vom Auftrag erstellten Protokollinformationen anzeigen.

   > [!TIP]
   > Im Dropdowndialogfeld **Save results** in der oberen linken Ecke des Abschnitts **Query Process Results** können Sie Ergebnisse herunterladen oder speichern.

### <a name="visual-explain"></a>Visuelle Erläuterung

Klicken Sie auf die Registerkarte **Visual Explain** (Visuelle Erläuterung) unter dem Arbeitsblatt, um eine Visualisierung des Abfrageplans anzuzeigen.

Die Ansicht **Visuelle Erläuterung** der Abfrage kann das Verständnis des Ablaufs komplexer Abfragen erleichtern. Sie können eine Textentsprechung dieser Ansicht mit der Schaltfläche **Explain** im Abfrage-Editor anzeigen.

### <a name="tez-ui"></a>Tez-Benutzeroberfläche

Klicken Sie auf die Registerkarte **Tez** unter dem Arbeitsblatt, um die Tez-Benutzeroberfläche für die Abfrage anzuzeigen.

> [!IMPORTANT]
> Tez wird nicht für die Auflösung aller Abfragen verwendet. Viele Abfragen können ohne Tez aufgelöst werden. 

Wenn die Abfrage mit Tez aufgelöst wurde, wird der Directed Acyclic Graph (DAG) angezeigt. Wenn Sie den DAG für Abfragen anzeigen möchten, die Sie in der Vergangenheit ausgeführt haben, oder den Tez-Prozess debuggen möchten, verwenden Sie stattdessen die [Tez-Ansicht](../hdinsight-debug-ambari-tez-view.md) .

## <a name="view-job-history"></a>Anzeige des Auftragsverlaufs

Die Registerkarte __Jobs__ (Aufträge) zeigt einen Verlauf der Hive-Abfragen an.

![Abbildung des Auftragsverlaufs](./media/apache-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Datenbanktabellen

Sie können die Registerkarte __Tables__ (Tabellen) verwenden, um innerhalb einer Hive-Datenbank mit Tabellen zu arbeiten.

![Abbildung der Registerkarte „Tabellen“](./media/apache-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Gespeicherte Abfragen

In der Registerkarte **Query** können Sie optional auch Abfragen speichern. Nachdem Sie eine Abfrage gespeichert haben, können Sie diese von der Registerkarte __Saved Queries__ aus wiederverwenden.

![Abbildung der Registerkarte „Gespeicherte Abfragen“](./media/apache-hadoop-use-hive-ambari-view/saved-queries.png)

> [!TIP]
> Gespeicherte Abfragen werden im Standard-Clusterspeicher gespeichert. Sie finden die gespeicherten Abfragen im Pfad `/user/<username>/hive/scripts`. Diese werden als Textdateien vom Typ `.hql` gespeichert.
>
> Wenn Sie den Cluster löschen, den Speicher jedoch beibehalten, können Sie die Abfragen mit einem Hilfsprogramm wie [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) oder Data Lake Storage-Explorer (aus dem [Azure-Portal](https://portal.azure.com)) abrufen.

## <a name="user-defined-functions"></a>Benutzerdefinierte Funktionen

Sie können Hive über benutzerdefinierte Funktionen (UDF) erweitern. Mit einer UDF-Datei können Sie Funktionen oder Logik implementieren, die sich nicht einfach in HiveQL modellieren lässt.

Deklarieren und speichern Sie eine Gruppe von UDFs mit der Registerkarte **UDF** ganz oben in der Hive-Ansicht. Diese UDFs können im **Abfrage-Editor** verwendet werden.

![Abbildung der Registerkarte „UDF“](./media/apache-hadoop-use-hive-ambari-view/user-defined-functions.png)

Nachdem Sie der Hive-Ansicht eine benutzerdefinierte Funktion hinzugefügt haben, wird ganz unten im **Abfrage-Editor** die Schaltfläche **Insert udfs** angezeigt. Wenn Sie diesen Eintrag wählen, wird eine Dropdownliste mit den UDFs angezeigt, die in der Hive-Ansicht definiert sind. Durch das Auswählen einer UDF werden Ihrer Abfrage HiveQL-Anweisungen hinzugefügt, um die UDF zu aktivieren.

Beispielsweise können Sie eine UDF mit den folgenden Eigenschaften definieren:

* Ressourcenname: myudfs

* Ressourcenpfad: /myudfs.jar

* UDF-Name: myawesomeudf

* UDF-Klassenname: com.myudfs.Awesome

Mit der Schaltfläche **Insert udfs** wird ein Eintrag mit dem Namen **myudfs** angezeigt, der über eine weitere Dropdownliste für jede UDF verfügt, die für diese Ressource definiert ist. In diesem Fall ist das **myawesomeudf**. Wenn Sie diesen Eintrag auswählen, wird am Anfang der Abfrage Folgendes hinzugefügt:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Sie können die UDF dann in Ihrer Abfrage verwenden. Beispiel: `SELECT myawesomeudf(name) FROM people;`.

Weitere Informationen zur Verwendung von UDFs mit Hive unter HDInsight finden Sie in den folgenden Artikeln:

* [Verwenden von Python mit Hive und Pig in HDInsight](python-udf-hdinsight.md)
* [How to add a custom Hive UDF to HDInsight (in englischer Sprache)](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Hive-Einstellungen

Sie können verschiedene Hive-Einstellungen ändern, z.B. die Ausführungs-Engine für Hive von Tez (Standard) in MapReduce.

## <a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Hive in HDInsight:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)
