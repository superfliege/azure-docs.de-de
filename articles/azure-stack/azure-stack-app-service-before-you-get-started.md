---
title: Vor der Bereitstellung von App Service in Azure Stack | Microsoft-Dokumentation
description: "Vor der Bereitstellung von App Service in Azure Stack auszuführende Schritte"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: anwestg
ms.openlocfilehash: 430101c398eff85b330d15242ed1e396a277a93a
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Vor den ersten Schritten mit App Service in Azure Stack

Azure App Service in Azure Stack setzt eine Reihe von erforderlichen Schritten voraus, die vor der Bereitstellung ausgeführt werden müssen:

- Herunterladen der Hilfsskripts für Azure App Service in Azure Stack
- Erforderliche Zertifikate für Azure App Service in Azure Stack
- Vorbereiten des Dateiservers
- Vorbereiten von SQL Server
- Erstellen einer Azure Active Directory-Anwendung
- Erstellen einer Active Directory-Verbunddienste-Anwendung

## <a name="download-the-azure-app-service-on-azure-stack-helper-scripts"></a>Herunterladen der Hilfsskripts für Azure App Service in Azure Stack

1. Laden Sie die [Hilfsskripts für die Bereitstellung von App Service in Azure Stack](http://aka.ms/appsvconmasrc1helper) herunter.
2. Extrahieren Sie die Dateien aus der ZIP-Datei der Hilfsskripts. Die folgende Datei- und Ordnerstruktur wird entpackt:
  - Create-AppServiceCerts.ps1
  - Create-AADIdentityApp.ps1
  - Create-ADFSIdentityApp.ps1
  - Module
    - AzureStack.Identity.psm1
    - GraphAPI.psm1
    
## <a name="certificates-required-for-azure-app-service-on-azure-stack"></a>Erforderliche Zertifikate für Azure App Service in Azure Stack

### <a name="certificates-required-for-the-azure-stack-development-kit"></a>Erforderliche Zertifikate für das Azure Stack Development Kit

Dieses erste Skript erstellt zusammen mit der Azure Stack-Zertifizierungsstelle vier Zertifikate, die App Service benötigt.

| Dateiname | Verwenden Sie |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | Standard-SSL-Zertifikat für App Service |
| Api.appservice.local.azurestack.external.pfx | SSL-Zertifikat für App Service-API |
| ftp.appservice.local.azurestack.external.pfx | SSL-Zertifikat für App Service-Herausgeber |
| Sso.appservice.local.azurestack.external.pfx | Zertifikat der App Service-Identitätsanwendung |

Führen Sie das Skript auf dem Azure Stack Development Kit-Host aus, und stellen Sie sicher, dass Sie PowerShell als azurestack\AzureStackAdmin ausführen.

1. Führen Sie in einer als azurestack\AzureStackAdmin ausgeführten PowerShell-Sitzung das Skript „Create-AppServiceCerts.ps1“ in dem Ordner aus, in den Sie die Hilfsskripts extrahiert haben. Das Skript erstellt in dem Ordner, der vom Skript zum Erstellen der von App Service benötigten Zertifikate verwendet wird, vier Zertifikate.
2. Geben Sie ein Kennwort ein, um die PFX-Dateien zu schützen, und notieren Sie das Kennwort. Sie müssen es im Installationsprogramm für App Service in Azure Stack eingeben.

#### <a name="create-appservicecertsps1-parameters"></a>Create-AppServiceCerts.ps1 – Parameter

| Parameter | Erforderlich/optional | Standardwert | Beschreibung |
| --- | --- | --- | --- |
| pfxPassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |
| DomainName | Erforderlich | local.azurestack.external | Azure Stack-Region und Domänensuffix |
| CertificateAuthority | Erforderlich | AzS-CA01.azurestack.local | Endpunkt der Zertifizierungsstelle |

### <a name="certificates-required-for-a-production-deployment-of-azure-app-service-on-azure-stack"></a>Erforderliche Zertifikate für eine Produktionsentwicklung von Azure App Service in Azure Stack

Um den Ressourcenanbieter in der Produktion einsetzen zu können, müssen Sie die folgenden vier Zertifikate bereitstellen:

#### <a name="default-domain-certificate"></a>Standarddomänenzertifikat

Das Standarddomänenzertifikat wird der Front-End-Rolle zugeordnet. Es wird von Benutzeranwendungen für Platzhalter- oder Standarddomänenanforderungen an Azure App Service verwendet. Das Zertifikat wird auch für Quellcodeverwaltungsvorgänge (KUDU) verwendet.

Das Zertifikat muss das PFX-Format aufweisen und sollte ein Platzhalterzertifikat mit zwei Antragstellern sein. Dadurch können sowohl die Standarddomäne als auch der SCM-Endpunkt für Quellcodeverwaltungsvorgänge durch ein einziges Zertifikat abgedeckt werden.

| Format | Beispiel |
| --- | --- |
| \*.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | \*.appservice.redmond.azurestack.external |
| \*.scm.appservice.<region>.<DomainName>.<extension> | \*.appservice.scm.redmond.azurestack.external |

#### <a name="api-certificate"></a>API-Zertifikat

Das API-Zertifikat wird der Verwaltungsrolle zugeordnet und vom Ressourcenanbieter verwendet, um API-Aufrufe zu schützen. Das Zertifikat für die Veröffentlichung muss einen Antragsteller enthalten, der dem API-DNS-Eintrag entspricht:

| Format | Beispiel |
| --- | --- |
| API.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | api.appservice.redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Veröffentlichungszertifikat

Das Zertifikat für die Herausgeberrolle schützt den FTPS-Datenverkehr für Anwendungsbesitzer, wenn diese Inhalte hochladen.  Das Zertifikat für die Veröffentlichung muss einen Antragsteller enthalten, der dem FTPS-DNS-Eintrag entspricht.

| Format | Beispiel |
| --- | --- |
| ftp.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | api.appservice.redmond.azurestack.external |

#### <a name="identity-certificate"></a>Identitätszertifikat

Das Zertifikat für die Identitätsanwendung ermöglicht Folgendes:
- Integration zwischen dem AAD/ADFS-Verzeichnis, Azure Stack und App Service zur Unterstützung der Integration mit dem Computeressourcenanbieter
- Szenarien mit einmaligem Anmelden für die erweiterten Entwicklertools in Azure App Service bei Azure Stack
Das Zertifikat für die Identität muss einen Antragsteller enthalten und dem folgenden Format entsprechen:

| Format | Beispiel |
| --- | --- |
| sso.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | sso.appservice.redmond.azurestack.external |

#### <a name="extract-the-azure-stack-azure-resource-manager-root-certificate"></a>Extrahieren des Azure Resource Manager-Stammzertifikats für Azure Stack

Führen Sie in einer als azurestack\AzureStackAdmin ausgeführten PowerShell-Sitzung das Skript „Get-AzureStackRootCert.ps1“ in dem Ordner aus, in den Sie die Hilfsskripts extrahiert haben. Das Skript erstellt in dem Ordner, der vom Skript zum Erstellen der von App Service benötigten Zertifikate verwendet wird, vier Zertifikate.

| Parameter für „Get-AzureStackRootCert.ps1“ | Erforderlich/optional | Standardwert | Beschreibung |
| --- | --- | --- | --- |
| EmergencyConsole | Erforderlich | AzS-ERCS01 | Notfallkonsole auf dem privilegierten Endpunkt |
| CloudAdminCredential | Erforderlich | AzureStack\AzureStackAdmin | Anmeldeinformationen für das Domänenkonto des Azure Stack-Cloudadministrators |


## <a name="prepare-the-file-server"></a>Vorbereiten des Dateiservers

Azure App Service erfordert die Verwendung eines Dateiservers. Für Produktionsbereitstellungen muss dieser für hohe Verfügbarkeit konfiguriert und in der Lage sein, Fehler zu beheben.

Für die Verwendung mit Azure Stack Development Kit-Bereitstellungen können Sie diese ARM-Beispielbereitstellungsvorlage verwenden, um einen Einzelknoten-Dateiserver bereitzustellen: https://aka.ms/appsvconmasdkfstemplate.

### <a name="provision-groups-and-accounts-in-active-directory"></a>Bereitstellen von Gruppen und Konten in Active Directory

1. Erstellen Sie die folgenden globalen Active Directory-Sicherheitsgruppen:
    - FileShareOwners
    - FileShareUsers
2. Erstellen Sie die folgenden Active Directory-Konten als Dienstkonten:
    - FileShareOwner
    - FileShareUser

    Aus Sicherheitsgründen wird empfohlen, die Benutzer für diese Konten (und für alle Webrollen) voneinander zu trennen und sichere Benutzernamen und Kennwörter zu erzwingen.
    Legen Sie die Kennwörter mit den folgenden Bedingungen fest:
     - Aktivieren Sie **Kennwort läuft nie ab**.
     - Aktivieren Sie **Benutzer kann Kennwort nicht ändern**.
     - Deaktivieren Sie **Benutzer muss Kennwort bei der nächsten Anmeldung ändern**.
3. Fügen Sie die Konten wie folgt Gruppen als Mitglieder hinzu:
    - Fügen Sie **FileShareOwner** der Gruppe **FileShareOwners** hinzu.
    - Fügen Sie **FileShareUser** der Gruppe **FileShareUsers** hinzu.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Bereitstellen von Gruppen und Konten in einer Arbeitsgruppe

Führen Sie in einer Arbeitsgruppe net- und WMIC-Befehle aus, um Gruppen und Konten bereitzustellen.

1. Führen Sie die folgenden Befehle aus, um die Konten FileShareOwner und FileShareUser zu erstellen. Ersetzen Sie <password> durch Ihre eigenen Werte.
    - net user FileShareOwner <password> /add /expires:never /passwordchg:no
    - net user FileShareUser <password> /add /expires:never /passwordchg:no
2. Legen Sie für die Kennwörter der Konten fest, dass sie nie ablaufen, indem Sie die folgenden WMIC-Befehle ausführen:
    - WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
    - WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
3. Erstellen Sie die lokalen Gruppen FileShareUsers und FileShareOwners, und fügen Sie diesen die Konten aus dem ersten Schritt hinzu.
    - net localgroup FileShareUsers /add
    - net localgroup FileShareUsers FileShareUser /add
    - net localgroup FileShareOwners /add
    - net localgroup FileShareOwners FileShareOwner /add

### <a name="provision-the-content-share"></a>Bereitstellen der Inhaltsfreigabe

Die Inhaltsfreigabe enthält die Websiteinhalte des Mandanten. Das Verfahren zum Bereitstellen der Inhaltsfreigabe auf einem einzelnen Dateiserver ist für Active Directory- und Arbeitsgruppenumgebungen identisch, es unterscheidet sich jedoch für einen Failovercluster in Active Directory.

#### <a name="provision-the-content-share-on-a-single-file-server-ad-or-workgroup"></a>Bereitstellen der Inhaltsfreigabe auf einem einzelnen Dateiserver (AD oder Arbeitsgruppe)

Führen Sie auf einem einzelnen Dateiserver die folgenden Befehle an einer Eingabeaufforderung mit erhöhten Rechten aus. Ersetzen Sie den Wert für <C:\WebSites> durch die entsprechenden Pfade in Ihrer Umgebung.

```powershell
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=<C:\WebSites>
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="to-enable-winrm-add-the-fileshareowners-group-to-the-local-administrators-group"></a>Um WinRM zu aktivieren, fügen Sie die Gruppe FileShareOwners der lokalen Administratorgruppe hinzu.

Damit die Windows-Remoteverwaltung ordnungsgemäß funktioniert, müssen Sie die Gruppe FileShareOwners der lokalen Administratorgruppe hinzufügen.

#### <a name="active-directory"></a>Active Directory

Führen Sie die folgenden Befehle an einer Eingabeaufforderung mit erhöhten Rechten auf dem Dateiserver oder auf einem beliebigen Knoten des Dateiserver-Failoverclusters aus. Ersetzen Sie den Wert für <DOMAIN> durch den Domänennamen, den Sie verwenden möchten.

```powershell
set DOMAIN=<DOMAIN>
net localgroup Administrators %DOMAIN%\FileShareOwners /add
```

#### <a name="workgroup"></a>Arbeitsgruppe

Führen Sie den folgenden Befehl auf dem Dateiserver an einer Eingabeaufforderung mit erhöhten Rechten aus.

net localgroup Administrators FileShareOwners /add

### <a name="configure-access-control-to-the-shares"></a>Konfigurieren der Zugriffssteuerung für die Freigaben

Führen Sie die folgenden Befehle an einer Eingabeaufforderung mit erhöhten Rechten auf dem Dateiserver oder auf dem Knoten des Dateiserver-Failoverclusters, der zurzeit der Besitzer der Clusterressource ist, aus. Ersetzen Sie die kursiven Werte durch die Werte für Ihre Umgebung.

#### <a name="active-directory"></a>Active Directory
```powershell
set DOMAIN=<DOMAIN>
set WEBSITES_FOLDER=<C:\WebSites>
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

#### <a name="workgroup"></a>Arbeitsgruppe
```powershell
set WEBSITES_FOLDER=<C:\WebSites>
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

## <a name="prepare-the-sql-server"></a>Vorbereiten von SQL Server

Für die Hosting- und Messdatenbanken von Azure App Service in Azure Stack müssen Sie eine SQL Server-Instanz vorbereiten, auf der die Datenbank der Websitesruntime für Windows Azure Pack gehostet wird.

Für die Verwendung mit dem Azure Stack Development Kit können Sie SQL Express 2012 SP1 oder höher verwenden. Informationen zum Herunterladen finden Sie unter [Download SQL Server 2012 Express with SP1](https://msdn.microsoft.com/evalcenter/hh230763.aspx) (Herunterladen von SQL Server 2012 Express mit SP1).
Für die Produktion und Lösungen mit hoher Verfügbarkeit sollten Sie eine Vollversion von SQL 2012 SP1 oder höher verwenden. Informationen zum Installieren von SQL Server finden Sie unter [Installation für SQL Server 2012](http://go.microsoft.com/fwlink/?LinkId=322141).
Aktivieren der Authentifizierung im gemischten Modus
Auf die SQL Server-Instanz für Azure App Service in Azure Stack muss von allen App Service-Rollen zugegriffen werden können.
Sie können für alle SQL Server-Rollen eine Standardinstanz oder eine benannte Instanz verwenden. Wenn Sie eine benannte Instanz verwenden, sollten Sie jedoch den SQL-Browserdienst manuell starten und Port 1434 öffnen.

## <a name="create-an-azure-active-directory-application"></a>Erstellen einer Azure Active Directory-Anwendung

Konfigurieren eines Azure AD-Dienstprinzipals für Folgendes:
- Integration von VM-Skalierungsgruppen auf Workerebenen
- Einmaliges Anmelden (SSO) für das Azure Functions-Portal und erweiterte Entwicklertools

Diese Schritte gelten nur für durch Azure AD gesicherte Azure Stack-Umgebungen.

Administratoren müssen SSO zu folgenden Zwecken konfigurieren:
- Aktivieren der erweiterten Entwicklertools in App Service (Kudu)
- Aktivieren der Verwendung des Azure Functions-Portals

Folgen Sie diesen Schritten:

1. Öffnen Sie eine PowerShell-Instanz als azurestack\azurestackadmin.
2. Wechseln Sie zum Speicherort der Skripts, die im [Vorbereitungsschritt](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-app-service-deploy#download-required-components) heruntergeladen und extrahiert wurden.
3. [Installieren](azure-stack-powershell-install.md) und [konfigurieren Sie eine Azure Stack-PowerShell-Umgebung](azure-stack-powershell-configure-admin.md).
4. Führen Sie in der gleichen PowerShell-Sitzung das Skript **CreateIdentityApp.ps1** aus. Wenn Sie zur Eingabe Ihrer Azure AD-Mandanten-ID aufgefordert werden, geben Sie die Azure AD-Mandanten-ID ein, die Sie für Ihre Azure Stack-Bereitstellung verwenden, z.B.: myazurestack.onmicrosoft.com.
5. Geben Sie im Fenster **Anmeldeinformationen** das Administratorkonto und -kennwort Ihres Azure AD-Diensts ein. Klicken Sie auf **OK**.
6. Geben Sie den Zertifikatdateipfad und das Zertifikatkennwort für das [zuvor erstellte Zertifikat](azure-stack-app-service-deploy.md) ein. Das für diesen Schritt standardmäßig erstellte Zertifikat lautet sso.appservice.local.azurestack.external.pfx.
7. Das Skript erstellt eine neue Anwendung im Azure AD des Mandanten und generiert ein neues PowerShell-Skript namens **UpdateConfigOnController.ps1**. Notieren Sie sich die Anwendungs-ID, die in der PowerShell-Ausgabe zurückgegeben wird. Sie benötigen diese Informationen, um in Schritt 11 danach suchen zu können.
8. Öffnen Sie ein neues Browserfenster, und melden Sie sich als **Azure Active Directory-Dienstadministrator** beim Azure-Portal (portal.azure.com) an.
9. Öffnen Sie den Azure AD-Ressourcenanbieter.
10. Klicken Sie auf **App-Registrierungen**.
11. Suchen Sie nach der **Anwendungs-ID**, die in Schritt 7 zurückgegeben wurde. Es wird eine App Service-Anwendung aufgeführt.
12. Klicken Sie in der Liste auf **Anwendung**.
13. Klicken Sie auf **Erforderliche Berechtigungen** > **Berechtigungen erteilen** > **Ja**.

| Parameter für „CreateIdentityApp.ps1“ | Erforderlich/optional | Standardwert | Beschreibung |
| --- | --- | --- | --- |
| DirectoryTenantName | Erforderlich | Null | Azure AD-Mandanten-ID; GUID oder Zeichenfolge angeben, z.B.: myazureaaddirectory.onmicrosoft.com |
| TenantAzure Resource ManagerEndpoint | Erforderlich | management.local.azurestack.external | Der Azure Resource Manager-Endpunkt des Mandanten |
| AzureStackCredential | Erforderlich | Null | Azure AD-Administrator |
| CertificateFilePath | Erforderlich | Null | Pfad zur zuvor generierten Zertifikatsdatei für die Identitätsanwendung |
| CertificatePassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |
| DomainName | Erforderlich | local.azurestack.external | Azure Stack-Region und Domänensuffix |
| AdfsMachineName | Optional | Name des AD FS-Computers, z.B.: AzS-ADFS01.azurestack.local | Bei einer AD FS-Bereitstellung erforderlich. Kann bei einer Azure AD-Bereitstellung ignoriert werden. |

## <a name="create-an-active-directory-federation-services-application"></a>Erstellen einer Active Directory-Verbunddienste-Anwendung

Für Azure Stack-Umgebungen, die durch AD FS geschützt werden, müssen Sie einen AD FS-Dienstprinzipal für Folgendes konfigurieren:
- Integration von VM-Skalierungsgruppen auf Workerebenen
- Einmaliges Anmelden (SSO) für das Azure Functions-Portal und erweiterte Entwicklertools

Administratoren müssen SSO zu folgenden Zwecken konfigurieren:
- Konfigurieren eines Dienstprinzipals für die Integration von VM-Skalierungsgruppen auf Workerebenen
- Aktivieren der erweiterten Entwicklertools in App Service (Kudu)
- Aktivieren der Verwendung des Azure Functions-Portals

Folgen Sie diesen Schritten:

1. Öffnen Sie eine PowerShell-Instanz als azurestack\azurestackadmin.
2. Wechseln Sie zum Speicherort der Skripts, die im [Vorbereitungsschritt](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-app-service-deploy#download-required-components) heruntergeladen und extrahiert wurden.
3. [Installieren](azure-stack-powershell-install.md) und [konfigurieren Sie eine Azure Stack-PowerShell-Umgebung](azure-stack-powershell-configure-admin.md).
4.  Führen Sie in der gleichen PowerShell-Sitzung das Skript **CreateIdentityApp.ps1** aus. Wenn Sie zur Eingabe Ihrer Azure AD-Mandanten-ID aufgefordert werden, geben Sie „ADFS“ ein.
5.  Geben Sie im Fenster **Anmeldeinformationen** das Administratorkonto und -kennwort Ihres AD FS-Diensts ein. Klicken Sie auf **OK**.
6.  Geben Sie den Zertifikatdateipfad und das Zertifikatkennwort für das [zuvor erstellte Zertifikat](azure-stack-app-service-deploy.md) ein. Das für diesen Schritt standardmäßig erstellte Zertifikat lautet sso.appservice.local.azurestack.external.pfx.

| Parameter für „CreateIdentityApp.ps1“ | Erforderlich/optional | Standardwert | Beschreibung |
| --- | --- | --- | --- |
| DirectoryTenantName | Erforderlich | Null | Verwenden Sie „ADFS“ für die AD FS-Umgebung. |
| TenantAzure Resource ManagerEndpoint | Erforderlich | management.local.azurestack.external | Der Azure Resource Manager-Endpunkt des Mandanten |
| AzureStackCredential | Erforderlich | Null | Azure AD-Administrator |
| CertificateFilePath | Erforderlich | Null | Pfad zur zuvor generierten Zertifikatsdatei für die Identitätsanwendung |
| CertificatePassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |
| DomainName | Erforderlich | local.azurestack.external | Azure Stack-Region und Domänensuffix |
| AdfsMachineName | Optional | Name des AD FS-Computers, z.B.: AzS-ADFS01.azurestack.local | Bei einer AD FS-Bereitstellung erforderlich. Kann bei einer Azure AD-Bereitstellung ignoriert werden. |


## <a name="next-steps"></a>Nächste Schritte

[Installieren Sie den App Service-Ressourcenanbieter](azure-stack-app-service-deploy.md).
