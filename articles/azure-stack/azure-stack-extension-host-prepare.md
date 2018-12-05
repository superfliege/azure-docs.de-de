---
title: Vorbereiten auf den Erweiterungshost für Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Vorbereitungen für den Erweiterungshost treffen, der automatisch über ein zukünftiges Azure Stack-Updatepaket aktiviert wird.
services: azure-stack
keywords: ''
author: mattbriggs
ms.author: mabrigg
ms.date: 11/27/2018
ms.topic: article
ms.service: azure-stack
ms.reviewer: thoroet
manager: femila
ms.openlocfilehash: 8de810e689a00f081df82365eca00131453a6db5
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52447111"
---
# <a name="prepare-for-extension-host-for-azure-stack"></a>Vorbereiten auf den Erweiterungshost für Azure Stack

Der Erweiterungshost sichert Azure Stack, indem die Anzahl der erforderlichen TCP/IP-Ports verringert wird. In diesem Artikel wird das Vorbereiten von Azure Stack für den Erweiterungshost, der über ein Azure Stack-Updatepaket nach dem Update 1808 automatisch aktiviert wird, beschrieben.

## <a name="certificate-requirements"></a>Zertifikatanforderungen

Der Erweiterungshost implementiert zwei neue Domänennamespaces, um eindeutige Hosteinträge für jede Portalerweiterung zu gewährleisten. Die neuen Domänennamespaces erfordern zwei zusätzliche Platzhalterzertifikate, um eine sichere Kommunikation sicherzustellen.

Die Tabelle zeigt die neuen Namespaces und die zugehörigen Zertifikate:

| Bereitstellungsordner | Erforderlicher Zertifikatantragsteller und alternative Antragstellernamen | Bereich (pro Region) | Namespace der Unterdomäne |
|-----------------------|------------------------------------------------------------------|-----------------------|------------------------------|
| Administrator-Erweiterungshost | *.adminhosting.\<Region>.\<FQDN> (SSL-Platzhalterzertifikate) | Administrator-Erweiterungshost | adminhosting.\<Region>.\<FQDN> |
| Öffentlicher Erweiterungshost | *.hosting.\<Region>.\<FQDN> (SSL-Platzhalterzertifikate) | Öffentlicher Erweiterungshost | hosting.\<region>.\<fqdn> |

Die ausführlichen Zertifikatanforderungen finden Sie im Artikel [Azure Stack-PKI-Zertifikatanforderungen](azure-stack-pki-certs.md).

## <a name="create-certificate-signing-request"></a>Erstellen der Zertifikatsignieranforderung

Das Tool Azure Stack Readiness Checker bietet die Möglichkeit, eine Zertifikatsignieranforderung für die beiden neuen, erforderlichen SSL-Zertifikate zu erstellen. Befolgen Sie die Schritte im Artikel [Generieren von Signieranforderungen für Azure Stack-Zertifikate](azure-stack-get-pki-certs.md).

> [!Note]  
> Sie können diesen Schritt abhängig davon überspringen, wie Sie Ihre SSL-Zertifikate angefordert haben.

## <a name="validate-new-certificates"></a>Überprüfen neuer Zertifikate

1. Öffnen Sie PowerShell mit erhöhten Berechtigungen auf dem Hardware-Lebenszyklushost oder dem Azure Stack-Verwaltungscomputer.
2. Führen Sie das folgende Cmdlet aus, um das Tool Azure Stack Readiness Checker zu installieren.

    ```PowerShell  
    Install-Module -Name Microsoft.AzureStack.ReadinessChecker
    ```

3. Führen Sie das folgende Skript aus, um die erforderliche Ordnerstruktur zu erstellen:

    ```PowerShell  
    New-Item C:\Certificates -ItemType Directory

    $directories = 'ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal', 'Admin extension host', 'Public extension host'

    $destination = 'c:\certificates'

    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```

    > [!Note]  
    > Wenn Sie die Bereitstellung mit Azure Active Directory-Verbunddiensten (AD FS) durchführen, müssen dem Skript unter **$directories** die folgenden Verzeichnisse hinzugefügt werden: `ADFS`, `Graph`.

4. Führen Sie die folgenden Cmdlets aus, um die Überprüfung der Zertifikate zu starten:

    ```PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
    ```

5. Platzieren Sie Ihre Zertifikate in den entsprechenden Verzeichnissen.

6. Überprüfen Sie die Ausgabe und ob alle Tests für alle Zertifikate erfolgreich waren.


## <a name="import-extension-host-certificates"></a>Importieren von Zertifikaten für den Erweiterungshost

Verwenden Sie für die folgenden Schritte einen Computer, der eine Verbindung mit dem für Azure Stack berechtigten Endpunkt herstellen kann. Vergewissern Sie sich, dass Sie Zugriff auf die neuen Zertifikatsdateien auf diesem Computer haben.

