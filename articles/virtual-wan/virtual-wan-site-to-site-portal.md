---
title: Verwenden von Azure Virtual WAN zum Erstellen einer Site-to-Site-Verbindung mit Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie Azure Virtual WAN verwenden, um eine Site-to-Site-VPN-Verbindung mit Azure zu erstellen.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 07/13/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: 1b61c335dec2c641862c08fd6f752d78b2ee5866
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056663"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan-preview"></a>Tutorial: Erstellen einer Site-to-Site-Verbindung per Azure Virtual WAN (Vorschauversion)

In diesem Tutorial wird beschrieben, wie Sie Virtual WAN zum Verbinden Ihrer Ressourcen in Azure über eine IPsec/IKE-VPN-Verbindung (IKEv2) nutzen. Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine extern zugängliche, öffentliche IP-Adresse zugewiesen ist. Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

> [!NOTE]
> Falls Sie über viele Sites verfügen, verwenden Sie normalerweise einen [Virtual WAN-Partner](https://aka.ms.virtualwan), um diese Konfiguration zu erstellen. Sie können diese Konfiguration aber auch selbst erstellen, wenn Sie mit Netzwerken vertraut sind und sich mit der Konfiguration Ihres eigenen VPN-Geräts auskennen.
>

![Virtual WAN-Diagramm](./media/virtual-wan-about/virtualwan.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines WAN
> * Erstellen einer Site
> * Erstellen eines Hubs
> * Verbinden eines Hubs mit einer Site
> * Verbinden eines VNET mit einem Hub
> * Herunterladen und Anwenden der VPN-Gerätekonfiguration
> * Anzeigen Ihrer Virtual WAN-Instanz
> * Anzeigen der Ressourcenintegrität
> * Überwachen einer Verbindung

> [!IMPORTANT]
> Azure Virtual WAN befindet sich derzeit in der verwalteten Vorschauphase (Public Preview). Für die Nutzung von Virtual WAN müssen Sie sich [für die Vorschauversion registrieren](#enroll).
>
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

* Stellen Sie sicher, dass Sie über ein kompatibles routenbasiertes VPN-Gerät (IKEv2-fähig) verfügen und dass das Gerät von einer Person konfiguriert werden kann. Wenn Sie mit einem Virtual WAN-Partner zusammenarbeiten, werden die Konfigurationseinstellungen automatisch erstellt, und Sie müssen sich keine Gedanken über die manuelle Konfiguration des Geräts machen.
* Vergewissern Sie sich, dass Sie über eine externe öffentliche IPv4-Adresse für Ihr VPN-Gerät verfügen. Diese IP-Adresse darf sich nicht hinter einer NAT befinden.
* Falls Sie bereits über ein virtuelles Netzwerk verfügen, mit dem Sie eine Verbindung herstellen möchten, sollten Sie Folgendes sicherstellen: Kein Subnetz Ihres lokalen Netzwerks darf sich mit den virtuellen Netzwerken überlappen, mit denen eine Verbindung hergestellt werden soll. Für Ihr virtuelles Netzwerk ist kein Gatewaysubnetz erforderlich, und es können keine Gateways für virtuelle Netzwerke verwendet werden. Falls Sie nicht über ein virtuelles Netzwerk verfügen, können Sie mit den Schritten in diesem Artikel ein Netzwerk dieser Art erstellen.
* Beschaffen Sie sich einen IP-Adressbereich für Ihre Hubregion. Der Hub ist ein virtuelles Netzwerk, und der von Ihnen für die Hubregion angegebene Adressbereich darf sich nicht mit einem Ihrer vorhandenen virtuellen Netzwerke überlappen, mit denen Sie eine Verbindung herstellen. Außerdem ist keine Überlappung mit Ihren Adressbereichen möglich, mit denen Sie lokal eine Verbindung herstellen. Falls Sie nicht mit den IP-Adressbereichen in Ihrer lokalen Netzwerkkonfiguration vertraut sind, wenden Sie sich an eine Person, die Ihnen diese Informationen zur Verfügung stellen kann.
* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="enroll"></a>1. Registrieren in der Vorschau

Bevor Sie Virtual WAN konfigurieren können, müssen Sie Ihr Abonnement zunächst für die Vorschauversion registrieren. Andernfalls können Sie Virtual WAN im Portal nicht verwenden. Senden Sie für die Registrierung eine E-Mail mit Ihrer Abonnement-ID an **azurevirtualwan@microsoft.com**. Sie erhalten eine E-Mail zurück, nachdem Ihr Abonnement registriert wurde.

## <a name="vnet"></a>2. Erstellen eines virtuellen Netzwerks

Falls Sie noch nicht über ein VNET verfügen, können Sie mit PowerShell schnell eins erstellen. Sie können auch über das Azure-Portal ein VNET erstellen.

* Stellen Sie sicher, dass sich der Adressraum für das von Ihnen erstellte VNET nicht mit den Adressbereichen anderer VNETs, mit denen Sie eine Verbindung herstellen möchten, oder mit Ihren Adressräumen des lokalen Netzwerks überlappt. 
* Wenn Sie bereits über ein VNET verfügen, sollten Sie sich vergewissern, dass es die erforderlichen Kriterien erfüllt und dass kein Gateway für virtuelle Netzwerke verwendet wird.

Sie können Ihr VNET leicht erstellen, indem Sie in diesem Artikel die Option zum Ausprobieren verwenden, um eine PowerShell-Konsole zu öffnen. Passen Sie die Werte an, und kopieren Sie die Befehle dann in das Konsolenfenster.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Passen Sie die PowerShell-Befehle an, und erstellen Sie anschließend eine Ressourcengruppe.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Erstellen eines VNET

Passen Sie die PowerShell-Befehle an, um das VNET zu erstellen, das für Ihre Umgebung kompatibel ist.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzureRmVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```

## <a name="wan"></a>3. Erstellen eines virtuellen WAN

1. Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.
2. Sie können auf Virtual WAN zugreifen, indem Sie zu **Alle Dienste** navigieren und nach Virtual WAN suchen. Sie können auch im Azure-Portal oben im Suchfeld nach Virtual WAN suchen. Klicken Sie auf **Virtual WAN**, um die Seite zu öffnen.
3. Klicken Sie auf **Erstellen**, um die Seite **WAN erstellen** zu öffnen. Wenn die Seite nicht verfügbar ist, haben Sie noch keine Genehmigung für diese Vorschauversion erhalten.

  ![Erstellen des WAN](./media/virtual-wan-site-to-site-portal/createwan.png)
4. Füllen Sie auf der Seite „WAN erstellen“ die unten angegebenen Felder aus.

  * **Name**: Wählen Sie den Namen aus, den Sie Ihrem WAN geben möchten.
  * **Abonnement**: Wählen Sie das Abonnement aus, das Sie verwenden möchten.
  * **Ressourcengruppe**: Erstellen Sie eine neue oder verwenden Sie eine vorhandene Ressourcengruppe.
  * **Ressourcenspeicherort**: Wählen Sie in der Dropdownliste einen Ressourcenspeicherort aus. Ein WAN ist eine globale Ressource, die nicht in einer bestimmten Region angeordnet ist. Sie müssen aber eine Region auswählen, damit Sie die von Ihnen erstellte WAN-Ressource leichter verwalten und finden können.
5. Klicken Sie auf **Erstellen**, um das WAN zu erstellen.

## <a name="site"></a>4. Erstellen einer Site

Erstellen Sie beliebig viele Sites, die Ihren physischen Standorten entsprechen. Erstellen Sie beispielsweise drei separate Sites, wenn Sie jeweils über eine Filiale in New York, London und Los Angeles verfügen. Diese Sites enthalten Ihre lokalen VPN-Geräteendpunkte. Derzeit können Sie für Ihre Site nur eine private Adresse angeben.

1. Navigieren Sie zu **Alle Ressourcen**.
2. Klicken Sie auf das virtuelle WAN, das Sie erstellt haben.
3. Klicken Sie oben auf der Seite auf **+Create site** (+Site erstellen), um die Seite **Create site** (Site erstellen) zu öffnen.

  ![Neue Site](media/virtual-wan-site-to-site-portal/createsite.png)
4. Füllen Sie auf der Seite **Create site** (Site erstellen) die folgenden Felder aus:

  *  **Name**: Dies ist der Name, den Sie Ihrer lokalen Site geben möchten.
  *  **Öffentliche IP-Adresse**: Dies ist die öffentliche IP-Adresse des VPN-Geräts, das sich in Ihrer lokalen Site befindet.
  *  **Privater Adressraum**: Dies ist der IP-Adressraum Ihrer lokalen Site. Der Datenverkehr, der für diesen Adressraum bestimmt ist, wird an Ihre lokale Site geleitet.
  *  **Abonnement**: Überprüfen Sie das Abonnement.
  *  **Ressourcengruppe**: Die Ressourcengruppe, die Sie verwenden möchten.
5. Klicken Sie auf **Show advanced** (Erweiterte anzeigen), um die zusätzlichen Einstellungen anzuzeigen. Sie können **BGP aktivieren** (optionales Feld) wählen, um diese Funktionalität für alle Verbindungen zu aktivieren, die für diese Site in Azure erstellt werden. Sie können auch unter **Geräteinformationen** (optionales Feld) Informationen eingeben. Das Azure-Team kann Ihre Umgebung so besser verstehen und in Zukunft weitere Optimierungsmöglichkeiten hinzufügen oder Sie bei der Problembehandlung unterstützen.

  ![BGP](media/virtual-wan-site-to-site-portal/sitebgp.png)
6. Klicken Sie auf **Bestätigen**, um die Site zu erstellen.
7. Wiederholen Sie diese Schritte für jede Site, die Sie erstellen möchten.

## <a name="hub"></a>5. Erstellen eines Hubs und Verbinden von Sites

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Sites**.
2. Unter **Unassociated sites** (Nicht zugeordnete Sites) wird eine Liste mit Sites angezeigt, die noch nicht mit einem Hub verbunden wurden.
3. Wählen Sie die Sites aus, die Sie zuordnen möchten.
4. Wählen Sie in der Dropdownliste die Region aus, der Ihr Hub zugeordnet werden soll. Sie sollten Ihren Hub der Region zuordnen, in der sich die VNETs befinden, mit denen eine Verbindung hergestellt werden soll.
5. Klicken Sie auf **Confirm** (Bestätigen). Falls sich in dieser Region noch kein Hub befindet, wird automatisch ein VNET mit virtuellem Hub erstellt. In diesem Fall wird die Seite **Regionale Hubs erstellen** angezeigt.
6. Geben Sie auf der Seite **Regionale Hubs erstellen** den Adressbereich für Ihr Hub-VNET ein. Dies ist das VNET, das Ihre Hubdienste enthält. Der hier eingegebene Bereich muss ein privater IP-Adressbereich sein und darf sich nicht mit Ihren lokalen Adressräumen oder Ihren VNET-Adressräumen überlappen. Im Hub-VNET wird ein nachfolgender VPN-Endpunkt erstellt. (Die automatische Erstellung von Hub und Gateway ist nur im Portal verfügbar.)
7. Klicken Sie auf **Create**.

## <a name="vnet"></a>6. Verbinden Ihres VNET mit einem Hub

In diesem Schritt erstellen Sie die Peeringverbindung zwischen Ihrem Hub und einem VNET. Wiederholen Sie diese Schritte für jedes VNET, mit dem Sie eine Verbindung herstellen möchten.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Virtual network connection** (VNET-Verbindung).
2. Klicken Sie auf der Seite für die VNET-Verbindung auf **+Add connection** (+Verbindung hinzufügen).
3. Füllen Sie auf der Seite **Add connection** (Verbindung hinzufügen) die folgenden Felder aus:

    * **Verbindungsname**: Dies ist der Name Ihrer Verbindung.
    * **Hubs**: Wählen Sie den Hub aus, den Sie dieser Verbindung zuordnen möchten.
    * **Abonnement**: Überprüfen Sie das Abonnement.
    * **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk aus, das Sie mit diesem Hub verbinden möchten. Für das virtuelle Netzwerk kann nicht bereits ein Gateway für virtuelle Netzwerke vorhanden sein.

## <a name="device"></a>7. Herunterladen der VPN-Konfiguration

Verwenden Sie die VPN-Gerätekonfiguration, um Ihr lokales VPN-Gerät zu konfigurieren.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Übersicht**.
2. Klicken Sie oben auf der Seite „Übersicht“ auf **VPN-Konfiguration herunterladen**. Azure erstellt ein Speicherkonto in der Ressourcengruppe „microsoft-network-[location]“, wobei „location“ für den WAN-Standort steht. Nachdem Sie die Konfiguration auf Ihre VPN-Geräte angewendet haben, können Sie dieses Speicherkonto löschen.
3. Nachdem die Erstellung der Datei abgeschlossen wurde, können Sie auf den Link klicken, um sie herunterzuladen.
4. Wenden Sie die Konfiguration auf Ihr VPN-Gerät an.

### <a name="understanding-the-vpn-device-configuration-file"></a>Grundlegendes zur Konfigurationsdatei für VPN-Geräte

Die Gerätekonfigurationsdatei enthält die Einstellungen, die beim Konfigurieren Ihrer lokalen VPN-Geräte verwendet werden. Beachten Sie beim Anzeigen dieser Datei die folgenden Informationen:

* **vpnSiteConfiguration**: In diesem Abschnitt sind die Gerätedetails für die Einrichtung einer Site angegeben, für die eine Verbindung mit dem virtuellen WAN hergestellt wird. Sie enthält den Namen und die öffentliche IP-Adresse des Branchgeräts.
* **vpnSiteConnections**: Dieser Abschnitt enthält die folgenden Informationen:

    * **Adressraum** des virtuellen Hub-VNET<br>Beispiel:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Adressraum** der VNETs, die mit dem Hub verbunden sind<br>Beispiel:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * **IP-Adressen** des „vpngateway“ für den virtuellen Hub. Da für das vpngateway jede Verbindung aus zwei Tunneln mit Aktiv-Aktiv-Konfiguration besteht, sind in dieser Datei beide IP-Adressen angegeben. In diesem Beispiel werden für jede Site „Instance0“ und „Instance1“ angezeigt.<br>Beispiel:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Details zur Konfiguration der vpngateway-Verbindung**, z.B. BGP, vorinstallierter Schlüssel usw. Der vorinstallierte Schlüssel (Pre-Shared Key, PSK) wird automatisch für Sie erstellt. Sie können die Verbindung für einen benutzerdefinierten PSK auf der Seite „Übersicht“ jederzeit bearbeiten.
  
### <a name="example-device-configuration-file"></a>Beispiel für Gerätekonfigurationsdatei

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.30.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

### <a name="configuring-your-vpn-device"></a>Konfigurieren Ihres VPN-Geräts

>[!NOTE]
> Wenn Sie mit einer Lösung eines Virtual WAN-Partners arbeiten, wird die VPN-Gerätekonfiguration automatisch durchgeführt. Der Gerätecontroller erhält die Konfigurationsdatei von Azure und wendet sie auf das Gerät an, um die Verbindung mit Azure einzurichten. Dies bedeutet, dass Sie nicht wissen müssen, wie Sie Ihr VPN-Gerät manuell konfigurieren.
>

Falls Sie eine Anleitung für die Konfiguration Ihres Geräts benötigen, können Sie die Anleitung auf der [Seite mit den Schritten für die VPN-Gerätekonfiguration](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) verwenden. Beachten Sie hierbei aber die folgenden Einschränkungen:

* Die Anleitung auf der Seite für die VPN-Geräte wurde nicht für Virtual WAN geschrieben, aber Sie können die Virtual WAN-Werte aus der Konfigurationsdatei verwenden, um Ihr VPN-Gerät manuell zu konfigurieren. 
* Die herunterladbaren Skripts für die Gerätekonfiguration, die für VPN Gateway bestimmt sind, funktionieren nicht für Virtual WAN, da sich die Konfiguration unterscheidet.
* Für Virtual WAN kann nur IKEv2 verwendet werden (nicht IKEv1).
* Für Virtual WAN können nur routenbasierte VPN-Geräte und die entsprechenden Geräteanweisungen verwendet werden.

## <a name="viewwan"></a>8. Anzeigen Ihrer Virtual WAN-Instanz

1. Navigieren Sie zum virtuellen WAN.
2. Auf der Seite „Übersicht“ steht jeder Punkt auf der Karte für einen Hub. Bewegen Sie den Mauszeiger jeweils auf einen dieser Punkte, um die Zusammenfassung zur Integrität des Hubs anzuzeigen.
3. Im Abschnitt mit den Hubs und Verbindungen können Sie den Hubstatus, die Site, die Region, den VPN-Verbindungsstatus und die Anzahl von ein- und ausgehenden Byte anzeigen.

## <a name="viewhealth"></a>9. Anzeigen der Ressourcenintegrität

1. Navigieren Sie zu Ihrem WAN.
2. Klicken Sie auf der Seite für Ihr WAN im Abschnitt **Support + Problembehandlung** auf **Integrität**, und zeigen Sie Ihre Ressource an.

## <a name="connectmon"></a>10. Überwachen einer Verbindung

Erstellen Sie eine Verbindung, um die Kommunikation zwischen einer Azure-VM und einem Remotestandort zu überwachen. Weitere Informationen zum Einrichten einer Verbindungsüberwachung finden Sie unter [Überwachen der Netzwerkkommunikation](~/articles/network-watcher/connection-monitor.md). Das Quellfeld ist die VM-IP in Azure, und die Ziel-IP ist die Site-IP.

## <a name="cleanup"></a>11. Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht mehr benötigen, können Sie den Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Ersetzen Sie „myResourceGroup“ durch den Namen Ihrer Ressourcengruppe, und führen Sie den folgenden PowerShell-Befehl aus:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="feedback"></a>Feedback zur Vorschauversion

Wir schätzen Ihr Feedback. Senden Sie eine E-Mail an <azurevirtualwan@microsoft.com>, um Probleme zu melden oder Feedback zu Virtual WAN zu geben (positiv oder negativ). Fügen Sie den Namen Ihres Unternehmens in eckigen Klammern in die Betreffzeile ein. Wenn Sie ein Problem melden, geben Sie auch Ihre Abonnement-ID an.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines WAN
> * Erstellen einer Site
> * Erstellen eines Hubs
> * Verbinden eines Hubs mit einer Site
> * Verbinden eines VNET mit einem Hub
> * Herunterladen und Anwenden der VPN-Gerätekonfiguration
> * Anzeigen Ihrer Virtual WAN-Instanz
> * Anzeigen der Ressourcenintegrität
> * Überwachen einer Verbindung

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).