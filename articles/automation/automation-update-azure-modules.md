---
title: Aktualisieren von Azure-Modulen in Azure Automation
description: In diesem Artikel wird beschrieben, wie Sie jetzt häufig verwendete Azure PowerShell-Module, die standardmäßig in Azure Automation bereitgestellt werden, aktualisieren können.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 11/20/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9fc605ab45241280d9331ad7d515ba007a015daa
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52583652"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Aktualisieren von Azure PowerShell-Modulen in Azure Automation

Die am häufigsten verwendeten Azure PowerShell-Module werden standardmäßig in jedem Automation-Konto bereitgestellt. Das Azure-Team aktualisiert die Azure-Module regelmäßig. Über Ihr Automation-Konto haben Sie die Möglichkeit, die Module im Konto zu aktualisieren, sobald neue Versionen im Portal verfügbar sind.

Da Module von der Produktgruppe regelmäßig aktualisiert werden, können in den enthaltenen Cmdlets Änderungen auftreten. Diese Aktion kann je nach Art der Änderung nachteilige Auswirkungen auf Ihre Runbooks haben, z.B. durch umbenannte Parameter oder ein vollständig eingestelltes Cmdlet. Um Auswirkungen auf Ihre Runbooks und die Prozesse zu vermeiden, die sie automatisieren, führen Sie vor dem Fortsetzen des Vorgangs Tests und Überprüfungen aus. Wenn Sie für diesen Zweck kein dediziertes Automation-Konto haben, sollten Sie eines erstellen, damit Sie während der Entwicklung Ihrer Runbooks viele verschiedene Szenarios testen können. Diese Tests sollten iterative Änderungen wie das Aktualisieren der PowerShell-Module einschließen. Nach dem Überprüfen der Ergebnisse und dem Anwenden aller erforderlichen Änderungen können Sie die Änderungen in die Produktion übernehmen.

> [!NOTE]
> Ein neues Automation-Konto enthält ggf. nicht die neuesten Module.

## <a name="updating-azure-modules"></a>Aktualisieren von Azure-Modulen

1. Auf der Seite „Module“ Ihres Automation-Kontos gibt es die Option **Azure-Module aktualisieren**. Diese ist immer aktiviert.<br><br> ![Option zum Aktualisieren von Azure-Modulen auf der Seite „Module“](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

  > [!NOTE]
  > Es wird empfohlen, Ihre Azure-Module zuerst in einem Automation-Testkonto zu aktualisieren. So wird sichergestellt, dass Ihre vorhandenen Skripts wie erwartet funktionieren, bevor die Aktualisierung für Ihre Azure-Module durchgeführt wird.
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

Wenn Sie Cmdlets aus diesen Azure PowerShell-Modulen in Ihren Runbooks verwenden, dann sollten Sie diesen Updatevorgang ungefähr jeden Monat ausführen, um sicherzustellen, dass Sie über die neuesten Module verfügen. Azure Automation verwendet die `AzureRunAsConnection`-Verbindung zur Authentifizierung beim Aktualisieren der Module. Wenn der Dienstprinzipal abgelaufen oder nicht mehr auf Abonnementebene vorhanden ist, tritt bei der Modulaktualisierung ein Fehler auf.

## <a name="alternative-ways-to-update-your-modules"></a>Andere Möglichkeiten zum Aktualisieren Ihrer Module

Wie bereits erwähnt, ist die Schaltfläche**Azure-Module aktualisieren** in Sovereign Clouds nicht verfügbar, sondern nur in der globalen Azure-Cloud. Der Grund ist, dass die aktuelle Version der Azure PowerShell-Module aus dem PowerShell-Katalog unter Umständen nicht mit den Resource Manager-Ressourcen funktioniert, die in diesen Clouds derzeit bereitgestellt wurden.

Sie können das Runbook [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) importieren und ausführen, um zu versuchen, die Azure-Module in Ihrem Automation-Konto zu aktualisieren. Bei diesem Vorgang kann ein Fehler auftreten, wenn die Versionen, die Sie aus dem Katalog importieren möchten, mit den derzeit in der Azure-Zielumgebung bereitgestellten Azure-Diensten nicht kompatibel sind. Daher müssen Sie möglicherweise sicherstellen, dass in den Runbook-Parametern die kompatiblen Versionen der Module angegeben sind.

Verwenden Sie den Parameter `AzureRmEnvironment`, um die richtige Umgebung an das Runbook zu übergeben.  Zulässige Werte sind **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud** und **AzureUSGovernment**. Diese Werte erhalten Sie durch `Get-AzureRmEnvironment | select Name`. Wenn Sie keinen Wert an diesen Parameter übergeben, wird für das Runbook standardmäßig die öffentliche Azure-Cloud **AzureCloud** verwendet.

Falls Sie nicht die aktuelle Version des Azure PowerShell-Moduls aus dem PowerShell-Katalog, sondern eine bestimmte andere Version verwenden möchten, können Sie diese Version an den optionalen Parameter `ModuleVersionOverrides` des Runbooks **Update-AzureModule** übergeben. Beispiele hierzu finden Sie im Runbook [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1). Azure PowerShell-Module, die im Parameter `ModuleVersionOverrides` nicht erwähnt sind, werden mit den aktuellen Modulversionen aus dem PowerShell-Katalog aktualisiert. Wenn Sie keine Argumente an den Parameter `ModuleVersionOverrides` übergeben, werden alle Module mit den aktuellen Modulversionen aus dem PowerShell-Katalog aktualisiert. Dieses Verhalten entspricht dem der Schaltfläche **Azure-Module aktualisieren**.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Integrationsmodulen und zum Erstellen benutzerdefinierter Module, um Automation weiter in andere Systeme, Dienste oder Lösungen zu integrieren, finden Sie unter [Integrationsmodule](automation-integration-modules.md).

