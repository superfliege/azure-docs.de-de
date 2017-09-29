---
title: "Erstellen und Installieren von VPN-Clientkonfigurationsdateien für P2S-RADIUS-Verbindungen: PowerShell: Azure | Microsoft-Dokumentation"
description: "Dieser Artikel unterstützt Sie beim Erstellen einer VPN-Clientkonfigurationsdatei für P2S-Verbindungen (Point-to-Site), die RADIUS-Authentifizierung nutzen."
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
ms.openlocfilehash: c56a9b06a11d25cf046a0faeb29af1d78d9a4a89
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication-preview"></a>Erstellen und Installieren von Clientkonfigurationsdateien für P2S-RADIUS-Authentifizierung (Vorschau)

VPN-Clientkonfigurationsdateien sind in einer ZIP-Datei enthalten. Konfigurationsdateien enthalten die Einstellungen, die für das Herstellen einer P2S-Verbindung zwischen einem nativen Windows- oder Mac-IKEv2-VPN-Client und einem VNET erforderlich sind. Der RADIUS-Server bietet mehrere Authentifizierungsoptionen, daher variiert die VPN-Clientkonfiguration je nach Option.

### <a name="workflow"></a>Workflow

  1. Richten Sie das Azure-VPN-Gateway für P2S-Konnektivität ein.
  2. Richten Sie den RADIUS-Server für die Authentifizierung ein. 
  3. Verwenden Sie die VPN-Clientkonfiguration für die gewünschte Authentifizierungsoption, um den VPN-Client auf dem Windows-Gerät einzurichten. Dadurch können Sie über eine P2S-Verbindung eine Verbindung mit Azure-VNETs herstellen.

>[!IMPORTANT]
>Wenn nach der Erstellung des VPN-Clientkonfigurationsprofils Änderungen an der P2S-VPN-Konfiguration (beispielsweise am VPN-Protokolltyp oder -Authentifizierungstyp) vorgenommen werden, müssen Sie eine neue VPN-Clientkonfiguration auf den Benutzergeräten erstellen und installieren.
>
>

## <a name="adeap"></a>Authentifizierung mit Benutzername und Kennwort

* **AD-Authentifizierung:** Ein gängiges Szenario ist die AD-Domänenauthentifizierung. In diesem Szenario verwenden Benutzer ihre Domänenanmeldeinformationen, um eine Verbindung mit Azure-VNETs herzustellen. Sie können VPN-Clientkonfigurationsdateien für die RADIUS-AD-Authentifizierung erstellen.

* **Authentifizierung ohne AD:** Sie können auch das RADIUS-Authentifizierungsszenario mit Benutzername und Kennwort ohne AD konfigurieren.

Stellen Sie sicher, dass alle Benutzer, die eine Verbindung herstellen, Benutzername-/Kennwortanmeldeinformationen besitzen, die über RADIUS authentifiziert werden können. Sie können nur eine Konfiguration für das EAP-MSCHAPv2-Authentifizierungsprotokoll mit Benutzername und Kennwort erstellen. Als „-AuthenticationMethod“ ist „EapMSChapv2“ angegeben.

### <a name="usernamefiles"></a>Generieren der VPN-Clientkonfigurationsdateien

Erstellen Sie die VPN-Clientkonfiguration mit dem folgenden Befehl:

