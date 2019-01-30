---
title: Erstellen von Warnungen mit dynamischen Schwellenwerten in Azure Monitor
description: Erstellen von Warnungen mit Machine Learning-basierten dynamischen Schwellenwerten
author: yanivlavi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: yalavi
ms.reviewer: mbullwin
ms.openlocfilehash: 4024ecddde4b0d020e2c657214a4a258ea0b2ea5
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449009"
---
# <a name="metric-alerts-with-dynamic-thresholds-in-azure-monitor-public-preview"></a>Metrikwarnungen mit dynamischen Schwellenwerten in Azure Monitor (Public Preview)

Die Metrikwarnung mit Erkennung dynamischer Schwellenwerte nutzt erweitertes Machine Learning (ML), um das bisherige Verhalten von Metriken zu erfassen sowie Muster und Anomalien zu erkennen, die auf mögliche Probleme hinweisen. Benutzer profitieren von einer einfachen Benutzeroberfläche sowie von skalierbaren Vorgängen und können über die Azure Resource Manager-API vollständig automatisierte Warnungsregeln konfigurieren.

Wenn eine Warnungsregel erstellt wurde, wird diese nur ausgelöst, wenn die überwachte Metrik nicht wie erwartet reagiert (basierend auf individuellen Schwellenwerten).

Wir freuen uns auf Ihr Feedback: azurealertsfeedback@microsoft.com

## <a name="why-and-when-is-using-dynamic-condition-type-recommended"></a>Warum und wann ist der dynamische Bedingungstyp empfehlenswert?

1. **Skalierbare Warnungen** – Mit dynamischen Schwellenwerten für Warnungsregeln können Sie individuelle Schwellenwerte für Hunderte von Metrikserien erstellen. Die Bereitstellung ist genauso einfach wie die Definition einer Warnungsregel für eine einzelne Metrik. Mit der Benutzeroberfläche oder der Azure Resource Manager-API müssen Sie weniger Warnungsregeln verwalten. Der skalierbare Ansatz ist besonders hilfreich beim Umgang mit Metrikdimensionen oder bei der Anwendung auf mehrere Ressourcen, z.B. alle Abonnementressourcen. So sparen Sie viel Zeit beim Verwalten und Erstellen von Warnungsregeln. [Erfahren Sie mehr über das Konfigurieren von Metrikwarnungen mit dynamischen Schwellenwerten mithilfe von Vorlagen](alerts-metric-create-templates.md).

1. **Intelligente Erkennung von Metrikmustern** – Mit unserer einzigartigen ML-Technologie können wir Metrikmuster automatisch erkennen und uns im Laufe der Zeit an Metrikänderungen anpassen, die häufig Saisonalität (stündlich/täglich/wöchentlich) enthalten. Durch die Anpassung an das Metrikverhalten und Warnungen basierend auf Abweichungen von Mustern müssen Sie nicht mehr den „richtigen“ Schwellenwert für die einzelnen Metriken ermitteln. Der ML-Algorithmus in dynamischen Schwellenwerten soll überflüssige (geringe Genauigkeit) oder zu weit gefasste (geringe Trefferquote) Schwellenwerte ohne erwartetes Muster verhindern.

1. **Intuitive Konfiguration** – Dynamische Schwellenwerte ermöglichen die Einrichtung von Metrikwarnungen mit allgemeinen Konzepten, ohne dass umfassendes Domänenwissen zur Metrik erforderlich ist.

## <a name="how-to-configure-alerts-rules-with-dynamic-thresholds"></a>Wie werden Warnungen mit dynamischen Schwellenwerten konfiguriert?

Warnungen mit dynamischen Schwellenwerten können über Metrikwarnungen in Azure Monitor konfiguriert werden. [Erfahren Sie mehr über die Konfiguration von Metrikwarnungen](alerts-metric.md).

## <a name="how-are-the-thresholds-calculated"></a>Wie werden die Schwellenwerte berechnet?

Dynamische Schwellenwerte erfassen fortlaufend Daten der Metrikreihe und versuchen, sie mit einem Satz von Algorithmen und Methoden zu modellieren. Zudem versuchen sie, sie mit einem Satz von Algorithmen und Methoden modellieren. Sie erkennen Muster in den Daten, z.B. die Saisonalität (stündlich/täglich/wöchentlich), und können überflüssige Metriken (z.B. Geräte-CPU oder Arbeitsspeicher) sowie Metriken mit geringer Genauigkeit (z.B. die Verfügbarkeit und Fehler) verarbeiten.

Die Schwellenwerte werden so ausgewählt, dass eine Abweichung von diesen Schwellenwerten eine Anomalie im Metrikverhalten ergibt.

## <a name="what-does-sensitivity-setting-in-dynamic-thresholds-mean"></a>Was bedeutet die „Sensitivity“-Einstellung in dynamischen Schwellenwerten?

