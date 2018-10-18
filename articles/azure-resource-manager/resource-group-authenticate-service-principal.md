---
title: Erstellen einer Identität für die Azure-App mit PowerShell | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Azure PowerShell eine Azure Active Directory-Anwendung und einen Dienstprinzipal erstellen sowie mittels rollenbasierter Zugriffssteuerung Zugriff auf Ressourcen gewähren. Es wird gezeigt, wie eine Anwendung mit einem Zertifikat authentifiziert wird.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/10/2018
ms.author: tomfitz
ms.openlocfilehash: 84b32cadbd7d574e01053b61ace1203d495983b4
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498605"
---
# <a name="use-azure-powershell-to-create-a-service-principal-with-a-certificate"></a>Verwenden von Azure PowerShell zum Erstellen eines Dienstprinzipals mit einem Zertifikat

Wenn eine App oder ein Skript Zugriff auf Ressourcen benötigt, können Sie eine Identität für die App einrichten und sie mit ihren eigenen Anmeldeinformationen authentifizieren. Diese Identität wird als Dienstprinzipal bezeichnet. Dieser Ansatz ermöglicht Ihnen Folgendes:

* Sie können der App-Identität Berechtigungen zuweisen, die sich von Ihren eigenen Berechtigungen unterscheiden. In der Regel sind diese Berechtigungen genau auf die Aufgaben der App beschränkt.
* Sie können ein Zertifikat für die Authentifizierung beim Ausführen eines unbeaufsichtigten Skripts verwenden.

> [!IMPORTANT]
> Anstatt einen Dienstprinzipal zu erstellen, sollten Sie die Verwendung verwalteter Identitäten für Azure-Ressourcen für Ihre Anwendungsidentität erwägen. Wenn Ihr Code unter einem Dienst ausgeführt wird, der verwaltete Identitäten unterstützt und auf Ressourcen zugreift, die die Azure Active Directory-Authentifizierung unterstützen, sind verwaltete Identitäten für Sie die besser geeignete Option. Weitere Informationen zu verwalteten Identitäten für Azure-Ressourcen (z.B. auch zu den Diensten, die diese zurzeit unterstützen) finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).

In diesem Artikel wird veranschaulicht, wie Sie einen Dienstprinzipal erstellen, der mit einem Zertifikat authentifiziert wird. Informationen zur Einrichtung eines Dienstprinzipals mit einem Kennwort finden Sie unter [Erstellen eines Azure-Dienstprinzipals mit Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps).

Für diesen Artikel benötigen Sie die [aktuelle Version](/powershell/azure/get-started-azureps) von PowerShell.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Zum Abschließen dieses Artikels benötigen Sie sowohl in Ihrer Azure Active Directory-Instanz als auch im Azure-Abonnement ausreichende Berechtigungen. Insbesondere müssen Sie eine App in der Azure Active Directory-Instanz erstellen und den Dienstprinzipal einer Rolle zuweisen können.

Die einfachste Möglichkeit zum Überprüfen, ob Ihr Konto über die erforderlichen Berechtigungen verfügt, ist über das Portal. Siehe [Überprüfen der erforderlichen Berechtigung](resource-group-create-service-principal-portal.md#required-permissions).

## <a name="create-service-principal-with-self-signed-certificate"></a>Erstellen eines Dienstprinzipals mit selbstsigniertem Zertifikat

Im folgenden Beispiel geht es um ein einfaches Szenario. [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) wird zum Erstellen eines Dienstprinzipals mit einem selbstsignierten Zertifikat verwendet, und [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) wird verwendet, um dem Dienstprinzipal die Rolle [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor) zuzuweisen. Die Rollenzuweisung ist auf Ihr derzeit ausgewähltes Azure-Abonnement beschränkt. Verwenden Sie [Set-AzureRmContext](/powershell/module/azurerm.profile/set-azurermcontext), um ein anderes Abonnement auszuwählen.

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" `
  -Subject "CN=exampleappScriptCert" `
  -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Das Beispiel befindet sich 20 Sekunden lang im Ruhezustand und schafft damit Zeit für die Verteilung des neuen Dienstprinzipals in Azure Active Directory. Falls das Skript nicht lange genug wartet, sehen Sie folgende Fehlermeldung: „Principal {ID} does not exist in the directory {DIR-ID}.“ (Prinzipal {ID} ist im Verzeichnis {DIR-ID} nicht vorhanden.). Sie können diesen Fehler beheben, indem Sie einen Moment warten und dann den Befehl **New-AzureRmRoleAssignment** erneut ausführen.

Sie können den Geltungsbereich für diese Rollenzuweisung mit dem Parameter **ResourceGroupName** auf eine bestimmte Ressourcengruppe festlegen. Sie können den Geltungsbereich durch die Verwendung der Parameter **ResourceType** und **ResourceName** auch auf eine bestimmte Ressource festlegen. 

Wenn Sie **nicht mit Windows 10 oder Windows Server 2016 arbeiten**, müssen Sie den [Generator für selbstsignierte Zertifikate](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) aus dem Skriptcenter von Microsoft herunterladen. Extrahieren Sie den Inhalt, und importieren Sie das benötigte Cmdlet.

```powershell
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```

Ersetzen Sie im Skript die folgenden zwei Zeilen, um das Zertifikat zu generieren.

```powershell
New-SelfSignedCertificateEx -StoreLocation CurrentUser `
  -Subject "CN=exampleapp" `
  -KeySpec "Exchange" `
  -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Bereitstellen eines Zertifikats über automatisiertes PowerShell-Skript

Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Azure Active Directory.

```powershell
$TenantId = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
$ApplicationId = (Get-AzureRmADApplication -DisplayNameStartWith exampleapp).ApplicationId

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match "CN=exampleappScriptCert" }).Thumbprint
 Connect-AzureRmAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Erstellen eines Dienstprinzipals mit Zertifikat von der Zertifizierungsstelle

