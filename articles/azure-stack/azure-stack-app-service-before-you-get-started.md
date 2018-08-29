---
title: Vor der Bereitstellung von App Service in Azure Stack | Microsoft-Dokumentation
description: Vor der Bereitstellung von App Service in Azure Stack auszuführende Schritte
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2018
ms.author: anwestg
ms.openlocfilehash: e5fc6b5d396a45d15548cfdd8f445158147ad12f
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "41948130"
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Vor den ersten Schritten mit App Service in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Bevor Sie Azure App Service in Azure Stack bereitstellen, müssen die erforderlichen vorbereitenden Schritte in diesem Artikel ausgeführt werden.

> [!IMPORTANT]
> Wenden Sie Update 1807 auf Ihr integriertes Azure Stack-System an, oder stellen Sie das aktuelle Azure Stack Development Kit (ASDK) vor der Bereitstellung von Azure App Service 1.3 bereit.

## <a name="download-the-installer-and-helper-scripts"></a>Herunterladen des Installationsprogramms und der Hilfsskripts

1. Laden Sie die [Hilfsskripts für die Bereitstellung von App Service in Azure Stack](https://aka.ms/appsvconmashelpers) herunter.
2. Laden Sie das [Installationsprogramm für App Service in Azure Stack](https://aka.ms/appsvconmasinstaller) herunter.
3. Extrahieren Sie die Dateien aus der ZIP-Datei der Hilfsskripts. Die folgenden Dateien und Ordner werden extrahiert:

   - Common.ps1
   - Create-AADIdentityApp.ps1
   - Create-ADFSIdentityApp.ps1
   - Create-AppServiceCerts.ps1
   - Get-AzureStackRootCert.ps1
   - Remove-AppService.ps1
   - Ordner „Modules“
     - GraphAPI.psm1

## <a name="high-availability"></a>Hochverfügbarkeit

Im Update 1802 für Azure Stack wurde Unterstützung für Fehlerdomänen hinzugefügt. Neue Bereitstellungen von Azure App Service in Azure Stack werden über Fehlerdomänen hinweg verteilt und sorgen für Fehlertoleranz.

Informationen zu vorhandenen Bereitstellungen von Azure App Service in Azure Stack, die vor dem Update 1802 bereitgestellt wurden, finden Sie im Artikel [Ausgleichen eines App Service-Ressourcenanbieters über mehrere Fehlerdomänen hinweg](azure-stack-app-service-fault-domain-update.md).

Stellen Sie darüber hinaus den erforderlichen Dateiserver und die SQL Server-Instanzen in einer Hochverfügbarkeitskonfiguration bereit.

## <a name="get-certificates"></a>Abrufen von Zertifikaten

### <a name="azure-resource-manager-root-certificate-for-azure-stack"></a>Azure Resource Manager-Stammzertifikats für Azure Stack

Öffnen Sie eine PowerShell-Sitzung mit erhöhten Rechten auf einem Computer, der den privilegierten Endpunkt für das in Azure Stack integrierte System oder den Azure Stack Development Kit-Host erreichen kann.

Führen Sie das Skript *Get-AzureStackRootCert.ps1* in dem Ordner aus, in den Sie die Hilfsskripts extrahiert haben. Das Skript erstellt in dem Ordner des Skripts, das App Service zum Erstellen von Zertifikaten benötigt, ein Stammzertifikat.

Wenn Sie den folgenden PowerShell-Befehl ausführen, müssen Sie den privilegierten Endpunkt und die Anmeldeinformationen für AzureStack\CloudAdmin angeben.

```PowerShell
    Get-AzureStackRootCert.ps1
```

#### <a name="get-azurestackrootcertps1-script-parameters"></a>Parameter des Skripts „Get-AzureStackRootCert.ps1“

| Parameter | Erforderlich oder optional | Standardwert | BESCHREIBUNG |
| --- | --- | --- | --- |
| PrivilegedEndpoint | Erforderlich | AzS-ERCS01 | Privilegierter Endpunkt |
| CloudAdminCredential | Erforderlich | AzureStack\CloudAdmin | Anmeldeinformationen des Domänenkontos für Azure Stack-Cloudadministratoren |

### <a name="certificates-required-for-asdk-deployment-of-azure-app-service"></a>Erforderliche Zertifikate für die ASDK-Bereitstellung von Azure App Service

Das Skript *Create-AppServiceCerts.ps1* erstellt zusammen mit der Azure Stack-Zertifizierungsstelle die vier Zertifikate, die App Service benötigt.

| Dateiname | Zweck |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | Standard-SSL-Zertifikat für App Service |
| api.appservice.local.azurestack.external.pfx | SSL-Zertifikat für App Service-API |
| ftp.appservice.local.azurestack.external.pfx | SSL-Zertifikat für App Service-Herausgeber |
| sso.appservice.local.azurestack.external.pfx | Zertifikat der App Service-Identitätsanwendung |

Um die Zertifikate zu erstellen, gehen Sie folgendermaßen vor:

1. Melden Sie sich am Azure Stack Development Kit-Host mit dem Konto „AzureStack\AzureStackAdmin“ an.
2. Öffnen Sie eine PowerShell-Sitzung mit erhöhten Rechten.
3. Führen Sie das Skript *Create-AppServiceCerts.ps1* in dem Ordner aus, in den Sie die Hilfsskripts extrahiert haben. Dieses Skript erstellt im gleichen Ordner wie das Skript vier Zertifikate, das App Service zum Erstellen von Zertifikaten benötigt.
4. Geben Sie ein Kennwort ein, um die PFX-Dateien zu schützen, und notieren Sie das Kennwort. Sie müssen es im Installationsprogramm für App Service in Azure Stack eingeben.

#### <a name="create-appservicecertsps1-script-parameters"></a>Parameter des Skripts „Create-AppServiceCerts.ps1“

| Parameter | Erforderlich oder optional | Standardwert | BESCHREIBUNG |
| --- | --- | --- | --- |
| pfxPassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |
| DomainName | Erforderlich | local.azurestack.external | Azure Stack-Region und Domänensuffix |

### <a name="certificates-required-for-azure-stack-production-deployment-of-azure-app-service"></a>Erforderliche Zertifikate für eine Azure Stack-Produktionsbereitstellung von Azure App Service

Um den Ressourcenanbieter in der Produktion ausführen zu können, müssen Sie die folgenden Zertifikate bereitstellen:

- Standarddomänenzertifikat
- API-Zertifikat
- Veröffentlichungszertifikat
- Identitätszertifikat

#### <a name="default-domain-certificate"></a>Standarddomänenzertifikat

Das Standarddomänenzertifikat wird der Front-End-Rolle zugeordnet. Benutzeranwendungen für Platzhalter- oder Standarddomänenanforderungen an Azure App Service verwenden dieses Zertifikat. Das Zertifikat wird auch für Quellcodeverwaltungsvorgänge (Kudu) verwendet.

Das Zertifikat muss das PFX-Format aufweisen und sollte ein Platzhalterzertifikat mit drei Antragstellern sein. Durch diese Anforderung können sowohl die Standarddomäne als auch der SCM-Endpunkt für Quellcodeverwaltungsvorgänge durch ein einziges Zertifikat abgedeckt werden.

| Format | Beispiel |
| --- | --- |
| \*.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | \*.appservice.redmond.azurestack.external |
| \*.scm.appservice.<region>.<DomainName>.<extension> | \*.scm.appservice.redmond.azurestack.external |
| \*.sso.appservice.<region>.<DomainName>.<extension> | \*.sso.appservice.redmond.azurestack.external |

#### <a name="api-certificate"></a>API-Zertifikat

Das API-Zertifikat wird der Verwaltungsrolle zugeordnet. Der Ressourcenanbieter verwendet es zum Schützen von API-Aufrufen. Das Zertifikat für die Veröffentlichung muss einen Antragsteller enthalten, der dem API-DNS-Eintrag entspricht.

| Format | Beispiel |
| --- | --- |
| api.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | api.appservice.redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Veröffentlichungszertifikat

Das Zertifikat für die Herausgeberrolle schützt den FTPS-Datenverkehr für Anwendungsbesitzer, wenn diese Inhalte hochladen. Das Zertifikat für die Veröffentlichung muss einen Antragsteller enthalten, der dem FTPS-DNS-Eintrag entspricht.

| Format | Beispiel |
| --- | --- |
| ftp.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | ftp.appservice.redmond.azurestack.external |

#### <a name="identity-certificate"></a>Identitätszertifikat

Das Zertifikat für die Identitätsanwendung ermöglicht Folgendes:

- Integration zwischen dem Azure AD/AD FS-Verzeichnis (Azure Active Directory/Active Directory-Verbunddienste), Azure Stack und App Service zur Unterstützung der Integration mit dem Computeressourcenanbieter
- Szenarien mit einmaligem Anmelden für die erweiterten Entwicklertools in Azure App Service bei Azure Stack

Das Zertifikat für die Identität muss einen Antragsteller enthalten und dem folgenden Format entsprechen.

| Format | Beispiel |
| --- | --- |
| sso.appservice.\<Region\>.\<Domänenname\>.\<Erweiterung\> | sso.appservice.redmond.azurestack.external |

## <a name="virtual-network"></a>Virtuelles Netzwerk

Mit Azure App Service in Azure Stack können Sie den Ressourcenanbieter in einem vorhandenen virtuellen Netzwerk bereitstellen oder ein virtuelles Netzwerk als Teil der Bereitstellung erstellen. Die Verwendung eines vorhandenen virtuellen Netzwerks ermöglicht die Verwendung von internen IP-Adressen zum Herstellen der Verbindung mit dem Dateiserver und dem Computer mit SQL Server, die für Azure App Service in Azure Stack erforderlich sind. Das virtuelle Netzwerk muss vor der Installation von Azure App Service in Azure Stack mit dem folgenden Adressbereich und den folgenden Subnetzen konfiguriert werden:

Virtual Network - /16

Subnetze

- ControllersSubnet /24
- ManagementServersSubnet /24
- FrontEndsSubnet /24
- PublishersSubnet /24
- WorkersSubnet /21

## <a name="prepare-the-file-server"></a>Vorbereiten des Dateiservers

Azure App Service erfordert die Verwendung eines Dateiservers. Für Produktionsbereitstellungen muss der Dateiserver für Hochverfügbarkeit konfiguriert und in der Lage sein, Fehler zu beheben.

Für reine Azure Stack Development Kit-Bereitstellungen können Sie die [Azure Resource Manager-Bereitstellungsbeispielvorlage](https://aka.ms/appsvconmasdkfstemplate) verwenden, um einen konfigurierten Einzelknoten-Dateiserver bereitzustellen. Der Einzelknoten-Dateiserver wird sich in einer Arbeitsgruppe befinden.

>[!IMPORTANT]
> Wenn Sie App Service in einem vorhandenen virtuellen Netzwerk bereitstellen möchten, muss der Dateiserver in einem anderen Subnetz als App Service bereitgestellt werden.

### <a name="provision-groups-and-accounts-in-active-directory"></a>Bereitstellen von Gruppen und Konten in Active Directory

1. Erstellen Sie die folgenden globalen Active Directory-Sicherheitsgruppen:

   - FileShareOwners
   - FileShareUsers

2. Erstellen Sie die folgenden Active Directory-Konten als Dienstkonten:

   - FileShareOwner
   - FileShareUser

   Als bewährte Sicherheitsmethode wird empfohlen, eindeutige Benutzer für diese Konten (und für alle Webrollen) sowie sichere Benutzernamen und Kennwörter zu verwenden. Legen Sie die Kennwörter mit den folgenden Bedingungen fest:

   - Aktivieren Sie **Kennwort läuft nie ab**.
   - Aktivieren Sie **Benutzer kann Kennwort nicht ändern**.
   - Deaktivieren Sie **Benutzer muss Kennwort bei der nächsten Anmeldung ändern**.

3. Fügen Sie die Konten wie folgt Gruppen als Mitglieder hinzu:

   - Fügen Sie **FileShareOwner** der Gruppe **FileShareOwners** hinzu.
   - Fügen Sie **FileShareUser** der Gruppe **FileShareUsers** hinzu.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Bereitstellen von Gruppen und Konten in einer Arbeitsgruppe

>[!NOTE]
> Wenn Sie einen Dateiserver konfigurieren, führen Sie die folgenden Befehle an einer **Administratoreingabeaufforderung** aus. <br>***Verwenden Sie nicht PowerShell.***

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

>[!NOTE]
> Einige Images für SQL-IaaS-VMs sind über das Marketplace-Verwaltungsfeature verfügbar. Achten Sie darauf, immer die neueste Version der SQL-IaaS-Erweiterung herunterzuladen, bevor Sie einen virtuellen Computer mit einem Marketplace-Artikel bereitstellen. Die SQL-Images sind mit den in Azure verfügbaren SQL-VMs identisch. Für virtuelle SQL-Computer, die mit diesen Images erstellt werden, stellen die IaaS-Erweiterung und die zugehörigen Portalerweiterungen Features wie das automatische Patchen und Sicherungsfunktionen bereit.
>
Sie können für alle SQL Server-Rollen eine Standardinstanz oder eine benannte Instanz verwenden. Wenn Sie eine benannte Instanz verwenden, sollten Sie den SQL Server-Browserdienst manuell starten und Port 1434 öffnen.

>[!IMPORTANT]
> Wenn Sie App Service in einem vorhandenen virtuellen Netzwerk bereitstellen möchten, muss der SQL Server in einem anderen Subnetz als App Service und der Dateiserver bereitgestellt werden.
>

## <a name="create-an-azure-active-directory-application"></a>Erstellen einer Azure Active Directory-Anwendung

Konfigurieren Sie einen Azure AD-Dienstprinzipal zur Unterstützung folgender Vorgänge:

- Integration von VM-Skalierungsgruppen auf Workerebenen
- Einmaliges Anmelden (SSO) für das Azure Functions-Portal und erweiterte Entwicklertools

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
13. Wählen Sie **Settings**aus.
14. Klicken Sie auf **Erforderliche Berechtigungen** > **Berechtigungen erteilen** > **Ja**.

```PowerShell
    Create-AADIdentityApp.ps1
```

| Parameter | Erforderlich oder optional | Standardwert | BESCHREIBUNG |
| --- | --- | --- | --- |
| DirectoryTenantName | Erforderlich | Null | Azure AD-Mandanten-ID; Geben Sie die GUID oder Zeichenfolge an. Beispiel: myazureaaddirectory.onmicrosoft.com |
| AdminArmEndpoint | Erforderlich | Null | Azure Resource Manager-Endpunkt des Administrators. Beispiel: adminmanagement.local.azurestack.external |
| TenantARMEndpoint | Erforderlich | Null | Azure Resource Manager-Endpunkt des Mandanten Beispiel: management.local.azurestack.external |
| AzureStackAdminCredential | Erforderlich | Null | Administratoranmeldeinformationen für den Azure AD-Dienst |
| CertificateFilePath | Erforderlich | Null | **Vollständiger Pfad** zur zuvor generierten Zertifikatsdatei für die Identitätsanwendung |
| CertificatePassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |
| Environment | Optional | AzureCloud | Der Name der unterstützten Cloudumgebung, in welcher der Azure Active Directory-Zielgraphdienst verfügbar ist  Zulässige Werte: „AzureCloud“, „AzureChinaCloud“, „AzureUSGovernment“, „AzureGermanCloud“.|

## <a name="create-an-active-directory-federation-services-application"></a>Erstellen einer Active Directory-Verbunddienste-Anwendung

Für Azure Stack-Umgebungen, die durch AD FS geschützt werden, müssen Sie einen AD FS-Dienstprinzipal zur Unterstützung folgender Vorgänge konfigurieren:

- Integration von VM-Skalierungsgruppen auf Workerebenen
- Einmaliges Anmelden (SSO) für das Azure Functions-Portal und erweiterte Entwicklertools

Administratoren müssen SSO zu folgenden Zwecken konfigurieren:

- Konfigurieren eines Dienstprinzipals für die Integration von VM-Skalierungsgruppen auf Workerebenen
- Aktivieren der erweiterten Entwicklertools in App Service (Kudu)
- Aktivieren der Verwendung des Azure Functions-Portals

Folgen Sie diesen Schritten:

1. Öffnen Sie eine PowerShell-Instanz als „azurestack\AzureStackAdmin“.
2. Wechseln Sie zum Speicherort der Skripts, die im [Vorbereitungsschritt](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts) heruntergeladen und extrahiert wurden.
3. [Installieren Sie PowerShell für Azure Stack](azure-stack-powershell-install.md).
4. Führen Sie das Skript **Create-ADFSIdentityApp.ps1** aus.
5. Geben Sie im Fenster **Anmeldeinformationen** das Administratorkonto und -kennwort Ihrer AD FS-Cloud ein. Klicken Sie auf **OK**.
6. Geben Sie den Zertifikatdateipfad und das Zertifikatkennwort für das [zuvor erstellte Zertifikat](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack) ein. Das für diesen Schritt standardmäßig erstellte Zertifikat lautet **sso.appservice.local.azurestack.external.pfx**.

```PowerShell
    Create-ADFSIdentityApp.ps1
```

| Parameter | Erforderlich oder optional | Standardwert | BESCHREIBUNG |
| --- | --- | --- | --- |
| AdminArmEndpoint | Erforderlich | Null | Azure Resource Manager-Endpunkt des Administrators. Beispiel: adminmanagement.local.azurestack.external |
| PrivilegedEndpoint | Erforderlich | Null | Privilegierter Endpunkt, Beispiel: AzS-ERCS01 |
| CloudAdminCredential | Erforderlich | Null | Anmeldeinformationen des Domänenkontos für Azure Stack-Cloudadministratoren. Beispiel: Azurestack\CloudAdmin |
| CertificateFilePath | Erforderlich | Null | **Vollständiger Pfad** zur PFX-Zertifikatsdatei der Identitätsanwendung |
| CertificatePassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen eines App Service-Ressourcenanbieters zu Azure Stack](azure-stack-app-service-deploy.md)
