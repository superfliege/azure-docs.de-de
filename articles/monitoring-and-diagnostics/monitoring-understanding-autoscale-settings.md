---
title: "Grundlegendes zu den Einstellungen für die automatische Skalierung | Microsoft-Dokumentation"
description: "Enthält eine ausführliche Aufschlüsselung der Einstellungen für die automatische Skalierung und deren Funktionsweise."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ce2930aa-fc41-4b81-b0cb-e7ea922467e1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: ancav
ms.openlocfilehash: 79602cf053d834bf3d6dc6b4d5568637b179d5c7
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="understand-autoscale-settings"></a>Grundlegendes zu Einstellungen für die automatische Skalierung
Mit den Einstellungen für die automatische Skalierung können Sie sicherstellen, dass Sie über die richtige Anzahl von ausgeführten Ressourcen verfügen, um die wechselnde Auslastung Ihrer Anwendung zu bewältigen. Sie können konfigurieren, dass Einstellungen für die automatische Skalierung basierend auf Metriken ausgelöst werden, mit denen die Auslastung oder die Leistung angezeigt werden, oder die Auslösung kann zu einem geplanten Datum bzw. einer geplanten Uhrzeit erfolgen. In diesem Artikel wird die Anatomie einer Einstellung für die automatische Skalierung ausführlich beschrieben. Zu Beginn des Artikels machen Sie sich mit dem Schema und den Eigenschaften einer Einstellung vertraut, und anschließend werden die verschiedenen Profiltypen beschrieben, die konfiguriert werden können. Am Ende wird beschrieben, wie bei der automatischen Skalierung ausgewertet wird, welches Profil jeweils zu einem bestimmten Zeitpunkt ausgeführt werden soll.

## <a name="autoscale-setting-schema"></a>Schema der Einstellung für die automatische Skalierung
Zur Veranschaulichung des Schemas der Einstellung für die automatische Skalierung wird die folgende Einstellung verwendet. Es ist wichtig zu beachten, dass diese Einstellung für die automatische Skalierung über Folgendes verfügt:
- Ein Profil 
- Dieses Profil enthält zwei Metrikregeln: eine für das horizontale Hochskalieren und eine für das horizontale Herunterskalieren.
- Die Regel für das horizontale Hochskalieren wird ausgelöst, wenn der Durchschnittswert für die Metrik „CPU in Prozent“ für die VM-Skalierungsgruppe in den letzten zehn Minuten über 85% gelegen hat.
- Die Regel für das horizontale Herunterskalieren wird ausgelöst, wenn der Durchschnittswert der VM-Skalierungsgruppe in der letzten Minute unter 60% gelegen hat.

