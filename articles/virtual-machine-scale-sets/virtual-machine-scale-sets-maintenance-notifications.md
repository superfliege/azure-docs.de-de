---
title: Behandeln von Wartungsbenachrichtigungen für Virtual Machine Scale Sets in Azure | Microsoft-Dokumentation
description: Zeigen Sie Wartungsbenachrichtigungen für Virtual Machine Scale Sets in Azure an, und starten Sie eine Self-Service-Wartung.
services: virtual-machine-scale-sets
documentationcenter: ''
author: shants123
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2018
ms.author: shants
ms.openlocfilehash: 4ce984686c2bb320d5d32771d31b81cdec1153af
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38506257"
---
# <a name="handling-planned-maintenance-notifications-for-virtual-machine-scale-sets"></a>Behandeln von Benachrichtigungen zu geplanten Wartungen für Virtual Machine Scale Sets

Azure führt regelmäßig Updates durch, um die Zuverlässigkeit, Leistung und Sicherheit der Hostinfrastruktur für virtuelle Computer zu verbessern. Zu Updates zählen Änderungen wie das Patchen der Hostumgebung oder das Upgraden und die Außerbetriebnahme von Hardware. Die meisten dieser Updates werden ohne Auswirkungen auf die gehosteten virtuellen Computer durchgeführt. Es gibt jedoch Ausnahmen:

- Wenn die Wartung keinen Neustart erfordert, verwendet Azure eine direkte Migration, um den virtuellen Computer anzuhalten, während der Host aktualisiert wird. Diese Wartungsvorgänge ohne Neustart werden einzeln für jede Fehlerdomäne angewendet, und das Fortschreiten wird beendet, wenn Warnsignale zur Integrität empfangen werden.

- Wenn die Wartung einen Neustart erfordert, werden Sie in einer Benachrichtigung über den geplanten Wartungstermin informiert. In diesen Fällen steht Ihnen ein Zeitfenster zur Verfügung, in dem Sie die Wartung zu einem für Sie günstigen Zeitpunkt selbst starten können.


Geplante Wartungen, die einen Neustart erfordern, werden in Wellen geplant. Jede Welle hat einen anderen Umfang (Regionen).

- Eine Welle beginnt mit einer Kundenbenachrichtigung. Die Benachrichtigung wird standardmäßig an Abonnementbesitzer und -mitbesitzer gesendet. Für die Benachrichtigungen können mit Azure-[Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) weitere Empfänger und Nachrichtenoptionen wie E-Mail, SMS und Webhooks hinzugefügt werden.  
- Zum Zeitpunkt der Benachrichtigung wird ein *Self-Service-Zeitfenster* verfügbar gemacht. Während dieses Zeitfensters können Sie ermitteln, welche Ihrer virtuellen Computer von der Welle betroffen sind, und die Wartung gemäß eigener Planungsanforderungen starten.
- Im Anschluss an das Self-Service-Zeitfenster beginnt das *Zeitfenster für die geplante Wartung*. Irgendwann in diesem Zeitfenster plant Azure die erforderliche Wartung und wendet sie auf Ihren virtuellen Computer an. 

Mit den beiden Zeitfenstern möchten wir Ihnen einerseits genügend Zeit geben, um die Wartung zu initiieren und Ihren virtuellen Computer neu zu starten, und Sie andererseits darüber informieren, wann Azure die Wartung automatisch startet.


Sie können das Azure-Portal, PowerShell, die REST-API und die Befehlszeilenschnittstelle verwenden, um die Wartungszeitfenster für Ihre Skalierungsgruppen-VMs abzufragen und eine Self-Service-Wartung zu starten.

  
## <a name="should-you-start-maintenance-during-the-self-service-window"></a>Sollten Sie die Wartung während des Self-Service-Fensters starten?  

Die folgenden Hinweise sollen Sie bei der Entscheidung unterstützen, ob Sie diese Funktion nutzen und die Wartung zu einem selbst gewählten Zeitpunkt starten sollten.

> [!NOTE] 
> Die Self-Service-Wartung ist unter Umständen nicht für alle virtuellen Computer verfügbar. Um zu ermitteln, ob die proaktive erneute Bereitstellung für Ihren virtuellen Computer verfügbar ist, überprüfen Sie, ob als Wartungsstatus **Jetzt starten** angezeigt wird. Die Self-Service-Wartung ist derzeit nicht für Cloud Services (Web-/Workerrolle) und Service Fabric verfügbar.


