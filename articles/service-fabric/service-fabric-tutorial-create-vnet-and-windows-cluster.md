---
title: Erstellen eines Service Fabric-Windows-Clusters in Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe der PowerShell einen Service Fabric-Windows-Cluster in einem vorhandenen virtuellen Azure-Netzwerk bereitstellen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 469246d6cb64d6aaf995ef3b7c4070f8d24372b1
ms.openlocfilehash: 7cee4f8d68062dcfd2b6f61d55319160a2a80a98
ms.contentlocale: de-de
ms.lasthandoff: 09/27/2017

---

# <a name="deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Bereitstellen eines Service Fabric-Windows-Clusters in einem virtuellen Azure-Netzwerk
Dieses Tutorial ist der erste Teil einer Serie. Hier erfahren Sie, wie Sie mithilfe der PowerShell einen Service Fabric-Windows-Cluster in einem vorhandenen virtuellen Azure-Netzwerk (VNET) und Subnetz bereitstellen. Wenn Sie fertig sind, verfügen Sie über einen Cluster, der in der Cloud ausgeführt wird und für den Sie Anwendungen bereitstellen können.  Informationen zum Erstellen eines Linux-Clusters per Azure CLI finden Sie unter [Erstellen eines sicheren Linux-Clusters in Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines VNET in Azure mithilfe von PowerShell
> * Erstellen eines Schlüsseltresors und Hochladen eines Zertifikats
> * Erstellen eines sicheren Service Fabric-Clusters in Azure PowerShell
> * Sichern des Clusters mit einem X.509-Zertifikat
> * Herstellen einer Verbindung mit dem Cluster über PowerShell
> * Entfernen eines Clusters

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines sicheren Clusters in Azure
> * [Bereitstellen von API Management mit Service Fabric](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Installieren Sie das [Service Fabric SDK und das PowerShell-Modul](service-fabric-get-started.md).
- Installieren Sie das [Azure PowerShell-Modul Version 4.1 oder höher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

Mit den folgenden Verfahren wird ein Service Fabric-Cluster mit fünf Knoten erstellt. Zum Berechnen der Kosten, die durch das Ausführen eines Service Fabric-Clusters in Azure anfallen, verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/).

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Anmelden bei Azure und Auswählen Ihres Abonnements
In dieser Anleitung wird Azure PowerShell verwendet. Wenn Sie eine neue PowerShell-Sitzung starten, melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus, bevor Sie Azure-Befehle ausführen.
 
Führen Sie das folgende Skript aus, um sich bei Ihrem Azure-Konto anzumelden und Ihr Abonnement auszuwählen:

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie eine neue Ressourcengruppe für die Bereitstellung, und geben Sie einen Namen und einen Speicherort an.

```powershell
$ResourceGroupName = "sfclustertutorialgroup"
New-AzureRmResourceGroup -Name $ResourceGroupName -Location centralus
```

## <a name="deploy-the-network-topology"></a>Bereitstellen der Netzwerktopologie
Richten Sie die Netzwerktopologie ein, in der API Management und der Service Fabric-Cluster bereitgestellt werden. Die Resource Manager-Vorlage [network.json][network-arm] ist so konfiguriert, dass ein virtuelles Netzwerk (VNET) sowie ein Subnetz und eine Netzwerksicherheitsgruppe (NSG) für Service Fabric und ein Subnetz sowie eine Netzwerksicherheitsgruppe für API Management erstellt werden. Weitere Informationen zu VNETs, Subnetzen und NSGs finden Sie [hier](../virtual-network/virtual-networks-overview.md).

Die Parameterdatei [network.parameters.json][network-parameters-arm] enthält die Namen der Subnetze und NSGs, für die Service Fabric und API Management bereitgestellt werden.  API Management wird im [nachfolgenden Tutorial](service-fabric-tutorial-deploy-api-management.md) bereitgestellt. Für diese Anleitung müssen die Parameterwerte nicht geändert werden. Diese Werte werden in den Service Fabric-Resource Manager-Vorlagen verwendet.  Wenn die Werte hier geändert werden, müssen Sie sie in den anderen in diesem Tutorial und im [Tutorial zum Bereitstellen von API Management](service-fabric-tutorial-deploy-api-management.md) verwendeten Resource Manager-Vorlagen ändern. 

Laden Sie die folgende Resource Manager-Vorlage und -Parameterdatei herunter:
- [network.json][network-arm]
- [network.parameters.json][network-parameters-arm]

Verwenden Sie den folgenden PowerShell-Befehl, um die Resource Manager-Vorlagen- und -Parameterdateien für die Netzwerkeinrichtung bereitzustellen:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>
## <a name="create-a-key-vault-and-upload-a-certificate"></a>Erstellen eines Schlüsseltresors und Hochladen eines Zertifikats
Die Resource Manager-Vorlage für den Service Fabric-Cluster im nächsten Schritt ist so konfiguriert, dass ein sicherer Cluster mit Zertifikatsicherheit erstellt wird. Das Zertifikat wird zum Sichern der Knoten-zu-Knoten-Kommunikation für Ihren Cluster und zur Verwaltung des Benutzerzugriffs auf Ihre Service Fabric-Cluster verwendet. API Management verwendet auch dieses Zertifikat, um auf den Service Fabric Naming Service für die Dienstermittlung zuzugreifen. Dies erfordert ein Zertifikat in Key Vault für die Clustersicherheit.

Mit dem folgenden Skript werden ein Schlüsseltresor in Azure und ein selbstsigniertes Zertifikat erstellt sowie das Zertifikat in den Schlüsseltresor hochgeladen.  Wenn Sie ein vorhandenes Zertifikat verwenden möchten, legen Sie **$CreateSelfSignedCertificate** auf „$false“ fest, und geben Sie den Speicherort in **$ExistingPfxFilePath** an.

```powershell
$VaultResourceGroupName = 'ryanwikeyvaultgroup'
$VaultName= 'ryanwikeyvault'
$Location = "westus"
$CertificateName = "ryanwicertificate1"
$Password = 'mypa$$word!'
$DnsName = "www.mycluster.westus.mydomain.com" #The certificate's subject name must match the domain used to access the Service Fabric cluster.
$OutputPath = "C:\MyCertificates" # location where you want the .PFX to be stored
$CreateSelfSignedCertificate = $true
$ExistingPfxFilePath = 'C:\MyCertificates\ryanwicertificate1.pfx'

$ErrorActionPreference = 'Stop'

Write-Host "Switching context to SubscriptionId $SubscriptionId"
Set-AzureRmContext -SubscriptionId $SubscriptionId | Out-Null

# New-AzureRmResourceGroup is idempotent as long as the location matches
Write-Host "Ensuring ResourceGroup $VaultResourceGroupName in $Location"
New-AzureRmResourceGroup -Name $VaultResourceGroupName -Location $Location -Force | Out-Null
$resourceId = $null

try
{
    $existingKeyVault = Get-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultResourceGroupName
    $resourceId = $existingKeyVault.ResourceId

    Write-Host "Using existing vault $VaultName in $($existingKeyVault.Location)"
}
catch
{
}

if(!$existingKeyVault)
{
    Write-Host "Creating new vault $VaultName in $location"
    $newKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultResourceGroupName -Location $Location -EnabledForDeployment
    $resourceId = $newKeyVault.ResourceId
}

if($CreateSelfSignedCertificate)
{
    $securePassword = ConvertTo-SecureString -String $password -AsPlainText -Force

    $NewPfxFilePath = Join-Path $OutputPath $($CertificateName+".pfx")

    Write-Host "Creating new self signed certificate at $NewPfxFilePath"
    
    ## Changes to PSPKI version 3.5.2 New-SelfSignedCertificate replaced by New-SelfSignedCertificateEx
    $PspkiVersion = (Get-Module PSPKI).Version
    if($PSPKIVersion.Major -ieq 3 -And $PspkiVersion.Minor -ieq 2 -And $PspkiVersion.Build -ieq 5) {
        New-SelfsignedCertificateEx -Subject "CN=$DnsName" -EKU "Server Authentication", "Client authentication" -KeyUsage "KeyEncipherment, DigitalSignature" -Path $NewPfxFilePath -Password $securePassword -Exportable
    }
    else {
        New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName $DnsName | Export-PfxCertificate -FilePath $NewPfxFilePath -Password $securePassword | Out-Null
    }

    $ExistingPfxFilePath = $NewPfxFilePath
}

Write-Host "Reading pfx file from $ExistingPfxFilePath"
$cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2 $ExistingPfxFilePath, $Password

$bytes = [System.IO.File]::ReadAllBytes($ExistingPfxFilePath)
$base64 = [System.Convert]::ToBase64String($bytes)

$jsonBlob = @{
   data = $base64
   dataType = 'pfx'
   password = $Password
   } | ConvertTo-Json

    $contentbytes = [System.Text.Encoding]::UTF8.GetBytes($jsonBlob)
    $content = [System.Convert]::ToBase64String($contentbytes)

    $secretValue = ConvertTo-SecureString -String $content -AsPlainText -Force

Write-Host "Writing secret to $CertificateName in vault $VaultName"
$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $CertificateName -SecretValue $secretValue

$output = @{};
$output.SourceVault = $resourceId;
$output.CertificateURL = $secret.Id;
$output.CertificateThumbprint = $cert.Thumbprint;

Write-Host "Source vault: " $output.SourceVault
Write-Host "Certificate URL: " $output.CertificateURL
Write-Host "Certificate Thumbprint: " $output.CertificateThumbprint
```

## <a name="deploy-the-service-fabric-cluster"></a>Bereitstellen des Service Fabric-Clusters
Nachdem die Bereitstellung der Netzwerkressourcen abgeschlossen ist, besteht der nächste Schritt in der Bereitstellung von Service Fabric-Clustern für das VNET im Subnetzt und in der NSG, die für den Service Fabric-Cluster angegeben wurden. Für die Bereitstellung eines Clusters in einem vorhandenen VNET und Subnetz (weiter oben in diesem Artikel bereitgestellt) ist eine Resource Manager-Vorlage erforderlich.  Weitere Informationen finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Für diese Tutorialreihe ist die Vorlage so vorkonfiguriert, dass die Namen des VNET, des Subnetzes und der NSG verwendet werden, die in einem vorherigen Schritt eingerichtet wurden.  Laden Sie die folgende Resource Manager-Vorlage und -Parameterdatei herunter:
- [cluster.json][cluster-arm]
- [cluster.parameters.json][cluster-parameters-arm]

Fügen Sie in der Datei *cluster.parameters.json* für Ihre Bereitstellung die leeren Parameter **clusterName**, **adminUserName**, **adminPassword**, **certificateThumbprint**, **certificateUrlValue** und **sourceVaultValue** ein.  Wenn Sie über ein vorhandenes Zertifikat verfügen, das zuvor in einen Schlüsseltresor hochgeladen wurde, können Sie die Werte **certificateThumbprint**, **certificateUrlValue** und **sourceVaultValue** für dieses Zertifikat einfügen.

Verwenden Sie den folgenden PowerShell-Befehl, um die Resource Manager-Vorlagen- und -Parameterdateien zum Erstellen des Service Fabric-Clusters bereitzustellen:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
```

## <a name="connect-to-the-secure-cluster"></a>Herstellen einer Verbindung mit dem sicheren Cluster
Stellen Sie mithilfe des Service Fabric-PowerShell-Moduls, das zusammen mit dem Service Fabric SDK installiert wird, eine Verbindung mit dem Cluster her.  Zunächst installieren Sie das Zertifikat im persönlichen Speicher des aktuellen Benutzers auf Ihrem Computer.  Führen Sie den folgenden PowerShell-Befehl aus:

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Sie können nun die Verbindung mit Ihrem sicheren Cluster herstellen.

Das **Service Fabric**-PowerShell-Modul bietet viele Cmdlets zum Verwalten von Service Fabric-Clustern, Anwendungen und Diensten.  Verwenden Sie das Cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster), um eine Verbindung mit dem Cluster herzustellen. Die Informationen zum Zertifikatfingerabdruck und zum Verbindungsendpunkt finden Sie in der Ausgabe aus einem vorherigen Schritt.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Überprüfen Sie mithilfe des Cmdlets [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth), ob Sie verbunden sind und der Cluster fehlerfrei ist.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
In den anderen Artikeln dieser Tutorialreihe wird der soeben erstellte Cluster verwendet. Wenn Sie nicht direkt mit dem nächsten Artikel fortfahren, kann es ratsam sein, den Cluster zu löschen, um anfallende Gebühren zu vermeiden. Die einfachste Möglichkeit zum Löschen des Clusters und aller darin genutzten Ressourcen besteht darin, die Ressourcengruppe zu löschen.

Melden Sie sich bei Azure an, und wählen Sie die Abonnement-ID aus, mit der Sie den Cluster entfernen möchten.  Sie finden Ihre Abonnement-ID, indem Sie sich beim [Azure-Portal](http://portal.azure.com) anmelden. Löschen Sie die Ressourcengruppe und alle Clusterressourcen mithilfe des Cmdlets [Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId "Subcription ID"

$ResourceGroupName = "sfclustertutorialgroup"
Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines VNET in Azure mithilfe von PowerShell
> * Erstellen eines Schlüsseltresors und Hochladen eines Zertifikats
> * Erstellen eines sicheren Service Fabric-Clusters in Azure mithilfe von PowerShell
> * Sichern des Clusters mit einem X.509-Zertifikat
> * Herstellen einer Verbindung mit dem Cluster über PowerShell
> * Entfernen eines Clusters

Als Nächstes fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie API Management mit Service Fabric bereitstellen.
> [!div class="nextstepaction"]
> [Bereitstellen von API Management](service-fabric-tutorial-deploy-api-management.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json

