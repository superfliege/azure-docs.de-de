---
title: Verwenden von Azure Virtual WAN zum Erstellen einer Point-to-Site-Verbindung mit Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie Azure Virtual WAN verwenden, um eine Point-to-Site-VPN-Verbindung mit Azure zu erstellen.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 09/21/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect remote users to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: bf0e766f082b2e137c90b5ea66bb7570bea2e1e6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963371"
---
# <a name="tutorial-create-a-point-to-site-connection-using-azure-virtual-wan-preview"></a>Tutorial: Erstellen einer Point-to-Site-Verbindung per Azure Virtual WAN (Vorschauversion)

In diesem Tutorial wird beschrieben, wie Sie Virtual WAN zum Verbinden Ihrer Ressourcen in Azure über eine IPsec/IKE (IKEv2)- oder OpenVPN-VPN-Verbindung nutzen. Für diese Art von Verbindung muss auf dem Clientcomputer ein Client konfiguriert sein. Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines WAN
> * Erstellen einer P2S-Konfiguration
> * Erstellen eines Hubs
> * Anwenden einer P2S-Konfiguration auf einen Hub
> * Verbinden eines VNET mit einem Hub
> * Herunterladen und Anwenden der VPN-Clientkonfiguration
> * Anzeigen Ihrer Virtual WAN-Instanz
> * Anzeigen der Ressourcenintegrität
> * Überwachen einer Verbindung

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]


## <a name="vnet"></a>1. Erstellen eines virtuellen Netzwerks

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="openvwan"></a>2. Erstellen eines virtuellen WAN

Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="hub"></a>3. Erstellen eines Hubs

> [!NOTE]
> Wählen Sie in diesem Schritt nicht die Einstellung „Point-to-Site-Gateway einschließen“ aus.
>

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="site"></a>4. Erstellen einer P2S-Konfiguration

Eine P2S-Konfiguration definiert die Parameter für das Herstellen der Verbindung mit Remoteclients.

1. Navigieren Sie zu **Alle Ressourcen**.
2. Klicken Sie auf das virtuelle WAN, das Sie erstellt haben.
3. Klicken Sie unter **Virtual WAN Architecture** (Virtual WAN-Architektur) auf **Point-to-Site-Konfigurationen**.
4. Klicken Sie oben auf der Seite auf **+ Point-to-Site-Konfiguration hinzufügen**, um die Seite **Neue Point-to-Site-Konfiguration erstellen** zu öffnen.
5. Füllen Sie auf der Seite **Neue Point-to-Site-Konfiguration erstellen** die folgenden Felder aus:

  *  **Konfigurationsname**: Dies ist der Name, unter dem Sie auf Ihre Konfiguration verweisen möchten.
  *  **Tunneltyp**: Das Protokoll, das für den Tunnel verwendet wird.
  *  **Adresspool**: Dies ist der IP-Adressenpool, aus dem Clients IDs zugewiesen werden.
  *  **Name des Stammzertifikats**: Ein beschreibender Name für das Zertifikat.
  *  **Root Certificate Data** (Daten des Stammzertifikats): X.509-Zertifikatdaten mit Base64-Codierung.

5. Klicken Sie auf **Erstellen**, um die Konfiguration zu erstellen.

## <a name="hub"></a>5. Bearbeiten der Hub-Zuweisung

1. Klicken Sie auf der Seite für Ihr Virtual WAN auf **Point-to-Site-Konfigurationen**.
2. Unter **Hub** wird eine Liste mit Konfigurationen angezeigt, für die noch keine Verbindung mit einem Hub erstellt wurde.
3. Wählen Sie die Konfiguration aus, die Sie zuordnen möchten, und klicken Sie auf **Edit hub assignment** (Hub-Zuweisung bearbeiten).
4. Wählen Sie in der Dropdownliste den bzw. die Hubs aus, denen Sie die Konfiguration zuordnen möchten.
5. Klicken Sie auf **Zuweisen**. 
6. Es kann bis zu 30 Minuten dauern, bis der Vorgang abgeschlossen ist.

## <a name="vnet"></a>6. Verbinden Ihres VNET mit einem Hub

