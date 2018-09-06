---
title: Azure Stack-Schnellstart – Erstellen eines virtuellen Windows-Computers
description: Azure Stack-Schnellstart – Erstellen eines virtuellen Windows-Computers mit dem Portal
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 08/15/2018
ms.author: mabrigg
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: efe6213e5c0261fb26ac40e74c2b0f6e0c9252dd
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "43775058"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-with-the-azure-stack-portal"></a>Schnellstart: Erstellen eines virtuellen Windows Server-Computers im Azure Stack-Portal

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können einen virtuellen Windows Server 2016-Computer im Azure Stack-Portal erstellen. Befolgen Sie die Schritte in diesem Artikel zum Erstellen und Verwenden eines virtuellen Computers.

## <a name="sign-in-to-the-azure-stack-portal"></a>Anmelden beim Azure Stack-Portal

Melden Sie sich beim Azure Stack-Portal an. Die Adresse des Azure Stack-Portals hängt davon ab, mit welchem Azure Stack-Produkt Sie eine Verbindung herstellen:

* Verwenden Sie für das Azure Stack Development Kit (ASDK) die folgende Adresse: https://portal.local.azurestack.external.
* Rufen Sie bei einem integrierten Azure Stack-System die vom Azure Stack-Operator bereitgestellte URL auf.

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Klicken Sie im Portal auf **Neu** > **Compute** > **Windows Server 2016 Datacenter Eval** > **Erstellen**. Wenn Sie den Eintrag **Windows Server 2016 Datacenter Eval** nicht sehen, wenden Sie sich an Ihren Azure Stack-Operator. Bitten Sie ihn, diesen dem Marketplace hinzuzufügen, wie im Artikel [Hinzufügen des VM-Images für Windows Server 2016 zum Azure Stack-Marketplace](../azure-stack-add-default-image.md) beschrieben.

    ![Schritte zum Erstellen eines virtuellen Windows-Computers im Portal](media/azure-stack-quick-windows-portal/image01.png)
2. Geben Sie unter **Grundlagen** Werte für **Name**, **Benutzername** und **Kennwort** ein. Wählen Sie ein **Abonnement**aus. Erstellen Sie eine **Ressourcengruppe**, oder wählen Sie eine vorhandene aus, wählen Sie einen **Speicherort** aus, und klicken Sie anschließend auf **OK**.

    ![Grundeinstellungen konfigurieren](media/azure-stack-quick-windows-portal/image02.png)
3. Klicken Sie unter **Größe auswählen** auf **D1 Standard** > **Auswählen**.
    ![Auswählen der Größe des virtuellen Computers](media/azure-stack-quick-windows-portal/image03.png)
4. Übernehmen Sie unter **Einstellungen** die Standardwerte, und klicken Sie auf **OK**.
    ![Konfigurieren der Einstellungen des virtuellen Computers](media/azure-stack-quick-windows-portal/image04.png)
5. Klicken Sie unter **Zusammenfassung** auf **OK**, um den virtuellen Computer zu erstellen.
    ![Anzeigen der Zusammenfassung und Erstellen des virtuellen Computers](media/azure-stack-quick-windows-portal/image05.png)
6. Klicken Sie zum Anzeigen Ihres neuen virtuellen Computers auf **Alle Ressourcen**, suchen Sie nach dem Namen des virtuellen Computers, und klicken Sie in den Suchergebnissen auf seinen Namen.
    ![Anzeigen des virtuellen Computers](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Nutzung des virtuellen Computers abgeschlossen ist, löschen Sie den virtuellen Computer und seine Ressourcen. Wählen Sie hierzu auf der Seite des virtuellen Computers die Ressourcengruppe aus, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen einfachen virtuellen Windows Server-Computer bereitgestellt. Um weitere Informationen zu virtuellen Computern unter Azure Stack zu erhalten, fahren Sie mit [Considerations for Virtual Machines in Azure Stack](azure-stack-vm-considerations.md) (Überlegungen zu virtuellen Computern in Azure Stack) fort.
