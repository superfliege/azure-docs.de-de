---
title: Azure Monitor-Metrik-Explorer
description: Erfahren Sie mehr über neue Funktionen im Azure Monitor-Metrik-Explorer.
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 576a7948d4df7957d934fb4c8888712fa94cb202
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2019
ms.locfileid: "57337073"
---
# <a name="azure-monitor-metrics-explorer"></a>Azure Monitor-Metrik-Explorer

Der Azure Monitor-Metrik-Explorer ist eine Komponente des Microsoft Azure-Portals, die das Zeichnen von Diagrammen, das visuelle Korrelieren von Trends und das Untersuchen von Spitzen und Tiefen in Metrikwerten ermöglicht. Der Metrik-Explorer ist ein wesentlicher Ausgangspunkt, um verschiedenste Leistungs- und Verfügbarkeitsprobleme bei Ihren in Azure gehosteten oder von Azure Monitor-Diensten überwachten Anwendungen und Infrastrukturen zu untersuchen.

## <a name="metrics-in-azure"></a>Metriken in Azure

[Metriken in Azure Monitor](data-collection.md#metrics) sind eine Reihe von Messwerten und Zahlen, die im Lauf der Zeit gesammelt und gespeichert werden. Es gibt Standardmetriken (bzw. sogenannte „Plattformmetriken“) und benutzerdefinierte Metriken. Die Standardmetriken werden Ihnen von der Azure-Plattform selbst bereitgestellt. Standardmetriken stellen die Integritäts- und Nutzungsstatistik Ihrer Azure-Ressourcen dar. Benutzerdefinierte Metriken werden hingegen von Ihren Anwendungen mithilfe der [Application Insights-API für benutzerdefinierte Ereignisse und Metriken](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics), der [Azure-Diagnoseerweiterung (Windows Azure Diagnostics, WAD)](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview) oder durch die [Azure Monitor-REST-API](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-store-custom-rest-api) an Azure gesendet.

## <a name="create-a-new-chart"></a>Erstellen eines neuen Diagramms

1. Öffnen Sie das Azure-Portal.
2. Navigieren Sie zur neuen Registerkarte **Überwachen**, und wählen Sie dann die Option **Metriken** aus.

   ![Abbildung "Metriken"](./media/metrics-charts/00001.png)

3. Der **Metrikselektor** wird automatisch für Sie geöffnet. Wählen Sie eine Ressource aus der Liste aus, um die zugehörigen Metriken anzuzeigen. Nur Ressourcen mit Metriken werden in der Liste angezeigt.

   ![Abbildung "Metriken"](./media/metrics-charts/00002.png)

   > [!NOTE]
   >Wenn Sie mehrere Azure-Abonnements besitzen, ruft der Metrik-Explorer die Ressourcen aller Abonnements ab, die in der Liste unter „Portaleinstellungen“ > „Nach Abonnements filtern“ ausgewählt sind. Um dies zu ändern, klicken Sie oben auf dem Bildschirm unter „Portaleinstellungen“ auf das Zahnradsymbol, und wählen Sie die Abonnements aus, die Sie verwenden möchten.

4. Für einige Ressourcentypen (Speicherkonten und virtuelle Computer) müssen Sie vor der Auswahl einer Metrik einen **Namespace** auswählen. Jeder Namespace ist mit einer eigenen Gruppe von Metriken ausgestattet, die nur für den jeweiligen Namespace und nicht für andere Namespaces relevant sind.

   Beispielsweise weist jede Azure Storage-Instanz Metriken für die Subdienste „Blobs“, „Dateien“, „Warteschlangen“ und „Tabellen“ auf, die alle zum Speicherkonto gehören. Die Metrik „Anzahl der Warteschlangenmeldungen“ gilt jedoch folgerichtig für den Subdienst „Warteschlange“ und nicht für andere Subdienste des Speicherkontos.

   ![Abbildung "Metriken"](./media/metrics-charts/00003.png)

5. Wählen Sie in der Liste eine Metrik aus. Wenn Sie einen Teil des Namens der gewünschten Metrik kennen, können Sie diesen bereits eingeben. Daraufhin wird eine gefilterte Liste der verfügbaren Metriken angezeigt:

   ![Abbildung "Metriken"](./media/metrics-charts/00004.png)

6. Nach der Auswahl einer Metrik wird das Diagramm mit der Standardaggregation für die ausgewählte Metrik gerendert. An dieser Stelle können Sie einfach außerhalb des **Metrikselektors** klicken, um ihn zu schließen. Optional können Sie auch in eine andere Aggregation des Diagramms wechseln. Bei einigen Metriken können Sie beim Wechseln der Aggregation auswählen, welcher Wert im Diagramm angezeigt werden soll. Beispielsweise können Sie zwischen den durchschnittlichen, minimalen und maximalen Werten wechseln. 

7. Durch Klicken auf **Metrik hinzufügen** und Wiederholen der Schritte 3 bis 6 können Sie weitere Metriken in diesem Diagramm hinzufügen.

   > [!NOTE]
   > In der Regel wird davon abgeraten, in einem Diagramm Metriken mit unterschiedlichen Maßeinheiten (d.h. „Millisekunden“ und „Kilobytes“) oder deutlich abweichender Skala zu verwenden. In diesem Fall sollten Sie vielmehr verschiedene Diagramme verwenden. Klicken Sie auf die Schaltfläche „Diagramm hinzufügen“, um mehrere Diagramme im Metrik-Explorer zu erstellen.

## <a name="apply-filters-to-charts"></a>Anwenden von Filtern auf Diagramme

Sie können Filter auf Diagramme anwenden, die Metriken mit Dimensionen aufweisen. Nehmen wir beispielsweise an, die Metrik „Anzahl von Transaktionen“ weist eine Dimension mit dem Namen „Antworttyp“ auf, die angibt, ob die Antwort von Transaktionen erfolgreich war oder nicht. Durch Filtern nach dieser Dimension wird dann eine Diagrammzeile gezeichnet, die nur erfolgreiche (oder nur fehlerhafte) Transaktionen anzeigt. 

### <a name="to-add-a-filter"></a>So fügen Sie einen Filter hinzu

1. Wählen Sie oberhalb des Diagramms **Filter hinzufügen** aus.

2. Wählen Sie aus, nach welcher Dimension (Eigenschaft) gefiltert werden soll.

   ![Abbildung eines Diagramms mit Metriken](./media/metrics-charts/00006.png)

3. Wählen Sie beim Zeichnen des Diagramms aus, welche Dimensionswerte eingeschlossen werden sollen (in diesem Beispiel werden die Filterergebnisse erfolgreicher Speichertransaktionen gezeigt):

   ![Abbildung eines Diagramms mit Metriken](./media/metrics-charts/00007.png)

4. Klicken Sie nach der Auswahl der Filterwerte außerhalb des Filterselektors, um ihn zu schließen. Nun zeigt das Diagramm an, wie viele Speichertransaktionen fehlerhaft sind:

   ![Abbildung eines Diagramms mit Metriken](./media/metrics-charts/00008.png)

5. Sie können die Schritte 1 bis 4 wiederholen, um mehrere Filter auf dieselben Diagramme anzuwenden.

## <a name="apply-splitting-to-a-chart"></a>Anwenden des Teilens auf ein Diagramm

Sie können eine Metrik nach Dimension unterteilen, um die verschiedenen Segmente der Metrik gegenüberzustellen und die äußeren Segmente einer Dimension zu identifizieren. 

### <a name="to-apply-splitting"></a>So wenden Sie das Teilen an

1. Klicken Sie oberhalb des Diagramms auf **Teilung anwenden**.
 
   > [!NOTE]
   > Das Teilen kann nicht bei Diagrammen verwendet werden, die mehrere Metriken aufweisen. Auch können Sie zwar mehrere Filter anwenden, jedoch kann nur eine Teilungsdimension auf ein beliebiges einzelnes Diagramm angewendet werden.

2. Wählen Sie eine Dimension, in der Ihr Diagramm segmentiert werden soll:

   ![Abbildung eines Diagramms mit Metriken](./media/metrics-charts/00010.png)

   Das Diagramm zeigt nun mehrere Zeilen an, eine Zeile für jedes Segment der Dimension:

   ![Abbildung eines Diagramms mit Metriken](./media/metrics-charts/00012.png)

3. Klicken Sie außerhalb des **Gruppierungsselektors**, um ihn zu schließen.

   > [!NOTE]
   > Blenden Sie in einer Dimension durch Filtern und Teilen Segmente aus, die für Ihr Szenario nicht relevant sind, und stellen Sie eine bessere Übersichtlichkeit der Diagramme sicher.

## <a name="lock-boundaries-of-chart-y-axis"></a>Festlegen der Grenzen der Y-Achse des Diagramms

Wenn das Diagramm geringfügige Schwankungen größerer Werte enthält, ist es wichtig, den Bereich der Y-Achse festzulegen. 

Ein Beispiel: Wenn die Menge an erfolgreichen Anforderungen von 99,99 Prozent auf 99,5 Prozent fällt, bedeutet das unter Umständen eine deutliche Verschlechterung der Servicequalität. Bei Verwendung der Standardeinstellungen des Diagramms ist eine solche geringfügige Schwankung jedoch nur sehr schwer oder sogar überhaupt nicht erkennbar. In diesem Fall können Sie die Untergrenze des Diagramms auf 99 Prozent festlegen, wodurch ein geringfügiger Rückgang leichter zu erkennen ist. 

Ein weiteres Beispiel wären Schwankungen beim verfügbaren Arbeitsspeicher, bei denen der Wert aus technischen Gründen niemals „0“ erreicht. Wenn Sie hier den Bereich auf einen höheren Wert festlegen, ist eine geringere Arbeitsspeicherverfügbarkeit leichter zu erkennen. 

Verwenden Sie zum Steuern des Bereichs der Y-Achse das Diagrammmenü „...“, und wählen Sie **Diagramm bearbeiten** aus, um auf die erweiterten Diagrammeinstellungen zuzugreifen. Ändern Sie die Werte im Abschnitt für den Bereich der Y-Achse, oder verwenden Sie die Schaltfläche **Automatisch**, um die Standardwerte wiederherzustellen.

![Abbildung eines Diagramms mit Metriken](./media/metrics-charts/00014-manually-set-granularity.png)

> [!WARNING]
> Wenn Sie die Grenzen der Y-Achse für Diagramme festlegen möchten, die verschiedene Anzahlwerte oder Summen für einen Zeitraum nachverfolgen und somit Anzahl-, Summen-, Minimum- oder Maximum-Aggregationen verwenden, muss in der Regel anstelle der automatischen Standardwerte auch eine feste Zeitgranularität angegeben werden. Dies ist erforderlich, da sich die Werte in Diagrammen ändern, wenn der Benutzer die Größe des Browserfensters oder die Bildschirmauflösung ändert, was eine automatische Änderung der Zeitgranularität zur Folge hat. Die resultierende Änderung der Zeitgranularität wirkt sich auf die Darstellung des Diagramms aus und macht die aktuelle Auswahl des Y-Achsenbereichs ungültig.

## <a name="pin-charts-to-dashboards"></a>Anheften von Diagrammen an Dashboards

Nach der Konfiguration der Diagramme sollten Sie sie zu Dashboards hinzufügen, um sie – beispielsweise im Kontext einer anderen Überwachungstelemetrie – erneut anzeigen zu können oder Ihrem Team zur Verfügung zu stellen.

So heften Sie ein konfiguriertes Diagramm an ein Dashboard an

Klicken Sie nach der Konfiguration Ihres Diagramms rechts oben im Diagramm auf das Menü **Diagrammaktionen** und dann auf **An Dashboard anheften**.

![Abbildung eines Diagramms mit Metriken](./media/metrics-charts/00013.png)

## <a name="create-alert-rules"></a>Erstellen von Warnungsregeln

Sie können die Kriterien, die Sie zum Visualisieren Ihrer Metriken festgelegt haben, als Grundlage für eine Metrik basierend auf einer Warnungsregel verwenden. Die neue Warnungsregel enthält Ihre Dimensionen für Zielressource, Metrik, Teilung und Filter aus dem Diagramm. Sie können diese Einstellungen später im Erstellungsbereich für Warnungsregeln ändern.

### <a name="to-create-a-new-alert-rule-click-new-alert-rule"></a>Zum Erstellen einer neuen Warnungsregel klicken Sie auf **Neue Warnungsregel**.

![Schaltfläche „Neue Warnungsregel“ rot hervorgehoben](./media/metrics-charts/015.png)

Sie wechseln in den Bereich zum Erstellen von Warnungsregeln, in dem die zugrunde liegenden Metrikdimensionen aus dem Diagramm vorab ausgefüllt wurden, um das Generieren benutzerdefinierter Warnungsregeln zu vereinfachen.

![Erstellen einer Warnungsregel](./media/metrics-charts/016.png)

Lesen Sie [diesen Artikel](alerts-metric.md), um mehr über das Einrichten von Metrikwarnungen zu erfahren.

## <a name="troubleshooting"></a>Problembehandlung

*In meinem Diagramm werden keine Daten angezeigt.*

* Filter gelten für alle Diagramme im Bereich. Stellen Sie sicher, dass Sie bei der Arbeit mit einem Diagramm keinen Filter festgelegt haben, mit dem alle Daten eines anderen Diagramms ausgeschlossen werden.

* Wenn Sie verschiedene Filter für verschiedene Diagramme festlegen möchten, sollten Sie diese in unterschiedlichen Blättern erstellen und als separate Favoriten speichern. Sie können sie auch im Dashboard anheften, damit sie nebeneinander angezeigt werden.

* Wenn Sie ein Diagramm nach einer Eigenschaft segmentieren, die nicht in der Metrik definiert ist, sind im Diagramm keine Daten enthalten. Versuchen Sie, die Segmentierung (Aufteilung) zu löschen, oder wählen Sie eine andere Eigenschaft aus.

## <a name="next-steps"></a>Nächste Schritte

  Lesen Sie [Erstellen von benutzerdefinierten KPI-Dashboards](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards), um mehr über die Best Practices zum Erstellen von Dashboards mit ausführbaren Aktionen und Metriken zu erfahren.

