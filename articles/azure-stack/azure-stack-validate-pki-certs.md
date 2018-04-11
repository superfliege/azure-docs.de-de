---
title: Überprüfen von PKI-Zertifikaten für Azure Stack für die Bereitstellung von in Azure Stack integrierten Systemen | Microsoft-Dokumentation
description: Beschreibt die Vorgehensweise zum Überprüfen von Azure Stack-PKI-Zertifikaten für in Azure Stack integrierte Systeme.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 1f234c2529213dfa7f42d6ee11b840236488065d
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="validate-azure-stack-pki-certificates"></a>Überprüfen von Azure Stack-PKI-Zertifikaten

Das in diesem Artikel beschriebene Azure Stack-Zertifikatüberprüfungstool wird vom OEM über die Datei „deploymentdata.json“ bereitgestellt und dient zum Überprüfen, ob die [generierten PKI-Zertifikate](azure-stack-get-pki-certs.md) für die Vorabbereitstellung geeignet sind. Es empfiehlt sich, Zertifikate rechtzeitig zu überprüfen, damit genügend Zeit zum Testen und ggf. zum erneuten Ausstellen bleibt.

Das Zertifikatüberprüfungstool (Certchecker) führt folgenden Überprüfungen aus:

- **Lesen der PFX-Datei:** Sucht nach einer gültigen PFX-Datei und einem korrekten Kennwort und gibt eine Warnung aus, falls öffentliche Informationen nicht durch das Kennwort geschützt sind. 
- **Signaturalgorithmus:** Überprüft, ob SHA1 als Signaturalgorithmus verwendet wird (was nicht der Fall sein darf).
- **Privater Schlüssel:** Überprüft, ob der private Schlüssel vorhanden ist und mit dem Attribut „Lokaler Computer“ exportiert wird. 
- **Vertrauenskette:** Überprüft, ob die Zertifikatkette intakt ist (auch für selbstsignierte Zertifikate). 
- **DNS-Namen:** Überprüft, ob das SAN relevante DNS-Namen für die einzelnen Endpunkte enthält oder ob ein unterstützender Platzhalter vorhanden ist. 
- **Schlüsselverwendung:** Überprüft, ob die Schlüsselverwendung eine digitale Signatur und Schlüsselverschlüsselung enthält und ob die erweiterte Schlüsselverwendung eine Serverauthentifizierung und eine Clientauthentifizierung enthält.
- **Schlüsselgröße:** Überprüft, ob die Schlüsselgröße mindestens 2048 beträgt.
- **Kettenreihenfolge:** Überprüft, ob die Reihenfolge der anderen Zertifikate, aus denen sich die Kette zusammensetzt, korrekt ist.
- **Andere Zertifikate:** Stellt sicher, dass die PFX-Datei nur das relevante Blattzertifikat und dessen Kette enthält und keine anderen Zertifikate in die Datei gepackt wurden.
- **Kein Profil:** Überprüft, ob ein neuer Benutzer die PFX-Daten ohne geladenes Benutzerprofil laden kann. Dadurch wird das Verhalten der gMSA-Konten während der Zertifikatwartung simuliert.

> [!IMPORTANT]  
> Die PFX-Datei des PKI-Zertifikats und das Kennwort müssen als vertrauliche Daten behandelt werden.

## <a name="prerequisites"></a>Voraussetzungen
Für die Überprüfung von PKI-Zertifikaten für Azure Stack muss Ihr System über Folgendes verfügen:
- CertChecker (in **PartnerToolKit** unter **\utils\certchecker**)
- Mindestens ein SSL-Zertifikat, das gemäß den [Vorbereitungsanweisungen](prepare-pki-certs.md) exportiert wurde
- DeploymentData.json
- Windows 10 oder Windows Server 2016

## <a name="perform-certificate-validation"></a>Durchführen der Zertifikatüberprüfung

Gehen Sie wie folgt vor, um die Azure Stack-PKI-Zertifikate vorzubereiten und zu überprüfen: 

1. Extrahieren Sie den Inhalt von „<partnerToolkit>\utils\certchecker“ in ein neues Verzeichnis (beispielsweise „**C:\certchecker**“).

2. Öffnen Sie PowerShell als Administrator, und wechseln Sie zum Ordner „certchecker“:

  ```powershell
  cd c:\certchecker
  ```
 
3. Erstellen Sie eine Verzeichnisstruktur für die Zertifikate. Führen Sie hierzu die folgenden PowerShell-Befehle aus:

  ```powershell 
  $directories = "ACS","ADFS","Admin Portal","ARM Admin","ARM Public","Graph","KeyVault","KeyVaultInternal","Public Portal" 
  $destination = '.\certs' 
  $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}  
  ```

  >  [!NOTE]
  >  Wenn der Identitätsanbieter für die Azure Stack-Bereitstellung Azure AD ist, entfernen Sie die Verzeichnisse **ADFS** und **Graph**. 

4. Platzieren Sie Ihre Zertifikate in den entsprechenden Verzeichnissen, die Sie im vorherigen Schritt erstellt haben: 
  - C:\certchecker\Certs\ACS\CustomerCertificate.pfx,  
  - C:\certchecker\Certs\Admin Portal\CustomerCertificate.pfx  
  - C:\certchecker\Certs\ARM Admin\CustomerCertificate.pfx  
  - Und so weiter. 

