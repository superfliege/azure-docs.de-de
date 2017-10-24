---
title: Vorhersehen der Kundenabwanderung mithilfe von Azure Machine Learning | Microsoft-Dokumentation
description: Informationen zum Analysieren der Kundenabwanderung mithilfe von Azure ML Workbench.
services: machine-learning
documentationcenter: 
author: miprasad
manager: kristin.tolle
editor: miprasad
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: miprasad
ms.openlocfilehash: 2ee7ec91700c66d5bedd917d0203a726b5c5e300
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="customer-churn-prediction-using-azure-machine-learning"></a>Vorhersehen der Kundenabwanderung mithilfe von Azure Machine Learning

Im Durchschnitt kostet die Bewahrung von Bestandskunden fünfmal weniger als die Gewinnung von Neukunden. Daher versuchen Marketingverantwortliche häufig, die Wahrscheinlichkeit einer Abwanderung von Kunden einzuschätzen und die notwendigen Maßnahmen zur Minimierung der Abwanderungsquote zu finden.

Ziel dieser Lösung ist das Demonstrieren einer Analyse einer vorhersehbaren Kundenabwanderung mithilfe von Azure Machine Learning Workbench. Diese Lösung bietet Einzelhändlern eine einfach zu nutzende Vorlage für die Entwicklung von Datenpipelines für die Vorhersage von Kundenabwanderungen. Die Vorlage kann mit unterschiedlichen Datasets und Definitionen von Kundenabwanderung verwendet werden. Das praktische Beispiel hat folgende Ziele:

1. Veranschaulichen der Datenvorbereitungstools von Azure Machine Learning Workbench zur Bereinigung und Erfassung von Kundenbeziehungsdaten für die Kundenabwanderungsanalyse

2. Durchführen von Featuretransformation zum Herausfiltern verrauschter heterogener Daten

3. Integrieren von Bibliotheken von Drittanbietern (wie z.B. `scikit-learn` und `azureml`) zum Entwickeln von bayesschen und strukturbasierten Klassifizierern für die Vorhersage der Kundenabwanderung

4. Bereitstellen der Lösung

## <a name="link-of-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys
Im Folgenden finden Sie den Link zum öffentlichen GitHub-Repository, in dem der gesamte Code gehostet wird:

[https://github.com/Azure/MachineLearningSamples-ChurnPrediction](https://github.com/Azure/MachineLearningSamples-ChurnPrediction)

## <a name="use-case-overview"></a>Übersicht über den Anwendungsfall
Unternehmen brauchen eine wirkungsvolle Strategie für den Umgang mit Kundenabwanderung. Kundenabwanderung beschreibt Kunden, die die Nutzung eines Diensts einstellen, zu einem Dienst eines Mitbewerbers wechseln, zu einem niedrigerer Tarif im Dienst wechseln oder ihr Engagement mit dem Dienst verringern.

In diesem Anwendungsfall untersuchen wir die Daten des französischen Telekommunikationsunternehmens Orange. Um dessen Dienstangebot zu verbessern, bestimmen wir Kunden, die in naher Zukunft wahrscheinlich abwandern werden, und entwickeln maßgeschneiderte Kampagnen, die zur Kundenbindung beitragen.

Telekommunikationsunternehmen sind auf einem wettbewerbsintensiven Markt aktiv. Viele Unternehmen verlieren aufgrund der Abwanderung von Vertragskunden Umsatz. Daher kann die Fähigkeit, Kundenabwanderungen genau zu bestimmen, ein großer Wettbewerbsvorteil sein.

Zu den Faktoren, die zur Abwanderung von Telekommunikationskunden beitragen, gehören unter anderem:

* Wahrgenommene häufige Dienstausfälle
* Schlechte Erfahrungen mit dem Kundendienst (online oder in Filialen)
* Angebote von Mitbewerbern (besserer Familientarif, Datentarif usw.).

In dieser Lösung verwenden wir ein konkretes Beispiel für den Aufbau eines Modells zur Vorhersage der Kundenabwanderung für Telekommunikationsunternehmen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar).

* Eine gemäß dem [Schnellstart-Installationshandbuch](./quickstart-installation.md) installierte Instanz von [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) zur Installation des Programms und der Erstellung eines Arbeitsbereichs.

