---
title: Manuelles Konfigurieren von in Azure Active Directory eingebundenen Hybridgeräten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure Active Directory eingebundene Hybridgeräte manuell konfigurieren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/18/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 33fc8a3822def68cc0baad4670233f57044d1985
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408406"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-joined-devices-manually"></a>Tutorial: Manuelles Konfigurieren von in Azure Active Directory eingebundenen Hybridgeräten 

Mit der Geräteverwaltung in Azure Active Directory (Azure AD) können Sie sicherstellen, dass Benutzer auf Ihre Ressourcen über Geräte zugreifen, die Ihren Standards für Sicherheit und Konformität entsprechen. Weitere Informationen finden Sie unter [Einführung in die Geräteverwaltung in Azure Active Directory](overview.md).


> [!TIP]
> Wenn die Verwendung von Azure AD Connect für Sie eine Option ist, helfen Ihnen die Informationen unter [Select your scenario](hybrid-azuread-join-plan.md#select-your-scenario) (Auswählen Ihres Szenarios) weiter. Durch die Verwendung von Azure AD Connect können Sie die Konfiguration einer Azure AD-Hybrideinbindung erheblich vereinfachen.



Wenn Sie in einer lokalen Active Directory-Umgebung Ihre in die Domäne eingebundenen Geräte in Azure AD einbinden möchten, kann dies durch Konfigurieren von in Azure AD eingebundenen Hybridgeräten erfolgen. In diesem Tutorial erfahren Sie, wie Sie die Azure AD-Hybrideinbindung für Ihre Geräte manuell konfigurieren.

> [!div class="checklist"]
> * Voraussetzungen
> * Konfigurationsschritte
> * Konfigurieren des Dienstverbindungspunkts
> * Einrichten der Ausstellung von Ansprüchen
> * Aktivieren von kompatiblen Windows-Geräten
> * Überprüfen der eingebundenen Geräte
> * Problembehandlung bei der Implementierung
 




## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie mit Folgendem vertraut sind:
    
-  [Einführung in die Geräteverwaltung in Azure Active Directory](../device-management-introduction.md)
    
-  [Planen der Implementierung einer Azure Active Directory-Hybrideinbindung](hybrid-azuread-join-plan.md)

-  [Steuern der Azure AD-Hybrideinbindung für Ihre Geräte](hybrid-azuread-join-control.md)


Bevor Sie beginnen, in Hybrid-Azure AD eingebundene Geräte in Ihrem Unternehmen zu aktivieren, müssen Sie Folgendes sicherstellen:

- Sie führen eine aktuelle Version von Azure AD Connect aus.

- Azure AD hat die Computerobjekte der Geräte für die Hybrid-Azure AD-Einbindung mit Azure AD synchronisiert. Wenn die Computerobjekte zu bestimmten Organisationseinheiten (OEs) gehören, müssen diese OEs auch für die Synchronisierung in Azure AD Connect konfiguriert werden.

  

Azure AD Connect:

- Sorgt dafür, dass die Zuordnung zwischen dem Computerkonto in Ihrer lokalen AD-Instanz (Active Directory) und dem Geräteobjekt in Azure AD beibehalten wird. 
- Ermöglicht die Verwendung von anderen gerätebezogenen Features, z.B. Windows Hello for Business.

Achten Sie darauf, dass von Computern innerhalb Ihres Unternehmensnetzwerks zur Registrierung von Computern bei Azure AD auf die folgenden URLs zugegriffen werden kann:

- https://enterpriseregistration.windows.net

- https://login.microsoftonline.com (zulassen)
- https://device.login.microsoftonline.com

- STS Ihrer Organisation (Verbunddomänen)

Wenn dies noch nicht erfolgt ist, sollte der STS Ihrer Organisation (für Verbunddomänen) in den lokalen Intraneteinstellungen des Benutzers eingefügt werden.

Wenn Ihre Organisation das nahtlose SSO verwenden möchte, müssen die folgenden URLs über die Computer innerhalb Ihrer Organisation erreichbar sein und zudem der lokalen Intranetzone des Benutzers hinzugefügt werden:

- https://autologon.microsoftazuread-sso.com

- Darüber hinaus muss die folgende Einstellung in der Intranetzone des Benutzers aktiviert werden: „Statusleistenupdates über Skript zulassen“.

Wenn Ihre Organisation die verwaltete Einrichtung (ohne Verbund) über lokales AD und nicht AD FS zum Herstellens eines Verbunds mit Azure AD verwendet, basiert die hybride Azure AD-Einbindung unter Windows 10 auf den Computerobjekten in AD, die mit Azure AD synchronisiert werden sollen. Stellen Sie sicher, dass alle Organisationseinheiten (OU), die die Computerobjekte enthalten, die hybrid in Azure AD eingebunden werden müssen, in der Azure AD Connect-Synchronisierungskonfiguration für die Synchronisierung aktiviert sind.

Wenn Ihre Organisation für Windows 10-Geräte mit Version 1703 oder einer älteren Version Zugriff auf das Internet über einen ausgehenden Proxy benötigt, müssen Sie die automatische Ermittlung von Webproxys (Web Proxy Auto-Discovery, WPAD) implementieren, damit Windows 10-Computer bei Azure AD registriert werden können. 

Ab Version 1803 von Windows 10 versucht das Gerät, die Azure AD-Hybrideinbindung unter Verwendung des synchronisierten Computers/Geräts durchzuführen, auch wenn die Azure AD-Hybrideinbindung durch ein Gerät in einer Verbunddomäne mit AD FS nicht erfolgreich war (sofern Azure AD Connect für die Synchronisierung des Computer-/Geräteobjekts mit Azure AD konfiguriert ist).

## <a name="configuration-steps"></a>Konfigurationsschritte

Sie können in Azure AD eingebundene Hybridgeräte für verschiedene Windows-Geräteplattformen konfigurieren. Dieses Thema enthält die erforderlichen Schritte für alle typischen Konfigurationsszenarien.  

Verwenden Sie die folgende Tabelle, um eine Übersicht über die Schritte zu erhalten, die für Ihr Szenario erforderlich sind:  



| Schritte                                      | Aktuelle Windows-Geräte und Kennworthashsynchronisierung | Aktuelle Windows-Geräte und Verbund | Kompatible Windows-Geräte |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Konfigurieren des Dienstverbindungspunkts | ![Prüfen][1]                            | ![Prüfen][1]                    | ![Prüfen][1]        |
| Einrichten der Ausstellung von Ansprüchen           |                                        | ![Prüfen][1]                    | ![Prüfen][1]        |
| Aktivieren von Geräten, auf denen nicht Windows 10 ausgeführt wird      |                                        |                                | ![Prüfen][1]        |
| Überprüfen der eingebundenen Geräte          | ![Prüfen][1]                            | ![Prüfen][1]                    | ![Prüfen][1]        |



## <a name="configure-service-connection-point"></a>Konfigurieren des Dienstverbindungspunkts

Das SCP-Objekt (Service Connection Point, Dienstverbindungspunkt) wird von Ihren Geräten während der Registrierung verwendet, um Informationen zum Azure AD-Mandanten zu ermitteln. In Ihrer lokalen Active Directory-Instanz (AD) muss das SCP-Objekt für die in Azure AD eingebundenen Hybridgeräte in der Partition für den Konfigurationsnamenskontext der Computergesamtstruktur vorhanden sein. Es gibt nur einen Konfigurationsnamenskontext pro Gesamtstruktur. In einer Active Directory-Konfiguration mit mehreren Gesamtstrukturen muss der Dienstverbindungspunkt in allen Gesamtstrukturen vorhanden sein, die in die Domäne eingebundene Computer enthalten.

Sie können das [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx)-Cmdlet verwenden, um den Konfigurationsnamenskontext aus Ihrer Gesamtstruktur abzurufen.  

Für eine Gesamtstruktur mit dem Active Directory-Domänennamen *fabrikam.com* lautet der Konfigurationsnamenskontext:

`CN=Configuration,DC=fabrikam,DC=com`

In Ihrer Gesamtstruktur befindet sich das SCP-Objekt für die automatische Registrierung von in die Domäne eingebundenen Geräten unter:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Je nachdem, wie Sie Azure AD Connect bereitgestellt haben, wurde das SCP-Objekt unter Umständen bereits konfiguriert.
Mit dem folgenden Windows PowerShell-Skript können Sie das Vorhandensein des Objekts überprüfen und die Ermittlungswerte abrufen: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

Die Ausgabe von **$scp.Keywords** enthält die Azure AD-Mandanteninformationen, z.B.:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Wenn der Dienstverbindungsendpunkt nicht vorhanden ist, können Sie ihn erstellen, indem Sie das `Initialize-ADSyncDomainJoinedComputerSync`-Cmdlet auf Ihrem Azure AD Connect-Server ausführen. Für die Ausführung dieses Cmdlets sind die Anmeldeinformationen eines Unternehmensadministrators erforderlich.  
Für das Cmdlet gilt Folgendes:

- Erstellt den Dienstverbindungspunkt in der Active Directory-Gesamtstruktur, mit der Azure AD Connect verbunden ist. 
- Erfordert, dass Sie den Parameter `AdConnectorAccount` angeben. Dies ist das Konto, das in Azure AD Connect als Active Directory-Connectorkonto konfiguriert ist. 


Mit dem folgenden Skript wird ein Beispiel für die Verwendung des Cmdlets veranschaulicht. In diesem Skript ist für `$aadAdminCred = Get-Credential` die Eingabe eines Benutzernamens erforderlich. Sie müssen den Benutzernamen im UPN-Format (Benutzerprinzipalname) eingeben: `user@example.com`. 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

Für das `Initialize-ADSyncDomainJoinedComputerSync`-Cmdlet gilt Folgendes:

- Es verwendet das Active Directory-PowerShell-Modul und die AD DS-Tools, die von der Ausführung von Active Directory-Webdiensten auf einem Domänencontroller abhängig sind. Active Directory-Webdienste werden auf Domänencontrollern mit Windows Server 2008 R2 und höher unterstützt.
- Es wird nur von der **MSOnline PowerShell-Modulversion 1.1.166.0** unterstützt. Dieses Modul können Sie [hier](https://msconfiggallery.cloudapp.net/packages/MSOnline/1.1.166.0/) herunterladen.   
- Wenn die AD DS-Tools nicht installiert sind, tritt bei `Initialize-ADSyncDomainJoinedComputerSync` ein Fehler auf.  Die AD DS-Tools können über Server-Manager unter „Features“ > „Remoteserver-Verwaltungstools“ > „Rollenverwaltungstools“ installiert werden.

Verwenden Sie für Domänencontroller mit Windows Server 2008 oder früher das unten angegebene Skript zum Erstellen des Dienstverbindungsendpunkts.

Bei einer Active Directory-Konfiguration mit mehreren Gesamtstrukturen sollten Sie das folgende Skript verwenden, um den Dienstverbindungsendpunkt in allen Gesamtstrukturen zu erstellen, in denen Computer vorhanden sind:
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your AD configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC
    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()

Im Skript oben

- ist `$verifiedDomain = "contoso.com"` ein Platzhalter, den Sie durch einen Ihrer überprüften Domänennamen in Azure AD ersetzen müssen. Sie müssen die Domäne besitzen, bevor Sie sie verwenden können.

Weitere Informationen zu überprüften Domänennamen finden Sie unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](../active-directory-domains-add-azure-portal.md).  
Zum Abrufen einer Liste mit Ihren überprüften Unternehmensdomänen können Sie das Cmdlet [Get-AzureADDomain](/powershell/module/Azuread/Get-AzureADDomain?view=azureadps-2.0) verwenden. 

![Get-AzureADDomain](./media/hybrid-azuread-join-manual-steps/01.png)

## <a name="setup-issuance-of-claims"></a>Einrichten der Ausstellung von Ansprüchen

Bei einer Azure AD-Verbundkonfiguration wird für Geräte AD FS (Active Directory-Verbunddienste) oder ein lokaler Drittanbieter-Verbunddienst genutzt, um die Authentifizierung bei Azure AD durchzuführen. Die Geräte führen die Authentifizierung durch, um ein Zugriffstoken für die Registrierung beim Geräteregistrierungsdienst von Azure Active Directory (Azure DRS) zu erhalten.

Für aktuelle Windows-Geräte wird die Authentifizierung per integrierter Windows-Authentifizierung gegenüber einem aktiven WS-Trust-Endpunkt (entweder Version 1.3 oder 2005) durchgeführt, der vom lokalen Verbunddienst gehostet wird.

> [!NOTE]
> Bei Verwendung von AD FS muss entweder **adfs/services/trust/13/windowstransport** oder **adfs/services/trust/2005/windowstransport** aktiviert sein. Wenn Sie einen Webauthentifizierungsproxy verwenden, stellen Sie außerdem sicher, dass dieser Endpunkt durch den Proxy veröffentlicht wird. Sie können in der AD FS-Verwaltungskonsole unter **Dienst > Endpunkte** sehen, welche Endpunkte aktiviert sind.
>
>Wenn Sie AD FS nicht als lokalen Verbunddienst nutzen, können Sie der Anleitung Ihres jeweiligen Anbieters entnehmen, ob WS-Trust 1.3- oder 2005-Endpunkte unterstützt und per MEX-Datei (Metadata Exchange) veröffentlicht werden.

Die folgenden Ansprüche müssen im Token vorhanden sein, das von Azure DRS empfangen wird, damit die Geräteregistrierung abgeschlossen werden kann. Von Azure DRS wird ein Geräteobjekt in Azure AD mit einigen dieser Informationen erstellt, die dann von Azure AD Connect verwendet werden, um das neu erstellte Geräteobjekt dem lokalen Computerkonto zuzuordnen.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Wenn Sie über mehr als einen verifizierten Domänennamen verfügen, müssen Sie für Computer den folgenden Anspruch angeben:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Falls Sie bereits einen ImmutableID-Anspruch ausstellen (z.B. alternative Anmelde-ID), müssen Sie für Computer einen entsprechenden Anspruch angeben:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

Die folgenden Abschnitte enthalten Informationen zu diesen Punkten:
 
- Werte, über die jeder Anspruch verfügen sollte
- Aussehen einer Definition in AD FS

Mithilfe der Definition können Sie überprüfen, ob die Werte vorhanden sind oder ob Sie sie erstellen müssen.

> [!NOTE]
> Wenn Sie als lokalen Verbundserver nicht AD FS nutzen, sollten Sie die Anleitung Ihres Anbieters befolgen, um die entsprechende Konfiguration zum Ausgeben dieser Ansprüche zu erstellen.

### <a name="issue-account-type-claim"></a>Ausstellen des Kontotypanspruchs

**`http://schemas.microsoft.com/ws/2012/01/accounttype`**: Dieser Anspruch muss den Wert **DJ** enthalten, mit dem das Gerät als in die Domäne eingebundener Computer identifiziert wird. In AD FS können Sie eine Ausstellungstransformationsregel hinzufügen, die wie folgt aussieht:

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a>Ausstellen der objectGUID des lokalen Computerkontos

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**: Dieser Anspruch muss den **objectGUID**-Wert des lokalen Computerkontos enthalten. In AD FS können Sie eine Ausstellungstransformationsregel hinzufügen, die wie folgt aussieht:

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a>Ausstellen der objectSID des lokalen Computerkontos

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**: Dieser Anspruch muss den **objectSid**-Wert des lokalen Computerkontos enthalten. In AD FS können Sie eine Ausstellungstransformationsregel hinzufügen, die wie folgt aussieht:

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>Ausstellen der issuerID für Computer, wenn in Azure AD mehrere verifizierte Domänennamen enthalten sind

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**: Dieser Anspruch muss den URI (Uniform Resource Identifier) von einem der verifizierten Domänennamen enthalten, für die eine Verbindung mit dem lokalen Verbunddienst (AD FS oder Drittanbieter) hergestellt wird, von dem das Token ausgestellt wird. In AD FS können Sie Ausstellungstransformationsregeln hinzufügen, die wie die unten angegebenen Beispiele aussehen (in dieser spezifischen Reihenfolge nach den obigen Regeln). Beachten Sie, dass eine Regel erforderlich ist, um die Regel explizit für Benutzer auszustellen. In den unten angegebenen Regeln wird eine erste Regel hinzugefügt, mit der die Benutzer-/Computerauthentifizierung identifiziert wird.

    @RuleName = "Issue account type with the value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


Im Anspruch oben

- ist `<verified-domain-name>` ein Platzhalter, den Sie durch einen Ihrer überprüften Domänennamen in Azure AD ersetzen müssen. Zum Beispiel: Wert = „http://contoso.com/adfs/services/trust/“



Weitere Informationen zu überprüften Domänennamen finden Sie unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](../active-directory-domains-add-azure-portal.md).  

