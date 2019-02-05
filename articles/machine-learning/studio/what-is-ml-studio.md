---
title: Info
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio ist ein Drag & Drop-Tool zum schnellen Erstellen von Modellen aus einer verwendungsbereiten Bibliothek mit Algorithmen und Modulen.
keywords: Azure Machine Learning,Azure ML,ML Studio
services: machine-learning
documentationcenter: ''
author: garyericson
ms.custom: seodec18
ms.author: garye
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/28/2018
ms.openlocfilehash: bb01db14cec73b8c5668915a9eaab5e7728724df
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469114"
---
# <a name="what-is-azure-machine-learning-studio"></a>Was ist Azure Machine Learning Studio?
Microsoft Azure Machine Learning Studio ist ein Tool für die Zusammenarbeit per Drag & Drop, mit der Sie Lösungen für Vorhersageanalysen erstellen, testen und bereitstellen können, die mit Ihren Daten arbeiten. Machine Learning Studio veröffentlicht Modelle als Webdienste, die von benutzerdefinierten Apps oder BI-Tools wie Excel problemlos genutzt werden können.

In Machine Learning Studio finden Datenwissenschaften, prädiktive Analysen, Cloudressourcen und Ihre Daten zusammen!

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Der interaktive Machine Learning Studio-Arbeitsbereich
Für die Entwicklung eines prädiktiven Analysemodells verwendet man typischerweise Daten aus einer oder mehreren Quellen, transformiert und analysiert diese Daten mithilfe verschiedener Datenbearbeitungen und Statistikfunktionen und generiert einen Ergebnissatz. Das Entwickeln eines solchen Modells ist ein iterativer Prozess. Sie ändern die verschiedenen Funktionen und deren Parameter, und die Ergebnisse nähern sich an, bis Sie der Ansicht sind, dass Sie ein trainiertes, effektives Modell erreicht haben.

**Azure Machine Learning Studio** stellt Ihnen einen interaktiven, visuellen Arbeitsbereich zur Verfügung, in dem Sie ein Vorhersageanalysemodell ganz einfach entwickeln, testen und durchlaufen können. Sie fügen per Drag & Drop ***DataSets*** und ***Analysemodule*** in eine interaktive Canvas ein und verbinden sie zu einem ***Experiment***, das Sie in Machine Learning Studio ausführen. Für die Iteration des Modelldesigns bearbeiten Sie das Experiment, speichern ggf. eine Kopie und führen es erneut aus. Wenn Sie bereit sind, können Sie Ihr ***Trainingsexperiment*** in ein ***Vorhersageexperiment*** konvertieren und anschließend als ***Webdienst*** veröffentlichen, damit andere Benutzer auf das Modell zugreifen können.

Es ist keine Programmierung erforderlich, sondern einfach nur das visuelle Verbinden von Datasets und Modulen zum Erstellen eines prädiktiven Analysemodells.

> [!TIP]
> Informationen zum Herunterladen und Drucken des Diagramms, mit dem Sie sich einen Überblick über die Machine Learning Studio-Funktionen verschaffen können, finden Sie unter [Übersichtsdiagramm der Azure Machine Learning Studio-Funktionen](studio-overview-diagram.md).
>
>

