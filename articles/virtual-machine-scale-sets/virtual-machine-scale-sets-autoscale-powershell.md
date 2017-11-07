---
title: Automatisches Skalieren von VM-Skalierungsgruppen mit Azure PowerShell | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Regeln für die automatische Skalierung für VM-Skalierungsgruppen mit Azure PowerShell erstellen."
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.openlocfilehash: 1fbfbbc79a415af5e874c304412854849e134eb7
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="automatically-scale-a-virtual-machine-scale-set-with-azure-powershell"></a>Automatisches Skalieren einer VM-Skalierungsgruppe mit Azure PowerShell
Beim Erstellen einer Skalierungsgruppe definieren Sie die Anzahl von VM-Instanzen, die Sie ausführen möchten. Wenn sich die Nachfrage nach Ihrer Anwendung ändert, können Sie die Anzahl von VM-Instanzen automatisch erhöhen oder verringern lassen. Dank der Möglichkeit zum automatischen Skalieren können Sie über den gesamten Lebenszyklus Ihrer App die Kundennachfrage decken oder auf Änderungen der Anwendungsleistung reagieren.

In diesem Artikel wird veranschaulicht, wie Sie mit Azure PowerShell Regeln für die automatische Skalierung erstellen, mit denen die Leistung der VM-Instanzen in Ihrer Skalierungsgruppe überwacht wird. Mit diesen Regeln für die automatische Skalierung wird die Anzahl von VM-Instanzen erhöht oder verringert, um auf die Werte dieser Leitungsmetriken zu reagieren. Sie können diese Schritte auch mit [Azure CLI 2.0](virtual-machine-scale-sets-autoscale-cli.md) oder im [Azure-Portal](virtual-machine-scale-sets-autoscale-portal.md) ausführen.


