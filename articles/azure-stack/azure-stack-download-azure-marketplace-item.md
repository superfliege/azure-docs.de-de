---
title: Herunterladen von Marketplace-Elementen von Azure | Microsoft-Dokumentation
description: Ich kann Marketplace-Elemente von Azure in meine Azure Stack-Bereitstellung herunterladen.
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
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 2e92dc96a69400f689e49b70d1b855c955084362
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Herunterladen von Marketplace-Elementen von Azure in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*


Wenn Sie entscheiden, welche Inhalte in Ihrem Azure Stack-Marketplace enthalten sein sollen, berücksichtigen Sie auch die im Azure-Marketplace verfügbaren Inhalte. Zum Herunterladen steht Ihnen eine Aufstellung ausgewählter Marketplace-Elemente zur Verfügung, deren Ausführung unter Azure Stack vorab getestet wurde. Dieser Liste werden regelmäßig neue Elemente hinzugefügt. Prüfen Sie daher, ob neue Inhalte verfügbar sind.

## <a name="download-marketplace-items-in-a-connected-scenario-with-internet-connectivity"></a>Herunterladen von Marketplace-Elementen in einem verbundenen Szenario (mit Internetverbindung)

1. Zum Herunterladen von Marketplace-Elementen müssen Sie zunächst [Azure Stack bei Azure registrieren](azure-stack-register.md).
2. Melden Sie sich beim Azure Stack-Administratorportal an. (Verwenden Sie für das ASDK Folgendes: https://portal.local.azurestack.external).
3. Einige Marketplace-Elemente sind unter Umständen groß. Stellen Sie sicher, dass auf Ihrem System ausreichend Speicherplatz vorhanden ist. Klicken Sie hierfür auf **Ressourcenanbieter** > **Speicher**.

    ![](media/azure-stack-download-azure-marketplace-item/image01.png)

4. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung).

    ![](media/azure-stack-download-azure-marketplace-item/image02.png)

4. Klicken Sie auf **Add from Azure** (Aus Azure hinzufügen), um eine Liste von Elementen anzuzeigen, die heruntergeladen werden können. Klicken Sie auf die einzelnen Elemente in der Liste, um deren Beschreibung und Downloadgröße anzuzeigen.

    ![](media/azure-stack-download-azure-marketplace-item/image03.png)

5. Wählen Sie das gewünschte Element in der Liste aus, und klicken Sie dann auf **Herunterladen**. Der Download des VM-Images für das ausgewählte Element beginnt. Die Downloadzeiten können variieren.

    ![](media/azure-stack-download-azure-marketplace-item/image04.png)

6. Wenn der Download abgeschlossen ist, können Sie Ihr neues Marketplace-Element entweder als Azure Stack-Betreiber oder -benutzer bereitstellen. Klicken Sie auf **+ Neu**, suchen Sie das neue Marketplace-Element in den Kategorien, und wählen Sie das Element aus.
7. Klicken Sie auf **Erstellen** , um die Erstellungsoberfläche für das neu heruntergeladenen Element zu öffnen. Führen Sie die schrittweisen Anweisungen aus, um Ihr Element bereitzustellen.

## <a name="download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Herunterladen von Marketplace-Elementen in einem nicht verbundenen oder partiell verbundenen Szenario (mit eingeschränkter Internetkonnektivität)

Wenn Sie Azure Stack in einem nicht verbundenen Modus (ohne Internetverbindung) bereitstellen, können Sie keine Marketplace-Elemente über das Azure Stack-Portal herunterladen. Sie können jedoch das Tool für die Marketplace-Syndikation verwenden, um die Marketplace-Elemente auf einen Computer mit Internetverbindung herunterzuladen und anschließend in Ihre Azure Stack-Umgebung zu übertragen.

### <a name="prerequisites"></a>Voraussetzungen
Vergewissern Sie sich vor der Verwendung des Tools für die Marketplace-Syndikation, dass [Azure Stack bei Ihrem Azure-Abonnement registriert](azure-stack-register.md) ist.  

Gehen Sie auf dem Computer, der über eine Internetverbindung verfügt, wie folgt vor, um die erforderlichen Marketplace-Elemente herunterzuladen:

1. Öffnen Sie eine PowerShell-Konsole als Administrator, und [installieren Sie Azure Stack-spezifische PowerShell-Module](azure-stack-powershell-install.md). Installieren Sie **mindestens Version 1.2.11 des Azure Stack-PowerShell-Moduls**.  

2. Fügen Sie das Azure-Konto hinzu, das Sie für die Registrierung von Azure Stack verwendet haben. Führen Sie zum Hinzufügen des Kontos das **Add-AzureRmAccount**-Cmdlet ohne Parameter aus. Sie werden aufgefordert, Ihre Anmeldeinformationen für das Azure-Konto einzugeben. Je nach Konfiguration Ihres Kontos müssen Sie möglicherweise die zweistufige Authentifizierung verwenden.  

3. Sollten Sie über mehrere Abonnements verfügen, führen Sie den folgenden Befehl aus, um das für die Registrierung verwendete Abonnement auszuwählen:  

   ```powershell
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
   cd \AzureStack-Tools-master

   ```

