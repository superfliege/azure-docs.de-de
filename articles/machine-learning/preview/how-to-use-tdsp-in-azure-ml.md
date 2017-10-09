---
title: Strukturieren von Projekten mit Team Data Science-Prozessvorlagen | Microsoft-Dokumentation
description: "Instanziieren der Team Data Science-Prozessvorlagen (TDSP) in Azure ML, die Projekte für die Zusammenarbeit strukturieren."
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
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 2e03b6bcb1bc5a617234c7801f22f8207a9f5a4e
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="structure-projects-with-team-data-science-process-template"></a>Strukturieren von Projekten mit Team Data Science-Prozessvorlagen

Dieses Dokument enthält Anleitungen zum Erstellen von Data Science-Projekten in Azure Machine Learning mit Team Data Science-Prozessvorlagen (TDSP), die Projekte für die Zusammenarbeit und die Reproduzierbarkeit strukturieren. 


## <a name="what-is-team-data-science-process"></a>Was ist der Team Data Science-Prozess?
Beim Team Data Science-Prozess handelt es sich um einen agilen, iterativen Data Science-Prozess zum Ausführen und Bereitstellen von erweiterten Analysemethoden. Dieser dient zum Verbessern der Zusammenarbeit und der Effizienz von Data Science-Teams in Unternehmen. Es werden diese Ziele mit vier Hauptkomponenten unterstützt:

