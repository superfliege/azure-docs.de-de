---
title: Verwalten von Key Vault in Azure Stack mithilfe von PowerShell | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Key Vault in Azure Stack mithilfe von PowerShell verwalten.
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: sngun
ms.openlocfilehash: e920ee20268f5f43592e5a27fe82dcf27cb85af1
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="manage-key-vault-in-azure-stack-by-using-powershell"></a>Verwalten von Key Vault in Azure Stack mithilfe von PowerShell

In diesem Artikel erfahren Sie, wie Sie Key Vault in Azure Stack mithilfe von PowerShell erstellen und verwalten. Die in diesem Artikel beschriebenen Key Vault-PowerShell-Cmdlets stehen im Azure PowerShell SDK zur Verfügung. In den folgenden Abschnitten werden die PowerShell-Cmdlets beschrieben, die für Folgendes erforderlich sind:
   - Erstellen eines Tresors. 
   - Speichern und Verwalten von kryptografischen Schlüsseln und Geheimnissen. 
   - Autorisieren von Benutzern bzw. Anwendungen zum Aufrufen von Vorgängen im Tresor. 

## <a name="prerequisites"></a>Voraussetzungen
* Sie müssen ein Angebot abonnieren, das den Azure Key Vault-Dienst umfasst.
* [Installieren Sie PowerShell für Azure Stack](azure-stack-powershell-install.md).  
* [Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers](azure-stack-powershell-configure-user.md).

## <a name="enable-your-tenant-subscription-for-key-vault-operations"></a>Aktivieren Ihres Mandantenabonnements für Key Vault-Vorgänge

Damit Sie Vorgänge für einen Schlüsseltresor ausführen können, muss Ihr Mandantenabonnement für Tresorvorgänge aktiviert sein. Um sicherzustellen, dass Tresorvorgänge aktiviert sind, führen Sie den folgenden Befehl aus:

```PowerShell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -Autosize
```
**Ausgabe**

Wenn Ihr Abonnement für Tresorvorgänge aktiviert ist, hat „RegistrationState“ in der Ausgabe für alle Ressourcentypen eines Schlüsseltresors den Wert „Registered“.

![Registrierungsstatus](media/azure-stack-kv-manage-powershell/image1.png)

Wenn keine Tresorvorgänge aktiviert sind, führen Sie den folgenden Befehl aus, um den Key Vault-Dienst in Ihrem Abonnement zu registrieren:

```PowerShell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault
```

**Ausgabe**

Wenn die Registrierung erfolgreich war, wird die folgende Ausgabe zurückgegeben:

![Registrieren](media/azure-stack-kv-manage-powershell/image2.png) Wenn Sie die Schlüsseltresorbefehle aufrufen, erhalten Sie möglicherweise eine Fehlermeldung wie: „Das Abonnement ist nicht für die Verwendung des Namespace „{0}“ registriert.“ Wenn Sie eine Fehlermeldung erhalten, bestätigen Sie, dass Sie [den Ressourcenanbieter Key Vault aktiviert](#enable-your-tenant-subscription-for-vault-operations) haben, wie in den bereits erwähnten Anweisungen beschrieben.

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors 

Erstellen Sie vor dem Erstellen eines Schlüsseltresors zunächst eine Ressourcengruppe, damit alle schlüsseltresorbezogenen Ressourcen in einer Ressourcengruppe enthalten sind. Verwenden Sie den folgenden Befehl, um eine neue Ressourcengruppe zu erstellen:

```PowerShell
New-AzureRmResourceGroup -Name “VaultRG” -Location local -verbose -Force

```

**Ausgabe**

![Neue Ressourcengruppe](media/azure-stack-kv-manage-powershell/image3.png)

Erstellen Sie nun mithilfe des Befehls **New-AzureRMKeyVault** einen Schlüsseltresor in der zuvor erstellten Ressourcengruppe. Dieser Befehl liest drei erforderliche Parameter: Ressourcengruppenname, Schlüsseltresorname und geografischer Standort. 

Führen Sie den folgenden Befehl aus, um einen Schlüsseltresor zu erstellen:

```PowerShell
New-AzureRmKeyVault -VaultName “Vault01” -ResourceGroupName “VaultRG” -Location local -verbose
```
**Ausgabe**

![Neuer Schlüsseltresor](media/azure-stack-kv-manage-powershell/image4.png)

Die Ausgabe dieses Befehls zeigt die Eigenschaften des von Ihnen erstellten Schlüsseltresors. Wenn eine Anwendung auf diesen Tresor zugreift, verwendet sie dazu die Eigenschaft **Vault URI** aus der Ausgabe. Beispielsweise ist der Uniform Resource Identifier (URI) des Tresors in diesem Fall „https://vault01.vault.local.azurestack.external“. Anwendungen, die mit diesem Schlüsseltresor über die REST-API interagieren, müssen diesen URI verwenden.

Wenn Sie in Active Directory Federation Services-basierten Bereitstellungen (AD FS) einen Schlüsseltresor mithilfe von PowerShell erstellen, wird unter Umständen eine Warnung mit dem Hinweis angezeigt, dass die Zugriffsrichtlinie nicht festgelegt ist, und dass keine Benutzer oder Anwendungen über Zugriffsberechtigungen für die Verwendung dieses Tresors verfügen. Legen Sie zur Behebung dieses Problems mithilfe des Befehls [Set-AzureRmKeyVaultAccessPolicy](azure-stack-kv-manage-powershell.md#authorize-an-application-to-use-a-key-or-secret) eine Zugriffsrichtlinie für den Tresor fest:

```PowerShell
# Obtain the security identifier(SID) of the active directory user
$adUser = Get-ADUser -Filter "Name -eq '{Active directory user name}'"
$objectSID = $adUser.SID.Value 

#Set the key vault access policy
Set-AzureRmKeyVaultAccessPolicy -VaultName "{key vault name}" -ResourceGroupName "{resource group name}" -ObjectId "{object SID}" -PermissionsToKeys {permissionsToKeys} -PermissionsToSecrets {permissionsToSecrets} -BypassObjectIdValidation 
```

## <a name="manage-keys-and-secrets"></a>Verwalten von Schlüsseln und Geheimnissen

Gehen Sie nach dem Erstellen eines Tresors wie folgt vor, um Schlüssel und Geheimnisse innerhalb des Tresors zu erstellen und zu verwalten.

### <a name="create-a-key"></a>Erstellen eines Schlüssels

Verwenden Sie den Befehl **Add-AzureKeyVaultKey**, um einen softwaregeschützten Schlüssel in einem Schlüsseltresor zu erstellen oder in einen Schlüsseltresor zu importieren. 

```PowerShell
Add-AzureKeyVaultKey -VaultName “Vault01” -Name “Key01” -verbose -Destination Software
```
Mit dem Parameter **Destination** wird angegeben, dass es sich um einen softwaregeschützten Schlüssel handelt. Nach erfolgreicher Erstellung des Schlüssels gibt der Befehl die Details des erstellten Schlüssels aus.

**Ausgabe**

![Neuer Schlüssel](media/azure-stack-kv-manage-powershell/image5.png)

Sie können nun mithilfe des dazugehörigen URIs auf den erstellten Schlüssel verweisen. Wenn Sie einen Schlüssel mit einem Namen erstellen oder importieren, der dem Namen eines bereits vorhandenen Schlüssels entspricht, wird der ursprüngliche Schlüssel mit den Werten des neuen Schlüssels aktualisiert. Über den versionsspezifischen URI des Schlüssels können Sie auf die vorherige Version des Schlüssels zugreifen. Beispiel: 

* Mit „https://vault10.vault.local.azurestack.external:443/keys/key01“ erhalten Sie immer die aktuelle Version. 
* Mit „https://vault010.vault.local.azurestack.external:443/keys/key01/d0b36ee2e3d14e9f967b8b6b1d38938a“ erhalten Sie diese spezifische Version.

### <a name="get-a-key"></a>Abrufen eines Schlüssels

Verwenden Sie den Befehl **Get-AzureKeyVaultKey**, um einen Schlüssel und die dazugehörigen Details zu lesen.

```PowerShell
Get-AzureKeyVaultKey -VaultName “Vault01” -Name “Key01”
```

### <a name="create-a-secret"></a>Erstellen eines Geheimnisses

Verwenden Sie den Befehl **Set-AzureKeyVaultSecret**, um ein Geheimnis in einem Tresor zu erstellen oder zu aktualisieren. Ein Geheimnis wird erstellt, wenn es nicht bereits vorhanden ist. Wenn es bereits vorhanden ist, wird eine neue Version des Geheimnisses erstellt.

```PowerShell
$secretvalue = ConvertTo-SecureString “User@123” -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName “Vault01” -Name “Secret01” -SecretValue $secretvalue
```

**Ausgabe**

![Erstellen eines Geheimnisses](media/azure-stack-kv-manage-powershell/image6.png)

### <a name="get-a-secret"></a>Abrufen eines Geheimnisses

Verwenden Sie den Befehl **Get-AzureKeyVaultSecret**, um ein Geheimnis in einem Schlüsseltresor zu lesen. Dieser Befehl kann alle oder bestimmte Versionen eines Geheimnisses zurückgeben. 

```PowerShell
Get-AzureKeyVaultSecret -VaultName “Vault01” -Name “Secret01”
```

Nach dem Erstellen von Schlüsseln und Geheimnissen können Sie deren Verwendung durch externe Anwendungen autorisieren.

## <a name="authorize-an-application-to-use-a-key-or-secret"></a>Autorisieren einer Anwendung zum Verwenden eines Schlüssels oder Geheimnisses

Verwenden Sie den Befehl **Set-AzureRmKeyVaultAccessPolicy**, um den Zugriff einer Anwendung auf einen Schlüssel oder auf ein Geheimnis im Schlüsseltresor zu autorisieren.
Im folgenden Beispiel lautet der Tresorname *ContosoKeyVault*, und die Anwendung, die Sie autorisieren möchten, hat die Client-ID *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*. Führen Sie den folgenden Befehl aus, um die Anwendung zu autorisieren. Mithilfe des Parameters **PermissionsToKeys** können Sie optional Berechtigungen für einen Benutzer, eine Anwendung oder eine Sicherheitsgruppe festlegen.

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Wenn Sie die gleiche Anwendung zum Lesen von Geheimnissen in Ihrem Tresor autorisieren möchten, führen Sie das folgende Cmdlet aus:

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300 -PermissionsToKeys Get
```

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines virtuellen Computers durch Abrufen des im Schlüsseltresor gespeicherten Kennworts](azure-stack-kv-deploy-vm-with-secret.md) 
* [Erstellen eines virtuellen Computers und Aufnehmen eines aus einem Schlüsseltresor abgerufenen Zertifikats](azure-stack-kv-push-secret-into-vm.md)

