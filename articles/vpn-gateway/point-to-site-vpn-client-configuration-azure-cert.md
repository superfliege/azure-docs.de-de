---
title: "Erstellen und Installieren von P2S-VPN-Clientkonfigurationsdateien für die Azure-Zertifikatauthentifizierung: PowerShell: Azure | Microsoft-Dokumentation"
description: "Dieser Artikel unterstützt Sie beim Erstellen und Installieren einer VPN-Clientkonfigurationsdatei für P2S-Verbindungen (Point-to-Site), die die Zertifikatauthentifizierung nutzen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 95f14f2b77565b53c6e270f6afbf9873cdac606a
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-p2s-configurations"></a>Erstellen und Installieren von VPN-Clientkonfigurationsdateien für P2S-Konfigurationen mit nativer Azure-Zertifikatauthentifizierung

VPN-Clientkonfigurationsdateien sind in einer ZIP-Datei enthalten. Konfigurationsdateien enthalten die Einstellungen, die zum Herstellen von P2S-Verbindungen mit nativer Azure-Zertifikatauthentifizierung zwischen einem nativen Windows- oder Mac-IKEv2-VPN-Client und einem VNET erforderlich sind.

>[!NOTE]
>IKEv2 für P2S befindet sich derzeit in der Vorschauversion.
>

### <a name="workflow"></a>P2S-Workflow

  1. Richten Sie das Azure-VPN-Gateway für eine P2S-Verbindung ein.
  2. Erstellen Sie das Stammzertifikat und Clientzertifikate. Laden Sie den öffentlichen Schlüssel des Stammzertifikats in Azure hoch, und installieren Sie die Clientzertifikate auf den Clientgeräten. Die Zertifikate dienen zum Authentifizieren der Benutzer, die eine Verbindung herstellen.
  3. Verwenden Sie die VPN-Clientkonfiguration für die gewünschte Authentifizierungsoption, um den VPN-Client auf dem Windows- oder Mac-Gerät einzurichten. Dadurch können Sie eine P2S-Verbindung mit Azure-VNETs herstellen.

>[!NOTE]
>Clientkonfigurationsdateien gelten spezifisch für die VPN-Konfiguration für das VNET. Wenn nach der Erstellung der VPN-Clientkonfigurationsdateien Änderungen an der P2S-VPN-Konfiguration (beispielsweise am VPN-Protokolltyp oder -Authentifizierungstyp) vorgenommen werden, müssen Sie neue VPN-Clientkonfigurationsdateien für die Benutzergeräte erstellen.
>
>

## <a name="generate"></a>Generieren der VPN-Clientkonfigurationsdateien

Stellen Sie zunächst sicher, dass für alle Benutzer, die eine Verbindung herstellen, ein gültiges Zertifikat auf den Benutzergeräten installiert ist. Weitere Informationen zum Installieren eines Clientzertifikats finden Sie unter [Install a client certificate for Point-to-Site Azure certificate authentication connections](point-to-site-how-to-vpn-client-install-azure-cert.md) (Installieren eines Clientzertifikats für Verbindungen für die P2S-Azure-Zertifikatauthentifizierung).

1. Beim Generieren von VPN-Clientkonfigurationsdateien ist für „-AuthenticationMethod“ der Wert „EapTls“ angegeben. Erstellen Sie die VPN-Clientkonfigurationsdateien mit dem folgenden Befehl:

  ```powershell
  New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -VirtualNetworkGatewayName "VNet1GW" -AuthenticationMethod "EapTls"
  ```
2. Der vorherige Befehl gibt einen Link zurück, über den Sie die Clientkonfigurationsdateien herunterladen. Kopieren Sie den Link, und fügen Sie ihn in einen Webbrowser ein, um die Datei „VpnClientConfiguration.zip“ herunterzuladen. Entzippen Sie die Datei, um die folgenden Ordner anzuzeigen:

  * **WindowsAmd64** und **WindowsX86**. Diese Ordner enthalten das Windows-32-Bit- bzw. das 64-Bit-Installer-Paket. Das Installer-Paket **WindowsAmd64** gilt nicht nur für Amd, sondern für alle unterstützten 64-Bit-Windows-Clients.
  * **Allgemein**: Dieser Ordner enthält allgemeine Informationen zum Erstellen Ihrer eigenen VPN-Clientkonfiguration. Ignorieren Sie diesen Ordner. Der Ordner „Allgemein“ wird nur bereitgestellt, wenn für das Gateway IKEv2 oder SSTP und IKEv2 konfiguriert wurde. Wenn nur SSTP konfiguriert ist, ist der Ordner „Allgemein“ nicht vorhanden.

### <a name="to-retrieve-client-configuration-files"></a>So rufen Sie die Clientkonfigurationsdateien ab

Wenn Sie die Clientkonfigurationsdateien bereits erstellt haben und sie nur abrufen müssen, können Sie das Cmdlet „Get-AzureRmVpnClientConfiguration“ verwenden. Das Cmdlet „Get-AzureRmVpnClientConfiguration“ gibt eine URL (Link) zurück, über die Sie die Datei „VpnClientConfiguration.zip“ herunterladen können. Wenn Sie Änderungen an der P2S-VPN-Konfiguration (beispielsweise am VPN-Protokolltyp oder -Authentifizierungstyp) vorgenommen haben, wird die Konfiguration nicht automatisch aktualisiert. Sie müssen stattdessen das Cmdlet „New-AzureRmVpnClientConfiguration“ ausführen, um die Konfiguration neu zu erstellen.

