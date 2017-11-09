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
ms.date: 10/26/2017
ms.author: zivr
ms.openlocfilehash: fec64b3c499577af6b1d6eddb1c761ee0af73772
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
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

## <a name="faq"></a>Häufig gestellte Fragen


**F: Wozu müssen Sie meine virtuellen Computer jetzt neu starten?**

**A:** Während die Mehrzahl der Updates und Upgrades für die Azure-Plattform die Verfügbarkeit des virtuellen Computers nicht beeinflusst, gibt es Fälle, in denen wir den Neustart in Azure gehosteter virtueller Computer nicht vermeiden können. Bei uns sind mehrere Änderungen zusammengekommen, die den Neustart unserer Server erfordern und damit auch zum Neustart virtueller Computer führen.

**F: Bin ich sicher, wenn ich ihre Empfehlungen für hohe Verfügbarkeit mit Verwendung einer Verfügbarkeitsgruppe befolge?**

**A:** In einer Verfügbarkeitsgruppe oder VM-Skalierungsgruppe bereitgestellte VMs werden wie Updatedomänen (UD) behandelt. Bei der Wartung berücksichtigt Azure die UD-Einschränkung und startet virtuelle Computer nicht von einer anderen UD (innerhalb derselben Verfügbarkeitsgruppe) aus neu.  Azure wartet auch mindestens 30 Minuten vor dem Wechsel zur nächsten Gruppe von virtuellen Computern. 

Weitere Informationen über hohe Verfügbarkeit finden Sie unter „Verwalten der Verfügbarkeit Ihrer virtuellen Windows-Computer in Azure“ oder „Verwalten der Verfügbarkeit Ihrer virtuellen Linux-Computer in Azure“.

**F: Ich habe die Notfallwiederherstellung in einer anderen Region festgelegt. Bin ich sicher?**

**A:** Jede Azure-Region ist mit einer anderen Region innerhalb desselben Gebiets (z.B. USA, Europa oder Asien) kombiniert. Geplante Azure-Updates werden nacheinander in den Regionen eines Paars eingeführt, um Ausfallzeiten und das Risiko von Anwendungsausfällen zu minimieren. Während einer geplanten Wartung kann Azure ein ähnliches Fenster für Benutzer für den Beginn der Wartung planen, doch das geplante Wartungsfenster wird in den kombinierten Regionen unterschiedlich sein.  

Weitere Informationen zu Azure-Regionen finden Sie in „Regionen und Verfügbarkeit für virtuelle Computer in Azure“.  Sie können die vollständige Liste von Regionspaaren hieranzeigen.

**F: Wie werde ich über eine geplante Wartung benachrichtigt?**

**A:** Eine geplante Wartungsaktion beginnt mit dem Festlegen eines Zeitplans für eine oder mehrere Azure-Regionen. Bald danach wird eine E-Mail-Benachrichtigung an die Abonnementbesitzer gesendet (pro Abonnement eine E-Mail). Zusätzliche Kanäle und Empfänger für diese Benachrichtigung können mit Aktivitätsprotokollwarnungen konfiguriert werden. Falls Sie einen virtuellen Computer in einer Region bereitstellen, wo bereits Wartung geplant ist, erhalten Sie die Benachrichtigung nicht, sondern müssen vielmehr den Wartungsstatus des virtuellen Computers überprüfen.

**F: Ich sehe weder im Portal noch in Powershell oder CLI einen Hinweis auf geplante Wartung – was stimmt nicht?**

**A:** Informationen im Zusammenhang mit geplanter Wartung stehen während einer geplanten Wartungsaktion nur für die VMs zur Verfügung, die davon betroffen sind. D.h., wenn Ihnen keine Daten angezeigt werden, ist es möglich, dass die Wartungsaktion bereits abgeschlossen (oder nicht gestartet) wurde, bzw. Ihr virtueller Computer bereits auf einem aktualisierten Server gehostet wird.

**F: Sollte ich die Wartung auf meinem virtuellen Computer starten?**

**A:** Im Allgemeinen werden Workloads, die in einem Clouddienst, einer Verfügbarkeitsgruppe oder VM-Skalierungsgruppe bereitgestellt werden, von der geplanten Wartung nicht beeinträchtigt.  Während einer geplanten Wartung ist zum jeweiligen Zeitpunkt immer nur eine Updatedomäne betroffen. Die Reihenfolge der betroffenen Updatedomänen ist jedoch nicht unbedingt sequenziell.

