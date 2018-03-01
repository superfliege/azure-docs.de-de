---
title: "Erstellen und Installieren von VPN-Clientkonfigurationsdateien für P2S-RADIUS-Verbindungen: PowerShell: Azure | Microsoft-Dokumentation"
description: "Erstellen Sie Windows-, Mac OS X- und Linux-VPN-Client-Konfigurationsdateien für Verbindungen, die RADIUS-Authentifizierung verwenden."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2018
ms.author: cherylmc
ms.openlocfilehash: ce914d2fd0472855ad7a17bf64ae43a76ceb5743
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>Erstellen und Installieren von Clientkonfigurationsdateien für P2S-RADIUS-Authentifizierung

Um eine P2S-Verbindung mit einem virtuellen Netzwerk herzustellen, müssen Sie das Clientgerät konfigurieren, über das die Verbindung hergestellt werden soll. Sie können P2S-VPN-Verbindungen über Windows, Mac OS x und Linux-Clientgeräte erstellen. Wenn Sie die RADIUS-Authentifizierung verwenden, stehen mehrere Authentifizierungsoptionen zur Verfügung: Authentifizierung per Benutzername/Kennwort, zertifikatbasierte Authentifizierung sowie weitere Authentifizierungstypen. Die VPN-Clientkonfiguration ist je nach Authentifizierungstyp unterschiedlich. Um den VPN-Client zu konfigurieren, verwenden Sie Clientkonfigurationsdateien, die die erforderlichen Einstellungen enthalten. Dieser Artikel hilft Ihnen beim Erstellen und Installieren der VPN-Clientkonfiguration für den gewünschten RADIUS-Authentifizierungstyp.

Der Konfigurationsworkflow für die P2S-RADIUS-Authentifizierung lautet wie folgt:

