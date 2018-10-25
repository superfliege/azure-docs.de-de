---
title: Erstellen eines Azure Service Fabric-Clusters mit allgemeinem Zertifikatnamen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Service Fabric-Cluster erstellen, für den ein Zertifikat mit allgemeinem Namen aus einer Vorlage verwendet wird.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: ryanwi
ms.openlocfilehash: 191471d3538a9151827ee24a5887aa559383345b
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785663"
---
# <a name="deploy-a-service-fabric-cluster-that-uses-certificate-common-name-instead-of-thumbprint"></a>Bereitstellen eines Service Fabric-Clusters mit allgemeinem Zertifikatnamen anstelle eines Fingerabdrucks
Keine zwei Zertifikate können den gleichen Fingerabdruck haben, was ein Clusterzertifikatrollover oder die Verwaltung erschwert. Mehrere Zertifikate können jedoch den gleichen allgemeinen Namen oder den gleichen Antragsteller haben.  Cluster mit allgemeinen Zertifikatnamen vereinfachen die Zertifikatverwaltung. In diesem Artikel wird beschrieben, wie Sie einen Service Fabric-Cluster für die Verwendung des allgemeinen Zertifikatnamens (anstelle des Zertifikatfingerabdrucks) bereitstellen.
 
## <a name="get-a-certificate"></a>Abrufen eines Zertifikats
Fordern Sie zunächst ein Zertifikat von einer [Zertifizierungsstelle (CA)](https://wikipedia.org/wiki/Certificate_authority) an.  Der allgemeine Name des Zertifikats sollte der Hostname des Clusters sein.  Beispiel: "meinclustername.southcentralus.cloudapp.azure.com".  

Zu Testzwecken können Sie von einer kostenlosen oder offenen Zertifizierungsstelle ein von der Zertifizierungsstelle signiertes Zertifikat abrufen.

> [!NOTE]
> Selbstsignierte Zertifikate, einschließlich der beim Bereitstellen eines Service Fabric-Clusters im Azure-Portal generierten Zertifikate, werden nicht unterstützt.

## <a name="upload-the-certificate-to-a-key-vault"></a>Hochladen des Zertifikats in einen Schlüsseltresor
In Azure wird ein Service Fabric-Cluster in einer VM-Skalierungsgruppe bereitgestellt.  Laden Sie das Zertifikat in einen Schlüsseltresor hoch.  Bei der Bereitstellung des Clusters wird das Zertifikat in der VM-Skalierungsgruppe installiert, in welcher der Cluster ausgeführt wird.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"

# Create new Resource Group 
New-AzureRmResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName -Location $region -EnabledForDeployment 
$resourceId = $newKeyVault.ResourceId 

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
```

## <a name="download-and-update-a-sample-template"></a>Herunterladen und Aktualisieren einer Beispielvorlage
In diesem Artikel werden die Beispielvorlage für einen [Sicherheitscluster mit 5 Knoten](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure) und die zugehörigen Vorlagenparameter verwendet. Laden Sie die Dateien *azuredeploy.json* und *azuredeploy-parameters.json* auf Ihren Computer herunter.

### <a name="update-parameters-file"></a>Aktualisieren der Parameterdatei
Öffnen Sie zunächst die Datei *azuredeploy.parameters.json* in einem Text-Editor, und fügen Sie den folgenden Parameterwert hinzu:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Legen Sie anschließend die Parameterwerte *certificateCommonName*, *sourceVaultValue* und *certificateUrlValue* auf die Werte fest, die vom vorhergehenden Skript zurückgegeben werden:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
"sourceVaultValue": {
  "value": "/subscriptions/<subscription>/resourceGroups/testvaultgroup/providers/Microsoft.KeyVault/vaults/testvault"
},
"certificateUrlValue": {
  "value": "https://testvault.vault.azure.net:443/secrets/testcert/5c882b7192224447bbaecd5a46962655"
},
```

### <a name="update-the-template-file"></a>Aktualisieren der Vorlagendatei
Öffnen Sie als Nächstes die Datei *azuredeploy.json* in einem Text-Editor, und nehmen Sie drei Aktualisierungen vor, damit der allgemeine Zertifikatname unterstützt wird.

1. Fügen Sie im Abschnitt **parameters** einen Parameter *certificateCommonName* hinzu:
    ```json
    "certificateCommonName": {
      "type": "string",
      "metadata": {
        "description": "Certificate Commonname"
      }
    },
    ```

    Außerdem könnten Sie *certificateThumbprint* entfernen, da dieser Parameter wahrscheinlich nicht mehr benötigt wird.

2. Legen Sie den Wert der Variablen *SfrpApiVersion* auf „2018-02-01“ fest:
    ```json
    "sfrpApiVersion": "2018-02-01",
    ```

3. Aktualisieren Sie in der Ressource **Microsoft.Compute/virtualMachineScaleSets** die VM-Erweiterung, damit in den Zertifikateinstellungen anstelle des Fingerabdrucks der allgemeine Name verwendet wird.  Fügen Sie unter **virtualMachineProfile**->**extensionProfile**->**extensions**->**properties**->**settings**->**certificate** den Eintrag 
    ```json
       "commonNames": [
        "[parameters('certificateCommonName')]"
       ],
    ```

    hinzu, und entfernen Sie `"thumbprint": "[parameters('certificateThumbprint')]",`.

    ```json
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              "type": "ServiceFabricNode",
              "autoUpgradeMinorVersion": true,
              "protectedSettings": {
                "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
              },
              "publisher": "Microsoft.Azure.ServiceFabric",
              "settings": {
                "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                "nodeTypeRef": "[variables('vmNodeType0Name')]",
                "dataPath": "D:\\SvcFab",
                "durabilityLevel": "Bronze",
                "enableParallelJobs": true,
                "nicPrefixOverride": "[variables('subnet0Prefix')]",
                "certificate": {
                  "commonNames": [
                     "[parameters('certificateCommonName')]"
                  ],
                  "x509StoreName": "[parameters('certificateStoreValue')]"
                }
              },
              "typeHandlerVersion": "1.0"
            }
          },
    ```

4.  Aktualisieren Sie in der Ressource **Microsoft.ServiceFabric/clusters** die API-Version auf „2018-02-01“.  Fügen Sie auch eine Einstellung **certificateCommonNames** mit der Eigenschaft **commonNames** hinzu, und entfernen Sie die Einstellung **certificate** (mit der Eigenschaft „Thumbprint“), wie im folgenden Beispiel gezeigt:
    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
        ],
        "properties": {
        "addonFeatures": [
            "DnsService",
            "RepairManager"
        ],        
        "certificateCommonNames": {
            "commonNames": [
            {
                "certificateCommonName": "[parameters('certificateCommonName')]",
                "certificateIssuerThumbprint": ""
            }
            ],
            "x509StoreName": "[parameters('certificateStoreValue')]"
        },
        ...
    ```

## <a name="deploy-the-updated-template"></a>Bereitstellen der aktualisierten Vorlage
Stellen Sie nach Abschluss der Änderungen die aktualisierte Vorlage erneut bereit.

```powershell
# Variables.
$groupname = "testclustergroup"
$clusterloc="southcentralus"  
$id="<subscription ID"

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $id 

# Create a new resource group and deploy the cluster.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\AzureDeploy.Parameters.json" -TemplateFile "C:\temp\cluster\AzureDeploy.json" -Verbose
```

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Clustersicherheit](service-fabric-cluster-security.md).
* Erfahren Sie, wie Sie einen [Rollover für ein Cluster-Zertifikat ausführen](service-fabric-cluster-rollover-cert-cn.md).
* [Aktualisieren und Verwalten von Clusterzertifikaten](service-fabric-cluster-security-update-certs-azure.md)
* Vereinfachen der Zertifikatverwaltung durch [Ändern des Clusters aus „Fingerabdruck“ in „Allgemeiner Name“](service-fabric-cluster-change-cert-thumbprint-to-cn.md)

[image1]: .\media\service-fabric-cluster-change-cert-thumbprint-to-cn\PortalViewTemplates.png
