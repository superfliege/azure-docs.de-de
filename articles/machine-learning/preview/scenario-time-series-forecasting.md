---
title: "Vorhersagen von Zeitreihen für den Energiebedarf | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie maschinelles Lernen auf die Vorhersage von Zeitreihen für den Energiebedarf in der Azure Machine Learning Workbench anwenden."
services: machine-learning
documentationcenter: 
author: anta
manager: ireiter
editor: anta
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: anta
ms.openlocfilehash: bd0ddfcffdb6f946f9a3786f3d0add1740be861b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="energy-demand-time-series-forecasting"></a>Vorhersagen von Zeitreihen für den Energiebedarf


Bei der Zeitreihenvorhersage werden zukünftige Werte in einer zeitlich geordneten Sequenz von Beobachtungen prognostiziert. Diese ist ein häufig auftretendes Problem, das in vielen Branchen Anwendung findet. Einzelhandelsunternehmen müssen beispielsweise zukünftige Produktverkäufe vorhersagen, damit sie ihre Lieferketten effektiv an den Bedarf anpassen können. Auf ähnliche Weise müssen Paketzusteller den Bedarf an ihren Diensten schätzen, um die erforderlichen Mitarbeiter und Lieferrouten im Voraus zu planen. In vielen Fällen stellen ungenaue Vorhersagen erhebliche finanzielle Risiken dar. Daher ist die Vorhersage häufig eine sehr wichtige Geschäftsaktivität.

Dieses Beispiel zeigt, wie Zeitreihenvorhersagen durch Anwenden von Verfahren zum maschinellen Lernen durchgeführt werden können. Sie werden durch sämtliche Schritte der Modellierung geführt. Dazu gehören:
- Vorbereiten der Daten durch Bereinigen und Formatieren
- Erstellen von Features für Modelle des maschinellen Lernens aus den unformatierten Zeitreihendaten
- Trainieren verschiedener Modelle des maschinellen Lernens
- Bewerten der Modelle durch Vergleichen ihrer Leistung anhand eines bereitgestellten Test-DataSets
- Umsetzen und Verfügbarmachen des besten Modells über einen Webdienst zum bedarfsgesteuerten Generieren von Vorhersagen

Die Azure Machine Learning Workbench unterstützt den Modellierungsprozess bei jedem Schritt: 
- Das Beispiel zeigt die Jupyter Notebook-Umgebung. Diese ist direkt über die Workbench verfügbar und kann die Entwicklung von Python-Code vereinfachen. Der Modellentwicklungsprozess kann anderen Personen mithilfe von Markdownanmerkungen und Diagrammen einfacher vermittelt werden. Diese Notebooks können direkt aus der Workbench angezeigt, bearbeitet und ausgeführt werden.
- Trainierte Modelle können dauerhaft gespeichert und in Blob Storage hochgeladen werden. Dadurch können Data Scientists die trainierten Modellobjekte nachverfolgen und sicherstellen, dass sie beibehalten werden und bei Bedarf abrufbar sind.
- Während der Ausführung eines Python-Skripts können Metriken protokolliert werden, mit deren Hilfe Data Scientists Bewertungen der Modellleistung nachverfolgen können.
- Die Workbench erstellt anpassbare Tabellen der protokollierten Metriken, die es Datenanalysten ermöglichen, Leistungsmetriken der einzelnen Modelle einfach zu vergleichen. Diagramme werden automatisch angezeigt, damit schnell das Modell mit der besten Leistung identifiziert werden kann.
- Schließlich wird im Beispiel gezeigt, wie ein trainiertes Modell operationalisiert werden kann, indem es in einem Echtzeit-Webdienst bereitgestellt wird.

## <a name="link-to-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys
Das öffentliche GitHub-Repository für dieses reale Szenario enthält alle Materialien, die in diesem Beispiel benötigt werden, einschließlich Codebeispielen:

[https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting)


## <a name="use-case-overview"></a>Übersicht über Anwendungsfälle

