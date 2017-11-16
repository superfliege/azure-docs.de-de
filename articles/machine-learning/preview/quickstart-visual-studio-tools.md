---
title: "Schnellstartanleitung für Visual Studio Code-Tools für Machine Learning in Azure | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt, wie Sie Visual Studio Code-Tools für Machine Learning verwenden, um ein Experiment zu erstellen, das Modell zu trainieren und einen Webdienst zu operationalisieren."
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: get-started-article
ms.date: 09/12/2017
ms.openlocfilehash: 400fc384519f2ff5c9bb7d83dab6499f5008a833
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="visual-studio-code-tools-for-ai"></a>Visual Studio Code Tools for AI
Visual Studio Code Tools for AI ist eine Entwicklungserweiterung zum Erstellen, Testen und Bereitstellen von Deep Learning- und KI-Lösungen. Die Erweiterung lässt sich nahtlos in Azure Machine Learning integrieren und zeigt den Ausführungsverlauf sowie die Leistung vorheriger Trainings und benutzerdefinierter Metriken an. Sie bietet eine Explorer-Ansicht mit Beispielen zum Durchsuchen und Durchführen eines Bootstraps eines neuen Projekts mit dem [Microsoft Cognitive Toolkit](http://www.microsoft.com/en-us/cognitive-toolkit) (früher als CNTK bezeichnet), [Google TensorFlow](https://www.tensorflow.org) und anderen Deep Learning-Frameworks. Sie bietet zudem einen Explorer für Computeziele, mit dem Sie Aufträge übermitteln können, um Modelle in Remoteumgebungen wie Azure Virtual Machines oder Linux-Servern mit GPU zu trainieren. 
 
## <a name="getting-started"></a>Erste Schritte 
Um zu beginnen, müssen Sie zunächst [Visual Studio Code](https://code.visualstudio.com/Download) herunterladen und installieren. Sobald Visual Studio Code geöffnet ist, führen Sie die folgenden Schritte aus:
1. Klicken Sie in der Aktivitätsleiste auf das Symbol für Erweiterungen. 
2. Suchen Sie nach „Visual Studio Code Tools for AI“. 
3. Klicken Sie auf die Schaltfläche **Installieren**. 
4. Klicken Sie nach der Installation auf die Schaltfläche **Erneut laden**. 

Sobald Visual Studio Code neu geladen wurde, ist die Erweiterung aktiviert. [Hier erhalten Sie weitere Informationen zum Installieren von Erweiterungen](https://code.visualstudio.com/docs/editor/extension-gallery).

## <a name="exploring-project-samples"></a>Untersuchen von Projektbeispielen
Im Lieferumfang von Visual Studio Code Tools for AI ist ein Beispiel-Explorer enthalten. Mit diesem Beispiel-Explorer können Sie ganz einfach und mit einigen wenigen Klicks Beispiele ermitteln und ausprobieren. Gehen Sie zum Öffnen des Explorers folgendermaßen vor:   
1. Öffnen Sie die Befehlspalette (Menü „Ansicht“ > **Befehlspalette** oder **STRG+UMSCHALTTASTE+P**).
2. Geben Sie „AI Sample“ ein. 
3. Sie erhalten eine Empfehlung für „AI: Open Azure ML Sample Explorer“. Wählen Sie diese Option aus, und drücken Sie die EINGABETASTE. 

Alternativ dazu können Sie auf das Symbol des Beispiel-Explorers klicken.

## <a name="creating-a-new-project-from-the-sample-explorer"></a>Erstellen eines neuen Projekts aus dem Beispiel-Explorer 
Sie können verschiedene Beispiele durchsuchen und weitere Informationen dazu erhalten. Suchen Sie das Beispiel „Classifying Iris“. Um auf Grundlage dieses Beispiels ein neues Projekt zu erstellen, gehen Sie folgendermaßen vor:
1. Klicken Sie im Projektbeispiel auf die Schaltfläche zum Installieren, und beachten Sie die angezeigten Befehle. Diese führen Sie durch die notwendigen Schritte zum Erstellen eines neuen Projekts. 
2. Wählen Sie einen Namen für das Projekt aus, z.B. „Iris“.
3. Wählen einen Pfad aus, in dem das Projekt erstellt werden soll, und drücken Sie die EINGABETASTE. 
4. Wählen Sie einen vorhandenen Arbeitsbereich aus, und drücken Sie die EINGABETASTE.

Das Projekt wird erstellt.

> [!TIP]
> Sie müssen angemeldet sein, um auf Ihre Azure-Ressource zuzugreifen. Geben Sie im integrierten Terminal die Zeichenfolge „az login“ ein, und folgen Sie den Anweisungen. 

## <a name="submitting-experiment-with-the-new-project"></a>Übermitteln eines Experiments mit dem neuen Projekt
Wenn das neue Projekt in Visual Studio Code offen ist, übermitteln Sie einen Auftrag an ein anderes Computeziel (lokal und VM mit Docker).
Visual Studio Code Tools for AI bietet verschiedene Möglichkeiten, ein Experiment zu übermitteln. 
1. Kontextmenü (Rechtsklick) – **AI: Submit Job** („KI: Auftrag übermitteln“).
2. Über die Befehlspalette: „AI: Submit Job“ („KI: Auftrag übermitteln“).
3. Alternativ dazu können Sie den Befehl auch im integrierten Terminal direkt mit der Azure-Befehlszeilenschnittstelle (Machine Learning-Befehle) ausführen.

Öffnen Sie „iris_sklearn.py“, klicken Sie mit der rechten Maustaste, und wählen Sie **AI: Submit Job** („KI: Auftrag übermitteln“) aus.
1. Wählen Sie Ihre Plattform aus: „Azure Machine Learning“.
2. Wählen Sie Ihre Ausführungskonfiguration aus: „Docker-Python“.

> [!NOTE]
> Wenn Sie zum ersten Mal einen Auftrag übermitteln, wird die folgende Meldung angezeigt: „No Machine Learning configuration found, creating...“ (Keine Machine Learning-Konfiguration gefunden. Konfiguration wird erstellt...). Eine JSON-Datei wird geöffnet. Speichern Sie diese (**STRG+S**).

Sobald der Auftrag übermittelt wurde, zeigt das integrierte Terminal den Fortschritt der Ausführungen an. 

## <a name="view-list-of-jobs"></a>Anzeigen einer Liste der Aufträge
Sobald die Aufträge übermittelt wurden, können Sie die Aufträge im Ausführungsverlauf auflisten.
1. Öffnen Sie die Befehlspalette (Menü „Ansicht“ > **Befehlspalette** oder **STRG+UMSCHALTTASTE+P**).
2. Geben Sie „AI List“ („KI auflisten“) ein.
3. Sie erhalten eine Empfehlung für „AI: List Jobs“ („KI: Aufträge auflisten“). Wählen Sie die Empfehlung aus, und drücken Sie die EINGABETASTE.

Die Auftragslistenansicht wird geöffnet und zeigt alle Ausführungen sowie einige zugehörige Informationen an.

## <a name="view-job-details"></a>Anzeigen von Auftragsdetails
Während die Auftragslistenansicht noch geöffnet ist, klicken Sie auf die erste Ausführung in der Liste.
Um genaue Details zu den Ergebnissen eines Auftrags zu erhalten, klicken Sie oben auf die **Auftrags-ID**. 

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Konfigurieren von Azure Machine Learning Workbench zum Arbeiten mit einer IDE](./how-to-configure-your-IDE.md)
