---
title: Häufig gestellte Fragen (FAQs) zu Machine Learning Studio
titleSuffix: Azure Machine Learning Studio
description: 'Azure Machine Learning Studio: Häufig gestellte Fragen (FAQs) zu Abrechnung, Funktionen und Einschränkungen von Clouddiensten für die optimierte Vorhersagemodellierung.'
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 06/02/2017
ms.openlocfilehash: 8c381ca02dcfb194b2807735415ef894d72f74fa
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462278"
---
# <a name="azure-machine-learning-studio-faq-capabilities-and-limitations"></a>Häufig gestellte Fragen (FAQs) zu Machine Learning Studio: Funktionen und Einschränkungen
Hier sind einige häufig gestellte Fragen (FAQs) und die entsprechenden Antworten zu Azure Machine Learning aufgeführt. Bei Azure Machine Learning handelt es sich um einen Clouddienst zum Entwickeln von Vorhersagemodellen und Operationalisieren von Lösungen mithilfe von Webdiensten. 

## <a name="general-questions"></a>Allgemeine Fragen
**Was ist Machine Learning Studio?**

Machine Learning Studio ist eine Drag & Drop-Canvas-Umgebung, auf die Sie mit einem Webbrowser zugreifen. Machine Learning Studio enthält eine Palette von Modulen in einer visuellen Kompositionsoberfläche, in der Sie durchgängige Datenworkflows in Form von Experimenten erstellen können.

Weitere Informationen zu Machine Learning Studio finden Sie unter [Was ist Machine Learning Studio?](what-is-ml-studio.md)

**Was ist der Machine Learning-API-Dienst?**

Mit dem Machine Learning-API-Dienst können Sie Ihre Vorhersagemodelle, z.B. in Machine Learning Studio erstellte Modelle, als skalierbare, fehlertolerante Webdienste bereitstellen. Die vom Machine Learning-API-Dienst erstellten Webdienste sind REST-APIs. Sie bieten eine Schnittstelle für die Kommunikation zwischen externen Anwendungen und Ihren Predictive Analytics-Modellen.

Weitere Informationen finden Sie unter [Nutzen eines Azure Machine Learning-Webdiensts](consume-web-services.md).

**Wo sind meine klassischen Webdienste aufgeführt? Wo sind meine neuen (Azure Resource Manager-basierten) Webdienste aufgeführt?**