```powershell 
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -VirtualNetworkGatewayName "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Nach der Ausführung des Befehls wird ein Link zurückgegeben. Kopieren Sie den Link, und fügen Sie ihn in einen Webbrowser ein, um die ZIP-Datei „VpnClientConfiguration.zip“ herunterzuladen. Entzippen Sie die Datei, um die folgenden Ordner anzuzeigen: 
 
* Die Ordner „WindowsAmd64“ und „WindowsX86“. Diese Ordner enthalten das Windows-64-Bit- bzw. das Windows-32-Bit-Installer-Paket. 
* Einen Ordner namens „GenericDevice“. Dieser Ordner enthält allgemeine Informationen zum Erstellen Ihrer eigenen VPN-Clientkonfiguration. Ignorieren Sie diesen Ordner.
* Wenn beim Erstellen des Gateways für virtuelle Netzwerke IKEv2 konfiguriert wurde, wird ein Ordner mit dem Namen „Mac“ angezeigt, der eine Datei namens „mobileconfig“ enthält. Diese Datei wird zum Konfigurieren von Mac-Clients verwendet.

Sie können Clientkonfigurationsdateien abrufen, die Sie bereits erstellt haben. Das Cmdlet „Get-AzureRmVpnClientConfiguration“ gibt eine URL (Link) zurück, über die Sie die Datei „VpnClientConfiguration.zip“ herunterladen können. Wenn Sie Änderungen an der P2S-VPN-Konfiguration (beispielsweise am VPN-Protokolltyp oder -Authentifizierungstyp) vornehmen, wird die Konfiguration nicht automatisch aktualisiert. Sie müssen das Cmdlet „New-AzureRmVpnClientConfiguration“ ausführen, um die Konfiguration neu zu erstellen.

Um zuvor generierte Clientkonfigurationsdateien abzurufen, verwenden Sie den folgenden Befehl:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -VirtualNetworkGatewayName "VNet1GW"
```
 
### <a name="adwincli"></a>Einrichten eines Windows-VPN-Clients

