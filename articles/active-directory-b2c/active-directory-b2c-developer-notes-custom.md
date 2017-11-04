---
title: 'Azure Active Directory B2C: Entwicklerhinweise zur Verwendung von benutzerdefinierten Richtlinien | Microsoft-Dokumentation'
description: "Enthält Hinweise für Entwickler, die das Konfigurieren und Verwalten von Azure AD B2C mit benutzerdefinierten Richtlinien betreffen."
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 10/13/2017
ms.author: joroja
ms.openlocfilehash: 4fa4665115e0682df7c3fe3d8e2664a0f7a77a07
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Versionshinweise für benutzerdefinierte Azure Active Directory B2C-Richtlinien – Öffentliche Vorschauversion
Der Featuresatz für benutzerdefinierte Richtlinien ist jetzt als öffentliche Vorschauversion für alle Azure AD B2C-Kunden (Azure Active Directory B2C) zur Evaluierung verfügbar. Dieser Featuresatz ist für fortgeschrittene Identitätsentwickler bestimmt, die sehr komplexe Identitätslösungen erstellen.  

Bei diesem Featuresatz müssen Entwickler das Identity Experience Framework direkt per Bearbeitung der XML-Datei konfigurieren. Dieses Konfigurationsverfahren ist leistungsstark und komplex. Fortgeschrittene Identitätsentwickler, die das Identity Experience Framework verwenden, sollten Zeit einplanen, z.B. für das Durcharbeiten von exemplarischen Vorgehensweisen und Lesen von Referenzdokumenten. 

## <a name="features-included-in-this-public-preview"></a>In der öffentlichen Vorschauversion enthaltene Features
Mit den neuen Features, die mit der öffentlichen Vorschauversion eingeführt werden, können Entwickler die folgenden Aufgaben durchführen:<br>

* Erstellen und Hochladen von benutzerdefinierten User Journeys für die Authentifizierung mit benutzerdefinierten Richtlinien 
   * Beschreiben von User Journeys als Austauschvorgänge zwischen Anspruchsanbietern (Schritt für Schritt) 
   * Definieren der bedingten Verzweigung in User Journeys 
* Integrieren von REST-API-fähigen Diensten in User Journeys zur benutzerdefinierten Authentifizierung  
* Hinzufügen von Verbünden mit Identitätsanbietern, die mit dem OpenID Connect-Standard konform sind <br>
* Hinzufügen von Verbünden mit Identitätsanbietern, die auf dem SAML-2.0-Protokoll basieren 

## <a name="terms-of-the-public-preview"></a>Bedingungen der öffentlichen Vorschauversion

* Die Nutzung der neuen Features sollte nur zu Evaluierungszwecken erfolgen.<br>
* Die neuen Features sind nicht für die Verwendung in einer Produktionsumgebung vorgesehen.<br>
* Vereinbarungen zum Servicelevel (SLAs) gelten für die neuen Features nicht. <br>
* Supportanfragen können über die üblichen Supportkanäle gesendet werden. <br>
* Es liegt kein festes Datum für die allgemeine Verfügbarkeit vor.<br>
* Microsoft behält sich vor, Szenarien und User Journeys, die den vorgesehenen Anwendungsumfang des Azure AD B2C-Produkts als CIAM-Plattform (Customer Identity and Access Management) überschreiten, zu kennzeichnen und aus beliebigen Gründen abzulehnen oder einzuschränken.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Zuständigkeiten für Entwickler von Featuregruppen für benutzerdefinierte Richtlinien
Die manuelle Richtlinienkonfiguration gewährt eingehenderen Zugriff auf die zugrunde liegende Plattform von Azure AD B2C und führt zur Erstellung eines eindeutigen und vollständig anpassbaren Vertrauensframeworks. Die möglichen Permutationen von benutzerdefinierten Identitätsanbietern, Vertrauensstellungen, Integrationen in externe Dienste und ausführlichen Workflows stellen für fortgeschrittene Entwickler, die sie nutzen, eine höhere Anforderung dar.

Um von der öffentlichen Vorschauversion in vollem Umfang zu profitieren, ist es ratsam, dass sich Entwickler, die die Featuregruppe für benutzerdefinierte Richtlinien verwenden, an folgende Vorgaben halten:
* Machen Sie sich mit der Konfigurationssprache des Identity Experience Framework und der Verwaltung von Schlüsseln/Geheimnissen vertraut.
* Übernehmen Sie den Besitz für Szenarien und benutzerdefinierte Integrationen.
* Führen Sie methodische Szenariotests durch.
* Nutzen Sie die bewährten Methoden für die Softwareentwicklung und das Staging, und verwenden Sie mindestens eine Entwicklungs-/Testumgebung und eine Produktionsumgebung.
* Informieren Sie sich regelmäßig über neue Entwicklungen bei den von Ihnen integrierten Identitätsanbietern und -diensten. Achten Sie beispielsweise auf Änderungen an Geheimnissen sowie auf geplante und ungeplante Änderungen am Dienst.
* Richten Sie eine aktive Überwachung ein, und überwachen Sie die Reaktionsfähigkeit von Produktionsumgebungen.
* Halten Sie die E-Mail-Adressen der Kontaktpersonen im Azure-Abonnement aktuell, und sorgen Sie dafür, dass auf die E-Mails des Microsoft-Livewebsite-Teams schnell reagiert wird.
* Ergreifen Sie ohne Verzögerung Maßnahmen, wenn Sie vom Microsoft-Livewebsite-Team dazu aufgefordert werden. 

