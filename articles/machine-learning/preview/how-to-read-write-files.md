---
title: "Lesen und Schreiben von großen Datendateien | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie bei Azure Machine Learning-Experimenten große Dateien lesen und schreiben."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/10/2017
ms.openlocfilehash: 2f3ca2e694fd9952319a70477e9887c332b08044
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="persisting-changes-and-working-with-large-files"></a>Beibehalten von Änderungen und Arbeiten mit großen Dateien
Mit dem Azure Machine Learning-Experimentieren-Dienst können Sie viele verschiedene Ausführungsziele konfigurieren. Einige Ziele sind lokale Ziele, z.B. ein lokaler Computer oder ein Docker-Container auf einem lokalen Computer. Andere Ziele sind Remoteziele, z.B. ein Docker-Container auf einem Remotecomputer oder ein HDInsight-Cluster. Weitere Informationen finden Sie unter [Übersicht über den Azure Machine Learning-Experimentieren-Ausführungsdienst](experiment-execution-configuration.md). 

Bevor die Ausführung auf einem Ziel möglich ist, müssen Sie den Projektordner auf das Computeziel kopieren. Dies ist auch bei einer lokalen Ausführung erforderlich, bei der für diesen Zweck ein lokaler temporärer Ordner verwendet wird. 

## <a name="execution-isolation-portability-and-reproducibility"></a>Isolation, Portabilität und Reproduzierbarkeit der Ausführung
Der Zweck dieses Entwurfs besteht darin, die Isolation, Reproduzierbarkeit und Portabilität der Ausführung sicherzustellen. Wenn Sie dasselbe Skript auf demselben bzw. einem anderen Computeziel zweimal ausführen, erhalten Sie die gleichen Ergebnisse. Die Änderungen, die während der ersten Ausführung vorgenommen wurden, sollten sich nicht auf die Änderungen der zweiten Ausführung auswirken. Mit diesem Entwurf können Sie Computeziele als zustandslose Berechnungsressourcen betrachten, die jeweils keine Affinität zu den Aufträgen aufweisen, die nach deren Abschluss ausgeführt werden.

## <a name="challenges"></a>Herausforderungen
Dieser Entwurf bietet zwar Vorteile in Bezug auf die Portabilität und Wiederholbarkeit, aber er birgt auch einige besondere Herausforderungen:

### <a name="persisting-state-changes"></a>Beibehalten von Zustandsänderungen
Wenn Ihr Skript den Zustand des Computekontexts ändert, werden die Änderungen für Ihre nächste Ausführung nicht beibehalten und auch nicht automatisch zurück an den Clientcomputer geleitet. 

Genauer gesagt: Wenn mit Ihrem Skript ein Unterordner erstellt oder eine Datei geschrieben wird, ist dieser Ordner bzw. die Datei nach der Ausführung nicht in Ihrem Projektverzeichnis enthalten. Die Dateien werden in der Umgebung des Computeziels in einem temporären Ordner gespeichert. Sie können sie zum Debuggen nutzen, aber Sie können sich nicht darauf verlassen, dass sie permanent vorhanden sind.

### <a name="working-with-large-files-in-the-project-folder"></a>Arbeiten mit großen Dateien im Projektordner

Wenn der Projektordner zu Beginn einer Ausführung in die Zielserverumgebung kopiert wird, führt dies zu Latenz, falls Ihr Ordner große Dateien enthält. Auch wenn die Ausführung lokal erfolgt, ist noch immer unnötiger Datenverkehr auf Datenträgern vorhanden, der vermieden werden sollte. Aus diesem Grund ist die maximale Projektgröße derzeit auf 25 MB beschränkt.

## <a name="option-1-use-the-outputs-folder"></a>Option 1: Verwenden des Ordners *outputs*
Diese Option ist vorzuziehen, wenn alle folgenden Bedingungen erfüllt sind:
* Ihr Skript erzeugt Dateien.
* Sie erwarten, dass sich die Dateien bei jedem Experiment ändern.
* Sie möchten für diese Dateien den Verlauf aufzeichnen. 

