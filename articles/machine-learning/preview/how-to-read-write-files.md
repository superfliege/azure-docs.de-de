---
title: "Lesen und Schreiben großer Datendateien | Microsoft-Dokumentation"
description: "So lesen und schreiben Sie große Dateien in Azure ML-Experimenten."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/10/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: fb3158ef786ad73440a59c07b38476a98ced0768
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="persisting-changes-and-dealing-with-large-files"></a>Beibehalten von Änderungen und Arbeiten mit großen Dateien

## <a name="execution-isolation-portability-and-reproducibility"></a>Ausführung von Isolierung, Portabilität und Reproduzierbarkeit
Mit dem Azure ML-Experimentierdienst können Sie unterschiedliche Ausführungsziele konfigurieren. Einige davon sind lokal, z.B. ein lokaler Computer oder ein Docker-Container auf einem lokalen Computer, und einige sind remote, z.B. ein Docker-Container auf einem Remotecomputer oder einem HDInsight-Cluster. Weitere Details finden Sie im Artikel [Configuration Experimentation Execution (Konfigurierung der Experimentausführung)](experiment-execution-configuration.md). Bevor etwas ausgeführt werden kann, wird der Projektordner zunächst in das Computeziel kopiert. Dies gilt auch im Falle einer lokalen Ausführung, wo ein lokaler temporärer Ordner für diesen Zweck verwendet wird. 

Der Zweck dieses Entwurfs ist es, die Ausführungsisolierung, Reproduzierbarkeit und Portabilität sicherzustellen. Wenn Sie dasselbe Skript zweimal im gleichen oder einem anderen Computeziel ausführen, erhalten Sie garantiert dieselben Ergebnisse. Die Änderungen, die durch die erste Ausführung in Kraft treten, sollten die zweite Ausführung nicht beeinflussen. Mit diesem Entwurf können wir Computeziele als zustandslose Berechnungsressourcen betrachten, die keine Affinität mit den ausgeführten Aufträgen haben, nachdem sie abgeschlossen wurden.

## <a name="challenges"></a>Herausforderungen
Durch die Vorteile der Portabilität und Wiederholbarkeit birgt dieser Entwurf auch einige besondere Herausforderungen:
### <a name="persisting-state-changes"></a>Beibehalten von Zustandsänderungen
Wenn Ihr Skript den Zustand im Computekontext ändert, werden die Änderungen für Ihre nächste Ausführung nicht beibehalten, und sie werden auch nicht automatisch zurück an den Clientcomputer geleitet. 

Genauer ausgedrückt: Wenn Ihr Skript einen Unterordner erstellt oder eine Datei schreibt, finden Sie keinen Ordner oder keine Datei dieser Art in Ihrem Projektverzeichnis nach der Ausführung. Sie verbleiben in einem temporären Ordner in der Computezielumgebung, egal wo sich dieser befindet. Sie können möglicherweise zum Debuggen verwendet werden, Sie können jedoch nie Abhängigkeiten auf deren Existenz berücksichtigen.

### <a name="dealing-with-large-files-in-project-folder"></a>Arbeiten mit großen Dateien im Projektordner

Wenn der Projektordner in die Zielserverumgebung zu Beginn jeder Ausführung kopiert wird, führt dies zu Latenz, wenn Ihr Projektordner große Dateien enthält. Auch wenn die Ausführung lokal erfolgt, ist noch immer unnötiger Datenverkehr auf Datenträgern vorhanden, der vermieden werden sollte. Deshalb haben wir derzeit eine Obergrenze für die Projektgröße bei 25 MB eingerichtet.

