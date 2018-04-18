---
title: Azure-Diagnoseerweiterung für Windows | Microsoft-Dokumentation
description: Überwachen von virtuellen Azure-Computern unter Windows mithilfe der Windows Azure-Diagnoseerweiterung
services: virtual-machines-windows
documentationcenter: ''
author: johnkemnetz
manager: ashwink
editor: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/06/2018
ms.author: johnkem
ms.openlocfilehash: a9621f6f93d8e14e53cfe05ca4a714e88b9d8572
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="windows-azure-diagnostics-extension"></a>Windows Azure-Diagnoseerweiterung

## <a name="overview"></a>Übersicht

Mit der VM-Erweiterung für die Windows Azure-Diagnose können Sie Überwachungsdaten wie Leistungsindikatoren und Ereignisprotokolle auf Ihrem virtuellen Windows-Computer sammeln. Sie können genau angeben, welche Daten erfasst und wo sie gespeichert werden sollen, etwa in einem Azure Storage-Konto oder einem Azure Event Hub. Sie können diese Daten auch zum Erstellen von Diagrammen im Azure-Portal bzw. zum Erstellen von Metrikwarnungen verwenden.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="operating-system"></a>Betriebssystem

Die Windows Azure-Diagnoseerweiterung ist für Windows 10-Clients und Windows Server 2008 R2, 2012, 2012 R2 und 2016 geeignet.

### <a name="internet-connectivity"></a>Internetkonnektivität

Um die Windows Azure-Diagnoseerweiterung verwenden zu können, muss der virtuelle Zielcomputer mit dem Internet verbunden sein. 

## <a name="extension-schema"></a>Erweiterungsschema

Die Schema- und Eigenschaftswerte der Windows Azure-Diagnoseerweiterung werden [in diesem Dokument](../../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md) beschrieben.

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Azure-VM-Erweiterungen können mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden. Das im vorherigen Abschnitt erläuterte JSON-Schema kann in einer Azure Resource Manager-Vorlage zum Ausführen der Windows Azure-Diagnoseerweiterung im Rahmen einer Bereitstellung mit einer Azure Resource Manager-Vorlage verwendet werden. Informationen finden Sie unter [Verwenden von Überwachung und Diagnose bei einer Windows-VM und Azure Resource Manager-Vorlagen](extensions-diagnostics-template.md).

## <a name="azure-cli-deployment"></a>Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle

Sie können die Windows Azure-Diagnoseerweiterung mithilfe der Azure-Befehlszeilenschnittstelle auf einem vorhandenen virtuellen Computer bereitstellen. Ersetzen Sie die geschützten Einstellungen und Einstellungseigenschaften mit gültigem JSON-Code aus dem oben aufgeführten Erweiterungsschema. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --version 1.9.0.0 --protected-settings protected-settings.json \
  --settings public-settings.json 
```

## <a name="powershell-deployment"></a>PowerShell-Bereitstellung

Sie können die Windows Azure-Diagnoseerweiterung mithilfe des Befehls `Set-AzureRmVMDiagnosticsExtension` auf einem vorhandenen virtuellen Computer hinzufügen. Informationen finden Sie außerdem unter [Aktivieren der Azure-Diagnose auf einer virtuellen Azure-Maschine unter Windows mithilfe von PowerShell](ps-extensions-diagnostics.md).
```powershell
$vm_resourcegroup = "myvmresourcegroup"
$vm_name = "myvm"
$diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup `
  -VMName $vm_name `
  -DiagnosticsConfigurationPath $diagnosticsconfig_path
```

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal und mithilfe der Azure-Befehlszeilenschnittstelle abgerufen werden. Führen Sie über die Azure-Befehlszeilenschnittstelle den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für einen bestimmten virtuellen Computer anzuzeigen.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

In [diesem Artikel](../../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) finden Sie ein umfassenderes Handbuch zur Problembehandlung für die Windows Azure-Diagnoseerweiterung.

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zur Windows Azure-Diagnoseerweiterung](../../monitoring-and-diagnostics/azure-diagnostics.md)
* [Informationen zum Schema und zu den Versionen der Erweiterung](../../monitoring-and-diagnostics/azure-diagnostics-schema.md)