Gängige Anwendungsfälle:
* Trainieren eines Modells
* Erstellen eines Datasets
* Plotten eines Graphen als Imagedatei im Rahmen Ihrer Trainingsausführung für Modelle 

Außerdem ist es ratsam, diese Ausgaben für mehrere Ausführungen zu vergleichen, eine Ausgabedatei auszuwählen (z.B. ein Modell), die bei einer vorherigen Ausführung erstellt wurde, und die Datei dann für eine nachfolgende Aufgabe (z.B. eine Bewertung) zu nutzen.

Sie können Dateien in einen Ordner mit dem Namen *outputs* schreiben, der relativ zum Stammverzeichnis angeordnet ist. Der Ordner erhält vom Experimentieren-Dienst eine besondere Behandlung. Alle Elemente, die von Ihrem Skript während der Ausführung in dem Ordner erstellt werden, z.B. eine Modelldatei, Datendatei oder gezeichnete Bilddatei (auch als _Artefakte_ bezeichnet), werden nach Abschluss der Ausführung in das Azure Blob Storage-Konto kopiert, das mit Ihrem Experimentieren-Konto verknüpft ist. Die Dateien werden zu einem Teil Ihres Ausführungsverlauf-Datensatzes.

Hier ist ein kurzes Beispiel für Code zum Speichern eines Modells im Ordner *outputs* angegeben:
```python
import os
import pickle

# m is a scikit-learn model. 
# we serialize it into a mode.plk file under the ./outputs folder.
with open(os.path.join('.', 'outputs', 'model.pkl'), 'wb') as f:    
    pickle.dump(m, f)
```
Sie können jedes beliebige Artefakt herunterladen, indem Sie zum Abschnitt **Ausgabedateien** der Seite „Ausführungsdetails“ einer bestimmten Ausführung in Azure Machine Learning Workbench navigieren. Wählen Sie einfach die Ausführung und dann die Schaltfläche **Herunterladen**. Alternativ dazu können Sie im Fenster der Befehlszeilenschnittstelle den Befehl `az ml asset download` eingeben.

Ein komplexeres Beispiel finden Sie im Python-Skript `iris_sklearn.py` im Beispielprojekt _Classifying Iris_ (Klassifizieren von Schwertlilien).

## <a name="option-2-use-the-shared-folder"></a>Option 2: Verwenden des freigegebenen Ordners
Die Verwendung eines freigegebenen Ordners, auf den von unabhängigen Ausführungen aus zugegriffen werden kann, kann eine gute Option für die folgenden Szenarien sein, solange Sie keinen Verlauf der Dateien für jede Ausführung beibehalten müssen: 
- Ihr Skript muss Daten aus lokalen Dateien, z.B. CSV-Dateien oder einem Verzeichnis mit Text- oder Imagedateien, als Trainings- oder Testdaten laden. 
- Ihr Skript verarbeitet Rohdaten und schreibt Zwischenergebnisse, z.B. Funktionstrainingsdaten aus Text- oder Imagedateien, die in einer nachfolgenden Trainingsausführung verwendet werden. 
- Ihr Skript gibt ein Modell aus, und Ihr nachfolgendes Bewertungsskript muss das Modell aufgreifen und es für die Auswertung verwenden. 

Beachten Sie hierbei, dass der freigegebene Ordner lokal auf Ihrem gewählten Computeziel angeordnet sein muss. Deshalb funktioniert diese Option nur, wenn alle Ihre Skriptausführungen, die auf diesen freigegebenen Ordner verweisen, auf demselben Computeziel ausgeführt werden und das Computeziel zwischen den einzelnen Ausführungen nicht wiederverwendet wird.

Indem Sie das Feature mit dem freigegebenen Ordner nutzen, können Sie aus einem speziellen Ordner lesen bzw. in diesen Ordner schreiben, der per Umgebungsvariable identifiziert wird: `AZUREML_NATIVE_SHARE_DIRECTORY`. 

