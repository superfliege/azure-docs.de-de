---
title: 'Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure-Portal | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das Azure-Portal zum Erstellen einer Azure Application Gateway-Instanz verwenden, mit der Webdatenverkehr an virtuelle Computer in einem Back-End-Pool geleitet wird.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 11/15/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 7e11affece7e7eb133aa22e159ec07d4f15e96f7
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52999603"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-portal"></a>Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure-Portal

Per Azure Application Gateway können Sie Ihren Webdatenverkehr für Anwendungen an bestimmte Ressourcen leiten, indem Sie Ports Listener zuweisen, Regeln erstellen und Ressourcen einem Back-End-Pool hinzufügen.

In dieser Schnellstartanleitung wird gezeigt, wie Sie das Azure-Portal zum schnellen Erstellen des Anwendungsgateways mit zwei virtuellen Computern im Back-End-Pool verwenden. Anschließend führen Sie einen Test durch, um sicherzustellen, dass alles richtig funktioniert.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Für die Kommunikation zwischen den von Ihnen erstellten Ressourcen ist ein virtuelles Netzwerk erforderlich. In diesem Beispiel werden zwei Subnetze erstellt: eins für das Anwendungsgateway und eins für die Back-End-Server. Sie können ein virtuelles Netzwerk zum gleichen Zeitpunkt erstellen wie das Anwendungsgateway.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf **Ressource erstellen**.
2. Klicken Sie auf **Netzwerk** und dann in der Liste mit den ausgewählten Elementen auf **Application Gateway**.

### <a name="basics"></a>Grundlagen

1. Geben Sie die folgenden Werte für das Anwendungsgateway ein:

    - *myAppGateway*: Name des Anwendungsgateways
    - *myResourceGroupAG*: neue Ressourcengruppe

    ![Erstellen eines neuen Anwendungsgateways](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

2. Übernehmen Sie die Standardwerte für die anderen Einstellungen, und klicken Sie auf **OK**.

### <a name="settings"></a>Einstellungen

1. Klicken Sie auf **Virtuelles Netzwerk auswählen** und dann auf **Neu erstellen**, und geben Sie dann die folgenden Werte für das virtuelle Netzwerk ein:

    - *myVNet*: Name des virtuellen Netzwerks
    - *10.0.0.0/16*: Adressraum des virtuellen Netzwerks
    - *myAGSubnet*: Subnetzname
    - *10.0.0.0/24*: Subnetzadressbereich

    ![Virtuelles Netzwerk erstellen](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

6. Klicken Sie auf **OK**, um zurück auf die Seite „Einstellungen“ zu wechseln.
7. Stellen Sie unter **Front-End-IP-Konfiguration** sicher, dass **IP-Adresstyp** auf **Öffentlich** festgelegt ist. Achten Sie außerdem darauf, dass unter **Öffentliche IP-Adresse** die Option **Neu erstellen** ausgewählt ist. Geben Sie *myAGPublicIPAddress* als Namen für die öffentliche IP-Adresse ein. Übernehmen Sie die Standardwerte für die anderen Einstellungen, und klicken Sie auf **OK**.

### <a name="summary"></a>Zusammenfassung

Überprüfen Sie die Einstellungen auf der Zusammenfassungsseite, und klicken Sie dann auf **OK**, um das virtuelle Netzwerk, die öffentliche IP-Adresse und das Anwendungsgateway zu erstellen. Es kann einige Minuten dauern, bis das Anwendungsgateway erstellt wird. Warten Sie, bis die Bereitstellung erfolgreich abgeschlossen ist, bevor Sie mit dem nächsten Abschnitt fortfahren.

## <a name="add-a-subnet"></a>Hinzufügen eines Subnetzes

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myVNet**.
2. Klicken Sie auf **Subnetze** und anschließend auf **+ Subnetz**.

    ![Erstellen eines Subnetzes](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Geben Sie als Name des Subnetzes *myBackendSubnet* ein, und klicken Sie auf **OK**.

## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Beispiel erstellen Sie zwei virtuelle Computer, die als Back-End-Server für das Anwendungsgateway verwendet werden. Sie installieren außerdem IIS auf den virtuellen Computern, um zu überprüfen, ob das Anwendungsgateway erfolgreich erstellt wurde.

### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Klicken Sie auf **Compute**, und wählen Sie dann in der Liste der ausgewählten Elemente die Option **Windows Server 2016 Datacenter**.
3. Geben Sie die folgenden Werte für den virtuellen Computer ein:

    - *myResourceGroupAG*: Ressourcengruppe
    - *myVM*: Name des virtuellen Computers
    - *azureuser*: Name des Administratorbenutzers
    - *Azure123456!* als Kennwort

   Übernehmen Sie die anderen Standardwerte, und klicken Sie auf **Weiter: Datenträger**.
4. Übernehmen Sie die Standardwerte für die Datenträger, und klicken Sie auf **Weiter: Netzwerk**.
5. Stellen Sie sicher, dass als virtuelles Netzwerk **myVNet** und als Subnetz **myBackendSubnet** ausgewählt ist.
6. Übernehmen Sie die anderen Standardwerte, und klicken Sie auf **Weiter: Verwaltung**.
7. Klicken Sie auf **Aus**, um die Startdiagnose zu deaktivieren. Übernehmen Sie die anderen Standardwerte, und klicken Sie auf **Überprüfen + erstellen**.
8. Überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und klicken Sie dann auf **Erstellen**.
9. Warten Sie, bis die Erstellung des virtuellen Computers abgeschlossen ist, bevor Sie fortfahren.

### <a name="install-iis"></a>Installieren von IIS

1. Öffnen Sie die interaktive Shell, und vergewissern Sie sich, dass **PowerShell** festgelegt ist.

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

3. Erstellen Sie einen zweiten virtuellen Computer, und installieren Sie IIS mithilfe der soeben ausgeführten Schritte. Geben Sie *myVM2* als Name und für „VMName“ in „Set-AzureRmVMExtension“ ein.

### <a name="add-backend-servers"></a>Hinzufügen von Back-End-Servern

1. Klicken Sie auf **Alle Ressourcen** und dann auf **myAppGateway**.
4. Klicken Sie auf **Back-End-Pools**. Ein Standardpool mit dem Anwendungsgateway wurde automatisch erstellt. Klicken Sie auf **appGatewayBackendPool**.
5. Klicken Sie unter **Ziele** auf **IP-Adresse oder FQDN**, und wählen Sie **Virtueller Computer**.
6. Fügen Sie unter **Virtueller Computer** die virtuellen Computer „myVM“ und „myVM2“ und die zugeordneten Netzwerkschnittstellen hinzu.

    ![Hinzufügen von Back-End-Servern](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. Klicken Sie auf **Speichern**.

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

1. Suchen Sie auf dem Übersichtsbildschirm die öffentliche IP-Adresse für das Anwendungsgateway. Klicken Sie auf **Alle Ressourcen** und dann auf **myAGPublicIPAddress**.

    ![Notieren der öffentlichen IP-Adresse des Anwendungsgateways](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein.

    ![Testen des Anwendungsgateways](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, das Anwendungsgateway und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe aus, die das Anwendungsgateway enthält, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten von Webdatenverkehr mit einem Anwendungsgateway per Azure CLI](./tutorial-manage-web-traffic-cli.md)
