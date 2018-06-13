---
title: Erstellen eines virtuellen Azure-Netzwerks mit mehreren Subnetzen – Portal | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe des Azure-Portals ein virtuelles Netzwerk mit mehreren Subnetzen erstellen.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 898bdef779282d7312c76696f744b97ec2dfcded
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2018
ms.locfileid: "29880480"
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-the-azure-portal"></a>Erstellen eines virtuellen Netzwerks mit mehreren Subnetzen mithilfe des Azure-Portals

In einem virtuellen Netzwerk können mehrere Arten von Azure-Ressourcen mit dem Internet und privat miteinander kommunizieren. Durch die Erstellung mehrerer Subnetze in einem virtuellen Netzwerk können Sie Ihr Netzwerk segmentieren, sodass Sie den Datenverkehr zwischen Subnetzen filtern oder steuern können. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines Subnetzes
> * Testen der Netzwerkkommunikation zwischen virtuellen Computern

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie **Netzwerk** und anschließend **Virtuelles Netzwerk** aus.
3. Geben Sie wie in der folgenden Abbildung gezeigt *myVirtualNetwork* für **Name**, *10.0.0.0/16* für **Adressraum**, **myResourceGroup** für **Ressourcengruppe**, *Public* für **Name** des Subnetzes und 10.0.0.0/24 für den **Adressbereich** des Subnetzes ein. Wählen Sie einen **Standort** und Ihr **Abonnement** aus, übernehmen Sie die übrigen Standardeinstellungen, und klicken Sie dann auf **Erstellen**:

    ![Erstellen eines virtuellen Netzwerks](./media/virtual-networks-create-vnet-arm-pportal/create-virtual-network.png)

    Der **Adressraum** und der **Adressbereich** werden in CIDR-Notation angegeben. Der angegebene **Adressbereich** enthält die IP-Adressen 10.0.0.0 bis 10.0.255.254. Der für das Subnetz angegebene **Adressbereich** muss innerhalb des für das virtuelle Netzwerk angegebenen **Adressraums** liegen. Azure DHCP weist in einem Subnetz bereitgestellten Ressourcen IP-Adressen aus einem Subnetzadressbereich zu. Azure weist die Adressen 10.0.0.4 bis 10.0.0.254 nur Ressourcen zu, die im Subnetz **Öffentlich** bereitgestellt werden, da Azure die ersten vier Adressen (in diesem Beispiel 10.0.0.0 bis 10.0.0.3 für das Subnetz) und die letzte Adresse (in diesem Beispiel 10.0.0.255 für das Subnetz) in jedem Subnetz reserviert.

## <a name="create-a-subnet"></a>Erstellen eines Subnetzes

1. Beginnen Sie oben im Portal im Feld **Ressourcen, Dienste und Dokumente durchsuchen** mit der Eingabe von *myVirtualNetwork*. Wenn **myVirtualNetwork** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
2. Klicken Sie auf **Subnetze** und anschließend auf **+ Subnetz**, wie in der folgenden Abbildung gezeigt:

     ![Hinzufügen eines Subnetzes](./media/virtual-networks-create-vnet-arm-pportal/add-subnet.png)
     
3. Geben Sie im daraufhin angezeigten Feld **Subnetz hinzufügen** für **Name** den Namen *Privat* und für **Adressbereich** den Adressbereich *10.0.1.0/24* ein, und klicken Sie dann auf **OK**.  Ein Subnetzadressbereich darf sich nicht mit den Adressbereichen anderer Adressbereiche innerhalb eines virtuellen Netzwerks überschneiden. 

