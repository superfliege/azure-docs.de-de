---
title: Includedatei
description: Includedatei
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 1/10/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 42c1856f30484532e1ace2e84187bcaaacdf4c72
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553517"
---
Azure Data Factory ist ein mehrinstanzenfähiger Dienst mit den folgenden Standardgrenzwerten, die sicherstellen sollen, dass Kundenabonnements vor anderen Workloads geschützt sind. Wenden Sie sich an den Support, um die Grenzwerte bis zum Höchstwert für Ihr Abonnement zu erhöhen.

### <a name="version-2"></a>Version 2

| Ressource | Standardlimit | Maximales Limit |
| -------- | ------------- | ------------- |
| Data Factorys in einem Azure-Abonnement | 50 | [Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Gesamtanzahl von Entitäten (z. B. Pipeline, Datasets, Trigger, verknüpfte Dienste, Integration Runtimes) in einer Data Factory | 5.000 | [Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Gesamtanzahl CPU-Kerne für Azure-SSIS Integration Runtimes unter einem Abonnement | 256 | [Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Gleichzeitige Pipelineausführungen pro Data Factory (von allen Pipelines der Factory gemeinsam genutzt) | 10.000  | [Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Maximale Aktivitäten pro Pipeline, einschließlich interner Aktivitäten für Container | 40 | 40 |
| Maximale Anzahl verknüpfter Integration Runtimes, die für eine einzelne selbstgehostete Integration Runtime erstellt werden kann | 20 | [Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Maximale Parameter pro Pipeline | 50 | 50 |
| ForEach-Elemente | 100.000 | 100.000 |
| ForEach-Parallelität | 20 | 50 |
| Zeichen pro Ausdruck | 8.192 | 8.192 |
| Mindestintervall für Trigger für ein rollierendes Fenster | 15 Min. | 15 Min. |
| Maximales Zeitlimit für Ausführungen von Pipelineaktivitäten | 7 Tage | 7 Tage |
| Bytes pro Objekt für Pipelineobjekte<sup>1</sup> | 200 KB | 200 KB |
| Bytes pro Objekt für Datasets und Objekte für verknüpfte Dienste<sup>1</sup> | 100 KB | 2.000 KB |
| Datenintegrationseinheiten pro Kopieraktivitätsausführung<sup>3</sup> | 256 | [Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Schreiben von API-Aufrufen | 2.500/h<br/><br/> Dieser Grenzwert wird von Azure Resource Manager, nicht von Azure Data Factory vorgegeben. | [Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Lesen von API-Aufrufen | 12.500/h<br/><br/> Dieser Grenzwert wird von Azure Resource Manager, nicht von Azure Data Factory vorgegeben. | [Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Überwachungsabfragen pro Minute | 1.000 | [Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| CRUD-Vorgänge für Entitäten pro Minute | 50 | [Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |


### <a name="version-1"></a>Version 1

| **Ressource** | **Standardlimit** | **Maximales Limit** |
| --- | --- | --- |
| Data Factorys in einem Azure-Abonnement |50 |[Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Pipelines innerhalb einer Data Factory |2.500 |[Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Datasets innerhalb einer Data Factory |5.000 |[Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Gleichzeitige Slices pro Dataset |10 |10 |
| Bytes pro Objekt für Pipelineobjekte<sup>1</sup> |200 KB |200 KB |
| Bytes pro Objekt für Datasets und Objekte für verknüpfte Dienste<sup>1</sup> |100 KB |2.000 KB |
| Bedarfsgesteuerte Azure HDInsight-Clusterkerne innerhalb eines Abonnements<sup>2</sup> |60 |[Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Einheiten für Clouddatenverschiebungen pro Kopieraktivitätsausführung<sup>3</sup> |32 |[Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Anzahl der Wiederholungsversuche für Ausführungen von Pipelineaktivitäten |1.000 |MaxInt (32 Bit) |

<sup>1</sup> Objekte für Pipelines, Datasets und verknüpfte Dienste stellen eine logische Gruppierung Ihrer Workload dar. Die Grenzwerte für diese Objekte beziehen sich nicht auf die Datenmenge, die Sie mit Azure Data Factory verschieben und verarbeiten können. Data Factory kann so skaliert werden, dass Petabytes an Daten verarbeitet werden können.

<sup>2</sup> Bedarfsgesteuerte HDInsight-Kerne werden über das Abonnement zugewiesen, das die Data Factory enthält. Daher ist der frühere Grenzwert der von Data Factory erzwungene Grenzwert für bedarfsgesteuerte HDInsight-Kerne. Er unterscheidet sich vom Grenzwert für Kerne, der Ihrem Azure-Abonnement zugeordnet ist.

<sup>3</sup> Die Datenintegrationseinheit (Data Integration Unit, DIU) für Version 2 oder die Einheit für Clouddatenverschiebungen (Cloud Data Movement Unit, DMU) für Version 1 wird in einem Cloud-zu-Cloud-Kopiervorgang verwendet. Sie stellt ein Maß für die Leistungsfähigkeit einer einzelnen Einheit in Data Factory dar. Sie kombiniert die Zuweisungen für CPU, Arbeitsspeicher und Netzwerkressourcen. Durch die Nutzung von weitere DMUs für einige Szenarien können Sie einen höheren Durchsatz erreichen. Weitere Informationen finden Sie unter [Einheiten für die Datenintegration (Version 2)](../articles/data-factory/copy-activity-performance.md#data-integration-units) und [Einheiten für Clouddatenverschiebungen (Version 1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units). Weitere Informationen zur Abrechnung finden Sie unter [Azure Data Factory – Preise](https://azure.microsoft.com/pricing/details/data-factory/).

<sup>4</sup> Bei der Integration Runtime (IR) handelt es sich um die Computeinfrastruktur, mit der Azure Data Factory die Datenintegrationsfunktionen für verschiedene Netzwerkumgebungen bereitstellt. Dazu gehöre u. a. die Datenverschiebung, das Auslagern von Aktivitäten in Computedienste und das Ausführen von SSIS-Paketen. Weitere Informationen finden Sie unter [Übersicht über die Integration Runtime](../articles/data-factory/concepts-integration-runtime.md).

| **Ressource** | **Unteres Standardlimit** | **Minimales Limit** |
| --- | --- | --- |
| Planungsintervall |15 Minuten |15 Minuten |
| Intervall zwischen Wiederholungsversuchen |1 Sekunde |1 Sekunde |
| Timeoutwert für Wiederholungsversuche |1 Sekunde |1 Sekunde |

#### <a name="web-service-call-limits"></a>Limits für Webdienstaufrufe
Azure Resource Manager weist Grenzwerte für API-Aufrufe auf. Sie können API-Aufrufe mit einer Rate innerhalb der [API-Limits von Azure Resource Manager](../articles/azure-subscription-service-limits.md#resource-group-limits)ausführen.