Sie können die Wartung in den folgenden Fällen selbst starten:
- Ihre Anwendung wird auf einem einzelnen virtuellen Computer ausgeführt, und Sie müssen die gesamte Wartung außerhalb der Geschäftszeiten durchführen.
- Sie müssen die Zeit der Wartung als Teil Ihrer SLA koordinieren.
- Sie benötigen sogar innerhalb einer Verfügbarkeitsgruppe mehr als 30 Minuten zwischen einzelnen VM-Neustarts.
- Sie möchten die gesamte Anwendung herunterfahren (mehrere Ebenen, mehrere Updatedomänen), um die Wartung schneller abzuschließen.

**F: Gibt es eine Möglichkeit, dass ich genau feststellen kann, wann mein virtueller Computer betroffen ist?**

**A:** Bei Festlegung des Zeitplans definieren wir ein Zeitfenster von einigen Tagen. Allerdings ist die genaue Reihenfolge von Servern (und VMs) innerhalb dieses Zeitfensters unbekannt. Kunden, die die genaue Zeit für ihre virtuellen Computer wissen möchten, können geplante Ereignisse verwenden, aus dem virtuellen Computer heraus abfragen und erhalten 10 Minuten vor dem VM-Neustart eine Benachrichtigung.
  
**F: Wie lange dauert es, meinen virtuellen Computer neu zu starten?**

**A:** Je nach Größe Ihres virtuellen Computers kann der Neustart bis zu mehreren Minuten dauern. Beachten Sie, dass Ihnen bei Verwendung von Clouddiensten, Skalierungsgruppen oder Verfügbarkeitsgruppen 30 Minuten zwischen den einzelnen Gruppen von virtuellen Computern (UD) zur Verfügung stehen. 

**F: Womit muss ich bei Clouddiensten, Skalierungsgruppen und Service Fabric rechnen?**

**A:** Während diese Plattformen von geplanter Wartung betroffen sind, ist die Sicherheit für Kunden gewährleistet, die diese Plattformen verwenden, da nur jeweils virtuelle Computer in einer einzelnen Upgradedomäne (UD) betroffen sind.  

**F: Ich habe eine E-Mail-Nachricht zum Außerbetriebsetzen von Hardware erhalten – ist dies mit geplanter Wartung gleichzusetzen?**

**A:** Das Außerbetriebsetzen von Hardware ist zwar ein geplantes Wartungsereignis, doch wir haben diesen Anwendungsfall noch nicht in das neue Verfahren integriert.  Wir befürchten, es könnte Kunden verwirren, wenn sie zwei ähnliche E-Mails zu zwei verschiedenen geplanten Wartungsaktionen erhalten.

**F: Auf meinen virtuellen Computern werden keine Wartungsinformationen angezeigt. Welche Probleme sind aufgetreten?**

**A:** Es gibt verschiedene Gründe, warum Sie keine Wartungsinformationen auf Ihren virtuellen Computern sehen:
1.  Sie verwenden ein Abonnement, das als Microsoft-intern markiert ist.
2.  Ihre virtuellen Computer sind nicht für die Wartung eingeplant. Es könnte sein, dass die Wartungsaktion beendet, abgebrochen oder so geändert wurde, dass Ihre virtuellen Computer nicht mehr davon betroffen sind.
3.  Sie haben Ihrer VM-Listenansicht nicht die Spalte „Wartung“ hinzugefügt. Obwohl wir diese Spalte der Standardansicht hinzugefügt haben, müssen Kunden, die die Anzeige nicht standardmäßiger Spalten konfiguriert haben, die Spalte **Wartung** ihrer VM-Listenansicht manuell hinzufügen.

**F: Meine VM ist zum zweiten Mal für die Wartung eingeplant. Warum?**

**A:** In mehreren Anwendungsfällen wird Ihr virtueller Computer zur Wartung eingeplant, nachdem Sie die erneute Bereitstellung nach Wartung bereits abgeschlossen haben:
1.  Wir haben die Wartungsaktion abgebrochen und einen Neustart mit anderer Nutzlast vorgenommen. Es könnte sein, dass wir fehlerhafte Nutzlast erkannt haben und einfach eine zusätzliche Nutzlast bereitstellen müssen.
2.  Die *Dienstreparatur* Ihres virtuellen Computers wurde aufgrund eines Hardwarefehlers auf einem anderen Knoten durchgeführt.
3.  Sie haben ausgewählt, den virtuellen Computer zu beenden (freizugeben) und neu zu starten.
4.  Sie haben **automatisches Herunterfahren** für den virtuellen Computer aktiviert.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie sich über einen virtuellen Computer mithilfe von [geplanten Ereignissen](scheduled-events.md) für Wartungsereignisse registrieren.