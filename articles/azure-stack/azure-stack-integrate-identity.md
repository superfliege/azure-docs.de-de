---
title: "Azure Stack-Datencenterintegration: Identität"
description: Erfahren Sie, wie Sie Azure Stack AD FS in AD FS des Datencenters integrieren.
services: azure-stack
author: troettinger
ms.service: azure-stack
ms.topic: article
ms.date: 10/20/2017
ms.author: victorh
keywords: 
ms.openlocfilehash: e43b9c7a854bc7150247a2b92d2d37ad6d74c705
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2017
---
# <a name="azure-stack-datacenter-integration---identity"></a>Azure Stack-Datencenterintegration: Identität

*Gilt für: Integrierte Azure Stack-Systeme*

Azure-Stack kann mithilfe von Azure Active Directory (Azure AD) oder den Active Directory-Verbunddiensten (AD FS) als Identitätsanbieter bereitgestellt werden. Diese Wahl muss vor der Bereitstellung getroffen werden. Die Bereitstellung mithilfe von AD FS wird auch als „Bereitstellen von Azure Stack im getrennten Modus“ bezeichnet.

Die folgende Tabelle zeigt die Unterschiede zwischen den Auswahlmöglichkeiten für den Identitätsanbieter:


||Physisch getrennt|Physisch verbunden|
|---------|---------|---------|
|Abrechnung|Muss „Capacity“ sein<br> Nur Enterprise Agreement (EA)|„Capacity“ oder nutzungsbasiert<br>EA oder Cloud Solution Provider (CSP)|
|Identity|Muss AD FS sein|Azure AD oder AD FS|
|Marketplace-Syndikation|Derzeit nicht verfügbar|Unterstützt<br>BYOL Lizenzierung|
|Registrierung|Empfohlen, erfordert Wechselmedium<br> und ein separates verbundenes Gerät.|Automatisiert|
|Patch und Update|Erforderlich, erfordert Wechselmedium<br> und ein separates verbundenes Gerät.|Updatepaket kann direkt aus dem Internet<br> in Azure Stack heruntergeladen werden.|

> [!IMPORTANT]
> Sie können den Identitätsanbieter nicht wechseln, ohne die gesamte Azure Stack-Lösung erneut bereitzustellen.

## <a name="active-directory-federation-services-and-graph"></a>Active Directory-Verbunddienste (AD FS) und Graph

Die Bereitstellung mit AD FS gestattet Identitäten in einer vorhandenen Active Directory-Gesamtstruktur die Authentifizierung mit Ressourcen in Azure Stack. Diese vorhandene Active Directory-Gesamtstruktur erfordert eine Bereitstellung von AD FS, um das Erstellen einer AD FS-Verbundvertrauensstellung zu ermöglichen.

Authentifizierung ist ein Teil der Identität. Damit rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) in Azure-Stack verwaltet werden kann, muss die Graph-Komponente konfiguriert werden. Wenn der Zugriff auf eine Ressource delegiert wird, sucht die Graph-Komponente das Benutzerkonto mithilfe des LDAP-Protokolls in der vorhandenen Active Directory-Gesamtstruktur.

![AD FS-Architektur von Azure Stack](media/azure-stack-integrate-identity/Azure-Stack-ADFS-architecture.png)

Die vorhandenen AD FS sind der Konto-Sicherheitstokendienst (STS), der Ansprüche an Azure Stack AD FS (den Ressourcen-STS) sendet. In Azure Stack erstellt die Automatisierung die Anspruchsanbieter-Vertrauensstellung mit dem Metadatenendpunkt für die vorhandenen AD FS.

Für die vorhandenen AD FS muss eine Vertrauensstellung der vertrauenden Seite konfiguriert werden. Dieser Schritt erfolgt nicht durch die Automatisierung und muss vom Operator konfiguriert werden. Der Azure Stack-Metadatenendpunkt wird in der Datei „AzureStackStampDeploymentInfo.JSON“ oder über den privilegierten Endpunkt durch Ausführen des Befehls `Get-AzureStackInfo` dokumentiert.

Die Vertrauensstellungskonfiguration der vertrauenden Seite erfordert außerdem, dass die Regeln für die Anspruchstransformation konfiguriert werden, die von Microsoft bereitgestellt werden.

Für die Graph-Konfiguration muss ein Dienstkonto bereitgestellt werden, das über Leseberechtigung im vorhandenen Active Directory verfügt. Dieses Konto ist als Eingabe für die Automatisierung erforderlich, um RBAC-Szenarien zu ermöglichen.

