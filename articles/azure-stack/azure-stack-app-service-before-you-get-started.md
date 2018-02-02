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
ms.date: 01/29/2018
ms.author: anwestg
ms.openlocfilehash: 18a671fe49b57dda3df33b58a464b300e574376f
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Vor den ersten Schritten mit App Service in Azure Stack
*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Bevor Sie Azure App Service in Azure Stack bereitstellen, müssen die erforderlichen Schritte in diesem Artikel ausgeführt werden.

## <a name="download-the-installer-and-helper-scripts"></a>Herunterladen des Installationsprogramms und der Hilfsskripts

1. Laden Sie die [Hilfsskripts für die Bereitstellung von App Service in Azure Stack](https://aka.ms/appsvconmashelpers) herunter.
2. Laden Sie das [Installationsprogramm für App Service in Azure Stack](https://aka.ms/appsvconmasinstaller) herunter.
3. Extrahieren Sie die Dateien aus der ZIP-Datei der Hilfsskripts. Die folgende Datei- und Ordnerstruktur wird entpackt:
   - Common.ps1
   - Create-AADIdentityApp.ps1
   - Create-ADFSIdentityApp.ps1
   - Create-AppServiceCerts.ps1
   - Get-AzureStackRootCert.ps1
   - Remove-AppService.ps1
   - Module
     - GraphAPI.psm1

## <a name="prepare-for-high-availability"></a>Vorbereiten für Hochverfügbarkeit

Für Azure App Service in Azure Stack kann derzeit keine Hochverfügbarkeit angeboten werden, da Azure Stack Workloads in nur einer Fehlerdomäne bereitstellt.

Wenn Sie Azure App Service in Azure Stack für Hochverfügbarkeit vorbereiten möchten, stellen Sie den erforderlichen Dateiserver und die SQL Server-Instanz in einer Hochverfügbarkeitskonfiguration bereit. Wenn Azure Stack mehrere Fehlerdomänen unterstützt, erhalten Sie von uns Anweisungen zum Aktivieren von Azure App Service in Azure Stack in einer Hochverfügbarkeitskonfiguration.


## <a name="get-certificates"></a>Abrufen von Zertifikaten

### <a name="certificates-required-for-the-azure-stack-development-kit"></a>Erforderliche Zertifikate für das Azure Stack Development Kit

Das erste Skript erstellt zusammen mit der Azure Stack-Zertifizierungsstelle vier Zertifikate, die App Service benötigt:

| Dateiname | Zweck |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | Standard-SSL-Zertifikat für App Service |
| Api.appservice.local.azurestack.external.pfx | SSL-Zertifikat für App Service-API |
| ftp.appservice.local.azurestack.external.pfx | SSL-Zertifikat für App Service-Herausgeber |
| Sso.appservice.local.azurestack.external.pfx | Zertifikat der App Service-Identitätsanwendung |

Führen Sie das Skript auf dem Azure Stack Development Kit-Host aus, und stellen Sie sicher, dass Sie PowerShell als azurestack\CloudAdmin ausführen:

1. Führen Sie in einer als azurestack\AzureStackAdmin ausgeführten PowerShell-Sitzung das Skript „Create-AppServiceCerts.ps1“ in dem Ordner aus, in den Sie die Hilfsskripts extrahiert haben. Das Skript erstellt in dem Ordner des Skripts, das App Service zum Erstellen von Zertifikaten benötigt, vier Zertifikate.
2. Geben Sie ein Kennwort ein, um die PFX-Dateien zu schützen, und notieren Sie das Kennwort. Sie müssen es im Installationsprogramm für App Service in Azure Stack eingeben.

#### <a name="create-appservicecertsps1-parameters"></a>Create-AppServiceCerts.ps1 – Parameter

| Parameter | Erforderlich oder optional | Standardwert | BESCHREIBUNG |
| --- | --- | --- | --- |
| pfxPassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |
| DomainName | Erforderlich | local.azurestack.external | Azure Stack-Region und Domänensuffix |

### <a name="certificates-required-for-a-production-deployment-of-azure-app-service-on-azure-stack"></a>Erforderliche Zertifikate für eine Produktionsentwicklung von Azure App Service in Azure Stack

Um den Ressourcenanbieter in der Produktion einsetzen zu können, müssen Sie die folgenden vier Zertifikate bereitstellen:

#### <a name="default-domain-certificate"></a>Standarddomänenzertifikat

Das Standarddomänenzertifikat wird der Front-End-Rolle zugeordnet. Benutzeranwendungen für Platzhalter- oder Standarddomänenanforderungen an Azure App Service verwenden dieses Zertifikat. Das Zertifikat wird auch für Quellcodeverwaltungsvorgänge (Kudu) verwendet.

Das Zertifikat muss das PFX-Format aufweisen und sollte ein Platzhalterzertifikat mit zwei Antragstellern sein. Dadurch können sowohl die Standarddomäne als auch der SCM-Endpunkt für Quellcodeverwaltungsvorgänge durch ein einziges Zertifikat abgedeckt werden.

| Format | Beispiel |
| --- | --- |
| \*.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | \*.appservice.redmond.azurestack.external |
| \*.scm.appservice.<region>.<DomainName>.<extension> | \*.appservice.scm.redmond.azurestack.external |

#### <a name="api-certificate"></a>API-Zertifikat

Das API-Zertifikat wird der Verwaltungsrolle zugeordnet. Der Ressourcenanbieter verwendet es zum Schützen von API-Aufrufen. Das Zertifikat für die Veröffentlichung muss einen Antragsteller enthalten, der dem API-DNS-Eintrag entspricht.

| Format | Beispiel |
| --- | --- |
| api.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | api.appservice.redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Veröffentlichungszertifikat

Das Zertifikat für die Herausgeberrolle schützt den FTPS-Datenverkehr für Anwendungsbesitzer, wenn diese Inhalte hochladen. Das Zertifikat für die Veröffentlichung muss einen Antragsteller enthalten, der dem FTPS-DNS-Eintrag entspricht.

| Format | Beispiel |
| --- | --- |
| ftp.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | api.appservice.redmond.azurestack.external |

#### <a name="identity-certificate"></a>Identitätszertifikat

Das Zertifikat für die Identitätsanwendung ermöglicht Folgendes:
- Integration zwischen dem Azure AD/AD FS-Verzeichnis (Azure Active Directory/Active Directory-Verbunddienste), Azure Stack und App Service zur Unterstützung der Integration mit dem Computeressourcenanbieter
- Szenarien mit einmaligem Anmelden für die erweiterten Entwicklertools in Azure App Service bei Azure Stack

Das Zertifikat für die Identität muss einen Antragsteller enthalten und dem folgenden Format entsprechen:

| Format | Beispiel |
| --- | --- |
| sso.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | sso.appservice.redmond.azurestack.external |

### <a name="azure-resource-manager-root-certificate-for-azure-stack"></a>Azure Resource Manager-Stammzertifikats für Azure Stack

Führen Sie in einer als azurestack\CloudAdmin ausgeführten PowerShell-Sitzung das Skript „Get-AzureStackRootCert.ps1“ in dem Ordner aus, in den Sie die Hilfsskripts extrahiert haben. Das Skript erstellt in dem Ordner des Skripts, das App Service zum Erstellen von Zertifikaten benötigt, vier Zertifikate.

| Parameter für „Get-AzureStackRootCert.ps1“ | Erforderlich oder optional | Standardwert | BESCHREIBUNG |
| --- | --- | --- | --- |
| PrivelegedEndpoint | Erforderlich | AzS-ERCS01 | Privilegierter Endpunkt |
| CloudAdminCredential | Erforderlich | AzureStack\CloudAdmin | Anmeldeinformationen des Domänenkontos für Azure Stack-Cloudadministratoren |


## <a name="prepare-the-file-server"></a>Vorbereiten des Dateiservers

Azure App Service erfordert die Verwendung eines Dateiservers. Für Produktionsbereitstellungen muss der Dateiserver für Hochverfügbarkeit konfiguriert und in der Lage sein, Fehler zu beheben.

Für reine Azure Stack Development Kit-Bereitstellungen können Sie die [Azure Resource Manager-Bereitstellungsbeispielvorlage](https://aka.ms/appsvconmasdkfstemplate) verwenden, um einen konfigurierten Einzelknoten-Dateiserver bereitzustellen. Der Einzelknoten-Dateiserver wird sich in einer Arbeitsgruppe befinden.

### <a name="provision-groups-and-accounts-in-active-directory"></a>Bereitstellen von Gruppen und Konten in Active Directory


1. Erstellen Sie die folgenden globalen Active Directory-Sicherheitsgruppen:
   - FileShareOwners
   - FileShareUsers
2. Erstellen Sie die folgenden Active Directory-Konten als Dienstkonten:
   - FileShareOwner
   - FileShareUser

   Aus Sicherheitsgründen wird empfohlen, die Benutzer für diese Konten (und für alle Webrollen) voneinander zu trennen und sichere Benutzernamen und Kennwörter zu erzwingen. Legen Sie die Kennwörter mit den folgenden Bedingungen fest:
   - Aktivieren Sie **Kennwort läuft nie ab**.
   - Aktivieren Sie **Benutzer kann Kennwort nicht ändern**.
   - Deaktivieren Sie **Benutzer muss Kennwort bei der nächsten Anmeldung ändern**.
3. Fügen Sie die Konten wie folgt Gruppen als Mitglieder hinzu:
   - Fügen Sie **FileShareOwner** der Gruppe **FileShareOwners** hinzu.
   - Fügen Sie **FileShareUser** der Gruppe **FileShareUsers** hinzu.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Bereitstellen von Gruppen und Konten in einer Arbeitsgruppe

>[!NOTE]
> Wenn Sie einen Dateiserver konfigurieren, führen Sie alle Befehle in einem Administrator-Eingabeaufforderungsfenster aus. *Verwenden Sie nicht PowerShell.*

Wenn Sie die Azure Resource Manager-Vorlage verwenden, wurden die Benutzer bereits erstellt.

1. Führen Sie die folgenden Befehle aus, um die Konten FileShareOwner und FileShareUser zu erstellen. Ersetzen Sie `<password>` durch Ihre eigenen Werte.
``` DOS
net user FileShareOwner <password> /add /expires:never /passwordchg:no
net user FileShareUser <password> /add /expires:never /passwordchg:no
```
2. Legen Sie für die Kennwörter der Konten fest, dass sie nie ablaufen, indem Sie die folgenden WMIC-Befehle ausführen:
``` DOS
WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
```
3. Erstellen Sie die lokalen Gruppen „FileShareUsers“ und „FileShareOwners“, und fügen Sie diesen die Konten aus dem ersten Schritt hinzu:
``` DOS
net localgroup FileShareUsers /add
net localgroup FileShareUsers FileShareUser /add
net localgroup FileShareOwners /add
net localgroup FileShareOwners FileShareOwner /add
```

### <a name="provision-the-content-share"></a>Bereitstellen der Inhaltsfreigabe

Die Inhaltsfreigabe enthält die Websiteinhalte des Mandanten. Das Verfahren zum Bereitstellen der Inhaltsfreigabe auf einem einzelnen Dateiserver ist für Active Directory- und Arbeitsgruppenumgebungen identisch. Für einen Failovercluster in Active Directory wird jedoch ein anderes Verfahren verwendet.

#### <a name="provision-the-content-share-on-a-single-file-server-active-directory-or-workgroup"></a>Bereitstellen der Inhaltsfreigabe auf einem einzelnen Dateiserver (Active Directory oder Arbeitsgruppe)

Führen Sie auf einem einzelnen Dateiserver die folgenden Befehle an einer Eingabeaufforderung mit erhöhten Rechten aus. Ersetzen Sie den Wert für `C:\WebSites` durch die entsprechenden Pfade in Ihrer Umgebung.

```DOS
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=C:\WebSites
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="add-the-fileshareowners-group-to-the-local-administrators-group"></a>Hinzufügen der Gruppe „FileShareOwners“ zur lokalen Administratorgruppe

Damit die Windows-Remoteverwaltung ordnungsgemäß funktioniert, müssen Sie die Gruppe „FileShareOwners“ der lokalen Administratorgruppe hinzufügen.

#### <a name="active-directory"></a>Active Directory

Führen Sie die folgenden Befehle auf dem Dateiserver an einer Eingabeaufforderung mit erhöhten Rechten oder auf einem beliebigen Dateiserver aus, der als Failoverclusterknoten fungiert. Ersetzen Sie den Wert für `<DOMAIN>` durch den Domänennamen, den Sie verwenden möchten.

```DOS
set DOMAIN=<DOMAIN>
net localgroup Administrators %DOMAIN%\FileShareOwners /add
```

#### <a name="workgroup"></a>Arbeitsgruppe

Führen Sie den folgenden Befehl auf dem Dateiserver an einer Eingabeaufforderung mit erhöhten Rechten aus:

```DOS
net localgroup Administrators FileShareOwners /add
```

### <a name="configure-access-control-to-the-shares"></a>Konfigurieren der Zugriffssteuerung für die Freigaben

Führen Sie die folgenden Befehle auf dem Dateiserver an einer Eingabeaufforderung mit erhöhten Rechten oder auf dem Failoverclusterknoten aus, der zurzeit der Besitzer der Clusterressource ist. Ersetzen Sie die kursiven Werte durch die Werte für Ihre Umgebung.

#### <a name="active-directory"></a>Active Directory
```DOS
set DOMAIN=<DOMAIN>
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

#### <a name="workgroup"></a>Arbeitsgruppe
```DOS
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

## <a name="prepare-the-sql-server-instance"></a>Vorbereiten der SQL Server-Instanz

Für die Hosting- und Messdatenbanken von Azure App Service in Azure Stack müssen Sie eine SQL Server-Instanz für die App Service-Datenbanken vorbereiten.

Für Azure Stack Development Kit-Bereitstellungen können Sie SQL Server Express 2014 SP2 oder höher verwenden.

Für die Produktion und Lösungen mit Hochverfügbarkeit sollten Sie eine Vollversion von SQL Server 2014 SP2 oder höher verwenden, Authentifizierung im gemischten Modus aktivieren und die Bereitstellung in einer [Hochverfügbarkeitskonfiguration](https://docs.microsoft.com/sql/sql-server/failover-clusters/high-availability-solutions-sql-server) durchführen.

Auf die SQL Server-Instanz für Azure App Service in Azure Stack muss von allen App Service-Rollen zugegriffen werden können. Sie können SQL Server im Standardabonnement des Anbieters in Azure Stack bereitstellen. Alternativ können Sie die vorhandene Infrastruktur in Ihrer Organisation nutzen (solange eine Verbindung mit Azure Stack besteht). Denken Sie bei Verwendung eines Azure Marketplace-Images daran, die Firewall entsprechend zu konfigurieren.

Sie können für alle SQL Server-Rollen eine Standardinstanz oder eine benannte Instanz verwenden. Wenn Sie eine benannte Instanz verwenden, sollten Sie den SQL Server-Browserdienst manuell starten und Port 1434 öffnen.

## <a name="create-an-azure-active-directory-application"></a>Erstellen einer Azure Active Directory-Anwendung

Konfigurieren eines Azure AD-Dienstprinzipals für Folgendes:
- Integration von VM-Skalierungsgruppen auf Workerebenen
- Einmaliges Anmelden (Single Sign-On, SSO) für das Azure Functions-Portal und erweiterte Entwicklertools

Diese Schritte gelten nur für durch Azure AD gesicherte Azure Stack-Umgebungen.

Administratoren müssen SSO zu folgenden Zwecken konfigurieren:
- Aktivieren der erweiterten Entwicklertools in App Service (Kudu)
- Aktivieren der Verwendung des Azure Functions-Portals

Folgen Sie diesen Schritten:

1. Öffnen Sie eine PowerShell-Instanz als „azurestack\AzureStackAdmin“.
2. Wechseln Sie zum Speicherort der Skripts, die im [Vorbereitungsschritt](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts) heruntergeladen und extrahiert wurden.
3. [Installieren Sie PowerShell für Azure Stack](azure-stack-powershell-install.md).
4. Führen Sie das Skript **Create-AADIdentityApp.ps1** aus. Geben Sie bei entsprechender Aufforderung die Azure AD-Mandanten-ID ein, die Sie für Ihre Azure Stack-Bereitstellung verwenden. Geben Sie beispielsweise **myazurestack.onmicrosoft.com** ein.
5. Geben Sie im Fenster **Anmeldeinformationen** das Administratorkonto und -kennwort Ihres Azure AD-Diensts ein. Klicken Sie auf **OK**.
6. Geben Sie den Zertifikatdateipfad und das Zertifikatkennwort für das [zuvor erstellte Zertifikat](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack) ein. Das für diesen Schritt standardmäßig erstellte Zertifikat lautet **sso.appservice.local.azurestack.external.pfx**.
7. Das Skript erstellt eine neue Anwendung in der Azure AD-Instanz des Mandanten. Notieren Sie sich die Anwendungs-ID, die in der PowerShell-Ausgabe zurückgegeben wird. Sie benötigen diese Informationen bei der Installation.
8. Öffnen Sie ein neues Browserfenster, und melden Sie sich als Azure Active Directory-Dienstadministrator beim [Azure-Portal](https://portal.azure.com) an.
9. Öffnen Sie den Azure AD-Ressourcenanbieter.
10. Klicken Sie auf **App-Registrierungen**.
11. Suchen Sie nach der Anwendungs-ID, die in Schritt 7 zurückgegeben wurde. Es wird eine App Service-Anwendung aufgeführt.
12. Klicken Sie in der Liste auf **Anwendung**.
13. Klicken Sie auf **Erforderliche Berechtigungen** > **Berechtigungen erteilen** > **Ja**.

| Parameter von „Create-AADIdentityApp.ps1“ | Erforderlich oder optional | Standardwert | BESCHREIBUNG |
| --- | --- | --- | --- |
| DirectoryTenantName | Erforderlich | Null | Azure AD-Mandanten-ID; Geben Sie die GUID oder Zeichenfolge an. Beispiel: myazureaaddirectory.onmicrosoft.com |
| AdminArmEndpoint | Erforderlich | Null | Azure Resource Manager-Endpunkt des Administrators. Beispiel: adminmanagement.local.azurestack.external |
| TenantARMEndpoint | Erforderlich | Null | Azure Resource Manager-Endpunkt des Mandanten Beispiel: management.local.azurestack.external |
| AzureStackAdminCredential | Erforderlich | Null | Administratoranmeldeinformationen für den Azure AD-Dienst |
| CertificateFilePath | Erforderlich | Null | Pfad zur zuvor generierten Zertifikatsdatei für die Identitätsanwendung |
| CertificatePassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |

## <a name="create-an-active-directory-federation-services-application"></a>Erstellen einer Active Directory-Verbunddienste-Anwendung

Für Azure Stack-Umgebungen, die durch AD FS geschützt werden, müssen Sie einen AD FS-Dienstprinzipal für Folgendes konfigurieren:
- Integration von VM-Skalierungsgruppen auf Workerebenen
- Einmaliges Anmelden (Single Sign-On, SSO) für das Azure Functions-Portal und erweiterte Entwicklertools

Administratoren müssen SSO zu folgenden Zwecken konfigurieren:
- Konfigurieren eines Dienstprinzipals für die Integration von VM-Skalierungsgruppen auf Workerebenen
- Aktivieren der erweiterten Entwicklertools in App Service (Kudu)
- Aktivieren der Verwendung des Azure Functions-Portals

Folgen Sie diesen Schritten:

1. Öffnen Sie eine PowerShell-Instanz als „azurestack\AzureStackAdmin“.
2. Wechseln Sie zum Speicherort der Skripts, die im [Vorbereitungsschritt](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts) heruntergeladen und extrahiert wurden.
3. [Installieren Sie PowerShell für Azure Stack](azure-stack-powershell-install.md).
4.  Führen Sie das Skript **Create-ADFSIdentityApp.ps1** aus.
5.  Geben Sie im Fenster **Anmeldeinformationen** das Administratorkonto und -kennwort Ihrer AD FS-Cloud ein. Klicken Sie auf **OK**.
6.  Geben Sie den Zertifikatdateipfad und das Zertifikatkennwort für das [zuvor erstellte Zertifikat](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack) ein. Das für diesen Schritt standardmäßig erstellte Zertifikat lautet **sso.appservice.local.azurestack.external.pfx**.

| Parameter von „Create-ADFSIdentityApp.ps1“ | Erforderlich oder optional | Standardwert | BESCHREIBUNG |
| --- | --- | --- | --- |
| AdminArmEndpoint | Erforderlich | Null | Azure Resource Manager-Endpunkt des Administrators. Beispiel: adminmanagement.local.azurestack.external |
| PrivilegedEndpoint | Erforderlich | Null | Privilegierter Endpunkt, Beispiel: AzS-ERCS01 |
| CloudAdminCredential | Erforderlich | Null | Anmeldeinformationen des Domänenkontos für Azure Stack-Cloudadministratoren. Beispiel: Azurestack\CloudAdmin |
| CertificateFilePath | Erforderlich | Null | Pfad zur PFX-Zertifikatdatei der Identitätsanwendung |
| CertificatePassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |


## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen eines App Service-Ressourcenanbieters zu Azure Stack](azure-stack-app-service-deploy.md)
