---
title: "Azure Data Factory – Häufig gestellte Fragen | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Azure Data Factory"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2017
ms.author: shlo
ms.openlocfilehash: 95b088d2fdc331dc33e973172d32892693fcb648
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-data-factory---frequently-asked-questions"></a>Azure Data Factory – Häufig gestellte Fragen
Dieser Artikel bezieht sich auf Version 2 des Azure Data Factory-Diensts. Er enthält eine Liste der häufig gestellten Fragen (FAQ) und die zugehörigen Antworten.  

## <a name="what-is-azure-data-factory"></a>Was ist Azure Data Factory? 
Data Factory ist ein vollständig verwalteter, cloudbasierter Datenintegrationsdienst, der das Verschieben und Transformieren von Daten automatisiert. Genau wie ein Betrieb, in dem Anlagen Rohmaterialien verarbeiten und in Endprodukte umwandeln, organisiert Azure Data Factory vorhandene Dienste so, dass Rohdaten gesammelt und in nutzbare Informationen transformiert werden. Azure Data Factory ermöglicht das Erstellen von datengesteuerten Workflows zum Verschieben von Daten zwischen lokalen und cloudbasierten Datenspeichern sowie zum Verarbeiten/Transformieren von Daten mithilfe von Computediensten wie Azure HDInsight, Azure Data Lake Analytics und SQL Server Integration Services (SSIS) Integration Runtime. Data Factory bietet Ihnen eine Option zur Datenverarbeitung in einem Azure-basierten Clouddienst oder die Nutzung Ihrer eigenen selbstgehosteten Compute-Umgebung wie SSIS, SQL Server und Oracle. Nachdem Sie eine Pipeline erstellt haben, die die gewünschte Aktion ausführt, können Sie die regelmäßige Ausführung der Pipeline planen (stündlich, täglich, wöchentlich usw.) oder die Pipeline bei einem Ereignisvorkommen auslösen. Weitere Informationen finden Sie unter [Einführung in Azure Data Factory](introduction.md).

## <a name="whats-different-in-version-2"></a>Was ist in Version 2 anders?
Version 2 von Azure Data Factory basiert auf dem ursprünglichen Azure Data Factory-Dienst für die Datenverschiebung und -transformierung, kann nun aber für ein breiteres Spektrum cloudgeprägter Datenintegrationsszenarien verwendet werden. In Version 2 von Azure Data Factory erwarten Sie folgende Funktionen:

- Ablaufsteuerung und Skalierung
- Bereitstellen und Ausführen von SSIS-Paketen in Azure

Nach der Veröffentlichung von Version 1 haben wir erkannt, dass Kunden komplexe Hybrid-Datenintegrationsszenarien mit Datenverschiebung und -verarbeitung in der Cloud, lokal und auf virtuellen Cloudcomputern benötigen. Zur Erfüllung dieser Anforderungen musste eine Möglichkeit zum Übertragen und Verarbeiten von Daten in geschützten VNet-Umgebungen sowie zum horizontalen Hochskalieren mit bedarfsgerechter Rechenleistung geschaffen werden.

Datenpipelines entwickeln sich mehr und mehr zu einem elementaren Bestandteil von Business Analytics-Strategien, und wir haben beobachtet, dass diese zentralen Datenaktivitäten eine flexible Planung erfordern, um inkrementelle Datenlasten und ereignisgesteuerte Ausführungen zu unterstützen. Mit zunehmender Komplexität dieser Orchestrierungen wird es außerdem immer wichtiger, dass der Dienst gängige Workflowparadigmen wie Verzweigungen, Schleifen und bedingte Verarbeitung unterstützt.

Mit Version 2 können Sie auch bereits vorhandene SSIS-Pakete (SQL Server Integration Services) in die Cloud migrieren und so SSIS in einen Azure-Dienst verwandeln, der innerhalb der Anwendungsdefinitionsdatei mithilfe eines neuen Features namens „Integration Runtime“ (IR) verwaltet wird. Mit einer SSIS-IR können Sie in Version 2 SSIS-Pakete in der Cloud ausführen, verwalten, überwachen und erstellen.

