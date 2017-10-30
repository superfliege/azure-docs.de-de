---
title: "Hinzufügen eines VM-Images zu Azure Stack | Microsoft-Dokumentation"
description: "Fügen Sie das benutzerdefinierte Windows- oder Linux-VM-Image Ihrer Organisation hinzu, damit es von Mandanten verwendet werden kann."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: e5a4236b-1b32-4ee6-9aaa-fcde297a020f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: sngun
ms.openlocfilehash: 520e4dfaadf1d476447a600ef2b3d092b6955a89
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="make-a-custom-virtual-machine-image-available-in-azure-stack"></a>Verfügbarmachen eines benutzerdefinierten VM-Images in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

In Azure Stack können Betreiber benutzerdefinierte VM-Images für ihre Benutzer verfügbar machen. Auf diese Images kann durch Azure Resource Manager-Vorlagen verwiesen werden, sie können aber auch der Azure Marketplace-Benutzeroberfläche als Marketplace-Element hinzugefügt werden. 

## <a name="add-a-vm-image-to-marketplace-by-using-powershell"></a>Hinzufügen eines VM-Images zu Marketplace mithilfe von PowerShell

Führen Sie die folgenden erforderlichen Schritte entweder über das [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) oder auf einem Windows-basierten externen Client aus, sofern [eine VPN-Verbindung](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) besteht:

1. [Installieren Sie PowerShell für Azure Stack](azure-stack-powershell-install.md).  

2. Laden Sie die [Tools herunter, die für die Arbeit mit Azure Stack benötigt werden](azure-stack-powershell-download.md).  

3. Bereiten Sie ein virtuelles Festplattenimage im VHD-Format mit einem Windows- oder Linux-Betriebssystem vor. (Verwenden Sie nicht das VHDX-Format.)
   
   * Eine Anleitung zum Vorbereiten eines Windows-Images finden Sie unter [Hochladen einer generalisierten VHD und Verwendung dieser zum Erstellen neuer VMs in Azure](../virtual-machines/windows/upload-generalized-managed.md).
   * Eine Anleitung für Linux-Images finden Sie unter [Hinzufügen von Linux-Images zu Azure Stack](azure-stack-linux.md). Führen Sie die Schritte zum Vorbereiten des Images aus, oder verwenden Sie ein bereits vorhandenes Azure Stack-Linux-Image, wie im Artikel beschrieben.  

Gehen Sie wie folgt vor, um das Image dem Azure Stack-Marketplace hinzuzufügen:

1. Importieren Sie das Connect- und das ComputeAdmin-Modul:
   
   ```powershell
   Set-ExecutionPolicy RemoteSigned

   # Import the Connect and ComputeAdmin modules.
   Import-Module .\Connect\AzureStack.Connect.psm1
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1
   ``` 

2. Melden Sie sich bei Ihrer Azure Stack-Umgebung an. Führen Sie eines der folgenden Skripts aus – je nachdem, ob Sie Ihre Azure Stack-Umgebung unter Verwendung von Azure Active Directory (Azure AD) oder unter Verwendung von Active Directory-Verbunddienste (AD FS) bereitgestellt haben. Ersetzen Sie den Azure AD-Mandantennamen (`tenantName`), den `GraphAudience`-Endpunkt und die `ArmEndpoint`-Werte durch Werte für Ihre Umgebungskonfiguration.

    * **Azure Active Directory:** Verwenden Sie das folgende Cmdlet:

      ```PowerShell
      # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
      $ArmEndpoint = "<Resource Manager endpoint for your environment>"

      # For Azure Stack Development Kit, this value is set to https://graph.windows.net/. To get this value for Azure Stack integrated systems, contact your service provider.
      $GraphAudience = "<GraphAuidence endpoint for your environment>"
      
      # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
      Add-AzureRMEnvironment `
        -Name "AzureStackAdmin" `
        -ArmEndpoint $ArmEndpoint

      Set-AzureRmEnvironment `
        -Name "AzureStackAdmin" `
        -GraphAudience $GraphAudience

      $TenantID = Get-AzsDirectoryTenantId `
        -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
        -EnvironmentName AzureStackAdmin

      Login-AzureRmAccount `
        -EnvironmentName "AzureStackAdmin" `
        -TenantId $TenantID 
      ```

   * **Active Directory-Verbunddienste:** Verwenden Sie das folgende Cmdlet:
    
        ```PowerShell
        # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
        $ArmEndpoint = "<Resource Manager endpoint for your environment>"

        # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
        $GraphAudience = "<GraphAuidence endpoint for your environment>"

        # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
        Add-AzureRMEnvironment `
          -Name "AzureStackAdmin" `
          -ArmEndpoint $ArmEndpoint

        Set-AzureRmEnvironment `
          -Name "AzureStackAdmin" `
          -GraphAudience $GraphAudience `
          -EnableAdfsAuthentication:$true

        $TenantID = Get-AzsDirectoryTenantId `
          -ADFS 
          -EnvironmentName AzureStackAdmin 

        Login-AzureRmAccount `
          -EnvironmentName "AzureStackAdmin" `
          -TenantId $TenantID 
        ```
    
3. Fügen Sie das VM-Image durch Aufruf des `Add-AzsVMImage`-Cmdlets hinzu. Geben Sie im Cmdlet `Add-AzsVMImage` den Betriebssystemtyp (`osType`) als Windows oder Linux an. Fügen Sie den Herausgeber, das Angebot, die SKU und die Version für das VM-Image ein. Informationen zu zulässigen Parametern finden Sie unter [Parameter](#parameters). Die Parameter werden in Azure Resource Manager-Vorlagen verwendet, um auf das VM-Image zu verweisen. Das folgende Beispiel ruft das Skript auf:
     
  ```powershell
  Add-AzsVMImage `
    -publisher "Canonical" `
    -offer "UbuntuServer" `
    -sku "14.04.3-LTS" `
    -version "1.0.0" `
    -osType Linux `
    -osDiskLocalPath 'C:\Users\AzureStackAdmin\Desktop\UbuntuServer.vhd' `
  ```

