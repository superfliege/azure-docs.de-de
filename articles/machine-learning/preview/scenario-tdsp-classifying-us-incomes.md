---
title: "Einkommensklassifizierung – Team Data Science-Prozess – Azure Machine Learning | Microsoft-Dokumentation"
description: Verwenden der Team Data Science-Prozessvorlage zum Erstellen eines Projekts in Azure Machine Learning, das US-Einkommen klassifiziert.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: bradsev
ms.openlocfilehash: c0fbd9b3e6f9f9f4f7a5d3e6bda18ce1312650e7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="income-classification-with-team-data-science-process-tdsp-project"></a>Einkommensklassifizierung mit dem Team Data Science-Prozessprojekt (TDSP)

## <a name="introduction"></a>Einführung

Die Standardisierung der Struktur und Dokumentation von Data Science-Projekten, die in einem feststehenden [Data Science-Lebenszyklus](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md) verankert sind, erleichtern die effektive Zusammenarbeit in Data Science-Teams. Das Erstellen von Azure Machine Learning-Projekten mit der Vorlage für den [Team Data Science-Prozess (TDSP)](https://github.com/Azure/Microsoft-TDSP) bietet ein Framework für eine solche Standardisierung.

Wir haben zuvor ein [GitHub-Repository für die TDSP-Projektstruktur und -Vorlagen](https://github.com/Azure/Azure-TDSP-ProjectTemplate) veröffentlicht. Aber bis jetzt war es nicht möglich, die TDSP-Struktur und -Vorlagen in einem Data Science-Tool zu instanziieren. Wir haben jetzt die Erstellung von Azure Machine Learning-Projekten aktiviert, die mit [TDSP-Struktur und -Dokumentationsvorlagen für Azure Machine Learning](https://github.com/amlsamples/tdsp) instanziiert sind. Anweisungen zur Verwendung von TDSP-Struktur und -Vorlagen in Azure Machine Learning werden [hier](https://aka.ms/how-to-use-tdsp-in-aml) bereitgestellt. Hier bieten wir ein Beispiel der Erstellung eines tatsächlichen Machine Learning-Projekts mit der TDSP-Struktur, die mit projektspezifischem Code, Artefakten und Dokumente gefüllt ist, und in Azure Machine Learning ausgeführt wird.

## <a name="link-to-github-repository"></a>Link zum GitHub-Repository
Eine zusammenfassende Dokumentation zum Beispiel finden Sie [hier](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome). Eine ausführlichere Dokumentation finden Sie auf der GitHub-Website.

### <a name="purpose"></a>Zweck
Der Hauptzweck dieses Beispiels ist zu zeigen, wie ein Machine Learning-Projekt mit [Team Data Science-Prozess (TDSP)](https://github.com/Azure/Microsoft-TDSP)-Struktur und -Vorlagen in Azure Machine Learning instanziiert und ausgeführt wird. Zu diesem Zweck verwenden wir die bekannten [Volkszählungsdaten der USA von 1994 aus dem UCI Machine Learning-Repository](https://archive.ics.uci.edu/ml/datasets/adult). Die Modellierungsaufgabe besteht darin, Vorhersagen für die jährlichen Einkommensklassen der USA aus den Volkszählungsdaten zu treffen (z.B. Alter, ethnische Gruppe, Bildungsstand, Herkunftsland, usw.)

### <a name="scope"></a>Umfang
 * Durchsuchen von Daten, Schulungen und Bereitstellung von Machine Learning-Modellen, die das Problem der Vorhersage in der Übersicht der Anwendungsfälle berücksichtigt. 
 * Die Projektausführung in Azure Machine Learning verwendet die Vorlage des Team Data Science-Prozesses (TDSP) aus Azure Machine Learning für dieses Projekt. Für die Ausführung und Berichterstattung des Projekts werden wir den TDSP-Lebenszyklus verwenden.
 * Operationalisierung der Projektmappe direkt aus Azure Machine Learning in Azure Container Service.

 Das Projekt hebt mehrere Funktionen von Azure Machine Learning hervor, wie z.B. die Instanziierung und Verwendung der TDSP-Struktur, die Ausführung von Code in Jupyter Notebooks sowie Python-Dateien und die einfache Operationalisierung in Azure Container Services mit Docker und Kubernetes.


## <a name="team-data-science-process-tdsp-lifecycle"></a>Team Data Science-Prozesslebenszyklus
Weitere Informationen unter [Team Data Science-Prozesslebenszyklus (TDSP)](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md)

![](./media/scenario-tdsp-classifying-us-incomes/tdsp-lifecycle.jpg)

## <a name="prerequisites"></a>Voraussetzungen
### <a name="required-subscription-hardware-software"></a>Erforderlich: Abonnement, Hardware, Software
1. Ein [Azure-Abonnement](https://azure.microsoft.com). Sie erhalten ein [kostenloses Abonnement](https://azure.microsoft.com/free/?v=17.16&WT.srch=1&WT.mc_id=AID559320_SEM_cZGgGOIg), um dieses Beispiel auszuführen.
2. Eine [Azure Data Science Virtual Machine (DSVM) Windows Server 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm), (VM-Größe: [DS3_V2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) mit 4 virtuellen CPUs und 14 GB Arbeitsspeicher). Obwohl es auf einem Azure-DSVM getestet wird, funktioniert es wahrscheinlich auf jedem beliebigen Windows 10-Computer.
3. Überprüfen Sie die Dokumentation zu Azure Machine Learning und die zugehörigen Dienste (Links siehe unten).
4. Stellen Sie sicher, dass Sie Azure Machine Learning ordnungsgemäß mit dem [Schnellstart-Installationshandbuch](quickstart-installation.md) installiert haben.

Das Dataset für dieses Beispiel kommt aus dem UCI ML-Repository [[Link]](https://archive.ics.uci.edu/ml/datasets/adult). Es stammt aus der US Census-Datenbank von 1994 und enthält Volkszählungs- und Einkommensdaten für ca. 50.000 Personen. Dieses strukturierte Dataset verfügt über numerische und kategorische Funktionen und ein kategorisches Ziel mit zwei Einkommenskategorien ('>50 K' oder '<=50 K'). 

### <a name="optional-version-control-repository"></a>Optional: Repository der Versionskontrolle
Wenn Sie Ihr Projekt und seine Inhalte speichern möchten, benötigen Sie ein Repository der Versionskontrolle, in dem dies ausgeführt werden kann. Sie können den Speicherort des Git-Repositorys beim Erstellen des neuen Projekts in Azure Machine Learning mit der TDSP-Vorlage eingeben. Weitere Details finden Sie unter [How to use Git in Azure Machine Learning (Verwenden von Git in Azure Machine Learning)](using-git-ml-project.md).

### <a name="informational-about-azure-machine-learning"></a>Informationen zu Azure Machine Learning
* [FAQ – Erste Schritte](frequently-asked-questions.md)
* [Übersicht](overview-what-is-azure-ml.md)
* [Installation](quickstart-installation.md)
* [Ausführung](experiment-execution-configuration.md)
* [Verwenden von TDSP](https://aka.ms/how-to-use-tdsp-in-aml)
* [Lesen und Schreiben von Dateien](how-to-read-write-files.md)
* [Using Git with Azure Machine Learning (Verwenden von Git mit Azure Machine Learning)](using-git-ml-project.md)
* [Deploying an ML model as a web service (Bereitstellen von ML-Modell als Webdienst)](model-management-service-deploy.md)

### <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie ein neues Projekt mit diesem Beispiel als Vorlage:
1.  Öffnen Sie Azure Machine Learning Workbench.
2.  Klicken Sie auf der Seite **Projekte** auf **+**, wählen Sie **Neues Projekt** aus, und signieren Sie es.
3.  Geben Sie die Informationen für das neue Projekt im Bereich **Neues Projekt erstellen** ein.
4.  Geben Sie im Suchfeld **Projektvorlagen suchen** den Suchbegriff „US-Einkommen klassifizieren – Dokumentsammlungsanalyse“ ein, und wählen Sie die Vorlage aus.
5.  Klicken Sie auf **Erstellen**

Wenn Sie während der Erstellung des Projekts eine leere Stelle des Git-Repositorys (im entsprechenden Feld) bereitstellen, wird dieses Repository nach der Projekterstellung mit der Projektstruktur und den Inhalten gefüllt.

## <a name="use-case-overview"></a>Übersicht über Anwendungsfälle
Das Problem ist, zu verstehen, wie die sozioökonomischen Daten der US-Volkszählung bei der Vorhersage des Jahreseinkommens von Personen in den USA helfen können. Auf der Grundlage dieser Volkszählungsfunktionen ist es die Machine Learning-Aufgabe, vorherzusagen, ob das Einkommen einer Person über 50.000 $ liegt oder nicht (binäre Klassifikationsaufgabe).

## <a name="data-description"></a>Datenbeschreibung
Ausführliche Informationen zu den Daten finden Sie unter der [Beschreibung](https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.names) des UCI-Repository. 

Diese Daten wurden aus der Datenbank des Census Bureau unter: https://www.census.gov/en.html extrahiert. 


* Es gibt insgesamt 48.842 Instanzen (vor Filtern), eine Mischung aus kontinuierlichen und diskreten Instanzen(Trainieren = 32.561, Testen = 16.281)
* Die Wahrscheinlichkeit für die Bezeichnung '>50 K': 23,93 % / 24,78 % (ohne Unbekannte)
* Die Wahrscheinlichkeit für die Bezeichnung '<=50 K': 76,07 % / 75,22 % (ohne Unbekannte)  

* **ZIEL**: Einkommensklasse '>50 K', '<=50 K'. Diese werden jeweils durch 1 und 0 in der Datenvorbereitungsphase ersetzt.
* **FUNKTIONEN**: Alter, Arbeitsklasse, Ausbildung, Bildungsstand, ethnische Gruppe, Geschlecht, Arbeitsstunden pro Woche, usw.


## <a name="project-structure-execution-and-reporting"></a>Projektstruktur, Ausführung und Berichterstellung

### <a name="structure"></a>Strukturdefinition
Für dieses Projekt verwenden wir die TDSP-Ordnerstruktur und die TDSP-Dokumentationsvorlagen (unten), die dem [TDSP-Lebenszyklus](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md) folgen. 

Das Projekt wird nach den [hier](https://aka.ms/how-to-use-tdsp-in-aml) bereitgestellten Anweisungen erstellt. Nachdem es mit dem Projekt-Code und den Projektartefakten aufgefüllt wird, sieht die Struktur wie folgt aus (siehe Projektstruktur in rot der folgenden Abbildung).


<img src="./media/scenario-tdsp-classifying-us-incomes/instantiation-4.png" width="900" height="700">

### <a name="execution"></a>Ausführung
In diesem Beispiel führen wir Code in der **lokalen Serverumgebung** aus. Weitere Details zu [Ausführungsoptionen](experiment-execution-configuration.md) finden Sie in den Azure Machine Learning-Dokumenten.

Es ist einfach, ein Python-Skript in einer lokalen Python-Laufzeit auszuführen:

    az ml experiment submit -c local my_script.py

Sie können in der Projektstruktur auf der linken Seite der Benutzeroberfläche von Azure Machine Learning zweimal auf die IPython Notebook-Dateien klicken und sie im Jypyter Notebook-Server ausführen.


Die Schritte des Data Science-Workflow sahen wie folgt aus:

* [**Die Datenerfassung und -auswertung**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/01_data_acquisition_and_understanding)

Die Daten wurden im CSV-Format aus URLs des UCI ML-Repositorys heruntergeladen [[Link]](https://archive.ics.uci.edu/ml/datasets/adult). Funktionen, Ziel und die Transformationen werden in der Datei ProjectReport.md im Detail beschrieben.

Code für die Datenerfassung und -auswertung befindet sich hier: / Code/01_data_acquisition_and_understanding.

Durchsuchen von Daten erfolgt mithilfe des [IDEAR-Dienstprogramms (Interaktive Datensuche und Berichterstattung)](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils/Python) von Python-3, das als Teil der [TDSP-Suite von Data Science-Tools](https://github.com/Azure/Azure-TDSP-Utilities) veröffentlicht wird. Dieses Dienstprogramm kann zum Generieren von standardisierten Berichten zum Durchsuchen von Daten genutzt werden, die numerische und kategorische Funktionen und Ziele enthalten. Details zur Verwendung des Hilfsprogramms Python 3 IDEAR werden unten bereitgestellt. 

Der Speicherort des letzten Berichts der Datenuntersuchung lautet [IDEAR.html](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/docs/deliverable_docs). Eine Ansicht des IDEAR-Berichts wird unten angezeigt:

![](./media/scenario-tdsp-classifying-us-incomes/idear.png)

* [**Modellierung**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/02_modeling)

Wir haben zwei Modelle mit der dreifachen Kreuzvalidierung erstellt: Elastic Net und Random Forest. Wir haben [59 Point-Sampling](http://www.jmlr.org/papers/volume13/bergstra12a/bergstra12a.pdf)für eine zufällige Rastersuche als Strategie für die Kreuzvalidierung und Optimierung der Modellparameter verwendet. Die Genauigkeit der Modelle wurden mit AUC (Bereich unter der Kurve) im Test-DataSet gemessen. 

Code für die Modellierung befindet sich hier: / Code/02_modeling.

AUC der Elastic Net- und Random Forest-Modelle waren > 0,85. Wir speichern beide Modelle in pickled.pkl-Dateien und geben die ROC-Plots beider Modelle aus. AUC des Random Forest- und des Elastic Net-Modells betrug jeweils 0,92 und 0,90. Darüber hinaus wird bei der Modellinterpretation die Funktionsgewichtung für das Random Forest-Modell in einer CSV-Datei ausgegeben und in einer PDF-Datei (nur die Top 20 Vorhersagefunktionen) dargestellt.

ROC-Kurve des **Random Forest-Modells** auf den Testdaten wird unten gezeigt. Dies war das Modell, das bereitgestellt wurde:

![](./media/scenario-tdsp-classifying-us-incomes/rf-auc.png)

Funktionsgewichtung (Top-20) des Random Forest-Modells wird unten dargestellt. Funktionen wie Kapitalgewinn, Bildungsstand, Familienstand haben die höchste Gewichtung.

![](./media/scenario-tdsp-classifying-us-incomes/featImportance.png)

* [**Bereitstellung**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/03_deployment)

Wir haben das Random Forest-Modell als Webdienst auf einem Cluster im [Azure Container Service (ACS)](https://azure.microsoft.com/services/container-service/) bereitgestellt. Die Operationalisierungsumgebung stellt Docker und Kubernetes im Cluster bereit, um die Bereitstellung der Webdienste zu verwalten. Weitere Informationen zum Operationalisierungsvorgang finden Sie [hier](model-management-service-deploy.md). 

Der Code für die Bereitstellung ist hier gespeichert: /code/03_deployment.


### <a name="final-project-reporthttpsgithubcomazuremachinelearningsamples-tdspuciadultincomeblobmasterdocsdeliverabledocsprojectreportmd"></a>[Endgültiger Projektbericht](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md)
Details zu jedem der oben aufgeführten Abschnitte werden im kompiliertem endgültigen Projektbericht [ProjectReport](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md) bereitgestellt. Der Projektbericht enthält auch weitere Details zu Anwendungsfall, Modellleistungsmetriken, Bereitstellung und Infrastruktur, auf dem das Projekt entwickelt und bereitgestellt wurde.

Der Projektbericht zusammen mit den Inhalten des gesamten Projektordners und dem Repository der Versionskontrolle werden möglicherweise an den Client übermittelt.


## <a name="conclusion"></a>Zusammenfassung

In diesem Beispiel werden Anweisungen zur Verwendung von TDSP-Struktur und -Vorlagen in Azure Machine Learning bereitgestellt. Mit den Dokument- und Artefaktvorlagen können Sie folgende Aktionen ausführen:
1. Zweck und Umfang eines Projekts definieren
2. Ein Projektteam mit verteilten Rollen und Zuständigkeiten erstellen
3. Projekte gemäß den TDSP-Lebenszyklusphasen strukturieren und ausführen
4. Standardisierte Berichte mithilfe der TDSP Data Science-Dienstprogramme (z.B. die IDEAR-Bericht zur Untersuchung und Visualisierung von Daten) entwickeln
5. Einen endgültigen Data Science-Projektbericht vorbereiten, der an einen Client übermittelt werden kann

Wir hoffen, dass Sie diese Funktion von Azure Machine Learning verwenden, um die Standardisierung und Zusammenarbeit mit Ihren Data Science-Teams zu vereinfachen.

## <a name="next-steps"></a>Nächste Schritte

Konsultieren Sie die folgenden Referenzen für Ihre ersten Schritte:

[How to use Team Data Science Process (TDSP) in Azure Machine Learning (Verwenden von Team Data Science-Prozess (TDSP) in Azure Machine Learning)](https://aka.ms/how-to-use-tdsp-in-aml)

[Team Data Science-Prozess (TDSP)](https://github.com/Azure/Microsoft-TDSP)

[TDSP project template for Azure Machine Learning (TDSP-Projektvorlage für Azure Machine Learning)](https://aka.ms/tdspamlgithubrepo)

[US Income data-set from UCI ML repository (Einkommensdatensatz der USA aus dem UCI ML-Repository)](https://archive.ics.uci.edu/ml/datasets/adult)