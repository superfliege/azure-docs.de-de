---
title: 'Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure-Portal | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das Azure-Portal zum Erstellen einer Azure Application Gateway-Instanz verwenden, mit der Webdatenverkehr an virtuelle Computer in einem Back-End-Pool geleitet wird.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 1/8/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 16e23f77509d2402f765981b39a30e08a2309f68
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2019
ms.locfileid: "54156526"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-portal"></a>Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure-Portal

In dieser Schnellstartanleitung wird gezeigt, wie Sie das Azure-Portal zum schnellen Erstellen des Anwendungsgateways mit zwei virtuellen Computern im Back-End-Pool verwenden. Anschließend führen Sie einen Test durch, um sicherzustellen, dass alles richtig funktioniert. Per Azure Application Gateway leiten Sie den Webdatenverkehr Ihrer Anwendungen an bestimmte Ressourcen, indem Sie Ports Listener zuweisen, Regeln erstellen und Ressourcen einem Back-End-Pool hinzufügen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Für die Kommunikation in Azure zwischen den von Ihnen erstellten Ressourcen ist ein virtuelles Netzwerk erforderlich. In diesem Beispiel erstellen Sie zwei Subnetze: eins für das Anwendungsgateway und eins für die Back-End-Server. Sie können ein virtuelles Netzwerk zum gleichen Zeitpunkt erstellen wie das Anwendungsgateway.

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressource erstellen**. Das Fenster **Neu** wird angezeigt.

2. Klicken Sie auf **Netzwerk**, und wählen Sie dann in der Liste **Ausgewählte** die Option **Application Gateway** aus.

### <a name="basics-page"></a>Seite „Grundlagen“

