---
title: Grundlagen der Datenaufbewahrung in der Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: In diesem Artikel werden zwei Einstellungen beschrieben, mit denen die Datenaufbewahrung in Ihrer Azure Time Series Insights-Umgebung gesteuert wird.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/09/2018
ms.openlocfilehash: e265a66b841530d1133d760ebdcdf56046d1aee1
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364126"
---
# <a name="understand-data-retention-in-time-series-insights"></a>Grundlagen der Datenaufbewahrung in Time Series Insights

In diesem Artikel werden zwei Einstellungen beschrieben, die sich auf die Datenaufbewahrung in Ihrer TSI-Umgebung (Time Series Insights) auswirken.

## <a name="video"></a>Video: 

### <a name="in-this-video-we-cover-time-series-insights-data-retention-and-how-to-plan-for-itbr"></a>In diesem Video erfahren Sie mehr über die Datenspeicherung mit Time Series Insights und, wie Sie diese planen.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

Jede TSI-Umgebung verfügt über eine Einstellung, mit der die **Datenaufbewahrungszeit**  gesteuert wird. Der Wert kann von 1 bis 400 Tagen reichen. Die Daten werden anhand der Speicherkapazität der Umgebung oder der Aufbewahrungsdauer (1 - 400) gelöscht, je nachdem, welcher Fall zuerst eintritt.

Jede TSI-Umgebung verfügt über die zusätzliche Einstellung **Verhalten bei Überschreitung des Speicherlimits**. Diese Einstellung steuert das Verhalten bei Eingang und Bereinigung, wenn in einer Umgebung die maximale Kapazität erreicht ist. Sie können zwischen zwei Verhaltensweisen wählen:
- **Alte Daten bereinigen** (Standard)  
- **Eingang anhalten**

> [!NOTE]
> Bei der Erstellung einer neuen Umgebung wird für die Aufbewahrung standardmäßig **Purge old data** (Alte Daten bereinigen) konfiguriert. Diese Einstellung kann nach der Erstellung im Azure-Portal auf der Seite **Konfigurieren** der TSI-Umgebung je nach Bedarf geändert werden.

Informationen zum Wechseln des Aufbewahrungsverhaltens finden Sie unter [Konfigurieren der Vermerkdauer in Time Series Insights](time-series-insights-how-to-configure-retention.md).

Vergleichen Sie das Verhalten in Bezug auf die Datenaufbewahrung:

## <a name="purge-old-data"></a>Purge old data (Alte Daten bereinigen)
- Dieses Verhalten ist das Standardverhalten für TSI-Umgebungen, das sich seit der Einführung der öffentlichen Vorschauversion von TSI-Umgebungen nicht geändert hat.  
- Das Verhalten sollte gewählt werden, wenn Benutzer in ihrer TSI-Umgebung immer die *aktuellen Daten* angezeigt bekommen möchten. 
- Daten werden hierbei *bereinigt*, wenn die Grenzwerte der Umgebung erreicht sind (Aufbewahrungsdauer, Größe oder Anzahl, je nachdem, was zuerst eintritt). Die Aufbewahrungsdauer ist standardmäßig auf 30 Tage festgelegt. 
- Die ältesten erfassten Daten werden zuerst bereinigt (FIFO-Ansatz).

### <a name="example-1"></a>Beispiel 1:
Wir sehen uns eine Beispielanwendung mit dem Aufbewahrungsverhalten **Continue ingress and purge old data** (Eingang fortsetzen und alte Daten bereinigen) an: In diesem Beispiel ist die **Datenaufbewahrungszeit** auf 400 Tage festgelegt. Die **Kapazität** ist auf die S1-Einheit festgelegt. Dies entspricht einer Gesamtkapazität von 30 GB.   Angenommen, jeden Tag fallen durchschnittlich 500 MB an eingehenden Daten an. In dieser Umgebung können bei dieser Eingangsrate nur Daten für 60 Tage aufbewahrt werden, da nach 60 Tagen die maximale Kapazität erreicht ist. Für die eingehenden Daten gilt Folgendes: 500 MB pro Tag x 60 Tage = 30 GB. 

In diesem Beispiel werden in der Umgebung am 61. Tag die aktuellen Daten angezeigt, aber Daten, die älter als 60 Tage sind, werden bereinigt. Durch die Bereinigung wird Platz für die neu eintreffenden Daten geschaffen, damit weiter neue Daten untersucht werden können. 

Falls Benutzer die Daten länger aufbewahren möchten, können sie die Größe der Umgebung erhöhen, indem sie zusätzliche Einheiten hinzufügen oder weniger Daten per Pushvorgang übertragen.  

### <a name="example-2"></a>Beispiel 2:
Wir sehen uns nun eine Umgebung an, für die wiederum das Aufbewahrungsverhalten **Continue ingress and purge old data** (Eingang fortsetzen und alte Daten bereinigen) festgelegt ist. In diesem Beispiel ist die **Datenaufbewahrungszeit** aber auf einen niedrigeren Wert von 180 Tagen festgelegt. Die **Kapazität** ist auf die S1-Einheit festgelegt. Dies entspricht einer Gesamtkapazität von 30 GB. Um Daten für die gesamten 180 Tage speichern zu können, darf die Menge an eingehenden Daten 0,166 GB (166 MB) pro Tag nicht überschreiten.  