Im folgenden Beispiel wird ein von einer Zertifizierungsstelle ausgestelltes Zertifikat verwendet, um einen Dienstprinzipal zu erstellen. Die Zuweisung ist auf das angegebene Azure-Abonnement beschränkt. Der Dienstprinzipal wird der Rolle [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor) hinzugefügt. Wenn während der Rollenzuweisung ein Fehler auftritt, wird versucht, die Zuweisung erneut durchzuführen.

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Connect-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -Subscription $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzureRmADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Bereitstellen eines Zertifikats über automatisiertes PowerShell-Skript
Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Azure Active Directory.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object `
  -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 `
  -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Connect-AzureRmAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

Die Anwendungs-ID und die Mandanten-ID sind keine sensiblen Daten, sodass Sie sie direkt in Ihr Skript einbetten können. Wenn Sie die Mandanten-ID abrufen möchten, verwenden Sie Folgendes:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Wenn Sie die Anwendungs-ID abrufen möchten, verwenden Sie Folgendes:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Ändern von Anmeldeinformationen

Wenn Sie die Anmeldeinformationen für eine AD-App entweder aufgrund einer Gefährdung der Sicherheit oder aufgrund des Ablaufs der Anmeldeinformationen ändern müssen, verwenden Sie die Cmdlets [Remove-AzureRmADAppCredential](/powershell/module/azurerm.resources/remove-azurermadappcredential) und [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential).

Zum Entfernen aller Anmeldeinformationen für eine Anwendung verwenden Sie Folgendes:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | Remove-AzureRmADAppCredential
```

Zum Hinzufügen eines Zertifikatwerts erstellen Sie ein selbstsigniertes Zertifikat, wie in diesem Artikel gezeigt. Verwenden Sie anschließend Folgendes:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | New-AzureRmADAppCredential `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
```

## <a name="debug"></a>Debuggen

Wenn Sie einen Dienstprinzipal erstellen, können folgende Fehler auftreten:

* **„Authentication_Unauthorized“** oder **„Kein Abonnement in diesem Kontext gefunden.“** Dieser Fehler wird angezeigt, wenn Ihr Konto nicht über die [erforderlichen Berechtigungen](#required-permissions) verfügt, um eine App in Azure Active Directory zu registrieren. In der Regel wird dieser Fehler angezeigt, wenn in Ihrem Azure Active Directory nur Administratorbenutzer Apps registrieren können und Ihr Konto kein Administratorkonto ist. Bitten Sie Ihren Administrator, Sie entweder einer Administratorrolle zuzuweisen, oder Benutzern zu ermöglichen, Apps zu registrieren.

* Ihr Konto **„hat keine Berechtigung zum Ausführen der Aktion 'Microsoft.Authorization/roleAssignments/write' über Bereich '/subscriptions/{guid}'.“**  – Dieser Fehler wird angezeigt, wenn Ihr Konto nicht über ausreichende Berechtigungen verfügt, um eine Rolle einer Identität zuzuweisen. Bitten Sie Ihren Abonnementadministrator, Sie der Rolle „Benutzerzugriffsadministrator“ hinzuzufügen.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zur Einrichtung eines Dienstprinzipals mit einem Kennwort finden Sie unter [Erstellen eines Azure-Dienstprinzipals mit Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps).
* Ausführliche Schritte zum Integrieren einer Anwendung in Azure zur Verwaltung von Ressourcen finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).
* Eine ausführlichere Erläuterung zu Anwendungen und Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](../active-directory/develop/app-objects-and-service-principals.md). 
* Weitere Informationen zur Azure Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien für Azure AD](../active-directory/develop/authentication-scenarios.md).
