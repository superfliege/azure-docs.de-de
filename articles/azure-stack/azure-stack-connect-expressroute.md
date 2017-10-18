---
title: Herstellen einer Verbindung von Azure Stack zu Azure mithilfe von ExpressRoute
description: Es wird beschrieben, wie Sie eine Verbindung zwischen virtuellen Netzwerken in Azure Stack und virtuellen Netzwerken in Azure mithilfe von ExpressRoute herstellen.
services: azure-stack
documentationcenter: 
author: victorar
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
ms.openlocfilehash: aa6973939c6cfe0688f5781fdcea5d39670249df
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="connect-azure-stack-to-azure-using-expressroute"></a>Herstellen einer Verbindung von Azure Stack zu Azure mithilfe von ExpressRoute

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Es gibt zwei unterstützte Methoden, um virtuelle Netzwerke in Azure Stack mit virtuellen Netzwerken in Azure zu verbinden:
   * **Standort-zu-Standort**

     Eine VPN-Verbindung über IPsec (IKE v1 und IKE v2). Für diese Art von Verbindung wird ein VPN-Gerät oder RRAS benötigt. Weitere Informationen finden Sie unter [Herstellen einer Verbindung von Azure Stack zu Azure mithilfe von VPN](azure-stack-connect-vpn.md).
   * **ExpressRoute**

     Eine direkte Verbindung mit Azure aus Ihrer Azure Stack-Bereitstellung. Bei ExpressRoute handelt es sich **nicht** um eine VPN-Verbindung über das öffentliche Internet. Weitere Informationen zu Azure ExpressRoute finden Sie unter [ExpressRoute-Übersicht](../expressroute/expressroute-introduction.md).

Dieser Artikel enthält ein Beispiel, in dem ExpressRoute zum Herstellen einer Verbindung von Azure Stack mit Azure verwendet wird.
## <a name="requirements"></a>Requirements (Anforderungen)
Hier sind die spezifischen Anforderungen für die Verbindungsherstellung zwischen Azure Stack und Azure mit ExpressRoute angegeben:
* Ein Azure-Abonnement zum Erstellen einer ExpressRoute-Verbindung und von VNets in Azure.
* Eine bereitgestellte ExpressRoute-Verbindung über einen [Konnektivitätsanbieter](../expressroute/expressroute-locations.md).
* Ein Router, für den die ExpressRoute-Verbindung mit den entsprechenden WAN-Ports verbunden ist.
* Die LAN-Seite des Routers ist mit dem mehrinstanzenfähigen Azure Stack-Gateway verknüpft.
* Der Router muss Site-to-Site-VPN-Verbindungen zwischen seiner LAN-Schnittstelle und dem mehrinstanzenfähigen Azure Stack-Gateway unterstützen.
* Wenn Ihrer Azure Stack-Bereitstellung mehr als ein Mandant hinzugefügt wird, muss der Router mehrere VRF-Einheiten (Virtual Routing and Forwarding) erstellen können.

Im folgenden Diagramm ist eine konzeptuelle Netzwerkansicht nach Abschluss der Konfiguration dargestellt:

![Konzeptdiagramm](media/azure-stack-connect-expressroute/Conceptual.png)

**Diagramm 1**

Im folgenden Architekturdiagramm ist dargestellt, wie für mehrere Mandanten eine Verbindung von der Azure Stack-Infrastruktur über den ExpressRoute-Router mit Azure im Microsoft-Edgebereich hergestellt wird:

![Architekturdiagramm](media/azure-stack-connect-expressroute/Architecture.png)

**Diagramm 2**

Im Beispiel in diesem Artikel wird die gleiche Architektur verwendet, um per privatem Peering mit ExpressRoute eine Verbindung mit Azure herzustellen. Es wird eine Site-to-Site-VPN-Verbindung vom Gateway des virtuellen Netzwerks in Azure Stack mit einem ExpressRoute-Router hergestellt. Die folgenden Schritte in diesem Artikel veranschaulichen, wie Sie eine End-to-End-Verbindung zwischen zwei VNETs von zwei unterschiedlichen Mandanten in Azure Stack mit den jeweiligen VNETs in Azure herstellen. Sie können beliebig viele Mandanten-VNETs hinzufügen und die Schritte für jeden Mandanten replizieren oder dieses Beispiel nutzen, um ein VNET mit nur einem Mandanten bereitzustellen.

## <a name="configure-azure-stack"></a>Konfigurieren von Azure Stack
Nun erstellen Sie die Ressourcen, die Sie zum Einrichten Ihrer Azure Stack-Umgebung als Mandant benötigen. Die Vorgehensweise wird in den folgenden Schritten veranschaulicht. In dieser Anleitung wird gezeigt, wie Sie Ressourcen mit dem Azure Stack-Portal erstellen, aber Sie können auch PowerShell verwenden.

