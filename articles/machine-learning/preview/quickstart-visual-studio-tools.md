---
title: "Schnellstartanleitung für Visual Studio-Tools für Machine Learning in Azure | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie Visual Studio-Tools für Machine Learning verwenden, um ein Experiment zu erstellen, das Modell zu trainieren und einen Webdienst zu operationalisieren."
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: get-started-article
ms.date: 11/15/2017
ms.openlocfilehash: 582ec5babf2bac34f20d4e9c7517f78ee2002e0b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
# <a name="visual-studio-tools-for-ai"></a>Visual Studio-Tools für AI
Visual Studio-Tools für AI ist eine Entwicklungserweiterung zum Erstellen, Testen und Bereitstellen von Deep Learning- und KI-Lösungen. Die Erweiterung lässt sich nahtlos in Azure Machine Learning integrieren und zeigt den Ausführungsverlauf sowie die Leistung vorheriger Trainings und benutzerdefinierter Metriken an. Sie bietet eine Explorer-Ansicht mit Beispielen zum Durchsuchen und Durchführen eines Bootstraps eines neuen Projekts mit dem [Microsoft Cognitive Toolkit](http://www.microsoft.com/en-us/cognitive-toolkit) (früher als CNTK bezeichnet), [Google TensorFlow](https://www.tensorflow.org) und anderen Deep Learning-Frameworks. Sie bietet zudem einen Explorer für Computeziele, mit dem Sie Aufträge übermitteln können, um Modelle in Remoteumgebungen wie Azure Virtual Machines oder Linux-Servern mit GPU zu trainieren. Außerdem ermöglicht die Erweiterung vereinfachten Zugriff auf [Azure Batch AI (Vorschauversion)](https://docs.microsoft.com/en-us/azure/batch-ai/).
 
## <a name="getting-started"></a>Erste Schritte 
Als Erstes müssen Sie [Visual Studio](https://www.visualstudio.com/downloads/) herunterladen und installieren. Führen Sie die folgenden Schritte aus, nachdem Visual Studio geöffnet wurde:
1. Klicken Sie in Visual Studio in der Menüleiste auf „Erweiterungen und Updates...“.
2. Klicken Sie auf die Registerkarte „Online“, und wählen Sie die Option zum Durchsuchen des Visual Studio Marketplace.
3. Suchen Sie nach „Visual Studio für AI“. 
3. Klicken Sie auf die Schaltfläche **Herunterladen** . 
4. Starten Sie Visual Studio nach der Installation neu. 

Nachdem Visual Studio neu geladen wurde, ist die Erweiterung aktiv. [Hier finden Sie weitere Informationen zum Suchen nach Erweiterungen](hhttps://docs.microsoft.com/en-us/visualstudio/ide/finding-and-using-visual-studio-extensions).

> [!NOTE]
> Für Visual Studio-Tools für AI ist Visual Studio 2015 oder 2017 erforderlich (Professional oder Enterprise Edition). Die Version Apple OSX wird nicht unterstützt. 


## <a name="exploring-project-samples"></a>Untersuchen von Projektbeispielen
Im Lieferumfang von Visual Studio-Tools für AI ist ein Beispiel-Explorer enthalten. Mit diesem Beispiel-Explorer können Sie ganz einfach und mit einigen wenigen Klicks Beispiele ermitteln und ausprobieren. Gehen Sie zum Öffnen des Explorers wie folgt vor:   
1. Klicken Sie in der Menüleiste auf **AI-Tools**.
2. Klicken Sie auf „Azure Machine Learning Gallery“.

Eine Registerkarte mit allen Azure ML-Beispielen wird geöffnet.

## <a name="creating-a-new-project-from-the-sample-explorer"></a>Erstellen eines neuen Projekts aus dem Beispiel-Explorer 
Sie können verschiedene Beispiele durchsuchen und weitere Informationen dazu erhalten. Suchen Sie das Beispiel „Classifying Iris“. Gehen Sie wie folgt vor, um basierend auf diesem Beispiel ein neues Projekt zu erstellen:
1. Klicken Sie im Projektbeispiel auf die Schaltfläche **Install** (Installieren). Ein neues Dialogfeld wird geöffnet. 
2. Wählen Sie eine Ressourcengruppe, ein Konto und einen Arbeitsbereich aus.
3. Als Projekttyp können Sie die Einstellung „Allgemein“ übernehmen.
4. Geben Sie einen Projektpfad und einen Projektnamen ein, und drücken Sie die EINGABETASTE. 
5. Ein Dialogfeld mit der Aufforderung zum Speichern einer Lösung wird geöffnet. Klicken Sie auf „Speichern“. 

Nach Abschluss des Vorgangs wird in einer neuen Instanz von Visual Studio ein neues Projekt geöffnet. 

> [!TIP]
> Sie müssen angemeldet sein, um auf Ihre Azure-Ressource zugreifen zu können. Geben Sie im integrierten Terminal die Zeichenfolge „az login“ ein, und folgen Sie den Anweisungen. 

## <a name="submitting-experiment-with-the-new-project"></a>Übermitteln eines Experiments mit dem neuen Projekt
Da das neue Projekt in Visual Studio geöffnet ist, können Sie einen Auftrag an ein Computeziel senden (lokal oder VM mit Docker).
Gehen Sie wie folgt vor, um den Auftrag zu übermitteln: 
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Datei, die Sie senden möchten, und wählen Sie **Als Startdatei festlegen**.
2. Wählen Sie den Projektnamen aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Auftrag übermitteln...**.
3. Ein neues Dialogfeld wird geöffnet, in dem Sie den Cluster (oder das Computeziel) für die Ausführung Ihres Skripts auswählen können.
4. Klicken Sie auf **Senden**.

Sobald der Auftrag übermittelt wurde, zeigt das integrierte Terminal den Fortschritt der Ausführungen an.

## <a name="view-list-of-jobs"></a>Anzeigen einer Liste der Aufträge
Nachdem der Auftrag übermittelt wurde, können Sie die Aufträge im Ausführungsverlauf auflisten.
1. Klicken Sie im **Server-Explorer** auf **AI-Tools**.
2. Wählen Sie anschließend **Azure Machine Learning**.
3. Klicken Sie auf das Menü **Aufträge**.

Im Auftrags-Explorer werden alle übermittelten Experimente für das Projekt aufgelistet. 

## <a name="view-job-details"></a>Anzeigen von Auftragsdetails
Klicken Sie bei geöffneter Auftrags-Explorer-Ansicht auf die erste Ausführung in der Liste.
Der Bereich „Auftragszusammenfassung“ und der Bereich für Protokolle und Ausgaben werden angezeigt.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Konfigurieren von Azure Machine Learning Workbench zum Arbeiten mit einer IDE](./how-to-configure-your-IDE.md)
