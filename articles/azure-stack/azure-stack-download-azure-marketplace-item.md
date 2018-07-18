---
title: Herunterladen von Marketplace-Elementen von Azure | Microsoft-Dokumentation
description: Der Cloudbediener kann Marketplace-Elemente von Azure in die Azure Stack-Bereitstellung herunterladen.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 5d403f7c1d0fff466f6c0fb9942ec777ab820eab
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604531"
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Herunterladen von Marketplace-Elementen von Azure in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Laden Sie als Cloudbediener Elemente aus Azure Marketplace herunter, und stellen Sie sie in Azure Stack bereit. Die zur Auswahl stehenden Optionen stammen aus einer speziell zusammengestellten Liste mit Azure Marketplace-Elementen, die vorab getestet wurden und für Azure Stack geeignet sind. Dieser Liste werden regelmäßig weitere Elemente hinzugefügt. Prüfen Sie daher, ob neue Inhalte verfügbar sind. 

Es gibt zwei Szenarien zum Herstellen einer Verbindung mit dem Azure Marketplace: 

- **Verbundenes Szenario:** Hierfür muss Ihre Azure Stack-Umgebung mit dem Internet verbunden sein. Elemente werden über das Azure Stack-Portal gesucht und heruntergeladen. 
- **Nicht verbundenes oder partiell verbundenes Szenario:** Für dieses Szenario müssen Sie mithilfe des Tools für die Marketplace-Syndikation auf das Internet zugreifen, um Marketplace-Elemente herunterzuladen. Anschließend übertragen Sie Ihre Downloads in die nicht verbundene Azure Stack-Installation. Für dieses Szenario wird PowerShell verwendet.

Eine Liste der zum Download verfügbaren Marketplace-Elemente finden Sie unter [Für Azure Stack verfügbare Azure Marketplace-Elemente](azure-stack-marketplace-azure-items.md).


## <a name="connected-scenario"></a>Verbundenes Szenario
Wenn Azure Stack eine Verbindung mit dem Internet herstellt, können Sie mithilfe des Verwaltungsportals Marketplace-Elemente herunterladen.

### <a name="prerequisites"></a>Voraussetzungen
Ihre Azure Stack-Bereitstellung muss über eine Internetverbindung verfügen und [bei Azure registriert](azure-stack-register.md) sein.

### <a name="use-the-portal-to-download-marketplace-items"></a>Herunterladen von Marketplace-Elementen mithilfe des Portals  
1. Melden Sie sich beim Azure Stack-Administratorportal an.

