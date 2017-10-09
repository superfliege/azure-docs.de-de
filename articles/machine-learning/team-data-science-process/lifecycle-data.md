---
title: "Phase der Datenerfassung und -auswertung des Team Data Science-Prozess-Lebenszyklus – Azure | Microsoft-Dokumentation"
description: "Die Ziele, Aufgaben und Projektleistungen für die Phase der Datenerfassung und -auswertung Ihrer Data Science-Projekte."
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
ms.openlocfilehash: eea3c357ebf6ad920c0ddebdb979aa07aece4794
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="data-acquisition-and-understanding"></a>Datenerfassung und -auswertung

In diesem Thema werden die Ziele, Aufgaben und Projektleistungen in Zusammenhang mit der **Phase der Datenerfassung und -auswertung** des Team Data Science-Prozesses behandelt. Dieser Prozess bietet einen empfohlenen Lebenszyklus, mit dem Sie Ihre Data Science-Projekte strukturieren können. Der Lebenszyklus beschreibt die wichtigsten Phasen, die Projekte typischerweise, oft iterativ, durchlaufen:

* **Geschäftliche Aspekte**
* **Datenerfassung und -auswertung**
* **Modellierung**
* **Bereitstellung**
* **Kundenakzeptanz**

Hier ist der **Team Data Science-Prozesslebenszyklus** grafisch dargestellt. 

