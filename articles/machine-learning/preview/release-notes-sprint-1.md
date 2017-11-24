---
title: Anmerkungen zu dieser Version von Azure ML Workbench (Sprint 1, November 2017)
description: "In diesem Dokument werden die Aktualisierungen für die Version „Sprint 1“ von Azure ML detailliert beschrieben."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/06/2017
ms.openlocfilehash: a4945c77be5763ffeda328184149f712572937c0
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="sprint-1---november-2017"></a>Sprint 1 – November 2017 

**Versionsnummer: 0.1.1710.31013**

Willkommen beim zweiten Update von Azure Machine Learning Workbench. Wir arbeiten ständig an Verbesserungen in den Bereichen Sicherheit, Stabilität und Wartbarkeit in der Workbench-App, der CLI und für die Back-End-Diensteebene. Vielen Dank, dass Sie uns ein Lächeln oder Stirnrunzeln gesendet haben. Bei vielen der unten angegebenen Aktualisierungen handelt es sich um direkte Ergebnisse aufgrund Ihres Feedbacks. Senden Sie weiter Feedback!

## <a name="notable-new-features"></a>Wichtige neue Features
- Azure ML ist jetzt in zwei neuen Azure-Regionen verfügbar: **Europa, Westen** und **Asien, Südosten**. Hierdurch werden die vorherigen Regionen **USA, Osten 2**, **USA, Westen-Mitte** und **Australien, Osten** erweitert, sodass es insgesamt fünf Bereitstellungsregionen sind.
- Wir haben in der Workbench-App die Hervorhebung der Python-Codesyntax aktiviert, um das Lesen und Bearbeiten von Python-Quellcode zu vereinfachen. 
- Sie können Ihre bevorzugte IDE jetzt direkt aus einer Datei starten, anstatt über das gesamte Projekt.  Wenn Sie in Workbench eine Datei öffnen und dann auf „Bearbeiten“ klicken, wird Ihre IDE (derzeit werden VS Code und PyCharm unterstützt) mit der aktuellen Datei und dem dazugehörigen Projekt gestartet.  Sie können auch auf den Pfeil der Schaltfläche „Bearbeiten“ klicken, um die Datei im Workbench-Text-Editor zu bearbeiten.  Die Dateien sind schreibgeschützt, bis Sie auf „Bearbeiten“ klicken, um versehentliche Änderungen zu verhindern.
- Version 2.1.0 der beliebten Bibliothek `matplotlib` für das Zeichnen ist jetzt im Lieferumfang der Workbench-App enthalten.
- Wir haben die .NET Core-Version für das Datenvorbereitungsmodul auf 2.0 aktualisiert. Es ist dadurch nicht mehr erforderlich, während der Installation der App unter macOS den Schritt „brew-install openssl“ auszuführen. Außerdem ist dies die Grundlage noch besserer Features für die Datenvorbereitung, die für die nahe Zukunft geplant sind. 
- Wir haben eine versionsspezifische App-Startseite aktiviert, damit Sie relevantere Versionshinweise und Updateaufforderungen je nach Ihrer aktuellen App-Version erhalten.
- Auch wenn Ihr lokaler Benutzername eine Leerstelle enthält, kann die Anwendung jetzt erfolgreich installiert werden. 

## <a name="detailed-updates"></a>Detaillierte Aktualisierungen
Unten ist eine Liste mit detaillierten Aktualisierungen für jeden Komponentenbereich von Azure Machine Learning in diesem Sprint angegeben.

### <a name="installer"></a>Installer
- Der App-Installer bereinigt nun das Installationsverzeichnis, das von der älteren Version der App erstellt wurde.
- Es wurde ein Fehler behoben, bei dem der Installer unter macOS High Sierra bei 100% hängen bleibt.
- Es ist jetzt ein direkter Link zum Installerverzeichnis vorhanden, damit Benutzer die Installerprotokolle prüfen können, falls die Installation nicht erfolgreich ist.
- Die Installation funktioniert nun für Benutzer, deren Benutzername eine Leerstelle enthält.

### <a name="workbench-authentication"></a>Workbench-Authentifizierung
- Es ist Unterstützung für die Authentifizierung im Proxy-Manager vorhanden.
- Das Anmelden funktioniert jetzt auch, wenn sich Benutzer hinter einer Firewall befinden. 
- Wenn Benutzer über Experimentieren-Konten in mehreren Azure-Regionen verfügen und eine Region nicht verfügbar ist, hängt die App nicht mehr.
- Wenn die Authentifizierung nicht abgeschlossen ist und das dazugehörige Dialogfeld noch sichtbar ist, versucht die App nicht mehr, den Arbeitsbereich aus dem lokalen Cache zu laden.

### <a name="workbench-app"></a>Workbench-App
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

