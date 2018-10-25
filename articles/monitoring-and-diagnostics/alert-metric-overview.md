---
title: Erhalten Sie Informationen zur Funktionsweise von Metrikwarnungen in Azure Monitor.
description: Verschaffen Sie sich einen Überblick darüber, was Sie mit Metrikwarnungen erreichen können und wie sie in Azure Monitor funktionieren.
author: snehithm
ms.author: snmuvva
ms.date: 9/18/2018
ms.topic: conceptual
ms.service: azure-monitor
ms.component: alerts
ms.openlocfilehash: 0ac19ec18c443f677260ba22b6bce1c06237fb2e
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816429"
---
# <a name="understand-how-metric-alerts-work-in-azure-monitor"></a>Informationen zur Funktionsweise von Metrikwarnungen in Azure Monitor

Die Metrikwarnungen in Azure Monitor ergänzen die mehrdimensionalen Metriken. Diese Metriken können Plattformmetriken, [benutzerdefinierte Metriken](metrics-custom-overview.md), [gängige Protokolle von Log Analytics, die in Metriken umgewandelt wurden](monitoring-metric-alerts-logs.md), und Standardmetriken von Application Insights sein. Metrikwarnungen werden in regelmäßigen Abständen ausgewertet, um zu überprüfen, ob die Bedingungen für eine oder mehrere metrische Zeitreihen erfüllt sind, und um Sie darüber zu informieren, wann die Auswertungen erfüllt sind. Metrikwarnungen sind zustandsbehaftet. Sie senden Benachrichtigungen nur dann, wenn sich der Zustand ändert.

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

### <a name="monitoring-multiple-resources-using-metric-alerts"></a>Überwachen mehrerer Ressourcen mithilfe von Metrikwarnungen

Wie Sie bereits im vorherigen Abschnitt gesehen haben, ist es möglich, eine einzelne Metrikwarnungsregel zu verwenden, die jede einzelne Dimensionskombination (d. h. eine metrische Zeitreihe) überwacht. Vorher galt weiterhin die Beschränkung, dass eine Ressource nach der anderen bearbeitet werden muss. Azure Monitor unterstützt auch die Überwachung von mehreren Ressourcen mit einer Metrikwarnungsregel. Dieses Feature befindet sich derzeit in der Vorschauphase und wird nur auf virtuellen Computern unterstützt. Mit nur einer Metrikwarnung können die Ressourcen einer Azure-Region überwacht werden.

Sie können den Bereich für die Überwachung mit einer einzelnen Metrikwarnung auf drei Arten angeben:

- Als Liste mit virtuellen Computern einer Azure-Region unter einem Abonnement
- Alle virtuellen Computer (in einer Azure-Region) in einer oder mehreren Ressourcengruppen eines Abonnements
- Alle virtuellen Computer (in einer Azure-Region) unter einem Abonnement

Die Erstellung von Metrikwarnungsregeln zur Überwachung mehrerer Ressourcen wird derzeit nicht vom Azure-Portal unterstützt. Sie können diese Regeln mit [Azure Resource Manager-Vorlagen](monitoring-create-metric-alerts-with-templates.md#resource-manager-template-for-metric-alert-that-monitors-multiple-resources) erstellen. Sie erhalten für jeden virtuellen Computer gesonderte Benachrichtigungen. 

## <a name="typical-latency"></a>Typische Wartezeit

Bei Metrikwarnungen werden Sie in der Regel in weniger als 5 Minuten benachrichtigt, wenn Sie die Häufigkeit der Warnungsregeln auf 1 Minute einstellen. Bei Benachrichtigungssystemen mit hoher Last kann es zu einer längeren Wartezeit kommen.

## <a name="supported-resource-types-for-metric-alerts"></a>Unterstützte Ressourcentypen für Metrikwarnungen

Die vollständige Liste der unterstützten Ressourcentypen finden Sie in diesem [Artikel](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported).

Wenn Sie im Moment klassische Metrikwarnungen verwenden und erfahren möchten, ob Metrikwarnungen alle verwendeten Ressourcentypen unterstützen, zeigt die folgende Tabelle die Ressourcentypen, die von klassischen Metrikwarnungen unterstützt werden, und ob sie von aktuellen Metrikwarnungen unterstützt werden.

|Ressourcentyp – von klassischen Metrikwarnungen unterstützt | Unterstützt von Metrikwarnungen |
|-------------------------------------------------|----------------------------|
| Microsoft.ApiManagement/service | JA |
| Microsoft.Batch/batchAccounts| JA|
|Microsoft.Cache/redis| JA
|Microsoft.ClassicCompute/virtualMachines | Nein  |
|Microsoft.ClassicCompute/domainNames/slots/roles | Nein |
|Microsoft.CognitiveServices/accounts | Nein  |
|Microsoft.Compute/virtualMachines | JA|
|Microsoft.Compute/virtualMachineScaleSets| JA|
|Microsoft.ClassicStorage/storageAccounts| Nein  |
|Microsoft.DataFactory/datafactories | JA|
|Microsoft.DBforMySQL/servers| JA|
|Microsoft.DBforPostgreSQL/servers| JA|
|Microsoft.Devices/IotHubs | Nein |
|Microsoft.DocumentDB/databaseAccounts| Nein |
|Microsoft.EventHub/namespaces | JA|
|Microsoft.Logic/workflows | JA|
|Microsoft.Network/loadBalancers |JA|
|Microsoft.Network/publicIPAddresses| JA|
|Microsoft.Network/applicationGateways| JA|
|Microsoft.Network/expressRouteCircuits| JA|
|Microsoft.Network/trafficManagerProfiles | JA|
|Microsoft.Search/searchServices | Nein |
|Microsoft.ServiceBus/namespaces| Nein |
|Microsoft.Storage/storageAccounts | JA|
|Microsoft.StreamAnalytics/streamingjobs| JA|
|Microsoft.TimeSeriesInsights/environments | JA|
|Microsoft. Web/serverfarms | JA |
|Microsoft. Web/sites (ohne Funktionen) | JA|
|Microsoft. Web/hostingEnvironments/multiRolePools | Nein |
|Microsoft. Web/hostingEnvironments/workerPools| Nein 
|Microsoft.SQL/Servers | Nein |

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie, wie Sie in Azure Metrikwarnungen erstellen, anzeigen und verwalten können.](alert-metric.md)
- [Erfahren Sie, wie Sie Metrikwarnungen mithilfe von Azure Resource Manager-Vorlagen bereitstellen können](monitoring-create-metric-alerts-with-templates.md).
- [Erfahren Sie mehr über Aktionsgruppen](monitoring-action-groups.md).
