---
title: "Hinzufügen des VM-Standardimages zum Azure Stack-Marketplace | Microsoft-Dokumentation"
description: "Fügen Sie das VM-Standardimage für Windows Server 2016 zum Azure Stack-Marketplace hinzu."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: sngun
ms.openlocfilehash: e5e7ab55800eccea98cbd71ae32bdc611ee3e961
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="add-the-windows-server-2016-vm-image-to-the-azure-stack-marketplace"></a>Hinzufügen des VM-Images für Windows Server 2016 zum Azure Stack-Marketplace

Im Azure Stack-Marketplace sind standardmäßig keine VM-Images verfügbar. Ein Azure Stack-Betreiber muss ein Image zum Marketplace hinzufügen, damit Benutzer darauf zugreifen können. Das Windows Server 2016-Image kann mit einer der folgenden Methoden zum Azure Stack-Marketplace hinzugefügt werden:

* [Herunterladen des Images aus dem Azure Marketplace:](#add-the-image-by-downloading-it-from-the-azure-marketplace) Verwenden Sie diese Option, wenn Sie ein verbundenes Szenario nutzen und Ihre Azure Stack-Instanz bei Azure registriert haben.

* [Hinzufügen des Images mithilfe von PowerShell:](#add-the-image-by-using-powershell) Verwenden Sie diese Option, wenn Sie Azure Stack in einem nicht verbundenen Szenario bereitgestellt haben, oder in Szenarien mit eingeschränkter Konnektivität.

## <a name="add-the-image-by-downloading-it-from-the-azure-marketplace"></a>Hinzufügen des Images durch Herunterladen aus dem Azure Marketplace

1. Stellen Sie Azure Stack bereit, und melden Sie sich anschließend bei Ihrem Azure Stack Development Kit an.

2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung) > **Add from Azure** (Aus Azure hinzufügen). 

3. Suchen Sie nach dem Image **Windows Server 2016 Datacenter – Eval**, und klicken Sie auf **Herunterladen**.

   ![Herunterladen eines Images von Azure](media/azure-stack-add-default-image/download-image.png)

Nach Abschluss des Downloadvorgangs steht das Image unter **Marketplace Management** (Marketplace-Verwaltung) zur Verfügung. Außerdem ist das Image unter **Virtual Machines** verfügbar.

## <a name="add-the-image-by-using-powershell"></a>Hinzufügen des Images mithilfe von PowerShell

### <a name="prerequisites"></a>Voraussetzungen 

Führen Sie die folgenden erforderlichen Schritte entweder über das [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) oder auf einem Windows-basierten externen Client aus, sofern [eine VPN-Verbindung](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) besteht:

1. Installieren Sie [mit Azure Stack kompatible Azure PowerShell-Module](azure-stack-powershell-install.md).  

2. Laden Sie die [Tools herunter, die für die Arbeit mit Azure Stack benötigt werden](azure-stack-powershell-download.md).  

3. Laden Sie auf der Seite mit den Windows Server-Evaluierungsversionen die [Evaluierungsversion von Windows Server 2016](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016) herunter. Wählen Sie die ISO-Version des Downloads aus, wenn Sie danach gefragt werden. Notieren Sie sich den Pfad des Downloadspeicherorts zur späteren Verwendung in diesem Artikel. Für diesen Schritt ist eine Internetverbindung erforderlich.  

### <a name="add-the-image-to-the-azure-stack-marketplace"></a>Hinzufügen des Images zum Azure Stack-Marketplace
   
1. Importieren Sie nun mit den folgenden Befehlen das Connect- und das ComputeAdmin-Modul von Azure Stack:

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
     -ADFS `
     -EnvironmentName "AzureStackAdmin" 

    Login-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID 
    ```
   
3. Fügen Sie das Windows Server 2016-Image zum Azure Stack-Marketplace hinzu. (Ersetzen Sie *fully_qualified_path_to_ISO* durch den Pfad der heruntergeladenen Windows Server 2016-ISO-Datei.)

    ```PowerShell
    $ISOPath = "<fully_qualified_path_to_ISO>"

    # Add a Windows Server 2016 Evaluation VM image.
    New-AzsServer2016VMImage `
      -ISOPath $ISOPath

    ```

Um sicherzustellen, dass das VM-Image für Windows Server 2016 das aktuelle kumulative Update enthält, nehmen Sie beim Ausführen des `New-AzsServer2016VMImage`-Cmdlets den `IncludeLatestCU`-Parameter mit auf. Informationen zu zulässigen Parametern für das `New-AzsServer2016VMImage`-Cmdlet finden Sie unter [Parameter](#parameters). Die Veröffentlichung des Images im Azure Stack-Marketplace dauert etwa eine Stunde. 

## <a name="parameters"></a>Parameter

|New-AzsServer2016VMImage-Parameter|Erforderlich|Beschreibung|
|-----|-----|------|
|ISOPath|Ja|Der vollqualifizierte Pfad zur heruntergeladenen ISO-Datei für Windows Server 2016|
|Net35|Nein|Installiert die .NET 3.5-Laufzeit für das Windows Server 2016-Image. Dieser Wert ist standardmäßig auf **true** festgelegt.|
|Version|Nein|Gibt **Core**, **Full** oder **Both** für das Windows Server 2016-Image an. Dieser Wert ist standardmäßig auf **Full** festgelegt.|
|VHDSizeInMB|Nein|Legt die Größe (in MB) des VHD-Images fest, das zu Ihrer Azure Stack-Umgebung hinzugefügt werden soll. Dieser Wert ist standardmäßig auf 40.960 MB festgelegt.|
|CreateGalleryItem|Nein|Gibt an, ob ein Marketplace-Element für das Windows Server 2016-Image erstellt werden soll. Dieser Wert ist standardmäßig auf **true** festgelegt.|
|location |Nein |Gibt den Ort an, an dem das Windows Server 2016-Image veröffentlicht werden soll.|
|IncludeLatestCU|Nein|Wendet das neueste kumulative Windows Server 2016-Update auf die neue VHD an.|
|CUUri |Nein |Legt fest, dass das kumulative Update für Windows Server 2016 von einem bestimmten URI aus ausgeführt werden soll. |
|CUPath |Nein |Legt fest, dass das kumulative Update für Windows Server 2016 von einem lokalen Pfad aus ausgeführt werden soll. Diese Option ist nützlich, wenn Sie die Azure Stack-Instanz in einer nicht verbundenen Umgebung bereitgestellt haben.|

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von virtuellen Computern](azure-stack-provision-vm.md)
