---
title: Hinzufügen eines Azure Stack-Marketplace-Elements aus einer lokalen Quelle | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie dem Azure Stack-Marketplace ein lokales Betriebssystemimage hinzufügen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 296719ddd23fb9ee717455420906e9a634a71a8d
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-add-an-azure-stack-marketplace-item-from-a-local-source"></a>Tutorial: Hinzufügen eines Azure Stack-Marketplace-Elements aus einer lokalen Quelle

Als Azure Stack-Betreiber müssen Sie zunächst dem Azure Stack-Marketplace ein VM-Image hinzufügen, um Benutzern die Bereitstellung eines virtuellen Computers (Virtual Machine, VM) zu ermöglichen. Standardmäßig wird nichts im Azure Stack-Marketplace veröffentlicht. Sie können jedoch ISO-VM-Images hochladen, um sie für Benutzer verfügbar zu machen. Verwenden Sie diese Option, wenn Sie Azure Stack in einem Szenario ohne Verbindung bereitgestellt haben, oder in Szenarien mit eingeschränkter Konnektivität.

In diesem Tutorial laden Sie ein Windows Server 2016-VM-Image von der Seite mit den Windows Server-Evaluierungsversionen herunter und laden es in den Azure Stack-Marketplace hoch.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen eines Windows Server 2016-VM-Images
> * Überprüfen der Verfügbarkeit des VM-Images 
> * Testen des VM-Images

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie zur Vorbereitung dieses Tutorials die folgenden Schritte aus:

