---
title: Problembehandlung bei Azure Punkt-zu-Standort-Verbindungsproblemen | Microsoft-Dokumentation
description: "Informieren Sie sich über die Problembehandlung für Punkt-zu-Standort-Verbindungsprobleme."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: genli
ms.openlocfilehash: 69d363b5ff0b94884cf6d13ae0260f3747e4e69a
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2017
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a>Problembehandlung: Azure Punkt-zu-Standort-Verbindungsprobleme

In diesem Artikel werden allgemeine Punkt-zu-Standort-Verbindungsprobleme aufgeführt, die auftreten können. Darüber hinaus werden die möglichen Ursachen und Lösungen für diese Probleme diskutiert.

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a>VPN-Clientfehler: Zertifikat wurde nicht gefunden

### <a name="symptom"></a>Symptom

Wenn Sie versuchen, mithilfe des VPN-Client mit einem virtuellen Netzwerk von Azure eine Verbindung herzustellen, erhalten Sie folgende Fehlermeldung:

**Es konnte kein Zertifikat gefunden werden, das mit dem Extensible Authentication-Protokoll verwendet werden kann. (Fehler 798)**

### <a name="cause"></a>Ursache

Dieses Problem tritt auf, wenn das Clientzertifikat in **Zertifikate – Aktueller Benutzer\Eigene Zertifikate\Zertifikate** fehlt.

### <a name="solution"></a>Lösung

Gehen Sie folgendermaßen vor, um dieses Problem zu beheben:

1. Stellen Sie sicher, dass sich die folgenden Zertifikate im richtigen Speicherort befinden:

    | Zertifikat | Speicherort |
    | ------------- | ------------- |
    | AzureClient.pfx  | Aktueller Benutzer\Eigene Zertifikate\Zertifikate |
    | Azuregateway-*GUID*.cloudapp.net  | Aktueller Benutzer\Vertrauenswürdige Stammzertifizierungsstellen|
    | AzureGateway-*GUID*.cloudapp.net, AzureRoot.cer    | Lokaler Computer\Vertrauenswürdige Stammzertifizierungsstellen|

2. Navigieren Sie zu „Users\<Benutzername>\AppData\Roaming\Microsoft\Network\Connections\Cm\<GUID>“, und installieren Sie manuell das Zertifikat (\*.CER-Datei) im Computerspeicher des Benutzers.

Weitere Informationen zum Installieren des Clientzertifikats finden Sie unter [Generieren und Exportieren von Zertifikaten für Punkt-zu-Standort-Verbindungen mithilfe von PowerShell](vpn-gateway-certificates-point-to-site.md).

> [!NOTE]
> Wenn Sie das Clientzertifikat importieren, wählen Sie nicht die Option **Verstärkte Sicherheit für den privaten Schlüssel aktivieren** aus.

## <a name="vpn-client-error-the-message-received-was-unexpected-or-badly-formatted"></a>VPN-Clientfehler: Das Format der empfangenen Nachricht war unerwartet oder fehlerhaft

### <a name="symptom"></a>Symptom

Wenn Sie versuchen, mithilfe des VPN-Client mit einem virtuellen Netzwerk von Azure eine Verbindung herzustellen, erhalten Sie folgende Fehlermeldung:

**Das Format der empfangenen Nachricht war unerwartet oder fehlerhaft. (Fehler 0x80090326)**

### <a name="cause"></a>Ursache

Dieses Problem tritt auf, wenn der öffentliche Schlüssel des Stammzertifikats nicht in das VPN-Gateway von Azure hochgeladen wurde. Es kann auch auftreten, wenn der Schlüssel beschädigt oder abgelaufen ist.

### <a name="solution"></a>Lösung

