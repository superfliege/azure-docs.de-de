---
title: Konfigurieren des Zeitplans für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster – Azure
description: Erfahren Sie, wie der Zeitplan für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster konfiguriert wird.
services: hdinsight
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: ef57608d092c05b30be63a54bb41ba87558eabc3
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55694617"
---
# <a name="os-patching-for-hdinsight"></a>Patchen des Betriebssystems für HDInsight 

> [!IMPORTANT]
> Ubuntu-Images stehen für die Erstellung neuer HDInsight-Cluster innerhalb von 3 Monaten nach der Veröffentlichung zur Verfügung. Seit Januar 2019 werden laufende Cluster **nicht** automatisch gepatcht. Kunden müssen Skriptaktionen oder andere Mechanismen verwenden, um einen laufenden Cluster zu patchen. Neu erstellte Cluster verfügen immer über die aktuellen verfügbaren Updates, einschließlich der neuesten Sicherheitspatches.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Konfigurieren des Zeitplans für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster
Die virtuellen Computer in einem HDInsight-Cluster müssen gelegentlich neu gestartet werden, damit wichtige Sicherheitspatches installiert werden können. 

Mithilfe der in diesem Artikel beschriebenen Skriptaktion können Sie den Zeitplan für Patches des Betriebssystems in folgender Weise ändern:
1. Aktivieren oder Deaktivieren des automatischen Neustarts
2. Festlegen der Häufigkeit von Neustarts (in Tagen zwischen Neustarts)
3. Festlegen des Wochentags, an dem Neustarts vorgenommen werden

> [!NOTE]  
> Diese Skriptaktion funktioniert nur mit Linux-basierten HDInsight-Clustern, die nach dem 1. August 2016 erstellt wurden. Patches werden nur wirksam, wenn die virtuellen Computer neu gestartet werden. 

## <a name="how-to-use-the-script"></a>Verwendung des Skripts 

Zur Verwendung des Skripts benötigen Sie die folgenden Informationen:
1. Skriptspeicherort: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh  HDInsight verwendet diesen URI zum Suchen und Ausführen des Skripts auf allen virtuellen Computern im Cluster.
  
2. Clusterknotentypen, auf die das Skript angewendet wird: Hauptknoten, Arbeitsknoten, Zookeeper. Dieses Skript muss auf alle Knotentypen im Cluster angewendet werden. Wenn es auf einen Knotentyp nicht angewendet wird, verwenden die virtuellen Computer für den betreffenden Knotentyp weiterhin den vorherigen Patchzeitplan.


3.  Parameter: Dieses Skript akzeptiert drei numerische Parameter:

    | Parameter | Definition |
    | --- | --- |
    | Aktivieren/Deaktivieren des automatischen Neustarts |0 oder 1. Der Wert 0 deaktiviert den automatischen Neustart, 1 aktiviert ihn. |
    | Frequency |7 bis 90 (einschließlich). Die Anzahl der Tage, die vor dem Neustart zum Patchen von virtuellen Computern, für die ein Neustart erforderlich ist, gewartet wird. |
    | Wochentag |1 bis 7 (einschließlich). Der Wert 1 gibt an, dass der Neustart an einem Montag stattfinden soll, 7 gibt den Sonntag an. Die Parameter 1 60 2 bewirken beispielsweise einen automatischen Neustart alle 60 Tage (höchstens) am Dienstag. |
    | Persistenz |Wenn Sie eine Skriptaktion auf einen vorhandenen Cluster anwenden, können Sie das Skript als permanent kennzeichnen. Permanente Skripts werden angewendet, wenn dem Cluster durch Skalierungsvorgänge neue Arbeitsknoten hinzugefügt werden. |

> [!NOTE]  
> Sie müssen dieses Skript als permanent kennzeichnen, wenn Sie es auf einen vorhandenen Cluster anwenden. Andernfalls verwenden durch Skalierungsvorgänge erstellte neue Knoten den Standardpatchzeitplan.  Wenn Sie das Skript im Rahmen der Clustererstellung anwenden, ist es automatisch permanent.

## <a name="next-steps"></a>Nächste Schritte

Spezielle Schritte zur Verwendung der Skriptaktion finden Sie in den folgenden Abschnitten in [Anpassen von Linux-basierten HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md):

* [Verwenden einer Skriptaktion während der Clustererstellung](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Anwenden einer Skriptaktion auf einen ausgeführten Cluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