![TDSP-Lebenszyklus 2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Ziele
* Ein sauberes Dataset von hoher Qualität, bei dem die Beziehungen zu den Zielvariablen in der entsprechenden Analyseumgebung klar sind und das bereit für die Modellierung ist.
* Eine Lösungsarchitektur der Datenpipeline wurde entwickelt, um Daten regelmäßig aktualisieren und bewerten zu können.

## <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase werden drei Hauptaufgaben durchgeführt:

* **Erfassen der Daten** in der Zielanalyseumgebung.
* **Untersuchen der Daten**, um zu ermitteln, ob die Datenqualität für die Beantwortung der Frage ausreicht. 
* **Einrichten einer Datenpipeline**, um neue oder regelmäßig aktualisierte Daten bewerten zu können.

### <a name="21-ingest-the-data"></a>2.1 Erfassen der Daten
Richten Sie den Prozess so ein, dass die Daten von den Quellspeicherorten an die Zielspeicherorte verschoben werden, an denen Analysevorgänge wie das Trainieren oder das Erstellen von Vorhersagen ausgeführt werden. Technische Details und Optionen zur Vorgehensweise mit den verschiedenen Azure-Datendiensten finden Sie unter [Laden von Daten in Speicherumgebungen für Analysen](ingest-data.md). 

### <a name="22-explore-the-data"></a>2.2 Untersuchen der Daten
Bevor Sie Modelle trainieren können, müssen Sie ein gutes Verständnis der Daten entwickeln. Datasets sind in der Praxis häufig überladen oder weisen fehlende Werte oder verschiedene andere Diskrepanzen auf. Sie können die Datenzusammenfassung und -visualisierung verwenden, um die Qualität Ihrer Daten zu überprüfen und die Informationen bereitzustellen, die vor der Modellerstellung zum Verarbeiten der Daten erforderlich sind. Dieser Prozess ist häufig iterativ.

TDSP verfügt über ein automatisiertes Hilfsprogramm mit dem Namen [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils), mit dem Daten visualisiert und Berichte zur Datenzusammenfassung visualisiert werden können. Es wird empfohlen, mit IDEAR zu beginnen, um die Daten zu untersuchen. Auf diese Weise können Sie interaktiv ohne Codierung ein erstes Verständnis der Daten entwickeln und anschließend benutzerdefinierten Code für die Untersuchung und Visualisierung der Daten schreiben. Eine Anleitung zum Bereinigen der Daten finden Sie unter [Aufgaben zur Vorbereitung von Daten für erweitertes Machine Learning](prepare-data.md).  

Wenn Sie mit der Qualität der bereinigten Daten zufrieden sind, besteht der nächste Schritt darin, die inhärenten Muster der Daten besser zu verstehen. Mit diesen Informationen können Sie eine passendes Vorhersagemodell für Ihr Ziel auswählen und entwickeln. Suchen Sie nach Anhaltspunkten dafür, wie gut die Daten mit dem Ziel verbunden sind und ob ausreichend Daten vorhanden sind, um mit den nächsten Schritten der Modellierung fortfahren zu können. Auch dieser Prozess ist häufig iterativ. Unter Umständen müssen Sie nach neuen Datenquellen mit genaueren bzw. relevanteren Daten suchen, um das Dataset zu erweitern, das im vorherigen Schritt ursprünglich identifiziert wurde.  

### <a name="23-set-up-a-data-pipeline"></a>2.3 Einrichten einer Datenpipeline
Zusätzlich zur ersten Erfassung und Bereinigung der Daten müssen Sie normalerweise einen Prozess einrichten, um neue Daten zu bewerten oder die Daten im Rahmen des ständigen Lernprozesses regelmäßig zu aktualisieren. Dies kann durch Einrichten einer Datenpipeline oder eines Workflows erfolgen. Hier ist ein [Beispiel](move-sql-azure-adf.md) dafür angegeben, wie Sie eine Pipeline mit der [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) einrichten. 

In dieser Phase wird eine Lösungsarchitektur der Datenpipeline entwickelt. Die Pipeline wird außerdem parallel zu den folgenden Phasen des Data Science-Projekts entwickelt. Die Pipeline kann auf Batches basieren oder eine Streaming-/Echtzeit- oder Hybridpipeline sein. Dies richtet sich nach Ihren geschäftlichen Anforderungen und den Einschränkungen Ihrer vorhandenen Systeme, in die diese Lösung integriert wird. 

## <a name="artifacts"></a>Artefakte
Hier sind die Ergebnisse dieser Phase angegeben:

* [**Bericht zur Datenqualität**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): Dieser Bericht enthält Datenzusammenfassungen, Beziehungen zwischen den einzelnen Attributen und Zielen, die Rangfolge der Variablen usw. Mit dem in TDSP enthaltenen Tool [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) können Sie diesen Bericht für alle Datasets in Tabellenform schnell erstellen, z.B. eine CSV-Datei oder eine relationale Tabelle. 
* **Lösungsarchitektur**: Dies kann ein Diagramm oder eine Beschreibung Ihrer Datenpipeline sein, die zum Ausführen von Bewertungen oder Vorhersagen für neue Daten nach der Erstellung eines Modells verwendet wird. Die Pipeline zum erneuten Trainieren Ihres Modells basierend auf den neuen Daten ist ebenfalls enthalten. Das Dokument ist im Verzeichnis [Projekt](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) gespeichert, wenn Sie die Vorlage für die TDSP-Verzeichnisstruktur verwenden.
* **Prüfpunktentscheidung**: Bevor Sie mit der vollständigen Featureentwicklung und Modellerstellung beginnen, können Sie das Projekt erneut auswerten, um zu ermitteln, ob Daten in ausreichender Menge vorhanden sind. Es kann beispielsweise sein, dass Sie bereit zum Fortfahren sind, mehr Daten erfassen müssen oder das Projekt beenden müssen, weil für die Beantwortung der Frage keine Daten zur Verfügung stehen.

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie Links zu jedem Schritt im Lebenszyklus des Team Data Science-Prozess:

* [1. Geschäftliche Aspekte](lifecycle-business-understanding.md)
* [2. Datenerfassung und -auswertung](lifecycle-data.md)
* [3. Modellierung](lifecycle-modeling.md)
* [4. Bereitstellung](lifecycle-deployment.md)
* [5. Kundenakzeptanz](lifecycle-acceptance.md)

Vollständige exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für **bestimmte Szenarien** gezeigt werden, sind ebenfalls verfügbar. Sie sind im Thema [Exemplarische Vorgehensweisen](walkthroughs.md) aufgeführt und mit Miniaturansichtsbeschreibungen verlinkt. Sie zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden.  

Beispiele zum Ausführen der Schritte im Team Data Science-Prozess mit Azure Machine Learning Studio finden Sie im Lernpfad [Mit Azure ML](http://aka.ms/datascienceprocess).
