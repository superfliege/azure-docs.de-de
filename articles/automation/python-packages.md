---
title: Verwalten von Python 2-Paketen in Azure Automation
description: Dieser Artikel beschreibt, wie Sie Python 2-Pakete in Azure Automation verwalten.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: de0998dffeac54db5311bbcde1c9499488b23556
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54434971"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Verwalten von Python 2-Paketen in Azure Automation

Mit Azure Automation können Sie Python 2-Runbooks in Azure und Hybrid Runbook Worker unter Linux ausführen. Um die Runbooks zu vereinfachen, können Sie mithilfe von Python-Paketen die erforderlichen Module importieren. Dieser Artikel beschreibt, wie Sie Python-Pakete in Azure Automation verwalten und verwenden.

## <a name="import-packages"></a>Importieren von Paketen

Wählen Sie in Ihrem Automation-Konto unter **Python 2-Pakete** die Option **Freigegebene Ressourcen** aus. Klicken Sie auf **Python 2-Paket hinzufügen**.

![Hinzufügen eines Python-Pakets](media/python-packages/add-python-package.png)

Wählen Sie auf der Seite **Python 2-Paket hinzufügen** ein lokales Paket für den Upload aus. Das Paket kann eine Datei im Format `.whl` oder `.tar.gz` sein. Klicken Sie nach der Auswahl auf **OK**, um das Paket hochzuladen.

![Hinzufügen eines Python-Pakets](media/python-packages/upload-package.png)

Sobald ein Paket importiert wurde, wird es in Ihrem Automation-Konto auf der Seite **Python 2-Pakete** aufgelistet. Um ein Paket zu entfernen, wählen Sie das gewünschte Paket und dann auf der Paketseite **Löschen** aus.

![Paketliste](media/python-packages/package-list.png)

## <a name="use-a-package-in-a-runbook"></a>Verwenden eines Pakets in einem Runbook

Nachdem Sie ein Paket importiert haben, können Sie es in einem Runbook verwenden. Das folgende Beispiel verwendet das [Azure Automation-Hilfsprogrammpaket](https://github.com/azureautomation/azure_automation_utility). Dieses Paket erleichtert die Verwendung von Python mit Azure Automation. Um das Paket zu verwenden, folgen Sie den Anweisungen im GitHub-Repository, und fügen Sie es dem Runbook hinzu, indem Sie beispielsweise mit `from azure_automation_utility import get_automation_runas_credential` die Funktion zum Abrufen des ausführenden Kontos importieren.

```python
import azure.mgmt.resource
import automationassets
from azure_automation_utility import get_automation_runas_credential

# Authenticate to Azure using the Azure Automation RunAs service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential()

# Intialize the resource management client with the RunAs credential and subscription
resource_client = azure.mgmt.resource.ResourceManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"]))

# Get list of resource groups and print them out
groups = resource_client.resource_groups.list()
for group in groups:
    print group.name
```

## <a name="develop-and-test-runbooks-offline"></a>Entwickeln und Testen von Runbooks im Offlinemodus

Um Ihre Python 2-Runbooks offline zu entwickeln und zu testen, können Sie die für das [Python-Modul emulierten Azure Automation-Assets](https://github.com/azureautomation/python_emulated_assets) auf GitHub verwenden. Mit diesem Modul können Sie auf Ihre freigegebenen Ressourcen wie Anmeldeinformationen, Variablen, Verbindungen und Zertifikate verweisen.

## <a name="next-steps"></a>Nächste Schritte

Eine Einführung in Python 2-Runbooks finden Sie unter [Mein erstes Python 2-Runbook](automation-first-runbook-textual-python2.md).
