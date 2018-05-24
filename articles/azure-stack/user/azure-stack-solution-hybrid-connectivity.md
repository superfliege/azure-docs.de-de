---
title: Konfigurieren der Hybrid Cloud-Konnektivität mit Azure und Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Hybrid Cloud-Konnektivität mit Azure und Azure Stack konfigurieren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 048e2636aabe406728c8fe1b93ef861f13346256
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>Tutorial: Konfigurieren der Hybrid Cloud-Konnektivität mit Azure und Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Mithilfe des Hybridkonnektivitätsmusters können Sie auf Ressourcen mit Sicherheit in globalen Azure- und Azure Stack-Instanzen zugreifen. 

In diesem Tutorial erstellen Sie eine Beispielumgebung, die Folgendes ermöglicht:

> [!div class="checklist"]
> - Lokale Aufbewahrung von Daten zur Einhaltung datenschutzbezogener und rechtlicher Anforderungen bei gleichzeitigem Zugriff auf globale Azure-Ressourcen
> - Verwendung eines Legacysystems sowie einer über die Cloud skalierten App-Bereitstellung und entsprechender Ressourcen in globalen Azure-Instanzen

## <a name="prerequisites"></a>Voraussetzungen

Für eine Bereitstellung mit Hybridkonnektivität müssen einige Komponenten erstellt werden. Dies dauert ggf. etwas. 

Ein Azure-OEM/-Hardwarepartner kann eine Azure Stack-Produktionsumgebung bereitstellen. Ein ASDK kann von allen Benutzern bereitgestellt werden. Ein Azure Stack-Bediener muss auch App Service bereitstellen, Pläne und Angebote sowie ein Mandantenabonnement erstellen und das Windows Server 2016-Image hinzufügen. 

Wenn Sie bereits über einige dieser Komponenten verfügen, stellen Sie vor Beginn sicher, dass diese die Anforderungen erfüllen.