Im letzten Schritt wird ein neuer Besitzer für das Anbieterstandardabonnement konfiguriert. Dieses Konto verfügt über Vollzugriff auf alle Ressourcen, wenn es am Azure Stack-Verwaltungsportal angemeldet ist.

Anforderungen:


|Komponente|Voraussetzung|
|---------|---------|
|Graph|Microsoft Active Directory 2012/2012 R2/2016|
|AD FS|Windows Server 2012/2012 R2/2016|

## <a name="setting-up-graph-integration"></a>Einrichten der Graph-Integration

Die folgenden Informationen sind als Eingabe für die Automatisierungsparameter erforderlich:


|Parameter|Beschreibung|Beispiel|
|---------|---------|---------|
|CustomADGlobalCatalog|FQDN der Active Directory-Zielgesamtstruktur,<br>mit der die Integration erfolgen soll.|Contoso.com|
|CustomADAdminCredentials|Ein Benutzer mit LDAP-Leseberechtigung.|IHREDOMAENE\graphservice|

### <a name="create-user-account-in-the-existing-active-directory-optional"></a>Erstellen eines Benutzerkontos im vorhandenen Active Directory (optional)

Optional können Sie ein Konto für den Graph-Dienst im vorhandenen Active Directory erstellen. Führen Sie diesen Schritt aus, wenn Sie noch nicht über ein Konto verfügen, das Sie verwenden möchten.

1. Erstellen Sie im vorhandenen Active Directory das folgende Benutzerkonto (Empfehlung):
   - Benutzername: graphservice
   - Kennwort: Verwenden Sie ein sicheres Kennwort.<br>Konfigurieren Sie das Kennwort so, dass es nie abläuft.

   Es sind keine speziellen Berechtigungen oder eine Mitgliedschaft erforderlich.

**Auslösen der Automatisierung zum Konfigurieren von Graph**

Verwenden Sie für diesen Vorgang einen Computer in Ihrem Datencenternetzwerk, der mit dem privilegierten Endpunkt in Azure Stack kommunizieren kann.

2. Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Rechten (Als Administrator ausführen), und stellen Sie eine Verbindung zur IP-Adresse des privilegierten Endpunkts her. Verwenden Sie die Anmeldeinformationen für die CloudAdmin-Authentifizierung.

   ```
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

3. Da Sie nun eine Verbindung mit dem privilegierten Endpunkt hergestellt haben, führen Sie folgenden Befehle aus. Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für das Benutzerkonto ein, das Sie für die Graph-Dienst verwenden möchten (z.B. „graphservice“).

   `Register-DirectoryService -CustomADGlobalCatalog contoso.com`

   > [!IMPORTANT]
   > Warten Sie, bis die Anmeldeinformationen angezeigt werden („Get-Credential“ wird im privilegierten Endpunkt nicht unterstützt), und geben Sie die Anmeldeinformationen des Graph-Dienstkontos ein.

**Graph-Protokolle und -Ports**

Der Graph-Dienst in Azure Stack verwendet die folgenden Protokolle und Ports für die Kommunikation mit dem Ziel-Active Directory:


|Typ|Port|Protocol|
|---------|---------|---------|
|LDAP|389|TCP und UDP|
|LDAP SSL|636|TCP|
|LDAP GC|3268|TCP|
|LDAP GC SSL|3269|TCP|

## <a name="setting-up-ad-fs-integration-by-downloading-federation-metadata"></a>Einrichten der AD FS-Integration durch Herunterladen von Verbundmetadaten

Die folgenden Informationen sind als Eingabe für die Automatisierungsparameter erforderlich:


|Parameter|Beschreibung|Beispiel|
|---------|---------|---------|
|CustomAdfsName|Der Name des Anspruchsanbieters.<cr>Er wird wie hier angegeben auf der AD FS-Startseite angezeigt.|Contoso|
|CustomAD<br>FSFederationMetadataEndpointUri|Verbundmetadatenlink|https://AD01.contoso.com/FederationMetadata/2007-06/FederationMetadata.Xml|


### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Auslösen der Automatisierung zum Konfigurieren der Anspruchsanbieter-Vertrauensstellung in Azure Stack

Verwenden Sie für diesen Vorgang einen Computer, der mit dem privilegierten Endpunkt in Azure Stack kommunizieren kann. Es wird erwartet, dass Azure Stack dem vom Konto-STS AD FS verwendeten Zertifikat vertraut.

1. Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Rechten, und stellen Sie eine Verbindung mit dem privilegierten Endpunkt her.

   ```
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Da Sie nun mit dem privilegierten Endpunkt verbunden sind, führen Sie den folgenden Befehl mit den Parametern aus, die für Ihre Umgebung geeignet sind:

   `Register-CustomAdfs -CustomAdfsName Contoso -CustomADFSFederationMetadataEndpointUri https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml`

