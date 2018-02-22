---
title: "Konfigurieren des gewünschten Zustands für Azure – Übersicht | Microsoft Docs"
description: "Übersicht über die Verwendung des Microsoft Azure-Erweiterungshandlers für PowerShell DSC. Mit Informationen zu Voraussetzungen, Architektur, Cmdlets..."
services: virtual-machines-windows
documentationcenter: 
author: mgreenegit
manager: timlt
editor: 
tags: azure-resource-manager
keywords: DSC
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 02/02/2018
ms.author: migreene
ms.openlocfilehash: ed8b5bc54baa3a5abfb596b202f0af58e1b6c74f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Einführung in den Handler der Azure-Erweiterung zum Konfigurieren des gewünschten Zustands

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Der Azure VM-Agent und die dazugehörigen Erweiterungen sind Teil der Microsoft Azure-Infrastrukturdienste.
VM-Erweiterungen sind Softwarekomponenten, die die VM-Funktionalität erweitern und verschiedene Verwaltungsvorgänge für virtuelle Computer vereinfachen.

Der primäre Anwendungsfall für die DSC-Erweiterung (Desired State Configuration) besteht im Bootstrap eines virtuellen Computers zum [Azure Automation DSC-Dienst](../../automation/automation-dsc-overview.md), der [Vorteile](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig.md#pull-service) einschließlich der fortlaufenden Verwaltung der VM-Konfiguration und der Integration mit anderen Betriebstools wie Azure Monitoring bereitstellt.

Die DSC-Erweiterung kann auch unabhängig vom Azure Automation DSC-Dienst verwendet werden, dies ist jedoch eine einmalige singuläre Aktion während der Bereitstellung, und es gibt nur lokal im virtuellen Computer eine fortlaufende Berichterstellung oder Verwaltung der Konfiguration.

Dieser Artikel enthält Informationen zu beiden Szenarien, dem Onboarding der DSC-Erweiterung für Azure Automation und der Verwendung der DSC-Erweiterung als Tool zum Zuweisen von Konfigurationen zu virtuellen Computern mit dem Azure SDK.

## <a name="prerequisites"></a>Voraussetzungen

- **Lokaler Computer:** Für die Interaktion mit der Erweiterung für virtuelle Azure-Computer müssen Sie das Azure-Portal oder das Azure PowerShell SDK verwenden.
- **Gast-Agent:** Der virtuelle Azure-Computer, der mit der DSC-Konfiguration konfiguriert wird, muss über ein Betriebssystem verfügen, das Version 4.0 oder höher von Windows Management Framework (WMF) unterstützt. Eine vollständige Liste der unterstützten Betriebssystemversionen finden Sie in der [Versionsgeschichte der DSC-Erweiterung](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Begriffe und Konzepte

Für dieses Handbuch müssen Sie mit folgenden Konzepten vertraut sein:

- **Konfiguration:** ein DSC-Konfigurationsdokument.
- **Knoten:** ein Ziel für die DSC-Konfiguration. In diesem Dokument bezieht sich „Knoten“ immer auf einen virtuellen Azure-Computer.
- **Konfigurationsdaten:** eine PSD1-Datei mit Umgebungsdaten für eine Konfiguration

## <a name="architectural-overview"></a>Übersicht über die Architektur

Die Azure DSC-Erweiterung nutzt das VM-Agent-Framework von Azure zur Übermittlung und Inkraftsetzung von DSC-Konfigurationen auf virtuellen Azure-Computern sowie zur Erstellung entsprechender Berichte.
Die DSC-Erweiterung nimmt ein Konfigurationsdokument und einen Satz von Parametern entgegen.
Wird keine Datei angegeben, wird ein [Standardkonfigurationsskript](###default-configuration-script) in die Erweiterung eingebettet, das ausschließlich zum Festlegen von Metadaten im [lokalen Konfigurations-Manager](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig) verwendet wird.

Wenn die Erweiterung das erste Mal aufgerufen wird, installiert es eine Version von Windows Management Framework (WMF) unter Verwendung der folgenden Logik:

1. Bei einem virtuellen Azure-Computer unter Windows Server 2016 wird keine Aktion ausgeführt. Unter Windows Server 2016 ist bereits die neueste Version von PowerShell installiert.
2. Bei Angabe der Eigenschaft `wmfVersion` wird die entsprechende WMF-Version installiert (es sei denn, sie ist nicht mit dem Betriebssystem des virtuellen Computers kompatibel).
3. Ohne Angabe der Eigenschaft `wmfVersion` wird die neueste geeignete WMF-Version installiert.

Die WMF-Installation erfordert einen Neustart.
Nach dem Neustart lädt die Erweiterung die in der `modulesUrl`-Eigenschaft angegebene ZIP-Datei (sofern zutreffend) herunter.
Befindet sich der Speicherort in Azure Blob Storage, kann in der `sasToken`-Eigenschaft ein SAS-Token für den Dateizugriff angegeben werden.
Nachdem die ZIP-Datei heruntergeladen und entpackt wurde, wird durch Ausführen der in `configurationFunction` definierten Konfigurationsfunktion eine MOF-Datei generiert.
Anschließend führt die Erweiterung `Start-DscConfiguration -Force` mit der generierten MOF-Datei aus.
Die Erweiterung erfasst die Ausgabe und schreibt sie in den Azure-Statuskanal.

### <a name="default-configuration-script"></a>Standardkonfigurationsskript

Die Azure-DSC-Erweiterung enthält ein Standardkonfigurationsskript, das für das Onboarding eines virtuellen Computers in den Azure Automation DSC-Dienst vorgesehen ist.
Die Skriptparameter sind an den konfigurierbaren Eigenschaften des [lokalen Konfigurations-Managers](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig) ausgerichtet.
Die Skriptparameter sind [dokumentiert](extensions-dsc-template.md##default-configuration-script), und das vollständige Skript steht bei [GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true) zur Verfügung.

## <a name="dsc-extension-in-arm-templates"></a>DSC-Erweiterung in ARM-Vorlagen

Bereitstellungsvorlagen von Azure Resource Manager (ARM) sind die erwartete Methode für die Arbeit mit der DSC-Erweiterung in den meisten Szenarien.
Weitere Informationen und Beispiele zum Einschließen der DSC-Erweiterung in ARM-Bereitstellungsvorlagen finden Sie auf der dedizierten Dokumentationsseite [DSC-Erweiterung mit Azure Resource Manager-Vorlagen](extensions-dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>PowerShell-Cmdlets für die DSC-Erweiterung

Die PowerShell-Cmdlets zum Verwalten der DSC-Erweiterung werden am besten für die interaktive Problembehandlung und zum Sammeln von Informationen verwendet.
Die Cmdlets können zum Verpacken, Veröffentlichen und Überwachen der Bereitstellung der DSC-Erweiterung verwendet werden.
Beachten Sie, dass Cmdlets für die DSC-Erweiterung noch nicht für die Verwendung mit dem [Standardkonfigurationsskript](###default-configuration-script) aktualisiert wurden.

`Publish-AzureRMVMDscConfiguration` durchsucht eine Konfigurationsdatei nach abhängigen DSC-Ressourcen und erstellt eine ZIP-Datei mit der Konfiguration und erforderlichen DSC-Ressourcen für deren Inkraftsetzung.
Mithilfe des Parameters `-ConfigurationArchivePath` kann das Paket auch lokal erstellt werden.
Andernfalls wird die ZIP-Datei in Azure Blob Storage veröffentlicht und durch ein SAS-Token geschützt.

Bei der durch dieses Cmdlet erstellten ZIP-Datei befindet sich das PS1-Skript im Stammverzeichnis des Archivordners.
Der Modulordner für Ressourcen wird im Archivordner platziert.

`Set-AzureRMVMDscExtension` fügt die erforderlichen Einstellungen für die PowerShell-DSC-Erweiterung in ein VM-Konfigurationsobjekt ein.

`Get-AzureRMVMDscExtension` ruft den DSC-Erweiterungsstatus eines bestimmten virtuellen Computers ab.

`Get-AzureRMVMDscExtensionStatus` ruft den Status der DSC-Konfiguration ab, die durch den DSC-Erweiterungs-Handler in Kraft gesetzt wurde.
Diese Aktion kann auf einem einzelnen virtuellen Computer oder einer Gruppe virtueller Computer ausgeführt werden.

`Remove-AzureRMVMDscExtension` entfernt den Erweiterungs-Handler von einem bestimmten virtuellen Computer.
Dieses Cmdlet bewirkt **nicht** die Entfernung der Konfiguration, die Deinstallation von WMF oder eine Änderung der angewendeten Einstellungen auf dem virtuellen Computer.
Es wird lediglich der Erweiterungshandler entfernt. 

Wichtige Informationen zu den Cmdlets für die AzureRM-DSC-Erweiterung:

- Azure Resource Manager-Cmdlets sind synchron.
- Bei ResourceGroupName, VMName, ArchiveStorageAccountName, Version und Location handelt es sich um erforderliche Parameter.
- ArchiveResourceGroupName ist ein optionaler Parameter. Diesen Parameter können Sie angeben, wenn Ihr Speicherkonto nicht der Ressourcengruppe angehört, in der der virtuelle Computer erstellt wird.
- Der Schalter AutoUpdate ermöglicht die automatische Aktualisierung des Erweiterungshandlers auf die neueste Version, sobald diese verfügbar ist. Hierdurch wird der virtuelle Computer unter Umständen neu gestartet, wenn eine neue WMF-Version veröffentlicht wird.

### <a name="getting-started-with-cmdlets"></a>Erste Schritte mit Cmdlets

Die Azure-DSC-Erweiterung kann DSC-Konfigurationsdokumente direkt zum Konfigurieren von virtuellen Azure-Computern während der Bereitstellung verwenden. Dabei wird jedoch der Knoten nicht bei Azure Automation registriert, sodass er **nicht** zentral verwaltet wird.

Im Anschluss sehen Sie ein einfaches Beispiel für eine Konfiguration.
Speichern Sie es lokal als „IisInstall.ps1“:

```powershell
configuration IISInstall
{
    node "localhost"
    {
        WindowsFeature IIS
        {
            Ensure = "Present"
            Name = "Web-Server"
        }
    }
}
```

In den folgenden Schritten wird das Skript „IisInstall.ps1“ auf dem angegebenen virtuellen Computer platziert, die Konfiguration ausgeführt und der Status gemeldet.

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.72 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"
```

## <a name="azure-portal-functionality"></a>Funktionen des Azure-Portals

Navigieren Sie zu einem virtuellen Computer. Klicken Sie unter „Einstellungen“ > „Allgemein“ auf „Erweiterungen“.
Ein neuer Bereich wird erstellt.
Klicken Sie auf „Hinzufügen“, und wählen Sie „PowerShell DSC“ aus.

Das Portal benötigt Eingabe.
**Configuration Modules or Script** (Konfigurationsmodule oder -skript): Dieses Feld ist obligatorisch. (Das Formular wurde nicht für das [Standardkonfigurationsskript](###default-configuration-script) aktualisiert.)
Erfordert eine PS1-Datei mit einem Konfigurationsskript oder eine ZIP-Datei mit einem PS1-Konfigurationsskript am Verzeichnisstamm und allen abhängigen Ressourcen in Modulordnern innerhalb der ZIP.
Die Datei kann mithilfe des Cmdlets `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` aus dem Azure PowerShell SDK erstellt werden.
Die ZIP-Datei wird in Ihren Benutzer-Blobspeicher hochgeladen und durch ein SAS-Token geschützt.

**PSD1-Konfigurationsdatendatei**: Dieses Feld ist optional.
Wenn Ihre Konfiguration eine Konfigurationsdatendatei in PSD1 erfordert, wählen Sie mithilfe dieses Felds eine solche Datei aus, und laden Sie sie in Ihren Benutzer-Blobspeicher hoch, wo sie durch ein SAS-Token geschützt wird.

**Modulspezifischer Konfigurationsname**: PS1-Dateien können mehrere Konfigurationsfunktionen besitzen.
Geben Sie den Namen des PS1-Konfigurationsskripts ein – gefolgt von \' und dem Namen der Konfigurationsfunktion.
Wenn der Name des PS1-Skripts z.B. „configuration.ps1“ und die Konfiguration „IisInstall“ lautet, geben Sie Folgendes ein: `configuration.ps1\IisInstall`

**Konfigurationsargumente**: Falls die Konfigurationsfunktion Argumente akzeptiert, geben Sie diese hier im folgenden Format ein: `argumentName1=value1,argumentName2=value2`.
Hierbei handelt es sich um ein anderes Format als bei den Konfigurationsargumenten, die von PowerShell-Cmdlets oder Resource Manager-Vorlagen akzeptiert werden.

## <a name="logging"></a>Protokollierung
Speicherort der Protokolle:

```powerShell
C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu PowerShell DSC finden Sie im [PowerShell-Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview). 

Sehen Sie sich die [Azure Resource Manager-Vorlage für die DSC-Erweiterung](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)an. 

Weitere Funktionen, die Sie mit PowerShell DSC verwalten können, finden Sie, indem Sie den [PowerShell-Katalog](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) nach weiteren DSC-Ressourcen durchsuchen.

Ausführliche Informationen zum Übergeben von sensiblen Parametern an Konfigurationen finden Sie unter [Übergeben von Anmeldeinformationen an den Azure DSC-Erweiterungshandler](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
