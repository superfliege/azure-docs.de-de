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
ms.date: 01/22/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 3b09e676a26336d1ef1e744f9e45066c4815fe21
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2018
---
# <a name="deploy-a-service-fabric-linux-cluster-into-an-azure-virtual-network"></a>Bereitstellen eines Service Fabric-Linux-Clusters in einem virtuellen Azure-Netzwerk
Dieses Tutorial ist der erste Teil einer Serie. Es wird beschrieben, wie Sie einen Linux Service Fabric-Cluster in einem [virtuellen Azure-Netzwerk (VNET)](../virtual-network/virtual-networks-overview.md) und eine [Netzwerksicherheitsgruppe (NSG)](../virtual-network/virtual-networks-nsg.md) bereitstellen, indem Sie die Azure-Befehlszeilenschnittstelle und eine Vorlage verwenden. Wenn Sie fertig sind, verfügen Sie über einen Cluster, der in der Cloud ausgeführt wird und für den Sie Anwendungen bereitstellen können. Informationen zum Erstellen eines Windows-Clusters mithilfe von PowerShell finden Sie unter [Bereitstellen eines sicheren Service Fabric-Windows-Clusters in einem virtuellen Azure-Netzwerk](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

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
> * [Horizontales Herunter- oder Hochskalieren eines Clusters](service-fabric-tutorial-scale-cluster.md)
> * [Aktualisieren der Runtime eines Clusters](service-fabric-tutorial-upgrade-cluster.md)
> * [Bereitstellen von API Management mit Service Fabric](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Installieren Sie die [Service Fabric CLI](service-fabric-cli.md).
- Installieren Sie [Azure CLI 2.0](/cli/azure/install-azure-cli).

Mit den folgenden Verfahren wird ein Service Fabric-Cluster mit fünf Knoten erstellt. Zum Berechnen der Kosten, die durch das Ausführen eines Service Fabric-Clusters in Azure anfallen, verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/).

## <a name="key-concepts"></a>Wichtige Begriffe
Ein [Service Fabric-Cluster](service-fabric-deploy-anywhere.md) enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden. Cluster können auf Tausende von Computern skaliert werden. Ein Computer oder ein virtueller Computer, der Teil eines Clusters ist, wird als Knoten bezeichnet. Jeder Knoten erhält einen Knotennamen (Zeichenfolge). Knoten weisen Merkmale wie etwa Platzierungseigenschaften auf.

Ein Knotentyp definiert die Größe, Anzahl und Eigenschaften einer Gruppe virtueller Computer im Cluster. Jeder definierte Knotentyp wird als [VM-Skalierungsgruppe](/azure/virtual-machine-scale-sets/) eingerichtet, eine Azure-Computeressource, mit der Sie mehrere virtuelle Computer als Gruppe bereitstellen und verwalten können. Jeden Knotentyp kann dann unabhängig zentral hoch- oder herunterskaliert werden, bei jedem Typ können unterschiedliche Portgruppen geöffnet sein, und die Typen können verschiedene Kapazitätsmetriken aufweisen. Mit Knotentypen werden Rollen für eine Gruppe von Clusterknoten definiert, z.B. „Front-End“ oder „Back-End“.  Der Cluster kann über mehrere Knotentypen verfügen, aber der primäre Knotentyp muss bei Clustern, die in Produktionsumgebungen eingesetzt werden, mindestens fünf virtuelle Computer umfassen (für Testcluster sind mindestens drei virtuelle Computer erforderlich).  [Service Fabric-Systemdienste](service-fabric-technical-overview.md#system-services) werden auf den Knoten des primären Knotentyps platziert.

Der Cluster wird durch ein Clusterzertifikat geschützt. Ein Clusterzertifikat ist ein X.509-Zertifikat, das zum Sichern einer Knoten-zu-Knoten-Kommunikation und zur Authentifizierung der Endpunkte der Clusterverwaltung bei einem Verwaltungsclient verwendet wird.  Dieses Clusterzertifikat stellt auch SSL für die HTTPS-Verwaltungs-API und für Service Fabric Explorer über HTTPS bereit. Selbstsignierte Zertifikate sind für Testcluster hilfreich.  Verwenden Sie für Produktionscluster ein Zertifikat einer Zertifizierungsstelle als Clusterzertifikat.

Für das Clusterzertifikat muss Folgendes gelten:

- Es muss einen privaten Schlüssel enthalten.
- Es muss für den Schlüsselaustausch erstellt werden, um in eine PFX-Datei (Personal Information Exchange) exportiert werden zu können.
- Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die Sie auf den Service Fabric-Cluster zugreifen. Diese Übereinstimmung ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne „.cloudapp.azure.com“ können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, den Sie für Ihren Cluster verwenden.

Zertifikate für Service Fabric-Cluster in Azure werden in Azure Key Vault verwaltet.  Wenn ein Cluster in Azure bereitgestellt wird, ruft der für die Erstellung von Service Fabric-Clustern zuständige Azure-Ressourcenanbieter Zertifikate aus dem Schlüsseltresor ab und installiert sie auf den virtuellen Clustercomputern.

In diesem Tutorial wird ein Cluster mit fünf Knoten desselben Knotentyps bereitgestellt. Die [Kapazitätsplanung](service-fabric-cluster-capacity.md) ist aber ein wichtiger Schritt bei jeder Clusterbereitstellung in einer Produktionsumgebung. Nachfolgend sind einige Aspekte aufgeführt, die dabei berücksichtigt werden müssen.

- Erforderliche Anzahl von Knoten und Knotentypen für Ihren Cluster 
- Die Eigenschaften der einzelnen Knotentypen (z.B. Größe, primärer Knotentyp, Internetzugriff und Anzahl von virtuellen Computern)
- Die Zuverlässigkeits- und Dauerhaftigkeitsmerkmale des Clusters

## <a name="download-and-explore-the-template"></a>Herunterladen und Erkunden der Vorlage
Laden Sie die folgenden Resource Manager-Vorlagendateien herunter:
- [vnet-linuxcluster.json][template]
- [vnet-linuxcluster.parameters.json][parameters]

Mit [vnet-linuxcluster.json][template] wird eine bestimmte Anzahl von Ressourcen bereitgestellt, z.B.:

### <a name="service-fabric-cluster"></a>Service Fabric-Cluster
Es wird ein Linux-Cluster mit den folgenden Merkmalen bereitgestellt:
- Nur ein Knotentyp 
- Fünf Knoten für den primären Knotentyp (in Vorlagenparametern konfigurierbar)
- Betriebssystem: Ubuntu 16.04 LTS (in den Vorlagenparametern konfigurierbar)
- Geschütztes Zertifikat (in den Vorlagenparametern konfigurierbar)
- [DNS-Dienst](service-fabric-dnsservice.md) aktiviert
- [Dauerhaftigkeitsstufe](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) „Bronze“ (in den Vorlagenparametern konfigurierbar)
- [Zuverlässigkeitsstufe](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) „Silber“ (in den Vorlagenparametern konfigurierbar)
- Clientverbindungsendpunkt: 19000 (in den Vorlagenparametern konfigurierbar)
- HTTP-Gatewayendpunkt: 19080 (in den Vorlagenparametern konfigurierbar)

### <a name="azure-load-balancer"></a>Azure Load Balancer
Ein Load Balancer wird bereitgestellt, und für die folgenden Ports werden Tests und Regeln eingerichtet:
- Clientverbindungsendpunkt: 19000
- HTTP-Gatewayendpunkt: 19080 
- Anwendungsport: 80
- Anwendungsport: 443

### <a name="virtual-network-subnet-and-network-security-group"></a>Virtuelles Netzwerk, Subnetz und Netzwerksicherheitsgruppe
Die Namen des virtuellen Netzwerks, des Subnetzes und der Netzwerksicherheitsgruppe werden in den Vorlagenparametern deklariert.  Adressräume des virtuellen Netzwerks und Subnetzes werden auch in den Vorlagenparametern deklariert:
- Adressraum des virtuellen Netzwerks: 10.0.0.0/16
- Service Fabric-Subnetzadressraum: 10.0.2.0/24

Die folgenden Regeln für den eingehenden Datenverkehr sind in der Netzwerksicherheitsgruppe aktiviert. Sie können die Portwerte ändern, indem Sie die Vorlagenvariablen ändern.
- ClientConnectionEndpoint (TCP): 19000
- HttpGatewayEndpoint (HTTP/TCP): 19080
- SMB: 445
- Internodecommunication: 1025, 1026, 1027
- Kurzlebiger Portbereich: 49152 bis 65534 (mindestens 256 Ports)
- Ports für die Anwendungsverwendung: 80 und 443
- Portbereich für Anwendungen: 49152 bis 65534 (wird für die Kommunikation von Dienst zu Dienst verwendet, ansonsten kein Öffnen auf dem Load Balancer)
- Blockieren aller anderen Ports

Wenn keine anderen Anwendungsports benötigt werden, müssen Sie die Ressourcen „Microsoft.Network/loadBalancers“ und „Microsoft.Network/networkSecurityGroups“ anpassen, um Datenverkehr zuzulassen.

## <a name="set-template-parameters"></a>Festlegen von Vorlagenparametern
In der Parameterdatei [vnet-cluster.parameters.json][parameters] werden viele Werte deklariert, die zum Bereitstellen des Clusters und der dazugehörigen Ressourcen verwendet werden. Hier sind einige Parameter angegeben, die Sie für Ihre Bereitstellung ggf. ändern müssen:

|Parameter|Beispielwert|Notizen|
|---|---||
|adminUserName|vmadmin| Administratorbenutzername für die Cluster-VMs |
|adminPassword|Password#1234| Administratorkennwort für die Cluster-VMs|
|clusterName|mysfcluster123| Name des Clusters |
|location|southcentralus| Standort des Clusters |
|certificateThumbprint|| <p>Der Wert sollte leer sein, wenn ein selbstsigniertes Zertifikat erstellt oder eine Zertifikatsdatei bereitgestellt wird.</p><p>Geben Sie den Wert für den Zertifikatfingerabdruck ein, wenn Sie ein vorhandenes Zertifikat verwenden möchten, das zuvor in einen Schlüsseltresor hochgeladen wurde. Beispiel: „6190390162C988701DB5676EB81083EA608DCCF3“. </p>| 
|certificateUrlValue|| <p>Der Wert sollte leer sein, wenn ein selbstsigniertes Zertifikat erstellt oder eine Zertifikatsdatei bereitgestellt wird.</p><p>Geben Sie die Zertifikat-URL ein, wenn Sie ein vorhandenes Zertifikat verwenden möchten, das zuvor in einen Schlüsseltresor hochgeladen wurde. Beispiel: „https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346“.</p>|
|sourceVaultValue||<p>Der Wert sollte leer sein, wenn ein selbstsigniertes Zertifikat erstellt oder eine Zertifikatsdatei bereitgestellt wird.</p><p>Geben Sie den Wert für den Quelltresor ein, wenn Sie ein vorhandenes Zertifikat verwenden möchten, das zuvor in einen Schlüsseltresor hochgeladen wurde. Beispiel: „/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT“.</p>|


<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Bereitstellen des virtuellen Netzwerks und des Clusters
Richten Sie als Nächstes die Netzwerktopologie ein, und stellen Sie den Service Fabric-Cluster bereit. Mit der Resource Manager-Vorlage [vnet-linuxcluster.json][template] werden ein virtuelles Netzwerk (VNET) sowie ein Subnetz und eine Netzwerksicherheitsgruppe (NSG) für Service Fabric erstellt. Außerdem wird mit der Vorlage ein Cluster mit aktivierter Zertifikatsicherheit bereitgestellt.  Verwenden Sie für Produktionscluster ein Zertifikat einer Zertifizierungsstelle als Clusterzertifikat. Es kann ein selbstsigniertes Zertifikat zum Schützen von Testclustern verwendet werden.

Im folgenden Skript werden der Befehl [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) und eine Vorlage verwendet, um einen neuen Cluster bereitzustellen, der durch ein vorhandenes Zertifikat geschützt ist. Außerdem wird mit dem Befehl ein neuer Schlüsseltresor in Azure erstellt und Ihr Zertifikat hochgeladen.

```azurecli
ResourceGroupName="sflinuxclustergroup"
Location="southcentralus"  
Password="q6D7nN%6ck@6"
VaultName="linuxclusterkeyvault"
VaultGroupName="linuxclusterkeyvaultgroup"
CertPath="C:\MyCertificates\MyCertificate.pem"

# sign in to your Azure account and select your subscription
az login
az account set --subscription <guid>

# Create a new resource group for your deployment and give it a name and a location.
az group create --name $ResourceGroupName --location $Location

# Create the Service Fabric cluster.
az sf cluster create --resource-group $ResourceGroupName --location $Location \
   --certificate-password $Password --certificate-file $CertPath \
   --vault-name $VaultName --vault-resource-group $ResourceGroupName  \
   --template-file vnet-linuxcluster.json --parameter-file vnet-linuxcluster.parameters.json
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


[template]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-linuxcluster.json
[parameters]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-linuxcluster.parameters.json
