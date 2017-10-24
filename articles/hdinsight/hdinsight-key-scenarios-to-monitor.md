---
title: "Überwachen der Clusterleistung – Azure HDInsight | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie die Kapazität und Leistung eines HDInsight-Clusters überwachen."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2017
ms.author: maxluk
ms.openlocfilehash: f2333202cdccc82edea649ff1c295752a414c8b8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-cluster-performance"></a>Überwachen der Clusterleistung

Das Überwachen der Integrität und Leistung eines HDInsight-Clusters ist sehr wichtig, um die maximale Leistung und die bestmögliche Ressourcenverwendung zu erzielen. Die Überwachung kann Sie auch dabei unterstützen, mögliche Fehler bei der Codierung oder Clusterkonfiguration zu beheben.

In den folgenden Abschnitten wird beschrieben, wie Sie das Laden von Clustern, die Effizienz von YARN-Warteschlangen und den Speicherzugriff optimieren.

## <a name="cluster-loading"></a>Laden von Clustern

In Hadoop-Clustern sollte das Laden auf die Knoten des Clusters verteilt werden. Durch diese Verteilung wird verhindert, dass die Verarbeitung von Aufgaben aufgrund von RAM-, CPU- oder Datenträgerressourcen eingeschränkt wird.

Melden Sie sich an der [Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md) an, und wählen Sie dann die Registerkarte **Hosts**, um sich einen allgemeinen Überblick über die Knoten Ihres Clusters und das dazugehörige Ladeverhalten zu verschaffen. Ihre Hosts werden anhand ihrer vollqualifizierten Domänennamen aufgelistet. Der Betriebsstatus jedes Hosts wird mit einer farbigen Integritätsanzeige angegeben:

| Farbe | Beschreibung |
| --- | --- |
| Rot | Mindestens eine Master-Komponente auf dem Host ist ausgefallen. Zeigen Sie mit der Maus darauf, um eine QuickInfo mit den betroffenen Komponenten anzuzeigen. |
| Orange | Mindestens eine Slave-Komponente auf dem Host ist ausgefallen. Zeigen Sie mit der Maus darauf, um eine QuickInfo mit den betroffenen Komponenten anzuzeigen. |
| Gelb | Ambari Server hat seit mehr als drei Minuten keinen Heartbeat vom Host empfangen. |
| Grün | Normaler Ausführungszustand |

Außerdem werden Spalten angezeigt, in denen die Anzahl von Kernen und die RAM-Menge für jeden Host sowie die Datenträgerauslastung und die durchschnittliche Auslastung angezeigt werden.

![Registerkarte „Hosts“](./media/hdinsight-key-scenarios-to-monitor/hosts-tab.png)

Wählen Sie einen beliebigen Hostnamen aus, um eine ausführliche Übersicht über die Komponenten, die auf diesem Host ausgeführt werden, und die dazugehörigen Metriken zu erhalten. Die Metriken werden als auswählbare Zeitachse für CPU-Auslastung, Ladezustand, Datenträgerauslastung, Speicherauslastung, Netzwerkauslastung und Anzahl von Prozessen angezeigt.

![Hostdetails](./media/hdinsight-key-scenarios-to-monitor/host-details.png)

Unter [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md) finden Sie Details zum Festlegen von Warnungen und Anzeigen von Metriken.

## <a name="yarn-queue-configuration"></a>Konfiguration von YARN-Warteschlangen

Hadoop verfügt über verschiedene Dienste, die auf der dazugehörigen verteilten Plattform ausgeführt werden. YARN (Yet Another Resource Negotiator) koordiniert diese Dienste, ordnet Clusterressourcen zu und verwaltet den Zugriff auf ein gemeinsames Dataset.

YARN teilt die beiden Zuständigkeiten von JobTracker – Ressourcenverwaltung und Auftragsplanung/-überwachung – auf zwei Daemons auf: einen globalen ResourceManager und einen anwendungsbezogenen ApplicationMaster (AM).

Der ResourceManager ist ein *reiner Scheduler*, dessen einzige Aufgabe darin besteht, die verfügbaren Ressourcen zwischen allen in Konkurrenz stehenden Anwendungen zu vermitteln. Mit dem ResourceManager wird sichergestellt, dass immer alle Ressourcen genutzt werden, und eine Optimierung anhand von verschiedenen Konstanten vorgenommen wird, z.B. SLAs, Kapazitätsgarantien usw. Der ApplicationMaster führt die Aushandlung für die Ressourcen des ResourceManager durch und arbeitet mit der bzw. den NodeManager-Komponenten zusammen, um die Container und ihre entsprechende Ressourcennutzung auszuführen und zu überwachen.