### <a name="data-preparation"></a>Vorbereitung der Daten 
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
- Das Herausfiltern von Werten einer Spalte mit dem Datentyp _date_ führt nicht mehr dazu, dass das Datenvorbereitungsmodul abstürzt.
- In der Metrikansicht werden jetzt Aktualisierungen der Strategie für die Stichprobenentnahme akzeptiert.
- Remoteaufträge für die Stichprobenentnahme funktionieren jetzt richtig.

### <a name="job-execution"></a>Auftragsausführung
- Das Argument ist jetzt im Ausführungsverlauf-Datensatz enthalten.
- In der CLI ausgelöste Aufträge werden jetzt automatisch im Auftragsbereich „Ausführungsverlauf“ angezeigt.
- Im Auftragsbereich werden jetzt Aufträge angezeigt, die von Gastbenutzern erstellt und dem AAD-Mandanten hinzugefügt wurden.
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

### <a name="jupyter-notebook"></a>Jupyter Notebook
- Die Notebook-Namenssuche funktioniert in der Ansicht „Notebooks“ jetzt richtig.
- Sie können ein Notebook jetzt in der Ansicht „Notebooks“ löschen.
- Das neue Magic-Element `%upload_artifact` wurde hinzugefügt, um Dateien, die in der Notebook-Ausführungsumgebung erzeugt wurden, in den Ausführungsverlauf-Datenspeicher hochzuladen.
- Kernel-Fehler werden jetzt im Notebook-Auftragsstatus angezeigt, um das Debuggen zu vereinfachen.
- Der Jupyter-Server wird jetzt richtig heruntergefahren, wenn sich der Benutzer von der App abmeldet.

### <a name="azure-portal"></a>Azure-Portal
- Das Experimentieren-Konto und das Konto für die Modellverwaltung können jetzt in zwei neuen Azure-Regionen erstellt werden: „Europa, Westen“ und „Asien, Südosten“.
- Der DevTest-Plan für das Konto für die Modellverwaltung ist jetzt nur verfügbar, wenn er im Abonnement als erster Plan erstellt wird. 
- Der Hilfelink im Azure-Portal wurde aktualisiert und führt jetzt zur richtigen Seite der Dokumentation.
- Das Beschreibungsfeld wurde von der Seite mit den Details zum Docker-Image entfernt, da es nicht benötigt wird.
- Der Seite mit den Details zum Webdienst wurden weitere Details hinzugefügt, z.B. AppInsights und Einstellungen für die automatische Skalierung.
- Die Seite für die Modellverwaltung wird jetzt auch gerendert, wenn Cookies von Drittanbietern im Browser deaktiviert sind. 

### <a name="operationalization"></a>Operationalisierung
- Der Webdienst mit „score“ im Namen führt nicht mehr zu einem Fehler.
- Benutzer können jetzt eine Bereitstellungsumgebung erstellen, für die nur der Zugriff vom Typ „Mitwirkender“ auf eine Azure-Ressourcengruppe oder das Abonnement besteht. Der Besitzerzugriff auf das gesamte Abonnement wird nicht mehr benötigt.
- Für die Operationalisierungs-CLI ist unter Linux jetzt die automatische Vervollständigung für Registerkarten möglich.
- Der Imageerstellungsdienst unterstützt jetzt das Erstellen von Images für Azure IoT-Dienste und -Geräte.

### <a name="sample-projects"></a>Beispielprojekte
- Beispielprojekt [_Klassifizieren von Iris_](./tutorial-classifying-iris-part-1.md):
    - `iris_pyspark.py` wird umbenannt in `iris_spark.py`.
    - `iris_score.py` wird umbenannt in `score_iris.py`.
    - `iris.dprep` und `iris.dsource` wurden aktualisiert, um die neuesten Aktualisierungen für das Datenvorbereitungsmodul widerzuspiegeln.
    - Das Notebook `iris.ipynb` wurde geändert, damit es in einem HDInsight-Cluster funktioniert.
    - Der Ausführungsverlauf wurde in Zellen des Notebooks `iris.ipynb` aktiviert.
- Für das Beispielprojekt [_Advanced Data Prep using Bike Share Data_](./tutorial-bikeshare-dataprep.md) (Erweiterte Datenaufbereitung mit Daten für Leihfahrräder) wurde der Schritt „Handle Error Value“ (Behandeln des Fehlerwerts) korrigiert.
- Das `docker.runconfig`-Format des Beispielprojekts [_MMLSpark on Adult Census Data_](https://github.com/Azure/MachineLearningSamples-mmlspark) (MMLSpark für Volkszählungsdaten) wurde von JSON in YAML aktualisiert.
- Das `docker.runconfig`-Format des Beispielprojekts [_Distributed Hyperparameter Tuning_](./scenario-distributed-tuning-of-hyperparameters.md) (Optimierung von verteilten Hyperparametern) wurde von JSON in YAML aktualisiert.
- Es ist ein neues Beispielprojekt mit dem Namen [_Bildklassifizierung mit CNTK_](./scenario-image-classification-using-cntk.md) verfügbar.