![Schritte für Netzwerkressourcen](media/azure-stack-connect-expressroute/image2.png)
### <a name="before-you-begin"></a>Voraussetzungen
Sie benötigen Folgendes, um mit der Konfiguration beginnen zu können:
* Eine Azure Stack-Bereitstellung.

   Informationen zum Bereitstellen eines Azure Stack Development Kit finden Sie unter [Schnellstart für die Azure Stack Development Kit-Bereitstellung](azure-stack-deploy-overview.md).
* Ein Angebot unter Azure Stack, das von Ihren Benutzern abonniert werden kann.

  Eine Anleitung hierzu finden Sie unter [Verfügbarmachen von virtuellen Computern für Ihre Azure Stack-Benutzer](azure-stack-tutorial-tenant-vm.md).

### <a name="create-network-resources-in-azure-stack"></a>Erstellen von Netzwerkressourcen in Azure Stack

Verwenden Sie die folgenden Verfahren, um die erforderlichen Netzwerkressourcen in Azure Stack für jeden Mandanten zu erstellen:

#### <a name="create-the-virtual-network-and-vm-subnet"></a>Erstellen des virtuellen Netzwerks und des VM-Subnetzes
1. Melden Sie sich im Benutzerportal mit einem Benutzerkonto (Mandanten) an.

2. Klicken Sie im Portal auf **Neu**.

   ![](media/azure-stack-connect-expressroute/MAS-new.png)

3. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.

4. Klicken Sie im Menü auf **Virtuelles Netzwerk**.

5. Geben Sie die Werte in die richtigen Felder ein, indem Sie die folgende Tabelle als Hilfe verwenden:

   |Feld  |Wert  |
   |---------|---------|
   |Name     |Tenant1VNet1         |
   |Adressraum     |10.1.0.0/16|
   |Subnetzname     |Tenant1-Sub1|
   |Subnetzadressbereich     |10.1.1.0/24|

6. Daraufhin sollte das Feld **Abonnement** mit dem zuvor erstellten Abonnement aufgefüllt werden.

    a. Als Ressourcengruppe können Sie entweder eine neue Ressourcengruppe erstellen oder **Use existing** (Vorhandene verwenden) auswählen, sofern Sie bereits über eine Ressourcengruppe verfügen.

    b. Überprüfen Sie den standardmäßigen Speicherort.

    c. Klicken Sie auf **An Dashboard anheften**.

    d. Klicken Sie auf **Erstellen**.



#### <a name="create-the-gateway-subnet"></a>Erstellen des Gatewaysubnetzes
1. Öffnen Sie über das Dashboard die zuvor erstellte Ressource für das virtuelle Netzwerk (Tenant1VNet1).
2. Wählen Sie im Abschnitt „Einstellungen“ die Option **Subnetze**.
3. Klicken Sie auf **Gatewaysubnetz**, um dem virtuellen Netzwerk ein Gatewaysubnetz hinzuzufügen.
   
    ![](media/azure-stack-connect-expressroute/gatewaysubnet.png)
4. Der Name des Subnetzes wird standardmäßig auf **GatewaySubnet** festgelegt.
   Gatewaysubnetze sind spezielle Subnetze und müssen diesen spezifischen Namen besitzen, damit sie ordnungsgemäß funktionieren.
5. Stellen Sie sicher, dass im Feld **Adressbereich** die Adresse **10.1.0.0/24** angegeben ist.
6. Klicken Sie auf **OK**, um das Gatewaysubnetz zu erstellen.

#### <a name="create-the-virtual-network-gateway"></a>Erstellen des Gateways für das lokale Netzwerk
1. Klicken Sie im Azure Stack-Benutzerportal auf **Neu**.
   
2. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.
3. Wählen Sie in der Liste mit den Netzwerkressourcen den Eintrag **Gateway von Virtual Network** aus.
4. Geben Sie im Feld **Name** die Zeichenfolge **GW1** ein.
5. Klicken Sie auf **Virtuelles Netzwerk**, um ein virtuelles Netzwerk auszuwählen.
   Wählen Sie in der Liste die Option **Tenant1VNet1** aus.
6. Klicken Sie auf das Menüelement **Öffentliche IP-Adresse**. Klicken Sie im Abschnitt **Öffentliche IP-Adresse auswählen** auf **Neu erstellen**.
7. Geben Sie im Feld **Name** die Zeichenfolge **GW1-PiP** ein, und klicken Sie auf **OK**.
8. Für **VPN-Typ** sollte standardmäßig **Routenbasiert** ausgewählt sein.
    Behalten Sie diese Einstellung bei.
9. Vergewissern Sie sich, dass **Abonnement** und **Speicherort** korrekt sind. Die Ressource kann auf Wunsch an das Dashboard angeheftet werden. Klicken Sie auf **Erstellen**.

