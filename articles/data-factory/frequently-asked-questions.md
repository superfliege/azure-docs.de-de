---
title: 'Azure Data Factory: Häufig gestellte Fragen | Microsoft-Dokumentation'
description: Hier finden Sie Antworten auf häufig gestellte Fragen zu Azure Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: shlo
ms.openlocfilehash: d900c9c438ed40841639d556643a231075f4f194
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249586"
---
# <a name="azure-data-factory-faq"></a>Azure Data Factory: Häufig gestellte Fragen
Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Azure Data Factory.  

## <a name="what-is-azure-data-factory"></a>Was ist Azure Data Factory? 
Data Factory ist ein vollständig verwalteter, cloudbasierter Datenintegrationsdienst, der das Verschieben und Transformieren von Daten automatisiert. Genau wie ein Betrieb, in dem Anlagen Rohmaterialien in Endprodukte umwandeln, organisiert Azure Data Factory vorhandene Dienste so, dass Rohdaten gesammelt und in nutzbare Informationen transformiert werden. 

Sie können mithilfe von Azure Data Factory datengesteuerte Workflows zum Verschieben von Daten zwischen lokalen Datenspeichern und Clouddatenspeichern erstellen. Und Sie können Daten mit Compute Services wie Azure HDInsight, Azure Data Lake Analytics und SQL Server Integration Services (SSIS) Integration Runtime verarbeiten und transformieren. 

Mit Data Factory können Sie die Datenverarbeitung in einem Azure-basierten Clouddienst oder in Ihrer eigenen selbstgehosteten Compute-Umgebung wie SSIS, SQL Server oder Oracle ausführen. Nachdem Sie eine Pipeline erstellt haben, die die gewünschte Aktion ausführt, können Sie die regelmäßige Ausführung der Pipeline planen (beispielsweise stündlich, täglich oder wöchentlich), einen Plan mit Zeitfenstern aufstellen oder die Pipeline über ein Ereignis auslösen. Weitere Informationen finden Sie unter [Einführung in Azure Data Factory](introduction.md).

### <a name="control-flows-and-scale"></a>Ablaufsteuerungen und Skalierung 
Zur Unterstützung der unterschiedlichen Abläufe und Muster im modernen Data Warehouse ermöglicht Data Factory die flexible Datenpipelinemodellierung mit umfassenden Paradigmen für die Programmierung der Ablaufsteuerung, z.B. bedingte Ausführung, Verzweigung in Datenpipelines und explizites Übergeben von Parametern für diese Abläufe (intern und übergreifend). Außerdem umfasst die Ablaufsteuerung die Transformation von Daten per Aktivitätsverteilung auf externe Ausführungsmodule und Datenflussfunktionen, z.B. bedarfsabhängige Datenverschiebungen, über die Copy-Aktivität.

Mit Data Factory können Sie nun flexibel einen beliebigen Ablaufstil modellieren, der für die Datenintegration benötigt wird und entweder bedarfsabhängig oder nach einem bestimmten Zeitplan angewendet werden soll. Einige allgemeine Abläufe, die mit diesem Modell ermöglicht werden, sind:   

- Ablaufsteuerungen:
    - Verketten Sie Aktivitäten in einer Sequenz innerhalb einer Pipeline.
    - Verzweigen Sie Aktivitäten innerhalb einer Pipeline.
    - Parameter
        - Definieren Sie Parameter auf Pipelineebene, und übergeben Sie Argumente, während die Pipelineausführung bei Bedarf oder mithilfe eines Triggers gestartet wird.
        - Aktivitäten können die an die Pipeline übergebenen Argumente nutzen.
    - Übergeben von benutzerdefinierten Zuständen
        - Aktivitätsausgaben (einschließlich des Zustands) können von einer Folgeaktivität in der Pipeline genutzt werden.
    - Schleifencontainer
        - ForEach 
- Triggerbasierte Abläufe:
    - Pipelines können nach Bedarf oder zeitgesteuert ausgelöst werden.
- Deltaabläufe:
    - Verwenden Sie Parameter, und definieren Sie die Obergrenze für Deltakopien beim Verschieben von Dimensions- oder Referenztabellen aus einem relationalen Speicher (entweder lokal oder in der Cloud), um die Daten in den Lake zu laden. 

Weitere Informationen finden Sie im [Tutorial zur Ablaufsteuerung](tutorial-control-flow.md).