### <a name="control-flow-and-scale"></a>Ablaufsteuerung und Skalierung 
Zur Unterstützung der Integration der vielfältigen Integrationsabläufe und -muster eines modernen Data Warehouse bietet Data Factory ein neues flexibles Datenpipelinemodell, das nicht mehr an Zeitreihendaten gebunden ist. In dieser Version können Sie konditionelle Abschnitte und Verzweigungen in der Ablaufsteuerung einer Datenpipeline modellieren sowie explizit Parameter für diese Abläufe intern und übergreifend übergeben.

Sie können nun flexibel einen beliebigen Ablaufstil modellieren, der für die Datenintegration benötigt wird und entweder nach Bedarf oder nach einem bestimmten Zeitplan angewendet werden soll. Im Anschluss finden Sie eine Auswahl gängiger Abläufe, die zuvor nicht möglich waren:   

- Ablaufsteuerung:
    - Verketten von Aktivitäten in einer Sequenz innerhalb einer Pipeline
    - Verzweigen von Aktivitäten innerhalb einer Pipeline
    - Parameter
        - Parameter können auf der Pipelineebene definiert und Argumente beim bedarfs- oder triggergesteuerten Aufrufen der Pipeline übergeben werden.
        - Aktivitäten können die an die Pipeline übergebenen Argumente nutzen.
    - Übergeben von benutzerdefinierten Zuständen
        - Aktivitätsausgaben (einschließlich des Zustands) können von einer Folgeaktivität in der Pipeline genutzt werden.
    - Schleifencontainer
        - ForEach 
- Triggerbasierte Abläufe
    - Pipelines können nach Bedarf oder zeitgesteuert ausgelöst werden.
- Deltaabläufe
    - Verwenden Sie Parameter, und definieren Sie die Obergrenze für Deltakopien beim Verschieben von Dimensions- oder Referenztabellen aus einem relationalen Speicher (entweder lokal oder in der Cloud), um die Daten in den Lake zu laden. 

Weitere Informationen finden Sie im [Tutorial zur Ablaufsteuerung](tutorial-control-flow.md).

### <a name="deploy-ssis-packages-to-azure"></a>Bereitstellen von SSIS-Paketen in Azure 
Wenn Sie Ihre SSIS-Workloads verschieben möchten, können Sie eine Data Factory der Version 2 erstellen und eine Azure-SSIS-IR (Integration Runtime) bereitstellen. Die Azure-SSIS-IR ist ein vollständig verwalteter Cluster mit virtuellen Azure-Computern (Knoten), die speziell für die Ausführung von SSIS-Paketen in der Cloud bestimmt sind. Eine ausführliche Anleitung finden Sie im [Tutorial zum Bereitstellen von SSIS-Paketen in Azure](tutorial-deploy-ssis-packages-azure.md). 
 

### <a name="sdks"></a>SDKs
Für erfahrene Benutzer, die auf der Suche nach einer befehlsorientierten Benutzerschnittstelle sind, bietet Version 2 ein breites Spektrum an SDKs zum Erstellen, Verwalten und Überwachen von Pipelines mit Ihrer bevorzugten IDE.

- .NET SDK – Das .NET SDK wird für Version 2 aktualisiert. 
- PowerShell – Die PowerShell-Cmdlets werden für Version 2 aktualisiert. Die Cmdlets der Version 2 enthalten **DataFactoryV2** im Namen. Beispiel: Get-AzureRmDataFactoryV2. 
- Python SDK – Dieses SDK ist neu in der Version 2.
- REST-API – Die REST-API wird für Version 2 aktualisiert.  