Self-Service-Wartung wird für Bereitstellungen mit **Verfügbarkeitsgruppen** nicht empfohlen, da es sich dabei um hoch verfügbare Setups handelt, bei denen jeweils nur immer eine Updatedomäne betroffen ist. 

- Überlassen Sie Azure das Auslösen der Wartung. Beachten Sie bei einer Wartung, die einen Neustart erfordert, dass die Wartung für jede Updatedomäne einzeln erfolgt, die Updatedomänen die Wartung nicht unbedingt in sequenzieller Reihenfolge erhalten und zwischen den Updatedomänen eine Pause von 30 Minuten liegt.
- Wenn ein vorübergehender Kapazitätsverlust (1/Updatedomänenanzahl) ein Problem darstellt, können Sie zum Ausgleich während der Wartung einfach zusätzliche Instanzen zuweisen.
- Bei einer Wartung, die keinen Neustart erfordert, werden Updates auf der Fehlerdomänenebene angewendet. 
    
Verwenden Sie Self-Service-Wartung **nicht** in den folgenden Szenarien: 

- Wenn Sie Ihre virtuellen Computer häufig herunterfahren (entweder manuell, mithilfe von DevTest Labs, mit der Funktion zum automatischen Herunterfahren oder gemäß eines Zeitplans), wird dadurch unter Umständen der Wartungsstatus zurückgesetzt, was zusätzliche Ausfallzeiten zur Folge haben kann.
- Bei virtuellen Computern mit kurzer Lebensdauer, die vor Ende der Wartungsaktion gelöscht werden 
- Bei Workloads, für die umfangreiche Zustandsinformationen auf dem lokalen (flüchtigen) Datenträger gespeichert sind, der bei einem Update erhalten bleiben soll 
- Bei häufiger Änderung der Größe Ihrer virtuellen Computer, da dadurch der Wartungsstatus wiederhergestellt werden kann 
- Wenn Sie 15 Minuten vor dem Herunterfahren für die Wartung geplante Ereignisse übernommen haben, die ein proaktives Failover oder das ordnungsgemäße Herunterfahren Ihrer Workload ermöglichen

**Verwenden** Sie die Self-Service-Wartung, wenn Sie den virtuellen Computer während der geplanten Wartungsphase unterbrechungsfrei ausführen möchten und keiner der oben aufgeführten Punkte zutrifft. 

Verwenden Sie Self-Service-Wartung am besten in den folgenden Fällen:

- Sie müssen den Vorgesetzten oder Endkunden ein exaktes Wartungsfenster mitteilen. 
- Sie müssen die Wartung bis zu einem bestimmten Termin abgeschlossen haben. 
- Sie müssen die Reihenfolge der Wartung steuern (etwa bei einer Anwendung mit mehreren Ebenen), um die Wiederherstellung gewährleisten zu können.
- Sie benötigen für den virtuellen Computer mehr als 30 Minuten Wiederherstellungszeit zwischen zwei Updatedomänen (UDs). Um die Zeit zwischen Updatedomänen zu steuern, müssen Sie auf Ihren virtuellen Computern die Wartung der Updatedomänen jeweils nacheinander auslösen.

 
## <a name="view-virtual-machine-scale-sets-impacted-by-maintenance-in-the-portal"></a>Anzeigen von VM-Skalierungsgruppe, die von der Wartung im Portal betroffen sind

Nachdem eine geplante Wartungsaktion geplant wurde, können Sie über das Azure-Portal die Liste mit den VM-Skalierungsgruppen anzeigen, die von der anstehenden Wartungsaktion betroffen sind. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im linken Navigationsbereich **Alle Dienste** aus, und wählen Sie dann **Virtual Machine Scale Sets** aus.
3. Wählen Sie auf der Seite **Virtual Machine Scale Sets** ganz oben die Option **Spalten bearbeiten** aus, um die Liste der verfügbaren Spalten zu öffnen.
4. Wählen Sie im Abschnitt **Verfügbare Spalten** das Element **Self-Service-Wartung** aus, und verschieben Sie es mit den Pfeilschaltflächen in die Liste **Ausgewählte Spalten**. Sie können die Dropdownliste im Abschnitt **Verfügbare Spalten** von **Alle** zu **Eigenschaften** umschalten, um das Element **Self-Service-Wartung** einfacher finden zu können. Nachdem Sie das Element **Self-Service-Wartung** im Abschnitt **Ausgewählte Spalten** markiert haben, wählen Sie unten auf der Seite **Übernehmen** aus. 

Nach dem Befolgen der oben angegebenen Schritte wird die Spalte **Self-Service-Wartung** in der Liste der VM-Skalierungsgruppen angezeigt. Jede VM-Skalierungsgruppe kann einen der folgenden Werte für die Spalte „Self-Service-Wartung“ aufweisen:

