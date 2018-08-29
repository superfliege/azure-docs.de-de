---
title: Syslog-Weiterleitung für Azure Stack
description: Erfahren Sie, wie Azure Stack mithilfe der Syslog-Weiterleitung in Überwachungslösungen integriert wird.
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 08/14/2018
ms.author: patricka
ms.reviewer: fiseraci
keywords: ''
ms.openlocfilehash: 8e59f2e7e2fceda7f30e12571cd9e2a552f76231
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2018
ms.locfileid: "41946476"
---
# <a name="azure-stack-datacenter-integration---syslog-forwarding"></a>Integration des Azure Stack-Datencenters: Syslog-Weiterleitung

Dieser Artikel veranschaulicht die Verwendung von Syslog zur Integration von Azure Stack-Infrastruktur in bereits in Ihrem Datencenter bereitgestellte Sicherheitslösungen, z.B. ein SIEM-System (Security Informationen Event Management). Der Syslog-Kanal macht Überprüfungen, Warnungen und Sicherheitsprotokolle aus allen Komponenten der Azure Stack-Infrastruktur verfügbar. Verwenden Sie die Syslog-Weiterleitung für die Integration von Lösungen für die Sicherheitsüberwachung und/oder zum Abrufen aller Überprüfungen, Warnungen und Sicherheitsprotokolle, um sie für die Aufbewahrung zu speichern. 

Ab dem Update 1805 verfügt Azure Stack über einen integrierten Syslog-Client, der nach seiner Konfiguration Syslog-Meldungen mit der Nutzlast in CEF (Common Event Format) ausgibt. 

> [!IMPORTANT]
> Die Syslog-Weiterleitung befindet sich in der Vorschauphase. Sie darf nicht in Produktionsumgebungen eingesetzt werden. 

Das folgende Diagramm zeigt die an der Syslog-Integration beteiligten Hauptkomponenten.

![Diagramm der Syslog-Weiterleitung](media/azure-stack-integrate-security/syslog-forwarding.png)

## <a name="configuring-syslog-forwarding"></a>Konfigurieren der Syslog-Weiterleitung

Der Syslog-Client in Azure Stack unterstützt die folgenden Konfigurationen:

1. **Syslog über TCP mit gegenseitiger Authentifizierung (Client und Server) und TLS 1.2-Verschlüsselung:** In dieser Konfiguration können der Syslog-Server und der Syslog-Client die Identität des jeweils anderen anhand von Zertifikaten überprüfen. Die Meldungen werden über einen Kanal mit TLS 1.2-Verschlüsselung gesendet.

2. **Syslog über TCP mit Serverauthentifizierung und TLS 1.2-Verschlüsselung:** In dieser Konfiguration kann der Syslog-Client die Identität des Syslog-Servers anhand eines Zertifikats überprüfen. Die Meldungen werden über einen Kanal mit TLS 1.2-Verschlüsselung gesendet.

3. **Syslog über TCP ohne Verschlüsselung:** In dieser Konfiguration überprüft weder der Syslog-Server noch der Syslog-Client die Identität des jeweils anderen. Die Meldungen werden als Klartext über TCP gesendet.

4. **Syslog über UDP ohne Verschlüsselung:** In dieser Konfiguration überprüft weder der Syslog-Server noch der Syslog-Client die Identität des jeweils anderen. Die Nachrichten werden als Klartext über UDP gesendet.

> [!IMPORTANT]
> Microsoft empfiehlt dringend die Verwendung von TCP mit Authentifizierung und Verschlüsselung (Konfiguration Nr. 1 oder als äußerstes Minimum Nr 2) für Produktionsumgebungen zum Schutz vor Man-in-the-Middle- oder Lauschangriffen.

