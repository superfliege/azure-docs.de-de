---
title: Automatisches Skalieren von Azure HDInsight-Clustern (Vorschau)
description: Verwenden des HDInsight-Features „Autoskalierung“ für die automatische Skalierung von Clustern
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: hrasheed
ms.openlocfilehash: 11828b3b056519d0ebe3233f078c6b3f6fc2ea1c
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997494"
---
# <a name="automatically-scale-azure-hdinsight-clusters-preview"></a>Automatisches Skalieren von Azure HDInsight-Clustern (Vorschau)

>[!Important]
>Die Funktion zum automatischen Skalieren von HDInsight ist aktuell als Vorschauversion verfügbar. Senden Sie eine E-Mail an hdiautoscalepm@microsoft.com, um die Autoskalierung für Ihr Abonnement aktivieren zu lassen.

Das Azure HDInsight-Feature „Autoskalierung“ skaliert die Anzahl der Workerknoten in einem Cluster basierend auf der Last innerhalb eines vordefinierten Bereichs automatisch zentral hoch oder herunter. Während der Erstellung eines neuen HDInsight-Clusters kann eine minimale und maximale Anzahl von Workerknoten festgelegt werden. Die automatische Skalierung überwacht dann die Ressourcenanforderungen der Analyselast und skaliert die Anzahl von Workerknoten dann entsprechend zentral hoch oder herunter. Für dieses Feature fallen keine zusätzlichen Gebühren an.

## <a name="getting-started"></a>Erste Schritte

### <a name="create-a-cluster-with-the-azure-portal"></a>Erstellen eines Clusters mit dem Azure-Portal

> [!Note]
> Die automatische Skalierung wird derzeit nur für Azure HDInsight Hive, MapReduce und Spark-Cluster ab Version 3.6 unterstützt.

Um das Feature für die automatische Skalierung zu aktivieren, gehen Sie als Teil des normalen Clustererstellungsvorgangs folgendermaßen vor:

1. Wählen Sie **Benutzerdefiniert (Größe, Einstellungen, Apps)** anstatt **Schnellerfassung** aus.
2. Aktivieren Sie unter **Benutzerdefiniert** in Schritt 5 (**Clustergröße**) das Kontrollkästchen **Autoskalierung von Workerknoten**.
3. Geben Sie die gewünschten Werte für die folgenden Eigenschaften ein:  

    * Anfängliche **Anzahl von Workerknoten**.  
    * **Mindestzahl** von Workerknoten.  
    * **Maximale** Anzahl von Workerknoten.  

![Aktivieren der Option für die automatische Skalierung des Workerknotens](./media/hdinsight-autoscale-clusters/usingAutoscale.png)

Die anfängliche Anzahl der Workerknoten kann vom Mindest- bis zum Höchstwert reichen. Dieser Wert definiert die Anfangsgröße des Clusters bei der Erstellung. Die Mindestzahl der Workerknoten muss größer als 0 (null) sein.

Nachdem Sie den VM-Typ für jeden Knotentyp ausgewählt haben, können Sie den Bereich der geschätzten Kosten für den gesamten Cluster sehen. Sie können diese Einstellungen dann Ihrem Budget anpassen.

Ihr Abonnement verfügt für jede Region über ein Kapazitätskontingent. Die Gesamtzahl der Kerne Ihrer Hauptknoten darf in Kombination mit der maximalen Anzahl von Workerknoten das Kapazitätskontingent nicht überschreiten. Dieses Kontingent ist jedoch eine weiche Grenze. Sie können immer ein Supportticket erstellen, um es problemlos erhöhen zu lassen.

> [!Note]  
> Wenn Sie die gesamte Kernkontingentgrenze überschreiten, informiert Sie eine Fehlermeldung darüber, dass die maximale Knotenzahl die Zahl der verfügbaren Kerne in dieser Region überschritten hat, und Sie werden aufgefordert, eine andere Region auszuwählen oder den Support um Erhöhung des Kontingents zu bitten.

Weitere Informationen zum Erstellen von HDInsight-Clustern mit dem Azure-Portal finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals](hdinsight-hadoop-create-linux-clusters-portal.md).  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Erstellen eines Clusters mithilfe einer Resource Manager-Vorlage

Um einen HDInsight-Cluster mit einer Azure Resource Manager-Vorlage zu erstellen, fügen Sie dem Abschnitt `computeProfile` > `workernode` einen `autoscale`-Knoten mit den Eigenschaften `minInstanceCount` und `maxInstanceCount` hinzu, wie im folgenden JSON-Codeausschnitt gezeigt.

```json
{                            
    "name": "workernode",
    "targetInstanceCount": 4,
    "autoscale": {
        "capacity": {
            "minInstanceCount": 2,
            "maxInstanceCount": 10
        }        
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

Weitere Informationen zum Erstellen von Clustern mit Resource Manager-Vorlagen finden Sie unter [Erstellen von Apache Hadoop-Clustern in HDInsight mithilfe von Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).  

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>Aktivieren und Deaktivieren der Autoskalierung für einen aktuell ausgeführten Cluster

Sie können die Autoskalierung nur für neue HDInsight-Cluster aktivieren oder deaktivieren.

## <a name="monitoring"></a>Überwachung

Sie können den Verlauf des zentralen Hoch- und Herunterskalierens des Clusters als Teil der Clustermetriken anzeigen. Sie können auch alle Skalierungsaktionen des letzten Tags, der letzten Woche oder eines längeren Zeitraums auflisten.

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

### <a name="cluster-scale-up"></a>Zentrales Hochskalieren von Clustern

Wenn die folgenden Bedingungen erkannt werden, fordert die automatische Skalierung ein zentrales Hochskalieren an:

* „CPU insgesamt für ausstehende“ ist länger als drei Minuten größer als die Anzahl der insgesamt freien CPUs.
* „Arbeitsspeicher insgesamt für ausstehende“ ist länger als drei Minuten größer als der insgesamt freie Arbeitsspeicher.

Wir berechnen, dass eine bestimmte Anzahl neuer Workerknoten benötigt wird, um die aktuellen CPU- und Speicheranforderungen zu erfüllen, und geben dann eine zentrale Hochskalierungsanforderung aus, die diese Anzahl neuer Workerknoten hinzufügt.

### <a name="cluster-scale-down"></a>Zentrales Herunterskalieren von Clustern

Wenn die folgenden Bedingungen erkannt werden, fordert die automatische Skalierung ein zentrales Herunterskalieren an:

* „CPU insgesamt für ausstehende“ ist länger als 10 Minuten kleiner als die Anzahl der insgesamt freien CPUs.
* „Arbeitsspeicher insgesamt für ausstehende“ ist länger als 10 Minuten kleiner als der insgesamt freie Arbeitsspeicher.

Basierend auf der Anzahl der AM-Container pro Knoten und der aktuellen CPU- und Arbeitsspeicheranforderungen gibt die Autoskalierung eine Anforderung zum Entfernen einer bestimmten Anzahl von Knoten aus, wobei angegeben wird, welche Knoten potenzielle Kandidaten zum Entfernen sind. Beim zentralen Herunterskalieren wird die Außerbetriebsetzung der Knoten ausgelöst, und diese werden entfernt, nachdem sie vollständig außer Betrieb gesetzt wurden.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über bewährte Methoden für die manuelle Skalierung von Clustern in [Skalieren von HDInsight-Clustern](hdinsight-scaling-best-practices.md)
