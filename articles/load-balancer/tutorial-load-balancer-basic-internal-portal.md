---
title: 'Tutorial: Erstellen eines öffentlichen Load Balancers im Tarif „Basic“ – Azure-Portal | Microsoft-Dokumentation'
description: In diesem Tutorial wird gezeigt, wie Sie einen internen Load Balancer im Tarif „Basic“ über das Azure-Portal erstellen.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As an IT administrator, I want to create a load balancer that load balances incoming internet traffic to virtual machines within a specific zone in a region.
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: c0d19c53a0bd217935a494dfb4affbaa85062247
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097477"
---
# <a name="tutorial-load-balance-internal-traffic-with-basic-load-balancer-to-vms-using-the-azure-portal"></a>Tutorial: Durchführen eines Lastenausgleichs für internen Datenverkehr an virtuelle Computer mit einem Load Balancer im Tarif „Basic“ über das Azure-Portal

Durch die Verteilung der eingehenden Anforderungen auf mehrere virtuelle Computer bietet ein Lastenausgleich ein höheres Maß an Verfügbarkeit und Skalierbarkeit. Sie können das Azure-Portal verwenden, um mithilfe eines Load Balancers im Tarif „Basic“ einen Lastenausgleich für internen Datenverkehr an virtuelle Computer durchzuführen. In diesem Tutorial erfahren Sie, wie Sie Netzwerkressourcen, Back-End-Server und einen internen Load Balancer im Tarif „Basic“ erstellen.

Dieser Artikel kann auch mit der [Azure-Befehlszeilenschnittstelle](load-balancer-get-started-ilb-arm-cli.md) oder mit [Azure PowerShell](load-balancer-get-started-ilb-arm-ps.md) durchgearbeitet werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
1. Klicken Sie links oben auf dem Bildschirm auf **Neu** > **Netzwerk** > **Virtuelles Netzwerk**, und geben Sie folgende Werte für das virtuelle Netzwerk an:
    - *myVnet*: Name des virtuellen Netzwerks
    - *myResourceGroupILB*: Name der vorhandenen Ressourcengruppe
    - *myBackendSubnet*: Subnetzname
2. Klicken Sie auf **Erstellen**, um das virtuelle Netzwerk zu erstellen.

![Einrichten eines Load Balancers](./media/tutorial-load-balancer-basic-internal-portal/1-load-balancer.png)

## <a name="create-a-basic-load-balancer"></a>Erstellen eines Load Balancers im Tarif „Basic“
Erstellen Sie einen internen Load Balancers im Tarif „Basic“ über das Portal.

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Load Balancer**.
2. Geben Sie auf der Seite **Lastenausgleich erstellen** folgende Werte für den Lastenausgleich ein:
    - *myLoadBalancer*: Name des Lastenausgleichs
    - **Intern**: Lastenausgleichstyp
    - **Basic**: SKU-Version
    - **10.1.0.7**: Statische private IP-Adresse
    - *myVNet*: Virtuelles Netzwerk, das Sie aus der Liste der vorhandenen Netzwerke auswählen
    - *mySubnet*: Subnetz, das Sie aus der Liste der vorhandenen Subnetze auswählen
    - *myResourceGroupILB*: Name der neuen Ressourcengruppe, die Sie erstellen
3. Klicken Sie auf **Erstellen**, um den Lastenausgleich zu erstellen.
   
    ## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt erstellen Sie zwei virtuelle Computer für den Back-End-Pool Ihres Load Balancers im Tarif „Basic“ und installieren anschließend IIS auf den virtuellen Computern, um den Lastenausgleich zu testen.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Compute** > **Windows Server 2016 Datacenter**, und geben Sie folgende Werte für den virtuellen Computer ein:
    - *myVM1*: Name des virtuellen Computers        
    - *azureuser*: Name des Administratorbenutzers   
    - *myResourceGroupILB*: Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** und anschließend *myResourceGroupILB* aus.
2. Klicken Sie auf **OK**.
3. Wählen Sie als Größe des virtuellen Computers **DS1_V2** aus, und klicken Sie auf **Auswählen**.
4. Geben Sie für die VM-Einstellungen folgende Werte ein:
    - *myAvailabilitySet*: Name der neuen Verfügbarkeitsgruppe, die Sie erstellen
    -  *myVnet*: Vergewissern Sie sich, dass als virtuelles Netzwerk diese Option ausgewählt ist.
    - *myBackendSubnet*: Vergewissern Sie sich, dass als Subnetz diese Option ausgewählt ist.
