---
title: Azure Monitor-Metrik-Explorer
description: Erfahren Sie mehr über neue Funktionen im Azure Monitor-Metrik-Explorer.
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2017
ms.author: vitaly.gorbenko
ms.component: metrics
ms.openlocfilehash: 21b0029ff12915c8416ad2366fbf6c45ddfaa288
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978418"
---
# <a name="azure-monitor-metrics-explorer"></a>Azure Monitor-Metrik-Explorer

In dieser Vorgehensweise wird die Diagrammerstellungsoberfläche des Azure Monitor-Metrik-Explorers der nächsten Generation beschrieben, die zurzeit als öffentliche Vorschau verfügbar ist. Die neue Oberfläche unterstützt das Rendern von Diagrammen für mehrdimensionale und grundlegende Metriken ohne Dimensionen. Sie können Diagramme zeichnen, die Metriken aus unterschiedlichen Ressourcentypen, mehreren Ressourcengruppen und Abonnements überlagern. Mehrdimensionale Metrikdiagramme können durch Anwendung von Dimensionsfiltern sowie Gruppierung angepasst werden. Es können beliebige Diagramme, einschließlich benutzerdefinierter Diagramme, an Dashboards angeheftet werden.

Informationen zur alten Benutzeroberfläche, die lediglich grundlegende Metriken ohne Dimensionen unterstützt, finden Sie im Abschnitt „Zugriff auf Metriken über das Portal“ im [Handbuch mit der Übersicht über Microsoft Azure-Metriken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

## <a name="what-is-azure-monitor-metrics-explorer"></a>Was ist der Azure Monitor-Metrik-Explorer?

Der Azure Monitor-Metrik-Explorer ist eine Komponente des Microsoft Azure-Portals, die das Zeichnen von Diagrammen, das visuelle Korrelieren von Trends und das Untersuchen von Spitzen und Tiefen in Metrikwerten ermöglicht. Der Metrik-Explorer ist ein wesentlicher Ausgangspunkt, um verschiedenste Leistungs- und Verfügbarkeitsprobleme bei Ihren in Azure gehosteten oder von Azure Monitor-Diensten überwachten Anwendungen und Infrastrukturen zu untersuchen. 

## <a name="what-are-metrics-in-azure"></a>Was stellen Metriken in Azure dar?

Metriken in Microsoft Azure sind eine Reihe von gemessenen Werten und Zahlen, die im Laufe der Zeit gesammelt und gespeichert werden. Es gibt Standardmetriken (bzw. sogenannte „Plattformmetriken“) und benutzerdefinierte Metriken. Die Standardmetriken werden Ihnen von der Azure-Plattform selbst bereitgestellt. Standardmetriken stellen die Integritäts- und Nutzungsstatistik Ihrer Azure-Ressourcen dar. Benutzerdefinierte Metriken hingegen werden von Ihren Anwendungen mithilfe der [Application Insights-API für benutzerdefinierte Ereignisse](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics) an Azure gesendet. Zusammen mit anderen anwendungsspezifischen Metriken werden benutzerdefinierte Metriken in den Application Insights-Ressourcen gespeichert.



## <a name="how-do-i-create-a-new-chart"></a>Wie erstelle ich ein neues Diagramm?

   > [!NOTE]
   > Einige der Funktionen der alten Oberfläche „Metriken“ sind noch nicht im neuen Metrik-Explorer verfügbar. Solange die neue Oberfläche in der Vorschauversion verfügbar ist, können Sie weiterhin die alte (nichtdimensionale) Ansicht „Metriken“ von Azure Monitor verwenden. 

1. Öffnen Sie das Azure-Portal.
2. Navigieren Sie zur neuen Registerkarte **Überwachen**, und wählen Sie dann die Option **Metriken (Vorschau)** aus.

   ![Abbildung der Ansicht „Metriken (Vorschau)“](./media/monitoring-metric-charts/0001.png)

3. Der **Metrikselektor** wird automatisch für Sie geöffnet. Wählen Sie eine Ressource aus der Liste aus, um die zugehörigen Metriken anzuzeigen. Nur Ressourcen mit Metriken werden in der Liste angezeigt.

   ![Abbildung der Ansicht „Metriken (Vorschau)“](./media/monitoring-metric-charts/0002.png)

   > [!NOTE]
   >Wenn Sie mehrere Azure-Abonnements besitzen, ruft der Metrik-Explorer die Ressourcen aller Abonnements ab, die in der Liste unter „Portaleinstellungen“ > „Nach Abonnements filtern“ ausgewählt sind. Um dies zu ändern, klicken Sie oben auf dem Bildschirm unter „Portaleinstellungen“ auf das Zahnradsymbol, und wählen Sie die Abonnements aus, die Sie verwenden möchten.

4. Für einige Ressourcentypen (d. h. Speicherkonten und virtuelle Computer) müssen Sie vor der Auswahl einer Metrik einen **Namespace** auswählen. Jeder Namespace ist mit einer eigenen Gruppe von Metriken ausgestattet, die nur für den jeweiligen Namespace und nicht für andere Namespaces relevant sind.

   Beispielsweise weist jede Azure Storage-Instanz Metriken für die Subdienste „Blobs“, „Dateien“, „Warteschlangen“ und „Tabellen“ auf, die alle zum Speicherkonto gehören. Die Metrik „Anzahl der Warteschlangenmeldungen“ gilt jedoch folgerichtig für den Subdienst „Warteschlange“ und nicht für andere Subdienste des Speicherkontos.

   ![Abbildung der Ansicht „Metriken (Vorschau)“](./media/monitoring-metric-charts/0003.png)

5. Wählen Sie in der Liste eine Metrik aus. Wenn Sie einen Teil des Namens der gewünschten Metrik kennen, können Sie diesen bereits eingeben. Daraufhin wird eine gefilterte Liste der verfügbaren Metriken angezeigt:

   ![Abbildung der Ansicht „Metriken (Vorschau)“](./media/monitoring-metric-charts/0004.png)

6. Nach der Auswahl einer Metrik wird das Diagramm mit der Standardaggregation für die ausgewählte Metrik gerendert. An dieser Stelle können Sie einfach außerhalb des **Metrikselektors** klicken, um ihn zu schließen. Optional können Sie auch in eine andere Aggregation des Diagramms wechseln. Bei einigen Metriken können Sie beim Wechseln der Aggregation auswählen, welcher Wert im Diagramm angezeigt werden soll. Beispielsweise können Sie zwischen den durchschnittlichen, minimalen und maximalen Werten wechseln. 

7. Durch Klicken auf das Symbol „Metrik hinzufügen“ ![Metriksymbol](./media/monitoring-metric-charts/icon001.png) und Wiederholen der Schritte 3 bis 6 können Sie weitere Metriken in diesem Diagramm hinzufügen.

   > [!NOTE]
   > In der Regel wird davon abgeraten, in einem Diagramm Metriken mit unterschiedlichen Maßeinheiten (d.h. „Millisekunden“ und „Kilobytes“) oder deutlich abweichender Skala zu verwenden. In diesem Fall sollten Sie vielmehr verschiedene Diagramme verwenden. Klicken Sie auf die Schaltfläche „Diagramm hinzufügen“, um mehrere Diagramme im Metrik-Explorer zu erstellen.

## <a name="how-do-i-apply-filters-to-the-charts"></a>Wie werden Filter auf die Diagramme angewendet?

Sie können Filter auf Diagramme anwenden, die Metriken mit Dimensionen aufweisen. Nehmen wir beispielsweise an, die Metrik „Anzahl von Transaktionen“ weist eine Dimension mit dem Namen „Antworttyp“ auf, die angibt, ob die Antwort von Transaktionen erfolgreich war oder nicht. Durch Filtern nach dieser Dimension wird dann eine Diagrammzeile gezeichnet, die nur erfolgreiche (oder nur fehlerhafte) Transaktionen anzeigt. 

### <a name="to-add-a-filter"></a>So fügen Sie einen Filter hinzu

1. Klicken Sie auf das Symbol „Filter hinzufügen“ ![Filtersymbol](./media/monitoring-metric-charts/icon002.png) oben im Diagramm.

2. Wählen Sie aus, nach welcher Dimension (Eigenschaft) gefiltert werden soll.

   ![Abbildung eines Diagramms mit Metriken](./media/monitoring-metric-charts/0006.png)

3. Wählen Sie beim Zeichnen des Diagramms aus, welche Dimensionswerte eingeschlossen werden sollen (in diesem Beispiel werden die Filterergebnisse erfolgreicher Speichertransaktionen gezeigt):

   ![Abbildung eines Diagramms mit Metriken](./media/monitoring-metric-charts/0007.png)

4. Klicken Sie nach der Auswahl der Filterwerte außerhalb des Filterselektors, um ihn zu schließen. Nun zeigt das Diagramm an, wie viele Speichertransaktionen fehlerhaft sind:

   ![Abbildung eines Diagramms mit Metriken](./media/monitoring-metric-charts/0008.png)

5. Sie können die Schritte 1 bis 4 wiederholen, um mehrere Filter auf dieselben Diagramme anzuwenden.

## <a name="how-do-i-segment-a-chart"></a>Wie segmentiere ich ein Diagramm?

Sie können eine Metrik nach Dimension unterteilen, um die verschiedenen Segmente der Metrik gegenüberzustellen und die äußeren Segmente einer Dimension zu identifizieren. 

### <a name="to-segment-a-chart"></a>So segmentieren Sie ein Diagramm

1. Klicken Sie auf das Symbol „Gruppierung hinzufügen“  ![Abbildung eines Diagramms mit Metriken](./media/monitoring-metric-charts/icon003.png) oben im Diagramm.
 
   > [!NOTE]
   > Sie können in einem Diagramm zwar mehrere Filter anwenden, jedoch nur eine Gruppierung.

2. Wählen Sie eine Dimension, in der Ihr Diagramm segmentiert werden soll: 

   ![Abbildung eines Diagramms mit Metriken](./media/monitoring-metric-charts/0010.png)

   Das Diagramm zeigt nun mehrere Zeilen an, eine Zeile für jedes Segment der Dimension:

   ![Abbildung eines Diagramms mit Metriken](./media/monitoring-metric-charts/0012.png)

3. Klicken Sie außerhalb des **Gruppierungsselektors**, um ihn zu schließen.

   > [!NOTE]
   > Blenden Sie in einer Dimension durch Filtern und Gruppieren Segmente aus, die für Ihr Szenario nicht relevant sind, und stellen Sie eine bessere Übersichtlichkeit der Diagramme sicher.

## <a name="how-do-i-pin-charts-to-dashboards"></a>Wie hefte ich Diagramme an Dashboards an?

Nach der Konfiguration der Diagramme sollten Sie sie zu Dashboards hinzufügen, um sie – beispielsweise im Kontext einer anderen Überwachungstelemetrie – erneut anzeigen zu können oder Ihrem Team zur Verfügung zu stellen. 

So heften Sie ein konfiguriertes Diagramm an ein Dashboard an

Klicken Sie nach der Konfiguration Ihres Diagramms rechts oben im Diagramm auf das Menü **Diagrammaktionen** und dann auf **An Dashboard anheften**.

   ![Abbildung eines Diagramms mit Metriken](./media/monitoring-metric-charts/0013.png)

## <a name="next-steps"></a>Nächste Schritte

  Lesen Sie [Erstellen von benutzerdefinierten KPI-Dashboards](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards), um mehr über die Best Practices zum Erstellen von Dashboards mit ausführbaren Aktionen und Metriken zu erfahren.