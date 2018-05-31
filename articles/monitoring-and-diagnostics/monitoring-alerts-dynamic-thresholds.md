---
title: Erstellen von Warnungen mit dynamischen Schwellenwerten in Azure Monitor | Microsoft-Dokumentation
description: Erstellen von Warnungen mit Machine Learning-basierten dynamischen Schwellenwerten
author: antonfrMSFT
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: antonfr;mbullwin
ms.openlocfilehash: 45d2f39ff7fb2e5a22cda74cd0c794db28e351f1
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33943296"
---
# <a name="alerts-with-dynamic-thresholds-in-azure-monitor-limited-public-preview"></a>Warnungen mit dynamischen Schwellenwerten in Azure Monitor (eingeschränkte Public Preview)

Bei Warnungen mit dynamischen Schwellenwerten handelt es sich um eine Erweiterung für Azure-Metrikwarnungen in Azure Monitor, die unter Zuhilfenahme fortschrittlicher ML-Funktionen (Machine Learning) das bisherige Verhalten von Metriken erfassen und auf dieser Grundlage automatische Baselines berechnen und als Warnungsschwellenwerte verwenden.

Dynamische Schwellenwerte haben folgende Vorteile:

- Es muss nicht umständlich eine vordefinierte feste Grenze festgelegt werden, da die Überwachung automatisch die bisherige Leistung einer Metrik erfasst und die Warnungsschwellenwerte mithilfe von ML-Algorithmen bestimmt.
- Sie können saisonales Verhalten erkennen und nur bei Abweichungen vom erwarteten saisonalen Verhalten warnen. Metrikwarnungen mit dynamischen Schwellenwerten werden nicht ausgelöst, wenn sich Ihr Dienst am Wochenende regelmäßig im Leerlauf befindet und montags jeweils Auslastungsspitzen auftreten. Derzeit wird stündliche, tägliche und wöchentliche Saisonalität unterstützt.
- Das Feature erfasst kontinuierlich die Metrikleistung und reagiert auf Veränderungen der Metrik.

