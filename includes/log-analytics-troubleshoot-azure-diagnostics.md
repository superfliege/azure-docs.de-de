---
author: mgoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 11/09/2018
ms.author: magoedte
ms.openlocfilehash: 44febf95c660601df78047fc473f61e0d3169890
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52270322"
---
### <a name="troubleshoot-azure-diagnostics"></a>Behandeln von Problemen mit Azure Diagnostics

Wenn Sie die folgende Fehlermeldung erhalten, ist der Ressourcenanbieter „Microsoft.insights“ nicht registriert:

`Failed to update diagnostics for 'resource'. {"code":"Forbidden","message":"Please register the subscription 'subscription id' with Microsoft.Insights."}`

Führen Sie im Azure-Portal die folgenden Schritte aus, um den Ressourcenanbieter zu registrieren:

1.  Klicken Sie im linken Navigationsbereich auf *Abonnements*.
2.  Wählen Sie das in der Fehlermeldung angegebene Abonnement aus.
3.  Klicken Sie auf *Ressourcenanbieter*.
4.  Suchen Sie nach dem Anbieter *Microsoft.insights*.
5.  Klicken Sie auf den Link *Registrieren*.

![Registrierung des Ressourcenanbieters „Microsoft.insights“](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

Wiederholen Sie nach der Registrierung des Ressourcenanbieters *Microsoft.insights* die Diagnosekonfiguration.


Wenn Sie in PowerShell die folgende Fehlermeldung erhalten, müssen Sie Ihre Version von PowerShell aktualisieren:

`Set-AzureRmDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

Aktualisieren Sie PowerShell auf die Version von November 2016 (v2.3.0) oder höher, indem Sie die Anleitung im Artikel [Erste Schritte mit Azure PowerShell-Cmdlets](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) befolgen.
