---
title: Strukturieren von Projekten mit Team Data Science-Prozessvorlagen | Microsoft-Dokumentation
description: "Informationen zum Instanziieren der Team Data Science-Prozessvorlagen (TDSP) in Azure Machine Learning, die Projekte für die Zusammenarbeit strukturieren"
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
ms.date: 10/16/2017
ms.author: bradsev
ms.openlocfilehash: efb681b85d3d7434e3114b8576abc64d00891f03
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2017
---
# <a name="structure-projects-with-the-team-data-science-process-template"></a>Strukturieren von Projekten mit Team Data Science-Prozessvorlagen

Dieses Dokument enthält Anleitungen zum Erstellen von Data Science-Projekten in Azure Machine Learning mit Team Data Science-Prozessvorlagen (TDSP). Diese Vorlagen helfen, Projekte für Zusammenarbeit und Reproduzierbarkeit zu strukturieren. 


## <a name="what-is-the-team-data-science-process"></a>Was ist der Team Data Science-Prozess (TDSP)?
Beim TDSP handelt es sich um einen agilen, iterativen Data Science-Prozess zum Ausführen und Bereitstellen erweiterter Analysemethoden. Der Prozess dient zum Verbessern der Zusammenarbeit und Effizienz von Data Science-Teams in Unternehmen. Es werden diese Ziele mit vier Hauptkomponenten unterstützt:

   * Eine Standarddefinition des [Data Science-Lebenszyklus](../team-data-science-process/lifecycle.md).
   * Standardisierte Projektstruktur, [Projektdokumentation und Berichterstellungsvorlagen](https://github.com/Azure/Azure-TDSP-ProjectTemplate).
   * Infrastruktur und Ressourcen für die Projektausführung, z.B. jeweils die Compute- und Speicherinfrastruktur und Coderepositorys.
   * [Tools und Hilfsprogramme](https://github.com/Azure/Azure-TDSP-Utilities) für Data Science-Projektaufgaben, wie z.B.:
      - Gemeinsame Versionskontrolle
      - Code Review
      - Durchsuchen und Modellieren von Daten
      - Arbeitsplanung

Eine umfassende Erläuterung zu TDSP finden Sie unter [Team Data Science Process overview (Übersicht über Team Data Science-Prozesse)](../team-data-science-process/overview.md).

## <a name="why-should-you-use-the-tdsp-structure-and-templates"></a>Warum sollten Sie die TDSP-Struktur und -Vorlagen verwenden?
Die Standardisierung der Struktur, des Lebenszyklus und der Dokumentation von Data Science-Projekten erleichtern die effektive Zusammenarbeit in Data Science-Teams. Das Erstellen von Machine Learning-Projekten mit der TDSP-Vorlage, um ein Framework für koordinierte Teamarbeit zu bieten.

Wir haben zuvor ein [GitHub-Repository für die TDSP-Projektstruktur und -Vorlagen](https://github.com/Azure/Azure-TDSP-ProjectTemplate) veröffentlicht, um Sie beim Erreichen dieser Ziele zu unterstützen. Bis jetzt war es jedoch nicht möglich, die TDSP-Struktur und -Vorlagen in einem Data Science-Tool zu instanziieren. Es ist nun möglich, ein Machine Learning-Projekt zu erstellen, das die TDSP-Struktur und die Dokumentationsvorlagen instanziiert. 

## <a name="things-to-note-before-creating-a-new-project"></a>Was Sie vor dem Erstellen eines neuen Projekts beachten sollten
Überprüfen Sie die folgenden Kriterien, *bevor* Sie ein neues Projekt erstellen:
* Überprüfen Sie die [TDSP Machine Learning](https://aka.ms/tdspamlgithubrepo)-Vorlage.
* Inhalte (außer denen, die im Ordner „docs“ bereits vorhanden sind) müssen kleiner als 25 MB sein. Siehe den Hinweis, der auf diese Liste folgt.
* Der Ordner „sample\_data“ ist nur für kleine Datendateien (weniger als 5 MB) gedacht, mit denen Sie Ihren Code testen oder eine frühen Entwicklung starten können.
* Durch Speichern von Dateien wie Word- und PowerPoint-Dateien kann sich die Größe des Ordners „docs“ wesentlich erhöhen. Es wird empfohlen, eine auf Zusammenarbeit ausgelegte Ressource wie ein Wiki, [SharePoint](https://products.office.com/en-us/sharepoint/collaboration) o.ä. zu bestimmen, um solche Dateien zu speichern.
* Informationen zum Umgang mit großen Dateien und Ausgaben in Machine Learning finden Sie unter [Beibehalten von Änderungen und Arbeiten mit großen Dateien](http://aka.ms/aml-largefiles).

> [!NOTE]
> Alle auf die Dokumentation bezogenen Inhalte (Text, Markdowns, Bilder, andere Dokumentdateien), die *nicht* während der Ausführung des Projekts verwendet werden, müssen sich im Ordner „docs“ (Kleinbuchstaben) befinden. Der Ordner „docs“ ist ein spezieller Ordner, der von der Machine Learning-Ausführung ignoriert wird, sodass die Inhalte in diesem Ordner nicht unnötig auf das Computeziel kopiert werden. Objekte in diesem Ordner werden auch nicht auf die 25-MB-Obergrenze für die Projektgröße angerechnet. Der Ordner „docs“ ist z.B. der Speicherort für große Bilddateien, die Sie in Ihrer Dokumentation benötigen. Diese Dateien werden von Git weiterhin im Ausführungsverlauf nachverfolgt. 

## <a name="instantiate-the-tdsp-structure-and-templates-from-the-machine-learning-template-gallery"></a>Instanziieren der TDSP-Struktur und -Vorlagen aus dem Machine Learning-Vorlagenkatalog
Führen Sie folgende Schritte zum Erstellen eines neuen Projekts mit der TDSP-Struktur und den Dokumentationsvorlagen aus.

### <a name="create-a-new-project"></a>Erstellen eines neuen Projekts
Öffnen Sie Azure Machine Learning, um ein neues Projekt zu erstellen. Klicken Sie links oben unter **Projects** auf das Pluszeichen (**+**) und dann auf **New Project**.

![Neues Projekt](./media/how-to-use-tdsp-in-azure-ml/instantiation-1.png)


### <a name="create-a-new-tdsp-structured-project"></a>Erstellen eines neuen Projekts mit TDSP-Struktur
   1. Geben Sie die Parameter und Informationen in das entsprechend Feld oder die Liste ein:

      - Projektname
      - Projektverzeichnis
      - Projektbeschreibung
      - Einen leeren Pfad des Git-Repositorys
      - Arbeitsbereichname

   2. Geben Sie **TDSP** in das Feld **Search** ein. 
   3. Wenn die Option **Structure a project with TDSP** angezeigt wird, klicken Sie darauf, um diese Vorlage auszuwählen. 
   4. Klicken Sie dann auf die Schaltfläche **Create**, um Ihr neues Projekt mit einer TDSP-Struktur zu erstellen. Wenn Sie während der Erstellung des Projekts ein leeres Git-Repository (im entsprechenden Textfeld) angeben, wird dieses Repository nach der Projekterstellung mit der Projektstruktur und den Inhalten gefüllt.

![Erstellen eines TDSP-Projekts](./media/how-to-use-tdsp-in-azure-ml/instantiation-2.png)


## <a name="examine-the-tdsp-project-structure"></a>Überprüfen der TDSP-Projektstruktur
Nachdem Ihr neues Projekt erstellt worden ist, können Sie die Struktur überprüfen (siehe den linken Bereich der nachstehenden Abbildung). Sie enthält alle Aspekte einer standardisierten Dokumentation zum Verstehen geschäftlicher Aspekte. Dazu gehören die Phasen des TDSP-Lebenszyklus, der Datenspeicherort, Definitionen und die Architektur dieser Dokumentationsvorlage. 

Die dargestellte Struktur wird von der TDSP-Struktur abgeleitet, die in der [TDSP-Projektstruktur, den Dokumenten und Artefaktvorlagen](https://github.com/Azure/Azure-TDSP-ProjectTemplate), bei diversen Modifikationen, veröffentlicht wird. Beispielsweise wurden einige der Dokumentationsvorlagen in einem Markdown namens [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) zusammengeführt. 

### <a name="project-folder-structure"></a>Ordnerstruktur des Projekts
Die TDSP-Projektvorlage enthält die folgenden Ordner auf oberster Ebene:
   - **code**: Enthält Code.
   - **docs**: Enthält die erforderliche Dokumentation zum Projekt (z.B. Markdowndateien und zugehörige Medien).
   - **sample_data**: Enthält **SAMPLE (small)**-Daten, die für die anfängliche Entwicklung oder Tests verwendet werden können. Typischerweise sind diese Datasets nicht größer als max. 5 MB. Dieser Ordner ist nicht für vollständige oder große Datasets geeignet.

![Beispieldaten](./media/how-to-use-tdsp-in-azure-ml/instantiation-3.png)


## <a name="use-the-tdsp-structure-and-templates"></a>Verwenden der TDSP-Struktur und -Vorlagen
Sie müssen die Struktur und Vorlagen mit projektspezifischen Informationen ergänzen. Von Ihnen wird erwartet, dass Sie diese mit dem Code und den für Ausführung und Lieferung Ihres Projekts erforderlichen Informationen füllen. Bei der Datei [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) handelt es sich um eine Vorlage, die mit Informationen geändert werden muss, die relevant für Ihr Projekt sind. Diese enthält eine Reihe von Fragen, die Sie beim Ausfüllen der Informationen für jede der vier Phasen des [TDSP-Lebenszyklus](../team-data-science-process/lifecycle.md) unterstützen.

Ein Beispiel, wie eine Projektstruktur während der Ausführung bzw. nach Fertigstellung eines Projektes aussieht, ist in der folgenden Abbildung im linken Bereich dargestellt. Dieses Projekt stammt aus dem Beispielprojekt des [Team Data Science-Prozesses: Klassifizieren von Einkommen aus den Volkszählungsdaten der USA in Azure Machine Learning](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome).

![Beispielstruktur eines Projekts](./media/how-to-use-tdsp-in-azure-ml/instantiation-4.png)

## <a name="document-your-project"></a>Dokumentieren Ihres Projekts
Weitere Informationen zum Dokumentieren Ihres Projekts finden Sie unter [TDSP-Dokumentationsvorlagen](https://github.com/Azure/Azure-TDSP-ProjectTemplate). In der aktuellen TDSP-Dokumentationsvorlage für Machine Learning wird empfohlen, dass Sie alle Informationen in der Datei [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) angeben. Diese Vorlage sollte mit projektspezifischen Informationen ausgefüllt werden. 

Wir bieten auch die Vorlage [ProjectLearnings](https://aka.ms/tdspamlgithubrepoprojectlearnings). Mithilfe dieser Vorlage können Sie Informationen angeben, die nicht im primären Projektdokument angegeben werden sollen, aber dennoch nützlich für das Dokument sind. 

### <a name="example-project-report"></a>Beispielprojektbericht
Sie können einen [Beispielprojektbericht](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md) abrufen. Der Projektbericht für das [Beispielprojekt für die Einkommensklassifizierung der USA](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) zeigt, wie die TDSP-Vorlage instanziiert und für ein Data Science-Projekt verwendet werden kann.

## <a name="next-steps"></a>Nächste Schritte
Damit Sie besser nachvollziehen können, wie die TDSP-Struktur und die TDSP-Vorlagen in Machine Learning-Projekten verwendet werden können, werden mehrere vervollständigte Projektbeispiele in der Dokumentation für Machine Learning bereitgestellt:

- Ein Beispiel für das Erstellen eines TDSP-Projekts in Azure Machine Learning finden Sie unter [Beispielprojekt des Team Data Science-Prozesses: Klassifizieren von Einkommen aus den Volkszählungsdaten der USA in Azure Machine Learning](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome).
- Ein Beispiel, das Deep Learning in Natural Language Processing (NLP, Verarbeitung natürlicher Sprache) in einem gemäß dem TDSP instanziierten Projekt in Machine Learning verwendet, finden Sie unter [Erkennung von biomedizinischen Entitäten mithilfe von Natural Language Processing mit Deep Learning](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction).

