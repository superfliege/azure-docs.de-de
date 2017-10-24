---
title: "Azure Stack-Schnellstart – Erstellen eines virtuellen Windows-Computers"
description: "Azure Stack-Schnellstart – Erstellen eines virtuellen Windows-Computers mit dem Portal"
services: azure-stack
author: ErikjeMS
manager: byronr
ms.service: azure-stack
ms.topic: azure-stack
ms.date: 09/15/2017
ms.author: erikje
ms.custom: mvc
ms.openlocfilehash: abca538f28bbc0a8f3f00311ca1a69d196f10272
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-virtual-machine-with-the-azure-stack-portal"></a>Erstellen eines virtuellen Windows-Computers mit dem Azure Stack-Portal

Sie können einen virtuellen Windows-Computer mit dem Azure Stack-Portal erstellen. Das Portal ist eine browserbasierte Benutzeroberfläche, in der Sie Ressourcen erstellen, konfigurieren und verwalten können.

## <a name="sign-in-to-the-azure-stack-portal"></a>Anmelden beim Azure Stack-Portal

Melden Sie sich beim Azure Stack-Portal an. Die Adresse des Azure Stack-Portals hängt davon ab, mit welchem Azure Stack-Produkt Sie eine Verbindung herstellen:

* Für Azure Stack Development Kit (ASDK) gehen Sie zu: „https://portal.local.azurestack.external“.
* Rufen Sie bei einem integrierten Azure Stack-System die vom Azure Stack-Operator bereitgestellte URL auf.

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Klicken Sie im Portal auf **Neu** > **Compute** > **Windows Server 2016 Datacenter Eval** > **Erstellen**. Wenn Sie den Eintrag **Windows Server 2016 Datacenter Eval** nicht sehen, wenden Sie sich an Ihren Azure Stack-Operator. Bitten Sie ihn, diesen dem Marketplace hinzuzufügen, wie im Artikel [Hinzufügen des VM-Images für Windows Server 2016 zum Azure Stack-Marketplace](../azure-stack-add-default-image.md) beschrieben. 
    ![](media/azure-stack-quick-windows-portal/image01.png)
2. Geben Sie unter **Grundlagen** Werte für **Name**, **Benutzername** und **Kennwort** ein. Wählen Sie ein **Abonnement**aus. Erstellen Sie eine **Ressourcengruppe**, oder wählen Sie eine vorhandene aus, wählen Sie einen **Speicherort** aus, und klicken Sie anschließend auf **OK**.

    ![](media/azure-stack-quick-windows-portal/image02.png)
3. Klicken Sie unter **Größe auswählen** auf **D1 Standard** > **Auswählen**.
    ![](media/azure-stack-quick-windows-portal/image03.png)
4. Übernehmen Sie unter **Einstellungen** die Standardwerte, und klicken Sie auf **OK**.
    ![](media/azure-stack-quick-windows-portal/image04.png)
5. Klicken Sie unter **Zusammenfassung** auf **OK**, um den virtuellen Computer zu erstellen. 
    ![](media/azure-stack-quick-windows-portal/image05.png)
6. Klicken Sie zum Anzeigen Ihres neuen virtuellen Computers auf **Alle Ressourcen**, und suchen Sie dann nach dem virtuellen Computer. Klicken Sie auf seinen Namen.
    ![](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie Ressourcengruppe, VM und alle zugehörigen Ressourcen nicht mehr benötigen, löschen Sie sie. Wählen Sie hierzu die Ressourcengruppe auf der Seite des virtuellen Computers aus, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie eine einfache Windows-VM bereitgestellt. Um weitere Informationen zu virtuellen Computern unter Azure Stack zu erhalten, fahren Sie mit [Considerations for Virtual Machines in Azure Stack](azure-stack-vm-considerations.md) (Überlegungen zu virtuellen Computern in Azure Stack) fort.
