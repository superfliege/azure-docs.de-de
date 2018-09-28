---
title: Konfigurieren von OpenVPN-Clients für Azure VPN Gateway | Microsoft-Dokumentation
description: Schritte zum Konfigurieren von OpenVPN-Clients für Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 141f29ea1c7ebdd8208dda6bc39a60f7873dab7d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977841"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway-preview"></a>Konfigurieren von OpenVPN-Clients für Azure VPN Gateway (Vorschau)

Dieser Artikel bietet Informationen zum Konfigurieren von OpenVPN-Clients.

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich, dass Sie die Schritte zum Konfigurieren von OpenVPN für Ihr VPN-Gateway abgeschlossen haben. Einzelheiten finden Sie unter [Konfigurieren von OpenVPN für Azure VPN Gateway](vpn-gateway-howto-openvpn.md).

## <a name="windows"></a>Windows-Clients

1. Laden Sie den OpenVPN-Client von der offiziellen [OpenVPN-Website](https://openvpn.net/index.php/open-source/downloads.html) herunter, und installieren Sie ihn.
2. Laden Sie das VPN-Profil für das Gateway herunter. Dies kann auf der Registerkarte „Point-to-Site-Konfiguration“ im Azure-Portal oder über „New-AzureRmVpnClientConfiguration“ in PowerShell erfolgen.
3. Entzippen Sie das Profil. Öffnen Sie dann in Editor im Ordner „OpenVPN“ die Konfigurationsdatei „vpnconfig.ovpn“.
4. Füllen Sie den Abschnitt „P2S client certificate“ mit dem öffentlichen Schlüssel für das P2S-Clientzertifikat in base64 aus. In einem Zertifikat im PEM-Format können Sie einfach die CER-Datei öffnen und den base64-Schlüssel zwischen den Zertifikatskopfzeilen herauskopieren. Hier erfahren Sie, wie Sie ein Zertifikat exportieren, um den codierten öffentlichen Schlüssel zu erhalten.
5. Füllen Sie den Abschnitt „Private key“ mit dem privaten Schlüssel für das P2S-Clientzertifikat in base64 aus. Informationen dazu, wie Sie den privaten Schlüssel extrahieren, finden Sie unter [Exportieren des Schlüssels](vpn-gateway-certificates-point-to-site.md#clientexport).
6. Ändern Sie keine anderen Felder. Verwenden Sie die ausgefüllte Konfiguration in der Clienteingabe, um eine Verbindung mit dem VPN herzustellen.
7. Kopieren Sie die Datei „vpnconfig.ovpn“ in den Ordner „C:\Program Files\OpenVPN\config“.
8. Klicken Sie mit rechten Maustaste auf das Symbol „OpenVPN“, und wählen Sie „Verbinden“ aus.

## <a name="mac"></a>Mac-Clients

1. Laden Sie einen OpenVPN-Client wie z.B. [Tunnelblick](https://tunnelblick.net/downloads.html) herunter, und installieren Sie ihn. 
2. Laden Sie das VPN-Profil für das Gateway herunter. Dies kann auf der Registerkarte „Point-to-Site-Konfiguration“ im Azure-Portal oder über „New-AzureRmVpnClientConfiguration“ in PowerShell erfolgen.
3. Entzippen Sie das Profil. Öffnen Sie dann in Editor im Ordner „OpenVPN“ die Konfigurationsdatei „vpnconfig.ovpn“.
4. Füllen Sie den Abschnitt „P2S client certificate“ mit dem öffentlichen Schlüssel für das P2S-Clientzertifikat in base64 aus. In einem Zertifikat im PEM-Format können Sie einfach die CER-Datei öffnen und den base64-Schlüssel zwischen den Zertifikatskopfzeilen herauskopieren. Unter [Exportieren des öffentlichen Schlüssels](vpn-gateway-certificates-point-to-site.md#cer) finden Sie Informationen dazu, wie Sie ein Zertifikat exportieren können, um den codierten öffentlichen Schlüssel zu erhalten.
5. Füllen Sie den Abschnitt „Private key“ mit dem privaten Schlüssel für das P2S-Clientzertifikat in base64 aus. Informationen dazu, wie Sie den privaten Schlüssel extrahieren, finden Sie unter [Exportieren des privaten Schlüssels](https://www.geotrust.eu/en/support/manuals/microsoft/all+windows+servers/export+private+key+or+certificate/).
6. Ändern Sie keine anderen Felder. Verwenden Sie die ausgefüllte Konfiguration in der Clienteingabe, um eine Verbindung mit dem VPN herzustellen.
7. Doppelklicken Sie auf die Profildatei, um das Profil in Tunnelblick zu erstellen.
8. Starten Sie Tunnelblick im Anwendungsordner.
9. Klicken Sie auf das Symbol „Tunnelblick“, und wählen Sie „Verbinden“ aus.

## <a name="linux"></a>Linux-Clients

1. Starten Sie eine neue Terminalsitzung. Sie können eine neue Sitzung öffnen, indem Sie STRG+ALT+T gleichzeitig drücken.
2. Geben Sie für die Installation der benötigten Komponenten den folgenden Befehl ein:

  ```
  sudo apt-get install openvpn
  sudo apt-get -y install network-manager-openvpn
  sudo service network-manager restart
  ```
3. Laden Sie das VPN-Profil für das Gateway herunter. Dies kann auf der Registerkarte „Point-to-Site-Konfiguration“ im Azure-Portal oder über „New-AzureRmVpnClientConfiguration“ in PowerShell erfolgen.
4. Füllen Sie den Abschnitt „Private key“ mit dem privaten Schlüssel für das P2S-Clientzertifikat in base64 aus. Informationen dazu, wie Sie den privaten Schlüssel extrahieren, finden Sie unter [Exportieren des privaten Schlüssels](https://www.geotrust.eu/en/support/manuals/microsoft/all+windows+servers/export+private+key+or+certificate/).
5. Um über die Befehlszeile eine Verbindung herzustellen, geben Sie Folgendes ein:
  
  ```
  Sudo openvpn –config <name and path of your VPN profile file>
  ```
5. Um über die grafische Benutzeroberfläche eine Verbindung herzustellen, wechseln Sie zu „Systemeinstellungen“.
6. Klicken Sie auf **+**, um eine neue VPN-Verbindung hinzuzufügen.
7. Wählen Sie unter **VPN hinzufügen**, den Befehl **Aus Datei importieren...** aus.
8. Navigieren Sie zur Profildatei, und doppelklicken auf **Öffnen**, oder wählen Sie diesen Befehl aus.
9. Klicken Sie im Fenster **VPN hinzufügen** auf **Hinzufügen**.
  
  ![Aus Datei importieren](./media/vpn-gateway-howto-openvpn-clients/importfromfile.png)
10. Sie können eine Verbindung herstellen, indem Sie auf der Seite **Netzwerkeinstellungen** oder unter dem Netzwerksymbol auf der Taskleiste das VPN auf **EIN** festlegen.

## <a name="next-steps"></a>Nächste Schritte

Wenn die VPN-Clients auf Ressourcen in einem anderen VNET (Produktion) zugreifen können sollen, folgen Sie den Anweisungen im Artikel [Konfigurieren einer VNET-zu-VNET-VPN-Gatewayverbindung über das Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), um eine VNET-zu-VNET-Verbindung einzurichten. Stellen Sie sicher, dass BGP für die Gateways und die Verbindungen aktiviert ist, da ansonsten kein Datenverkehr fließt.
