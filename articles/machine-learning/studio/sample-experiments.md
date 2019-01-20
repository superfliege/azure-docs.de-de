---
Titel: Erstellen von Machine Learning Studio-Experimenten aus Beispielen titleSuffix: Azure Machine Learning Studio Beschreibung: Erfahren Sie, wie Sie Machine Learning-Beispielexperimente verwenden, um mit der Azure AI Gallery und Azure Machine Learning Studio neue Experimente zu erstellen.
Dienste: machine-learning ms.service: machine-learning ms.component: studio ms.topic: conceptual

Autor: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18, previous-author=heatherbshapiro, previous-ms.author=hshapiro ms.date: 05.01.2018
---
# <a name="create-azure-machine-learning-studio-experiments-from-working-examples-in-azure-ai-gallery"></a>Erstellen von Azure Machine Learning Studio-Experimenten aus funktionierenden Beispielen in Azure AI Gallery

Hier erfahren Sie, wie Sie Beispielexperimente aus dem [Azure AI-Katalog](https://gallery.cortanaintelligence.com/) verwenden, anstatt Machine Learning-Experimente von Grund auf neu zu erstellen. Auf der Grundlage der Beispiele können Sie eine eigene Machine Learning-Lösung erstellen.

Der Katalog enthält Beispielexperimente des Microsoft Azure Machine Learning-Teams und Beispiele aus der Machine Learning-Community. Sie können zudem auch Fragen stellen oder Anmerkungen zu Experimenten veröffentlichen.

Die Nutzung des Katalogs wird in dem dreiminütigen Video [Kopieren der Arbeit anderer für Ihre Data Science](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) der Reihe [Data Science für Einsteiger](data-science-for-beginners-the-5-questions-data-science-answers.md) veranschaulicht.



## <a name="find-an-experiment-to-copy-in-azure-ai-gallery"></a>Suchen nach einem zu kopierenden Experiment im Azure AI-Katalog
Navigieren Sie zum [Katalog](https://gallery.cortanaintelligence.com/) und klicken Sie oben auf der Seite auf **Experiments** (Experimente), um anzuzeigen, welche Experimente verfügbar sind.

### <a name="find-the-newest-or-most-popular-experiments"></a>Suchen nach den neuesten oder beliebtesten Experimenten
Auf dieser Seite können Sie Experimente der Kategorie **Recently added** anzeigen oder nach unten zu **What‘s popular** oder den neuesten **Popular Microsoft experiments** scrollen.

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Suchen nach einem Experiment, das bestimmte Anforderungen erfüllt
So durchsuchen Sie alle Experimente:

1. Klicken Sie oben auf der Seite auf **Browse all** (Alle durchsuchen).
2. Wählen Sie auf der linken Seite unter **Refine by** (Einschränken nach) im Abschnitt **Categories** (Kategorien) die Option **Experiment** aus, um alle im Katalog enthaltenen Experimente anzuzeigen.
3. Es gibt verschiedene Möglichkeiten zur Ermittlung von Experimenten, die Ihre Anforderungen erfüllen:
   * **Wählen Sie auf der linken Seite Filter aus.** Gehen Sie beispielsweise wie folgt vor, wenn Sie Experimente durchsuchen möchten, für die ein Algorithmus zur PCA-basierten Erkennung von Anomalien verwendet wird: Klicken Sie unter **Categories** (Kategorien) auf **Experiment**. Klicken Sie anschließend unter **Algorithms Used** (Verwendete Algorithmen) auf **Show all** (Alle anzeigen), und wählen Sie im Dialogfeld die Option **PCA-based Anomaly Detection** (PCA-basierte Erkennung von Anomalien). Unter Umständen müssen Sie scrollen, um die Option anzuzeigen.<br></br>
     ![Filter auswählen](./media/sample-experiments/choose-an-algorithm.png)
   * **Verwenden Sie das Suchfeld.** Wenn Sie etwa nach Experimenten von Microsoft zur Ziffernerkennung suchen möchten, bei denen ein 2-Klassen-Support Vector Machine-Algorithmus verwendet wird, geben Sie im Suchfeld „digit recognition“ (Ziffernerkennung) ein. Wählen Sie anschließend die Filter **Experiment**, **Microsoft content only** und **Two-Class Support Vector Machine** aus:<br></br>
     ![Suchfeld verwenden](./media/sample-experiments/search-for-experiments.png)
4. Klicken Sie auf ein Experiment, um mehr darüber zu erfahren.
5. Zum Ausführen und/oder Ändern des Experiments klicken Sie auf der Seite des Experiments auf **Open in Studio** (In Studio öffnen). <br></br>

    ![Beispielexperiment](./media/sample-experiments/example-experiment.png)

    > [!NOTE]
    > Wenn Sie ein Experiment in Machine Learning Studio zum ersten Mal öffnen, können Sie es kostenlos testen oder ein Azure-Abonnement kaufen. [Informationen zur kostenlosen Testversion von Machine Learning Studio im Vergleich zum kostenpflichtigen Dienst](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a>Erstellen eines neuen Experiments mit einem Beispiel als Vorlage
Sie können in Machine Learning Studio auch ein neues Experiment erstellen, indem Sie ein Beispiel aus dem Katalog als Vorlage verwenden.

1. Melden Sie sich mit den Anmeldeinformationen für Ihr Microsoft-Konto bei [Studio](https://studio.azureml.net)an, und klicken Sie dann auf **Neu**, um ein Experiment zu erstellen.
2. Durchsuchen Sie die Beispiele, und wählen Sie eines aus.

In Ihrem Machine Learning Studio-Arbeitsbereich wird auf der Grundlage des Beispielexperiments ein neues Experiment erstellt.

## <a name="next-steps"></a>Nächste Schritte
* [Importieren von Daten aus verschiedenen Quellen](import-data.md)
* [Schnellstart-Tutorial für die Sprache R in Machine Learning](r-quickstart.md)
* [Bereitstellen eines Machine Learning-Webdiensts](publish-a-machine-learning-web-service.md)
