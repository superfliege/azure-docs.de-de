---
title: "Behandeln von Wartungsbenachrichtigungen für virtuelle Linux-Computer in Azure | Microsoft-Dokumentation"
description: "Zeigen Sie Wartungsbenachrichtigungen für in Azure ausgeführte virtuelle Linux-Computer an, und starten Sie eine Self-Service-Wartung."
services: virtual-machines-linux
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: zivr
ms.openlocfilehash: 2fc96898c9b7ebefc7648b5819975d9015e76c0c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="handling-planned-maintenance-notifications-for-linux-virtual-machines"></a>Behandeln von Benachrichtigungen zu geplanten Wartungen für virtuelle Linux-Computer

Azure führt regelmäßig Updates durch, um die Zuverlässigkeit, Leistung und Sicherheit der Hostinfrastruktur für virtuelle Computer zu verbessern. Zu Updates zählen Änderungen wie das Patchen der Hostumgebung oder das Upgraden und die Außerbetriebnahme von Hardware. Die meisten dieser Updates werden ohne Auswirkungen auf die gehosteten virtuellen Computer durchgeführt. Es gibt jedoch Ausnahmen:

- Wenn die Wartung keinen Neustart erfordert, verwendet Azure eine direkte Migration, um den virtuellen Computer anzuhalten, während der Host aktualisiert wird.

- Wenn die Wartung einen Neustart erfordert, werden Sie in einer Benachrichtigung über den geplanten Wartungstermin informiert. In diesen Fällen steht Ihnen ein Zeitfenster zur Verfügung, in dem Sie die Wartung zu einem für Sie günstigen Zeitpunkt selbst starten können.


Geplante Wartungen, die einen Neustart erfordern, werden in Wellen geplant. Jede Welle hat einen anderen Umfang (Regionen).

- Eine Welle beginnt mit einer Kundenbenachrichtigung. Die Benachrichtigung wird standardmäßig an Abonnementbesitzer und -mitbesitzer gesendet. Für die Benachrichtigungen können weitere Empfänger und Nachrichtenoptionen wie E-Mail, SMS und Webhooks hinzugefügt werden. 
- Nach der Benachrichtigung wird zeitnah ein Self-Service-Zeitfenster festgelegt. Während dieses Zeitfensters können Sie ermitteln, welche Ihrer virtuellen Computer von der Welle betroffen sind, und die Wartung mittels proaktiver erneuter Bereitstellung starten. 
- Im Anschluss an das Self-Service-Zeitfenster beginnt das Zeitfenster für die geplante Wartung. An diesem Punkt plant Azure die erforderliche Wartung und wendet sie auf Ihren virtuellen Computer an.  

Mit den beiden Zeitfenstern möchten wir Ihnen einerseits genügend Zeit geben, um die Wartung zu initiieren und Ihren virtuellen Computer neu zu starten, und Sie andererseits darüber informieren, wann Azure die Wartung automatisch startet.

Sie können die Azure-Befehlszeilenschnittstelle, PowerShell, die REST-API und das Azure-Portal verwenden, um die Wartungszeitfenster für Ihre virtuellen Computer abzufragen und eine Self-Service-Wartung zu starten.

 > [!NOTE]
 > Wenn sich die Wartung nicht erfolgreich starten lässt, markiert Azure Ihren virtuellen Computer als **Übersprungen** und startet ihn während des Zeitfensters für die geplante Wartung nicht neu. Stattdessen werden Sie zu einem späteren Zeitpunkt mit einem neuen Zeitplan kontaktiert. 

## <a name="find-vms-scheduled-for-maintenance-using-cli"></a>Ermitteln der virtuellen Computer mit geplanter Wartung über die Befehlszeilenschnittstelle

Informationen zu geplanten Wartungen können mithilfe von [azure vm get-instance-view](/cli/azure/vm?view=azure-cli-latest#az_vm_get_instance_view) angezeigt werden.
 
Wartungsinformationen werden nur zurückgegeben, wenn eine Wartung geplant ist. Ist keine Wartung für den virtuellen Computer geplant, gibt der Befehl keine Wartungsinformationen zurück. 

```azure-cli
az vm get-instance-view  - g rgName  -n vmName 
```

Unter „MaintenanceRedeployStatus“ werden folgende Werte zurückgegeben: 

| Wert | Beschreibung   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Gibt an, ob Sie zum aktuellen Zeitpunkt die Wartung für den virtuellen Computer starten können. ||
| PreMaintenanceWindowStartTime         | Der Anfang des Self-Service-Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| PreMaintenanceWindowEndTime           | Das Ende des Self-Service-Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| MaintenanceWindowStartTime            | Der Anfang des geplanten Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| MaintenanceWindowEndTime              | Das Ende des geplanten Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| LastOperationResultCode               | Das Ergebnis des letzten Wartungsinitiierungsversuchs für den virtuellen Computer. ||


## <a name="start-maintenance-on-your-vm-using-cli"></a>Starten der Wartung für Ihren virtuellen Computer über die Befehlszeilenschnittstelle

Der folgende Aufruf initiiert die Wartung für einen virtuellen Computer, wenn `IsCustomerInitiatedMaintenanceAllowed` auf „true“ festgelegt ist:

```azure-cli
az vm perform-maintenance rgName vmName 
```

[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie sich über einen virtuellen Computer mithilfe von [geplanten Ereignissen](scheduled-events.md) für Wartungsereignisse registrieren.