---
title: Integrieren von Azure Automation in die Azure DevOps Services-Quellcodeverwaltung
description: In diesem Szenario wird das Einrichten der Integration mit einem Azure Automation-Konto und der Azure DevOps Services-Quellcodeverwaltung erläutert.
services: automation
author: eamonoreilly
ms.author: eamono
keywords: Azure PowerShell, Azure DevOps Services, Quellcodeverwaltung, Automation
ms.service: automation
ms.component: process-automation
ms.topic: conceptual
ms.date: 03/19/2017
ms.openlocfilehash: ef21060e98972dd7bc561f9a4311fa0c4bdec3b4
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227060"
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-azure-devops"></a>Azure Automation-Szenario – Integration der Automation-Quellcodeverwaltung mit Azure DevOps Services

In diesem Szenario verwenden Sie ein Azure DevOps-Projekt, mit dem Sie Azure Automation-Runbooks oder DSC-Konfigurationen unter Quellcodeverwaltung verwalten.
Dieser Artikel beschreibt, wie Azure DevOps in Ihre Azure Automation-Umgebung integriert wird, sodass bei jedem Einchecken Continuous Integration erfolgt.

## <a name="getting-the-scenario"></a>Abrufen des Szenarios

Dieses Szenario basiert auf zwei PowerShell-Runbooks, die Sie direkt aus dem [Runbookkatalog](automation-runbook-gallery.md) des Azure-Portals importieren oder aus dem [PowerShell-Katalog](https://www.powershellgallery.com) herunterladen können.

### <a name="runbooks"></a>Runbooks

Runbook | BESCHREIBUNG| 
--------|------------|
Sync-VSTS | Importieren Sie Runbooks oder Konfigurationen aus der Azure DevOps-Quellcodeverwaltung, wenn ein Eincheckvorgang ausgeführt wird. Bei der manuellen Ausführung werden alle Runbooks oder Konfigurationen in das Automation-Konto importiert und veröffentlicht.| 
Sync-VSTSGit | Importieren Sie Runbooks oder Konfigurationen aus Azure DevOps unter der Git-Quellcodeverwaltung, wenn ein Eincheckvorgang ausgeführt wird. Bei der manuellen Ausführung werden alle Runbooks oder Konfigurationen in das Automation-Konto importiert und veröffentlicht.|

### <a name="variables"></a>Variables

Variable | BESCHREIBUNG|
-----------|------------|
VSToken | Sichern Sie das erstellte Variablenasset, das das persönliche Azure DevOps-Zugriffstoken enthält. Sie finden Informationen zum Erstellen eines persönlichen Azure DevOps-Zugriffstokens auf der [Azure DevOps-Authentifizierungsseite](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).
## <a name="installing-and-configuring-this-scenario"></a>Installieren und Konfigurieren dieses Szenarios

Erstellen Sie ein [persönliches Zugriffstoken](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) in Azure DevOps, mit dem Sie die Runbooks oder Konfigurationen in Ihrem Automation-Konto synchronisieren.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Erstellen Sie eine [sichere Variable](automation-variables.md) in Ihrem Automation-Konto, die das persönliche Zugriffstoken aufnimmt, damit das Runbook bei Azure DevOps authentifiziert werden kann und die Runbooks oder Konfigurationen im Automation-Konto synchronisiert werden können. Sie können es VSToken nennen.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Importieren Sie das Runbook, das Ihre Runbooks oder Konfigurationen im Automation-Konto synchronisiert. Sie können das [Azure DevOps-Beispielrunbook](https://www.powershellgallery.com/packages/Sync-VSTS) oder das [Azure DevOps-Beispielrunbook mit Git](https://www.powershellgallery.com/packages/Sync-VSTSGit) aus dem [PowerShell-Katalog](https://www.powershellgallery.com) verwenden. Dies hängt davon ab, ob Sie die Azure DevOps-Quellcodeverwaltung oder Azure DevOps mit Git verwenden. Stellen Sie das Runbook in Ihrem Automation-Konto bereit.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Sie können dieses Runbook jetzt [veröffentlichen](automation-creating-importing-runbook.md#publishing-a-runbook), sodass Sie einen Webhook erstellen können. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Erstellen Sie einen [Webhook](automation-webhooks.md) für dieses Sync-VSTS-Runbook, und geben Sie die Parameter an, wie unten dargestellt. Sie müssen die Webhook-URL kopieren, da Sie sie für einen Service Hook in Azure DevOps benötigen. VSAccessTokenVariableName ist der Name (VSToken) der sicheren Variablen, die Sie zuvor für das persönliche Zugriffstoken erstellt haben. 

Für das Integrieren in Azure DevOps (Sync-VSTS.ps1) werden die folgenden Parameter verwendet:
### <a name="sync-vsts-parameters"></a>Sync-VSTS-Parameter

Parameter | BESCHREIBUNG| 
--------|------------|
WebhookData | Dieser Parameter enthält die vom Azure DevOps-Service Hook gesendeten Informationen über das Einchecken. Sie sollten diesen Parameter leer lassen.| 
ResourceGroup | Dies ist der Name der Ressourcengruppe, in der sich das Automation-Konto befindet.|
AutomationAccountName | Der Name des Automation-Kontos, das mit Azure DevOps synchronisiert wird.|
VSFolder | Der Name des Ordners in Azure DevOps, in dem sich die Runbooks und Konfigurationen befinden.|
VSAccount | Der Name der Azure DevOps-Organisation.| 
VSAccessTokenVariableName | Der Name der sicheren Variablen (VSToken), die das persönliche Azure DevOps-Zugriffstoken enthält.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Wenn Sie Azure DevOps mit Git (Sync-VSTSGit.ps1) verwenden, werden die folgenden Parameter verwendet.

Parameter | BESCHREIBUNG|
--------|------------|
WebhookData | Dieser Parameter enthält die vom Azure DevOps-Service Hook gesendeten Informationen über das Einchecken. Sie sollten diesen Parameter leer lassen.| 
ResourceGroup | Dies ist der Name der Ressourcengruppe, in der sich das Automation-Konto befindet.|
AutomationAccountName | Der Name des Automation-Kontos, das mit Azure DevOps synchronisiert wird.|
VSAccount | Der Name der Azure DevOps-Organisation.|
VSProject | Der Name des Projekts in Azure DevOps, in dem sich die Runbooks und Konfigurationen befinden.|
GitRepo | Der Name des Git-Repositorys.|
GitBranch | Der Name der Verzweigung im Azure DevOps-Git-Repository.|
Ordner | Der Name des Ordners im Azure DevOps-Git-Branch.|
VSAccessTokenVariableName | Der Name der sicheren Variablen (VSToken), die das persönliche Azure DevOps-Zugriffstoken enthält.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Erstellen Sie einen Service Hook in Azure DevOps für Eincheckvorgänge in den Ordner, der diesen Webhook beim Einchecken von Code auslöst. Wählen Sie **Webhooks** als Dienst für die Integration aus, wenn Sie ein neues Abonnement erstellen. Weitere Informationen zu Service Hooks finden Sie in der [Azure DevOps Service Hooks-Dokumentation](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Sie sollten jetzt in der Lage sein, alle Eincheckvorgänge Ihrer Runbooks und Konfigurationen in Azure DevOps durchzuführen und diese automatisch in Ihrem Automation-Konto zu synchronisieren.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Wenn Sie dieses Runbook manuell ausführen, ohne dass es von Azure DevOps ausgelöst wird, können Sie den webhookdata-Parameter leer lassen. Dann wird eine vollständige Synchronisierung aus dem angegebenen Azure DevOps-Ordner durchgeführt.

Wenn Sie das Szenario deinstallieren möchten, entfernen Sie den Service Hook aus Azure DevOps, löschen Sie das Runbook und die VSToken-Variable.