## <a name="prerequisites"></a>Voraussetzungen
Zum Erstellen von Regeln für die automatische Skalierung benötigen Sie eine vorhandene VM-Skalierungsgruppe. Sie können eine Skalierungsgruppe im [Azure-Portal](virtual-machine-scale-sets-portal-create.md), mit [Azure PowerShell](virtual-machine-scale-sets-create.md#create-from-azure-cli) oder der [Azure CLI 2.0](virtual-machine-scale-sets-create.md#create-from-powershell) erstellen.

Definieren Sie einige Variablen für Ihre Skalierungsgruppe, um die Erstellung der Regeln für die automatische Skalierung zu vereinfachen. Im folgenden Beispiel werden Variablen für die Skalierungsgruppe *myScaleSet* in der Ressourcengruppe *myResourceGroup* in der Region *USA, Osten* definiert. Sie können Ihre Abonnement-ID mit [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription) abrufen. Wenn Ihrem Konto mehrere Abonnements zugeordnet sind, wird nur das erste Abonnement zurückgegeben. Passen Sie die Namen und die Abonnement-ID wie folgt an:

```powershell
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"
```


## <a name="create-a-rule-to-automatically-scale-out"></a>Erstellen einer Regel für das automatische horizontale Hochskalieren
Wenn sich die Nachfrage für Ihre Anwendung erhöht, erhöht sich auch die Last für die VM-Instanzen in Ihrer Skalierungsgruppe. Falls es sich um eine dauerhafte Last und nicht nur um eine kurzzeitige höhere Nachfrage handelt, können Sie die Regeln für die automatische Skalierung konfigurieren, um die Anzahl von VM-Instanzen in der Skalierungsgruppe zu erhöhen. Nachdem diese VM-Instanzen erstellt und Ihre Anwendungen bereitgestellt wurden, beginnt die Skalierungsgruppe damit, über das Lastenausgleichsmodul Datenverkehr darauf zu verteilen. Sie steuern, welche Metriken überwacht werden, z.B. CPU oder Datenträger, wie lange die Anwendungslast einen bestimmten Schwellenwert einhalten muss und wie viele VM-Instanzen der Skalierungsgruppe hinzugefügt werden sollen.

Wir erstellen mit [New-AzureRmAutoscaleRule](/powershell/module/AzureRM.Insights/New-AzureRmAutoscaleRule) eine Regel, mit der die Anzahl von VM-Instanzen in einer Skalierungsgruppe erhöht wird, wenn die durchschnittliche CPU-Last für einen Zeitraum von 5 Minuten über 70 % liegt. Wenn die Regel ausgelöst wird, wird die Anzahl von VM-Instanzen um 20% erhöht. In Skalierungsgruppen mit einer kleinen Anzahl von VM-Instanzen können Sie `-ScaleActionScaleType` auslassen und nur `-ScaleActionValue` festlegen, um die Anzahl um *1* oder *2* Instanzen zu erhöhen. In Skalierungsgruppen mit einer großen Zahl von VM-Instanzen ist eine Erhöhung um 10 oder 20 Prozent für die VM-Instanzen unter Umständen angemessener.

Für diese Regel werden die folgenden Parameter verwendet:

| Parameter               | Erklärung                                                                                                         | Wert          |
|-------------------------|---------------------------------------------------------------------------------------------------------------------|----------------|
| *-MetricName*           | Die Leistungsmetrik zum Überwachen und Anwenden von Skalierungsgruppenaktionen.                                                   | CPU in Prozent |
| *-TimeGrain*            | Gibt an, wie häufig Metriken zu Analysezwecken gesammelt werden.                                                                   | 1 Minute       |
| *-MetricStatistic*      | Definiert, wie die gesammelten Metriken zu Analysezwecken aggregiert werden sollen.                                                | Durchschnitt        |
| *-TimeWindow*           | Der überwachte Zeitraum, bevor die Metrik und die Schwellenwerte verglichen werden.                                   | 10 Minuten      |
| *-Operator*             | Operator zum Vergleichen der Metrikdaten mit dem Schwellenwert.                                                     | Größer als   |
| *-Threshold*            | Der Wert, der für die Regel für die automatische Skalierung das Auslösen einer Aktion bewirkt.                                                      | 70 %            |
| *-ScaleActionDirection* | Definiert, ob die Skalierungsgruppe zentral hoch- oder herunterskaliert werden soll, wenn die Regel zutrifft.                                             | Erhöhung       |
| *-ScaleActionScaleType* | Gibt an, dass die Anzahl von VM-Instanzen um einen Prozentbetrag geändert werden soll.                                 | Prozentuale Änderung |
| *-ScaleActionValue*     | Der Prozentsatz der VM-Instanzen sollte geändert werden, wenn diese Regel ausgelöst wird.                                            | 20             |
| *-ScaleActionCooldown*  | Gibt an, wie lange gewartet wird, bevor die Regel erneut angewendet wird, damit die Aktionen für die automatische Skalierung wirksam werden können. | 5 Minuten      |

Das folgende Beispiel erstellt das Objekt *myRuleScaleOut*, das diese Regel für das zentrale Hochskalieren enthält. Die *-MetricResourceId* verwendet die Variablen, die zuvor für die Abonnement-ID, den Namen der Ressourcengruppe und den Namen der Skalierungsgruppe definiert wurden:

```powershell
$myRuleScaleOut = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -TimeGrain 00:01:00 `
  -MetricStatistic Average `
  -TimeWindow 00:10:00 `
  -Operator GreaterThan `
  -Threshold 70 `
  -ScaleActionDirection Increase `
  –ScaleActionScaleType PercentChangeCount `
  -ScaleActionValue 20 `
  -ScaleActionCooldown 00:05:00
```


## <a name="create-a-rule-to-automatically-scale-in"></a>Erstellen einer Regel für das automatische horizontale Herunterskalieren
Es kann sein, dass die Nachfrage nach Ihrer Anwendung abends oder am Wochenende abnimmt. Wenn diese Verringerung der Last für einen bestimmten Zeitraum anhält, können Sie die Regeln der automatischen Skalierung konfigurieren, um die Anzahl von VM-Instanzen in der Skalierungsgruppe zu reduzieren. Mit dieser Aktion zum horizontalen Herunterskalieren werden die Kosten für die Ausführung Ihrer Skalierungsgruppe gesenkt, da Sie nur so viele Instanzen ausführen, wie für die Erfüllung der derzeitigen Nachfrage erforderlich sind.

Erstellen Sie mit [New-AzureRmAutoscaleRule](/powershell/module/AzureRM.Insights/New-AzureRmAutoscaleRule) eine Regel, mit der die Anzahl von VM-Instanzen in einer Skalierungsgruppe verringert wird, wenn die durchschnittliche CPU-Last über einen Zeitraum von 10 Minuten unter 30 % fällt. Bei Auslösung dieser Regel wird die Anzahl der VM-Instanzen um 20 % verringert. Das folgende Beispiel erstellt das Objekt *myRuleScaleDown*, das diese Regel für das zentrale Hochskalieren enthält. Die *-MetricResourceId* verwendet die Variablen, die zuvor für die Abonnement-ID, den Namen der Ressourcengruppe und den Namen der Skalierungsgruppe definiert wurden:

```powershell
$myRuleScaleIn = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:10:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  –ScaleActionScaleType PercentChangeCount `
  -ScaleActionValue 20
```


## <a name="define-an-autoscale-profile"></a>Definieren eines Profils für die automatische Skalierung
Um Ihre Regeln zur automatischen Skalierung einer Skalierungsgruppe zuzuordnen, erstellen Sie ein Profil. Das Profil für die automatische Skalierung definiert die Standard-, Mindest- und Höchstkapazität der Skalierungsgruppe und ordnet Regeln zur automatischen Skalierung zu. Erstellen Sie mit [New-AzureRmAutoscaleProfile](/powershell/module/AzureRM.Insights/New-AzureRmAutoscaleProfile) ein Profil für die automatische Skalierung. Im folgenden Beispiel werden als Standard- und Mindestwert *2* und als Maximalwert *10* VM-Instanzen festgelegt. Anschließend werden die Regeln für das horizontale Hoch- und Herunterskalieren aus den vorherigen Schritten zugeordnet:

```powershell
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleOut,$myRuleScaleIn `
  -Name "autoprofile"
```


## <a name="apply-autoscale-rules-to-a-scale-set"></a>Anwenden von Regeln für die automatische Skalierung auf eine Skalierungsgruppe
Der letzte Schritt ist das Anwenden des Profils für die automatische Skalierung auf Ihre Skalierungsgruppe. Ihre Skalierung kann dann automatisch je nach Anwendungsnachfrage das horizontale Herunter- oder Hochskalieren durchführen. Sie wenden das Profil für die automatische Skalierung wie folgt mit [Add-AzureRmAutoscaleSetting](/powershell/module/AzureRM.Insights/Add-AzureRmAutoscaleSetting) an:

```powershell
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```


## <a name="monitor-number-of-instances-in-a-scale-set"></a>Überwachen der Anzahl von Instanzen in einer Skalierungsgruppe
Zum Prüfen der Anzahl und des Status von VM-Instanzen können Sie mit [Get-AzureRmVmssVM](/powershell/module/AzureRM.Compute/Get-AzureRmVmssVM) eine Liste der Instanzen in Ihrer Skalierungsgruppe anzeigen. Der Status zeigt an, ob die VM-Instanz beim automatischen horizontalen Hochskalieren bereitgestellt wird oder ob die Bereitstellung beim automatischen horizontalen Herunterskalieren aufgehoben wird. Im folgenden Beispiel wird der VM-Instanzstatus für die Skalierungsgruppe *myScaleSet* in der Ressourcengruppe *myResourceGroup* angezeigt:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="autoscale-based-on-a-schedule"></a>Automatisches Skalieren nach einem Zeitplan
In den vorherigen Beispielen wurde eine Skalierungsgruppe anhand von einfachen Hostmetriken, z.B. der CPU-Auslastung, automatisch herunter- oder hochskaliert. Sie können Regeln für die automatische Skalierung auch anhand von Zeitplänen erstellen. Mit diesen zeitplanbasierten Regeln können Sie die Anzahl von VM-Instanzen vor einem erwarteten Anstieg der Anwendungsnachfrage, z.B. der Kernarbeitszeit, automatisch horizontal hochskalieren und dann für Zeiträume mit geringerer Nachfrage, z.B. am Wochenende, wieder automatisch zentral herunterskalieren.

Verwenden Sie zum Erstellen von Regeln zur automatischen Skalierung basierend auf einem Zeitplan anstelle von Hostmetriken das Azure-Portal. Regeln auf Grundlage eines Zeitplans können derzeit nicht mit Azure PowerShell erstellt werden.


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde beschrieben, wie Sie mit Regeln für die automatische Skalierung horizontal skalieren und die *Anzahl* von VM-Instanzen in Ihrer Skalierungsgruppe erhöhen oder verringern. Sie können auch vertikal skalieren, um die *Größe* der VM-Instanz zu erhöhen oder zu verringern. Weitere Informationen finden Sie unter [Vertikale automatische Skalierung mit VM-Skalierungsgruppen](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Informationen zur Verwaltung Ihrer VM-Instanzen finden Sie unter [Manage a virtual machine scale set with Azure PowerShell](virtual-machine-scale-sets-windows-manage.md) (Verwalten einer VM-Skalierungsgruppe mit Azure PowerShell).

Informationen zum Generieren von Warnungen bei der Auslösung von Regeln für die automatische Skalierung finden Sie unter [Verwenden von automatischen Skalierungsvorgängen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). Informieren Sie sich auch über das [Aufrufen eines Webhooks für Azure-Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
