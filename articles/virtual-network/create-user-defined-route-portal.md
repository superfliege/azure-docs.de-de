---
title: "Erstellen einer benutzerdefinierten Route zum Weiterleiten des Netzwerkdatenverkehrs über ein virtuelles Netzwerkgerät – Azure-Portal | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine benutzerdefinierte Route erstellen, mit der Sie den Netzwerkdatenverkehr über ein virtuelles Netzwerkgerät weiterleiten und so das Azure-Standardrouting außer Kraft setzen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2017
ms.author: jdial
ms.openlocfilehash: 0319029277091611673f15c94604604850cbfcbe
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="create-a-user-defined-route---azure-portal"></a>Erstellen einer benutzerdefinierten Route – Azure-Portal

In diesem Tutorial erfahren Sie, wie Sie eine benutzerdefinierte Route erstellen, mit der Sie Netzwerkdatenverkehr über ein virtuelles Netzwerkgerät von einem Subnetz an ein anderes Subnetz weiterleiten, die beide Teil eines [virtuellen Netzwerks](virtual-networks-overview.md) sind. Ein virtuelles Netzwerkgerät ist eine VM, auf der eine Netzwerkanwendung wie z.B. eine Firewall ausgeführt wird. Weitere Informationen zu vorkonfigurierten virtuellen Netzwerkgeräten, die Sie in einem virtuellen Azure-Netzwerk bereitstellen können, finden Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