1. Eine Standarddefinition des [Data Science-Lebenszyklus](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md).
2. Eine standardisierte Projektstruktur für die [Projektdokumentation und die Berichterstellungsvorlagen](https://github.com/Azure/Azure-TDSP-ProjectTemplate)
3. Infrastruktur und Ressourcen für die Projektausführung, z.B. die Compute- und Speicherinfrastruktur und Coderepositorys.
4. [Tools und Hilfsprogramme](https://github.com/Azure/Azure-TDSP-Utilities) für Data Science-Projektaufgaben, z.B. die kollaborative Versionskontrolle und Code Review, die Untersuchung und das Modellieren von Daten und die Arbeitsplanung.

Eine umfassende Erläuterung zu TDSP finden Sie unter [Team Data Science Process overview (Übersicht über Team Data Science-Prozesse)](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/README.md).

## <a name="why-should-you-use-tdsp-structure-and-templates"></a>Warum sollten Sie TDSP-Strukturen und -Vorlagen verwenden?
Die Standardisierung der Struktur, des Lebenszyklus und der Dokumentation von Data Science-Projekten erleichtern die effektive Zusammenarbeit in Data Science-Teams. Das Erstellen von Azure Machine Learning-Projekten mit der TDSP-Vorlage bietet ein Framework für koordinierte Teamarbeit.

Wir haben zuvor ein [GitHub-Repository für die TDSP-Projektstruktur und -Vorlagen](https://github.com/Azure/Azure-TDSP-ProjectTemplate) veröffentlicht, um Sie beim Erreichen dieser Ziele zu unterstützen. Bis jetzt war es jedoch nicht möglich, die TDSP-Struktur und -Vorlagen in einem Data Science-Tool zu instanziieren. Es ist nun möglich, ein Azure Machine Learning-Projekt zu erstellen, das die TDSP-Struktur und die Dokumentationsvorlagen instanziiert. 

## <a name="things-to-note-before-creating-a-new-project"></a>Was Sie *vor* dem Erstellen eines neuen Projekts beachten sollten
Folgendes sollten Sie beachten oder überprüfen, *bevor* Sie ein neues Projekt erstellen:
* [TDSP-Vorlagen](https://aka.ms/tdspamlgithubrepo) für Azure Machine Learning
* Inhalte (außer denen im Ordner „docs“) müssen kleiner als 25 MB sein. Siehe folgenden **Hinweis**.
* Der Ordner „sample\_data“ ist nur für kleine Datendateien (kleiner als 5 MB), mit denen Sie Ihren Code testen oder eine frühen Entwicklung durchführen können.
* Speichern von Dateien wie Office Word, PowerPoint usw. kann die Größe des Ordners „docs“ wesentlich erhöhen. Es wird empfohlen, eine kollaborative Wiki, [SharePoint](https://products.office.com/en-us/sharepoint/collaboration) oder andere kollaborative Ressourcen zu suchen, um solche Dateien zu speichern.
* Lesen Sie [Folgendes](http://aka.ms/aml-largefiles) zum Behandeln von großen Dateien und Ausgaben in Azure Machine Learning.

> [!NOTE]
> Achten Sie darauf, dass die Datei „readme.md“ und alle auf die Dokumentation bezogenen Inhalte (Text, Markdowns, Bilder, andere Dokumentdateien), die nicht während der Ausführung des Projekts verwendet werden, sich im Ordner „docs“ (Kleinbuchstaben) befinden. Dies ist ein spezieller Ordner, der von der Ausführung von Azure Machine Learning ignoriert wird, sodass die Inhalte in diesem Ordner nicht unnötig auf das Computeziel kopiert werden. Die Objekte in diesem Ordner werden außerdem nicht auf die Kapazität der Projektgröße von 25 MB angerechnet. Sie können also beispielsweise große Bilddateien speichern, die Sie in Ihrer Dokumentation benötigen. Diese werden von Git immer noch durch den Ausführungsverlauf nachverfolgt. 

## <a name="instantiating-tdsp-structure-and-templates-from-the-azure-machine-learning-template-gallery"></a>Instanziieren von TDSP-Strukturen und -Vorlagen aus dem Azure Machine Learning-Vorlagenkatalog
Führen Sie folgende Prozeduren zum Erstellen eines neuen Projekts mit der Team Data Science-Prozessstruktur und den Dokumentationsvorlagen aus: 

### <a name="click-on-new-project"></a>Klicken auf „Neues Projekt“
Öffnen Sie Azure Machine Learning. Klicken Sie oben links unter **Projekte** auf **+**, und klicken Sie auf **Neues Projekt**, um ein neues Projekt zu erstellen.

![Neues Projekt](./media/how-to-use-tdsp-in-azure-ml/instantiation-1.png)


### <a name="creating-a-new-tdsp-structured-project"></a>Erstellen eines neuen Projekts mit TDSP-Struktur
Geben Sie die Parameter und Informationen in die entsprechenden Felder ein:

- Projektname
- Projektverzeichnis
- Projektbeschreibung
- Einen leeren Pfad des Git-Repositorys
- Arbeitsbereichname

Geben Sie dann *TDSP* in das Feld **Suche** ein. Wenn **Structure a project with TDSP** (Projekt mit TDSP strukturieren) angezeigt wird, klicken Sie darauf, um diese Vorlage auszuwählen. Klicken Sie dann auf die Schaltfläche **Erstellen**, um Ihr neues Projekt mit der TDSP-Struktur zu erstellen. Wenn Sie während der Erstellung des Projekts ein leeres Git-Repository (im entsprechenden Feld) bereitstellen, wird dieses Repository nach der Projekterstellung mit der Projektstruktur und den Inhalten gefüllt.

![Erstellen eines TDSP-Projekts](./media/how-to-use-tdsp-in-azure-ml/instantiation-2.png)


## <a name="examine-the-tdsp-project-structure"></a>Überprüfen der TDSP-Projektstruktur
Nachdem Ihr neues Projekt erstellt wurde, können Sie die Struktur überprüfen (linker Bereich der Abbildung unten). Diese enthält alle Aspekte der standardisierten Dokumentation für geschäftliche Aspekte, die Phasen des TDSP-Lebenszyklus, den Datenspeicherort, die Definition und die Architektur in dieser Dokumentationsvorlage. Diese Struktur wird mit einigen Änderungen aus der [hier](https://github.com/Azure/Azure-TDSP-ProjectTemplate) veröffentlichten TDSP-Struktur abgeleitet. Beispielsweise wurden einige der Dokumentationsvorlagen in einem Markdown namens [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) zusammengeführt. 

### <a name="project-folder-structure"></a>Ordnerstruktur des Projekts
Die TDSP-Projektvorlage enthält folgende Ordner auf oberster Ebene:
1. **code**: enthält Code
2. **docs**: enthält die erforderliche Dokumentation zum Projekt (z.B. Markdowndateien, verwandte Medien usw.)
3. **sample_data**: enthält **SAMPLE (small)**-Daten, die für die frühe Entwicklung oder das Testen verwendet werden können. In der Regel nicht mehr als einige (5) MB. Nicht für vollständige oder große Datasets.

![Beispieldaten](./media/how-to-use-tdsp-in-azure-ml/instantiation-3.png)


## <a name="using-the-tdsp-structure-and-templates"></a>Verwenden der TDSP-Strukturen und -Vorlagen
Die Struktur und die Vorlagen müssen mit projektspezifischen Informationen aufgefüllt werden. Sie sollten diese mit dem notwendigen Code und den notwendigen Informationen für das Ausführen und Bereitstellen Ihres Projekts auffüllen. Bei der Datei [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) handelt es sich um eine Vorlage, die direkt mit den Informationen geändert werden sollte, die relevant für Ihr Projekt sind. Diese enthält eine Reihe von Fragen, die Sie beim Ausfüllen der Informationen für jede der vier Phasen des [Team Data Science-Prozesslebenszyklus](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md) unterstützen.

Im Folgenden finden Sie ein Beispiel für eine Projektstruktur während oder nach der Ausführung (linker Bereich der Abbildung unten). Dieses stammt aus dem [Beispielprojekt des Team Data Science-Prozesses: Klassifizieren von Einkommen aus den Volkszählungsdaten der USA in Azure Machine Learning](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome).

![Beispielstruktur eines Projekts](./media/how-to-use-tdsp-in-azure-ml/instantiation-4.png)

## <a name="documenting-your-project"></a>Dokumentieren Ihres Projekts
Weitere Informationen zum Dokumentieren Ihres Projekts finden Sie unter [TDSP-Dokumentationsvorlagen](https://github.com/Azure/Azure-TDSP-ProjectTemplate). In der aktuellen TDSP-Dokumentationsvorlage für Azure Machine Learning wird empfohlen, dass Sie alle Informationen in der Datei [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) angeben. Diese Vorlage sollte mit projektspezifischen Informationen ausgefüllt werden. 

Es wird ebenfalls eine weitere Vorlage bereitgestellt, [ProjectLearnings](https://aka.ms/tdspamlgithubrepoprojectlearnings), um Informationen anzugeben, die nicht im primären Projektdokument angegeben werden sollen, die aber dennoch nützlich für das Dokument sind. 

### <a name="example-project-report"></a>Beispielprojektbericht
Ein Beispiel für einen Projektbericht finden Sie [hier](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliveralbe_docs/ProjectReport.md). Dabei handelt es sich um den Projektbericht für das [Beispielprojekt für die Einkommensklassifizierung der USA](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome), in dem dargestellt wird, wie eine TDSP-Vorlage instanziiert und für ein Data Science-Projekt verwendet werden kann.

## <a name="next-steps"></a>Nächste Schritte
Damit Sie besser nachvollziehen können, wie die TDSP-Struktur und die TDSP-Vorlagen in Azure Machine Learning-Projekten verwendet werden können, werden mehrere ausgearbeitete Projektbeispiele in der Dokumentation für Azure Machine Learning bereitgestellt.

- Ein Beispiel für das Erstellen eines TDSP-Projekts in Azure Machine Learning finden Sie unter [Beispielprojekt des Team Data Science-Prozesses: Klassifizieren von Einkommen aus den Volkszählungsdaten der USA in Azure Machine Learning](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome). 
- Ein Beispiel, das Deep Learning in NLP in einem TDSP-instanziierten Projekt in Azure Machine Learning verwendet, finden Sie unter [Erkennung von biomedizinischen Entitäten mithilfe von Natural Language Processing mit Deep Learning](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction).

