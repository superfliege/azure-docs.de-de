---
title: Verwenden von Apache DataFu mit Apache Pig in HDInsight – Azure
description: Bei Apache DataFu handelt es sich um eine Sammlung von Bibliotheken für die Verwendung mit Apache Pig in Apache Hadoop. Erfahren Sie, wie Sie DataFu mit Pig in Ihrem HDInsight-Cluster verwenden können.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/16/2018
ms.author: hrasheed
ms.openlocfilehash: 982ed4533c34cd4a6613d289ef4712001977538f
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436211"
---
# <a name="use-apache-datafu-pig-with-apache-pig-on-hdinsight"></a>Verwenden von Apache DataFu Pig mit Apache Pig in HDInsight

Erfahren Sie, wie Apache DataFu Pig mit HDInsight verwendet wird.

Bei Apache DataFu handelt es sich um eine Sammlung von Open-Source-Bibliotheken für die Verwendung mit Apache Pig in Apache Hadoop.
Weitere Informationen zu DataFu Pig finden Sie unter [https://datafu.apache.org/](https://datafu.apache.org/).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement.

* Ein Azure HDInsight-Cluster (Linux- oder Windows-basiert)

  > [!IMPORTANT]  
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Grundlegende Kenntnisse der [Verwendung von Apache Pig für HDInsight](hdinsight-use-pig.md)

## <a name="install-datafu-on-linux-based-hdinsight"></a>Installieren von DataFu für Linux-basiertes HDInsight

> [!IMPORTANT]  
> DataFu wird in Linux-basierten Clustern ab Version 3.3 und in Windows-basierten Clustern installiert. In Linux-basierten Clustern mit einer Version vor 3.3 wird DataFu nicht installiert.
>
> Wenn Sie einen Windows-basierten Cluster oder einen Linux-basierten Cluster über Version 3.3 verwenden, überspringen Sie diesen Abschnitt.

DataFu kann aus dem Maven-Repository heruntergeladen und installiert werden. Führen Sie die folgenden Schritte aus, um die gewünschte Version zu finden und Ihrem HDInsight-Cluster hinzuzufügen:

> [!WARNING]  
> DataFu-Versionen haben möglicherweise Anforderungen, die von HDInsight nicht erfüllt werden. Wenn Sie beispielsweise eine ältere Version von DataFu verwenden, kann es sein, dass Sie eine andere Version von Pig benötigen als die, die in HDInsight enthalten ist.

### <a name="find-a-version"></a>Suchen einer Version

1. Navigieren Sie in Ihrem Webbrowser zu https://mvnrepository.com/artifact/org.apache.datafu/datafu-pig, und suchen Sie die Version, die Sie benötigen.

2. Wählen Sie die verknüpften Versionsnummer.

3. Wählen Sie __Alle anzeigen__, um alle Dateien anzuzeigen.

4. Suchen Sie in der Liste der Dateien die JAR-Datei. Diese Datei ist in der Regel die größte der aufgeführten Dateien, da sie alle Abhängigkeiten enthält. Klicken Sie mit der rechten Maustaste auf den Link und kopieren Sie die Linkadresse.

### <a name="download-datafu-to-hdinsight"></a>Herunterladen von DataFu zu HDInsight

1. Stellen Sie per SSH eine Verbindung mit Ihrem Linux-basierten HDInsight-Cluster her. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Verwenden Sie den folgenden Befehl, um die JAR-Datei von DataFu mit dem Hilfsprogramm „Wget“ herunterzuladen:

    > [!IMPORTANT]  
    > Ersetzen Sie den Link im Befehl durch die URL, die Sie zuvor kopiert haben.

    ```
    wget https://central.maven.org/maven2/org/apache/datafu/datafu-pig/1.4.0/datafu-pig-1.4.0.jar
    ```

3. Laden Sie als Nächstes die Datei in den Standardspeicher für Ihren HDInsight-Cluster hoch. Wenn Sie die Datei im Standardspeicher ablegen, steht sie allen Clusterknoten zur Verfügung.

    > [!IMPORTANT]  
    > Ersetzen Sie die Versionsnummer im Dateinamen durch die Version, die Sie heruntergeladen haben.

    ```
    hdfs dfs -put datafu-pig-1.4.0.jar /example/jars
    ```

    > [!NOTE]  
    > Im vorherigen Befehl wird die JAR-Datei unter `/example/jars` gespeichert, da dieses Verzeichnis im Clusterspeicher bereits vorhanden ist. Sie können im HDInsight-Clusterspeicher einen beliebigen Speicherort verwenden.

## <a name="use-datafu-with-pig"></a>Verwenden von DataFu mit Pig

Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit Pig in HDInsight vertraut sind. Weitere Informationen zum Verwenden von Pig für HDInsight finden Sie unter [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md).

> [!IMPORTANT]  
> Wenn Sie DataFu mithilfe der Schritte im vorherigen Abschnitt manuell installiert haben, müssen Sie es vor der Verwendung registrieren.
>
> * Wenn Ihr Cluster Azure Storage verwendet, verwenden Sie einen `wasb://`-Pfad. Beispiel: `register wasb:///example/jars/datafu-pig-1.4.0.jar`.
>
> * Wenn Ihr Cluster Azure Data Lake Store verwendet, verwenden Sie einen `adl://`-Pfad. Beispiel: `register adl://home/example/jars/datafu-pig-1.4.0.jar`.

Gewöhnlich definieren Sie für DataFu-Funktionen einen Alias. Das folgende Beispiel definiert einen Alias für `SHA`:

```piglatin
DEFINE SHA datafu.pig.hash.SHA();
```

Diesen Alias können Sie dann in einem Pig Latin-Skript verwenden, um einen Hashwert für die Eingabedaten zu generieren. Im folgenden Code wird beispielsweise der Speicherort in den Eingabedaten durch einen Hashwert ersetzt:

```piglatin
raw = LOAD '/HdiSamples/HdiSamples/SensorSampleData/building/building.csv' USING
    org.apache.pig.piggybank.storage.CSVExcelStorage(',', 'NO_MULTILINE', 'UNIX', 'SKIP_INPUT_HEADER') AS
    (int1:int,
     id1:chararray,
     int2:int,
     id2:chararray,
     location:chararray);
mask = FOREACH raw GENERATE int1, id1, int2, id2, SHA(location);
DUMP mask;
```

Dadurch wird folgende Ausgabe generiert:

    (1,M1,25,AC1000,aa5ab35a9174c2062b7f7697b33fafe5ce404cf5fecf6bfbbf0dc96ba0d90046)
    (2,M2,27,FN39TG,7a1ca4ef7515f7276bae7230545829c27810c9d9e98ab2c06066bee6270d5153)
    (3,M3,28,JDNS77,07f62b021771d3cf67e2e1faf18769cc5e5c119ad7d4d1847a11e11d6d5a7ecb)
    (4,M4,17,GG1919,aed8f531aa7e785be255bc435e2582e74c58defedebcb629eeabf365b809bd6f)
    (5,M5,3,ACMAX22,1ed8c7e56c947bebc0cfcf88c4ea0f02c944568f71df893a99970e4f0c78cddc)
    (6,M6,9,AC1000,c96dd81db196cca5f57bd4270bbb9d9e9d1b242d67f9364005ee1dfdc2632523)
    (7,M7,13,FN39TG,3e049d78d958038ac6bd5dcf038075cc73362b4956aaf7308c3a69c8eca76297)
    (8,M8,25,JDNS77,c1ef40ce0484c698eb4bd27fe56c1e7b68d74f9780ed674210d0e5013dae45e9)
    (9,M9,11,GG1919,a7d355b26bda6bf1196ccffead0b2cf2b81f0a9de5b4876b44407f1dc07e51e6)
    (10,M10,23,ACMAX22,10436829032f361a3de50048de41755140e581467bc1895e6c1a17f423e42d10)
    (11,M11,14,AC1000,348841ef53dbd5a64008a86be432973db790774fb28b59b0d99702a3188b3705)
    (12,M12,26,FN39TG,aed8f531aa7e785be255bc435e2582e74c58defedebcb629eeabf365b809bd6f)
    (13,M13,25,JDNS77,ed9ad13611d7164839dd3feaf9a7f6a75a4138f389e0d45f8e07fa38da1116a2)
    (14,M14,17,GG1919,80db4ccdca106d37b920206331fcfe3e9e50a9e763d89b54ce3ad5ac8cf30f03)
    (15,M15,19,ACMAX22,3552ac0c032467fbf592cb4d10c5c9267800c01e343ad8ca557256d882ae9327)
    (16,M16,23,AC1000,07c67d76ef92ac9853588e098983fefba4ba5965f8fec95f42ab0d04c27865ba)
    (17,M17,11,FN39TG,557c1599d9a04895d3817d293e0806a4419a14de31958386182798d0d2ed3a56)
    (18,M18,25,JDNS77,dbc74a36d8e7439c45c64d856388506cc9b1218619cef979c3d605115a7a4546)
    (19,M19,14,GG1919,be55ef3f4c4e6c2d9c2afe2a33ac90ad0f50d4de7f9163999877e2a9ca5a54f8)
    (20,M20,19,ACMAX22,ea0b937ea317101ee2c26b03a4843a19ceced8a2b9673c3cf409a726ca2b0fd8)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu DataFu oder Pig finden Sie in den folgenden Dokumenten:

* [Apache DataFu Pig – Erste Schritte](https://datafu.apache.org/docs/datafu/getting-started.html).
* [Verwenden von Apache Pig mit HDInsight](hdinsight-use-pig.md)
