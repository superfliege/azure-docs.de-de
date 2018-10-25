---
title: Verwalten von ausführenden Azure Automation-Konten
description: In diesem Artikel wird beschrieben, wie Sie mit PowerShell oder über das Portal Ihre ausführenden Konten verwalten.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 09/12/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d2023d30cdb86a218d27024c8ccf0f397a7a5d09
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816607"
---
# <a name="manage-azure-automation-run-as-accounts"></a>Verwalten von ausführenden Azure Automation-Konten

In Azure Automation werden ausführende Konten verwendet, um die Authentifizierung für die Verwaltung von Ressourcen in Azure mit Azure-Cmdlets zu ermöglichen.

Beim Erstellen eines ausführenden Kontos wird ein neuer Dienstprinzipalbenutzer in Azure Active Directory erstellt, und diesem Benutzer wird dann auf Abonnementebene die Rolle „Mitwirkender“ zugewiesen. Für Runbooks, die Hybrid Runbook Worker auf virtuellen Azure-Computern nutzen, können Sie anstelle von ausführenden Konten [verwaltete Identitäten für Azure-Ressourcen](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) für die Authentifizierung bei Ihren Azure-Ressourcen verwenden.

Es gibt zwei Typen von ausführenden Konten:

* **Ausführendes Azure-Konto**: Dieses Konto wird zum Verwalten von Resource Manager-Bereitstellungsmodellressourcen verwendet.
  * Erstellt eine Azure AD-Anwendung mit einem selbstsignierten Zertifikat, erstellt ein Dienstprinzipalkonto für die Anwendung in Azure AD und weist die Rolle „Mitwirkender“ für das Konto in Ihrem aktuellen Abonnement zu. Sie können diese Einstellung auch in „Besitzer“ oder eine beliebige andere Rolle ändern. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
  * Erstellt ein Automation-Zertifikatasset mit dem Namen *AzureRunAsCertificate* im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Schlüssel des Zertifikats, der von der Azure AD-Anwendung verwendet wird.
  * Erstellt ein Automation-Verbindungsasset mit dem Namen *AzureRunAsConnection* im angegebenen Automation-Konto. Das Verbindungsasset enthält die applicationId, tenantId, subscriptionId und den Zertifikatfingerabdruck.

