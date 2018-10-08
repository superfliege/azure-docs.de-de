---
title: Aktualisieren von Azure-Modulen in Azure Automation
description: In diesem Artikel wird beschrieben, wie Sie jetzt häufig verwendete Azure PowerShell-Module, die standardmäßig in Azure Automation bereitgestellt werden, aktualisieren können.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fbb57753117f3c60010fe910616b8d0af5178360
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434822"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Aktualisieren von Azure PowerShell-Modulen in Azure Automation

Die am häufigsten verwendeten Azure PowerShell-Module werden standardmäßig in jedem Automation-Konto bereitgestellt. Das Azure-Team aktualisiert die Azure-Module regelmäßig. Über das Automation-Konto erhalten Sie die Möglichkeit, die Module im Konto zu aktualisieren, sobald neue Versionen im Portal verfügbar sind.

Da Module von der Produktgruppe regelmäßig aktualisiert werden, können Änderungen bei den enthaltenen Cmdlets auftreten. Je nachdem, welche Änderung ausgeführt wurde, z.B. ob ein Parameter umbenannt oder ein Cmdlet vollständig eingestellt wurde, können negative Auswirkungen auf Ihre Runbooks auftreten. Um Auswirkungen auf Ihre Runbooks und die Prozesse zu vermeiden, die sie automatisieren, wird empfohlen, vor dem Fortsetzen des Vorgangs Tests und Überprüfungen auszuführen. Wenn Sie für diesen Zweck kein dediziertes Automation-Konto haben, sollten Sie eines erstellen. So können Sie während der Entwicklung Ihrer Runbooks viele verschiedene Szenarios und Permutationen testen und iterative Änderungen wie das Aktualisieren der PowerShell-Module ausführen. Nachdem die Ergebnisse überprüft werden und Sie alle erforderlichen Änderungen angewendet haben, koordinieren Sie die Migration aller Runbooks, für die Änderungen erforderlich waren, und führen Sie das folgende Update wie beschrieben in der Produktion aus.

> [!NOTE]
> Ein neues Automation-Konto enthält ggf. nicht die neuesten Module.

## <a name="updating-azure-modules"></a>Aktualisieren von Azure-Modulen

1. Auf der Seite „Module“ Ihres Automation-Kontos gibt es eine Option namens **Azure-Module aktualisieren**. Sie ist immer aktiviert.<br><br> ![Option zum Aktualisieren von Azure-Modulen auf der Seite „Module“](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

  > [!NOTE]
  > Es wird empfohlen, Ihre Azure-Module vor dem Aktualisieren in einem Automation-Testkonto zu aktualisieren. So wird sichergestellt, dass Ihre vorhandenen Skripts wie erwartet funktionieren, bevor die Aktualisierung für Ihre Azure-Module durchgeführt wird.
  >
  > Die Schaltfläche **Azure-Module aktualisieren** ist nur in der öffentlichen Cloud verfügbar. In [Regionen mit Datenhoheit](https://azure.microsoft.com/global-infrastructure/) ist sie nicht verfügbar. Weitere Informationen finden Sie im Abschnitt [Andere Möglichkeiten zum Aktualisieren Ihrer Module](#alternative-ways-to-update-your-modules).


2. Klicken Sie auf **Azure-Module aktualisieren**. Daraufhin wird eine Bestätigungsbenachrichtigung angezeigt, in der Sie gefragt werden, ob Sie den Vorgang fortsetzen möchten.<br><br> ![Benachrichtigung zum Aktualisieren von Azure-Modulen](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Klicken Sie auf **Ja**, um den Aktualisierungsvorgang für die Module zu starten. Der Aktualisierungsvorgang dauert etwa 15 bis 20 Minuten, und die folgenden Module werden aktualisiert:

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Wenn die Module bereits auf dem neuesten Stand sind, ist der Prozess in wenigen Sekunden abgeschlossen. Wenn der Updatevorgang abgeschlossen ist, werden Sie benachrichtigt.<br><br> ![Aktualisierungsstatus beim Aktualisieren der Azure-Module](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

    Die .NET Core-AzureRm-Module (AzureRm.*.Core) werden in Azure Automation nicht unterstützt und können nicht importiert werden.

> [!NOTE]
> In Azure Automation werden die neuesten Module in Ihrem Automation-Konto verwendet, wenn ein neuer geplanter Auftrag ausgeführt wird.  

Wenn Sie Cmdlets aus diesen Azure PowerShell-Modulen in Ihren Runbooks verwenden, dann sollten Sie diesen Updatevorgang ungefähr jeden Monat ausführen, um sicherzustellen, dass Sie über die neuesten Module verfügen. Azure Automation verwendet die „AzureRunAsConnection“-Verbindung, um sich beim Aktualisieren der Module zu authentifizieren. Wenn der Dienstprinzipal abgelaufen ist oder auf Abonnementebene nicht mehr vorhanden ist, misslingt die Modulaktualisierung.

## <a name="alternative-ways-to-update-your-modules"></a>Andere Möglichkeiten zum Aktualisieren Ihrer Module

Wie bereits erwähnt, ist die Schaltfläche**Azure-Module aktualisieren** in „Sovereign Clouds“ nicht verfügbar, sondern nur in der globalen Azure-Cloud. Der Grund ist, dass die aktuelle Version der Azure PowerShell-Module aus dem PowerShell-Katalog unter Umständen nicht mit den Resource Manager-Diensten funktioniert, die in diesen Clouds derzeit bereitgestellt wurden.

Die Aktualisierung der Module ist trotzdem möglich, indem das Runbook [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) in Ihr Automation-Konto importiert und ausgeführt wird.

Verwenden Sie den Parameter `AzureRmEnvironment`, um die richtige Umgebung an das Runbook zu übergeben.  Zulässige Werte sind **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud** und **AzureUSGovernmentCloud**. Wenn Sie keinen Wert an diesen Parameter übergeben, wird für das Runbook standardmäßig die öffentliche Azure-Cloud (**AzureCloud**) verwendet.

Falls Sie nicht die aktuelle Version des Azure PowerShell-Moduls aus dem PowerShell-Katalog, sondern eine bestimmte andere Version verwenden möchten, können Sie diese Version an den optionalen Parameter `ModuleVersionOverrides` des Runbooks **Update-AzureModule** übergeben. Beispiele hierzu finden Sie im Runbook [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1). Azure PowerShell-Module, die im Parameter `ModuleVersionOverrides` nicht erwähnt werden, werden mit den aktuellen Modulversionen aus dem PowerShell-Katalog aktualisiert. Wenn an den Parameter `ModuleVersionOverrides` nichts übergeben wird, werden alle Module mit den aktuellen Modulversionen aus dem PowerShell-Katalog aktualisiert. Dies ist das Verhalten der Schaltfläche **Azure-Module aktualisieren**.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Integrationsmodulen und zum Erstellen benutzerdefinierter Module, um Automation weiter in andere Systeme, Dienste oder Lösungen zu integrieren, finden Sie unter [Integrationsmodule](automation-integration-modules.md).

* Sie sollten die Verwendung der Integration von Quellcodeverwaltung mit [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) oder [Azure DevOps](automation-scenario-source-control-integration-with-vsts.md) in Betracht ziehen, um Releases Ihrer Automation-Runbooks und Konfigurationsportfolios zentral zu verwalten und zu steuern.  