Überprüfen Sie zur Behebung dieses Problems den Status des Stammzertifikats im Azure-Portal, um festzustellen, ob es widerrufen wurde. Wenn es nicht widerrufen wurde, versuchen Sie, das Stammzertifikat zu löschen und es erneut hochzuladen. Weitere Informationen finden Sie unter [Erstellen von Zertifikaten](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a>VPN-Clientfehler: Eine Zertifikatkette wurde verarbeitet, aber beendet 

### <a name="symptom"></a>Symptom 

Wenn Sie versuchen, mithilfe des VPN-Client mit einem virtuellen Netzwerk von Azure eine Verbindung herzustellen, erhalten Sie folgende Fehlermeldung:

**Eine Zertifikatkette wurde verarbeitet, aber in einem Stammzertifikat beendet, dem der Vertrauensanbieter nicht vertraut.**

### <a name="solution"></a>Lösung

1. Stellen Sie sicher, dass sich die folgenden Zertifikate im richtigen Speicherort befinden:

    | Zertifikat | Speicherort |
    | ------------- | ------------- |
    | AzureClient.pfx  | Aktueller Benutzer\Eigene Zertifikate\Zertifikate |
    | Azuregateway-*GUID*.cloudapp.net  | Aktueller Benutzer\Vertrauenswürdige Stammzertifizierungsstellen|
    | AzureGateway-*GUID*.cloudapp.net, AzureRoot.cer    | Lokaler Computer\Vertrauenswürdige Stammzertifizierungsstellen|

2. Wenn die Zertifikate bereits im Speicherort vorhanden sind, versuchen Sie, die Zertifikate zu löschen und neu zu installieren. Das **azuregateway-*GUID*.cloudapp.net** Zertifikat finden Sie im VPN-Clientkonfigurationspaket, das Sie vom Azure-Portal heruntergeladen haben. Sie können Dateiarchivierer verwenden, um die Dateien aus dem Paket zu extrahieren.

## <a name="file-download-error-target-uri-is-not-specified"></a>Dateidownloadfehler: Ziel-URI ist nicht angegeben

### <a name="symptom"></a>Symptom

Sie erhalten die folgende Fehlermeldung:

**Fehler beim Herunterladen der Datei. Ziel-URI ist nicht angegeben.**

### <a name="cause"></a>Ursache 

Dieses Problem tritt aufgrund eines falschen Gatewaytyps auf. 

### <a name="solution"></a>Lösung

Der VPN-Gatewaytyp muss **VPN** sein, und der VPN-Typ muss als **RouteBased** festgelegt sein.

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a>VPN-Clientfehler: Azure-VPN benutzerdefinierter Skriptfehler 

### <a name="symptom"></a>Symptom

Wenn Sie versuchen, mithilfe des VPN-Client mit einem virtuellen Netzwerk von Azure eine Verbindung herzustellen, erhalten Sie folgende Fehlermeldung:

**Custom script (to update your routing table) failed. (Fehler mit benutzerdefiniertem Skript (beim Aktualisieren Ihrer Routingtabelle).) (Fehler 8007026f)**

### <a name="cause"></a>Ursache

Dieses Problem kann auftreten, wenn Sie versuchen, die Standort-zu-Punkt-VPN-Verbindung mithilfe einer Verknüpfung zu öffnen.

### <a name="solution"></a>Lösung 

Öffnen Sie das VPN-Clientpaket direkt, anstatt es über die Verknüpfung zu öffnen.

## <a name="cannot-install-the-vpn-client"></a>VPN-Client kann nicht installiert werden

### <a name="cause"></a>Ursache 

Ein zusätzliches Zertifikat ist erforderlich, um dem VPN-Gateway für Ihr virtuelles Netzwerk zu vertrauen. Das Zertifikat ist im VPN-Clientkonfigurationspaket enthalten, das vom Azure-Portal generiert wird.

### <a name="solution"></a>Lösung

Extrahieren Sie das VPN-Clientkonfigurationspaket, und suchen Sie nach der CER-Datei. Um das Zertifikat zu installieren, führen Sie die folgenden Schritte aus:

1. Öffnen Sie „mmc.exe“.
2. Fügen Sie das **Zertifikate** -Snap-in hinzu.
3. Wählen Sie das **Computer** -Konto für den lokalen Computer aus.
4. Klicken Sie mit der rechten Maustaste auf den Knoten **Vertrauenswürdige Stammzertifizierungsstellen**. Klicken Sie auf **All-Task** (Alle Aufgaben) >  **Importieren**, und suchen Sie nach der CER-Datei, die Sie aus dem VPN-Clientkonfigurationspaket extrahiert haben.
5. Starten Sie den Computer neu. 
6. Versuchen Sie eine erneute Installation des VPN-Clients.

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-data-is-invalid"></a>Azure-Portal-Fehler: Fehler beim Speichern des VPN-Gateways, und die Daten sind ungültig

### <a name="symptom"></a>Symptom

Wenn Sie versuchen, die Änderungen für das VPN-Gateway im Azure-Portal zu speichern, erhalten Sie die folgende Fehlermeldung:

*Fehler beim Speichern des Gateways für virtuelle Netzwerke *Gatewayname&lt;**&gt;. Die Daten für die &lt;*Zertifikat-ID*&gt; des Zertifikats sind ungültig.**

### <a name="cause"></a>Ursache 

Dieses Problem kann auftreten, wenn der öffentliche Schlüssel des Stammzertifikats, den Sie hochgeladen haben, ein ungültiges Zeichen wie z.B. ein Leerzeichen enthält.

### <a name="solution"></a>Lösung

Stellen Sie sicher, dass die Daten im Zertifikat keine ungültigen Zeichen wie Zeilenumbrüche (Wagenrückläufe) enthalten. Der gesamte Wert sollte in einer langen Zeile sein. Der folgende Text ist ein Beispiel für das Zertifikat:

    -----BEGIN CERTIFICATE-----
    MIIC5zCCAc+gAwIBAgIQFSwsLuUrCIdHwI3hzJbdBjANBgkqhkiG9w0BAQsFADAW
    MRQwEgYDVQQDDAtQMlNSb290Q2VydDAeFw0xNzA2MTUwMjU4NDZaFw0xODA2MTUw
    MzE4NDZaMBYxFDASBgNVBAMMC1AyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEF
    AAOCAQ8AMIIBCgKCAQEAz8QUCWCxxxTrxF5yc5uUpL/bzwC5zZ804ltB1NpPa/PI
    sa5uwLw/YFb8XG/JCWxUJpUzS/kHUKFluqkY80U+fAmRmTEMq5wcaMhp3wRfeq+1
    G9OPBNTyqpnHe+i54QAnj1DjsHXXNL4AL1N8/TSzYTm7dkiq+EAIyRRMrZlYwije
    407ChxIp0stB84MtMShhyoSm2hgl+3zfwuaGXoJQwWiXh715kMHVTSj9zFechYd7
    5OLltoRRDyyxsf0qweTFKIgFj13Hn/bq/UJG3AcyQNvlCv1HwQnXO+hckVBB29wE
    sF8QSYk2MMGimPDYYt4ZM5tmYLxxxvGmrGhc+HWXzMeQIDAQABozEwLzAOBgNVHQ8B
    Af8EBAMCAgQwHQYDVR0OBBYEFBE9zZWhQftVLBQNATC/LHLvMb0OMA0GCSqGSIb3
    DQEBCwUAA4IBAQB7k0ySFUQu72sfj3BdNxrXSyOT4L2rADLhxxxiK0U6gHUF6eWz
    /0h6y4mNkg3NgLT3j/WclqzHXZruhWAXSF+VbAGkwcKA99xGWOcUJ+vKVYL/kDja
    gaZrxHlhTYVVmwn4F7DWhteFqhzZ89/W9Mv6p180AimF96qDU8Ez8t860HQaFkU6
    2Nw9ZMsGkvLePZZi78yVBDCWMogBMhrRVXG/xQkBajgvL5syLwFBo2kWGdC+wyWY
    U/Z+EK9UuHnn3Hkq/vXEzRVsYuaxchta0X2UNRzRq+o706l+iyLTpe6fnvW6ilOi
    e8Jcej7mzunzyjz4chN0/WVF94MtxbUkLkqP
    -----END CERTIFICATE-----

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-resource-name-is-invalid"></a>Azure-Portal-Fehler: Fehler beim Speichern des VPN-Gateways, und der Ressourcenname ist ungültig

### <a name="symptom"></a>Symptom

Wenn Sie versuchen, die Änderungen für das VPN-Gateway im Azure-Portal zu speichern, erhalten Sie die folgende Fehlermeldung: 

**Fehler beim Speichern des Gateways für virtuelle Netzwerke &lt;*Gatewayname*&gt;. Fehler: Der Ressourcenname &lt;*Zertifikatname, den Sie versuchen hochzuladen,*&gt; ist ungültig.

### <a name="cause"></a>Ursache

Dieses Problem tritt auf, da der Name des Zertifikats ein ungültiges Zeichen enthält, z.B. ein Leerzeichen. 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a>Azure-Portal-Fehler: Dateidownloadfehler 503 des VPN-Pakets

### <a name="symptom"></a>Symptom

Wenn Sie versuchen, das VPN-Clientkonfigurationspaket herunterzuladen, erhalten Sie die folgende Fehlermeldung:

**Fehler beim Herunterladen der Datei. Fehlerdetails: Fehler 503. Der Server ist ausgelastet.**
 
### <a name="solution"></a>Lösung

Dieser Fehler kann durch ein vorübergehendes Netzwerkproblem verursacht werden. Versuchen Sie, das VPN-Paket nach ein paar Minuten erneut herunterzuladen.

## <a name="azure-vpn-gateway-upgrade-all-p2s-clients-are-unable-to-connect"></a>Azure VPN Gatewayupgrade: Alle P2S-Clients können keine Verbindung herstellen

### <a name="cause"></a>Ursache

Wenn mehr als 50 Prozent der Lebensdauer des Zertifikats vorbei sind, wird das Zertifikat ausgetauscht.

### <a name="solution"></a>Lösung

Erstellen Sie neue Zertifikate, und verteilen Sie diese erneut an die VPN-Clients, um das Problem zu beheben. 

## <a name="too-many-vpn-clients-connected-at-once"></a>Zu viele VPN-Clients gleichzeitig verbunden

Für jedes VPN-Gateway ist 128 die maximale Anzahl an zulässigen Verbindungen. Sie können sich die Gesamtzahl von verbundenen Clients im Azure-Portal anzeigen lassen.

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-to-the-route-table"></a>Punkt-zu-Standort-VPN fügt fälschlicherweise eine Route für 10.0.0.0/8 zur Routingtabelle hinzu

### <a name="symptom"></a>Symptom

Wenn Sie die VPN-Verbindung auf dem Punkt-zu-Standort-Client anwählen, sollte der VPN-Client eine Route zum virtuellen Azure-Netzwerk hinzufügen. Der IP-Hilfsdienst sollte eine Route für das Subnetz der VPN-Clients hinzufügen. 

Der VPN-Clientbereich gehört zu einem kleineren Subnetz von 10.0.0.0/8, z.B. 10.0.12.0/24. Statt eine Route für 10.0.12.0/24 wird eine Route für 10.0.0.0/8 hinzugefügt, was eine höhere Priorität besitzt. 

Diese fehlerhafte Route unterbricht die Verbindung mit anderen lokalen Netzwerken, die möglicherweise zu einem anderen Subnetz innerhalb des Bereichs von 10.0.0.0/8 gehören, z.B. 10.50.0.0/24, die keine bestimmte Route definiert haben. 

### <a name="cause"></a>Ursache

Dieses Verhalten ist für Windows-Clients beabsichtigt . Wenn der Client das PPP IPCP-Protokoll verwendet, erhält der die IP-Adresse für die Tunnelschnittstelle vom Server (in diesem Fall VPN-Gateway). Allerdings verfügt der Client aufgrund einer Einschränkung im Protokoll nicht über die Subnetzmaske. Da es keine andere Möglichkeit zum Abrufen gibt, versucht der Client, die Subnetzmaske basierend auf der Klasse der IP-Adresse der Tunnelschnittstelle zu erraten. 

Aus diesem Grund wird eine Route anhand der folgenden statischen Zuordnung hinzugefügt: 

Wenn die Adresse zu Klasse A gehört, wenden Sie /8 an.

Wenn die Adresse zu Klasse B gehört, wenden Sie /16 an

Wenn die Adresse zu Klasse C gehört, wenden Sie /24 an.

## <a name="vpn-client-cannot-access-network-file-shares"></a>VPN-Client kann nicht auf die Netzwerkdateifreigaben zugreifen

### <a name="symptom"></a>Symptom

Der VPN-Client wurde mit dem virtuellen Azure-Netzwerk verbunden. Jedoch kann der Client nicht auf die Netzwerkfreigaben zugreifen.

### <a name="cause"></a>Ursache

Das SMB-Protokoll wird für den Zugriff auf Dateifreigaben verwendet. Wenn die Verbindung initiiert wird, werden die Anmeldeinformationen für die Sitzung vom VPN-Client hinzugefügt, und der Fehler tritt auf. Nachdem die Verbindung hergestellt wurde, wird der Client gezwungen, die Cache-Anmeldeinformationen für die Kerberos-Authentifizierung zu verwenden. Durch diesen Prozess werden Abfragen an das Schlüsselverteilungscenter (ein Domänencontroller) initiiert, um ein Token abzurufen. Weil der Clients eine Verbindung aus dem Internet herstellt, kann er möglicherweise den Domänencontroller nicht erreichen. Aus diesem Grund kann der Client kein Failover von Kerberos nach NTLM ausführen. 

Der Client wird nur dann nach Anmeldeinformation gefragt, wenn er ein gültiges Zertifikat (mit SAN=UPN) besitzt, das von der Domäne ausgestellt wurde, mit der der Client verbunden ist. Der Client muss auch physisch mit dem Domänennetzwerk verbunden sein. In diesem Fall versucht der Client, das Zertifikat zu verwenden, und wendet sich an den Domänencontroller. Anschließend gibt das Schlüsselverteilungscenter den Fehler „KDC_ERR_C_PRINCIPAL_UNKNOWN“ zurück. Dies zwingt den Client, ein Failover nach NTLM auszuführen. 

### <a name="solution"></a>Lösung

Deaktivieren Sie das Zwischenspeichern von Anmeldeinformationen für die Domäne aus dem folgenden Registrierungsunterschlüssel, um das Problem zu umgehen: 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set the value to 1 


## <a name="cannot-find-the-point-to-site-vpn-connection-in-windows-after-reinstalling-the-vpn-client"></a>Kann die Punkt-zu-Standort-VPN-Verbindung in Windows nach der Neuinstallation des VPN-Clients nicht finden

### <a name="symptom"></a>Symptom

Sie entfernen die Punkt-zu-Standort-VPN-Verbindung und installieren anschließend erneut den VPN-Client. In diesem Fall wurde die VPN-Verbindung nicht erfolgreich konfiguriert. Sie sehen keine VPN-Verbindung in den Einstellungen zu **Netzwerkverbindungen** in Windows.

### <a name="solution"></a>Lösung

Löschen Sie die alte VPN-Client-Konfigurationsdateien von **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, und führen Sie anschließend das VPN-Client-Installationsprogramm erneut aus, um das Problem zu beheben.

## <a name="point-to-site-vpn-client-cannot-resolve-the-fqdn-of-the-resources-in-the-local-domain"></a>Auflösung des FQDN der Ressourcen in der lokalen Domäne durch den Point-to-Site-VPN-Client nicht möglich

### <a name="symptom"></a>Symptom

Wenn der Client mithilfe einer Point-to-Site-VPN-Verbindung eine Verbindung mit Azure herstellt, kann Azure nicht den FQDN der Ressourcen in Ihrer lokalen Domäne auflösen.

### <a name="cause"></a>Ursache

Der Point-to-Site-VPN-Client verwendet Azure DNS-Server, die im virtuellen Azure-Netzwerk konfiguriert sind. Die Azure DNS-Server haben gegenüber den im Client konfigurierten lokalen DNS-Servern Vorrang, sodass alle DNS-Abfragen an die Azure DNS-Server gesendet werden. Wenn die Azure DNS-Server nicht die Datensätze für die lokalen Ressourcen beinhalten, tritt ein Fehler bei der Abfrage auf.

### <a name="solution"></a>Lösung

Stellen Sie zur Behebung des Problems sicher, dass die Azure DNS-Server, die im virtuellen Azure-Netzwerk verwendet werden, die DNS-Datensätze für lokale Ressourcen auflösen können. Hierfür können Sie DNS-Weiterleitungen oder bedingte Weiterleitungen verwenden. Weitere Informationen finden Sie unter [Namensauflösung mithilfe eines eigenen DNS-Servers](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="the-point-to-site-vpn-connection-is-established-but-you-still-cannot-connect-to-azure-resources"></a>Herstellen einer Verbindung mit Azure-Ressourcen trotz bestehender Point-to-Site-VPN-Verbindung nicht möglich 

### <a name="cause"></a>Ursache

Dieses Problem kann auftreten, wenn der VPN-Client nicht die Routen vom Azure-VPN-Gateway erhält.

### <a name="solution"></a>Lösung

Um dieses Problem zu lösen, [setzen Sie das Azure-VPN-Gateway zurück](vpn-gateway-resetgw-classic.md).

## <a name="error-the-revocation-function-was-unable-to-check-revocation-because-the-revocation-server-was-offlineerror-0x80092013"></a>Fehler: „Die Sperrfunktion konnte die Sperre nicht überprüfen, weil der Sperrserver offline war.(Fehler 0x80092013)“

### <a name="causes"></a>Ursachen
Diese Fehlermeldung tritt auf, wenn der Client nicht auf „http://crl3.digicert.com/ssca-sha2-g1.crl“ und „http://crl4.digicert.com/ssca-sha2-g1.cr“ zugreifen kann.  Für die Sperrprüfung ist Zugriff auf diese beiden Websites erforderlich.  Dieses Problem tritt in der Regel auf dem Client auf, für den der Proxyserver konfiguriert wurde. In einigen Umgebungen wird diese an der Edgefirewall verweigert, wenn die Anforderungen nicht über den Proxyserver erfolgen.

### <a name="solution"></a>Lösung

Überprüfen Sie die Proxyservereinstellungen, und stellen Sie sicher, dass der Client auf „http://crl3.digicert.com/ssca-sha2-g1.crl“ und „http://crl4.digicert.com/ssca-sha2-g1.cr“ zugreifen kann.

## <a name="vpn-client-error-the-connection-was-prevented-because-of-a-policy-configured-on-your-rasvpn-server-error-812"></a>VPN-Clientfehler: Die Verbindung wurde aufgrund einer auf dem RAS-/VPN-Server konfigurierten Richtlinie verhindert. (Fehler 812)

### <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn der für die Authentifizierung des VPN-Clients verwendete RADIUS-Server falsche Einstellungen aufweist. 

### <a name="solution"></a>Lösung

Stellen Sie sicher, dass der RADIUS-Server ordnungsgemäß konfiguriert ist. Weitere Informationen finden Sie unter [Integration der RADIUS-Authentifizierung mit dem Azure Multi-Factor Authentication-Server](../multi-factor-authentication/multi-factor-authentication-get-started-server-radius.md).

## <a name="error-405-when-you-download-root-certificate-from-vpn-gateway"></a>„Fehler 405“ beim Herunterladen des Stammzertifikats aus VPN Gateway

### <a name="cause"></a>Ursache

Das Stammzertifikat wurde nicht installiert. Das Stammzertifikat ist im **Zertifikatspeicher für vertrauenswürdige Anbieter** des Clients installiert.