![Azure Machine Learning Studio-Diagramm: Erstellen von Experimenten, Lesen von Daten aus vielen Quellen, Schreiben der ausgewerteten Daten, Erstellen von Modellen.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Erste Schritte mit Machine Learning Studio
Wenn Sie [Machine Learning Studio](https://studio.azureml.net) zum ersten Mal öffnen, sehen Sie die **Startseite** . Hier finden Sie die Dokumentation, Videos, Webinare und weitere wertvolle Ressourcen.

Klicken Sie auf das Menü links oben. ![Menü](./media/what-is-ml-studio/menu.png) Daraufhin werden mehrere Optionen angezeigt.

### <a name="cortana-intelligence"></a>Cortana Intelligence
Klicken Sie auf **Cortana Intelligence**, um zur Startseite der [Cortana Intelligence Suite](https://www.microsoft.com/cloud-platform/cortana-intelligence-suite) zu gelangen. Die Cortana Intelligence Suite ist eine vollständig verwaltete Big Data-Lösung für erweiterte Analysen, mit der Sie Ihre Daten in intelligente Aktionen verwandeln können. Eine umfassende Dokumentation sowie Kundenstimmen finden Sie auf der Startseite der Suite.

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
Hier stehen zwei Optionen zur Verfügung: **Startseite** (die Seite, auf der Sie begonnen haben), und **Studio**.

Klicken Sie auf **Studio**, um zu **Azure Machine Learning Studio** zu gelangen. Sie werden zunächst aufgefordert, sich mit Ihrem Microsoft-Konto oder Ihrem Geschäfts-, Schul- oder Unikonto anzumelden. Nach der Anmeldung sehen Sie auf der linken Seite die folgenden Registerkarten:

* **PROJECTS** (PROJEKTE): Sammlungen mit Experimenten, Datasets, Notebooks und anderen Ressourcen, die ein einzelnes Projekt darstellen
* **EXPERIMENTS**: Experimente, die Sie erstellt und ausgeführt oder als Entwürfe gespeichert haben
* **WEB SERVICES** : Webdienste, die Sie über Ihre Experimente bereitgestellt haben
* **NOTEBOOKS** : Jupyter-Notebooks, die Sie erstellt haben
* **DATASETS** : DataSets, die Sie in Studio hochgeladen haben
* **TRAINED MODELS** : Modelle, die Sie in Experimenten trainiert und in Studio gespeichert haben
* **EINSTELLUNGEN** – eine Sammlung von Einstellungen, mit denen Sie Ihr Konto und Ihre Ressourcen konfigurieren

### <a name="gallery"></a>Gallery
Wenn Sie auf **Katalog** klicken, gelangen Sie zum **[Azure AI-Katalog](http://gallery.cortanaintelligence.com/)**. In diesem Katalog teilt eine Community von Datenwissenschaftlern und Entwicklern Lösungen, die mithilfe von Komponenten der Cortana Intelligence Suite erstellt wurden.

Weitere Informationen zum Katalog finden Sie unter [Teilen und Entdecken von Lösungen im Azure AI-Katalog](gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Komponenten eines Experiments
Ein Experiment besteht auf Datasets, die Daten für Analysemodule bereitstellen, die Sie miteinander verbinden und so ein Vorhersagemodell konstruieren. Ein gültiges Experiment hat spezifisch folgende Eigenschaften:

* Das Experiment besteht aus mindestens einem Dataset und einem Modul.
* Datasets können nur mit Modulen verbunden sein.
* Module können entweder mit Datasets oder mit anderen Modulen verbunden sein.
* Alle Eingangsports für Module müssen eine Verbindung zum Datenfluss aufweisen.
* Alle erforderlichen Parameter der einzelnen Module müssen festgelegt sein.

Sie können ein Experiment von Grund auf neu erstellen, oder Sie können ein vorhandenes Beispielexperiment als Vorlage verwenden. Weitere Informationen finden Sie unter [Kopieren von Beispielexperimenten zum Erstellen neuer Machine Learning-Experimente](sample-experiments.md).

Ein Beispiel für das Erstellen eines einfachen Experiments finden Sie unter [Erstellen eines einfachen Experiments in Azure Machine Learning Studio](create-experiment.md).

Eine ausführlichere exemplarische Vorgehensweise zum Erstellen einer Lösung für Vorhersageanalysen finden Sie unter [Entwickeln einer Vorhersagelösung mit Azure Machine Learning](walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Datasets
Ein DataSet besteht aus Daten, die in Machine Learning Studio hochgeladen wurden, sodass sie im Modellierungsprozess verwendet werden können. Machine Learning Studio enthält eine Reihe von Beispiel-DataSets, mit denen Sie experimentieren können, und bei Bedarf lassen sich weitere DataSets hochladen. Hier einige Beispiele der enthaltenen Datasets:

* **MPG-Daten für verschiedene Autos**: Meilen-pro-Gallone-Werte für Autos, die anhand der Anzahl von Zylindern, Motorleistung usw. aufgeführt sind.
* **Brustkrebsdaten:** diagnostische Brustkrebsdaten.
* **Waldbranddaten:** Größe von Waldbränden im nordöstlichen Portugal.

Beim Erstellen eines Experiments können Sie links neben dem Arbeitsbereich aus der Liste der Datasets auswählen.

Eine Liste der in Machine Learning Studio enthaltenen Beispiel-DataSets finden Sie unter [Verwenden von Beispiel-DataSets in Azure Machine Learning Studio](use-sample-datasets.md).

### <a name="modules"></a>Module
Ein Modul ist ein Algorithmus, den Sie auf Ihre Daten anwenden können. Machine Learning Studio hat eine Reihe von Modulen, die von Funktionen für die Dateneinspeisung bis zu Trainings-, Bewertungs- und Überprüfungsvorgängen reichen. Hier einige Beispiele der enthaltenen Module:

* [In ARFF konvertieren:][convert-to-arff] Konvertiert ein serialisiertes .NET-Dataset in ARFF (Attribute-Relation File Format).
* [Elementare Statistiken berechnen:][elementary-statistics] Berechnet elementare Statistiken wie Mittelwert, Standardabweichung usw.
* [Lineare Regression:][linear-regression] Erstellt ein lineares Onlineregressionsmodell, das auf einem Gradientenverfahrenmodell beruht.
* [Bewertungsmodell:][score-model] Bewertet ein trainiertes Klassifizierungs- oder Regressionsmodell.

Beim Erstellen eines Experiments können Sie links neben dem Arbeitsbereich aus der Liste der Module auswählen.

Ein Modul kann eine Reihe von Parametern haben, die Sie zum Konfigurieren der internen Algorithmen des Moduls einsetzen können. Wenn Sie ein Modul im Arbeitsbereich auswählen, werden dessen Parameter rechts neben dem Arbeitsbereich im Bereich **Eigenschaften** angezeigt. Sie können die Parameter in diesem Bereich zur Abstimmung Ihres Modells verändern.

Hilfe zur Navigation durch die umfassende Bibliothek verfügbarer Machine Learning-Algorithmen finden Sie unter [Auswählen von Algorithmen für Microsoft Azure Machine Learning](algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Bereitstellen eines Predictive Analytics-Webdiensts
Wenn Ihr Predictive Analytics-Modell bereit ist, können Sie es als Webdienst direkt von Machine Learning Studio aus bereitstellen. Weitere Informationen zu diesem Verfahren finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts](publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/what-is-ml-studio/azure-ml-studio-diagram.jpg



## <a name="key-machine-learning-terms-and-concepts"></a>Wichtige Machine Learning-Begriffe und -Konzepte
Machine Learning-Begriffe können manchmal verwirrend sein. Hier finden Sie deshalb die Definitionen der wichtigsten Begriffe. Sie können auch gern die Kommentarfunktion nutzen, um uns andere Begriffe mitzuteilen, für die Sie eine Definition wünschen.

### <a name="data-exploration-descriptive-analytics-and-predictive-analytics"></a>Durchsuchen von Daten, beschreibende Analyse und Predictive Analytics

**Durchsuchen von Daten** werden Informationen über ein umfangreiches und häufig unstrukturiertes Dataset erfasst, um Merkmale für eine gezielte Analyse zu ermitteln.

**Data Mining** bezieht sich auf das automatisierte Durchsuchen von Daten.

Bei der **beschreibenden Analyse** wird ein Dataset analysiert, um Vorgänge zusammenzufassen. Bei den weitaus meisten Business Analytics-Prozessen – z. B. Verkaufsberichten, Webmetriken und Analysen sozialer Netzwerke – handelt es sich um beschreibende Analysen.

**Predictive Analytics** werden Modelle basierend auf vergangenen oder aktuellen Daten entwickelt, um zukünftige Ergebnisse vorhersagen zu können.

### <a name="supervised-and-unsupervised-learning"></a>Überwachtes und nicht überwachtes Lernen
 **überwachte Lernen** werden mit bezeichneten Daten trainiert, also mit Daten, die aus Beispielen der gewünschten Antworten bestehen. Ein Modell zur Identifizierung von betrügerischer Kreditkartennutzung wird beispielsweise mit einem Dataset trainiert, das beschriftete Datenpunkte für bekannte betrügerische und gültige Buchungen enthält. Die meisten maschinellen Lernprozesse sind überwacht.

 **Nicht überwachte Lernprozesse** werden bei Daten ohne Bezeichnungen verwendet. Das Ziel hierbei ist es, Beziehungen zwischen den Daten zu finden. Ein Beispiel hierfür ist das Ermitteln von Kundengruppen mit ähnlichem Kaufverhalten.

### <a name="model-training-and-evaluation"></a>Trainieren und Auswerten des Modells
Ein Modell zum maschinellen Lernen ist eine Abstraktion der Frage, die Sie beantworten möchten, oder des Ergebnisses, das Sie vorhersagen möchten. Modelle werden anhand vorhandener Daten trainiert und ausgewertet.

#### <a name="training-data"></a>Trainingsdaten
Wenn Sie ein Modell mit Daten trainieren, verwenden Sie ein bekanntes Dataset und nehmen basierend auf den Datenmerkmalen Anpassungen am Modell vor, um eine möglichst genaue Antwort zu erhalten. In Azure Machine Learning wird ein Modell aus einem Algorithmusmodul entwickelt, das Trainingsdaten und funktionale Module, wie z. B. ein Bewertungsmodul, verarbeitet.

Wenn Sie ein Betrugserkennungsmodell in einem überwachten Lernprozess trainieren, verwenden Sie einen Satz Transaktionen, die entweder als betrügerisch oder als gültig bezeichnet sind. Sie teilen Ihr Dataset nach dem Zufallsprinzip und verwenden einen Teil zum Trainieren und einen Teil zum Testen oder Auswerten des Modells.

#### <a name="evaluation-data"></a>Auswertungsdaten
Nachdem Sie das Modell trainiert haben, werden Sie es mithilfe der verbleibenden Testdaten aus. Hierzu verwenden Sie Daten, deren Ergebnisse Sie bereits kennen, um herauszufinden, ob die Vorhersage Ihres Modells genau ist.

## <a name="other-common-machine-learning-terms"></a>Weitere gängige Begriffe des maschinellen Lernens
* **Algorithmus**: Ein eigenständiger Regelsatz zum Lösen von Problemen mithilfe von Datenverarbeitung, mathematischen Berechnungen oder automatisierter Argumentation.
* **Anomalieerkennung**: Ein Modell, bei dem ungewöhnliche Ereignisse oder Werte gemeldet werden, damit Probleme erkannt werden können. Bei der Erkennung von Kreditkartenbetrug wird beispielsweise nach ungewöhnlichen Käufen gesucht.
* **Kategorische Daten**: Daten, die nach Kategorien organisiert sind und in Gruppen unterteilt werden können. Ein kategorisches Dataset für Fahrzeuge könnte z. B. Jahr, Marke, Modell und Preis angeben.
* **Klassifizierung**: Ein Modell für die Einordnung von Datenpunkten in Kategorien, basierend auf einem Dataset, für das die Kategoriegruppierungen bereits bekannt sind.
* **Featureentwicklung**: Der Prozess des Extrahierens oder Auswählens von Features in Zusammenhang mit einem Dataset, um das Dataset zu erweitern und die Ergebnisse zu verbessern. Flugpreisdaten könnten z. B. durch Wochentage und Ferien erweitert werden. Siehe [Entwicklung und Auswahl von Features in Azure Machine Learning](../team-data-science-process/create-features.md).
* **Modul**: Ein Funktionselement in einem Machine Learning Studio-Modell, z.B. das Modul zur Dateneingabe, das die Eingabe und Bearbeitung kleiner Datasets ermöglicht. Auch bei einem Algorithmus handelt es sich um eine Art Modul in Machine Learning Studio.
* **Modell**: Ein Modell für überwachtes Lernen ist das Produkt eines Machine Learning-Experiments, das aus Trainingsdaten, einem Algorithmusmodul und Funktionsmodulen besteht, z.B. einem Bewertungsmodellmodul.
* **Numerische Daten**: Daten, die eine Bedeutung als Messung (kontinuierliche Daten) oder Zählung (diskrete Daten) haben. Diese Daten werden auch als *quantitative Daten*bezeichnet.
* **Partitionieren**: Die Methode, mit der Sie Daten in Stichproben unterteilen. Weitere Informationen finden Sie unter [Partition and Sample](https://msdn.microsoft.com/library/azure/dn905960.aspx) .
* **Vorhersage**: Eine Vorhersage ist die Prognose eines oder mehrerer Werte aus einem Machine Learning-Modell. Ihnen wird möglicherweise auch der Begriff „vorhergesagte Bewertung“ begegnen. Hierbei handelt es sich aber nicht um die finalen Ergebnisse eines Modells. Der Bewertung folgt eine Auswertung des Modells.
* **Regression**: Ein Modell zur Vorhersage eines Werts basierend auf unabhängigen Variablen, um z.B. den Preis eines Autos anhand des Baujahrs und der Marke vorherzusagen.
* **Bewertung**: Ein vorhergesagter Wert, der mithilfe des Moduls [Score Model](https://msdn.microsoft.com/library/azure/dn905995.aspx) in Machine Learning Studio aus einem trainierten Klassifizierungs- oder Regressionsmodell generiert wurde. Klassifizierungsmodelle geben auch eine Bewertung für die Wahrscheinlichkeit des vorhergesagten Werts zurück. Sobald Sie Bewertungen aus einem Modell generiert haben, können Sie die Genauigkeit des Modells mithilfe des Moduls [Evaluate Model](https://msdn.microsoft.com/library/azure/dn905915.aspx)auswerten.
* **Stichprobe**: Ein Teil eines Datasets, das repräsentativ für das gesamte Dataset steht. Stichproben können nach dem Zufallsprinzip oder basierend auf bestimmten Features des Datasets ausgewählt werden.

## <a name="next-steps"></a>Nächste Schritte
Die Grundlagen von Predictive Analytics und Machine Learning werden anhand eines [schrittweisen Tutorials](create-experiment.md) und auf der [Grundlage von Beispielen](sample-experiments.md) vermittelt.


<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
