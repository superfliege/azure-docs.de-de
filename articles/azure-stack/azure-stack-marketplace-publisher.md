---
title: "Erstellen und Veröffentlichen von Marketplace-Elementen mithilfe des Marketplace-Toolkits | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe des Veröffentlichungstoolkits in kurzer Zeit Marketplace-Elemente erstellen."
services: azure-stack
documentationcenter: 
author: HeathL17
manager: ByronR
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/14/2017
ms.author: helaw
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 5b2c04d2cbc06e1572dc2e40712f6cf9d886aa1e
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---

#  <a name="add-marketplace-items-using-publishing-tool"></a>Hinzufügen von Marketplace-Elementen mithilfe des Veröffentlichungstools
Durch Hinzufügen Ihrer Inhalte zum [Azure Stack-Marketplace](azure-stack-marketplace.md) werden Ihre Lösungen für Sie und Ihre Mandanten für die Bereitstellung verfügbar.  Das Marketplace-Toolkit erstellt Azure Marketplace-Paketdateien (AZPKG-Dateien) auf der Grundlage Ihrer IaaS-Azure Resource Manager-Vorlagen oder VM-Erweiterungen.  Darüber hinaus können Sie mit dem Marketplace-Toolkit AZPKG-Dateien veröffentlichen, die entweder mit dem Tool oder [manuell](azure-stack-create-and-publish-marketplace-item.md) erstellt wurden.  In diesem Thema erfahren Sie, wie Sie das Tool herunterladen, ein Marketplace-Element auf der Grundlage einer VM-Vorlage erstellen und das Element anschließend im Azure Stack-Marketplace veröffentlichen.     


