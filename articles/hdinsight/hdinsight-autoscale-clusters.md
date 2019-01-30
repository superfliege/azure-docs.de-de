---
title: Automatisches Skalieren von Azure HDInsight-Clustern
description: Verwenden des HDInsight-Features „Autoskalierung“ für die automatische Skalierung von Clustern
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: hrasheed
ms.openlocfilehash: 043c83e2039d87b1650ba17f770ce16a2ad2c13d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811161"
---
# <a name="automatically-scale-azure-hdinsight-clusters"></a>Automatisches Skalieren von Azure HDInsight-Clustern

Das Azure HDInsight-Feature „Autoskalierung“ skaliert die Anzahl der Workerknoten in einem Cluster basierend auf der Last innerhalb eines vordefinierten Bereichs automatisch zentral hoch oder herunter. Während der Erstellung eines neuen HDInsight-Clusters kann eine minimale und maximale Anzahl von Workerknoten festgelegt werden. Die automatische Skalierung überwacht dann die Ressourcenanforderungen der Analyselast und skaliert die Anzahl von Workerknoten dann entsprechend zentral hoch oder herunter. Für dieses Feature fallen keine zusätzlichen Gebühren an.

## <a name="getting-started"></a>Erste Schritte

### <a name="create-cluster-with-azure-portal"></a>Erstellen des Clusters mit dem Azure-Portal

> [!Note]
> Die automatische Skalierung wird derzeit nur für Azure HDInsight Hive, MapReduce und Spark-Cluster ab Version 3.6 unterstützt.

Führen Sie die Schritte in [Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals](hdinsight-hadoop-create-linux-clusters-portal.md) aus, und wenn Sie Schritt 5, **Clustergröße**, erreichen, wählen Sie die Option **Autoskalierung von Workerknoten (Vorschau)** wie unten dargestellt aus. 

![Aktivieren der Option für die automatische Skalierung des Workerknotens](./media/hdinsight-autoscale-clusters/worker-node-autoscale-option.png)

Durch Aktivieren dieser Option können Sie Folgendes angeben:

* Die anfängliche Anzahl der Workerknoten
* Die Mindestzahl der Workerknoten
* Die Höchstzahl der Workerknoten

Die anfängliche Anzahl der Workerknoten kann vom Mindest- bis zum Höchstwert reichen. Dieser Wert definiert die Anfangsgröße des Clusters bei der Erstellung. Die Mindestzahl der Workerknoten muss größer als 0 (null) sein.

Nachdem Sie den VM-Typ für jeden Knotentyp ausgewählt haben, können Sie den Bereich der geschätzten Kosten für den gesamten Cluster sehen. Sie können diese Einstellungen dann Ihrem Budget anpassen.

Ihr Abonnement verfügt für jede Region über ein Kapazitätskontingent. Die Gesamtzahl der Kerne Ihrer Hauptknoten darf in Kombination mit der maximalen Anzahl von Workerknoten das Kapazitätskontingent nicht überschreiten. Dieses Kontingent ist jedoch eine weiche Grenze. Sie können immer ein Supportticket erstellen, um es problemlos erhöhen zu lassen.

> [!Note]
> Wenn Sie die gesamte Kernkontingentgrenze überschreiten, informiert Sie eine Fehlermeldung darüber, dass die maximale Knotenzahl die Zahl der verfügbaren Kerne in dieser Region überschritten hat, und Sie werden aufgefordert, eine andere Region auszuwählen oder den Support um Erhöhung des Kontingents zu bitten.

### <a name="create-cluster-with-an-resource-manager-template"></a>Erstellen von Clustern mithilfe einer Resource Manager-Vorlage

Wenn Sie einen HDInsight-Cluster mit einer Resource Manager-Vorlage erstellen, müssen Sie die folgenden Einstellungen im Abschnitt „workerknoten“ von „ComputeProfile“ hinzufügen:

```json
{                            
    "name": "workernode",
    "targetInstanceCount": 4,
    "autoscale": {
        "minInstanceCount": 2,
        "maxInstanceCount": 10
    },
    "hardwareProfile": {
        "vmSize": "Standard_D13_V2"
    },
    "osProfile": {
        "linuxOperatingSystemProfile": {
            "username": "[parameters('sshUserName')]",
            "password": "[parameters('sshPassword')]"
        }
    },
    "virtualNetworkProfile": null,
    "scriptActions": []
}
```

