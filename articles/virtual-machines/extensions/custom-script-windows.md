---
title: Azure – benutzerdefinierte Skripterweiterung für Windows | Microsoft-Dokumentation
description: Automatisieren von Konfigurationsaufgaben für virtuelle Windows-Computer mithilfe der benutzerdefinierten Skripterweiterung
services: virtual-machines-windows
documentationcenter: ''
author: zroiy
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/24/2018
ms.author: roiyz
ms.openlocfilehash: 5c105c6adba1a5fca52a85f6d54751e1b54fd721
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412830"
---
# <a name="custom-script-extension-for-windows"></a>CustomScript-Erweiterung für Windows

Die benutzerdefinierte Skripterweiterung lädt Skripts auf virtuelle Azure-Computer herunter und führt sie aus. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Skripts können aus Azure Storage oder GitHub heruntergeladen oder dem Azure-Portal zur Laufzeit für die Erweiterung bereitgestellt werden. Die benutzerdefinierte Skripterweiterung kann in Azure Resource Manager-Vorlagen integriert und auch mithilfe der Azure-Befehlszeilenschnittstelle, mithilfe von PowerShell, über das Azure-Portal oder unter Verwendung der REST-API für virtuelle Azure-Computer ausgeführt werden.

In diesem Dokument erfahren Sie, wie Sie die benutzerdefinierte Skripterweiterung über das Azure PowerShell-Modul ausführen. Sie erfahren, wie Sie eine Azure Resource Manager-Vorlage verwenden, und erhalten Informationen zu Problembehandlungsschritten für Windows-Systeme.

## <a name="prerequisites"></a>Voraussetzungen

> [!NOTE]  
> Verwenden Sie die benutzerdefinierte Skripterweiterung nicht, um „Update-AzureRmVM“ mit der gleichen VM als Parameter auszuführen, da der Befehl dann auf sich selbst wartet.  
>   
> 

### <a name="operating-system"></a>Betriebssystem

Die Erweiterung für benutzerdefinierte Skripts für Linux wird auf den von Erweiterungen unterstützten Betriebssystemen ausgeführt. Weitere Informationen finden Sie in diesem [Artikel](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems).

### <a name="script-location"></a>Speicherort des Skripts

Mit der Erweiterung können Sie unter Verwendung Ihrer Azure Blob Storage-Anmeldeinformationen auf Azure Blob Storage zugreifen. Alternativ kann das Skript an einem beliebigen Ort gespeichert sein, solange der virtuelle Computer ein Routing an diesen Endpunkt (GitHub, interner Dateiserver oder Ähnliches) durchführen kann.


### <a name="internet-connectivity"></a>Internetverbindung
Wenn Sie ein Skript extern herunterladen möchten (etwa im Fall von GitHub oder Azure Storage), müssen zusätzliche Firewall-/Netzwerksicherheitsgruppen-Ports geöffnet werden. Wenn sich Ihr Skript also beispielsweise in Azure Storage befindet, können Sie Zugriff über Azure-NSG-Diensttags für [Storage](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) gewähren.

Befindet sich Ihr Skript auf einem lokalen Server, müssen gegebenenfalls noch weitere Firewall-/Netzwerksicherheitsgruppen-Ports geöffnet werden.