## <a name="prerequisites"></a>Voraussetzungen
 - Das Toolkit muss auf dem Azure Stack-Host ausgeführt werden oder auf dem Computer, auf dem es ausgeführt wird, über [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn)-Konnektivität verfügen.

 - Laden Sie die [Azure Stack-Schnellstartvorlagen](https://github.com/Azure/AzureStack-QuickStart-Templates/archive/master.zip) herunter, und extrahieren Sie sie.

 - Laden Sie das [Azure Gallery Packaging-Tool](http://aka.ms/azurestackmarketplaceitem) (AzureGalleryPackage.exe) herunter. 

 - Für die Veröffentlichung in Marketplace werden Symbole und eine Miniaturansichtsdatei benötigt.  Sie können entweder eigene Inhalte verwenden oder die [Beispieldateien](azure-stack-marketplace-publisher.md#support-files) für dieses Beispiel lokal speichern.

## <a name="download-the-tool"></a>Herunterladen des Tools
Das Marketplace-Toolkit kann aus dem [Repository für Azure Stack-Tools](azure-stack-powershell-download.md) heruntergeladen werden.


##  <a name="create-marketplace-items"></a>Erstellen von Marketplace-Elementen
In diesem Abschnitt erstellen Sie mithilfe des Marketplace-Toolkits ein Marketplace-Elementpaket im AZPKG-Format.  

### <a name="provide-marketplace-information-with-wizard"></a>Angeben von Marketplace-Informationen mithilfe des Assistenten
1. Führen Sie das Marketplace-Toolkit über eine PowerShell-Sitzung aus:
```PowerShell
    .\MarketplaceToolkit.ps1
```

2. Klicken Sie auf die Registerkarte **Lösung**.  Auf diesem Bildschirm können Sie Informationen zu Ihrem Marketplace-Element angeben. Geben Sie die Informationen zu Ihrem Element so ein, wie Sie im Marketplace angezeigt werden sollen.  Sie können auch eine [Parameterdatei](azure-stack-marketplace-publisher.md#use-a-parameters-file) angeben, um das Formular vorab aufzufüllen.  
    
    ![Screenshot des ersten Bildschirms des Marketplace-Toolkits](./media/azure-stack-marketplace-publisher/image7.png)
3. Klicken Sie auf **Durchsuchen**, und wählen Sie für die Symbol- und Screenshotfelder jeweils eine Bilddatei aus.  Verwenden Sie entweder eigene Symbole oder die Beispielsymbole aus dem Abschnitt [Unterstützungsdateien](azure-stack-marketplace-publisher.md#support-files).
4. Füllen Sie alle Felder aus, und wählen Sie anschließend „Preview Solution“ (Lösungsvorschau anzeigen) aus, um eine Vorschau der Lösung im Marketplace anzuzeigen.  Sie können den Text, die Bilder und den Screenshot anpassen und bearbeiten, bevor Sie auf **Weiter** klicken.  

### <a name="import-template-and-create-package"></a>Importieren der Vorlage und Erstellen des Pakets
In diesem Abschnitt importieren Sie die Vorlage und verwenden Eingaben für Ihre Lösung.

1.  Klicken Sie auf **Durchsuchen**, und wählen Sie die Datei *azuredeploy.json* aus dem Ordner „101-Simple-Windows-VM“ in den heruntergeladenen Vorlagen aus.

    ![Screenshot des zweiten Bildschirms des Marketplace-Toolkits](./media/azure-stack-marketplace-publisher/image8.png)
2.  Der Bereitstellungs-Assistent wird mit *einem* einfachen Schritt und Eingabeelementen für die einzelnen Parameter aus der Vorlage aufgefüllt.  Sie können zusätzliche Schritte hinzufügen und Eingaben zwischen Schritten verschieben.  So können Sie beispielsweise die Schritte „Front-End-Konfiguration“ und „Back-End-Konfiguration“ für Ihre Lösung hinzufügen.
3.  Geben Sie den Pfad zur Datei „AzureGalleryPackager.exe“ an.  
4.  Klicken Sie auf **Erstellen**. Daraufhin wird Ihre Lösung vom Marketplace-Toolkit in eine AZPKG-Datei gepackt.  Nach Abschluss des Vorgangs zeigt der Assistent den Pfad zu Ihrer Lösungsdatei an und gibt Ihnen die Möglichkeit, mit der Veröffentlichung Ihres Pakets in Azure Stack fortzufahren.


## <a name="publish-marketplace-items"></a>Veröffentlichen von Marketplace-Elementen
In diesem Abschnitt veröffentlichen Sie das Marketplace-Element in Ihrem Azure Stack-Marketplace.

![Screenshot des ersten Bildschirms des Marketplace-Toolkits](./media/azure-stack-marketplace-publisher/image9.png)

1.  Der Assistent benötigt Informationen, um Ihre Lösung veröffentlichen zu können:
    
    |Feld|Beschreibung|
    |-----|-----|
    | Service Admin Name (Dienstadministratorname) | Das Dienstadministratorkonto.  Beispiel: ServiceAdmin@mydomain.onmicrosoft.com |
    | Kennwort | Das Kennwort für das Dienstadministratorkonto. |
    | API-Endpunkt | Der Azure Stack-Azure Resource Manager-Endpunkt.  Beispiel: management.local.azurestack.external |
2.  Klicken Sie auf **Veröffentlichen**. Das Veröffentlichungsprotokoll wird angezeigt.
3.  Nun können Sie Ihr veröffentlichtes Element über das Azure Stack-Portal bereitstellen.


## <a name="use-a-parameters-file"></a>Verwenden einer Parameterdatei
Die Informationen zum Marketplace-Element können auch mithilfe einer Parameterdatei angegeben werden.  

Das Marketplace-Toolkit enthält die Datei *solution.parameters.ps1*, mit der Sie Ihre eigene Parameterdatei erstellen können.


## <a name="support-files"></a>Unterstützungsdateien
| Beschreibung | Beispiel |
| ----- | ----- |
| PNG-Symbol (40 x 40) | ![](./media/azure-stack-marketplace-publisher/image1.png) |
| PNG-Symbol (90 x 90) | ![](./media/azure-stack-marketplace-publisher/image2.png) |
| PNG-Symbol (115 x 115) | ![](./media/azure-stack-marketplace-publisher/image3.png) |
| PNG-Symbol (255 x 115) | ![](./media/azure-stack-marketplace-publisher/image4.png) |
| PNG-Miniaturansicht (533 x 324) | ![](./media/azure-stack-marketplace-publisher/image5.png) |



