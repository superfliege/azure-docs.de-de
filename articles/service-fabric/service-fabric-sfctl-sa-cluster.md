---
title: Azure Service Fabric-CLI – sfctl sa-cluster | Microsoft-Dokumentation
description: Beschreibt die sfctl-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI) für eigenständige Cluster.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: ffdbff7edc5af187071615c8b1e61790b3a38429
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763961"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Verwaltet eigenständige Service Fabric-Cluster.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| config | Ruft die Konfiguration eines eigenständigen Service Fabric-Clusters ab. |
| config-upgrade | Startet das Upgrade der Konfiguration eines eigenständigen Service Fabric-Clusters. |
| upgrade-status | Ruft den Upgradestatus der Clusterkonfiguration eines eigenständigen Service Fabric-Clusters ab. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa-cluster config
Ruft die Konfiguration eines eigenständigen Service Fabric-Clusters ab.

Ruft die Konfiguration eines eigenständigen Service Fabric-Clusters ab. Die Clusterkonfiguration enthält Eigenschaften des Clusters wie z.B. die unterschiedlichen Knotentypen im Cluster, Sicherheitskonfigurationen, Fehler- und Upgradedomänentopologien usw.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --configuration-api-version [erforderlich] | Die API-Version der JSON-Konfiguration des eigenständigen Clusters. |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: json, jsonc, table, tsv.  Standardwert\: json. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>sfctl sa-cluster config-upgrade
Startet das Upgrade der Konfiguration eines eigenständigen Service Fabric-Clusters.

Überprüft die bereitgestellten Parameter für das Konfigurationsupgrade und startet das Upgrade der Clusterkonfiguration, wenn die Parameter gültig sind.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --cluster-config            [erforderlich] | Die Clusterkonfiguration. |
| --delta-unhealthy-nodes | Der während des Upgrades maximal zulässige Prozentsatz der Integritätsminderung (Delta). Zulässige Werte sind ganze Zahlen von 0 bis 100. |
| --health-check-retry | Die Zeitspanne zwischen den Versuchen, Integritätsprüfungen durchzuführen, wenn die Anwendung oder der Cluster nicht fehlerfrei ist.  Standardwert\: PT0H0M0S. |
| --health-check-stable | Die Zeitspanne, während der die Anwendung oder der Cluster fehlerfrei bleiben muss.  Standardwert\: PT0H0M0S. |
| --health-check-wait | Die Zeitspanne, während der nach dem Abschließen einer Upgradedomäne gewartet werden soll, bevor der Integritätsprüfungsprozess gestartet wird.  Standardwert\: PT0H0M0S. |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |
| --unhealthy-applications | Der maximal zulässige Prozentsatz von Anwendungen mit Fehlern während des Upgrades. Zulässige Werte sind ganze Zahlen von 0 bis 100. |
| --unhealthy-nodes | Der maximal zulässige Prozentsatz von Knoten mit Fehlern während des Upgrades. Zulässige Werte sind ganze Zahlen von 0 bis 100. |
| --upgrade-domain-delta-unhealthy-nodes | Der während des Upgrades maximal zulässige Prozentsatz der Integritätsminderung in der Upgradedomäne (Delta). Zulässige Werte sind ganze Zahlen von 0 bis 100. |
| --upgrade-domain-timeout | Das Timeout für die Upgradedomäne.  Standardwert\: PT0H0M0S. |
| --upgrade-timeout | Das Upgradetimeout.  Standardwert\: PT0H0M0S. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: json, jsonc, table, tsv.  Standardwert\: json. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-cluster upgrade-status
Ruft den Upgradestatus der Clusterkonfiguration eines eigenständigen Service Fabric-Clusters ab.

Ruft die Details zum Upgradestatus der Clusterkonfiguration eines eigenständigen Service Fabric-Clusters ab.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: json, jsonc, table, tsv.  Standardwert\: json. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)