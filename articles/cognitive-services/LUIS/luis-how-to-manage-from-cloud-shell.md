---
title: Anzeigen der LUIS-Nutzung in der Azure Cloud Shell | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Nutzungsinformationen für LUIS in der Azure Cloud Shell abrufen können.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/08/2017
ms.author: diberry
ms.openlocfilehash: f9d3a489328263bd300d60935bfde2f0dfc0245c
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224040"
---
# <a name="manage-luis-service-from-azure-cloud-shell"></a>Verwalten des LUIS-Diensts in der Azure Cloud Shell
Das Azure-Portal ermöglicht Ihnen die Verwendung von PowerShell-Cmdlets zum Arbeiten mit LUIS-Ressourcen. 

Mithilfe dieser Cmdlets können Sie ein LUIS-Abonnement [erstellen](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/new-azurermcognitiveservicesaccount?view=azurermps-6.0.0), Informationen über das Abonnement abrufen, einschließlich der [Nutzung](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0), und das Abonnement [entfernen](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/remove-azurermcognitiveservicesaccount?view=azurermps-6.0.0). 

## <a name="cloud-shell-storage-account-and-authentication"></a>Cloud Shell-Speicherkonto und Authentifizierung
Um PowerShell in der [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell) im Azure-Portal verwenden zu können, benötigen Sie ein Azure-Speicherkonto. Wenn Sie nicht über ein [Speicherkonto](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage#set-up-a-clouddrive-file-share) verfügen, werden Sie aufgefordert, eins zu erstellen. Das Speicherkonto ermöglicht Ihnen das Speichern von PowerShell-Skripts in der Cloud Shell.  

Darüber hinaus müssen Sie sich in der Cloud Shell bei Azure authentifizieren, um auf Ressourcen zuzugreifen. 

Sobald Sie über ein Speicherkonto verfügen und authentifiziert sind, können Sie PowerShell-Cmdlets ausführen.

## <a name="open-cloud-shell"></a>Öffnen von Cloud Shell
Wenn Sie die Cloud Shell im Azure-Portal verwenden, arbeiten Sie immer mit der aktuellsten PowerShell-Version. 

Verwenden Sie die Schaltfläche **Launch Cloud Shell** (Cloud Shell starten), um die Cloud Shell zu öffnen, oder öffnen Sie einen Browser mit [https://shell.azure.com](https://shell.azure.com). 

<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>

## <a name="luis-endpoint-usage-information"></a>Nutzungsinformationen zum LUIS-Endpunkt

Das PowerShell 6.x-Cmdlet `Get-AzureRmCognitiveServicesAccountUsage` stellt Nutzungsinformationen für Microsoft Cognitive Services bereit, einschließlich LUIS. Für [Get-AzureRmCognitiveServicesAccountUsage](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0) sind die Ressourcengruppe und der Ressourcenname des Diensts erforderlich. 

Die Befehlssyntax ist wie folgt:

```
Get-AzureRmCognitiveServicesAccountUsage -ResourceGroupName my-resource-group -Name my-luis-service-name
```

Im folgenden Beispiel ist der Name der Ressourcengruppe `luis-westus-rg` und der Name des LUIS-Dienstabonnements `luis-westus-1`. Diese Namen werden beide bei der Erstellung des LUIS-Diensts ausgewählt. 

Das Cmdlet gibt Nutzungsinformationen in Form von 16 von 10.000 möglichen Endpunktaufrufen innerhalb eines Zeitraums von 30 Tagen bei einem Zeitraumende am 7. Juni zurück:

```
CurrentValue  : 16
Name          : LUIS.Calls
Limit         : 10000
Status        : Included
Unit          : Count
QuotaPeriod   : 30.00:00:00
NextResetTime : 2018-06-07T18:28:52Z
```

Speichern Sie den Befehl in dem Azure-Speicherkonto, das der Cloud Shell zugeordnet ist, als PowerShell-Datei (*.ps1), und führen Sie ihn nach Belieben aus. 

![Ausführen des Skripts aus dem Speicher](./media/luis-how-to-manage-from-powershell/run-script-from-storage.png)

Nachdem das Skript auf dem Cloud-Laufwerk gespeichert wurde, können Sie das PowerShell-Skript aus der Cloud Shell der Azure-Smartphone-App ausführen. 

![Ausführen des Skripts aus dem Speicher in der Smartphone-App](./media/luis-how-to-manage-from-powershell/phone-app.png)