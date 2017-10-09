---
title: Herstellen einer Verbindung von Azure Stack zu Azure mithilfe von VPN
description: Herstellen einer Verbindung zwischen virtuellen Netzwerken in Azure Stack und virtuellen Netzwerken in Azure mithilfe von VPN
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: victorh
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: c06eb0bb44bdfeab956e9b5051786b5bc631acf5
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="connect-azure-stack-to-azure-using-vpn"></a>Herstellen einer Verbindung von Azure Stack zu Azure mithilfe von VPN

*Gilt für: Integrierte Azure Stack-Systeme*

In diesem Artikel wird das Erstellen eines Site-to-Site-VPNs beschrieben, mit dem ein virtuelles Netzwerk in Azure Stack mit einem virtuellen Netzwerk in Azure verbunden werden soll.

### <a name="connection-diagram"></a>Verbindungsdiagramm
Das folgende Diagramm zeigt die endgültige Verbindungskonfiguration:

![Konfiguration einer Site-to-Site-VPN-Verbindung](media/azure-stack-connect-vpn/image2.png)

### <a name="before-you-begin"></a>Voraussetzungen
Vergewissern Sie sich vor dem Durchführen der Verbindungskonfiguration, dass Sie über folgende Elemente verfügen:

* Eine Bereitstellung (mit mehreren Knoten) für integrierte Azure Stack-Systeme, die direkt mit dem Internet verbunden ist. Das bedeutet, dass der externe öffentliche Bereich Ihrer IP-Adresse direkt über das öffentliche Internet erreichbar sein muss.
* Ein gültiges Azure-Abonnement.  Wenn Sie über kein Azure-Abonnement verfügen, können Sie [hier ein kostenloses Azure-Konto](https://azure.microsoft.com/free/?b=17.06) erstellen.

## <a name="network-example-values-table"></a>Tabelle mit Netzwerk-Beispielwerten
Die Tabelle enthält die Beispielwerte für Netzwerke, die in diesem Artikel verwendet werden. Sie können diese Werte verwenden oder sie zum besseren Verständnis der Beispiele in diesem Artikel heranziehen.

**Tabelle mit Netzwerk-Beispielwerten**
|   |Azure Stack|Azure|
|---------|---------|---------|
|Name des virtuellen Netzwerks     |Azs-VNet|AzureVNet |
|Adressraum des virtuellen Netzwerks |10.1.0.0/16|10.100.0.0/16|
|Subnetzname     |FrontEnd|FrontEnd|
|Subnetzadressbereich|10.1.0.0/24 |10.100.0.0/24 |
|Gatewaysubnetz     |10.1.1.0/24|10.100.1.0/24|

## <a name="create-the-network-resources-in-azure"></a>Erstellen der Netzwerkressourcen in Azure

Erstellen Sie zunächst die Netzwerkressourcen für Azure. Die folgenden Anweisungen zeigen, wie die Ressourcen mithilfe des [Azure-Portals](http://portal.azure.com/) erstellt werden.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Erstellen des virtuellen Netzwerks und des VM-Subnetzes

1. Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Portal](http://portal.azure.com/) an.
2. Wählen Sie im Benutzerportal die Option **Neu**.
3. Wechseln Sie zu **Marketplace**, und wählen Sie dann **Netzwerk** aus.
4. Wählen Sie **Virtuelles Netzwerk** aus.
5. Verwenden Sie als **Name**, **Adressraum**, **Subnetzname** und **Subnetzadressbereich** für Azure die Werte aus der Netzwerk-Konfigurationstabelle.
6. Erstellen Sie entweder eine neue **Ressourcengruppe**, oder wählen Sie **Use existing** (Vorhandene verwenden) auswählen, wenn Sie bereits über eine verfügen.
7. Wählen Sie den **Standort** Ihres VNets aus.  Wenn Sie die Beispielwerte verwenden, wählen Sie **Osten, USA** oder einen anderen Standort Ihrer Wahl aus.
8. Wählen Sie die Option **An Dashboard anheften** aus.
9. Klicken Sie auf **Erstellen**.

### <a name="create-the-gateway-subnet"></a>Erstellen des Gatewaysubnetzes
1. Öffnen Sie über das Dashboard die zuvor erstellte Ressource des virtuellen Netzwerks (**AzureVNet**).
2. Wählen Sie im Bereich **Einstellungen** die Option **Subnetze** aus.
3. Wählen Sie **Gatewaysubnetz**, um dem virtuellen Netzwerk ein Gatewaysubnetz hinzuzufügen.
4. Der Name des Subnetzes wird standardmäßig auf **GatewaySubnet** festgelegt.
   Gatewaysubnetze sind spezielle Subnetze und müssen diesen spezifischen Namen besitzen, damit sie ordnungsgemäß funktionieren.
5. Stellen Sie sicher, dass im Feld **Adressbereich** die Adresse **10.100.0.0/24** angegeben ist.
6. Wählen Sie **OK**, um das Gatewaysubnetz zu erstellen.

### <a name="create-the-virtual-network-gateway"></a>Erstellen des Gateways für das lokale Netzwerk
1. Wählen Sie im Azure-Portal die Option **Neu**.  
2. Wechseln Sie zu **Marketplace**, und wählen Sie dann **Netzwerk** aus.
3. Wählen Sie in der Liste mit den Netzwerkressourcen den Eintrag **Gateway des virtuellen Netzwerks** aus.
4. Geben Sie unter **Name** den Namen **Azure-GW** ein.
5. Klicken Sie auf **Virtuelles Netzwerk**, um ein virtuelles Netzwerk auszuwählen. Wählen Sie anschließend aus der Liste **AzureVnet** aus.
6. Wählen Sie **Öffentliche IP-Adresse**. Wählen Sie Bereich **Öffentliche IP-Adresse auswählen** die Option **Neu erstellen** aus.
7. Geben Sie unter **Name** den Namen **Azure-GW-PiP** ein, und klicken Sie auf **OK**.
8. Als **VPN-Typ** ist standardmäßig **Routenbasiert** ausgewählt.
    Behalten Sie **Routenbasiert** als VPN-Typ bei.
9. Vergewissern Sie sich, dass **Abonnement** und **Speicherort** korrekt sind. Die Ressource kann an das Dashboard angeheftet werden. Klicken Sie auf **Erstellen**.

### <a name="create-the-local-network-gateway-resource"></a>Erstellen der Ressource des lokalen Netzwerkgateways

1. Wählen Sie im Azure-Portal die Option **Neu**. 
4. Wechseln Sie zu **Marketplace**, und wählen Sie dann **Netzwerk** aus.
5. Wählen Sie in der Ressourcenliste den Eintrag für das **lokale Netzwerkgateway** aus.
6. Geben Sie unter **Name** den Namen **Azs-GW** ein.
7. Geben Sie unter **IP-Adresse** die öffentliche IP-Adresse für Ihr Gateway des virtuellen Azure Stack-Netzwerks ein, die in der Netzwerk-Konfigurationstabelle oben aufgelistet ist.
8. Geben Sie unter **Adressraum** für Azure Stack den Adressraum **10.0.10.0/23** für **AzureVNet** ein.
9. Vergewissern Sie sich, dass Ihr **Abonnement**, die **Ressourcengruppe** und der **Speicherort** richtig sind, und klicken Sie auf **Erstellen**.

## <a name="create-the-connection"></a>Erstellen der Verbindung
1. Wählen Sie im Benutzerportal die Option **Neu**. 
2. Wechseln Sie zu **Marketplace**, und wählen Sie dann **Netzwerk** aus.
3. Wählen Sie in der Ressourcenliste die Option **Verbindung** aus.
4. Wählen Sie im Einstellungsbereich **Grundlagen** unter **Verbindungstyp** die Option **Site-to-Site (IPsec)** aus.
5. Wählen Sie Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** aus, und wählen Sie anschließend **OK**.
6. Wählen Sie im Bereich **Einstellungen** die Option **Gateway des virtuellen Netzwerks** aus, und wählen Sie anschließend **Azure-GW** aus.
7. Wählen Sie **Gateway des lokalen Netzwerks** aus, und wählen Sie dann **Azs-GW** aus.
8. Geben Sie unter **Verbindungsname** den Namen **Azure-Azs** ein.
9. Geben Sie unter **Gemeinsam verwendeter Schlüssel (PSK)** den Wert **12345** ein. Falls Sie einen anderen Wert verwenden, vergessen Sie nicht, dass er dem Wert für den gemeinsam verwendeten Schlüssel entsprechen *muss*, den Sie am anderen Ende der Verbindung erstellt haben. Klicken Sie auf **OK**.
10. Überprüfen Sie den Bereich **Zusammenfassung**, und klicken Sie auf **OK**.

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers
Erstellen Sie jetzt einen virtuellen Computer in Azure, und platzieren Sie ihn im VM-Subnetz Ihres virtuellen Netzwerks.

1. Wählen Sie im Azure-Portal die Option **Neu**.
2. Wechseln Sie zu **Marketplace**, und wählen Sie dann **Compute** aus.
3. Wählen Sie in der Liste mit den VM-Images das Image **Windows Server 2016 Datacenter Eval** aus.
4. Geben Sie im Bereich **Grundlagen** unter **Name** den Namen **AzureVM** ein.
5. Geben Sie einen gültigen Benutzernamen und ein gültiges Kennwort ein. Mit diesem Konto melden Sie sich am virtuellen Computer an, nachdem dieser erstellt wurde.
6. Geben Sie Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** an, und wählen Sie anschließend **OK**.
7. Wählen Sie im Bereich **Größe** für diese Instanz eine Größe für den virtuellen Computer aus, und klicken Sie dann auf **Auswählen**.
8. Übernehmen Sie im Bereich **Einstellungen** die Standardeinstellungen. Stellen Sie sicher, dass das virtuelle Netzwerk **AzureVnet** ausgewählt und das Subnetz auf **10.0.20.0/24** festgelegt ist. Klicken Sie auf **OK**.
9. Überprüfen Sie die Einstellungen im Bereich **Zusammenfassung**, und klicken Sie anschließend auf **OK**.

## <a name="create-the-network-resources-in-azure-stack"></a>Erstellen der Netzwerkressourcen in Azure Stack
Erstellen Sie als Nächstes die Netzwerkressourcen in Azure Stack.

### <a name="sign-in-as-a-user"></a>Anmelden als Benutzer
Ein Dienstadministrator kann sich als Benutzer anmelden, um die Pläne, Angebote und Abonnements zu testen, die den Benutzern zur Verfügung stehen. Falls Sie noch kein Benutzerkonto besitzen, [erstellen Sie ein Benutzerkonto](azure-stack-add-new-user-aad.md), bevor Sie sich anmelden.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Erstellen des virtuellen Netzwerks und des VM-Subnetzes
1. Verwenden Sie ein Benutzerkonto für die Anmeldung beim Benutzerportal.
2. Wählen Sie im Benutzerportal die Option **Neu**.

    ![Neues virtuelles Netzwerk erstellen](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)

3. Wechseln Sie zu **Marketplace**, und wählen Sie dann **Netzwerk** aus.
4. Wählen Sie **Virtuelles Netzwerk** aus.
5. Verwenden Sie für **Name**, **Adressraum**, **Subnetzname** und **Subnetzadressbereich** die Werte aus der Netzwerk-Konfigurationstabelle.
6. In **Abonnement** wird das zuvor von Ihnen erstellte Abonnement angezeigt.
7. Als **Ressourcengruppe** können Sie entweder eine Ressourcengruppe erstellen oder **Use existing** (Vorhandene verwenden) auswählen, sofern Sie bereits über eine Ressourcengruppe verfügen.
8. Überprüfen Sie den standardmäßigen Speicherort.
9. Wählen Sie die Option **An Dashboard anheften** aus.
10. Klicken Sie auf **Erstellen**.

### <a name="create-the-gateway-subnet"></a>Erstellen des Gatewaysubnetzes
1. Öffnen Sie im Dashboard die zuvor erstellte Ressource des virtuellen Netzwerks „Azs-VNet“.
2. Wählen Sie im Bereich **Einstellungen** die Option **Subnetze** aus.
3. Wählen Sie **Gatewaysubnetz**, um dem virtuellen Netzwerk ein Gatewaysubnetz hinzuzufügen.
   
    ![Gatewaysubnetz hinzufügen](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)

4. Der Subnetzname ist standardmäßig auf **GatewaySubnet** festgelegt.
   Gateway-Subnetz sind speziell. Sie müssen den Namen *GatewaySubnet* verwenden, um ordnungsgemäß zu funktionieren.
5. Überprüfen Sie im **Adressbereich**, dass die Adresse **10.1.1.0/24** lautet.
6. Wählen Sie **OK**, um das Gatewaysubnetz zu erstellen.

### <a name="create-the-virtual-network-gateway"></a>Erstellen des Gateways für das lokale Netzwerk
1. Wählen Sie im Azure Stack-Portal die Option **Neu** aus. 
2. Wechseln Sie zu **Marketplace**, und wählen Sie dann **Netzwerk** aus.
3. Wählen Sie in der Liste mit den Netzwerkressourcen den Eintrag **Gateway des virtuellen Netzwerks** aus.
4. Geben Sie unter **Name** den Namen **Azs-GW** ein.
5. Wählen Sie **Virtuelles Netzwerk**, um ein virtuelles Netzwerk auszuwählen.
   Wählen Sie aus der Liste **Azs-VNet** aus.
6. Wählen Sie das Menüelement **Öffentliche IP-Adresse**. Wählen Sie Bereich **Öffentliche IP-Adresse auswählen** die Option **Neu erstellen** aus.
7. Geben Sie unter **Name** den Namen **Azs-GW-PiP** ein, und klicken Sie auf **OK**.
8.  Als **VPN-Typ** ist standardmäßig **Routenbasiert** ausgewählt.
    Behalten Sie **Routenbasiert** als VPN-Typ bei.
9. Vergewissern Sie sich, dass **Abonnement** und **Speicherort** korrekt sind. Die Ressource kann an das Dashboard angeheftet werden. Klicken Sie auf **Erstellen**.

### <a name="create-the-local-network-gateway"></a>Erstellen des Gateways des lokalen Netzwerks
Es gibt kleine Unterschiede zwischen einem *Gateway des lokalen Netzwerks* in Azure Stack und dem Gateway in einer Azure-Bereitstellung.

In einer Azure-Bereitstellung stellt ein lokales Netzwerkgateway ein lokales physisches Gerät (am Benutzerstandort) dar, mit dem eine Verbindung mit einem virtuellen Netzwerkgateway in Azure hergestellt wird. In Azure Stack sind beide Enden der Verbindung virtuelle Netzwerkgateways.

Etwas allgemeiner betrachtet wird mit der Ressource des lokalen Netzwerkgateways immer das Remotegateway am anderen Ende der Verbindung angegeben. 

### <a name="create-the-local-network-gateway-resource"></a>Erstellen der Ressource des lokalen Netzwerkgateways
1. Melden Sie sich beim Azure Stack-Portal an.
2. Wählen Sie im Benutzerportal die Option **Neu**.
3. Wechseln Sie zu **Marketplace**, und wählen Sie dann **Netzwerk** aus.
4. Wählen Sie in der Ressourcenliste den Eintrag für das **lokale Netzwerkgateway** aus.
5. Geben Sie unter **Name** den Namen **Azure-GW** ein.
6. Geben Sie unter **IP-Adresse** die öffentliche IP-Adresse für das Gateway des virtuellen Netzwerks in Azure **Azure-GW-PiP** ein. Diese Adresse wird weiter oben in der Netzwerk-Konfigurationstabelle angezeigt.
7. Geben Sie unter **Adressraum** den Adressraum für das zuvor erstellte Azure VNET **10.0.20.0/23** ein.
8. Vergewissern Sie sich, dass Ihr **Abonnement**, die **Ressourcengruppe** und der **Speicherort** richtig sind, und klicken Sie auf **Erstellen**.

### <a name="create-the-connection"></a>Erstellen der Verbindung
1. Wählen Sie im Benutzerportal die Option **Neu**.
2. Wechseln Sie zu **Marketplace**, und wählen Sie dann **Netzwerk** aus.
3. Wählen Sie in der Ressourcenliste die Option **Verbindung** aus.
4. Wählen Sie im Einstellungsbereich **Grundlagen** unter **Verbindungstyp** die Option **Site-to-Site (IPsec)** aus.
5. Wählen Sie Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** aus, und wählen Sie anschließend **OK**.
6. Wählen Sie im Bereich **Einstellungen** die Option **Gateway des virtuellen Netzwerks** aus, und wählen Sie anschließend **Azs-GW** aus.
7. Wählen Sie **Gateway des lokalen Netzwerks** aus, und wählen Sie dann **Azure-GW** aus.
8. Geben Sie unter **Verbindungsname** den Namen **Azs-Azure** ein.
9. Geben Sie unter **Gemeinsam verwendeter Schlüssel (PSK)** den Wert **12345** ein, und klicken Sie auf **OK**.
10. Wählen Sie im Bereich **Zusammenfassung** die Option **OK** aus.

### <a name="create-a-vm"></a>Erstellen einer VM
Zum Überprüfen der durch die VPN-Verbindung geleiteten Daten müssen Sie an beiden Enden virtuelle Computer zum Senden und Empfangen von Daten über den VPN-Tunnel erstellen. 

1. Wählen Sie im Azure-Portal die Option **Neu**.
2. Wechseln Sie zu **Marketplace**, und wählen Sie dann **Compute** aus.
3. Wählen Sie in der Liste mit den VM-Images das Image **Windows Server 2016 Datacenter Eval** aus.
4. Geben Sie im Bereich **Grundlagen** unter **Name** den Namen **Azs-VM** ein.
5. Geben Sie einen gültigen Benutzernamen und ein gültiges Kennwort ein. Mit diesem Konto melden Sie sich am virtuellen Computer an, nachdem dieser erstellt wurde.
6. Geben Sie Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** an, und wählen Sie anschließend **OK**.
7. Wählen Sie im Bereich **Größe** für diese Instanz eine Größe für den virtuellen Computer aus, und klicken Sie dann auf **Auswählen**.
8. Übernehmen Sie im Bereich **Einstellungen** die Standardeinstellungen. Stellen Sie sicher, dass das virtuelle Netzwerk **Azs-VNet** ausgewählt ist. Stellen Sie sicher, dass das Subnetz auf **10.1.0.0/24** festgelegt ist. Wählen Sie dann **OK**aus.
9. Überprüfen Sie die Einstellungen im Bereich **Zusammenfassung**, und klicken Sie anschließend auf **OK**.


## <a name="test-the-connection"></a>Testen der Verbindung
Nach der Einrichtung der Site-to-Site-Verbindung sollten Sie sich vergewissern, dass darüber Datenverkehr abgewickelt werden kann. Melden Sie sich dazu bei einem virtuellen Computer an, den Sie in Azure Stack erstellt haben. Pingen Sie anschließend den virtuellen Computer, den Sie in Azure erstellt haben. 

Um sicherzustellen, dass der Datenverkehr über die Site-to-Site-Verbindung gesendet wird, pingen Sie anstelle der VIP-Adresse die direkte IP-Adresse (DIP) des virtuellen Computers im Remotesubnetz. Dazu müssen Sie die DIP-Adresse am anderen Ende der Verbindung ermitteln. Speichern Sie die Adresse für die spätere Verwendung.

### <a name="sign-in-to-the-user-vm-in-azure-stack"></a>Anmelden beim virtuellen Benutzercomputer in Azure Stack
1. Melden Sie sich beim Azure Stack-Portal an.
2. Wählen Sie in der Navigationsleiste links die Option **Virtuelle Computer** aus.
3. Suchen Sie in der Liste mit den virtuellen Computern nach dem zuvor erstellten Computer **Azs-VM**, und wählen Sie ihn aus.
4. Klicken Sie im Bereich für den virtuellen Computer auf **Verbinden**. Öffnen Sie anschließend die Datei „Azs-VM.rdp“.
   
     ![Schaltfläche „Verbinden“](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Melden Sie sich mit dem Konto an, das Sie beim Erstellen des virtuellen Computers konfiguriert haben.
6. Öffnen Sie ein **Windows PowerShell**-Fenster mit erhöhten Rechten.
7. Geben Sie **ipconfig /all** ein.
8. Suchen Sie in der Ausgabe die **IPv4-Adresse**, und speichern Sie diese zur späteren Verwendung. Diese Adresse wird später von Azure aus gepingt. In der Beispielumgebung wird die Adresse **10.0.10.4** verwendet, in Ihrer Umgebung lautet sie aber unter Umständen anders. Sie sollte innerhalb des zuvor erstellten Subnetzes **10.0.10.0/24** liegen.
9. Zum Erstellen einer Firewallregel, zulässt, dass der virtuelle Computer auf Pings reagiert, führen Sie den folgenden PowerShell-Befehl aus:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-azure"></a>Anmelden beim virtuellen Mandantencomputer in Azure
1. Melden Sie sich beim Azure-Portal an.
2. Klicken Sie in der Navigationsleiste links auf **Virtuelle Computer**.
3. Suchen Sie in der Liste mit den virtuellen Computern nach dem zuvor erstellten Computer **Azure-VM**, und wählen Sie ihn aus.
4. Klicken Sie im Bereich für den virtuellen Computer auf **Verbinden**.
5. Melden Sie sich mit dem Konto an, das Sie beim Erstellen des virtuellen Computers konfiguriert haben.
6. Öffnen Sie ein **Windows PowerShell**-Fenster mit erhöhten Rechten.
7. Geben Sie **ipconfig /all** ein.
8. Daraufhin sollte eine IPv4-Adresse aus dem Adressbereich **10.0.20.0/24** angezeigt werden. In der Beispielumgebung wird die Adresse **10.0.20.4** verwendet, Ihre Adresse lautet aber unter Umständen anders.
9. Zum Erstellen einer Firewallregel, zulässt, dass der virtuelle Computer auf Pings reagiert, führen Sie den folgenden PowerShell-Befehl aus:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

10. Pingen Sie vom virtuellen Computer in Azure den virtuellen Computer in Azure Stack durch den Tunnel. Hierzu pingen Sie die DIP-Adresse, die Sie für „Azs-VM“ erfasst haben.
   In der Beispielumgebung wird die Adresse **10.0.10.4** verwendet. Achten Sie jedoch darauf, dass Sie die Adresse pingen, die Sie in Ihrem Lab notiert haben. Das Ergebnis sollte in etwa wie auf dem folgenden Screenshot aussehen:
   
    ![Ping erfolgreich](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
11. Wenn Sie eine Antwort vom virtuellen Remotecomputer erhalten, war der Test erfolgreich! Sie können das Fenster für den virtuellen Computer schließen. Zum Testen Ihrer Verbindung können andere Arten von Datenübertragungen wie eine Dateikopie durchführen.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Anzeigen der Datenübertragungsstatistik über die Gatewayverbindung
Im Bereich **Verbindung** können Sie die Datenmenge ermitteln, die über die Site-to-Site-Verbindung übertragen wird. Mit diesem Test können Sie auch überprüfen, ob der soeben gesendete Ping tatsächlich die VPN-Verbindung durchlaufen hat.

1. Verwenden Sie Ihr Benutzerkonto zur Anmeldung beim Benutzerportal, während Sie beim virtuellen Benutzercomputer in Azure Stack angemeldet sind.
2. Wechseln Sie zu **Alle Ressourcen**, und wählen Sie die Verbindung **Azs-Azure** aus. **Verbindungen** wird angezeigt.
4. Im Bereich **Verbindung** wird die Statistik für **eingehende Daten** und **ausgehende Daten** angezeigt. Die hohen Zahlen im folgenden Screenshot sind auf zusätzliche Dateiübertragungen zurückzuführen. Dort sollten andere Werte als Null angezeigt werden.
   
    ![Eingehende und ausgehende Daten](media/azure-stack-connect-vpn/Connection.png)

## <a name="next-steps"></a>Nächste Schritte

[Deploy apps to Azure and Azure Stack (Bereitstellen von Apps in Azure und Azure Stack)](azure-stack-solution-pipeline.md)
