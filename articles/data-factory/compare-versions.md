---
title: Vergleich der Azure Data Factory-Versionen 1 und 2 | Microsoft-Dokumentation
description: In diesem Artikel werden Version 1 und Version 2 von Azure Data Factory verglichen.
services: data-factory
documentationcenter: 
author: kromerm
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/20/2017
ms.author: makromer
ms.openlocfilehash: f55348e9974de17c6d7e704e185f1ea9b21ff66e
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
---
# <a name="compare-azure-data-factory-versions-1-and-2"></a>Vergleich der Azure Data Factory-Versionen 1 und 2
In diesem Artikel werden Version 2 (V2) und Version 1 (V1) von Azure Data Factory verglichen. Eine Einführung in V1 des Diensts finden Sie unter [Einführung in Azure Data Factory](v1/data-factory-introduction.md) und für V2 unter [Einführung in Azure Data Factory](introduction.md).

## <a name="feature-comparison"></a>Funktionsvergleich
In der folgenden Tabelle werden die Funktionen von V1 und V2 verglichen. 

| Feature | V1 | V2 | 
| ------- | --------- | --------- | 
| Datasets | Eine benannte Ansicht mit Daten, in der auf die Daten verwiesen wird, die Sie in Ihren Aktivitäten als Ein- und Ausgabe verwenden möchten. Datasets bestimmen Daten in verschiedenen Datenspeichern, z.B. Tabellen, Dateien, Ordnern und Dokumenten. Ein Azure-Blobdataset kann beispielsweise den Blobcontainer und -ordner in Blob Storage angeben, aus dem die Aktivität die Daten lesen soll.<br/><br/>Mit **Verfügbarkeit** wird das Modell für die Aufteilung in Verarbeitungsfenster für das Dataset (z.B. stündlich, täglich usw.) definiert. | Datasets sind in V2 identisch. Sie müssen aber keine **Verfügbarkeits**zeitpläne für die Datasets definieren. Sie können eine Triggerressource definieren, mit der Pipelines über ein Taktplaner-Paradigma geplant werden können. Weitere Informationen finden Sie unter [Trigger](concepts-pipeline-execution-triggers.md#triggers) und [Datasets](concepts-datasets-linked-services.md). | 
| Verknüpfte Dienste | Verknüpfte Dienste ähneln Verbindungszeichenfolgen, mit denen die Verbindungsinformationen definiert werden, die für Data Factory zum Herstellen einer Verbindung mit externen Ressourcen erforderlich sind. | Entspricht Data Factory V1, aber mit einer neuen **connectVia**-Eigenschaft für die Nutzung der Data Factory V2 Integration Runtime-Computeumgebung. Weitere Informationen finden Sie unter [Integrationslaufzeit in Azure Data Factory](concepts-integration-runtime.md) und [Eigenschaften des verknüpften Diensts](connector-azure-blob-storage.md#linked-service-properties). |
| Pipelines | Eine Data Factory kann eine oder mehrere Aktivitäten aufweisen. Bei einer Pipeline handelt es sich um eine logische Gruppierung von Aktivitäten, die zusammen eine Aufgabe bilden. Sie haben startTime, endTime und isPaused verwendet, um Pipelines zu planen und auszuführen. | Pipelines sind weiterhin Gruppen mit Aktivitäten, die für Daten durchgeführt werden. Die Zeitplanung von Aktivitäten in der Pipeline wurde aber in neue Triggerressourcen unterteilt. Sie können sich Pipelines in Data Factory V2 eher als „Workfloweinheiten“ vorstellen, die Sie separat über Trigger planen. <br/><br/>Pipelines verfügen in Data Factory V2 nicht über „Fenster“ für die zeitabhängige Ausführung. Die Konzepte startTime, endTime und isPaused aus Data Factory V1 sind in Data Factory V2 nicht mehr vorhanden. Weitere Informationen finden Sie unter [Pipelineausführung und Trigger in Azure Data Factory](concepts-pipeline-execution-triggers.md) und [Pipelines und Aktivitäten in Azure Data Factory](concepts-pipelines-activities.md). |
| Aktivitäten | Mit Aktivitäten werden Aktionen definiert, die Sie auf Ihre Daten in einer Pipeline anwenden. Aktivitäten für die Datenverschiebung (Kopieraktivität) und Datentransformation (z.B. Hive, Pig und MapReduce) werden unterstützt. | In Data Factory V2 sind Aktivitäten weiterhin definierte Aktionen in einer Pipeline. In V2 werden neue [Aktivitäten für die Ablaufsteuerung](concepts-pipelines-activities.md#control-activities) eingeführt. Sie verwenden diese Aktivitäten bei der Ablaufsteuerung (Schleifen und Verzweigungen). Aktivitäten für die Datenverschiebung und Datentransformation, die in V1 unterstützt wurden, werden auch in V2 unterstützt. Sie können Transformationsaktivitäten definieren, ohne Datasets in V2 zu verwenden. |
| Hybriddatenverschiebung und Aktivitätsverteilung | Wurde bisher als [Datenverwaltungsgateway](v1/data-factory-data-management-gateway.md) bezeichnet und hat das Verschieben von Daten zwischen dem lokalen Standort und der Cloud unterstützt. Wird jetzt als „Integration Runtime“ (Integrationslaufzeit) bezeichnet.| Das Datenverwaltungsgateway wird jetzt als selbstgehostete Integrationslaufzeit bezeichnet. Hierfür sind die gleichen Funktionen wie in V1 verfügbar. <br/><br/> Die Azure-SSIS Integration Runtime in V2 unterstützt auch das Bereitstellung und Ausführen von SSIS-Paketen (SQL Server Integration Services) in der Cloud. Ausführliche Informationen finden Sie im Artikel [Integrationslaufzeit in Azure Data Factory](concepts-integration-runtime.md).|
| Parameter | NA | Parameter sind Schlüssel-Wert-Paare mit schreibgeschützten Konfigurationseinstellungen, die in Pipelines definiert sind. Beim manuellen Ausführen der Pipeline können Sie Argumente für die Parameter übergeben. Wenn Sie einen Planer-Trigger verwenden, kann der Trigger auch Werte für die Parameter übergeben. Die Parameterwerte werden von Aktivitäten in der Pipeline genutzt.  |
| Ausdrücke | In Data Factory V1 können Sie Funktionen und Systemvariablen in Datenauswahlabfragen und Aktivitäts-/Dataseteigenschaften verwenden. | In Data Factory V2 können Sie Ausdrücke in einem JSON-Zeichenfolgenwert beliebig anordnen. Weitere Informationen finden Sie unter [Ausdrücke und Funktionen in Azure Data Factory](control-flow-expression-language-functions.md).|
| Pipelineausführungen | NA | Eine einzelne Instanz einer Pipelineausführung. Beispiel: Angenommen, Sie haben eine Pipeline, die um 8:00, 9:00 und 10:00 Uhr ausgeführt wird. In diesem Fall erfolgen drei separate Ausführungen der Pipeline (Pipelineausführungen). Jede Pipelineausführung weist eine eindeutige Pipelineausführungs-ID aus. Dabei handelt es sich um eine GUID, die die jeweilige Pipelineausführung eindeutig definiert. Zur Instanziierung von Pipelineausführungen werden in der Regel Argumente an in Pipelines definierte Parameter übergeben. |
| Aktivitätsausführungen | NA | Eine Instanz einer Aktivitätsausführung in einer Pipeline. | 
| Triggerausführungen | NA | Eine Instanz einer Triggerausführung. Weitere Informationen finden Sie unter [Trigger](concepts-pipeline-execution-triggers.md). |
| Zeitplanung | Die Zeitplanung basiert auf der Start- und Endzeit der Pipeline und der Datasetverfügbarkeit. | Planer-Trigger oder Ausführung über externen Planer. Weitere Informationen finden Sie unter [Pipelineausführung und -trigger](concepts-pipeline-execution-triggers.md). |

Die folgenden Abschnitte enthalten weitere Informationen zu den Funktionen von Version 2: 

## <a name="control-flow"></a>Ablaufsteuerung  
Zur Unterstützung der Integration von vielfältigen Integrationsabläufen und -mustern eines modernen Data Warehouse bietet Data Factory V2 ein neues flexibles Datenpipelinemodell, das nicht mehr an Zeitreihendaten gebunden ist. Im Folgenden finden Sie eine Auswahl gängiger Abläufe, die zuvor nicht möglich waren:   

### <a name="chaining-activities"></a>Verketten von Aktivitäten
In Version 1 mussten Sie die Ausgabe einer Aktivität als Eingabe einer anderen Aktivität konfigurieren, um sie zu verketten. In V2 können Sie Aktivitäten in einer Sequenz innerhalb einer Pipeline verketten. Sie können die **dependsOn**-Eigenschaft in einer Aktivitätsdefinition verwenden, um eine Verkettung mit einer Upstream-Aktivität herzustellen. Weitere Informationen und ein Beispiel finden Sie in den Artikeln [Pipelines und Aktivitäten in Azure Data Factory](concepts-pipelines-activities.md#multiple-activities-in-a-pipeline) und [Verzweigen und Verketten von Aktivitäten in einer Data Factory-Pipeline](tutorial-control-flow.md). 

### <a name="branching-activities"></a>Verzweigen von Aktivitäten
In V2 können Sie Aktivitäten jetzt in einer Pipeline verzweigen. Die [Aktivität „If Condition“](control-flow-if-condition-activity.md) bietet die gleiche Funktionalität wie eine `if`-Anweisung in Programmiersprachen. Sie wertet eine Aktivitätengruppe aus, wenn die Bedingung als `true` ausgewertet wird, und eine weitere Aktivitätengruppe, wenn die Bedingung als `false` ausgewertet wird. Ein Beispiel für das Verzweigen von Aktivitäten finden Sie im Tutorial [Verzweigen und Verketten von Aktivitäten in einer Data Factory-Pipeline](tutorial-control-flow.md).

### <a name="parameters"></a>Parameter 
Sie können Parameter auf Pipelineebene definieren und Argumente übergeben, während die Pipelineausführung bei Bedarf oder mithilfe eines Triggers gestartet wird. Aktivitäten können die an die Pipeline übergebenen Argumente nutzen. Weitere Informationen finden Sie unter [Pipelineausführung und Trigger in Azure Data Factory](concepts-pipeline-execution-triggers.md). 

### <a name="custom-state-passing"></a>Übergeben von benutzerdefinierten Zuständen
Aktivitätsausgaben (einschließlich des Zustands) können von einer Folgeaktivität in der Pipeline genutzt werden. In der JSON-Definition einer Aktivität können Sie beispielsweise auf die Ausgabe der vorherigen Aktivität zugreifen, indem Sie die folgende Syntax verwenden: `@activity('NameofPreviousActivity').output.value`. Mit diesem Feature können Sie Workflows erstellen, bei denen Werte Aktivitäten durchlaufen können.

### <a name="looping-containers"></a>Schleifencontainer
Mit der [ForEach](control-flow-for-each-activity.md)-Aktivität wird eine wiederholte Ablaufsteuerung in Ihrer Pipeline definiert. Diese Aktivität wird verwendet, um eine Sammlung zu durchlaufen. Sie führt die angegebenen Aktivitäten in einer Schleife aus. Die Schleifenimplementierung dieser Aktivität ähnelt der Foreach-Schleifenstruktur in Programmiersprachen. 

Die [Until](control-flow-until-activity.md)-Aktivität erfüllt die gleiche Funktion wie eine do-until-Schleifenstruktur in Programmiersprachen. Sie führt eine Reihe von Aktivitäten in einer Schleife aus, bis die der Aktivität zugeordnete Bedingung als „true“ ausgewertet wird. In Data Factory können Sie einen Timeoutwert für die Until-Aktivität angeben.  

### <a name="trigger-based-flows"></a>Triggerbasierte Abläufe
Pipelines können nach Bedarf oder zeitgesteuert ausgelöst werden. Der Artikel [Pipelineausführung und Trigger in Azure Data Factory](concepts-pipeline-execution-triggers.md) enthält die ausführlichen Informationen zu Triggern. 

### <a name="invoke-a-pipeline-from-another-pipeline"></a>Aufrufen einer Pipeline aus einer anderen Pipeline
Mit der [Aktivität „Pipeline ausführen“](control-flow-execute-pipeline-activity.md) kann eine Data Factory-Pipeline eine andere Pipeline aufrufen.

### <a name="delta-flows"></a>Deltaabläufe
Ein Anwendungsfall für Schlüssel in ETL-Mustern sind Deltaladevorgänge. Hierbei werden nur die Daten geladen, die sich seit dem letzten Durchlauf einer Pipeline geändert haben. Mit den neuen Funktionen in Version 2, z.B. [Lookup-Aktivität](control-flow-lookup-activity.md), flexible Zeitplanung und Ablaufsteuerung, wird dieser Anwendungsfall auf natürliche Weise ermöglicht. Ein Tutorial mit einer Schritt-für-Schritt-Anleitung finden Sie im Tutorial [Inkrementelles Laden von Daten aus Azure SQL-Datenbank in Azure Blob Storage](tutorial-incremental-copy-powershell.md).

### <a name="other-control-flow-activities"></a>Weitere Aktivitäten der Ablaufsteuerung
Hier sind einige weitere Aktivitäten der Ablaufsteuerung angegeben, die von Data Factory V2 unterstützt werden: 

Steuerungsaktivität | Beschreibung
---------------- | -----------
[Aktivität „ForEach“](control-flow-for-each-activity.md) | Mit der ForEach-Aktivität wird eine wiederholte Ablaufsteuerung in Ihrer Pipeline definiert. Diese Aktivität wird verwendet, um eine Sammlung zu durchlaufen. Sie führt die angegebenen Aktivitäten in einer Schleife aus. Die Schleifenimplementierung dieser Aktivität ähnelt der Foreach-Schleifenstruktur in Programmiersprachen.
[Aktivität „Web“](control-flow-web-activity.md) | Die Web-Aktivität kann verwendet werden, um einen benutzerdefinierten REST-Endpunkt aus einer Data Factory-Pipeline aufzurufen. Sie können Datasets und verknüpfte Dienste zur Verwendung und für den Zugriff durch die Aktivität übergeben. 
[Aktivität „Lookup“](control-flow-lookup-activity.md) | Mit der Lookup-Aktivität können Sie einen Datensatz/Tabellennamen/Wert in einer externen Quelle lesen oder suchen. Auf die Ausgabe kann durch nachfolgende Aktivitäten verwiesen werden. 
[Aktivität „Metadaten abrufen“](control-flow-get-metadata-activity.md) | Die Aktivität „Metadaten abrufen“ kann zum Abrufen von Metadaten für alle Daten in Azure Data Factory verwendet werden. 
[Aktivität „Warten“](control-flow-wait-activity.md) | Die Pipeline wartet (pausiert) so lange, wie angegeben wurde.

## <a name="deploy-ssis-packages-to-azure"></a>Bereitstellen von SSIS-Paketen in Azure 
Wenn Sie Ihre SSIS-Workloads (SQL Server Integration Services) in die Cloud verschieben möchten, müssen Sie eine Data Factory V2 erstellen und eine Instanz der Azure-SSIS Integration Runtime (IR) bereitstellen. Die Azure-SSIS-IR ist ein vollständig verwalteter Cluster mit virtuellen Azure-Computern (Knoten), die speziell für die Ausführung von SSIS-Paketen in der Cloud bestimmt sind. Nachdem Sie eine Azure-SSIS IR-Instanz bereitgestellt haben, können Sie dieselben Tools verwenden, die Sie zum Bereitstellen der SSIS-Pakete in einer lokalen SSIS-Umgebung eingesetzt haben. Sie können beispielsweise SQL Server Data Tools (SSDT) oder SQL Server Management Studio (SSMS) zum Bereitstellen von SSIS-Paketen für diese Runtime in Azure verwenden. Eine Schritt-für-Schritt-Anleitung finden Sie im Tutorial [Bereitstellen von SQL Server Integration Services-Paketen in Azure](tutorial-deploy-ssis-packages-azure.md). 

## <a name="flexible-scheduling"></a>Flexible Zeitplanung
In Data Factory V2 müssen Sie keine Zeitpläne für die Datasetverfügbarkeit definieren. Sie können eine Triggerressource definieren, mit der Pipelines über ein Taktplaner-Paradigma geplant werden können. Sie können auch Parameter über einen Trigger an Pipelines übergeben, um ein flexibles Modell für die Zeitplanung bzw. Ausführung zu erhalten. Pipelines verfügen in Data Factory V2 nicht über „Fenster“ für die zeitabhängige Ausführung. Die Konzepte startTime, endTime und isPaused aus Data Factory V1 sind in Data Factory V2 nicht mehr vorhanden. In diesem Artikel wird beschrieben, wie Sie eine Pipeline in Data Factory V2 erstellen und dann planen: [Pipelineausführung und Trigger in Azure Data Factory](concepts-pipeline-execution-triggers.md).

## <a name="support-for-more-data-stores"></a>Unterstützung für weitere Datenspeicher
V2 unterstützt das Kopieren von Daten für eine größere Zahl von Datenspeichern als in V1. Eine Liste mit den unterstützten Datenspeichern finden Sie in den folgenden Artikeln:

- [V1: Unterstützte Datenspeicher und Formate](v1/data-factory-data-movement-activities.md#supported-data-stores-and-formats)
- [V2: Unterstützte Datenspeicher und Formate](copy-activity-overview.md#supported-data-stores-and-formats)

## <a name="support-for-on-demand-spark-cluster"></a>Unterstützung für einen bedarfsgesteuerten Spark-Cluster
In V2 wird die Erstellung eines bedarfsgesteuerten HDInsight Spark-Clusters unterstützt. Geben Sie zum Erstellen eines bedarfsgesteuerten Spark-Clusters den Clustertyp „Spark“ in der Definition Ihres bedarfsgesteuerten verknüpften HDInsight-Diensts an. Anschließend können Sie die Spark-Aktivität in Ihrer Pipeline konfigurieren, um diesen verknüpften Dienst zu verwenden. Wenn die Aktivität zur Laufzeit ausgeführt wird, erstellt der Data Factory-Dienst den Spark-Cluster für Sie automatisch. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Transformieren von Daten mit der Spark-Aktivität in Azure Data Factory](transform-data-using-spark.md)
- [Bedarfsgesteuerter verknüpfter Azure HDInsight-Dienst](compute-linked-services.md#azure-hdinsight-on-demand-linked-service)

## <a name="custom-activities"></a>Benutzerdefinierte Aktivitäten
In V1 implementieren Sie den Code einer (benutzerdefinierten) DotNet-Aktivität, indem Sie ein .NET-Klassenbibliotheksprojekt mit einer Klasse erstellen, die die Execute-Methode der IDotNetActivity-Schnittstelle implementiert. Aus diesem Grund muss der benutzerdefinierte Code in .NET Framework 4.5.2 geschrieben und auf Windows-basierten Azure Batch-Poolknoten ausgeführt werden. 

In einer benutzerdefinierten Aktivität von V2 muss keine .NET-Schnittstelle implementiert werden. Sie können jetzt direkt Befehle und Skripts ausführen und Ihren eigenen benutzerdefinierten Code als ausführbare Datei ausführen. 

Weitere Informationen finden Sie unter [Unterschied zwischen einer benutzerdefinierten Aktivität in Azure Data Factory V2 und einer (benutzerdefinierten) DotNet-Aktivität in Azure Data Factory V1](transform-data-using-dotnet-custom-activity.md#difference-between-custom-activity-in-azure-data-factory-v2-and-custom-dotnet-activity-in-azure-data-factory-v1).

## <a name="sdks"></a>SDKs
Version 2 von Data Factory verfügt über einen umfassenderen SDK-Satz, der zum Erstellen, Verwalten und Überwachen von Pipelines verwendet werden kann.

- **.NET SDK**: Das .NET SDK wurde für V2 aktualisiert. 
- **PowerShell**: Die PowerShell-Cmdlets werden für V2 aktualisiert. Die Cmdlets von V2 enthalten **DataFactoryV2** im Namen. Beispiel: Get-AzureRmDataFactoryV2. 
- **Python SDK**: Dieses SDK ist neu in V2.
- **REST-API**: Die REST-API wurde für V2 aktualisiert. 

Die für V2 aktualisierten SDKs sind nicht abwärtskompatibel mit Clients der Version 1. 

## <a name="authoring-experience"></a>Benutzeroberfläche für die Erstellung
Mit Data Factory V1 können Sie Pipelines erstellen, indem Sie den Data Factory-Editor im Azure-Portal verwenden. Data Factory V2 unterstützt derzeit nur eine programmgesteuerte Vorgehensweise (.NET SDK, REST-API, PowerShell, Python usw.) zum Erstellen von Data Factorys. Eine Benutzeroberfläche wird noch nicht unterstützt.  Data Factory V1 verfügt über Unterstützung für die Erstellung per SDK, REST und PowerShell.

## <a name="monitoring-experience"></a>Benutzeroberfläche für die Überwachung
In V2 können Sie Data Factorys auch unterstützen, indem Sie [Azure Monitor](monitor-using-azure-monitor.md) verwenden. Die neuen PowerShell-Cmdlets unterstützen die Überwachung von [Integrationslaufzeiten](monitor-integration-runtime.md). Sowohl V1 als auch V2 unterstützen die visuelle Überwachung per Überwachungsanwendung, die über das Azure-Portal gestartet werden kann.


## <a name="next-steps"></a>Nächste Schritte
Ausführliche Informationen zum Erstellen einer Data Factory finden Sie in den folgenden Schnellstartanleitungen: [PowerShell](quickstart-create-data-factory-powershell.md), [.NET](quickstart-create-data-factory-dot-net.md), [Python](quickstart-create-data-factory-python.md), [REST-API](quickstart-create-data-factory-rest-api.md). 