| Wert | BESCHREIBUNG |
|-------|-------------|
| Ja | Mindestens ein virtueller Computer in Ihrer VM-Skalierungsgruppe befindet sich in einem Self-Service-Fenster. Sie können die Wartung jederzeit in diesem Self-Service-Zeitfenster starten. | 
| Nein  | Es gibt keine virtuellen Computer in einem Self-Service-Fenster in der betroffenen VM-Skalierungsgruppe. | 
| - | Ihre VM-Skalierungsgruppen sind nicht Teil einer geplanten Wartungsaktion.| 

## <a name="notification-and-alerts-in-the-portal"></a>Benachrichtigungen und Warnungen im Portal

Azure sendet eine E-Mail an die Gruppe der Abonnementbesitzer und -mitbesitzer, um sie über den Zeitplan für eine geplante Wartung zu informieren. Sie können Azure-Aktivitätsprotokollwarnungen erstellen, um weitere Empfänger und Kanäle hinzuzufügen. Weitere Informationen finden Sie unter [Überwachen der Abonnementaktivität per Azure-Aktivitätsprotokoll](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im Menü auf der linken Seite auf **Überwachen**. 
3. Klicken Sie im Bereich **Überwachen – Warnungen (klassisch)** auf **+ Aktivitätsprotokollwarnung hinzufügen**.
4. Füllen Sie die Informationen auf der Seite **Aktivitätsprotokollwarnung hinzufügen** aus, und legen Sie die folgenden **Kriterien** fest:
   - **Ereigniskategorie:** Dienstintegrität
   - **Dienste:** Virtual Machine Scale Sets und Virtual Machines
   - **Typ:** geplante Wartung 
    
Weitere Informationen zum Konfigurieren von Aktivitätsprotokollwarnungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md).
    
    
## <a name="start-maintenance-on-your-virtual-machine-scale-set-from-the-portal"></a>Starten der Wartung für Ihre VM-Skalierungsgruppe über das Portal

In der Übersicht über VM-Skalierungsgruppen können Sie auch weitere wartungsbezogene Details anzeigen. Wenn mindestens ein virtueller Computer in der VM-Skalierungsgruppe in der geplanten Wartungsaktion enthalten ist, wird im oberen Bereich der Seite ein neues Benachrichtigungsmenüband hinzugefügt. Sie können auf das Benachrichtigungsmenüband klicken, um die Seite **Wartung** zu erreichen, auf der angezeigt wird, welche Instanz eines virtuellen Computers von der geplanten Wartung betroffen ist. 

Von dort können Sie die Wartung starten, indem Sie das Kontrollkästchen neben dem betroffenen virtuellen Computer aktivieren und dann auf die Option **Wartung starten** klicken.

Nach dem Starten werden die betroffenen virtuellen Computer in Ihrer VM-Skalierungsgruppe gewartet und stehen vorübergehend nicht zur Verfügung. Wenn Sie das Self-Service-Zeitfenster verpasst haben, wird das Zeitfenster, in dem Ihre VM-Skalierungsgruppe von Azure gewartet wird, weiterhin angezeigt.
 
## <a name="check-maintenance-status-using-powershell"></a>Überprüfen des Wartungsstatus mithilfe von PowerShell

Sie können mithilfe von Azure PowerShell einsehen, wann die Wartung von VMs in Ihrer VM-Skalierungsgruppe geplant ist. Informationen zur geplanten Wartung können mit dem Cmdlet [Get-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss) unter Verwendung des Parameters `-InstanceView` ermittelt werden.
 
Wartungsinformationen werden nur zurückgegeben, wenn eine Wartung geplant ist. Ist keine Wartung geplant, die Auswirkungen auf die VM-Instanz hat, gibt das Cmdlet keine Wartungsinformationen zurück. 

```powershell
Get-AzureRmVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -InstanceView
```

Unter „MaintenanceRedeployStatus“ werden folgende Eigenschaften zurückgegeben: 
| Wert | BESCHREIBUNG   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Gibt an, ob Sie zum aktuellen Zeitpunkt die Wartung für den virtuellen Computer starten können. ||
| PreMaintenanceWindowStartTime         | Der Anfang des Self-Service-Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| PreMaintenanceWindowEndTime           | Das Ende des Self-Service-Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| MaintenanceWindowStartTime            | Der Anfang der geplanten Wartung, zu dem Azure die Wartung für Ihren virtuellen Computer initiiert ||
| MaintenanceWindowEndTime              | Das Ende des geplanten Wartungszeitfensters, in dem Azure die Wartung für Ihren virtuellen Computer initiiert ||
| LastOperationResultCode               | Das Ergebnis des letzten Wartungsinitiierungsversuchs für den virtuellen Computer. ||



