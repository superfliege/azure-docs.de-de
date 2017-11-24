---
title: Erstellen eines Service Fabric-Linux-Clusters in Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe der Azure CLI einen Service Fabric-Linux-Cluster in einem vorhandenen virtuellen Azure-Netzwerk bereitstellen.
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
ms.date: 09/26/2017
ms.author: ryanwi
ms.openlocfilehash: 84b219d31635af6fbdb6bd618e3a9bb4e4848809
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-a-service-fabric-linux-cluster-into-an-azure-virtual-network"></a>Bereitstellen eines Service Fabric-Linux-Clusters in einem virtuellen Azure-Netzwerk
Dieses Tutorial ist der erste Teil einer Serie. Hier erfahren Sie, wie Sie mithilfe der Azure CLI einen Service Fabric-Linux-Cluster in einem vorhandenen virtuellen Azure-Netzwerk (VNET) und Subnetz bereitstellen. Wenn Sie fertig sind, verfügen Sie über einen Cluster, der in der Cloud ausgeführt wird und für den Sie Anwendungen bereitstellen können. Informationen zum Erstellen eines Windows-Clusters mithilfe von PowerShell finden Sie unter [Bereitstellen eines sicheren Service Fabric-Windows-Clusters in einem virtuellen Azure-Netzwerk](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines VNET in Azure mithilfe der Azure CLI
> * Erstellen eines sicheren Service Fabric-Clusters in Azure mithilfe der Azure CLI
> * Sichern des Clusters mit einem X.509-Zertifikat
> * Herstellen einer Verbindung mit dem Cluster mit der Service Fabric-Befehlszeilenschnittstelle
> * Entfernen eines Clusters

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines sicheren Clusters in Azure
> * [Horizontales Herunter- oder Hochskalieren eines Clusters](/service-fabric-tutorial-scale-cluster.md)
> * [Bereitstellen von API Management mit Service Fabric](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Installieren Sie die [Service Fabric CLI](service-fabric-cli.md).
- Installieren Sie [Azure CLI 2.0](/cli/azure/install-azure-cli).

Mit den folgenden Verfahren wird ein Service Fabric-Cluster mit fünf Knoten erstellt. Zum Berechnen der Kosten, die durch das Ausführen eines Service Fabric-Clusters in Azure anfallen, verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/).

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Anmelden bei Azure und Auswählen Ihres Abonnements
In dieser Anleitung wird die Azure-Befehlszeilenschnittstelle verwendet. Wenn Sie eine neue Sitzung starten, melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus, bevor Sie Azure-Befehle ausführen.
 
Führen Sie das folgende Skript aus, um sich bei Ihrem Azure-Konto anzumelden und Ihr Abonnement auszuwählen:

```azurecli
az login
az account set --subscription <guid>
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie eine neue Ressourcengruppe für die Bereitstellung, und geben Sie einen Namen und einen Speicherort an.

```azurecli
ResourceGroupName="sflinuxclustergroup"
Location="southcentralus"
az group create --name $ResourceGroupName --location $Location
```

## <a name="deploy-the-network-topology"></a>Bereitstellen der Netzwerktopologie
Richten Sie die Netzwerktopologie ein, in der API Management und der Service Fabric-Cluster bereitgestellt werden. Die Resource Manager-Vorlage [network.json][network-arm] ist so konfiguriert, dass ein virtuelles Netzwerk (VNET) sowie ein Subnetz und eine Netzwerksicherheitsgruppe (NSG) für Service Fabric und ein Subnetz sowie eine Netzwerksicherheitsgruppe für API Management erstellt werden. Weitere Informationen zu VNETs, Subnetzen und NSGs finden Sie [hier](../virtual-network/virtual-networks-overview.md).

Die Parameterdatei [network.parameters.json][network-parameters-arm] enthält die Namen der Subnetze und NSGs, für die Service Fabric und API Management bereitgestellt werden.  API Management wird im [nachfolgenden Tutorial](service-fabric-tutorial-deploy-api-management.md) bereitgestellt. Für diese Anleitung müssen die Parameterwerte nicht geändert werden. Diese Werte werden in den Service Fabric-Resource Manager-Vorlagen verwendet.  Wenn die Werte hier geändert werden, müssen Sie sie in den anderen in diesem Tutorial und im [Tutorial zum Bereitstellen von API Management](service-fabric-tutorial-deploy-api-management.md) verwendeten Resource Manager-Vorlagen ändern. 

Laden Sie die folgende Resource Manager-Vorlage und -Parameterdatei herunter:
- [network.json][network-arm]
- [network.parameters.json][network-parameters-arm]

Verwenden Sie das folgende Skript, um die Resource Manager-Vorlagendatei und die Parameterdatei für die Netzwerkeinrichtung bereitzustellen:

```azurecli
az group deployment create \
    --name VnetDeployment \
    --resource-group $ResourceGroupName \
    --template-file network.json \
    --parameters @network.parameters.json
```
<a id="createvaultandcert" name="createvaultandcert_anchor"></a>
## <a name="deploy-the-service-fabric-cluster"></a>Bereitstellen des Service Fabric-Clusters
Nachdem die Bereitstellung der Netzwerkressourcen abgeschlossen ist, besteht der nächste Schritt in der Bereitstellung von Service Fabric-Clustern für das VNET im Subnetzt und in der NSG, die für den Service Fabric-Cluster angegeben wurden. Für die Bereitstellung eines Clusters in einem vorhandenen VNET und Subnetz (weiter oben in diesem Artikel bereitgestellt) ist eine Resource Manager-Vorlage erforderlich.  Weitere Informationen finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Für diese Tutorialreihe ist die Vorlage so vorkonfiguriert, dass die Namen des VNET, des Subnetzes und der NSG verwendet werden, die in einem vorherigen Schritt eingerichtet wurden.  

Laden Sie die folgende Resource Manager-Vorlage und -Parameterdatei herunter:
- [linuxcluster.json][cluster-arm]
- [linuxcluster.parameters.json][cluster-parameters-arm]

Verwenden Sie diese Vorlage, um einen sicheren Cluster zu erstellen.  Ein Clusterzertifikat ist ein X.509-Zertifikat, das zum Sichern einer Knoten-zu-Knoten-Kommunikation und zur Authentifizierung der Endpunkte der Clusterverwaltung bei einem Verwaltungsclient verwendet wird.  Dieses Clusterzertifikat stellt auch SSL für die HTTPS-Verwaltungs-API und für Service Fabric Explorer über HTTPS bereit. Zertifikate für Service Fabric-Cluster in Azure werden in Azure Key Vault verwaltet.  Wenn ein Cluster in Azure bereitgestellt wird, ruft der für die Erstellung von Service Fabric-Clustern zuständige Azure-Ressourcenanbieter Zertifikate aus dem Schlüsseltresor ab und installiert sie auf den virtuellen Clustercomputern. 

Sie können ein Zertifikat aus einer Zertifizierungsstelle als Clusterzertifikat verwenden. Erstellen Sie zu Testzwecken alternativ ein selbstsigniertes Zertifikat. Für das Clusterzertifikat muss Folgendes gelten:

- Es muss einen privaten Schlüssel enthalten.
- Es muss für den Schlüsselaustausch erstellt werden, um in eine PFX-Datei (Personal Information Exchange) exportiert werden zu können.
- Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die Sie auf den Service Fabric-Cluster zugreifen. Diese Übereinstimmung ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne „.cloudapp.azure.com“ können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, den Sie für Ihren Cluster verwenden.

Geben Sie für die Bereitstellung die leeren Parameter in der Datei *linuxcluster.parameters.json* an:

|Parameter|Wert|
|---|---|
|adminPassword|Password#1234|
|adminUserName|vmadmin|
|clusterName|mysfcluster|

Lassen Sie die Parameter **certificateThumbprint**, **certificateUrlValue** und **sourceVaultValue** leer, um ein selbstsigniertes Zertifikat zu erstellen.  Wenn Sie ein vorhandenes Zertifikat verwenden möchten, das zuvor in einen Schlüsseltresor hochgeladen wurde, geben Sie die entsprechenden Parameterwerte ein.

Das folgende Skript verwendet den Befehl und die Vorlage [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create), um einen neuen Cluster in Azure bereitzustellen. Das Cmdlet erstellt auch einen neuen Schlüsseltresor in Azure, fügt diesem ein neues selbstsigniertes Zertifikat hinzu und lädt das Zertifikat an einen lokalen Speicherort herunter. Sie können ein vorhandenes Zertifikat und/oder einen vorhandenen Schlüsseltresor angeben, indem Sie andere Parameter des Befehls [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) verwenden.

```azurecli
Password="q6D7nN%6ck@6"
Subject="mysfcluster.southcentralus.cloudapp.azure.com"
VaultName="linuxclusterkeyvault"
az group create --name $ResourceGroupName --location $Location

az sf cluster create --resource-group $ResourceGroupName --location $Location \
   --certificate-output-folder . --certificate-password $Password --certificate-subject-name $Subject \
   --vault-name $VaultName --vault-resource-group $ResourceGroupName  \
   --template-file linuxcluster.json --parameter-file linuxcluster.parameters.json

```

## <a name="connect-to-the-secure-cluster"></a>Herstellen einer Verbindung mit dem sicheren Cluster
Stellen Sie eine Verbindung mit dem Cluster über den Service Fabric-CLI-Befehl `sfctl cluster select` unter Verwendung Ihres Schlüssels her.  Verwenden Sie für ein selbstsigniertes Zertifikat nur die Option **--no-verify**.

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Überprüfen Sie mithilfe des Befehls `sfctl cluster health`, ob die Verbindung hergestellt wurde und der Cluster fehlerfrei ist.

```azurecli
sfctl cluster health
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
In den anderen Artikeln dieser Tutorialreihe wird der soeben erstellte Cluster verwendet. Wenn Sie nicht direkt mit dem nächsten Artikel fortfahren, kann es ratsam sein, den Cluster zu löschen, um anfallende Gebühren zu vermeiden. Die einfachste Möglichkeit zum Löschen des Clusters und aller darin genutzten Ressourcen besteht darin, die Ressourcengruppe zu löschen.

Melden Sie sich bei Azure an, und wählen Sie die Abonnement-ID aus, mit der Sie den Cluster entfernen möchten.  Sie finden Ihre Abonnement-ID, indem Sie sich beim [Azure-Portal](http://portal.azure.com) anmelden. Löschen Sie die Ressourcengruppe und alle Clusterressourcen mithilfe des Befehls [az group delete](/cli/azure/group?view=azure-cli-latest#az_group_delete).

```azurecli
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines VNET in Azure mithilfe der Azure CLI
> * Erstellen eines sicheren Service Fabric-Clusters in Azure mithilfe der Azure CLI
> * Sichern des Clusters mit einem X.509-Zertifikat
> * Herstellen einer Verbindung mit dem Cluster mit der Service Fabric-Befehlszeilenschnittstelle
> * Entfernen eines Clusters

Fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie Ihren Cluster skalieren.
> [!div class="nextstepaction"]
> [Skalieren eines Clusters](service-fabric-tutorial-scale-cluster.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/linuxcluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/linuxcluster.parameters.json
