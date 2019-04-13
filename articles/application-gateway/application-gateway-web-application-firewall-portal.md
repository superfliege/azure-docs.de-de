---
title: 'Tutorial: Erstellen eines Anwendungsgateways mit einer Web Application Firewall – Azure-Portal | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie mithilfe des Azure-Portals ein Anwendungsgateway mit einer Web Application Firewall erstellen.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 03/25/2019
ms.author: victorh
ms.openlocfilehash: 1284ddec4cd9cea3ea53c20d437550405dd614d9
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905867"
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-using-the-azure-portal"></a>Erstellen eines Anwendungsgateways mit einer Web Application Firewall über das Azure-Portal

> [!div class="op_single_selector"]
>
> - [Azure-Portal](application-gateway-web-application-firewall-portal.md)
> - [PowerShell](tutorial-restrict-web-traffic-powershell.md)
> - [Azure-Befehlszeilenschnittstelle](tutorial-restrict-web-traffic-cli.md)
>
> 

In diesem Tutorial erfahren Sie, wie Sie mithilfe des Azure-Portals ein [Anwendungsgateway](application-gateway-introduction.md) mit einer [Web Application Firewall](application-gateway-web-application-firewall-overview.md) (WAF) erstellen. Die WAF verwendet [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project)-Regeln, um Ihre Anwendung zu schützen. Diese Regeln beinhalten den Schutz vor Angriffen z.B. durch Einschleusung von SQL-Befehlen, siteübergreifendes Scripting und Sitzungsübernahmen. Nach der Erstellung wird das Anwendungsgateway getestet, um sicherzustellen, dass es ordnungsgemäß funktioniert. Mit Azure Application Gateway leiten Sie den Webdatenverkehr Ihrer Anwendungen an bestimmte Ressourcen weiter, indem Sie Ports Listener zuweisen, Regeln erstellen und Ressourcen zu einem Back-End-Pool hinzufügen. Der Einfachheit halber wird in diesem Artikel ein einfaches Setup mit einer öffentlichen Front-End-IP-Adresse, einem grundlegenden Listener zum Hosten einer einzelnen Website auf diesem Anwendungsgateway, zwei virtuellen Computern für den Back-End-Pool und einer Routingregel für grundlegende Anforderungen verwendet.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen eines Anwendungsgateways mit aktivierter WAF
> * Erstellen der virtuellen Computer, die als Back-End-Server verwendet werden
> * Erstellen eines Speicherkontos und Konfigurieren der Diagnose

![Web Application Firewall – Beispiel](./media/application-gateway-web-application-firewall-portal/scenario-waf.png)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Für die Kommunikation in Azure zwischen den von Ihnen erstellten Ressourcen ist ein virtuelles Netzwerk erforderlich. Sie können ein neues virtuelles Netzwerk erstellen oder ein vorhandenes auswählen. In diesem Beispiel erstellen wir ein neues virtuelles Netzwerk. Sie können ein virtuelles Netzwerk zum gleichen Zeitpunkt erstellen wie das Anwendungsgateway. Application Gateway-Instanzen werden in separaten Subnetzen erstellt. In diesem Beispiel erstellen Sie zwei Subnetze: eins für das Anwendungsgateway und eins für die Back-End-Server.

Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressource erstellen**. Das Fenster **Neu** wird angezeigt.

Klicken Sie auf **Netzwerk**, und wählen Sie dann in der Liste **Ausgewählte** die Option **Application Gateway** aus.

### <a name="basics-page"></a>Seite „Grundlagen“

1. Geben Sie auf der Seite **Grundlagen** die folgenden Werte für die Einstellungen des Anwendungsgateways ein:
   - **Name**: Geben Sie *myAppGateway* als Namen des Anwendungsgateways ein.
   - **Ressourcengruppe**: Wählen Sie **myResourceGroupAG** als Ressourcengruppe aus. Falls diese Gruppe nicht vorhanden ist, wählen Sie **Neue erstellen** aus, um sie zu erstellen.
   - Wählen Sie *WAF* als Ebene des Anwendungsgateways aus.![Erstellen eines neuen Anwendungsgateways](./media/application-gateway-web-application-firewall-portal/application-gateway-create.png)

Übernehmen Sie die Standardwerte für die anderen Einstellungen, und klicken Sie auf **OK**.

### <a name="settings-page"></a>Seite "Einstellungen"