#### <a name="create-the-local-network-gateway"></a>Erstellen des Gateways des lokalen Netzwerks

Der Zweck der Ressource „ Gateway des lokalen Netzwerks“ besteht darin, das Remotegateway am anderen Ende der VPN-Verbindung anzugeben. In diesem Beispiel ist die Remoteseite die LAN-Unterschnittstelle des ExpressRoute-Routers. Für „Tenant 1“ (Mandant 1) lautet die Remoteadresse in diesem Beispiel 10.60.3.255, wie in Diagramm 2 dargestellt.

1. Melden Sie sich auf dem physischen Computer für Azure Stack an.
2. Melden Sie sich mit Ihrem Benutzerkonto am Benutzerportal an, und klicken Sie auf **Neu**.
3. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.
4. Wählen Sie in der Ressourcenliste den Eintrag für das **lokale Netzwerkgateway** aus.
5. Geben Sie im Feld **Name** den Text **ER-Router-GW** ein.
6. Verwenden Sie zum Eingeben der **IP-Adresse** Diagramm 2 als Hilfe. Die IP-Adresse der LAN-Unterschnittstelle für den ExpressRoute-Router von „Tenant 1“ lautet 10.60.3.255. Geben Sie für Ihre eigene Umgebung die IP-Adresse der entsprechenden Schnittstelle Ihres Routers ein.
7. Geben Sie im Feld **Adressraum** den Adressraum der VNETs ein, mit denen Sie in Azure eine Verbindung herstellen möchten. Nutzen Sie für dieses Beispiel Diagramm 2 als Hilfe. Beachten Sie, dass die erforderlichen Subnetze für „Tenant 1“ **192.168.2.0/24** (Hub-VNET in Azure) und **10.100.0.0/16** (Spoke-VNET in Azure) lauten. Geben Sie die entsprechenden Subnetze für Ihre eigene Umgebung ein.
   > [!IMPORTANT]
   > In diesem Beispiel wird vorausgesetzt, dass Sie für die Site-to-Site-VPN-Verbindung zwischen dem Azure Stack-Gateway und dem ExpressRoute-Router statische Routen verwenden.

8. Überprüfen Sie **Abonnement**, **Ressourcengruppe** und **Speicherort** auf Richtigkeit, und klicken Sie auf **Erstellen**.

#### <a name="create-the-connection"></a>Erstellen der Verbindung
1. Klicken Sie im Azure Stack-Benutzerportal auf **Neu**.
2. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.
3. Wählen Sie in der Ressourcenliste die Option **Verbindung** aus.
4. Wählen Sie im Abschnitt **Grundlagen** mit den Einstellungen die Option **Standort-zu-Standort (IPsec)** als **Verbindungstyp** aus.
5. Wählen Sie Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** aus, und klicken Sie anschließend auf **OK**.
6. Klicken Sie im Abschnitt **Einstellungen** auf **Gateway für virtuelle Netzwerke** und dann auf **GW1**.
7. Klicken Sie auf **Gateway für lokales Netzwerk** und dann auf **ER Router GW**.
8. Geben Sie im Feld **Verbindungsname** die Zeichenfolge **ConnectToAzure** ein.
9. Geben Sie im Feld **Gemeinsam verwendeter Schlüssel (PSK)** die Zeichenfolge **abc123** ein, und klicken Sie anschließend auf **OK**.
10. Klicken Sie im Abschnitt **Zusammenfassung** auf **OK**.

    Nachdem die Verbindung hergestellt wurde, wird die öffentliche IP-Adresse angezeigt, die vom Gateway des virtuellen Netzwerks verwendet wird. Navigieren Sie zum Gateway des virtuellen Netzwerks, um im Azure Stack-Portal nach der Adresse zu suchen. Suchen Sie in der **Übersicht** nach der **öffentlichen IP-Adresse**. Notieren Sie sich diese Adresse. Sie verwenden sie im nächsten Abschnitt als *interne IP-Adresse* (falls für Ihre Bereitstellung zutreffend).

    ![](media/azure-stack-connect-expressroute/GWPublicIP.png)

#### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers
Zum Überprüfen von Daten, die über die VPN-Verbindung fließen, benötigen Sie virtuelle Computer, um Daten im Azure Stack-VNET zu senden und zu empfangen. Erstellen Sie jetzt einen virtuellen Computer, und platzieren Sie ihn dann im VM-Subnetz Ihres virtuellen Netzwerks.