1. Verwenden Sie für die folgenden Schritte einen Computer, der eine Verbindung mit dem für Azure Stack berechtigten Endpunkt herstellen kann. Vergewissern Sie sich, dass Sie auf die neuen Zertifikatsdateien auf diesem Computer zugreifen.
2. Öffnen der PowerShell-ISE zum Ausführen der nächsten Skriptblöcke
3. Importieren Sie das Zertifikat für das Administrator-Hosting-Endpunkt.

    ```PowerShell  

    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$AdminHostingCertContent = [Byte[]](Get-Content c:\certificate\myadminhostingcertificate.pfx -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($AdminHostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($AdminHostingCertContent, $CertPassword)
            Import-AdminHostingServiceCert $AdminHostingCertContent $certPassword
    }
    ```
4. Importieren Sie das Zertifikat für den Hostingendpunkt.
    ```PowerShell  
    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$HostingCertContent = [Byte[]](Get-Content c:\certificate\myhostingcertificate.pfx  -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($HostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($HostingCertContent, $CertPassword)
            Import-UserHostingServiceCert $HostingCertContent $certPassword
    }
    ```

### <a name="update-dns-configuration"></a>Aktualisieren der DNS-Konfiguration

> [!Note]  
> Dieser Schritt ist nicht erforderlich, wenn Sie die DNS-Zonendelegierung für die DNS-Integration verwenden.
Wenn einzelne A-Datensätze des Hosts zum Veröffentlichen von Azure Stack-Endpunkten konfiguriert wurden, müssen Sie zwei zusätzliche A-Datensätze für den Host erstellen:

| IP | Hostname | Typ |
|----|------------------------------|------|
| \<IP> | *.Adminhosting.\<Region>.\<FQDN> | Eine Datei |
| \<IP> | *.Hosting.\<Region>.\<FQDN> | Eine Datei |

Die zugeordneten IP-Adressen können mithilfe des berechtigten Endpunkts durch Ausführen des Cmdlets **Get-AzureStackStampInformation** abgerufen werden.

### <a name="ports-and-protocols"></a>Ports und Protokolle

Im Artikel [Integration des Azure Stack-Datencenters – Veröffentlichen von Endpunkten](azure-stack-integrate-endpoints.md) werden die Ports und Protokolle behandelt, für die vor dem Rollout des Erweiterungshosts eine eingehende Kommunikation zum Veröffentlichen von Azure Stack erforderlich ist.

### <a name="publish-new-endpoints"></a>Veröffentlichen neuer Endpunkte

Es gibt zwei neue Endpunkte, die über die Firewall hinweg veröffentlicht werden müssen. Die zugewiesenen IP-Adressen aus dem öffentlichen VIP-Pool können mit dem Cmdlet **Get-AzureStackStampInformation** abgerufen werden.

> [!Note]  
> Nehmen Sie diese Änderung vor dem Aktivieren des Erweiterungshosts vor. Dadurch sind die Azure Stack-Portale ohne Unterbrechung erreichbar.

| Endpunkt (VIP) | Protokoll | Ports |
|----------------|----------|-------|
| AdminHosting | HTTPS | 443 |
| Hosting | HTTPS | 443 |

### <a name="update-existing-publishing-rules-post-enablement-of-extension-host"></a>Aktualisieren vorhandener Veröffentlichungsregeln (nach der Aktivierung des Erweiterungshosts)

> [!Note]  
> Das Paket für das Azure Stack-Update 1808 aktiviert den Erweiterungshost **nicht**. Es erlaubt die Vorbereitung für den Erweiterungshost durch das Importieren der erforderlichen Zertifikate. Schließen Sie keine Ports, bevor der Erweiterungshost nicht automatisch über ein Azure Stack-Updatepaket nach dem Update 1808 aktiviert wurde.

Die folgenden vorhandenen Endpunktports müssen in vorhandenen Firewallregeln geschlossen werden.

> [!Note]  
> Es wird empfohlen, die Ports erst nach einer erfolgreichen Validierung zu schließen.

| Endpunkt (VIP) | Protokoll | Ports |
|----------------------------------------|----------|-------------------------------------------------------------------------------------------------------------------------------------|
| Portal (Administrator) | HTTPS | 12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015 |
| Portal (Benutzer) | HTTPS | 12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003 |
| Azure Resource Manager (Administrator) | HTTPS | 30024 |
| Azure Resource Manager (Benutzer) | HTTPS | 30024 |

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [Firewallintegration](azure-stack-firewall.md).
- Informieren Sie sich über das [Generieren von Signieranforderungen für Azure Stack-Zertifikate](azure-stack-get-pki-certs.md).