In diesem Thema wird davon ausgegangen, dass Sie bereits über Kenntnisse zu Azure und Azure Stack verfügen. Wenn Sie mehr darüber erfahren möchten, bevor Sie fortfahren, schauen Sie sich diese Themen an:
 - [Einführung in Azure](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure Stack Key Concepts (Wichtige Begriffe für Azure Stack)](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure
 - Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
 - Erstellen Sie eine [Web-App](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts#create-an-azure-web-app-using-the-portal) in Azure. Notieren Sie sich die neue URL der Web-App, da Sie diese später benötigen.

### <a name="azure-stack"></a>Azure Stack
 - Verwenden Sie Ihre Azure Stack-Produktionsumgebung, oder stellen Sie das hier verlinkte Azure Stack Development Kit bereit:
    - https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 
    - Der Installationsvorgang dauert in der Regel ein paar Stunden, also planen Sie entsprechend.
 - Stellen Sie PaaS-Dienste als [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) für Azure Stack bereit. 
 - [Erstellen Sie Pläne/Angebote](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) in Ihrer Azure Stack-Umgebung. 
 - [Erstellen Sie ein Mandantenabonnement](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) in Ihrer Azure Stack-Umgebung. 


### <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

 - Vergewissern Sie sich, dass Sie über eine externe öffentliche IPv4-Adresse für Ihr VPN-Gerät verfügen. Diese IP-Adresse darf sich nicht hinter einer NAT befinden.
 - Stellen Sie sicher, dass alle Ressourcen in der gleichen Region/am gleichen Standort bereitgestellt werden.

#### <a name="example-values"></a>Beispielwerte

In den Beispielen dieses Artikels werden die folgenden Werte verwendet. Sie können diese Werte zum Erstellen einer Testumgebung verwenden oder zum besseren Verständnis der Beispiele in diesem Artikel heranziehen. Weitere Informationen zu Gatewayeinstellungen im Allgemeinen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings).

Verbindungsangaben:
 - **VPN-Typ:** Routenbasiert
 - **Verbindungstyp:** Site-to-Site (IPsec)
 - **Gatewaytyp:** VPN
 - **Azure Verbindungsname:** Azure-Gateway-AzureStack-S2SGateway (automatisch vom Portal ausgefüllt)
 - **Azure Stack-Verbindungsname:** AzureStack-Gateway-Azure-S2SGateway (automatisch vom Portal ausgefüllt)
 - **Gemeinsam verwendeter Schlüssel:** beliebiger, mit VPN-Hardware kompatibler Schlüssel mit entsprechenden Werten auf beiden Seiten der Verbindung
 - **Abonnement:** beliebiges bevorzugtes Abonnement
 - **Ressourcengruppe:** Test-Infra

| Azure/Azure Stack-Verbindung | Name | Subnetz | IP-Adresse |
|-------------------------------------|---------------------------------------------|---------------------------------------|-----------------------------|
| Azure: VNet | ApplicationvNet<br>10.100.102.9/23 | ApplicationSubnet<br>10.100.102.0/24 |  |
|  |  | GatewaySubnet<br>10.100.103.0/24 |  |
| Azure Stack: VNet | ApplicationvNet<br>10.100.100.0/23 | ApplicationSubnet <br>10.100.100.0/24 |  |
|  |  | GatewaySubnet <br>10.100101.0/24 |  |
| Azure: Gateway für virtuelle Netzwerke | Azure-Gateway |  |  |
| Azure Stack: Gateway für virtuelle Netzwerke | AzureStack-Gateway |  |  |
| Azure: öffentliche IP-Adresse | Azure-GatewayPublicIP |  | Bei Erstellung bestimmt |
| Azure Stack: öffentliche IP-Adresse | AzureStack-GatewayPublicIP |  | Bei Erstellung bestimmt |
| Azure: lokales Netzwerkgateway | AzureStack-S2SGateway<br>   10.100.100.0/23 |  | Azure Stack: öffentlicher IP-Wert |
| Azure Stack: lokales Netzwerkgateway | Azure-S2SGateway<br>10.100.102.0/23 |  | Azure: öffentlicher IP-Wert |

## <a name="create-a-virtual-network-in-global-azure-and-azure-stack"></a>Erstellen eines virtuellen Netzwerks in globalen Azure- und Azure Stack-Instanzen

> [!Note]  
> Die IP-Adressen in den Adressräumen des VNets von Azure oder Azure Stack dürfen sich nicht überschneiden. 

Im Anschluss wird beschrieben, wie Sie über das Azure-Portal ein VNet mit dem Resource Manager-Bereitstellungsmodell erstellen. Verwenden Sie die [Beispielwerte](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values), wenn Sie diese Schritte für ein Tutorial ausführen. Wenn Sie diese Schritte nicht als Tutorial ausführen, achten Sie darauf, die Werte durch eigene Werte zu ersetzen. 

1. Navigieren Sie in einem Browser zum [Azure-Portal](http://portal.azure.com/) , und melden Sie sich mit Ihrem Azure-Konto an.
2. Klicken Sie auf **Ressource erstellen**. Geben Sie im Feld **Marketplace durchsuchen** die Zeichenfolge `virtual network` ein. Suchen Sie in der zurückgegebenen Liste nach **Virtuelles Netzwerk**, und öffnen Sie die Seite **Virtual Network**.
3. Wählen Sie unten auf der Seite „Virtual Network“ in der Liste **Bereitstellungsmodell auswählen** die Option **Resource Manager** aus, und klicken Sie anschließend auf **Erstellen**. Die Seite „Virtuelles Netzwerk erstellen“ wird geöffnet.
4. Konfigurieren Sie auf der Seite **Virtuelles Netzwerk erstellen** die VNET-Einstellungen. Beim Ausfüllen der Felder ändert sich das rote Ausrufezeichen in ein grünes Häkchen, sofern die in das Feld eingegebenen Zeichen zulässig sind.
5. Wiederholen Sie diese Schritte im **Mandantenportal** von Azure Stack.

## <a name="add-a-gateway-subnet"></a>Hinzufügen eines Gatewaysubnetzes

Bevor Sie das virtuelle Netzwerk mit einem Gateway verbinden, müssen Sie das Gatewaysubnetz für das virtuelle Netzwerk erstellen, mit dem Sie eine Verbindung herstellen möchten. Für die Gatewaydienste werden die im Gatewaysubnetz angegebenen IP-Adressen verwendet.

Navigieren Sie im [Portal](http://portal.azure.com/) zum virtuellen Resource Manager-Netzwerk, für das Sie ein virtuelles Netzwerkgateway erstellen möchten.

1. Klicken Sie auf der Seite Ihres VNets im Abschnitt **Einstellungen** auf **Subnetze**, um die Seite **Subnetze** zu erweitern.
2. Klicken Sie auf der Seite **Subnetze** auf **+Gatewaysubnetz**, um die Seite **Subnetz hinzufügen** zu öffnen.

    ![](media/azure-stack-solution-hybrid-connectivity/image4.png)

3. Als **Name** für Ihr Subnetz wird automatisch der Wert „GatewaySubnet“ eingefügt. Dieser Wert ist erforderlich, damit Azure das Subnetz als Gatewaysubnetz erkennt. Passen Sie die automatisch ausgefüllten Werte für **Adressbereich** gemäß Ihren Konfigurationsanforderungen an, und klicken Sie unten auf der Seite auf **OK**, um das Subnetz zu erstellen.

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>Erstellen eines Gateways für virtuelle Netzwerke in Azure und Azure Stack

1. Klicken Sie im Portal auf der linken Seite auf „+“, und geben Sie als Suchbegriff „Gateway für virtuelle Netzwerke“ ein. Wählen unter **Ergebnisse** die Option **Gateway für virtuelle Netzwerke** aus.
2. Klicken Sie unten auf der Seite **Gateway für virtuelle Netzwerke** auf **Erstellen**. Die Seite **Gateway für virtuelle Netzwerke erstellen** wird geöffnet.
3. Geben Sie auf der Seite **Gateway für virtuelle Netzwerke erstellen** die in den Beispielwerten angegebenen Werte für Ihr Gateway für virtuelle Netzwerke sowie die folgenden zusätzlichen Werte an:
    - **SKU:** Basic
    - **Virtuelles Netzwerk:** Wählen Sie das zuvor erstellte virtuelle Netzwerk aus. Das zuvor erstellte Gatewaysubnetz wird automatisch ausgewählt. 
    - **Erste IP-Konfiguration:** Die öffentliche IP-Adresse Ihres Gateways. 
        - Klicken Sie auf **Gateway-IP-Konfiguration erstellen**, um zur Seite **Öffentliche IP-Adresse auswählen** zu gelangen.
        - Klicken Sie auf **+Neu erstellen**, um die Seite **Öffentliche IP-Adresse erstellen** zu öffnen.
        - Geben Sie unter **Name** einen Namen für die öffentliche IP-Adresse ein. Behalten Sie für die SKU den Wert **Basic** bei, und klicken Sie anschließend unten auf der Seite auf **OK**, um Ihre Änderungen zu speichern.

    > [!Note]  
    > VPN Gateway unterstützt derzeit nur die dynamische Zuweisung öffentlicher IP-Adressen. Das bedeutet jedoch nicht, dass sich die IP-Adresse ändert, nachdem sie Ihrem VPN-Gateway zugewiesen wurde. Die öffentliche IP-Adresse ändert sich nur, wenn das Gateway gelöscht und neu erstellt wird. Sie ändert sich nicht, wenn die Größe geändert wird, das VPN-Gateway zurückgesetzt wird oder andere interne Wartungs-/Upgradevorgänge für das VPN-Gateway durchgeführt werden.

4. Überprüfen Sie die Einstellungen. 
5. Klicken Sie auf **Erstellen**, um das VPN-Gateway zu erstellen. Die Einstellungen werden überprüft, und auf dem Dashboard wird die Kachel mit dem Hinweis angezeigt, dass das Gateway des virtuellen Netzwerks bereitgestellt wird. Die Erstellung eines Gateways kann bis zu 45 Minuten dauern. Unter Umständen müssen Sie die Portalseite aktualisieren, um den Status „Abgeschlossen“ anzuzeigen.

    Zeigen Sie nach der Erstellung des Gateways unter dem virtuellen Netzwerk im Portal die zugewiesene IP-Adresse an. Das Gateway wird als verbundenes Gerät angezeigt. Wenn Sie auf das verbundene Gerät (Ihr Gateway für virtuelle Netzwerke) klicken, werden weitere Informationen angezeigt.
6. Wiederholen Sie diese Schritte für Ihre Azure Stack-Bereitstellung.

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>Erstellen des lokalen Netzwerkgateways in Azure und Azure Stack

Mit dem Gateway des lokalen Netzwerks ist normalerweise Ihr lokaler Standort gemeint. Sie geben dem Standort einen Namen, über den Azure oder Azure Stack darauf verweisen kann, und geben dann die IP-Adresse des lokalen VPN-Geräts an, mit dem Sie eine Verbindung herstellen. Außerdem geben Sie die IP-Adresspräfixe an, die über das VPN-Gateway an das VPN-Gerät weitergeleitet werden. Die von Ihnen angegebenen Adresspräfixe befinden sich in Ihrem lokalen Netzwerk. Wenn bei dem lokalen Netzwerk Änderungen vorgenommen werden oder Sie die öffentliche IP-Adresse des VPN-Geräts ändern müssen, können Sie dies Werte später bequem aktualisieren.

1. Klicken Sie im Portal auf **+Ressource erstellen**.
2. Geben Sie im Suchfeld den Text **Lokales Netzwerkgateway** ein, und drücken **Sie die**EINGABETASTE. Daraufhin wird eine Liste mit Ergebnissen zurückgegeben. Klicken Sie auf **Lokales Netzwerkgateway** und anschließend auf die Schaltfläche **Erstellen**, um die Seite **Lokales Netzwerkgateway erstellen** zu öffnen.
3. Geben Sie auf der Seite **Lokales Netzwerkgateway erstellen** die in den Beispielwerten angegebenen Werte für Ihr lokales Netzwerkgateway sowie die folgenden zusätzlichen Werte an:
    - **IP-Adresse:** Die öffentliche IP-Adresse des VPN-Geräts, mit dem Azure oder Azure Stack eine Verbindung herstellen soll. Geben Sie eine gültige öffentliche IP-Adresse ein. Die IP-Adresse darf sich nicht hinter einer NAT befinden und muss für Azure erreichbar sein. Falls Sie die IP-Adresse gerade nicht zur Hand haben, können Sie die Werte aus dem Beispiel verwenden. In diesem Fall müssen Sie allerdings später die Platzhalter-IP-Adresse durch die öffentliche IP-Adresse Ihres VPN-Geräts ersetzen. Andernfalls kann Azure keine Verbindung herstellen.
    - **Adressraum** bezieht sich auf die Adressbereiche für das Netzwerk, das dieses lokale Netzwerk darstellt. Sie können mehrere Adressraumbereiche hinzufügen. Achten Sie darauf, dass sich die hier angegebenen Bereiche nicht mit den Bereichen anderer Netzwerke überschneiden, mit denen Sie eine Verbindung herstellen möchten. Azure leitet den Adressbereich, den Sie angeben, an die lokale IP-Adresse des VPN-Geräts weiter. Verwenden Sie hier anstelle der Werte aus dem Beispiel Ihre eigenen Werte, wenn Sie eine Verbindung mit Ihrem lokalen Standort herstellen möchten.
    - **BGP-Einstellungen konfigurieren:** Aktivieren Sie diese Option nur, wenn Sie BGP konfigurieren. Lassen Sie sie andernfalls deaktiviert.
    - **Abonnement:** Vergewissern Sie sich, dass das richtige Abonnement angezeigt wird.
    - **Ressourcengruppe:** Wählen Sie die gewünschte Ressourcengruppe aus. Sie können entweder eine neue Ressourcengruppe erstellen oder eine auswählen, die Sie bereits erstellt haben.
    - **Standort:** Wählen Sie den Standort aus, an dem dieses Objekt erstellt wird. Es empfiehlt sich unter Umständen, den gleichen Ort auszuwählen, an dem sich auch Ihr VNet befindet, dies ist aber nicht zwingend erforderlich.
4. Wenn Sie die Werte angegeben haben, klicken Sie unten auf der Seite auf die Schaltfläche **Erstellen**, um das lokale Netzwerkgateway zu erstellen.
5. Wiederholen Sie diese Schritte für Ihre Azure Stack-Bereitstellung.

## <a name="configure-your-connection"></a>Konfigurieren der Verbindung

Für Site-to-Site-Verbindungen mit einem lokalen Netzwerk ist ein VPN-Gerät erforderlich. In diesem Schritt konfigurieren Sie Ihr VPN-Gerät (eine so genannte Verbindung). Zum Konfigurieren Ihrer Verbindung benötigen Sie Folgendes:
    - Einen gemeinsam verwendeten Schlüssel. Dies ist derselbe gemeinsame Schlüssel, den Sie beim Erstellen Ihrer Site-to-Site-VPN-Verbindung angeben. In unseren Beispielen verwenden wir einen einfachen gemeinsamen Schlüssel. Es wird empfohlen, einen komplexeren Schlüssel zu generieren.
    - Die öffentliche IP-Adresse Ihres Gateways für virtuelle Netzwerke. Sie können die öffentliche IP-Adresse mit dem Azure-Portal, mit PowerShell oder mit der CLI anzeigen. Die öffentliche IP-Adresse Ihres VPN-Gateways können Sie über das Azure-Portal ermitteln, indem Sie zu „Gateways für virtuelle Netzwerke“ navigieren und auf den Namen Ihres Gateways klicken.
Erstellen Sie die Site-to-Site-VPN-Verbindung zwischen dem Gateway Ihres virtuellen Netzwerks und Ihrem lokalen VPN-Gerät.
1. Klicken Sie im Portal auf **+Ressource erstellen**.
2. Geben Sie im Suchfeld den Text **Verbindungen** ein, und drücken **Sie die**EINGABETASTE. Daraufhin wird eine Liste mit Ergebnissen zurückgegeben. Klicken Sie auf **Verbindungen** und anschließend auf die Schaltfläche „Erstellen“, um die Seite **Verbindungen erstellen** zu öffnen.
3. Konfigurieren Sie auf der Seite **Verbindungen hinzufügen** die Werte für Ihre Verbindung.
     - Grundlegendes:
        1. **Verbindungstyp:** Wählen Sie „Site-to-Site (IPsec)“ aus.
        2. **Ressourcengruppe:** Wählen Sie Ihre Testressourcengruppe aus.
     - Einstellungen:
        1. **Gateway für virtuelle Netzwerke:** Wählen Sie das zuvor erstellte Gateway für virtuelle Netzwerke aus.
        2. **Lokales Netzwerkgateway:** Wählen Sie das zuvor erstellte lokale Netzwerkgateway aus. 
        3. **Verbindungsname:** Dieser Wert wird automatisch mit den Werten der beiden Gateways ausgefüllt. 
        4. **Gemeinsam verwendeter Schlüssel:** Dieser Wert muss dem Wert entsprechen, den Sie für Ihr lokales VPN-Gerät verwenden. Im Beispiel wird „abc123“ verwendet. Sie können (und sollten) allerdings einen komplexeren Wert verwenden. Entscheidend ist Folgendes: Der Wert, den Sie hier angeben, muss dem Wert entsprechen, den Sie beim Konfigurieren Ihres VPN-Geräts angeben.
    - Die verbleibenden Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** wurden korrigiert.
4. Klicken Sie auf **OK** , um die Verbindung zu erstellen. Auf dem Bildschirm blinkt der Hinweis, dass die Verbindung erstellt wird.
5. Die Verbindung wird auf der Seite **Verbindungen** des Gateways für virtuelle Netzwerke angezeigt. Der Status wechselt von **Unbekannt** zu **Verbindung wird hergestellt** und dann zu **Erfolgreich**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure-Cloudmustern finden Sie unter [Cloudentwurfsmuster](https://docs.microsoft.com/azure/architecture/patterns).
