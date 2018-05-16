---
title: Bereitstellen von VM-Skalierungsgruppen in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Cloudbediener VM-Skalierungsgruppen zum Azure Stack-Marketplace hinzufügen können.
services: azure-stack
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: kivenkat
ms.openlocfilehash: 12425ab53ca16bb985a0a8658b5058998565b01a
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Bereitstellen von VM-Skalierungsgruppen in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

VM-Skalierungsgruppen sind eine Azure Stack-Computeressource. Verwenden Sie sie zum Bereitstellen und Verwalten eines Satzes identischer virtueller Computer. Da alle virtuellen Computer gleich konfiguriert sind, ist bei Skalierungsgruppen keine Vorabbereitstellung der virtuellen Computer erforderlich. Es ist einfacher, umfangreiche Dienste zu erstellen, die auf hohe Rechenleistung, Big Data und Workloads in Containern ausgelegt sind.

Dieser Artikel beschreibt, wie Skalierungsgruppen im Azure Stack-Marketplace verfügbar gemacht werden. Wenn Sie dieses Verfahren abgeschlossen haben, können Ihre Benutzer VM-Skalierungsgruppen zu ihren Abonnements hinzufügen.

VM-Skalierungsgruppen in Azure Stack sind wie VM-Skalierungsgruppen in Azure. Weitere Informationen finden Sie in den folgenden Videos:
* [Mark Russinovich spricht über Azure-Skalierungsgruppen](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [VM-Skalierungsgruppen mit Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

In Azure Stack unterstützen VM-Skalierungsgruppen keine automatische Skalierung. Sie können einer Skalierungsgruppe mithilfe des Azure Stack-Portals, Resource Manager-Vorlagen oder PowerShell weitere Instanzen hinzufügen.

## <a name="prerequisites"></a>Voraussetzungen
* **PowerShell und Tools**

   PowerShell muss installiert und für Azure Stack und die Azure Stack-Tools konfiguriert sein. Weitere Informationen finden Sie unter [Einrichten von PowerShell in Azure Stack](azure-stack-powershell-configure-quickstart.md).

   Importieren Sie nach der Installation der Azure Stack-Tools das folgende PowerShell-Modul (Pfad relativ zum Ordner „.\ComputeAdmin“ im Ordner „AzureStack-Tools-master“):
  ````PowerShell
        Import-Module .\AzureStack.ComputeAdmin.psm1
  ````

* **Betriebssystemimage**

   Wenn Sie Ihrem Azure Stack-Marketplace kein Betriebssystemimage hinzugefügt haben, finden Sie unter [Add the Windows Server 2016 VM image to the Azure Stack marketplace](azure-stack-add-default-image.md) (Hinzufügen des VM-Images für Windows Server 2016 zum Azure Stack-Marketplace) weitere Informationen.

   Wenn Sie Unterstützung für Linux benötigen, laden Sie Ubuntu Server 16.04 herunter, und fügen Sie es mithilfe von ```Add-AzsPlatformImage``` mit den folgenden Parametern hinzu: ```-publisher "Canonical" -offer "UbuntuServer" -sku "16.04-LTS"```.


## <a name="add-the-virtual-machine-scale-set"></a>Hinzufügen der VM-Skalierungsgruppe

Bearbeiten Sie das folgende PowerShell-Skript für Ihre Umgebung, und führen Sie es anschließend aus, um eine VM-Skalierungsgruppe zu Ihrem Azure Stack-Marketplace hinzuzufügen. 

``$User`` ist das Konto, das zum Herstellen einer Verbindung mit dem Administratorportal verwendet wird. Beispiel: serviceadmin@contoso.onmicrosoft.com.

````PowerShell  
$Arm = "https://adminmanagement.local.azurestack.external"
$Location = "local"

Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint $Arm

$Password = ConvertTo-SecureString -AsPlainText -Force "<your Azure Stack administrator password>"

$User = "<your Azure Stack service administrator user name>"

$Creds =  New-Object System.Management.Automation.PSCredential $User, $Password

$AzsEnv = Get-AzureRmEnvironment AzureStackAdmin
$AzsEnvContext = Add-AzureRmAccount -Environment $AzsEnv -Credential $Creds

Select-AzureRmSubscription -SubscriptionName "Default Provider Subscription"

Add-AzsVMSSGalleryItem -Location $Location
````

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Aktualisieren von Images in einer VM-Skalierungsgruppe 
Nach der Erstellung einer VM-Skalierungsgruppe können Benutzer Images in der Skalierungsgruppe aktualisieren, ohne dass diese neu erstellt werden muss. Die Vorgehensweise zum Aktualisieren eines Images hängt von den folgenden Szenarien ab:

1. In der Bereitstellungsvorlage der VM-Skalierungsgruppe ist **latest** für *version* angegeben:  

   Wenn im Abschnitt *imageReference* der Vorlage für eine Skalierungsgruppe *version* auf **latest** festgelegt ist, nutzen Vorgänge zum zentralen Hochskalieren für die Skalierungsgruppe die neueste verfügbare Version des Images für die Skalierungsgruppeninstanzen. Nach dem Abschluss eines Vorgangs zum zentralen Hochskalieren können Sie ältere VM-Skalierungsgruppeninstanzen löschen.  (Die Werte für *publisher*, *offer* und *sku* bleiben unverändert). 

   Hier sehen Sie ein Beispiel für die Angabe von *latest*:  

    ```Json  
    "imageReference": {
        "publisher": "[parameters('osImagePublisher')]",
        "offer": "[parameters('osImageOffer')]",
        "sku": "[parameters('osImageSku')]",
        "version": "latest"
        }
    ```

   Bevor beim zentralen Hochskalieren ein neues Image verwendet werden kann, müssen Sie dieses Image herunterladen:  

   - Wenn das Image im Marketplace neuer ist als das Image in der Skalierungsgruppe: Laden Sie das neue Image herunter, das das ältere Image ersetzt. Nachdem der Benutzer das Image ersetzt hat, kann er mit dem zentralen Hochskalieren fortfahren. 

   - Wenn die Imageversion im Marketplace der Imageversion in der Skalierungsgruppe entspricht: Löschen Sie das in der Skalierungsgruppe verwendete Image, und laden Sie das neue Image herunter. In der Zeit zwischen dem Entfernen des ursprünglichen Images und dem Herunterladen des neuen Images können Sie nicht zentral hochskalieren. 
      
     Bei diesem Vorgang müssen Images neu syndiziert werden, die das in Version 1803 eingeführte Sparsedateiformat verwenden. 
 

2. In der Bereitstellungsvorlage der VM-Skalierungsgruppe ist **nicht** „latest“ für *version*, sondern eine Versionsnummer angegeben:  

     Wenn Sie ein Image mit einer neueren Version herunterladen (wodurch sich die verfügbare Version ändert), kann die Skalierungsgruppe nicht zentral hochskaliert werden. Dies ist beabsichtigt, da die in der Skalierungsgruppenvorlage angegebene Imageversion verfügbar sein muss.  

Weitere Informationen finden Sie unter [Einführung in virtuelle Azure Stack-Computer](.\user\azure-stack-compute-overview.md#operating-system-disks-and-images).  


## <a name="remove-a-virtual-machine-scale-set"></a>Entfernen einer VM-Skalierungsgruppe

Führen Sie zum Entfernen eines Katalogelements einer VM-Skalierungsgruppe den folgenden PowerShell-Befehl aus:

```PowerShell  
    Remove-AzsVMSSGalleryItem
````

> [!NOTE]
> Das Katalogelement wird unter Umständen nicht sofort entfernt. Möglicherweise müssen Sie das Portal mehrmals aktualisieren, bevor das Element aus dem Marketplace angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte
[Häufig gestellte Fragen zu Azure Stack](azure-stack-faq.md)