Zum Abrufen einer Liste mit Ihren überprüften Unternehmensdomänen können Sie das [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0)-Cmdlet verwenden. 

![Get-MsolDomain](./media/hybrid-azuread-join-manual-steps/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>Ausstellen der ImmutableID für Computer, wenn ein Wert für Benutzer vorhanden ist (z.B. Angabe der alternativen Anmelde-ID)

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**: Dieser Anspruch muss einen gültigen Wert für Computer enthalten. In AD FS können Sie wie folgt eine Ausstellungstransformationsregel erstellen:

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a>Hilfsskript zum Erstellen der AD FS-Ausstellungstransformationsregeln

Das folgende Skript dient Ihnen als Hilfe beim Erstellen der oben beschriebenen Ausstellungstransformationsregeln.

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with the value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

### <a name="remarks"></a>Anmerkungen 

- Mit diesem Skript werden die Regeln an die bereits vorhandenen Regeln angefügt. Führen Sie das Skript nicht zweimal aus, weil der Regelsatz dann doppelt hinzugefügt wird. Stellen Sie sicher, dass keine entsprechenden Regeln für diese Ansprüche vorhanden sind (unter den jeweiligen Bedingungen), bevor Sie das Skript erneut ausführen.

- Wenn Sie über mehrere verifizierte Domänennamen verfügen (wie im Azure AD-Portal oder per Get-MsolDomains-Cmdlet angegeben), legen Sie den Wert von **$multipleVerifiedDomainNames** im Skript auf **$true** fest. Stellen Sie außerdem sicher, dass Sie alle vorhandenen issuerid-Ansprüche entfernen, die unter Umständen von Azure AD Connect oder auf anderem Wege erstellt wurden. Hier ist ein Beispiel für diese Regel angegeben:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Wenn Sie bereits einen **ImmutableID**-Anspruch für Benutzerkonten ausgestellt haben, legen Sie den Wert von **$immutableIDAlreadyIssuedforUsers** im Skript auf **$true** fest.

## <a name="enable-windows-down-level-devices"></a>Aktivieren von kompatiblen Windows-Geräten

Wenn es sich bei einigen Ihrer in die Domäne eingebundenen Geräte um kompatible Windows-Geräte handelt, gehen Sie wie folgt vor:

- Legen Sie eine Richtlinie in Azure AD fest, um Benutzern das Registrieren von Geräten zu ermöglichen.
 
- Konfigurieren Sie Ihren lokalen Verbunddienst für das Ausstellen von Ansprüchen, um die **Integrierte Windows-Authentifizierung (IWA)** für die Geräteregistrierung zu unterstützen.
 
- Fügen Sie den Endpunkt für die Azure AD-Geräteauthentifizierung den lokalen Intranetzonen hinzu, um beim Authentifizieren des Geräts Zertifikataufforderungen zu vermeiden.

### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Festlegen einer Richtlinie in Azure AD, um Benutzern das Registrieren von Geräten zu ermöglichen

Zum Registrieren von kompatiblen Windows-Geräten müssen Sie sicherstellen, dass die Einstellung festgelegt ist, mit der Benutzer Geräte in Azure AD registrieren können. Im Azure-Portal finden Sie diese Einstellung unter:

`Azure Active Directory > Users and groups > Device settings`
    
Die folgende Richtlinie muss auf **Alle** festgelegt sein: **Benutzer dürfen ihre Geräte für Azure AD registrieren**

![Registrieren von Geräten](./media/hybrid-azuread-join-manual-steps/23.png)


### <a name="configure-on-premises-federation-service"></a>Konfigurieren eines lokalen Verbunddiensts 

Ihr lokaler Verbunddienst muss das Ausstellen der Ansprüche **authenticationmehod** und **wiaormultiauthn** unterstützen, wenn eine Authentifizierungsanforderung an die vertrauende Seite von Azure AD empfangen wird, die wie unten gezeigt den Parameter „resource_params“ mit einem codierten Wert enthält:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Wenn eine Anforderung dieser Art eingeht, muss der lokale Verbunddienst den Benutzer per integrierter Windows-Authentifizierung authentifizieren und die folgenden beiden Ansprüche ausstellen, sofern der Vorgang erfolgreich ist:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

In AD FS müssen Sie eine Ausstellungstransformationsregel hinzufügen, die die Authentifizierungsmethode weitergibt.  

**Gehen Sie wie folgt vor, um diese Regel hinzuzufügen:**

1. Navigieren Sie in der AD FS-Verwaltungskonsole zu `AD FS > Trust Relationships > Relying Party Trusts`.
2. Klicken Sie mit der rechten Maustaste auf das Vertrauensstellungsobjekt der vertrauenden Seite, „Microsoft Office 365 Identity Platform“, und wählen Sie dann **Anspruchsregeln bearbeiten** aus.
3. Wählen Sie auf der Registerkarte **Ausstellungstransformationsregeln** die Option **Regel hinzufügen** aus.
4. Wählen Sie in der Vorlagenliste **Anspruchsregel** die Option **Ansprüche mit benutzerdefinierter Regel senden** aus.
5. Wählen Sie **Weiter**.
6. Geben Sie in das Feld **Anspruchsregelname** den Text **Anspruchsregel für Authentifizierungsmethode** ein.
7. Geben Sie im Feld **Anspruchsregel** die folgende Regel ein:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. Geben Sie auf Ihrem Verbundserver den hier dargestellten PowerShell-Befehl ein, nachdem Sie **\<RPObjectName\>** durch den Objektnamen der vertrauenden Seite für Ihr Azure AD-Vertrauensstellungsobjekt der vertrauenden Seite ersetzt haben. Dieses Objekt trägt normalerweise den Namen **Microsoft Office 365 Identity Platform**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a>Hinzufügen des Endpunkts für die Azure AD-Geräteauthentifizierung zu den lokalen Intranetzonen

Um die Anzeige von Zertifikataufforderungen zu vermeiden, wenn Benutzer beim Registrieren von Geräten die Authentifizierung für Azure AD durchführen, können Sie eine Richtlinie auf Ihre in die Domäne eingebundenen Geräte übertragen, um die folgende URL in Internet Explorer den lokalen Intranetzonen hinzuzufügen:

`https://device.login.microsoftonline.com`


## <a name="verify-joined-devices"></a>Überprüfen der eingebundenen Geräte

Sie können die erfolgreiche Einbindung für die Geräte Ihrer Organisation überprüfen, indem Sie das Cmdlet [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) im [Azure Active Directory PowerShell-Modul](/powershell/azure/install-msonlinev1?view=azureadps-2.0) verwenden.

In der Ausgabe dieses Cmdlets werden Geräte angezeigt, die in Azure AD registriert und eingebunden sind. Verwenden Sie zum Abrufen aller Geräte den Parameter **-All**, und filtern Sie sie anschließend mit der **deviceTrustType**-Eigenschaft. In die Domäne eingebundene Geräte weisen den Wert **In die Domäne eingebunden** auf.



## <a name="troubleshoot-your-implementation"></a>Problembehandlung bei der Implementierung

Wenn bei der Azure AD-Hybrideinbindung für in Domänen eingebundene Windows-Geräte Probleme auftreten, finden Sie weitere Informationen unter:

- [Problembehandlung für in Azure AD eingebundene aktuelle Windows-Hybridgeräte](troubleshoot-hybrid-join-windows-current.md)
- [Problembehandlung für in Azure AD eingebundene kompatible Windows-Hybridgeräte](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in die Geräteverwaltung in Azure Active Directory](overview.md)



<!--Image references-->
[1]: ./media/hybrid-azuread-join-manual-steps/12.png