1. Wählen Sie auf der Seite **Einstellungen** unter **Subnetzkonfiguration** die Option **Virtuelles Netzwerk auswählen** aus. <br>
2. Wählen Sie auf der Seite **Virtuelles Netzwerk auswählen** die Option **Neu erstellen** aus, und geben Sie Werte für die folgenden Einstellungen des virtuellen Netzwerks ein:
   - **Name**: Geben Sie *myVNet* als Namen des virtuellen Netzwerks ein.
   - **Adressraum:** Geben Sie *10.0.0.0/16* für den Adressraum des virtuellen Netzwerks ein.
   - **Subnetzname**: Geben Sie *myAGSubnet* als Subnetznamen ein.<br>Das Subnetz für das Anwendungsgateway kann nur Anwendungsgateways enthalten. Andere Ressourcen sind nicht zulässig.
   - **Subnetzadressbereich:** Geben Sie *10.0.0.0/24* für den Subnetzadressbereich ein.![Virtuelles Netzwerk erstellen](./media/application-gateway-web-application-firewall-portal/application-gateway-vnet.png)
3. Klicken Sie auf **OK**, um das virtuelle Netzwerk und das Subnetz zu erstellen.
4. Wählen Sie die **Front-End-IP-Konfiguration** aus. Vergewissern Sie sich unter **Frontend-IP-Konfiguration**, dass **IP-Adresstyp** auf **Öffentlich** festgelegt ist. Vergewissern Sie sich unter **Öffentliche IP-Adresse**, dass **Neue erstellen** ausgewählt ist. <br>Je nach Anwendungsfall können Sie konfigurieren, dass die Front-End-IP-Adresse öffentlich oder privat ist. In diesem Beispiel verwenden wir eine öffentliche Front-End-IP-Adresse. 
5. Geben Sie *myAGPublicIPAddress* als Namen für die öffentliche IP-Adresse ein. 
6. Übernehmen Sie die Standardwerte für die anderen Einstellungen, und klicken Sie dann auf **OK**.<br>In diesem Artikel wählen wir der Einfachheit halber Standardwerte aus, Sie können jedoch für die anderen Einstellungen je nach Anwendungsfall auch benutzerdefinierte Werte konfigurieren. 

### <a name="summary-page"></a>Seite „Zusammenfassung“

Überprüfen Sie die Einstellungen auf der Seite **Zusammenfassung**, und klicken Sie dann auf **OK**, um das virtuelle Netzwerk, die öffentliche IP-Adresse und das Anwendungsgateway zu erstellen. Die Erstellung des Anwendungsgateways in Azure kann einige Minuten in Anspruch nehmen. Warten Sie, bis die Bereitstellung erfolgreich abgeschlossen ist, bevor Sie mit dem nächsten Abschnitt fortfahren.

## <a name="add-backend-pool"></a>Hinzufügen eines Back-End-Pools

Der Back-End-Pool wird zum Weiterleiten von Anforderungen an die Back-End-Server verwendet, die die Anforderung verarbeiten. Back-End-Pools können Netzwerkkarten, VM-Skalierungsgruppen, öffentliche IP-Adressen, interne IP-Adressen, vollqualifizierte Domänennamen (Fully Qualified Domain Names, FQDN) und Back-Ends mit mehreren Mandanten wie Azure App Service umfassen. Sie müssen Ihre Back-End-Ziele zu einem Back-End-Pool hinzufügen.

In diesem Beispiel verwenden wir virtuelle Computer als das Ziel-Back-End. Wir können entweder vorhandene virtuelle Computer verwenden oder neue erstellen. In diesem Beispiel erstellen wir zwei virtuelle Computer, die von Azure als Back-End-Server für das Anwendungsgateway verwendet werden. Dazu führen wir die folgenden Schritte aus:

1. Erstellen eines neuen Subnetzes (*myBackendSubnet*), in dem die neuen virtuellen Computer erstellt werden. 
2. Erstellen von zwei neuen virtuellen Computern (*myVM* und *myVM2*), die als Back-End-Server verwendet werden.
3. Installieren von IIS auf den virtuellen Computern, um zu überprüfen, ob das Anwendungsgateway erfolgreich erstellt wurde.
4. Fügen Sie die Back-End-Server zum Back-End-Pool hinzu.

### <a name="add-a-subnet"></a>Hinzufügen eines Subnetzes

