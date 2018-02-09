---
title: Migrieren von lokalen AD FS-Apps zu Azure | Microsoft-Dokumentation
description: Dieses Dokument soll Organisationen als Hilfe dienen und beschreiben, wie lokale Anwendungen zu Azure AD migriert werden. Der Schwerpunkt liegt hierbei auf SaaS-Verbundanwendungen.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/29/2018
ms.author: billmath
ms.openlocfilehash: ec0731534da2543d48bedc575bf882b790fa136b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="migrate-ad-fs-on-premises-apps-to-azure"></a>Migrieren von lokalen AD FS-Apps zu Azure 

In diesem Dokument wird für Organisationen beschrieben, wie lokale Anwendungen zu Azure AD migriert werden.  Der Schwerpunkt liegt hierbei auf SaaS-Verbundanwendungen.  Dieses Dokument enthält keine Schritt-für-Schritt-Anleitung.  Es enthält eine konzeptuelle Anleitung zur Durchführung der Migration, indem erläutert wird, wie lokale Konfigurationen in Azure AD übersetzt werden können. Es wird auch angegeben, was für die häufigsten Szenarien erforderlich ist.

## <a name="introduction"></a>Einführung

Wenn Sie über ein lokales Verzeichnis mit Benutzerkonten verfügen, ist die Wahrscheinlichkeit hoch, dass Sie mindestens eine oder zwei Apps nutzen.  Diese Apps sind so konfiguriert, dass Benutzer darauf zugreifen können, indem sie sich mit diesen Identitäten anmelden.

Wie die meisten anderen Organisationen auch, befinden Sie sich wahrscheinlich auf dem Weg zur Einführung von Cloudanwendungen und -identitäten.  Vielleicht funktionieren Office 365 und Azure AD Connect bei Ihnen schon.  Vielleicht haben Sie cloudbasierte SaaS-Anwendungen für einige wichtige Workloads eingerichtet, aber noch nicht für alle.  

Viele Organisationen nutzen SaaS- oder benutzerdefinierte Branchen-Apps (Line-of-Business, LoB) im direkten Verbund mit einem lokalen Anmeldedienst, z.B. Active Directory-Verbunddienste (AD FS), neben Office 365- und Azure AD-basierten Apps.  In diesem Migrationsleitfaden wird beschrieben, warum und wie lokale Anwendungen zu Azure AD migriert werden.

>[!NOTE]
>Der Leitfaden enthält ausführliche Informationen zur Konfiguration und Migration von SaaS-Apps sowie allgemeine Informationen zu benutzerdefinierten Branchen-Apps.  Eine ausführlichere Anleitung für benutzerdefinierte Branchen-Apps ist für die Zukunft geplant.

Abbildung 1: Apps mit direkter lokaler Verbindung ![Lokal](media/migrate-adfs-apps-to-azure/migrate1.png)

Abbildung 2: Apps im Verbund über Azure AD ![Azure](media/migrate-adfs-apps-to-azure/migrate2.png)

## <a name="why-migrate-apps-to-azure-ad"></a>Gründe für das Migrieren von Apps zu Azure AD

Für Organisationen, die bereits AD FS, Ping oder einen anderen lokalen Authentifizierungsanbieter verwenden, hat die Migration von Apps zu Azure AD die folgenden Vorteile:

**Höhere Zugriffssicherheit**
- Konfigurieren Sie präzise Zugriffssteuerungen pro Anwendung, einschließlich Multi-Factor Authentication (MFA), indem Sie den [bedingten Zugriff in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) verwenden.  Die Richtlinien können auf SaaS- und benutzerdefinierte Apps genauso angewendet werden, wie Sie dies ggf. bereits für Office 365 durchführen.
- Wenn Sie basierend auf Machine Learning und Heuristik zur Identifizierung von risikobehaftetem Datenverkehr Bedrohungen erkennen und die Anmeldung schützen möchten, können Sie die integrierten Azure AD-Funktionen mit [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) nutzen, die ständig weiterentwickelt werden.

**Azure AD B2B-Zusammenarbeit**
- Nachdem die Anmeldung an SaaS-Apps basierend auf Azure AD eingerichtet wurde, können Sie Partnern per [Azure AD B2B-Zusammenarbeit](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) Zugriff auf Cloudressourcen gewähren.