* **Klassisches ausführendes Azure-Konto**: Dieses Konto wird zum Verwalten von klassischen Bereitstellungsmodellressourcen verwendet.
  * Erstellt ein Automation-Zertifikatasset mit dem Namen *AzureClassicRunAsCertificate* im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Zertifikatschlüssel, der vom Verwaltungszertifikat verwendet wird.
  * Erstellt ein Automation-Verbindungsasset mit dem Namen *AzureClassicRunAsConnection* im angegebenen Automation-Konto. Das Verbindungsasset enthält den Abonnementnamen, die subscriptionId und den Namen des Zertifikatassets.
  
  > [!NOTE]
  > Azure Cloud Solution Provider-Abonnements (Azure CSP) unterstützen nur das Azure Resource Manager-Modell. Dienste, die nicht auf Azure Resource Manager basieren, sind in diesem Programm nicht verfügbar. Bei Verwendung eines CSP-Abonnements wird kein klassisches ausführendes Azure-Konto erstellt. Das ausführende Azure-Konto wird weiterhin erstellt. Weitere Informationen zu CSP-Abonnements finden Sie unter [Verfügbare Dienste in CSP-Abonnements](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services#comments).

## <a name="permissions"></a>Berechtigungen zum Konfigurieren von ausführenden Konten

Um ein ausführendes Konto erstellen oder aktualisieren zu können, müssen Sie über bestimmte Rechte und Berechtigungen verfügen. Ein globaler Administrator/Co-Administrator kann alle Aufgaben ausführen. In der folgenden Tabelle werden die Aufgaben, das entsprechende Cmdlet und die erforderlichen Berechtigungen für Situationen mit Aufgabentrennung aufgeführt:

|Aufgabe|Cmdlet  |Mindestberechtigungen  |
|---|---------|---------|
|Erstellen einer Azure AD-Anwendung|[New-AzureRmADApplication](/powershell/module/azurerm.resources/new-azurermadapplication)     | Rolle „Anwendungsentwickler“        |
|Hinzufügen von Anmeldeinformationen zur Anwendung|[New-AzureRmADAppCredential](/powershell/module/AzureRM.Resources/New-AzureRmADAppCredential)     | Anwendungsadministrator oder GLOBALER ADMINISTRATOR         |
|Erstellen und Abrufen eines Azure AD-Dienstprinzipals|[New-AzureRMADServicePrincipal](/powershell/module/AzureRM.Resources/New-AzureRmADServicePrincipal)</br>[Get-AzureRmADServicePrincipal](/powershell/module/AzureRM.Resources/Get-AzureRmADServicePrincipal)     | Anwendungsadministrator oder GLOBALER ADMINISTRATOR        |
|Zuweisen oder Erhalten der RBAC-Rolle für den angegebenen Prinzipal|[New-AzureRMRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment)</br>[Get-AzureRMRoleAssignment](/powershell/module/AzureRM.Resources/Get-AzureRmRoleAssignment)      | Benutzerzugriffsadministrator oder Besitzer        |
|Erstellen oder Entfernen eines Automation-Zertifikats|[New-AzureRmAutomationCertificate](/powershell/module/AzureRM.Automation/New-AzureRmAutomationCertificate)</br>[Remove-AzureRmAutomationCertificate](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationCertificate)     | Mitwirkender der Ressourcengruppe         |
|Erstellen oder Entfernen einer Automation-Verbindung|[New-AzureRmAutomationConnection](/powershell/module/AzureRM.Automation/New-AzureRmAutomationConnection)</br>[Remove-AzureRmAutomationConnection](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationConnection)|Mitwirkender der Ressourcengruppe |

* Ein AD-Benutzerkonto mit Berechtigungen, die der Rolle „Mitwirkender“ für Microsoft.Automation-Ressourcen entsprechen. Dies wird im Artikel [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md#contributor) erläutert.  
* Benutzer Ihres Azure AD-Mandanten, die keine Administratoren sind, können [AD-Anwendungen registrieren](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions), wenn die Option **Benutzer können Anwendungen registrieren** des Azure AD-Mandanten auf der Seite **Benutzereinstellungen** auf **Ja** festgelegt ist. Wenn für die App-Registrierungen **Nein** festgelegt ist, muss der Benutzer, der diese Aktion ausführt, ein globaler Administrator in Azure AD sein.

Wenn Sie kein Mitglied der Active Directory-Instanz des Abonnements sind, bevor Sie der Rolle „Globaler Administrator/Co-Administrator“ des Abonnements hinzugefügt werden, werden Sie als Gast hinzugefügt. In diesem Fall wird auf der Seite **Automation-Konto hinzufügen** eine Warnung vom Typ `You do not have permissions to create…` angezeigt. Benutzer, die zuerst der Rolle des globalen Administrators/Co-Administrators hinzugefügt wurden, können aus der Active Directory-Instanz des Abonnements entfernt und erneut hinzugefügt werden, um sie als Vollbenutzer in Active Directory einzurichten. Sie können dies im Azure-Portal im Bereich **Azure Active Directory** überprüfen. Wählen Sie hierzu **Benutzer und Gruppen**, **Alle Benutzer** und nach Auswahl des jeweiligen Benutzers die Option **Profil**. Als Wert des Attributs **Benutzertyp** im Benutzerprofil darf nicht **Gast** angegeben sein.

## <a name="create-a-run-as-account-in-the-portal"></a>Erstellen eines ausführenden Kontos im Portal

Führen Sie in diesem Abschnitt die folgenden Schritte aus, um im Azure-Portal Ihr Azure Automation-Konto zu aktualisieren. Sie erstellen das ausführende Konto und das klassische ausführende Konto separat. Wenn Sie keine klassischen Ressourcen verwalten müssen, ist es nur erforderlich, das ausführende Azure-Konto zu erstellen.  

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.
2. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Automation** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie die Option **Automation-Konten**.
3. Wählen Sie auf der Seite **Automation-Konten** in der entsprechenden Liste Ihr Automation-Konto aus.
4. Wählen Sie im linken Bereich im Abschnitt **Kontoeinstellungen** die Option **Ausführende Konten**.  
5. Abhängig vom benötigten Konto wählen Sie **Ausführendes Azure-Konto** oder **Klassisches ausführendes Azure-Konto** aus. Nach der Auswahl wird das Blatt **Ausführendes Azure-Konto hinzufügen** oder **Klassisches ausführendes Azure-Konto hinzufügen** angezeigt. Überprüfen Sie die Übersichtsinformationen, und klicken Sie auf **Erstellen**, um mit dem Erstellen des ausführenden Kontos fortzufahren.  
6. Während das ausführende Konto in Azure erstellt wird, können Sie den Status unter **Benachrichtigungen** im Menü nachverfolgen. Es wird auch ein Banner mit dem Hinweis angezeigt, dass das Konto erstellt wird. Dieser Vorgang kann einige Minuten dauern.  

## <a name="create-run-as-account-using-powershell"></a>Erstellen eines ausführenden Kontos mit PowerShell

## <a name="prerequisites"></a>Voraussetzungen

In der folgenden Liste sind die Anforderungen zum Erstellen eines ausführenden Kontos in PowerShell aufgeführt:

* Windows 10 oder Windows Server 2016 mit Azure Resource Manager-Modul 3.4.1 und höher. Das PowerShell-Skript unterstützt keine früheren Versionen von Windows.
* Azure PowerShell 1.0 und höher. Informationen zur PowerShell 1.0-Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
* Ein Automation-Konto, auf das als Wert für die Parameter *-AutomationAccountName* und *-ApplicationDisplayName* verwiesen wird.
* Berechtigungen, die den unter [Berechtigungen zum Konfigurieren von ausführenden Konten](#permissions) aufgelisteten Angaben entsprechen

Gehen Sie wie folgt vor, um die Werte für *SubscriptionID*, *ResourceGroup* und *AutomationAccountName* (erforderliche Parameter für das Skript) abzurufen:

1. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Automation** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie die Option **Automation-Konten**.
1. Wählen Sie auf der Seite „Automation-Konto“ Ihr Automation-Konto und dann unter **Kontoeinstellungen** die Option **Eigenschaften**.  
1. Beachten Sie auf der Seite **Eigenschaften** die Werte **Abonnement-ID**, **Name** und **Ressourcengruppe**.

   ![Seite „Eigenschaften“ des Automation-Kontos](media/manage-runas-account/automation-account-properties.png)

Dieses PowerShell-Skript unterstützt folgende Konfigurationen:

* Erstellen eines ausführenden Kontos mit einem selbstsignierten Zertifikat
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat
* Erstellen Sie ein ausführendes Konto und ein klassisches ausführendes Konto, indem Sie ein Zertifikat nutzen, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde.
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat in der Azure Government-Cloud

>[!NOTE]
> Wenn Sie eine Option zum Erstellen eines klassischen ausführenden Kontos wählen, müssen Sie nach dem Ausführen des Skripts das öffentliche Zertifikat (Dateinamenerweiterung „.cer“) in den Verwaltungsspeicher für das Abonnement hochladen, unter dem das Automation-Konto erstellt wurde.

1. Speichern Sie das folgende Skript auf dem Computer. In diesem Beispiel speichern Sie es mit dem Dateinamen *New-RunAsAccount.ps1*.

   Das Skript verwendet mehrere Azure Resource Manager-Cmdlets zum Erstellen von Ressourcen. In der folgenden Tabelle sind die Cmdlets und die erforderlichen Berechtigungen aufgeführt.

    ```powershell
    #Requires -RunAsAdministrator
    Param (
        [Parameter(Mandatory = $true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory = $true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory = $true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory = $true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory = $true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory = $true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory = $false)]
        [string] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [ValidateSet("AzureCloud", "AzureUSGovernment")]
        [string]$EnvironmentName = "AzureCloud",

        [Parameter(Mandatory = $false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
    )

    function CreateSelfSignedCertificate([string] $certificateName, [string] $selfSignedCertPlainPassword,
        [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired) -HashAlgorithm SHA256

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
    }

    function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $keyId = (New-Guid).Guid

        # Create an Azure AD application, AD App Credential, AD ServicePrincipal

        # Requires Application Developer Role, but works with Application administrator or GLOBAL ADMIN
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $keyId) 
        # Requires Application administrator or GLOBAL ADMIN
        $ApplicationCredential = New-AzureRmADAppCredential -ApplicationId $Application.ApplicationId -CertValue $keyValue -StartDate $PfxCert.NotBefore -EndDate $PfxCert.NotAfter
        # Requires Application administrator or GLOBAL ADMIN
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId 
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        # Requires User Access Administrator or Owner.
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6) {
            Sleep -s 10
            New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
        return $Application.ApplicationId.ToString();
    }

    function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName, [string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
    }

    function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
    }

    Import-Module AzureRM.Profile
    Import-Module AzureRM.Resources

    $AzureRMProfileVersion = (Get-Module AzureRM.Profile).Version
    if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 4) -or ($AzureRMProfileVersion.Major -gt 3))) {
        Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
        return
    }

    Connect-AzureRmAccount -Environment $EnvironmentName 
    $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

    # Create a Run As account by using a service principal
    $CertifcateAssetName = "AzureRunAsCertificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionTypeName = "AzureServicePrincipal"

    if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
    }
    else {
        $CertificateName = $AutomationAccountName + $CertifcateAssetName
        $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
        $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
        $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
        CreateSelfSignedCertificate $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
    }

    # Create a service principal
    $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
    $ApplicationId = CreateServicePrincipal $PfxCert $ApplicationDisplayName

    # Create the Automation certificate asset
    CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

    # Populate the ConnectionFieldValues
    $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
    $TenantID = $SubscriptionInfo | Select TenantId -First 1
    $Thumbprint = $PfxCert.Thumbprint
    $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

    # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
    CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

    if ($CreateClassicRunAsAccount) {
        # Create a Run As account by using a service principal
        $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
        $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
        $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
        $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
        "Log in to the Microsoft Azure portal (https://portal.azure.com) and select Subscriptions -> Management Certificates." + [Environment]::NewLine +
        "Then click Upload and upload the .cer format of #CERT#"

        if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        }
        else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName + $ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName   $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red       $UploadMessage
    }
    ```

    > [!IMPORTANT]
    > **Add-AzureRmAccount** ist jetzt ein Alias für **Connect-AzureRMAccount**. Wenn **Connect-AzureRMAccount** beim Durchsuchen der Bibliothekselemente nicht angezeigt wird, können Sie **Add-AzureRmAccount** verwenden oder [Ihre Module in Ihrem Automation-Konto aktualisieren](automation-update-azure-modules.md).

1. Starten Sie auf Ihrem Computer **Windows PowerShell** über den **Startbildschirm** mit erhöhten Benutzerrechten.
1. Wechseln Sie von der Befehlszeilenshell mit erhöhten Rechten zu dem Ordner, der das in Schritt 1 erstellte Skript enthält.  
1. Führen Sie das Skript aus, indem Sie die Parameterwerte für die benötigte Konfiguration verwenden.

    **Erstellen eines ausführenden Kontos mit einem selbstsignierten Zertifikat**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
    ```

    **Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
    ```

    **Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem Unternehmenszertifikat**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
    ```

    **Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat in der Azure Government-Cloud**
  
    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment
    ```

    > [!NOTE]
    > Nach dem Ausführen des Skripts werden Sie aufgefordert, sich gegenüber Azure zu authentifizieren. Melden Sie sich mit einem Konto an, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist.

Beachten Sie nach dem erfolgreichen Ausführen des Skripts Folgendes:

* Falls Sie ein klassisches ausführendes Konto mit einem selbstsignierten öffentlichen Zertifikat (CER-Datei) erstellt haben, führt das Skript die Erstellung durch und speichert es im Ordner für temporäre Dateien auf Ihrem Computer unter dem Benutzerprofil *%USERPROFILE%\AppData\Local\Temp*, das Sie zum Ausführen der PowerShell-Sitzung verwendet haben.

* Wenn Sie ein klassisches ausführendes Konto mit einem öffentlichen Unternehmenszertifikat (CER-Datei) erstellt haben, sollten Sie dieses Zertifikat verwenden. Befolgen Sie die Anweisungen zum [Hochladen eines Verwaltungs-API-Zertifikats in das Azure-Portal](../azure-api-management-certs.md).

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Löschen eines ausführenden Kontos oder klassischen ausführenden Kontos

In diesem Abschnitt wird beschrieben, wie Sie ein ausführendes Konto oder klassisches ausführendes Konto löschen und neu erstellen. Bei dieser Aktion wird das Automation-Konto beibehalten. Nach dem Löschen eines ausführenden Kontos oder klassischen ausführenden Kontos können Sie es im Azure-Portal neu erstellen.

1. Öffnen Sie das Automation-Konto im Azure-Portal.

2. Klicken Sie auf der Seite **Automation-Konto** auf **Ausführende Konten**.

3. Wählen Sie auf der Eigenschaftenseite **Ausführende Konten** entweder das ausführende Konto oder das klassische ausführende Konto aus, das Sie löschen möchten. Klicken Sie anschließend auf der Seite **Eigenschaften** für das ausgewählte Konto auf **Löschen**.

 ![Löschen des ausführenden Azure-Kontos](media/manage-runas-account/automation-account-delete-runas.png)

1. Während das Konto gelöscht wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen.

1. Nach dem Löschen des Kontos können Sie das Konto neu erstellen, indem Sie auf der Eigenschaftenseite **Ausführende Konten** auf die Erstellungsoption für **Ausführendes Azure-Konto** klicken.

 ![Neuerstellen des ausführenden Automation-Kontos](media/manage-runas-account/automation-account-create-runas.png)

## <a name="cert-renewal"></a>Erneuerung eines selbstsignierten Zertifikats

Das Zertifikat muss vor Ablauf Ihres ausführenden Kontos erneuert werden. Wenn Sie der Meinung sind, dass das ausführende Konto kompromittiert wurde, können Sie es löschen und neu erstellen. In diesem Abschnitt wird beschrieben, wie Sie dabei vorgehen.

Das selbstsignierte Zertifikat, das Sie für das ausführende Konto erstellt haben, läuft ein Jahr nach dem Erstellungsdatum ab. Sie können es vor dem Ablaufdatum jederzeit erneuern. Beim Erneuern wird das aktuell gültige Zertifikat beibehalten, um sicherzustellen, dass sich keine negativen Auswirkungen auf Runbooks ergeben, die sich in der Warteschlange befinden oder aktiv ausgeführt werden und die mit dem ausführenden Konto authentifiziert werden. Das Zertifikat bleibt bis zum Ablaufdatum gültig.

> [!NOTE]
> Wenn Sie Ihr ausführendes Automation-Konto für die Verwendung eines Zertifikats konfiguriert haben, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde, und diese Option verwenden, wird das Unternehmenszertifikat durch ein selbstsigniertes Zertifikat ersetzt.

Führen Sie folgende Schritte aus, um das Zertifikat zu erneuern:

1. Öffnen Sie das Automation-Konto im Azure-Portal.

1. Wählen Sie unter **Kontoeinstellungen** die Option **Ausführende Konten** aus.

    ![Eigenschaftenbereich des Automation-Kontos](media/manage-runas-account/automation-account-properties-pane.png)

1. Wählen Sie auf der Eigenschaftenseite **Ausführende Konten** entweder das ausführende Konto oder das klassische ausführende Konto aus, für das Sie das Zertifikat erneuern möchten.

1. Klicken Sie im Bereich **Eigenschaften** für das ausgewählte Konto auf **Zertifikat erneuern**.

    ![Erneuern des Zertifikats für das ausführende Konto](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Während das Zertifikat erneuert wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen. 

## <a name="limiting-run-as-account-permissions"></a>Einschränken der Berechtigungen für ausführendes Konto

Um die Ausrichtung der Automatisierung für Ressourcen in Azure Automation zu steuern, werden dem ausführenden Konto standardmäßig Berechtigungen als Mitwirkender im Abonnement gewährt. Wenn Sie die Möglichkeiten des ausführenden Dienstprinzipals einschränken müssen, können Sie das Konto aus der Rolle „Mitwirkender“ des Abonnements entfernen und es als „Mitwirkender“ zu den Ressourcengruppen hinzufügen, die Sie angeben möchten.

Wählen Sie im Azure-Portal **Abonnements**, und wählen Sie das Abonnement Ihres Automation-Kontos. Wählen Sie **Zugriffssteuerung (IAM)**, und suchen Sie nach dem Dienstprinzipal für Ihr Automation-Konto (er sieht etwa wie folgt aus: \<AutomationKontoname\>_eindeutiger Bezeichner). Wählen Sie das Konto aus, und klicken Sie auf **Entfernen**, um es aus dem Abonnement zu entfernen.

![Abonnementmitwirkende](media/manage-runas-account/automation-account-remove-subscription.png)

Um den Dienstprinzipal zu einer Ressourcengruppe hinzuzufügen, wählen Sie die Ressourcengruppe im Azure-Portal und wählen dann **Zugriffssteuerung (IAM)**. Wählen Sie **Hinzufügen**, um die Seite **Berechtigungen hinzufügen** zu öffnen. Wählen Sie als **Rolle** die Option **Mitwirkender**. Geben Sie in das Textfeld **Auswählen** den Namen des Dienstprinzipals für Ihr ausführendes Konto ein, und wählen Sie es aus der Liste aus. Klicken Sie zum Speichern der Änderungen auf **Speichern**. Wiederholen Sie diese Schritte für die Ressourcengruppen, auf die der ausführende Azure Automation-Dienstprinzipal Zugriff haben soll.

## <a name="misconfiguration"></a>Fehlkonfiguration

Einige Konfigurationselemente, die erforderlich sind, damit das ausführende Konto bzw. das klassische ausführende Konto richtig funktioniert, wurden ggf. gelöscht oder bei der anfänglichen Einrichtung nicht richtig erstellt. Beispiele für diese Elemente sind:

* Zertifikatasset
* Verbindungsasset
* Entfernung des ausführenden Kontos aus der Rolle „Mitwirkender“
* Dienstprinzipal oder Anwendung in Azure AD

In den obigen und anderen Fällen einer Fehlkonfiguration erkennt das Automation-Konto die Änderungen und zeigt auf der Eigenschaftenseite **Ausführende Konten** für das Konto den Status *Unvollständig* an.

![Konfigurationsstatus „Unvollständig“ für ausführendes Konto](media/manage-runas-account/automation-account-runas-incomplete-config.png)

Wenn Sie das ausführende Konto auswählen, wird im Bereich **Eigenschaften** die folgende Fehlermeldung angezeigt:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

Sie können diese Probleme mit ausführenden Konten schnell beheben, indem Sie das Konto löschen und neu erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](../active-directory/develop/app-objects-and-service-principals.md).
* Weitere Informationen zu Zertifikaten und Azure-Diensten finden Sie unter [Übersicht über Zertifikate für Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).
