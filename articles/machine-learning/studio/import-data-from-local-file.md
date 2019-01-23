---
Titel: Importieren von Daten aus Datei titleSuffix: Azure Machine Learning Studio – Beschreibung: Erfahren Sie, wie Sie eine Trainingsdatendatei von der Festplatte in Azure Machine Learning Studio hochladen. So wird ein Datasetmodul im Arbeitsbereich erstellt.
Dienste: machine-learning ms.service: machine-learning ms.component: studio ms.topic: Artikel

Autor: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18 ms.date: 20.03.2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-azure-machine-learning-studio"></a>Importieren von Trainingsdaten aus einer Datei auf der Festplatte in Azure Machine Learning Studio

Erfahren Sie, wie Sie eine Datendatei zur Verwendung als Trainingsdaten von der Festplatte in Azure Machine Learning Studio hochladen. Mit dem Import der Datendatei erhalten Sie ein Datasetmodul, das zur Verwendung in Ihrem Arbeitsbereich bereit ist.

## <a name="steps-to-import-data-from-a-local-file"></a>Schritte zum Importieren von Daten aus einer lokalen Datei
Sie können wie folgt Daten von einer lokalen Festplatte importieren:

1. Klicken Sie unten im Machine Learning Studio-Fenster auf **+NEW** .
2. Wählen Sie **DATASET** und **FROM LOCAL FILE** aus.
3. Navigieren Sie im Dialogfeld **Upload a new dataset** zu der Datei, die Sie hochladen möchten.
4. Geben Sie einen Namen ein, identifizieren Sie den Datentyp, und geben Sie optional eine Beschreibung ein. Eine Beschreibung wird empfohlen: Damit können Sie beliebige Merkmale der Daten erfassen, die Sie bei zukünftiger Nutzung der Daten berücksichtigen möchten.
5. Mit dem Kontrollkästchen **This is the new version of an existing dataset** können Sie ein vorhandenes Dataset mit neuen Daten aktualisieren. Aktivieren Sie einfach dieses Kontrollkästchen, und geben Sie dann den Namen eines vorhandenen Datasets ein.

![Hochladen eines neuen Datasets](./media/import-data-from-local-file/upload-dataset.png)

Während des Uploads wird eine Meldung angezeigt, dass die Datei hochgeladen wird. Die Dauer für das Hochladen hängt von der Größe Ihrer Daten und der Geschwindigkeit der Verbindung mit dem Dienst ab. Wenn Sie wissen, dass das Hochladen der Datei eine lange Zeit dauern kann, können Sie andere Dinge in Machine Learning Studio erledigen, während Sie warten. Wenn Sie den Browser schließen, tritt jedoch beim Hochladen der Daten ein Fehler auf.

## <a name="dataset-module-is-ready-for-use"></a>Datasetmodul ist zur Verwendung bereit
Sobald Ihre Daten hochgeladen wurden, werden sie in einem Datasetmodul gespeichert und stehen für alle Experimente im Arbeitsbereich zur Verfügung.

Wenn Sie ein Experiment bearbeiten, finden Sie die Datasets, die Sie erstellt haben, in der Modulpalette unter der Liste **Gespeicherte Datasets** in der Liste **Meine Datasets**. Sie können das Dataset in den Experimentbereich ziehen und dort ablegen, wenn Sie es für weitere Analysen und maschinelles Lernen verwenden möchten.