## <a name="option-1-use-the-outputs-folder"></a>Option 1: Verwenden des Ausgabeordners
Dies ist die bevorzugte Option, wenn Ihr Skript Dateien erstellt und Sie möchten, dass sich diese Dateien mit jeder Experimentausführung ändern, und Sie wollen einen Verlauf dieser Dateien behalten. Der häufigste Anwendungsfall ist, wenn Sie ein Modell trainieren, ein Dataset erstellen oder ein Diagramm als Bilddatei als Teil Ihrer Modelltrainingsausführung zeichnen. Sie sollten diese Ausgaben auf mehreren Ausführungen vergleichen, dann zurückgehen und eine Ausgabedatei auswählen (wie etwa ein Modell), die zuvor durch eine Ausführung erstellt wurde. Verwenden Sie diese Datei dann für eine nachfolgende Aufgabe (z.B. für eine Bewertung).

Im Grunde genommen können Sie eine oder mehrere Dateien in einen Ordner mit der Bezeichnung `outputs`, relativ zum Stammverzeichnis, schreiben. Dies ist ein spezieller Ordner, der besondere Behandlung durch den Experimentierdienst erfährt. Alles, was Ihr Skript darin während der Ausführung erstellt (z.B. eine Modelldatei, eine Datendatei oder eine gezeichnete Bilddatei, auch bekannt als _Artefakte_), wird nach der Ausführung in das Azure Blob Storage-Konto kopiert, das mit Ihrem Experimentieren-Konto verknüpft ist. All dies wird Teil Ihres Ausführungsverlauf-Datasets.

Nachfolgend sehen Sie ein kurzes Beispiel für das Speichern eines Modells im `outputs`-Ordner:
```python
import os
import pickle

# m is a scikit-learn model. 
# we serialize it into a mode.plk file under the ./outputs folder.
with open(os.path.join('.', 'outputs', 'model.pkl'), 'wb') as f:    
    pickle.dump(m, f)
```
Sie können jedes beliebige _Artefakt_ herunterladen, indem Sie zum Abschnitt **Ausgabedateien** der Seite „Ausführungsdetails“ einer bestimmten Ausführung in Azure ML Workbench navigieren. Wählen Sie das gewünschte Artefakt aus, und klicken Sie auf die Schaltfläche **Herunterladen**. Alternativ können Sie den Befehl `az ml asset download` aus dem CLI-Fenster verwenden.

Ein komplexeres Beispiel finden Sie im `iris_sklearn.py` Python-Skript im Beispielprojekt _Classifying Iris_ (Klassifizieren von Schwertlilien).

## <a name="option-2-use-the-shared-folder"></a>Option 2: Verwenden des freigegebenen Ordners
Die Verwendung eines freigegebenen Ordners, auf den von unabhängigen Ausführungen aus zugegriffen werden kann, kann eine gute Option für die folgenden Szenarios sein, solange Sie keinen Verlauf dieser Dateien für jede Ausführung beibehalten müssen: 
- Ihr Skript muss Daten aus lokalen Dateien laden, z.B. CSV-Dateien oder ein Verzeichnis von Text- oder Imagedateien, wie etwas Trainings- oder Testdaten. 
- Ihr Skript verarbeitet Rohdaten und schreibt Zwischenergebnisse, z.B. Funktionstrainingsdaten aus Text-/Imagedateien, die in einer nachfolgenden Trainingsausführung verwendet werden. 
- Ihr Skript gibt ein Modell aus, und Ihr nachfolgendes Bewertungsskript muss das Modell aufgreifen und es für die Auswertung verwenden. 

Eine wichtige Einschränkung ist, dass sich der freigegebenen Ordner lokal auf demselben Computeziel befindet, das Sie auswählen. Deshalb funktioniert die Methode nur, wenn alle Ihre Skriptausführungen, die auf diesen freigegebenen Ordner verweisen, auf dem selben Computeziel ausgeführt werden, und das Computeziel zwischen den einzelnen Ausführungen nicht wiederverwendet wird.

Die Funktion des freigegebenen Ordners erlaubt Ihnen, aus einem bestimmten Ordner zu lesen oder in diesen zu schreiben, der von einer Umgebungsvariable, `AZUREML_NATIVE_SHARE_DIRECTORY`, identifiziert wird. 