3. Führen Sie den folgenden Befehl aus, um den Besitzer des Anbieterstandardabonnements zu aktualisieren. Verwenden Sie dabei für Ihre Umgebung geeignete Parameter:

   `Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"`

## <a name="setting-up-ad-fs-integration-by-providing-federation-metadata-file"></a>Einrichten der AD FS-Integration durch Bereitstellen einer Verbundmetadatendatei

Verwenden Sie diese Methode, wenn eine der folgenden Bedingungen erfüllt ist:

- Die Zertifikatskette ist für AD FS im Vergleich zu allen anderen Endpunkten in Azure Stack unterschiedlich.
- Es gibt keine Netzwerkkonnektivität mit dem vorhandenen AD FS-Server aus der AD FS-Instanz von Azure Stack.

Die folgenden Informationen sind als Eingabe für die Automatisierungsparameter erforderlich:


|Parameter|Beschreibung|Beispiel|
|---------|---------|---------|
|CustomAdfsName|Der Name der Anspruchsanbieter-Vertrauensstellung. Er wird wie hier angegeben auf der AD FS-Startseite angezeigt.|Contoso|
|CustomADFSFederationMetadataFile|Verbundmetadatendatei|https://AD01.contoso.com/FederationMetadata/2007-06/FederationMetadata.Xml|

### <a name="create-federation-metadata-file"></a>Erstellen der Verbundmetadatendatei

Für das folgende Verfahren müssen Sie einen Computer verwenden, der über eine Netzwerkverbindung mit der vorhandenen AD FS-Bereitstellung verfügt, die zum Konto-STS wird. Darüber hinaus müssen die erforderlichen Zertifikate installiert sein.

1. Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Rechten, und führen Sie den folgenden Befehl mit den Parametern aus, die für Ihre Umgebung geeignet sind:

   ```
   [XML]$Metadata = Invoke-WebRequest -URI https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml -UseBasicParsing

   $Metadata.outerxml|out-file c:\metadata.xml
   ```

2. Kopieren Sie die Metadatendatei auf eine Freigabe, auf die vom privilegierten Endpunkt aus zugegriffen werden kann.


### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Auslösen der Automatisierung zum Konfigurieren der Anspruchsanbieter-Vertrauensstellung in Azure Stack

Verwenden Sie für diesen Vorgang einen Computer, der mit dem privilegierten Endpunkt in Azure Stack kommunizieren kann.

1. Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Rechten, und stellen Sie eine Verbindung mit dem privilegierten Endpunkt her.

   ```
   $creds=Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Da Sie nun mit dem privilegierten Endpunkt verbunden sind, führen Sie den folgenden Befehl mit den Parametern aus, die für Ihre Umgebung geeignet sind:

   `Register-CustomAdfs -CustomAdfsName Contoso – CustomADFSFederationMetadataFile \\share\metadataexample.xml`

3. Führen Sie den folgenden Befehl aus, um den Besitzer des Anbieterstandardabonnements zu aktualisieren. Verwenden Sie dabei für Ihre Umgebung geeignete Parameter:

   `Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"`

## <a name="configure-relying-party-on-existing-ad-fs-deployment-account-sts"></a>Konfigurieren der vertrauenden Seite für eine vorhandene AD FS-Bereitstellung (Konto-STS)

Microsoft stellt ein Skript zur Verfügung, das die Vertrauensstellung der vertrauenden Seite (einschließlich der Anspruchstransformationsregeln) konfiguriert. Die Verwendung des Skripts ist optional, da Sie die Befehle auch manuell ausführen können.