- Installieren Sie die [mit Azure Stack kompatiblen Azure PowerShell-Module](asdk-post-deploy.md#install-azure-stack-powershell).
- Laden Sie die [Azure Stack-Tools](asdk-post-deploy.md#download-the-azure-stack-tools) herunter.
- Laden Sie auf der Seite mit den Windows Server-Evaluierungsversionen das [VM-ISO-Image für Windows Server 2106](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016) herunter.

## <a name="add-a-windows-server-vm-image"></a>Hinzufügen eines Windows Server-VM-Images
Sie können das Windows Server 2016-Image im Azure Stack-Marketplace veröffentlichen, indem Sie mithilfe von PowerShell ein zuvor heruntergeladenes ISO-Image hinzufügen. 

Verwenden Sie diese Option, wenn Sie Azure Stack in einem Szenario ohne Verbindung bereitgestellt haben, oder in Szenarien mit eingeschränkter Konnektivität.

1. Importieren Sie mithilfe der folgenden Befehle die Azure Stack-PowerShell-Module `Connect` und `ComputeAdmin` aus dem Azure Stack-Toolverzeichnis:

   ```powershell
   Set-ExecutionPolicy RemoteSigned

   # Import the Connect and ComputeAdmin modules.   
   Import-Module .\Connect\AzureStack.Connect.psm1
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   ```

2. Führen Sie abhängig davon, ob Sie Ihre Azure Stack-Umgebung unter Verwendung von Azure Active Directory (Azure AD) oder unter Verwendung von Active Directory-Verbunddienste (AD FS) bereitgestellt haben, auf Ihrem ASDK-Hostcomputer eines der folgenden Skripts aus:

  - Befehle für **Azure AD-Bereitstellungen**: 

      ```PowerShell
      # To get this value for Azure Stack integrated systems, contact your service provider.
      $ArmEndpoint = "https://adminmanagement.local.azurestack.external"

      # To get this value for Azure Stack integrated systems, contact your service provider.
      $GraphAudience = "https://graph.windows.net/"
      
      # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
      Add-AzureRMEnvironment `
        -Name "AzureStackAdmin" `
        -ArmEndpoint $ArmEndpoint

      Set-AzureRmEnvironment `
        -Name "AzureStackAdmin" `
        -GraphAudience $GraphAudience

      $TenantID = Get-AzsDirectoryTenantId `
      # Replace the AADTenantName value to reflect your Azure AD tenant name.
        -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
        -EnvironmentName AzureStackAdmin

      Login-AzureRmAccount `
        -EnvironmentName "AzureStackAdmin" `
        -TenantId $TenantID 
      ```

  - Befehle für **AD FS-Bereitstellungen**:
      
      ```PowerShell
      # To get this value for Azure Stack integrated systems, contact your service provider.
      $ArmEndpoint = "https://adminmanagement.local.azurestack.external"

      # To get this value for Azure Stack integrated systems, contact your service provider.
      $GraphAudience = "https://graph.local.azurestack.external/"

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
   
3. Fügen Sie das Windows Server 2016-Image dem Azure Stack-Marketplace hinzu. (Ersetzen Sie *fully_qualified_path_to_ISO* durch den Pfad der heruntergeladenen Windows Server 2016-ISO-Datei.)

    ```PowerShell
    $ISOPath = "<fully_qualified_path_to_ISO>"

    # Add a Windows Server 2016 Evaluation VM image.
    New-AzsServer2016VMImage `
      -ISOPath $ISOPath

    ```

## <a name="verify-the-marketplace-item-is-available"></a>Überprüfen der Verfügbarkeit des Marketplace-Elements
Gehen Sie wie folgt vor, um sich zu vergewissern, dass das neue VM-Image im Azure Stack-Marketplace verfügbar ist:

1. Melden Sie sich beim [ASDK-Administratorportal](https://adminportal.local.azurestack.external) an.

2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung). 

3. Vergewissern Sie sich, dass das Windows Server 2016 Datacenter-VM-Image erfolgreich hinzugefügt wurde.

   ![Von Azure heruntergeladenes Image](media/asdk-marketplace-item/azs-marketplace-ws2016.png)

## <a name="test-the-vm-image"></a>Testen des VM-Images
Als Azure Stack-Betreiber können Sie über das [Administratorportal](https://adminportal.local.azurestack.external) einen virtuellen Testcomputer erstellen und sich vergewissern, dass das Image erfolgreich verfügbar gemacht wurde. 

1. Melden Sie sich beim [ASDK-Administratorportal](https://adminportal.local.azurestack.external) an.

2. Klicken Sie im Portal auf **Neu** > **Compute** > **Windows Server 2016 Datacenter** > **Erstellen**.  
 ![Erstellen eines virtuellen Computers auf der Grundlage eines Marketplace-Images](media/asdk-marketplace-item/new-compute.png)

3. Geben Sie auf dem Blatt **Grundlagen** die folgenden Informationen ein, und klicken Sie anschließend auf **OK**:
  - **Name:** test-vm-1
  - **Benutzername:** AdminTestUser
  - **Kennwort:** AzS-TestVM01
  - **Abonnement:** Übernehmen Sie das Standardabonnement des Anbieters.
  - **Ressourcengruppe:** test-vm-rg
  - **Standort:** lokal

4. Klicken Sie auf dem Blatt **Größe auswählen** auf **A1 Standard** und anschließend auf **Auswählen**.  

5. Behalten Sie auf dem Blatt **Einstellungen** die Standardwerte bei, und klicken Sie auf **OK**.

6. Klicken Sie auf dem Blatt **Zusammenfassung** auf **OK**, um den virtuellen Computer zu erstellen.  
> [!NOTE]
> Die Bereitstellung des virtuellen Computers dauert einige Minuten.

7. Klicken Sie zum Anzeigen des neuen virtuellen Computers auf **Virtuelle Computer**, suchen Sie nach **test-vm-1**, und klicken Sie auf den Namen.
    ![Erstes VM-Testimage angezeigt](media/asdk-marketplace-item/first-test-vm.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Nachdem Sie sich erfolgreich bei dem virtuellen Computer angemeldet und sich vergewissert haben, dass das Testimage ordnungsgemäß funktioniert, sollten Sie die Testressourcengruppe löschen. Dadurch geben Sie Ressourcen frei, die bei ASDK-Installationen mit einem einzelnen Knoten nur in begrenztem Umfang verfügbar sind.

Wenn Sie die Ressourcengruppe, den virtuellen Computer und die dazugehörigen Ressourcen nicht mehr benötigen, können Sie sie wie folgt löschen:

1. Melden Sie sich beim [ASDK-Administratorportal](https://adminportal.local.azurestack.external) an.
2. Klicken Sie auf **Ressourcengruppen** > **test-vm-rg** > **Ressourcengruppe löschen**.
3. Geben Sie den Ressourcengruppennamen **test-vm-rg** ein, und klicken Sie anschließend auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Hinzufügen eines Windows Server 2016-VM-Images
> * Überprüfen der Verfügbarkeit des VM-Images 
> * Testen des VM-Images

Im nächsten Tutorial erfahren Sie, wie Sie ein Azure Stack-Angebot und einen entsprechenden Plan erstellen.

> [!div class="nextstepaction"]
> [Anbieten von Azure Stack-IaaS-Diensten](asdk-offer-services.md)