### <a name="example"></a>Beispiel
Nachfolgend sehen Sie einen Beispiel-Python-Code für die Verwendung dieses Freigabeordners, um eine Textdatei zu lesen und zu schreiben:
```python
import os

# write to the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', 'wb') as f:
    f.write(“Hello World”)

# read from the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', 'r') as f:
    text = file.read()
```

Ein komplexeres Beispiel finden Sie in der `iris_sklearn_shared_folder.py`-Datei im Beispielprojekt _Classifying Iris_ (Klassifizieren von Schwertlilien).

Bevor Sie diese Funktion verwenden können, müssen Sie einige einfache Konfigurationen in der `.compute`-Datei festlegen, die den Zielausführungskontext im `aml_config`-Ordner darstellt. Der tatsächliche Pfad zu diesem Ordner kann je nach dem von Ihnen ausgewählten Computeziel und konfigurierten Wert variieren.

### <a name="configure-local-compute-context"></a>Konfigurieren von lokalem Computekontext

Um diese Funktion in einem lokalen Computekontext zu aktivieren, fügen Sie einfach die folgende Zeile Ihrer `.compute`-Datei hinzu, die die _lokale_ Umgebung darstellt (normalerweise als `local.compute` bezeichnet).
```
# local.runconfig
...
nativeSharedDirectory: ~/.azureml/share
...
```

`~/.azureml/share` ist der Standardpfad für den Ordner. Sie können ihn zu jedem lokalen absoluten Pfad ändern, der über die Skriptausführung zugänglich ist. Der Name des Experimentieren-Kontos, der Arbeitsbereichname sowie der Projektname werden automatisch dem Basisverzeichnis angehängt, was den vollständigen Pfad des freigegebenen Verzeichnisses bildet. Die Dateien können z.B. in den folgenden Pfad geschrieben (oder aus diesem abgerufen) werden, wenn Sie den nachstehenden Standardwert verwenden:

```
# on Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# on macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

### <a name="configure-docker-compute-context-local-or-remote"></a>Konfigurieren des Docker-Computekontextes (lokal oder remote)
Sie müssen die folgenden zwei Zeilen Ihrer lokalen oder remoten _.compute_-Datei von Docker anfügen, um diese Funktion in einem Docker-Computekontext zu aktivieren.

```
# docker.compute
...
sharedVolumes: true
nativeSharedDirectory: ~/.azureml/share
...
```
>[!IMPORTANT]
>`sharedVolume` muss auf `true` festgelegt sein, wenn Sie die Umgebungsvariable `AZUREML_NATIVE_SHARE_DIRECTORY` verwenden, um auf den freigegebenen Ordner zuzugreifen, andernfalls schlägt die Ausführung fehl.

Der Code, der im Docker-Container ausgeführt wird, sieht diesen freigegebenen Ordner immer als `/azureml-share/` an. So wie dieser Ordnerpfad vom Docker-Container gesehen wird, ist er nicht konfigurierbar. Sie sollten deshalb keine Abhängigkeit auf diesem Ordnernamen in Ihrem Code nutzen. Verwenden Sie stattdessen immer den Namen der Umgebungsvariable, `AZUREML_NATIVE_SHARE_DIRECTORY`, um diesen Ordner zu adressieren. Sie ist einem lokalen Ordner auf dem Hostcomputer von Docker bzw. dem Computekontext zugeordnet. Das Basisverzeichnis auf diesem lokalen Ordner ist der konfigurierbare Wert der Einstellung `nativeSharedDirectory` in der `.compute`-Datei. Der lokale Pfad des freigegebenen Ordners auf dem Hostcomputer – falls Sie den Standardwert von oben verwenden – ist Folgender:
```
# Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/

# Ubuntu Linux
/home/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

