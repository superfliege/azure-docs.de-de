---
title: Erstellen eines Azure Service Fabric-Clusters auf der Grundlage einer Vorlage| Microsoft-Dokumente
description: In diesem Artikel erfahren Sie, wie Sie unter Verwendung von Azure Resource Manager, Azure Key Vault und Azure Active Directory (Azure AD) für die Clientauthentifizierung einen sicheren Service Fabric-Cluster in Azure einrichten.
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: chackdan
ms.openlocfilehash: ebf4ebd563bab1395f32654bc71955a9416c7a5a
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager 
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure-Portal](service-fabric-cluster-creation-via-portal.md)
>
>

In dieser Schritt-für-Schritt-Anleitung wird die Einrichtung eines sicheren Azure Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager beschrieben. Der Artikel ist recht lang. Dennoch sollten Sie sich genau an die einzelnen Schritte halten, sofern Sie nicht bereits bestens mit dem Inhalt vertraut sind.

In diesem Leitfaden werden folgende Verfahren behandelt:

* Grundlegende Konzepte, die Sie vor dem Bereitstellen eines Service Fabric-Clusters kennen sollten
* Erstellen eines Clusters in Azure mithilfe der Service Fabric-Resource Manager-Module
* Einrichten von Azure Active Directory (Azure AD) zum Authentifizieren von Benutzern, die auf dem Cluster Verwaltungsvorgänge ausführen
* Erstellen und Bereitstellen einer benutzerdefinierten Azure Resource Manager-Vorlage für Ihren Cluster

## <a name="key-concepts-to-be-aware-of"></a>Wichtige Konzepte, die Sie kennen sollten
In Azure erfordert Service Fabric die Verwendung eines X.509-Zertifikats zum Schützen Ihres Clusters und seiner Endpunkte. Zertifikate werden in Service Fabric zur Authentifizierung und Verschlüsselung verwendet, um verschiedene Aspekte eines Clusters und der zugehörigen Anwendungen zu sichern. Für den Clientzugriff/zum Ausführen von Verwaltungsvorgängen auf dem Cluster, einschließlich Bereitstellen, Upgraden und Löschen von Anwendungen, Diensten und der darin enthaltenen Daten, können Sie Zertifikate oder Azure Active Directory-Anmeldeinformationen verwenden. Die Verwendung von Azure Active Directory wird dringend empfohlen, da dies die einzige Möglichkeit ist, um die Weitergabe von Zertifikaten auf Ihren Clients zu verhindern.  Weitere Informationen zur Verwendung von Zertifikaten in Service Fabric finden Sie unter [Szenarien für die Clustersicherheit in Service Fabric][service-fabric-cluster-security].

Service Fabric verwendet X.509-Zertifikate, um einen Cluster zu sichern und Sicherheitsfunktionen für Anwendungen bereitzustellen. [Key Vault][key-vault-get-started] dient zum Verwalten von Zertifikaten für Service Fabric-Cluster in Azure. 


### <a name="cluster-and-server-certificate-required"></a>Cluster- und Serverzertifikat (erforderlich)
Diese Zertifikate (ein primäres und optional ein sekundäres Zertifikat) sind erforderlich, um einen Cluster zu schützen und nicht autorisierten Zugriff zu verhindern. Es sorgt auf zwei Arten für Clustersicherheit:

* **Clusterauthentifizierung** : Authentifiziert die Kommunikation zwischen Knoten für einen Clusterverbund. Nur Knoten, die ihre Identität mit diesem Zertifikat nachweisen können, dürfen dem Cluster beitreten.
* **Serverauthentifizierung** : Authentifiziert die Verwaltungsendpunkte des Clusters bei einem Verwaltungsclient, sodass der Verwaltungsclient weiß, dass die Kommunikation wirklich mit dem Cluster und nicht mit einem „Man in the Middle“ erfolgt. Dieses Zertifikat stellt auch SSL für die HTTPS-Verwaltungs-API und für Service Fabric Explorer über HTTPS bereit.

Für diese Zwecke muss das Zertifikat die folgenden Anforderungen erfüllen:

* Das Zertifikat muss einen privaten Schlüssel enthalten. Diese Zertifikate weisen in der Regel die Erweiterung „.pfx“ oder „.pem“ auf.  
* Das Zertifikat muss für den Schlüsselaustausch erstellt werden, um in eine PFX-Datei (Personal Information Exchange; privater Informationsaustausch) exportiert werden zu können.
* Der **Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die Sie auf den Service Fabric-Cluster zugreifen**. Dies ist erforderlich, damit SSL für den HTTPS-Verwaltungsendpunkt des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne *.cloudapp.azure.com können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle (ZS) beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, den Sie für Ihren Cluster verwenden.

### <a name="set-up-azure-active-directory-for-client-authentication-optional-but-recommended"></a>Einrichten von Azure Active Directory für die Clientauthentifizierung (optional, aber empfohlen)

Mit Azure AD können Organisationen (so genannte Mandanten) den Benutzerzugriff auf Anwendungen verwalten. Bei den Anwendungen wird zwischen Anwendungen mit webbasierter Anmeldebenutzeroberfläche und Anwendungen mit nativer Clientumgebung unterschieden. In diesem Artikel wird davon ausgegangen, dass Sie bereits einen Mandanten erstellt haben. Falls nicht, sollten Sie sich zunächst mit dem Artikel [Einrichten eines Azure Active Directory-Mandanten][active-directory-howto-tenant] befassen.

Service Fabric-Cluster bieten unterschiedliche Einstiegspunkte für ihre Verwaltungsfunktionen. Hierzu zählen etwa der webbasierte [Service Fabric Explorer][service-fabric-visualizing-your-cluster] und [Visual Studio][service-fabric-manage-application-in-visual-studio]. Daher erstellen Sie zwei Azure AD-Anwendungen, um den Zugriff auf den Cluster zu steuern: eine Webanwendung und eine native Anwendung.

Weitere Informationen zum Einrichten finden Sie weiter unten in diesem Dokument.

### <a name="application-certificates-optional"></a>Anwendungszertifikate (optional)
Zum Zweck der Anwendungssicherheit kann eine beliebige Anzahl zusätzlicher Zertifikate in einem Cluster installiert werden. Bevor Sie den Cluster erstellen, betrachten Sie die verschiedenen Szenarien zur Anwendungssicherheit, in denen ein Zertifikat auf den Knoten installiert werden muss, beispielsweise:

* Verschlüsselung und Entschlüsselung von Anwendungskonfigurationswerten
* Knotenübergreifende Verschlüsselung von Daten während der Replikation

Das Konzept der Erstellung sicherer Cluster ist für Linux und Windows identisch. 

### <a name="client-authentication-certificates-optional"></a>Clientauthentifizierungszertifikate (optional)
Eine beliebige Anzahl weiterer Zertifikate kann für Administrator- oder Benutzerclientvorgänge angegeben werden. Standardmäßig hat das Clusterzertifikat Administratorrechte für den Client. Diese zusätzlichen Clientzertifikate dürfen nicht im Cluster installiert werden, sie müssen lediglich in der Clusterkonfiguration als zulässig festgelegt werden. Es ist jedoch erforderlich, sie auf den Clientcomputern zu installieren, um eine Verbindung mit dem Cluster herzustellen und Verwaltungsvorgänge auszuführen.


## <a name="prerequisites"></a>Voraussetzungen 
Das Konzept der Erstellung sicherer Cluster ist für Linux und Windows identisch. Dieser Leitfaden behandelt die Verwendung von Azure Powershell oder der Azure CLI zum Erstellen neuer Cluster. Die Voraussetzungen sind 

