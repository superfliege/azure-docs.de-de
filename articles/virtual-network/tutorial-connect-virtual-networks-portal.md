---
title: Herstellen von Verbindungen zwischen virtuellen Netzwerken durch Peerings für virtuelle Netzwerke – Azure-Portal | Microsoft-Dokumentation
description: Hier erfahren Sie, wie mithilfe von Peerings in virtuellen Netzwerken Verbindungen zwischen virtuellen Netzwerken hergestellt werden.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/06/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: ac0c1033546758a77b43298a5fa8cba5f5204650
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-portal"></a>Herstellen von Verbindungen zwischen virtuellen Netzwerken durch Peerings für virtuelle Netzwerke mit dem Azure-Portal

Sie können mithilfe von Peerings für virtuelle Netzwerke Verbindungen zwischen virtuellen Netzwerken herstellen. Sobald ein Peering zwischen virtuellen Netzwerken erstellt wurde, können Ressourcen in beiden virtuellen Netzwerken untereinander mit derselben Latenz und Bandbreite kommunizieren, als befänden sie sich im selben virtuellen Netzwerk. In diesem Artikel werden die Erstellung von virtuellen Netzwerken und das Peering zwischen diesen behandelt. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen zweier virtueller Netzwerke
> * Erstellen eines Peerings zwischen virtuellen Netzwerken
> * Testen des Peerings

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-virtual-networks"></a>Erstellen virtueller Netzwerke

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie **Netzwerk** und anschließend **Virtuelles Netzwerk** aus.
3. Geben Sie wie in der folgenden Abbildung gezeigt *myVirtualNetwork1* für **Name**, *10.0.0.0/16* für **Adressraum**, **myResourceGroup** für **Ressourcengruppe**, *Subnet1* für **Name** des Subnetzes und 10.0.0.0/24 für den **Adressbereich** des Subnetzes ein. Wählen Sie einen **Standort** und Ihr **Abonnement** aus, übernehmen Sie die übrigen Standardeinstellungen, und klicken Sie dann auf **Erstellen**:

    ![Erstellen eines virtuellen Netzwerks](./media/tutorial-connect-virtual-networks-portal/create-virtual-network.png)

4. Führen Sie erneut die Schritte 1 bis 3 mit Ausnahme der folgenden Änderungen durch:
    - **Name**: *myVirtualNetwork2*
    - **Ressourcengruppe**: Wählen Sie **Vorhandene verwenden** und dann **myResourceGroup** aus.
    - **Adressraum**: *10.1.0.0/16*
    - **Subnetzadressbereich**: *10.1.0.0/24*

    Das Adresspräfix für das virtuelle Netzwerk *myVirtualNetwork2* überschneidet sich nicht mit dem Adressraum des virtuellen Netzwerks *myVirtualNetwork1*. Sie können kein Peering zwischen virtuellen Netzwerken mit sich überschneidenden Adressräumen erstellen.

## <a name="peer-virtual-networks"></a>Erstellen eines Peerings für virtuelle Netzwerke

1. Beginnen Sie oben im Azure-Portal im Feld „Suchen“ mit der Eingabe von *MyVirtualNetwork1*. Wenn **myVirtualNetwork1** in den Suchergebnissen angezeigt wird, wählen Sie es aus.
2. Klicken Sie unter **EINSTELLUNGEN** auf **Peerings**, und wählen Sie dann **+ Hinzufügen** aus, wie in der folgenden Abbildung gezeigt wird:

    ![Erstellen eines Peerings](./media/tutorial-connect-virtual-networks-portal/create-peering.png)

3. Geben Sie die in der folgenden Abbildung dargestellten Informationen ein, oder wählen Sie sie aus. Klicken Sie dann auf **OK**. Wählen Sie die Option **Virtuelles Netzwerk** und dann *myVirtualNetwork2* aus.

    ![Peeringeinstellungen](./media/tutorial-connect-virtual-networks-portal/peering-settings.png)

    Der **PEERINGSTATUS** lautet *Initiiert*, wie in der folgenden Abbildung gezeigt wird:

    ![Peeringstatus](./media/tutorial-connect-virtual-networks-portal/peering-status.png)

    Wenn der Status nicht angezeigt wird, aktualisieren Sie Ihren Browser.

4. Suchen Sie nach dem virtuellen Netzwerk *myVirtualNetwork2*. Wenn Sie es in den Suchergebnissen finden, wählen Sie es aus.
5. Führen Sie erneut die Schritte 1 bis 3 mit Ausnahme der folgenden Änderungen durch, und klicken Sie anschließend auf **OK**:
    - **Name**: *myVirtualNetwork2-myVirtualNetwork1*
    - **Virtuelles Netzwerk**: *myVirtualNetwork1*

    Der **PEERINGSTATUS** lautet *Verbunden*. Azure hat zudem den Peeringstatus für das Peering *myVirtualNetwork2-myVirtualNetwork1* von *Initiiert* in *Verbunden* geändert. Das Peering für die virtuellen Netzwerke wird erst erstellt, wenn der Peeringstatus für beide virtuellen Netzwerke *Verbunden* lautet. 

