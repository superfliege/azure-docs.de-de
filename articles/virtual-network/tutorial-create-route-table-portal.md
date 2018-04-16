---
title: Weiterleiten von Netzwerkdatenverkehr – Tutorial – Azure-Portal | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie unter Verwendung des Azure-Portals Netzwerkdatenverkehr mit einer Routingtabelle weiterleiten.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 7254e9336fca14daee2021d5bde4c5538509fe35
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="tutorial-route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Tutorial: Weiterleiten von Netzwerkdatenverkehr unter Verwendung des Azure-Portals

Standardmäßig leitet Azure automatisch den Datenverkehr aller Subnetze des virtuellen Netzwerks weiter. Sie können eigene Routen erstellen, um das Azure-Standardrouting außer Kraft zu setzen. Die Möglichkeit zum Erstellen von benutzerdefinierten Routen ist beispielsweise hilfreich, wenn Sie über ein virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) Datenverkehr zwischen Subnetzen weiterleiten möchten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Routingtabelle
> * Erstellen einer Route
> * Erstellen eines virtuellen Netzwerks mit mehreren Subnetzen
> * Zuordnen einer Routingtabelle zu einem Subnetz
> * Erstellen eines virtuellen Netzwerkgeräts, das Datenverkehr weiterleitet
> * Bereitstellen von VMs in unterschiedlichen Subnetzen
> * Weiterleiten von Datenverkehr aus einem Subnetz zu einem anderen über ein virtuelles Netzwerkgerät

Dieser Artikel kann auch mit der [Azure-Befehlszeilenschnittstelle](tutorial-create-route-table-cli.md) oder mit [Azure PowerShell](tutorial-create-route-table-powershell.md) durchgearbeitet werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.