Wenn mehrere Mandanten einen großen Cluster gemeinsam nutzen, wird auch ein Konkurrenzkampf um die Ressourcen des Clusters ausgetragen. Der CapacityScheduler ist ein austauschbarer Scheduler, der den Ressourcenaustausch unterstützt, indem Anforderungen in Warteschlangen eingereiht werden. Der CapacityScheduler unterstützt auch *hierarchische Warteschlangen*, um sicherzustellen, dass Ressourcen zwischen den Unterwarteschlangen einer Organisation gemeinsam genutzt werden, bevor Warteschlangen anderer Anwendungen kostenlose Ressourcen verwenden dürfen.

Mithilfe von YARN können diesen Warteschlangen Ressourcen zugeordnet werden, und es wird angezeigt, ob Ihre gesamten verfügbaren Ressourcen zugewiesen wurden. Zum Anzeigen von Informationen zu Ihren Warteschlangen melden Sie sich an der Ambari-Webbenutzeroberfläche an und wählen im oberen Menü die Option **YARN Queue Manager**.

![YARN Queue Manager](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager.png)

Auf der Seite „YARN Queue Manager“ wird links eine Liste mit Ihren Warteschlangen und der jeweils zugewiesene Kapazitätsprozentsatz angezeigt.

![Seite mit Details zum YARN Queue Manager](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager-details.png)

Einen ausführlicheren Überblick über Ihre Warteschlangen erhalten Sie, indem Sie im Ambari-Dashboard in der Liste auf der linken Seite den Dienst **YARN** auswählen. Wählen Sie anschließend im Dropdownmenü **Quick Links** (Quicklinks) unter Ihrem aktiven Knoten die Option **ResourceManager UI** (ResourceManager-UI).

![Menülink „ResourceManager UI“](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui-menu.png)

Wählen Sie in der ResourceManager-Benutzeroberfläche im Menü auf der linken Seite die Option **Scheduler**. Unter *Application Queues* (Anwendungswarteschlangen) wird eine Liste mit Ihren Warteschlangen angezeigt. Hier sind die verwendete Kapazität für die einzelnen Warteschlangen, der Grad der Verteilung auf die Warteschlangen und etwaige Ressourceneinschränkungen für Aufträge angegeben.

![Menülink „ResourceManager UI“](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui.png)

## <a name="storage-throttling"></a>Speicherdrosselung

Zu einem Leistungsengpass kann es für einen Cluster auf der Speicherebene kommen. Der Grund für diese Art von Engpass ist meistens eine *Blockierung* der E/A-Vorgänge. Dies passiert, wenn Ihre ausgeführten Aufgaben mehr E/A-Daten senden, als vom Speicherdienst bewältigt werden können. Diese Blockierung führt zu einer Warteschlange mit E/A-Anforderungen, die auf ihre Verarbeitung nach der Verarbeitung der aktuellen E/A-Daten warten. Zu den Blockierungen kommt es aufgrund von *Speicherdrosselung*. Es handelt sich hierbei nicht um ein physisches Limit, sondern um eine Beschränkung durch den Speicherdienst per Vereinbarung zum Servicelevel (SLA). Mit dieser Beschränkung wird sichergestellt, dass kein einzelner Client oder Mandant über ein Monopol in Bezug auf den Dienst verfügt. Durch die Vereinbarung zum Servicelevel ist die Anzahl von E/A-Vorgängen pro Sekunde (IOs per second, IOPS) für Azure Storage beschränkt. Ausführliche Informationen finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Wenn Sie Azure Storage nutzen und Informationen zur Überwachung von speicherbezogenen Problemen suchen, finden Sie diese unter [Microsoft Azure-Speicher: Überwachung, Diagnose und Problembehandlung](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Wenn Sie Azure Data Lake Store (ADLS) als Sicherungsspeicher für Ihren Cluster verwenden, erfolgt die Drosselung bei Ihnen wahrscheinlich aufgrund von Einschränkungen der Bandbreite. Die Drosselung lässt sich in diesem Fall durch Beobachten der Drosselungsfehler in den Taskprotokollen ermitteln. Informationen zu ADLS finden Sie in den folgenden Artikeln jeweils im Abschnitt zur Drosselung für den entsprechenden Dienst:

* [Anleitung für die Leistungsoptimierung für Hive in HDInsight und Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-hive.md)
* [Anleitung für die Leistungsoptimierung für MapReduce in HDInsight und Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-mapreduce.md)
* [Anleitung für die Leistungsoptimierung für Storm in HDInsight und Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-storm.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung und Überwachung für Ihre Cluster finden Sie unter den folgenden Links:

* [Analysieren von HDInsight-Protokollen](hdinsight-debug-jobs.md)
* [Debuggen von Apps mit YARN-Protokollen](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Aktivieren von Heapdumps für Hadoop-Dienste auf Linux-basierten HDInsight-Clustern](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
