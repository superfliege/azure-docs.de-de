---
title: Erhalten Sie Informationen zur Funktionsweise von Metrikwarnungen in Azure Monitor.
description: Verschaffen Sie sich einen Überblick darüber, was Sie mit Metrikwarnungen erreichen können und wie sie in Azure Monitor funktionieren.
author: snehithm
ms.author: snmuvva
ms.date: 9/18/2018
ms.topic: conceptual
ms.service: azure-monitor
ms.component: alerts
ms.openlocfilehash: 1ec47ddf5769dd8ed624277a86db57f449581b90
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948688"
---
# <a name="understand-how-metric-alerts-work-in-azure-monitor"></a>Informationen zur Funktionsweise von Metrikwarnungen in Azure Monitor

Die Metrikwarnungen in Azure Monitor ergänzen die mehrdimensionalen Metriken. Diese Metriken können Plattformmetriken, benutzerdefinierte Metriken (Vorschau), gängige Protokolle von Log Analytics, die in Metriken umgewandelt wurden, und Standardmetriken von Application Insights sein. Metrikwarnungen werden in regelmäßigen Abständen ausgewertet, um zu überprüfen, ob die Bedingungen für eine oder mehrere metrische Zeitreihen erfüllt sind, und um Sie darüber zu informieren, wann die Auswertungen erfüllt sind. Metrikwarnungen sind zustandsbehaftet, d. h. sie senden Benachrichtigungen nur dann aus, wenn sich der Zustand ändert.

## <a name="how-do-metric-alerts-work"></a>Funktionsweise von Metrikwarnungen

Sie können eine Metrikwarnungsregel definieren, indem Sie eine zu überwachende Zielressource, den metrischen Namen und die Bedingung (einen Operator und einen Schwellenwert) sowie eine Aktionsgruppe angeben, die bei Auslösung der Warnregel ausgelöst werden soll.
Nehmen wir an, Sie haben eine einfache Metrikwarnungsregel wie folgt erstellt:

- Zielressource (die zu überwachende Azure-Ressource): myVM
- Metrik: CPU in Prozent
- Zeitaggregation (Statistik, die über Rohmetriken geführt wird. Unterstützte Zeitaggregationen sind Minimum, Maximum, Durchschnitt, Gesamtwert): Durchschnitt
- Zeitraum (das zurückliegende Zeitfenster, über das Metrikwerte geprüft werden): Über die letzten 5 Minuten
- Häufigkeit (die Häufigkeit, mit der die Metrikwarnung überprüft werden soll, wenn die Bedingungen erfüllt sind): 1 Minute
- Operator: Größer als
- Schwellenwert: 70

Ab dem Zeitpunkt der Erstellung der Warnungsregel wird der Monitor jede Minute ausgeführt, der dann die Metrikwerte der letzten fünf Minuten betrachtet und überprüft, ob der Durchschnitt dieser Werte 70 übersteigt. Wenn die Bedingung erfüllt ist, d. h. der durchschnittliche Wert für „CPU in Prozent“ überschreitet für die letzten fünf Minuten den Wert 70, löst die Warnungsregel eine aktivierte Benachrichtigung aus. Wenn Sie eine E-Mail- oder Webhook-Aktion in der Aktionsgruppe konfiguriert haben, die der Warnungsregel zugeordnet ist, erhalten Sie für beide eine aktivierte Benachrichtigung.

Diese besondere Instanz der Auslösung der Warnungsregel kann auch im Azur-Portal auf dem Blatt „Alle Warnungen“ eingesehen werden.

Angenommen, die Verwendung von „myVM“ liegt bei nachfolgenden Prüfungen weiterhin über dem Schwellenwert, dann wird die Warnungsregel nicht erneut ausgelöst, bevor die Bedingung aufgelöst wurde.

Nach einer gewissen Zeit, wenn die Nutzung von „myVM“ wieder einen normalen Wert annimmt, d. h. unter den angegebenen Schwellenwert fällt. Die Warnungsregel überwacht die Bedingung noch zweimal, um eine Benachrichtigung zur aufgelösten Bedingung zu versenden. Die Warnungsregel sendet eine aufgelöste/deaktivierte Nachricht, wenn die Warnungsbedingung drei aufeinanderfolgende Zeiträume lang nicht erfüllt ist, um Störungen im Falle von Fluktuationsbedingungen zu reduzieren.

Da die aufgelöste Benachrichtigung über einen Webhook oder eine E-Mail versendet wird, wird auch der Status der Warnungsinstanz (Monitorzustand) im Azure-Portal auf „aufgelöst“ festgelegt.