Sie können auf jedem Windows-Clientcomputer das gleiche VPN-Clientkonfigurationspaket verwenden – vorausgesetzt, es handelt sich dabei um die passende Version für die Architektur des jeweiligen Clients. Die Liste mit den unterstützten Clientbetriebssystemen finden Sie im Abschnitt [Point-to-Site-Verbindungen](vpn-gateway-vpn-faq.md#P2S) der häufig gestellten Fragen zu VPN Gateway.

Führen Sie die folgenden Schritte aus, um den nativen Windows-VPN-Client für die Zertifkatauthentifizierung zu konfigurieren:

1. Wählen Sie die VPN-Clientkonfigurationsdateien, die der Architektur des Windows-Computers entsprechen. Wählen Sie für eine 64-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupAmd64“ aus. Wählen Sie für eine 32-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupX86“ aus. 
2. Doppelklicken Sie auf das Paket, um es zu installieren. Sollte ein SmartScreen-Popup erscheinen, klicken Sie auf **Weitere Informationen** und anschließend auf **Trotzdem ausführen**.
3. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die VPN-Verbindung zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird. 

### <a name="admaccli"></a>Einrichten eines Mac-VPN-Clients (OSX)

1. Wählen Sie die Datei **VpnClientSetup mobileconfig**, und senden Sie sie an alle Benutzer. Sie können sie per E-Mail senden oder eine andere Methode verwenden.

2. Navigieren Sie auf dem Mac zur Datei **mobileconfig**.

  ![Navigieren zur Datei „mobilconfig“](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)
3. Doppelklicken Sie auf das Profil, um es zu installieren, und klicken Sie dann auf **Weiter**. Der Profilname ist identisch mit dem Namen des VNET.

  ![Installieren](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
4. Klicken Sie auf **Weiter**, um dem Absender des Profils zu vertrauen und die Installation fortzusetzen.

  ![Weiter](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
5. Während der Profilinstallation haben Sie die Möglichkeit, den Benutzernamen und das Kennwort für die VPN-Authentifizierung anzugeben. Es ist nicht erforderlich, diese Informationen einzugeben. Wenn Sie sie angegeben, werden die Informationen gespeichert und automatisch verwendet, wenn Sie eine Verbindung initiieren. Klicken Sie auf **Installieren**, um den Vorgang fortzusetzen.

  ![Einstellungen](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
6. Geben Sie einen Benutzernamen und ein Kennwort für die Berechtigungen ein, die zum Installieren des Profils auf Ihrem Computer erforderlich sind. Klicken Sie auf **OK**.

  ![Benutzername und Kennwort](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
7. Nach der Installation wird das Profil im Dialogfeld **Profile** angezeigt. Dieses Dialogfeld kann später auch über **Systemeinstellungen** geöffnet werden.

  ![Systemeinstellungen]](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
8. Öffnen Sie zum Zugreifen auf die VPN-Verbindung über **Systemeinstellungen** das Dialogfeld **Netzwerk**.

  ![Netzwerk](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
9. Die VPN-Verbindung wird als **IkeV2-VPN** angezeigt. Der Name kann durch Aktualisierung der Datei **mobileconfig** geändert werden.

  ![connection](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
10. Klicken Sie auf **Authentifizierungseinstellungen**. Wählen Sie in der Dropdownliste **Benutzername**, und geben Sie Ihre Anmeldeinformationen ein. Wenn Sie die Anmeldeinformationen zuvor eingegeben haben, wird **Benutzername** in der Dropdownliste automatisch ausgewählt, und Benutzername und Kennwort werden vorab aufgefüllt. Klicken Sie auf **OK** , um die Einstellungen zu speichern. Dadurch gelangen Sie zurück zum Dialogfeld „Netzwerk“.

  ![authenticate](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
11. Klicken Sie auf **Übernehmen**, um die Änderungen zu speichern. Klicken Sie zum Initiieren der Verbindung auf **Verbinden**.

## <a name="certeap"></a>Zertifikatauthentifizierung
 
Sie können VPN-Clientkonfigurationsdateien für die RADIUS-Zertifikatauthentifizierung erstellen, die das EAP-TLS-Protokoll nutzt. In der Regel wird ein vom Unternehmen ausgestelltes Zertifikat zum Authentifizieren eines Benutzers für VPN verwendet. Stellen Sie sicher, dass alle Benutzer, die eine Verbindung herstellen, ein Zertifikat auf dem Benutzergerät installiert haben und das Zertifikat vom RADIUS-Server überprüft werden kann.
 
* Für „-AuthenticationMethod“ ist „EapTls“ angegeben.
* Während der Zertifikatauthentifizierung überprüft der Client den RADIUS-Server durch Überprüfen des Zertifikats. Bei „-RadiusRootCert“ handelt es sich um die CER-Datei mit dem Stammzertifikat, das zum Überprüfen des RADIUS-Servers verwendet wird.  
* In einigen Fällen besitzt ein Windows-Gerät mehrere Clientzertifikate. Während der Authentifizierung kann das zur Anzeige eines Popupdialogfelds führen, in dem alle Zertifikate aufgeführt sind. Der Benutzer muss dann das zu verwendende Zertifikat auswählen. Das richtige Zertifikat kann durch Angabe des Stammzertifikats herausgefiltert werden, mit dem das Clientzertifikat verkettet werden soll. „-ClientRootCert“ ist die CER-Datei mit dem Stammzertifikat. Dies ist ein optionaler Parameter. Wenn auf dem Gerät, von dem Sie eine Verbindung herstellen möchten, nur ein Clientzertifikat vorhanden ist, muss dieser Parameter nicht angegeben werden.

### <a name="certfiles"></a>Generieren der VPN-Clientkonfigurationsdateien

Erstellen Sie die VPN-Clientkonfiguration mit dem folgenden Befehl:
 
```powershell
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -VirtualNetworkGatewayName "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root>
```

Das Cmdlet-Ergebnis gibt einen Link zurück. Kopieren Sie den Link, und fügen Sie ihn in einen Webbrowser ein, um die ZIP-Datei „VpnClientConfiguration.zip“ herunterzuladen. Entzippen Sie die Datei, um die folgenden Ordner anzuzeigen:

* Die Ordner „WindowsAmd64“ und „WindowsX86“. Diese Ordner enthalten das Windows-64-Bit- bzw. das Windows-32-Bit-Installer-Paket. 
* Einen Ordner namens „GenericDevice“. Dieser Ordner enthält allgemeine Informationen zum Erstellen Ihrer eigenen VPN-Clientkonfiguration.

Sie können Clientkonfigurationsdateien abrufen, die Sie bereits erstellt haben. Das Cmdlet „Get-AzureRmVpnClientConfiguration“ gibt eine URL (Link) zurück, über die Sie die Datei „VpnClientConfiguration.zip“ herunterladen können. Wenn Sie Änderungen an der P2S-VPN-Konfiguration (beispielsweise am VPN-Protokolltyp oder -Authentifizierungstyp) vornehmen, wird die Konfiguration nicht automatisch aktualisiert. Sie müssen das Cmdlet „New-AzureRmVpnClientConfiguration“ ausführen, um die Konfiguration neu zu erstellen.

Um zuvor generierte Clientkonfigurationsdateien abzurufen, verwenden Sie den folgenden Befehl:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -VirtualNetworkGatewayName "VNet1GW"
```
 
### <a name="certwincli"></a>Einrichten eines Windows-VPN-Clients

1. Wählen Sie ein Konfigurationspaket aus, und installieren Sie es auf dem Clientgerät. Wählen Sie für eine 64-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupAmd64“ aus. Wählen Sie für eine 32-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupX86“ aus. Sollte ein SmartScreen-Popup erscheinen, klicken Sie auf **Weitere Informationen** und anschließend auf **Trotzdem ausführen**. Sie können das Paket auch speichern und auf anderen Clientcomputern installieren.
2. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die VPN-Verbindung zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird.

### <a name="certmaccli"></a>Einrichten eines MAC-VPN-Clients

Für jedes Mac-Gerät, das eine Verbindung mit dem Azure-VNET herstellt, muss ein separates Profil erstellt werden. Das liegt daran, dass für diese Geräte das Benutzerzertifikat für die Authentifizierung im Profil angegeben werden muss. Der Ordner **Allgemein** enthält alle zum Erstellen eines Profils notwendigen Informationen.

  * **VpnSettings.xml** enthält wichtige Einstellungen wie Serveradresse und Tunneltyp.
  * **VpnServerRoot.cer** enthält das Stammzertifikat, das zum Überprüfen des VPN-Gateways während der P2S-Verbindungseinrichtung erforderlich ist.
  * **RadiusServerRoot.cer** enthält das Stammzertifikat, das zum Überprüfen des RADIUS-Servers während der Authentifizierung erforderlich ist.

Führen Sie die folgenden Schritte aus, um den nativen VPN-Client unter Mac für die Zertifkatauthentifizierung zu konfigurieren:

1. Importieren Sie die Stammzertifikate **VpnServerRoot** und **RadiusServerRoot** auf Ihrem Mac. Hierzu können Sie die Datei auf den Mac kopieren und darauf doppelklicken.  
Klicken Sie zum Importieren auf **Hinzufügen**.

  **Hinzufügen von „VpnServerRoot“**

  ![Hinzufügen des Zertifikats](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

  **Hinzufügen von „RadiusServerRoot“**

  ![Hinzufügen des Zertifikats](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Öffnen Sie unter **Network Preferences** (Netzwerkeinstellungen) das Dialogfeld **Netzwerk**, und klicken Sie auf **„+“**, um ein neues VPN-Clientverbindungsprofil für eine P2S-Verbindung mit dem Azure-NET zu erstellen.

  Für **Schnittstelle** ist der Wert „VPN“ und für **VPN-Typ** der Wert „IKEv2“ angegeben. Geben Sie im Feld **Dienstname** einen Namen für das Profil ein, und klicken Sie auf **Erstellen**, um das VPN-Clientverbindungsprofil zu erstellen.

  ![Netzwerk](./media/point-to-site-vpn-client-configuration-radius/network.png)
3. Kopieren Sie aus der Datei **VpnSettings.xml** im Ordner **Allgemein** den Tagwert **VpnServer**. Fügen Sie diesen Wert in die Felder **Serveradresse** und **Remote-ID** des Profils ein. Lassen Sie das Feld **Local ID** (Lokale ID) leer.

  ![Serverinformationen](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
4. Klicken Sie auf **Authentifizierungseinstellungen**, und wählen Sie **Zertifikat** aus. 

  ![Authentifizierungseinstellungen](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
5. Klicken Sie auf **Auswählen...**, um das Zertifikat auszuwählen, das Sie für die Authentifizierung verwenden möchten.

  ![Zertifikat](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
6. Unter **Choose An Identity** (Identität auswählen) wird eine Liste mit Zertifikaten zur Auswahl angezeigt. Wählen Sie das richtige Zertifikat aus, und klicken Sie auf **Weiter**.

  ![Identität](./media/point-to-site-vpn-client-configuration-radius/identity.png)
7. Geben Sie im Feld **Local ID** (Lokale ID) den Namen des Zertifikats (aus Schritt 5) ein. In diesem Beispiel lautet er „ikev2Client.com“. Klicken Sie auf die Schaltfläche **Übernehmen**, um die Änderungen zu speichern.

  ![apply](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
8. Klicken Sie im Dialogfeld **Netzwerk** auf **Übernehmen**, um alle Änderungen zu speichern. Klicken Sie auf **Verbinden**, um die P2S-Verbindung mit dem Azure-VNET zu starten.

## <a name="otherauth"></a>Andere Authentifizierungstypen oder -protokolle

Wenn Sie anstelle von Benutzername/Kennwort oder Zertifikaten einen anderen Authentifizierungstyp (etwa OTP) oder wenn Sie ein anderes Authentifizierungsprotokoll (etwa PEAP-MSCHAPv2 anstelle von EAP-MSCHAPv2) verwenden möchten, müssen Sie ein eigenes VPN-Clientkonfigurationsprofil erstellen. Für die Erstellung des Profils benötigen Sie Informationen wie die IP-Adresse des Gateways für virtuelle Netzwerke, den Tunneltyp und Routen mit geteilten Tunneln. Diese Informationen können mit den folgenden Schritten abgerufen werden:

1. Generieren Sie mit dem Cmdlet „Get-AzureRmVpnClientConfiguration“ die VPN-Clientkonfiguration für EapMSChapv2. Anweisungen hierzu finden Sie in [diesem Abschnitt](#ccradius) des Artikels.

2. Entzippen Sie die Datei „VpnClientConfiguration.zip“, und suchen Sie den Ordner „GenenericDevice“. Ignorieren Sie die Ordner mit den Windows-Installationsprogrammen für 64-Bit- und 32-Bit-Architekturen.
 
3. Der Ordner „GenenericDevice“ enthält eine XML-Datei mit dem Namen „VpnSettings“. Diese Datei enthält alle erforderlichen Informationen.

  * VpnServer: Der vollqualifizierte Domänenname des Azure-VPN-Gateways. Dies ist die Adresse, mit der der Client eine Verbindung herstellt.
  * VpnType : Der zum Herstellen einer Verbindung verwendete Tunneltyp
  * Routen: Routen, die in Ihrem Profil konfiguriert werden müssen, damit nur Datenverkehr für das Azure-VNET über den P2S-Tunnel gesendet wird
  * Der Ordner „GenenericDevice“ enthält darüber hinaus eine CER-Datei mit dem Namen „VpnServerRoot“. Diese Datei enthält das Stammzertifikat, das zum Überprüfen des Azure-VPN-Gateways während der P2S-Verbindungseinrichtung erforderlich ist. Installieren Sie das Zertifikat auf allen Geräten, die eine Verbindung mit dem Azure-VNET herstellen. 
 
## <a name="next-steps"></a>Nächste Schritte

Rufen Sie erneut den Artikel zum [Abschließen Ihrer P2S-Konfiguration](point-to-site-how-to-radius-ps.md) auf.
