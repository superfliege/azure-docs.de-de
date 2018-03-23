---
title: Weiterleiten von Netzwerkdatenverkehr – Azure-Portal | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie mithilfe des Azure-Portals Netzwerkdatenverkehr mit einer Routingtabelle weitergeleitet wird.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 4cc814e1fd3ee8979662695f9dec3dcce335dc2a
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Weiterleiten von Netzwerkdatenverkehr mithilfe des Azure-Portals

Standardmäßig leitet Azure den Datenverkehr automatisch durch alle Subnetze des virtuellen Netzwerks. Sie können Ihre eigenen Routen zum Überschreiben des Azure-Standardroutings erstellen. Die Möglichkeit zum Erstellen von benutzerdefinierten Routen ist hilfreich, wenn Sie z.B. über eine Firewall Datenverkehr durch Subnetze weiterleiten möchten. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Erstellen einer Routingtabelle
> * Erstellen einer Route
> * Zuordnen einer Routingtabelle zu einem Subnetz des virtuellen Netzwerks
> * Testen des Routings
> * Problembehandlung für Routingfehler

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-a-route-table"></a>Erstellen einer Routingtabelle

Standardmäßig leitet Azure den Datenverkehr durch alle Subnetze des virtuellen Netzwerks. Weitere Informationen zu Azure-Standardrouten finden Sie unter [Systemrouten](virtual-networks-udr-overview.md). Um das Azure-Standardrouting zu überschreiben, erstellen Sie eine Routingtabelle mit Routen und ordnen die Routingtabelle zum Subnetz eines virtuellen Netzwerks zu.

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie die Option **Netzwerk** und dann **Routingtabelle**.
3. Wählen Sie Ihr **Abonnement** aus, oder geben Sie die folgenden Informationen ein, und wählen Sie dann **Erstellen** aus:
 
    ![Erstellen einer Routingtabelle](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Erstellen einer Route

Eine Routingtabelle enthält null oder mehr Routen. 

1. Beginnen Sie oben im Portal im Feld *Ressourcen, Dienste und Dokumente durchsuchen* mit der Eingabe von *myRouteTablePublic*. Wenn **myRouteTablePublic** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Klicken Sie unter **EINSTELLUNGEN** auf **Routen** und wählen Sie dann **+ Hinzufügen** aus, wie in der folgenden Abbildung gezeigt wird:

    ![Hinzufügen einer Route](./media/tutorial-create-route-table-portal/add-route.png) 
 
4. Wählen Sie unter **Route hinzufügen** die folgenden Informationen aus, oder geben Sie sie ein, und klicken Sie auf **OK**:
    - **Routenname**: *ToPrivateSubnet*
    - **Adresspräfix**: 10.0.1.0/24
    - **Typ des nächsten Hops**: Wählen Sie **Virtuelles Gerät** aus.
    - **Adresse des nächsten Hops**: 10.0.2.4

    Die Route leitet sämtlichen an das Adresspräfix 10.0.1.0/24 gerichteten Datenverkehr über ein virtuelles Netzwerkgerät mit der IP-Adresse 10.0.2.4 weiter. Das virtuelle Netzwerkgerät und das Subnetz mit dem angegebenen Adresspräfix werden in den späteren Schritten erstellt. Die Route überschreibt das Azure-Standardrouting, das direkt Datenverkehr durch Subnetze weiterleitet. Jede Route gibt einen Typ des nächsten Hops an. Der Typ des nächste Hops weist Azure an, wie der Datenverkehr weitergeleitet werden soll. In diesem Beispiel handelt es sich beim Typ des nächsten Hops um *VirtualAppliance*. Um mehr über alle in Azure verfügbaren Typen des nächsten Hops und deren Verwendung zu erfahren, lesen Sie den Artikel zu den [Typen des nächsten Hops](virtual-networks-udr-overview.md#custom-routes).

## <a name="associate-a-route-table-to-a-subnet"></a>Zuordnen einer Routingtabelle zu einem Subnetz

Bevor Sie eine Routingtabelle zu einem Subnetz zuordnen können, müssen Sie ein virtuelles Netzwerk und ein Subnetz erstellen:

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie **Netzwerk** und anschließend **Virtuelles Netzwerk** aus.
3. Wählen Sie unter **Virtuelles Netzwerk erstellen** aus, oder geben Sie die folgenden Informationen ein, und wählen Sie dann **Erstellen** aus:
    
    - **Name**: *myVirtualNetwork*
    - **Adressraum**: *10.0.0.0/16*
    - **Abonnement**: Wählen Sie Ihr Abonnement aus.
    - **Ressourcengruppe**: Wählen Sie **Vorhandene verwenden** aus, und wählen Sie **myResourceGroup** aus.
    - **Standort**: Wählen Sie *USA, Osten* aus.
    - **Subnetzname**: *Öffentlich*
    - **Adressbereich:** *10.0.0.0/24*

4. Beginnen Sie oben im Portal im Feld **Ressourcen, Dienste und Dokumente durchsuchen** mit der Eingabe von *myVirtualNetwork*. Wenn **myVirtualNetwork** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
5. Fügen Sie zwei zusätzliche Subnetze zum virtuellen Netzwerk hinzu. Klicken Sie unter **EINSTELLUNGEN** auf **Subnetze** und anschließend auf **+ Subnetz**, wie in der folgenden Abbildung gezeigt:

    ![Hinzufügen des Subnetzes](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Wählen Sie folgende Informationen aus, oder geben Sie sie ein, und klicken Sie auf **OK**:
    - **Name**: Privat
    - **Adressraum**: *10.0.1.0/24*

7. Führen Sie erneut die Schritte 5 und 6 durch, und geben Sie dabei folgende Informationen an:
    - **Name**: DMZ
    - **Adressraum**: *10.0.2.0/24*

Sie können null oder mehr Subnetzen eine Routingtabelle zuordnen. Jedem Subnetz können null oder eine Routingtabelle zugeordnet werden. Ausgehender Datenverkehr aus einem Subnetz wird auf Grundlage der Azure-Standardrouten weitergeleitet und alle benutzerdefinierten Routen, die Sie einer Routentabelle hinzugefügt haben, ordnen Sie einem Subnetz zu. Ordnen Sie die Routingtabelle *myRouteTablePublic* dem Subnetz *Öffentlich* zu.

1. Das Feld **myVirtualNetwork – Subnetze** wird nach Durchführung des vorhergehenden Schritts angezeigt. Klicken Sie unter **EINSTELLUNGEN** auf **Subnetze**, und wählen Sie dann **Öffentlich** aus.
2. Wählen Sie wie in der folgenden Abbildung gezeigt **Routingtabelle** und dann **MyRouteTablePublic** aus.

    ![Zuordnen einer Routingtabelle](./media/tutorial-create-route-table-portal/associate-route-table.png) 

3. Wählen Sie **Speichern**aus.

## <a name="test-routing"></a>Testen des Routings

Erstellen Sie zum Testen des Routings einen virtuellen Computer, der als virtuelles Netzwerkgerät fungiert, über das die im vorherigen Schritt erstellte Route Datenverkehr weiterleitet. Nachdem Sie das virtuelle Netzwerkgerät erstellt haben, stellen Sie einen virtuellen Computer in den Subnetzen *Öffentlich* und *Privat* bereit. Dann leiten Sie über das virtuelle Netzwerkgerät Datenverkehr vom Subnetz *Privat* an das Subnetz *Öffentlich* weiter.

### <a name="create-a-network-virtual-appliance"></a>Erstellen eines virtuellen Netzwerkgeräts

In einem vorherigen Schritt haben Sie eine Route erstellt, die ein virtuelles Netzwerkgerät als Typ des nächsten Hops angibt. Wenn ein virtueller Computer eine Netzwerkanwendung ausführt, wird er häufig als virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) bezeichnet. In Produktionsumgebungen handelt es sich bei dem virtuellen Netzwerkgerät, das Sie bereitstellen, häufig um einen vorkonfigurierten virtuellen Computer. Die verschiedenen virtuellen Netzwerkgeräte sind im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1) verfügbar. In diesem Artikel wird ein grundlegender virtueller Computer erstellt.

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**. Sie können ein anderes Betriebssystem auswählen, bei den übrigen Schritten wird jedoch davon ausgegangen, dass Sie **Windows Server 2016 Datacenter** ausgewählt haben. 
3. Wählen Sie folgende Informationen für **Grundlagen** aus, oder geben Sie sie ein, und klicken Sie auf **OK**:
    - **Name**: *myVmNva*
    - **Ressourcengruppe**: Wählen Sie **Vorhandene verwenden** und dann *myResourceGroup* aus.
    - **Standort**: Wählen Sie *USA, Osten* aus.

    Der eingegebene **Benutzername** und das **Kennwort** werden in einem späteren Schritt verwendet. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen. Der ausgewählte **Standort** und das ausgewählte **Abonnement** müssen mit dem Standort und dem Abonnement des virtuellen Netzwerks übereinstimmen. Sie müssen nicht dieselbe Ressourcengruppe auswählen, in der das virtuelle Netzwerk erstellt wurde, für dieses Tutorial wird jedoch die gleiche Ressourcengruppe ausgewählt.
4. Wählen Sie unter **Größe auswählen** eine VM-Größe aus.
5. Wählen Sie folgende Informationen für **Einstellungen** aus, oder geben Sie sie ein, und klicken Sie auf **OK**:
    - **Virtuelles Netzwerk**: Stellen Sie sicher, dass **myVirtualNetwork** ausgewählt ist. Falls dies nicht der Fall sein sollte, klicken Sie auf **Virtuelles Netzwerk**, und wählen Sie unter **Virtuelles Netzwerk auswählen** das Netzwerk **myVirtualNetwork** aus.
    - **Subnetz**: Wählen Sie unter **Subnetz wählen** die Option **Subnetz** und dann **DMZ** aus.
    - **Öffentliche IP-Adresse**: Wählen Sie **Öffentliche IP-Adresse** und **Keine** unter **Öffentliche IP-Adresse wählen** aus. Diesem virtuellen Computer ist keine öffentliche IP-Adresse zugewiesen, da über diesen keine Internetverbindung hergestellt wird.
6. Wählen Sie auf der Seite **Zusammenfassung** unter **Erstellen** die Option **Erstellen**, um die Bereitstellung des virtuellen Computers zu starten.

Das Erstellen des virtuellen Computers dauert einige Minuten. Fahren Sie erst mit dem nächsten Schritt fort, wenn Azure den virtuellen Computer erstellt und ein Feld mit Informationen zum virtuellen Computer geöffnet hat.

Bei der Erstellung des virtuellen Computers wird auch eine [Netzwerkschnittstelle](virtual-network-network-interface.md) im Subnetz *DMZ* erstellt und die Netzwerkschnittstelle wird dem virtuellen Computer angefügt. Die IP-Weiterleitung muss für jede Azure-Netzwerkschnittstelle aktiviert sein, die für eine beliebige IP-Adresse bestimmten Datenverkehr, der nicht der Netzwerkschnittstelle zugewiesen ist, weiterleitet.

1. Wählen Sie in dem Feld, das für den virtuellen Computer nach dessen Erstellung geöffnet wird, unter **EINSTELLUNGEN** die Option **Netzwerk** aus, und wählen Sie dann **myvmnva158** aus (die von Azure erstellte Netzwerkschnittstelle für Ihren virtuellen Computer ist mit einer anderen Nummer nach **myvmnva** versehen), wie in der folgenden Abbildung gezeigt:

    ![Netzwerk für virtuelle Computer](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

    Bei der Erstellung des virtuellen Netzwerkgeräts im Subnetz *DMZ* hat Azure automatisch die Netzwerkschnittstelle erstellt, die Netzwerkschnittstelle dem virtuellen Computer angefügt und der Netzwerkschnittstelle die private IP-Adresse zugewiesen *10.0.2.4*. 

2. Klicken Sie unter **EINSTELLUNGEN** auf **IP-Konfigurationen**, auf **Aktiviert** für die Option **IP-Weiterleitung** und abschließend auf **Speichern** . Dies wird in der folgenden Abbildung veranschaulicht:

    ![Aktivieren der IP-Weiterleitung](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk, damit Sie in einem späteren Schritt diesen Datenverkehr aus dem Subnetz *Öffentlich* über das virtuelle Netzwerkgerät an das Subnetz *Privat* weiterleiten können.

Führen Sie die Schritte 1 bis 6 unter [Erstellen eines virtuellen Computers für das virtuelle Netzwerkgerät](#create-a-network-virtual-appliance) durch. Verwenden Sie die gleichen Einstellungen wie in Schritt 3 und 5 mit Ausnahme der folgenden Änderungen:

|Virtueller Computer   |NAME      |Subnetz      | Öffentliche IP-Adresse     |
|---------         |--------- | -----------|---------              |
|Virtueller Computer 1 | myVmWeb  | Öffentlich     | Standardeinstellung des Portals beibehalten |
|Virtueller Computer 2 | myVmMgmt | Private    | Standardeinstellung des Portals beibehalten |

Sie können den virtuellen Computer *myVmMgmt* erstellen, während Azure den virtuellen Computer *myVmWeb* erstellt. Fahren Sie erst mit den nächsten Schritten fort, wenn Azure die Erstellung beider virtueller Computer abgeschlossen hat.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät

1. Beginnen Sie oben im Portal im Feld *Suche* mit der Eingabe von *myVmMgmt*. Wenn **myVmMgmt** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Erstellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer *myVmMgmt*, indem Sie auf **Verbinden** klicken, wie in der folgenden Abbildung gezeigt:

    ![Herstellen der Verbindung mit dem virtuellen Computer](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit dem virtuellen Computer herzustellen. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.
4. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers angegeben haben. (Unter Umständen müssen Sie auf **More choices** (Weitere Optionen) und dann auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.) Klicken Sie dann auf **OK**.
5. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wählen Sie **Ja** aus, um mit dem Herstellen der Verbindung fortzufahren.
6. In einem späteren Schritt wird das Routing mithilfe des Befehls „tracert.exe“ getestet. Tracert nutzt das ICMP, das von der Windows-Firewall verweigert wird. Aktivieren Sie das ICMP für die Windows-Firewall, indem Sie an einer Eingabeaufforderung den folgenden Befehl eingeben:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    In diesem Artikel wird zwar Tracert verwendet, das Zulassen des ICMP-Datenverkehrs durch die Windows-Firewall wird jedoch nicht für Produktionsbereitstellungen empfohlen.
7. Sie haben die IP-Weiterleitung innerhalb von Azure für die Netzwerkschnittstelle des virtuellen Computers unter [IP-Weiterleitung aktivieren](#enable-ip-forwarding) aktiviert. Ein Betriebssystem oder eine Anwendung, das bzw. die auf dem virtuellen Computer ausgeführt wird, muss ebenfalls Netzwerkdatenverkehr weiterleiten können. Wenn Sie ein virtuelles Netzwerkgerät in einer Produktionsumgebung bereitstellen, führt das Gerät in der Regel vor der Weiterleitung von Datenverkehr eine Filterung oder Protokollierung durch oder führt eine andere Funktion aus. In diesem Artikel leitet das Betriebssystem jedoch einfach den gesamten Datenverkehr weiter, den es empfängt. Aktivieren Sie die IP-Weiterleitung innerhalb des Betriebssystems von *myVmNva*, indem Sie die folgenden Schritte auf dem virtuellen Computer *myVmMgmt* durchführen:

    Stellen Sie an einer Eingabeaufforderung auf dem virtuellen Computer *myVmNva* eine Remotedesktopverbindung mit dem folgenden Befehl her:

    ``` 
    mstsc /v:myvmnva
    ```
    
    Um die IP-Weiterleitung innerhalb des Betriebssystems zu aktivieren, geben Sie den folgenden Befehl in PowerShell ein:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Starten Sie den virtuellen Computer neu, wodurch auch die Remotedesktopsitzung getrennt wird.
8. Nachdem der virtuelle Computer *myVmNva* bei bestehender Verbindung mit dem virtuellen Computer *myVmMgmt* neu gestartet wird, erstellen Sie mit dem folgenden Befehl eine Remotedesktopsitzung auf dem virtuellen Computer *myVmWeb*:

    ``` 
    mstsc /v:myVmWeb
    ```
    
    Aktivieren Sie das ICMP für die Windows-Firewall, indem Sie an einer Eingabeaufforderung den folgenden Befehl eingeben:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

9. Geben Sie an einer Eingabeaufforderung den folgenden Befehl ein, um Netzwerkdatenverkehr vom virtuellen Computer *myVmWeb* an den virtuellen Computer *myVmMgmt* weiterzuleiten:

    ```
    tracert myvmmgmt
    ```

    Die Antwort ähnelt dem folgenden Beispiel:
    
    ```
    Tracing route to myvmmgmt.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:
        
    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4
        
    Trace complete.
    ```
      
    Wie Sie sehen können, ist der erste Hop 10.0.2.4 – die private IP-Adresse des virtuellen Netzwerkgeräts. Der zweite Hop ist 10.0.1.4, die private IP-Adresse des virtuellen Computers *myVmMgmt*. Die Route, die zur Routingtabelle *myRouteTablePublic* hinzugefügt und dem Subnetz *Öffentlich* zugeordnet wurde, hat bewirkt, dass Azure Datenverkehr über die NVA weiterleitet statt direkt über das Subnetz *Privat*.
10.  Schließen Sie die Remotedesktopsitzung für den virtuellen Computer *myVmWeb*, sodass Sie nur noch mit dem virtuellen Computer *myVmMgmt* verbunden sind.
11. Geben Sie an einer Eingabeaufforderung den folgenden Befehl ein, um Netzwerkdatenverkehr vom virtuellen Computer *myVmMgmt* an den virtuellen Computer *myVmWeb* weiterzuleiten:

    ```
    tracert myvmweb
    ```

    Die Antwort ähnelt dem folgenden Beispiel:

    ```
    Tracing route to myvmweb.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:
    
    1     1 ms     1 ms     1 ms  10.0.0.4
    
    Trace complete.
    ```

    Beachten Sie, dass Datenverkehr direkt vom virtuellen Computer *myVmMgmt* an den virtuellen Computer *myVmWeb* weitergeleitet wird. Azure-Routen leiten standardmäßig Datenverkehr direkt durch Subnetze weiter.
12. Schließen Sie die Remotedesktopsitzung für den virtuellen Computer *myVmMgmt*.

## <a name="troubleshoot-routing"></a>Problembehandlung für Routingfehler

Wie Sie in den vorherigen Schritten erfahren haben, wendet Azure Standardrouten an, die Sie wahlweise mit Ihren eigenen Routen überschreiben können. In manchen Fällen kann Datenverkehr nicht gemäß ihren Vorstellungen weitergeleitet werden. Sie können die Funktion [Nächster Hop](../network-watcher/network-watcher-check-next-hop-portal.md) von Azure Network Watcher verwenden, um festzustellen, wie Azure Datenverkehr zwischen zwei virtuellen Computern weiterleitet. 

1. Beginnen Sie oben im Portal im Feld *Suchen* mit der Eingabe von *Network Watcher*. Wählen Sie **Network Watcher** aus, wenn der Begriff in den Suchergebnissen angezeigt wird.
2. Wählen Sie unter **NETZWERKDIAGNOSETOOLS** die Option **Nächster Hop** aus.
3. Um das Datenverkehrrouting zwischen dem virtuellen Computer *myVmWeb* (10.0.0.4) und dem virtuellen Computer *myVmMgmt* (10.0.1.4) zu testen, wählen Sie Ihr Abonnement aus, geben Sie die in der folgenden Abbildung gezeigten Informationen ein (der Name Ihrer **Netzwerkschnittstelle** weicht geringfügig ab), und wählen Sie dann **Nächster Hop** aus:

    ![Nächster Hop](./media/tutorial-create-route-table-portal/next-hop.png)  

    Die **Ergebnisausgabe** gibt an, dass die IP-Adresse des nächsten Hops für den Datenverkehr von *myVmWeb* an *myVmMgmt* 10.0.2.4 lautet (der virtuelle Computer *myVmNva*), der nächste Hop vom Typ *VirtualAppliance* ist und die das Routing auslösende Routingtabelle *myRouteTablePublic* ist.

Die effektiven Routen für die einzelnen Netzwerkschnittstellen stellen eine Kombination aus Azure-Standardrouten und sämtlichen Routen dar, die Sie definieren. Um alle effektiven Routen für eine Netzwerkschnittstelle auf einem virtuellen Computer anzuzeigen, führen Sie die folgenden Schritte durch:

1. Beginnen Sie oben im Portal im Feld *Suchen* mit der Eingabe von *myVmWeb*. Wenn **myVmWeb** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Klicken Sie unter **EINSTELLUNGEN** auf **Netzwerke** und dann auf **myvmweb369** (die von Azure erstellte Netzwerkschnittstelle für Ihren virtuellen Computer ist mit einer anderen Nummer nach **myvmweb** versehen).
3. Wählen Sie unter **SUPPORT + PROBLEMBEHANDLUNG** die Option **Effektive Routen** aus, wie in der folgenden Abbildung dargestellt:

    ![Effektive Routen](./media/tutorial-create-route-table-portal/effective-routes.png) 

    Sie finden die Azure-Standardrouten und die von Ihnen hinzugefügte Route in der Routingtabelle *myRouteTablePublic*. Um mehr darüber zu erfahren, wie Azure eine Route aus der Liste der Routen auswählt, lesen Sie [Auswahl einer Route durch Azure](virtual-networks-udr-overview.md#how-azure-selects-a-route).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit allen ihren Ressourcen, wenn Sie sie nicht mehr benötigen: 

1. Geben Sie im oben im Portal im Feld *Suche* die Zeichenfolge **myResourceGroup** ein. Wenn **myResourceGroup** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie für **Geben Sie den Ressourcengruppennamen ein:** den Namen *myResourceGroup* ein, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine Routingtabelle erstellt und einem Subnetz zugeordnet. Sie haben ein virtuelles Netzwerkgerät erstellt, das Datenverkehr von einem öffentlichen Subnetz an ein privates Subnetz weiterleitet. Sie können zwar viele Azure-Ressourcen in einem virtuellen Netzwerk bereitstellen, für einige Azure-PaaS-Dienste können jedoch keine Ressourcen in einem virtuellen Netzwerk bereitgestellt werden. Allerdings können Sie den Zugriff auf die Ressourcen einiger Azure-PaaS-Dienste nach wie vor ausschließlich auf den Datenverkehr eines Subnetzes eines virtuellen Netzwerks beschränken. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie den Netzwerkzugriff auf Azure-PaaS-Ressourcen beschränken können.

> [!div class="nextstepaction"]
> [Netzwerkzugriff auf PaaS-Ressourcen beschränken](virtual-network-service-endpoints-configure.md#azure-portal)