## <a name="features-by-stage-and-known-issues"></a>Features nach Phase und bekannte Probleme
Funktionen für benutzerdefinierte Richtlinien und Identity Experience Framework werden laufend und schnell weiterentwickelt.  Die folgende Tabelle dient als Index für die Verfügbarkeit von Features und Komponenten.

Fragen können Sie in Stack Overflow unter [aka.ms/aadb2cso](http://aka.ms/aadb2cso) stellen.


### <a name="identity-providers-tokens-protocols"></a>Identitätsanbieter, Token, Protokolle
Schnittstellen mit externen Komponenten und Anwendungen

| Feature | Entwicklung | Vorschau | Allgemein verfügbar | Hinweise |
|---------------------------------------------|-------------|---------|----|-------|
| IDP-OpenIDConnect |  | x |  | z.B. Google+ |
| IDP-OAUTH2 |  | x |  | z.B. Facebook  |
| IDP-OAUTH1 |  | x |  | z.B. Twitter |
| IDP-SAML |  | x |  | z.B. Salesforce, ADFS |
| IDP-WSFED | x |  |  |  |
| OAUTH der vertrauenden Seite |  | x |  |  |
| OIDC der vertrauenden Seite |  | x |  |  |
| SAML der vertrauenden Seite | x |  |  |  |
| WSFED der vertrauenden Seite | x |  |  |  |
| REST-API mit Standard- und Zertifikatauthentifizierung |  | x |  | z.B. Azure Functions |


### <a name="component-support"></a>Unterstützung für Komponenten


| Feature | Entwicklung | Vorschau | Allgemein verfügbar | Hinweise |
|-------------------------------------------|-------------|---------|----|-------|
| Azure Multi-Factor Authentication |  | x |  |  |
| Azure Active Directory als lokales Verzeichnis |  | x |  |  |
| Azure-E-Mail-Subsystem für 2FA |  | x |  |  |
| Unterstützung für mehrere Sprachen|  | x |  |  |
| Kennwortkomplexität | x |  |  |  |


### <a name="content-definition"></a>Inhaltsdefinition

| Feature | Entwicklung | Vorschau | Allgemein verfügbar | Hinweise |
|-----------------------------------------------------------------------------|-------------|---------|----|-------|
|   Fehlerseite, api.error |  | x |  |  |
|   IDP-Auswahlseite, api.idpselections |  | x |  |  |
|   IDP-Auswahl zur Registrierung, api.idpselections.signup |  | x |  |  |
|   Kennwort vergessen, api.localaccountpasswordreset |  | x |  |  |
|   Anmeldung mit lokalem Konto, api.localaccountsignin |  | x |  |  |
|   Registrierung mit lokalem Konto, api.localaccountsignup |  | x |  |  |
|   MFA-Seite, api.phonefactor |  | x |  |  |
|   Selbstbestätigt – z.B. Registrierung über Konto für soziales Netzwerk, api.selfasserted |  | x |  |  |
|   Selbstbestätigte Profilaktualisierung, api.selfasserted.profileupdate |  | x |  |  |
|   Einheitliche Seite für Registrierung oder Anmeldung, api.signuporsignin |  | x |  |  |


### <a name="app-ief-integration"></a>App-IEF-Integration
| Feature | Entwicklung | Vorschau | Allgemein verfügbar | Hinweise |
|--------------------------------------------------|-------------|---------|----|-------------------------------------------------|
| Abfragezeichenfolgen-Parameter „id_token_hint“ | x |  |  |  |
| Abfragezeichenfolgen-Parameter „domain_hint“ |  | x |  | verfügbar als Anspruch, kann an IDP übergeben werden |
| Abfragezeichenfolgen-Parameter „login_hint“ |  | x |  | verfügbar als Anspruch, kann an IDP übergeben werden |
| Einfügen von JSON in UserJourney über „client_assertion“ | x |  |  | wird eingestellt |
| Einfügen von JSON in UserJourney als „id_token_hint“ | x |  |  | Go-Forward-Ansatz zum Übergeben von JSON |


### <a name="session-management"></a>Sitzungsverwaltung

| Feature | Entwicklung | Vorschau | Allgemein verfügbar | Hinweise |
|---------------------------------|-------------|---------|----|-------|
| SSO-Sitzungsanbieter |  | x |  |  |
| Sitzungsanbieter mit externer Anmeldung |  | x |  |  |
| SAML SSO-Sitzungsanbieter |  | x |  |  |


### <a name="security"></a>Sicherheit
| Feature | Entwicklung | Vorschau | Allgemein verfügbar | Hinweise |
|---------------------------------------------|-------------|---------|----|-------|
| Richtlinienschlüssel – Generieren, Manuell, Hochladen |  | x |  |  |
| Richtlinienschlüssel – RSA/Zertifikate, Geheimnisse |  | x |  |  |


### <a name="developer-interface"></a>Entwicklerschnittstelle
| Feature | Entwicklung | Vorschau | Allgemein verfügbar | Hinweise |
|---------------------------------------------|-------------|---------|----|-------|
| Azure-Portal – IEF UX |  | x |  |  |
| Application Insights, UserJourney-Protokolle  |  | x |  |  |
| Application Insights, Ereignisprotokolle |x|  |  |  |



## <a name="next-steps"></a>Nächste Schritte
[Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md)