2.  Überprüfen Sie den verfügbaren Speicherplatz, bevor Sie Marketplace-Elemente herunterladen. Wenn Sie später Elemente zum Download auswählen, können Sie die Downloadgröße mit der verfügbaren Speicherkapazität vergleichen. Ziehen Sie bei eingeschränkter Kapazität Optionen für die [Verwaltung des verfügbaren Speicherplatzes](azure-stack-manage-storage-shares.md#manage-available-space) in Betracht. 

    Wählen Sie zum Überprüfen des verfügbaren Speicherplatzes unter **Regionsverwaltung** die entsprechende Region aus, und klicken Sie dann auf **Ressourcenanbieter** > **Speicher**.

    ![Überprüfen der Speicherkapazität](media/azure-stack-download-azure-marketplace-item/storage.png)  

    
3. Öffnen Sie den Azure Stack-Marketplace, und stellen Sie eine Verbindung mit Azure her. Klicken Sie dazu auf **Marketplace-Verwaltung** und anschließend auf **Add from Azure** (Aus Azure hinzufügen).

    ![Hinzufügen aus Azure](media/azure-stack-download-azure-marketplace-item/marketplace.png)

    Im Portal wird die Liste der Elemente angezeigt, die im Azure Marketplace zum Download zur Verfügung stehen. Sie können auf die einzelnen Elemente klicken, um eine Beschreibung und zusätzliche Informationen anzuzeigen, u.a. die Downloadgröße. 

    ![Marketplace-Liste](media/azure-stack-download-azure-marketplace-item/image03.png)

4. Wählen Sie das gewünschte Element aus, und klicken Sie auf **Weiter**. Die Downloadzeiten können variieren.

    ![Downloadmeldung](media/azure-stack-download-azure-marketplace-item/image04.png)

    Wenn der Download abgeschlossen ist, können Sie das neue Marketplace-Element entweder als Azure Stack-Bediener oder -benutzer bereitstellen.

5. Klicken Sie zum Bereitstellen des heruntergeladenen Elements auf **+ Neu**, und suchen Sie in den Kategorien das neue Marketplace-Element. Wählen Sie als Nächstes das Element aus, um den Bereitstellungsprozess zu starten. Der Prozess variiert für verschiedene Marketplace-Elemente. 

## <a name="disconnected-or-a-partially-connected-scenario"></a>Nicht verbundenes oder partiell verbundenes Szenario

Wenn sich Azure Stack im Modus „Getrennt“ befindet und keine Internetverbindung besteht, verwenden Sie PowerShell und das *Tool für die Marketplace-Syndikation*, um die Marketplace-Elemente auf einen Computer mit Internetverbindung herunterzuladen. Anschließend übertragen Sie die Elemente in Ihre Azure Stack-Umgebung. In einer nicht verbundenen Umgebung können Sie Marketplace-Elemente nicht mithilfe des Azure Stack-Portals herunterladen. 

Das Tool für die Marketplace-Syndikation kann auch in einem verbundenen Szenario verwendet werden. 

Dieses Szenario besteht aus zwei Teilen:
- **Teil 1:** Herunterladen aus dem Azure Marketplace. Konfigurieren Sie auf dem Computer mit Internetzugriff PowerShell, und laden Sie das Tool für die Syndikation und anschließend Elemente aus dem Azure Marketplace herunter.  
- **Teil 2:** Hochladen und Veröffentlichen im Azure Stack-Marketplace. Verschieben Sie die heruntergeladenen Dateien in die Azure Stack-Umgebung, importieren Sie sie in Azure Stack, und veröffentlichen Sie sie anschließend im Azure Stack-Marketplace.  


### <a name="prerequisites"></a>Voraussetzungen
- Ihre Azure Stack-Bereitstellung muss [bei Azure registriert](azure-stack-register.md) sein.  

- Auf dem Computer mit Internetverbindung muss **mindestens Version 1.2.11 des Azure Stack-PowerShell-Moduls**  installiert sein. [Installieren Sie die Azure Stack-spezifischen PowerShell-Module](azure-stack-powershell-install.md), falls diese noch nicht vorhanden sind.  

- Die [PowerShell-Umgebung für den Azure Stack-Bediener](azure-stack-powershell-configure-admin.md) muss konfiguriert sein, um den Import eines heruntergeladenen Marketplace-Elements zu ermöglichen.  

- Sie benötigen ein [Speicherkonto](azure-stack-manage-storage-accounts.md) in Azure Stack mit einem öffentlich zugänglichen Container (Speicherblob). Sie verwenden den Container als temporären Speicher für die Katalogdateien der Marketplace-Elemente. Wenn Sie mit Speicherkonten und Containern nicht vertraut sind, lesen Sie den Artikel [Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs über das Azure-Portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) in der Azure-Dokumentation.

- Das Tool für die Marketplace-Syndikation wird beim ersten Verfahren heruntergeladen. 

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>Herunterladen von Marketplace-Elementen mithilfe des Tools für die Marketplace-Syndikation

1. Öffnen Sie auf einem Computer mit Internetverbindung eine PowerShell-Konsole als Administrator.

2. Fügen Sie das Azure-Konto hinzu, das Sie für die Registrierung von Azure Stack verwendet haben. Führen Sie zum Hinzufügen des Kontos in PowerShell `Add-AzureRmAccount` ohne Parameter aus. Sie werden aufgefordert, Ihre Anmeldeinformationen für das Azure-Konto einzugeben. Je nach Konfiguration Ihres Kontos müssen Sie möglicherweise die zweistufige Authentifizierung verwenden.

3. Sollten Sie über mehrere Abonnements verfügen, führen Sie den folgenden Befehl aus, um das für die Registrierung verwendete Abonnement auszuwählen:  

   ```PowerShell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Laden Sie mithilfe des folgenden Skripts die neueste Version des Tools für die Marketplace-Syndikation herunter:  

   ```PowerShell
   # Download the tools archive.
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip `
     -OutFile master.zip

   # Expand the downloaded files.
   expand-archive master.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd .\AzureStack-Tools-master

   ```

5. Führen Sie das folgende Skript aus, um das Syndikationsmodul zu importieren und zu starten. Ersetzen Sie *destination folder path* durch einen Speicherort, an dem die aus dem Azure Marketplace heruntergeladenen Dateien gespeichert werden sollen.   

   ```PowerShell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination "Destination folder path" `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Wenn Sie das Tool ausführen, werden Sie zur Eingabe Ihrer Azure-Anmeldeinformationen aufgefordert. Melden Sie sich bei dem Azure-Konto an, das Sie für die Registrierung von Azure Stack verwendet haben. Nach erfolgreicher Anmeldung wird ungefähr folgender Bildschirm mit einer Liste der verfügbaren Marketplace-Elemente angezeigt:  

   ![Popup mit Azure Marketplace-Elementen](media/azure-stack-download-azure-marketplace-item/image05.png)

7. Wählen Sie das Image aus, das Sie herunterladen möchten, und notieren Sie sich die *Version*. (Sie können mehrere Images auswählen, indem Sie *STRG* gedrückt halten.) Sie benötigen die *Version*, wenn Sie das Element im nächsten Verfahren importieren. 
   
   Die Liste mit den Images kann mithilfe der Option **Kriterien hinzufügen** gefiltert werden.

8. Klicken Sie auf **OK**, lesen Sie sich dann die rechtlichen Bedingungen durch, und bestätigen Sie diese. 

9. Die Dauer des Downloads hängt von der Größe des Elements ab. Nach Abschluss des Downloads ist das Element in dem Ordner verfügbar, den Sie im Skript angegeben haben. Der Download enthält eine VHD-Datei (für virtuelle Computer) oder eine ZIP-Datei (für VM-Erweiterungen). Darüber hinaus ist ein Katalogelement im Format *AZPKG* enthalten. (Ein *AZPKG*-Paket ist eine *ZIP*-Datei.)
 

### <a name="import-the-download-and-publish-to-azure-stack-marketplace"></a>Importieren des Downloads und Veröffentlichen im Azure Stack-Marketplace
1. Die [zuvor heruntergeladenen](#use-the-marketplace-syndication-tool-to-download-marketplace-items) Dateien für VM-Images oder Lösungsvorlagen müssen für die Azure Stack-Umgebung lokal verfügbar gemacht werden.  

2. Importieren Sie VHD-Dateien in Azure Stack. Um ein VM-Image erfolgreich importieren zu können, benötigen Sie die folgenden Informationen zum virtuellen Computer:
   - Die *Version*, die Sie in Schritt 7 des vorherigen Verfahrens notiert haben.
   - Die Werte für *publisher*, *offer* und *sku* des virtuellen Computers. Ändern Sie zum Abrufen dieser Werte bei einer Kopie der **AZPKG**-Datei die Dateierweiterung in **.zip**. Anschließend können Sie mit einem Text-Editor **DeploymentTemplates\CreateUiDefinition.json** öffnen. Suchen Sie in der JSON-Datei den Abschnitt *imageReference*. Dieser Abschnitt enthält die Werte für das Marketplace-Element. Im folgenden Beispiel wird veranschaulicht, wie diese Informationen angezeigt werden:

     ```json  
     "imageReference": {  
        "publisher": "MicrosoftWindowsServer",  
        "offer": "WindowsServer",  
        "sku": "2016-Datacenter-Server-Core"  
      }
     ```  

   Importieren Sie das Image mithilfe des Cmdlets **Add-AzsPlatformimage** in Azure Stack. Ersetzen Sie bei Verwendung dieses Cmdlets die Werte für *Herausgeber*, *Angebot* und andere Parameter durch die Werte des Images, das Sie importieren möchten. Die Werte für *publisher*, *offer* und *sku* des Images finden Sie in der Textdatei, die zusammen mit der AZPKG-Datei heruntergeladen und am Zielspeicherort abgelegt wird. 

   Im folgenden Beispielskript werden die Werte für den virtuellen Computer „Windows Server 2016 Datacenter – Server Core“ verwendet. 

   ```PowerShell  
   Add-AzsPlatformimage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2016.127.20171215" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Windows-Server-2016-DatacenterCore-20171215-en.us-127GB.vhd" `
   ```
   **Informationen zu Lösungsvorlagen:** Einige Vorlagen können eine kleine VHD-Datei mit 3 MB und dem Namen **fixed3.vhd** enthalten. Sie müssen diese Datei nicht in Azure Stack importieren. Fixed3.vhd.  Diese Datei ist in einigen Lösungsvorlagen enthalten, um Veröffentlichungsanforderungen für den Azure Marketplace zu erfüllen.

   Lesen Sie die Vorlagenbeschreibung, laden Sie zusätzlich erforderliche Komponenten (etwa für die Verwendung mit der Lösungsvorlage erforderliche VHDs) herunter, und importieren Sie sie anschließend.

3. Verwenden Sie das Verwaltungsportal, um das Marketplace-Elementpaket (die AZPKG-Datei) in den Azure Stack-Blobspeicher hochzuladen. Durch das Hochladen des Pakets wird es für Azure Stack verfügbar gemacht, sodass Sie das Element später im Azure Stack-Marketplace veröffentlichen können.

   Für den Upload benötigen Sie ein Speicherkonto mit einem öffentlich zugänglichen Container. (Informationen hierzu finden Sie in den Voraussetzungen für dieses Szenario.)   
   1. Navigieren Sie im Azure Stack-Verwaltungsportal zu **Weitere Dienste** > **Speicherkonten**.  
   
   2. Wählen Sie in Ihrem Abonnement ein Speicherkonto aus, und klicken Sie anschließend unter **BLOB-DIENST** auf **Container**.  
      ![Blob-Dienst](media/azure-stack-download-azure-marketplace-item/blob-service.png)  
   
   3. Wählen Sie den Container aus, den Sie verwenden möchten, und klicken Sie dann auf **Hochladen**, um den Bereich **Block hochladen** zu öffnen.  
      ![Container](media/azure-stack-download-azure-marketplace-item/container.png)  
   
   4. Navigieren Sie im Bereich „Blob hochladen“ zu den Dateien, die Sie in den Speicher laden möchten, und wählen Sie dann **Hochladen**.  
      ![upload](media/azure-stack-download-azure-marketplace-item/upload.png)  

   5. Hochgeladene Dateien werden im Bereich „Container“ angezeigt. Wählen Sie eine Datei aus, und kopieren Sie die URL aus dem Bereich **Blob-Eigenschaften**. Sie verwenden diese URL im nächsten Schritt beim Importieren des Marketplace-Elements in Azure Stack.  In der folgenden Abbildung heißt der Container *blob-test-storage* und die Datei *Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg*.  Die Datei-URL lautet *https://testblobstorage1.blob.local.azurestack.external/blob-test-storage/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg*.  
      ![Blob-Eigenschaften](media/azure-stack-download-azure-marketplace-item/blob-storage.png)  

4.  Verwenden Sie PowerShell und das Cmdlet **Add-AzsGalleryItem**, um das Marketplace-Element in Azure Stack zu veröffentlichen. Beispiel:   
    ```PowerShell  
    Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg" `
     –Verbose
    ```
5. Nach dem Veröffentlichen eines Katalogelements können Sie es unter **Weitere Dienste** > **Marketplace** anzeigen.  Falls es sich bei dem Download um eine Lösungsvorlage handelt, fügen Sie auch alle abhängigen VHD-Images für sie hinzu.  
  ![Anzeigen des Marketplace](media/azure-stack-download-azure-marketplace-item/view-marketplace.png)  

> [!NOTE]
> Mit der Veröffentlichung von Azure Stack PowerShell 1.3.0 können Sie nun VM-Erweiterungen hinzufügen.

Beispiel: 

````PowerShell
Add-AzsVMExtension -Publisher "Microsoft" -Type "MicroExtension" -Version "0.1.0" -ComputeRole "IaaS" -SourceBlob "https://github.com/Microsoft/PowerShell-DSC-for-Linux/archive/v1.1.1-294.zip" -SupportMultipleExtensions -VmOsType "Linux"
````

## <a name="next-steps"></a>Nächste Schritte
[Erstellen und Veröffentlichen eines Marketplace-Elements](azure-stack-create-and-publish-marketplace-item.md)