Peerings werden zwischen zwei virtuellen Netzwerken erstellt, Sie sind jedoch nicht transitiv. Wenn Sie daher z.B. ein Peering zwischen *myVirtualNetwork2* und *myVirtualNetwork3* erstellen möchten, müssen Sie ein zusätzliches Peering zwischen den virtuellen Netzwerken *myVirtualNetwork2* und *myVirtualNetwork3* erstellen. Auch wenn ein Peering zwischen *myVirtualNetwork1* und *myVirtualNetwork2* erstellt wurde, können Ressourcen in *myVirtualNetwork1* nur auf Ressourcen in *myVirtualNetwork3* zugreifen, wenn auch ein Peering zwischen *myVirtualNetwork1* und *myVirtualNetwork3* erstellt wurde. 

Vor der Erstellung von Peerings zwischen virtuellen Netzwerken wird empfohlen, sich gründlich mit der [Übersicht über das Peering](virtual-network-peering-overview.md), der [Verwaltung des Peerings](virtual-network-manage-peering.md) und den [Grenzwerten für virtuelle Netzwerke](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vertraut zu machen. In diesem Artikel wird zwar ein Peering zwischen zwei virtuellen Netzwerke im selben Abonnement und am selben Standort veranschaulicht, Sie können jedoch auch Peerings zwischen virtuellen Netzwerken in [unterschiedlichen Regionen](#register) und [verschiedenen Azure-Abonnements](create-peering-different-subscriptions.md#portal) erstellen. Darüber hinaus können Sie [Hub-Spoke-Netzwerkentwürfe](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) mit Peering erstellen.

## <a name="test-peering"></a>Testen des Peerings

Um die Netzwerkkommunikation zwischen virtuellen Computern in verschiedenen virtuellen Netzwerken durch das Peering zu testen, stellen Sie in jedem Subnetz einen virtuellen Computer bereit, und kommunizieren Sie dann zwischen den virtuellen Computern. 

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie einen virtuellen Computer in jedem virtuellen Netzwerk, damit Sie die private Kommunikation zwischen ihnen in einem späteren Schritt überprüfen können.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**. Sie können ein anderes Betriebssystem auswählen, bei den übrigen Schritten wird jedoch davon ausgegangen, dass Sie **Windows Server 2016 Datacenter** ausgewählt haben. 
3. Wählen Sie folgende Informationen für **Grundlagen** aus, oder geben Sie sie ein, und klicken Sie auf **OK**:
    - **Name**: *myVm1*
    - **Ressourcengruppe**: Wählen Sie **Vorhandene verwenden** und dann *myResourceGroup* aus.
    - **Standort**: Wählen Sie *USA, Osten* aus.

    Der eingegebene **Benutzername** und das **Kennwort** werden in einem späteren Schritt verwendet. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen. Der ausgewählte **Standort** und das ausgewählte **Abonnement** müssen mit dem Standort und dem Abonnement des virtuellen Netzwerks übereinstimmen. Sie müssen nicht dieselbe Ressourcengruppe auswählen, in der das virtuelle Netzwerk erstellt wurde, für diesen Artikel wird jedoch die gleiche Ressourcengruppe ausgewählt.
4. Wählen Sie unter **Größe auswählen** eine VM-Größe aus.
5. Wählen Sie folgende Informationen für **Einstellungen** aus, oder geben Sie sie ein, und klicken Sie auf **OK**:
    - **Virtuelles Netzwerk**: Stellen Sie sicher, dass **myVirtualNetwork1** ausgewählt ist. Falls dies nicht der Fall sein sollte, klicken Sie auf **Virtuelles Netzwerk**, und wählen Sie unter **Virtuelles Netzwerk wählen** das Netzwerk **myVirtualNetwork1** aus.
    - **Subnetz**: Stellen Sie sicher, dass **Subnet1** ausgewählt ist. Falls dies nicht der Fall sein sollte, klicken Sie auf **Subnetz**, und wählen Sie dann unter **Subnetz auswählen** das Subnetz **Subnet1**, wie in der folgenden Abbildung gezeigt:
    
        ![Einstellungen des virtuellen Computers](./media/tutorial-connect-virtual-networks-portal/virtual-machine-settings.png)
 
6. Wählen Sie auf der Seite **Zusammenfassung** unter **Erstellen** die Option **Erstellen**, um die Bereitstellung des virtuellen Computers zu starten.
7. Führen Sie die Schritte 1 bis 6 erneut aus, geben Sie jedoch als **Name** des virtuellen Computers *myVm2* ein, und wählen Sie für **Virtuelles Netzwerk** die Option *myVirtualNetwork2* aus.

Azure hat *10.0.0.4* als private IP-Adresse des virtuellen Computers *myVm1* sowie 10.1.0.4 für den virtuellen Computer *myVm2* zugewiesen, da es sich bei diesen um die ersten verfügbaren IP-Adressen in *Subnet1* der virtuellen Netzwerke *myVirtualNetwork1* bzw. *myVirtualNetwork2* handelte.

Das Erstellen des virtuellen Computers dauert einige Minuten. Fahren Sie nicht mit den übrigen Schritten fort, bis beide virtuellen Computer erstellt wurden.

### <a name="test-virtual-machine-communication"></a>Testen der Kommunikation zwischen virtuellen Computern

1. Beginnen Sie oben im Portal im Feld *Suchen* mit der Eingabe von *myVm1*. Wenn **myVm1** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Erstellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer *myVm1*, indem Sie auf **Verbinden** klicken, wie in der folgenden Abbildung gezeigt:

    ![Herstellen der Verbindung mit dem virtuellen Computer](./media/tutorial-connect-virtual-networks-portal/connect-to-virtual-machine.png)  

3. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit dem virtuellen Computer herzustellen. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.
4. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers angegeben haben. (Unter Umständen müssen Sie auf **More choices** (Weitere Optionen) und dann auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.) Klicken Sie dann auf **OK**.
5. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wählen Sie **Ja** aus, um mit dem Herstellen der Verbindung fortzufahren.
6. In einem späteren Schritt wird auf dem virtuellen Computer *myVm2* per Ping mit dem virtuellen Computer *myVmWeb* kommuniziert. Der Ping nutzt das ICMP, das von der Windows-Firewall standardmäßig verweigert wird. Aktivieren Sie das ICMP für die Windows-Firewall, indem Sie an einer Eingabeaufforderung den folgenden Befehl eingeben:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    In diesem Artikel wird für Testzwecke zwar ein Ping verwendet, das Zulassen des ICMP durch die Windows-Firewall wird jedoch nicht für Produktionsbereitstellungen empfohlen.

7. Um eine Verbindung mit dem virtuellen Computer *myVm2* herzustellen, geben Sie an einer Eingabeaufforderung auf dem virtuellen Computer *myVm1* den folgenden Befehl ein:

    ```
    mstsc /v:10.1.0.4
    ```
    
8. Da Sie den Ping für *myVm1* aktiviert haben, können Sie jetzt anhand der IP-Adresse pingen:

    ```
    ping 10.0.0.4
    ```
    
    Sie erhalten vier Antworten. Wenn Sie anhand des Namens des virtuellen Computers (*myVm1*) anstatt seiner IP-Adresse pingen, tritt beim Pingen ein Fehler auf, da *myVm1* ein unbekannter Hostname ist. Die Standardnamensauflösung von Azure funktioniert zwischen virtuellen Computern im selben virtuellen Netzwerk, jedoch nicht zwischen virtuellen Computern in verschiedenen virtuellen Netzwerken. Um Namen über verschiedene virtuelle Netzwerke hinweg aufzulösen, müssen Sie [einen eigenen DNS-Server bereitstellen](virtual-networks-name-resolution-for-vms-and-role-instances.md) oder [private Azure DNS-Domänen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) verwenden.

9. Trennen Sie Ihre RDP-Sitzungen für *myVm1* und *myVm2*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit allen ihren Ressourcen, wenn Sie sie nicht mehr benötigen: 

1. Geben Sie im oben im Portal im Feld *Suche* die Zeichenfolge **myResourceGroup** ein. Wenn **myResourceGroup** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie für **Geben Sie den Ressourcengruppennamen ein:** den Namen *myResourceGroup* ein, und klicken Sie auf **Löschen**.

**<a name="register"></a>Registrieren für die globale Vorschauversion für das Peering für virtuelle Netzwerke**

Das Peering von virtuellen Netzwerken in derselben Region befindet sich in der Phase der allgemeinen Verfügbarkeit. Das Peering von virtuellen Netzwerken in unterschiedlichen Regionen befindet sich derzeit in der Vorschauphase. Verfügbare Regionen finden Sie unter [Updates für virtuelle Netzwerke](https://azure.microsoft.com/updates/?product=virtual-network). Um ein Peering für virtuelle Netzwerke in verschiedenen Regionen zu erstellen, müssen Sie sich zuerst für die Vorschauversion registrieren. Sie können sich nicht mithilfe des Portals registrieren, jedoch mit [PowerShell](tutorial-connect-virtual-networks-powershell.md#register) oder der [Azure CLI](tutorial-connect-virtual-networks-cli.md#register). Wenn Sie versuchen ein Peering für virtuelle Netzwerke in verschiedenen Regionen zu erstellen, bevor Sie sich für diese Funktion registrieren, tritt bei der Erstellung des Peerings ein Fehler auf.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie zwischen zwei Netzwerken durch das Peering für virtuelle Netzwerke eine Verbindung hergestellt wird.

Stellen Sie als Nächstes über ein VPN eine Verbindung zwischen Ihrem eigenen Computer und einem virtuellen Netzwerk her, und interagieren Sie mit Ressourcen in einem virtuellen Netzwerk oder in durch Peerings verbundenen virtuellen Netzwerken.

> [!div class="nextstepaction"]
> [Verbinden eines Computers mit einem virtuellen Netzwerk](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