Wenn Sie Subnetze in einem virtuellen Netzwerk erstellen, erstellt Azure [Standardsystemrouten](virtual-networks-udr-overview.md#system-routes), durch die Ressourcen in allen Subnetzen wie in der folgenden Abbildung gezeigt miteinander kommunizieren können:

![Standardrouten](./media/create-user-defined-route/default-routes.png)

In diesem Tutorial erstellen Sie wie in der folgenden Abbildung gezeigt ein virtuelles Netzwerk mit einem öffentlichen und privaten Subnetz sowie einem DMZ-Subnetz. In der Regel werden Webserver in einem öffentlichen Subnetz und Anwendungen oder Datenbankserver in einem privaten Subnetz bereitgestellt. Sie erstellen in diesem Tutorial eine VM, die als virtuelles Netzwerkgerät innerhalb des DMZ-Subnetzes verwendet wird. Optional können Sie außerdem in jedem Subnetz eine VM erstellen, die über das virtuelle Netzwerkgerät mit einer anderen VM kommuniziert. Der gesamte Netzwerkdatenverkehr wird wie in der folgenden Abbildung gezeigt über das virtuelle Netzwerkgerät vom öffentlichen an das private Subnetz weitergeleitet:

![Benutzerdefinierte Routen](./media/create-user-defined-route/user-defined-routes.png)

In diesem Artikel wird das Erstellen einer benutzerdefinierten Route über das Azure Resource Manager-Bereitstellungsmodell beschrieben. Dieses Bereitstellungsmodell wird für das Erstellen benutzerdefinierter Routen empfohlen. Wenn Sie stattdessen eine benutzerdefinierte Route mithilfe des klassischen Azure-Bereitstellungsmodells erstellen müssen, finden Sie unter [Erstellen einer benutzerdefinierten Route (klassisch)](virtual-network-create-udr-classic-ps.md) weitere Informationen. Wenn Sie mit den Azure-Bereitstellungsmodellen nicht vertraut sind, lesen Sie unter [Grundlegendes zu Azure-Bereitstellungsmodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nach. Für weitere Informationen zu benutzerdefinierten Routen können Sie sich die [Übersicht zu benutzerdefinierten Routen](virtual-networks-udr-overview.md#user-defined) ansehen.

## <a name="create-routes-and-network-virtual-appliance"></a>Erstellen der Routen und des virtuellen Netzwerkgeräts

1. **Erforderliche Vorbereitung**: Erstellen Sie ein virtuelles Netzwerk mit zwei Subnetzen, indem Sie die Schritte unter [Erstellen eines virtuellen Netzwerks](#create-a-virtual-network) ausführen.
2. Nachdem Sie das virtuelle Netzwerk in einem Internetbrowser erstellt haben, browsen Sie zum [Azure-Portal](https://portal.azure.com). Melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an.
3. Geben Sie im oben im Portal im Feld **Ressourcen suchen** die Zeichenfolge *myResourceGroup* ein. Klicken Sie auf **myResourceGroup**, wenn sie in den Suchergebnissen angezeigt wird. Die Ressourcengruppe wurde bei der Vorbereitung erstellt.
4. Klicken Sie auf **myVnet**, wie in der folgenden Abbildung gezeigt:

    ![Einstellungen von Netzwerkschnittstellen](./media/create-user-defined-route/virtual-network.png)

5. Erstellen Sie ein Subnetz für das virtuelle Netzwerkgerät:
 
    - Klicken Sie unter **myVnet** auf der linken Seite unter **EINSTELLUNGEN** auf **Subnetze**.
    - Klicken Sie auf **+ Subnetz**, wie in der folgenden Abbildung gezeigt:

        ![Subnetze](./media/create-user-defined-route/subnets.png) 
    - Geben Sie die folgenden Werte unter **Subnetz hinzufügen** ein, und klicken Sie dann auf **OK**:

        |Einstellung|Wert|
        |-----|-----|
        |Name|DMZ|
        |Adressbereich (CIDR-Block)|10.0.2.0/24|

6. Erstellen eines virtuellen Computers für das virtuelle Netzwerkgerät:

    - Klicken Sie auf der linken Seite im Portal auf **+ Neu**, anschließend auf **Compute** und dann auf **Windows Server 2016 Datacenter** oder **Ubuntu Server 16.04 LTS**.
    - Geben Sie auf dem daraufhin angezeigten Blatt **Grundlagen** die folgenden Werte ein, und klicken Sie anschließend auf **OK**.

        |Einstellung|Wert|
        |---|---|
        |Name|myVm-Nva|
        |Benutzername|azureuser|
        |Kennwort und Kennwortbestätigung|Ein Kennwort Ihrer Wahl|
        |Abonnement|Wählen Sie Ihr Abonnement aus.|
        |Ressourcengruppe|Klicken Sie auf **Vorhandene verwenden**, und wählen Sie dann **myResourceGroup** aus.|
        |Standort|USA (Ost)|
    - Klicken Sie auf dem angezeigten Blatt **Größe auswählen** auf **DS1_V2 Standard** und anschließend auf **Auswählen**.
    - Klicken Sie auf dem angezeigten Blatt **Einstellungen** auf **Virtuelles Netzwerk**. Klicken Sie auf dem angezeigten Blatt **Virtuelles Netzwerk auswählen** auf **myVnet**.
    - Klicken Sie auf dem Blatt **Einstellungen** auf **Subnetz**. Klicken Sie auf dem angezeigten Blatt **Subnetz auswählen** auf **DMZ**. 
    - Behalten Sie die Standardwerte für die verbleibenden Einstellungen bei, und klicken Sie auf **OK**.
    - Klicken Sie auf dem angezeigten Blatt **Erstellen** auf **Erstellen**. Die Bereitstellung des virtuellen Computers dauert einige Minuten.

    > [!NOTE]
    > Zusätzlich zum virtuellen Computer erstellt das Azure-Portal standardmäßig auch eine öffentliche IP-Adresse und weist diese dem virtuellen Computer zu. Wenn Sie den virtuellen Computer in einer Produktionsumgebung bereitstellen, müssen Sie ihm keine öffentliche IP-Adresse zuweisen. Sie können stattdessen über andere virtuelle Computer im virtuellen Netzwerk auf das virtuelle Netzwerkgerät zugreifen. Weitere Informationen zu öffentlichen IP-Adressen finden Sie unter [Verwalten öffentlicher IP-Adressen](virtual-network-public-ip-address.md).

7. Weisen Sie eine statische private IP-Adresse zu, und aktivieren Sie die IP-Weiterleitung für die Netzwerkschnittstelle, die das Portal im vorherigen Schritt erstellt hat. 
    - Geben Sie im oben auf der Seite im Feld **Ressourcen suchen** die Zeichenfolge *myVm-Nva* ein.
    - Klicken Sie auf **myVm-Nva**, wenn es in den Suchergebnissen angezeigt wird.
    - Klicken Sie auf der linken Seite unter **EINSTELLUNGEN** auf **Netzwerk**.
    - Klicken Sie unter **myVm-Nva – Netzwerkschnittstellen** auf den Namen der Netzwerkschnittstelle. Der Name lautet **myvm-nva***X*, wobei *X* eine Zahl ist, die vom Portal zugewiesen wird.
    - Klicken Sie für die ausgewählte Netzwerkschnittstelle unter **EINSTELLUNGEN** auf **IP-Konfigurationen**, wie in der folgenden Abbildung gezeigt:

        ![Einstellungen von Netzwerkschnittstellen](./media/create-user-defined-route/network-interface-settings.png)
        
    - Klicken Sie für die Einstellung **IP-Weiterleitung** auf **Aktiviert** und dann auf **Speichern**. Die IP-Weiterleitung muss für jede Netzwerkschnittstelle aktiviert werden, die Datenverkehr empfängt, der nicht an eine IP-Adresse gerichtet ist. Durch das Aktivieren der IP-Weiterleitung wird die Überprüfung von Azure-Quelle und -Ziel für die Netzwerkschnittstelle deaktiviert.
    - Klicken Sie in der Liste der IP-Konfigurationen auf **ipconfig1**.
    - Klicken Sie für die **Zuweisung** der privaten IP-Adresse unter **ipconfig1** auf **Statisch**, wie in der folgenden Abbildung gezeigt:

        ![IP-Konfiguration](./media/create-user-defined-route/ip-configuration.png)
    - Geben Sie wie in der vorherigen Abbildung dargestellt *10.0.2.4* unter **IP-Adresse** in den **Einstellungen für private IP-Adressen** ein. Durch das Zuweisen einer statischen IP-Adresse zur Netzwerkschnittstelle wird sichergestellt, dass die Adresse sich für die Lebensdauer des virtuellen Computers, mit dem die Netzwerkschnittstelle verbunden ist, nicht ändert. Die eingegebene Adresse darf keiner anderen Ressource im DMZ-Subnetz, zu dem die Netzwerkschnittstelle gehört, zugewiesen sein. 
    - Klicken Sie zum Speichern der Konfiguration unter **ipconfig1** auf **Speichern**. Schließen Sie das Feld „ipconfig1“ erst, nachdem Sie im Portal benachrichtigt wurden, dass die Netzwerkschnittstelle gespeichert wurde.
 
8. Erstellen Sie zwei Routingtabellen. Routingtabellen enthalten null oder mehr Routen:

    - Klicken Sie auf der linken Seite des Portals auf **+ Neu** > **Netzwerk** > **Routingtabelle**.
    - Geben Sie unter **Routingtabelle erstellen** die folgenden Werte ein, und klicken Sie dann auf **Erstellen**:

        |Einstellung|Wert|
        |---|---|
        |Name|myRouteTable-Public|
        |Abonnement|Wählen Sie Ihr Abonnement aus.|
        |Ressourcengruppe|Wählen Sie **Vorhandene verwenden** und dann **myResourceGroup** aus.|
        |Standort|USA (Ost)|
    
    - Führen Sie die vorherigen Teilschritte von Schritt 8 erneut aus, benennen Sie sie Routingtabelle jedoch mit *myRouteTable-Private*.
9. Fügen Sie der Routingtabelle *myRouteTable-Public* Routen hinzu, und weisen Sie die Routingtabelle dem *öffentlichen* Subnetz zu:

    - Geben Sie im oben im Portal im Feld **Ressourcen suchen** die Zeichenfolge *myRouteTable-Public* ein. Klicken Sie auf **myRouteTable-Public**, wenn sie in den Suchergebnissen angezeigt wird.
    - Klicken Sie unter **myRouteTable-Public** in der Liste der **EINSTELLUNGEN** auf **Routen**.
    - Klicken Sie unter **myRouteTable-Public – Routen** auf **+ Hinzufügen**.
    -  Standardmäßig leitet Azure den Datenverkehr durch alle Subnetze des virtuellen Netzwerks. Damit der Datenverkehr des *öffentlichen* Subnetzes nicht direkt an das *private* Subnetz, sondern über das virtuelle Netzwerkgerät weitergeleitet wird, müssen Sie eine Route erstellen, um das Azure-Standardrouting außer Kraft zu setzen. Geben Sie auf dem angezeigten Blatt **Route hinzufügen** die folgenden Werte ein, und klicken Sie anschließend auf **OK**:

        |Einstellung|Wert|Erklärung|
        |---|---|---|
        |Routenname|ToPrivateSubnet|Diese Route leitet Datenverkehr über das virtuelle Netzwerkgerät an das private Subnetz weiter.|
        |Adresspräfix|10.0.1.0/24| Sämtlicher Datenverkehr an Adressen in diesem Adresspräfix (10.0.1.0 – 10.0.1.254) wird an das virtuelle Netzwerkgerät gesendet.|
        |Typ des nächsten Hops| Wählen Sie **Virtuelles Gerät** aus.||
        |Adresse des nächsten Hops|10.0.2.4| Die statische private IP-Adresse der Netzwerkschnittstelle, die mit dem virtuellen Netzwerkgerät verbunden ist. Der einzige Hoptyp, für den Sie eine Adresse angeben können, ist **Virtuelles Gerät**.|

    - Klicken Sie unter **myRouteTable-Public** unter **Einstellungen** auf **Subnetze**. 
    - Klicken Sie unter **myRouteTable-Public – Subnetze** auf **+ Zuordnen**.
    - Klicken Sie unter **Subnetz zuordnen** auf **Virtuelles Netzwerk** und dann auf **myVnet**.
    - Klicken Sie unter **Subnetz zuordnen** auf **Subnetz** und dann unter **Subnetz auswählen** auf **Öffentlich**. 
    - Um die Konfiguration zu speichern, klicken Sie unter **Subnetz zuordnen** auf **OK**. Jedem Subnetz können null oder eine Routingtabelle zugeordnet werden.
10. Führen Sie Schritt 9 erneut aus, suchen Sie nach **myRouteTable-Private**, erstellen Sie eine Route mit den folgenden Einstellungen, und ordnen Sie dann die Routingtabelle dem **privaten** Subnetz des virtuellen Netzwerks **myVnet** zu:

    |Einstellung|Wert|Erklärung|
    |---|---|---|
    |Routenname|ToPublicSubnet|Diese Route leitet Datenverkehr über das virtuelle Netzwerkgerät an das öffentliche Subnetz weiter.|
    |Adresspräfix|10.0.0.0/24| Sämtlicher Datenverkehr an Adressen in diesem Adresspräfix (10.0.0.0 – 10.0.1.254) wird an das virtuelle Netzwerkgerät gesendet.|
    |Typ des nächsten Hops| Wählen Sie **Virtuelles Gerät** aus.||
    |Adresse des nächsten Hops|10.0.2.4||

    Netzwerkdatenverkehr zwischen Ressourcen in den privaten und öffentlichen Subnetzen durchläuft das virtuelle Netzwerkgerät. 
11. **Optional**: Erstellen Sie jeweils eine VM im öffentlichen und privaten Subnetz, und überprüfen Sie, ob der Datenverkehr zwischen den VMs über das virtuelle Netzwerkgerät weitergeleitet wird. Führen Sie hierzu die Schritte im Abschnitt [Überprüfen des Routings](#validate-routing) aus. 
12. **Optional:** Löschen Sie die Ressourcen, die Sie in diesem Tutorial erstellt haben, indem Sie die Schritte unter [Löschen von Ressourcen](#delete-resources) durchführen.

## <a name="validate-routing"></a>Überprüfen des Routings

1. Führen Sie die Schritte unter [Erstellen von Routen und virtuellen Netzwerkgeräten](#create-routes-and-network-virtual-appliance) aus, wenn Sie dies noch nicht getan haben.
2. Klicken Sie im folgenden Feld auf die Schaltfläche **Ausprobieren**. Dadurch wird Azure Cloud Shell geöffnet. Melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) bei Azure an, wenn Sie dazu aufgefordert werden. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)registrieren. Azure Cloud Shell ist eine kostenlose Bash-Shell, für die Azure CLI vorinstalliert ist. 

    Mit den folgenden Skripts werden zwei VMs erstellt – eine im *öffentlichen* und eine im *privaten* Subnetz. Mit den Skripts wird auch die IP-Weiterleitung für die Netzwerkschnittstelle innerhalb des Betriebssystems des virtuellen Netzwerkgeräts aktiviert. Dadurch kann das Betriebssystem den Netzwerkdatenverkehr über die Netzwerkschnittstelle weiterleiten. Ein virtuelles Netzwerkgerät in einer Produktionsumgebung überprüft den Netzwerkdatenverkehr in der Regel vor der Weiterleitung. In diesem Tutorial leitet das einfache virtuelle Netzwerkgerät den Datenverkehr allerdings ohne Überprüfung weiter. 

    Klicken Sie im folgenden **Linux**- oder **Windows**-Skript auf die Schaltfläche **Kopieren**, und fügen Sie den Inhalt des Skripts in einen Text-Editor ein. Ändern Sie das Kennwort für die Variable *adminPassword*, und fügen Sie das Skript in Azure Cloud Shell ein. Führen Sie das Skript für das Betriebssystem aus, das Sie in Schritt 6 im Abschnitt [Erstellen von Routen und virtuellen Netzwerkgeräten](#create-routes-and-network-virtual-appliance) bei der Erstellung des virtuellen Netzwerkgeräts ausgewählt haben. 

    **Linux**

    ```azurecli-interactive
    #!/bin/bash

    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.    
    az vm extension set \
      --resource-group $rgName \
      --vm-name myVm-Nva \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
    ```

    **Windows**

    ```azurecli-interactive

    #!/bin/bash
    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Public \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Private \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1"}'

    # Restart the NVA virtual machine.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Restart-Computer -ComputerName myVm-Nva -Force"}'
    ```

3. Überprüfen Sie, ob die VMs des öffentlichen und privaten Subnetzes miteinander kommunizieren. 

    - Stellen Sie unter Linux eine [SSH-Verbindung](../virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) oder unter Windows eine [Remotedesktop-Verbindung](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) mit der öffentlichen IP-Adresse der *myVm-Public-VM* her.
    - Geben Sie in der Eingabeaufforderung der *myVm-Public-VM* `ping myVm-Private` ein. Es werden Antworten empfangen, da das virtuelle Netzwerkgerät den Netzwerkdatenverkehr vom öffentlichen an das private Subnetz weiterleitet.
    - Führen Sie in der *myVm-Public-VM* einen Traceroute-Vorgang für die VMs im öffentlichen und privaten Subnetz aus. Geben Sie je nach installiertem Betriebssystem der VM im öffentlichen und privaten Subnetz einen der folgenden Befehle ein:
        - **Windows**: Führen Sie in der Eingabeaufforderung den Befehl `tracert myvm-private` aus.
        - **Ubuntu**: Führen Sie den Befehl `tracepath myvm-private` aus.
      Der Netzwerkdatenverkehr wird zuerst an die IP-Adresse 10.0.2.4 (das virtuelle Netzwerkgerät) und anschließend an die IP-Adresse 10.0.1.4 (die VM im privaten Subnetz) weitergeleitet. 
    - Führen Sie die vorherigen Schritte aus, indem Sie eine Verbindung mit der *myVm-Private-VM* herstellen und die *myVm-Public-VM* anpingen. Die Traceroute veranschaulicht den Netzwerkdatenverkehr, der über die IP-Adresse 10.0.2.4 an die IP-Adresse 10.0.0.4 (die VM im öffentlichen Subnetz) weitergeleitet wird.

      > [!NOTE]
      > Mithilfe der vorhergehenden Schritte können Sie die Weiterleitung zwischen privaten Azure-IP-Adressen bestätigen. Falls Sie Datenverkehr durch eine virtuelle Netzwerkanwendung an öffentliche IP-Adressen weiterleiten – also einen Proxy bereitstellen – möchten, gilt:
      > - Die Appliance muss Netzwerkadressübersetzung oder Proxyfunktionen bereitstellen. Im Fall von Netzwerkadressübersetzung muss die Appliance die Quell-IP-Adresse in ihre eigene übersetzen und diese Anforderung anschließend an die öffentliche IP-Adresse weiterleiten. Unabhängig davon, ob die Appliance die Quelladresse in eine Netzwerkadresse übersetzt oder als Proxy fungiert, übersetzt Azure die private IP-Adresse der virtuellen Appliance in eine öffentliche IP-Adresse. Weitere Informationen über die verschiedenen Methoden, die Azure verwendet, um private IP-Adressen in öffentliche IP-Adressen zu übersetzen finden Sie unter [Understanding outbound connections](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Grundlegendes zu ausgehenden Verbindungen).
      > - Eine zusätzliche Route in der Routingtabelle, wie etwa Präfix: 0.0.0.0/0, Typ des nächsten Hops VirtualAppliance und IP-Adresse des nächsten Hops 10.0.2.4 (ausgehend vom vorhergehenden Beispielskript).
      >
    - **Optional:** Verwenden Sie die Funktion „Nächster Hop“ von Azure Network Watcher, um den nächsten Hop zwischen zwei virtuellen Computern in Azure zu überprüfen. Vor der Verwendung von Network Watcher müssen Sie zuerst [eine Instanz von Azure Network Watcher](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json) für die Region erstellen, in der Sie diesen Dienst verwenden möchten. In diesem Tutorial wird die Region „USA, Osten“ verwendet. Nachdem Sie eine Network Watcher-Instanz für die Region erstellt haben, geben Sie den folgenden Befehl ein, um sich Informationen zum nächsten Hop zwischen den VMs im öffentlichen und privaten anzeigen Subnetz anzeigen zu lassen:
     
        ```azurecli-interactive
        az network watcher show-next-hop --resource-group myResourceGroup --vm myVm-Public --source-ip 10.0.0.4 --dest-ip 10.0.1.4
        ```

       In der Ausgabe wird *10.0.2.4* für **nextHopIpAddress** und *VirtualAppliance* für **nextHopType** angezeigt. 

> [!NOTE]
> Um die Konzepte in diesem Tutorial zu veranschaulichen, werden den VMs im öffentlichen und privaten Subnetz öffentliche IP-Adressen zugewiesen. Außerdem wird der Zugriff auf Netzwerkports in Azure für beide VMs aktiviert. Wenn Sie VMs für die Produktion erstellen, dürfen Sie diesen keine öffentliche IP-Adressen zuweisen. Außerdem stehen Ihnen zwei Möglichkeiten zur Verfügung, den an das private Subnetz weitergeleitete Netzwerkdatenverkehr filtern: Sie können entweder ein virtuelles Netzwerkgerät vor dem Subnetz bereitstellen oder aber den Subnetzen, der Netzwerkschnittstelle oder beiden eine Netzwerksicherheitsgruppe zuweisen. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Netzwerksicherheitsgruppen](virtual-networks-nsg.md).

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Für dieses Tutorial ist ein vorhandenes virtuelles Netzwerk mit zwei Subnetzen erforderlich. Klicken Sie im folgenden Feld auf **Ausprobieren**, um schnell ein virtuelles Netzwerk zu erstellen. Durch Klicken auf die Schaltfläche **Ausprobieren** wird [Azure Cloud Shell](../cloud-shell/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) geöffnet. Obwohl in Cloud Shell sowohl PowerShell als auch die Bash-Shell ausgeführt werden können, wird in diesem Abschnitt die Bash-Shell verwendet, um das virtuelle Netzwerk zu erstellen. Für die Bash-Shell ist die Azure CLI installiert. Melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) bei Azure an, wenn Sie von Cloud Shell dazu aufgefordert werden. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)registrieren. Klicken Sie zur Erstellung des in diesem Tutorial verwendeten virtuellen Netzwerks im folgenden Feld auf die Schaltfläche **Kopieren**, und fügen Sie das Skript in Azure Cloud Shell ein:

```azurecli-interactive
#!/bin/bash

#Set variables used in the script.
rgName="myResourceGroup"
location="eastus"

# Create a resource group.
az group create \
  --name $rgName \
  --location $location

# Create a virtual network with one subnet named Public.
az network vnet create \
  --name myVnet \
  --resource-group $rgName \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24

# Create an additional subnet named Private in the virtual network.
az network vnet subnet create \
  --name Private \
  --address-prefix 10.0.1.0/24 \
  --vnet-name myVnet \
  --resource-group $rgName
```

Weitere Informationen zur Verwendung des Portals, von PowerShell oder von Azure Resource Manager-Vorlagen zum Erstellen eines virtuellen Netzwerks finden Sie unter [Erstellen eines virtuellen Netzwerks](virtual-networks-create-vnet-arm-pportal.md).

## <a name="delete-resources"></a>Löschen von Ressourcen

Wenn Sie dieses Tutorial abgeschlossen haben, möchten Sie die Ressourcen, die Sie erstellt haben, möglicherweise wieder löschen, damit keine Nutzungsgebühren anfallen. Durch das Löschen einer Ressourcengruppe werden auch alle Ressourcen in dieser Ressourcengruppe gelöscht. Führen Sie im geöffneten Portal anschließend die folgenden Schritte aus:

1. Geben Sie in das Suchfeld im Portal **myResourceGroup** ein. Klicken Sie in den Suchergebnissen auf **myResourceGroup**.
2. Klicken Sie auf dem Blatt **myResourceGroup** auf das **Löschsymbol**.
3. Um den Löschvorgang zu bestätigen, geben Sie in das Feld **TYPE THE RESOURCE GROUP NAME** (Ressourcengruppennamen eingeben) **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie ein [hoch verfügbares virtuelles Netzwerkgerät](/azure/architecture/reference-architectures/dmz/nva-ha?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Virtuellen Netzwerkgeräten sind häufig mehrere Netzwerkschnittstellen und IP-Adressen zugewiesen. Erfahren Sie, wie Sie [einer vorhandenen VM Netzwerkschnittstellen hinzufügen](virtual-network-network-interface-vm.md#vm-add-nic) und [einer vorhandenen Netzwerkschnittstelle IP-Adressen hinzufügen](virtual-network-network-interface-addresses.md#add-ip-addresses). Obwohl VMs aller Größenkategorien mindestens über zwei Netzwerkschnittstellen verfügen, ist für jede Größenkategorie eine maximale Anzahl von unterstützten Netzwerkschnittstellen vorgesehen. Informationen zur maximalen Anzahl von unterstützten Netzwerkschnittstellen für die jeweilige Größenkategorie der VM finden Sie in den Artikeln zu [Windows](../virtual-machines/windows/sizes.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json)- und [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-VM-Größenkategorien. 
- Erstellen Sie eine benutzerdefinierte Route, um den Netzwerkdatenverkehr mittels Tunnelerzwingung über eine [Site-to-Site-VPN-Verbindung](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) an einen lokalen Standort zu übertragen.
