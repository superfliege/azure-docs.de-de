---
title: Includedatei
description: Includedatei
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 06/20/2018
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 4aa4809c57eaf26b10053d432f9191580ec143a0
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44381081"
---
Data Factory ist ein mehrinstanzenfähiger Dienst mit den folgenden Standardlimits, um sicherzustellen, dass Kundenabonnements vor anderen Arbeitslasten geschützt sind. Viele der Limits können problemlos für Ihr Abonnement bis zum maximalen Limit angehoben werden, indem Sie sich an den Support wenden.

### <a name="version-2"></a>Version 2

| Ressource | Standardlimit | Maximales Limit |
| -------- | ------------- | ------------- |
| Data Factorys in einem Azure-Abonnement | 50 | [Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gesamtanzahl von Entitäten (Pipeline, Datasets, Trigger, verknüpfte Dienste, Integration Runtimes) in einer Data Factory | 5.000 | [Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gesamtzahl CPU-Kerne für Azure-SSIS Integration Runtime(s) unter einem Abonnement | 128 | [Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gleichzeitige Pipelineausführungen pro Pipeline | 100 | [Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gleichzeitige Pipelineausführungen pro Data Factory | 10.000  | [Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Max. Aktivitäten pro Pipeline (einschließlich interner Aktivitäten für Container) | 40 | 40 |
| Max. Parameter pro Pipeline | 50 | 50 |
| ForEach-Elemente | 100.000 | 100.000 |
| ForEach-Parallelität | 20 | 50 |
| Zeichen pro Ausdruck | 8.192 | 8.192 |
| Mindestintervall für Trigger für ein rollierendes Fenster | 15 Min. | 15 Min. |
| Max. Zeitlimit für Ausführungen von Pipelineaktivitäten | 7 Tage | 7 Tage |
| Bytes pro Objekt für Pipelineobjekte<sup>1</sup> | 200 KB | 200 KB |
| Bytes pro Objekt für DataSets und Objekte für verknüpfte Dienste<sup>1</sup> | 100 KB | 2000 KB |
| Datenintegrationseinheiten pro Kopieraktivitätsausführung <sup>3</sup> | 256 | [Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Schreiben von API-Aufrufen | 2500/h<br/><br/> Dieser Grenzwert wird von Azure Resource Manager, nicht von Azure Data Factory vorgegeben. | [Wenden Sie sich an den Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Lesen von API-Aufrufen | 12.500/h<br/><br/> Dieser Grenzwert wird von Azure Resource Manager, nicht von Azure Data Factory vorgegeben. | [Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Überwachungsabfragen pro Minute | 1000 | [Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| CRUD-Vorgänge für Entitäten pro Minute | 50 | [Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |


### <a name="version-1"></a>Version 1

| **Ressource** | **Standardlimit** | **Maximales Limit** |
| --- | --- | --- |
| Data Factorys in einem Azure-Abonnement |50 |[Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Pipelines innerhalb einer Data Factory |2500 |[Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| DataSets innerhalb einer Data Factory |5.000 |[Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gleichzeitige Slices pro DataSet |10 |10 |
| Bytes pro Objekt für Pipelineobjekte<sup>1</sup> |200 KB |200 KB |
| Bytes pro Objekt für DataSets und Objekte für verknüpfte Dienste<sup>1</sup> |100 KB |2000 KB |
| Bedarfsgesteuerte HDInsight-Clusterkerne innerhalb eines Abonnements <sup>2</sup> |60 |[Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Einheiten für Clouddatenverschiebungen pro Kopieraktivitätsausführung <sup>3</sup> |32 |[Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Anzahl der Wiederholungsversuche für Ausführungen von Pipelineaktivitäten |1000 |MaxInt (32 Bit) |

<sup>1</sup> Pipeline-, Dataset- und verknüpfte Dienstobjekte stellen eine logische Gruppierung Ihrer Workload dar. Limits für diese Objekte beziehen sich nicht auf die Datenmenge, die Sie mit dem Azure Data Factory-Dienst verschieben und verarbeiten können. Data Factory kann so skaliert werden, dass Petabytes an Daten verarbeitet werden können.

<sup>2</sup> Bedarfsgesteuerte HDInsight-Kerne werden über das Abonnement zugewiesen, das die Data Factory enthält. Daher ist das obige Limit das von der Data Factory erzwungene Kernlimit für bedarfsgesteuerte HDInsight-Kerne und unterscheidet sich vom Kernlimit, das für Ihr Azure-Abonnement gilt.

<sup>3</sup> Die Datenintegrationseinheit (Data Integration Unit, DIU) für v2 oder die Einheit für Clouddatenverschiebungen (Cloud Data Movement Unit, DMU) für v1 wird in einem Cloud-zu-Cloud-Kopiervorgang verwendet. Sie ist eine Messgröße für die Leistungsfähigkeit (Kombination aus zugeteilten CPU-, Speicher- und Netzwerkressourcen) einer einzelnen Einheit in Data Factory. Durch die Nutzung von weitere DMUs für einige Szenarien können Sie einen höheren Durchsatz erreichen. Ausführliche Informationen finden Sie in den Abschnitten [Datenintegrationseinheiten (V2)](../articles/data-factory/copy-activity-performance.md#data-integration-units) und [Einheiten für Clouddatenverschiebungen (V1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units). Informationen zu den Auswirkungen auf die Abrechnung finden Sie auf der [Seite mit Preisdetails zu Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/).

<sup>4</sup> Bei der Integration Runtime (IR) handelt es sich um die Computeinfrastruktur, mit der Azure Data Factory die folgenden Datenintegrationsfunktionen für verschiedene Netzwerkumgebungen bereitstellt: Verschieben von Daten, Auslagern von Aktivitäten in Computedienste, Ausführen von SSIS-Paketen. Weitere Informationen finden Sie unter [Übersicht über die Integration Runtime](../articles/data-factory/concepts-integration-runtime.md).

| **Ressource** | **Unteres Standardlimit** | **Minimales Limit** |
| --- | --- | --- |
| Planungsintervall |15 Minuten |15 Minuten |
| Intervall zwischen Wiederholungsversuchen |1 Sekunde |1 Sekunde |
| Timeoutwert für Wiederholungsversuche |1 Sekunde |1 Sekunde |

#### <a name="web-service-call-limits"></a>Limits für Webdienstaufrufe
Azure Resource Manager weist Grenzwerte für API-Aufrufe auf. Sie können API-Aufrufe mit einer Rate innerhalb der [API-Limits von Azure Resource Manager](../articles/azure-subscription-service-limits.md#resource-group-limits)ausführen.