In diesem Schritt erstellen Sie die Peeringverbindung zwischen Ihrem Hub und einem VNET. Wiederholen Sie diese Schritte für jedes VNET, mit dem Sie eine Verbindung herstellen möchten.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Virtual network connection** (VNET-Verbindung).
2. Klicken Sie auf der Seite für die VNET-Verbindung auf **+Add connection** (+Verbindung hinzufügen).
3. Füllen Sie auf der Seite **Add connection** (Verbindung hinzufügen) die folgenden Felder aus:

    * **Verbindungsname**: Dies ist der Name Ihrer Verbindung.
    * **Hubs**: Wählen Sie den Hub aus, den Sie dieser Verbindung zuordnen möchten.
    * **Abonnement**: Überprüfen Sie das Abonnement.
    * **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk aus, das Sie mit diesem Hub verbinden möchten. Für das virtuelle Netzwerk kann nicht bereits ein Gateway für virtuelle Netzwerke vorhanden sein.

## <a name="device"></a>7. Herunterladen des VPN-Profils

Verwenden Sie das VPN-Profil, um Ihre Clients zu konfigurieren.

1. Klicken Sie auf der Seite für Ihr Virtual WAN auf **Point-to-Site-Konfigurationen**.
2. Klicken Sie oben auf der Seite auf **Point-to-Site-Profil herunterladen**. 
3. Nachdem die Erstellung der Datei abgeschlossen wurde, können Sie auf den Link klicken, um sie herunterzuladen.
4. Verwenden Sie die Profildatei, um die Point-to-Site-Clients zu konfigurieren.

## <a name="device"></a>8. Konfigurieren von Point-to-Site-Clients
Verwenden Sie das heruntergeladene Profil, um die Clients für den Remotezugriff zu konfigurieren. Das Verfahren ist für jedes Betriebssystem anders. Befolgen Sie jeweils die unten angegebene Anleitung:

### <a name="microsoft-windows"></a>Microsoft Windows
#### <a name="openvpn"></a>OpenVPN

1.  Laden Sie den OpenVPN-Client von der offiziellen Website herunter, und installieren Sie ihn.
2.  Laden Sie das VPN-Profil für das Gateway herunter. Dies kann auf der Registerkarte „Point-to-Site-Konfiguration“ im Azure-Portal oder über „New-AzureRmVpnClientConfiguration“ in PowerShell erfolgen.
3.  Entzippen Sie das Profil. Öffnen Sie dann im Editor im Ordner „OpenVPN“ die Konfigurationsdatei „vpnconfig.ovpn“.
4.  Füllen Sie den Abschnitt „P2S client certificate“ mit dem öffentlichen Schlüssel für das P2S-Clientzertifikat in Base64 aus. In einem Zertifikat im PEM-Format können Sie einfach die CER-Datei öffnen und den Base64-Schlüssel zwischen den Zertifikatskopfzeilen herauskopieren. Hier erfahren Sie, wie Sie ein Zertifikat exportieren, um den codierten öffentlichen Schlüssel zu erhalten.
5.  Füllen Sie den Abschnitt „Private key“ mit dem privaten Schlüssel für das P2S-Clientzertifikat in Base64 aus. Hier ist angegeben, wie Sie den privaten Schlüssel extrahieren.
6.  Ändern Sie keine anderen Felder. Verwenden Sie die ausgefüllte Konfiguration in der Clienteingabe, um eine Verbindung mit dem VPN herzustellen.
7.  Kopieren Sie die Datei „vpnconfig.ovpn“ in den Ordner „C:\Programme\OpenVPN\config“.
8.  Klicken Sie mit rechten Maustaste auf das Symbol „OpenVPN“, und klicken Sie auf „Verbinden“.

#### <a name="ikev2"></a>IKEv2

1. Wählen Sie die VPN-Clientkonfigurationsdateien, die der Architektur des Windows-Computers entsprechen. Wählen Sie für eine 64-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupAmd64“ aus. Wählen Sie für eine 32-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupX86“ aus.
2. Doppelklicken Sie auf das Paket, um es zu installieren. Sollte ein SmartScreen-Popup erscheinen, klicken Sie auf „Weitere Informationen“ und anschließend auf „Trotzdem ausführen“.
3. Navigieren Sie auf dem Clientcomputer zu „Netzwerkeinstellungen“, und klicken Sie auf „VPN“. Die VPN-Verbindung zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird.
4. Bevor Sie versuchen, eine Verbindung herzustellen, überprüfen Sie, ob Sie auf dem Clientcomputer ein Clientzertifikat installiert haben. Ein Clientzertifikat ist für die Authentifizierung erforderlich, wenn Sie den Typ „native Azure-Zertifikatauthentifizierung“ verwenden. Weitere Informationen zum Generieren von Zertifikaten finden Sie unter „Generieren von Zertifikaten“. Weitere Informationen zum Installieren eines Clientzertifikats finden Sie unter „Installieren eines Clientzertifikats“.

### <a name="mac-os-x"></a>Mac (OS X)
#### <a name="openvpn"></a>OpenVPN