### <a name="cmdlets-to-configure-syslog-forwarding"></a>Cmdlets zum Konfigurieren der Syslog-Weiterleitung
Für das Konfigurieren der Syslog-Weiterleitung wird Zugriff auf den privilegierten Endpunkt (PEP) benötigt. Dem PEP wurden zwei PowerShell-Cmdlets zum Konfigurieren der Syslog-Weiterleitung hinzugefügt:


```powershell
### cmdlet to pass the syslog server information to the client and to configure the transport protocol, the encryption and the authentication between the client and the server

Set-SyslogServer [-ServerName <String>] [-NoEncryption] [-SkipCertificateCheck] [-SkipCNCheck] [-UseUDP] [-Remove]

### cmdlet to configure the certificate for the syslog client to authenticate with the server

Set-SyslogClient [-pfxBinary <Byte[]>] [-CertPassword <SecureString>] [-RemoveCertificate] 
```
#### <a name="cmdlets-parameters"></a>Parameter des Cmdlets

Parameter für das Cmdlet *Set-SyslogServer*:

| Parameter | BESCHREIBUNG | Typ |
|---------|---------| ---------|
| *ServerName* | FQDN oder IP-Adresse des Syslog-Servers | Zeichenfolge |
|*NoEncryption*| Erzwingen, dass der Client Syslog-Meldungen in Klartext sendet | Flag | 
|*SkipCertificateCheck*| Überspringen der Überprüfung des vom Syslog-Server beim ersten TLS-Handshake bereitgestellten Zertifikats | Flag |
|*SkipCNCheck*| Überspringen der Überprüfung des Werts für den allgemeinen Namen des vom Syslog-Server beim ersten TLS-Handshake bereitgestellten Zertifikats | Flag |
|*UseUDP*| Verwenden von Syslog mit UDP als Transportprotokoll |Flag |
|*Remove*| Entfernen der Konfiguration des Servers vom Client und Beenden der Syslog-Weiterleitung| Flag |

Parameter für das Cmdlet *Set-SyslogClient*:
| Parameter | BESCHREIBUNG | Typ |
|---------|---------| ---------|
| *pfxBinary* | PFX-Datei mit dem Zertifikat, das vom Client als Identität für die Authentifizierung beim Syslog-Server zu verwenden ist  | Byte[] |
| *CertPassword* |  Kennwort zum Importieren des privaten Schlüssels, welcher der PFX-Datei zugeordnet ist | SecureString |
|*RemoveCertificate* | Entfernen des Zertifikats vom Client | Flag|

### <a name="configuring-syslog-forwarding-with-tcp-mutual-authentication-and-tls-12-encryption"></a>Konfigurieren der Syslog-Weiterleitung mit TCP, gegenseitiger Authentifizierung und TLS 1.2-Verschlüsselung

In dieser Konfiguration leitet der Syslog-Client in Azure Stack die Meldungen über TCP mit TLS 1.2-Verschlüsselung an den Syslog-Server weiter. Während des anfänglichen Handshakes überprüft der Client, ob der Server ein gültiges und vertrauenswürdiges Zertifikat bereitstellt, und auch der Client stellt dem Server ein Zertifikat als Nachweis seiner Identität bereit. Diese Konfiguration ist die sicherste Einstellung, da sie eine vollständige Überprüfung der Identität des Clients sowie des Servers bietet und Meldungen über einen verschlüsselten Kanal sendet. 

> [!IMPORTANT]
> Microsoft empfiehlt dringend, diese Konfiguration für Produktionsumgebungen zu verwenden. 

Zum Konfigurieren der Syslog-Weiterleitung mit TCP, gegenseitiger Authentifizierung und TLS 1.2-Verschlüsselung führen Sie diese beiden Cmdlets aus:
```powershell
# Configure the server
Set-SyslogServer -ServerName <FQDN or ip address of syslog server>

# Provide certificate to the client to authenticate against the server
Set-SyslogClient -pfxBinary <Byte[] of pfx file> -CertPassword <SecureString, password for accessing the pfx file>
```
Das Clientzertifikat muss den gleichen Stamm aufweisen, wie das während der Bereitstellung von Azure Stack angegebene Zertifikat. Es muss auch einen privaten Schlüssel enthalten.

