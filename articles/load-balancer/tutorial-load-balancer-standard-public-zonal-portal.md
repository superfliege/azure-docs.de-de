---
title: Tutorial – Durchführen eines Lastenausgleichs für virtuelle Computer innerhalb einer Zone – Azure-Portal | Microsoft-Dokumentation
description: In diesem Tutorial wird gezeigt, wie Sie einen Load Balancer im Tarif „Standard“ mit einem zonalen Front-End erstellen, um mit dem Azure-Portal einen Lastenausgleich für virtuelle Computer innerhalb einer Verfügbarkeitszone vorzunehmen.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As an IT administrator, I want to create a load balancer that load balances incoming internet traffic to virtual machines within a specific zone in a region.
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 9067ea350997ed0c4fc5c65dccb72f403adfa774
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="tutorialload-balance-vms-within-an-availability-zone-with-a-standard-load-balancer-using-the-azure-portal"></a>Tutorial: Durchführen eines Lastenausgleichs für virtuelle Computer innerhalb einer Verfügbarkeitszone mit einem Load Balancer im Tarif „Standard“ im Azure-Portal

In diesem Tutorial wird die Erstellung eines öffentlichen [Load Balancers im Tarif „Standard“](https://aka.ms/azureloadbalancerstandard) mit einem zonalen Front-End mithilfe einer öffentlichen Standard-IP-Adresse und des Azure-Portals erläutert. In diesem Szenario können Sie auch eine bestimmte Zone für Ihre Front-End- und Back-End-Instanzen angeben, um Ihren Datenpfad und Ihre Ressourcen mit einer bestimmten Zone zu verbinden. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines Azure-Load Balancers im Tarif „Standard“ mit einem zonalen Front-End
> * Erstellen von Netzwerksicherheitsgruppen zum Definieren von Regeln für eingehenden Datenverkehr
> * Erstellen zonaler virtueller Computer und Anfügen an einen Load Balancer
> * Erstellen eines Integritätstests für den Load Balancer
> * Erstellen von Load Balancer-Regeln
> * Erstellen einer einfachen IIS-Website
> * Anzeigen eines Load Balancers im Betrieb

Informationen zur Verwendung von Verfügbarkeitszonen mit einem Load Balancer im Tarif „Standard“ finden Sie unter [Load Balancer Standard und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md).

Sie können dieses Tutorial auch mit der [Azure-Befehlszeilenschnittstelle](load-balancer-standard-public-zonal-cli.md) durcharbeiten.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter [http://portal.azure.com](http://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-public-standard-load-balancer"></a>Erstellen eines öffentlichen Load Balancers im Tarif „Standard“

Ein Load Balancer im Tarif „Standard“ unterstützt nur eine öffentliche Standard-IP-Adresse. Wenn Sie eine neue öffentliche IP-Adresse beim Erstellen des Load Balancers erstellen, wird dieser automatisch als Version mit der SKU „Standard“ konfiguriert und ist automatisch auch zonenredundant.

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Load Balancer**.
2. Geben Sie auf der Seite **Lastenausgleich erstellen** folgende Werte für den Lastenausgleich ein:
    - *myLoadBalancer*: Name des Lastenausgleichs
    - **Öffentlich**: Lastenausgleichstyp
     - *myPublicIPZonal*: Für die neue öffentliche IP-Adresse, die Sie erstellen. Klicken Sie hierfür auf **Öffentliche IP-Adresse auswählen** und dann auf **Neue erstellen**. Geben Sie als Name *myPublicIP* ein. Für die SKU ist standardmäßig „Standard“ ausgewählt. Wählen Sie unter **Verfügbarkeitszone** die Option **Zone 1** aus.
    - *myResourceGroupZLB*: Name der neuen Ressourcengruppe, die Sie erstellen
    - **westeurope**: Standort
3. Klicken Sie auf **Erstellen**, um den Lastenausgleich zu erstellen.
   
    ![Erstellen einer zonalen Load Balancer Standard-Instanz mit dem Azure-Portal](./media/tutorial-load-balancer-standard-zonal-portal/create-load-balancer-zonal-frontend.png)


## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk sowie zwei virtuelle Computer in der gleichen Zone (d.h. Zone 1) für die Region, die dem Back-End-Pool Ihres Lastenausgleichs hinzugefügt werden soll, und installieren anschließend IIS auf den virtuellen Computern, um den zonenredundanten Lastenausgleich zu testen. Fällt ein virtueller Computer aus, tritt infolgedessen ein Fehler beim Integritätstest für den virtuellen Computer in derselben Zone auf, und der Datenverkehr wird weiterhin von anderen virtuellen Computern in der gleichen Zone verarbeitet.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk**, und geben Sie folgende Werte für das virtuelle Netzwerk ein:
    - *myVnet*: Name des virtuellen Netzwerks
    - *myResourceGroupZLB*: Name der vorhandenen Ressourcengruppe
    - *myBackendSubnet*: Subnetzname
2. Klicken Sie auf **Erstellen**, um das virtuelle Netzwerk zu erstellen.

    ![Erstellen eines virtuellen Netzwerks](./media/tutorial-load-balancer-standard-zonal-portal/create-virtual-network.png)

## <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe

1. Klicken Sie auf der oberen linken Seite des Bildschirms auf **Ressource erstellen**, geben Sie in das Suchfeld *Netzwerksicherheitsgruppe* ein, und klicken Sie auf der Seite „Netzwerksicherheitsgruppe erstellen“ auf **Erstellen**.
2. Geben Sie auf der Seite „Netzwerksicherheitsgruppe erstellen“ die folgenden Werte ein:
    - *myNetworkSecurityGroup*: Für den Namen der Netzwerksicherheitsgruppe.
    - *myResourceGroupLBAZ*: Für den Namen der vorhandenen Ressourcengruppe.
   
    ![Erstellen eines virtuellen Netzwerks](./media/tutorial-load-balancer-standard-zonal-portal/create-network-security-group.png)

### <a name="create-nsg-rules"></a>Erstellen von NSG-Regeln

In diesem Abschnitt erstellen Sie NSG-Regeln, um eingehende HTTP- und RDP-Verbindungen mit dem Azure-Portal zuzulassen.

1. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**. Suchen Sie nach **myNetworkSecurityGroup**, und klicken Sie darauf (in der Ressourcengruppe **myResourceGroupZLB**).
2. Klicken Sie unter **Einstellungen** auf **Eingangssicherheitsregeln** und anschließend auf **Hinzufügen**.
3. Geben Sie für die Eingangssicherheitsregel *myHTTPRule* folgende Werte ein, um eine eingehende HTTP-Verbindung über den Port 80 zuzulassen:
    - *Service Tag* für **Quelle**
    - *Internet* für **Quelldiensttag**
    - *80* für **Zielportbereiche**
    - *TCP* für **Protokoll**
    - *Zulassen* für **Aktion**
    - *100* für **Priorität**
    - *myHTTPRule* für **Name**
    - *Allow HTTP* für **Beschreibung**
4. Klicken Sie auf **OK**.
 
 ![Erstellen eines virtuellen Netzwerks](./media/load-balancer-standard-public-availability-zones-portal/8-load-balancer-nsg-rules.png)

5. Wiederholen Sie die Schritte 2 bis 4, um eine weitere Regel namens *myRDPRule* zu erstellen und eine eingehende RDP-Verbindung über den Port 3389 zu ermöglichen. Verwenden Sie dabei die folgenden Werte:
    - *Service Tag* für **Quelle**
    - *Internet* für **Quelldiensttag**
    - *3389* für **Zielportbereiche**
    - *TCP* für **Protokoll**
    - *Zulassen* für **Aktion**
    - *200* für **Priorität**
    - *myRDPRule* als Name
    - *Allow RDP* als Beschreibung

    ![Erstellen einer RDP-Regel](./media/tutorial-load-balancer-standard-zonal-portal/create-rdp-rule.png)

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Compute** > **Windows Server 2016 Datacenter**, und geben Sie folgende Werte für den virtuellen Computer ein:
    - *myVM1*: Name des virtuellen Computers        
    - *azureuser*: Name des Administratorbenutzers    
    - *myResourceGroupZLB*: Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** und anschließend *myResourceGroupZLB* aus.
2. Klicken Sie auf **OK**.
3. Wählen Sie als Größe des virtuellen Computers **DS1_V2** aus, und klicken Sie auf **Auswählen**.
4. Geben Sie für die VM-Einstellungen folgende Werte ein:
    - *Zone 1*: Für die Verfügbarkeitszone, in der der virtuelle Computer platziert werden soll
    -  *myVnet*: Vergewissern Sie sich, dass als virtuelles Netzwerk diese Option ausgewählt ist.
    - *myVM1PIP*: Für die öffentliche Standard-IP-Adresse, die Sie erstellen. Klicken Sie auf *Neu erstellen*, geben Sie als Name *myVM1PIP* ein, und wählen Sie für „Zone“ die Option **1**. Als SKU der IP-Adresse ist standardmäßig die Option „Standard“ festgelegt.
    - *myBackendSubnet*: Vergewissern Sie sich, dass als Subnetz diese Option ausgewählt ist.
    - *myNetworkSecurityGroup*: Name der Netzwerksicherheitsgruppe (Firewall), die bereits vorhanden ist
5. Klicken Sie auf **Deaktiviert**, um die Startdiagnose zu deaktivieren.
6. Klicken Sie auf **OK**, überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und klicken Sie dann auf **Erstellen**.
7. Erstellen Sie anhand der Schritte 1-6 in Zone 1 einen zweiten virtuellen Computer namens *myVM2* und mit *myVnet* als virtuelles Netzwerk, *myVM2PIP* als öffentliche Standard-IP-Adresse, *myBackendSubnet* als Subnetz und **myNetworkSecurityGroup* als Netzwerksicherheitsgruppe.

    ![Erstellen einer RDP-Regel](./media/tutorial-load-balancer-standard-zonal-portal/create-virtual-machine.png) 

### <a name="install-iis-on-vms"></a>Installieren der IIS auf VMs

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und anschließend in der Ressourcenliste auf **myVM1** (in der Ressourcengruppe *myResourceGroupZLB*).
2. Klicken Sie auf der Seite **Übersicht** auf **Verbinden**, um eine RDP-Verbindung mit dem virtuellen Computer herzustellen.
3. Melden Sie sich mit dem Benutzernamen und dem Kennwort an, die Sie beim Erstellen des virtuellen Computers angegeben haben. (Unter Umständen müssen Sie auf **Weitere Optionen** und dann auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.) Klicken Sie anschließend auf **OK**. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wählen Sie **Ja** aus, um mit dem Herstellen der Verbindung fortzufahren.
4. Navigieren Sie auf dem Serverdesktop zu **Windows-Verwaltungsprogramme**>**Windows PowerShell**.
6. Führen Sie im PowerShell-Fenster die folgenden Befehle aus, um den IIS-Server zu installieren, die Datei „default.htm“ zu entfernen und eine neue Datei „default.htm“ hinzuzufügen, die den Namen des virtuellen Computers enthält:

   ```azurepowershell-interactive
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello from" + $env:computername)
   ```
8. Schließen Sie die RDP-Sitzung mit *myVM1*.
9. Wiederholen Sie die Schritte 1 bis 8 für die Installation von IIS auf *myVM2*.

## <a name="create-load-balancer-resources"></a>Erstellen von Lastenausgleichsressourcen

In diesem Abschnitt konfigurieren Sie Lastenausgleichseinstellungen für einen Back-End-Adresspool und einen Integritätstest. Außerdem geben Sie Lastenausgleichs- und NAT-Regeln an.


### <a name="create-a-backend-address-pool"></a>Erstellen eines Back-End-Adresspools

Zum Verteilen von Datenverkehr auf die virtuellen Computer enthält ein Back-End-Adresspool die IP-Adressen der virtuellen NICs, die mit dem Load Balancer verbunden sind. Erstellen Sie den Back-End-Adresspool *myBackendPool* mit *VM1* und *VM2*.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer*.
2. Klicken Sie unter **Einstellungen** auf **Back-End-Pools** und anschließend auf **Hinzufügen**.
3. Gehen Sie auf der Seite **Back-End-Pool hinzufügen** wie folgt vor:
    - Geben Sie unter „Name“ die Zeichenfolge *myBackEndPool* als Name für Ihren Back-End-Pool ein.
    - Klicken Sie für **Virtuelles Netzwerk** im Dropdownmenü auf *myVNet*
    - Wählen Sie unter **Virtueller Computer** und **IP-Adresse** die Computer *myVM1* und *myVM2* und ihre entsprechenden öffentlichen IP-Adressen aus.
4. Klicken Sie auf **Hinzufügen**.
5. Vergewissern Sie sich, dass in der Back-End-Pool-Einstellung Ihres Lastenausgleichs beide virtuellen Computer (**myVM1** und **myVM2**) angezeigt werden.
 
    ![Erstellen eines Back-End-Pools](./media/tutorial-load-balancer-standard-zonal-portal/create-backend-pool.png) 

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Damit der Load Balancer den Status Ihrer App überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. Erstellen Sie zur Überwachung der Integrität der virtuellen Computer einen Integritätstest namens *myHealthProbe*.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Integritätstests** und anschließend auf **Hinzufügen**.
3. Verwenden Sie folgende Werte, um den Integritätstest zu erstellen:
    - *myHealthProbe*: Name des Integritätstests
    - **HTTP**: Protokolltyp
    - *80*: Portnummer
    - *15*: Wert für **Intervall** (Sekunden zwischen Testversuchen)
    - *2*: Wert für **Fehlerschwellenwert** oder Anzahl aufeinander folgender Testfehler, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird
4. Klicken Sie auf **OK**.

   ![Hinzufügen eines Tests](./media/load-balancer-standard-public-availability-zones-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport. Erstellen Sie eine Load Balancer-Regel namens *myLoadBalancerRuleWeb*, die an Port 80 des Front-Ends *FrontendLoadBalancer* lauscht und den Netzwerkdatenverkehr nach erfolgtem Lastenausgleich an den Back-End-Adresspool *myBackEndPool* sendet, wobei ebenfalls der Port 80 verwendet wird. 

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Lastenausgleichsregeln** und anschließend auf **Hinzufügen**.
3. Konfigurieren Sie die Lastenausgleichsregel mit folgenden Werten:
    - *myHTTPRule*: Name der Lastenausgleichsregel
    - **TCP**: Protokolltyp
    - *80*: Portnummer
    - *80*: Back-End-Port
    - *myBackendPool*: Name des Back-End-Pools
    - *myHealthProbe*: Name des Integritätstests
4. Klicken Sie auf **OK**.
    
    ![Hinzufügen einer Lastenausgleichsregel](./media/tutorial-load-balancer-standard-zonal-portal/load-balancing-rule.png)

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs
1. Ermitteln Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für den Load Balancer. Klicken Sie auf **Alle Ressourcen** und anschließend auf **myPublicIP**. 

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Die Standardseite mit dem Namen der Webserverseite wird im Browser angezeigt.

      ![IIS-Webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)
3. Wenn Sie den Lastenausgleich in Aktion sehen möchten, können Sie das Beenden des angezeigten virtuellen Computers erzwingen und den Browser aktualisieren, um den anderen Servernamen im Browser anzuzeigen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Lastenausgleich und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe aus, die den Lastenausgleich enthält, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Standard Load Balancer](load-balancer-standard-overview.md).
- [Lastenausgleich für virtuelle Computer über Verfügbarkeitszonen hinweg](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
