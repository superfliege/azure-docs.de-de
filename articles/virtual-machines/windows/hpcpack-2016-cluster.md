---
title: HPC Pack 2016-Cluster in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Cluster mit HPC Pack 2016 in Azure bereitstellen.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 3dde6a68-e4a6-4054-8b67-d6a90fdc5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 03/09/2018
ms.author: danlep
ms.openlocfilehash: c26dd85d896445e19efb9906d953fd535fc1fb5c
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a>Bereitstellen eines HPC Pack 2016-Clusters in Azure

Befolgen Sie die in diesem Artikel beschriebenen Schritte, um einen [Microsoft HPC Pack 2016 Update 1](https://technet.microsoft.com/library/cc514029)-Cluster auf virtuellen Azure-Computern bereitzustellen. HPC Pack ist die kostenlose HPC-Lösung von Microsoft, die auf Microsoft Azure und Windows Server-Technologien basiert und eine breite Palette von HPC-Workloads unterstützt.

Verwenden Sie eine der [Azure Resource Manager-Vorlagen](https://github.com/MsHpcPack/HPCPack2016), um den HPC Pack 2016-Cluster bereitzustellen. Sie haben die Wahl zwischen verschiedenen Clustertopologien mit unterschiedlich vielen Clusterhauptknoten und Computeknoten verschiedener Typen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="pfx-certificate"></a>PFX-Zertifikat

Für einen Microsoft HPC Pack 2016-Cluster ist ein Personal Information Exchange-Zertifikat (PFX) erforderlich, um die Kommunikation zwischen den HPC-Knoten zu schützen. Das Zertifikat muss die folgenden Anforderungen erfüllen:

* Privater Schlüssel mit der Fähigkeit zum Schlüsselaustausch
* Schlüsselverwendung mit digitale Signatur und Schlüsselchiffrierung
* Erweiterte Schlüsselverwendung mit Clientauthentifizierung und die Serverauthentifizierung

Wenn Sie noch kein Zertifikat haben, das diese Anforderungen erfüllt, können Sie das Zertifikat bei einer Zertifizierungsstelle anfordern. Alternativ verwenden Sie die folgenden Befehle basierend auf dem Betriebssystem, auf dem Sie den Befehl ausführen, um das selbstsignierte Zertifikat zu generieren. Anschließend exportieren Sie das Zertifikat als kennwortgeschützte PFX-Datei mit privatem Schlüssel.

* **Für Windows 10 oder Windows Server 2016** führen Sie das integrierte PowerShell-Cmdlet **New-SelfSignedCertificate** wie folgt aus:

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* **Für ältere Betriebssysteme als Windows 10 oder Windows Server 2016** laden Sie den [Generator für selbst signierte Zertifikate](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) aus dem Microsoft Script Center herunter. Extrahieren Sie den Inhalt, und führen Sie die folgenden Befehle an einer PowerShell-Eingabeaufforderung aus:

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

Nachdem das Zertifikat im Speicher des aktuellen Benutzers erstellt wurde, verwenden Sie das Zertifikat-Snap-In, um das Zertifikat als kennwortgeschützte PFX-Datei mit privatem Schlüssel zu exportieren. Sie können das Zertifikat auch mit dem PowerShell-Cmdlet [Export-Pfxcertificate](/powershell/module/pkiclient/export-pfxcertificate?view=win10-ps) exportieren.

### <a name="upload-certificate-to-an-azure-key-vault"></a>Hochladen des Zertifikats in einen Azure-Schlüsseltresor

Laden Sie vor dem Bereitstellen des HPC-Clusters das PFX-Zertifikat in den [Azure Key Vault](../../key-vault/index.md) als Geheimnis hoch, und notieren Sie sich die folgenden Informationen für die Nutzung während der Bereitstellung: **Tresorname**, **Tresorressourcengruppe**, **Zertifikats-URL** und **Zertifikatfingerabdruck**.

Es folgt ein PowerShell-Beispielskript, um das Zertifikat hochzuladen, den Schlüsselspeicher zu erstellen und die erforderlichen Informationen zu generieren. Weitere Informationen zum Hochladen eines Zertifikats in einen Azure Key Vault finden Sie unter [Erste Schritte mit Azure Key Vault](../../key-vault/key-vault-get-started.md).

```powershell
#Give the following values
$VaultName = "mytestvault"
$SecretName = "hpcpfxcert"
$VaultRG = "myresourcegroup"
$location = "westus"
$PfxFile = "c:\Temp\mytest.pfx"
$Password = "yourpfxkeyprotectionpassword"
#Validate the pfx file
try {
    $pfxCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $PfxFile, $Password
}
catch [System.Management.Automation.MethodInvocationException]
{
    throw $_.Exception.InnerException
}
$thumbprint = $pfxCert.Thumbprint
$pfxCert.Dispose()
# Create and encode the JSON object
$pfxContentBytes = Get-Content $PfxFile -Encoding Byte
$pfxContentEncoded = [System.Convert]::ToBase64String($pfxContentBytes)
$jsonObject = @"
{
"data": "$pfxContentEncoded",
"dataType": "pfx",
"password": "$Password"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
#Create an Azure key vault and upload the certificate as a secret
$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
$rg = Get-AzureRmResourceGroup -Name $VaultRG -Location $location -ErrorAction SilentlyContinue
if($null -eq $rg)
{
    $rg = New-AzureRmResourceGroup -Name $VaultRG -Location $location
}
$hpcKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultRG -Location $location -EnabledForDeployment -EnabledForTemplateDeployment
$hpcSecret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secret
"The following Information will be used in the deployment template"
"Vault Name             :   $VaultName"
"Vault Resource Group   :   $VaultRG"
"Certificate URL        :   $($hpcSecret.Id)"
"Certificate Thumbprint :   $thumbprint"

```


## <a name="supported-topologies"></a>Unterstützte Topologien

Wählen Sie eine der [Azure Resource Manager-Vorlagen](https://github.com/MsHpcPack/HPCPack2016), um den HPC Pack 2016-Cluster bereitzustellen. Die folgenden Abbildungen zeigen die allgemeine Architektur von drei Beispielclustertopologien. Hochverfügbarkeitstopologien umfassen mehrere Clusterhauptknoten.

1. Hochverfügbarkeitscluster mit Active Directory-Domäne (AD)

    ![Hochverfügbarkeitscluster in einer AD-Domäne](./media/hpcpack-2016-cluster/haad.png)


2. Hochverfügbarkeitscluster ohne Active Directory-Domäne

    ![Hochverfügbarkeitscluster ohne AD-Domäne](./media/hpcpack-2016-cluster/hanoad.png)

3. Cluster mit einem einzelnen Hauptknoten

   ![Cluster mit einzelnem Hauptknoten](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a>Bereitstellen eines Clusters

Wählen Sie zum Erstellen eines Clusters eine Vorlage aus, und klicken Sie auf **In Azure bereitstellen**. Geben Sie für die Vorlage im [Azure-Portal](https://portal.azure.com) die Parameter an – wie in den folgenden Schritte beschrieben. Mit jeder Vorlage können alle Azure-Ressourcen erstellt werden, die für die HPC-Clusterinfrastruktur erforderlich sind. Zu den Ressourcen zählen virtuelle Azure-Netzwerke, öffentliche IP-Adressen, Load Balancer (nur für Hochverfügbarkeitscluster), Netzwerkschnittstellen, Verfügbarkeitsgruppen, Speicherkonten und virtuelle Computer.

### <a name="step-1-select-the-subscription-location-and-resource-group"></a>Schritt 1: Auswählen von Abonnement, Speicherort und Ressourcengruppe

**Abonnement** und **Speicherort** müssen den Angaben entsprechen, die Sie beim Hochladen Ihres PFX-Zertifikats gemacht haben (siehe Voraussetzungen). Es wird empfohlen, für die Bereitstellung eine andere **Ressourcengruppe** zu erstellen.

### <a name="step-2-specify-the-parameter-settings"></a>Schritt 2: Festlegen der Parametereinstellungen

Geben Sie Werte für die Vorlagenparameter ein, oder ändern Sie diese. Klicken Sie auf das Symbol neben jedem Parameter, um Hilfeinformationen anzuzeigen. Siehe dazu auch den Leitfaden für [verfügbare VM-Größen](sizes.md).

Geben Sie die Werte an, die Sie für die folgenden Parameter in den Voraussetzungen festgelegt haben: **Tresorname**, **Tresorressourcengruppe**, **Zertifikats-URL** und **Zertifikatfingerabdruck**.

### <a name="step-3-review-terms-and-create"></a>Schritt 3: Prüfen der Bedingungen und Erstellen
Prüfen Sie die mit der Vorlage verknüpften Geschäftsbedingungen. Wenn Sie diesen zustimmen, klicken Sie auf **Kaufen**, um den Bereitstellungsprozess zu starten.

Je nach Clustertopologie kann die Bereitstellung 30 Minuten oder länger dauern.

## <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster
1. Nachdem der HPC Pack-Cluster bereitgestellt wurde, navigieren Sie zum [Azure-Portal](https://portal.azure.com). Klicken Sie auf **Ressourcengruppen**, und suchen Sie nach der Ressourcengruppe, in der der Cluster bereitgestellt wurde. Sie können hier nun die Hauptknoten der virtuellen Computer sehen.

    ![Clusterhauptknoten im Portal](./media/hpcpack-2016-cluster/clusterhns.png)

2. Klicken Sie auf einen Hauptknoten (klicken Sie in einem Hochverfügbarkeitscluster auf einen beliebigen Hauptknoten). Unter **Übersicht** finden Sie die öffentliche IP-Adresse oder den vollständigen DNS-Namen des Clusters.

    ![Clusterverbindungseinstellungen](./media/hpcpack-2016-cluster/clusterconnect.png)

3. Klicken Sie auf **Verbinden**, um sich über Remotedesktop mit dem angegebenen Administratorbenutzernamen bei einem der Hauptknoten anzumelden. Wenn sich der von Ihnen bereitgestellte Cluster in einer Active Directory-Domäne befindet, weist der Benutzername das folgende Format auf: \<NamePrivateDomäne>\\\<Administratorbenutzername> (zum Beispiel „hpc.local\hpcadmin“).

## <a name="next-steps"></a>Nächste Schritte
* Senden Sie Aufträge an Ihren Cluster. Weitere Informationen finden Sie unter [Übermitteln von HPC-Aufträgen von einem lokalen Computer an einen in Azure bereitgestellten HPC Pack-Cluster](hpcpack-cluster-submit-jobs.md) und [Verwalten eines HPC Pack 2016-Clusters in Azure mithilfe von Azure Active Directory](hpcpack-cluster-active-directory.md).

