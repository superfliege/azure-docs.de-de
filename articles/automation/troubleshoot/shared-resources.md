---
title: Beheben von Fehlern bei freigegebenen Azure Automation-Ressourcen
description: Erfahren Sie, wie Sie Probleme mit freigegebenen Azure Automation-Ressourcen beheben.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 11/05/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 385d2969e65647ab0b5c5e21c07b127104587e7e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263415"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Beheben von Fehlern bei freigegebenen Ressourcen

In diesem Artikel werden Lösungen zum Beheben von Problemen beschrieben, die bei Verwendung der freigegebenen Ressourcen in Azure Automation auftreten können.

## <a name="modules"></a>Module

### <a name="module-stuck-importing"></a>Szenario: Ein Modul bleibt beim Importieren hängen.

#### <a name="issue"></a>Problem

Während Sie Ihre Module in Azure Automation importieren oder aktualisieren, finden Sie ein Modul, das im Zustand **Importieren** hängen geblieben ist.

#### <a name="error"></a>Error

Das Importieren von PowerShell-Modulen ist ein komplexer Vorgang mit mehreren Schritten. Bei diesen Vorgang besteht die Möglichkeit, dass ein Modul nicht ordnungsgemäß importiert wird. In diesem Fall kann das Modul, das Sie importieren, in einem Übergangszustand hängen bleiben. Weitere Informationen zu diesem Vorgang finden Sie unter [Importieren eines PowerShell-Moduls]( /powershell/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Lösung

Um dieses Problem zu beheben, müssen Sie das Modul, das im Zustand **Importieren** hängen geblieben ist, mithilfe des Cmdlets [Remove-AzureRmAutomationModule](/powershell/module/azurerm.automation/remove-azurermautomationmodule) entfernen. Sie können dann das Importieren des Moduls erneut versuchen.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.