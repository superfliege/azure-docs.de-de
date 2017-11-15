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
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: ryanwi
ms.openlocfilehash: 31e35432ecc10b06c7a6400a1e0904e7bc2cd8c9
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2017
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
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc
```

## <a name="deploy-the-network-topology"></a>Bereitstellen der Netzwerktopologie
Richten Sie die Netzwerktopologie ein, in der API Management und der Service Fabric-Cluster bereitgestellt werden. Die Resource Manager-Vorlage [network.json][network-arm] ist so konfiguriert, dass ein virtuelles Netzwerk (VNET) sowie ein Subnetz und eine Netzwerksicherheitsgruppe (NSG) für Service Fabric und ein Subnetz sowie eine Netzwerksicherheitsgruppe für API Management erstellt werden. Weitere Informationen zu VNETs, Subnetzen und NSGs finden Sie [hier](../virtual-network/virtual-networks-overview.md).

Die Parameterdatei [network.parameters.json][network-parameters-arm] enthält die Namen der Subnetze und NSGs, für die Service Fabric und API Management bereitgestellt werden.  API Management wird im [nachfolgenden Tutorial](service-fabric-tutorial-deploy-api-management.md) bereitgestellt. Für diese Anleitung müssen die Parameterwerte nicht geändert werden. Diese Werte werden in den Service Fabric-Resource Manager-Vorlagen verwendet.  Wenn die Werte hier geändert werden, müssen Sie sie in den anderen in diesem Tutorial und im [Tutorial zum Bereitstellen von API Management](service-fabric-tutorial-deploy-api-management.md) verwendeten Resource Manager-Vorlagen ändern. 

Laden Sie die folgende Resource Manager-Vorlage und -Parameterdatei herunter:
- [network.json][network-arm]
- [network.parameters.json][network-parameters-arm]

Verwenden Sie den folgenden PowerShell-Befehl, um die Resource Manager-Vorlagen- und -Parameterdateien für die Netzwerkeinrichtung bereitzustellen:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateFile C:\winclustertutorial\network.json -TemplateParameterFile C:\winclustertutorial\network.parameters.json -Verbose
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>
## <a name="deploy-the-service-fabric-cluster"></a>Bereitstellen des Service Fabric-Clusters
Nachdem die Bereitstellung der Netzwerkressourcen abgeschlossen ist, besteht der nächste Schritt in der Bereitstellung von Service Fabric-Clustern für das VNET im Subnetzt und in der NSG, die für den Service Fabric-Cluster angegeben wurden. Für die Bereitstellung eines Clusters in einem vorhandenen VNET und Subnetz (weiter oben in diesem Artikel bereitgestellt) ist eine Resource Manager-Vorlage erforderlich.  Für diese Tutorialreihe ist die Vorlage so vorkonfiguriert, dass die Namen des VNET, des Subnetzes und der NSG verwendet werden, die in einem vorherigen Schritt eingerichtet wurden.  

Laden Sie die folgende Resource Manager-Vorlage und -Parameterdatei herunter:
- [cluster.json][cluster-arm]
- [cluster.parameters.json][cluster-parameters-arm]

Verwenden Sie diese Vorlage, um einen sicheren Cluster zu erstellen.  Ein Clusterzertifikat ist ein X.509-Zertifikat, das zum Sichern einer Knoten-zu-Knoten-Kommunikation und zur Authentifizierung der Endpunkte der Clusterverwaltung bei einem Verwaltungsclient verwendet wird.  Dieses Clusterzertifikat stellt auch SSL für die HTTPS-Verwaltungs-API und für Service Fabric Explorer über HTTPS bereit. Zertifikate für Service Fabric-Cluster in Azure werden in Azure Key Vault verwaltet.  Wenn ein Cluster in Azure bereitgestellt wird, ruft der für die Erstellung von Service Fabric-Clustern zuständige Azure-Ressourcenanbieter Zertifikate aus dem Schlüsseltresor ab und installiert sie auf den virtuellen Clustercomputern. 

Sie können ein Zertifikat aus einer Zertifizierungsstelle als Clusterzertifikat verwenden. Erstellen Sie zu Testzwecken alternativ ein selbstsigniertes Zertifikat. Für das Clusterzertifikat muss Folgendes gelten:

- Es muss einen privaten Schlüssel enthalten.
- Es muss für den Schlüsselaustausch erstellt werden, um in eine PFX-Datei (Personal Information Exchange) exportiert werden zu können.
- Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die Sie auf den Service Fabric-Cluster zugreifen. Diese Übereinstimmung ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne „.cloudapp.azure.com“ können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, den Sie für Ihren Cluster verwenden.

Füllen Sie diese leeren Parameter in der *cluster.parameters.json*-Datei für Ihre Bereitstellung ein:

|Parameter|Wert|
|---|---|
|adminPassword|Password#1234|
|adminUserName|vmadmin|
|clusterName|mysfcluster|
|location|southcentralus|

Lassen Sie die Parameter *certificateThumbprint*, *certificateUrlValue* und *sourceVaultValue* leer, um ein selbstsigniertes Zertifikat zu erstellen.  Wenn Sie ein vorhandenes Zertifikat verwenden möchten, das zuvor in einen Schlüsseltresor hochgeladen wurde, geben Sie die entsprechenden Parameterwerte ein.

Das folgende Skript verwendet das Cmdlet [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster), um einen neuen Cluster in Azure bereitzustellen. Das Cmdlet erstellt auch einen neuen Schlüsseltresor in Azure, fügt diesem ein neues selbstsigniertes Zertifikat hinzu und lädt das Zertifikat an einen lokalen Speicherort herunter. Sie können ein vorhandenes Zertifikat und/oder einen vorhandenen Schlüsseltresor angeben, indem Sie andere Parameter des Cmdlets [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) verwenden.

```powershell
# Variables.
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster"
$vaultname = "clusterkeyvault111"
$vaultgroupname="clusterkeyvaultgroup111"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile 'C:\winclustertutorial\cluster.json' `
-ParameterFile 'C:\winclustertutorial\cluster.parameters.json' -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResouceGroupName $vaultgroupname -CertificateSubjectName $subname
```