1.  Laden Sie einen OpenVPN-Client herunter, z.B. Tunnelblick von https://tunnelblick.net/downloads.html, und installieren Sie ihn. 
2.  Laden Sie das VPN-Profil für das Gateway herunter. Dies kann auf der Registerkarte „Point-to-Site-Konfiguration“ im Azure-Portal oder über „New-AzureRmVpnClientConfiguration“ in PowerShell erfolgen.
3.  Entzippen Sie das Profil. Öffnen Sie dann im Editor im Ordner „OpenVPN“ die Konfigurationsdatei „vpnconfig.ovpn“.
4.  Füllen Sie den Abschnitt „P2S client certificate“ mit dem öffentlichen Schlüssel für das P2S-Clientzertifikat in Base64 aus. In einem Zertifikat im PEM-Format können Sie einfach die CER-Datei öffnen und den Base64-Schlüssel zwischen den Zertifikatskopfzeilen herauskopieren. Hier erfahren Sie, wie Sie ein Zertifikat exportieren, um den codierten öffentlichen Schlüssel zu erhalten.
5.  Füllen Sie den Abschnitt „Private key“ mit dem privaten Schlüssel für das P2S-Clientzertifikat in Base64 aus. Hier ist angegeben, wie Sie den privaten Schlüssel extrahieren.
6.  Ändern Sie keine anderen Felder. Verwenden Sie die ausgefüllte Konfiguration in der Clienteingabe, um eine Verbindung mit dem VPN herzustellen.
7.  Doppelklicken Sie auf die Profildatei, um das Profil in Tunnelblick zu erstellen.
8.  Starten Sie Tunnelblick im Anwendungsordner.
9.  Klicken Sie in der Taskleiste auf das Symbol „Tunnelblick“, und wählen Sie „Verbinden“.

#### <a name="ikev2"></a>IKEv2

Azure bietet für die native Azure-Zertifikatauthentifizierung nicht die MOBILECONFIG-Datei. Sie müssen den IKEv2-VPN-Client auf jedem Mac, der sich mit Azure verbindet, manuell konfigurieren. Der Ordner „Allgemein“ enthält alle Informationen, die zum Konfigurieren des Clients erforderlich sind. Enthält der Download nicht den Ordner „Allgemein“, wurde als Tunneltyp wahrscheinlich nicht IKEv2 ausgewählt. Wählen Sie IKEv2 aus, und generieren Sie anschließend die ZIP-Datei erneut, um den Ordner „Allgemein“ abzurufen. Der Ordner „Allgemein“ enthält die folgenden Dateien:

- VpnSettings.xml: Diese Datei enthält wichtige Einstellungen wie Serveradresse und Tunneltyp.
- VpnServerRoot.cer: Diese Datei enthält das Stammzertifikat, das zum Überprüfen des Azure-VPN-Gateways während der P2S-Verbindungseinrichtung erforderlich ist.

## <a name="viewwan"></a>9. Anzeigen Ihrer Virtual WAN-Instanz

1. Navigieren Sie zum virtuellen WAN.
2. Auf der Seite „Übersicht“ steht jeder Punkt auf der Karte für einen Hub. Bewegen Sie den Mauszeiger jeweils auf einen dieser Punkte, um die Zusammenfassung zur Integrität des Hubs anzuzeigen.
3. Im Abschnitt mit den Hubs und Verbindungen können Sie den Hubstatus, die Site, die Region, den VPN-Verbindungsstatus und die Anzahl von ein- und ausgehenden Byte anzeigen.

## <a name="viewhealth"></a>10. Anzeigen der Ressourcenintegrität

1. Navigieren Sie zu Ihrem WAN.
2. Klicken Sie auf der Seite für Ihr WAN im Abschnitt **Support + Problembehandlung** auf **Integrität**, und zeigen Sie Ihre Ressource an.

## <a name="connectmon"></a>11. Überwachen einer Verbindung

Erstellen Sie eine Verbindung, um die Kommunikation zwischen einer Azure-VM und einem Remotestandort zu überwachen. Weitere Informationen zum Einrichten einer Verbindungsüberwachung finden Sie unter [Überwachen der Netzwerkkommunikation](~/articles/network-watcher/connection-monitor.md). Das Quellfeld ist die VM-IP in Azure, und die Ziel-IP ist die Site-IP.

## <a name="cleanup"></a>12. Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht mehr benötigen, können Sie den Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Ersetzen Sie „myResourceGroup“ durch den Namen Ihrer Ressourcengruppe, und führen Sie den folgenden PowerShell-Befehl aus:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

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