Mit dem klassischen Bereitstellungsmodell und dem neuen Azure Resource Manager-Bereitstellungsmodell erstellte Webdienste sind im Portal [Microsoft Azure Machine Learning-Webdienste](https://services.azureml.net/) aufgeführt.

Klassische Webdienste finden Sie auch in [Machine Learning Studio](http://studio.azureml.net) auf der Registerkarte **Webdienste**.

## <a name="azure-machine-learning-questions"></a>Fragen zu Azure Machine Learning
**Was sind Azure Machine Learning-Webdienste?**

Machine Learning-Webdienste stellen eine Schnittstelle zwischen einer Anwendung und einem Machine Learning-Workflow-Bewertungsmodell dar. Von einer externen Anwendung kann Azure Machine Learning verwendet werden, um in Echtzeit mit einem Machine Learning-Workflow-Bewertungsmodell zu kommunizieren. Mit einem Aufruf eines Machine Learning-Webdiensts werden Vorhersageergebnisse an eine externe Anwendung zurückgegeben. Zur Durchführung eines Webdienstaufrufs übergeben Sie einen API-Schlüssel, der beim Bereitstellen des Webdiensts erstellt wurde. Ein Machine Learning-Webdienst basiert auf REST, einer verbreiteten Architektur für Webprogrammierungsprojekte.

Azure Machine Learning verfügt über zwei Arten von Webdiensten:

* Anforderung-/Antwort-Dienst (Request-Response Service, RRS): Ein hochskalierbarer Webdienst mit kurzer Wartezeit, der eine Schnittstelle für zustandslose Modelle bereitstellt, die mit Machine Learning Studio erstellt und bereitgestellt wurden.
* Batchausführungsdienst (Batch Execution Service, BES): Ein asynchroner Dienst für die Bewertung eines Batches für Datensätze.

Es gibt mehrere Möglichkeiten, die REST-API zu nutzen und auf den Webdienst zuzugreifen. Beispielsweise können Sie eine Anwendung in C#, R oder Python schreiben und dabei den Beispielcode verwenden, der für Sie beim Bereitstellen des Webdiensts generiert wurde.

Den Beispielcode finden Sie hier:
- Seite „Consume“ (Verbrauchen) für den Webdienst im Azure Machine Learning-Webdienste-Portal
- API-Hilfeseite im Webdienstdashboard in Machine Learning Studio

Sie können auch die Microsoft Excel-Beispielarbeitsmappe verwenden, die für Sie erstellt wurde und im Webdienstdashboard in Machine Learning Studio verfügbar ist.

**Was sind die wichtigsten Updates für Azure Machine Learning?**

Die neuesten Updates finden Sie unter [Neuerungen in Azure Machine Learning](../../active-directory/fundamentals/whats-new.md).

## <a name="import-and-export-data-for-machine-learning"></a>Importieren und Exportieren von Daten für Machine Learning
**Welche Datenquellen unterstützt Machine Learning?**

Sie haben drei Möglichkeiten, um Daten in ein Machine Learning Studio-Experiment herunterzuladen:

- Hochladen einer lokalen Datei als Dataset
- Verwenden eines Moduls zum Importieren von Daten aus Clouddatendiensten
- Importieren eines in einem anderen Experiment gespeicherten Datasets

Weitere Informationen zu den unterstützten Dateiformaten finden Sie unter [Importieren von Trainingsdaten in Machine Learning Studio](import-data.md).

### <a id="ModuleLimit"></a>Wie groß können Datasets für meine Module sein?
Module in Machine Learning Studio unterstützen in normalen Anwendungsfällen Datasets bis zu einer Größe von 10 GB an dichten numerischen Daten. Wenn ein Modul mehrere Eingaben akzeptiert, entspricht der Wert von 10 GB der Summe aller Eingabegrößen. Sie können auch Teile größerer Datasets übernehmen, indem Sie Abfragen aus Hive oder Azure SQL-Datenbank verwenden, oder Sie können die Vorverarbeitung per Lernen nach Anzahl vor der Erfassung nutzen.  

Die folgenden Typen von Daten können während der Featurenormalisierung in größere Datasets erweitert werden und sind auf weniger als 10 GB beschränkt:

* Platzsparend
* Kategorisch
* Zeichenfolgen
* Binärdaten

Die folgenden Module sind auf Datasets mit einer Größe von unter 10 GB beschränkt:

* Empfohlene Module
* Modul „Synthetic Minority Oversampling Technique (SMOTE)“
* Skriptingmodule: R, Python, SQL
* Module, bei denen die Größe der Ausgabedaten die der Eingabedaten überschreiten kann, z.B. Join oder Feature Hashing.
* Kreuzvalidierung, Tune Model Hyperparameters, Ordinal Regression und One-vs-All Multiclass, wenn eine sehr große Anzahl von Iterationen durchgeführt wird.

### <a id="UploadLimit"></a>Was sind die Limits für Datenuploads?
Laden Sie die Daten für Datasets, die größer als einige GB sind, in Azure Storage oder Azure SQL-Datenbank hoch, oder verwenden Sie Azure HDInsight, anstatt die Daten direkt aus einer lokalen Datei hochzuladen.

**Können Daten von Amazon S3 gelesen werden?**

Wenn Ihre Daten nicht sehr umfangreich sind und Sie diese Daten über eine HTTP-URL verfügbar machen möchten, können Sie das Modul zum [Importieren von Daten][import-data] verwenden. Größere Datenmengen sollten Sie zunächst in Azure Storage übertragen und anschließend mit dem Modul zum [Importieren von Daten][import-data] in das Experiment übernehmen.
<!--

<SEE CLOUD DS PROCESS>
-->

**Gibt es eine integrierte Bildeingabefunktion?**

Informationen zur Bildeingabefunktion finden Sie in der Referenz unter [Importieren von Bildern][image-reader].

## <a name="modules"></a>Module
**Der gewünschte Algorithmus, die Datenquelle, das Datenformat oder die Datentransformation sind nicht in Azure Machine Learning Studio enthalten. Welche Optionen habe ich?**

Sie können im [Forum für Benutzerfeedback](https://go.microsoft.com/fwlink/?LinkId=404231) nachsehen, welche Funktionswünsche wir momentan verfolgen. Stimmen Sie für den entsprechenden Vorschlag ab, wenn eine von Ihnen gewünschte Funktion bereits angefordert wurde. Falls die gewünschte Funktion nicht vorhanden ist, können Sie eine neue Anfrage erstellen. Sie können den Status Ihrer Anfrage ebenfalls in diesem Forum verfolgen. Wir verfolgen diese Liste regelmäßig und aktualisieren den Verfügbarkeitsstatus von Features häufig. Außerdem können Sie die integrierte Unterstützung von R und Python nutzen, um bei Bedarf benutzerdefinierte Transformationen zu erstellen.

**Kann ich meinen vorhandenen Code in Machine Learning Studio verwenden?**

Ja. Sie können Ihren vorhandenen R- oder Python-Code in Machine Learning Studio importieren, zusammen im gleichen Experiment mit den Lernmodulen in Azure Machine Learning ausführen und die Lösung als Webdienst über Azure Machine Learning bereitstellen. Weitere Informationen finden Sie unter [Erweitern Ihres Experiments mit R](extend-your-experiment-with-r.md) und [Ausführen von Python-Machine Learning-Skripts in Azure Machine Learning Studio](execute-python-scripts.md).

**Ist es möglich, z.B. mit [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) ein Modell zu definieren?**

Nein, die Predictive Model Markup Language (PMML) wird nicht unterstützt. Sie können benutzerdefinierten R- und Python-Code verwenden, um ein Modul zu definieren.

**Wie viele Module kann ich parallel in meinem Experiment ausführen?**  

Sie können bis zu vier Module in einem Experiment parallel ausführen.

## <a name="data-processing"></a>Datenverarbeitung
**Gibt es eine Möglichkeit zum interaktiven Anzeigen von Daten (neben R-Visualisierungen) im Experiment?**

Klicken Sie auf die Ausgabe eines Moduls, um die Daten zu visualisieren und Statistiken abzurufen.

**Bei der Vorschau von Ergebnissen oder Daten in einem Browser ist die Anzahl von Zeilen und Spalten beschränkt. Warum?**

Da unter Umständen große Datenmengen an einen Browser gesendet werden, ist die Datengröße beschränkt, um eine Verlangsamung von Machine Learning Studio zu verhindern. Es ist besser, alle Daten herunterzuladen und Excel oder ein anderes Tool zu verwenden, um die gesamten Daten bzw. Ergebnisse zu visualisieren.

## <a name="algorithms"></a>Algorithmen
**Welche vorhandenen Algorithmen werden in Machine Learning Studio unterstützt?**

Machine Learning Studio unterstützt moderne Algorithmen, z.B. skalierbare Boosted Decision-Strukturen, Bayessche Empfehlungssysteme, tiefe neuronale Netze und die von Microsoft Research entwickelten „Entscheidungsdschungel“. Skalierbare Open Source-Pakete für Machine Learning-Pakete, z.B. Vowpal Wabbit, sind ebenfalls enthalten. Machine Learning Studio unterstützt Algorithmen für Machine Learning für mehrklassige und binäre Klassifizierung, Regression und Clustering. Weitere Informationen finden Sie in der vollständigen Liste der [Machine Learning-Module][machine-learning-modules].

**Wird automatisch der richtige Machine Learning-Algorithmus für meine Daten vorgeschlagen?**

Nein. Es gibt in Machine Learning Studio aber verschiedene Möglichkeiten zum Vergleichen der Ergebnisse jedes Algorithmus, um die richtige Wahl für das jeweilige Problem zu treffen.

**Gibt es Richtlinien zum Auswählen eines Algorithmus für die bereitgestellten Algorithmen?**

Informationen hierzu finden Sie unter [Auswählen eines Algorithmus](algorithm-choice.md).

**Wurden die bereitgestellten Algorithmen in R oder Python geschrieben?**

Diese Algorithmen wurden größtenteils in kompilierten Sprachen geschrieben, um eine bessere Leistung zu erzielen.

**Gibt es nähere Informationen zu den Algorithmen?**

Die Dokumentation enthält Informationen zu den Algorithmen und Parameter für die Feinabstimmung, damit Sie den Algorithmus für Ihre Zwecke optimieren können.  

**Gibt es Unterstützung für das Onlinelernen?**

Nein, derzeit wird nur programmgesteuertes erneutes Trainieren unterstützt.

**Können die Ebenen eines Modells für ein neuronales Netz mit dem integrierten Modul visualisiert werden?**

Nein.

**Kann ich eigene Module in C# oder einer anderen Sprache erstellen?**

Derzeit können Sie nur R verwenden, um neue benutzerdefinierte Module zu erstellen.

## <a name="r-module"></a>R-Modul
**Welche R-Pakete sind in Machine Learning Studio verfügbar?**

Machine Learning Studio unterstützt bereits über 400 CRAN R-Pakete. Hier finden Sie die [aktuelle Liste](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) mit allen enthaltenen Paketen. Lesen Sie auch die Anweisungen zum Abrufen dieser Liste unter [Erweitern des Experiments mit R](extend-your-experiment-with-r.md). Falls das gewünschte Paket nicht in der Liste enthalten ist, können Sie den Namen des Pakets im [Forum für Benutzer-Feedback](https://go.microsoft.com/fwlink/?LinkId=404231) hinterlassen.

**Ist es möglich, benutzerdefinierte R-Module zu erstellen?**

Ja. Weitere Informationen finden Sie unter [Erstellen von benutzerdefinierten R-Modulen in Azure Machine Learning](custom-r-modules.md).

**Gibt es eine REPL-Umgebung für R?**

Nein, in Studio ist keine REPL-Umgebung (Read-Eval-Print-Loop) enthalten.

## <a name="python-module"></a>Python-Modul
**Ist es möglich, benutzerdefinierte Python-Module zu erstellen?**

Derzeit nicht. Sie können aber ein oder mehrere Module vom Typ [Python-Skript ausführen][python] verwenden, um das gleiche Ergebnis zu erzielen.

**Gibt es eine REPL-Umgebung für Python?**

Sie können die „Jupyter Notebooks“ in Machine Learning Studio verwenden. Weitere Informationen finden Sie unter [Introducing Jupyter Notebooks in Azure ML Studio](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx)(Einführung in Jupyter Notebooks in Azure Machine Learning Studio).

## <a name="web-service"></a>Webdienst

**Wie kann ich Azure Machine Learning-Modelle programmgesteuert neu trainieren?**

Verwenden Sie die APIs für das erneute Trainieren. Weitere Informationen finden Sie unter [Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen](retrain-models-programmatically.md). Außerdem ist Beispielcode unter [Microsoft Azure Machine Learning Retraining Demo](https://azuremlretrain.codeplex.com/)(Microsoft Azure Machine Learning – Demo für erneutes Trainieren) verfügbar.

**Kann ich das Modell lokal oder in einer Anwendung ohne Internetverbindung bereitstellen?**

Nein.

**Gibt es eine Grundlatenz, die für alle Webdienste erwartet wird?**

Informationen hierzu finden Sie unter [Einschränkungen für Azure-Abonnements](../../azure-subscription-service-limits.md).

**Wann sollte ich mein Vorhersagemodell als Stapelausführungsdienst oder als Anfrage-Antwort-Dienst ausführen?**

Der Anfrage-Antwort-Dienst (Request Response Service, RRS) ist ein hoch skalierbarer Webdienst mit niedriger Latenz, der eine Schnittstelle für zustandslose Modelle bereitstellt, die in der Experimentumgebung erstellt und bereitgestellt wurden. Der Stapelausführungsdienst (Batch Execution Service, BES) dient zur asynchronen Bewertung eines Stapels von Datensätzen. Die Eingabe für BES gleicht der für RRS verwendeten Dateneingabe. BES liest im Gegensatz zu RRS einen Block von Einträgen aus einer Vielzahl von Quellen, z.B. Azure Blob Storage, Azure Table Storage, Azure SQL-Datenbank, HDInsight (Hive-Abfrage) und HTTP-Quellen. Weitere Informationen finden Sie unter [Nutzen eines Azure Machine Learning-Webdiensts](consume-web-services.md).

**Wie aktualisiere ich das Modell für den bereitgestellten Webdienst?**

Sie können ein Vorhersagemodell für einen bereitgestellten Dienst aktualisieren, indem Sie das Experiment bearbeiten und erneut ausführen, das Sie beim Erstellen und Speichern des trainierten Modells verwendet haben. Nachdem die neue Version des trainierten Modells verfügbar ist, werden Sie von Machine Learning Studio gefragt, ob Sie Ihren Webdienst aktualisieren möchten. Ausführliche Informationen zum Aktualisieren eines bereitgestellten Webdiensts finden Sie unter [Bereitstellen von Machine Learning-Webdiensten](publish-a-machine-learning-web-service.md).

Sie können auch die APIs zum erneuten Trainieren verwenden.
Weitere Informationen finden Sie unter [Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen](retrain-models-programmatically.md). Außerdem ist Beispielcode unter [Microsoft Azure Machine Learning Retraining Demo](https://azuremlretrain.codeplex.com/)(Microsoft Azure Machine Learning – Demo für erneutes Trainieren) verfügbar.

**Wie überwache ich meinen Webdienst in der Produktionsumgebung?**

Nachdem Sie ein Vorhersagemodell bereitgestellt haben, können Sie es über das Portal der Azure Machine Learning-Webdienste überwachen. Jeder bereitgestellte Dienst hat ein eigenes Dashboard mit Überwachungsinformationen für den jeweiligen Dienst. Weitere Informationen dazu, wie Sie bereitgestellte Webdienste verwalten, finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning-Webdienste-Portal](manage-new-webservice.md) und [Verwalten eines Azure Machine Learning-Arbeitsbereichs](manage-workspace.md).

**Kann ich die Ausgabe meines RRS/BES an einer Stelle einsehen?**

Für RRS wird das Ergebnis normalerweise in der Antwort des Webdiensts ausgegeben. Sie können es auch in Azure Blob Storage schreiben. Bei BES wird die Ausgabe standardmäßig in ein Blob geschrieben. Sie können die Ausgabe außerdem mit dem Modul zum [Exportieren von Daten][export-data] in eine Datenbank oder Tabelle schreiben.

**Können Webdienste nur aus Modellen erstellt werden, die in Machine Learning Studio erstellt wurden?**

Nein. Webdienste können auch direkt mit Jupyter Notebooks und RStudio erstellt werden.

**Wo finde ich Informationen zu Fehlercodes?**

Eine Liste mit Fehlercodes und den dazugehörigen Beschreibungen finden Sie unter [Machine Learning-Modul-Fehlercodes](https://msdn.microsoft.com/library/azure/dn905910.aspx) .

**Wie skalierbar ist der Webdienst?**

Derzeit wird der Standardendpunkt mit 20 gleichzeitigen RRS-Anforderungen pro Endpunkt bereitgestellt. Sie können dies auf 200 gleichzeitige Anforderungen pro Endpunkt skalieren, und jeder Webdienst kann auf 10.000 Endpunkte pro Webdienst skaliert werden, wie unter [Skalieren eines Webdiensts](scaling-webservice.md) beschrieben. Bei BES kann jeder Endpunkt 40 Anforderungen gleichzeitig verarbeiten, und weitere Anforderungen werden in eine Warteschlange eingereiht. Diese Anforderungen in der Warteschlange werden automatisch ausgeführt, wenn die Warteschlange geleert wird.

**Werden R-Aufträge auf die Knoten verteilt?**

Nein.  

**Wie viele Daten kann ich für das Training verwenden?**

Module in Machine Learning Studio unterstützen in normalen Anwendungsfällen Datasets bis zu einer Größe von 10 GB an dichten numerischen Daten. Wenn für ein Modul mehr als eine Eingabe verwendet wird, beträgt die Gesamtgröße für alle Eingaben 10 GB. Sie können über Hive- oder Azure SQL-Datenbankabfragen oder per Vorverarbeitung durch Module vom Typ [Lernen durch Anzahl][counts] auch Teile größerer Datasets übernehmen.  

Die folgenden Typen von Daten können während der Featurenormalisierung in größere Datasets erweitert werden und sind auf weniger als 10 GB beschränkt:

* Platzsparend
* Kategorisch
* Zeichenfolgen
* Binärdaten

Die folgenden Module sind auf Datasets mit einer Größe von unter 10 GB beschränkt:

* Empfohlene Module
* Modul „Synthetic Minority Oversampling Technique (SMOTE)“
* Skriptingmodule: R, Python, SQL
* Module, bei denen die Größe der Ausgabedaten die der Eingabedaten überschreiten kann, z.B. Join oder Feature Hashing.
* Cross-Validate, Tune Model Hyperparameters, Ordinal Regression und One-vs-All Multiclass, wenn eine sehr große Anzahl von Iterationen durchgeführt wird.

Laden Sie die Daten für Datasets, die größer als einige GB sind, in Azure Storage oder Azure SQL-Datenbank hoch, oder verwenden Sie HDInsight, anstatt die Daten direkt aus einer lokalen Datei hochzuladen.

**Gibt es Vektorgrößenbeschränkungen?**

Zeilen und Spalten sind jeweils auf die .NET-Einschränkung für Ganzzahlen beschränkt: 2.147.483.647.

**Kann ich die Größe des virtuellen Computers anpassen, auf dem der Webdienst ausgeführt wird?**

Nein.  

## <a name="security-and-availability"></a>Sicherheit und Verfügbarkeit
**Wer hat standardmäßig Zugriff auf den HTTP-Endpunkt für den Webdienst? Wie kann ich den Zugriff auf den Endpunkt einschränken?**

Nach der Bereitstellung eines Webdiensts wird ein Standardendpunkt für den Dienst erstellt. Der Standardendpunkt kann über seinen API-Schlüssel aufgerufen werden. Sie können zusätzliche Endpunkte mit eigenen Schlüsseln über das Webdiensteportal oder programmgesteuert mithilfe der APIs der Webdienstverwaltung hinzufügen. Zugriffsschlüssel sind erforderlich, um Aufrufe für den Webdienst durchzuführen. Weitere Informationen finden Sie unter [Nutzen eines Azure Machine Learning-Webdiensts](consume-web-services.md).

**Was passiert, wenn mein Azure-Speicherkonto nicht gefunden werden kann?**

In Machine Learning Studio ist ein vom Benutzer bereitgestelltes Azure-Speicherkonto zum Speichern von temporären Daten beim Ausführen des Workflows erforderlich. Dieses Speicherkonto wird für Machine Learning Studio beim Erstellen eines Arbeitsbereichs zur Verfügung gestellt. Wenn das Speicherkonto nach dem Erstellen des Arbeitsbereichs gelöscht wird, funktioniert der Arbeitsbereich nicht mehr, und alle darin enthaltenen Experimente schlagen fehl.

Wenn Sie das Speicherkonto versehentlich löschen, können Sie es mit identischem Namen und in derselben Region neu erstellen. Führen Sie anschließend die erneute Synchronisierung des Zugriffsschlüssels durch.

**Was geschieht, wenn mein Speicherkonto-Zugriffsschlüssel nicht mehr synchronisiert ist?**

In Machine Learning Studio ist ein vom Benutzer bereitgestelltes Azure-Speicherkonto zum Speichern von temporären Daten beim Ausführen des Workflows erforderlich. Dieses Speicherkonto wird für Machine Learning Studio beim Erstellen des Arbeitsbereichs zur Verfügung gestellt, und die Zugriffsschlüssel werden diesem Arbeitsbereich zugewiesen. Wenn Zugriffsschlüssel nach dem Erstellen des Arbeitsbereichs geändert werden, kann dieser Arbeitsbereich nicht mehr auf das Speicherkonto zugreifen. Er funktioniert nicht mehr, und alle in diesem Arbeitsbereich enthaltenen Experimente schlagen fehl.

Wenn Sie Zugriffsschlüssel für Speicherkonten geändert haben, müssen Sie die Zugriffsschlüssel im Arbeitsbereich über das Azure-Portal neu synchronisieren.  


## <a name="billing-questions"></a>Fragen zur Abrechnung

Weitere Informationen zur Abrechnung und Preisen finden Sie unter [Machine Learning-Preise](https://azure.microsoft.com/pricing/details/machine-learning/).


<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx
