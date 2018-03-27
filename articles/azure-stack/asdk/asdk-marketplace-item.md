---
title: Hinzufügen eines Azure Stack-Marketplace-Elements aus Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie dem Azure Stack-Marketplace ein Azure-basiertes Windows Server-VM-Image hinzufügen.
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
ms.openlocfilehash: 25f179f825526e20377c0e94ccb38a788cadd898
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-add-an-azure-stack-marketplace-item-from-azure"></a>Tutorial: Hinzufügen eines Azure Stack-Marketplace-Elements aus Azure

Als Azure Stack-Betreiber müssen Sie zunächst dem Azure Stack-Marketplace ein VM-Image hinzufügen, um Benutzern die Bereitstellung eines virtuellen Computers (Virtual Machine, VM) zu ermöglichen. Standardmäßig wird nichts im Azure Stack-Marketplace veröffentlicht. Sie können jedoch Elemente aus einer [Liste ausgewählter Azure Marketplace-Elemente](.\.\azure-stack-marketplace-azure-items.md) herunterladen, die vorab mit Azure Stack getestet wurden. Verwenden Sie diese Option, wenn Sie ein verbundenes Szenario nutzen und Ihre Azure Stack-Instanz bei Azure registriert haben.

In diesem Tutorial fügen Sie das Windows Server 2016-VM-Image aus dem Azure Marketplace dem Azure Stack-Marketplace hinzu.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen eines Azure Stack-Marketplace-Elements in Form eines virtuellen Windows Server-Computers
> * Überprüfen der Verfügbarkeit des VM-Images 
> * Testen des VM-Images

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie zur Vorbereitung dieses Tutorials die folgenden Schritte aus:

- Installieren Sie die [mit Azure Stack kompatiblen Azure PowerShell-Module](asdk-post-deploy.md#install-azure-stack-powershell).
- Laden Sie die [Azure Stack-Tools](asdk-post-deploy.md#download-the-azure-stack-tools) herunter.
- [Registrieren Sie das ASDK](asdk-register.md) bei Ihrem Azure-Abonnement.

## <a name="add-a-windows-server-vm-image"></a>Hinzufügen eines Windows Server-VM-Images
Sie können dem Azure Stack-Marketplace ein Windows Server 2016-Image hinzufügen, indem Sie es aus dem Azure Marketplace herunterladen. Verwenden Sie diese Option, wenn Sie ein verbundenes Szenario nutzen und bereits [Ihre Azure Stack-Instanz bei Azure registriert](asdk-register.md) haben.

1. Melden Sie sich beim [ASDK-Administratorportal](https://adminportal.local.azurestack.external) an.

2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung) > **Add from Azure** (Aus Azure hinzufügen). 

   ![Hinzufügen aus Azure](media/asdk-marketplace-item/azs-marketplace.png)

3. Suchen Sie nach dem Image **Windows Server 2016 Datacenter**.

4. Wählen Sie das Image **Windows Server 2016 Datacenter** aus, und klicken Sie auf **Herunterladen**.

   ![Herunterladen eines Images von Azure](media/asdk-marketplace-item/azure-marketplace-ws2016.png)


> [!NOTE]
> Das Herunterladen des Images sowie das Veröffentlichen im Azure Stack-Marketplace dauert etwa eine Stunde. 

Nach Abschluss des Downloadvorgangs wird das Image veröffentlicht und steht unter **Marketplace Management** (Marketplace-Verwaltung) zur Verfügung. Das Image ist auch unter **Compute** für die Erstellung neuer virtueller Computer verfügbar.

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
> * Hinzufügen eines Azure Stack-Marketplace-Elements in Form eines virtuellen Windows Server-Computers
> * Überprüfen der Verfügbarkeit des VM-Images 
> * Testen des VM-Images

Im nächsten Tutorial erfahren Sie, wie Sie ein Azure Stack-Angebot und einen entsprechenden Plan erstellen.

> [!div class="nextstepaction"]
> [Anbieten von Azure Stack-IaaS-Diensten](asdk-offer-services.md)