-  [Azure PowerShell 4.1 und höher][azure-powershell] oder [Azure CLI 2.0 und höher][azure-CLI].
-  Details zu den Service Fabric-Modulen finden Sie unter [AzureRM.ServiceFabric](https://docs.microsoft.com/powershell/module/azurerm.servicefabric) und [az SF](https://docs.microsoft.com/cli/azure/sf?view=azure-cli-latest).


## <a name="use-service-fabric-rm-module-to-deploy-a-cluster"></a>Verwenden des Service Fabric RM-Moduls zum Bereitstellen eines Clusters

In diesem Dokument verwenden wir das Service Fabric-RM-Powershell- und das CLI-Modul zum Bereitstellen eines Clusters. Der Powershell oder CLI-Modulbefehl lässt mehrere Szenarien zu. Diese möchten wir uns hier einzeln ansehen. Wählen Sie das Szenario aus, das Ihre Anforderungen am besten erfüllt. 

- Erstellen eines neuen Clusters – mithilfe eines vom System generierten selbstsignierten Zertifikats
    - Verwenden einer Standardclustervorlage
    - Verwenden einer bereits vorhandenen Vorlage
- Erstellen eines neuen Clusters – mithilfe eines bereits vorhandenen Zertifikats
    - Verwenden einer Standardclustervorlage
    - Verwenden einer bereits vorhandenen Vorlage

### <a name="create-new-cluster----using-a-system-generated-self-signed-certificate"></a>Erstellen eines neuen Clusters – mithilfe eines vom System generierten selbstsignierten Zertifikats

Verwenden Sie den folgenden Befehl zum Erstellen des Clusters, wenn das System ein selbstsigniertes Zertifikat generieren und zum Schützen des Clusters verwenden soll. Dieser Befehl richtet ein primäres Clusterzertifikat ein, das für die Clustersicherheit und zum Einrichten von Administratorzugriff verwendet wird, um mithilfe dieses Zertifikats Verwaltungsvorgänge auszuführen.

### <a name="login-in-to-azure"></a>Anmelden bei Azure

```Powershell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```
#### <a name="use-the-default-5-node-1-nodetype-template-that-ships-in-the-module-to-set-up-the-cluster"></a>Verwenden der im Modul inbegriffenen Standardvorlage „5 Node 1 nodetype“ zum Einrichten des Clusters

Verwenden Sie den folgenden Befehl, um einen Cluster schnell und unter Angabe der minimal erforderlichen Parameter zu erstellen.

Die Vorlage, die verwendet wird, ist unter den [Azure Service Fabric-Vorlagenbeispielen: Windows-Vorlage](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) und [Ubuntu-Vorlage](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure) verfügbar.

Die folgenden Befehle funktionieren zum Erstellen von Windows- und Linux-Clustern. Sie müssen nur das Betriebssystem entsprechend angeben. Die PowerShell-/CLI-Befehle geben außerdem das Zertifikat im angegebenen CertificateOutputFolder-Ordner zurück. Stellen Sie daher sicher, dass der Zertifikatordner bereits erstellt ist. Der Befehl akzeptiert auch andere Parameter wie VM-SKU.

```Powershell

$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password123!@#" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword="Password4321!@#" | ConvertTo-SecureString -AsPlainText -Force
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"
$certOutputFolder="c:\certificates"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -OS $os -VmPassword $vmpassword -VmUserName $vmuser

```

```CLI

declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare CertSubjectName="mylinux.westus.cloudapp.azure.com"
declare vmpassword="Password!1"
declare certpassword="Password!4321"
declare vmuser="myadmin"
declare vmOs="UbuntuServer1604"
declare certOutputFolder="c:\certificates"



az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certpassword  \
    --vault-name $vaultName --vault-resource-group $resourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath --vm-os $vmOs  \
    --vm-password $vmpassword --vm-user-name $vmuser

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Verwenden einer bereits vorhandenen benutzerdefinierten Vorlage 

Wenn Sie eine benutzerdefinierte Vorlage für Ihre speziellen Anforderungen erstellen müssen, wird dringend empfohlen, dass Sie mit einer der Vorlagen beginnen, die unter den [Azure Service Fabric-Vorlagenbeispielen](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) verfügbar sind. Beachten Sie die Anleitung und Erklärungen zum [Anpassen Ihrer Clustervorlage][customize-your-cluster-template] im folgenden Abschnitt.

Wenn Sie bereits über eine benutzerdefinierte Vorlage verfügen, überprüfen Sie, ob alle drei zertifikatbezogenen Parameter in der Vorlage und der Parameterdatei wie folgt benannt wurden und die Werte wie folgt NULL sind.

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```PowerShell


$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```

Im Folgenden sehen Sie den entsprechenden CLI-Befehl hierfür. Ändern Sie die Werte in den declare-Anweisungen in die entsprechenden Werte. Die CLI unterstützt alle anderen Parameter, die vom oben genannten PowerShell-Befehl unterstützt werden.

```CLI

declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath

```


### <a name="create-new-cluster---using-the-certificate-you-bought-from-a-ca-or-you-already-have"></a>Erstellen eines neuen Clusters – mithilfe eines von einer ZS gekauften oder bereits vorhandenen Zertifikats

Verwenden Sie den folgenden Befehl zum Erstellen eines Clusters, wenn Sie über ein Zertifikat verfügen, das Sie zum Schützen des Clusters verwenden möchten.

Wenn dies ein von einer ZS signiertes Zertifikat ist, das Sie letztendlich auch für andere Zwecke einsetzen möchten, dann empfiehlt es sich, eine unterschiedliche Ressourcengruppe speziell für Ihren Schlüsseltresor bereitzustellen. Es empfiehlt sich, den Schlüsseltresor in einer eigenen Ressourcengruppe zu platzieren. Dadurch können Sie die Compute- und Speicherressourcengruppen (einschließlich der Ressourcengruppe mit Ihrem Service Fabric-Cluster) entfernen, ohne Ihre Schlüssel und geheimen Schlüssel zu verlieren. **Die Ressourcengruppe mit Ihrem Schlüsseltresor _muss sich in der gleichen Region_ befinden wie der Cluster, der den Tresor verwendet.**


#### <a name="use-the-default-5-node-1-nodetype-template-that-ships-in-the-module"></a>Verwenden der im Modul inbegriffenen Standardvorlage „5 Node 1 nodetype“
Die Vorlage, die verwendet wird, ist unter den [Azure-Beispielen: Windows-Vorlage](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) und [Ubuntu-Vorlage](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure) verfügbar.

```PowerShell

$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword=("Password!4321" | ConvertTo-SecureString -AsPlainText -Force) 
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile C:\MyCertificates\chackocertificate3.pfx -CertificatePassword $certPassword -OS $os -VmPassword $vmpassword -VmUserName $vmuser 

```

```CLI

declare vmPassword="Password!1"
declare certPassword="Password!1"
declare vmUser="myadmin"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare certificate-file="c:\certificates\mycert.pem"
declare vmOs="UbuntuServer1604"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $certPassword  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --vm-os vmOs \
    --vm-password $vmPassword --vm-user-name $vmUser

```

#### <a name="use-the-custom-template-that-you-have"></a>Verwenden einer vorhandenen benutzerdefinierten Vorlage 
Wenn Sie eine benutzerdefinierte Vorlage für Ihre speziellen Anforderungen erstellen müssen, wird dringend empfohlen, dass Sie mit einer der Vorlagen beginnen, die unter den [Azure Service Fabric-Vorlagenbeispielen](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) verfügbar sind. Beachten Sie die Anleitung und Erklärungen zum [Anpassen Ihrer Clustervorlage][customize-your-cluster-template] im folgenden Abschnitt.

Wenn Sie bereits über eine benutzerdefinierte Vorlage verfügen, überprüfen Sie, ob alle drei zertifikatbezogenen Parameter in der Vorlage und der Parameterdatei wie folgt benannt wurden und die Werte wie folgt NULL sind.

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```PowerShell

$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$os="WindowsServer2016DatacenterwithContainers"
$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"
$certificateFile="C:\MyCertificates\chackonewcertificate3.pem"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -TemplateFile $templateFilePath -ParameterFile $parameterFilePath -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile $certificateFile -CertificatePassword $certPassword

```

Im Folgenden sehen Sie den entsprechenden CLI-Befehl hierfür. Ändern Sie die Werte in den declare-Anweisungen in die entsprechenden Werte.

```CLI

declare certPassword="Password!1"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $password  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 
```

#### <a name="use-a-pointer-to-the-secret-you-already-have-uploaded-into-the-key-vault"></a>Verwenden eines Zeigers auf den bereits in den Schlüsseltresor hochgeladenen geheimen Schlüssel

Wenn Sie einen bereits vorhandenen Schlüsseltresor verwenden möchten, _muss er für die Bereitstellung aktiviert werden_, damit der Compute-Ressourcenanbieter Zertifikate daraus abrufen und ihn auf Clusterknoten installieren kann:

```PowerShell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment


$parameterFilePath="c:\mytemplates\mytemplate.json"
$templateFilePath="c:\mytemplates\mytemplateparm.json"
$secretID="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroup -SecretIdentifier $secretId -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```
Im Folgenden sehen Sie den entsprechenden CLI-Befehl hierfür. Ändern Sie die Werte in den declare-Anweisungen in die entsprechenden Werte.

```CLI
declare $resourceGroupName = "testRG"
declare $parameterFilePath="c:\mytemplates\mytemplate.json"
declare $templateFilePath="c:\mytemplates\mytemplateparm.json"
declare $secertId="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --secret-identifier az $secretID  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 

```

<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Einrichten von Azure Active Directory für die Clientauthentifizierung

Mit Azure AD können Organisationen (so genannte Mandanten) den Benutzerzugriff auf Anwendungen verwalten. Bei den Anwendungen wird zwischen Anwendungen mit webbasierter Anmeldebenutzeroberfläche und Anwendungen mit nativer Clientumgebung unterschieden. In diesem Artikel wird davon ausgegangen, dass Sie bereits einen Mandanten erstellt haben. Falls nicht, sollten Sie sich zunächst mit dem Artikel [Einrichten eines Azure Active Directory-Mandanten][active-directory-howto-tenant] befassen.

Service Fabric-Cluster bieten unterschiedliche Einstiegspunkte für ihre Verwaltungsfunktionen. Hierzu zählen etwa der webbasierte [Service Fabric Explorer][service-fabric-visualizing-your-cluster] und [Visual Studio][service-fabric-manage-application-in-visual-studio]. Daher erstellen Sie zwei Azure AD-Anwendungen, um den Zugriff auf den Cluster zu steuern: eine Webanwendung und eine native Anwendung.

Wir haben eine Reihe von Windows PowerShell-Skripts erstellt, um einige Schritte der Konfiguration von Azure AD mit einem Service Fabric-Cluster zu vereinfachen.

> [!NOTE]
> Vor der Clustererstellung müssen folgende Schritte ausgeführt werden. Da in den Skripts Clusternamen und Endpunkte erwartet werden, sollte es sich bei den Werten nicht um bereits erstellte, sondern um geplante Werte handeln.

1. [Laden Sie die Skripts auf Ihren Computer herunter][sf-aad-ps-script-download].
2. Klicken Sie mit der rechten Maustaste auf die ZIP-Datei, wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Entsperren**, und klicken Sie anschließend auf **Übernehmen**.
3. Extrahieren Sie die ZIP-Datei.
4. Führen Sie `SetupApplications.ps1` aus, und geben Sie „TenantId“, „ClusterName“ und „WebApplicationReplyUrl“ als Parameter an. Beispiel: 

```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'

```

Die Mandanten-ID (TenantId) können Sie mithilfe des PowerShell-Befehls `Get-AzureSubscription` ermitteln. Wenn Sie diesen Befehl ausführen, wird die Mandanten-ID für jedes Abonnement angezeigt.

„ClusterName“ wird als Präfix für durch das Skript erstellte Azure AD-Anwendungen verwendet. Der Wert muss nicht exakt dem tatsächlichen Clusternamen entsprechen. Er dient lediglich zur Vereinfachung der Zuordnung von Azure AD-Artefakten zu dem Service Fabric-Cluster, mit dem sie verwendet werden.

„WebApplicationReplyUrl“ ist der Standardendpunkt, den Azure AD nach Abschluss des Anmeldeprozesses an Ihre Benutzer zurückgibt. Legen Sie diesen Endpunkt auf den Service Fabric Explorer-Endpunkt für Ihren Cluster fest. Dies ist standardmäßig folgender Wert:

https://&lt;Cluster-Domäne&gt;:19080/Explorer

Sie werden aufgefordert, sich bei einem Konto anzumelden, das über Administratorrechte für den Azure AD-Mandanten verfügt. Nach der Anmeldung erstellt das Skript die Webanwendung und die native Anwendung, die Ihren Service Fabric-Cluster darstellen. Wenn Sie sich die Anwendungen des Mandanten im [Azure-Portal][azure-portal] ansehen, sollten zwei neue Einträge angezeigt werden:

   * *Clustername*\_Cluster
   * *Clustername*\_Client

Das Skript gibt den für die Azure Resource Manager-Vorlage erforderlichen JSON-Code aus, wenn Sie im nächsten Abschnitt den Cluster erstellen. Es empfiehlt sich daher, das PowerShell-Fenster geöffnet zu lassen.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

<a id="customize-arm-template" ></a>

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Erstellen einer Resource Manager-Vorlage für Service Fabric-Cluster
Dieser Abschnitt ist für Benutzer bestimmt, die eine Resource Manager-Vorlage für Service Fabric-Cluster benutzerdefiniert erstellen möchten. Wenn Sie eine Vorlage erstellt haben, können Sie wieder zurück navigieren und zum Bereitstellen die Powershell- oder CLI-Module verwenden. 

Resource Manager-Beispielvorlagen stehen in den [Azure-Beispielen auf GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates) zur Verfügung. Diese Vorlagen können als Ausgangspunkt für Ihre Clustervorlage verwendet werden.

### <a name="create-the-resource-manager-template"></a>Erstellen der Resource Manager-Vorlage
In diesem Leitfaden werden die Beispielvorlage für einen [Sicherheitscluster mit 5 Knoten][service-fabric-secure-cluster-5-node-1-nodetype] und die zugehörigen Vorlagenparameter verwendet. Laden Sie `azuredeploy.json` und `azuredeploy.parameters.json` auf Ihren Computer herunter, und öffnen Sie beide Dateien in einem Texteditor.

### <a name="add-certificates"></a>Hinzufügen von Zertifikaten
Um Zertifikate einer Resource Manager-Vorlage für einen Cluster hinzuzufügen, verweisen Sie auf den Schlüsseltresor mit den Zertifikatschlüsseln. Fügen Sie die Schlüsseltresorparameter und -werte zu einer Resource Manager-Vorlagenparameterdatei (azuredeploy.parameters.json) hinzu. 

#### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Hinzufügen aller Zertifikate zur VM-Skalierungsgruppe „osProfile“
Jedes im Cluster installierte Zertifikat muss im osProfile-Abschnitt der Skalierungsgruppenressource (Microsoft.Compute/virtualMachineScaleSets) konfiguriert werden. Dadurch wird der Ressourcenanbieter angewiesen, das Zertifikat auf den virtuellen Computern zu installieren. Die Installation umfasst sowohl das Clusterzertifikat als auch sämtliche Anwendungssicherheitszertifikate, die Sie für Ihre Anwendungen verwenden möchten:

```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-the-service-fabric-cluster-certificate"></a>Konfigurieren des Service Fabric-Clusterzertifikats
Das Zertifikat für die Clusterauthentifizierung muss sowohl in der Service Fabric-Clusterressource (Microsoft.ServiceFabric/clusters) als auch in der Service Fabric-Erweiterung für VM-Skalierungsgruppen in der VM-Skalierungsgruppenressource konfiguriert werden. Dadurch kann der Service Fabric-Ressourcenanbieter das Zertifikat für die Clusterauthentifizierung und die Serverauthentifizierung für Verwaltungsendpunkte konfigurieren.

##### <a name="add-the-certificate-information-the-virtual-machine-scale-set-resource"></a>Fügen Sie die Zertifikatinformationen in der VM-Skalierungsgruppenressource hinzu:
```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="add-the-certificate-information-to-the-service-fabric-cluster-resource"></a>Fügen Sie die Zertifikatinformationen zur Service Fabric-Clusterressource hinzu:
```json
{
  "apiVersion": "[variables('sfrpApiVersion')]",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Hinzufügen der Azure AD-Konfiguration zum Verwenden von Azure AD für den Clientzugriff

Fügen Sie die Azure AD-Konfiguration zu einer Resource Manager-Vorlage für einen Cluster hinzu, indem Sie auf den Schlüsseltresor mit den Zertifikatschlüsseln verweisen. Fügen Sie die Azure AD-Parameter und -werte zu einer Resource Manager-Vorlagenparameterdatei (azuredeploy.parameters.json) hinzu.

```json
{
  "apiVersion": "[variables('sfrpApiVersion')]",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### <a name="populate-the-parameter-file-with-the-values"></a>Füllen Sie die Parameterdatei mit den Werten auf.
Füllen Sie die Parameterdatei abschließend mit den Ausgabewerten aus den Schlüsseltresor- und Azure AD-PowerShell-Befehlen auf:

Wenn Sie die Azure Service Fabric-RM-PowerShell-Module verwenden möchten, dann ist es nicht erforderlich, die Clusterzertifikatinformationen aufzufüllen. Wenn das System das selbstsignierte Zertifikat für die Clustersicherheit erstellen soll, belassen Sie sie bei NULL. 

> [!NOTE]
> Für die RM-Module, die diese leeren Parameterwerte übernehmen und auffüllen sollen, müssen die Parameternamen mit den folgenden Namen übereinstimmen.
>

```json
        "clusterCertificateThumbprint": {
            "value": ""
        },
        "clusterCertificateUrlValue": {
            "value": ""
        },
        "sourceVaultvalue": {
            "value": ""
        },
```

Wenn Sie Anwendungszertifikate oder einen vorhandenen Cluster verwenden, den Sie in den Schlüsseltresor hochgeladen haben, rufen Sie diese Informationen ab und füllen Sie sie auf. 

Die RM-Module können die Azure AD-Konfiguration nicht für Sie erstellen. Wenn Sie Azure AD für den Clientzugriff verwenden möchten, müssen Sie es auffüllen.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```

### <a name="test-your-template"></a>Testen der Vorlage  
Verwenden Sie den folgenden PowerShell-Befehl, um Ihre Resource Manager-Vorlage mit einer Parameterdatei zu testen:

```PowerShell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Falls Probleme auftreten und Sie kryptische Nachrichten erhalten, verwenden Sie „-Debug“ als Option.

```PowerShell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json -Debug
```

Das folgende Diagramm veranschaulicht die Platzierung der Schlüsseltresor- und Azure AD-Konfiguration in der Resource Manager-Vorlage.

![Abhängigkeiten in Resource Manager][cluster-security-arm-dependency-map]

## <a name="create-the-cluster-using-azure-resource-template"></a>Erstellen des Clusters mithilfe einer Azure-Ressourcenvorlage 

Sie können Ihren Cluster jetzt mithilfe der zuvor beschriebenen Schritte bereitstellen. Wenn Sie die Parameterdatei mit den Werten aufgefüllt haben, können Sie den Cluster direkt mithilfe der [Azure-Ressourcenvorlagenbereitstellung][resource-group-template-deploy] erstellen.

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Falls Probleme auftreten und Sie kryptische Nachrichten erhalten, verwenden Sie „-Debug“ als Option.


<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Zuweisen von Benutzern zu Rollen
Nachdem Sie die Anwendungen für Ihren Cluster erstellt haben, müssen Ihre Benutzer den von Service Fabric unterstützten Rollen zugewiesen werden: „read-only“ (schreibgeschützt) und „admin“ (Administrator). Für die Rollenzuweisung können Sie das [Azure-Portal][azure-portal] verwenden.

1. Wählen Sie im Azure-Portal rechts oben Ihren Mandanten aus.

    ![Schaltfläche zur Auswahl des Mandanten][select-tenant-button]
2. Wählen Sie auf der Registerkarte links **Azure Active Directory** aus, und klicken Sie dann auf „Unternehmensanwendungen“.
3. Wählen Sie „Alle Anwendungen“ aus, und suchen Sie dann nach einer Webanwendung mit einem Namen wie `myTestCluster_Cluster`. Wählen Sie die Webanwendung aus.
4. Klicken Sie auf die Registerkarte **Benutzer und Gruppen**.

    ![Registerkarte „Benutzer und Gruppen“][users-and-groups-tab]
5. Klicken Sie auf der neuen Seite auf die Schaltfläche **Benutzer hinzufügen**, wählen Sie einen Benutzer und die zuzuweisende Rolle aus, und klicken Sie dann unten auf der Seite auf die Schaltfläche **Auswählen**.

    ![Seite zum Zuweisen von Benutzern zu Rollen][assign-users-to-roles-page]
6. Klicken Sie unten auf der Seite auf die Schaltfläche **Zuweisen**.

    ![Bestätigung der Zuweisung][assign-users-to-roles-confirm]

> [!NOTE]
> Weitere Informationen zu Rollen in Service Fabric finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md).
>
>


## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Problembehandlung beim Einrichten von Azure Active Directory
Das Einrichten und Verwenden von Azure AD ist unter Umständen recht schwierig. Im Folgenden finden Sie daher einige Hinweise, wie Sie Probleme beheben können.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer fordert Sie auf, ein Zertifikat auszuwählen.
#### <a name="problem"></a>Problem
Nach erfolgreicher Anmeldung bei Azure AD über Service Fabric Explorer zeigt der Browser wieder die Startseite an und fordert Sie auf, ein Zertifikat auszuwählen.

![SFX-Zertifikatdialogfeld][sfx-select-certificate-dialog]

#### <a name="reason"></a>Grund
Dem Benutzer ist in der Azure AD-Clusteranwendung keine Rolle zugewiesen. Daher tritt bei der Azure AD-Authentifizierung im Service Fabric-Cluster ein Fehler auf. Service Fabric Explorer greift auf die Zertifikatauthentifizierung zurück.

#### <a name="solution"></a>Lösung
Führen Sie die Schritte zum Einrichten von Azure AD aus, und weisen Sie Benutzerrollen zu. Darüber hinaus empfiehlt es sich, „Benutzerzuweisung für den Zugriff auf die App erforderlich“ zu aktivieren, wie dies bei `SetupApplications.ps1` der Fall ist.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Beim Herstellen der Verbindung mit PowerShell tritt der Fehler „Die angegebenen Anmeldeinformationen sind ungültig.“ auf.
#### <a name="problem"></a>Problem
Wenn Sie die Verbindung mit dem Cluster über PowerShell und unter Verwendung des Sicherheitsmodus „AzureActiveDirectory“ herstellen, tritt nach erfolgreicher Anmeldung bei Azure AD folgender Verbindungsfehler auf: „Die angegebenen Anmeldeinformationen sind ungültig.“

#### <a name="solution"></a>Lösung
Siehe vorherige Lösung.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer gibt bei der Anmeldung den Fehler „AADSTS50011“ zurück.
#### <a name="problem"></a>Problem
Wenn Sie versuchen, sich in Service Fabric Explorer bei Azure AD anzumelden, wird der Fehler „AADSTS50011“ mit dem Hinweis zurückgegeben, dass die Antwortadresse &lt;URL&gt; nicht den für die Anwendung &lt;GUID&gt; konfigurierten Antwortadressen entspricht.

![Die SFX-Antwortadresse stimmt nicht überein.][sfx-reply-address-not-match]

#### <a name="reason"></a>Grund
Die Cluster(web)anwendung für Service Fabric Explorer versucht, sich bei Azure AD zu authentifizieren, und gibt im Rahmen der Anforderung die Rückgabe-URL für die Umleitung an. Die URL ist jedoch nicht in der Liste **ANTWORT-URL** der Azure AD-Anwendung enthalten.

#### <a name="solution"></a>Lösung
Klicken Sie auf der AAD-Seite auf „App-Registrierungen“, wählen Sie Ihre Clusteranwendung aus, und klicken Sie dann auf die Schaltfläche **Antwort-URLs**. Fügen Sie auf der Seite „Antwort-URLs“ die URL von Service Fabric Explorer zur Liste hinzu, oder ersetzen Sie eines der Elemente in der Liste. Speichern Sie anschließend Ihre Änderung.

![Antwort-URL für Webanwendung][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Herstellen der Clusterverbindung unter Verwendung der Azure AD-Authentifizierung über PowerShell
Verwenden Sie das folgende PowerShell-Befehlsbeispiel, um eine Service Fabric-Clusterverbindung herzustellen:

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Informationen zum Connect-ServiceFabricCluster-Cmdlet finden Sie unter [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Kann ich den gleichen Azure AD-Mandanten in mehreren Clustern verwenden?
Ja. Denken Sie jedoch daran, die URL von Service Fabric Explorer Ihrer Cluster(web)anwendung hinzuzufügen. Andernfalls funktioniert Service Fabric Explorer nicht.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Warum benötige ich ein Serverzertifikat, auch wenn Azure AD aktiviert ist?
„FabricClient“ und „FabricGateway“ führen eine wechselseitige Authentifizierung durch. Bei der Azure AD-Authentifizierung stellt die Azure AD-Integration die Identität des Clients für den Server bereit, und das Serverzertifikat dient zum Überprüfen der Serveridentität. Weitere Informationen zu Service Fabric-Zertifikaten finden Sie unter [X.509-Zertifikate und Service Fabric][x509-certificates-and-service-fabric].

## <a name="next-steps"></a>Nächste Schritte
Sie verfügen jetzt über einen sicheren Cluster, für den Azure Active Directory die Authentifizierung für die Verwaltung bereitstellt. Als Nächstes [stellen Sie eine Verbindung mit dem Cluster her](service-fabric-connect-to-secure-cluster.md) und erfahren, wie Sie [Anwendungsgeheimnisse verwalten](service-fabric-application-secret-management.md).


<!-- Links -->
[azure-powershell]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric
[customize-your-cluster-template]: service-fabric-cluster-creation-via-arm.md#create-a-service-fabric-cluster-resource-manager-template

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[select-tenant-button]: ./media/service-fabric-cluster-creation-via-arm/select-tenant-button.png
[users-and-groups-tab]: ./media/service-fabric-cluster-creation-via-arm/users-and-groups-tab.png
[assign-users-to-roles-page]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-page.png
[assign-users-to-roles-confirm]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-confirm.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png