5. Kopieren Sie **deploymentdata.json** in das Verzeichnis **C:\certchecker**.

6. Führen Sie im PowerShell-Fenster die folgenden Befehle aus: 

  ```powershell
  $password = Read-Host -Prompt "Enter PFX Password" -AsSecureString 
  .\CertChecker.ps1 -CertificatePath .\Certs\ -pfxPassword $password -deploymentDataJSONPath .\DeploymentData.json  
  ```

7. In der Ausgabe sollte für alle überprüften Zertifikate und Attribute „OK“ angegeben sein: 

  ```powershell
  Starting Azure Stack Certificate Validation 1.1802.221.1
  Testing: ADFS\ContosoSSL.pfx
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Testing: KeyVaultInternal\ContosoSSL.pfx
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Testing: ACS\ContosoSSL.pfx
  WARNING: Pre-1803 certificate structure. The folder structure for Azure Stack 1803 and above is: ACSBlob, ACSQueue, ACSTable instead of ACS folder. Refer to deployment documentation for further informat
  ion.
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Detailed log can be found C:\CertChecker\CertChecker.log 
  ```

### <a name="known-issues"></a>Bekannte Probleme 
**Symptom:** Certchecker wird vorzeitig beendet, und die folgende Fehlermeldung wird angezeigt: 
> Fehler

> Detail: This command cannot be run due to the error: The directory name is invalid. (Details: Dieser Befehl kann aufgrund des folgenden Fehlers nicht ausgeführt werden: Der Verzeichnisname ist ungültig.) 

**Ursache:** „certchecker.ps1“ wird über einen eingeschränkten Ordner ausgeführt (beispielsweise „C:\temp“ oder „%temp%“). 

**Lösung:** Verschieben Sie das Certchecker-Tool in ein anderes Verzeichnis (beispielsweise „C:\CertChecker“). 


**Symptom:** Certchecker gibt eine Warnung aufgrund der Verwendung einer Zertifikatstruktur aus einer Version vor 1803 aus (siehe Beispiel in Schritt 7):

> [!WARNING]
> Pre-1803 certificate structure. The folder structure for Azure Stack 1803 and above is: ACSBlob, ACSQueue, ACSTable instead of ACS folder. For more information see the deployment documentation. (Zertifikatstruktur aus einer Version vor 1803. Ab Azure Stack 1803 wird anstelle des Ordners „ACS“ die Ordnerstruktur „ACSBlob“, „ACSQueue“ und „ACSTable“ verwendet. Weitere Informationen finden Sie in der Bereitstellungsdokumentation.)

**Ursache:** Certchecker hat die Verwendung eines einzelnen ACS-Ordners festgestellt, was für Bereitstellungen vor der Version 1803 korrekt ist. Bei Bereitstellungen ab Azure Stack 1803 muss dagegen die Ordnerstruktur „ACSTable“, „ACSQueue“ und „ACSBlob“ verwendet werden. Certchecker wurde bereits zur Unterstützung dieser Funktion aktualisiert.

**Lösung:** Bei Bereitstellungen der Version 1802 ist keine Aktion erforderlich. Bei Bereitstellungen ab der Version 1803 müssen Sie „ACS“ durch „ACSTable“, „ACSQueue“ und „ACSBlob“ ersetzen und die ACS-Zertifikate in diese Ordner kopieren.

**Symptom:** Tests werden übersprungen.

**Ursache:** Certchecker überspringt bestimmte Tests, wenn eine Abhängigkeit nicht erfüllt ist:
- „Andere Zertifikate“ wird übersprungen, wenn „Vertrauenskette“ nicht erfolgreich ist.
- „Kein Profil“ wird in folgenden Fällen übersprungen:
  - Eine Sicherheitsrichtlinie verhindert die Erstellung eines temporären Benutzers und die Ausführung von PowerShell als dieser Benutzer.
  - Die Überprüfung „Privater Schlüssel“ ist nicht erfolgreich.

**Lösung:** Folgen Sie den Toolanweisungen im Detailabschnitt unter den Tests des jeweiligen Zertifikats.


## <a name="prepare-deployment-script-certificates"></a>Vorbereiten von Bereitstellungsskriptzertifikaten 
Im letzten Schritt müssen alle vorbereiteten Zertifikate in den entsprechenden Verzeichnissen des Bereitstellungshosts platziert werden. Erstellen Sie auf Ihrem Bereitstellungshost einen Ordner namens ** Certificates\*\*, und platzieren Sie Ihre exportierten Zertifikatdateien in den entsprechenden Unterordnern, wie im Abschnitt [Erforderliche Zertifikate](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates) angegeben:

```
\Certificates
\ACS\ssl.pfx
\Admin Portal\ssl.pfx
\ARM Admin\ssl.pfx
\ARM Public\ssl.pfx
\KeyVault\ssl.pfx
\KeyVaultInternal\ssl.pfx
\Public Portal\ssl.pfx
\ADFS\ssl.pfx*
\Graph\ssl.pfx*
```

<sup>\*</sup> Die mit einem Sternchen (\*) gekennzeichneten Zertifikate sind nur erforderlich, wenn AD FS als Identitätsspeicher verwendet wird.


## <a name="next-steps"></a>Nächste Schritte
[Azure Stack-Datencenterintegration: Identität](azure-stack-integrate-identity.md)