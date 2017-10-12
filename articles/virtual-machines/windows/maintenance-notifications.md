---
title: "Behandeln von Wartungsbenachrichtigungen für virtuelle Windows-Computer in Azure | Microsoft-Dokumentation"
description: "Zeigen Sie Wartungsbenachrichtigungen für in Azure ausgeführte virtuelle Windows-Computer an, und starten Sie eine Self-Service-Wartung."
services: virtual-machines-windows
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: zivr
ms.openlocfilehash: 90129bee5771534b01ac12473c6cfed77840ae20
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="handling-planned-maintenance-notifications-for-windows-virtual-machines"></a>Behandeln von Benachrichtigungen zu geplanten Wartungen für virtuelle Windows-Computer

Azure führt regelmäßig Updates durch, um die Zuverlässigkeit, Leistung und Sicherheit der Hostinfrastruktur für virtuelle Computer zu verbessern. Zu Updates zählen Änderungen wie das Patchen der Hostumgebung oder das Upgraden und die Außerbetriebnahme von Hardware. Die meisten dieser Updates werden ohne Auswirkungen auf die gehosteten virtuellen Computer durchgeführt. Es gibt jedoch Ausnahmen:

- Wenn die Wartung keinen Neustart erfordert, verwendet Azure eine direkte Migration, um den virtuellen Computer anzuhalten, während der Host aktualisiert wird.

- Wenn die Wartung einen Neustart erfordert, werden Sie in einer Benachrichtigung über den geplanten Wartungstermin informiert. In diesen Fällen steht Ihnen ein Zeitfenster zur Verfügung, in dem Sie die Wartung zu einem für Sie günstigen Zeitpunkt selbst starten können.


Geplante Wartungen, die einen Neustart erfordern, werden in Wellen geplant. Jede Welle hat einen anderen Umfang (Regionen).

- Eine Welle beginnt mit einer Kundenbenachrichtigung. Die Benachrichtigung wird standardmäßig an Abonnementbesitzer und -mitbesitzer gesendet. Für die Benachrichtigungen können weitere Empfänger und Nachrichtenoptionen wie E-Mail, SMS und Webhooks hinzugefügt werden.  
- Nach der Benachrichtigung wird zeitnah ein Self-Service-Zeitfenster festgelegt. Während dieses Zeitfensters können Sie ermitteln, welche Ihrer virtuellen Computer von der Welle betroffen sind, und die Wartung mittels proaktiver erneuter Bereitstellung starten. 
- Im Anschluss an das Self-Service-Zeitfenster beginnt das Zeitfenster für die geplante Wartung. An diesem Punkt plant Azure die erforderliche Wartung und wendet sie auf Ihren virtuellen Computer an. 

Mit den beiden Zeitfenstern möchten wir Ihnen einerseits genügend Zeit geben, um die Wartung zu initiieren und Ihren virtuellen Computer neu zu starten, und Sie andererseits darüber informieren, wann Azure die Wartung automatisch startet.


Sie können das Azure-Portal, PowerShell, die REST-API und die Befehlszeilenschnittstelle verwenden, um die Wartungszeitfenster für Ihre virtuellen Computer abzufragen und eine Self-Service-Wartung zu starten.

 > [!NOTE]
 > Wenn sich die Wartung nicht erfolgreich starten lässt, markiert Azure Ihren virtuellen Computer als **Übersprungen** und startet ihn während des Zeitfensters für die geplante Wartung nicht neu. Stattdessen werden Sie zu einem späteren Zeitpunkt mit einem neuen Zeitplan kontaktiert. 


[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]

## <a name="check-maintenance-status-using-powershell"></a>Überprüfen des Wartungsstatus mithilfe von PowerShell

Sie können auch mithilfe von Azure PowerShell prüfen, wann die Wartung Ihrer virtuellen Computer geplant ist. Informationen zur geplanten Wartung können mit dem Cmdlet [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) unter Verwendung des Parameters `-status` ermittelt werden.
 
Wartungsinformationen werden nur zurückgegeben, wenn eine Wartung geplant ist. Ist keine Wartung für den virtuellen Computer geplant, gibt das Cmdlet keine Wartungsinformationen zurück. 

```powershell
Get-AzureRmVM -ResourceGroupName rgName -Name vmName -Status
```

Unter „MaintenanceRedeployStatus“ werden folgende Eigenschaften zurückgegeben: 
| Wert | Beschreibung   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Gibt an, ob Sie zum aktuellen Zeitpunkt die Wartung für den virtuellen Computer starten können. ||
| PreMaintenanceWindowStartTime         | Der Anfang des Self-Service-Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| PreMaintenanceWindowEndTime           | Das Ende des Self-Service-Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| MaintenanceWindowStartTime            | Der Anfang des geplanten Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| MaintenanceWindowEndTime              | Das Ende des geplanten Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| LastOperationResultCode               | Das Ergebnis des letzten Wartungsinitiierungsversuchs für den virtuellen Computer. ||



Sie können auch den Wartungsstatus für alle virtuellen Computer in einer Ressourcengruppe abrufen. Verwenden Sie hierzu [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) ohne Angabe eines virtuellen Computers.
 
```powershell
Get-AzureRmVM -ResourceGroupName rgName --Status
```

Die folgende PowerShell-Funktion gibt auf der Grundlage Ihrer Abonnement-ID eine Liste mit virtuellen Computern aus, für die eine Wartung geplant ist.

```powershell

function MaintenanceIterator
{
    Select-AzureRmSubscription -SubscriptionId $args[0]

    $rgList= Get-AzureRmResourceGroup 

    for ($rgIdx=0; $rgIdx -lt $rgList.Length ; $rgIdx++)
    {
        $rg = $rgList[$rgIdx]
        $vmList = Get-AzureRMVM -ResourceGroupName $rg.ResourceGroupName 
        for ($vmIdx=0; $vmIdx -lt $vmList.Length ; $vmIdx++)
        {
            $vm = $vmList[$vmIdx]
            $vmDetails = Get-AzureRMVM -ResourceGroupName $rg.ResourceGroupName -Name $vm.Name -Status
              if ($vmDetails.MaintenanceRedeployStatus )
            {
                Write-Output "VM: $($vmDetails.Name)  IsCustomerInitiatedMaintenanceAllowed: $($vmDetails.MaintenanceRedeployStatus.IsCustomerInitiatedMaintenanceAllowed) $($vmDetails.MaintenanceRedeployStatus.LastOperationMessage)"               
            }
          }
    }
}

```

### <a name="start-maintenance-on-your-vm-using-powershell"></a>Starten der Wartung für Ihren virtuellen Computer mithilfe von PowerShell

Der folgende Befehl enthält Informationen aus der Funktion des vorherigen Abschnitts und startet die Wartung für einen virtuellen Computer, sofern **IsCustomerInitiatedMaintenanceAllowed** auf „true“ festgelegt ist.

```powershell
Restart-AzureRmVM -PerformMaintenance -name $vm.Name -ResourceGroupName $rg.ResourceGroupName 
```
 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie sich über einen virtuellen Computer mithilfe von [geplanten Ereignissen](scheduled-events.md) für Wartungsereignisse registrieren.