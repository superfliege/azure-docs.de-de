---
title: NVIDIA-GPU-Treibererweiterung – Azure-Linux-VMs | Microsoft-Dokumentation
description: Microsoft Azure-Erweiterung für die Installation von NVIDIA-GPU-Treibern auf Compute-VMs der N-Serie unter Linux.
services: virtual-machines-linux
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/20/2018
ms.author: roiyz
ms.openlocfilehash: 307bdb5fa7a5d14a77c71d0ea40634a55d8507b6
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "42144366"
---
# <a name="nvidia-gpu-driver-extension-for-linux"></a>NVIDIA-GPU-Treibererweiterung für Linux

## <a name="overview"></a>Übersicht

Diese Erweiterung installiert NVIDIA-GPU-Treiber auf Linux-VMs der N-Serie. Je nach VM-Familie installiert die Erweiterung CUDA- oder GRID-Treiber. Bei der Installation von NVIDIA Treibern mit dieser Erweiterung akzeptieren Sie die Bedingungen des NVIDIA-Endbenutzer-Lizenzvertrags und stimmen diesen zu. Während der Installation wird Ihr virtueller Computer möglicherweise neu gestartet, um die Treibereinrichtung abzuschließen.

Es ist auch eine Erweiterung zum Installieren von NVIDIA-GPU-Treibern auf [Windows-VMs der N-Serie](hpccompute-gpu-windows.md) verfügbar.

Die Bestimmungen des NVIDIA-Endbenutzer-Lizenzvertrags befinden sich hier: https://go.microsoft.com/fwlink/?linkid=874330

## <a name="prerequisites"></a>Voraussetzungen

### <a name="operating-system"></a>Betriebssystem

Diese Erweiterung unterstützt die folgenden Betriebssysteme:

| Distribution | Version |
|---|---|
| Linux: Ubuntu | 16.04 LTS |
| Linux: Red Hat Enterprise Linux | 7.3, 7.4 |
| Linux: CentOS | 7.3, 7.4 |

### <a name="internet-connectivity"></a>Internetkonnektivität

Die Microsoft Azure-Erweiterung für NVIDIA-GPU-Treiber erfordert, dass der virtuelle Zielcomputer mit dem Internet verbunden ist und Zugriff hat.

## <a name="extension-schema"></a>Erweiterungsschema

Der folgende JSON-Code zeigt das Schema für die Erweiterung.

```json
{
  "name": "<myExtensionName>",
  "type": "extensions",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <myVM>)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="property-values"></a>Eigenschaftswerte

| NAME | Wert/Beispiel | Datentyp |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| Herausgeber | Microsoft.HpcCompute | Zeichenfolge |
| type | NvidiaGpuDriverLinux | Zeichenfolge |
| typeHandlerVersion | 1.1 | int |


## <a name="deployment"></a>Bereitstellung


### <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage 

Azure-VM-Erweiterungen können mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden. Vorlagen sind ideal, wenn Sie virtuelle Computer bereitstellen, die nach der Bereitstellung konfiguriert werden müssen.

Die JSON-Konfiguration für eine VM-Erweiterung kann innerhalb der VM-Ressource geschachtelt oder im Stamm bzw. auf der obersten Ebene einer Resource Manager-JSON-Vorlage platziert werden. Die Platzierung der JSON-Konfiguration wirkt sich auf den Wert von Name und Typ der Ressource aus. Weitere Informationen finden Sie unter [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md) (Festlegen von Name und Typ für untergeordnete Ressourcen). 

Im folgenden Beispiel wird davon ausgegangen, dass die Erweiterung in der VM-Ressource geschachtelt ist. Beim Schachteln der Ressource für die Erweiterung wird der JSON-Code im `"resources": []`-Objekt des virtuellen Computers platziert.

```json
{
  "name": "myExtensionName",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', myVM)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="powershell"></a>PowerShell

```powershell
Set-AzureRmVMExtension
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Location "southcentralus" `
    -Publisher "Microsoft.HpcCompute" `
    -ExtensionName "NvidiaGpuDriverLinux" `
    -ExtensionType "NvidiaGpuDriverLinux" `
    -TypeHandlerVersion 1.1 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverLinux `
  --publisher Microsoft.HpcCompute `
  --version 1.1 `
  --settings '{ `
  }'
```

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal und mithilfe von Azure PowerShell und der Azure-Befehlszeilenschnittstelle abgerufen werden. Führen Sie den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für einen bestimmten virtuellen Computer anzuzeigen.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Die Ausgabe der Erweiterungsausführung wird in der folgenden Datei protokolliert:

```bash
/var/log/azure/nvidia-vmext-status
```

### <a name="exit-codes"></a>Exitcodes

| Exitcode | Bedeutung | Mögliche Aktion |
| :---: | --- | --- |
| 0 | Vorgang erfolgreich |
| 1 | Falsche Verwendung der Erweiterung. | Wenden Sie sich mit dem Ausgabeprotokoll zur Ausführung an den Support. |
| 10 | Linux Integration Services für Hyper-V und Azure ist nicht verfügbar oder nicht installiert. | Überprüfen Sie die Ispci-Ausgabe. |
| 11 | NVIDIA-GPU kann bei dieser VM-Größe nicht gefunden werden. | Verwenden Sie eine [VM-Größe und ein Betriebssystem](../linux/n-series-driver-setup.md), die unterstützt werden. |
| 12 | Imageangebot nicht unterstützt |
| 13 | VM-Größe nicht unterstützt | Verwenden eines virtuellen Computers der N-Serie für die Bereitstellung |
| 14 | Vorgang nicht erfolgreich | |
| 21 | Fehler beim Update unter Ubuntu | Überprüfen Sie die Ausgabe von „sudo apt-get update“ |


### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/community/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Erweiterungen finden Sie unter [Erweiterungen und Features für virtuelle Computer für Linux](features-linux.md).

Weitere Informationen zu virtuellen Computern der N-Serie finden Sie unter [Für GPU optimierte VM-Größen](../linux/sizes-gpu.md).