Mit der Empfindlichkeit des Schwellenwerts legen Sie fest, bei welcher Abweichung vom Metrikverhalten eine Warnung ausgelöst werden soll.
Diese Option erfordert kein Domänenwissen zur Metrik, z.B. statische Schwellenwerte. Die verfügbaren Optionen sind:

- Hoch – Die Schwellenwerte sind streng und nah am Muster der Metrikreihe. Die Warnungsregel wird schon bei der kleinsten Abweichung ausgelöst, sodass mehr Warnungen auftreten.
- Mittel – Weniger strenge und ausgewogenere Schwellenwerte, sodass weniger Warnungen auftreten als bei der hohen Empfindlichkeit (Standard).
- Niedrig – Die Schwellenwerte werden mit zunehmender Distanz zum Muster der Metrikreihe lockerer. Die Warnungsregel wird nur bei großen Abweichungen ausgelöst, was zu weniger Warnungen führt.

## <a name="what-are-the-operator-setting-options-in-dynamic-thresholds"></a>Was bedeuten die „Operator“-Einstellungen in dynamischen Schwellenwerten?

Mit der Warnungsregel für dynamische Schwellwerte können Sie individuelle Schwellenwerte festlegen, die auf dem Metrikverhalten für die obere und untere Grenze mit der gleichen Warnungsregel basieren.
Sie können die Warnung bei einer der drei folgenden Bedingungen auslösen:

- Überschreitung des oberen Schwellenwerts oder Unterschreitung des unteren Schwellenwerts (Standardeinstellung)
- Überschreitung des oberen Schwellenwerts
- Unterschreitung des unteren Schwellenwerts

## <a name="what-do-the-advanced-settings-in-dynamic-thresholds-mean"></a>Was bedeuten die erweiterten Einstellungen im dynamischen Schwellenwerten?

**Zeiträume, die Fehler ausweisen** – Mit dem dynamischen Schwellenwert können Sie die „Anzahl von Verstößen zum Auslösen der Warnung“ festlegen. Damit bestimmen Sie, ab wie vielen Abweichungen innerhalb eines bestimmten Zeitfensters das System eine Warnung auslöst (Das Zeitfenster ist standardmäßig auf vier Abweichungen in 20 Minuten festgelegt). Der Benutzer kann Zeiträume, die Fehler aufweisen, konfigurieren und die Warnungen auswählen, indem er die Zeiträume, die Fehler aufweisen, und das Zeitfenster auswählt. So reduzieren Sie überflüssige Warnungen als Reaktion auf vorübergehende Spitzen. Beispiel: 

Mit den folgenden Einstellungen wird eine Warnung ausgelöst, wenn das Problem 20 Minuten lang (viermal hintereinander in einem Zeitraum von jeweils fünf Minuten) aufgetreten ist:

![Einstellungen für Zeiträume, die Fehler aufweisen, bei kontinuierlichen Problemen über einen Zeitraum von 20 Minuten, viermal hintereinander in einem Zeitraum von jeweils fünf Minuten](media/alerts-dynamic-thresholds/0008.png)

Mit den folgenden Einstellungen wird eine Warnung ausgelöst, wenn ein dynamischer Schwellenwert innerhalb der letzten 30 Minuten für 20 Minuten (Zeitraum von fünf Minuten) verletzt wurde:

![Einstellungen für Zeiträume, die Fehler aufweisen, über einen Zeitraum von 30 Minuten, mehrmals hintereinander hintereinander in einem Zeitraum von jeweils fünf Minuten](media/alerts-dynamic-thresholds/0009.png)

**Vor dem folgenden Datum liegende Daten ignorieren** – Benutzer können optional auch ein Startdatum definieren, ab dem das System mit der Berechnung der Schwellenwerte beginnen soll. Ein typischer Anwendungsfall kann auftreten, wenn eine Ressource in einem Testmodus ausgeführt wurde und nun höher gestuft wird, um eine Produktionsworkload zu verarbeiten. Dann sollte das Verhalten jeder beliebigen Metrik während der Testphase ignoriert werden.

## <a name="will-slow-behavior-change-in-the-metric-trigger-an-alert"></a>Wird bei einer langsamen Veränderungsveränderung der Metrik eine Warnung ausgelöst?

Wahrscheinlich nicht. Dynamische Schwellenwerte eignen sich besser für die Erkennung erheblicher Abweichungen und weniger für Probleme, die sich langsam entwickeln.

## <a name="how-much-data-is-used-to-preview-and-then-calculate-thresholds"></a>Wie viele Daten werden für die Vorschau und die anschließende Berechnung der Schwellenwerte verwendet?

Die im Diagramm angezeigten Schwellenwerte werden vor dem Erstellen einer Warnungsregel für die Metrik auf der Grundlage der Verlaufsdaten der letzten 10 Tage berechnet. Nach der Erstellung der Warnungsregel erfassen die dynamischen Schwellenwerte zusätzliche verfügbare Verlaufsdaten und berücksichtigen neue Daten, um die Präzision der Schwellenwerte zu verbessern.
