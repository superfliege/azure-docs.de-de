---
title: Konfigurieren des gewünschten Zustands für Azure – Übersicht | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Verwendung des Microsoft Azure-Erweiterungshandlers für PowerShell Desired State Configuration (DSC). Der Artikel enthält Informationen zu Voraussetzungen, Architektur und Cmdlets.
services: virtual-machines-windows
documentationcenter: ''
author: mgreenegit
manager: jeconnoc
editor: ''
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
ms.openlocfilehash: b6bfe48df685952d2b465d9549e2f1c086c1c490
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32192926"
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Einführung in den Handler der Azure-Erweiterung zum Konfigurieren des gewünschten Zustands

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Der Azure VM-Agent und die dazugehörigen Erweiterungen sind Teil der Microsoft Azure-Infrastrukturdienste. VM-Erweiterungen sind Softwarekomponenten, die die VM-Funktionalität erweitern und verschiedene Verwaltungsvorgänge für virtuelle Computer vereinfachen.

Der primäre Anwendungsfall für die Azure DSC-Erweiterung (Desired State Configuration) besteht im Bootstrap eines virtuellen Computers zum [Azure Automation DSC-Dienst](../../automation/automation-dsc-overview.md). Ein Bootstrap eines virtuellen Computers bietet [Vorteile](https://docs.microsoft.com/powershell/dsc/metaconfig#pull-service) wie die fortlaufende Verwaltung der VM-Konfiguration und die Integration mit anderen Betriebstools wie Azure Monitoring.

Sie können die DSC-Erweiterung unabhängig vom Automation DSC-Dienst verwenden. Dies ist jedoch eine einmalige Aktion während der Bereitstellung. Eine fortlaufende Berichterstellung oder Verwaltung der Konfiguration erfolgt ausschließlich lokal im virtuellen Computer.

Dieser Artikel enthält Informationen zu beiden Szenarien: die Verwendung der DSC-Erweiterung für Automation Onboarding und die Verwendung der DSC-Erweiterung als Tool zum Zuweisen von Konfigurationen zu virtuellen Computern mit dem Azure SDK.

## <a name="prerequisites"></a>Voraussetzungen

- **Lokaler Computer:** Für die Interaktion mit der Erweiterung für virtuelle Azure-Computer müssen Sie das Azure-Portal oder das Azure PowerShell SDK verwenden.
- **Gast-Agent:** Der mit der DSC-Konfiguration konfigurierte Azure-Computer muss über ein Betriebssystem verfügen, das Version 4.0 oder höher von Windows Management Framework (WMF) unterstützt. Die vollständige Liste der unterstützten Betriebssystemversionen finden Sie im [Versionsverlauf der DSC-Erweiterung](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Begriffe und Konzepte

Für dieses Handbuch müssen Sie mit folgenden Konzepten vertraut sein:

- **Konfiguration:** Ein DSC-Konfigurationsdokument.
- **Knoten:** Ein Ziel für die DSC-Konfiguration. In diesem Dokument bezieht sich *Knoten* immer auf einen virtuellen Azure-Computer.
- **Konfigurationsdaten:** Eine PSD1-Datei mit Umgebungsdaten für eine Konfiguration.

## <a name="architecture"></a>Architecture

Die Azure DSC-Erweiterung nutzt das VM-Agent-Framework von Azure zur Übermittlung und Inkraftsetzung von DSC-Konfigurationen auf virtuellen Azure-Computern sowie zur Erstellung entsprechender Berichte. Die DSC-Erweiterung nimmt ein Konfigurationsdokument und einen Satz von Parametern entgegen. Wird keine Datei angegeben, wird ein [Standardkonfigurationsskript](#default-configuration-script) in die Erweiterung eingebettet. Dieses wird ausschließlich zum Festlegen von Metadaten im [lokalen Konfigurations-Manager](https://docs.microsoft.com/powershell/dsc/metaconfig) verwendet.

Wenn die Erweiterung das erste Mal aufgerufen wird, installiert sie mit der folgenden Logik eine Version von Windows Management Framework (WMF):

* Bei einem virtuellen Azure-Computer unter Windows Server 2016 wird keine Aktion ausgeführt. Unter Windows Server 2016 ist bereits die neueste Version von PowerShell installiert.
* Bei Angabe der Eigenschaft **wmfVersion** wird die entsprechende WMF-Version installiert (es sei denn, die Version ist nicht mit dem Betriebssystem des virtuellen Computers kompatibel).
* Ohne Angabe der Eigenschaft **wmfVersion** wird die neueste geeignete WMF-Version installiert.

Für die Installation von WMF ist ein Neustart erforderlich. Nach dem Neustart lädt die Erweiterung die in der **modulesUrl**-Eigenschaft angegebene ZIP-Datei herunter (falls vorhanden). Befindet sich der Speicherort in Azure Blob Storage, können Sie in der **sasToken**-Eigenschaft ein SAS-Token für den Dateizugriff angeben. Nachdem Herunterladen und Entpacken der ZIP-Datei wird durch Ausführen der in **configurationFunction** definierten Konfigurationsfunktion eine MOF-Datei generiert. Anschließend führt die Erweiterung **Start-DscConfiguration -Force** mit der generierten MOF-Datei aus. Die Erweiterung erfasst die Ausgabe und schreibt sie in den Azure-Statuskanal.

### <a name="default-configuration-script"></a>Standardkonfigurationsskript

Die Azure-DSC-Erweiterung enthält ein Standardkonfigurationsskript, das für das Onboarding eines virtuellen Computers in den Azure Automation DSC-Dienst vorgesehen ist. Die Skriptparameter sind an den konfigurierbaren Eigenschaften des [lokalen Konfigurations-Managers](https://docs.microsoft.com/powershell/dsc/metaconfig) ausgerichtet. Informationen zu Skriptparametern erhalten Sie in Abschnitt [Standardkonfigurationsskript](extensions-dsc-template.md#default-configuration-script) des Artikels [Erweiterung zum Konfigurieren des gewünschten Zustands mit Azure Resource Manager-Vorlagen](extensions-dsc-template.md). Das vollständige Skript finden Sie in der [Azure Schnellstartvorlage in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true).

## <a name="dsc-extension-in-resource-manager-templates"></a>DSC-Erweiterung in Resource Manager-Vorlagen

In den meisten Szenarien sind Resource Manger-Bereitstellungsvorlagen die erwartete Methode für die Arbeit mit der DSC-Erweiterung. Weitere Informationen und Beispiele zum Einschließen der DSC-Erweiterung in Resource Manager-Bereitstellungsvorlagen finden Sie unter [Erweiterung zum Konfigurieren des gewünschten Zustands mit Azure Resource Manager-Vorlagen](extensions-dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>PowerShell-Cmdlets für die DSC-Erweiterung

Die PowerShell-Cmdlets zum Verwalten der DSC-Erweiterung werden am besten für die interaktive Problembehandlung und zum Sammeln von Informationen verwendet. Sie können die Cmdlets zum Verpacken, Veröffentlichen und Überwachen der Bereitstellung der DSC-Erweiterung verwenden. Beachten Sie, dass Cmdlets für die DSC-Erweiterung noch nicht für die Verwendung mit dem [Standardkonfigurationsskript](#default-configuration-script) aktualisiert wurden.

Das Cmdlet **Publish-AzureRMVMDscConfiguration** durchsucht eine Konfigurationsdatei nach abhängigen DSC-Ressourcen und erstellt eine ZIP-Datei. Die ZIP-Datei enthält die Konfiguration und die erforderlichen Ressourcen für deren Inkraftsetzung. Mit dem Parameter *-OutputArchivePath* kann das Cmdlet das Paket auch lokal erstellen. Andernfalls veröffentlicht das Cmdlet die ZIP-Datei in Azure Blob Storage und schützt sie durch ein SAS-Token.

Das von diesem Cmdlet erstellte PS1-Konfigurationsskript befindet sich in der ZIP-Datei im Stammverzeichnis des Archivordners. Der Modulordner wird im Archivordner in den Ressourcen platziert.

Das Cmdlet **Set AzureRMVMDscExtension** fügt die von der PowerShell DSC-Erweiterung erforderten Einstellungen in einem VM-Konfigurationsobjekt ein.

Das Cmdlet **Get AzureRMVMDscExtension** ruft den DSC-Erweiterungsstatus eines bestimmten virtuellen Computers ab.

Das Cmdlet **Get AzureRMVMDscExtensionStatus** ruft den Status der DSC-Konfiguration ab, die vom DSC-Erweiterungshandlers durchgeführt wird. Diese Aktion kann auf einem einzelnen virtuellen Computer oder einer Gruppe virtueller Computer ausgeführt werden.

Das Cmdlet **Remove-AzureRMVMDscExtension** entfernt den Erweiterungshandler aus einer bestimmten virtuellen Maschine. Dieses Cmdlet bewirkt *nicht* die Entfernung der Konfiguration, die Deinstallation von WMF oder eine Änderung der angewendeten Einstellungen auf dem virtuellen Computer. Es wird lediglich der Erweiterungshandler entfernt. 

Wichtige Informationen zu Cmdlets für die Resource Manager-DSC-Erweiterung:

- Azure Resource Manager-Cmdlets sind synchron.
- Die Parameter *ResourceGroupName*, *VMName*, *ArchiveStorageAccountName*, *Version* und *Location* sind erforderlich.
- *ArchiveResourceGroupName* ist ein optionaler Parameter. Diesen Parameter können Sie angeben, wenn Ihr Speicherkonto nicht der Ressourcengruppe angehört, in der der virtuelle Computer erstellt wird.
- Der Schalter **AutoUpdate** ermöglicht die automatische Aktualisierung des Erweiterungshandlers auf die neueste Version, sobald diese verfügbar ist. Hierdurch wird der virtuelle Computer unter Umständen neu gestartet, wenn eine neue WMF-Version veröffentlicht wird.

### <a name="get-started-with-cmdlets"></a>Erste Schritte mit Cmdlets

Die Azure-DSC-Erweiterung kann DSC-Konfigurationsdokumente zum Konfigurieren von virtuellen Azure-Computern während der Bereitstellung verwenden. Bei diesem Schritt wird der Knoten nicht bei Azure Automation registriert. Der Knoten wird *nicht* zentral verwaltet.

Nachfolgend sehen Sie ein einfaches Beispiel für eine Konfiguration. Speichern Sie die Konfiguration lokal als „IisInstall.ps1“.

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

Mit den folgenden Befehlen platzieren Sie das Skript „IisInstall.ps1“ auf dem angegebenen virtuellen Computer. Mit den Befehlen führen Sie auch die Konfiguration aus und melden anschließend den Status.

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script to user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.72 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"
```

## <a name="azure-portal-functionality"></a>Funktionen des Azure-Portals

So richten Sie DSC im Portal ein:

1. Wechseln Sie zu einem virtuellen Computer. 
2. Klicken Sie unter **Einstellungen** > **Allgemein** auf **Erweiterungen**. 
3. Wählen Sie in dem neu erstellten Bereich **Hinzufügen** und dann **PowerShell DSC**.

Für dieses Portal sind folgende Eingaben erforderlich:

* **Konfigurationsmodule oder -skript**: Dieses Feld ist obligatorisch. (Das Formular wurde nicht für das [Standardkonfigurationsskript](#default-configuration-script) aktualisiert.) Konfigurationsmodule und Skripts benötigen eine PS1-Datei, die ein Konfigurationsskript oder eine ZIP-Datei mit einer ps1-Konfigurationsskript auf der Stammebene aufweist. Wenn Sie eine ZIP-Datei verwenden, müssen alle abhängigen Ressourcen in Modulordnern in der ZIP-Ordner enthalten sein. Sie können die ZIP-Datei mit dem im Azure PowerShell SDK enthaltenen Cmdlet **Publish-AzureVMDscConfiguration -OutputArchivePath** erstellen. Die ZIP-Datei wird in Ihren Benutzer-Blobspeicher hochgeladen und durch ein SAS-Token geschützt.

* **PSD1-Konfigurationsdatendatei**: Dieses Feld ist optional. Wenn Ihre Konfiguration eine Konfigurationsdatendatei in PSD1 erfordert, wählen Sie mithilfe dieses Felds eine solche Datei aus, und laden Sie sie in Ihren Benutzer-Blobspeicher hoch. Die Konfigurationsdatei wird durch ein SAS-Token in einem Blobspeicher geschützt.

* **Modulspezifischer Konfigurationsname**: Sie können mehrere Konfigurationsfunktionen in eine PS1-Datei einfügen. Geben Sie den Namen des PS1-Konfigurationsskripts ein – gefolgt von \\ und dem Namen der Konfigurationsfunktion. Wenn der Name des PS1-Skripts z.B. „configuration.ps1“ und die Konfiguration **IisInstall** lautet, geben Sie **configuration.ps1\IisInstall** ein.

* **Konfigurationsargumente**: Falls die Konfigurationsfunktion Argumente akzeptiert, geben Sie diese hier im Format **argumentName1=value1,argumentName2=value2** ein. Hierbei handelt es sich um ein anderes Format als bei den Konfigurationsargumenten, die von PowerShell-Cmdlets oder Resource Manager-Vorlagen akzeptiert werden.

## <a name="logs"></a>Protokolle
Speichern Sie Protokolle in diesem Verzeichnis:

```powerShell
C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\<version number>
```

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu PowerShell DSC finden Sie im [PowerShell-Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview). 
* Sehen Sie sich die [Resource Manager-Vorlage für die DSC-Erweiterung](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) an. 
* Weitere Funktionen, die Sie mit PowerShell DSC verwalten können, sowie weitere DSC-Ressourcen finden Sie im [PowerShell-Katalog](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).
* Ausführliche Informationen zum Übergeben von sensiblen Parametern an Konfigurationen finden Sie unter [Übergeben von Anmeldeinformationen an den Azure DSC-Erweiterungshandler](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

