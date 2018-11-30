---
title: Durchsuchen von Daten in Hive-Tabellen mithilfe von Hive-Abfragen | Microsoft Dcos
description: Durchsuchen Sie Daten in Hive-Tabellen mithilfe von Hive-Abfragen.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: c50c6de28c60bbe6a6c0d510e62f35cd037d6e95
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52443311"
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Durchsuchen von Daten in Hive-Tabellen mithilfe von Hive-Abfragen

Dieser Artikel enthält Hive-Beispielskripts, die zum Durchsuchen von Daten in Hive-Tabellen in einem HDInsight Hadoop-Cluster verwendet werden.

Dieser Task ist ein Schritt im [Team Data Science-Prozess](overview.md).

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie Folgendes abgeschlossen haben:

* Sie haben ein Azure-Speicherkonto erstellt. Anweisungen finden Sie unter [Erstellen eines Azure-Speicherkontos](../../storage/common/storage-quickstart-create-account.md).
* Sie haben einen angepassten Hadoop-Cluster mit dem HDInsight-Dienst bereitgestellt. Anweisungen hierzu finden Sie unter [Anpassen von Azure HDInsight Hadoop-Clustern für die erweiterte Analyse](customize-hadoop-cluster.md).
* Sie haben die Daten in Hive-Tabellen auf Azure HDInsight Hadoop-Clustern hochgeladen. Wenn dies nicht der Fall ist, führen Sie die unter [Erstellen und Laden von Daten in Hive-Tabellen](move-hive-tables.md) beschriebenen Schritte zum Hochladen von Daten in Hive-Tabellen aus.
* Sie haben den Remotezugriff auf den Cluster aktiviert. Anweisungen finden Sie unter [Zugreifen auf den Hauptknoten von Hadoop-Clustern](customize-hadoop-cluster.md).
* Informationen zum Übermitteln von Hive-Abfragen finden Sie unter [Übermitteln von Hive-Abfragen](move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Hive-Beispielabfrageskripts für das Durchsuchen von Daten
1. Abrufen der Anzahl der Vorkommen pro Partition `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Abrufen der Anzahl der Vorkommen pro Tag `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. Abrufen der Ebenen in einer Kategoriespalte:  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. Abrufen der Anzahl von Ebenen in Kombination zweier kategorischer Spalten `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. Abrufen der Verteilung der numerischen Spalten:  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. Extrahieren von Datensätzen durch Zusammenführen von zwei Tabellen:
   
        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Zusätzliche Abfrageskripts für Szenarios mit Taxi-Verkehrsdaten
Beispiele für Abfragen speziell für Szenarien mit den [NYC Taxi Trip-Daten](http://chriswhong.com/open-data/foil_nyc_taxi/) stehen auch im [GitHub-Repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) zur Verfügung. Für diese Abfragen ist bereits ein Datenschema angegeben, sodass sie bereit für die Übermittlung zur Ausführung sind.