Warnungen, die auf dynamischen Schwellenwerten basieren, stehen für alle Azure Monitor-basierten Metrikquellen aus [diesem Artikel](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts#what-resources-can-i-create-near-real-time-metric-alerts-for) zur Verfügung.

## <a name="sign-up-to-access-the-preview"></a>Registrieren für Zugriff auf die Vorschauversion

Wenn Sie diese Funktion einmal ausprobieren möchten, [registrieren Sie sich für die Vorschauversion](http://aka.ms/DynamicThresholdMetricAlerts). Wir freuen uns wie immer auf Ihr Feedback: [azurealertsfeedback@microsoft.com](mailto:azurealertsfeedback@microsoft.com)

## <a name="how-to-configure-alerts-with-dynamic-thresholds"></a>Konfigurieren von Warnungen mit dynamischen Schwellenwerten

Warnungen mit dynamischen Schwellenwerten können über Warnungen in Azure Monitor konfiguriert werden.

![Warnungen (Vorschauversion)](./media/monitoring-alerts-dynamic-thresholds/0001.png)

## <a name="creating-an-alert-rule-with-dynamic-thresholds"></a>Erstellen einer Warnungsregel mit dynamischen Schwellenwerten

1. Klicken Sie in Monitor im Bereich „Warnungen“ auf die Schaltfläche **Neue Warnungsregel**, um eine neue Warnung in Azure zu erstellen.

   ![Neue Warnungsregel](./media/monitoring-alerts-dynamic-thresholds/002.png)

2. Der Abschnitt „Warnung erstellen“ wird angezeigt. Dieser besteht aus drei Teilen: _Warnungsbedingung definieren_, _Warnungsdetails definieren_ und _Aktionsgruppe definieren_. Beginnen Sie im Abschnitt _Warnungsbedingung definieren_. Verwenden Sie den Link **Ziel auswählen**, um das Ziel anzugeben, und wählen Sie eine Ressource aus. Klicken Sie nach der Wahl einer geeigneten Ressource auf die Schaltfläche „Fertig“.

   ![Auswählen des Ziels](./media/monitoring-alerts-dynamic-thresholds/0003.png)

3. Klicken Sie als Nächstes auf die Schaltfläche **Kriterien hinzufügen**, um die Liste mit den verfügbaren Signaloptionen für die Ressource anzuzeigen, und wählen Sie eine geeignete **Metrikoption** aus. (Beispiel: CPU in Prozent.)

   ![Hinzufügen von Kriterien](./media/monitoring-alerts-dynamic-thresholds/004.png)

4. Im Abschnitt „Warnungslogik“ des Bildschirms „Signallogik konfigurieren“ haben Sie die Möglichkeit, den Bedingungstyp auf „Dynamisch“ festzulegen. Dadurch werden entlang der Metrik (blaue Linie) automatisch dynamische Schwellenwerte (rote Linien) generiert.

   ![Dynamisch](./media/monitoring-alerts-dynamic-thresholds/005.png)

5. Die im Diagramm angezeigten Schwellenwerte werden auf der Grundlage der Verlaufsdaten der letzten sieben Tage berechnet. Nach der Warnungserstellung erfassen die dynamischen Schwellenwerte zusätzliche verfügbare Verlaufsdaten und berücksichtigt neue Daten, um die Präzision der Schwellenwerte zu verbessern.

6. Zusätzliche Einstellungen für die Warnungslogik:
   - Bedingung: Die Warnung kann durch eine der drei folgenden Bedingungen ausgelöst werden:
       - Überschreitung des oberen Schwellenwerts oder Unterschreitung des unteren Schwellenwerts (Standardeinstellung)
       - Überschreitung des oberen Schwellenwerts
       - Unterschreitung des unteren Schwellenwerts
   - Zeitaggregation: Durchschnitt (Standardeinstellung), Summe, Minimum, Maximum
   - Warnungsempfindlichkeit:
       - Hoch: Mehr Warnungen, da bei der kleinsten Abweichung eine Warnung ausgelöst wird.
       - Medium: Weniger empfindlich als „Hoch“ und weniger Warnungen als bei hoher Empfindlichkeit. (Standardeinstellung)
       - Niedrig: Die geringste Empfindlichkeit für den Schwellenwert.

    ![Einstellungen für die Warnungslogik](./media/monitoring-alerts-dynamic-thresholds/00007.png)

7. Auswertung basierend auf:
    -  In welchem Zeitraum die Warnung nach der angegebenen Bedingung suchen soll (auszuwählen unter **Zeitraum**).

    ![Auswertung basierend auf](./media/monitoring-alerts-dynamic-thresholds/007.png)

   > [!NOTE]
   > Unterstützte Werte: 5 Minuten, 10 Minuten, 30 Minuten und 1 Stunde.

   Zur Verringerung unnötiger Warnungen bei vorübergehenden Spitzen empfiehlt sich die Verwendung der Einstellung „Anzahl von Verstößen zum Auslösen der Warnung“. Mit dieser Funktion können Sie dafür sorgen, dass Sie nur dann eine Warnung erhalten, wenn der Schwellenwert x Mal hintereinander oder y Mal in den letzten z Zeiträumen verletzt wurde. Beispiel: 

    Mit den folgenden Einstellungen wird eine Warnung ausgelöst, wenn das Problem 15 Minuten lang (dreimal hintereinander in einem Zeitraum von jeweils fünf Minuten) aufgetreten ist:

   ![Auswertung basierend auf](./media/monitoring-alerts-dynamic-thresholds/0008.png)

    Mit den folgenden Einstellungen wird eine Warnung ausgelöst, wenn ein dynamischer Schwellenwert innerhalb der letzten 30 Minuten für 15 Minuten (Zeitraum: fünf Minuten) verletzt wurde.

   ![Auswertung basierend auf](./media/monitoring-alerts-dynamic-thresholds/0009.png)

8. Derzeit können Benutzer Warnungen mit dynamischen Schwellenwertkriterien als einzelnes Kriterium verwenden.

   ![Erstellen einer Regel](./media/monitoring-alerts-dynamic-thresholds/010.png)

## <a name="q--a"></a>Fragen und Antworten

- F: Wird bei Verwendung dynamischer Schwellenwerte eine Warnung ausgelöst, wenn sich die Metrik im Laufe der Zeit langsam verändert?

- A: Wahrscheinlich nicht. Dynamische Schwellenwerte eignen sich besser für die Erkennung erheblicher Abweichungen und weniger für Probleme, die sich langsam entwickeln.

- F: Kann ich dynamische Schwellenwerte über eine API konfigurieren?

- A: Wir arbeiten daran.
