---
title: 'Schnellstart: Erstellen eines virtuellen Windows-Computers im Azure-Portal | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe des Azure-Portals einen virtuellen Windows-Computer erstellen.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/03/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f5a92d421bbf7bfe485252c148d5f64ae2fb8e23
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916114"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-the-azure-portal"></a>Schnellstart: Erstellen eines virtuellen Windows-Computer im Azure-Portal

Virtuelle Azure-Computer (VMs) können über das Azure-Portal erstellt werden. Bei dieser Methode können Sie die browserbasierte Benutzeroberfläche nutzen, um virtuelle Computer und ihre dazugehörigen Ressourcen zu erstellen. In dieser Schnellstartanleitung wird gezeigt, wie Sie über das Azure-Portal einen virtuellen Computer unter Windows Server 2016 in Azure bereitstellen. Wenn Sie den virtuellen Computer in Aktion sehen möchten, stellen Sie anschließend eine RDP-Verbindung mit dem virtuellen Computer her und installieren den IIS-Webserver.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.

2. Suchen Sie im Suchfeld oberhalb der Liste der Azure Marketplace-Ressourcen nach **Windows Server 2016 Datacenter**, und klicken Sie auf **Erstellen**.

3. Geben Sie einen Namen für den virtuellen Computer ein, etwa *myVM*, übernehmen Sie den Datenträgertyp *SSD*, und geben Sie einen Benutzernamen (etwa *azureuser*) an. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](faq.md#what-are-the-password-requirements-when-creating-a-vm) erfüllen.

    ![Eingeben grundlegender Informationen zu Ihrem virtuellen Computer im Portalblatt](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)

5. Klicken Sie unter „Ressourcengruppe“ auf **Neu erstellen**, und geben Sie einen Namen (etwa *myResourceGroup*) ein. Wählen Sie den **Standort**, und klicken Sie dann auf **OK**.

4. Wählen Sie eine Größe für den virtuellen Computer. Sie können beispielsweise nach *Computetyp* oder *Datenträgertyp* filtern. Eine mögliche VM-Größe ist beispielsweise *D2s_v3*. Klicken Sie auf **Auswählen**, nachdem Sie eine Größe gewählt haben.

    ![Screenshot: VM-Größen](./media/quick-create-portal/create-windows-vm-portal-sizes.png)

5. Wählen Sie auf der Seite **Einstellungen** unter **Netzwerk** > **Netzwerksicherheitsgruppe** > **Öffentliche Eingangsports hinzufügen** in der Dropdownliste die Optionen **HTTP** und **RDP (3389)**. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **OK**.

6. Wählen Sie auf der Seite „Zusammenfassung“ die Option **Erstellen** aus, um die Bereitstellung des virtuellen Computers zu starten.

7. Der virtuelle Computer wird auf dem Dashboard des Azure-Portals angeheftet. Nach Abschluss der Bereitstellung wird automatisch die VM-Zusammenfassung geöffnet.

## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Erstellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer. In dieser Anleitung wird erläutert, wie Sie über einen Windows-Computer eine Verbindung mit Ihrem virtuellen Computer herstellen. Auf einem Macintosh benötigen Sie einen RDP-Client, z.B. diesen [Remotedesktopclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) aus dem Mac App Store.

1. Klicken Sie auf der Seite mit den Eigenschaften des virtuellen Computers auf die Schaltfläche **Verbinden**. 

    ![Herstellen einer Verbindung mit einem virtuellen Azure-Computer über das Portal](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png)
    
2. Übernehmen Sie auf der Seite zum **Herstellen der Verbindung mit dem virtuellen Computer** die Standardoptionen, um basierend auf dem DNS-Namen eine Verbindung über Port 3389 herzustellen. Klicken Sie anschließend auf **RDP-Datei herunterladen**.

2. Öffnen Sie die heruntergeladene RDP-Datei, und klicken Sie auf **Verbinden**, wenn Sie dazu aufgefordert werden. 

3. Wählen Sie im Fenster **Windows-Sicherheit** die Option **Weitere Optionen** und dann **Anderes Konto verwenden** aus. Geben Sie den Benutzernamen im Format *VM-Name*\\*Benutzername* sowie das Kennwort ein, das Sie für den virtuellen Computer erstellt haben, und klicken Sie dann auf **OK**.

4. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** oder **Weiter**, um die Verbindung zu erstellen.

## <a name="install-web-server"></a>Installieren des Webservers

Installieren Sie den IIS-Webserver, um den virtuellen Computer in Aktion zu sehen. Öffnen Sie eine PowerShell-Eingabeaufforderung auf dem virtuellen Computer, und führen Sie den folgenden Befehl aus:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Beenden Sie anschließend die RDP-Verbindung mit dem virtuellen Computer.


## <a name="view-the-iis-welcome-page"></a>Anzeigen der IIS-Willkommensseite

Wählen Sie im Portal die VM aus, und verwenden Sie in der Übersicht der VM die Schaltfläche **Klicken Sie zum Kopieren** rechts von der IP-Adresse, um sie zu kopieren und in eine Browserregisterkarte einzufügen. Die Standardbegrüßungsseite für IIS wird geöffnet und sollte wie folgt aussehen:

![IIS-Standardwebsite](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie löschen. Wählen Sie dazu die Ressourcengruppe für den virtuellen Computer aus, klicken Sie auf **Löschen**, und bestätigen Sie den Namen der zu löschenden Ressourcengruppe.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen einfachen virtuellen Computer bereitgestellt, einen Netzwerkport für den Webdatenverkehr geöffnet und einen einfachen Webserver installiert. Fahren Sie mit dem Tutorial für virtuelle Windows-Computer fort, um weitere Informationen zu virtuellen Azure-Computern zu erhalten.

> [!div class="nextstepaction"]
> [Azure-Tutorials zu virtuellen Windows-Computern](./tutorial-manage-vm.md)