Die für Version 2 aktualisierten SDKs sind nicht abwärtskompatibel mit Clients der Version 1. 

### <a name="monitoring"></a>Überwachung
Version 2 unterstützt derzeit nur das Überwachen von Data Factorys mithilfe von SDKs. Das Portal verfügt noch nicht über die Unterstützung zur Überwachung von Data Factorys der Version 2. 

## <a name="what-is-integration-runtime"></a>Was ist die Integrationslaufzeit?
Bei der Integrationslaufzeit (Integration Runtime, IR) handelt es sich um die Computeinfrastruktur, mit der Azure Data Factory die folgenden Datenintegrationsfunktionen für verschiedene Netzwerkumgebungen bereitstellt:

- **Datenverschiebung**: Dient zum Verschieben von Daten zwischen Datenspeichern im öffentlichen Netzwerk und Datenspeichern im privaten Netzwerk (lokales oder virtuelles privates Netzwerk). Es wird Unterstützung für integrierte Connectors, Formatkonvertierung, Spaltenzuordnung und eine leistungsstarke und skalierbare Datenübertragung bereitgestellt.
- **Verteilungsaktivitäten**: Dienen zum Verteilen und Überwachen von Transformationsaktivitäten, die in vielen verschiedenen Computediensten wie Azure HDInsight, Azure Machine Learning, Azure SQL-Datenbank, SQL Server und vielen weiteren ausgeführt werden.
- **SSIS-Pakete ausführen**: Dient zum nativen Ausführen von SSIS-Paketen (SQL Server Integration Services) in einer verwalteten Azure-Compute-Umgebung.

Sie können zum Verschieben und Transformieren von Daten je nach Bedarf eine oder mehrere Instanzen der Integrationslaufzeit bereitstellen.  Die Integrationslaufzeit kann in einem öffentlichen Azure-Netzwerk oder einem privaten Netzwerk (lokal, Azure Virtual Network oder die virtuelle Private Cloud (VPC) der Amazon Web Services) ausgeführt werden. 