Der Schwerpunkt dieses Szenarios liegt auf der Vorhersage des Energiebedarfs, die zum Ziel hat, die zukünftige Belastung eines Energienetzes vorherzusagen. Dies ist ein zentraler Geschäftsvorgang für Unternehmen im Energiesektor, da Operatoren ein ausgewogenes Verhältnis zwischen der verbrauchten Energie in einem Netz und der bereitgestellten Energie sicherstellen müssen. Wird im Netz zu viel Energie bereitgestellt, kann dies zu Energieverschwendung oder technischen Fehlern führen. Zu wenig Energie hingegen kann zu Stromausfällen bei den Kunden führen. Netzoperatoren können in der Regel kurzfristige Entscheidungen zur Energiebereitstellung im Netz treffen und so die Last im Gleichgewicht halten. Daher ist eine präzise kurzfristige Vorhersage des Energiebedarfs für den Operator enorm wichtig, um die richtigen Entscheidungen zu treffen.

Dieses Szenario enthält Informationen zur Erstellung einer Lösung für die Vorhersage des Energiebedarfs mithilfe von maschinellem Lernen. Die Lösung wird mit einem öffentlichen DataSet von [New York Independent System Operator (NYISO)](http://www3.dps.ny.gov/W/PSCWeb.nsf/All/298372E2CE4764E885257687006F39DF?OpenDocument) trainiert. Dieser Anbieter unterhält das Stromnetz für den Bundesstaat New York. Das DataSet enthält stundenweise Daten für den Energiebedarf von New York City über einen Zeitraum von fünf Jahren. Ein zusätzliches DataSet mit stündlichen Wetterdaten in New York City über denselben Zeitraum wurde von [darksky.net](https://darksky.net) entnommen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar)
- Eine installierte Kopie der [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) nach dem [Schnellstart-Installationshandbuch](./quickstart-installation.md) zur Installation des Programms und der Erstellung eines Arbeitsbereichs.
- In diesem Beispiel wird davon ausgegangen, dass Sie die Azure ML Workbench mit einem lokal installierten [Docker-Modul](https://www.docker.com/) unter Windows 10 ausführen. Wenn Sie macOS verwenden, sind die Anweisungen weitgehend identisch.
- Installierte Azure Machine Learning-Operationalisierung mit einer lokal eingerichteten Bereitstellungsumgebung und einem Konto zur Modellverwaltung, das wie in diesem [Handbuch](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/getting-started.md) beschrieben erstellt wurde.
- Als Voraussetzung für dieses Beispiel müssen Sie die Pandas-Installation auf Version 0.20.3 oder höher aktualisieren und matplotlib installieren. Klicken Sie in der Workbench im Menü *Datei* auf *Eingabeaufforderung öffnen*, und führen Sie die folgenden Befehle aus, um diese Abhängigkeiten zu installieren:

    ```
    conda install "pandas>=0.20.3"

    conda install matplotlib
    ```
    
## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie ein neues Projekt mit diesem Beispiel als Vorlage:
1.  Öffnen Sie Azure Machine Learning Workbench.
2.  Klicken Sie auf der Seite **Projekte** auf **+**, wählen Sie **Neues Projekt** aus, und signieren Sie es.
3.  Geben Sie die Informationen für das neue Projekt im Bereich **Neues Projekt erstellen** ein.
4.  Geben Sie im Suchfeld **Projektvorlagen suchen** den Suchbegriff „Energy Demand Time Series Forecasting“ ein, und wählen Sie die Vorlage aus.
5.  Klicken Sie auf **Erstellen**


## <a name="data-description"></a>Datenbeschreibung

Es gibt zwei DataSets: `nyc_demand.csv` und `nyc_weather.csv`:

**nyc_demand.csv** enthält die stündlich erfassten Energiebedarfswerte für New York City in den Jahren 2012 bis 2017. Die Daten haben die folgende einfache Struktur:

| timeStamp | demand |
| --- | --- |
| 2012-01-01 00:00:00 | 4937.5 |
| 2012-01-01 01:00:00 | 4752.1 |
| 2012-01-01 02:00:00 | 4542.6 |
| 2012-01-01 03:00:00 | 4357.7 |

Die Bedarfswerte sind in Megawattstunden (MWh) angegeben. Das folgende Diagramm zeigt den Energiebedarf über einen Zeitraum von 7 Tagen im Juli 2017:

![Energiebedarf](./media/scenario-time-series-forecasting/energy_demand.png  "Energiebedarf")

**nyc_weather.csv** enthält die stündlich erfassten Wetterdaten für New York City in den Jahren 2012 bis 2017:

| timeStamp | precip | temp
| --- | --- | --- |
| 2012-01-01 00:00:00 | 0.0 | 46.13 |
| 2012-01-01 01:00:00 | 0.01 | 45.89 |
| 2012-01-01 02:00:00 | 0.05 | 45.04 |
| 2012-01-01 03:00:00 | 0.02 | 45.03 |

Der Wert unter *precip* ist das prozentuale Maß der Niederschlagsmenge. Die Werte unter *temp* (Temperatur) wurden angepasst, damit alle Werte in das Intervall [0, 100] fallen.

## <a name="scenario-structure"></a>Szenariostruktur

Wenn Sie dieses Projekt zum ersten Mal in der Azure Machine Learning Workbench öffnen, navigieren Sie zum Bereich *Dateien* auf der linken Seite. In diesem Beispiel werden die folgenden Codedateien bereitgestellt:
- `1-data-preparation.ipynb`: Dieses Jupyter Notebook lädt die Daten herunter und verarbeitet sie, um sie für die Modellierung vorzubereiten. Dies ist das erste Notebook, das Sie ausführen.
- `2-linear-regression.ipynb`: Dieses Notebook trainiert ein lineares Regressionsmodell anhand der Trainingsdaten.
- `3-ridge.ipynb`: trainiert ein Ridge-Regressionsmodell.
- `4-ridge-poly2.ipynb`: trainiert ein Ridge-Regressionsmodell mit polynomischen Funktionen 2. Grades.
- `5-mlp.ipynb`: trainiert ein mehrschichtiges neuronales Perceptron-Netzwerk.
- `6-dtree.ipynb`: trainiert ein Entscheidungsstrukturmodell.
- `7-gbm.ipynb`: trainiert ein Gradient-Boosted-Machine-Modell.
- `8-evaluate-model.py`: Dieses Skript lädt ein trainiertes Modell und erstellt damit Vorhersagen für ein bereitgestelltes Test-DataSet. Es erstellt Modellauswertungsmetriken, damit die Leistung der verschiedenen Modelle verglichen werden kann.
- `9-forecast-output-exploration.ipynb`: Dieses Notebook erzeugt Visualisierungen der Vorhersagen, die von den Modelle des maschinellen Lernens generiert wurden.
- `10-deploy-model.ipynb`: Dieses Notebook veranschaulicht, wie ein trainiertes Vorhersagemodell in einem Echtzeit-Webdienst operationalisiert werden kann.
- `evaluate-all-models.py`: Dieses Skript bewertet alle trainierten Modelle. Es stellt eine Alternative zur Ausführung von `8-evaluate-model.py` für jedes trainierte Modell dar.

### <a name="1-data-preparation-and-cleaning"></a>1. Vorbereiten und Analysieren der Daten

Klicken Sie zum Starten der Ausführung des Beispiels zuerst auf `1-data-preparation.ipynb`, um das Notebook im Vorschaumodus zu öffnen. Klicken Sie auf ***Start Notebook Server*** (Notebook-Server starten), um einen Jupyter Notebook-Server und einen Python-Kernel auf Ihrem Computer zu starten. Sie können entweder die Notebooks innerhalb der Workbench ausführen oder in Ihrem Browser zur URL [http://localhost:8888](http://localhost:8888) browsen. Beachten Sie, dass Sie den Kernel in *PROJEKTNAME local* ändern müssen. Sie können dies im Menü *Kernel* im Notebook unter *Change kernel* (Kernel ändern) durchführen. Drücken Sie ***UMSCHALT + EINGABETASTE***, um den Code in den einzelnen Notebook-Zellen auszuführen, oder klicken Sie im Menü *Zelle* auf *Alle ausführen*, um das gesamte Notebook auszuführen.

Das Notebook lädt zunächst die Daten aus einem in Azure gehosteten Blob Storage-Container herunter. Die Daten werden dann auf dem Computer in `AZUREML_NATIVE_SHARE_DIRECTORY` gespeichert. Auf diesen Speicherort kann von allen Notebooks oder Skripts aus zugegriffen werden, die Sie über die Workbench ausführen. Deshalb eignet er sich gut für das Speichern von Daten und trainierten Modellen.

Nachdem die Daten heruntergeladen wurden, bereinigt das Notebook die Daten durch Auffüllen der Lücken in den Zeitreihen und der fehlenden Werte durch Interpolation. Durch das Bereinigen der Zeitreihendaten auf diese Weise wird die zum Trainieren der Modelle zur Verfügung stehende Datenmenge maximiert.

Es werden mehrere Modellfeatures aus den bereinigten Rohdaten erstellt. Diese Features können in zwei Gruppen eingeteilt werden:

- **Zeitgesteuerte Features** werden von der *timestamp*-Variable abgeleitet, z.B. *month*, *dayofweek* und *hour*.
- **Isolierte Features** sind zeitversetzte Werte des tatsächlichen Bedarfs oder Temperaturwerte. Diese Features sollen umweltbedingte Abhängigkeiten zwischen aufeinanderfolgenden Zeiträumen im Modell erfassen.

Das resultierende Feature-DataSet kann dann für die Entwicklung von Vorhersagemodellen verwendet werden.

### <a name="2-model-development"></a>2. Entwickeln der Modelle

Ein erster Modellierungsansatz könnte ein einfaches lineares Regressionsmodell sein. Das Notebook `2-linear-regression.ipynb` zeigt, wie ein lineares Regressionsvorhersagemodell für den zukünftigen Energiebedarf trainiert wird. Insbesondere wird das Modell trainiert, um den Energiebedarf mit einem Vorlauf von einer Stunde (*t + 1*) nach dem aktuellen Zeitraum (*t*) vorherzusagen. Wir können dieses „1-Schritt“-Modell während der Testphase jedoch nicht rekursiv anwenden, um Vorhersagen für zukünftige Zeiträume (*t + n*) zu generieren.

Um ein Modell zu trainieren, wird eine Vorhersagepipeline erstellt, die aus drei unterschiedlichen Schritten besteht:

- **Eine Datentransformation:** Die erforderlichen Formate für die Eingabedaten können je nach dem Algorithmus für das maschinelle Lernen variieren. In diesem Fall erfordert das lineare Regressionsmodell one-hot-codierte Kategorievariablen.
- **Eine Kreuzvalidierungsroutine:** Modelle des maschinellen Lernens enthalten häufig einen oder mehrere Hyperparameter, die durch Experimente optimiert werden müssen. Mithilfe der Kreuzvalidierung kann der optimalen Satz von Parameterwerten ermittelt werden. Das Modell wird mehrmals trainiert und anhand unterschiedlicher Teilmengen des DataSets bewertet. Die am besten geeigneten Parameter liefern eine optimale Modellleistung bei Auswertung über die Teilmengen-Kreuzvalidierung. Dieser Prozess wird unter `2-linear-regression.ipynb` ausführlicher erklärt.
- **Das Trainieren des endgültigen Modells:** Das Modell wird mit dem gesamten DataSet und anhand des optimalen Satzes von Hyperparametern trainiert.

Wir haben eine Reihe von sechs unterschiedlichen Modellen in den Notebooks 2 bis 7 eingefügt. Jedes Notebook trainiert ein anderes Modell und speichert es unter `AZUREML_NATIVE_SHARE_DIRECTORY`. Nachdem Sie alle Modelle, die für dieses Szenario entwickelt wurden, trainiert haben, können Sie diese wie im nächsten Abschnitt beschrieben bewerten und vergleichen.

### <a name="3-model-evaluation-and-comparison"></a>3. Bewerten und Vergleichen der Modelle

Sie können ein trainiertes Modell verwenden, um Vorhersagen für zukünftige Zeiträume zu machen. Es wird empfohlen, diese Modelle anhand eines Test-DataSets zu bewerten. Durch die Bewertung der verschiedenen Modelle anhand desselben unbekannten DataSets, können Sie ihre Leistung gut vergleichen und das beste Modell ermitteln. In diesem Szenario wenden wir das trainierte Modell rekursiv für eine Vorhersage mehrerer Zeitschritte in der Zukunft an. Wir generieren insbesondere Prognosen für bis zu sechs Stunden (*t + 6*) nach der aktuellen Stunde *t*. Diese Vorhersagen werden anhand des tatsächlichen Bedarfs für jeden Zeitraum ausgewertet.

Um ein Modell zu bewerten, öffnen Sie das Skript `8-evaluate-model.py` im Bereich *Dateien* in der Workbench. Stellen Sie sicher, dass *Laufzeitkonfiguration* auf **lokal** festgelegt ist, und geben Sie dann den Modellnamen in das Feld *Argumente* ein. Der Modellname muss genau mit der *model_name*-Variable übereinstimmen, die am Anfang des Notebooks, in dem das Modell trainiert wird, festgelegt ist. Geben Sie beispielsweise „linear_regression“ ein, um das trainierte Modell für die lineare Regression zu bewerten. Alternativ können Sie alle Modelle mit einem Befehl durch Ausführen von `evaluate-all-models.py` bewerten, nachdem sie trainiert wurden. Zum Ausführen dieses Skripts öffnen Sie in der Workbench eine Eingabeaufforderung und führen den folgenden Befehl aus:

```
python evaluate-all-models.py
```
Das Skript `8-evaluate-model.py` führt folgende Aktionen aus:

- Laden einer trainierten Modellpipeline vom Datenträger
- Erstellen von Vorhersagen anhand des Test-DataSets
- Berechnen und Protokollieren der Modellleistungsmetriken
- Speichern der Vorhersagen für das Test-DataSet unter `AZUREML_NATIVE_SHARE_DIRECTORY` für die spätere Überprüfung und Analyse
- Speichern der trainierten Modellpipeline im Ordner *outputs*.

> [!Note]
> Durch das Speichern des Modells im Ordner *output* wird das Modellobjekt automatisch in das Azure Blob Storage-Konto, das Ihrem Experimentieren-Konto zugeordnet ist, kopiert. Dies bedeutet, dass Sie das gespeicherte Modell jederzeit aus dem Blob abrufen können, auch wenn Sie den Computer oder den Rechenkontext ändern.

Navigieren Sie zum Bereich *Ausführungsverlauf*, und klicken Sie auf `8-evaluate-model.py`. Dort werden Diagramme von verschiedenen Modellleistungsmetriken angezeigt:

- **ME** (Mean Error): mittlerer Fehler der Vorhersage. Dieser kann als die durchschnittliche Abweichung der Vorhersage interpretiert werden.
- **MPE:** [Mean Percentage Error](https://en.wikipedia.org/wiki/Mean_percentage_error) (ME ausgedrückt als Prozentsatz des tatsächlichen Bedarfs)
- **MSE:** [Mean Squared Error](https://en.wikipedia.org/wiki/Mean_squared_error)
- **RMSE:** Root Mean Squared Error (Quadratwurzel von MSE)
- **MAPE:** [Mean Absolute Percentage Error](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
- **sMAPE:** [symmetric Mean Absolute Percentage Error](https://en.wikipedia.org/wiki/Symmetric_mean_absolute_percentage_error)
- **MAPE_base:** MAPE einer Baselinevorhersage, bei der der letzte bekannte Bedarfswert gleich dem Vorhersagewert ist
- **MdRAE:** Median Relative Absolute Error. Das Medianverhältnis des Vorhersagefehlers zum Baseline-Vorhersagefehler. Ein Wert kleiner 1 bedeutet, dass die Vorhersage besser als die Baseline ist.

Alle oben aufgeführten Metriken beziehen sich auf die Vorhersage *t + 1*. Zusätzlich zu den oben aufgeführten Metriken werden auch weitere zugehörige Metriken mit dem Suffix *_horizon* angezeigt. Dies ist die gemittelte Metrik über alle Zeiträume in der Vorhersage vom Zeitraum *t + 1* bis zum Zeitraum *t + 6*.

Wenn im Diagrammbereich keine Metriken angezeigt werden, klicken Sie auf das Zahnradsymbol in der rechten oberen Ecke. Vergewissern Sie sich, dass die Modellleistungsmetriken, an denen Sie interessiert sind, aktiviert sind. Die Metriken werden auch in einer Tabelle unterhalb der Diagramme angezeigt. Auch diese Tabelle ist anpassbar, indem Sie auf das Zahnradsymbol klicken. Sortieren Sie die Tabelle nach einer Leistungsmetrik, um das beste Modell zu ermitteln. Wenn Sie wissen möchten, wie sich die Vorhersageleistung von Zeitraum *t + 1* bis Zeitraum *t + 6* ändert, klicken Sie auf den Eintrag für das Modell in der Tabelle. Diagramme der Metriken MAPE, MPE und MdRAE über den Vorhersagezeitraum werden unter *Visualisierungen* angezeigt.

Beim Bewerten der Vorhersagemodelle kann es hilfreich sein, die ausgegebenen Vorhersagen zu visualisieren. Dadurch können Data Scientists bestimmen, ob die erstellte Vorhersage realistisch erscheint. Darüber hinaus können so auch Probleme bei der Vorhersage identifiziert werden, wenn die Vorhersage z.B. in bestimmten Zeiträumen schlechte Ergebnisse liefert. Das Notebook `9-forecast-output-exploration.ipynb` erstellt Visualisierungen der Vorhersagen für das Test-DataSet.

### <a name="4-deployment"></a>4. Bereitstellung

Das beste Modell kann durch die Bereitstellung als Echtzeit-Webdienst operationalisiert werden. Dieser Webdienst kann dann aufgerufen werden, um Vorhersagen zum Bedarf zu erstellen, wenn neue Daten verfügbar werden. In diesem Szenario muss einmal pro Stunde eine neue Vorhersage generiert werden, um dem Energiebedarf in der nächsten Stunde vorherzusagen. Für diese Aufgabe kann ein Webdienst bereitgestellt werden, der ein Array von Features für einen bestimmten Stunden-Zeitraum als Eingabe akzeptiert und den vorhergesagten Bedarf als Ausgabe zurückgibt.

In diesem Beispiel wird ein Webdienst auf einem Windows 10-Computer bereitgestellt. Stellen Sie sicher, dass die Voraussetzungen für die lokale Bereitstellung, die in diesem [Leitfaden zu den ersten Schritten](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/getting-started.md) für die Operationalisierungs-Befehlszeilenschnittstelle beschrieben werden, erfüllt sind. Nachdem Sie Ihre lokalen Umgebung und das Konto für die Modellverwaltung eingerichtet haben, führen Sie das Notebook `10-deploy-model.ipynb` aus, um den Webdienst bereitzustellen.

## <a name="conclusion"></a>Zusammenfassung

Dieses Beispiel veranschaulicht das Erstellen einer umfassenden Lösung zur Zeitreihenvorhersage zur Prognose des Energiebedarfs. Viele der in diesem Beispiel beschriebenen Prinzipien können auf andere Vorhersageszenarien und Branchen angewendet werden.

Dieses Szenario zeigt, wie die Azure Machine Learning Workbench Data Scientists helfen kann, Lösungen für den praktischen Einsatz mit nützlichen Features wie der Jupyter Notebook-Umgebung und Funktionen zur Protokollierung von Metriken zu entwickeln. Das Beispiel veranschaulicht auch das Operationalisieren eines Modells und dessen Bereitstellung mithilfe der Azure Machine Learning-Operationalisierungs-Befehlszeilenschnittstelle. Nach der Bereitstellung können Entwickler oder Datentechniker die Webdienst-API verwenden, um das Vorhersagemodell in einer größere Datenpipeline zu integrieren.