1. [Richten Sie das Azure-VPN-Gateway für P2S-Konnektivität ein](point-to-site-how-to-radius-ps.md).
2. [Richten Sie den RADIUS-Server für die Authentifizierung ein](point-to-site-how-to-radius-ps.md#radius). 
3. **Verwenden Sie die VPN-Clientkonfiguration für die gewünschte Authentifizierungsoption, um den VPN-Client einzurichten**. (Dieser Artikel)
4. [Schließen Sie die P2S-Konfiguration ab, und stellen Sie eine Verbindung her](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>Wenn nach der Erstellung des VPN-Clientkonfigurationsprofils Änderungen an der P2S-VPN-Konfiguration (beispielsweise am VPN-Protokolltyp oder -Authentifizierungstyp) vorgenommen werden, müssen Sie eine neue VPN-Clientkonfiguration auf den Benutzergeräten erstellen und installieren.
>
>

Entscheiden Sie zunächst, welchen Authentifizierungstyp Sie verwenden möchten: Benutzername/Kennwort, Zertifikat oder andere Authentifizierungstypen. In jedem Abschnitt finden Sie drei Schritte für Windows, Mac OS X und Linux (derzeit sind nur begrenzte Schritte verfügbar).

## <a name="adeap"></a>Authentifizierung mit Benutzername und Kennwort

Es gibt zwei Möglichkeiten zum Konfigurieren der Authentifizierung per Benutzername/Kennwort. Sie können die Authentifizierung per AD konfigurieren oder sich ohne AD authentifizieren. Stellen Sie in jedem Szenario sicher, dass alle Benutzer, die eine Verbindung herstellen, über Benutzername/Kennwort verfügen und über RADIUS authentifiziert werden können.

* Sie können beim Konfigurieren der Authentifizierung per Benutzername/Kennwort nur eine Konfiguration für das EAP-MSCHAPv2-Authentifizierungsprotokoll mit Benutzername und Kennwort erstellen.
* Für „-AuthenticationMethod“ ist „EapMsChapv2“ angegeben.

### <a name="usernamefiles"></a> 1. Generieren der VPN-Clientkonfigurationsdateien

Generieren Sie VPN-Clientkonfigurationsdateien zur Verwendung für die Authentifizierung mit Benutzername und Kennwort. Sie können die VPN-Clientkonfigurationsdateien mit dem folgenden Befehl generieren:

```powershell 
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Bei Ausführung des Befehls wird ein Link zurückgegeben. Kopieren Sie den Link, und fügen Sie ihn in einen Webbrowser ein, um „VpnClientConfiguration.zip“ herunterzuladen. Entzippen Sie die Datei, um die folgenden Ordner anzuzeigen: 
 
* **WindowsAmd64** und **WindowsX86**: Diese Ordner enthalten das Windows-64-Bit- bzw. das 32-Bit-Installer-Paket. 
* **Generic**: Dieser Ordner enthält allgemeine Informationen zum Erstellen Ihrer eigenen VPN-Clientkonfiguration. Dieser Ordner ist für Konfigurationen der Authentifizierung mit Benutzername und Kennwort nicht erforderlich.
* **Mac**: Wenn beim Erstellen des Gateways für virtuelle Netzwerke IKEv2 konfiguriert wurde, wird ein Ordner mit dem Namen „Mac“ angezeigt, der eine Datei **mobileconfig** enthält. Diese Datei wird zum Konfigurieren von Mac-Clients verwendet.

Wenn Sie die Clientkonfigurationsdateien bereits erstellt haben, können Sie diese mithilfe des Cmdlets „Get-AzureRmVpnClientConfiguration“ abrufen. Wenn Sie jedoch Änderungen an der P2S-VPN-Konfiguration (beispielsweise am VPN-Protokolltyp oder -Authentifizierungstyp) vornehmen, wird die Konfiguration nicht automatisch aktualisiert. Sie müssen das Cmdlet „New-AzureRmVpnClientConfiguration“ ausführen, um einen neuen Konfigurationsdownload zu erstellen.

Um zuvor generierte Clientkonfigurationsdateien abzurufen, verwenden Sie den folgenden Befehl:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

### <a name="setupusername"></a> 2. VPN-Clients konfigurieren

Sie können die folgenden VPN-Clients konfigurieren:

* [Windows](#adwincli)
* [Mac (OS X)](#admaccli)
* [Linux über strongSwan](#adlinuxcli)
 
#### <a name="adwincli"></a>Setup für Windows-VPN-Clients

Sie können auf jedem Windows-Clientcomputer das gleiche VPN-Clientkonfigurationspaket verwenden – vorausgesetzt, es handelt sich dabei um die passende Version für die Architektur des jeweiligen Clients. Die Liste mit den unterstützten Clientbetriebssystemen finden Sie im Abschnitt [Point-to-Site-Verbindungen](vpn-gateway-vpn-faq.md#P2S) der häufig gestellten Fragen zu VPN Gateway.

Führen Sie die folgenden Schritte aus, um den nativen Windows-VPN-Client für die Zertifkatauthentifizierung zu konfigurieren:

1. Wählen Sie die VPN-Clientkonfigurationsdateien, die der Architektur des Windows-Computers entsprechen. Wählen Sie für eine 64-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupAmd64“ aus. Wählen Sie für eine 32-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupX86“ aus. 
2. Installieren Sie das Paket per Doppelklick. Sollte ein SmartScreen-Popup erscheinen, klicken Sie auf **Weitere Informationen** und anschließend auf **Trotzdem ausführen**.
3. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die VPN-Verbindung zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird. 

#### <a name="admaccli"></a>Setup des Mac-VPN-Clients (OS X)

1. Wählen Sie die Datei **VpnClientSetup mobileconfig**, und senden Sie sie an alle Benutzer. Sie können sie per E-Mail senden oder eine andere Methode verwenden.

2. Navigieren Sie auf dem Mac zur Datei **mobileconfig**.

  ![Navigieren zur Datei „mobilconfig“](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)
3. Doppelklicken Sie auf das Profil, um es zu installieren, und klicken Sie dann auf **Weiter**. Der Profilname ist identisch mit dem Namen des VNET.

  ![Installieren](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
4. Klicken Sie auf **Weiter**, um dem Absender des Profils zu vertrauen und die Installation fortzusetzen.

  ![Weiter](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
5. Während der Profilinstallation haben Sie die Möglichkeit, den Benutzernamen und das Kennwort für die VPN-Authentifizierung anzugeben. Es ist nicht erforderlich, diese Informationen einzugeben. Wenn Sie sie angegeben, werden die Informationen gespeichert und automatisch verwendet, wenn Sie eine Verbindung initiieren. Klicken Sie auf **Installieren**, um den Vorgang fortzusetzen.

  ![settings](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
6. Geben Sie einen Benutzernamen und ein Kennwort für die Berechtigungen ein, die zum Installieren des Profils auf Ihrem Computer erforderlich sind. Klicken Sie auf **OK**.

  ![Benutzername und Kennwort](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
7. Nach der Installation wird das Profil im Dialogfeld **Profile** angezeigt. Dieses Dialogfeld kann später auch über **Systemeinstellungen** geöffnet werden.

  ![Systemeinstellungen](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
8. Öffnen Sie zum Zugreifen auf die VPN-Verbindung über **Systemeinstellungen** das Dialogfeld **Netzwerk**.

  ![Netzwerk](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
9. Die VPN-Verbindung wird als **IkeV2-VPN** angezeigt. Der Name kann durch Aktualisierung der Datei **mobileconfig** geändert werden.

  ![connection](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
10. Klicken Sie auf **Authentifizierungseinstellungen**. Wählen Sie in der Dropdownliste **Benutzername**, und geben Sie Ihre Anmeldeinformationen ein. Wenn Sie die Anmeldeinformationen zuvor eingegeben haben, wird **Benutzername** in der Dropdownliste automatisch ausgewählt, und Benutzername und Kennwort werden vorab aufgefüllt. Klicken Sie auf **OK** , um die Einstellungen zu speichern. Dadurch gelangen Sie zurück zum Dialogfeld „Netzwerk“.

  ![authenticate](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
11. Klicken Sie auf **Übernehmen**, um die Änderungen zu speichern. Klicken Sie zum Initiieren der Verbindung auf **Verbinden**.

#### <a name="adlinuxcli"></a>Setup des Linux VPN-Clients über strongSwan

Die folgenden Anweisungen wurden mithilfe von strongSwan 5.5.1 auf Ubuntu 17.0.4 erstellt. Die Anzeige kann je nach Ihrer Version von Linux und strongSwan abweichen.

1. Öffnen Sie das **Terminal**, und installieren Sie **strongSwan** und den Netzwerk-Manager, indem Sie den Befehl im Beispiel ausführen. Wenn Sie einen Fehler für „libcharon-extra-plugins“ erhalten, ersetzen Sie diesen durch „strongswan-plugin-eap-mschapv2“.

  ```Terminal
  sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
  ```
2. Klicken Sie auf das Symbol für den **Netzwerk-Manager** (Pfeil nach oben/nach unten), und wählen Sie **Verbindungen bearbeiten**.

  ![Verbindung bearbeiten](./media/point-to-site-vpn-client-configuration-radius/EditConnection.png)
3. Erstellen Sie über die Schaltfläche **Hinzufügen** eine neue Verbindung.

  ![Verbindung hinzufügen](./media/point-to-site-vpn-client-configuration-radius/AddConnection.png)
4. Wählen Sie **IPsec/IKEv2 (strongswan)** aus dem Dropdownmenü, und klicken Sie auf **Erstellen**. Sie können Ihre Verbindung in diesem Schritt umbenennen.

  ![ikev2 hinzufügen](./media/point-to-site-vpn-client-configuration-radius/AddIKEv2.png)
5. Öffnen Sie die Datei **vpnSettings.xml** im Ordner **Allgemein** der heruntergeladenen Clientkonfigurationsdateien. Suchen Sie das Tag **VpnServer**, und kopieren Sie den Namen, der mit „azuregateway“ beginnt und mit „cloudapp.net“ endet.

  ![VPN-Einstellungen](./media/point-to-site-vpn-client-configuration-radius/VpnSettings.png)
6. Fügen Sie diesen Namen im Feld **Adresse** Ihrer neuen VPN-Verbindung im Abschnitt **Gateway** ein. Klicken Sie anschließend auf das Ordnersymbol am Ende des Felds **Zertifikat**, navigieren Sie zum Ordner „Allgemein“, und wählen Sie dort die Datei **VpnServerRoot**.
7. Wählen Sie im Abschnitt **Client** der Verbindung **EAP** als **Authentifizierung**, und geben Sie Ihre Anmeldedaten ein. Sie müssen möglicherweise das Sperrsymbol auf der rechten Seite wählen, um diese Informationen zu speichern. Klicken Sie dann auf **Speichern**.

  ![Verbindungseinstellungen bearbeiten](./media/point-to-site-vpn-client-configuration-radius/editconnectionsettings.png)
8. Klicken Sie auf das Symbol für den **Netzwerk-Manager** (Pfeil nach oben/nach unten), und wählen Sie **Verbindungen bearbeiten**. Sie sehen die von Ihnen erstellte VPN-Verbindung. Wählen Sie die Verbindung aus, um sie zu initiieren.

  ![Verbindungsradius](./media/point-to-site-vpn-client-configuration-radius/ConnectRADIUS.png)

## <a name="certeap"></a>Zertifikatauthentifizierung
 
Sie können VPN-Clientkonfigurationsdateien für die RADIUS-Zertifikatauthentifizierung erstellen, die das EAP-TLS-Protokoll nutzt. In der Regel wird ein vom Unternehmen ausgestelltes Zertifikat zum Authentifizieren eines Benutzers für VPN verwendet. Stellen Sie sicher, dass alle Benutzer, die eine Verbindung herstellen, ein Zertifikat auf dem Benutzergerät installiert haben und das Zertifikat vom RADIUS-Server überprüft werden kann.
 
* Für „-AuthenticationMethod“ ist „EapTls“ angegeben.
* Während der Zertifikatauthentifizierung überprüft der Client den RADIUS-Server durch Überprüfen des Zertifikats. Bei „-RadiusRootCert“ handelt es sich um die CER-Datei mit dem Stammzertifikat, das zum Überprüfen des RADIUS-Servers verwendet wird.
* Auf jedem VPN-Client-Gerät muss ein Clientzertifikat installiert werden.
* In einigen Fällen besitzt ein Windows-Gerät mehrere Clientzertifikate. Während der Authentifizierung kann das zur Anzeige eines Popupdialogfelds führen, in dem alle Zertifikate aufgeführt sind. Der Benutzer muss dann das zu verwendende Zertifikat auswählen. Das richtige Zertifikat kann durch Angabe des Stammzertifikats herausgefiltert werden, mit dem das Clientzertifikat verkettet werden soll. „-ClientRootCert“ ist die CER-Datei mit dem Stammzertifikat. Dies ist ein optionaler Parameter. Wenn auf dem Gerät, von dem Sie eine Verbindung herstellen möchten, nur ein Clientzertifikat vorhanden ist, muss dieser Parameter nicht angegeben werden.

### <a name="certfiles"></a>1. Generieren der VPN-Clientkonfigurationsdateien

Generieren Sie VPN-Clientkonfigurationsdateien zur Verwendung für die Zertifikatauthentifizierung. Sie können die VPN-Clientkonfigurationsdateien mit dem folgenden Befehl generieren:
 
```powershell
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root> | fl
```

Bei Ausführung des Befehls wird ein Link zurückgegeben. Kopieren Sie den Link, und fügen Sie ihn in einen Webbrowser ein, um „VpnClientConfiguration.zip“ herunterzuladen. Entzippen Sie die Datei, um die folgenden Ordner anzuzeigen:

* **WindowsAmd64** und **WindowsX86**: Diese Ordner enthalten das Windows-64-Bit- bzw. das 32-Bit-Installer-Paket. 
* **GenericDevice**: Dieser Ordner enthält allgemeine Informationen zum Erstellen Ihrer eigenen VPN-Clientkonfiguration.

Wenn Sie die Clientkonfigurationsdateien bereits erstellt haben, können Sie diese mithilfe des Cmdlets „Get-AzureRmVpnClientConfiguration“ abrufen. Wenn Sie jedoch Änderungen an der P2S-VPN-Konfiguration (beispielsweise am VPN-Protokolltyp oder -Authentifizierungstyp) vornehmen, wird die Konfiguration nicht automatisch aktualisiert. Sie müssen das Cmdlet „New-AzureRmVpnClientConfiguration“ ausführen, um einen neuen Konfigurationsdownload zu erstellen.

Um zuvor generierte Clientkonfigurationsdateien abzurufen, verwenden Sie den folgenden Befehl:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" | fl
```
 
### <a name="setupusername"></a> 2. VPN-Clients konfigurieren

Sie können die folgenden VPN-Clients konfigurieren:

* [Windows](#certwincli)
* [Mac (OS X)](#certmaccli)
* Linux (unterstützt, noch keine Schritte im Artikel)

#### <a name="certwincli"></a>Setup für Windows-VPN-Clients

1. Wählen Sie ein Konfigurationspaket aus, und installieren Sie es auf dem Clientgerät. Wählen Sie für eine 64-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupAmd64“ aus. Wählen Sie für eine 32-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupX86“ aus. Sollte ein SmartScreen-Popup erscheinen, klicken Sie auf **Weitere Informationen** und anschließend auf **Trotzdem ausführen**. Sie können das Paket auch speichern und auf anderen Clientcomputern installieren.
2. Jeder Clientcomputer benötigt ein Clientzertifikat für die Authentifizierung. Installieren Sie das Clientzertifikat. Informationen zu Clientzertifikaten finden Sie im [Artikel zu Clientzertifikaten für Point-to-Site-Verbindungen](vpn-gateway-certificates-point-to-site.md). Informationen zum Installieren eines generierten Zertifikats finden Sie unter [Installieren eines Zertifikats auf Windows-Clients](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die VPN-Verbindung zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird.

#### <a name="certmaccli"></a>Setup des Mac-VPN-Clients (OS X)

Für jedes Mac-Gerät, das eine Verbindung mit dem Azure-VNET herstellt, muss ein separates Profil erstellt werden. Das liegt daran, dass für diese Geräte das Benutzerzertifikat für die Authentifizierung im Profil angegeben werden muss. Der Ordner **Allgemein** enthält alle zum Erstellen eines Profils notwendigen Informationen.

  * **VpnSettings.xml** enthält wichtige Einstellungen wie Serveradresse und Tunneltyp.
  * **VpnServerRoot.cer** enthält das Stammzertifikat, das zum Überprüfen des VPN-Gateways während der P2S-Verbindungseinrichtung erforderlich ist.
  * **RadiusServerRoot.cer** enthält das Stammzertifikat, das zum Überprüfen des RADIUS-Servers während der Authentifizierung erforderlich ist.

Führen Sie die folgenden Schritte aus, um den nativen VPN-Client unter Mac für die Zertifkatauthentifizierung zu konfigurieren:

1. Importieren Sie die Stammzertifikate **VpnServerRoot** und **RadiusServerRoot** auf Ihrem Mac. Hierzu können Sie die Datei auf den Mac kopieren und darauf doppelklicken.  
Klicken Sie zum Importieren auf **Hinzufügen**.

  *Hinzufügen von „VpnServerRoot“*

  ![Hinzufügen des Zertifikats](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

  *Hinzufügen von „RadiusServerRoot“*

  ![Hinzufügen des Zertifikats](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Jeder Clientcomputer benötigt ein Clientzertifikat für die Authentifizierung. Installieren Sie das Clientzertifikat auf dem Clientgerät.
3. Öffnen Sie unter **Network Preferences** (Netzwerkeinstellungen) das Dialogfeld **Netzwerk**, und klicken Sie auf **„+“**, um ein neues VPN-Clientverbindungsprofil für eine P2S-Verbindung mit dem Azure-NET zu erstellen.

  Für **Schnittstelle** ist der Wert „VPN“ und für **VPN-Typ** der Wert „IKEv2“ angegeben. Geben Sie im Feld **Dienstname** einen Namen für das Profil ein, und klicken Sie auf **Erstellen**, um das VPN-Clientverbindungsprofil zu erstellen.

  ![Netzwerk](./media/point-to-site-vpn-client-configuration-radius/network.png)
4. Kopieren Sie aus der Datei **VpnSettings.xml** im Ordner **Allgemein** den Tagwert **VpnServer**. Fügen Sie diesen Wert in die Felder **Serveradresse** und **Remote-ID** des Profils ein. Lassen Sie das Feld **Local ID** (Lokale ID) leer.

  ![Serverinformationen](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
5. Klicken Sie auf **Authentifizierungseinstellungen**, und wählen Sie **Zertifikat** aus. 

  ![Authentifizierungseinstellungen](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
6. Klicken Sie auf **Auswählen...**, um das Zertifikat auszuwählen, das Sie für die Authentifizierung verwenden möchten.

  ![Zertifikat](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
7. Unter **Choose An Identity** (Identität auswählen) wird eine Liste mit Zertifikaten zur Auswahl angezeigt. Wählen Sie das richtige Zertifikat aus, und klicken Sie auf **Weiter**.

  ![identity](./media/point-to-site-vpn-client-configuration-radius/identity.png)
8. Geben Sie im Feld **Local ID** (Lokale ID) den Namen des Zertifikats (aus Schritt 6) ein. In diesem Beispiel lautet er „ikev2Client.com“. Klicken Sie auf die Schaltfläche **Übernehmen**, um die Änderungen zu speichern.

  ![apply](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
9. Klicken Sie im Dialogfeld **Netzwerk** auf **Übernehmen**, um alle Änderungen zu speichern. Klicken Sie auf **Verbinden**, um die P2S-Verbindung mit dem Azure-VNET zu starten.

## <a name="otherauth"></a>Arbeiten mit anderen Authentifizierungstypen oder -protokollen

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

Informationen zur P2S-Problembehandlung finden Sie unter [Problembehandlung: Probleme mit Azure P2S-Verbindungen (Point-to-Site)](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).