### <a name="example"></a>Beispiel
Hier ist ein Beispiel für Python-Code zur Verwendung dieses Freigabeordners angegeben, um eine Textdatei zu lesen und in die Datei zu schreiben:
```python
import os

# write to the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', 'wb') as f:
    f.write(“Hello World”)

# read from the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', 'r') as f:
    text = file.read()
```

Ein umfassenderes Beispiel finden Sie im Beispielprojekt _Classifying Iris_ (Klassifizieren von Schwertlilien) in der Datei *iris_sklearn_shared_folder.py*.

Bevor Sie diese Funktion verwenden können, müssen Sie in der *.compute*-Datei einige einfache Konfigurationen festlegen, die den Zielausführungskontext im Ordner *aml_config* darstellen. Der tatsächliche Pfad zum Ordner kann je nach ausgewähltem Computeziel und konfiguriertem Wert variieren.

### <a name="configure-local-compute-context"></a>Konfigurieren von lokalem Computekontext

Um dieses Feature in einem lokalen Computekontext zu ermöglichen, fügen Sie der *.compute*-Datei einfach die folgende Zeile hinzu, die für die _lokale_ Umgebung steht (normalerweise als *local.compute* bezeichnet).
```
# local.runconfig
...
nativeSharedDirectory: ~/.azureml/share
...
```

Der Pfad „~/.azureml/share“ ist der Standardbasispfad des Ordners. Sie können ihn in einen beliebigen lokalen absoluten Pfad ändern, der für die Skriptausführung zugänglich ist. Der Name des Experimentieren-Kontos, der Arbeitsbereichsname sowie der Projektname werden automatisch an den Namen des Basisverzeichnisses angefügt, der dann zum vollständigen Pfad des freigegebenen Verzeichnisses wird. Die Dateien können z.B. in den folgenden Pfad geschrieben (oder aus diesem abgerufen) werden, wenn Sie den nachstehenden Standardwert verwenden:

```
# on Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# on macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

### <a name="configure-the-docker-compute-context-local-or-remote"></a>Konfigurieren des Docker-Computekontexts (lokal oder remote)
Sie müssen die folgenden zwei Zeilen an Ihre lokale *.compute*-Docker-Datei oder die Remoteversion dieser Datei anfügen, um diese Funktion in einem Docker-Computekontext zu aktivieren.

```
# docker.compute
...
sharedVolumes: true
nativeSharedDirectory: ~/.azureml/share
...
```
>[!IMPORTANT]
>Die **sharedVolumes**-Eigenschaft muss auf *true* festgelegt sein, wenn Sie die Umgebungsvariable `AZUREML_NATIVE_SHARE_DIRECTORY` verwenden, um auf den freigegebenen Ordner zuzugreifen. Andernfalls schlägt die Ausführung fehl.

Der Code, der im Docker-Container ausgeführt wird, sieht diesen freigegebenen Ordner immer als */azureml-share/* an. Der Ordnerpfad ist aus Sicht des Docker-Containers nicht konfigurierbar. Verwenden Sie diesen Ordnernamen nicht in Ihrem Code. Verwenden Sie stattdessen immer den Namen der Umgebungsvariablen `AZUREML_NATIVE_SHARE_DIRECTORY`, um auf diesen Ordner zu verweisen. Sie ist einem lokalen Ordner auf dem Hostcomputer von Docker bzw. dem Computekontext zugeordnet. Das Basisverzeichnis auf diesem lokalen Ordner ist der konfigurierbare Wert der Einstellung `nativeSharedDirectory` in der *.compute*-Datei. Der lokale Pfad des freigegebenen Ordners auf dem Hostcomputer – falls Sie den Standardwert verwenden – lautet wie folgt:
```
# Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/

# Ubuntu Linux
/home/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

>[!NOTE]
>Unabhängig davon, ob es sich um einen lokalen Computekontext oder einen lokalen Docker-Computekontext handelt, ist der Pfad des freigegebenen Ordners auf dem lokalen Datenträger identisch. Dies bedeutet, dass Sie sogar Dateien zwischen einer lokalen Ausführung und einer lokalen Docker-Ausführung freigeben können.