Weitere Informationen finden Sie unter [Integrationslaufzeit in Azure Data Factory](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Welcher Grenzwert besteht hinsichtlich der Anzahl von Integrationslaufzeiten?
Es gibt keine festen Grenzwerte hinsichtlich der Anzahl von Instanzen für die Integrationslaufzeit, über die Sie in einer Data Factory verfügen können. Es gibt jedoch einen Grenzwerte hinsichtlich der Anzahl der Kerne der virtuellen Computer (VM), die von der Integrationslaufzeit pro Abonnement für die Ausführung von SSIS-Paketen verwendet werden können. Weitere Informationen finden Sie unter [Data Factory-Grenzwerte](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="when-to-use-version-2-rather-than-version-1"></a>Wann Version 2 anstelle von Version 1 verwendet werden sollte: 
Wenn Sie mit Azure Data Factory nicht vertraut sind, starten Sie direkt mit Version 2. Wenn Sie bereits Version 1 verwenden, erstellen Sie Ihre Data Factorys für Version 2 neu.

> [!WARNING]
> Version 2 der Data Factory befindet sich in der Vorschauversion und ist nicht allgemein verfügbar (General Availability, GA). Aus diesem Grund unterliegt sie nicht denselben Verpflichtungen der Vereinbarung zum Servicelevel (SLA) für Azure wie Version 1 der Data Factory, die allgemein verfügbar ist (GA). 

## <a name="what-are-the-top-level-concepts-of-version-2"></a>Wie lauten die allgemeinen Konzepte der Version 2?
Ein Azure-Abonnement kann über mindestens eine Azure Data Factory-Instanz (oder Data Factory) verfügen. Azure Data Factory besteht aus vier Hauptkomponenten, die zusammen die Plattform bereitstellen, auf der Sie datengesteuerte Workflows mit Schritten zum Verschieben und Transformieren von Daten zusammenstellen können.

### <a name="pipeline"></a>Pipeline
Eine Data Factory kann eine oder mehrere Pipelines aufweisen. Bei einer Pipeline handelt es sich um eine logische Gruppierung von Aktivitäten zur Durchführung einer Arbeitseinheit. Gemeinsam führen die Aktivitäten einer Pipeline eine Aufgabe durch. Eine Pipeline kann beispielsweise eine Gruppe mit Aktivitäten enthalten, die Daten aus einem Azure-Blob erfasst, und anschließend eine Hive-Abfrage in einem HDInsight-Cluster ausführen, um die Daten zu partitionieren. Der Vorteil hierbei ist, dass die Pipeline Ihnen das Verwalten der Aktivitäten als Gruppe ermöglicht – anstatt für jede Aktivität einzeln. Die Aktivitäten in einer Pipeline können miteinander verkettet und sequenziell durchgeführt oder aber unabhängig voneinander parallel durchgeführt werden.

### <a name="activity"></a>Aktivität
Aktivitäten stellen einen Verarbeitungsschritt in einer Pipeline dar. Beispielsweise können Sie eine Kopieraktivität verwenden, um Daten zwischen zwei Datenspeichern zu kopieren. Analog dazu können Sie eine Hive-Aktivität verwenden, die eine Hive-Abfrage für einen Azure-HDInsight-Cluster ausführt, um Ihre Daten zu transformieren oder zu analysieren. Data Factory unterstützt drei Arten von Aktivitäten: Datenverschiebungsaktivitäten, Datentransformationsaktivitäten und Steuerungsaktivitäten.

### <a name="datasets"></a>DATASETS
Datasets stellen Datenstrukturen in den Datenspeichern dar, die einfach auf die Daten zeigen bzw. verweisen, die Sie in Ihren Aktivitäten als Eingaben oder Ausgaben verwenden möchten. 

### <a name="linked-services"></a>Verknüpfte Dienste
Verknüpfte Dienste ähneln Verbindungszeichenfolgen, mit denen die Verbindungsinformationen definiert werden, die für Data Factory zum Herstellen einer Verbindung mit externen Ressourcen erforderlich sind. Sie können sich dies wie folgt vorstellen: Ein verknüpfter Dienst definiert die Verbindung mit der Datenquelle, und ein Dataset stellt die Struktur der Daten dar. So gibt etwa ein verknüpfter Azure Storage-Dienst die Verbindungszeichenfolge an, um eine Verbindung mit einem Azure Storage-Konto herzustellen. Und ein Azure-Blobdataset gibt den Blobcontainer und den Ordner an, der die Daten enthält.

(Beispiele: Azure Storage, lokale SQL Server-Instanz, Azure HDInsight) Verknüpfte Dienste haben in Data Factory zwei Zwecke:

- Sie stellen einen **Datenspeicher** dar (beispielsweise eine lokale SQL Server-Instanz, eine Oracle-Datenbank, eine Dateifreigabe oder ein Azure Blob Storage-Konto). Eine Liste mit unterstützten Datenspeichern finden Sie im [Artikel zur Kopieraktivität](copy-activity-overview.md).
- Sie stellen eine **Computeressource** dar, die die Ausführung einer Aktivität hosten kann. So wird beispielsweise die HDInsightHive-Aktivität in einem HDInsight-Hadoop-Cluster ausgeführt. Eine Liste mit Transformationsaktivitäten und unterstützten Compute-Umgebungen finden Sie im [Artikel zur Datentransformation](transform-data.md).

### <a name="triggers"></a>Trigger
Trigger stellen die Verarbeitungseinheit dar, die bestimmt, wann eine Pipelineausführung initiiert werden soll. Es stehen verschiedene Arten von Triggern für verschiedene Arten von Ereignissen zur Verfügung. In der Vorschauversion werden zeitgesteuerte Trigger unterstützt. 


### <a name="pipeline-runs"></a>Pipelineausführungen
Eine Pipelineausführung ist eine Instanz der ausgeführten Pipeline. Zur Instanziierung von Pipelineausführungen werden in der Regel die Argumente an die in Pipelines definierten Parameter übergeben. Die Argumente können manuell oder im Rahmen der Triggerdefinition übergeben werden.

### <a name="parameters"></a>Parameter
Parameter sind Schlüssel-Wert-Paare einer schreibgeschützten Konfiguration.  Sie werden in der Pipeline definiert, und die Argumente für die definierten Parameter werden im Rahmen der Ausführung aus dem Ausführungskontext eines Triggers oder einer manuell ausgeführten Pipeline übergeben. Die Parameterwerte werden von Aktivitäten in der Pipeline genutzt.
Ein Dataset ist ein stark typisierter Parameter und eine wiederverwendbare/referenzierbare Entität. Eine Aktivität kann auf Datasets verweisen und die definierten Eigenschaften aus der Datasetdefinition nutzen.

Bei einem verknüpften Dienst handelt es sich ebenfalls um einen stark typisierten Parameter mit Verbindungsinformationen für einen Datenspeicher oder eine Compute-Umgebung. Er ist auch eine wiederverwendbare/referenzierbare Entität.

### <a name="control-flow"></a>Ablaufsteuerung
Die Orchestrierung von Pipelineaktivitäten umfasst die Verkettung von Aktivitäten in einer Sequenz, Verzweigungen, auf der Pipelineebene definierbare Parameter sowie Argumente, die beim bedarfs- oder triggergesteuerten Aufrufen der Pipeline übergeben werden. Darüber hinaus umfasst sie das Übergeben von benutzerdefinierten Zuständen und Schleifencontainern (ForEach-Iteratoren).


Weitere Informationen zu den Data Factory-Konzepten finden Sie in den folgenden Artikeln:

- [Datasets und verknüpfte Dienste](concepts-datasets-linked-services.md)
- [Pipelines und Aktivitäten](concepts-pipelines-activities.md)
- [Integrationslaufzeit](concepts-integration-runtime.md)

## <a name="what-is-the-pricing-model-for-data-factory"></a>Wie sieht das Preismodell für Data Factory aus?
Preisinformationen zu Azure Data Factory finden Sie auf der Seite [Data Factory – Preisübersicht](https://azure.microsoft.com/pricing/details/data-factory/).

## <a name="what-regions-support-azure-data-factory-version-2"></a>Welche Regionen unterstützen Azure Data Factory, Version 2?
Derzeit können Data Factorys der Version 2 in den Regionen „USA, Osten“ und „USA, Osten 2“ erstellt werden. Allerdings kann eine Data Factory die Integrationslaufzeit in einer anderen Region verwenden, zum Daten zwischen Datenspeichern, Aktivitäten für Computedienste oder SSIS-Pakete zu verteilen.  Weitere Informationen finden Sie unter [Data Factory-Speicherorte](concepts-integration-runtime.md#integration-runtime-location).

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>Wie kann ich hinsichtlich der Informationen zur Data Factory auf dem neuesten Stand bleiben?
Aktuelle Informationen zu Azure Data Factory finden Sie auf den folgenden Websites:

- [Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Startseite der Dokumentation](/azure/data-factory)
- [Produktstartseite](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Technischer Einblick 

### <a name="can-i-have-version-1-and-version-2-pipelines-run-side-by-side"></a>Kann ich Pipelines der Version 1 und Version 2 parallel ausführen?
Nein. Data Factorys der Version 2 oder Version 1 dürfen keine Entitäten (verknüpfte Dienste, Datasets, Pipelines usw.) einer anderen Version aufweisen.   

### <a name="do-i-still-need-to-define-datasets-in-version-2"></a>Muss ich in Version 2 weiterhin Datasets definieren?
Ein Dataset ist für die meisten Aktivitäten keine obligatorische Entität mehr. Es ist für die Aktivitäten „Kopieren“, „Machine Learning“, „Nachschlagen“, „Validierung“ sowie für benutzerdefinierte Aktivitäten erforderlich, die das Schema und andere Metadateninformationen im Dataset für die Transformation verwenden. Die übrigen Aktivitäten benötigen keine Datasets mehr.

### <a name="can-i-chain-two-activities-without-a-dataset-in-version-2"></a>Kann ich zwei Aktivitäten ohne ein Dataset in Version 2 verketten?
Ja. Sie können Aktivitäten in Version 2 ohne Datasets verketten. Sie verketten Aktivitäten mithilfe der **dependsOn**-Eigenschaft in der JSON-Definition der Pipeline. 

### <a name="are-all-the-version-1-activities-supported-in-version-2"></a>Werden alle Aktivitäten der Version 1 in Version 2 unterstützt? 
Ja, es werden alle Aktivitäten der Version 1 unterstützt.

### <a name="how-can-i-schedule-a-version-2-pipeline"></a>Wie kann ich eine Pipeline der Version 2 planen? 
Sie können mit dem Planer-Trigger eine Pipeline der Version 2 planen. Er verwendet einen Kalenderplan und ermöglicht es Benutzern, Pipelines entweder regelmäßig oder mithilfe kalenderbasierter wiederkehrender Muster zu planen (z. B. wöchentlich, jeden Montag um 18:00 Uhr und jeden Donnerstag um 21:00 Uhr). Weitere Informationen finden Sie unter [Pipelineausführung und -trigger](concepts-pipeline-execution-triggers.md).

### <a name="can-i-pass-parameters-to-a-pipeline-run-in-version-2"></a>Kann ich Parameter an eine Pipelineausführung in Version 2 übergeben?
Ja, Parameter sind in Version 2 ein hervorragendes allgemeines Konzept. Sie können Parameter auf Pipelineebene definieren und Argumente übergeben, während die Pipelineausführung bei Bedarf oder mithilfe eines Triggers gestartet wird.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Kann ich Standardwerte für die Pipelineparameter definieren? 
Ja. Sie können Standardwerte für die Parameter in den Pipelines definieren. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-passed-to-a-pipeline-run"></a>Kann eine Aktivität in einer Pipeline Argumente verarbeiten, die an eine Pipelineausführung übergeben werden? 
Ja. Jede Aktivität innerhalb der Pipeline kann den Parameterwert verwenden, der über das `@parameter`-Konstrukt an die Pipelineausführung übergeben wurde. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Kann die Ausgabeeigenschaft einer Aktivität in einer anderen Aktivität verwendet werden? 
Ja. Die Ausgabe einer Aktivität kann mit dem @activity-Konstrukt in einer nachfolgenden Aktivität verwendet werden.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Wie werden NULL-Werte ordnungsgemäß in der Ausgabe einer Aktivität behandelt? 
NULL-Werte können mithilfe des `@coalesce`-Konstrukts in den Ausdrücken ordnungsgemäß behandelt werden. 

### <a name="can-i-use-retries-timeouts-at-the-activity-level-in-version-2"></a>Kann ich auf der Aktivitätsebene in Version 2 Wiederholungen und Timeouts verwenden?
Ja. Sie können Wiederholungen und Timeouts auf der Aktivitätsebene konfigurieren, um die Ausführung von Aktivitäten in Version 2 wie in Version 1 zu steuern. 

## <a name="next-steps"></a>Nächste Schritte
Schrittweise Anweisungen zum Erstellen einer Data Factory der Version 2 finden Sie in den folgenden Tutorials:

- [Schnellstart: Erstellen einer Data Factory](quickstart-create-data-factory-dot-net.md)
- [Tutorial: Kopieren von Daten in die Cloud](tutorial-copy-data-dot-net.md)