1. Klicken Sie im Azure Stack-Benutzerportal auf **Neu**.
2. Wählen Sie im Marketplace-Menü die Option **Virtual Machines** aus.
3. Wählen Sie in der Liste mit den VM-Images das Image **Windows Server 2016 Datacenter Eval** aus, und klicken Sie auf **Erstellen**.
4. Geben Sie im Abschnitt **Grundlagen** im Feld **Name** die Zeichenfolge **VM01** ein.
5. Geben Sie einen gültigen Benutzernamen und ein Kennwort ein. Mit diesem Konto melden Sie sich bei dem virtuellen Computer an, nachdem dieser erstellt wurde.
6. Geben Sie Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** an, und klicken Sie anschließend auf **OK**.
7. Klicken Sie im Abschnitt **Größe** für diese Instanz auf eine Größe für den virtuellen Computer und anschließend auf **Auswählen**.
8. Übernehmen Sie im Abschnitt **Einstellungen** die Standardeinstellungen. Vergewissern Sie sich aber, dass als virtuelles Netzwerk **Tenant1VNet1** und als Subnetz **10.1.1.0/24** ausgewählt ist. Klicken Sie auf **OK**.
9. Überprüfen Sie die Einstellungen im Abschnitt **Zusammenfassung**, und klicken Sie anschließend auf **OK**.

Wiederholen Sie für jedes Mandanten-VNET, für das Sie eine Verbindung herstellen möchten, die vorherigen Schritte in den Abschnitten **Erstellen des virtuellen Netzwerks und des VM-Subnetzes** bis **Erstellen eines virtuellen Computers**.

### <a name="configure-the-nat-virtual-machine-for-gateway-traversal"></a>Konfigurieren des virtuellen NAT-Computers für das Durchlaufen des Gateways
> [!IMPORTANT]
> Dieser Abschnitt gilt nur für Azure Stack Development Kit-Bereitstellungen. NAT ist für Bereitstellungen mit mehreren Knoten nicht erforderlich.

Das Azure Stack Development Kit ist eigenständig und von dem Netzwerk isoliert, in dem der physische Host bereitgestellt wird. Das „externe“ VIP-Netzwerk, mit dem die Gateways verbunden sind, ist also nicht extern, sondern hinter einem Router mit Netzwerkadressübersetzung (NAT) angeordnet.
 
Der Router ist ein virtueller Windows Server-Computer (**AzS-BGPNAT01**), der die RRAS-Rolle (Routing and Remote Access Services, Routing- und RAS-Dienste) in der Azure Stack Development Kit-Infrastruktur ausführt. Sie müssen NAT auf dem virtuellen Computer „AzS-BGPNAT01“ so konfigurieren, dass die Site-to-Site-VPN-Verbindung an beiden Enden möglich ist.

#### <a name="configure-the-nat"></a>Konfigurieren der NAT

1. Melden Sie sich mit Ihrem Administratorkonto am physischen Azure Stack-Computer an.
2. Kopieren und bearbeiten Sie das folgende PowerShell-Skript, und führen Sie es in einer Windows PowerShell ISE mit erhöhten Rechten aus. Fügen Sie Ihr Administratorkennwort ein. Die zurückgegebene Adresse ist Ihre *externe BGPNAT-Adresse*.

   ```
   cd \AzureStack-Tools-master\connect
   Import-Module .\AzureStack.Connect.psm1
   $Password = ConvertTo-SecureString "<your administrator password>" `
    -AsPlainText `
    -Force
   Get-AzureStackNatServerAddress `
    -HostComputer "azs-bgpnat01" `
    -Password $Password
   ```
4. Kopieren und bearbeiten Sie das folgende PowerShell-Skript, und führen Sie es in einer Windows PowerShell ISE mit erhöhten Rechten aus, um NAT zu konfigurieren. Bearbeiten Sie das Skript, um die *externe BGPNAT-Adresse* und die *interne IP-Adresse* zu ersetzen (zuvor im Abschnitt **Erstellen der Verbindung** notiert).

   In den Beispieldiagrammen ist die *externe BGPNAT-Adresse* 10.10.0.62 und die *interne IP-Adresse* 192.168.102.1.

   ```
   # Designate the external NAT address for the ports that use the IKE authentication.
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress <External BGPNAT address> `
      -PortStart 499 `
      -PortEnd 501}
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress <External BGPNAT address> `
      -PortStart 4499 `
      -PortEnd 4501}
   # create a static NAT mapping to map the external address to the Gateway
   # Public IP Address to map the ISAKMP port 500 for PHASE 1 of the IPSEC tunnel
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress <External BGPNAT address> `
      -InternalIPAddress <Internal IP address> `
      -ExternalPort 500 `
      -InternalPort 500}
   # Finally, configure NAT traversal which uses port 4500 to
   # successfully establish the complete IPSEC tunnel over NAT devices
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress <External BGPNAT address> `
      -InternalIPAddress <Internal IP address> `
      -ExternalPort 4500 `
      -InternalPort 4500}
   ```

