---
title: 'Tutorial: Erstellen und Verwalten einer Load Balancer Standard-Instanz – Azure-Portal | Microsoft-Dokumentation'
description: In diesem Tutorial wird gezeigt, wie Sie über das Azure-Portal eine Load Balancer Standard-Instanz erstellen und verwalten.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create and Standard Load balancer so that I can load balance internet traffic to VMs and add and remove VMs from the load-balanced set.
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/18
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 7c3e5c0cc8297ba60925d36d667e0b72a5072553
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44380045"
---
# <a name="tutorial-create-and-manage-standard-load-balancer-using-the-azure-portal"></a>Tutorial: Erstellen und Verwalten einer Load Balancer Standard-Instanz mit dem Azure-Portal

Durch die Verteilung der eingehenden Anforderungen auf mehrere virtuelle Computer bietet ein Lastenausgleich ein höheres Maß an Verfügbarkeit und Skalierbarkeit. In diesem Tutorial lernen Sie die verschiedenen Komponenten von Azure Load Balancer Standard kennen, mit denen Datenverkehr verteilt und Hochverfügbarkeit bereitgestellt wird. Folgendes wird vermittelt:


> [!div class="checklist"]
> * Erstellen einer Azure Load Balancer-Instanz
> * Erstellen von virtuellen Computern und Installieren des IIS-Servers
> * Erstellen von Load Balancer-Ressourcen
> * Anzeigen eines Load Balancers im Betrieb
> * Hinzufügen und Entfernen von virtuellen Computern zu bzw. aus einem Load Balancer

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich unter [http://portal.azure.com](http://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-standard-load-balancer"></a>Einrichten eines Load Balancers im Tarif „Standard“

In diesem Abschnitt erstellen Sie einen öffentlichen Load Balancer für den Lastenausgleich virtueller Computer. Ein Load Balancer im Standard-Tarif unterstützt nur eine öffentliche Standard-IP-Adresse. Wenn Sie einen Load Balancer im Standard-Tarif (Load Balancer Standard) erstellen, müssen Sie dafür auch eine neue öffentliche Standard-IP-Adresse erstellen, die als Front-End konfiguriert ist. Dieses hat standardmäßig den Namen *LoadBalancerFrontend*. 

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Load Balancer**.
2. Geben Sie auf der Seite **Lastenausgleich erstellen** die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Erstellen**:
    
    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | NAME                   | *myLoadBalancer*                                   |
    | Typ          | Öffentlich                                        |
    | SKU           | Standard                          |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**, und geben Sie *myPublicIP* in das Textfeld ein. Für die öffentliche IP-Adresse ist standardmäßig die Standard-SKU ausgewählt. Wählen Sie für **Verfügbarkeitszone** die Option **Zonenredundant** aus. |
    | Abonnement               | Wählen Sie Ihr Abonnement aus.    |
    |Ressourcengruppe | Klicken Sie auf **Neu erstellen**, und geben Sie *myResourceGroupSLB* ein.    |
    | Standort           | Wählen Sie **Europa, Westen** aus.                          |
    

![Einrichten eines Load Balancers](./media/load-balancer-standard-public-portal/create-load-balancer.png)
   
## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk sowie drei virtuelle Computer für den Back-End-Pool Ihres Load Balancers und installieren anschließend IIS auf den virtuellen Computern, um den Lastenausgleich zu testen.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
1. Wählen Sie im Azure-Portal links oben **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus, und geben Sie dann diese Werte für das virtuelle Netzwerk ein:
    |Einstellung|Wert|
    |---|---|
    |NAME|Geben Sie *myVNet* ein.|
    |Abonnement| Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe| Wählen Sie **Vorhandene verwenden** und dann *myResourceGroupSLB* aus.|
    |Subnetzname| Geben Sie *myBackendSubnet* ein.|
    
2. Klicken Sie auf **Erstellen**, um das virtuelle Netzwerk zu erstellen.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

1. Wählen Sie im Azure-Portal links oben die Option **Ressource erstellen** > **Compute** > **Windows Server 2016 Datacenter**, und geben Sie folgende Werte für den virtuellen Computer ein:
    1. Geben Sie *myVM1* als Name des virtuellen Computers ein.        
    2. Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** und dann *myResourceGroupSLB* aus.
2. Klicken Sie auf **OK**.
3. Wählen Sie als Größe des virtuellen Computers **DS1_V2** aus, und klicken Sie auf **Auswählen**.
4. Geben Sie für die VM-Einstellungen folgende Werte ein:
    1. Stellen Sie sicher, dass als virtuelles Netzwerk *myVNet* und als Subnetz *myBackendSubnet* ausgewählt ist.
    2. Wählen Sie im Bereich **Öffentliche IP-Adresse erstellen** für **Öffentliche IP-Adresse** die Option **Standard** aus, und klicken Sie dann auf **OK**.
    3. Wählen Sie unter **Netzwerksicherheitsgruppe** die Option **Erweitert**, und führen Sie die folgenden Schritte aus:
        1. Wählen Sie *Netzwerksicherheitsgruppe (Firewall)* und auf der Seite **Netzwerksicherheitsgruppe auswählen** die Option **Neu erstellen**. 
        2. Geben Sie auf der Seite **Netzwerksicherheitsgruppe auswählen** unter **Name** den Namen *myNetworkSecurityGroup* als Name für die neue Netzwerksicherheitsgruppe ein, und wählen Sie dann **OK**.
5. Klicken Sie auf **Deaktiviert**, um die Startdiagnose zu deaktivieren.
6. Klicken Sie auf **OK**, überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und klicken Sie dann auf **Erstellen**.
7. Erstellen Sie zwei weitere virtuelle Computer mit den Namen *VM2* und *VM3* mit *myVnet* als virtuelles Netzwerk, *myBackendSubnet* als Subnetz und *myNetworkSecurityGroup* als Netzwerksicherheitsgruppe, indem Sie die Schritte 1 bis 6 ausführen. 

### <a name="create-network-security-group-rule"></a>Erstellen einer Netzwerksicherheitsgruppen-Regel

In diesem Abschnitt erstellen Sie eine NSG-Regel, um eingehende Verbindungen über HTTP zuzulassen.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und anschließend in der Ressourcenliste auf **myNetworkSecurityGroup** (in der Ressourcengruppe **myResourceGroupSLB**).
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
4. Wählen Sie **Hinzufügen**.

### <a name="install-iis-on-vms"></a>Installieren von IIS auf VMs

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und anschließend in der Ressourcenliste auf **myVM1** (in der Ressourcengruppe *myResourceGroupSLB*).
2. Klicken Sie auf der Seite **Übersicht** auf **Verbinden**, um eine RDP-Verbindung mit dem virtuellen Computer herzustellen.
3. Wählen Sie im Fenster **Herstellen der Verbindung mit dem virtuellen Computer** die Option **RDP-Datei herunterladen** aus, und öffnen Sie dann die heruntergeladene RDP-Datei.
4. Klicken Sie im Fenster **Remotedesktopverbindung** auf **Verbinden**.
5. Melden Sie sich mit den Anmeldeinformationen, die Sie während der Erstellung dieses virtuellen Computers bereitgestellt haben, beim virtuellen Computer an. Dadurch wird eine Remotedesktopsitzung mit dem virtuellen Computer *myVM1* gestartet.
6. Navigieren Sie auf dem Serverdesktop zu **Windows-Verwaltungsprogramme**>**Windows PowerShell**.
7. Führen Sie im PowerShell-Fenster die folgenden Befehle aus, um den IIS-Server zu installieren, die Standarddatei „iisstart.htm“ zu entfernen und dann eine neue Datei „iisstart.htm“ hinzuzufügen, die den Namen des virtuellen Computers enthält:

   ```azurepowershell-interactive
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
6. Schließen Sie die RDP-Sitzung mit *myVM1*.
7. Wiederholen Sie die Schritte 1 bis 6, um IIS und die aktualisierte Datei „iisstart.htm“ auf *myVM2* und *myVM3* zu installieren.

## <a name="create-load-balancer-resources"></a>Erstellen von Load Balancer-Ressourcen

In diesem Abschnitt konfigurieren Sie Lastenausgleichseinstellungen für einen Back-End-Adresspool und einen Integritätstest. Außerdem legen Sie eine Lastenausgleichsregel fest.

### <a name="create-a-backend-address-pool"></a>Erstellen eines Back-End-Adresspools

Zum Verteilen von Datenverkehr auf die virtuellen Computer enthält ein Back-End-Adresspool die IP-Adressen der virtuellen NICs, die mit dem Load Balancer verbunden sind. Erstellen Sie den Back-End-Adresspool *myBackendPool* mit *VM1* und *VM2*.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Back-End-Pools** und anschließend auf **Hinzufügen**.
3. Gehen Sie auf der Seite **Back-End-Pool hinzufügen** wie folgt vor:
   - Geben Sie unter „Name“ die Zeichenfolge *myBackEndPool* als Name für Ihren Back-End-Pool ein.
   - Wählen Sie unter **Virtuelles Netzwerk** die Option *myVNet*.
   - Fügen Sie unter **Virtueller Computer** die Computer *myVM1*, *myVM2* und *myVM3* zusammen mit den entsprechenden IP-Adressen hinzu, und klicken Sie auf **Hinzufügen**.
4. Vergewissern Sie sich, dass in der Back-End-Pool-Einstellung für Ihren Lastenausgleich alle drei virtuellen Computer (*myVM1*, *myVM2* und *myVM3*) angezeigt werden, und klicken Sie dann auf **OK**.

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

   ![Hinzufügen eines Tests](./media/load-balancer-standard-public-portal/4-load-balancer-probes.png)

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

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs
1. Ermitteln Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für den Load Balancer. Klicken Sie auf **Alle Ressourcen** und anschließend auf **myPublicIP**.

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Die Standardseite des IIS-Webservers wird im Browser angezeigt.

      ![IIS-Webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Sie können eine erzwungene Aktualisierung Ihres Webbrowsers durchführen, um zu verfolgen, wie der Load Balancer den Datenverkehr auf die drei virtuellen Computer verteilt, auf denen Ihre App ausgeführt wird.

## <a name="remove-or-add-vms-from-the-backend-pool"></a>Entfernen oder Hinzufügen von virtuellen Computern aus dem Back-End-Pool
Für die virtuellen Computer, auf denen Ihre App ausgeführt wird, sind unter Umständen gelegentlich Wartungsarbeiten erforderlich (etwa die Installation von Betriebssystemupdates). Zur Bewältigung eines höheren Datenverkehrsaufkommens für Ihre App müssen gegebenenfalls weitere virtuelle Computer hinzugefügt werden. In diesem Abschnitt erfahren Sie, wie Sie einen virtuellen Computer aus dem Load Balancer entfernen oder dem Load Balancer einen virtuellen Computer hinzufügen.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Back-End-Pools** und dann in der Liste des Back-End-Pools auf **myBackendPool**.
3. Klicken Sie zum Entfernen von *VM1* aus dem Back-End auf der Seite **myBackendPool** unter **Zielnetzwerk-IP-Konfigurationen** neben **Virtual machine:myVM1** auf das Löschen-Symbol.

Wenn *myVM1* nicht mehr im Back-End-Adresspool enthalten ist, können Sie alle Wartungsaufgaben auf *myVM1* durchführen, z.B. die Installation von Softwareupdates. Aufgrund des Fehlens von „VM1“ wird die Last jetzt über *myVM2* und *myVM3* ausgeglichen. 

Führen Sie das Verfahren im Abschnitt *Hinzufügen von VMs zum Back-End-Pool* dieses Artikels durch, um *myVM1* wieder dem Back-End-Pool hinzuzufügen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Lastenausgleich und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe aus, die den Lastenausgleich enthält, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Load Balancer Standard erstellt, virtuelle Computer hinzugefügt, die Datenverkehrsregel für den Lastenausgleich sowie einen Integritätstest konfiguriert und den Lastenausgleich getestet. Außerdem haben Sie eine VM aus der Gruppe mit Lastenausgleich entfernt und sie wieder dem Back-End-Adresspool hinzugefügt. Weitere Informationen zu Azure Load Balancer finden Sie in den Tutorials zu Azure Load Balancer.

> [!div class="nextstepaction"]
> [Azure Load Balancer-Tutorials](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
