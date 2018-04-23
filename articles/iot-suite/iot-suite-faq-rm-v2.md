---
title: Azure IoT Suite-Remoteüberwachung – häufig gestellte Fragen | Microsoft-Dokumentation
description: Häufig gestellte Fragen zur vorkonfigurierten IoT Suite-Remoteüberwachungslösung
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: b0d6dbb567f66537c599d7ac3e4337988b3374c0
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="frequently-asked-questions-for-iot-suite-remote-monitoring-preconfigured-solution"></a>Häufig gestellte Fragen zur vorkonfigurierten IoT Suite-Remoteüberwachungslösung

Informationen finden Sie auch in den allgemeinen [häufig gestellten Fragen](iot-suite-faq.md).

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Wie hoch sind die Kosten für die Bereitstellung der neuen Remoteüberwachungslösung?

Die neue vorkonfigurierte Lösung bietet zwei Bereitstellungsoptionen:

* Die Option *Basic* für Entwickler, die niedrigere Entwicklungskosten wünschen, oder für Kunden, die eine Demonstrationsversion oder ein Proof of Concept erstellen möchten.
* Die Option *Standard* für Unternehmen, die eine produktionsbereite Infrastruktur bereitstellen möchten.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>Wie kann ich sicherstellen, dass ich die Kosten beim Entwickeln meiner Lösung gering halte?

Zusätzlich zur Bereitstellung von zwei unterschiedlichen Bereitstellungen verfügt die neue Remoteüberwachungslösung über eine Einstellung zum Aktivieren oder Deaktivieren aller simulierten Geräte nach Bedarf. Durch die Deaktivierung der Simulation werden die in der Lösung erfassten Daten und somit die Gesamtkosten reduziert.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Worin besteht der Unterschied zwischen der Basic- und der Standard-Bereitstellung? Wie treffe ich die Wahl zwischen den beiden Bereitstellungsoptionen?

Jede Bereitstellungsoption entspricht unterschiedlichen Anforderungen. Die Basic-Bereitstellung dient dem Einstieg und der Entwicklung eines Proof of Concept und kleiner Pilotprojekte. Sie bietet eine optimierte Architektur mit den erforderlichen Mindestressourcen und geringen Kosten. Die Standard-Bereitstellung dient dem Erstellen und Anpassen einer produktionsbereiten Lösung und bietet eine Bereitstellung mit allen dafür erforderlichen Elementen. Für Zuverlässigkeit und Skalierbarkeit werden Anwendungsmicroservices als Docker-Container erstellt und über einen Orchestrator bereitgestellt (standardmäßig ist Kubernetes ausgewählt). Der Orchestrator ist für die Bereitstellung, Skalierung und Verwaltung der Anwendung zuständig. Sie sollten eine Option basierend auf Ihren aktuellen Anforderungen auswählen. Je nach Projektphase können Sie jeweils eine der beiden Optionen oder eine Kombination aus beiden verwenden.

### <a name="how-do-i-configure-a-dynamic-map-on-the-dashboard"></a>Wie konfiguriere ich eine dynamische Zuordnung auf dem Dashboard?

Weitere Informationen finden Sie unter [Aktualisieren des Zuordnungsschlüssels zum Anzeigen von Geräten in einer dynamischen Zuordnung](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map).

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Wie viele Bing Maps-APIs im Tarif „Free“ kann ich in einem Abonnement bereitstellen?

Zwei. In einem Azure-Abonnement können für Bing Karten für Unternehmen nur zwei Tarife für die erste interne Transaktionsebene erstellt werden. Die Remoteüberwachungslösung wird standardmäßig mit dem Tarif für die erste interne Transaktionsebene bereitgestellt. Daher können Sie in einem Abonnement ohne Modifikationen nur bis zu zwei vorkonfigurierte Remoteüberwachungslösungen bereitstellen.

### <a name="next-steps"></a>Nächste Schritte

Sie können auch einige andere Features und Funktionen der vorkonfigurierten IoT Suite-Lösungen ausprobieren:

* [Erkunden der Funktionen der vorkonfigurierten Remoteüberwachungslösung](iot-suite-remote-monitoring-explore.md)
* [Übersicht über die vorkonfigurierte Predictive Maintenance-Lösung](iot-suite-predictive-overview.md)
* [Übersicht über die vorkonfigurierte Lösung für eine verbundene Factory](iot-suite-connected-factory-overview.md)
* [Sicherheit im Internet der Dinge von Anfang an](securing-iot-ground-up.md)