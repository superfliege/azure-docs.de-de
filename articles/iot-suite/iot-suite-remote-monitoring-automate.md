---
title: Erkennen von Geräteproblemen in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird erläutert, wie Sie mithilfe von Regeln und Aktionen automatisch schwellenwertbasierte Geräteprobleme in der Remoteüberwachungslösung erkennen können.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 5acf35ed19a5b6baa2885fd58cfb7fbbe1ac3cd8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="detect-issues-using-threshold-based-rules"></a>Erkennen von Problemen mithilfe schwellenwertbasierter Regeln

In diesem Tutorial werden die Funktionen der Regel-Engine in der Remoteüberwachungslösung veranschaulicht. Zur Einführung dieser Funktionen wird in diesem Tutorial ein Szenario in der Contoso IoT-Anwendung verwendet.

Contoso verfügt über eine Regel, die eine kritische Warnung generiert, wenn der von einem Gerät des Typs **Chiller** gemeldete Druck 250 PSI überschreitet. Als Bediener möchten Sie **Chiller**-Geräte identifizieren, die möglicherweise problematische Sensoren aufweisen, indem Sie nach Anfangsdruckspitzen suchen. Um diese Geräte zu identifizieren, erstellen Sie eine Regel, die eine Warnung generiert, wenn der Druck 150 PSI überschreitet.

Ihnen wurde auch gesagt, dass eine kritische Warnung ausgelöst werden muss, wenn die durchschnittliche Luftfeuchtigkeit des **Chiller**-Geräts in den letzten fünf Minuten über 80 Prozent lag und die Temperatur des **Chiller**-Geräts in den letzten fünf Minuten mehr als 75 Grad Fahrenheit betrug.

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Anzeigen der Regeln in der Lösung
> * Erstellen einer neuen Regel
> * Erstellen einer neuen Regel mit mehreren Bedingungen
> * Bearbeiten einer vorhandenen Regel
> * Löschen einer Regel

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz der Remoteüberwachungslösung in Ihrem Azure-Abonnement.

Sollten Sie die Remoteüberwachungslösung noch nicht bereitgestellt haben, absolvieren Sie zuerst das Tutorial [Bereitstellen des Solution Accelerators für die Remoteüberwachung](iot-suite-remote-monitoring-deploy.md).

## <a name="view-the-rules-in-your-solution"></a>Anzeigen der Regeln in der Lösung

In der Lösung wird auf der Seite **Regeln** eine Liste mit allen aktuellen Regeln angezeigt:

![Seite „Rules and Actions“ (Regeln und Aktionen)](media/iot-suite-remote-monitoring-automate/rulesactions_v2.png)

Wenden Sie einen Filter an, um nur die Regeln anzuzeigen, die für **Chiller**-Geräte gelten:

![Filtern der Regelliste](media/iot-suite-remote-monitoring-automate/rulesactionsfilter_v2.png)

Sie können weitere Informationen zu einer Regel anzeigen und diese bearbeiten, wenn Sie sie in der Liste auswählen:

![Anzeigen der Regeldetails](media/iot-suite-remote-monitoring-automate/rulesactionsdetail_v2.png)

Wählen Sie zum Deaktivieren, Aktivieren oder Löschen einer oder mehrerer Regeln mehrere Regeln in der Liste aus:

![Auswählen mehrerer Regeln](media/iot-suite-remote-monitoring-automate/rulesactionsmultiselect_v2.png)

## <a name="create-a-new-rule"></a>Neue Regel erstellen

Um eine neue Regel hinzuzufügen, die eine Warnung generiert, wenn der Druck in einem **Chiller**-Gerät 150 PSI überschreitet, wählen Sie **New rule** (Neue Regel) aus:

![Erstellen einer Regel](media/iot-suite-remote-monitoring-automate/rulesactionsnewrule_v2.png)

Verwenden Sie zum Erstellen der Regel die folgenden Werte:

| Einstellung          | Wert                                 |
| ---------------- | ------------------------------------- |
| Regelname        | Chiller warning                       |
| Beschreibung      | Chiller pressure has exceeded 150 PSI |
| Gerätegruppe     | **Chillers** device group             |
| Berechnung      | Sofort                               |
| Feld für die erste Bedingung| pressure                              |
| Operator für die erste Bedingung | Größer als                      |
| Wert für die erste Bedingung    | 150                               |
| Schweregrad  | Warnung                               |