## <a name="monitoring-at-scale-using-metric-alerts-in-azure-monitor"></a>Bedarfsorientierte Überwachung mithilfe von Metrikwarnungen in Azure Monitor

### <a name="using-dimensions"></a>Verwenden von Dimensionen

Metrikwarnungen in Azure Monitor unterstützen auch die Überwachung von mehrdimensionalen Wertekombinationen mit einer Regel. Lassen Sie uns anhand eines Beispiels erläutern, warum Sie mehrere Dimensionskombinationen verwenden können.

Angenommen, Sie verfügen über einen App Service-Plan für Ihre Website. Sie möchten die CPU-Auslastung mehrerer Instanzen überwachen, die Ihre Website/App ausführen. Dies können Sie mithilfe einer Metrikwarnungsregel wie folgt durchführen

- Zielressource: myAppServicePlan
- Metrik: CPU in Prozent
- Dimensionen
  - Instanz = InstanceName1, InstanceName2
- Zeitaggregation: Durchschnitt
- Zeitraum = Innerhalb der letzten 5 Minuten
- Häufigkeit: 1 Minute
- Operator: GreaterThan
- Schwellenwert: 70

Wie zuvor überwacht diese Regel, ob die durchschnittliche CPU-Auslastung in den letzten 5 Minuten 70 % übersteigt. Mit derselben Regel können Sie jedoch zwei Instanzen überwachen, die Ihre Website ausführen. Jede Instanz wird einzeln überwacht und Sie erhalten individuelle Benachrichtigungen.

Angenommen, Sie verfügen über eine Web-App, die eine massive Nachfrage verzeichnet, und Sie müssen weitere Instanzen hinzufügen. Die obige Regel überwacht weiterhin nur die beiden Instanzen. Sie können jedoch wie folgt eine Regel erstellen.

- Zielressource: myAppServicePlan
- Metrik: CPU in Prozent
- Dimensionen
  - Instanz = *
- Zeitaggregation: Durchschnitt
- Zeitraum = Innerhalb der letzten 5 Minuten
- Häufigkeit: 1 Minute
- Operator: GreaterThan
- Schwellenwert: 70

Diese Regel überwacht automatisch alle Werte für die Instanz, d. h. Sie können Ihre Instanzen überwachen, sobald sie auftreten, ohne Ihre Metrikwarnungsregel erneut ändern zu müssen.

### <a name="monitoring-multiple-resource-using-metric-alerts"></a>Überwachen mehrerer Ressourcen mithilfe von Metrikwarnungen

Wie Sie bereits im vorherigen Abschnitt gesehen haben, ist es möglich, eine einzelne Metrikwarnungsregel zu verwenden, die jede einzelne Dimensionskombination (d. h. eine metrische Zeitreihe) überwacht. Sie sind jedoch nach wie vor darauf beschränkt, eine Ressource nach der anderen zu bearbeiten. Metrikwarnungen unterstützen jetzt auch die Überwachung mehrerer Ressourcen mit einer Regel in der Vorschau. Wenn in Ihrem Abonnement 100 VMs enthalten sind, können Sie deren Überwachung mit diesem neuen Feature schnell einrichten. 

Diese Funktion steht derzeit als Vorschau zur Verfügung. Die Erstellung von Metrikwarnungsregeln zur Überwachung mehrerer Ressourcen wird derzeit nicht vom Azure-Portal unterstützt. Sie können diese Regeln über Azure Resource Manager-Vorlagen erstellen.

## <a name="typical-latency"></a>Typische Wartezeit

Bei Metrikwarnungen werden Sie in der Regel in weniger als 5 Minuten benachrichtigt, wenn Sie die Häufigkeit der Warnungsregeln auf 1 Minute einstellen. Bei Benachrichtigungssystemen mit hoher Last kann es zu einer längeren Wartezeit kommen.

## <a name="supported-resource-types-for-metric-alerts"></a>Unterstützte Ressourcentypen für Metrikwarnungen

Die vollständige Liste der unterstützten Ressourcentypen finden Sie in diesem [Artikel](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported).

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie, wie Sie in Azure Metrikwarnungen erstellen, anzeigen und verwalten können.](alert-metric.md)
- [Erfahren Sie, wie Sie Metrikwarnungen mithilfe von Azure Resource Manager-Vorlagen bereitstellen können](monitoring-create-metric-alerts-with-templates.md).
- [Erfahren Sie mehr über Aktionsgruppen](monitoring-action-groups.md).