> [!NOTE]
> Eine Einstellung kann mehrere Profile umfassen. Weitere Informationen hierzu finden Sie im Abschnitt zu den [Profilen](#autoscale-profiles).
> Ein Profil kann auch über mehrere definierte Regeln für das horizontale Hoch- und Herunterskalieren verfügen. Informationen zur Evaluierung finden Sie im Abschnitt zur [Evaluierung](#autoscale-evaluation).

```JSON
{
  "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/setting1",
  "name": "setting1",
  "type": "Microsoft.Insights/autoscaleSettings",
  "location": "East US",
  "properties": {
    "enabled": true,
    "targetResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
    "profiles": [
      {
        "name": "mainProfile",
        "capacity": {
          "minimum": "1",
          "maximum": "4",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 85
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
    {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 60
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ]
  }
}
```

| Abschnitt | Elementname | BESCHREIBUNG |
| --- | --- | --- |
| Einstellung | ID | Die Ressourcen-ID der Einstellung für die automatische Skalierung. Einstellungen für die automatische Skalierung sind eine Azure Resource Manager-Ressource. |
| Einstellung | name | Der Name der Einstellung für die automatische Skalierung. |
| Einstellung | location | Der Speicherort der Einstellung für die automatische Skalierung. Dieser Speicherort kann sich vom Speicherort der zu skalierenden Ressource unterscheiden. |
| Eigenschaften | targetResourceUri | Die Ressourcen-ID der zu skalierenden Ressource. Sie können pro Ressource nur eine Einstellung für die automatische Skalierung verwenden. |
| Eigenschaften | Profile | Eine Einstellung für die automatische Skalierung besteht aus einem oder mehreren Profilen. Bei jeder Ausführung des Moduls für die automatische Skalierung wird ein Profil ausgeführt. |
| Profil | name | Der Name des Profils. Sie können einen beliebigen Namen wählen, der Ihnen die Identifizierung des Profils erleichtert. |
| Profil | Capacity.maximum | Die maximal zulässige Kapazität. Hiermit wird sichergestellt, dass bei der automatischen Skalierung während der Ausführung dieses Profils für Ihre Ressourcen keine Skalierung über diesen Wert hinaus durchgeführt wird. |
| Profil | Capacity.minimum | Die zulässige Mindestkapazität. Hiermit wird sichergestellt, dass bei der automatischen Skalierung während der Ausführung dieses Profils für Ihre Ressourcen keine Skalierung durchgeführt wird, die zu einer niedrigeren Kapazität führt. |
| Profil | Capacity.default | Falls ein Problem beim Lesen der Ressourcenmetriken vorliegt (hier die CPU von „vmss1“) und die aktuelle Kapazität unterhalb der Standardkapazität liegt, wird von der automatischen Skalierung zur Sicherstellung der Verfügbarkeit der Ressource das horizontale Hochskalieren auf den Standardwert durchgeführt. Wenn die aktuelle Kapazität bereits über der Standardkapazität liegt, wird die automatische Skalierung nicht horizontal herunterskaliert. |
| Profil | Regeln | Die automatische Skalierung skaliert automatisch zwischen der maximalen und minimalen Kapazität, indem die Regeln im Profil verwendet werden. Sie können in einem Profil mehrere Regeln nutzen. Das grundlegende Szenario besteht darin, zwei Regeln zu verwenden: eine zum Ermitteln des Zeitpunkts für das horizontale Hochskalieren und die andere zum Ermitteln des Zeitpunkts für das horizontale Herunterskalieren. |
| Regel | metricTrigger | Definiert die Metrikbedingung der Regel. |
| metricTrigger | metricName | Der Name der Metrik. |
| metricTrigger |  metricResourceUri | Die Ressourcen-ID der Ressource, von der die Metrik ausgegeben wird. Meistens ist dies auch die zu skalierende Ressource. In einigen Fällen kann es sich auch um eine andere Ressource handeln. Sie können beispielsweise eine VM-Skalierungsgruppe basierend auf der Anzahl von Nachrichten in einer Speicherwarteschlange skalieren. |
| metricTrigger | timeGrain | Die Dauer der Stichprobenentnahme aus den Metriken. Beispiel: Bei TimeGrain = „PT1M“ werden die Metriken jede Minute über die unter „statistic“ angegebene Aggregationsmethode aggregiert. |
| metricTrigger | statistic | Dies ist die Aggregationsmethode innerhalb des timeGrain-Zeitraums. Beispiel: statistic = „Average“ und timeGrain = „PT1M“ bedeutet, dass die Metriken jede Minute anhand des Durchschnitts aggregiert werden. Diese Eigenschaft gibt vor, wie die Stichprobennahme für die Metrik erfolgt. |
| metricTrigger | timeWindow | Gibt an, wie weit auf Metriken zurückgegriffen werden soll. timeWindow = „PT10M“ bedeutet beispielsweise, dass bei jeder Ausführung der automatischen Skalierung die Metriken der letzten zehn Minuten abgefragt werden. Dieses Zeitfenster ermöglicht, dass Ihre Metriken normalisiert werden, und eine Reaktion auf vorübergehende Spitzen wird vermieden. |
| metricTrigger | timeAggregation | Die Aggregationsmethode, die zum Aggregieren der Stichprobenmetriken verwendet wird. Mit TimeAggregation = „Average“ werden die Stichprobenmetriken beispielsweise aggregiert, indem der Durchschnittswert herangezogen wird. Im obigen Fall wird der Mittelwert für die zehn 1-Minuten-Stichproben ermittelt. |
| Regel | scaleAction | Die Aktion, die nach dem Auslösen des metricTrigger-Elements der Regel durchgeführt wird. |
| scaleAction | direction | Mit „Increase“ wird horizontal hochskaliert, und mit „Decrease“ wird horizontal herunterskaliert.|
| scaleAction | value | Der Wert für die Erhöhung bzw. Verringerung der Kapazität einer Ressource. |
| scaleAction | cooldown | Gibt an, wie lange nach einem Skalierungsvorgang gewartet werden soll, bevor erneut skaliert wird. Für cooldown = „PT10M“ wird nach einem Skalierungsvorgang von der automatischen Skalierung zehn Minuten lang nicht versucht, einen neuen Vorgang zu starten. Der Zweck des cooldown-Vorgangs besteht darin, dass sich die Metriken nach dem Hinzufügen bzw. Entfernen von Instanzen stabilisieren können. |

## <a name="autoscale-profiles"></a>Profile für die automatische Skalierung

Es gibt drei Arten von Profilen für die automatische Skalierung:

1. **Reguläres Profil:** Dies ist das am häufigsten verwendete Profil. Wenn Sie Ihre Ressource nicht basierend auf dem Wochentag jeweils anders skalieren müssen, reicht es aus, in der Einstellung für die automatische Skalierung ein reguläres Profil einzurichten. Dieses Profil kann dann mit Metrikregeln konfiguriert werden, die vorgeben, wann horizontal hoch- oder herunterskaliert werden soll. Sie sollten nur ein reguläres Profil definieren.

    Das Beispielprofil weiter oben in diesem Artikel ist ein Beispiel für ein reguläres Profil. Es ist auch möglich, ein Profil für die Skalierung auf eine statische Instanzanzahl für Ihre Ressource festzulegen.

2. **Profil für ein festgelegtes Datum:** Angenommen, Sie haben ein reguläres Profil definiert, und am 26. Dezember 2017 (PST) steht ein wichtiges Ereignis an. Sie möchten erreichen, dass die minimale bzw. maximale Kapazität Ihrer Ressource an diesem Tag anders ist, aber für die Skalierung sollen trotzdem die gleichen Metriken verwendet werden. In diesem Fall sollten Sie der Profilliste Ihrer Einstellung ein Profil für ein festgelegtes Datum hinzufügen. Das Profil ist nur für die Ausführung am Tag des Ereignisses konfiguriert. An allen anderen Tagen wird das reguläre Profil ausgeführt.

    ``` JSON
    "profiles": [{
    "name": " regularProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    },
    {
    ...
    }]
    },
    {
    "name": "eventProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    }, {
    ...
    }],
    "fixedDate": {
        "timeZone": "Pacific Standard Time",
               "start": "2017-12-26T00:00:00",
               "end": "2017-12-26T23:59:00"
    }}
    ]
    ```
    
3. **Wiederholungsprofil:** Bei diesem Profiltyp können Sie sicherstellen, dass es immer an einem bestimmten Wochentag verwendet wird. Wiederholungsprofile verfügen nur über eine Startzeit und werden daher so lange ausgeführt, bis das nächste Wiederholungsprofil oder Profil für ein festgelegtes Datum beginnt. Bei einer Einstellung für die automatische Skalierung mit nur einem Wiederholungsprofil wird dieses Profil auch dann ausgeführt, wenn unter derselben Einstellung ein reguläres Profil definiert ist. In den beiden folgenden Beispielen wird die Nutzung dieses Profils veranschaulicht:

    **Beispiel 1: Wochentage und Wochenenden** Angenommen, an Wochenenden soll Ihre maximale Kapazität „4“ betragen, aber da Sie an Wochentagen eine höhere Auslastung erwarten, soll die maximale Kapazität dann „10“ betragen. In diesem Fall enthält Ihre Einstellung zwei Wiederholungsprofile: eins für die Ausführung an Wochenenden und das andere für die Wochentage.
    Die Einstellung würde wie folgt aussehen:

    ``` JSON
    "profiles": [
    {
    "name": "weekdayProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }}
    },
    {
    "name": "weekendProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Saturday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```

    In der vorherigen Einstellung sehen Sie, dass jedes Wiederholungsprofil über einen Zeitplan verfügt. Anhand dieses Zeitplans wird festgelegt, wann die Ausführung des Profils gestartet wird. Die Ausführung des Profils wird beendet, wenn die Ausführung eines anderen Profils ansteht.

    In der obigen Einstellung ist „weekdayProfile“ beispielsweise auf Montag um 0:00 Uhr (12 a.m.) festgelegt. Dies bedeutet, dass die Ausführung dieses Profils am Montag um 0:00 Uhr beginnt. Die Ausführung endet am Samstag um 0:00 Uhr, da für diesen Zeitpunkt der Ausführungsstart von „weekendProfile“ geplant ist.

    **Beispiel 2: Geschäftszeiten** Wir sehen uns nun ein weiteres Beispiel an. Angenommen, Sie möchten den Metrikschwellenwert „x“ für die Geschäftszeiten von 9 Uhr bis 17 Uhr und dann für 17 Uhr bis 9 Uhr am nächsten Tag den Metrikschwellenwert „y“ festlegen.
    Die Einstellung würde wie folgt aussehen:
    
    ``` JSON
    "profiles": [
    {
    "name": "businessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                9
            ],
            "minutes": [
                0
            ]
        }
    }
    },
    {
    "name": "nonBusinessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                17
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```
    
    Wir sehen in der vorherigen Einstellung, dass die Ausführung von „businessHoursProfile“ am Montag um 9 Uhr beginnt und bis 17 Uhr dauert, da zu diesem Zeitpunkt die Ausführung von „nonBusinessHoursProfile“ beginnt. „nonBusinessHoursProfile“ wird bis 9 Uhr am Dienstag ausgeführt, und dann beginnt „businessHoursProfile“. Dies wiederholt sich bis Freitag 17 Uhr, und „nonBusinessHoursProfile“ wird dann bis Montag 9 Uhr ausgeführt, da „businessHoursProfile“ erst wieder am Montag um 9 Uhr beginnt.
    
> [!Note]
> In der Benutzeroberfläche für die automatische Skalierung im Azure-Portal werden Endzeiten für Wiederholungsprofile erzwungen, und die Ausführung des Standardprofils für die Einstellung für die automatische Skalierung beginnt in den Zeiträumen zwischen den Wiederholungsprofilen.
    
## <a name="autoscale-evaluation"></a>Auswertung der automatischen Skalierung
Da Einstellungen für die automatische Skalierung über mehrere Profile für die automatische Skalierung verfügen können und jedes Profil mehrere Metrikregeln aufweisen kann, ist es wichtig zu verstehen, wie eine Einstellung für die automatische Skalierung ausgewertet wird. Bei jeder Ausführung des Auftrags für die automatische Skalierung wird zuerst das zutreffende Profil ausgewählt, und anschließend werden die Mindest- und Höchstwerte sowie alle Metrikregeln des Profils ausgewertet. Außerdem wird die Entscheidung getroffen, ob eine Skalierungsaktion erforderlich ist.

### <a name="which-profile-will-autoscale-pick"></a>Welches Profil wird von der automatischen Skalierung ausgewählt?
- Bei der automatischen Skalierung wird zuerst nach einem Profil für ein festgelegtes Datum gesucht, das für die sofortige Ausführung konfiguriert ist. Wenn ja, wird es von der automatischen Skalierung ausgeführt. Wenn mehrere Profile für ein festgelegtes Datum vorhanden sind, die ausgeführt werden sollen, wählt die automatische Skalierung das erste Profil aus.
- Falls keine Profile für ein festgelegtes Datum vorhanden sind, sucht die automatische Skalierung nach Wiederholungsprofilen. Wenn die Suche erfolgreich ist, wird das entsprechende Profil ausgeführt.
- Sind keine Profile für ein festgelegtes Datum oder Wiederholungsprofile vorhanden, führt die automatische Skalierung das reguläre Profil aus.

### <a name="how-does-autoscale-evaluate-multiple-rules"></a>Wie werden von der automatischen Skalierung mehrere Regeln ausgewertet?

Nachdem die automatische Skalierung ermittelt hat, welches Profil ausgeführt werden soll, werden zuerst alle Regeln für das horizontale Hochskalieren ausgewertet, die im Profil vorhanden sind (Regeln mit direction = „Increase“).
- Wenn eine oder mehrere Regeln für das horizontale Hochskalieren ausgelöst werden, berechnet die automatische Skalierung die neue Kapazität die in diesen Regeln jeweils per scaleAction bestimmt wird. Anschließend wird das horizontale Hochskalieren auf den Höchstwert dieser Kapazitäten durchgeführt, um die Dienstverfügbarkeit sicherzustellen.
- Beispiel: Angenommen, es sind eine VM-Skalierungsgruppe mit einer aktuellen Kapazität von 10 und zwei Regeln für das horizontale Hochskalieren vorhanden: eine zum Erhöhen der Kapazität um 10% und eine zum Erhöhen der Kapazität um den Wert 3. Die erste Regel ergibt eine neue Kapazität von 11, und die zweite Regel führt zu einem Kapazitätswert von 13. Zur Sicherstellung der Dienstverfügbarkeit wählt die automatische Skalierung die Aktion, die zur höchsten Kapazität führt. Also wird die zweite Regel gewählt.

Falls keine Regeln für das horizontale Hochskalieren ausgelöst werden, wertet die automatische Skalierung alle Regeln für das horizontale Herunterskalieren aus (mit direction = „Decrease“). Bei der automatischen Skalierung wird nur eine Aktion für das horizontale Herunterskalieren verwendet, wenn alle Regeln für das horizontale Herunterskalieren ausgelöst werden.
- Bei der automatischen Skalierung wird die neue Kapazität berechnet, die mit scaleAction für die einzelnen Regeln ermittelt wird. Anschließend wird die Skalierungsaktion gewählt, die zum maximalen Wert für diese Kapazitäten führt, um die Dienstverfügbarkeit sicherzustellen.
- Beispiel: Angenommen, es sind eine VM-Skalierungsgruppe mit einer aktuellen Kapazität von 10 und zwei Regeln für das horizontale Herunterskalieren vorhanden: eine zum Verringern der Kapazität um 50% und eine zum Verringern der Kapazität um den Wert 3. Die erste Regel ergibt eine neue Kapazität von 5, und die zweite Regel führt zu einem Kapazitätswert von 7. Zur Sicherstellung der Dienstverfügbarkeit wählt die automatische Skalierung die Aktion, die zur höchsten Kapazität führt. Also wird die zweite Regel gewählt.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur automatischen Skalierung finden Sie in den folgenden Ressourcen:

* [Übersicht über die automatische Skalierung](monitoring-overview-autoscale.md)
* [Allgemeine Metriken für die automatische Skalierung in Azure Monitor](insights-autoscale-common-metrics.md)
* [Best Practices für die automatische Skalierung in Azure Monitor](insights-autoscale-best-practices.md)
* [Verwenden von automatischen Skalierungsvorgängen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen](insights-autoscale-to-webhook-email.md)
* [REST-API für die automatische Skalierung](https://msdn.microsoft.com/library/dn931953.aspx)