### <a name="tips-and-tricks"></a>Tipps und Tricks
* Die meisten Fehler für diese Erweiterung sind auf Syntaxfehler im Skript zurückzuführen. Vergewissern Sie sich, dass das Skript fehlerfrei ausgeführt wird, und versehen Sie es zusätzlich mit einer Protokollierung, um die Fehlersuche zu erleichtern.
* Schreiben Sie idempotente Skripts, um Systemänderungen zu vermeiden, wenn das Skript versehentlich mehrmals ausgeführt wird.
* Stellen Sie sicher, dass während der Skriptausführung keine Benutzereingaben erforderlich sind.
* Die Skriptausführung darf maximal 90 Minuten dauern. Danach gilt die Bereitstellung der Erweiterung als nicht erfolgreich.
* Das Skript darf keine Systemneustarts enthalten, da diese zu Problemen mit anderen Erweiterungen führen, die installiert werden. Außerdem wird die Erweiterung nach dem Neustart nicht fortgesetzt. 
* Wenn Sie über ein Skript verfügen, das einen Neustart des Systems bewirkt, installieren Sie Anwendungen, und führen Sie Skripts usw. aus. Planen Sie den Neustart des Systems mithilfe eines geplanten Windows-Tasks oder mit einem Tool wie DSC oder Chef oder mit Puppet-Erweiterungen.
* Die Erweiterung führt jedes Skript nur einmal aus. Soll ein Skript bei jedem Neustart des Systems ausgeführt werden, müssen Sie die Erweiterung zum Erstellen eines geplanten Windows-Tasks verwenden.
* Wenn Sie den Ausführungszeitpunkt eines Skripts planen möchten, erstellen Sie mithilfe der Erweiterung einen geplanten Windows-Task. 
* Während der Skriptausführung wird im Azure-Portal sowie in der CLI nur ein Übergangsstatus für die Erweiterung angezeigt. Sollten Sie häufigere Statusaktualisierungen für ein ausgeführtes Skript benötigen, müssen Sie eine eigene Lösung erstellen.
* Die Erweiterung für benutzerdefinierte Skripts verfügt über keine native Proxyserverunterstützung. Sie können innerhalb Ihres Skripts jedoch ein Dateiübertragungstool mit Proxyserverunterstützung verwenden (beispielsweise *cURL*). 
* Achten Sie auf nicht standardmäßige Verzeichnispfade, von denen Ihre Skripts oder Befehle gegebenenfalls abhängen, und verwenden Sie eine entsprechende Logik, um diese zu behandeln.


## <a name="extension-schema"></a>Erweiterungsschema

In der Konfiguration der benutzerdefinierten Skripterweiterung werden Aspekte wie der Skriptspeicherort und der auszuführende Befehl angegeben. Sie können diese Konfiguration in Konfigurationsdateien speichern oder sie in der Befehlszeile oder in einer Azure Resource Manager-Vorlage angeben. 

Sie können sensible Daten in einer geschützten Konfiguration speichern. Diese ist verschlüsselt und wird nur auf dem virtuellen Computer entschlüsselt. Die geschützte Konfiguration ist hilfreich, wenn der Ausführungsbefehl vertrauliche Informationen (beispielsweise ein Kennwort) enthält.

Diese Elemente müssen als vertrauliche Daten behandelt und in der Konfiguration mit den geschützten Einstellungen der Erweiterung angegeben werden. Die geschützten Einstellungsdaten der Azure-VM-Erweiterung werden verschlüsselt und nur auf dem virtuellen Zielcomputer entschlüsselt.