**Vereinfachte Verwaltungsoberfläche und zusätzliche Azure AD-Funktionen**
- Azure AD als Identitätsanbieter für SaaS-Apps unterstützt zusätzliche Funktionen, z.B. Tokensignaturzertifikate pro Anwendung, [konfigurierbare Zertifikatablaufdaten](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs) und die [automatisierte Bereitstellung](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) von Benutzerkonten (in wichtigen Katalog-Apps) basierend auf Azure AD-Identitäten.

**Bewahren der Vorteile eines lokalen Identitätsanbieters**
- Auch nachdem Sie in den Genuss der Azure AD-Vorteile gekommen sind, können Sie Ihre lokale Lösung für die Authentifizierung beibehalten, um lokale MFA-Lösungen, Protokollierung und Überwachung weiter zu nutzen. 

**Erleichterter Einstieg in die Aussonderung des lokalen Identitätsanbieters**
- Für Organisationen, die das Produkt für die lokale Authentifizierung aussondern möchten, ermöglicht die Migration von Apps zu Azure AD einen einfacheren Übergang, indem einige Arbeitsschritte abgegeben werden können. 

## <a name="mapping-types-of-apps-on-premises-to-types-of-apps-in-azure-ad"></a>Zuordnen von lokalen App-Typen zu Apps in Azure AD
Die meisten Apps können basierend auf dem verwendeten Anmeldetyp einigen bestimmten Kategorien zugeordnet werden.  Anhand dieser Kategorien wird bestimmt, wie die App in Azure AD dargestellt wird.

Kurz ausgedrückt: SAML 2.0-Anwendungen können entweder über den Azure AD-Anwendungskatalog oder als katalogexterne Anwendungen in Azure AD integriert werden.  Apps, für die OAuth 2.0 oder OpenID Connect verwendet wird, können auf ähnliche Weise als „App-Registrierungen“ in Azure AD integriert werden.  Im Folgenden erhalten Sie ausführlichere Informationen.

### <a name="federated-saas-apps-vs-custom-lob-apps"></a>Vergleich von SaaS-Verbund-Apps und benutzerdefinierten Branchen-Apps
Verbund-Apps sind Apps, die in die aufgeführten Kategorien fallen.

- SaaS-Apps 
    - Wenn sich Ihre Benutzer an SaaS-Apps anmelden, z.B. Salesforce, ServiceNow oder Workday, und Sie die Integration in einen lokalen Identitätsanbieter wie AD FS oder Ping durchführen, verwenden Sie die Verbundanmeldung für SaaS-Apps.
    - Im Allgemeinen nutzen Apps das SAML 2.0-Protokoll für die Verbundanmeldung.
    - Anwendungen, die in diese Kategorie fallen, können als Unternehmensanwendungen in Azure AD integriert werden – entweder über den Katalog oder als katalogexterne Anwendungen.
- Benutzerdefinierte Branchenanwendungen
    - Hiermit sind Nicht-SaaS-Apps gemeint, die intern von Ihrer Organisation entwickelt wurden oder als Standardpaketprodukt verfügbar sind, das in Ihrem Rechenzentrum installiert wird.  Hierin sind auch SharePoint-Apps und per Windows Identity Foundation (WIF) erstellte Apps enthalten.
    - Für Apps kann SAML 2.0, Webdiensteverbund, OAuth oder OpenID Connect für die Verbundanmeldung verwendet werden.
    - Benutzerdefinierte Apps, für die OAuth 2.0, OpenID Connect oder Webdiensteverbund verwendet wird, können in Azure AD als App-Registrierungen integriert werden. Benutzerdefinierte Apps, für die SAML 2.0 oder WS-Federation verwendet wird, können als katalogexterne Anwendungen in Unternehmensanwendungen integriert werden.

### <a name="non-federated-apps"></a>Nicht im Verbund angeordnete Apps
Außerdem können nicht im Verbund angeordnete Apps in Azure AD integriert werden, indem der Azure AD-Anwendungsproxy und die dazugehörigen Funktionen verwendet werden.  Weitere Informationen zu den Funktionen finden Sie unter diesen Links:
- Apps, die die Integrierte Windows-Authentifizierung (WIA) direkt für Active Directory verwenden
    - Diese Apps können per [Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) in Azure AD integriert werden.