### <a name="start-maintenance-on-your-vm-instance-using-powershell"></a>Starten der Wartung für Ihre VM-Instanz mithilfe von PowerShell

Sie können die Wartung eines virtuellen Computers starten, wenn **IsCustomerInitiatedMaintenanceAllowed** mithilfe des Cmdlets [Set-AzureRmVmss](/powershell/module/azurerm.compute/set-azurermvmss) mit dem Parameter `-PerformMaintenance` auf TRUE festgelegt wurde.

```powershell
Set-AzureRmVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -PerformMaintenance 
```

## <a name="check-maintenance-status-using-cli"></a>Überprüfen des Wartungsstatus mithilfe der Befehlszeilenschnittstelle

Informationen zu geplanten Wartungen können mithilfe von [az vmss list-instances](/cli/azure/vmss?view=azure-cli-latest#az-vmss-list-instances) angezeigt werden.
 
Wartungsinformationen werden nur zurückgegeben, wenn eine Wartung geplant ist. Ist keine Wartung geplant, die Auswirkungen auf die VM-Instanz hat, gibt der Befehl keine Wartungsinformationen zurück. 

```azure-cli
az vmss list-instances -g rgName -n vmssName --expand instanceView
```

Unter MaintenanceRedeployStatus für die einzelnen VM-Instanzen werden folgende Eigenschaften zurückgegeben: 
| Wert | BESCHREIBUNG   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Gibt an, ob Sie zum aktuellen Zeitpunkt die Wartung für den virtuellen Computer starten können. ||
| PreMaintenanceWindowStartTime         | Der Anfang des Self-Service-Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| PreMaintenanceWindowEndTime           | Das Ende des Self-Service-Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. ||
| MaintenanceWindowStartTime            | Der Anfang der geplanten Wartung, zu dem Azure die Wartung für Ihren virtuellen Computer initiiert ||
| MaintenanceWindowEndTime              | Das Ende des geplanten Wartungszeitfensters, in dem Azure die Wartung für Ihren virtuellen Computer initiiert ||
| LastOperationResultCode               | Das Ergebnis des letzten Wartungsinitiierungsversuchs für den virtuellen Computer. ||


### <a name="start-maintenance-on-your-vm-instance-using-cli"></a>Starten der Wartung für Ihre VM-Instanz mithilfe der Befehlszeilenschnittstelle

Der folgende Aufruf initiiert die Wartung für eine VM-Instanz, wenn `IsCustomerInitiatedMaintenanceAllowed` auf TRUE festgelegt ist:

```azure-cli
az vmss perform-maintenance -g rgName -n vmssName --instance-ids id
```

## <a name="faq"></a>Häufig gestellte Fragen


**F: Wozu müssen Sie meine virtuellen Computer jetzt neu starten?**

**A:** Während die Mehrzahl der Updates und Upgrades für die Azure-Plattform die Verfügbarkeit des virtuellen Computers nicht beeinflusst, gibt es Fälle, in denen wir den Neustart in Azure gehosteter virtueller Computer nicht vermeiden können. Bei uns sind mehrere Änderungen zusammengekommen, die den Neustart unserer Server erfordern und damit auch zum Neustart virtueller Computer führen.

**F: Bin ich sicher, wenn ich ihre Empfehlungen für Hochverfügbarkeit mit Verwendung einer Verfügbarkeitsgruppe befolge?**

**A:** In einer Verfügbarkeitsgruppe oder VM-Skalierungsgruppe bereitgestellte virtuelle Computer werden wie Updatedomänen (UD) behandelt. Bei der Wartung berücksichtigt Azure die UD-Einschränkung und startet virtuelle Computer nicht von einer anderen UD (innerhalb derselben Verfügbarkeitsgruppe) aus neu.  Azure wartet auch mindestens 30 Minuten vor dem Wechsel zur nächsten Gruppe von virtuellen Computern. 

Weitere Informationen zu Hochverfügbarkeit finden Sie unter [Regionen und Verfügbarkeit für virtuelle Computer in Azure](../virtual-machines/windows/regions-and-availability.md).

**F: Wie werde ich über eine geplante Wartung benachrichtigt?**

**A:** Eine geplante Wartungsaktion beginnt mit dem Festlegen eines Zeitplans für eine oder mehrere Azure-Regionen. Bald danach wird eine E-Mail-Benachrichtigung an die Abonnementbesitzer gesendet (pro Abonnement eine E-Mail). Zusätzliche Kanäle und Empfänger für diese Benachrichtigung können mit Aktivitätsprotokollwarnungen konfiguriert werden. Falls Sie einen virtuellen Computer in einer Region bereitstellen, wo bereits Wartung geplant ist, erhalten Sie die Benachrichtigung nicht, sondern müssen vielmehr den Wartungsstatus des virtuellen Computers überprüfen.

**F: Mir wird weder im Portal noch über PowerShell oder über die Befehlszeilenschnittstelle ein Hinweis auf eine geplante Wartung angezeigt. Was ist schiefgelaufen?**

**A:** Informationen im Zusammenhang mit geplanter Wartung stehen während einer geplanten Wartungsaktion nur für die virtuellen Computer zur Verfügung, die davon betroffen sind. D.h., wenn Ihnen keine Daten angezeigt werden, ist es möglich, dass die Wartungsaktion bereits abgeschlossen (oder nicht gestartet) wurde, bzw. Ihr virtueller Computer bereits auf einem aktualisierten Server gehostet wird.

**F: Gibt es eine Möglichkeit, dass ich genau feststellen kann, wann mein virtueller Computer betroffen ist?**

**A:** Bei Festlegung des Zeitplans definieren wir ein Zeitfenster von einigen Tagen. Allerdings ist die genaue Reihenfolge von Servern (und VMs) innerhalb dieses Zeitfensters unbekannt. Kunden, die die genaue Zeit für ihre virtuellen Computer wissen möchten, können [geplante Ereignisse](../virtual-machines/windows/scheduled-events.md) verwenden, aus dem virtuellen Computer heraus abfragen und erhalten 15 Minuten vor dem VM-Neustart eine Benachrichtigung.

**F: Wie lange dauert es, meinen virtuellen Computer neu zu starten?**

**A:** Je nach Größe Ihres virtuellen Computers kann der Neustart während des Self-Service-Wartungsfensters mehrere Minuten dauern. Die von Azure im geplanten Wartungsfenster initiierten Neustarts dauern in der Regel etwa 25 Minuten. Beachten Sie, dass Ihnen bei Verwendung von Cloud Services (Web-/Workerrolle), VM-Skalierungsgruppen oder Verfügbarkeitsgruppen während des geplanten Wartungsfensters zwischen den einzelnen Gruppen von virtuellen Computern (UD) 30 Minuten zur Verfügung stehen. 

**F: Auf meinen virtuellen Computern werden keine Wartungsinformationen angezeigt. Welche Probleme sind aufgetreten?**

**A:** Es gibt verschiedene Gründe, warum Sie keine Wartungsinformationen auf Ihren virtuellen Computern sehen:
   - Sie verwenden ein Abonnement, das als Microsoft-intern markiert ist.
   - Ihre virtuellen Computer sind nicht für die Wartung eingeplant. Es könnte sein, dass die Wartungsaktion beendet, abgebrochen oder so geändert wurde, dass Ihre virtuellen Computer nicht mehr davon betroffen sind.
   - Sie haben Ihrer VM-Listenansicht nicht die Spalte **Wartung** hinzugefügt. Obwohl wir diese Spalte der Standardansicht hinzugefügt haben, müssen Kunden, die die Anzeige nicht standardmäßiger Spalten konfiguriert haben, die Spalte **Wartung** ihrer VM-Listenansicht manuell hinzufügen.

**F: Meine VM ist zum zweiten Mal für die Wartung eingeplant. Warum?**

**A:** In mehreren Anwendungsfällen wird Ihr virtueller Computer zur Wartung eingeplant, nachdem Sie die erneute Bereitstellung nach Wartung bereits abgeschlossen haben:
   - Wir haben die Wartungsaktion abgebrochen und einen Neustart mit anderer Nutzlast vorgenommen. Es könnte sein, dass wir fehlerhafte Nutzlast erkannt haben und einfach eine zusätzliche Nutzlast bereitstellen müssen.
   - Die *Dienstreparatur* Ihres virtuellen Computers wurde aufgrund eines Hardwarefehlers auf einem anderen Knoten durchgeführt.
   - Sie haben ausgewählt, den virtuellen Computer zu beenden (freizugeben) und neu zu starten.
   - Sie haben **automatisches Herunterfahren** für den virtuellen Computer aktiviert.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie sich über einen virtuellen Computer mithilfe von [geplanten Ereignissen](../virtual-machines/windows/scheduled-events.md) für Wartungsereignisse registrieren.
