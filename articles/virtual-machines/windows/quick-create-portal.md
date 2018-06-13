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
ms.date: 05/09/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: c28686c3b6494a0cf8938d39ab9b8338de7aa0c1
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012579"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-the-azure-portal"></a>Schnellstart: Erstellen eines virtuellen Windows-Computer im Azure-Portal

Virtuelle Azure-Computer (VMs) können über das Azure-Portal erstellt werden. Bei dieser Methode können Sie die browserbasierte Benutzeroberfläche nutzen, um virtuelle Computer und ihre dazugehörigen Ressourcen zu erstellen. In dieser Schnellstartanleitung wird gezeigt, wie Sie über das Azure-Portal einen virtuellen Computer unter Windows Server 2016 in Azure bereitstellen. Um den virtuellen Computer in Aktion zu sehen, stellen Sie anschließend eine RDP-Verbindung mit dem virtuellen Computer her und installieren den IIS-Webserver.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.

2. Suchen Sie im Suchfeld oberhalb der Liste der Azure Marketplace-Ressourcen nach **Windows Server 2016 Datacenter**, und klicken Sie auf **Erstellen**.

3. Geben Sie einen Namen für den virtuellen Computer ein, etwa *myVM*, übernehmen Sie den Datenträgertyp *SSD*, und geben Sie einen Benutzernamen (etwa *azureuser*) an. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](faq.md#what-are-the-password-requirements-when-creating-a-vm) erfüllen.

    ![Eingeben grundlegender Informationen zu Ihrem virtuellen Computer im Portalblatt](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)

5. Klicken Sie unter „Ressourcengruppe“ auf **Neu erstellen**, und geben Sie einen Namen (etwa *myResourceGroup*) ein. Wählen Sie den gewünschten **Standort**, und klicken Sie dann auf **OK**.

4. Wählen Sie eine Größe für den virtuellen Computer. Sie können beispielsweise nach *Computetyp* oder *Datenträgertyp* filtern. Eine mögliche VM-Größe ist beispielsweise *D2s_v3*.

    ![Screenshot: VM-Größen](./media/quick-create-portal/create-windows-vm-portal-sizes.png)

5. Übernehmen Sie unter **Einstellungen** die Standardwerte, und klicken Sie auf **OK**.

6. Wählen Sie auf der Seite „Zusammenfassung“ die Option **Erstellen** aus, um die Bereitstellung des virtuellen Computers zu starten.

7. Der virtuelle Computer wird auf dem Dashboard des Azure-Portals angeheftet. Nach Abschluss der Bereitstellung wird automatisch die VM-Zusammenfassung geöffnet.

## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Erstellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer. In dieser Anleitung wird erläutert, wie Sie über einen Windows-Computer eine Verbindung mit Ihrem virtuellen Computer herstellen. Auf einem Macintosh benötigen Sie einen RDP-Client, z.B. diesen [Remotedesktopclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) aus dem Mac App Store.

1. Klicken Sie auf der Seite mit den Eigenschaften des virtuellen Computers auf die Schaltfläche **Verbinden**. 

    ![Herstellen einer Verbindung mit einem virtuellen Azure-Computer über das Portal](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png)
    
2. Übernehmen Sie auf der Seite zum **Herstellen der Verbindung mit dem virtuellen Computer** die Standardoptionen, um basierend auf dem DNS-Namen eine Verbindung über Port 3389 herzustellen. Klicken Sie anschließend auf **RDP-Datei herunterladen**.

2. Öffnen Sie die heruntergeladene RDP-Datei, und klicken Sie auf **Verbinden**, wenn Sie dazu aufgefordert werden. 

3. Wählen Sie im Fenster **Windows-Sicherheit** die Option **Weitere Optionen** und dann **Anderes Konto verwenden** aus. Geben Sie den Benutzernamen im Format „*VM-Name*\*Benutzername*“ sowie das Kennwort ein, das Sie für den virtuellen Computer erstellt haben, und klicken Sie dann auf **OK**.

4. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.

## <a name="install-web-server"></a>Installieren des Webservers

Wenn Sie den virtuellen Computer in Aktion sehen möchten, installieren Sie den IIS-Webserver. Öffnen Sie eine PowerShell-Eingabeaufforderung auf dem virtuellen Computer, und führen Sie den folgenden Befehl aus:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Beenden Sie anschließend die RDP-Verbindung mit dem virtuellen Computer.

## <a name="open-port-80-for-web-traffic"></a>Öffnen von Port 80 für Webdatenverkehr

Mit einer Netzwerksicherheitsgruppe (NSG) wird eingehender und ausgehender Datenverkehr geschützt. Wenn im Azure-Portal eine VM erstellt wird, wird für RDP-Verbindungen an Port 3389 eine Regel für eingehenden Datenverkehr erstellt. Da dieser virtuelle Computer einen Webserver hostet, muss für Port 80 eine NSG-Regel erstellt werden.

1. Klicken Sie auf der Übersichtsseite des virtuellen Computers auf **Netzwerk**.
2. Die Liste der Regeln für eingehenden und ausgehenden Datenverkehr wird angezeigt. Klicken Sie auf **Add inbound port rule** (Regel für Eingangsport hinzufügen).
3. Wählen Sie oben die Option **Basic** und anschließend in der Liste der verfügbaren Dienste *HTTP* aus. Port 80, eine Priorität und ein Name werden für Sie bereitgestellt.
4. Klicken Sie zum Erstellen der Regel auf **Hinzufügen**.

## <a name="view-the-iis-welcome-page"></a>Anzeigen der IIS-Willkommensseite

Nachdem Sie IIS installiert und Port 80 auf Ihrem virtuellen Computer für den Zugriff über das Internet geöffnet haben, zeigen Sie nun mit einem Webbrowser Ihrer Wahl die IIS-Standardwillkommensseite an. Verwenden Sie die öffentliche IP-Adresse Ihres virtuellen Computers, die Sie in einem vorherigen Schritt abgerufen haben. Im folgenden Beispiel ist die IIS-Standardwebsite dargestellt:

![IIS-Standardwebsite](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie löschen. Wählen Sie dazu die Ressourcengruppe für den virtuellen Computer aus, klicken Sie auf **Löschen**, und bestätigen Sie den Namen der zu löschenden Ressourcengruppe.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen einfachen virtuellen Computer bereitgestellt, einen Netzwerkport für den Webdatenverkehr geöffnet und einen einfachen Webserver installiert. Fahren Sie mit dem Tutorial für virtuelle Windows-Computer fort, um weitere Informationen zu virtuellen Azure-Computern zu erhalten.

> [!div class="nextstepaction"]
> [Azure-Tutorials zu virtuellen Windows-Computern](./tutorial-manage-vm.md)
