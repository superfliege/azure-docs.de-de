---
title: Bereitstellen einer VM von Ihren VHDs für den Azure Marketplace | Microsoft-Dokumentation
description: Dieser Artikel erläutert, wie Sie eine VM von einer in Azure bereitgestellten virtuellen Festplatte registrieren.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 2771549af29b3e717d117afb42de6db03fbee226
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639126"
---
# <a name="deploy-a-vm-from-your-vhds"></a>Bereitstellen eines virtuellen Computers auf Basis der VHDs

Dieser Artikel erläutert, wie Sie einen virtuellen Computer (VM) von einer in Azure bereitgestellten virtuellen Festplatte (VHD) registrieren.  Hier werden die erforderlichen Tools aufgeführt und erklärt, wie Sie diese verwenden, um ein Benutzer-VM-Image zu erstellen und dieses dann entweder über das [Microsoft Azure-Portal](https://ms.portal.azure.com/) oder mithilfe von PowerShell-Skripts in Azure bereitstellen. 

Nachdem Sie Ihre virtuellen Festplatten – die generalisierte Betriebssystem-VHD und ggf. Datenträger-VHDs – in Ihr Azure-Speicherkonto hochgeladen haben, können Sie die Festplatten als Benutzer-VM-Image registrieren. Anschließend können Sie das Image testen. Da Ihre Betriebssystem-VHD generalisiert wurde, können Sie den virtuellen Computer nicht direkt bereitstellen, indem Sie die VHD-URL angeben.

Weitere Informationen zu VM-Images finden Sie in den folgenden Blogbeiträgen:

- [VM-Image](https://azure.microsoft.com/blog/vm-image-blog-post/)
- [VM Image PowerShell How To](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/) (VM-Images in PowerShell – Vorgehensweisen)


## <a name="set-up-the-necessary-tools"></a>Einrichten der erforderlichen Tools

Falls Sie dies noch nicht getan haben, installieren Sie Azure PowerShell und die Azure CLI gemäß den folgenden Anweisungen:

<!-- TD: Change the following URLs (in this entire topic) to relative paths.-->

- [Installieren von Azure PowerShell unter Windows mit PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)
- [Installieren der Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)


## <a name="create-a-user-vm-image"></a>Erstellen eines Benutzer-VM-Images

Als Nächstes erstellen Sie aus Ihrer generalisierten virtuellen Festplatte ein nicht verwaltetes Image.

#### <a name="capture-the-vm-image"></a>Erfassen des VM-Images

Befolgen Sie die Anweisungen im folgenden Artikel zum Erfassen der VM, die Ihrer Zugriffsweise entspricht:

-  PowerShell: [Erstellen eines nicht verwalteten VM-Images aus einer Azure-VM](../../../virtual-machines/windows/capture-image-resource.md)
-  Azure CLI: [Vorgehensweise zum Erstellen eines Images von einem virtuellen Computer oder einer VHD](../../../virtual-machines/linux/capture-image.md)
-  API: [VMs – Erfassen](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)

### <a name="generalize-the-vm-image"></a>Generalisieren des VM-Images

Da Sie das Benutzerimage aus einer zuvor generalisierten virtuellen Festplatte generiert haben, sollte das Image ebenfalls generalisiert werden.  Wählen Sie aus der folgenden Liste denjenigen Artikel aus, der Ihrer Zugriffsweise entspricht.  (Möglicherweise haben Sie die Festplatte bereits beim Erfassen generalisiert.)

-  PowerShell: [Generalisieren des virtuellen Computers](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  Azure CLI: [Schritt 2: Erstellen des VM-Images](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [VMs – Generalisieren](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize)


## <a name="deploy-a-vm-from-a-user-vm-image"></a>Bereitstellen eines virtuellen Computers aus einem Benutzer-VM-Image

Als Nächstes stellen Sie im Azure-Portal oder über PowerShell einen virtuellen Computer aus einem Benutzer-VM-Image bereit.

<!-- TD: Recapture following hilited images and replace with red-box. -->

### <a name="deploy-a-vm-from-azure-portal"></a>Bereitstellen einer VM im Azure-Portal

Gehen Sie folgendermaßen vor, um Ihre Benutzer-VM im Azure-Portal bereitzustellen.

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2.  Klicken Sie auf **Neu**, suchen Sie nach **Vorlagenbereitstellung**, und wählen Sie dann **Eigene Vorlage im Editor erstellen** aus.  <br/>
  ![Erstellen einer Vorlage für die VHD-Bereitstellung im Azure-Portal](./media/publishvm_021.png)

3. Kopieren Sie diese [JSON-Vorlage](./cpp-deploy-json-template.md), fügen Sie sie in den Editor ein, und klicken Sie auf **Speichern**. <br/>
  ![Speichern der Vorlage für die VHD-Bereitstellung im Azure-Portal](./media/publishvm_022.png)

4. Geben Sie die Parameterwerte für die angezeigten Eigenschaftenseiten für die **benutzerdefinierte Bereitstellung** ein.

   <table> <tr> <td valign="top"> <img src="./media/publishvm_023.png" alt="Custom deployment property page 1"> </td> <td valign="top"> <img src="./media/publishvm_024.png" alt="Custom deployment property page 2"> </td> </tr> </table> <br/> 

   |  **Parameter**              |   **Beschreibung**                                                            |
   |  -------------              |   ---------------                                                            |
   | Name des Speicherkontos des Benutzers   | Name des Speicherkontos, in dem sich die generalisierte VHD befindet                    |
   | Name des Speichercontainers des Benutzers | Name des Containers, in dem sich die generalisierte VHD befindet                          |
   | DNS-Name für öffentliche IP-Adresse      | DNS-Name der öffentlichen IP-Adresse                                                           |
   | Administratorbenutzername             | Der Benutzername des Administratorkontos für die neue VM                                  |
   | Administratorkennwort              | Das Kennwort des Administratorkontos für die neue VM                                  |
   | OS Type (Betriebssystemtyp)                     | VM-Betriebssystem: `Windows` \| `Linux`                                    |
   | Abonnement-ID             | Bezeichner des ausgewählten Abonnements                                      |
   | Standort                    | Geografischer Standort der Bereitstellung                                        |
   | VM-Größe                     | [Größe des virtuellen Azure-Computers](https://docs.microsoft.com/azure/virtual-machines/windows/sizes), z.B. `Standard_A2` |
   | Öffentliche IP-Adresse      | Name der öffentlichen IP-Adresse                                               |
   | VM-Name                     | Name des neuen virtuellen Computers                                                           |
   | Name des virtuellen Netzwerks        | Name des virtuellen Netzwerks, das von der VM verwendet wird                                   |
   | NIC-Name                    | Name der Netzwerkschnittstellenkarte, über die das virtuelle Netzwerk ausgeführt wird               |
   | VHD-URL                     | Vollständige URL der VHD mit dem Betriebssystemdatenträger                                                     |
   |  |  |
            
5. Nachdem Sie diese Werte angegeben haben, klicken Sie auf **Kauf**. 

Azure startet die Bereitstellung: Im angegebenen Speicherkontopfad wird eine neue VM mit der angegebenen nicht verwalteten VHD erstellt.  Sie können den Fortschritt im Azure-Portal nachverfolgen, indem Sie im Portal auf der linken Seite auf **Virtuelle Computer** klicken.  Nachdem der virtuelle Computer erstellt wurde, ändert sich der Status von `Starting` zu `Running`. 


### <a name="deploy-a-vm-from-powershell"></a>Bereitstellen eines virtuellen Computers mit PowerShell

Zum Bereitstellen eines großen virtuellen Computers aus dem soeben erstellten generalisierten VM-Image verwenden Sie die folgenden Cmdlets.

``` powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```

<!-- TD: The following is a marketplace-publishing article and may be out-of-date.  TD: update and move topic.
For help with issues, see [Troubleshooting common issues encountered during VHD creation](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting) for additional assistance.
-->

## <a name="next-steps"></a>Nächste Schritte

Nach der Bereitstellung des virtuellen Computers können Sie ihn [konfigurieren](./cpp-configure-vm.md).