* Für die Operationalisierung wird empfohlen, dass Sie das Docker-Modul installieren und lokal ausführen. Wenn dies nicht erfolgt, können Sie die Clusteroption verwenden. Beachten Sie allerdings, dass die Ausführung eines Azure Container Service (ACS) häufig sehr kostspielig sein kann.

* Bei dieser Lösung wird davon ausgegangen, dass Sie die Azure Machine Learning Workbench unter Windows 10 mit einem lokal installierten Docker-Modul ausführen. Wenn Sie macOS verwenden, sind die Anweisungen weitgehend identisch.

## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie ein neues Projekt mit diesem Beispiel als Vorlage:
1.  Öffnen Sie Azure Machine Learning Workbench.
2.  Klicken Sie auf der Seite **Projekte** auf **+**, wählen Sie **Neues Projekt** aus, und signieren Sie es.
3.  Geben Sie im Bereich **Neues Projekt erstellen** die Informationen für das neue Projekt ein.
4.  Geben Sie im Suchfeld **Projektvorlagen suchen** den Suchbegriff „Customer Churn Prediction“ ein, und wählen Sie die Vorlage aus.
5.  Klicken Sie auf **Erstellen**

## <a name="data-description"></a>Datenbeschreibung

Das Dataset, das in der Lösung verwendet wird, stammt aus dem SIDKDD-Wettbewerb von 2009. Es heißt `CATelcoCustomerChurnTrainingSample.csv` und befindet sich im Ordner [`data`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/tree/master/data). Das Dataset besteht aus heterogenen verrauschten Daten (numerische/Kategorievariablen) des französischen Telekommunikationsunternehmens Orange und ist anonymisiert.

Die Variablen erfassen demografische Kundeninformationen, Anrufstatistiken (wie z.B. durchschnittliche Anrufdauer, Anruffehlerquote usw.), Vertragsinformationen und Reklamationsstatistiken. Die Abwanderungsvariable ist binär (0 = nicht abgewandert, 1 = abgewandert).

## <a name="scenario-structure"></a>Struktur des Szenarios

Die Ordnerstruktur ist wie folgt angeordnet:

__data__: Enthält das Dataset, das in der Lösung verwendet wird  

__docs__: Enthält die praktischen Übungseinheiten

Die Reihenfolge der praktische Übungseinheiten zum Ausführen der Lösung lautet wie folgt:
1. Vorbereitung der Daten: Die Hauptdatei im Zusammenhang mit der Vorbereitung der Daten im Ordner „data“ heißt `CATelcoCustomerChurnTrainingSample.csv`.
2. Modellierung und Auswertung: Die Hauptdatei im Zusammenhang mit der Modellierung und Auswertung im Stammordner heißt `CATelcoCustomerChurnModeling.py`.
3. Modellierung und Auswertung in Docker: Die Hauptdatei für diese Aufgabe im Stammordner heißt `CATelcoCustomerChurnModelingDocker.py`.
4. Operationalisierung: Die wichtigsten Dateien für die Bereitstellung sind das Modell (`model.pkl`) und `churn_schema_gen.py`.

| Reihenfolge| Dateiname | Zugehörige Dateien |
|--|-----------|------|
| 1 | [`DataPreparation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/DataPreparation.md) | data/CATelcoCustomerChurnTrainingSample.csv |
| 2 | [`ModelingAndEvaluation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluation.md) | CATelcoCustomerChurnModeling.py |
| 3 | [`ModelingAndEvaluationDocker.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluationDocker.md) | CATelcoCustomerChurnModelingDocker.py |
| 4 | [`Operationalization.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/Operationalization.md) | model.pkl<br>churn_schema_gen.py |

Führen Sie die Übungseinheiten in der oben angegebenen Reihenfolge aus.

## <a name="conclusion"></a>Zusammenfassung
In diesem praktische Szenario wurde veranschaulicht, wie mit Azure Machine Learning Workbench eine Vorhersage der Kundenabwanderung erfolgt. Wir haben zuerst die Daten bereinigt, um verrauschte und heterogene Daten herauszufiltern. Anschließend ist eine Feature-Entwicklung mit den Tools zur Datenvorbereitung erfolgt. Anschließend haben wir ein Klassifizierungsmodell mithilfe von Open-Source-Machine Learning-Tools erstellt und ausgewertet. Im Anschluss haben wir einen lokalen Docker-Container zum Bereitstellen des betriebsbereiten Modells in der Produktionsumgebung genutzt.