## <a name="configure-azure"></a>Konfigurieren von Azure
Nachdem Sie die Azure Stack-Konfiguration abgeschlossen haben, können Sie einige Azure-Ressourcen bereitstellen. Im folgenden Diagramm ist ein Beispiel für ein virtuelles Mandantennetzwerk in Azure dargestellt. Sie können für Ihr VNET in Azure ein beliebiges Namens- und Adressierungsschema verwenden. Der Adressbereich der VNETs in Azure und Azure Stack muss aber eindeutig sein und darf sich nicht überlappen.

![Azure-VNETs](media/azure-stack-connect-expressroute/AzureArchitecture.png)

**Diagramm 3**

Die Ressourcen, die Sie in Azure bereitstellen, ähneln den Ressourcen, die Sie in Azure Stack bereitgestellt haben. Auf ähnliche Weise stellen Sie Folgendes bereit:
* Virtuelle Netzwerke und Subnetze
* Ein Gatewaysubnetz
* Ein virtuelles Netzwerkgateway
* Eine Verbindung
* Eine ExpressRoute-Verbindung

Die Azure-Netzwerkinfrastruktur im Beispiel ist wie folgt konfiguriert:
* Es wird ein Hub-and-Spoke-VNET-Standardmodell verwendet (Hub: 192.168.2.0/24, Spoke 10.100.0.0./16).
* Die Workloads werden im Spoke-VNET bereitgestellt, und die ExpressRoute-Verbindung wird mit dem Hub-VNET hergestellt.
* Die beiden VNETs werden mit dem Feature „VNET-Peering“ verknüpft.

### <a name="configure-vnets"></a>Konfigurieren von VNETs
1. Melden Sie sich mit Ihren Azure-Anmeldeinformationen am Azure-Portal an.
2. Erstellen Sie das Hub-VNET, indem Sie den Adressraum 192.168.2.0/24 verwenden. Erstellen Sie über den Adressbereich 192.168.2.0/25 ein Subnetz, und fügen Sie ein Gatewaysubnetz hinzu, indem Sie den Adressbereich 192.168.2.128/27 nutzen.
3. Erstellen Sie das Spoke-VNET und das Subnetz über den Adressbereich 10.100.0.0/16.