Sie können Eingabedaten direkt in diesen Ordnern platzieren und erwarten, dass Ihre lokalen Ausführungen oder Docker-Ausführungen diese auf dem Computer abrufen können. Sie können von Ihren lokalen oder Docker-Ausführungen auch Dateien in diesen Ordner schreiben und erwarten, dass die Dateien in diesem Ordner beibehalten werden und somit den Ausführungslebenszyklus überstehen.

Weitere Informationen finden Sie unter [Azure Machine Learning Workbench-Ausführungskonfigurationsdateien](experiment-execution-configuration-reference.md).

>[!NOTE]
>Die Umgebungsvariable `AZUREML_NATIVE_SHARE_DIRECTORY` wird in einem HDInsight-Computekontext nicht unterstützt. Es ist aber einfach, das gleiche Ergebnis zu erreichen. Verwenden Sie hierzu explizit einen absoluten Azure-Blobspeicherpfad, um aus dem angefügten Blobspeicher zu lesen bzw. um darin zu schreiben.

## <a name="option-3-use-external-durable-storage"></a>Option 3: Verwenden des externen permanenten Speichers

Sie können externen permanenten Speicher verwenden, um den Zustand während der Ausführung beizubehalten. Diese Option ist in den folgenden Szenarien nützlich:
- Ihre Eingabedaten werden bereits in permanentem Speicher gespeichert, auf den von der Zielserverumgebung aus zugegriffen werden kann.
- Die Dateien müssen nicht Teil der Ausführungsverlauf-Datensätze sein.
- Die Dateien müssen von Ausführungen über verschiedene Computeumgebungen hinweg freigegeben werden.
- Außerdem müssen die Dateien den Computekontext selbst überleben können.

Ein Ansatz dieser Art ist die Verwendung von Azure-Blobspeicher über Ihren Python- oder PySpark-Code. Hier ist ein kurzes Beispiel angegeben:

```python
from azure.storage.blob import BlockBlobService
import glob
import os

ACCOUNT_NAME = "<your blob storage account name>"
ACCOUNT_KEY = "<account key>"
CONTAINER_NAME = "<container name>"

blob_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

## Create a new container if necessary, or use an existing one
my_service.create_container(CONTAINER_NAME, fail_on_exist=False, public_access=PublicAccess.Container)

# df is a pandas DataFrame
df.to_csv('mydata.csv', sep='\t', index=False)

# Export the mydata.csv file to Blob storage.
for name in glob.iglob('mydata.csv'):
    blob_service.create_blob_from_path(CONTAINER_NAME, 'single_file.csv', name)
```

Nachfolgend sehen Sie ein kurzes Beispiel für das Anfügen eines beliebigen Azure-Blobspeichers an die HDI Spark-Runtime:
```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)
 
attach_storage_container(spark, "<storage account name>", "<storage key>”)
```

## <a name="conclusion"></a>Zusammenfassung
Da Azure Machine Learning Skripts durch Kopieren des gesamten Projektordners in den Zielserverkontext ausführt, müssen große Eingabe-, Ausgabe- und zwischengeschaltete Dateien mit großer Sorgfalt behandelt werden. Für große Dateitransaktionen können Sie den speziellen Ordner „outputs“ verwenden. Dies ist der freigegebene Ordner, auf den über die Umgebungsvariable `AZUREML_NATIVE_SHARE_DIRECTORY` oder externen permanenten Speicher zugegriffen werden kann. 

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie den Artikel [Azure Machine Learning Workbench-Ausführungskonfigurationsdateien](experiment-execution-configuration-reference.md).
- Sehen Sie sich an, wie im Projekt des Tutorials [Classifying Iris](tutorial-classifying-iris-part-1.md) (Klassifizieren von Schwertlilien) der Ordner „outputs“ zum Beibehalten eines trainierten Modells verwendet wird.