```powershell
##Example on how to set your syslog client with the ceritificate for mutual authentication. 
##Run these cmdlets from your hardware lifecycle host or privileged access workstation.

$ErcsNodeName = "<yourPEP>"
$password = ConvertTo-SecureString -String "<your cloudAdmin account password" -AsPlainText -Force
 
$cloudAdmin = "<your cloudAdmin account name>"
$CloudAdminCred = New-Object System.Management.Automation.PSCredential ($cloudAdmin, $password)
 
$certPassword = $password
$certContent = Get-Content -Path C:\cert\<yourClientCertificate>.pfx -Encoding Byte
 
$params = @{ 
    ComputerName = $ErcsNodeName 
    Credential = $CloudAdminCred 
    ConfigurationName = "PrivilegedEndpoint" 
}

$session = New-PSSession @params
 
$params = @{ 
    Session = $session 
    ArgumentList = @($certContent, $certPassword) 
}
Write-Verbose "Invoking cmdlet to set syslog client certificate..." -Verbose 
Invoke-Command @params -ScriptBlock { 
    param($CertContent, $CertPassword) 
    Set-SyslogClient -PfxBinary $CertContent -CertPassword $CertPassword 
```

### <a name="configuring-syslog-forwarding-with-tcp-server-authentication-and-tls-12-encryption"></a>Konfigurieren der Syslog-Weiterleitung mit TCP, Serverauthentifizierung und TLS 1.2-Verschlüsselung

In dieser Konfiguration leitet der Syslog-Client in Azure Stack die Meldungen über TCP mit TLS 1.2-Verschlüsselung an den Syslog-Server weiter. Während des anfänglichen Handshakes überprüft der Client auch, dass der Server ein gültiges und vertrauenswürdiges Zertifikat bereitstellt. Dies verhindert, dass der Client Meldungen an nicht vertrauenswürdige Ziele sendet.
TCP mit Authentifizierung und Verschlüsselung ist die Standardkonfiguration und stellt die minimale Sicherheitsebene dar, die Microsoft für eine Produktionsumgebung empfiehlt. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server>
```

Wenn Sie die Integration Ihres Syslog-Servers mit dem Azure Stack-Client mithilfe eines selbst signierten und/oder nicht vertrauenswürdigen Zertifikats testen möchten, können Sie diese Flags verwenden, um die Serverüberprüfung durch den Client während des anfänglichen Handshakes zu überspringen.

```powershell
 #Skip validation of the Common Name value in the server certificate. Use this flag if you provide an IP address for your syslog server
 Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -SkipCNCheck
 
 #Skip entirely the server certificate validation
 Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -SkipCertificateCheck
```
> [!IMPORTANT]
> Microsoft rät von der Verwendung des Flags „SkipCertificateCheck“ für Produktionsumgebungen ab. 


### <a name="configuring-syslog-forwarding-with-tcp-and-no-encryption"></a>Konfigurieren der Syslog-Weiterleitung mit TCP ohne Verschlüsselung

In dieser Konfiguration leitet der Syslog-Client in Azure Stack die Meldungen über TCP ohne Verschlüsselung an den Syslog-Server weiter. Der Client überprüft die Identität des Servers nicht und stellt auch die eigene Identität nicht für die Überprüfung durch den Server bereit. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -NoEncryption
```
> [!IMPORTANT]
> Microsoft rät von der Verwendung dieser Konfiguration für Produktionsumgebungen ab. 


### <a name="configuring-syslog-forwarding-with-udp-and-no-encryption"></a>Konfigurieren der Syslog-Weiterleitung mit UDP ohne Verschlüsselung