Fügen Sie dem virtuellen Netzwerk, das Sie erstellt haben, wie folgt ein Subnetz hinzu:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Alle Ressourcen** aus, geben Sie *myVNet* in das Suchfeld ein, und wählen Sie dann in den Suchergebnissen **myVNet** aus.

2. Wählen Sie im Menü auf der linken Seite die Option **Subnetze** aus, und klicken Sie dann auf **+ Subnetz**. 

   ![Erstellen eines Subnetzes](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Geben Sie auf der Seite **Subnetz hinzufügen** als **Namen** des Subnetzes *myBackendSubnet* ein, und klicken Sie auf **OK**.

### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**. Das Fenster **Neu** wird angezeigt.
2. Klicken Sie auf **Compute**, und wählen Sie dann in der Liste **Ausgewählte** die Option **Windows Server 2016 Datacenter** aus. Die Seite **Virtuellen Computer erstellen** wird angezeigt.<br>Application Gateway kann Datenverkehr an jeden beliebigen virtuellen Computer weiterleiten, der im Back-End-Pool verwendet wird. In diesem Beispiel verwenden Sie Windows Server 2016 Datacenter.
3. Geben Sie auf der Registerkarte **Grundlagen** die folgenden Werte für die VM-Einstellungen ein:
   - **Ressourcengruppe**: Wählen Sie **myResourceGroupAG** als Namen der Ressourcengruppe aus.
   - **Name des virtuellen Computers**: Geben Sie *myVM* als Namen der VM ein.
   - **Benutzername**: Geben *azureuser* als Namen des Administratorbenutzers ein.
   - **Kennwort**: Geben Sie *Azure123456!* als Administratorkennwort ein.
4. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Weiter: Datenträger**.  
5. Übernehmen Sie auf der Registerkarte **Datenträger** die Standardwerte, und klicken Sie auf **Weiter: Netzwerk**.
6. Vergewissern Sie sich auf der Registerkarte **Netzwerk**, dass **myVNet** für **Virtuelles Netzwerk** ausgewählt und **Subnetz** auf **myBackendSubnet** festgelegt ist. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Weiter: Verwaltung** aus.<br>Application Gateway kann mit Instanzen außerhalb des eigenen virtuellen Netzwerks kommunizieren, es muss jedoch sichergestellt werden, dass eine IP-Verbindung besteht. 
7. Legen Sie auf der Registerkarte **Verwaltung** die Option **Startdiagnose** auf **Aus** fest. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Bewerten + erstellen**.
8. Überprüfen Sie auf der Registerkarte **Bewerten + erstellen** die Einstellungen, beheben Sie alle Validierungsfehler, und wählen Sie dann **Erstellen** aus.
9. Warten Sie, bis die Erstellung des virtuellen Computers abgeschlossen ist, bevor Sie fortfahren.

### <a name="install-iis-for-testing"></a>Installieren von IIS für Testzwecke

In diesem Beispiel installieren Sie IIS auf den virtuellen Computern, um zu überprüfen, ob Azure das Anwendungsgateway erfolgreich erstellt hat. 

1. Öffnen Sie [Azure PowerShell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell). Wählen Sie dazu in der oberen Navigationsleiste des Azure-Portals **Cloud Shell** und dann in der Dropdownliste **PowerShell** aus. 

   ![Installieren der benutzerdefinierten Erweiterung](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

2. Führen Sie den folgenden Befehl aus, um IIS auf dem virtuellen Computer zu installieren: 

   ```azurepowershell-interactive
   Set-AzVMExtension `
     -ResourceGroupName myResourceGroupAG `
     -ExtensionName IIS `
     -VMName myVM `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
   ```

3. Erstellen Sie eine zweite VM, und installieren Sie IIS mithilfe der zuvor ausgeführten Schritte. Verwenden Sie *myVM2* für den VM-Namen und für die Einstellung **VMName** des Cmdlets **Set-AzVMExtension**.

### <a name="add-backend-servers-to-backend-pool"></a>Hinzufügen von Back-End-Servern zu Back-End-Pools

1. Wählen Sie **Alle Ressourcen** und dann **myAppGateway** aus.

2. Klicken Sie im Menü auf der linken Seite auf **Back-End-Pools**. Azure erstellt automatisch einen Standardpool **appGatewayBackendPool**, wenn Sie das Anwendungsgateway erstellen. 

3. Wählen Sie **appGatewayBackendPool** aus.

4. Wählen Sie unter **Ziele** in der Dropdownliste die Option **Virtueller Computer** aus.

5. Wählen Sie unter **VIRTUELLER COMPUTER** und **NETZWERKSCHNITTSTELLEN** in den Dropdownlisten die VMs **myVM** und **myVM2** sowie die zugehörigen Netzwerkschnittstellen aus.

   ![Hinzufügen von Back-End-Servern](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. Wählen Sie **Speichern** aus.

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Erstellen eines Speicherkontos und Konfigurieren der Diagnose

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

In diesem Tutorial verwendet das Anwendungsgateway ein Speicherkonto, um Daten zum Zweck der Erkennung und Prävention zu speichern. Sie können auch Azure Monitor-Protokolle oder Event Hub verwenden, um Daten aufzuzeichnen.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf **Neu**.
2. Wählen Sie die Option **Speicher** und anschließend **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** aus.
3. Geben Sie den Namen des Speicherkontos ein, und wählen Sie für die Ressourcengruppe **Vorhandene verwenden** und anschließend **myResourceGroupAG** aus. In diesem Beispiel lautet der Name des Speicherkontos *myagstore1*. Übernehmen Sie die Standardwerte für die anderen Einstellungen, und klicken Sie auf **Erstellen**.

### <a name="configure-diagnostics"></a>Konfigurieren der Diagnose

Konfigurieren Sie die Diagnose so, dass Daten in den Protokollen „ApplicationGatewayAccessLog“, „ApplicationGatewayPerformanceLog“ und „ApplicationGatewayFirewallLog“ aufgezeichnet werden.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann auf *myAppGateway*.
2. Klicken Sie unter „Überwachen“ auf **Diagnoseprotokolle**.
3. Klicken Sie auf **Diagnoseeinstellung hinzufügen**.
4. Geben Sie als Name für die Diagnoseeinstellungen *myDiagnosticsSettings* ein.
5. Aktivieren Sie **In einem Speicherkonto archivieren**, und klicken Sie dann auf **Konfigurieren**, um das zuvor erstellte Speicherkonto *myagstore1* auszuwählen.
6. Wählen Sie die Anwendungsgatewayprotokolle aus, die erfasst und aufbewahrt werden sollen.
7. Klicken Sie auf **Speichern**.

    ![Konfigurieren der Diagnose](./media/application-gateway-web-application-firewall-portal/application-gateway-diagnostics.png)

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

IIS ist für die Erstellung des Anwendungsgateways zwar nicht erforderlich, Sie haben die Installation in dieser Schnellstartanleitung aber ausgeführt, um zu überprüfen, ob Azure das Anwendungsgateway erfolgreich erstellt hat. Testen des Anwendungsgateways mit IIS:

1. Suchen Sie auf der Seite **Übersicht** des Anwendungsgateways nach der öffentlichen IP-Adresse für das Anwendungsgateway.![Notieren der öffentlichen IP-Adresse des Anwendungsgateways](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)Alternativ können Sie **Alle Ressourcen** auswählen, *myAGPublicIPAddress* in das Suchfeld eingeben und den Eintrag dann in den Suchergebnissen auswählen. Azure zeigt die öffentliche IP-Adresse auf der Seite **Übersicht** an.
2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein.
3. Überprüfen Sie die Antwort. Eine gültige Antwort überprüft, ob das Anwendungsgateway erfolgreich erstellt wurde und erfolgreich eine Verbindung mit dem Back-End herstellen kann.![Testen des Anwendungsgateways](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem Anwendungsgateway erstellten Ressourcen nicht mehr benötigen, entfernen Sie die Ressourcengruppe. Beim Entfernen der Ressourcengruppe werden auch das Anwendungsgateway und alle zugehörigen Ressourcen entfernt. 

So entfernen Sie die Ressourcengruppe:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Ressourcengruppen** aus.
2. Suchen Sie auf der Seite **Ressourcengruppen** in der Liste nach **myResourceGroupAG**, und wählen Sie den Eintrag aus.
3. Wählen Sie auf der Seite für die **Ressourcengruppe** die Option **Ressourcengruppe löschen** aus.
4. Geben Sie *myResourceGroupAG* für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Anwendungsgateways mit aktivierter WAF
> * Erstellen der virtuellen Computer, die als Back-End-Server verwendet werden
> * Erstellen eines Speicherkontos und Konfigurieren der Diagnose

Weitere Informationen zu Anwendungsgateways und den zugehörigen Ressourcen finden Sie in den Artikeln mit empfohlenen Vorgehensweisen.