Immer wenn die tägliche Eingangsrate für diese Umgebung über den Wert von 0,166 GB pro Tag steigt, können Daten nicht 180 Tage lang aufbewahrt werden, da einige Daten bereinigt werden. Stellen Sie sich diese Umgebung nun für Zeiten mit hoher Auslastung vor. Angenommen, die Eingangsrate der Umgebung steigt auf durchschnittlich 0,189 GB pro Tag an. Während dieser Zeit mit hoher Auslastung werden Daten etwa 158 Tage lange aufbewahrt (30GB/0,189 = 158,73 Aufbewahrungstage). Dieser Zeitraum ist kürzer als der gewünschte Zeitrahmen für die Datenaufbewahrung.

## <a name="pause-ingress"></a>Pause ingress (Eingang anhalten)
- Mit diesem Verhalten soll sichergestellt werden, dass Daten nicht bereinigt werden, wenn die Grenzwerte für die Größe und Anzahl vor dem Ende des Aufbewahrungszeitraums erreicht werden.  
- Benutzer erhalten hierbei zusätzliche Zeit, um die Kapazität ihrer Umgebung zu erhöhen, bevor Daten bereinigt werden, weil die Regeln für den Aufbewahrungszeitraum verletzt wurden.
- Dieses Verhalten dient als Schutz vor Datenverlust, aber es kann auch passieren, dass die aktuellsten Daten verloren gehen, falls der Dateneingang über den Aufbewahrungszeitraum Ihrer Ereignisquelle hinaus angehalten wird.
- Nachdem die maximale Kapazität einer Umgebung erreicht ist, wird der Dateneingang aber von der Umgebung angehalten, bis zusätzliche Aktionen durchgeführt wurden: 
   - Sie erhöhen die maximale Kapazität der Umgebung. Weitere Informationen zum Hinzufügen von weiteren Skalierungseinheiten finden Sie unter [Skalieren der Time Series Insights-Umgebung](time-series-insights-how-to-scale-your-environment.md).
   - Der Zeitraum für die Datenaufbewahrung ist erreicht, und Daten werden bereinigt, sodass für die Umgebung die maximale Kapazität überschritten wird.

### <a name="example-3"></a>Beispiel 3:
Bei der nächsten Umgebung ist für das Aufbewahrungsverhalten **Pause ingress** (Eingang anhalten) konfiguriert. In diesem Beispiel ist die **Datenaufbewahrungszeit** auf 60 Tage festgelegt. Die **Kapazität** ist auf drei S1-Einheiten festgelegt. Angenommen, diese Umgebung verfügt pro Tag über einen Dateneingang von 2 GB. In dieser Umgebung wird der Dateneingang angehalten, nachdem die maximale Kapazität erreicht ist. Anschließend wird in der Umgebung dasselbe Dataset angezeigt, bis der Dateneingang fortgesetzt oder „Continue ingress“ (Eingang fortsetzen) aktiviert wird (sodass ältere Daten wieder bereinigt werden, um Platz für neue Daten zu schaffen). 

Bei Fortsetzung des Dateneingangs:
- Daten fließen in der Reihenfolge, in der sie von der Ereignisquelle empfangen wurden.
- Die Ereignisse werden basierend auf ihrem Zeitstempel indiziert, sofern Sie nicht gegen die Aufbewahrungsrichtlinien auf Ihrer Ereignisquelle verstoßen haben. Weitere Informationen zur Konfiguration der Aufbewahrung auf der Ereignisquelle finden Sie unter [Häufig gestellte Fragen zu Event Hubs](../event-hubs/event-hubs-faq.md).

> [!IMPORTANT]
> Sie sollten Warnungen festlegen, damit Hinweise angezeigt werden und möglichst verhindert wird, dass der Dateneingang angehalten wird. Es kann zu Datenverlust kommen, da die Standardaufbewahrungsdauer für Azure-Ereignisquellen einen Tag beträgt. Nachdem der Dateneingang angehalten wurde, gehen daher vermutlich die aktuellsten Daten verloren, sofern Sie nicht weitere Maßnahmen ergreifen. Sie müssen die Kapazität erhöhen oder das Verhalten auf **Purge old data** (Alte Daten bereinigen) festlegen, um potenzielle Datenverluste zu vermeiden.

Erwägen Sie für die betroffenen Event Hubs die Anpassung der Eigenschaft **Nachrichtenaufbewahrung**, um Datenverlust zu verringern, wenn in Time Series Insights der Dateneingang angehalten wird.

![Event Hub-Nachrichtenbeibehaltung](media/time-series-insights-contepts-retention/event-hub-retention.png)

Wenn für die Ereignisquelle (timeStampPropertyName) keine Eigenschaften konfiguriert werden, wird für TSI als X-Achse standardmäßig der Zeitstempel der Ankunft im Event Hub verwendet. Falls für timeStampPropertyName eine andere Konfiguration eingerichtet wurde, sucht die Umgebung im Datenpaket nach dem konfigurierten timeStampPropertyName-Element, wenn Ereignisse analysiert werden. 

Sofern Sie Ihre Umgebung zentral hochskalieren müssen, um zusätzliche Kapazität bereitzustellen oder die Länge des Aufbewahrungszeitraums zu erhöhen, helfen Ihnen die Informationen unter [Gewusst wie: Skalieren der Azure Time Series Insights-Umgebung](time-series-insights-how-to-scale-your-environment.md) weiter.  

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Wechseln des Aufbewahrungsverhaltens finden Sie unter [Konfigurieren der Vermerkdauer in Time Series Insights](time-series-insights-how-to-configure-retention.md).