## <a name="create-a-route-table"></a>Erstellen einer Routingtabelle

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie die Option **Netzwerk** und dann **Routingtabelle**.
3. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Erstellen**:

    |Einstellung|Wert|
    |---|---|
    |NAME|myRouteTablePublic|
    |Abonnement| Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe | Klicken Sie auf **Neu erstellen**, und geben Sie *myResourceGroup* ein.|
    |Speicherort|USA (Ost)|
 
    ![Erstellen einer Routingtabelle](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Erstellen einer Route

1. Beginnen Sie oben im Portal im Feld *Ressourcen, Dienste und Dokumente durchsuchen* mit der Eingabe von *myRouteTablePublic*. Wenn **myRouteTablePublic** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Klicken Sie unter **EINSTELLUNGEN** auf **Routen** und wählen Sie dann **+ Hinzufügen** aus, wie in der folgenden Abbildung gezeigt wird:

    ![Hinzufügen einer Route](./media/tutorial-create-route-table-portal/add-route.png) 
 
3. Geben Sie unter **Route hinzufügen** die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Erstellen**:

    |Einstellung|Wert|
    |---|---|
    |Routenname|ToPrivateSubnet|
    |Adresspräfix| 10.0.1.0/24|
    |Typ des nächsten Hops | Wählen Sie **Virtuelles Gerät** aus.|
    |Adresse des nächsten Hops| 10.0.2.4|

## <a name="associate-a-route-table-to-a-subnet"></a>Zuordnen einer Routingtabelle zu einem Subnetz

Bevor Sie eine Routingtabelle einem Subnetz zuordnen können, müssen Sie ein virtuelles Netzwerk und ein Subnetz erstellen. Danach können Sie die Routingtabelle einem Subnetz zuordnen.

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie **Netzwerk** und anschließend **Virtuelles Netzwerk** aus.
3. Geben Sie unter **Virtuelles Netzwerk erstellen** die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Erstellen**:

    |Einstellung|Wert|
    |---|---|
    |NAME|myVirtualNetwork|
    |Adressraum| 10.0.0.0/16|
    |Abonnement | Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe|Wählen Sie **Vorhandene verwenden** und dann **myResourceGroup**.|
    |Speicherort|Wählen Sie *USA, Osten* aus.|
    |Subnetzname|Öffentlich|
    |Adressbereich|10.0.0.0/24|
    
4. Beginnen Sie oben im Portal im Feld **Ressourcen, Dienste und Dokumente durchsuchen** mit der Eingabe von *myVirtualNetwork*. Wenn **myVirtualNetwork** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
5. Klicken Sie unter **EINSTELLUNGEN** auf **Subnetze** und anschließend auf **+ Subnetz**, wie in der folgenden Abbildung gezeigt:

    ![Hinzufügen des Subnetzes](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Wählen Sie folgende Informationen aus, oder geben Sie sie ein, und klicken Sie auf **OK**:

    |Einstellung|Wert|
    |---|---|
    |NAME|Private|
    |Adressraum| 10.0.1.0/24|

7. Führen Sie erneut die Schritte 5 und 6 durch, und geben Sie dabei folgende Informationen an:

    |Einstellung|Wert|
    |---|---|
    |NAME|DMZ|
    |Adressraum| 10.0.2.0/24|

8. Das Feld **myVirtualNetwork – Subnetze** wird nach Durchführung des vorhergehenden Schritts angezeigt. Klicken Sie unter **EINSTELLUNGEN** auf **Subnetze**, und wählen Sie dann **Öffentlich** aus.
9. Wählen Sie wie in der folgenden Abbildung dargestellt **Routingtabelle** und dann **MyRouteTablePublic** aus, und klicken Sie anschließend auf **Speichern**.

    ![Zuordnen einer Routingtabelle](./media/tutorial-create-route-table-portal/associate-route-table.png) 

## <a name="create-an-nva"></a>Erstellen eines virtuellen Netzwerkgeräts

Ein virtuelles Netzwerkgerät ist eine VM, die eine Netzwerkfunktion wie Routing, Firewall oder WAN-Optimierung ausführt.

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**. Sie können ein anderes Betriebssystem auswählen, bei den übrigen Schritten wird jedoch davon ausgegangen, dass Sie **Windows Server 2016 Datacenter** ausgewählt haben. 
3. Wählen Sie folgende Informationen für **Grundlagen** aus, oder geben Sie sie ein, und klicken Sie auf **OK**:

    |Einstellung|Wert|
    |---|---|
    |NAME|myVmNva|
    |Benutzername|Geben Sie den gewünschten Benutzernamen ein.|
    |Password|Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    |Ressourcengruppe| Wählen Sie **Vorhandene verwenden** und dann *myResourceGroup*.|
    |Speicherort|Wählen Sie **USA, Osten** aus.|
4. Wählen Sie unter **Größe auswählen** eine VM-Größe aus.
5. Wählen Sie folgende Informationen für **Einstellungen** aus, oder geben Sie sie ein, und klicken Sie auf **OK**:

    |Einstellung|Wert|
    |---|---|
    |Virtuelles Netzwerk|myVirtualNetwork. Falls dieses Netzwerk nicht ausgewählt ist, klicken Sie auf **Virtuelles Netzwerk**, und wählen Sie dann unter **Virtuelles Netzwerk auswählen** den Eintrag **myVirtualNetwork** aus.|
    |Subnetz|Klicken Sie auf **Subnetz**, und wählen Sie dann unter **Subnetz auswählen** die Option **DMZ** aus. |
    |Öffentliche IP-Adresse| Klicken Sie auf **Öffentliche IP-Adresse**, und wählen Sie dann unter **Öffentliche IP-Adresse wählen** die Option **Keine** aus. Diesem virtuellen Computer wird keine öffentliche IP-Adresse zugewiesen, da mit ihm keine Verbindung über das Internet hergestellt wird.
6. Klicken Sie auf der Seite **Zusammenfassung** unter **Erstellen** auf **Erstellen**, um die Bereitstellung der VM zu starten.

    Die Erstellung des virtuellen Computers dauert einige Minuten. Fahren Sie erst mit dem nächsten Schritt fort, wenn Azure die VM erstellt und ein Feld mit Informationen zur VM geöffnet hat.

7. Klicken Sie wie in der folgenden Abbildung dargestellt in dem Feld, das nach der Erstellung für die VM geöffnet wird, unter **EINSTELLUNGEN** auf **Netzwerk**, und wählen Sie dann **myvmnva158** aus (die von Azure erstellte Netzwerkschnittstelle für Ihre VM weist eine andere Nummer nach **myvmnva** auf):

    ![VM-Netzwerk](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

8. Damit Netzwerkdatenverkehr, der nicht für seine eigene IP-Adresse bestimmt ist, an eine Netzwerkschnittstelle gesendet werden kann, muss die IP-Weiterleitung für die Netzwerkschnittstelle aktiviert sein. Klicken Sie unter **EINSTELLUNGEN** auf **IP-Konfigurationen**, auf **Aktiviert** für die Option **IP-Weiterleitung** und abschließend auf **Speichern** . Dies wird in der folgenden Abbildung veranschaulicht:

    ![Aktivieren der IP-Weiterleitung](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei VMs im virtuellen Netzwerk, damit Sie in einem späteren Schritt überprüfen können, ob Datenverkehr aus dem Subnetz *Öffentlich* über das virtuelle Netzwerkgerät an das Subnetz *Privat* weitergeleitet wird. Führen Sie die Schritte 1 bis 6 unter [Erstellen eines virtuellen Netzwerkgeräts](#create-a-network-virtual-appliance) aus. Verwenden Sie die gleichen Einstellungen wie in Schritt 3 und 5 mit Ausnahme der folgenden Änderungen:

|Name des virtuellen Computers      |Subnetz      | Öffentliche IP-Adresse     |
|--------- | -----------|---------              |
| myVmPublic  | Öffentlich     | Standardeinstellung des Portals beibehalten |
| myVmPrivate | Private    | Standardeinstellung des Portals beibehalten |

Sie können die VM *myVmPrivate* erstellen, während Azure die VM *myVmPublic* erstellt. Fahren Sie erst mit den nächsten Schritten fort, wenn Azure die Erstellung beider VMs abgeschlossen hat.

## <a name="route-traffic-through-an-nva"></a>Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät

1. Beginnen Sie oben im Portal im Feld *Suchen* mit der Eingabe des Namens *myVmPrivate*. Wenn die VM **myVmPrivate** in den Suchergebnissen angezeigt wird, wählen Sie sie aus.
2. Erstellen Sie wie in der folgenden Abbildung dargestellt eine Remotedesktopverbindung mit der VM *myVmPrivate*, indem Sie auf **Verbinden** klicken:

    ![Herstellen einer Verbindung mit dem virtuellen Computer ](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit dem virtuellen Computer herzustellen. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.
4. Geben Sie den Benutzernamen und das Kennwort ein, wie beim Erstellen des virtuellen Computers angegeben. (Unter Umständen müssen Sie auf **Weitere Optionen** und dann auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.) Klicken Sie anschließend auf **OK**.
5. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wählen Sie **Ja** aus, um mit dem Herstellen der Verbindung fortzufahren.
6. In einem späteren Schritt wird das Routing mithilfe des Traceroute-Tools getestet. Traceroute verwendet das Internet Control Message-Protokoll (ICMP). Dieses Protokoll wird durch die Windows-Firewall blockiert. Geben Sie auf dem virtuellen Computer *myVmPrivate* in PowerShell den folgenden Befehl ein, um ICMP in der Windows-Firewall zuzulassen:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    In diesem Tutorial wird Traceroute verwendet, um das Routing zu testen. Das Zulassen von ICMP-Datenverkehr durch die Windows-Firewall wird für Produktionsbereitstellungen jedoch nicht empfohlen.
7. Sie haben die IP-Weiterleitung innerhalb von Azure für die Netzwerkschnittstelle der VM unter [Aktivieren der IP-Weiterleitung](#enable-ip-forwarding) aktiviert. Das Betriebssystem der VM oder eine Anwendung, die auf der VM ausgeführt wird, muss ebenfalls Netzwerkdatenverkehr weiterleiten können. Aktivieren Sie im Betriebssystem des virtuellen Computers *myVmNva* die IP-Weiterleitung:

    Stellen Sie über eine Eingabeaufforderung auf dem virtuellen Computer *myVmPrivate* eine Remotedesktopverbindung mit dem virtuellen Computer *myVmNva* her:

    ``` 
    mstsc /v:myvmnva
    ```
    
    Geben Sie auf dem virtuellen Computer *myVmNva* in PowerShell den folgenden Befehl ein, um im Betriebssystem die IP-Weiterleitung zu aktivieren:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Starten Sie den virtuellen Computer *myVmNva* neu. Dadurch wird auch die Remotedesktopsitzung getrennt.
8. Erstellen Sie nach dem Neustart des virtuellen Computers *myVmNva* bei bestehender Verbindung mit dem virtuellen Computer *myVmPrivate* eine Remotedesktopsitzung mit dem virtuellen Computer *myVmPublic*:

    ``` 
    mstsc /v:myVmPublic
    ```
    
    Geben Sie auf dem virtuellen Computer *myVmPublic* in PowerShell den folgenden Befehl ein, um ICMP in der Windows-Firewall zuzulassen:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

9. Geben Sie auf dem virtuellen Computer *myVmPublic* in PowerShell den folgenden Befehl ein, um die Weiterleitung von Netzwerkdatenverkehr des virtuellen Computers *myVmPublic* an den virtuellen Computer *myVmPrivate* zu testen:

    ```
    tracert myVmPrivate
    ```

    Die Antwort ähnelt dem folgenden Beispiel:
    
    ```
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:
        
    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4
        
    Trace complete.
    ```
      
    Wie Sie sehen können, ist der erste Hop 10.0.2.4, die private IP-Adresse des virtuellen Netzwerksgeräts. Der zweite Hop ist 10.0.1.4, die private IP-Adresse der VM *myVmPrivate*. Die Route, die der Routingtabelle *myRouteTablePublic* hinzugefügt und dem Subnetz *Public* zugeordnet wurde, hat bewirkt, dass Azure Datenverkehr über das virtuelle Netzwerkgerät weiterleitet und nicht direkt an das Subnetz *Private*.
10.  Schließen Sie die Remotedesktopsitzung mit der VM *myVmPublic*, sodass Sie nur noch mit der VM *myVmPrivate* verbunden sind.
11. Geben Sie auf dem virtuellen Computer *myVmPrivate* an einer Eingabeaufforderung den folgenden Befehl ein, um die Weiterleitung von Netzwerkdatenverkehr des virtuellen Computers *myVmPrivate* an den virtuellen Computer *myVmPublic* zu testen:

    ```
    tracert myVmPublic
    ```

    Die Antwort ähnelt dem folgenden Beispiel:

    ```
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:
    
    1     1 ms     1 ms     1 ms  10.0.0.4
    
    Trace complete.
    ```

    Wie Sie sehen, wird Datenverkehr direkt von der VM *myVmPrivate* zur VM *myVmPublic* weitergeleitet. Azure-Routen leiten standardmäßig Datenverkehr direkt durch Subnetze weiter.
12. Schließen Sie die Remotedesktopsitzung mit der VM *myVmPrivate*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit allen ihren Ressourcen, wenn Sie sie nicht mehr benötigen: 

1. Geben Sie im oben im Portal im Feld *Suche* die Zeichenfolge **myResourceGroup** ein. Wenn **myResourceGroup** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie für **Geben Sie den Ressourcengruppennamen ein:** den Namen *myResourceGroup* ein, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Routingtabelle erstellt und einem Subnetz zugeordnet. Sie haben ein einfaches virtuelles Netzwerkgerät erstellt, das Datenverkehr von einem öffentlichen Subnetz an ein privates Subnetz weiterleitet. Über den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking) können Sie eine Vielzahl von vorkonfigurierten virtuellen Netzwerkgeräten bereitstellen, die Netzwerkfunktionen wie Firewall und WAN-Optimierung ausführen. Weitere Informationen zum Routing finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md) und [Erstellen, Ändern oder Löschen einer Routingtabelle](manage-route-table.md).


Sie können zwar in einem virtuellen Netzwerk viele Azure-Ressourcen bereitstellen, für einige Azure-PaaS-Dienste können jedoch keine Ressourcen in einem virtuellen Netzwerk bereitgestellt werden. Allerdings können Sie den Zugriff auf die Ressourcen einiger Azure-PaaS-Dienste nach wie vor auf den Datenverkehr nur eines Subnetzes eines virtuellen Netzwerks beschränken. Im nächsten Tutorial erfahren Sie, wie Sie den Netzwerkzugriff auf Azure-PaaS-Ressourcen einschränken.

> [!div class="nextstepaction"]
> [Beschränken des Netzwerkzugriffs auf PaaS-Ressourcen](tutorial-restrict-network-access-to-resources.md)
