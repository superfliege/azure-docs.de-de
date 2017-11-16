---
title: "Erkennen von Geräteproblemen in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation"
description: "In diesem Tutorial wird erläutert, wie Sie mithilfe von Regeln und Aktionen automatisch schwellenwertbasierte Geräteprobleme in der Remoteüberwachungslösung erkennen können."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 173ffbdd70313ef5a0d2af2cf1c8996d2395274a
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="detect-issues-using-threshold-based-rules"></a>Erkennen von Problemen mithilfe schwellenwertbasierter Regeln

In diesem Tutorial werden die Funktionen des Regelmoduls in der Remoteüberwachungslösung veranschaulicht. Zur Einführung dieser Funktionen wird in diesem Tutorial ein Szenario in der Contoso IoT-Anwendung verwendet.

Contoso verfügt über eine Regel, die eine kritische Warnung generiert, wenn der von einem Gerät des Typs **Chiller** gemeldete Druck 250 PSI überschreitet. Als Bediener möchten Sie **Chiller**-Geräte identifizieren, die möglicherweise problematische Sensoren aufweisen, indem Sie nach Anfangsdruckspitzen suchen. Um diese Geräte zu identifizieren, erstellen Sie eine Regel, die eine Warnung generiert, wenn der Druck 150 PSI überschreitet.

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Anzeigen der Regeln in der Lösung
> * Neue Regel erstellen
> * Bearbeiten einer vorhandenen Regel
> * Löschen einer Regel

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz der Remoteüberwachungslösung in Ihrem Azure-Abonnement.

Falls Sie die Remoteüberwachungslösung noch nicht bereitgestellt haben, ist es ratsam, das Tutorial [Bereitstellen der vorkonfigurierten Remoteüberwachungslösung](iot-suite-remote-monitoring-deploy.md) durchzuarbeiten.

## <a name="view-the-rules-in-your-solution"></a>Anzeigen der Regeln in der Lösung

Auf der Seite **Rules & Actions** (Regeln und Aktionen) in der Lösung wird eine Liste aller aktuellen Regeln angezeigt:

![Seite „Rules and Actions“ (Regeln und Aktionen)](media/iot-suite-remote-monitoring-automate/rulesactions.png)

Wenden Sie einen Filter an, um nur die Regeln anzuzeigen, die für **Chiller**-Geräte gelten:

![Filtern der Regelliste](media/iot-suite-remote-monitoring-automate/rulesactionsfilter.png)

Sie können weitere Informationen zu einer Regel anzeigen und diese bearbeiten, wenn Sie sie in der Liste auswählen:

![Anzeigen der Regeldetails](media/iot-suite-remote-monitoring-automate/rulesactionsdetail.png)

Wählen Sie zum Deaktivieren, Aktivieren oder Löschen einer oder mehrerer Regeln mehrere Regeln in der Liste aus:

![Auswählen mehrerer Regeln](media/iot-suite-remote-monitoring-automate/rulesactionsmultiselect.png)

## <a name="create-a-new-rule"></a>Neue Regel erstellen

Um eine neue Regel hinzuzufügen, die eine Warnung generiert, wenn der Druck in einem **Chiller**-Gerät 150 PSI überschreitet, wählen Sie **New rule** (Neue Regel) aus:

![Erstellen einer Regel](media/iot-suite-remote-monitoring-automate/rulesactionsnewrule.png)

Verwenden Sie zum Erstellen der Regel die folgenden Werte:

| Einstellung          | Wert                                 |
| ---------------- | ------------------------------------- |
| Name             | Chiller warning                       |
| Quelle           | **Chiller** device group              |
| Trigger field    | Pressure                              |
| Trigger operator | Größer als                          |
| Trigger value    | 150                                   |
| Schweregrad   | Warnung                               |
| Alarm event text | Chiller pressure has exceeded 150 PSI |

Um die neue Regel zu speichern, wählen Sie **Apply** (Übernehmen) aus.

Sie können den Zeitpunkt, zu dem die Regel ausgelöst wird, auf der Seite **Rules & Actions** (Regeln und Aktionen) oder auf der Seite **Dashboard** anzeigen.

## <a name="edit-an-existing-rule"></a>Bearbeiten einer vorhandenen Regel

Um eine Änderung an einer vorhandenen Regel vorzunehmen, wählen Sie sie in der Regelliste aus. Wählen Sie dann im Bereich **Rule Detail** (Regeldetails) die Option **Edit mode** (Bearbeitungsmodus) aus.

![Bearbeiten einer Regel](media/iot-suite-remote-monitoring-automate/rulesactionsedit.png)

## <a name="disable-a-rule"></a>Deaktivieren einer Regel

Um eine Regel vorübergehend auszuschalten, können Sie sie in der Regelliste deaktivieren. Wählen Sie die zu deaktivierende Regel und dann **Disable** (Deaktivieren) aus. Der **Status** der Regel in der Liste ändert sich, um anzugeben, dass die Regel jetzt deaktiviert ist. Sie können eine zuvor deaktivierte Regel mit dem gleichen Verfahren wieder aktivieren.

![Deaktivieren einer Regel](media/iot-suite-remote-monitoring-automate/rulesactionsdisable.png)

Sie können mehrere Regeln gleichzeitig aktivieren und deaktivieren, indem Sie mehrere Regeln in der Liste auswählen.

## <a name="delete-a-rule"></a>Löschen einer Regel

Um eine Regel dauerhaft zu löschen, wählen Sie sie in der Regelliste aus, und wählen Sie dann **Löschen** aus.

Sie können mehrere Regeln gleichzeitig löschen, indem Sie mehrere Regeln in der Liste auswählen.

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