Der Befehl bewirkt Folgendes:

* Authentifizieren bei der Azure Stack-Umgebung
* Hochladen der lokalen VHD in ein neu erstelltes temporäres Speicherkonto
* Hinzufügen des VM-Images zum VM-Image-Repository
* Erstellen eines Marketplace-Elements

Vergewissern Sie sich, dass der Befehl erfolgreich ausgeführt wurde, indem Sie im Portal zum Marketplace navigieren. Überprüfen Sie, ob das VM-Image in der Kategorie **Virtual Machines** verfügbar ist.

![Hinzugefügtes VM-Image](./media/azure-stack-add-vm-image/image5.PNG) 

## <a name="remove-a-vm-image-by-using-powershell"></a>Entfernen eines VM-Images mithilfe von PowerShell

Wenn Sie das hochgeladene VM-Image nicht mehr benötigen, können Sie es im Marketplace mithilfe des folgenden Cmdlets löschen:

```powershell
Remove-AzsVMImage `
  -publisher "Canonical" `
  -offer "UbuntuServer" `
  -sku "14.04.3-LTS" `
  -version "1.0.0" `
```

## <a name="parameters"></a>Parameter

| Parameter | Beschreibung |
| --- | --- |
| **publisher** |Das von Benutzern bei der Bereitstellung des VM-Images verwendete Segment mit dem Herausgebernamen. Beispiel: **Microsoft**. Dieses Feld darf kein Leerzeichen und kein anderes Sonderzeichen enthalten. |
| **offer** |Das von Benutzern bei der Bereitstellung des VM-Images verwendete Segment mit dem Angebotsnamen. Beispiel: **WindowsServer**. Dieses Feld darf kein Leerzeichen und kein anderes Sonderzeichen enthalten. |
| **sku** |Das von Benutzern bei der Bereitstellung des VM-Images verwendete Segment mit dem SKU-Namen. Beispiel: **Datacenter2016**. Dieses Feld darf kein Leerzeichen und kein anderes Sonderzeichen enthalten. |
| **version** |Die von Benutzern bei der Bereitstellung des VM-Images verwendete Version des VM-Images. Diese Version wird im Format *\#.\#.\#* angegeben. Beispiel: **1.0.0**. Dieses Feld darf kein Leerzeichen und kein anderes Sonderzeichen enthalten. |
| **osType** |Als Betriebssystemtyp des Images muss **Windows** oder **Linux** angegeben werden. |
| **osDiskLocalPath** |Der lokale Pfad zur VHD mit dem Betriebssystem-Datenträger, die Sie als VM-Image in Azure Stack hochladen |
| **dataDiskLocalPaths** |Ein optionales Array der lokalen Pfade für Datenträger, die als Teil des VM-Images hochgeladen werden können |
| **CreateGalleryItem** |Ein boolesches Flag, das angibt, ob ein Element im Marketplace erstellt werden soll. Ist standardmäßig auf **true** festgelegt. |
| **title** |Der Anzeigename des Marketplace-Elements. Ist standardmäßig auf den `Publisher-Offer-Sku`-Wert des VM-Images festgelegt. |
| **description** |Die Beschreibung des Marketplace-Elements |
| **location** |Der Ort, an dem das VM-Image veröffentlicht werden soll. Dieser Wert ist standardmäßig auf **lokal** festgelegt.|
| **osDiskBlobURI** |(Optional) Dieses Skript akzeptiert auch einen Blobspeicher-URI für `osDisk`. |
| **dataDiskBlobURIs** |(Optional) Dieses Skript akzeptiert auch ein Array mit Blobspeicher-URIs, um dem Image Datenträger hinzuzufügen. |

## <a name="add-a-vm-image-through-the-portal"></a>Hinzufügen eines VM-Images über das Portal

> [!NOTE]
> Mit dieser Methode muss das Marketplace-Element separat erstellt werden.

Auf Images muss über einen Blobspeicher-URI verwiesen werden können. Bereiten Sie ein Image mit Windows- oder Linux-Betriebssystem im VHD-Format (nicht VHDX) vor, und laden Sie es anschließend in ein Speicherkonto in Azure oder Azure Stack hoch. Falls Ihr Image bereits in den Blobspeicher in Azure oder Azure Stack hochgeladen wurde, können Sie Schritt 1 überspringen.

1. [Laden Sie für Resource Manager-Bereitstellungen ein Windows-VM-Image in Azure hoch](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/), oder gehen Sie für ein Linux-Image wie unter [Hinzufügen von Linux-Images zu Azure Stack](azure-stack-linux.md) beschrieben vor. Berücksichtigen Sie vor dem Hochladen des Images unbedingt folgende Faktoren:

   * Es ist effizienter, ein Image in den Azure Stack-Blobspeicher als in Azure Blob Storage hochzuladen, da die Übertragung des Images per Push in das Azure Stack-Image-Repository schneller geht. 
   
   * Ersetzen Sie beim Hochladen des [Windows-VM-Images](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) den Schritt **Anmelden bei Azure** durch den Schritt [Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers](azure-stack-powershell-configure-admin.md).  

   * Notieren Sie sich den Blobspeicher-URI, in den Sie das Image hochladen. Der Blobspeicher-URI hat folgendes Format: *&lt;Speicherkonto&gt;/&lt;Blobcontainer&gt;/&lt;Ziel-VHD-Name&gt;*.vhd.

   * Damit auf das Blob anonym zugegriffen werden kann, navigieren Sie zum Speicherkonto-Blobcontainer, in den die VM-Image-VHD hochgeladen wurde. Klicken Sie auf **Blob** und anschließend auf **Zugriffsrichtlinie**. Sie können optional auch eine Shared Access Signature für den Container generieren und in den Blob-URI aufnehmen.

   ![Navigieren zu Speicherkontoblobs](./media/azure-stack-add-vm-image/image1.png)

   ![Festlegen des öffentlichen Blobzugriffs](./media/azure-stack-add-vm-image/image2.png)

2. Melden Sie sich bei Azure Stack als Bediener an. Klicken Sie im Menü auf **Weitere Dienste** > **Ressourcenanbieter**. Klicken Sie anschließend auf **Compute** > **VM-Images** > **Hinzufügen**.

3. Geben Sie unter **Add a VM Image** (VM-Image hinzufügen) den Herausgeber, das Angebot, die SKU und die Version des VM-Images ein. Diese Namenssegmente verweisen in Resource Manager-Vorlagen auf das VM-Image. Wählen Sie den korrekten Wert für **osType** aus. Geben Sie unter **OS Disk Blob URI** (Blob-URI des Betriebssystemdatenträgers) den Blob-URI ein, an den das Image hochgeladen wurde. Klicken Sie anschließend auf **Erstellen**, um mit der Erstellung des VM-Images zu beginnen.
   
   ![Starten der Imageerstellung](./media/azure-stack-add-vm-image/image4.png)

   Nach Erstellung des Images ändert sich der VM-Imagestatus in **Erfolgreich**.

4. Damit über die Benutzeroberfläche leichter auf das VM-Image zugegriffen werden kann, empfiehlt sich die [Erstellung eines Marketplace-Elements](azure-stack-create-and-publish-marketplace-item.md).

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von virtuellen Computern](azure-stack-provision-vm.md)