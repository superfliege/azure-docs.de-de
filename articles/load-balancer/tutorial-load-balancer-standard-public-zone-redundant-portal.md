---
title: Lastenausgleich für VMs über alle Verfügbarkeitszonen hinweg – Azure-Portal | Microsoft-Dokumentation
description: Erstellen Sie einen Standard-Lastenausgleich mit einem zonenredundanten Front-End, um mit dem Azure-Portal einen Lastenausgleich für VMs über alle Verfügbarkeitszonen hinweg durchzuführen.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/18
ms.author: kumud
ms.openlocfilehash: ad476922342844a908961960407eb344711932f5
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="load-balance-vms-across-availability-zones-with-a-standard-load-balancer-using-the-azure-portal"></a>Lastenausgleich für VMs über alle Verfügbarkeitszonen hinweg mit einem Standard-Lastenausgleich im Azure-Portal

In diesem Artikel werden die Schritte zum Erstellen eines öffentlichen Standard-Lastenausgleichs mit einem zonenredundanten Front-End erläutert, um Zonenredundanz ohne Abhängigkeit von mehreren DNS-Einträgen zu erzielen. Eine einzelne Front-End-IP-Adresse in einem Standard-Lastenausgleich ist automatisch zonenredundant. Durch die Verwendung eines zonenredundanten Front-Ends für Ihren Lastenausgleich können Sie mit einer einzelnen IP-Adresse jetzt eine beliebige VM in einem virtuellen Netzwerk innerhalb einer Region über alle Verfügbarkeitszonen hinweg erreichen. Verwenden Sie Verfügbarkeitszonen, um Ihre Apps und Daten vor einem unwahrscheinlichen Fehler oder Ausfall eines gesamten Datencenters zu schützen. Bei Zonenredundanz können eine oder mehrere Verfügbarkeitszonen ausfallen, wobei der Datenpfad solange überdauert, wie eine Zone in der Region fehlerfrei bleibt. 

