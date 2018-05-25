---
title: VM-Erweiterung für den Azure Network Watcher-Agent für Windows | Microsoft-Dokumentation
description: Stellen Sie den Network Watcher-Agent mithilfe einer VM-Erweiterung auf einem virtuellen Windows-Computer bereit.
services: virtual-machines-windows
documentationcenter: ''
author: gurudennis
manager: amku
editor: ''
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 29f346b2a42f8d12e26bd59fbab86d763d3f29f0
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>VM-Erweiterung für den Network Watcher-Agent für Windows

## <a name="overview"></a>Übersicht

[Azure Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md) ist ein Dienst zur Überwachung, Diagnose und Analyse der Netzwerkleistung, der die Überwachung von Azure-Netzwerken ermöglicht. Die VM-Erweiterung des Network Watcher-Agents ist eine Voraussetzung für die bedarfsgesteuerte Erfassung von Netzwerkdatenverkehr und andere erweiterte Funktionalität auf virtuellen Azure-Computern.


Dieses Dokument enthält ausführliche Informationen zu den unterstützten Plattformen und Bereitstellungsoptionen für die VM-Erweiterung für den Network Watcher-Agent für Windows. Die Installation des Agents führt nicht zu einer Unterbrechung, und der virtuelle Computer muss nicht neu gestartet werden.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="operating-system"></a>Betriebssystem

Die Network Watcher-Agent-Erweiterung für Windows kann unter Windows Server 2008 R2, 2012, 2012 R2 und 2016 ausgeführt werden. Nano Server wird zurzeit nicht unterstützt.

### <a name="internet-connectivity"></a>Internetkonnektivität

Für einige Funktionen des Network Watcher-Agents muss der virtuelle Zielcomputer mit dem Internet verbunden sein. Ohne die Möglichkeit zum Herstellen ausgehender Verbindungen kann der Network Watcher-Agent keine Paketerfassungen in Ihr Speicherkonto hochladen. Weitere Informationen finden Sie in der [Dokumentation zu Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>Erweiterungsschema

Der folgende JSON-Code zeigt das Schema für die Network Watcher-Agent-Erweiterung. Die Erweiterung erfordert oder unterstützt keine vom Benutzer bereitgestellten Einstellungen, sondern verwendet ausschließlich die Standardkonfiguration.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Eigenschaftswerte

| NAME | Wert/Beispiel |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Herausgeber | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Sie können Azure-VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen bereitstellen. Sie können das im vorherigen Abschnitt erläuterte JSON-Schema in einer Azure Resource Manager-Vorlage verwenden, um die Network Watcher-Agent-Erweiterung während einer Bereitstellung über eine Azure Resource Manager-Vorlage auszuführen.

## <a name="powershell-deployment"></a>PowerShell-Bereitstellung

Mit dem Befehl `Set-AzureRmVMExtension` können Sie die VM-Erweiterung für den Network Watcher-Agent auf einem vorhandenen virtuellen Computer bereitstellen:

```powershell
Set-AzureRmVMExtension `
  -ResourceGroupName "myResourceGroup1" `
  -Location "WestUS" `
  -VMName "myVM1" `
  -Name "networkWatcherAgent" `
  -Publisher "Microsoft.Azure.NetworkWatcher" `
  -Type "NetworkWatcherAgentWindows" `
  -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a>Problembehandlung und Support

### <a name="troubleshooting"></a>Problembehandlung

Sie können Daten zum Status von Erweiterungsbereitstellungen über das Azure-Portal und mithilfe von PowerShell abrufen. Führen Sie über das Azure PowerShell-Modul den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für eine bestimmte VM anzuzeigen:

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Die Ausgabe der Erweiterungsausführung wird in Dateien im folgenden Verzeichnis protokolliert:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Support

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie die Benutzerdokumentation zu Network Watcher lesen oder den Azure-Experten im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) Fragen stellen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).