>[!TIP]
>Beachten Sie, dass der Pfad des freigegebenen Ordners auf dem lokalen Datenträger derselbe ist, wie zwischen einem lokalen Computekontext und einem lokalen Docker-Computekontext. Das bedeutet, dass Sie sogar Dateien zwischen einer lokalen Ausführung und einer lokalen Docker-Ausführung freigeben können.

Sie können Eingabedateien direkt in diesen Ordnern platzieren und erwarten, dass Ihre lokalen Ausführungen oder Docker-Ausführungen diese auf diesem Computer abrufen. Sie können von Ihren lokalen oder Docker-Ausführungen auch Dateien in diesen Ordner schreiben und erwarten, dass die Dateien in diesem Ordner beibehalten werden und somit den Ausführungslebenszyklus überstehen.

Weitere Informationen zu den Konfigurationsdateien im Azure ML-Ausführungsdienst finden Sie in diesem Artikel: [Execution Configuration Files (Ausführungskonfigurationsdateien)](experiment-execution-configuration-reference.md).

>[!NOTE]
>Die Umgebungsvariable `AZUREML_NATIVE_SHARE_DIRECTORY` wird im HDInsight-Computekontext nicht unterstützt. Es ist jedoch einfach, das gleiche Ergebnis zu erreichen. Verwenden Sie hierzu explizit einen absoluten WASB-Pfad, um aus dem angefügten Blobspeicher zu lesen bzw. um darin zu schreiben.

## <a name="option-3-use-external-durable-storage"></a>Option 3: Verwenden des externen permanenten Speichers

Sie können natürlich einen externen permanenten Speicher verwenden, um den Zustand während der Ausführung beizubehalten. Dies ist in folgenden Szenarios nützlich:
- Ihre Eingabedaten werden bereits in einem permanenten Speicher gespeichert, auf den von der Zielserverumgebung aus zugegriffen werden kann.
- Diese Dateien müssen nicht Teil der Ausführungsverlauf-Datensätze sein.
- Diese Dateien müssen von Ausführungen auf mehreren Computeumgebungen freigegeben werden.
- Sie müssen den Computekontext selbst überstehen.

Ein Beispiel hierfür ist die Verwendung von Azure Blob Storage von Ihrem Python-/PySpark-Code.

Hier sehen Sie ein kurzes Beispiel der Verwendung von Azure Blob Storage von Python-Code:
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

# Export the mydata.csv file to blob storage.
for name in glob.iglob('mydata.csv'):
    blob_service.create_blob_from_path(CONTAINER_NAME, 'single_file.csv', name)
```

Nachfolgend sehen Sie nochmals ein kurzes Beispiel für das Anfügen eines beliebigen Azure Blob Storage an die HDI Spark-Runtime:
```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)
 
attach_storage_container(spark, "<storage account name>", "<storage key>”)
```

## <a name="conclusion"></a>Zusammenfassung
Da Azure ML Skripts durch Kopieren des gesamten Projektordners in den Zielserverkontext ausführt, müssen große Eingabe-, Ausgabe- und zwischengeschaltete Dateien mit großer Sorgfalt behandelt werden. Sie können den speziellen `outputs`-Ordner verwenden, also den freigegebenen Ordner, auf den über die Umgebungsvariable `AZUREML_NATIVE_SHARE_DIRECTORY` zugegriffen werden kann, oder externen permanenten Speicher für große Dateitransaktionen. 

## <a name="next-steps"></a>Nächste Schritte
- Sehen Sie sich die Seite [Configuring Experimentation Execution (Konfigurieren von Experimentausführungen)](experiment-execution-configuration-reference.md) an.
- Sehen Sie sich an, wie das Tutorialprojekt [Classifying Iris (Klassifizieren von Schwertlilien)](tutorial-classifying-iris-part-1.md) den Ordner `outputs` verwenden, um das trainierte Modell beizubehalten.