### <a name="transform-your-data-at-scale-with-code-free-pipelines"></a>Bedarfsabhängiges Transformieren Ihrer Daten mit Pipelines ohne Code
Die neue browserbasierte Toolumgebung ermöglicht die Erstellung und Bereitstellung von Pipelines ohne Code mit einer modernen, interaktiven webbasierten Benutzeroberfläche.

Für Entwickler von visuellen Daten und Datentechniker ist die ADF-Webbenutzeroberfläche die Entwurfsumgebung ohne Code, die zum Erstellen von Pipelines verwendet wird. Sie ist vollständig mit Visual Studio Online Git integriert und ermöglicht die Integration für CI/CD und die iterative Entwicklung mit Debugoptionen.

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>Umfassende plattformübergreifende SDKs für erfahrene Benutzer
Für erfahrene Benutzer, die auf der Suche nach einer befehlsorientierten Benutzerschnittstelle sind, bietet ADF V2 ein breites Spektrum an SDKs zum Erstellen, Verwalten und Überwachen von Pipelines mit Ihrer bevorzugten IDE.
1.  Python SDK
2.  PowerShell CLI
3.  C#-SDK-Benutzer können außerdem die dokumentierten REST-APIs als Schnittstelle für ADF V2 nutzen.

### <a name="iterative-development-and-debugging-using-visual-tools"></a>Iterative Entwicklung und Debugging mit visuellen Tools
Mit den visuellen Tools von Azure Data Factory (ADF) können Sie die iterative Entwicklung und das Debuggen durchführen. Mit der Funktion „Debuggen“ auf der Pipelinecanvas können Sie Pipelines erstellen und Testläufe ausführen, ohne eine einzige Codezeile schreiben zu müssen. Sie können die Ergebnisse der Testläufe auf der Pipelinecanvas im Fenster „Ausgabe“ anzeigen. Sie können der Pipeline nach der erfolgreichen Ausführung des Testlaufs weitere Aktivitäten hinzufügen und das iterative Debuggen fortsetzen. Außerdem können Sie die Testläufe auch abbrechen, wenn sich diese in Bearbeitung befinden. Es ist nicht erforderlich, Ihre Änderungen am Data Factory-Dienst zu veröffentlichen, bevor Sie auf „Debuggen“ klicken. Dies ist in Fällen hilfreich, in denen Sie sicherstellen möchten, dass die neuen Hinzufügungen bzw. Änderungen wie erwartet funktionieren, bevor Sie Ihre Data Factory-Workflows in Entwicklungs-, Test- oder Produktionsumgebungen aktualisieren. 

### <a name="deploy-ssis-packages-to-azure"></a>Bereitstellen von SSIS-Paketen in Azure 
Wenn Sie Ihre SSIS-Workloads verschieben möchten, können Sie eine Data Factory-Instanz erstellen und eine Azure-SSIS-Integration Runtime bereitstellen. Die Azure-SSIS-IR ist ein vollständig verwalteter Cluster mit virtuellen Azure-Computern (Knoten), die speziell für die Ausführung von SSIS-Paketen in der Cloud bestimmt sind. Eine ausführliche Anleitung finden Sie im Tutorial [Bereitstellen von SSIS-Paketen in Azure](tutorial-create-azure-ssis-runtime-portal.md). 
 
### <a name="sdks"></a>SDKs
Für erfahrene Benutzer, die auf der Suche nach einer befehlsorientierten Benutzerschnittstelle sind, bietet ADF ein breites Spektrum an SDKs zum Erstellen, Verwalten und Überwachen von Pipelines mit Ihrer bevorzugten IDE. Die Sprachunterstützung umfasst .NET, PowerShell, Python und REST.

### <a name="monitoring"></a>Überwachung
Sie können Ihre Data Factorys per PowerShell, SDK oder mit den visuellen Überwachungstools auf der Benutzeroberfläche des Browsers überwachen. Sie können bedarfsabhängige, triggerbasierte und zeitgesteuerte benutzerdefinierte Datenflüsse auf effiziente und effektive Weise überwachen und verwalten. Brechen Sie vorhandene Aufgaben ab, nutzen Sie die Fehlerübersicht, führen Sie einen Drilldown aus, um ausführliche Fehlermeldungen zu erhalten, und debuggen Sie alle Probleme an einem zentralen Ort, ohne den Kontext wechseln oder zwischen Fenstern navigieren zu müssen. 

### <a name="new-features-for-ssis-in-adf"></a>Neue Features für SSIS in ADF
Seit der ersten öffentlichen Vorschauversion von 2017 wurden über Data Factory die folgenden Features für SSIS hinzugefügt:

-   Unterstützung für drei weitere Konfigurationen/Varianten von Azure SQL-Datenbank (DB) zum Hosten des SSIS-Katalogs von Projekten/Paketen (SSISDB):
-   Azure SQL-Datenbank mit VNET-Dienstendpunkten
-   Verwaltete Instanz
-   Pool für elastische Datenbanken
-   Unterstützung für Azure Resource Manager Virtual Network (VNET) zusätzlich zum klassischen VNET, das in Zukunft als veraltet eingestuft wird. Dies ermöglicht es Ihnen, für Ihre Azure-SSIS-Integration Runtime (IR) das Einfügen bzw. den Beitritt zu einem VNET durchzuführen, das für Azure SQL-Datenbank mit VNET-Dienstendpunkten, einer verwalteten Instanz und lokalem Datenzugriff konfiguriert ist (siehe https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) 
-   Unterstützung für AAD-Authentifizierung (Azure Active Directory) zusätzlich zur SQL-Authentifizierung für die Verbindungsherstellung mit Ihrer SSISDB. Auf diese Weise können Sie AAD-Authentifizierung mit Ihrer ADF-verwalteten Identität für Azure-Ressourcen verwenden.
-   Unterstützung für die Bereitstellung Ihrer eigenen lokalen SQL Server-Lizenz zur Erzielung erheblicher Kosteneinsparungen über den Azure-Hybridvorteil
-   Unterstützung für die Enterprise Edition von Azure-SSIS-IR, die Ihnen die Verwendung von erweiterten bzw. Premium-Features, des benutzerdefinierten Setups zum Installieren von zusätzlichen Komponenten/Erweiterungen und eines Drittanbieter-Ökosystems ermöglicht (siehe https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/) 
-   Tiefere Integration von SSIS in ADF, die Ihnen das Aufrufen bzw. Auslösen von erstklassigen Aktivitäten zum Ausführen des SSIS-Pakets in ADF-Pipelines und die Planung per SSMS ermöglicht (siehe https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/) 


## <a name="what-is-integration-runtime"></a>Was ist Integration Runtime?
Integration Runtime ist die Computeinfrastruktur, mit der Azure Data Factory die folgenden Datenintegrationsfunktionen für verschiedene Netzwerkumgebungen bereitstellt:

- **Datenverschiebung**: Zur Datenverschiebung werden die Daten von der Integration Runtime zwischen dem Quell- und Zieldatenspeicher verschoben, und es wird gleichzeitig Unterstützung für integrierte Connectors, Formatkonvertierungen, Spaltenzuordnungen und performante sowie skalierbare Datenübertragungen bereitgestellt.
- **Verteilungsaktivitäten**: Für die Transformation bietet die Integration Runtime Funktionen zur nativen Ausführung von SSIS-Paketen.
- **SSIS-Pakete ausführen**: Dient zum nativen Ausführen von SSIS-Paketen in einer verwalteten Azure-Compute-Umgebung. Zudem unterstützt die Integration Runtime auch die Übermittlung und Überwachung von Transformationsaktivitäten, die in verschiedensten Computediensten wie Azure HDInsight, Azure Machine Learning, Azure SQL-Datenbank, SQL Server und vielen weiteren ausgeführt werden.

Sie können zum Verschieben und Transformieren von Daten je nach Bedarf eine oder mehrere Integration Runtime-Instanzen bereitstellen. Integration Runtime kann in einem öffentlichen Azure-Netzwerk oder einem privaten Netzwerk (lokal, Azure Virtual Network oder Virtual Private Cloud (VPC) von Amazon Web Services) ausgeführt werden. 