1. Geben Sie auf der Seite **Grundlagen** die folgenden Werte für die Einstellungen des Anwendungsgateways ein:

    - **Name**: Geben Sie *myAppGateway* als Namen des Anwendungsgateways ein.
    - **Ressourcengruppe**: Wählen Sie **myResourceGroupAG** als Ressourcengruppe aus. Falls diese Gruppe nicht vorhanden ist, wählen Sie **Neue erstellen** aus, um sie zu erstellen.

    ![Erstellen eines neuen Anwendungsgateways](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

2. Übernehmen Sie die Standardwerte für die anderen Einstellungen, und klicken Sie dann auf **OK**.

### <a name="settings-page"></a>Seite "Einstellungen"

1. Wählen Sie auf der Seite **Einstellungen** unter **Subnetzkonfiguration** die Option **Virtuelles Netzwerk auswählen** aus.

2. Wählen Sie auf der Seite **Virtuelles Netzwerk auswählen** die Option **Neu erstellen** aus, und geben Sie Werte für die folgenden Einstellungen des virtuellen Netzwerks ein:

    - **Name**: Geben Sie *myVNet* als Namen des virtuellen Netzwerks ein.

    - **Adressraum:** Geben Sie *10.0.0.0/16* für den Adressraum des virtuellen Netzwerks ein.

    - **Subnetzname**: Geben Sie *myAGSubnet* als Subnetznamen ein.<br>Das Subnetz für das Anwendungsgateway kann nur Anwendungsgateways enthalten. Andere Ressourcen sind nicht zulässig.

    - **Subnetzadressbereich:** Geben Sie *10.0.0.0/24* für den Subnetzadressbereich ein.

    ![Virtuelles Netzwerk erstellen](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

3. Klicken Sie auf **OK**, um zur Seite **Einstellungen** zurückzukehren.

4. Vergewissern Sie sich unter **Frontend-IP-Konfiguration**, dass **IP-Adresstyp** auf **Öffentlich** festgelegt ist. Vergewissern Sie sich unter **Öffentliche IP-Adresse**, dass **Neue erstellen** ausgewählt ist. 

5. Geben Sie *myAGPublicIPAddress* als Namen für die öffentliche IP-Adresse ein. 

6. Übernehmen Sie die Standardwerte für die anderen Einstellungen, und klicken Sie dann auf **OK**.

### <a name="summary-page"></a>Seite „Zusammenfassung“

Überprüfen Sie die Einstellungen auf der Seite **Zusammenfassung**, und klicken Sie dann auf **OK**, um das virtuelle Netzwerk, die öffentliche IP-Adresse und das Anwendungsgateway zu erstellen. Die Erstellung des Anwendungsgateways in Azure kann einige Minuten in Anspruch nehmen. Warten Sie, bis die Bereitstellung erfolgreich abgeschlossen ist, bevor Sie mit dem nächsten Abschnitt fortfahren.

## <a name="add-a-subnet"></a>Hinzufügen eines Subnetzes

Fügen Sie dem virtuellen Netzwerk, das Sie erstellt haben, wie folgt ein Subnetz hinzu:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Alle Ressourcen** aus, geben Sie *myVNet* in das Suchfeld ein, und wählen Sie dann in den Suchergebnissen **myVNet** aus.

2. Wählen Sie im Menü auf der linken Seite die Option **Subnetze** aus, und klicken Sie dann auf **+ Subnetz**. 

    ![Erstellen eines Subnetzes](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Geben Sie auf der Seite **Subnetz hinzufügen** als **Namen** des Subnetzes *myBackendSubnet* ein, und klicken Sie auf **OK**.

## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Beispiel erstellen Sie zwei VMs, die von Azure als Back-End-Server für das Anwendungsgateway verwendet werden. Sie installieren außerdem IIS auf den VMs, um zu überprüfen, ob Azure das Anwendungsgateway erfolgreich erstellt hat.

### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**. Das Fenster **Neu** wird angezeigt.

2. Klicken Sie auf **Compute**, und wählen Sie dann in der Liste **Ausgewählte** die Option **Windows Server 2016 Datacenter** aus. Die Seite **Virtuellen Computer erstellen** wird angezeigt.

3. Geben Sie auf der Registerkarte **Grundlagen** die folgenden Werte für die VM-Einstellungen ein:

    - **Ressourcengruppe**: Wählen Sie **myResourceGroupAG** als Namen der Ressourcengruppe aus.
    - **Name des virtuellen Computers**: Geben Sie *myVM* als Namen der VM ein.
    - **Benutzername**: Geben *azureuser* als Namen des Administratorbenutzers ein.
    - **Kennwort**: Geben Sie *Azure123456!* als Administratorkennwort ein.

4. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Weiter: Datenträger**.  

5. Übernehmen Sie auf der Registerkarte **Datenträger** die Standardwerte, und klicken Sie auf **Weiter: Netzwerk**.

6. Vergewissern Sie sich auf der Registerkarte **Netzwerk**, dass **myVNet** für **Virtuelles Netzwerk** ausgewählt und **Subnetz** auf **myBackendSubnet** festgelegt ist. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Weiter: Verwaltung** aus.

8. Legen Sie auf der Registerkarte **Verwaltung** die Option **Startdiagnose** auf **Aus** fest. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Bewerten + erstellen**.

9. Überprüfen Sie auf der Registerkarte **Bewerten + erstellen** die Einstellungen, beheben Sie alle Validierungsfehler, und wählen Sie dann **Erstellen** aus.

10. Warten Sie, bis die Erstellung des virtuellen Computers abgeschlossen ist, bevor Sie fortfahren.

### <a name="install-iis"></a>Installieren von IIS

1. Öffnen Sie [Azure PowerShell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell). Wählen Sie dazu in der oberen Navigationsleiste des Azure-Portals **Cloud Shell** und dann in der Dropdownliste **PowerShell** aus. 

    ![Installieren der benutzerdefinierten Erweiterung](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

2. Führen Sie den folgenden Befehl aus, um IIS auf dem virtuellen Computer zu installieren: 

    ```azurepowershell-interactive
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. Erstellen Sie eine zweite VM, und installieren Sie IIS mithilfe der zuvor ausgeführten Schritte. Verwenden Sie *myVM2* für den VM-Namen und für die Einstellung **VMName** des Cmdlets **Set-AzureRmVMExtension**.

### <a name="add-backend-servers"></a>Hinzufügen von Back-End-Servern

1. Wählen Sie **Alle Ressourcen** und dann **myAppGateway** aus.

2. Klicken Sie im Menü auf der linken Seite auf **Back-End-Pools**. Azure erstellt automatisch einen Standardpool **appGatewayBackendPool**, wenn Sie das Anwendungsgateway erstellen. 

3. Wählen Sie **appGatewayBackendPool** aus.

4. Wählen Sie unter **Ziele** in der Dropdownliste die Option **Virtueller Computer** aus.

5. Wählen Sie unter **VIRTUELLER COMPUTER** und **NETZWERKSCHNITTSTELLEN** in den Dropdownlisten die VMs **myVM** und **myVM2** sowie die zugehörigen Netzwerkschnittstellen aus.

    ![Hinzufügen von Back-End-Servern](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. Wählen Sie **Speichern** aus.

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

1. Suchen Sie auf der Seite **Übersicht** des Anwendungsgateways nach der öffentlichen IP-Adresse für das Anwendungsgateway.

    ![Notieren der öffentlichen IP-Adresse des Anwendungsgateways](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

    Alternativ können Sie **Alle Ressourcen** auswählen, *myAGPublicIPAddress* in das Suchfeld eingeben und den Eintrag dann in den Suchergebnissen auswählen. Azure zeigt die öffentliche IP-Adresse auf der Seite **Übersicht** an.

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein.

    ![Testen des Anwendungsgateways](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem Anwendungsgateway erstellten Ressourcen nicht mehr benötigen, entfernen Sie die Ressourcengruppe. Beim Entfernen der Ressourcengruppe werden auch das Anwendungsgateway und alle zugehörigen Ressourcen entfernt. 

So entfernen Sie die Ressourcengruppe:
1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Ressourcengruppen** aus.
2. Suchen Sie auf der Seite **Ressourcengruppen** in der Liste nach **myResourceGroupAG**, und wählen Sie den Eintrag aus.
3. Wählen Sie auf der Seite für die **Ressourcengruppe** die Option **Ressourcengruppe löschen** aus.
4. Geben Sie *myResourceGroupAG* für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten von Webdatenverkehr mit einem Anwendungsgateway per Azure CLI](./tutorial-manage-web-traffic-cli.md)