Sie können das Hilfsskript unter [Azure Stack-Tools](https://github.com/Azure/AzureStack-Tools/tree/vnext/DatacenterIntegration/Identity) auf Github herunterladen.

Wenn Sie die Befehle manuell ausführen möchten, gehen Sie folgendermaßen vor:

1. Kopieren Sie den folgenden Inhalt in eine TXT-Datei (z.B. unter „C:\ClaimRules.txt“ gespeichert) in Ihrer AD FS-Instanz oder Ihrem Farmmitglied des Datencenters:

   ```
   @RuleTemplate = "LdapClaims"
   @RuleName = "Name claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"), query = ";userPrincipalName;{0}", param = c.Value);

   @RuleTemplate = "LdapClaims"
   @RuleName = "UPN claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"), query = ";userPrincipalName;{0}", param = c.Value);

   @RuleTemplate = "LdapClaims"
   @RuleName = "ObjectID claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"]
   => issue(Type = "http://schemas.microsoft.com/identity/claims/objectidentifier", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, Value = c.Value, ValueType = c.ValueType);

   @RuleName = "Family Name and Given claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname", "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"), query = ";sn,givenName;{0}", param = c.Value);

   @RuleTemplate = "PassThroughClaims"
   @RuleName = "Pass through all Group SID claims"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
   => issue(claim = c);

   @RuleTemplate = "PassThroughClaims"
   @RuleName = "Pass through all windows account name claims"
   c:[Type == http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname]
   => issue(claim = c);
   ```

2. Um Windows Forms-basierte Authentifizierung zu aktivieren, öffnen Sie eine Windows PowerShell-Sitzung als Benutzer mit erhöhten Rechten, und führen Sie den folgenden Befehl aus:

   `Set-AdfsProperties -WIASupportedUserAgents @("MSAuthHost/1.0/In-Domain","MSIPC","Windows Rights Management Client","Kloud")`

3. Zum Hinzufügen der Vertrauensstellung der vertrauenden Seite führen Sie den folgenden Windows PowerShell-Befehl für Ihre AD FS-Instanz oder Ihr Farmmitglied aus. Stellen Sie sicher, dass Sie den AD FS-Endpunkt aktualisieren, und verweisen Sie auf die Datei, die Sie in Schritt 1 erstellt haben.

   **Für AD FS 2016**

   `Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true -AccessControlPolicyName "Permit everyone"`

   **Für AD FS 2012/2012 R2**

   `Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true`

   > [!IMPORTANT]
   > Sie müssen das AD FS MMC-Snap-In verwenden, um die Ausstellungsautorisierungsregeln zu konfigurieren, wenn Windows Server 2012 oder 2012 R2 AD FS verwendet wird.

4. Wenn Sie Internet Explorer oder den Edge-Browser verwenden, um auf Azure Stack zuzugreifen, müssen Sie Tokenbindungen ignorieren. Andernfalls tritt beim Anmeldeversuch ein Fehler auf. Führen Sie für Ihre AD FS-Instanz oder Ihr Farmmitglied den folgenden Befehl aus:

   `Set-AdfsProperties -IgnoreTokenBinding $true`

## <a name="spn-creation"></a>SPN-Erstellung

Es gibt viele Szenarien, die die Verwendung eines Dienstprinzipalnamens (SPN) für die Authentifizierung erfordern. Hier einige Beispiele:
- CLI-Verwendung mit der AD FS-Bereitstellung von Azure Stack
- System Center Management Pack für Azure Stack bei der Bereitstellung mit AD FS
- Ressourcenanbieter in Azure Stack bei der Bereitstellung mit AD FS
- Verschiedene Anwendungen
- Sie benötigen eine nicht-interaktive Anmeldung

Weitere Informationen zum Erstellen eines SPN finden Sie unter [Erstellen eines Dienstprinzipals für AD FS](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals#create-service-principal-for-ad-fs).


## <a name="troubleshooting"></a>Problembehandlung

### <a name="configuration-rollback"></a>Konfigurationsrollback

Wenn ein Fehler auftritt, der die Umgebung in einem Zustand hinterlässt, in dem keine Authentifizierung mehr erfolgen kann, ist eine Rollbackoption verfügbar.

1. Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Rechten, und führen Sie die folgenden Befehle aus:

   ```
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Führen Sie dann das folgende Cmdlet aus:

   `Reset-DatacenterIntegationConfiguration`

   Nachdem die Rollbackaktion ausgeführt wurde, wird für alle Änderungen an der Konfiguration ein Rollback ausgeführt. Authentifizierung ist nur mit dem integrierten CloudAdmin-Benutzer möglich.

   > [!IMPORTANT]
   > Sie müssen den ursprünglichen Besitzer des Anbieterstandardabonnements konfigurieren.

   `Set-ServiceAdminOwner -ServiceAdminOwnerUpn "azurestackadmin@[Internal Domain]"`

### <a name="collecting-additional-logs"></a>Erfassen zusätzlicher Protokolle

Wenn für eines der Cmdlets ein Fehler auftritt, können Sie zusätzliche Protokolle erfassen, indem Sie das Cmdlet `Get-Azurestacklogs` verwenden.

1. Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Rechten, und führen Sie die folgenden Befehle aus:

   ```
   $creds = Get-Credential
   Enter-pssession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Führen Sie dann das folgende Cmdlet aus:

   `Get-AzureStackLog -OutputPath \\myworstation\AzureStackLogs -FilterByRole ECE`


## <a name="next-steps"></a>Nächste Schritte

[Azure Stack datacenter integration - publish endpoints (Integration des Azure Stack-Datencenters – Veröffentlichen von Endpunkten)](azure-stack-integrate-endpoints.md)