- Apps, die über einen Agent in Ihren Anbieter für einmaliges Anmelden integriert werden und Header für die Autorisierung nutzen
    - Lokale Apps, die einen installierten Agent für die Anmeldung und die headerbasierte Autorisierung verwenden, können für die Azure AD-basierte Anmeldung konfiguriert werden, indem der Azure AD-Anwendungsproxy mit [Ping-Zugriff für Azure AD](https://blogs.technet.microsoft.com/enterprisemobility/2017/06/15/ping-access-for-azure-ad-is-now-generally-available-ga/) genutzt wird.

## <a name="translating-on-premises-federated-apps-to-azure-ad"></a>Übersetzen von lokalen Verbund-Apps in Azure AD 
Da sich die Funktionsweise von AD FS und Azure AD glücklicherweise ähnelt, gelten die Konzepte für das Konfigurieren der Vertrauensstellung, der Anmelde- und Abmelde-URLs und der Bezeichner in beiden Fällen.  Es gibt aber einige geringfügige Unterschiede, mit denen Sie vertraut sein sollten, wenn Sie die Umstellung durchführen.

In der Tabelle sind mehrere wichtige Ideen aufgeführt, die für AD FS, Azure AD und SaaS-Apps gelten und Ihnen als Unterstützung bei der Übersetzung dienen sollen. 

### <a name="representing-the-app-in-azure-ad-or-ad-fs"></a>Darstellen der App in Azure AD oder AD FS
Die Migration beginnt damit, wie die Anwendung lokal konfiguriert wird, und mit der Zuordnung der Konfiguration zu Azure AD.  Unten ist eine Zuordnung der Konfigurationselemente der vertrauenden AD FS-Seite zu den entsprechenden Elementen in Azure AD angegeben.  
- AD FS-Begriff: Vertrauende Seite bzw. Vertrauensstellung der vertrauenden Seite
- Azure AD-Begriff: Unternehmensanwendung oder App-Registrierung (je nach App-Typ)

|App-Konfigurationselement|Beschreibung|Innerhalb der AD FS-Konfiguration|Entsprechende Position in der Azure AD-Konfiguration|SAML-Tokenelement|
|-----|-----|-----|-----|-----|
|Anmelde-URL der App|URL der Anmeldeseite dieser Anwendung. Dies ist der Ort, an dem Benutzer für einen SP-initiierten SAML-Fluss eine Anmeldung an der App initiieren.|N/V|In Azure AD ist die Anmelde-URL im Azure-Portal in den SSO-Eigenschaften der Anwendung konfiguriert.</br></br>(Unter Umständen müssen Sie auf „Erweiterte URL-Einstellungen anzeigen“ klicken, um die Anmelde-URL anzuzeigen.)||
|Antwort-URL der App|Die URL der App aus IdP-Sicht.  An diesen Ort werden der Benutzer und das Token gesendet, nachdem sich der Benutzer am IdP angemeldet hat.</br></br>  Dies wird auch als Consumerendpunkt der SAML-Assertion bezeichnet.|Sie befindet sich in der Vertrauensstellung der vertrauenden Seite von AD FS für die App.  Klicken Sie mit der rechten Maustaste auf die vertrauende Seite, und wählen Sie „Eigenschaften“ > Registerkarte „Endpunkte“.|In Azure AD ist die Antwort-URL im Azure-Portal in den SSO-Eigenschaften der Anwendung konfiguriert.</br></br>(Unter Umständen müssen Sie auf „Erweiterte URL-Einstellungen anzeigen“ klicken, um die Antwort-URL anzuzeigen.)|Ist im SAML-Token dem Destination-Element zugeordnet.</br></br>  Beispielwert: https://contoso.my.salesforce.com|
|Abmelde-URL der App|URL, an die Anforderungen der Art „Abmeldebereinigung“ gesendet werden, wenn sich ein Benutzer von einer App abmeldet, um die Abmeldung von allen anderen Apps durchzuführen, an denen der Benutzer vom IdP angemeldet wurde.|Befindet sich in der AD FS-Verwaltung unter „Vertrauensstellungen der vertrauenden Seite“.  Klicken Sie mit der rechten Maustaste auf die vertrauende Seite, und wählen Sie „Eigenschaften“ > Registerkarte „Endpunkte“.|N/V – Für Azure AD wird das einmalige Abmelden (also das Abmelden von allen Apps) nicht unterstützt.  Der Benutzer wird einfach nur von Azure AD abgemeldet.|N/V|
|App-ID|Bezeichner der App aus IdP-Sicht: Der Wert der Abmelde-URL wird häufig für den Bezeichner verwendet (aber nicht immer).</br></br>  Wird in der App in einigen Fällen auch als „Entitäts-ID“ bezeichnet.|In AD FS ist dies die ID der vertrauenden Seite: Klicken Sie mit der rechten Maustaste auf die Vertrauensstellung der vertrauenden Seite, und wählen Sie „Eigenschaften“ > Registerkarte „Bezeichner“.|In Azure AD ist der Bezeichner im Azure-Portal in den SSO-Eigenschaften der Anwendung als Bezeichner unter „Domäne und URLs“ konfiguriert (ggf. müssen Sie das Kontrollkästchen „Erweiterte URL-Einstellungen anzeigen“ aktivieren).|Entspricht dem Audience-Element im SAML-Token.|
|Verbundmetadaten der App|Speicherort der Verbundmetadaten einer App.  Wird vom IdP zum automatischen Aktualisieren von bestimmten Konfigurationseinstellungen, z.B. Endpunkten oder Verschlüsselungszertifikaten, verwendet.|Die Verbundmetadaten-URL der App befindet sich in der AD FS-Vertrauensstellung der vertrauenden Seite einer App.  Klicken Sie mit der rechten Maustaste auf die Vertrauensstellung, wählen Sie „Eigenschaften“, und klicken Sie anschließend auf die Registerkarte „Überwachung“.|N/V – Für Azure AD wird die direkte Nutzung von Anwendungsverbundmetadaten nicht unterstützt.|N/V|
|Benutzer-ID/NameID|Attribut zum eindeutigen Angeben der Benutzeridentität von Azure AD oder AD FS für Ihre App.</br></br>  Dies ist normalerweise entweder der UPN oder die E-Mail-Adresse des Benutzers.|In AD FS kommt dies als Anspruchsregel der vertrauenden Seite vor.  In den meisten Fällen ist dies die Anspruchsregel, die einen Anspruch mit einem Typ ausstellt, der auf „nameidentifier“ endet.|In Azure AD finden Sie die Benutzer-ID im Azure-Portal in den SSO-Eigenschaften der Anwendung unter der Überschrift „Benutzerattribute“.</br></br>Standardmäßig wird der UPN verwendet.|Wird vom IdP im SAML-Token als „NameID“ an die App kommuniziert.|
|Andere Ansprüche, die an die App gesendet werden|Zusätzlich zu Benutzer-ID und NameID werden häufig auch andere Anspruchsinformationen vom IdP an die App gesendet, z.B. Vorname, Nachname, E-Mail-Adresse und Gruppen, in denen der Benutzer Mitglied ist.|In AD FS kommt dies unter den weiteren Anspruchsregeln der vertrauenden Seite vor.|Klicken Sie für Azure AD im Azure-Portal in den SSO-Eigenschaften der Anwendung unter der Überschrift „Benutzerattribute“ auf „Ansicht“, und bearbeiten Sie alle anderen Benutzerattribute.|| 

### <a name="representing-azure-ad-as-an-identity-provider-idp-in-a-saas-app"></a>Darstellen von Azure AD als Identitätsanbieter (IdP) in einer SaaS-App
Im Rahmen der Migration muss die App so konfiguriert werden, dass sie auf Azure AD verweist (über den lokalen Identitätsanbieter).  In diesem Abschnitt geht es hauptsächlich um SaaS-Apps, für die das SAML-Protokoll verwendet wird, und nicht um benutzerdefinierte Apps oder Branchen-Apps. Die beschriebenen Konzepte gelten aber auch für benutzerdefinierte Apps und Branchen-Apps. 

Es gibt einige allgemeine wichtige Dinge, die für das Verweisen für eine SaaS-App auf Azure AD bekannt sein sollten.
- Aussteller des Identitätsanbieters: https&#58;//sts.windows.net/{Mandanten-ID}/
- Anmelde-URL des Identitätsanbieters: https&#58;//login.microsoftonline.com/{Mandanten-ID}/saml2
- Abmelde-URL des Identitätsanbieters: https&#58;//login.microsoftonline.com/{Mandanten-ID}/saml2 
- Speicherort der Verbundmetadaten: https&#58;//login.windows.net/{Mandanten-ID} <Mandanten-ID>/federationmetadata/2007-06/federationmetadata.xml?appid={<Anwendungs-ID} 

Hierbei wird {Mandanten-ID} durch Ihre Mandanten-ID ersetzt, die im Azure-Portal unter „Azure Active Directory“ > „Eigenschaften“ als „Verzeichnis-ID“ angegeben ist, und {Anwendungs-ID} wird durch Ihre Anwendungs-ID ersetzt, die in den Eigenschaften der Anwendung als „Anwendungs-ID“ angegeben ist.

In der Tabelle werden die wichtigen IdP-Konfigurationselemente zum Konfigurieren der SSO-Einstellungen in der App und die dazugehörigen Werte und Speicherorte in AD FS und Azure AD ausführlicher beschrieben.  Als Referenz für die Tabelle dient die SaaS-App, die über die Informationen verfügen muss, wohin die Authentifizierungsanforderungen gesendet und wie die empfangenen Token überprüft werden sollen.

|Konfigurationselement|Beschreibung|AD FS|Azure AD|
|---|---|---|---|
|IdP- </br>Anmelde- </br>URL|Anmelde-URL des IdP aus App-Sicht (an die der Benutzer zur Anmeldung umgeleitet wird).|Die AD FS-Anmelde-URL ist der AD FS-Verbunddienstname gefolgt von „/adfs/ls/“. Beispiel: https&#58;//fs.contoso.com/adfs/ls/|Der entsprechende Wert für Azure AD basiert auf einem Muster, bei dem {Mandanten-ID} durch Ihre Mandanten-ID ersetzt wird. Sie ist im Azure-Portal unter „Azure Active Directory“ > „Eigenschaften“ als „Verzeichnis-ID“ angegeben.</br></br>Für Apps, die das SAML-P-Protokoll nutzen: https&#58;//login.microsoftonline.com</br>/{Mandanten-ID}/saml2 </br></br>Für Apps, die das Webdiensteverbund-Protokoll nutzen: https&#58;//login.microsoftonline.com</br>/{Mandanten-ID}/wsfed|
|IdP- </br>Abmelde- </br>URL|Abmelde-URL des IdP aus App-Sicht (an die der Benutzer umgeleitet wird, wenn er sich von der App abmelden möchte).|Für AD FS ist die Abmelde-URL entweder mit der Anmelde-URL identisch, oder die URL verfügt über den Zusatz „wa=wsignout1.0“, z.B. „https&#58;//fs.contoso.com/adfs/ls /?wa=wsignout1.0“.|Der entsprechende Wert für Azure AD hängt davon ab, ob die App die SAML 2.0-Abmeldung unterstützt.</br></br>Wenn die SAML-Abmeldung von der App unterstützt wird, folgt der Wert einem Muster, bei dem der Wert für {Mandanten-ID} durch die Mandanten-ID ersetzt wird, die im Azure-Portal unter „Azure Active Directory“ > „Eigenschaften“ als „Verzeichnis-ID“ angegeben ist. https&#58;//login.microsoftonline.com</br>/{Mandanten-ID}/saml2</br></br>Falls die App die SAML-Abmeldung nicht unterstützt: https&#58;//login.microsoftonline.com</br>/common /wsfederation?wa=wsignout1.0|
|Token- </br>signatur- </br>zertifikat|Zertifikat, dessen privater Schlüssel vom IdP zum Signieren von ausgestellten Token verwendet wird.  Überprüft, ob das Token von demselben IdP stammt, für den für die App die Vertrauensstellung konfiguriert wurde.|Das AD FS-Tokensignaturzertifikat befindet sich in der AD FS-Verwaltung unter „Zertifikate“.|In Azure AD befindet sich das Tokensignaturzertifikat im Azure-Portal in den SSO-Eigenschaften der Anwendung unter dem Header „SAML-Signaturzertifikat“. Sie können das Zertifikat dort für den Upload in die App herunterladen.</br></br>  Falls die Anwendung über mehr als ein Zertifikat verfügt, sind alle Zertifikate in der XML-Datei mit den Verbundmetadaten enthalten.|
|Bezeichner/ </br>„Aussteller“|Bezeichner des IdP aus App-Sicht (auch als „Aussteller“ oder „Aussteller-ID“ bezeichnet).</br></br>Im SAML-Token wird der Wert als „Issuer“-Element angezeigt.|Der Bezeichner für AD FS ist normalerweise der Verbunddienstbezeichner in der AD FS-Verwaltung unter „Dienst“ > „Verbunddiensteigenschaften bearbeiten“.  Beispiel: http&#58;//fs.contoso.com/adfs/services/trust|Der entsprechende Wert für Azure AD basiert auf einem Muster, bei dem der Wert für {Mandanten-ID} durch Ihre Mandanten-ID ersetzt wird. Sie ist im Azure-Portal unter „Azure Active Directory“ > „Eigenschaften“ als „Verzeichnis-ID“ angegeben.  https&#58;//sts.windows.net/{Mandanten-ID}/|
|IdP- </br>Verbund- </br>metadaten|Speicherort der öffentlich verfügbaren Verbundmetadaten des IdP.  (Verbundmetadaten werden von einigen Apps als Alternative zur individuellen Verwaltung der Konfiguration der URLs, des Bezeichners und des Tokensignaturzertifikats verwendet.)|Die URL für die AD FS-Verbundmetadaten befindet sich in der AD FS-Verwaltung unter „Dienst“ > „Endpunkte“ > „Metadaten“ > „Typ: Verbundmetadaten“, z.B. „https&#58;//fs.contoso.com/ FederationMetadata/2007-06/“.</br>FederationMetadata.xml|Der entsprechende Wert für Azure AD basiert auf dem Muster „https&#58;//login.microsoftonline.com“.</br>/{TenantDomainName}/FederationMetadata/2007-06/</br>FederationMetadata.xml. Hierbei wird der Wert für {TenantDomainName} durch den Namen Ihres Mandanten im Format „contoso.onmicrosoft.com“ ersetzt. </br></br>[Erfahren Sie mehr](https://docs.microsoft.com/azure/active-directory/develop/active-directory-federation-metadata) zu Verbundmetadaten in Azure AD.

## <a name="migrating-saas-apps"></a>Migrieren von SaaS-Apps
Das Migrieren von SaaS-Apps aus AD FS oder einem anderen Identitätsanbieter zu Azure AD ist derzeit ein manueller Prozess. App-spezifische Anleitungen finden Sie in der [Liste mit den Tutorials zur Integration von im Katalog enthaltenen SaaS-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

In den Tutorials zur Integration wird vorausgesetzt, dass Sie eine Greenfield-Integration durchführen.  Es gibt einige wichtige spezifische Konzepte für die Migration, die Sie kennen sollten, wenn Sie für Ihre Apps die Planung, Bewertung, Konfiguration und Umstellung durchführen.  
- Einige Apps können zwar leicht migriert werden, aber für Apps mit komplexeren Anforderungen, z.B. benutzerdefinierten Ansprüchen, ist unter Umständen zusätzlicher Konfigurationsaufwand in Azure AD bzw. Azure AD Connect erforderlich.
- In den meisten häufigen Szenarien werden nur der NameId-Anspruch und andere gängige Benutzer-ID-Ansprüche für eine App benötigt. Um zu ermitteln, ob zusätzliche Ansprüche erforderlich sind, können Sie untersuchen, welche Ansprüche Sie über AD FS oder Ihren Identitätsdrittanbieter ausstellen.
- Nachdem Sie ermittelt haben, dass zusätzliche Ansprüche benötigt werden, müssen Sie sicherstellen, dass sie in Azure AD verfügbar sind.  Überprüfen Sie die Konfiguration der Azure AD Connect-Synchronisierung, um sich zu vergewissern, dass ein erforderliches Attribut, z.B. samAccountName, mit Azure AD synchronisiert wird.
- Nachdem diese Attribute in Azure AD verfügbar sind, fügen Sie Regeln für die Anspruchsausstellung in Azure AD hinzu, um diese Attribute als Ansprüche in ausgestellten Token einzubinden.  Dies erfolgt in den SSO-Eigenschaften der App in Azure AD.

### <a name="assessing-what-can-be-migrated"></a>Bewerten der migrierbaren Elemente
SAML 2.0-Anwendungen können entweder über den Azure AD-Anwendungskatalog oder als katalogexterne Anwendungen in Azure AD integriert werden.  

Bei einigen Konfigurationen sind zusätzliche Schritte für die Konfiguration in Azure AD erforderlich, und andere werden derzeit nicht unterstützt.  Sehen Sie sich die aktuelle Konfiguration der einzelnen Apps an, um zu ermitteln, was verschoben werden kann. Achten Sie besonders auf Folgendes:
- Konfigurierte Anspruchsregeln (Ausstellungstransformationsregeln)
- SAML-NameID – Format und Attribut
- Ausgestellte SAML-Tokenversionen
- Andere Konfigurationselemente, z.B. Ausstellungsautorisierungsregeln oder Zugriffssteuerungsrichtlinien und Regeln für die mehrstufige Authentifizierung (zusätzliche Authentifizierung)

#### <a name="what-can-be-migrated-today"></a>Elemente, die derzeit migriert werden können
Zu den derzeit leicht zu migrierenden Apps gehören SAML 2.0-Apps, für die der Standardsatz von Konfigurationselementen und Ansprüchen verwendet wird.  Diese Apps können aus folgenden Elementen bestehen:
- Benutzerprinzipalname
- E-Mail-Adresse
- Vorname
- Nachname
- Alternatives Attribut als SAML-NameID, z.B. Azure AD-E-Mail-Attribut, E-Mail-Präfix, employeeid, Erweiterungsattribute 1 bis 15 oder lokaler SamAccountName (siehe [Bearbeiten des NameIdentifier-Anspruchs)](./develop/active-directory-saml-claims-customization.md)
- Benutzerdefinierte Ansprüche ([hier](active-directory-claims-mapping.md) und [hier](./develop/active-directory-saml-claims-customization.md) finden Sie Informationen zu unterstützten Anspruchszuordnungen)

Zusätzlich zu benutzerdefinierten Ansprüchen und nameID-Elementen gibt es die folgenden Konfigurationen, für die in Azure AD im Rahmen der Migration zusätzliche Konfigurationsschritte erforderlich sind:
- Benutzerdefinierte Autorisierung oder MFA-Regeln in AD FS (Konfiguration mit dem Feature [Bedingter Zugriff auf Azure AD](active-directory-conditional-access-azure-portal.md))
- Apps mit mehreren SAML-Endpunkten können in Azure AD mit PowerShell konfiguriert werden (Funktion nicht im Portal verfügbar)
- Webdiensteverbund-Apps, z.B. SharePoint-Apps, für die Token der Version SAML 1.1 benötigt werden, müssen manuell mit PowerShell konfiguriert werden

#### <a name="appsconfigurations-not-supported-in-azure-ad-today"></a>Apps/Konfigurationen, die in Azure AD derzeit nicht unterstützt werden
Apps, für die die folgenden Funktionen erforderlich sind, können derzeit nicht migriert werden.  Geben Sie im Kommentarbereich Feedback an, falls Sie über Apps mit diesen Featureanforderungen verfügen.
- Protokollfunktionen
    - Unterstützung für das einmalige SAML-Abmelden (SLO) von allen angemeldeten Apps
    - Unterstützung für WS-Trust ActAs-Muster
    - SAML-Artefaktauflösung 
    - Signaturüberprüfung der signierten SAML-Anforderungen (Beachten Sie Folgendes: Signierte Anforderungen werden akzeptiert, aber die Signatur ist nicht verifiziert.)
 - Tokenfunktionen 
     - SAML-Tokenverschlüsselung 
     - Token der Version SAML 1.1 in SAML-Protokollantworten 
- Ansprüche in Tokenfunktionen
    - Ausstellen von lokalen Gruppennamen als Ansprüche
    - Ansprüche aus anderen Speichern als Azure AD
    - Komplexe Transformationsregeln für die Anspruchsausstellung (Informationen zur Zuordnung von unterstützten Ansprüchen finden Sie in [diesem](active-directory-claims-mapping.md) und [diesem](./develop/active-directory-saml-claims-customization.md) Dokument)
    - Ausstellen von Verzeichniserweiterungen als Ansprüche
    - Benutzerdefinierte Angabe des NameID-Formats
    - Ausstellung von mehrwertigen Attributen

>[!NOTE]
>Azure AD wird ständig weiterentwickelt, um in diesem Bereich zusätzliche Funktionen hinzuzufügen. Dieses Dokument wird in regelmäßigen Abständen aktualisiert. 

### <a name="configuring-azure-ad"></a>Konfigurieren von Azure AD    
#### <a name="configure-single-sign-on-sso-settings-for-the-saas-app"></a>Konfigurieren der Einstellungen für das einmalige Anmelden (SSO) für die SaaS-App

In Azure AD wird die für Ihre App erforderliche SAML-Anmeldung in den SSO-Eigenschaften der App unter „Benutzerattribute“ konfiguriert. Dies ist hier dargestellt:

![](media/migrate-adfs-apps-to-azure/migrate3.png)
- Klicken Sie auf „Alle weiteren Benutzerattribute anzeigen und bearbeiten“, um die Attribute anzuzeigen, die als Ansprüche im Sicherheitstoken gesendet werden sollen.

![](media/migrate-adfs-apps-to-azure/migrate4.png)
- Klicken Sie auf eine bestimmte Attributzeile, die Sie bearbeiten möchten, oder klicken Sie auf „Attribut hinzufügen“, um ein neues Attribut hinzuzufügen. 
![](media/migrate-adfs-apps-to-azure/migrate5.png)

#### <a name="assign-users-to-the-app"></a>Zuweisen von Benutzern zur App
Damit sich Benutzer in Azure AD an einer bestimmten SaaS-App anmelden können, muss ihnen in Azure AD Zugriff gewährt werden.

Navigieren Sie zum Zuweisen von Benutzern im Azure AD-Portal zum Bildschirm der SaaS-App im Portal, und klicken Sie in der Randleiste dann auf „Benutzer und Gruppen“. Klicken Sie oben auf dem Bildschirm auf „Benutzer hinzufügen“, um einen Benutzer oder eine Gruppe hinzuzufügen. 

![](media/migrate-adfs-apps-to-azure/migrate6.png) 

![](media/migrate-adfs-apps-to-azure/migrate7.png) Zum Überprüfen des Zugriffs sollte dem Benutzer im [Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) beim Anmelden die entsprechende SaaS-App angezeigt werden, die sich unter „http://myapps.microsoft.com“ befindet. Beispielsweise kann dem Benutzer erfolgreich sowohl Zugriff auf Salesforce als auch auf ServiceNow zugewiesen werden.

![](media/migrate-adfs-apps-to-azure/migrate8.png)

### <a name="configuring-the-saas-app"></a>Konfigurieren der SaaS-App
Der Umstellungsprozess vom lokalen Verbund auf Azure AD hängt davon ab, ob die von Ihnen verwendete SaaS-App mehrere Identitätsanbieter (IdPs) unterstützt.  Hier sind einige häufige Fragen zur Unterstützung mehrerer Identitätsanbieter aufgeführt:

   **F: Was bedeutet für eine App die Unterstützung von mehreren IdPs?**
    
   A: Mit SaaS-Apps, die mehrere IdPs unterstützen, können Sie alle Informationen zum neuen IdP (hier Azure AD) eingeben, bevor Sie den Commit für die Änderung der Anmeldeoberfläche durchführen.  Nachdem die Konfiguration abgeschlossen ist, können Sie für die Authentifizierungskonfiguration der App die Umstellung auf Azure AD vornehmen.

   F: Warum ist es wichtig, ob die SaaS-App mehrere IdPs unterstützt?

   A: Falls die Unterstützung für mehrere IdPs fehlt, muss der Administrator ein kurzes Zeitfenster für einen Dienst- bzw. Wartungsausfall bereitstellen, in dem Azure AD als neuer IdP einer App konfiguriert werden kann. Während dieses Ausfalls sollten Benutzer darüber informiert werden, dass sie sich nicht an ihren Konten anmelden können.

   Wenn eine App mehrere IdPs unterstützt, kann die Konfiguration des zusätzlichen IdP vorher durchgeführt werden, sodass der Administrator den IdP während der Azure-Umstellung einfach wechseln kann.

   Außerdem können Benutzer bei der Authentifizierung auf ihrer Anmeldeseite eine IdP-Auswahl treffen, wenn die App mehrere IdPs unterstützt und Sie mehrere IdPs für die gleichzeitige Verarbeitung der Authentifizierung während der Anmeldung auswählen.

#### <a name="example-multiple-idp-support"></a>Beispiel: Unterstützung mehrerer IdPs
In Salesforce finden Sie die IdP-Konfiguration beispielsweise unter „Settings“ > „Company Settings“ > „My Domain“ > „Authentication Configuration“ („Einstellungen“ > „Unternehmenseinstellungen“ > „Meine Domäne“ > „Authentifizierungskonfiguration“).

![](media/migrate-adfs-apps-to-azure/migrate9.png)

Da Sie zuvor unter „Identität“ > „Einzelanmeldungseinstellungen“ die entsprechende Konfiguration erstellt haben, sollten Sie Ihren IdP für die Authentifizierungskonfiguration beispielsweise von AD FS in Azure AD ändern können. 

![](media/migrate-adfs-apps-to-azure/migrate10.png)

### <a name="optional-configure-user-provisioning-in-azure-ad"></a>Optional: Konfigurieren der Benutzerbereitstellung in Azure AD
Optional: Falls Sie möchten, dass Azure AD die Benutzerbereitstellung für eine bestimmte SaaS-App direkt durchführt, hilft Ihnen das Dokument zur Verwaltung der Benutzerkontobereitstellung für Unternehmens-Apps in Azure AD weiter.

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Anwendungen mit Azure Active Directory](active-directory-enable-sso-scenario.md)
- [Verwalten des Zugriffs auf Apps](active-directory-managing-access-to-apps.md)
- [Azure AD Connect und Verbund](active-directory-aadconnectfed-whatis.md)