Informationen zur Verwendung von Verfügbarkeitszonen mit einem Standard-Lastenausgleich finden Sie unter [Standard-Lastenausgleich und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter [http://portal.azure.com](http://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-public-standard-load-balancer"></a>Erstellen eines öffentlichen Lastenausgleichs im Standard-Tarif

Ein Load Balancer im Standard-Tarif unterstützt nur eine öffentliche Standard-IP-Adresse. Wenn Sie eine neue öffentliche IP-Adresse beim Erstellen des Lastenausgleichs erstellen, wird dieser automatisch als Version mit der SKU „Standard“ konfiguriert und ist automatisch auch zonenredundant.

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Load Balancer**.
2. Geben Sie auf der Seite **Lastenausgleich erstellen** folgende Werte für den Lastenausgleich ein:
    - *myLoadBalancer*: Name des Lastenausgleichs
    - **Öffentlich**: Lastenausgleichstyp
     - *myPublicIP*: Für die neue öffentliche IP-Adresse, die Sie erstellen. Klicken Sie hierfür auf **Öffentliche IP-Adresse auswählen** und dann auf **Neue erstellen**. Wählen Sie für den Namenstyp *myPublicIP* die SKU „Standard“ und **Zonenredundant** für **Verfügbarkeitszone** aus.
    - *myResourceGroupLBAZ*: Für den Namen der neuen Ressourcengruppe, die Sie erstellen.
    - **westeurope**: Standort
3. Klicken Sie auf **Erstellen**, um den Lastenausgleich zu erstellen.
   
    ![Einrichten eines Load Balancers](./media/load-balancer-standard-public-availability-zones-portal/1a-load-balancer.png)


## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk sowie virtuelle Computer in verschiedenen Zonen (Zone 1, Zone 2 und Zone 3) für die Region, die dem Back-End-Pool Ihres Lastenausgleichs hinzugefügt werden soll, und installieren anschließend die IIS auf den virtuellen Computern, um den zonenredundanten Lastenausgleich zu testen. Fällt eine Zone aus, tritt infolgedessen ein Fehler beim Integritätstest für die VM in derselben Zone auf, und der Datenverkehr wird weiterhin von VMs in den anderen Zonen verarbeitet.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk**, und geben Sie folgende Werte für das virtuelle Netzwerk ein:
    - *myVnet*: Name des virtuellen Netzwerks
    - *myResourceGroupLBAZ*: Für den Namen der vorhandenen Ressourcengruppe.
    - *myBackendSubnet*: Subnetzname
2. Klicken Sie auf **Erstellen**, um das virtuelle Netzwerk zu erstellen.

    ![Erstellen eines virtuellen Netzwerks](./media/load-balancer-standard-public-availability-zones-portal/2-load-balancer-virtual-network.png)

## <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe

1. Klicken Sie auf der oberen linken Seite des Bildschirms auf **Ressource erstellen**, geben Sie in das Suchfeld *Netzwerksicherheitsgruppe* ein, und klicken Sie auf der Seite „Netzwerksicherheitsgruppe erstellen“ auf **Erstellen**.
2. Geben Sie auf der Seite „Netzwerksicherheitsgruppe erstellen“ die folgenden Werte ein:
    - *myNetworkSecurityGroup*: Für den Namen der Netzwerksicherheitsgruppe.
    - *myResourceGroupLBAZ*: Für den Namen der vorhandenen Ressourcengruppe.
   
![Erstellen eines virtuellen Netzwerks](./media/load-balancer-standard-public-availability-zones-portal/create-nsg.png)

### <a name="create-nsg-rules"></a>Erstellen von NSG-Regeln

In diesem Abschnitt erstellen Sie NSG-Regeln, um eingehende HTTP- und RDP-Verbindungen mit dem Azure-Portal zuzulassen.

1. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**. Suchen Sie nach **myNetworkSecurityGroup**, und klicken Sie darauf (in der Ressourcengruppe **myResourceGroupLBAZ**).
2. Klicken Sie unter **Einstellungen** auf **Eingangssicherheitsregeln** und anschließend auf **Hinzufügen**.
3. Geben Sie für die Eingangssicherheitsregel *myHTTPRule* folgende Werte ein, um eine eingehende HTTP-Verbindung über den Port 80 zuzulassen:
    - *Service Tag* für **Quelle**
    - *Internet* für **Quelldiensttag**
    - *80* für **Zielportbereiche**
    - *TCP* für **Protokoll**
    - *Zulassen* für **Aktion**
    - *100* für **Priorität**
    - *myHTTPRule* als Name
    - *Allow HTTP* als Beschreibung
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


### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Compute** > **Windows Server 2016 Datacenter**, und geben Sie folgende Werte für den virtuellen Computer ein:
    - *myVM1*: Name des virtuellen Computers        
    - *azureuser*: Name des Administratorbenutzers    
    - *myResourceGroupLBAZ*: Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** und anschließend *myResourceGroupLBAZ* aus.
2. Klicken Sie auf **OK**.
3. Wählen Sie als Größe des virtuellen Computers **DS1_V2** aus, und klicken Sie auf **Auswählen**.
4. Geben Sie für die VM-Einstellungen folgende Werte ein:
    - *Zone 1* – Für die Zone, in die die VM platziert werden soll.
    -  *myVnet*: Vergewissern Sie sich, dass als virtuelles Netzwerk diese Option ausgewählt ist.
    - *myBackendSubnet*: Vergewissern Sie sich, dass als Subnetz diese Option ausgewählt ist.
    - *myNetworkSecurityGroup*: Für den Namen der Netzwerksicherheitsgruppe (Firewall).
5. Klicken Sie auf **Deaktiviert**, um die Startdiagnose zu deaktivieren.
6. Klicken Sie auf **OK**, überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und klicken Sie dann auf **Erstellen**.
  
 ![Erstellen eines virtuellen Computers](./media/load-balancer-standard-public-availability-zones-portal/create-vm-standard-ip.png)

7. Erstellen Sie anhand der Schritte 1-6 in Zone 2 eine zweite VM namens *VM2* und in Zone 3 eine dritte VM mit *myVnet* als virtuelles Netzwerk, *myBackendSubnet* als Subnetz und **myNetworkSecurityGroup* als Netzwerksicherheitsgruppe.

### <a name="install-iis-on-vms"></a>Installieren der IIS auf VMs

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und anschließend in der Ressourcenliste auf **myVM1** (in der Ressourcengruppe *myResourceGroupLBAZ*).
2. Klicken Sie auf der Seite **Übersicht** auf **Verbinden**, um eine RDP-Verbindung mit dem virtuellen Computer herzustellen.
3. Melden Sie mit dem Benutzernamen *azureuser* bei dem virtuellen Computer.
4. Navigieren Sie auf dem Serverdesktop zu **Windows-Verwaltungsprogramme**>**Server-Manager**.
5. Klicken Sie auf der Schnellstartseite des Server-Manager auf **Rollen und Features hinzufügen**.

   ![Hinzufügen zum Back-End-Adresspool ](./media/load-balancer-standard-public-availability-zones-portal/servermanager.png)    

1. Verwenden Sie im Assistenten **Rollen und Features hinzufügen** folgende Werte:
    - Klicken Sie auf der Seite **Installationstyp auswählen** auf die Option **Rollenbasierte oder featurebasierte Installation**.
    - Klicken Sie auf der Seite **Zielserver auswählen** auf **myVM1**.
    - Klicken Sie auf der Seite **Serverrolle auswählen** auf **Webserver (IIS)**.
    - Folgen Sie den Anweisungen, um die restlichen Schritte des Assistenten abzuschließen.
2. Schließen Sie die RDP-Sitzung mit dem virtuellen Computer *myVM1*.
3. Wiederholen Sie die Schritte 1 bis 7 für die Installation der IIS auf den VMs *myVM2* und *myVM3*.

## <a name="create-load-balancer-resources"></a>Erstellen von Lastenausgleichsressourcen

In diesem Abschnitt konfigurieren Sie Lastenausgleichseinstellungen für einen Back-End-Adresspool und einen Integritätstest. Außerdem geben Sie Lastenausgleichs- und NAT-Regeln an.


### <a name="create-a-backend-address-pool"></a>Erstellen eines Back-End-Adresspools

Zum Verteilen von Datenverkehr auf die virtuellen Computer enthält ein Back-End-Adresspool die IP-Adressen der virtuellen NICs, die mit dem Load Balancer verbunden sind. Erstellen Sie den Back-End-Adresspool *myBackendPool* mit *VM1* und *VM2*.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Back-End-Pools** und anschließend auf **Hinzufügen**.
3. Gehen Sie auf der Seite **Back-End-Pool hinzufügen** wie folgt vor:
    - Geben Sie unter „Name“ die Zeichenfolge „*myBackEndPool“ als Name für Ihren Back-End-Pool ein.
    - Klicken Sie für **Virtuelles Netzwerk** im Dropdownmenü auf **myVNet**
    - Klicken Sie für **Virtueller Computer** im Dropdownmenü auf **myVM1**.
    - Klicken Sie für **IP-Adresse** im Dropdownmenü auf die IP-Adresse von myVM1.
4. Klicken Sie auf **Neue Back-End-Ressource hinzufügen**, um jeden virtuelle Computer hinzuzufügen (*myVM2* und *myVM3*), um den Back-End-Pool des Lastenausgleichs hinzuzufügen.
5. Klicken Sie auf **Hinzufügen**.

    ![Hinzufügen zum Back-End-Adresspool ](./media/load-balancer-standard-public-availability-zones-portal/add-backend-pool.png)

3. Vergewissern Sie sich, dass in der Back-End-Pool-Einstellung Ihres Lastenausgleichs alle drei VMs (**myVM1**, **myVM2** und **myVM3**) angezeigt werden.

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

Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport. Erstellen Sie eine Lastenausgleichsregel namens *myLoadBalancerRuleWeb*, die an Port 80 des Front-Ends *FrontendLoadBalancer* lauscht und den Netzwerkdatenverkehr nach erfolgtem Lastenausgleich an den Back-End-Adresspool *myBackEndPool* sendet, wobei ebenfalls der Port 80 verwendet wird. 

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
    
    ![Hinzufügen einer Lastenausgleichsregel](./media/load-balancer-standard-public-availability-zones-portal/load-balancing-rule.png)

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs
1. Ermitteln Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für den Load Balancer. Klicken Sie auf **Alle Ressourcen** und anschließend auf **myPublicIP**.

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Die Standardseite des IIS-Webservers wird im Browser angezeigt.

      ![IIS-Webserver](./media/load-balancer-standard-public-availability-zones-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Lastenausgleich und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe aus, die den Lastenausgleich enthält, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Übersicht: Azure Load Balancer Standard (Preview)](load-balancer-standard-overview.md).
