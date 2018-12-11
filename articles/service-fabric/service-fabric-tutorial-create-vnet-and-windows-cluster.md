---
title: Erstellen eines Service Fabric-Windows-Clusters in Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von PowerShell einen Service Fabric-Windows-Cluster in einem virtuellen Azure-Netzwerk und einer Netzwerksicherheitsgruppe bereitstellen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/27/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: a720bb906192731b8b636939e22b13a8e52bbe76
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52632890"
---
# <a name="tutorial-deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Tutorial: Bereitstellen eines Service Fabric-Windows-Clusters in einem virtuellen Azure-Netzwerk

Dieses Tutorial ist der erste Teil einer Serie. Es wird beschrieben, wie Sie einen Service Fabric-Cluster mit Windows in einem [virtuellen Azure-Netzwerk (VNET)](../virtual-network/virtual-networks-overview.md) und einer [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) bereitstellen, indem Sie PowerShell und eine Vorlage verwenden. Wenn Sie fertig sind, verfügen Sie über einen Cluster, der in der Cloud ausgeführt wird und für den Sie Anwendungen bereitstellen können.  Informationen zum Erstellen eines Linux-Clusters per Azure CLI finden Sie unter [Erstellen eines sicheren Linux-Clusters in Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

In diesem Tutorial wird ein Produktionsszenario beschrieben.  Wenn Sie schnell einen kleinen Cluster für Testzwecke erstellen möchten, helfen Ihnen die Informationen unter [Erstellen von Service Fabric-Clustern](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) weiter.

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
> * [Horizontales Herunter- oder Hochskalieren eines Clusters](service-fabric-tutorial-scale-cluster.md)
> * [Aktualisieren der Runtime eines Clusters](service-fabric-tutorial-upgrade-cluster.md)
> * [Löschen eines Clusters](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Installieren Sie das [Service Fabric SDK und das PowerShell-Modul](service-fabric-get-started.md).
* Installieren Sie das [Azure PowerShell-Modul Version 4.1 oder höher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

Mit den folgenden Verfahren wird ein Service Fabric-Cluster mit fünf Knoten erstellt. Zum Berechnen der Kosten, die durch das Ausführen eines Service Fabric-Clusters in Azure anfallen, verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/).

## <a name="key-concepts"></a>Wichtige Begriffe

Ein [Service Fabric-Cluster](service-fabric-deploy-anywhere.md) enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden. Cluster können auf Tausende von Computern skaliert werden. Ein Computer oder ein virtueller Computer, der Teil eines Clusters ist, wird als Knoten bezeichnet. Jeder Knoten erhält einen Knotennamen (Zeichenfolge). Knoten weisen Merkmale wie etwa Platzierungseigenschaften auf.

Ein Knotentyp definiert die Größe, Anzahl und Eigenschaften einer Gruppe virtueller Computer im Cluster. Jeder definierte Knotentyp wird als [VM-Skalierungsgruppe](/azure/virtual-machine-scale-sets/) eingerichtet, eine Azure-Computeressource, mit der Sie mehrere virtuelle Computer als Gruppe bereitstellen und verwalten können. Jeden Knotentyp kann dann unabhängig zentral hoch- oder herunterskaliert werden, bei jedem Typ können unterschiedliche Portgruppen geöffnet sein, und die Typen können verschiedene Kapazitätsmetriken aufweisen. Mit Knotentypen werden Rollen für eine Gruppe von Clusterknoten definiert, z.B. „Front-End“ oder „Back-End“.  Der Cluster kann über mehrere Knotentypen verfügen, aber der primäre Knotentyp muss bei Clustern, die in Produktionsumgebungen eingesetzt werden, mindestens fünf virtuelle Computer umfassen (für Testcluster sind mindestens drei virtuelle Computer erforderlich).  [Service Fabric-Systemdienste](service-fabric-technical-overview.md#system-services) werden auf den Knoten des primären Knotentyps platziert.

Der Cluster wird durch ein Clusterzertifikat geschützt. Ein Clusterzertifikat ist ein X.509-Zertifikat, das zum Sichern einer Knoten-zu-Knoten-Kommunikation und zur Authentifizierung der Endpunkte der Clusterverwaltung bei einem Verwaltungsclient verwendet wird.  Dieses Clusterzertifikat stellt auch SSL für die HTTPS-Verwaltungs-API und für Service Fabric Explorer über HTTPS bereit. Selbstsignierte Zertifikate sind für Testcluster hilfreich.  Verwenden Sie für Produktionscluster ein Zertifikat einer Zertifizierungsstelle als Clusterzertifikat.

Für das Clusterzertifikat muss Folgendes gelten:

* Es muss einen privaten Schlüssel enthalten.
* Es muss für den Schlüsselaustausch erstellt werden, um in eine PFX-Datei (Personal Information Exchange) exportiert werden zu können.
* Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die Sie auf den Service Fabric-Cluster zugreifen. Diese Übereinstimmung ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne „.cloudapp.azure.com“ können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, den Sie für Ihren Cluster verwenden.

Zertifikate für Service Fabric-Cluster in Azure werden in Azure Key Vault verwaltet.  Wenn ein Cluster in Azure bereitgestellt wird, ruft der für die Erstellung von Service Fabric-Clustern zuständige Azure-Ressourcenanbieter Zertifikate aus dem Schlüsseltresor ab und installiert sie auf den virtuellen Clustercomputern.

In diesem Tutorial wird ein Cluster mit fünf Knoten desselben Knotentyps bereitgestellt. Die [Kapazitätsplanung](service-fabric-cluster-capacity.md) ist aber ein wichtiger Schritt bei jeder Clusterbereitstellung in einer Produktionsumgebung. Nachfolgend sind einige Aspekte aufgeführt, die dabei berücksichtigt werden müssen.

* Erforderliche Anzahl von Knoten und Knotentypen für Ihren Cluster
* Die Eigenschaften der einzelnen Knotentypen (z.B. Größe, primärer Knotentyp, Internetzugriff und Anzahl von virtuellen Computern)
* Die Zuverlässigkeits- und Dauerhaftigkeitsmerkmale des Clusters

## <a name="download-and-explore-the-template"></a>Herunterladen und Erkunden der Vorlage

Laden Sie die folgenden Resource Manager-Vorlagendateien herunter:

* [azuredeploy.json][template]
* [azuredeploy.parameters.json][parameters]

Diese Vorlage stellt einen sicheren Cluster aus fünf virtuellen Computern und einem einzigen Knotentyp in einem virtuellen Netzwerk und einer Netzwerksicherheitsgruppe bereit.  Weitere Beispielvorlagen finden Sie auf [GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates).  [azuredeploy.json][template] stellt verschiedene Ressourcen bereit, einschließlich der folgenden.

### <a name="service-fabric-cluster"></a>Service Fabric-Cluster

In der Ressource **Microsoft.ServiceFabric/clusters** wird ein Windows-Cluster mit den folgenden Merkmalen konfiguriert:

* Nur ein Knotentyp
* Fünf Knoten für den primären Knotentyp (in Vorlagenparametern konfigurierbar)
* Betriebssystem: Windows Server 2016 Datacenter mit Containern (in den Vorlagenparametern konfigurierbar)
* Geschütztes Zertifikat (in den Vorlagenparametern konfigurierbar)
* [Reverseproxy](service-fabric-reverseproxy.md) aktiviert
* [DNS-Dienst](service-fabric-dnsservice.md) aktiviert
* [Dauerhaftigkeitsstufe](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) „Bronze“ (in den Vorlagenparametern konfigurierbar)
* [Zuverlässigkeitsstufe](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) „Silber“ (in den Vorlagenparametern konfigurierbar)
* Clientverbindungsendpunkt: 19000 (in den Vorlagenparametern konfigurierbar)
* HTTP-Gatewayendpunkt: 19080 (in den Vorlagenparametern konfigurierbar)

### <a name="azure-load-balancer"></a>Azure Load Balancer

In der Ressource **Microsoft.Network/loadBalancers** wird ein Lastenausgleich konfiguriert, und es werden Tests und Regeln für die folgenden Ports eingerichtet:

* Clientverbindungsendpunkt: 19000
* HTTP-Gatewayendpunkt: 19080
* Anwendungsport: 80
* Anwendungsport: 443
* Service Fabric-Reverseproxy: 19081

Falls andere Anwendungsports benötigt werden, müssen Sie die Ressourcen **Microsoft.Network/loadBalancers** und **Microsoft.Network/networkSecurityGroups** anpassen, um den eingehenden Datenverkehr zuzulassen.

### <a name="virtual-network-subnet-and-network-security-group"></a>Virtuelles Netzwerk, Subnetz und Netzwerksicherheitsgruppe

Die Namen des virtuellen Netzwerks, des Subnetzes und der Netzwerksicherheitsgruppe werden in den Vorlagenparametern deklariert.  Adressräume des virtuellen Netzwerks und des Subnetzes werden auch in den Vorlagenparametern deklariert und in der Ressource **Microsoft.Network/virtualNetworks** konfiguriert:

* Adressraum des virtuellen Netzwerks: 172.16.0.0/20
* Service Fabric-Subnetzadressraum: 172.16.2.0/23

Die folgenden Regeln für eingehenden Datenverkehr sind für die Ressource **Microsoft.Network/networkSecurityGroups** aktiviert. Sie können die Portwerte ändern, indem Sie die Vorlagenvariablen ändern.

* ClientConnectionEndpoint (TCP): 19000
* HttpGatewayEndpoint (HTTP/TCP): 19080
* SMB: 445
* Internodecommunication: 1025, 1026, 1027
* Kurzlebiger Portbereich: 49152 bis 65534 (mindestens 256 Ports)
* Ports für die Anwendungsverwendung: 80 und 443
* Portbereich für Anwendungen: 49152 bis 65534 (wird für die Kommunikation von Dienst zu Dienst verwendet, ansonsten kein Öffnen auf dem Load Balancer)
* Blockieren aller anderen Ports

Falls andere Anwendungsports benötigt werden, müssen Sie die Ressourcen **Microsoft.Network/loadBalancers** und **Microsoft.Network/networkSecurityGroups** anpassen, um den eingehenden Datenverkehr zuzulassen.

## <a name="set-template-parameters"></a>Festlegen von Vorlagenparametern

In der Parameterdatei [azuredeploy.parameters][parameters] werden viele Werte deklariert, die zum Bereitstellen des Clusters und der zugehörigen Ressourcen verwendet werden. Hier sind einige Parameter angegeben, die Sie für Ihre Bereitstellung ggf. ändern müssen:

|Parameter|Beispielwert|Notizen|
|---|---||
|adminUserName|vmadmin| Administratorbenutzername für die virtuellen Clustercomputer. [Benutzernamenanforderungen für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-username-requirements-when-creating-a-vm) |
|adminPassword|Password#1234| Administratorkennwort für die Cluster-VMs [Kennwortanforderungen für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm)|
|clusterName|mysfcluster123| Name des Clusters Darf nur Buchstaben und Zahlen enthalten. Kann 3 bis 23 Zeichen enthalten.|
|location|southcentralus| Standort des Clusters |
|certificateThumbprint|| <p>Der Wert sollte leer sein, wenn ein selbstsigniertes Zertifikat erstellt oder eine Zertifikatsdatei bereitgestellt wird.</p><p>Um ein vorhandenes, zuvor in einen Schlüsseltresor hochgeladenes Zertifikat zu verwenden, geben Sie den Wert für den SHA-1-Zertifikatfingerabdruck ein. Beispiel: „6190390162C988701DB5676EB81083EA608DCCF3“</p>zu erstellen und zu verwalten. |
|certificateUrlValue|| <p>Der Wert sollte leer sein, wenn ein selbstsigniertes Zertifikat erstellt oder eine Zertifikatsdatei bereitgestellt wird. </p><p>Geben Sie die Zertifikat-URL ein, wenn Sie ein vorhandenes Zertifikat verwenden möchten, das zuvor in einen Schlüsseltresor hochgeladen wurde. Beispiel: „https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346“.</p>|
|sourceVaultValue||<p>Der Wert sollte leer sein, wenn ein selbstsigniertes Zertifikat erstellt oder eine Zertifikatsdatei bereitgestellt wird.</p><p>Geben Sie den Wert für den Quelltresor ein, wenn Sie ein vorhandenes Zertifikat verwenden möchten, das zuvor in einen Schlüsseltresor hochgeladen wurde. Beispiel: „/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT“.</p>|

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Bereitstellen des virtuellen Netzwerks und des Clusters

Richten Sie als Nächstes die Netzwerktopologie ein, und stellen Sie den Service Fabric-Cluster bereit. Mit der Resource Manager-Vorlage [azuredeploy.json][template] werden ein virtuelles Netzwerk (VNET) sowie ein Subnetz und eine Netzwerksicherheitsgruppe (NSG) für Service Fabric erstellt. Außerdem wird mit der Vorlage ein Cluster mit aktivierter Zertifikatsicherheit bereitgestellt.  Verwenden Sie für Produktionscluster ein Zertifikat einer Zertifizierungsstelle als Clusterzertifikat. Es kann ein selbstsigniertes Zertifikat zum Schützen von Testclustern verwendet werden.

### <a name="create-a-cluster-using-an-existing-certificate"></a>Erstellen eines Clusters mit einem vorhandenen Zertifikat

Im folgenden Skript werden das [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster)-Cmdlet und eine Vorlage verwendet, um einen neuen Cluster in Azure bereitzustellen. Außerdem wird mit dem Cmdlet ein neuer Schlüsseltresor in Azure erstellt und Ihr Zertifikat hochgeladen.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$clustername = "mysfcluster123"  # must match the clusterName parameter in the template
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateFile $certpath
```

### <a name="create-a-cluster-using-a-new-self-signed-certificate"></a>Erstellen eines Clusters mit einem neuen selbstsignierten Zertifikat

Im folgenden Skript werden das [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster)-Cmdlet und eine Vorlage verwendet, um einen neuen Cluster in Azure bereitzustellen. Das Cmdlet erstellt auch einen neuen Schlüsseltresor in Azure, fügt diesem ein neues selbstsigniertes Zertifikat hinzu und lädt das Zertifikat an einen lokalen Speicherort herunter.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster123"
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateSubjectName $subname

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

Das **Service Fabric**-PowerShell-Modul bietet viele Cmdlets zum Verwalten von Service Fabric-Clustern, Anwendungen und Diensten.  Verwenden Sie das Cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster), um eine Verbindung mit dem Cluster herzustellen. Die Informationen zum SHA-1-Zertifikatfingerabdruck und zum Verbindungsendpunkt finden Sie in der Ausgabe des vorherigen Schritts.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
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

In den anderen Artikeln dieser Tutorialreihe wird der soeben erstellte Cluster verwendet. Wenn Sie nicht direkt mit dem nächsten Artikel fortfahren, sollten Sie [den Cluster löschen](service-fabric-cluster-delete.md), um anfallende Gebühren zu vermeiden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines VNET in Azure mithilfe von PowerShell
> * Erstellen eines Schlüsseltresors und Hochladen eines Zertifikats
> * Erstellen eines sicheren Service Fabric-Clusters in Azure mithilfe von PowerShell
> * Sichern des Clusters mit einem X.509-Zertifikat
> * Herstellen einer Verbindung mit dem Cluster über PowerShell
> * Entfernen eines Clusters

Fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie Ihren Cluster skalieren.
> [!div class="nextstepaction"]
> [Skalieren eines Clusters](service-fabric-tutorial-scale-cluster.md)

[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/5-VM-Windows-1-NodeTypes-Secure-NSG/azuredeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/5-VM-Windows-1-NodeTypes-Secure-NSG/azuredeploy.parameters.json
