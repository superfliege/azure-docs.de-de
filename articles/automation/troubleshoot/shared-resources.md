---
title: Beheben von Fehlern bei freigegebenen Azure Automation-Ressourcen
description: Erfahren Sie, wie Sie Probleme mit freigegebenen Azure Automation-Ressourcen beheben.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 12/3/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: ce78c86cdae9a06100fd17d00e0229805e42983b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848458"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Beheben von Fehlern bei freigegebenen Ressourcen

In diesem Artikel werden Lösungen zum Beheben von Problemen beschrieben, die bei Verwendung der freigegebenen Ressourcen in Azure Automation auftreten können.

## <a name="modules"></a>Module

### <a name="module-stuck-importing"></a>Szenario: Ein Modul bleibt beim Import hängen

#### <a name="issue"></a>Problem

Ein Modul bleibt beim Importieren oder Aktualisieren in Azure Automation im Zustand **Importieren** hängen.

#### <a name="cause"></a>Ursache

Das Importieren von PowerShell-Modulen ist ein komplexer Vorgang mit mehreren Schritten. Bei diesen Vorgang besteht die Möglichkeit, dass ein Modul nicht ordnungsgemäß importiert wird. In diesem Fall kann das Modul, das Sie importieren, in einem Übergangszustand hängen bleiben. Weitere Informationen zu diesem Vorgang finden Sie unter [Importieren eines PowerShell-Moduls]( /powershell/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Lösung

Um dieses Problem zu beheben, müssen Sie das Modul, das im Zustand **Importieren** hängen geblieben ist, mithilfe des Cmdlets [Remove-AzureRmAutomationModule](/powershell/module/azurerm.automation/remove-azurermautomationmodule) entfernen. Sie können dann das Importieren des Moduls erneut versuchen.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

## <a name="run-as-accounts"></a>Ausführende Konten

### <a name="unable-create-update"></a>Szenario: Ausführendes Konto kann nicht erstellt oder aktualisiert werden

#### <a name="issue"></a>Problem

Beim Versuch, ein ausführendes Konto zu erstellen oder zu aktualisieren, erhalten Sie eine ähnliche Fehlermeldung wie die folgende:

```error
You do not have permissions to create…
```

#### <a name="cause"></a>Ursache

Sie haben keine Berechtigungen zum Erstellen oder Aktualisieren des ausführenden Kontos, oder die Ressource ist auf Ressourcengruppenebene gesperrt.

#### <a name="resolution"></a>Lösung

Um ein ausführendes Konto zu erstellen oder zu aktualisieren, müssen Sie über geeignete Berechtigungen für die verschiedenen Ressourcen verfügen, die vom ausführenden Konto verwendet werden. Informationen zu den benötigten Berechtigungen zum Erstellen oder Aktualisieren eines ausführenden Kontos finden Sie unter [Berechtigungen zum Konfigurieren von ausführenden Konten](../manage-runas-account.md#permissions).

Wenn das Problem durch eine Sperre verursacht wird, überprüfen Sie, ob die Sperre entfernt werden kann. Navigieren Sie zur gesperrten Ressource, klicken Sie mit der rechten Maustaste auf das Schlosssymbol, und klicken Sie auf **Löschen**, um die Sperre zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Supportwebsite](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support anfordern** aus.