In dieser Konfiguration leitet der Syslog-Client in Azure Stack die Meldungen über UDP ohne Verschlüsselung an den Syslog-Server weiter. Der Client überprüft die Identität des Servers nicht und stellt auch die eigene Identität nicht für die Überprüfung durch den Server bereit. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -UseUDP
```
UDP ohne Verschlüsselung ist zwar am einfachsten zu konfigurieren, bietet jedoch keinen Schutz vor Man-in-the-Middle- und Lauschangriffen. 

> [!IMPORTANT]
> Microsoft rät von der Verwendung dieser Konfiguration für Produktionsumgebungen ab. 


## <a name="removing-syslog-forwarding-configuration"></a>Entfernen der Konfiguration der Syslog-Weiterleitung

So entfernen die Syslog-Serverkonfiguration vollständig und beenden die Syslog-Weiterleitung:

**Entfernen der Syslog-Serverkonfiguration vom Client**

```PowerShell  
Set-SyslogServer -Remove
```

**Entfernen des Clientzertifikats vom Client**

```PowerShell  
Set-SyslogClient -RemoveCertificate
```

## <a name="verifying-the-syslog-setup"></a>Überprüfen der Syslog-Einrichtung

Wenn Sie den Syslog-Client erfolgreich mit Ihrem Syslog-Server verbunden haben, sollten Sie in Kürze Ereignisse empfangen. Wenn keine Ereignisse eingehen, überprüfen Sie die Konfiguration Ihres Syslog-Clients, indem Sie die folgenden Cmdlets ausführen:

**Überprüfen der Serverkonfiguration auf dem Syslog-Client**

```PowerShell  
Get-SyslogServer
```

**Überprüfen der Zertifikateinrichtung auf dem Syslog-Client**

```PowerShell  
Get-SyslogClient
```

## <a name="syslog-message-schema"></a>Syslog-Meldungsschema

Die Syslog-Weiterleitung der Azure Stack-Infrastruktur sendet Meldungen im CEF-Format (Common Event Format).
Jede Syslog-Meldung ist basierend auf diesem Schema strukturiert: 

```Syslog
<Time> <Host> <CEF payload>
```

Die CEF-Nutzlast basiert auf der folgenden Struktur, die Zuordnung für die einzelnen Felder variiert jedoch je nach Art der Meldung (Windows-Ereignis, Warnung erstellt, Warnung geschlossen).

```CEF
# Common Event Format schema
CEF: <Version>|<Device Vendor>|<Device Product>|<Device Version>|<Signature ID>|<Name>|<Severity>|<Extensions>
* Version: 0.0 
* Device Vendor: Microsoft
* Device Product: Microsoft Azure Stack
* Device Version: 1.0
```

### <a name="cef-mapping-for-windows-events"></a>CEF-Zuordnung für die Windows-Ereignisse
```
* Signature ID: ProviderName:EventID
* Name: TaskName
* Severity: Level (for details, see the severity table below)
* Extension: Custom Extension Name (for details, see the Custom Extension table below)
```

Schweregradtabelle für Windows-Ereignisse: 
| CEF-Schweregradwert | Windows-Ereignisebene | Numerischer Wert |
|--------------------|---------------------| ----------------|
|0|Undefined|Wert: 0. Weist auf Protokolle auf allen Ebenen hin|
|10|Kritisch|Wert: 1. Weist auf Protokolle für eine kritische Warnung hin|
|8|Error| Wert: 2. Weist auf Protokolle für einen Fehler hin|
|5|Warnung|Wert: 3. Weist auf Protokolle für eine Warnung hin|
|2|Information|Wert: 4. Weist auf Protokolle für eine Informationsmeldung hin|
|0|Ausführlich|Wert: 5. Weist auf Protokolle auf allen Ebenen hin|

Tabelle der benutzerdefinierten Erweiterungen für Windows-Ereignisse in Azure Stack:
| Name der benutzerdefinierten Erweiterung | Beispiel für Windows-Ereignis | 
|-----------------------|---------|
|MasChannel | System|
|MasComputer | test.azurestack.contoso.com|
|MasCorrelationActivityID| C8F40D7C-3764-423B-A4FA-C994442238AF|
|MasCorrelationRelatedActivityID| C8F40D7C-3764-423B-A4FA-C994442238AF|
|MasEventData| svchost!!4132,G,0!!!!EseDiskFlushConsistency!!ESENT!!0x800000|
|MasEventDescription| Die Gruppenrichtlinieneinstellungen für den Benutzer wurden erfolgreich verarbeitet. Seit der letzten erfolgreichen Verarbeitung der Gruppenrichtlinie wurden keine Änderungen erkannt.|
|MasEventID|1501|
|MasEventRecordID|26637|
|MasExecutionProcessID | 29380|
|MasExecutionThreadID |25480|
|MasKeywords |0x8000000000000000|
|MasKeywordName |Überprüfung erfolgreich|
|MasLevel |4|
|MasOpcode |1|
|MasOpcodeName |info|
|MasProviderEventSourceName ||
|MasProviderGuid |AEA1B4FA-97D1-45F2-A64C-4D69FFFD92C9|
|MasProviderName |Microsoft-Windows-GroupPolicy|
|MasSecurityUserId |\<Windows SID\> |
|MasTask |0|
|MasTaskCategory| Prozesserstellung|
|MasUserData|KB4093112!!5112!!Installed!!0x0!!WindowsUpdateAgent Xpath: /Event/UserData/*|
|MasVersion|0|

### <a name="cef-mapping-for-alerts-created"></a>CEF-Zuordnung für Warnungen erstellt
```
* Signature ID: Microsoft Azure Stack Alert Creation : FaultTypeId
* Name: FaultTypeId : AlertId
* Severity: Alert Severity (for details, see alerts severity table below)
* Extension: Custom Extension Name (for details, see the Custom Extension table below)
```
Tabelle der Alarmschweregrade:
| Severity | Ebene |
|----------|-------|
|0|Undefined|
|10|Kritisch|
|5|Warnung|

Tabelle der benutzerdefinierten Erweiterungen für in Azure Stack erstellte Warnungen:
| Name der benutzerdefinierten Erweiterung | Beispiel | 
|-----------------------|---------|
|MasEventDescription|BESCHREIBUNG: Das Benutzerkonto \<TestUser\> wurde für \<TestDomain\> erstellt. Es stellt ein potenzielles Sicherheitsrisiko dar. – PROBLEMBEHANDLUNG: Wenden Sie sich an den Support. Die Behebung dieses Problems muss durch den Kundendienst erfolgen. Versuchen Sie nicht, dieses Problem ohne dessen Hilfe zu beheben. Bevor Sie eine Supportanfrage stellen, starten Sie anhand der Anleitung aus https://aka.ms/azurestacklogfiles den Erfassungsprozess für Protokolldateien. |

### <a name="cef-mapping-for-alerts-closed"></a>CEF-Zuordnung für Warnungen geschlossen
```
* Signature ID: Microsoft Azure Stack Alert Creation : FaultTypeId
* Name: FaultTypeId : AlertId
* Severity: Information
```

Das folgende Beispiel zeigt eine Syslog-Meldung mit CEF-Nutzlast:
```
2018:05:17:-23:59:28 -07:00 TestHost CEF:0.0|Microsoft|Microsoft Azure Stack|1.0|3|TITLE: User Account Created -- DESCRIPTION: A user account \<TestUser\> was created for \<TestDomain\>. It's a potential security risk. -- REMEDIATION: Please contact Support. Customer Assistance is required to resolve this issue. Do not try to resolve this issue without their assistance. Before you open a support request, start the log file collection process using the guidance from https://aka.ms/azurestacklogfiles|10
```
## <a name="next-steps"></a>Nächste Schritte

[Azure Stack-Wartungsrichtlinie](azure-stack-servicing-policy.md)