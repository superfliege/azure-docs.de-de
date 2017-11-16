---
title: Erstellen eines Service Fabric-Clusters in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Windows- oder Linux-Service Fabric-Cluster in Azure mithilfe von PowerShell erstellen.
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
ms.date: 10/03/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 217b9f2f0dfed5b095e1bac1c8146abf4753fadc
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-secure-cluster-in-azure-by-using-powershell"></a>Erstellen eines sicheren Clusters in Azure mithilfe von PowerShell
Dieser Artikel ist einer von vielen in einer Reihe von Tutorials, mit dem Sie mithilfe von Azure Service Fabric-Clustern sowie Containern eine .NET-Anwendung in die Cloud verschieben. In den folgenden Schritten lernen Sie, wie Sie einen Service Fabric-Cluster (Windows oder Linux) erstellen können, der in Azure ausgeführt wird. Wenn Sie fertig sind, verfügen Sie über einen sicheren Cluster, der in der Cloud ausgeführt wird und für den Sie Anwendungen bereitstellen können.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.
- Installieren Sie das [Service Fabric SDK](service-fabric-get-started.md).
- Installieren Sie das [Azure PowerShell-Modul Version 4.1 oder höher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). (Installieren Sie bei Bedarf [Azure PowerShell](/powershell/azure/overview), [oder führen Sie ein Update auf eine neuere Version aus](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0#update-azps).)


## <a name="create-a-service-fabric-cluster"></a>Erstellen von Service Fabric-Clustern

Dieses Skript erstellt einen Service Fabric-Vorschaucluster mit einem einzelnen Knoten. Der Cluster wird durch ein selbstsigniertes Zertifikat geschützt. Das Skript erstellt das Zertifikat zusammen mit dem Cluster und platziert dann das Zertifikat in einem Schlüsseltresor. Sie können Einzelknotencluster nicht über einen virtuellen Computer hinaus skalieren, und Sie können Vorschaucluster auch nicht auf neuere Versionen upgraden.

Zum Berechnen der Kosten, die durch das Ausführen eines Service Fabric-Clusters in Azure anfallen, verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/).
Weitere Informationen zum Erstellen von Service Fabric-Clustern finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="log-in-to-azure"></a>Anmelden an Azure
Öffnen Sie eine PowerShell-Konsole, melden Sie sich bei Azure an, und wählen Sie das Abonnement aus, in dem Sie den Cluster bereitstellen möchten:

   ```PowerShell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```

## <a name="cluster-parameters"></a>Clusterparameter

   Dieses Skript verwendet die folgenden Parameter und Konzepte. Passen Sie die Parameter an Ihre Anforderungen an.

   | Parameter       | Beschreibung | Empfohlener Wert |
   | --------------- | ----------- | --------------- |
   | Standort | Die Azure-Region, in der Sie den Cluster bereitgestellt haben. | Beispielsweise *Europa, Westen*, *Asien, Osten* oder *USA, Osten* |
   | Name     | Der Name des Clusters, den Sie erstellen möchten. Der Name muss zwischen 4 und 23 Zeichen lang sein und darf nur Kleinbuchstaben, Zahlen sowie Bindestriche enthalten. | Beispielsweise *bobs-sfpreviewcluster* |
   | ResourceGroupName   | Der Name der Ressourcengruppe, in der der Cluster erstellt werden soll, | beispielsweise *meineressourcengruppe*. |
   | VmSku  | Die SKU eines virtuellen Computers, die für die Knoten verwendet wird. | Eine beliebige gültige SKU eines virtuellen Computers |
   | Betriebssystem  | Betriebssystem eines virtuellen Computers, das für die Knoten verwendet wird. | Ein beliebiges gültiges Betriebssystem eines virtuellen Computers |
   | KeyVaultName | Der Name des neuen Schlüsseltresors, der dem Cluster zugeordnet ist. | Beispielsweise *mykeyvault* |
   | ClusterSize | Die Anzahl der virtuellen Computer in Ihrem Cluster (kann *1* oder *3–99* sein).| Gibt für einen Vorschaucluster nur einen einzelnen virtuellen Computer an |
   | CertificateSubjectName | Der Antragstellername des Zertifikats, das erstellt werden soll | Entspricht dem Format *<name>*.*<location>*.cloudapp.azure.com |

### <a name="default-parameter-values"></a>Standardwerte für Parameter
**Virtueller Computer**: Optionale Einstellungen. Wenn Sie diese nicht angeben, ist der Benutzername des Administrators standardmäßig *vmadmin*, und PowerShell fragt Sie nach einem VM-Kennwort, bevor der Cluster erstellt wird.

**Ports**: standardmäßig die Ports 80 und 8081 Sie können zusätzliche Ports angeben, indem Sie den Leitfaden für [Ports in Service Fabric-Clustern](https://docs.microsoft.com/en-us/azure/service-fabric/create-load-balancer-rule) befolgen.

**Diagnosen**: standardmäßig aktiviert

**DNS-Dienst**: standardmäßig deaktiviert

**Reverseproxy**: standardmäßig deaktiviert

## <a name="create-the-cluster-with-your-parameters"></a>Erstellen des Clusters mit Ihren Parametern

Sobald Sie sich für die Parameter entschieden haben, die Ihren Anforderungen entsprechen, führen Sie den folgenden Befehl aus, um einen sicheren Service Fabric-Cluster und dessen Zertifikat zu erstellen.

Sie können dieses Skript ändern, um zusätzliche Parameter einzuschließen. Weitere Informationen zu Parametern für die Clustererstellung finden Sie im Cmdlet [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/new-azurermservicefabriccluster.md).

>[!NOTE]
>Sie müssen vor der Ausführung dieses Befehls einen Ordner erstellen, in dem das Clusterzertifikat gespeichert werden kann.

```PowerShell

# Set the certificate variables. This creates and encrypts a password that Service Fabric will use.
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force

# You must create the folder where you want to store the certificate on your machine before you start this step.
$certfolder="c:\mycertificates\"

# Set the variables for common values. Change the values to fit your needs.
$clusterloc="WestUS"
$clustername = "mysfcluster"
$groupname="mysfclustergroup"       
$vmsku = "Standard_D2_v2"
$vaultname = "mykeyvault"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Set the number of cluster nodes. The possible values are 1 and 3-99.
$clustersize=1

# Create the Service Fabric cluster and its self-signed certificate. The OS you specify here lets you use this cluster with any applications that are also using containers.
New-AzureRmServiceFabricCluster -Name $clustername -ResourceGroupName $groupname -Location $clusterloc `
-ClusterSize $clustersize -CertificateSubjectName $subname `
-CertificatePassword $certpwd -CertificateOutputFolder $certfolder `
-OS WindowsServer2016DatacenterwithContainers -VmSku $vmsku -KeyVaultName $vaultname
```

Der Erstellungsvorgang kann einige Minuten in Anspruch nehmen. Nach Abschluss des Konfigurationsvorgangs werden Informationen zum in Azure erstellten Cluster ausgegeben. Darüber hinaus wird das Clusterzertifikat in das Verzeichnis „-CertificateOutputFolder“ an dem Pfad kopiert, den Sie für diesen Parameter angegeben haben.

Notieren Sie sich die URL **ManagementEndpoint** für Ihren Cluster. Diese kann in etwa wie folgt aussehen: https://mycluster.westeurope.cloudapp.azure.com:19080.

## <a name="import-the-certificate"></a>Importieren des Zertifikats

Wenn der Cluster erfolgreich erstellt wurde, führen Sie den folgenden Befehl aus, um sicherzustellen, dass Sie das selbstsignierte Zertifikat verwenden können:

```PowerShell

# To connect to the cluster, install the certificate into the Personal (My) store of the current user on your computer.
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
-FilePath C:\mycertificates\mysfclustergroup20170531104310.pfx `
-Password $certpwd
```

Mit diesem Befehl wird das Zertifikat auf dem aktuellen Benutzer Ihres Computers installiert. Dieses Zertifikat wird für den Zugriff auf Service Fabric Explorer und zum Anzeigen der Clusterintegrität benötigt.


## <a name="view-your-cluster-optional"></a>Anzeigen Ihres Clusters (optional)

Sobald Sie über den Cluster und das importierte Zertifikat verfügen, können Sie eine Verbindung zum Cluster herstellen und dessen Integrität anzeigen. Es gibt mehrere Verbindungsmöglichkeiten, entweder über Service Fabric Explorer oder PowerShell.

### <a name="service-fabric-explorer"></a>Service Fabric Explorer
Sie können die Integrität Ihres Clusters über Service Fabric Explorer anzeigen. Navigieren Sie hierzu zur **ManagementEndpoint**-URL für Ihren Cluster, und wählen Sie dann das auf Ihrem Computer gespeicherte Zertifikat aus.

>[!NOTE]
>Beim Öffnen von Service Fabric Explorer wird ein Zertifikatfehler angezeigt, da Sie ein selbstsigniertes Zertifikat verwenden. In Edge müssen Sie auf **Details** und dann auf den Link **Webseite trotzdem laden** klicken. In Chrome müssen Sie auf **Advanced** (Erweitert) und dann auf den Link **proceed** (Fortfahren) klicken.

### <a name="powershell"></a>PowerShell

Das Service Fabric-PowerShell-Modul bietet viele Cmdlets zum Verwalten von Service Fabric-Clustern, Anwendungen und Diensten. Verwenden Sie das Cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster), um eine Verbindung mit dem Cluster herzustellen. Die Informationen zum Zertifikatfingerabdruck und zum Verbindungsendpunkt finden Sie in der Ausgabe aus einem vorherigen Schritt.

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
-KeepAliveIntervalInSec 10 `
-X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
-FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
-StoreLocation CurrentUser -StoreName My
```

Sie können auch mithilfe des Cmdlets [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) prüfen, ob Sie verbunden sind und der Cluster fehlerfrei ist.

```PowerShell
Get-ServiceFabricClusterHealth
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Sie einen sicheren Service Fabric-Cluster in Azure mithilfe von PowerShell erstellen.

Als Nächstes fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie eine vorhandene Anwendung bereitstellen:
> [!div class="nextstepaction"]
> [Bereitstellen einer .NET-Anwendung mit Docker Compose](service-fabric-host-app-in-a-container.md)

 