Um die neue Regel zu speichern, wählen Sie **Apply** (Übernehmen) aus.

Der Auslösezeitpunkt der Regel wird auf der Seite **Regeln** und auf der Seite **Dashboard** angezeigt.

## <a name="create-a-new-rule-with-multiple-conditions"></a>Erstellen einer neuen Regel mit mehreren Bedingungen

Klicken Sie auf **Neue Regel**, um eine neue Regel mit mehreren Bedingungen zu erstellen, die eine kritische Warnung generiert, wenn die durchschnittliche Luftfeuchtigkeit des **Chiller**-Geräts in den letzten fünf Minuten über 80 Prozent lag und die Temperatur des **Chiller**-Geräts in den letzten fünf Minuten mehr als 75 Grad Fahrenheit betrug:

![Erstellen einer Regel mit mehreren Bedingungen](media/iot-suite-remote-monitoring-automate/rulesactionsnewrule_mult_v2.png)

Verwenden Sie zum Erstellen der Regel die folgenden Werte:

| Einstellung          | Wert                                 |
| ---------------- | ------------------------------------- |
| Regelname        | Chiller humidity and temp critical    |
| Beschreibung      | Humidity and temperature are critical |
| Gerätegruppe     | **Chillers** device group             |
| Berechnung      | Durchschnitt                               |
| Zeitraum      | 5                                     |
| Feld für die erste Bedingung| Luftfeuchtigkeit                              |
| Operator für die erste Bedingung | Größer als                      |
| Wert für die erste Bedingung    | 80                               |
| Schweregrad  | Kritisch                              |

Klicken Sie zum Hinzufügen der zweiten Bedingung auf „+ Bedingung hinzufügen“.

![Erstellen der zweiten Bedingung](media/iot-suite-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2.png)

Konfigurieren Sie die neue Bedingung mit folgenden Werten:

| Einstellung          | Wert                                 |
| ---------------- | ------------------------------------- |
| Feld für die zweite Bedingung| Temperatur                           |
| Operator für die zweite Bedingung | Größer als                      |
| Wert für die zweite Bedingung    | 75                                |

Um die neue Regel zu speichern, wählen Sie **Apply** (Übernehmen) aus.

Der Auslösezeitpunkt der Regel wird auf der Seite **Regeln** und auf der Seite **Dashboard** angezeigt.

## <a name="edit-an-existing-rule"></a>Bearbeiten einer vorhandenen Regel

Um eine Änderung an einer vorhandenen Regel vorzunehmen, wählen Sie sie in der Regelliste aus.

![Bearbeiten einer Regel](media/iot-suite-remote-monitoring-automate/rulesactionsedit_v2.png)

<!--## Disable a rule

To temporarily switch off a rule, you can disable it in the list of rules. Choose the rule to disable, and then choose **Disable**. The **Status** of the rule in the list changes to indicate the rule is now disabled. You can re-enable a rule that you previously disabled using the same procedure.

![Disable rule](media/iot-suite-remote-monitoring-automate/rulesactionsdisable.png)

You can enable and disable multiple rules at the same time if you select multiple rules in the list.-->

<!--## Delete a rule

To permanently delete a rule, choose the rule in the list of rules and then choose **Delete**.

You can delete multiple rules at the same time if you select multiple rules in the list.-->

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde Folgendes veranschaulicht:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Anzeigen der Regeln in der Lösung
> * Neue Regel erstellen
> * Bearbeiten einer vorhandenen Regel
> * Löschen einer Regel

Sie wissen nun, wie Probleme mithilfe schwellenwertbasierter Regeln erkannt werden. In den nächsten empfohlenen Schritten erfahren Sie Folgendes:

* [Verwalten und Konfigurieren von Geräten](./iot-suite-remote-monitoring-manage.md)
* [Beheben von Geräteproblemen](./iot-suite-remote-monitoring-maintain.md)
* [Testen der Lösung mit simulierten Geräten](iot-suite-remote-monitoring-test.md)

<!-- Next tutorials in the sequence -->