Weitere Informationen finden Sie unter [Integrationslaufzeit in Azure Data Factory](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Welcher Grenzwert besteht hinsichtlich der Anzahl von Integrationslaufzeiten?
Es gibt keine festen Grenzwerte hinsichtlich der Anzahl von Integration Runtime-Instanzen in einer Data Factory. Es gibt jedoch einen Grenzwert hinsichtlich der Anzahl von VM-Kernen, die von Integration Runtime pro Abonnement für die Ausführung von SSIS-Paketen verwendet werden können. Weitere Informationen finden Sie unter [Data Factory-Grenzwerte](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>Wie lauten die Hauptkonzepte von Azure Data Factory?
Ein Azure-Abonnement kann über mindestens eine Azure Data Factory-Instanz (oder Data Factory) verfügen. Azure Data Factory enthält vier Hauptkomponenten, die zusammen als Plattform fungieren, auf der Sie datengesteuerte Workflows mit Schritten zum Verschieben und Transformieren von Daten zusammenstellen können.

### <a name="pipelines"></a>Pipelines
Eine Data Factory kann eine oder mehrere Pipelines haben. Bei einer Pipeline handelt es sich um eine logische Gruppierung von Aktivitäten zur Durchführung einer Arbeitseinheit. Gemeinsam führen die Aktivitäten einer Pipeline eine Aufgabe durch. Eine Pipeline kann beispielsweise eine Gruppe von Aktivitäten enthalten, die Daten aus einem Azure-Blob erfasst, und anschließend eine Hive-Abfrage in einem HDInsight-Cluster ausführen, um die Daten zu partitionieren. Der Vorteil ist, dass Sie eine Pipeline zum Verwalten der Aktivitäten als Gruppe verwenden können, statt jede Aktivität einzeln zu verwalten. Sie können die Aktivitäten in einer Pipeline miteinander verketten, um sie sequenziell auszuführen. Sie können sie aber auch unabhängig voneinander parallel ausführen.

### <a name="activity"></a>Aktivität
Aktivitäten stellen einen Verarbeitungsschritt in einer Pipeline dar. Beispielsweise können Sie eine *Kopieraktivität* verwenden, um Daten zwischen zwei Datenspeichern zu kopieren. Analog dazu können Sie eine Hive-Aktivität verwenden, die eine Hive-Abfrage für einen Azure HDInsight-Cluster ausführt, um Ihre Daten zu transformieren oder zu analysieren. Data Factory unterstützt drei Arten von Aktivitäten: Datenverschiebungsaktivitäten, Datentransformationsaktivitäten und Steuerungsaktivitäten.

### <a name="datasets"></a>Datasets
Datasets stellen Datenstrukturen in den Datenspeichern dar, die einfach auf die Daten zeigen bzw. verweisen, die Sie in Ihren Aktivitäten als Eingaben oder Ausgaben verwenden möchten. 

### <a name="linked-services"></a>Verknüpfte Dienste
Verknüpfte Dienste ähneln Verbindungszeichenfolgen, mit denen die Verbindungsinformationen definiert werden, die für Data Factory zum Herstellen einer Verbindung mit externen Ressourcen erforderlich sind. Sie können sich dies wie folgt vorstellen: Ein verknüpfter Dienst definiert die Verbindung mit der Datenquelle, und ein Dataset stellt die Struktur der Daten dar. So gibt etwa ein verknüpfter Azure Storage-Dienst die Verbindungszeichenfolge an, um eine Verbindung mit einem Azure Storage-Konto herzustellen. Und ein Azure-Blobdataset gibt den Blobcontainer und den Ordner an, der die Daten enthält.

Verknüpfte Dienste haben in Data Factory zwei Funktionen:

- Sie stellen einen *Datenspeicher* dar, der beispielsweise eine lokale SQL Server-Instanz, eine Oracle-Datenbankinstanz, eine Dateifreigabe oder ein Azure Blob Storage-Konto enthalten kann (er ist aber nicht darauf beschränkt). Eine Liste der unterstützten Datenspeicher finden Sie unter [Kopieraktivität in Azure Data Factory](copy-activity-overview.md).
- Sie stellen eine *Computeressource* dar, die die Ausführung einer Aktivität hosten kann. So wird beispielsweise die HDInsight-Hive-Aktivität in einem HDInsight-Hadoop-Cluster ausgeführt. Eine Liste mit Transformationsaktivitäten und unterstützten Compute-Umgebungen finden Sie unter [Transformieren von Daten in Azure Data Factory](transform-data.md).

### <a name="triggers"></a>Trigger
Trigger stellen Verarbeitungseinheiten dar, die bestimmen, wann eine Pipelineausführung initiiert wird. Es gibt verschiedene Arten von Triggern für unterschiedliche Arten von Ereignissen. 

### <a name="pipeline-runs"></a>Pipelineausführungen
Eine Pipelineausführung ist eine Instanz einer ausgeführten Pipeline. Zur Instanziierung einer Pipelineausführung werden in der Regel die Argumente an die in Pipelines definierten Parameter übergeben. Die Argumente können manuell oder im Rahmen der Triggerdefinition übergeben werden.

### <a name="parameters"></a>Parameter
Parameter sind Schlüssel-Wert-Paare in einer schreibgeschützten Konfiguration. Sie definieren Parameter in einer Pipeline, und Sie übergeben die Argumente für die definierten Parameter während der Ausführung über einen Ausführungskontext. Der Ausführungskontext wird durch einen Trigger oder über eine Pipeline erstellt, die Sie manuell ausführen. Die Parameterwerte werden von Aktivitäten in der Pipeline genutzt.

Ein Dataset ist ein stark typisierter Parameter und eine Entität, die Sie wiederverwenden oder auf die Sie verweisen können. Eine Aktivität kann auf Datasets verweisen und die Eigenschaften nutzen, die in der Datasetdefinition festgelegt sind.

Bei einem verknüpften Dienst handelt es sich ebenfalls um einen stark typisierten Parameter mit Verbindungsinformationen für einen Datenspeicher oder eine Compute-Umgebung. Er ist auch eine Entität, die Sie wiederverwenden oder auf die Sie verweisen können.

### <a name="control-flows"></a>Ablaufsteuerungen
Ablaufsteuerungen orchestrieren Pipelineaktivitäten. Dies umfasst die Verkettung von Aktivitäten in einer Sequenz, Verzweigungen, auf Pipelineebene definierte Parameter sowie Argumente, die beim bedarfs- oder triggergesteuerten Aufrufen der Pipeline übergeben werden. Ablaufsteuerungen umfassen zudem das Übergeben von benutzerdefinierten Zuständen und Schleifencontainern (ForEach-Iteratoren).


Weitere Informationen zu den Data Factory-Konzepten finden Sie in den folgenden Artikeln:

- [Datasets und verknüpfte Dienste](concepts-datasets-linked-services.md)
- [Pipelines und Aktivitäten](concepts-pipelines-activities.md)
- [Integrationslaufzeit](concepts-integration-runtime.md)

## <a name="what-is-the-pricing-model-for-data-factory"></a>Wie sieht das Preismodell für Data Factory aus?
Preisinformationen zu Azure Data Factory finden Sie unter [Data Factory – Preisübersicht](https://azure.microsoft.com/pricing/details/data-factory/).

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>Wie kann ich hinsichtlich der Informationen zu Data Factory auf dem neuesten Stand bleiben?
Aktuelle Informationen zu Azure Data Factory finden Sie auf den folgenden Websites:

- [Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Startseite der Dokumentation](/azure/data-factory)
- [Produktstartseite](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Technischer Einblick 

### <a name="how-can-i-schedule-a-pipeline"></a>Wie kann ich eine Pipeline planen? 
Sie können den Planer-Trigger oder Zeitfenster-Trigger verwenden, um eine Pipeline zu planen. Der Trigger verwendet einen Kalenderplan, und Sie können damit die Ausführung von Pipelines so planen, dass sie entweder regelmäßig oder mithilfe kalenderbasierter wiederkehrender Muster erfolgt (z.B. wöchentlich, jeden Montag um 18:00 Uhr und jeden Donnerstag um 21:00 Uhr). Weitere Informationen finden Sie unter [Pipelineausführung und -trigger](concepts-pipeline-execution-triggers.md).

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>Kann ich Parameter an eine Pipelineausführung übergeben?
Ja. Parameter sind in ADF ein hervorragendes wichtiges Konzept. Sie können Parameter auf Pipelineebene definieren und Argumente übergeben, während die Pipelineausführung bei Bedarf oder mithilfe eines Triggers gestartet wird.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Kann ich Standardwerte für die Pipelineparameter definieren? 
Ja. Sie können Standardwerte für die Parameter in den Pipelines definieren. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Kann eine Aktivität in einer Pipeline Argumente verarbeiten, die an eine Pipelineausführung übergeben werden? 
Ja. Jede Aktivität innerhalb der Pipeline kann den Parameterwert verwenden, der über das `@parameter`-Konstrukt an die Pipelineausführung übergeben wird. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Kann die Ausgabeeigenschaft einer Aktivität in einer anderen Aktivität verwendet werden? 
Ja. Die Ausgabe einer Aktivität kann mit dem `@activity`-Konstrukt in einer nachfolgenden Aktivität verwendet werden.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Wie werden NULL-Werte ordnungsgemäß in der Ausgabe einer Aktivität behandelt? 
NULL-Werte können mithilfe des `@coalesce`-Konstrukts in den Ausdrücken ordnungsgemäß behandelt werden. 

## <a name="next-steps"></a>Nächste Schritte
Schritt-für-Schritt-Anleitungen zum Erstellen einer Data Factory-Instanz finden Sie in den folgenden Tutorials:

- [Schnellstart: Erstellen einer Data Factory](quickstart-create-data-factory-dot-net.md)
- [Tutorial: Kopieren von Daten in die Cloud](tutorial-copy-data-dot-net.md)