5. Wählen Sie unter **Netzwerksicherheitsgruppe** die Option **Erweitert**. Wählen Sie anschließend für **Netzwerksicherheitsgruppe (Firewall)** die Option **Keine**.
5. Klicken Sie auf **Deaktiviert**, um die Startdiagnose zu deaktivieren.
6. Klicken Sie auf **OK**, überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und klicken Sie dann auf **Erstellen**.
7. Erstellen Sie anhand der Schritte 1 bis 6 einen zweiten virtuellen Computer mit dem Namen *VM2* mit *myAvailabilityset* als Verfügbarkeitsgruppe, *myVnet* als virtuellem Netzwerk, *myBackendSubnet* als Subnetz und **Keine** für **Netzwerksicherheitsgruppe (Firewall)**. 

### <a name="install-iis-and-customize-the-default-web-page"></a>Installieren von IIS und Anpassen der Standardwebseite

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und anschließend in der Ressourcenliste auf **myVM1** (in der Ressourcengruppe *myResourceGroupILB*).
2. Klicken Sie auf der Seite **Übersicht** auf **Verbinden**, um eine RDP-Verbindung mit dem virtuellen Computer herzustellen.
3. Melden Sie sich am virtuellen Computer an.
4. Navigieren Sie auf dem Serverdesktop zu **Windows-Verwaltungsprogramme**>**Server-Manager**.
5. Starten Sie Windows PowerShell auf VM1, und verwenden Sie die folgenden Befehle, um den IIS-Server zu installieren und die HTM-Standarddatei zu aktualisieren.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```
5. Schließen Sie die RDP-Verbindung mit *myVM1*.
6. Wiederholen Sie die Schritte 1 bis 5 zum Installieren von IIS und Anpassen der Standardwebseite für *myVM2*.

## <a name="create-basic-load-balancer-resources"></a>Erstellen von Ressourcen für den Load Balancer im Tarif „Basic“

In diesem Abschnitt konfigurieren Sie Lastenausgleichseinstellungen für einen Back-End-Adresspool und einen Integritätstest. Außerdem geben Sie Lastenausgleichs- und NAT-Regeln an.


### <a name="create-a-backend-address-pool"></a>Erstellen eines Back-End-Adresspools

Zum Verteilen von Datenverkehr auf die virtuellen Computer enthält ein Back-End-Adresspool die IP-Adressen der virtuellen NICs, die mit dem Load Balancer verbunden sind. Erstellen Sie den Back-End-Adresspool *myBackendPool* mit *VM1* und *VM2*.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Back-End-Pools** und anschließend auf **Hinzufügen**.
3. Gehen Sie auf der Seite **Back-End-Pool hinzufügen** wie folgt vor:
    - Geben Sie unter „Name“ die Zeichenfolge *myBackEndPool* als Name für Ihren Back-End-Pool ein.
    - Wählen Sie in der Dropdownliste **Zugeordnet zu** die Option **Verfügbarkeitsgruppe** aus.
    - Wählen Sie unter **verfügbarkeitsgruppe** die Option **myAvailabilitySet** aus.
    - Klicken Sie auf **+ Zielnetzwerk-IP-Konfiguration hinzufügen**, um die virtuellen Computer (*myVM1* & *myVM2*), die Sie erstellt haben, dem Back-End-Pool hinzuzufügen.
    - Klicken Sie auf **OK**.

        ![Hinzufügen zum Back-End-Adresspool ](./media/tutorial-load-balancer-basic-internal-portal/3-load-balancer-backend-02.png)

3. Vergewissern Sie sich, dass in der Back-End-Pool-Einstellung Ihres Lastenausgleichs beide virtuellen Computer (**VM1** und **VM2**) angezeigt werden.

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Damit der Load Balancer im Tarif „Basic“ den Status Ihrer App überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. Erstellen Sie zur Überwachung der Integrität der virtuellen Computer einen Integritätstest namens *myHealthProbe*.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Integritätstests** und anschließend auf **Hinzufügen**.
3. Verwenden Sie folgende Werte, um den Integritätstest zu erstellen:
    - *myHealthProbe*: Name des Integritätstests
    - **HTTP**: Protokolltyp
    - *80*: Portnummer
    - *15*: Wert für **Intervall** (Sekunden zwischen Testversuchen)
    - *2*: Wert für **Fehlerschwellenwert** oder Anzahl aufeinander folgender Testfehler, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird
4. Klicken Sie auf **OK**.

   ![Hinzufügen eines Tests](./media/tutorial-load-balancer-basic-internal-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport. Erstellen Sie eine Load Balancer-Regel namens *myLoadBalancerRuleWeb*, die an Port 80 des Front-Ends *LoadBalancerFrontEnd* lauscht und den Netzwerkdatenverkehr nach erfolgtem Lastenausgleich an den Back-End-Adresspool *myBackEndPool* sendet, wobei ebenfalls der Port 80 verwendet wird. 

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
    
    ![Hinzufügen einer Lastenausgleichsregel](./media/tutorial-load-balancer-basic-internal-portal/5-load-balancing-rules.png)

## <a name="create-a-virtual-machine-to-test-the-load-balancer"></a>Erstellen eines virtuellen Computers zum Testen des Load Balancers
Zum Testen des internen Load Balancers müssen Sie einen virtuellen Computer erstellen, der sich in demselben virtuellen Netzwerk wie die virtuellen Computer der Back-End-Server befindet.
1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Compute** > **Windows Server 2016 Datacenter**, und geben Sie folgende Werte für den virtuellen Computer ein:
    - *myVMTest*: Name des virtuellen Computers        
    - *myResourceGroupILB*: Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** und anschließend *myResourceGroupILB* aus.
2. Klicken Sie auf **OK**.
3. Wählen Sie als Größe des virtuellen Computers **DS1_V2** aus, und klicken Sie auf **Auswählen**.
4. Geben Sie für die VM-Einstellungen folgende Werte ein:
    -  *myVnet*: Vergewissern Sie sich, dass als virtuelles Netzwerk diese Option ausgewählt ist.
    - *myBackendSubnet*: Vergewissern Sie sich, dass als Subnetz diese Option ausgewählt ist.
5. Klicken Sie auf **Deaktiviert**, um die Startdiagnose zu deaktivieren.
6. Klicken Sie auf **OK**, überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und klicken Sie dann auf **Erstellen**.

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs
1. Ermitteln Sie im Azure-Portal auf dem Bildschirm **Übersicht** die private IP-Adresse für den Load Balancer. Gehen Sie dazu wie folgt vor: a. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer**.
    b. Kopieren Sie auf der Detailseite **Übersicht** die private IP-Adresse (in diesem Beispiel 10.1.0.7).

2. Erstellen Sie eine Remoteverbindung mit *myVMTest* wie folgt: a. Klicken Sie im linken Menü auf **Alle Ressourcen** und anschließend in der Ressourcenliste auf **myVMTest** (in der Ressourcengruppe *myResourceGroupILB*).
2. Klicken Sie auf der Seite **Übersicht** auf **Verbinden**, um eine Remotesitzung mit dem virtuellen Computer zu starten.
3. Melden Sie sich bei *myVMTest* an.
3. Fügen Sie die private IP-Adresse in der Adressleiste des Browsers in *myVMTest* ein. Die Standardseite des IIS-Webservers wird im Browser angezeigt.

      ![IIS-Webserver](./media/tutorial-load-balancer-basic-internal-portal/9-load-balancer-test.png)

Sie können eine erzwungene Aktualisierung Ihres Webbrowsers durchführen, um zu verfolgen, wie der Load Balancer den Datenverkehr auf die beiden virtuellen Computer verteilt, auf denen Ihre App ausgeführt wird.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Lastenausgleich und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe aus, die den Lastenausgleich enthält, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Ressourcengruppe, Netzwerkressourcen und Back-End-Server erstellt. Anschließend haben Sie diese Ressourcen verwendet, um einen internen Load Balancer für den Lastenausgleich beim internen Datenverkehr an virtuelle Computer zu erstellen. Erfahren Sie als Nächstes, wie Sie [einen verfügbarkeitszonenübergreifenden Lastenausgleich für virtuelle Computer durchführen](tutorial-load-balancer-standard-public-zone-redundant-portal.md).
