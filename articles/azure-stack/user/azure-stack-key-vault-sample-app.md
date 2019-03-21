---
title: Zulassen des Abrufs von Azure Stack Key Vault-Geheimnissen durch Anwendungen | Microsoft-Dokumentation
description: Verwenden einer Beispiel-App zum Arbeiten mit Azure Stack Key Vault
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sethm
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: 578ca59b3f00bc1f4e876fd2b007c4b45901f86c
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57782376"
---
# <a name="a-sample-application-that-uses-keys-and-secrets-stored-in-a-key-vault"></a>Eine Beispielanwendung, die in einem Schlüsseltresor gespeicherte Schlüssel und Geheimnisse verwendet

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Nutzen Sie die Schritte in diesem Artikel, um eine Beispielanwendung namens **HelloKeyVault** auszuführen, die Schlüssel und Geheimnisse aus einem Schlüsseltresor in Azure Stack abruft.

## <a name="prerequisites"></a>Voraussetzungen

Sie können die folgenden erforderlichen Komponenten über das [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) von Azure Stack oder einen Windows-basierten externen Client installieren, sofern [eine VPN-Verbindung besteht](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Installieren Sie [mit Azure Stack kompatible Azure PowerShell-Module](azure-stack-powershell-install.md).
* Laden Sie die [Tools herunter, die für die Arbeit mit Azure Stack benötigt werden](azure-stack-powershell-download.md).

## <a name="create-and-get-the-key-vault-and-application-settings"></a>Erstellen und Abrufen der Schlüsseltresor- und Anwendungseinstellungen

Bereiten Sie die Beispielanwendung wie folgt vor:

* Erstellen Sie einen Schlüsseltresor in Azure Stack.
* Registrieren Sie eine Anwendung in Azure Active Directory (Azure AD).

Sie können das Azure-Portal oder PowerShell nutzen, um die Vorbereitungen für die Beispielanwendung durchzuführen. In diesem Artikel wird gezeigt, wie Sie einen Schlüsseltresor erstellen und eine Anwendung mit PowerShell registrieren.

>[!NOTE]
>Standardmäßig wird mit dem PowerShell-Skript eine neue Anwendung in Active Directory erstellt. Sie können aber auch eine Ihrer vorhandenen Anwendungen registrieren.

Stellen Sie vor dem Ausführen des folgenden Skripts sicher, dass Sie Werte für die Variablen `aadTenantName` und `applicationPassword` angeben. Wenn Sie keinen Wert für `applicationPassword` angeben, generiert dieses Skript ein zufälliges Kennwort.

```powershell
$vaultName           = 'myVault'
$resourceGroupName   = 'myResourceGroup'
$applicationName     = 'myApp'
$location            = 'local'

# Password for the application. If not specified, this script will generate a random password during app creation.
$applicationPassword = ''

# Function to generate a random password for the application.
Function GenerateSymmetricKey()
{
    $key = New-Object byte[](32)
    $rng = [System.Security.Cryptography.RNGCryptoServiceProvider]::Create()
    $rng.GetBytes($key)
    return [System.Convert]::ToBase64String($key)
}

Write-Host 'Please log into your Azure Stack user environment' -foregroundcolor Green

$tenantARM = "https://management.local.azurestack.external"
$aadTenantName = "FILL THIS IN WITH YOUR AAD TENANT NAME. FOR EXAMPLE: myazurestack.onmicrosoft.com"

# Configure the Azure Stack operator’s PowerShell environment.
Add-AzureRMEnvironment `
  -Name "AzureStackUser" `
  -ArmEndpoint $tenantARM

Set-AzureRmEnvironment `
  -Name "AzureStackAdmin" `
  -GraphAudience "https://graph.windows.net/"

$TenantID = Get-AzsDirectoryTenantId `
  -AADTenantName $aadTenantName `
  -EnvironmentName AzureStackUser

# Sign in to the user portal.
Add-AzureRmAccount `
  -EnvironmentName "AzureStackUser" `
  -TenantId $TenantID `

$now = [System.DateTime]::Now
$oneYearFromNow = $now.AddYears(1)

$applicationPassword = GenerateSymmetricKey

# Create a new Azure AD application.
$identifierUri = [string]::Format("http://localhost:8080/{0}",[Guid]::NewGuid().ToString("N"))
$homePage = "http://contoso.com"

Write-Host "Creating a new AAD Application"
$ADApp = New-AzureRmADApplication `
  -DisplayName $applicationName `
  -HomePage $homePage `
  -IdentifierUris $identifierUri `
  -StartDate $now `
  -EndDate $oneYearFromNow `
  -Password $applicationPassword

Write-Host "Creating a new AAD service principal"
$servicePrincipal = New-AzureRmADServicePrincipal `
  -ApplicationId $ADApp.ApplicationId

# Create a new resource group and a key vault in that resource group.
New-AzureRmResourceGroup `
  -Name $resourceGroupName `
  -Location $location

Write-Host "Creating vault $vaultName"
$vault = New-AzureRmKeyVault -VaultName $vaultName `
  -ResourceGroupName $resourceGroupName `
  -Sku standard `
  -Location $location

# Specify full privileges to the vault for the application.
Write-Host "Setting access policy"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName `
  -ObjectId $servicePrincipal.Id `
  -PermissionsToKeys all `
  -PermissionsToSecrets all

Write-Host "Paste the following settings into the app.config file for the HelloKeyVault project:"
'<add key="VaultUrl" value="' + $vault.VaultUri + '"/>'
'<add key="AuthClientId" value="' + $servicePrincipal.ApplicationId + '"/>'
'<add key="AuthClientSecret" value="' + $applicationPassword + '"/>'
Write-Host
```

Die folgende Abbildung zeigt die Ausgabe des Skripts, das zum Erstellen des Schlüsseltresors verwendet wird:

![Schlüsseltresor mit Zugriffsschlüsseln](media/azure-stack-key-vault-sample-app/settingsoutput.png)

Notieren Sie die vom Skript zurückgegeben Werte für **VaultUrl**, **AuthClientId** und **AuthClientSecret**. Diese Werte verwenden Sie zum Ausführen der Anwendung „HelloKeyVault“.

## <a name="download-and-configure-the-sample-application"></a>Herunterladen und Konfigurieren der Beispielanwendung

Laden Sie den Beispielschlüsseltresor von der Seite mit den [Azure Key Vault-Clientbeispielen](https://www.microsoft.com/download/details.aspx?id=45343) herunter. Extrahieren Sie den Inhalt der ZIP-Datei auf Ihrer Entwicklungsarbeitsstation. Der Beispielordner enthält zwei Anwendungen. In diesem Artikel wird **HelloKeyVault** verwendet.

So laden Sie das **HelloKeyVault**-Beispiel:

* Navigieren Sie zum Ordner **Microsoft.Azure.KeyVault.Samples** > **samples** > **HelloKeyVault**.
* Öffnen Sie die Anwendung **HelloKeyVault** in Visual Studio.

### <a name="configure-the-sample-application"></a>Konfigurieren der Beispielanwendung

In Visual Studio:

* Öffnen Sie die Datei „HelloKeyVault\App.config“, und suchen Sie nach dem Element &lt;**appSettings**&gt;.
* Aktualisieren Sie die Schlüssel **VaultUrl**, **AuthClientId** und **AuthClientSecret** mit den Werten, die von den Schlüsseln zurückgegeben wurden, die zum Erstellen des Schlüsselspeichers verwendet wurden. Standardmäßig enthält die Datei „App.config“ einen Platzhalter für `AuthCertThumbprint`. Ersetzen Sie diesen Platzhalter durch `AuthClientSecret`.

  ![App-Einstellungen](media/azure-stack-key-vault-sample-app/appconfig.png)

* Erstellen Sie die Projektmappe neu.

## <a name="run-the-application"></a>Ausführen der Anwendung

Beim Ausführen von HelloKeyVault meldet sich die Anwendung an Azure AD an und nutzt dann das AuthClientSecret-Token, um den Schlüsseltresor in Azure Stack zu authentifizieren.

Sie können das Beispiel HelloKeyVault für folgende Zwecke verwenden:

* Führen Sie grundlegende Vorgänge wie das Erstellen, Verschlüsseln, Umschließen und Löschen der Schlüssel und Geheimnisse durch.
* Übergeben Sie Parameter (z.B. `encrypt` und `decrypt`) an HelloKeyVault, und wenden Sie die angegebenen Änderungen auf einen Schlüsseltresor an.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen eines virtuellen Computers mit einem Key Vault-Kennwort](azure-stack-key-vault-deploy-vm-with-secret.md)
- [Bereitstellen eines virtuellen Computers mit einem Key Vault-Zertifikat](azure-stack-key-vault-push-secret-into-vm.md)