Weitere Informationen zur Erstellung von virtuellen Netzwerken in Azure finden Sie unter [Erstellen eines virtuellen Netzwerks mit mehreren Subnetzen](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

### <a name="configure-an-expressroute-circuit"></a>Konfigurieren einer ExpressRoute-Verbindung

1. Überprüfen Sie die ExpressRoute-Voraussetzungen unter [Voraussetzungen und Checkliste für ExpressRoute](../expressroute/expressroute-prerequisites.md).
2. Führen Sie die Schritte unter [Erstellen und Ändern einer ExpressRoute-Verbindung](../expressroute/expressroute-howto-circuit-portal-resource-manager.md) aus, um mit Ihrem Azure-Abonnement eine ExpressRoute-Verbindung zu erstellen.
3. Teilen Sie den Dienstschlüssel aus dem vorherigen Schritt Ihrem Hoster/Anbieter mit, damit dieser die Bereitstellung Ihrer ExpressRoute-Verbindung auf seiner Seite durchführen kann.
4. Führen Sie die Schritte unter [Erstellen und Ändern des Peerings für eine ExpressRoute-Verbindung](../expressroute/expressroute-howto-routing-portal-resource-manager.md) aus, um das private Peering für die ExpressRoute-Verbindung zu konfigurieren.

### <a name="create-the-virtual-network-gateway"></a>Erstellen des Gateways für das lokale Netzwerk

* Führen Sie die Schritte unter [Konfigurieren eines Gateways für ein virtuelles Netzwerk für ExpressRoute mit PowerShell](../expressroute/expressroute-howto-add-gateway-resource-manager.md) aus, um ein Gateway des virtuellen Netzwerks für ExpressRoute im Hub-VNET zu erstellen.

### <a name="create-the-connection"></a>Erstellen der Verbindung

* Führen Sie die Schritte unter [Verbinden eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md) aus, um die ExpressRoute-Verbindung mit dem Hub-VNET zu verknüpfen.

### <a name="peer-the-vnets"></a>Verknüpfen der VNETs per Peering

* Verknüpfen Sie die Hub-and-Spoke-VNETs mit den Schritten unter [Erstellen eines VNET-Peerings mit dem Azure-Portal](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md). Stellen Sie beim Konfigurieren des VNET-Peerings sicher, dass Sie die folgenden Optionen auswählen:
   * Von Hub zu Spoke: **Gatewaytransit zulassen**
   * Von Spoke zu Hub: **Remotegateway verwenden**

### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

* Stellen Sie Ihre virtuellen Workloadcomputer im Spoke-VNET bereit.

Wiederholen Sie diese Schritte für alle zusätzlichen Mandanten-VNETs, für die Sie in Azure über die jeweiligen ExpressRoute-Verbindungen eine Verbindung herstellen möchten.

## <a name="configure-the-router"></a>Konfigurieren des Routers

Sie können das folgende Diagramm mit der End-to-End-Infrastruktur für die Konfiguration Ihres ExpressRoute-Routers verwenden. In diesem Diagramm sind zwei Mandanten („Tenant 1“ und „Tenant 2“) mit ihren jeweiligen ExpressRoute-Verbindungen dargestellt. Es besteht jeweils eine Verbindung mit einem eigenen VRF-Modul (Virtual Routing and Forwarding) auf der LAN- und WAN-Seite des ExpressRoute-Routers, um sicherzustellen, dass zwischen den beiden Mandanten für eine End-to-End-Isolation gesorgt ist. Notieren Sie sich die IP-Adressen, die für die Routerschnittstellen verwendet werden, während Sie die Beispielkonfiguration durcharbeiten.

![End-to-End-Diagramm](media/azure-stack-connect-expressroute/EndToEnd.png)

**Diagramm 4**

Sie können einen beliebigen Router verwenden, der ein IKEv2-VPN und BGP unterstützt, um die Site-to-Site-VPN-Verbindung von Azure Stack zu beenden. Derselbe Router wird verwendet, um über eine ExpressRoute-Verbindung eine Verbindung mit Azure herzustellen. 

Hier ist eine Beispielkonfiguration von einer Cisco ASR 1000-Einheit angegeben, die die in Diagramm 4 dargestellte Netzwerkinfrastruktur unterstützt:

```
ip vrf Tenant 1
 description Routing Domain for PRIVATE peering to Azure for Tenant 1
 rd 1:1
!
ip vrf Tenant 2
 description Routing Domain for PRIVATE peering to Azure for Tenant 2
 rd 1:5
!
crypto ikev2 proposal V2-PROPOSAL2 
description IKEv2 proposal for Tenant 1 
encryption aes-cbc-256
 integrity sha256
 group 2
crypto ikev2 proposal V4-PROPOSAL2 
description IKEv2 proposal for Tenant 2 
encryption aes-cbc-256
 integrity sha256
 group 2
!
crypto ikev2 policy V2-POLICY2 
description IKEv2 Policy for Tenant 1 
match fvrf Tenant 1
 match address local 10.60.3.255
 proposal V2-PROPOSAL2
description IKEv2 Policy for Tenant 2
crypto ikev2 policy V4-POLICY2 
 match fvrf Tenant 2
 match address local 10.60.3.251
 proposal V4-PROPOSAL2
!
crypto ikev2 profile V2-PROFILE
description IKEv2 profile for Tenant 1 
match fvrf Tenant 1
 match address local 10.60.3.255
 match identity remote any
 authentication remote pre-share key abc123
 authentication local pre-share key abc123
 ivrf Tenant 1
!
crypto ikev2 profile V4-PROFILE
description IKEv2 profile for Tenant 2
 match fvrf Tenant 2
 match address local 10.60.3.251
 match identity remote any
 authentication remote pre-share key abc123
 authentication local pre-share key abc123
 ivrf Tenant 2
!
crypto ipsec transform-set V2-TRANSFORM2 esp-gcm 256 
 mode tunnel
crypto ipsec transform-set V4-TRANSFORM2 esp-gcm 256 
 mode tunnel
!
crypto ipsec profile V2-PROFILE
 set transform-set V2-TRANSFORM2 
 set ikev2-profile V2-PROFILE
!
crypto ipsec profile V4-PROFILE
 set transform-set V4-TRANSFORM2 
 set ikev2-profile V4-PROFILE
!
interface Tunnel10
description S2S VPN Tunnel for Tenant 1
 ip vrf forwarding Tenant 1
 ip address 11.0.0.2 255.255.255.252
 ip tcp adjust-mss 1350
 tunnel source TenGigabitEthernet0/1/0.211
 tunnel mode ipsec ipv4
 tunnel destination 10.10.0.62
 tunnel vrf Tenant 1
 tunnel protection ipsec profile V2-PROFILE
!
interface Tunnel20
description S2S VPN Tunnel for Tenant 2
 ip vrf forwarding Tenant 2
 ip address 11.0.0.2 255.255.255.252
 ip tcp adjust-mss 1350
 tunnel source TenGigabitEthernet0/1/0.213
 tunnel mode ipsec ipv4
 tunnel destination 10.10.0.62
 tunnel vrf VNET3
 tunnel protection ipsec profile V4-PROFILE
!
interface GigabitEthernet0/0/1
 description PRIMARY Express Route Link to AZURE over Equinix
 no ip address
 negotiation auto
!
interface GigabitEthernet0/0/1.100
description Primary WAN interface of Tenant 1
 description PRIMARY ER link supporting Tenant 1 to Azure
 encapsulation dot1Q 101
 ip vrf forwarding Tenant 1
 ip address 192.168.1.1 255.255.255.252
!
interface GigabitEthernet0/0/1.102
description Primary WAN interface of Tenant 2
 description PRIMARY ER link supporting Tenant 2 to Azure
 encapsulation dot1Q 102
 ip vrf forwarding Tenant 2
 ip address 192.168.1.17 255.255.255.252
!
interface GigabitEthernet0/0/2
 description BACKUP Express Route Link to AZURE over Equinix
 no ip address
 negotiation auto
!
interface GigabitEthernet0/0/2.100
description Secondary WAN interface of Tenant 1
 description BACKUP ER link supporting Tenant 1 to Azure
 encapsulation dot1Q 101
 ip vrf forwarding Tenant 1
 ip address 192.168.1.5 255.255.255.252
!
interface GigabitEthernet0/0/2.102
description Secondary WAN interface of Tenant 2 
description BACKUP ER link supporting Tenant 2 to Azure
 encapsulation dot1Q 102
 ip vrf forwarding Tenant 2
 ip address 192.168.1.21 255.255.255.252
!
interface TenGigabitEthernet0/1/0
 description Downlink to ---Port 1/47
 no ip address
!
interface TenGigabitEthernet0/1/0.211
 description LAN interface of Tenant 1
description Downlink to --- Port 1/47.211
 encapsulation dot1Q 211
 ip vrf forwarding Tenant 1
 ip address 10.60.3.255 255.255.255.254
!
interface TenGigabitEthernet0/1/0.213
description LAN interface of Tenant 2
 description Downlink to --- Port 1/47.213
 encapsulation dot1Q 213
 ip vrf forwarding Tenant 2
 ip address 10.60.3.251 255.255.255.254
!
router bgp 65530
 bgp router-id <removed>
 bgp log-neighbor-changes
 description BGP neighbor config and route advertisement for Tenant 1 VRF 
 address-family ipv4 vrf Tenant 1
  network 10.1.0.0 mask 255.255.0.0
  network 10.60.3.254 mask 255.255.255.254
  network 192.168.1.0 mask 255.255.255.252
  network 192.168.1.4 mask 255.255.255.252
  neighbor 10.10.0.62 remote-as 65100
  neighbor 10.10.0.62 description VPN-BGP-PEER-for-Tenant 1
  neighbor 10.10.0.62 ebgp-multihop 5
  neighbor 10.10.0.62 activate
  neighbor 10.60.3.254 remote-as 4232570301
  neighbor 10.60.3.254 description LAN peer for CPEC:INET:2112 VRF
  neighbor 10.60.3.254 activate
  neighbor 10.60.3.254 route-map BLOCK-ALL out
  neighbor 192.168.1.2 remote-as 12076
  neighbor 192.168.1.2 description PRIMARY ER peer for Tenant 1 to Azure
  neighbor 192.168.1.2 ebgp-multihop 5
  neighbor 192.168.1.2 activate
  neighbor 192.168.1.2 soft-reconfiguration inbound
  neighbor 192.168.1.2 route-map Tenant 1-ONLY out
  neighbor 192.168.1.6 remote-as 12076
  neighbor 192.168.1.6 description BACKUP ER peer for Tenant 1 to Azure
  neighbor 192.168.1.6 ebgp-multihop 5
  neighbor 192.168.1.6 activate
  neighbor 192.168.1.6 soft-reconfiguration inbound
  neighbor 192.168.1.6 route-map Tenant 1-ONLY out
  maximum-paths 8
 exit-address-family
 !
description BGP neighbor config and route advertisement for Tenant 2 VRF 
address-family ipv4 vrf Tenant 2
  network 10.1.0.0 mask 255.255.0.0
  network 10.60.3.250 mask 255.255.255.254
  network 192.168.1.16 mask 255.255.255.252
  network 192.168.1.20 mask 255.255.255.252
  neighbor 10.10.0.62 remote-as 65300
  neighbor 10.10.0.62 description VPN-BGP-PEER-for-Tenant 2
  neighbor 10.10.0.62 ebgp-multihop 5
  neighbor 10.10.0.62 activate
  neighbor 10.60.3.250 remote-as 4232570301
  neighbor 10.60.3.250 description LAN peer for CPEC:INET:2112 VRF
  neighbor 10.60.3.250 activate
  neighbor 10.60.3.250 route-map BLOCK-ALL out
  neighbor 192.168.1.18 remote-as 12076
  neighbor 192.168.1.18 description PRIMARY ER peer for Tenant 2 to Azure
  neighbor 192.168.1.18 ebgp-multihop 5
  neighbor 192.168.1.18 activate
  neighbor 192.168.1.18 soft-reconfiguration inbound
  neighbor 192.168.1.18 route-map VNET-ONLY out
  neighbor 192.168.1.22 remote-as 12076
  neighbor 192.168.1.22 description BACKUP ER peer for Tenant 2 to Azure
  neighbor 192.168.1.22 ebgp-multihop 5
  neighbor 192.168.1.22 activate
  neighbor 192.168.1.22 soft-reconfiguration inbound
  neighbor 192.168.1.22 route-map VNET-ONLY out
  maximum-paths 8
 exit-address-family
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$
ip route vrf Tenant 1 10.1.0.0 255.255.0.0 Tunnel10
ip route vrf Tenant 2 10.1.0.0 255.255.0.0 Tunnel20
!
ip prefix-list BLOCK-ALL seq 5 deny 0.0.0.0/0 le 32
!
route-map BLOCK-ALL permit 10
 match ip address prefix-list BLOCK-ALL
!
route-map VNET-ONLY permit 10
 match as-path 1
!
```

## <a name="test-the-connection"></a>Testen der Verbindung

Testen Sie Ihre Verbindung, nachdem Sie die Site-to-Site-Verbindung und die ExpressRoute-Verbindung hergestellt haben. Diese Aufgabe ist einfach.  Melden Sie sich an einem der virtuellen Computer an, die Sie in Ihrem Azure VNET erstellt haben, und senden Sie einen Ping an den virtuellen Computer, den Sie in der Azure Stack-Umgebung erstellt haben (bzw. umgekehrt). 

Um sicherzustellen, dass Sie den Datenverkehr über die Site-to-Site- und ExpressRoute-Verbindungen senden, müssen Sie die dedizierte IP-Adresse (DIP) des virtuellen Computers an beiden Enden pingen, und nicht die VIP-Adresse des virtuellen Computers. Hierfür müssen Sie also die Adresse am anderen Ende der Verbindung ermitteln und notieren.

### <a name="allow-icmp-in-through-the-firewall"></a>Zulassen von ICMP in eingehender Richtung über die Firewall
Standardmäßig lässt Windows Server 2016 keine ICMP-Pakete in eingehender Richtung durch die Firewall zu. Führen Sie also für jeden virtuellen Computer, den Sie im Test verwenden, das folgende Cmdlet in einem PowerShell-Fenster mit erhöhten Rechten aus:


   ```
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="ping-the-azure-stack-virtual-machine"></a>Pingen des virtuellen Azure Stack-Computers

1. Melden Sie sich mit einem Mandantenkonto am Azure Stack-Benutzerportal an.
2. Klicken Sie auf der linken Navigationsleiste auf **Virtual Machines**.
3. Suchen Sie nach dem zuvor erstellten virtuellen Computer, und klicken Sie darauf.
4. Klicken Sie im Abschnitt für den virtuellen Computer auf **Verbinden**.
5. Öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, und geben Sie **ipconfig /all** ein.
6. Suchen Sie in der Ausgabe nach der IPv4-Adresse, und notieren Sie sich die Adresse. Pingen Sie diese Adresse vom virtuellen Computer im Azure VNET aus. In der Beispielumgebung stammt die Adresse aus dem Subnetz 10.1.1.x/24. In Ihrer Umgebung ist dies unter Umständen eine andere Adresse. Sie sollte aber innerhalb des Subnetzes liegen, das Sie für das VNET-Subnetz des Mandanten erstellt haben.


### <a name="view-data-transfer-statistics"></a>Anzeigen der Datenübertragungsstatistik

Wenn Sie wissen möchten, wie viel Datenverkehr über Ihre Verbindung fließt, helfen Ihnen die entsprechenden Informationen im Azure Stack-Benutzerportal im Abschnitt „Verbindung“ weiter. Diese Informationen sind auch eine gute Möglichkeit, um zu überprüfen, ob der soeben gesendete Ping tatsächlich die VPN- und ExpressRoute-Verbindung durchlaufen hat.

1. Melden Sie sich mit Ihrem Mandantenkonto am Microsoft Azure Stack-Benutzerportal an.
2. Navigieren Sie zu der Ressourcengruppe, in der Ihr VPN Gateway erstellt wurde, und wählen Sie den Objekttyp **Verbindungen** aus.
3. Klicken Sie in der Liste auf die Verbindung **ConnectToAzure**.
4. Im Abschnitt **Verbindungen** wird die Statistik für **eingehende Daten** und **ausgehende Daten** angezeigt. Bei Ihnen sollten dort Werte angezeigt werden, die nicht Null sind.

   ![Eingehende Daten, ausgehende Daten](media/azure-stack-connect-expressroute/DataInDataOut.png)

## <a name="next-steps"></a>Nächste Schritte
[Deploy apps to Azure and Azure Stack (Bereitstellen von Apps in Azure und Azure Stack)](azure-stack-solution-pipeline.md)