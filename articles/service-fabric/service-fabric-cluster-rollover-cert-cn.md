---
title: Rollover für ein Azure Service Fabric-Clusterzertifikat | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Rollover für ein Service Fabric-Clusterzertifikat ausführen, das anhand seines allgemeinen Zertifikatsnamens identifiziert wird.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: aljo
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: ryanwi
ms.openlocfilehash: df919e23fd608cdf41e93844f13342ca00657adb
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34205143"
---
# <a name="manually-roll-over-a-service-fabric-cluster-certificate"></a>Manuelles Rollover für ein Service Fabric-Clusterzertifikat
Wenn ein Service Fabric-Clusterzertifikat in Kürze abläuft, müssen Sie das Zertifikat aktualisieren.  Ein Zertifikatrollover ist einfach, wenn der Cluster [für die Verwendung von Zertifikaten auf Basis des allgemeinen Namens konfiguriert wurde](service-fabric-cluster-change-cert-thumbprint-to-cn.md) (anstelle eines Fingerabdrucks).  Fordern Sie ein neues Zertifikat von einer Zertifizierungsstelle mit einem neuen Ablaufdatum an.  Selbstsignierte Zertifikate, einschließlich der beim Bereitstellen eines Service Fabric-Clusters im Azure-Portal generierten Zertifikate, werden nicht unterstützt.  Das neue Zertifikat muss den gleichen allgemeinen Name wie das ältere Zertifikat haben. 

Mit dem folgenden Skript wird ein neues Zertifikat in einen Schlüsseltresor hochgeladen und anschließend in der VM-Skalierungsgruppe installiert.  Der Service Fabric-Cluster verwendet automatisch das Zertifikat mit dem Ablaufdatum, das am weitesten in der Zukunft liegt.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  <subscription ID>

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "keyvaultgroup"
$VaultName = "cntestvault2"
$certFilename = "C:\users\sfuser\sftutorialcluster20180419110824.pfx"
$certname = "cntestcert"
$Password  = "!P@ssw0rd321"
$VmssResourceGroupName     = "sfclustertutorialgroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzureRmResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Get the key vault.  The key vault must be enabled for deployment.
$keyVault = Get-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName 
$resourceId = $keyVault.ResourceId  

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certName  -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    

Set-StrictMode -Version 3
$ErrorActionPreference = "Stop"

$certConfig = New-AzureRmVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzureRmVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -Name $VmssName -VirtualMachineScaleSet $vmss  -Verbose
```

Weitere Informationen finden Sie in folgenden Artikeln:
* Erfahren Sie mehr über [Clustersicherheit](service-fabric-cluster-security.md).
* [Aktualisieren und Verwalten von Clusterzertifikaten](service-fabric-cluster-security-update-certs-azure.md)