Um zuvor generierte Clientkonfigurationsdateien abzurufen, verwenden Sie das folgende Beispiel:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -VirtualNetworkGatewayName "VNet1GW"
```
 
## <a name="installwin"></a>Installieren eines Windows-VPN-Clientkonfigurationspakets

Sie können auf jedem Windows-Clientcomputer das gleiche VPN-Clientkonfigurationspaket verwenden – vorausgesetzt, es handelt sich dabei um die passende Version für die Architektur des jeweiligen Clients. Die Liste mit den unterstützten Clientbetriebssystemen finden Sie im Abschnitt [Point-to-Site-Verbindungen](vpn-gateway-vpn-faq.md#P2S) der häufig gestellten Fragen zu VPN Gateway.

Führen Sie die folgenden Schritte aus, um den nativen Windows-VPN-Client für die Zertifkatauthentifizierung zu konfigurieren:

1. Wählen Sie die VPN-Clientkonfigurationsdateien, die der Architektur des Windows-Computers entsprechen. Wählen Sie für eine 64-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupAmd64“ aus. Wählen Sie für eine 32-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupX86“ aus. 
2. Doppelklicken Sie auf das Paket, um es zu installieren. Sollte ein SmartScreen-Popup erscheinen, klicken Sie auf **Weitere Informationen** und anschließend auf **Trotzdem ausführen**.
3. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die VPN-Verbindung zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird. 

## <a name="installmac"></a>Installieren einer Mac-VPN-Clientkonfiguration (OSX)

Für jedes Mac-Gerät, das eine Verbindung mit dem Azure-VNET herstellt, muss eine separate VPN-Clientkonfiguration erstellt werden. Sie können die gleichen Konfigurationsdateien für mehrere Mac-Geräte verwenden. Das liegt daran, dass für diese Geräte das Benutzerzertifikat in den VPN-Clientkonfigurationsdateien angegeben werden muss. Der Ordner **Allgemein** enthält alle zum Erstellen einer VPN-Clientkonfiguration notwendigen Informationen. Der Ordner „Allgemein“ enthält die folgenden Dateien:

* **VpnSettings.xml**. Diese Datei enthält wichtige Einstellungen wie Serveradresse und Tunneltyp. 
* **VpnServerRoot.cer**. Diese Datei enthält das Stammzertifikat, das zum Überprüfen des Azure-VPN-Gateways während der P2S-Verbindungseinrichtung erforderlich ist.

Führen Sie die folgenden Schritte aus, um den nativen VPN-Client unter Mac für die Zertifkatauthentifizierung zu konfigurieren:

1. Importieren Sie das Stammzertifikat **VpnServerRoot** auf Ihrem Mac. Hierzu können Sie die Datei auf den Mac kopieren und darauf doppelklicken.  
Klicken Sie zum Importieren auf **Hinzufügen**.

  ![Hinzufügen des Zertifikats](./media/point-to-site-vpn-client-configuration-azure-cert/addcert.png)
2. Öffnen Sie unter **Network Preferences** (Netzwerkeinstellungen) das Dialogfeld **Netzwerk**, und klicken Sie auf **„+“**, um ein neues VPN-Clientverbindungsprofil für eine P2S-Verbindung mit dem Azure-NET zu erstellen.

  Für **Schnittstelle** ist der Wert „VPN“ und für **VPN-Typ** der Wert „IKEv2“ angegeben. Geben Sie im Feld **Dienstname** einen Namen für das Profil ein, und klicken Sie auf **Erstellen**, um das VPN-Clientverbindungsprofil zu erstellen.

  ![Netzwerk](./media/point-to-site-vpn-client-configuration-azure-cert/network.png)
3. Kopieren Sie aus der Datei **VpnSettings.xml** im Ordner **Allgemein** den Tagwert **VpnServer**. Fügen Sie diesen Wert in die Felder **Serveradresse** und **Remote-ID** des Profils ein.

  ![Serverinformationen](./media/point-to-site-vpn-client-configuration-azure-cert/server.png)
4. Klicken Sie auf **Authentifizierungseinstellungen**, und wählen Sie **Zertifikat** aus. 

  ![Authentifizierungseinstellungen](./media/point-to-site-vpn-client-configuration-azure-cert/authsettings.png)
5. Klicken Sie auf **Auswählen...**, um das Zertifikat auszuwählen, das Sie für die Authentifizierung verwenden möchten.

  ![Zertifikat](./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png)
6. Unter **Choose An Identity** (Identität auswählen) wird eine Liste mit Zertifikaten zur Auswahl angezeigt. Wählen Sie das richtige Zertifikat aus, und klicken Sie auf **Weiter**.

  ![Identität](./media/point-to-site-vpn-client-configuration-azure-cert/identity.png)
7. Geben Sie im Feld **Local ID** (Lokale ID) den Namen des Zertifikats (aus Schritt 5) ein. In diesem Beispiel lautet er „ikev2Client.com“. Klicken Sie auf die Schaltfläche **Übernehmen**, um die Änderungen zu speichern.

  ![apply](./media/point-to-site-vpn-client-configuration-azure-cert/applyconnect.png)
8. Klicken Sie im Dialogfeld **Netzwerk** auf **Übernehmen**, um alle Änderungen zu speichern. Klicken Sie auf **Verbinden**, um die P2S-Verbindung mit dem Azure-VNET zu starten.

## <a name="next-steps"></a>Nächste Schritte

Rufen Sie erneut den Artikel zum [Abschließen Ihrer P2S-Konfiguration](vpn-gateway-howto-point-to-site-rm-ps.md) auf.