```json
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

  **Hinweis**: Auf einem virtuellen Computer kann jeweils nur eine Version einer Erweiterung installiert werden. Wenn Sie die Erweiterung für benutzerdefinierte Skripts zweimal in derselben Resource Manager-Vorlage für denselben Computer angeben, treten Fehler auf. 

### <a name="property-values"></a>Eigenschaftswerte

| NAME | Wert/Beispiel | Datentyp |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| Herausgeber | Microsoft.Compute | Zeichenfolge |
| type | CustomScriptExtension | Zeichenfolge |
| typeHandlerVersion | 1.9 | int |
| fileUris (Beispiel) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 | Array |
| commandToExecute (Beispiel) | powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 | Zeichenfolge |
| storageAccountName (Beispiel) | examplestorageacct | Zeichenfolge |
| storageAccountKey (Beispiel) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | Zeichenfolge |

>[!NOTE]
>Bei Eigenschaftennamen wird zwischen Groß- und Kleinschreibung unterschieden. Um Bereitstellungsprobleme zu vermeiden, verwenden Sie die Namen wie hier gezeigt.

#### <a name="property-value-details"></a>Details zu Eigenschaftswerten
 * `commandToExecute` (**erforderlich**, Zeichenfolge): Das auszuführende Skript für den Einstiegspunkt. Verwenden Sie dieses Feld, wenn der Befehl Geheimnisse (z.B. Kennwörter) enthält oder die „fileUris“ vertraulich sind.
* `fileUris` (optional, Zeichenfolgenarray): die URLs für die herunterzuladenden Dateien.
* `storageAccountName` (optional, Zeichenfolge): der Name des Speicherkontos. Wenn Sie Speicheranmeldeinformationen angeben, muss es sich bei allen `fileUris` um URLs für Azure-Blobs handeln.
* `storageAccountKey` (optional, Zeichenfolge): der Zugriffsschlüssel des Speicherkontos.

Die folgenden Werte können in öffentlichen oder geschützten Einstellungen festgelegt werden. Die Erweiterung lehnt jedoch jede Konfiguration ab, bei der die Werte sowohl in öffentlichen als auch in geschützten Einstellungen festgelegt sind.
* `commandToExecute`

Die Verwendung öffentlicher Einstellungen kann zwar beim Debuggen hilfreich sein, es wird jedoch dringend empfohlen, geschützte Einstellungen zu verwenden.

Öffentliche Einstellungen werden in Klartext an den virtuellen Computer gesendet, auf dem das Skript ausgeführt wird.  Geschützte Einstellungen werden mit einem Schlüssel verschlüsselt, der nur in Azure und auf dem virtuellen Computer bekannt ist. Die Einstellungen werden unverändert auf dem virtuellen Computer gespeichert. Waren die Einstellungen also verschlüsselt, werden sie auch verschlüsselt auf dem virtuellen Computer gespeichert. Das Zertifikat zum Entschlüsseln der verschlüsselten Werte wird auf dem virtuellen Computer gespeichert und gegebenenfalls zur Laufzeit zum Entschlüsseln der Einstellungen verwendet.

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Azure-VM-Erweiterungen können mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden. Das im vorherigen Abschnitt erläuterte JSON-Schema kann in einer Azure Resource Manager-Vorlage zum Ausführen der benutzerdefinierten Skripterweiterung im Rahmen einer Azure Resource Manager-Bereitstellung verwendet werden. Eine Beispielvorlage, die die benutzerdefinierte Skripterweiterung enthält, finden Sie hier auf [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell-Bereitstellung

Mit dem `Set-AzureRmVMCustomScriptExtension`-Befehl können Sie die benutzerdefinierte Skripterweiterung einem vorhandenen virtuellen Computer bereitstellen. Weitere Informationen finden Sie unter [Set-AzureRmVMCustomScriptExtension](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```
## <a name="further-examples"></a>Weitere Beispiele

### <a name="using-multiple-script"></a>Verwenden mehrerer Skripts
Das folgende Beispiel umfasst drei Skripts, mit denen der Server erstellt wird. commandToExecute ruft das erste Skript auf. Dann stehen verschiedene Optionen zum Aufrufen der anderen Skripts zur Verfügung. Beispielsweise können Sie über ein Masterskript die Ausführung mit der richtigen Fehlerbehandlung, Protokollierung und Statusverwaltung steuern.

```powershell

$fileUri = @("https://xxxxxxx.blob.core.windows.net/buildServer1/1_Add_Tools.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/2_Add_Features.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/3_CompleteInstall.ps1")

$Settings = @{"fileUris" = $fileUri};

$storageaccname = "xxxxxxx"
$storagekey = "1234ABCD"
$ProtectedSettings = @{"storageAccountName" = $storageaccname; "storageAccountKey" = $storagekey; "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File 1_Add_Tools.ps1"};

#run command
Set-AzureRmVMExtension -ResourceGroupName myRG `
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "buildserver1" ` 
    -Publisher "Microsoft.Compute" ` 
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" ` 
    -Settings $Settings ` 
    -ProtectedSettings $ProtectedSettings `
