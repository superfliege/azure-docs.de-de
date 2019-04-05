---
title: Aktualisieren von Azure-Modulen in Azure Automation
description: In diesem Artikel wird beschrieben, wie Sie jetzt häufig verwendete Azure PowerShell-Module, die standardmäßig in Azure Automation bereitgestellt werden, aktualisieren können.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 03174e6336589f8aa49a7fc7197da1301ff54400
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58009782"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Aktualisieren von Azure PowerShell-Modulen in Azure Automation

Zum Aktualisieren der Azure-Module in Ihrem Automation-Konto empfiehlt es sich, ab jetzt das [Update Azure Modules-Runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) zu verwenden, das nunmehr als Open Source verfügbar ist. Darüber hinaus können Sie nach wie vor die Schaltfläche **Azure-Module aktualisieren** im Portal verwenden, um Ihre Azure-Module zu aktualisieren. Weitere Informationen zum Ausführen des Open Source-Runbooks finden Sie unter [Aktualisieren von Azure-Modulen mit dem Open Source-Runbook](#open-source).

Die am häufigsten verwendeten Azure PowerShell-Module werden standardmäßig in jedem Automation-Konto bereitgestellt. Das Azure-Team aktualisiert die Azure-Module regelmäßig. Über Ihr Automation-Konto haben Sie die Möglichkeit, die Module im Konto zu aktualisieren, sobald neue Versionen im Portal verfügbar sind.

Da Module von der Produktgruppe regelmäßig aktualisiert werden, können in den enthaltenen Cmdlets Änderungen auftreten. Diese Aktion kann je nach Art der Änderung nachteilige Auswirkungen auf Ihre Runbooks haben, z.B. durch umbenannte Parameter oder ein vollständig eingestelltes Cmdlet.

Um Auswirkungen auf Ihre Runbooks und die Prozesse zu vermeiden, die sie automatisieren, führen Sie vor dem Fortsetzen des Vorgangs Tests und Überprüfungen aus. Wenn Sie für diesen Zweck kein dediziertes Automation-Konto haben, sollten Sie eines erstellen, damit Sie während der Entwicklung Ihrer Runbooks viele verschiedene Szenarios testen können. Diese Tests sollten iterative Änderungen wie das Aktualisieren der PowerShell-Module einschließen. 

Wenn Sie Ihre Skripts lokal entwickeln, wird empfohlen, lokal die gleichen Modulversionen wie in Ihrem Automation-Konto beim Testen zur Verfügung zu haben, um sicherzustellen, dass Sie die gleichen Ergebnisse erhalten. Nach dem Überprüfen der Ergebnisse und dem Anwenden aller erforderlichen Änderungen können Sie die Änderungen in die Produktion übernehmen.

> [!NOTE]
> Ein neues Automation-Konto enthält ggf. nicht die neuesten Module.

## <a name="open-source"></a>Aktualisieren von Azure-Modulen mit dem Open Source-Runbook

Um in die Verwendung des **Update-AutomationAzureModulesForAccount**-Runbooks zum Aktualisieren Ihrer Azure-Module einzusteigen, laden Sie es aus dem [Update Azure Modules-Runbook-Repository](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) auf GitHub herunter. Anschließend können Sie es in Ihr Automation-Konto importieren oder es als Skript ausführen. Anweisungen zur Vorgehensweise finden Sie im [Update Azure Modules-Runbook-Repository](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update).

### <a name="considerations"></a>Überlegungen

Die folgenden Überlegungen sollten bei der Anwendung dieses Prozesses zum Aktualisieren Ihrer Azure-Module berücksichtigt werden:

* Wenn Sie dieses Runbook mit seinem ursprünglichen Namen `Update-AutomationAzureModulesForAccount` importieren, überschreibt es das interne Runbook dieses Namens. Das hat zur Folge, dass beim Betätigen der Schaltfläche **Azure-Module aktualisieren** oder beim direkten Aufruf des Runbooks über die Azure Resource Manager-API für dieses Automation-Konto das importierte Runbook ausgeführt wird.

* Dieses Runbook unterstützt nur die gleichzeitige Aktualisierung der Module **Azure** und **AzureRM**. [Azure PowerShell-Az-Module](/powershell/azure/new-azureps-module-az) werden in Automation-Konten unterstützt, können aber mit diesem Runbook nicht aktualisiert werden.

* Vermeiden Sie die Ausführung dieses Runbooks in Automation-Konten, die Az-Module enthalten.

* Bevor Sie dieses Runbook starten, vergewissern Sie sich, dass für Ihr Automation-Konto [Azure Run As-Kontoanmeldeinformationen](manage-runas-account.md) erstellt wurden.

* Sie können diesen Code als reguläres PowerShell-Skript statt als Runbook verwenden: Melden Sie sich einfach zuerst mit dem Befehl [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) bei Azure an, und übergeben Sie dann `-Login $false` an das Skript.

* Zur Ausführung dieses Runbooks in Sovereign Clouds verwenden Sie den `AzureRmEnvironment`-Parameter, um die richtige Umgebung an das Runbook zu übergeben.  Zulässige Werte sind **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud** und **AzureUSGovernment**. Diese Werte können mithilfe von `Get-AzureRmEnvironment | select Name` abgerufen werden. Wenn Sie keinen Wert an diesen Parameter übergeben, wird für das Runbook standardmäßig die öffentliche Azure-Cloud **AzureCloud** verwendet.

* Falls Sie eine bestimmte Azure PowerShell-Modulversion anstelle der neuesten im PowerShell-Katalog verfügbaren verwenden möchten, übergeben Sie diese Versionen im optionalen Parameter `ModuleVersionOverrides` des **Update-AutomationAzureModulesForAccount**-Runbooks. Beispiele finden Sie im [Update-AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
)-Runbook. Azure PowerShell-Module, die im Parameter `ModuleVersionOverrides` nicht erwähnt sind, werden mit den aktuellen Modulversionen aus dem PowerShell-Katalog aktualisiert. Wenn Sie keine Argumente an den Parameter `ModuleVersionOverrides` übergeben, werden alle Module mit den aktuellen Modulversionen aus dem PowerShell-Katalog aktualisiert. Dieses Verhalten entspricht dem der Schaltfläche **Azure-Module aktualisieren**.

## <a name="update-azure-modules-in-the-azure-portal"></a>Aktualisieren von Azure-Modulen im Azure-Portal

1. Auf der Seite „Module“ Ihres Automation-Kontos gibt es die Option **Azure-Module aktualisieren**. Diese ist immer aktiviert.<br><br> ![Option zum Aktualisieren von Azure-Modulen auf der Seite „Module“](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

   > [!NOTE]
   > Es wird empfohlen, Ihre Azure-Module zuerst in einem Automation-Testkonto zu aktualisieren. So wird sichergestellt, dass Ihre vorhandenen Skripts wie erwartet funktionieren, bevor die Aktualisierung für Ihre Azure-Module durchgeführt wird.
   >
   > Die Schaltfläche **Azure-Module aktualisieren** ist nur in der öffentlichen Cloud verfügbar. In [Regionen mit Datenhoheit](https://azure.microsoft.com/global-infrastructure/) ist sie nicht verfügbar. Bitte verwenden Sie das **Update-AutomationAzureModulesForAccount**-Runbook zum Aktualisieren Ihrer Azure-Module. Sie können es aus dem [Update Azure Modules-Runbook-Repository](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) herunterladen. Weitere Informationen zum Ausführen des Open Source-Runbooks finden Sie unter [Aktualisieren von Azure-Modulen mit dem Open Source-Runbook](#open-source).

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

## <a name="known-issues"></a>Bekannte Probleme

Beim Aktualisieren der AzureRM-Module in einem Automation-Konto tritt in Ressourcengruppen mit einem mit 0 beginnenden numerischen Namen ein bekanntes Problem auf. Um die Azure-Module in Ihrem Automation-Konto zu aktualisieren, müssen sich die Module in einer Ressourcengruppe befinden, die einen alphanumerischen aufweist. Ressourcengruppen, deren numerischer Name mit 0 beginnt, können zurzeit keine AzureRM-Module aktualisieren.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie die Seite [Update Azure Modules-Runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update), um mehr darüber zu erfahren.