## <a name="connect-to-the-secure-cluster"></a>Herstellen einer Verbindung mit dem sicheren Cluster
Stellen Sie mithilfe des Service Fabric-PowerShell-Moduls, das zusammen mit dem Service Fabric SDK installiert wird, eine Verbindung mit dem Cluster her.  Zunächst installieren Sie das Zertifikat im persönlichen Speicher des aktuellen Benutzers auf Ihrem Computer.  Führen Sie den folgenden PowerShell-Befehl aus:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Sie können nun die Verbindung mit Ihrem sicheren Cluster herstellen.

Das **Service Fabric**-PowerShell-Modul bietet viele Cmdlets zum Verwalten von Service Fabric-Clustern, Anwendungen und Diensten.  Verwenden Sie das Cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster), um eine Verbindung mit dem Cluster herzustellen. Die Informationen zum Zertifikatfingerabdruck und zum Verbindungsendpunkt finden Sie in der Ausgabe des vorherigen Schritts.

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
In den anderen Artikeln dieser Tutorialreihe wird der soeben erstellte Cluster verwendet. Wenn Sie nicht direkt mit dem nächsten Artikel fortfahren, kann es ratsam sein, den Cluster und den Schlüsseltresor zu löschen, um anfallende Gebühren zu vermeiden. Die einfachste Möglichkeit zum Löschen des Clusters und aller darin genutzten Ressourcen besteht darin, die Ressourcengruppe zu löschen.

Löschen Sie die Ressourcengruppe und alle Clusterressourcen mithilfe des Cmdlets [Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).  Löschen Sie auch die Ressourcengruppe, die den Schlüsseltresor enthält.

```powershell
Remove-AzureRmResourceGroup -Name $groupname -Force
Remove-AzureRmResourceGroup -Name $vaultgroupname -Force
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

Als Nächstes fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie Ihren Cluster skalieren.
> [!div class="nextstepaction"]
> [Skalieren eines Clusters](service-fabric-tutorial-scale-cluster.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json