### <a name="enable-and-disabling-autoscale-for-a-running-cluster"></a>Aktivieren und Deaktivieren der automatischen Skalierung für einen ausgeführten Cluster

Aktivieren der automatischen Skalierung für einen ausgeführten Cluster wird in der privaten Vorschau nicht unterstützt. Sie muss während der Clustererstellung aktiviert werden.

Deaktivieren der automatischen Skalierung oder Änderung der Einstellungen der automatischen Skalierung für einen ausgeführten Cluster werden in der privaten Vorschau nicht unterstützt. Sie müssen den Cluster löschen und einen neuen Cluster erstellen, um die Einstellungen zu löschen oder zu ändern.

## <a name="monitoring"></a>Überwachung

Sie können den Verlauf des zentralen Hoch- und Herunterskalierens des Clusters als Teil der Clustermetriken anzeigen. Sie können alle Skalierungsaktionen des letzten Tags, der letzten Woche oder eines längeren Zeitraums auflisten.

## <a name="how-it-works"></a>So funktioniert's

### <a name="metrics-monitoring"></a>Überwachen von Metriken

Die automatische Skalierung überwacht kontinuierlich die Cluster und sammelt die folgenden Metriken:

1. **CPU insgesamt für ausstehende**: Die Gesamtanzahl von Kernen, die zum Starten der Ausführung aller ausstehenden Container erforderlich sind.
2. **Arbeitsspeicher insgesamt für ausstehende**: Die Gesamtgröße des Arbeitsspeichers (in MB), die zum Starten der Ausführung aller ausstehenden Container erforderlich ist.
3. **Freie CPUs insgesamt**: Die Summe aller nicht verwendeten Kerne auf den aktiven Workerknoten.
4. **Freier Arbeitsspeicher insgesamt**: Die Summe aller nicht verwendeten Kerne (in MB) auf den aktiven Workerknoten.
5. **Verwendeter Arbeitsspeicher pro Knoten**: Die Last auf einem Workerknoten. Ein Workerknoten, auf dem 10GB Arbeitsspeicher verwendet werden, ist höher ausgelastet als ein Workerknoten, auf dem 2GB verwendet werden.
6. **Anzahl der Anwendungsmaster pro Knoten**: Die Anzahl der Anwendungsmastercontainer (Application Master, AM), die auf einem Workerknoten ausgeführt werden. Ein Workerknoten, der 2 AM-Container hostet, gilt als wichtiger als ein Workerknoten, der 0 AM-Container hostet.

Die oben aufgeführten Metriken werden alle 60 Sekunden überprüft. Die automatische Skalierung trifft auf Basis dieser Metriken Entscheidungen zum zentralen Hoch- und Herunterskalieren.

### <a name="cluster-scale-up"></a>Cluster zentral hochskalieren

Wenn die folgenden Bedingungen erkannt werden, fordert die automatische Skalierung ein zentrales Hochskalieren an:

* „CPU insgesamt für ausstehende“ ist länger als eine Minute größer als die Anzahl der insgesamt freien CPUs.
* „Arbeitsspeicher insgesamt für ausstehende“ ist länger als 1 Minute größer als der insgesamt freie Arbeitsspeicher.

Wir berechnen, dass N neue Workerknoten erforderlich sind, um die aktuellen CPU- und Arbeitsspeicheranforderungen zu erfüllen und dann durch Anfordern von N neuen Workerknoten eine Anforderung zum zentralen Hochskalieren auszugeben.

### <a name="cluster-scale-down"></a>Cluster zentral herunterskalieren

Wenn die folgenden Bedingungen erkannt werden, fordert die automatische Skalierung ein zentrales Herunterskalieren an:

* „CPU insgesamt für ausstehende“ ist länger als 10 Minuten kleiner als die Anzahl der insgesamt freien CPUs.
* „Arbeitsspeicher insgesamt für ausstehende“ ist länger als 10 Minuten kleiner als der insgesamt freie Arbeitsspeicher.

Basierend auf der Anzahl der AM-Container pro Knoten als auch der aktuellen CPU- und Arbeitsspeicheranforderungen gibt die automatische Skalierung eine Anforderung zum Entfernen von N Knoten aus, wobei angegeben wird, welche Knoten potenzielle Kandidaten zum Entfernen sind. Standardmäßig werden zwei Knoten in einem Zyklus entfernt.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über bewährte Methoden für die manuelle Skalierung von Clustern in [Skalieren von HDInsight-Clustern](hdinsight-scaling-best-practices.md)
