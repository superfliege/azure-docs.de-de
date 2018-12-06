---
title: Neuerungen in Azure Machine Learning
description: In diesem Dokument werden die Updates für Azure Machine Learning beschrieben.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
author: hning86
ms.author: haining
ms.reviewer: j-martens
ms.date: 10/24/2018
ms.openlocfilehash: 6007a7e32e168ada529feb6aa24b8d572671d835
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291339"
---
# <a name="azure-machine-learning-service-release-notes"></a>Azure Machine Learning-Dienst – Anmerkungen zu dieser Version

Erfahren Sie in diesem Artikel mehr über die Versionen des Azure Machine Learning-Diensts. 

## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>Azure Machine Learning SDK für Python v0.1.80

+ **Wichtige Änderungen** 
  * Der Namespace *azureml.train.widgets* wurde in *azureml.widgets* verschoben.
  * *azureml.core.compute.AmlCompute* führt zur Veraltung der folgenden Klassen: *azureml.core.compute.BatchAICompute* und *azureml.core.compute.DSVMCompute*. Die letztgenannte Klasse wird in künftigen Versionen nicht mehr enthalten sein. Die AmlCompute-Klasse verfügt jetzt über eine einfachere Definition und benötigt lediglich Angaben für „vm_size“ und „max_nodes“. Sie skaliert Ihren Cluster automatisch von 0 auf den Wert von „max_nodes“, wenn ein Auftrag übermittelt wird. Unsere [Beispielnotebooks] (https://github.com/Azure/MachineLearningNotebooks/tree/master/training) wurden mit diesen Informationen aktualisiert und enthalten Beispiele zur Verwendung. Wir hoffen, dass Ihnen diese Vereinfachung gefällt – genauso wie viele weitere interessante Features in den nachfolgenden Releases!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning Data Prep SDK v0.5.1 

Erfahren Sie mehr zum Data Prep SDK, indem Sie die [Referenzdokumente](https://aka.ms/data-prep-sdk) lesen.
+ **Neue Features**
   * Es wurde eine neue DataPrep CLI erstellt, mit der DataPrep-Pakete ausgeführt werden können und das Datenprofil für ein Dataset oder einen Dataflow angezeigt werden kann.
   * Die SetColumnType-API wurde neu gestaltet, um die Nutzbarkeit zu verbessern.
   * „smart_read_file“ wurde in „auto_read_file“ umbenannt.
   * Datenschiefe und Kurtosis sind jetzt im Datenprofil enthalten.
   * Stichprobenentnahme mit geschichteten Stichproben ist möglich.
   * Das Lesen aus ZIP-Dateien, die CSV-Dateien enthalten, ist möglich.
   * Datasets können zeilenweise per zufälliger Aufteilung geteilt werden (z.B. in Test/Training-Sätze).
   * Alle Spaltendatentypen können aus einem Dataflow oder Datenprofil abgerufen werden, indem „.dtypes“ aufgerufen wird.
   * Die Zeilenanzahl kann aus einem Dataflow oder Datenprofil abgerufen werden, indem „.row_count“ aufgerufen wird.

+ **Fehlerbehebungen**
   * Fehler bei Konvertierung von „long“ in „double“ behoben 
   * Fehler bei Assert-Vorgang nach jedem Hinzufügen einer Spalte behoben 
   * FuzzyGrouping-Fehler behoben, bei dem in einigen Fällen Gruppen nicht erkannt wurden
   * Fehler bei Sortierfunktion behoben, damit die Sortierreihenfolge für mehrere Spalten beibehalten wird
   * Fehler mit Ausdrücken behoben bzw. an Verarbeitung durch Pandas angepasst
   * Fehler beim Lesen aus dem dbfs-Pfad behoben
   * Verständlichkeit von Fehlermeldungen verbessert 
   * Fehler behoben, der beim Lesen auf dem Remotecomputeziel mit dem AML-Token aufgetreten ist
   * Fehler für Linux DSVM behoben
   * Kein Absturz mehr, wenn in Zeichenfolgenprädikaten andere Werte als Zeichenfolgen enthalten sind
   * Assertionsfehler werden bei Dataflow-Fehler jetzt richtig behandelt
   * Per dbutils bereitgestellte Speicherorte werden unter Azure Databricks jetzt unterstützt

## <a name="2018-11-05"></a>05.11.2018

### <a name="azure-portal"></a>Azure-Portal 
Im Azure-Portal für den Azure Machine Learning-Dienst wurden folgende Updates vorgenommen:
  * Neue Registerkarte namens **Pipelines** für veröffentlichte Pipelines
  * Unterstützung für das Anfügen eines vorhandenen HDInsight-Clusters als Computeziel hinzugefügt

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>Azure Machine Learning SDK für Python v0.1.74

+ **Wichtige Änderungen** 
  * *Workspace.compute_targets, datastores, experiments, images, models* und *webservices* sind Eigenschaften, keine Methoden. Ersetzen Sie beispielsweise *Workspace.compute_targets()* durch *Workspace.compute_targets*.
  * *Run.get_context* ersetzt *Run.get_submitted_run*. Die letztgenannte Methode wird in künftigen Versionen entfernt.
  * Die Klasse *PipelineData* erwartet nun anstelle von „datastore_name“ ein datastore-Objekt als Parameter. Analog dazu akzeptiert *Pipeline* nun „default_datastore“ anstelle von „default_datastore_name“.

+ **Neue Features**
  * Das [Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) für Azure Machine Learning-Pipelines verwendet jetzt MPI-Schritte.
  * Das RunDetails-Widget für Jupyter-Notebooks wird aktualisiert, um eine Visualisierung der Pipeline anzuzeigen.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>Azure Machine Learning Data Prep SDK v0.4.0 
 
+ **Neue Features**
  * Typanzahl zum Datenprofil hinzugefügt 
  * Wertanzahl und Histogramm nun verfügbar
  * Mehr Perzentile im Datenprofil
  * Median in der Zusammenfassung verfügbar
  * Python 3.7 nun unterstützt
  * Wenn Sie einen Dataflow mit Datenspeichern in einem Datenvorbereitungspaket speichern, werden die Datenspeicherinformationen als Teil des Datenvorbereitungspakets gespeichert.
  * Schreibvorgänge für den Datenspeicher nun unterstützt 
        
+ **Behobene Fehler**
  * 64-Bit-Ganzzahlüberläufe ohne Vorzeichen werden nun unter Linux ordnungsgemäß behandelt.
  * Falscher Text für Nur-Text-Dateien in „smart_read“ behoben
  * Zeichenfolgenspaltentyp wird nun in der Metrikansicht angezeigt
  * Typanzahl korrigiert, sodass nun Wertarten (ValueKinds) angezeigt werden, die einem einzelnen Feldtyp (FieldType) zugeordnet sind (anstatt einzelne)
  * Bei „Write_to_csv“ tritt kein Fehler mehr auf, wenn der Pfad als Zeichenfolge angegeben wird.
  * Wenn bei Verwendung von „Ersetzen“ das Suchfeld leer ist, tritt kein Fehler mehr auf. 

## <a name="2018-10-12"></a>12.10.2018

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Azure Machine Learning SDK für Python v0.1.68

+ **Neue Features**
  * Unterstützung für mehrere Mandanten beim Erstellen eines neuen Arbeitsbereichs.

+ **Behobene Fehler**
  * Die Version der pynacl-Bibliothek muss beim Bereitstellen des Webdiensts nicht mehr angeheftet werden.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure Machine Learning – Datenaufbereitungs-SDK v0.3.0

+ **Neue Features**
  * Methode transform_partition_with_file(script_path) hinzugefügt, die es Benutzern ermöglicht, den Pfad einer auszuführenden Python-Datei zu übergeben

## <a name="2018-10-01"></a>01.10.2018

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Azure Machine Learning SDK für Python v0.1.65
[Version 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) enthält neue Features, mehr Dokumentation, Fehlerbehebungen und weitere [Beispielnotebooks](https://aka.ms/aml-notebooks).

Sehen Sie die [Liste der bekannten Probleme](resource-known-issues.md) an, um mehr über bekannte Fehler und Problemumgehungen zu erfahren.

+ **Wichtige Änderungen**
  * Workspace.experiments, Workspace.models, Workspace.compute_targets, Workspace.images, Workspace.web_services geben ein Wörterbuch zurück, bisher wurde eine Liste zurückgegeben. Weitere Informationen finden Sie in der [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py)-API-Dokumentation.

  * Der Normalized-Mean-Square-Fehler in den primären Metriken wurde mit automatisiertem maschinellem Lernen entfernt.

+ **HyperDrive**
  * Verschiedene HyperDrive-Fehlerbehebungen für Bayessche Wahrscheinlichkeit, verbesserte Leistung für Abrufaufrufe der Metriken. 
  * Tensorflow 1.10-Upgrade von 1.9 
  * Docker-Image-Optimierung für den Kaltstart. 
  * Aufträge melden jetzt den richtigen Status, selbst wenn sie mit einem anderen Fehlercode als 0 beendet werden. 
  * Überprüfung des RunConfig-Attributs in SDK. 
  * Das HyperDrive-Ausführungsobjekt unterstützt jetzt Abbrechen, ähnlich einer normalen Ausführung: Das Übergeben von Parametern ist nicht erforderlich. 
  * Verbesserte Widgets, um den Status von Dropdownwerten für verteilte Ausführungen und HyperDrive-Ausführungen beizubehalten. 
  * TensorBoard und andere Protokolldateien unterstützen „Fixed“ für den Parameter „Server“. 
  * Dienstseitige Intel(R) MPI-Unterstützung. 
  * Fehlerbehebung bei der Parameteroptimierung für verteilte Ausführungskorrekturen während der Überprüfung in BatchAI. 
  * Der Kontext-Manager identifiziert jetzt die primäre Instanz. 

+ **Azure-Portal-Benutzeroberfläche**
  * log_table() und log_row() werden in den Ausführungsdetails unterstützt. 
  * Erstellen Sie automatisch Diagramme für Tabellen und Zeilen mit 1,2 oder 3 numerischen Spalten und einer optionalen Rubrikspalte.

+ **Automatisiertes maschinelles Lernen**
  * Verbesserte Fehlerbehandlung und Dokumentation 
  * Behobene Leistungsprobleme beim Abrufen der Ausführungseigenschaft. 
  * Behobenes Problem beim Fortsetzen der Ausführung. 
  * Behobene Iterationsprobleme.
  * Behobener Fehler wegen fehlender Reaktion beim Training unter MAC OS.
  * Downsampling der durchschnittlichen PR/ROC-Kurve im benutzerdefinierten Überprüfungsszenario.
  * Zusätzliche Indexlogik wurde entfernt.
  * Filter aus der get_output-API entfernt.

+ **Pipelines**
  * Hinzugefügte Methode Pipeline.publish() zum direkten Veröffentlichen von Pipelines ohne vorhergehende Ausführung.   
  * Hinzugefügte Methode PipelineRun.get_pipeline_runs() zum Abfangen der Pipelineausführungen, die von einer veröffentlichten Pipeline generiert wurden.

+ **Project Brainwave**
  * Aktualisierte Unterstützung für neue KI-Modelle auf FPGAs verfügbar.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>Azure Machine Learning – Datenaufbereitungs-SDK v0.2.0
[Version 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) umfasst folgende Features und Fehlerbehebungen:

+ **Neue Features**
  * Unterstützung für 1-aus-n-Codierung
  * Unterstützung für Quantiltransformation
   
+ **Behobener Fehler:**
  * Funktioniert mit jeder Tornado-Version, ein Downgrade Ihrer Tornado-Version ist nicht erforderlich
  * Der Wert gilt für alle Werte, nicht nur für die höchsten drei

## <a name="2018-09-public-preview-refresh"></a>09.2018 (Aktualisierung der öffentlichen Vorschau)

Eine neue, vollständig aktualisierte Version von Azure Machine Learning: Weitere Informationen zu dieser Version: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/

## <a name="older-notes-sept-2017---jun-2018"></a>Altere Versionsanmerkungen: September 2017 – Juni 2018
### <a name="2018-05-sprint-5"></a>2018-05 (Sprint 5)

Mit diesem Release von Azure Machine Learning können Sie folgende Aktionen ausführen:
+ Bilder mit einer quantisierten Version von ResNet 50 mit Features versehen, einen Klassifizierer auf diesen Features basierend trainieren und [dieses Modell auf einem FPGA in Azure bereitstellen](../service/how-to-deploy-fpga-web-service.md) für Rückschlüsse mit extrem geringer Latenz.

+ Äußerst genaue Machine Learning- und Deep Learning-Modelle mit [benutzerdefinierten Azure Machine Learning-Paketen](../desktop-workbench/reference-python-package-overview.md) für die folgenden Domänen schnell erstellen und bereitstellen:
  + [Maschinelles Sehen](../desktop-workbench/how-to-build-deploy-image-classification-models.md)
  + [Textanalyse](../desktop-workbench/how-to-build-deploy-text-classification-models.md)
  + [Vorhersagen](../desktop-workbench/how-to-build-deploy-forecast-models.md)

### <a name="2018-03-sprint-4"></a>2018-03 (Sprint 4)
**Versionsnummer**: 0.1.1801.24353 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;([Suchen Sie Ihre Version.](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))


Bei vielen der unten angegebenen Aktualisierungen wurde Ihr Feedback direkt umgesetzt. Senden Sie weiter Feedback!

**Wichtige neue Features und Änderungen**

- Native Unterstützung der Ausführung von Skripts auf virtuellen Ubuntu-Remotecomputern in Ihrer eigenen Umgebung (zusätzlich zur Docker-basierten Remoteausführung)
- Neue Umgebung in der Workbench-App zum Erstellen von Computezielen und Laufzeitkonfigurationen (zusätzlich zur CLI-basierten Umgebung)
![Registerkarte für Umgebungen](media/azure-machine-learning-release-notes/environment-page.png)
- Anpassbare Ausführungsverlaufsberichte ![Abbildung neuer Ausführungsverlaufsberichte](media/azure-machine-learning-release-notes/new-run-history-reports.png)

**Detaillierte Aktualisierungen**

Die nachstehende Liste zeigt die detaillierten Aktualisierungen für jeden Komponentenbereich von Azure Machine Learning in diesem Sprint.

#### <a name="workbench-ui"></a>Workbench-Benutzeroberfläche
- Anpassbare Ausführungsverlaufsberichte
  - Verbesserte Diagrammkonfiguration für Ausführungsverlaufsberichte
    - Die verwendeten Einstiegspunkte können geändert werden.
    - Filter der obersten Ebene können hinzugefügt und geändert werden. ![Hinzufügen von Filtern](media/azure-machine-learning-release-notes/add-filters.jpg)
    - Diagramme und Statistiken können hinzugefügt/geändert und per Drag & Drop neu angeordnet werden.
    ![Erstellen neuer Diagramme](media/azure-machine-learning-release-notes/configure-charts.png)

  - CRUD für Ausführungsverlaufsberichte
  - Alle vorhandenen Listenansichtskonfigurationen für den Ausführungsverlauf wurden in serverseitige Berichte verschoben, die als Pipelines für Ausführungen der ausgewählten Einstiegspunkte fungieren.

- Registerkarte für Umgebungen
  - Fügen Sie Ihrem Projekt ganz einfach neue Computeziel- und Ausführungskonfigurationsdateien hinzu. ![Neues Computeziel](media/azure-machine-learning-release-notes/add-new-environments.png)
  - Verwalten und aktualisieren Sie Ihre Konfigurationsdateien über eine einfache, formularbasierte Benutzerumgebung.
  - Neue Schaltfläche zum Vorbereiten Ihrer Umgebungen für die Ausführung

- Leistungsverbesserungen für die Dateiliste auf der Randleiste

#### <a name="data-preparation"></a>Vorbereitung der Daten 
- Mit Azure Machine Learning Workbench können Sie jetzt anhand eines bekannten Spaltennamens nach einer Spalte suchen.


#### <a name="experimentation"></a>Experimentieren
- Azure Machine Learning Workbench unterstützt jetzt das native Ausführen Ihrer Skripts in Ihrer eigenen Python- oder PySpark-Umgebung. Für diese Funktion erstellt und verwaltet der Benutzer seine eigene Umgebung auf dem virtuellen Remotecomputer und verwendet Azure Machine Learning Workbench, um seine Skripts für dieses Ziel auszuführen. Weitere Informationen finden Sie unter [Konfigurieren des Azure Machine Learning-Experimentieren-Diensts](../desktop-workbench/experimentation-service-configuration.md). 

#### <a name="model-management"></a>Modellverwaltung
- Unterstützung der Anpassung der bereitgestellten Container: Ermöglicht das Anpassen des Containerimages durch Installation externer Bibliotheken mit „apt-get“ usw. Ist nicht mehr auf Bibliotheken beschränkt, die über pip installiert werden können. Weitere Informationen finden Sie in der [Dokumentation](../desktop-workbench/model-management-custom-container.md).
  - Verwenden Sie das Flag `--docker-file myDockerStepsFilename` und den Dateinamen in Manifest-, Image- oder Diensterstellungsbefehlen.
  - Beachten Sie, dass das Ubuntu-Basisimage nicht geändert werden kann.
  - Beispielbefehl: 
  
      ```shell
      $ az ml image create -n myimage -m mymodel.pkl -f score.py --docker-file mydockerstepsfile
      ```



### <a name="2018-01-sprint-3"></a>2018-01 (Sprint 3) 
**Versionsnummer**: 0.1.1712.18263  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;([Suchen Sie Ihre Version.](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Nachfolgend werden die Aktualisierungen und Verbesserungen in diesem Sprint beschrieben. Viele dieser Aktualisierungen sind eine direkte Umsetzung des Benutzerfeedbacks. 


Die nachstehende Liste zeigt die detaillierten Aktualisierungen für jeden Komponentenbereich von Azure Machine Learning in diesem Sprint.

- Aktualisierung des Authentifizierungsstapels erzwingt Anmeldung und Kontoauswahl beim Start

#### <a name="workbench"></a>Workbench
- Möglichkeit zum Installieren/Deinstallieren der App über „Programme hinzufügen/entfernen“
- Aktualisierung des Authentifizierungsstapels erzwingt Anmeldung und Kontoauswahl beim Start
- Verbesserte Oberfläche für einmaliges Anmelden (SSO) in Windows
- Benutzer, die mehreren Mandanten mit unterschiedlichen Anmeldeinformationen angehören, können sich jetzt bei Workbench anmelden

#### <a name="ui"></a>Benutzeroberfläche
- Allgemeine Verbesserungen und Fehlerbehebungen

#### <a name="notebooks"></a>Notebooks
- Allgemeine Verbesserungen und Fehlerbehebungen

#### <a name="data-preparation"></a>Vorbereitung der Daten 
- Verbesserte automatische Vorschläge beim Durchführen von Transformationen nach Beispiel
- Verbesserter Algorithmus für Musterhäufigkeitsprüfung
- Möglichkeit zum Senden von Beispieldaten und Feedback während der Durchführung von Transformationen nach Beispiel ![Screenshot mit dem Link zum Senden von Feedback bei der Transformation zur Spaltenableitung](media/azure-machine-learning-release-notes/SendFeedbackFromDeriveColumn.png)
- Verbesserungen an der Spark-Laufzeit
- Pyspark durch Scala ersetzt
- Korrektur eines Fehlers, durch den „Daten nicht anwendbar“ für den Time Series-Inspektor nicht geschlossen werden konnte 
- Korrektur eines Fehlers, bei dem bei der Datenvorbereitung für HDI keine Reaktion erfolgte

#### <a name="model-management-cli-updates"></a>CLI-Aktualisierungen für Modellverwaltung 
  - Für das Bereitstellen von Ressourcen ist es nicht mehr erforderlich, der Besitzer des Abonnements zu sein. Um die Bereitstellungsumgebung einzurichten, reicht der Zugriff „Mitwirkender“ für die Ressourcengruppe aus.
  - Für kostenlose Abonnements wurde die Einrichtung der lokalen Umgebung aktiviert. 

### <a name="2017-12-sprint-2-qfe"></a>2017-12 (Sprint 2 QFE) 
**Versionsnummer**: 0.1.1711.15323  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;([Suchen Sie Ihre Version.](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Dies ist das QFE (Quick Fix Engineering)-Release, eine Nebenversion. Sie bietet Korrekturen für verschiedene Telemetrieprobleme und unterstützt das Produktteam dabei, besseren Einblick in die Produktverwendung zu erhalten. Die gewonnenen Erkenntnisse werden dazu genutzt, die Produktbenutzerfreundlichkeit weiter zu verbessern. 

Zusätzlich werden zwei wichtige Aktualisierungen bereitgestellt:

- Korrektur eines Fehlers beim Vorbereiten der Daten, der verhinderte, dass der Time Series-Inspektor in Datenvorbereitungspaketen angezeigt wurde.
- Im Befehlszeilentool ist es nicht länger erforderlich, Machine Learning-Compute-ACS-Cluster als Azure-Abonnementbesitzer bereitzustellen. 

### <a name="2017-12-sprint-2"></a>2017-12 (Sprint 2)
**Versionsnummer**: 0.1.1711.15263  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;([Suchen Sie Ihre Version.](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Willkommen beim dritten Update von Azure Machine Learning. Dieses Update enthält Verbesserungen in der Workbench-App, der Befehlszeilenschnittstelle (CLI) und den Back-End-Diensten. Vielen Dank, dass Sie uns ein Lächeln oder Stirnrunzeln gesendet haben. Bei vielen der unten angegebenen Aktualisierungen wurde Ihr Feedback direkt umgesetzt. 

**Wichtige neue Features**
- [Unterstützung für SQL Server und Azure SQL-Datenbank als Datenquelle](../desktop-workbench/data-prep-appendix2-supported-data-sources.md#types) 
- [Deep Learning in Spark mit GPU-Unterstützung unter Verwendung von MMLSpark](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [Alle AML-Container sind nach der Bereitstellung mit Azure IoT Edge-Geräten kompatibel (keine zusätzlichen Schritte erforderlich)](https://aka.ms/aml-iot-edge-blog)
- Liste der registrierten Modelle und Detailansichten im Azure-Portal verfügbar
- Zugriff auf Computeziele unter Verwendung der Authentifizierung mit SSH-Schlüsseln zusätzlich zum Zugriff über Benutzername/Kennwort 
- Neue Musterhäufigkeitsprüfung bei der Datenvorbereitung 

**Detaillierte Aktualisierungen** Die nachstehende Liste zeigt die detaillierten Aktualisierungen für jeden Komponentenbereich von Azure Machine Learning in diesem Sprint.

#### <a name="installer"></a>Installer
- Der Installer kann eigenständig Updates ausführen, sodass Fehlerbehebungen und neue Features ohne eine erneute Installation unterstützt werden können.

#### <a name="workbench-authentication"></a>Workbench-Authentifizierung
- Verschiedene Fehlerbehebungen für das Authentifizierungssystem. Teilen Sie uns mit, wenn weiterhin Anmeldeprobleme auftreten.
- Änderungen an der Benutzeroberfläche erleichtern das Auffinden der Proxy-Manager-Einstellungen.

#### <a name="workbench"></a>Workbench
- Die schreibgeschützte Dateiansicht wird jetzt mit einem hellblauen Hintergrund angezeigt.
- Die Schaltfläche „Bearbeiten“ wurde nach rechts verschoben, damit sie leichter aufzufinden ist.
- Die Dateiformate „dsource“, „dprep“ und „ipynb“ können jetzt als unformatierter Text gerendert werden.
- Die Workbench verfügt nun über eine neue Bearbeitungsoberfläche, um den Benutzer an die Verwendung externer IDEs zum Bearbeiten von Skripts heranzuführen und die Workbench nur zum Bearbeiten von Dateitypen mit umfangreicher Bearbeitungsfunktionalität zu nutzen (z.B. Notebooks, Datenquellen, Datenvorbereitungspakete).
- Die Liste der Arbeitsbereiche und Projekte, auf die der Benutzer Zugriff hat, wird wesentlich schneller geladen.

#### <a name="data-preparation"></a>Vorbereitung der Daten 
- Mithilfe einer Musterhäufigkeitsprüfung können die Zeichenfolgenmuster in einer Spalte angezeigt werden. Sie können Ihre Daten auch mithilfe dieser Muster filtern. Die angezeigte Ansicht ähnelt der Prüfung für die Anzahl von Werten. Der Unterschied liegt darin, dass die Musterhäufigkeit anstelle der Anzahl eindeutiger Daten die Anzahl eindeutiger Datenmuster zeigt. Sie können auch einen Filter verwenden, mit dem alle Zeilen, die einem bestimmten Muster entsprechen, entweder angezeigt werden oder nicht angezeigt werden.

![Abbildung zur Musterhäufigkeitsprüfung für die Produktnummer](media/azure-machine-learning-release-notes/pattern-inspector-product-number.png)

- Leistungsverbesserungen bei gleichzeitiger Empfehlung von Grenzfällen zur Überprüfung in der Transformation „Derive Column by Example“ (Spalte nach Beispiel ableiten)

- [Unterstützung für SQL Server und Azure SQL-Datenbank als Datenquelle](../desktop-workbench/data-prep-appendix2-supported-data-sources.md#types) 

![Abbildung zum Erstellen einer neuen SQL Server-Datenquelle](media/azure-machine-learning-release-notes/sql-server-data-source.png)

- Die Ansicht „Auf einen Blick“ wurde für Zeilen- und Spaltenanzahl aktiviert.

![Abbildung zur Zeilen- und Spaltenanzahl auf einen Blick](media/azure-machine-learning-release-notes/row-col-count.png)

- Die Datenvorbereitung ist jetzt in allen Computekontexten aktiviert.
- Datenquellen mit Verwendung einer SQL Server-Datenbank sind in allen Computekontexten aktiviert.
- Rasterspalten zur Datenvorbereitung können nach Datentyp gefiltert werden.
- Ein Problem beim Konvertieren mehrerer Spalten in Datumswerte wurde behoben.
- Es wurde ein Problem behoben, durch das der Benutzer in „Derive Column by Example“ die Ausgabespalte als Quelle auswählen konnte, wenn der Name der Ausgabespalte im erweiterten Modus durch den Benutzer geändert wurde.

#### <a name="job-execution"></a>Auftragsausführung
Sie können ab sofort mithilfe der auf SSH-Schlüsseln basierenden Authentifizierung ein Remotedocker- oder Clustercomputeziel erstellen und darauf zugreifen, indem Sie die folgenden Schritte ausführen:
- Fügen Sie mithilfe des folgenden Befehls in der CLI ein Computeziel an.

    ```azure-cli
    $ az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
    ```
>[!NOTE]
>Die Option „-k“ (oder „--use-azureml-ssh-key“) im Befehl gibt an, dass ein SSH-Schlüssel generiert und verwendet werden soll.

- Azure ML Workbench generiert einen öffentlichen Schlüssel und gibt diesen in Ihrer Konsole aus. Melden Sie sich beim Computeziel mit demselben Benutzernamen an, und fügen Sie die Datei „~/.ssh/authorized_keys“ mit diesem öffentlichen Schlüssel an.

- Sie können dieses Computeziel vorbereiten und zur Ausführung verwenden, und Azure ML Workbench verwendet diesen Schlüssel für die Authentifizierung.  

Weitere Informationen zum Erstellen von Computezielen finden Sie unter [Konfigurieren des Azure Machine Learning-Experimentieren-Diensts](../desktop-workbench/experimentation-service-configuration.md).

#### <a name="visual-studio-tools-for-ai"></a>Visual Studio-Tools für AI
- Ab sofort werden die [Visual Studio-Tools für AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vstoolsai-vs2017) unterstützt. 

#### <a name="command-line-interface-cli"></a>Befehlszeilenschnittstelle (Command Line Interface, CLI)
- Der neu hinzugefügte Befehl `az ml datasource create` ermöglicht das Erstellen einer Datenquellendatei über die Befehlszeile.

#### <a name="model-management-and-operationalization"></a>Modellverwaltung und Operationalisierung
- [Alle AML-Container sind nach der Operationalisierung mit Azure IoT Edge-Geräten kompatibel (keine zusätzlichen Schritte erforderlich)](https://aka.ms/aml-iot-edge-blog) 
- Verbesserungen an den Fehlermeldungen in der o16n-CLI
- Fehlerbehebungen für die Portalbenutzeroberfläche für die Modellverwaltung  
- Konsistente Groß- und Kleinschreibung für Modellverwaltungsattribute auf der Detailseite
- Timeout für Aufrufe zur Echtzeitbewertung auf 60 Sekunden festgelegt
- Liste der registrierten Modelle und Detailansichten im Azure-Portal verfügbar

![Modelldetails im Portal](media/azure-machine-learning-release-notes/model-list.jpg)

![Modellübersicht im Portal](media/azure-machine-learning-release-notes/model-overview-portal.jpg)

#### <a name="mmlspark"></a>MMLSpark
- Deep Learning in Spark mit [GPU-Unterstützung](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- Unterstützung für Resource Manager-Vorlagen für eine einfache Ressourcenbereitstellung
- Unterstützung für das SparklyR-Ökosystem
- [AZTK-Integration](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

#### <a name="sample-projects"></a>Beispielprojekte
- Die [Iris](https://github.com/Azure/MachineLearningSamples-Iris)- und [MMLSpark](https://github.com/Azure/mmlspark)-Beispiele wurden mit der neuen Azure ML SDK-Version aktualisiert.

#### <a name="breaking-changes"></a>Wichtige Änderungen
- Die Option `--type` in `az ml computetarget attach` wurde zu einem Unterbefehl hochgestuft. 

    - `az ml computetarget attach --type remotedocker` ist jetzt `az ml computetarget attach remotedocker`
    - `az ml computetarget attach --type cluster` ist jetzt `az ml computetarget attach cluster`

### <a name="2017-11-sprint-1"></a>2017-11 (Sprint 1) 
**Versionsnummer**: 0.1.1710.31013  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;([Suchen Sie Ihre Version.](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

In diesem Release haben wir Verbesserungen in den Bereichen Sicherheit, Stabilität und Wartbarkeit in der Workbench-App, der CLI und für die Back-End-Diensteebene vorgenommen. Vielen Dank, dass Sie uns ein Lächeln oder Stirnrunzeln gesendet haben. Bei vielen der unten angegebenen Aktualisierungen handelt es sich um direkte Ergebnisse aufgrund Ihres Feedbacks. Senden Sie weiter Feedback!

#### <a name="notable-new-features"></a>Wichtige neue Features
- Azure ML ist jetzt in zwei neuen Azure-Regionen verfügbar: **Europa, Westen** und **Asien, Südosten**. Hierdurch werden die vorherigen Regionen **USA, Osten 2**, **USA, Westen-Mitte** und **Australien, Osten** erweitert, sodass es insgesamt fünf Bereitstellungsregionen sind.
- Wir haben in der Workbench-App die Hervorhebung der Python-Codesyntax aktiviert, um das Lesen und Bearbeiten von Python-Quellcode zu vereinfachen. 
- Sie können Ihre bevorzugte IDE jetzt direkt aus einer Datei starten, anstatt über das gesamte Projekt.  Wenn Sie in Workbench eine Datei öffnen und dann auf „Bearbeiten“ klicken, wird Ihre IDE (derzeit werden VS Code und PyCharm unterstützt) mit der aktuellen Datei und dem dazugehörigen Projekt gestartet.  Sie können auch auf den Pfeil der Schaltfläche „Bearbeiten“ klicken, um die Datei im Workbench-Text-Editor zu bearbeiten.  Die Dateien sind schreibgeschützt, bis Sie auf „Bearbeiten“ klicken, um versehentliche Änderungen zu verhindern.
- Version 2.1.0 der beliebten Bibliothek `matplotlib` für das Zeichnen ist jetzt im Lieferumfang der Workbench-App enthalten.
- Wir haben die .NET Core-Version für die  Datenvorbereitungs-Engine auf 2.0 aktualisiert. Es ist dadurch nicht mehr erforderlich, während der Installation der App unter macOS den Schritt „brew-install openssl“ auszuführen. Außerdem ist dies die Grundlage noch besserer Features für die Datenvorbereitung, die für die nahe Zukunft geplant sind. 
- Wir haben eine versionsspezifische App-Startseite aktiviert, damit Sie relevantere Versionshinweise und Updateaufforderungen je nach Ihrer aktuellen App-Version erhalten.
- Auch wenn Ihr lokaler Benutzername eine Leerstelle enthält, kann die Anwendung jetzt erfolgreich installiert werden. 

#### <a name="detailed-updates"></a>Detaillierte Aktualisierungen
Unten ist eine Liste mit detaillierten Aktualisierungen für jeden Komponentenbereich von Azure Machine Learning in diesem Sprint angegeben.

##### <a name="installer"></a>Installer
- Der App-Installer bereinigt nun das Installationsverzeichnis, das von der älteren Version der App erstellt wurde.
- Es wurde ein Fehler behoben, bei dem der Installer unter macOS High Sierra bei 100% hängen bleibt.
- Es ist jetzt ein direkter Link zum Installerverzeichnis vorhanden, damit Benutzer die Installerprotokolle prüfen können, falls die Installation nicht erfolgreich ist.
- Die Installation funktioniert nun für Benutzer, deren Benutzername eine Leerstelle enthält.

##### <a name="workbench-authentication"></a>Workbench-Authentifizierung
- Es ist Unterstützung für die Authentifizierung im Proxy-Manager vorhanden.
- Das Anmelden funktioniert jetzt auch, wenn sich Benutzer hinter einer Firewall befinden. 
- Wenn Benutzer über Experimentieren-Konten in mehreren Azure-Regionen verfügen und eine Region nicht verfügbar ist, hängt die App nicht mehr.
- Wenn die Authentifizierung nicht abgeschlossen ist und das dazugehörige Dialogfeld noch sichtbar ist, versucht die App nicht mehr, den Arbeitsbereich aus dem lokalen Cache zu laden.

##### <a name="workbench-app"></a>Workbench-App
- Die Hervorhebung der Python-Codesyntax ist im Text-Editor aktiviert.
- Mit der Schaltfläche „Bearbeiten“ im Text-Editor können Sie die Datei entweder in einer IDE (VS Code und PyCharm werden unterstützt) oder im integrierten Text-Editor bearbeiten.
- Der Text-Editor befindet sich standardmäßig im schreibgeschützten Modus. 
- Der Anzeigezustand der Schaltfläche „Speichern“ wird nach dem Speichern der aktuellen Datei, die dann keine ungespeicherten Änderungen enthält, jetzt in den deaktivierten Zustand geändert.
- Workbench speichert _alle_ ungespeicherten Dateien, wenn Sie eine Ausführung initiieren.
- Workbench speichert den zuletzt verwendeten Arbeitsbereich auf dem lokalen Computer, damit er automatisch geöffnet werden kann.
- Es kann jetzt nur noch eine einzelne Instanz von Workbench ausgeführt werden. Bisher konnten mehrere Instanzen gestartet werden, und es konnte zu Problemen kommen, wenn darin an demselben Projekt gearbeitet wurde.
- Im Menü „Datei“ wurde die Option „Open Project...“ (Projekt öffnen...) in „Add Existing Folder as Project...“ (Vorhandenen Ordner als Projekt hinzufügen...) umbenannt. 
- Registerkarten können nun viel schneller gewechselt werden.
- Dem Dialogfeld „Configuring IDE“ (IDE konfigurieren) wurden Hilfelinks hinzugefügt.
- Im Feedbackformular wird jetzt die E-Mail-Adresse gespeichert, die Sie beim letzten Besuch eingegeben haben.
- Im Formular ist der Textbereich für Lächeln und Stirnrunzeln jetzt größer, damit Sie uns mehr Feedback senden können! 
- Der Hilfetext zum Switch `--owner` unter `az ml workspace create` wurde korrigiert.
- Wir haben das Dialogfeld „About“ (Info) hinzugefügt, damit Benutzer die Versionsnummer der App leicht anzeigen und kopieren können.
- Dem Menü „Hilfe“ wurde das Menüelement „Feature vorschlagen“ hinzugefügt.
- Der Name des Experimentieren-Kontos ist jetzt in der App-Titelleiste sichtbar und wird vor dem App-Namen „Azure Machine Learning Workbench“ angezeigt.
- Basierend auf der erkannten App-Version wird jetzt eine versionsspezifische App-Homepage angezeigt.

##### <a name="data-preparation"></a>Vorbereitung der Daten 
- Eine externe Website kann über den Karteninspektor nicht mehr geladen werden, um potenzielle Sicherheitsprobleme zu verhindern.
- Die Inspektoren für Histogramm und Wertanzahl enthalten jetzt eine Option zum Anzeigen des Graphen mit einer logarithmischen Skala.
- Bei Durchführung einer Berechnung wird im Balken zur Datenqualität jetzt eine andere Farbe angezeigt, um den Berechnungszustand anzugeben.
- Spaltenmetriken zeigen jetzt statistische Daten für Spalten mit Kategoriewerten an.
- Das letzte Zeichen des Datenquellennamens wird nicht mehr abgeschnitten.
- Das Datenvorbereitungspaket bleibt beim Wechseln von Registerkarten jetzt geöffnet, um die Leistung deutlich zu erhöhen.
- In der Datenquelle ändert sich die Reihenfolge der Spalten beim Wechseln zwischen Daten- und Metrikansicht nicht mehr.
- Wenn eine ungültige `.dprep`- oder `.dsource`-Datei geöffnet wird, stürzt Workbench nicht mehr ab.
- Im Datenvorbereitungspaket kann der relative Pfad jetzt für die Ausgabe in der Transformation _Write to CSV_ (In CSV schreiben) verwendet werden.
- Mit der Transformation _Keep Column_ (Spalte beibehalten) können Benutzer bei der Bearbeitung jetzt zusätzliche Spalten hinzufügen.
- Über das Menü _Replace this_ (Ersetzen) wird jetzt das Dialogfeld _Replace Value_ (Wert ersetzen) geöffnet.
- Mit _Replace Value_ (Wert ersetzen) werden Funktionen jetzt wie erwartet transformiert, und es wird kein Fehler mehr ausgelöst.
- Für das Datenvorbereitungspaket wird jetzt der absolute Pfad verwendet, wenn auf Datendateien außerhalb des Projektordners verwiesen wird, sodass das Paket im lokalen Kontext mit dem absoluten Pfad zur Datendatei ausgeführt werden kann.
- _Full file_ (Vollständige Datei) wird jetzt als Strategie für die Stichprobenentnahme unterstützt, wenn Azure Blob als Datenquelle verwendet wird.
- Generierter Python-Code (aus dem Datenvorbereitungspaket) enthält jetzt Wagenrückläufe und Zeilenvorschübe, um die Benutzerfreundlichkeit für Windows zu erhöhen.
- In der Dropdownliste _Auswählen von Metriken_ wird „property“ jetzt ausgeblendet, wenn zur Datenansicht gewechselt wird.
- In Workbench können Parquet-Dateien jetzt auch verarbeitet werden, wenn die Python-Laufzeit verwendet wird. Bisher konnte beim Verarbeiten von Parquet-Dateien nur Spark verwendet werden. 
- Das Herausfiltern von Werten einer Spalte mit dem Datentyp _date_ führt nicht mehr dazu, dass die Datenvorbereitungs-Engine abstürzt.
- In der Metrikansicht werden jetzt Aktualisierungen der Strategie für die Stichprobenentnahme akzeptiert.
- Remoteaufträge für die Stichprobenentnahme funktionieren jetzt richtig.

##### <a name="job-execution"></a>Auftragsausführung
- Das Argument ist jetzt im Ausführungsverlauf-Datensatz enthalten.
- In der CLI ausgelöste Aufträge werden jetzt automatisch im Auftragsbereich „Ausführungsverlauf“ angezeigt.
- Im Auftragsbereich werden jetzt Aufträge angezeigt, die von Gastbenutzern erstellt und dem Azure AD-Mandanten hinzugefügt wurden.
- Die Aktionen zum Abbrechen und Löschen im Auftragsbereich sind jetzt stabiler.
- Beim Klicken auf die Schaltfläche „Ausführen“ wird jetzt eine Fehlermeldung ausgelöst, wenn das Format der Konfigurationsdateien fehlerhaft ist.
- Beim Beenden der App kommt es nicht mehr zu Konflikten mit Aufträgen, die in der CLI ausgelöst wurden.
- Für Aufträge, die in der CLI ausgelöst wurden, wird die „standard-out“-Ausgabe jetzt auch nach einer Stunde der Ausführung fortgesetzt.
- Es werden bessere Fehlermeldungen angezeigt, wenn die Ausführung des Datenvorbereitungspakets in Python/PySpark fehlschlägt.
- Für `az ml experiment clean` werden jetzt auch Docker-Images auf dem virtuellen Remotecomputer bereinigt.
- `az ml experiment clean` funktioniert für das lokale Ziel unter macOS jetzt richtig.
- Fehlermeldungen für Docker-Ausführungen (lokal oder remote) wurden bereinigt und sind einfacher zu lesen.
- Es wird eine bessere Fehlermeldung angezeigt, wenn der Name des Hauptknotens für den HDInsight-Cluster bei Anfügung als Ausführungsziel nicht richtig formatiert ist.
- Es wird eine bessere Fehlermeldung angezeigt, wenn das Geheimnis im Anmeldeinformationsdienst nicht gefunden wird. 
- Die MMLSpark-Bibliothek wurde aktualisiert, damit Apache Spark 2.2 unterstützt wird.
- MMLSpark enthält jetzt eine Transformation für die Antragstellercodierung („Mesh Encoding“) für medizinische Dokumente.
- Version 2.1.0 von `matplotlib` ist jetzt im Lieferumfang von Workbench enthalten.

##### <a name="jupyter-notebook"></a>Jupyter Notebook
- Die Notebook-Namenssuche funktioniert in der Ansicht „Notebooks“ jetzt richtig.
- Sie können ein Notebook jetzt in der Ansicht „Notebooks“ löschen.
- Das neue Magic-Element `%upload_artifact` wurde hinzugefügt, um Dateien, die in der Notebook-Ausführungsumgebung erzeugt wurden, in den Ausführungsverlauf-Datenspeicher hochzuladen.
- Kernel-Fehler werden jetzt im Notebook-Auftragsstatus angezeigt, um das Debuggen zu vereinfachen.
- Der Jupyter-Server wird jetzt richtig heruntergefahren, wenn sich der Benutzer von der App abmeldet.

##### <a name="azure-portal"></a>Azure-Portal
- Das Experimentieren-Konto und das Konto für die Modellverwaltung können jetzt in zwei neuen Azure-Regionen erstellt werden: „Europa, Westen“ und „Asien, Südosten“.
- Der DevTest-Plan für das Konto für die Modellverwaltung ist jetzt nur verfügbar, wenn er im Abonnement als erster Plan erstellt wird. 
- Der Hilfelink im Azure-Portal wurde aktualisiert und führt jetzt zur richtigen Seite der Dokumentation.
- Das Beschreibungsfeld wurde von der Seite mit den Details zum Docker-Image entfernt, da es nicht benötigt wird.
- Der Seite mit den Details zum Webdienst wurden weitere Details hinzugefügt, z.B. AppInsights und Einstellungen für die automatische Skalierung.
- Die Seite für die Modellverwaltung wird jetzt auch gerendert, wenn Cookies von Drittanbietern im Browser deaktiviert sind. 

##### <a name="operationalization"></a>Operationalisierung
- Der Webdienst mit „score“ im Namen führt nicht mehr zu einem Fehler.
- Benutzer können jetzt eine Bereitstellungsumgebung erstellen, für die nur der Zugriff vom Typ „Mitwirkender“ auf eine Azure-Ressourcengruppe oder das Abonnement besteht. Der Besitzerzugriff auf das gesamte Abonnement wird nicht mehr benötigt.
- Für die Operationalisierungs-CLI ist unter Linux jetzt die automatische Vervollständigung für Registerkarten möglich.
- Der Imageerstellungsdienst unterstützt jetzt das Erstellen von Images für Azure IoT-Dienste und -Geräte.

##### <a name="sample-projects"></a>Beispielprojekte
- Beispielprojekt [_Klassifizieren von Iris_](../desktop-workbench/tutorial-classifying-iris-part-1.md):
    - `iris_pyspark.py` wird umbenannt in `iris_spark.py`.
    - `iris_score.py` wird umbenannt in `score_iris.py`.
    - `iris.dprep` und `iris.dsource` wurden aktualisiert, um die neuesten Aktualisierungen für die Datenvorbereitungs-Engine widerzuspiegeln.
    - Das Notebook `iris.ipynb` wurde geändert, damit es in einem HDInsight-Cluster funktioniert.
    - Der Ausführungsverlauf wurde in Zellen des Notebooks `iris.ipynb` aktiviert.
- Für das Beispielprojekt [_Advanced Data Prep using Bike Share Data_](../desktop-workbench/tutorial-bikeshare-dataprep.md) (Erweiterte Datenaufbereitung mit Daten für Leihfahrräder) wurde der Schritt „Handle Error Value“ (Behandeln des Fehlerwerts) korrigiert.
- Das `docker.runconfig`-Format des Beispielprojekts [_MMLSpark on Adult Census Data_](https://github.com/Azure/MachineLearningSamples-mmlspark) (MMLSpark für Volkszählungsdaten) wurde von JSON in YAML aktualisiert.
- Das `docker.runconfig`-Format des Beispielprojekts [_Distributed Hyperparameter Tuning_](../desktop-workbench/scenario-distributed-tuning-of-hyperparameters.md) (Optimierung von verteilten Hyperparametern) wurde von JSON in YAML aktualisiert.
- Es ist ein neues Beispielprojekt mit dem Namen [_Bildklassifizierung mit CNTK_](../desktop-workbench/scenario-image-classification-using-cntk.md) verfügbar.


### <a name="2017-10-sprint-0"></a>2017-10 (Sprint 0) 
**Versionsnummer**: 0.1.1710.31013  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;([Suchen Sie Ihre Version.](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Willkommen beim ersten Update für Azure Machine Learning Workbench, das auf unsere erste, auf der Microsoft Ignite 2017-Konferenz vorgestellte Public Preview-Version folgt. Die wichtigsten Änderungen in dieser Version sind Korrekturen für mehr Zuverlässigkeit und Stabilität.  Einige der von uns aufgegriffenen kritischen Punkte sind u.a:

#### <a name="new-features"></a>Neue Funktionen
- macOS High Sierra wird nun unterstützt.

#### <a name="bug-fixes"></a>Fehlerbehebungen
##### <a name="workbench-experience"></a>Workbench-Oberfläche
- Beim Ziehen und Ablegen einer Datei in Workbench stürzt Workbench ab.
- Das Terminalfenster in Visual Studio Code, das als IDE für Workbench konfiguriert ist, erkennt keine Befehle des Typs _az ml_.

##### <a name="workbench-authentication"></a>Workbench-Authentifizierung
Wir haben eine Reihe von Änderungen vorgenommen, um verschiedene gemeldete Anmelde- und Authentifizierungsprobleme zu korrigieren.
- Das Authentifizierungsfenster wird ständig eingeblendet, insbesondere wenn die Internetverbindung nicht stabil ist.
- Korrigierte Zuverlässigkeitsprobleme im Zusammenhang mit dem Ablauf von Authentifizierungstoken.
- In einigen Fällen wird das Authentifizierungsfenster zweimal angezeigt.
- Im Workbench-Hauptfenster wird immer noch die Meldung zur laufenden Authentifizierung angezeigt, wenn der Authentifizierungsvorgang abgeschlossen ist und das Popupdialogfeld bereits geschlossen wurde.
- Wenn keine Internetverbindung besteht, wird der Authentifizierungsdialog mit einem leeren Bildschirm angezeigt.

##### <a name="data-preparation"></a>Vorbereitung der Daten 
- Wenn ein bestimmter Wert gefiltert wird, werden auch Fehler und fehlende Werte herausgefiltert.
- Wenn Sie eine Stichprobenstrategie ändern, werden nachfolgende vorhandene Join-Vorgänge entfernt.
- Beim Ersetzen der Transformation „Fehlender Wert“ wird NaN nicht berücksichtigt.
- Datumstypableitung bewirkt eine Ausnahme, wenn NULL-Wert gefunden wird.

##### <a name="job-execution"></a>Auftragsausführung
- Es gibt keine eindeutige Fehlermeldung, wenn die Auftragsausführung den Projektordner nicht hochladen kann, weil die Größenbeschränkung überschritten wurde.
- Wenn das Python-Skript des Benutzers das Arbeitsverzeichnis ändert, werden die Dateien, die in die Ausgabeordner geschrieben wurden, nicht nachverfolgt. 
- Wenn sich das aktive Azure-Abonnement von dem des aktuellen Projekts unterscheidet, führt die Auftragsübermittlung zum Fehler 403.
- Wenn Docker nicht vorhanden ist, wird keine eindeutige Fehlermeldung zurückgegeben, sobald der Benutzer versucht, Docker als Ausführungsziel zu verwenden.
- Die RUNCONFIG-Datei wird nicht automatisch gespeichert, wenn der Benutzer auf die Schaltfläche _Ausführen_ klickt.

##### <a name="jupyter-notebook"></a>Jupyter Notebook
- Notebook-Server kann nicht gestartet werden, wenn der Benutzer diesen mit bestimmten Anmeldungstypen verwendet.
- Notebook-Serverfehlermeldungen werden in den für Benutzer sichtbaren Protokollen nicht angezeigt.

##### <a name="azure-portal"></a>Azure-Portal
- Wenn Sie das Design „Dunkel“ des Azure-Portals auswählen, wird das Blatt „Modellverwaltung“ als schwarzes Feld angezeigt.

##### <a name="operationalization"></a>Operationalisierung
- Die Wiederverwendung eines Manifests zur Aktualisierung eines Webdiensts führt zu einem neu erstellten Docker-Image mit einem Zufallsnamen.
- Webdienstprotokolle können nicht aus einem Kubernetes-Cluster abgerufen werden.
- Irreführende Fehlermeldung wird ausgegeben, wenn der Benutzer versucht, ein Modellverwaltungskonto oder ein ML-Computekonto zu erstellen. Zudem treten Berechtigungsprobleme auf.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Übersicht für [Azure Machine Learning](../service/overview-what-is-azure-ml.md).