```

### <a name="running-scripts-from-a-local-share"></a>Ausführen von Skripts über eine lokale Freigabe
In diesem Beispiel können Sie einen lokalen SMB-Server als Skriptspeicherort verwenden. Beachten Sie, dass Sie mit Ausnahme von *commandToExecute* keine weiteren Einstellungen übergeben müssen.

```powershell
$ProtectedSettings = @{"commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File \\filesvr\build\serverUpdate1.ps1"};
 
Set-AzureRmVMExtension -ResourceGroupName myRG 
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "serverUpdate" 
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" `
    -ProtectedSettings $ProtectedSettings

```

### <a name="how-to-run-custom-script-more-than-once-with-cli"></a>Mehrmaliges Ausführen der Erweiterung für benutzerdefinierte Skripts über die Befehlszeilenschnittstelle
Wenn Sie die Erweiterung für benutzerdefinierte Skripts mehrmals ausführen möchten, ist dies nur unter folgenden Bedingungen möglich:
1. Der Parameter „Name“ der Erweiterung ist identisch mit der vorherigen Bereitstellung der Erweiterung.
2. Sie müssen die Konfiguration aktualisieren, andernfalls wird der Befehl nicht erneut ausgeführt. Sie können beispielsweise eine dynamische Eigenschaft im Befehl (z.B. einen Zeitstempel) hinzufügen. 

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal und mithilfe des Azure PowerShell-Moduls abgerufen werden. Führen Sie den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für einen bestimmten virtuellen Computer anzuzeigen:

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Die Ausgabe der Erweiterungsausführung wird in Dateien im folgenden Verzeichnis auf dem virtuellen Zielcomputer protokolliert.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Die angegebenen Dateien werden in das folgende Verzeichnis auf dem virtuellen Zielcomputer heruntergeladen.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
Hierbei ist `<n>` eine dezimale Ganzzahl, die sich zwischen verschiedenen Ausführungen der Erweiterung ändern kann.  Der Wert `1.*` entspricht dem tatsächlichen aktuellen `typeHandlerVersion`-Wert der Erweiterung.  Das tatsächliche Verzeichnis könnte z.B. folgendermaßen lauten: `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Beim Ausführen des Befehls `commandToExecute` legt die Erweiterung dieses Verzeichnis (z.B. `...\Downloads\2`) als aktuelles Arbeitsverzeichnis fest. Dies ermöglicht die Verwendung relativer Pfade, um die heruntergeladenen Dateien mithilfe der `fileURIs`-Eigenschaft aufzufinden. Beispiele hierfür finden Sie in der unten stehenden Tabelle.

Da sich der absolute Downloadpfad im Lauf der Zeit ändern kann, empfiehlt es sich, in der Zeichenfolge `commandToExecute` nach Möglichkeit relative Pfade zu Skripts und Dateien zu verwenden. Beispiel: 
```json
    "commandToExecute": "powershell.exe . . . -File \"./scripts/myscript.ps1\""
```

Pfadinformationen nach dem ersten URI-Segment werden für Dateien beibehalten, die über die `fileUris`-Eigenschaftenliste heruntergeladen werden.  Wie in der Tabelle unten gezeigt, werden heruntergeladene Dateien zu Unterverzeichnissen für den Download zugeordnet, um die Struktur der `fileUris`-Werte widerzuspiegeln.  

#### <a name="examples-of-downloaded-files"></a>Beispiele für heruntergeladene Dateien

| URI in „fileUris“ | Relativer Speicherort für Downloads | Absoluter Speicherort für Downloads* |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\* Wie oben bereits erwähnt, ändern sich absolute Verzeichnispfade während der Lebensdauer eines virtuellen Computers, nicht jedoch innerhalb einer einzelnen Ausführung der CustomScript-Erweiterung.

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).