5. Führen Sie die folgenden Befehle aus, um das Syndikationsmodul zu importieren und zu starten:  

   ```powershell
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination "<Destination folder path>" `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Wenn Sie das Tool ausführen, werden Sie zur Eingabe Ihrer Azure-Anmeldeinformationen aufgefordert. Melden Sie sich bei dem Azure-Konto an, das Sie für die Registrierung von Azure Stack verwendet haben. Nach erfolgreicher Anmeldung wird der folgende Bildschirm mit einer Liste der verfügbaren Marketplace-Elemente angezeigt:  

   ![Popup mit Azure Marketplace-Elementen](./media/azure-stack-download-azure-marketplace-item/image05.png)

7. Wählen Sie das Image aus, das Sie herunterladen möchten, und notieren Sie sich die Imageversion. Sie können mehrere Images auswählen, indem Sie die STRG-Taste gedrückt halten. Sie verwenden die Imageversion, um das Image im nächsten Abschnitt zu importieren.  Klicken Sie als Nächstes auf **OK**, und akzeptieren Sie anschließend die rechtlichen Bedingungen, indem Sie auf **Ja** klicken. Die Liste mit den Images kann mithilfe der Option **Kriterien hinzufügen** gefiltert werden. 

   Der Downloadvorgang kann abhängig von der Imagegröße eine Weile dauern. Nach Abschluss des Downloadvorgangs steht das Image am zuvor angegebenen Zielpfad zur Verfügung. Der Download enthält eine VHD-Datei (für virtuelle Computer) oder eine ZIP-Datei (für VM-Erweiterungen) und ein Katalogelement im AZPKG-Format.

### <a name="import-the-image-and-publish-it-to-azure-stack-marketplace"></a>Importieren des Images und Veröffentlichen im Azure Stack-Marketplace
Im Marketplace gibt es drei Arten von Elementen: virtuelle Computer, VM-Erweiterungen und Lösungsvorlagen. Lösungsvorlagen werden weiter unten erläutert.
> [!NOTE]
> VM-Erweiterungen können Azure Stack derzeit nicht hinzugefügt werden.

1. Speichern Sie das heruntergeladene Image und das Katalogpaket sowie die entsprechenden Inhalte im Ordner „AzureStack-Tools-master“ auf einem Wechseldatenträger, und kopieren Sie ihn in die Azure Stack-Umgebung. (Sie können ihn an einen beliebigen lokalen Speicherort kopieren, z.B. „C:\MarketplaceImages“.)     

2. Stellen Sie vor dem Importieren des Images eine Verbindung mit der Umgebung des Azure Stack-Betreibers her. Gehen Sie hierzu wie unter [Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers](azure-stack-powershell-configure-admin.md) beschrieben vor.  

3. Wenn der Download eine kleine VHD-Datei (3 MB) namens „fixed3.vhd“ enthält, handelt es sich dabei um eine Lösungsvorlage. Diese Datei wird nicht benötigt. Fahren Sie mit Schritt 5 fort. Laden Sie alle abhängigen Elemente herunter, wie in der Beschreibung für den Download angegeben.

4. Importieren Sie das Image mithilfe des Cmdlets „Add-AzsVMImage“ in Azure Stack. Ersetzen Sie bei Verwendung dieses Cmdlets die Werte für *Herausgeber*, *Angebot* und andere Parameter durch die Werte des Images, das Sie importieren möchten. Die Werte für *Herausgeber*, *Angebot* und *SKU* des Images können Sie über das Objekt „imageReference“ der zuvor heruntergeladenen AZPKG-Datei ermitteln. Den Wert für *Version* erhalten Sie in Schritt 6 des vorherigen Abschnitts.

Das Objekt „imageReference“ finden Sie, indem Sie die AZPKG-Datei mit der Erweiterung „.ZIP“ umbenennen, die Datei in ein temporäres Verzeichnis extrahieren und die Datei „DeploymentTemplates\CreateUiDefinition.json“ mit einem Text-Editor öffnen. Suchen Sie den folgenden Abschnitt:

   ```json
   "imageReference": {
      "publisher": "MicrosoftWindowsServer",
      "offer": "WindowsServer",
      "sku": "2016-Datacenter-Server-Core"
    }
   ```

   Ersetzen Sie die Parameterwerte, und führen Sie den folgenden Befehl aus:

   ```powershell
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   Add-AzsVMImage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2016.127.20171215" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Windows-Server-2016-DatacenterCore-20171215-en.us-127GB.vhd" `
    -CreateGalleryItem $False `
    -Location Local
   ```

5. Laden Sie Ihr Marketplace-Element (AZPKG-Datei) über das Portal in den Azure Stack-Blobspeicher hoch. Sie können es in Azure Stack-Speicher oder Azure Storage hochladen. (Es handelt sich um einen temporären Speicherort für das Paket.) Vergewissern Sie sich, dass das Blob öffentlich zugänglich ist, und notieren Sie sich den URI.  

6. Verwenden Sie **Add-AzsGalleryItem**, um das Marketplace-Element in Azure Stack zu veröffentlichen. Beispiel: 

   ```powershell
   Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.2.azpkg" `
     –Verbose
   ```

7. Das veröffentlichte Katalogelement können Sie unter **Neu** > **Marketplace** anzeigen. Falls es sich bei dem Download um eine Lösungsvorlage gehandelt hat, laden Sie auch das abhängige VHD-Image herunter.

   ![Marketplace](./media/azure-stack-download-azure-marketplace-item/image06.png)

## <a name="next-steps"></a>Nächste Schritte

[Erstellen und Veröffentlichen eines Marketplace-Elements](azure-stack-create-and-publish-marketplace-item.md)