Es wird empfohlen, dass Sie sich vor der Bereitstellung von virtuellen Azure-Netzwerken und Subnetzen für die Produktion eingehend mit den [Überlegungen](manage-virtual-network.md#create-a-virtual-network) zu Adressbereichen und den [Netzwerkgrenzwerten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vertraut machen. Nach der Bereitstellung von Ressourcen in Subnetzen müssen bei bestimmten Änderungen an virtuellen Netzwerken und Subnetzen (etwa bei Änderung der Adressbereiche) unter Umständen in Subnetzen bereitgestellte vorhandene Azure-Ressourcen erneut bereitgestellt werden.

## <a name="test-network-communication"></a>Testen der Netzwerkkommunikation

In einem virtuellen Netzwerk können mehrere Arten von Azure-Ressourcen mit dem Internet und privat miteinander kommunizieren. Eine Art von Ressource, die Sie in einem virtuellen Netzwerk bereitstellen können, ist ein virtueller Computer. Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk, damit Sie die Netzwerkkommunikation zwischen ihnen und dem Internet in einem späteren Schritt überprüfen können.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**. Sie können ein anderes Betriebssystem auswählen, bei den übrigen Schritten wird jedoch davon ausgegangen, dass Sie **Windows Server 2016 Datacenter** ausgewählt haben. 
3. Wählen Sie folgende Informationen für **Grundlagen** aus, oder geben Sie sie ein, und klicken Sie auf **OK**:
    - **Name**: *myVmWeb*
    - **Ressourcengruppe**: Wählen Sie **Vorhandene verwenden** und dann *myResourceGroup* aus.
    - **Standort**: Wählen Sie *USA, Osten* aus.

    Der eingegebene **Benutzername** und das **Kennwort** werden in einem späteren Schritt verwendet. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen. Der ausgewählte **Standort** und das ausgewählte **Abonnement** müssen mit dem Standort und dem Abonnement des virtuellen Netzwerks übereinstimmen. Sie müssen nicht dieselbe Ressourcengruppe auswählen, in der das virtuelle Netzwerk erstellt wurde, für dieses Tutorial wird jedoch die gleiche Ressourcengruppe ausgewählt.
4. Wählen Sie unter **Größe auswählen** eine VM-Größe aus.
5. Wählen Sie folgende Informationen für **Einstellungen** aus, oder geben Sie sie ein, und klicken Sie auf **OK**:
    - **Virtuelles Netzwerk**: Stellen Sie sicher, dass **myVirtualNetwork** ausgewählt ist. Falls nicht, klicken Sie auf **Virtuelles Netzwerk**, und wählen Sie unter **Virtuelles Netzwerk auswählen** das Netzwerk **myVirtualNetwork** aus.
    - **Subnetz**: Stellen Sie sicher, dass **Öffentlich** ausgewählt ist. Falls nicht, klicken Sie auf **Subnetz**, und wählen Sie dann unter **Subnetz auswählen** das Subnetz **Öffentlich**, wie in der folgenden Abbildung gezeigt:
    
        ![Einstellungen des virtuellen Computers](./media/virtual-networks-create-vnet-arm-pportal/virtual-machine-settings.png)
 
6. Wählen Sie auf der Seite **Zusammenfassung** unter **Erstellen** die Option **Erstellen**, um die Bereitstellung des virtuellen Computers zu starten.
7. Führen Sie die Schritte 1 bis 6 erneut aus, geben Sie jedoch als **Name** des virtuellen Computers *myVmMgmt* ein, und wählen Sie für **Subnetz** die Option **Privat** aus.

Das Erstellen des virtuellen Computers dauert einige Minuten. Fahren Sie nicht mit den übrigen Schritten fort, bis beide virtuellen Computer erstellt wurden.

Die in diesem Artikel erstellten virtuellen Computer verfügen über eine [Netzwerkschnittstelle](virtual-network-network-interface.md) mit einer IP-Adresse, die der Netzwerkschnittstelle dynamisch zugewiesen wird. Nach dem Bereitstellen des virtuellen Computers können Sie [mehrere öffentliche und private IP-Adressen hinzufügen oder für die IP-Adresszuweisung die statische Methode festlegen](virtual-network-network-interface-addresses.md#add-ip-addresses). Sie können [Netzwerkschnittstellen hinzufügen](virtual-network-network-interface-vm.md#vm-add-nic). Dabei gilt in Bezug auf die Anzahl der Grenzwert, der von der [VM-Größe](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) unterstützt wird, die Sie beim Erstellen eines virtuellen Computers auswählen. Sie können für einen virtuellen Computer auch [E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) aktivieren](create-vm-accelerated-networking-powershell.md). Dies ist jedoch nur möglich, wenn Sie einen virtuellen Computer mit einer VM-Größe erstellen, die diese Funktion unterstützt.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Kommunikation zwischen virtuellen Computern und dem Internet

1. Beginnen Sie oben im Portal im Feld *Suche* mit der Eingabe von *myVmMgmt*. Wenn **myVmMgmt** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Erstellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer *myVmMgmt*, indem Sie auf **Verbinden** klicken, wie in der folgenden Abbildung gezeigt:

    ![Herstellen der Verbindung mit dem virtuellen Computer](./media/virtual-networks-create-vnet-arm-pportal/connect-to-virtual-machine.png)  

3. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit dem virtuellen Computer herzustellen. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.
4. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers angegeben haben. (Unter Umständen müssen Sie auf **More choices** (Weitere Optionen) und dann auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.) Klicken Sie dann auf **OK**.
5. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wählen Sie **Ja** aus, um mit dem Herstellen der Verbindung fortzufahren.
6. In einem späteren Schritt wird auf dem virtuellen Computer *myVmWeb* per Ping mit dem virtuellen Computer *myVmMgmt* kommuniziert. Der Ping nutzt das ICMP, das von der Windows-Firewall standardmäßig verweigert wird. Aktivieren Sie das ICMP für die Windows-Firewall, indem Sie an einer Eingabeaufforderung den folgenden Befehl eingeben:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    In diesem Artikel wird zwar ein Ping verwendet, das Zulassen des ICMP durch die Windows-Firewall wird jedoch nicht für Produktionsbereitstellungen empfohlen.
7. Aus Sicherheitsgründen wird die Anzahl virtueller Computer, mit denen in einem virtuellen Netzwerk eine Verbindung hergestellt werden kann, häufig begrenzt. In diesem Tutorial wird der virtuelle Computer *myVmMgmt* zur Verwaltung des virtuellen Computers *myVmWeb* im virtuellen Netzwerk verwendet. Geben Sie an einer Eingabeaufforderung den folgenden Befehl ein, um über den virtuellen Computer *myVmMgmt* eine Remotedesktopverbindung mit dem virtuellen Computer *myVmWeb* herzustellen:

    ``` 
    mstsc /v:myVmWeb
    ```
8. Geben Sie an einer Eingabeaufforderung den folgenden Befehl ein, um über den virtuellen Computer *myVmWeb* mit dem virtuellen Computer *myVmMgmt* zu kommunizieren:

    ```
    ping myvmmgmt
    ```

    Eine Ausgabe ähnlich folgendem Beispiel wird angezeigt:
    
    ```
    Pinging myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net [10.0.1.4] with 32 bytes of data:
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    
    Ping statistics for 10.0.1.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 0ms, Average = 0ms
    ```
      
    Sie sehen, dass die Adresse des virtuellen Computers *myVmMgmt* 10.0.1.4 lautet. 10.0.1.4 war die erste verfügbare IP-Adresse im Adressbereich des Subnetzes *Privat*, in dem Sie in einem vorherigen Schritt den virtuellen Computer *myVmMgmt* bereitgestellt haben.  Der vollqualifizierte Domänenname des virtuellen Computers lautet *myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net*. Der Teil *dar5p44cif3ulfq00wxznl3i3f* des Domänennamens weicht für Ihren virtuellen Computer zwar ab, die übrigen Teil sind jedoch identisch. 

    Standardmäßig verwenden alle virtuellen Azure-Computer den Azure DNS-Standarddienst. Alle virtuellen Computer in einem virtuellen Netzwerk können die Namen aller anderen virtuellen Computer im gleichen virtuellen Netzwerk mithilfe des DNS-Standarddiensts von Azure auflösen. Anstelle des DNS-Standarddiensts von Azure können Sie auch Ihren eigenen DNS-Server oder die Funktion für private Domänen des Azure DNS-Diensts verwenden. Ausführliche Informationen finden Sie unter [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) oder [Verwenden von Azure DNS für private Domänen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

9. Wenn Sie Internetinformationsdienste (Internet Information Services, IIS) für Windows Server auf dem virtuellen Computer *myVmWeb* installieren möchten, geben Sie in einer PowerShell-Sitzung den folgenden Befehl ein:

    ```powershell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    ```

10. Trennen Sie nach der Installation von IIS die Remotedesktopsitzung mit *myVmWeb*. Dadurch befinden Sie sich in der *myVmMgmt*-Remotedesktopsitzung. Öffnen Sie einen Webbrowser, und gehen Sie zu http://myvmweb. Die IIS-Willkommensseite wird angezeigt.
11. Trennen Sie die *myVmMgmt*-Remotedesktopsitzung.
12. Ermitteln Sie die öffentliche IP-Adresse des virtuellen Computers *myVmWeb*. Bei der Erstellung des virtuellen Computers *myVmWeb* durch Azure wurde auch eine öffentliche IP-Adressressource namens *myVmWeb* erstellt und dem virtuellen Computer zugewiesen. In der Abbildung in Schritt 2 sehen Sie, dass dem virtuellen Computer *myVmMgmt* unter **Öffentliche IP-Adresse** die Adresse 52.170.5.92 zugewiesen wurde. Um die dem virtuellen Computer *myVmWeb* zugewiesene öffentliche IP-Adresse zu ermitteln, suchen Sie über das Suchfeld des Portals nach *myVmWeb*, und wählen Sie den virtuellen Computer in den Suchergebnissen aus.

    Einem virtuellen Computer muss zwar keine öffentliche IP-Adresse zugewiesen werden, Azure weist aber dennoch standardmäßig jedem von Ihnen erstellten virtuellen Computer eine öffentliche IP-Adresse zu. Für die Kommunikation zwischen Internet und virtuellem Computer muss einem virtuellen Computer eine öffentliche IP-Adresse zugewiesen werden. Alle virtuellen Computer können in ausgehender Richtung mit dem Internet kommunizieren, unabhängig davon, ob dem virtuellen Computer eine öffentliche IP-Adresse zugewiesen ist. Weitere Informationen zu ausgehenden Internetverbindungen finden Sie unter [Ausgehende Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
13. Navigieren Sie auf Ihrem eigenen Computer zur öffentlichen IP-Adresse des virtuellen Computers *myVmWeb*. Beim Versuch, die IIS-Willkommensseite auf Ihrem eigenen Computer anzuzeigen, tritt ein Fehler auf. Der Fehler tritt auf, da beim Bereitstellen der virtuellen Computer von Azure standardmäßig eine Netzwerksicherheitsgruppe für jeden virtuellen Computer erstellt wurde. 

     Eine Netzwerksicherheitsgruppe enthält Sicherheitsregeln, die eingehenden und ausgehenden Netzwerkdatenverkehr nach Port und IP-Adresse zulassen oder verweigern. Die von Azure erstellte Standard-Netzwerksicherheitsgruppe lässt über alle Ports die Kommunikation zwischen Ressourcen im gleichen virtuellen Netzwerk zu. Bei virtuellen Windows-Computern verweigert die Standard-Netzwerksicherheitsgruppe den gesamten eingehenden Datenverkehr aus dem Internet an allen Ports mit Ausnahme von TCP-Port 3389 (RDP). Daher können Sie standardmäßig auch eine RDP-Verbindung direkt mit dem virtuellen Computer *myVmWeb* über das Internet herstellen, auch wenn Port 3389 für einen Webserver nicht offen sein sollte. Da beim Surfen im Internet Port 80 verwendet wird, tritt bei der Kommunikation vom Internet ein Fehler auf, da die Standard-Netzwerksicherheitsgruppe keine Regel enthält, die Datenverkehr über Port 80 zulässt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit allen ihren Ressourcen, wenn Sie sie nicht mehr benötigen: 

1. Geben Sie im oben im Portal im Feld *Suche* die Zeichenfolge **myResourceGroup** ein. Wenn **myResourceGroup** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie für **Geben Sie den Ressourcengruppennamen ein:** den Namen *myResourceGroup* ein, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie einen virtuellen Computer mit mehreren Subnetzen bereitstellen. Darüber hinaus haben Sie gelernt, dass beim Erstellen eines virtuellen Windows-Computers von Azure eine Netzwerkschnittstelle, die an den virtuellen Computer angefügt wird, sowie eine Netzwerksicherheitsgruppe erstellt wird, die Datenverkehr aus dem Internet nur über Port 3389 zulässt. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Netzwerkdatenverkehr nicht für einzelne virtuelle Computer, sondern für Subnetze filtern.

> [!div class="nextstepaction"]
> [Erstellen von Netzwerksicherheitsgruppen mit der Azure CLI](./virtual-networks-create-nsg-arm-pportal.md)
