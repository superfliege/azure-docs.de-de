---
title: "Phase der geschäftlichen Aspekte des Team Data Science-Prozess-Lebenszyklus – Azure | Microsoft-Dokumentation"
description: "Die Ziele, Aufgaben und Projektleistungen für die Phase der geschäftlichen Aspekte Ihrer Data Science-Projekte."
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
ms.date: 09/02/2017
ms.author: bradsev;
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 2adce61f8185bd86a6a870bb5752fe936701b0af
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="business-understanding"></a>Geschäftliche Aspekte

In diesem Thema werden die Ziele, Aufgaben und Projektleistungen in Zusammenhang mit der **Phase der geschäftlichen Aspekte** des Team Data Science-Prozesses behandelt. Dieser Prozess bietet einen empfohlenen Lebenszyklus, mit dem Sie Ihre Data Science-Projekte strukturieren können. Der Lebenszyklus beschreibt die wichtigsten Phasen, die Projekte typischerweise, oft iterativ, durchlaufen:

* **Geschäftliche Aspekte**
* **Datenerfassung und -auswertung**
* **Modellierung**
* **Bereitstellung**
* **Kundenakzeptanz**

Hier ist der **Team Data Science-Prozesslebenszyklus** grafisch dargestellt. 

![TDSP-Lebenszyklus 2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Ziele
* Es werden die **Hauptvariablen** angegeben, die als **Modellziele** dienen sollen und deren verwandte Metriken verwendet werden, um den Erfolg des Projekts zu bestimmen.
* Die relevanten **Datenquellen**, auf die das Unternehmen Zugriff hat oder benötigt, werden identifiziert.

## <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase werden zwei Hauptaufgaben durchgeführt: 

* **Definieren von Zielen**: Ermitteln Sie zusammen mit Ihrem Kunden und anderen Projektbeteiligten die geschäftlichen Probleme, um sich damit vertraut zu machen. Formulieren Sie Fragestellungen, mit denen die geschäftlichen Ziele definiert werden und die mit Data Science-Verfahren gelöst werden können.
* **Identifizieren von Datenquellen**: Suchen Sie nach den relevanten Daten, mit denen Sie die Fragen beantworten können, die den Zielen des Projekts zugrunde liegen.

### <a name="11-define-objectives"></a>1.1 Definieren von Zielen

1. Ein zentrales Ziel dieses Schritts ist die Identifizierung der wichtigsten **geschäftlichen Variablen**, die von der Analyse vorhergesagt werden müssen. Diese Variablen werden als **Modellziele** bezeichnet, und die dazugehörigen Metriken werden verwendet, um den Erfolg des Projekts zu bestimmen. Zwei Beispiele für diese Ziele sind Umsatzprognosen oder die Wahrscheinlichkeit, dass eine Bestellung betrügerisch ist.

2. Definieren Sie die **Projektziele**, indem Sie präzise Fragen stellen und verfeinern, die relevant, spezifisch und eindeutig sind. Data Science ist der Prozess zur Verwendung von Namen und Zahlen mit dem Ziel, Antworten auf diese Fragen zu finden. Weitere Informationen zum Stellen präziser Fragen finden Sie im Blog [How to do Data Science](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) (Informationen zu Data Science). Data Science/Machine Learning wird normalerweise verwendet, um fünf Arten von Fragen zu beantworten:
 
   * Wie viel bzw. wie viele? (Regression)
   * Welche Kategorie? (Klassifizierung)
   * Welche Gruppe? (Clustering)
   * Ist dies merkwürdig? (Anomalieerkennung)
   * Welche Option sollte gewählt werden? (Empfehlung)

    Legen Sie fest, welche dieser Fragen gestellt wird und wie durch ihre Beantwortung Ihre Geschäftsziele erreicht werden.

3. Definieren Sie das **Projektteam**, indem Sie die Rollen und Zuständigkeiten der Mitglieder angeben. Entwickeln Sie einen groben Plan mit Meilensteinen, den Sie abarbeiten, wenn weitere Informationen ermittelt werden.  

4. **Definieren von Erfolgsmetriken**: Beispiel: Erreichung einer Genauigkeit von X% in Bezug auf die Prognose der Kundenabwanderung am Ende dieses dreimonatigen Projekts, damit wir Sonderangebote anbieten können, um die Abwanderung zu reduzieren. Die Metriken sollten **SMART** sein: 
   * **S**pecific (Spezifisch) 
   * **M**easurable (Messbar)
   * **A**chievable (Realistisch) 
   * **R**elevant (Relevant) 
   * **T**ime-bound (Zeitbezogen) 

### <a name="12-identify-data-sources"></a>1.2 Identifizieren von Datenquellen
Identifizieren Sie Datenquellen, die bekannte Beispiele für Antworten auf Ihre präzisen Fragen enthalten. Suchen Sie nach den folgenden Daten:

* Daten, die für die Frage **relevant** sind. Verfügen wir über Kennzahlen für das Ziel und Features, die sich auf das Ziel beziehen?
* Dies sind Daten, bei denen es sich um **genaue Kennzahlen** unseres Modellziels und die passenden Features handelt.

Es ist z. B. nicht ungewöhnlich festzustellen, dass vorhandene Systeme zusätzliche Arten von Daten sammeln und protokollieren müssen, um das Problem anzugehen und die Projektziele zu erreichen. In diesem Fall kann es ratsam sein, nach externen Datenquellen zu suchen oder die Systeme zu aktualisieren, um neue Daten zu erfassen.

## <a name="artifacts"></a>Artefakte
Hier sind die Ergebnisse dieser Phase angegeben:

* [**Charta-Dokument**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): In der Strukturdefinition des TDSP-Projekts ist eine Standardvorlage angegeben. Dieses Dokument wird während des gesamten Projekts entsprechend aktualisiert, wenn neue Erkenntnisse gewonnen werden und sich geschäftliche Anforderungen ändern. Der Schlüssel ist, dass dieses Dokument immer wieder durchlaufen wird und während des Ermittlungsprozesses weitere Details hinzugefügt werden. Beziehen Sie den Kunden und andere Projektbeteiligte in das Vornehmen der Änderungen ein, und informieren Sie sie eindeutig über die Gründe für die Änderungen.  
* [**Datenquellen**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): Dies ist der Abschnitt **Raw Data Sources** (Rohdatenquellen) des Berichts **Datendefinitionen** im Ordner **Data Report** (Datenbericht) des TDSP-Projekts. Er enthält den ursprünglichen Speicherort und den Zielspeicherort für die Rohdaten. In späteren Phasen fügen Sie weitere Details ein, z.B. Skripts, um die Daten in die Analyseumgebung zu verschieben.  
* [**Data Dictionaries**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/DataDictionaries) (Datenwörterbücher): Dieses Dokument enthält Beschreibungen der Daten, die durch den Client bereitgestellt werden. Diese Beschreibungen enthalten Informationen zum Schema (Datentypen, Informationen zu Validierungsregeln, sofern vorhanden) und zu den Diagrammen zu den Entitätsbeziehungen.

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie Links zu jedem Schritt im Lebenszyklus des Team Data Science-Prozess:

* [1. Geschäftliche Aspekte](lifecycle-business-understanding.md)
* [2. Datenerfassung und -auswertung](lifecycle-data.md)
* [3. Modellierung](lifecycle-modeling.md)
* [4. Bereitstellung](lifecycle-deployment.md)
* [5. Kundenakzeptanz](lifecycle-acceptance.md)

Vollständige exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für **bestimmte Szenarien** gezeigt werden, sind ebenfalls verfügbar. Sie sind im Thema [Exemplarische Vorgehensweisen](walkthroughs.md) aufgeführt und mit Miniaturansichtsbeschreibungen verlinkt. Sie zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden. 

Beispiele zum Ausführen der Schritte im Team Data Science-Prozess mit Azure Machine Learning Studio finden Sie im Lernpfad [Mit Azure ML](http://aka.ms/datascienceprocess).
