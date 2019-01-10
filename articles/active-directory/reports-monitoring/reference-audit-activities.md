---
title: Referenz zu Überwachungsaktivitäten von Azure Active Directory (Azure AD) | Microsoft-Dokumentation
description: Hier finden Sie eine Übersicht über die Überwachungsaktivitäten, die in Ihren Überwachungsprotokollen in Azure Active Directory (Azure AD) protokolliert werden können.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: db801acbed649c4baa039858ce1d097237c8e227
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53971374"
---
# <a name="azure-ad-audit-activity-reference"></a>Referenz zu Überwachungsaktivitäten von Azure AD

Mit Azure AD-Berichten (Azure Active Directory) können Sie alle Informationen abrufen, die Sie zum Ermitteln des Zustands Ihrer Umgebung benötigen.

Die Architektur für die Berichterstellung in Azure AD umfasst die folgenden Komponenten:

- **Aktivitätsberichte** 
    - [Anmeldungen](concept-sign-ins.md): Bietet Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung.
    - [Überwachungsprotokolle:](concept-audit-logs.md) Ermöglichen die Nachverfolgung sämtlicher Änderungen, die von verschiedenen Features in Azure AD vorgenommen wurden. 
    
- **Sicherheitsberichte** 
    - [Riskante Anmeldungen:](concept-risky-sign-ins.md) Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. 
    - [Benutzer mit Risikomarkierung:](concept-user-at-risk.md) Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. 

Dieser Artikel enthält die Überwachungsaktivitäten, die in Ihren Überwachungsprotokollen protokolliert werden können:

## <a name="access-reviews"></a>Zugriffsüberprüfungen

|Überwachungskategorie|Aktivität|
|---|---|
|Verzeichnisverwaltung|Verwaltungseinheit hinzufügen|
|Verzeichnisverwaltung|Mitglied der Verwaltungseinheit hinzufügen|
|Verzeichnisverwaltung|Verwaltungseinheit löschen|
|Verzeichnisverwaltung|Mitglied aus Verwaltungseinheit entfernen|
|Verzeichnisverwaltung|Verwaltungseinheit aktualisieren|
|Verzeichnisverwaltung|Verwaltung|
|Benutzerverwaltung|Directory Operation (Verzeichnisvorgang)|
|Benutzerverwaltung|Export|
|Benutzerverwaltung|Importieren|
|Benutzerverwaltung|Andere|
|Benutzerverwaltung|Process Escrow (Hinterlegung verarbeiten)|
|Benutzerverwaltung|Synchronization Rule Action (Synchronisierungsregelaktion)|
|Benutzerverwaltung|Anwendung hinzufügen|
|Benutzerverwaltung|Löschen der Anwendung|
|Benutzerverwaltung|Aktualisieren der Anwendung|
|Benutzerverwaltung|Update application Single Sign-On Mode (SSO-Modus der Anwendung aktualisieren)|
|Benutzerverwaltung|Automatisiertes Kennwortrollover|
|Benutzerverwaltung|Add V2 application permissions (V2-Anwendungsberechtigungen hinzufügen)|
|Benutzerverwaltung|Create V1 application (V1-Anwendung erstellen)|
|Benutzerverwaltung|Create V2 application (V2-Anwendung erstellen)|
|Benutzerverwaltung|Delete V1 application (V1-Anwendung löschen)|
|Benutzerverwaltung|Delete V2 application (V2-Anwendung löschen)|
|Benutzerverwaltung|Delete V2 application permission grant (Berechtigungszuweisung für V2-Anwendung löschen)|
|Benutzerverwaltung|Get V1 and V2 applications (V1- und V2-Anwendungen abrufen)|
|Benutzerverwaltung|Get V1 application (V1-Anwendung abrufen)|
|Benutzerverwaltung|Get V1 applications (V1-Anwendungen abrufen)|
|Benutzerverwaltung|Get V2 application (V2-Anwendung abrufen)|
|Benutzerverwaltung|Get V2 applications (V2-Anwendungen abrufen)|

## <a name="account-provisioning"></a>Kontobereitstellung

|Überwachungskategorie|Aktivität|
|---|---|
|Anwendungsverwaltung|Retrieve V2 application permissions grants (Berechtigungszuweisungen für V2-Anwendung abrufen)|
|Anwendungsverwaltung|Retrieve V2 application service principals in the current tenant (Dienstprinzipale der V2-Anwendung im aktuellen Mandanten abrufen)|
|Anwendungsverwaltung|Update V1 application (V1-Anwendung aktualisieren)|
|Anwendungsverwaltung|Update V2 application (V2-Anwendung aktualisieren)|
|Anwendungsverwaltung|Update V2 application permission grant (Berechtigungszuweisung für V2-Anwendung aktualisieren)|
|Anwendungsverwaltung|Add OAuth2PermissionGrant (OAuth2PermissionGrant hinzufügen)|
|Anwendungsverwaltung|Add app role assignment to service principal (App-Rollenzuweisung zu Dienstprinzipal hinzufügen)|

## <a name="application-proxy"></a>Anwendungsproxy

|Überwachungskategorie|Aktivität|
|---|---|
|Anwendungsverwaltung|Anwendung hinzufügen|
|Anwendungsverwaltung|Besitzer der Anwendung hinzufügen|
|Anwendungsverwaltung|Add owner to service principal (Besitzer zu Dienstprinzipal hinzufügen)|
|Anwendungsverwaltung|Richtlinie dem Dienstprinzipal hinzufügen|
|Verzeichnisverwaltung|Dienstprinzipal hinzufügen|
|Verzeichnisverwaltung|Anmeldeinformationen für Dienstprinzipal hinzufügen|
|Verzeichnisverwaltung|Consent to application (Anwendung zustimmen)|
|Verzeichnisverwaltung|Löschen der Anwendung|
|Verzeichnisverwaltung|Hard Delete application (Anwendung endgültig löschen)|
|Verzeichnisverwaltung|Remove OAuth2PermissionGrant (OAuth2PermissionGrant entfernen)|
|Verzeichnisverwaltung|Remove app role assignment from service principal (App-Rollenzuweisung von Dienstprinzipal entfernen)|
|Verzeichnisverwaltung|Besitzer aus Anwendung entfernen|
|Ressource|Remove owner from service principal (Besitzer aus Dienstprinzipal entfernen)|
|Ressource|Richtlinie aus Dienstprinzipal entfernen|
|Ressource|Dienstprinzipal entfernen|


## <a name="automated-password-rollover"></a>Automated password rollover (Automatisiertes Kennwortrollover)

|Überwachungskategorie|Aktivität|
|---|---|
|Anwendungsverwaltung|Anmeldeinformationen für Dienstprinzipal entfernen|


## <a name="b2c"></a>B2C

|Überwachungskategorie|Aktivität|
|---|---|
|Anwendungsverwaltung|Anwendung wiederherstellen|
|Anwendungsverwaltung|Zustimmung widerrufen|
|Anwendungsverwaltung|Aktualisieren der Anwendung|
|Anwendungsverwaltung|Externe geheime Schlüssel aktualisieren|
|Anwendungsverwaltung|Update service principal (Dienstprinzipal aktualisieren)|
|Anwendungsverwaltung|Issue an access token to the application (Zugriffstoken für die Anwendung ausstellen)|
|Anwendungsverwaltung|Issue an authorization code to the application (Autorisierungscode für die Anwendung ausstellen)|
|Anwendungsverwaltung|Issue an id_token to the application (ID-Token für die Anwendung ausstellen)|
|Anwendungsverwaltung|Validate local account credentials (Anmeldeinformationen des lokalen Kontos überprüfen)|
|Anwendungsverwaltung|Validate user authentication (Benutzerauthentifizierung überprüfen)|
|Anwendungsverwaltung|Add V2 application permissions (V2-Anwendungsberechtigungen hinzufügen)|
|Anwendungsverwaltung|Add a key based on ASCII secret to a CPIM key container (Schlüssel basierend auf einem ASCII-Geheimnis zu einem CPIM-Schlüsselcontainer hinzufügen)|
|Anwendungsverwaltung|Add a key to a CPIM key container (Schlüssel zu einem CPIM-Schlüsselcontainer hinzufügen)|
|Anwendungsverwaltung|AdminPolicyDatas-SetResources|
|Anwendungsverwaltung|AdminUserJourneys-GetResources|
|Anwendungsverwaltung|AdminUserJourneys-RemoveResources|
|Authentifizierung|AdminUserJourneys-SetResources|
|Authentifizierung|Create IdentityProvider (IdentityProvider-Element erstellen)|
|Authentifizierung|Create V1 application (V1-Anwendung erstellen)|
|Authentifizierung|Create V2 application (V2-Anwendung erstellen)|
|Authentifizierung|Create a custom domains in the tenant (Benutzerdefinierte Domäne im Mandanten erstellen)|
|Autorisierung|Create a new AdminUserJourney (Neues AdminUserJourney-Element erstellen)|
|Autorisierung|Create localized resource json (Lokalisierten JSON-Ressourcencode erstellen)|
|Autorisierung|Create new Custom IDP (Neuen benutzerdefinierten IDP erstellen)|
|Autorisierung|Create new IDP (Neuen IDP erstellen)|
|Autorisierung|Create or update a B2C directory resource (B2C-Verzeichnisressourcen erstellen oder aktualisieren)|
|Autorisierung|Richtlinie erstellen|
|Autorisierung|Create trustFramework policy (Vertrauensframework-Richtlinie erstellen)|
|Autorisierung|Create trustFramework policy with configurable prefix (Vertrauensframework-Richtlinie mit konfigurierbarem Präfix erstellen)|
|Autorisierung|Create user attribute (Benutzerattribut erstellen)|
|Autorisierung|CreateTrustFrameworkPolicy|
|Autorisierung|Creates or Update an new AdminUserJourney (Neues AdminUserJourney-Element erstellen oder aktualisieren)|
|Autorisierung|Identitätsanbieter löschen|
|Autorisierung|Delete IdentityProvider (IdentityProvider-Element löschen)|
|Autorisierung|Delete V1 application (V1-Anwendung löschen)|
|Autorisierung|Delete V2 application (V2-Anwendung löschen)|
|Autorisierung|Delete V2 application permission grant (Berechtigungszuweisung für V2-Anwendung löschen)|
|Autorisierung|Delete a B2C directory resource (B2C-Verzeichnisressource löschen)|
|Autorisierung|Delete a CPIM key container (CPIM-Schlüsselcontainer löschen)|
|Autorisierung|Delete trustFramework policy (Vertrauensframework-Richtlinie löschen)|
|Autorisierung|Benutzerattribut löschen|
|Autorisierung|Enable B2C feature (B2C-Feature aktivieren)|
|Autorisierung|Get B2C directory resources in a subscription (B2C-Verzeichnisressourcen in einem Abonnement abrufen)|
|Autorisierung|Get Custom IDP (Benutzerdefinierten IDP abrufen)|
|Autorisierung|Get IDP (IDP abrufen)|
|Autorisierung|Get V1 and V2 applications (V1- und V2-Anwendungen abrufen)|
|Autorisierung|Get V1 application (V1-Anwendung abrufen)|
|Autorisierung|Get V1 applications (V1-Anwendungen abrufen)|
|Autorisierung|Get V2 application (V2-Anwendung abrufen)|
|Autorisierung|Get V2 applications (V2-Anwendungen abrufen)|
|Autorisierung|B2C-Verzeichnisressource abrufen|
|Autorisierung|Get a list of custom domains in the tenant (Liste der benutzerdefinierten Domänen im Mandanten abrufen)|
|Autorisierung|Get a user journey (User Journey abrufen)|
|Autorisierung|Get allowed application claims for user journey (Zulässige Anwendungsansprüche für User Journey abrufen)|
|Autorisierung|Get allowed self-asserted claims for user journey (Zulässige selbstbestätigte Ansprüche für User Journey abrufen)|
|Autorisierung|Get allowed self-asserted claims of policy (Zulässige selbstbestätigte Ansprüche der Richtlinie abrufen)|
|Autorisierung|Get available output claims list (Liste der verfügbaren Ausgabeansprüche abrufen)|
|Autorisierung|Get content definitions for user journey (Inhaltsdefinitionen für User Journey abrufen)|
|Autorisierung|Get idps for a specific admin flow (IDPs für einen bestimmten Verwaltungsflow abrufen)|
|Autorisierung|Get key container active key metadata in JWK (Schlüsselcontainermetadaten zu aktiven Schlüsseln abrufen)|
|Autorisierung|Get list of all admin flows (Liste aller Verwaltungsflows abrufen)|
|Autorisierung|Get list of tags for all admin flows for all users (Liste der Tags für alle Verwaltungsflows für alle Benutzer abrufen)|
|Autorisierung|Get list of tenants for a user (Liste der Mandanten für einen Benutzer abrufen)|
|Autorisierung|Get local accounts' self-asserted claims (Selbstbestätigte Ansprüche der lokalen Konten abrufen)|
|Autorisierung|Get localized resource json (Lokalisierten JSON-Ressourcencode abrufen)|
|Autorisierung|Get operations of Microsoft.AzureActiveDirectory resource provider (Vorgänge des Microsoft.AzureActiveDirectory-Ressourcenanbieters abrufen)|
|Autorisierung|Get policies (Richtlinien abrufen)|
|Autorisierung|Get policy (Richtlinie abrufen)|
|Autorisierung|Get resource properties of a tenant (Ressourceneigenschaften eines Mandanten abrufen)|
|Autorisierung|Get supported IDP list (Liste der unterstützten IDPs abrufen)|
|Autorisierung|Get supported IDP list of the user journey (Liste der unterstützten IDPs der User Journey abrufen)|
|Autorisierung|Get tenant Info (Abrufen der Mandanteninformationen)|
|Autorisierung|Get tenant allowed features (Vom Mandanten zugelassene Features abrufen)|
|Autorisierung|Get tenant defined Custom IDP list (Vom Mandanten festgelegte Liste der benutzerdefinierten IDPs abrufen)|
|Autorisierung|Get tenant defined IDP list (Vom Mandanten festgelegte IDP-Liste)|
|Autorisierung|Get tenant defined local IDP list (Vom Mandanten festgelegte Liste der lokalen IDPs)|
|Autorisierung|Get tenant details for a user for resource creation (Mandantendetails für einen Benutzer zur Ressourcenerstellung abrufen)|
|Autorisierung|Get tenant list (Mandantenliste abrufen)|
|Autorisierung|Get tenantDomains (tenantDomains-Element abrufen)|
|Autorisierung|Get the default supported culture for CPIM (Standardmäßige unterstützte Kultur für CPIM abrufen)|
|Autorisierung|Get the details of an admin flow (Details eines Verwaltungsflows abrufen)|
|Autorisierung|Get the list of UserJourneys for this tenant (UserJourneys-Liste für diesen Mandanten abrufen)|
|Autorisierung|Get the set of available supported cultures for CPIM (Satz der verfügbaren unterstützten Kulturen für CPIM abrufen)|
|Autorisierung|Get trustFramework policy (Vertrauensframework-Richtlinie abrufen)|
|Autorisierung|Get trustFramework policy as xml (Vertrauensframework-Richtlinie als XML abrufen)|
|Autorisierung|Get user attribute (Benutzerattribut abrufen)|
|Autorisierung|Get user attributes (Benutzerattribute abrufen)|
|Autorisierung|Get user journey list (User Journey-Liste abrufen)|
|Autorisierung|GetIEFPolicies|
|Autorisierung|GetIdentityProviders|
|Autorisierung|GetTrustFrameworkPolicy|
|Autorisierung|Gets a CPIM key container in jwk format (CPIM-Schlüsselcontainer im JWK-Format abrufen)|
|Autorisierung|Gets list of key containers in the tenant (Liste der Schlüsselcontainer im Mandanten abrufen)|
|Autorisierung|Gets the type of tenant (Mandantentyp abrufen)|
|Autorisierung|MigrateTenantMetadata|
|Autorisierung|Patch IdentityProvider (IdentityProvider patchen)|
|Autorisierung|PutTrustFrameworkPolicy|
|Autorisierung|PutTrustFrameworkpolicy|
|Autorisierung|User Journey entfernen|
|Autorisierung|Restore a CPIM key container backup (Sicherung eines CPIM-Schlüsselcontainers wiederherstellen)|
|Autorisierung|Retrieve V2 application permissions grants (Berechtigungszuweisungen für V2-Anwendung abrufen)|
|Autorisierung|Retrieve V2 application service principals in the current tenant (Dienstprinzipale der V2-Anwendung im aktuellen Mandanten abrufen)|
|Autorisierung|Update Custom IDP (Benutzerdefinierten IDP aktualisieren)|
|Autorisierung|Update IDP (IDP aktualisieren)|
|Autorisierung|Lokalen IDP aktualisieren|
|Autorisierung|Update V1 application (V1-Anwendung aktualisieren)|
|Autorisierung|Update V2 application (V2-Anwendung aktualisieren)|
|Autorisierung|Update V2 application permission grant (Berechtigungszuweisung für V2-Anwendung aktualisieren)|
|Autorisierung|Aktualisieren von Richtlinien|
|Autorisierung|Update user attribute (Benutzerattribut aktualisieren)|
|Autorisierung|Upload a CPIM encrypted key (Verschlüsselten CPIM-Schlüssel hochladen)|
|Autorisierung|Benutzerautorisierung: Die API ist für die Mandantenfeatures deaktiviert.|
|Autorisierung|Benutzerautorisierung: Dem Benutzer wurde Zugriff als „Mandantenadministrator“ gewährt.|
|Autorisierung|Benutzerautorisierung: Dem Benutzer wurden Zugriffsrechte vom Typ „Authentifizierte Benutzer“ gewährt.|
|Autorisierung|Verify if B2C feature is enabled (Überprüfen, ob das B2C-Feature aktiviert ist)|
|Autorisierung|Überprüfen, ob das Feature aktiviert ist|
|Autorisierung|Programm erstellen|
|Autorisierung|Programm löschen|
|Autorisierung|Programmsteuerung verknüpfen|
|Autorisierung|Integration in Azure AD-Zugriffsüberprüfungen durchführen|
|Autorisierung|Verknüpfung der Programmsteuerung aufheben|
|Autorisierung|Programm aktualisieren|
|Autorisierung|Disable Desktop Sso (Desktop-SSO deaktivieren)|
|Autorisierung|Disable Desktop Sso for a specific domain (Desktop-SSO für eine bestimmte Domäne deaktivieren)|
|Autorisierung|Anwendungsproxy deaktivieren|
|Autorisierung|Passthrough-Authentifizierung deaktivieren|
|Autorisierung|Enable Desktop Sso (Desktop-SSO aktivieren)|
|Verzeichnisverwaltung|Enable Desktop Sso for a specific domain (Desktop-SSO für eine bestimmte Domäne aktivieren)|
|Verzeichnisverwaltung|Anwendungsproxy aktivieren|
|Verzeichnisverwaltung|Passthrough-Authentifizierung aktivieren|
|Verzeichnisverwaltung|Create a custom domains in the tenant (Benutzerdefinierte Domäne im Mandanten erstellen)|
|Verzeichnisverwaltung|Enable B2C feature (B2C-Feature aktivieren)|
|Verzeichnisverwaltung|Get a list of custom domains in the tenant (Liste der benutzerdefinierten Domänen im Mandanten abrufen)|
|Verzeichnisverwaltung|Get resource properties of a tenant (Ressourceneigenschaften eines Mandanten abrufen)|
|Verzeichnisverwaltung|Get tenant Info (Abrufen der Mandanteninformationen)|
|Verzeichnisverwaltung|Get tenant allowed features (Vom Mandanten zugelassene Features abrufen)|
|Verzeichnisverwaltung|Get tenantDomains (tenantDomains-Element abrufen)|
|Schlüssel|Gets the type of tenant (Mandantentyp abrufen)|
|Schlüssel|Verify if B2C feature is enabled (Überprüfen, ob das B2C-Feature aktiviert ist)|
|Schlüssel|Überprüfen, ob das Feature aktiviert ist|
|Schlüssel|Partner zu Unternehmen hinzufügen|
|Schlüssel|Add inverified domain (Nicht überprüfte Domäne hinzufügen)|
|Schlüssel|Add verified domain (Überprüfte Domäne hinzufügen)|
|Schlüssel|Unternehmen erstellen|
|Schlüssel|Unternehmenseinstellungen erstellen|
|Schlüssel|Unternehmenseinstellungen löschen|
|Schlüssel|Partner tiefer stufen|
|Schlüssel|Directory deleted (Verzeichnis gelöscht)|
|Andere|Directory deleted permanently (Verzeichnis unwiderruflich gelöscht)|
|Andere|Directory scheduled for deletion (Löschen des Verzeichnisses geplant)|
|Ressource|Promote company to partner (Unternehmen zu Partner heraufstufen)|
|Ressource|Rights Management-Eigenschaften bereinigen|
|Ressource|Partner aus Unternehmen entfernen|
|Ressource|Remove unverified domain (Nicht überprüfte Domäne entfernen)|
|Ressource|Remove verified domain (Überprüfte Domäne entfernen)|
|Ressource|Unternehmensinformationen festlegen|
|Ressource|DirSync-Funktion festlegen|
|Ressource|DirSyncEnabled-Flag festlegen|
|Ressource|Set Partnership (Partnerschaft festlegen)|
|Ressource|Schwellenwert für versehentliches Löschen festlegen|
|Ressource|Zulässigen Datenspeicherort für Unternehmen festlegen|
|Ressource|Funktion für multinationales Unternehmen auf „Aktiviert“ festlegen|
|Ressource|Verzeichnisfunktion für Mandanten festlegen|
|Ressource|Domänenauthentifizierung festlegen|
|Ressource|Verbundeinstellungen für Domäne festlegen|
|Ressource|Kennwortrichtlinie festlegen|
|Ressource|Eigenschaften für Rights Management festlegen|
|Ressource|Update company (Unternehmen aktualisieren)|
|Ressource|Unternehmenseinstellungen aktualisieren|
|Ressource|Domäne aktualisieren|
|Ressource|Domäne überprüfen|
|Ressource|Per E-Mail verifizierte Domäne überprüfen|
|Ressource|Onboarding|
|Ressource|Warnungseinstellungen aktualisieren|
|Ressource|Update weekly digest settings (Einstellungen für wöchentliche Übersicht aktualisieren)|
|Ressource|Disable password writeback for directory (Kennwortrückschreiben für Verzeichnis deaktivieren)|
|Ressource|Enable password writeback for directory (Kennwortrückschreiben für Verzeichnis aktivieren)|
|Ressource|Add app role assignment to group (App-Rollenzuweisung zu Gruppe hinzufügen)|
|Ressource|Gruppe hinzufügen|
|Ressource|Mitglied zur Gruppe hinzufügen|
|Ressource|Add owner to group (Besitzer zu Gruppe hinzufügen)|
|Ressource|Create group settings (Gruppeneinstellungen erstellen)|
|Ressource|Gruppe löschen|
|Ressource|Delete group settings (Gruppeneinstellungen löschen)|
|Ressource|Finish applying group based license to users (Gruppenbasierte Lizenzzuweisung zu Benutzern fertig stellen)|
|Ressource|Hard Delete group (Gruppe endgültig löschen)|
|Ressource|Remove app role assignment from group (App-Rollenzuweisung von Gruppe entfernen)|
|Ressource|Mitglied aus Gruppe entfernen|
|Ressource|Remove owner from group (Besitzer aus Gruppe entfernen)|
|Ressource|Gruppe wiederherstellen|
|Ressource|Set group license (Gruppenlizenz festlegen)|
|Ressource|Gruppe für Verwaltung durch Benutzer festgelegt|
|Ressource|Start applying group based license to users (Starten der gruppenbasierten Lizenzzuweisung zu Benutzern)|
|Ressource|Trigger group license recalculation (Neuberechnung der Gruppenlizenzen auslösen)|
|Ressource|Gruppe aktualisieren|
|Ressource|Update group settings (Gruppeneinstellungen aktualisieren)|
|Ressource|Mitglied hinzufügen|
|Ressource|Erstellen einer Gruppe|
|Ressource|Gruppe löschen|
|Ressource|Mitglied entfernen|
|Ressource|Gruppe aktualisieren|
|Ressource|Approve a pending request to join a group (Ausstehende Gruppenbeitrittsanforderung genehmigen)|
|Ressource|Cancel a pending request to join a group (Ausstehende Gruppenbeitrittsanforderung abbrechen)|
|Ressource|Create lifecycle management policy (Richtlinie für Lebenszyklusverwaltung erstellen)|
|Ressource|Delete a pending request to join a group (Ausstehende Gruppenbeitrittsanforderung löschen)|
|Ressource|Reject a pending request to join a group (Ausstehende Gruppenbeitrittsanforderung ablehnen)|
|Ressource|Gruppe verlängern|
|Ressource|Request to join a group (Gruppenbeitritt anfordern)|
|Ressource|Set dynamic group properties (Dynamische Gruppeneigenschaften festlegen)|
|Ressource|Update lifecycle management policy (Richtlinie für Lebenszyklusverwaltung aktualisieren)|
|Ressource|Add a key based on ASCII secret to a CPIM key container (Schlüssel basierend auf einem ASCII-Geheimnis zu einem CPIM-Schlüsselcontainer hinzufügen)|
|Ressource|Add a key to a CPIM key container (Schlüssel zu einem CPIM-Schlüsselcontainer hinzufügen)|
|Ressource|Delete a CPIM key container (CPIM-Schlüsselcontainer löschen)|
|Ressource|Delete key container (Schlüsselcontainer löschen)|
|Ressource|Get key container active key metadata in JWK (Schlüsselcontainermetadaten zu aktiven Schlüsseln abrufen)|
|Ressource|Get key container metadata (Metadaten des Schlüsselcontainers abrufen)|
|Ressource|Gets a CPIM key container in jwk format (CPIM-Schlüsselcontainer im JWK-Format abrufen)|
|Ressource|Gets list of key containers in the tenant (Liste der Schlüsselcontainer im Mandanten abrufen)|
|Ressource|Restore a CPIM key container backup (Sicherung eines CPIM-Schlüsselcontainers wiederherstellen)|
|Ressource|Save key container (Schlüsselcontainer speichern)|
|Ressource|Upload a CPIM encrypted key (Verschlüsselten CPIM-Schlüssel hochladen)|
|Ressource|Issue an authorization code to the application (Autorisierungscode für die Anwendung ausstellen)|
|Ressource|Issue an id_token to the application (ID-Token für die Anwendung ausstellen)|


## <a name="core-directory"></a>Core directory (Kernverzeichnis)

|Überwachungskategorie|Aktivität|
|---|---|
|Verwaltung administrativer Einheiten|Download a single risk event type (Einzelnen Risikoereignistyp herunterladen)|
|Verwaltung administrativer Einheiten|Download admins and status of weekly digest opt-in (Administratoren und Status der Aktivierung der wöchentlichen Übersicht herunterladen)|
|Verwaltung administrativer Einheiten|Download all risk event types (Alle Risikoereignistypen herunterladen)|
|Verwaltung administrativer Einheiten|Download free user risk events (Kostenlose Benutzerrisikoereignisse herunterladen)|
|Verwaltung administrativer Einheiten|Download users flagged for risk (Benutzer herunterladen, für die ein Risiko angezeigt wird)|
|Anwendungsverwaltung|Verarbeitete Batch-Einladungen|
|Anwendungsverwaltung|Batch-Einladungen hochgeladen|
|Anwendungsverwaltung|Add owner to policy (Besitzer zu Richtlinie hinzufügen)|
|Anwendungsverwaltung|Richtlinie hinzufügen|
|Anwendungsverwaltung|Löschen von Richtlinien|
|Anwendungsverwaltung|Richtlinien-Anmeldeinformationen entfernen|
|Anwendungsverwaltung|Aktualisieren von Richtlinien|
|Anwendungsverwaltung|Set MFA registration policy (MFA-Registrierungsrichtlinie festlegen)|
|Anwendungsverwaltung|Set sign-in risk policy (Richtlinie zum Anmelderisiko festlegen)|
|Anwendungsverwaltung|Set user risk policy (Richtlinie zum Benutzerrisiko festlegen)|
|Anwendungsverwaltung|Accept Terms Of Use (Nutzungsbedingungen akzeptieren)|
|Anwendungsverwaltung|Create Terms Of Use (Nutzungsbedingungen erstellen)|
|Anwendungsverwaltung|Decline Terms Of Use (Nutzungsbedingungen ablehnen)|
|Anwendungsverwaltung|Delete Terms Of Use (Nutzungsbedingungen löschen)|
|Anwendungsverwaltung|Edit Terms Of Use (Nutzungsbedingungen bearbeiten)|
|Anwendungsverwaltung|Publish Terms Of Use (Nutzungsbedingungen veröffentlichen)|
|Anwendungsverwaltung|Unpublish Terms Of Use (Veröffentlichung der Nutzungsbedingungen aufheben)|
|Anwendungsverwaltung|Add application SSL certificate (Anwendungs-SSL-Zertifikat hinzufügen)|
|Anwendungsverwaltung|Delete SSL binding (SSL-Bindung löschen)|
|Anwendungsverwaltung|Register connector (Connector registrieren)|
|Anwendungsverwaltung|AdminPolicyDatas-RemoveResources|
|Anwendungsverwaltung|AdminPolicyDatas-SetResources|
|Anwendungsverwaltung|AdminUserJourneys-GetResources|
|Verzeichnisverwaltung|AdminUserJourneys-RemoveResources|
|Verzeichnisverwaltung|AdminUserJourneys-SetResources|
|Verzeichnisverwaltung|Create IdentityProvider (IdentityProvider-Element erstellen)|
|Verzeichnisverwaltung|Create a new AdminUserJourney (Neues AdminUserJourney-Element erstellen)|
|Verzeichnisverwaltung|Create localized resource json (Lokalisierten JSON-Ressourcencode erstellen)|
|Verzeichnisverwaltung|Create new Custom IDP (Neuen benutzerdefinierten IDP erstellen)|
|Verzeichnisverwaltung|Create new IDP (Neuen IDP erstellen)|
|Verzeichnisverwaltung|Create or update a B2C directory resource (B2C-Verzeichnisressourcen erstellen oder aktualisieren)|
|Verzeichnisverwaltung|Richtlinie erstellen|
|Verzeichnisverwaltung|Create trustFramework policy (Vertrauensframework-Richtlinie erstellen)|
|Verzeichnisverwaltung|Create trustFramework policy with configurable prefix (Vertrauensframework-Richtlinie mit konfigurierbarem Präfix erstellen)|
|Verzeichnisverwaltung|Create user attribute (Benutzerattribut erstellen)|
|Verzeichnisverwaltung|CreateTrustFrameworkPolicy|
|Verzeichnisverwaltung|Identitätsanbieter löschen|
|Verzeichnisverwaltung|Delete IdentityProvider (IdentityProvider-Element löschen)|
|Verzeichnisverwaltung|Delete a B2C directory resource (B2C-Verzeichnisressource löschen)|
|Verzeichnisverwaltung|Delete trustFramework policy (Vertrauensframework-Richtlinie löschen)|
|Verzeichnisverwaltung|Benutzerattribut löschen|
|Verzeichnisverwaltung|Get B2C directory resources in a resource group (B2C-Verzeichnisressourcen in einer Ressourcengruppe abrufen)|
|Verzeichnisverwaltung|Get B2C directory resources in a subscription (B2C-Verzeichnisressourcen in einem Abonnement abrufen)|
|Verzeichnisverwaltung|Get Custom IDP (Benutzerdefinierten IDP abrufen)|
|Verzeichnisverwaltung|Get IDP (IDP abrufen)|
|Verzeichnisverwaltung|B2C-Verzeichnisressource abrufen|
|Verzeichnisverwaltung|Get a user journey (User Journey abrufen)|
|Verzeichnisverwaltung|Get allowed application claims for user journey (Zulässige Anwendungsansprüche für User Journey abrufen)|
|Verzeichnisverwaltung|Get allowed self-asserted claims for user journey (Zulässige selbstbestätigte Ansprüche für User Journey abrufen)|
|Verzeichnisverwaltung|Get allowed self-asserted claims of policy (Zulässige selbstbestätigte Ansprüche der Richtlinie abrufen)|
|Verzeichnisverwaltung|Get available output claims list (Liste der verfügbaren Ausgabeansprüche abrufen)|
|Verzeichnisverwaltung|Get content definitions for user journey (Inhaltsdefinitionen für User Journey abrufen)|
|Verzeichnisverwaltung|Get idps for a specific admin flow (IDPs für einen bestimmten Verwaltungsflow abrufen)|
|Verzeichnisverwaltung|Get list of all admin flows (Liste aller Verwaltungsflows abrufen)|
|Verzeichnisverwaltung|Get list of tags for all admin flows for all users (Liste der Tags für alle Verwaltungsflows für alle Benutzer abrufen)|
|Gruppenverwaltung|Get list of tenants for a user (Liste der Mandanten für einen Benutzer abrufen)|
|Gruppenverwaltung|Get local accounts' self-asserted claims (Selbstbestätigte Ansprüche der lokalen Konten abrufen)|
|Gruppenverwaltung|Get localized resource json (Lokalisierten JSON-Ressourcencode abrufen)|
|Gruppenverwaltung|Get operations of Microsoft.AzureActiveDirectory resource provider (Vorgänge des Microsoft.AzureActiveDirectory-Ressourcenanbieters abrufen)|
|Gruppenverwaltung|Get policies (Richtlinien abrufen)|
|Gruppenverwaltung|Get policy (Richtlinie abrufen)|
|Gruppenverwaltung|Get supported IDP list (Liste der unterstützten IDPs abrufen)|
|Gruppenverwaltung|Get supported IDP list of the user journey (Liste der unterstützten IDPs der User Journey abrufen)|
|Gruppenverwaltung|Get tenant defined Custom IDP list (Vom Mandanten festgelegte Liste der benutzerdefinierten IDPs abrufen)|
|Gruppenverwaltung|Get tenant defined IDP list (Vom Mandanten festgelegte IDP-Liste)|
|Gruppenverwaltung|Get tenant defined local IDP list (Vom Mandanten festgelegte Liste der lokalen IDPs)|
|Gruppenverwaltung|Get tenant details for a user for resource creation (Mandantendetails für einen Benutzer zur Ressourcenerstellung abrufen)|
|Gruppenverwaltung|Get the default supported culture for CPIM (Standardmäßige unterstützte Kultur für CPIM abrufen)|
|Gruppenverwaltung|Get the details of an admin flow (Details eines Verwaltungsflows abrufen)|
|Gruppenverwaltung|Get the list of UserJourneys for this tenant (UserJourneys-Liste für diesen Mandanten abrufen)|
|Gruppenverwaltung|Get the set of available supported cultures for CPIM (Satz der verfügbaren unterstützten Kulturen für CPIM abrufen)|
|Gruppenverwaltung|Get trustFramework policy (Vertrauensframework-Richtlinie abrufen)|
|Gruppenverwaltung|Get trustFramework policy as xml (Vertrauensframework-Richtlinie als XML abrufen)|
|Gruppenverwaltung|Get user attribute (Benutzerattribut abrufen)|
|Richtlinienverwaltung|Get user attributes (Benutzerattribute abrufen)|
|Richtlinienverwaltung|Get user journey list (User Journey-Liste abrufen)|
|Richtlinienverwaltung|GetIEFPolicies|
|Richtlinienverwaltung|GetIdentityProviders|
|Richtlinienverwaltung|GetTrustFrameworkPolicy|
|Ressource|MigrateTenantMetadata|
|Ressource|Verschieben von Ressourcen|
|Ressource|Patch IdentityProvider (IdentityProvider patchen)|
|Ressource|PutTrustFrameworkPolicy|
|Ressource|PutTrustFrameworkpolicy|
|Ressource|User Journey entfernen|
|Ressource|Update Custom IDP (Benutzerdefinierten IDP aktualisieren)|
|Ressource|Update IDP (IDP aktualisieren)|
|Ressource|Lokalen IDP aktualisieren|
|Ressource|Update a B2C directory resource (B2C-Verzeichnisressource aktualisieren)|
|Ressource|Aktualisieren von Richtlinien|
|Ressource|Update subscription status (Abonnementstatus aktualisieren)|
|Rollenverwaltung|Update user attribute (Benutzerattribut aktualisieren)|
|Rollenverwaltung|Validate move resources (Verschiebung von Ressourcen überprüfen)|
|Rollenverwaltung|Gerät hinzufügen|
|Rollenverwaltung|Add device configuration (Gerätekonfiguration hinzufügen)|
|Rollenverwaltung|Add registered owner to device (Registrierten Besitzer zu Gerät hinzufügen)|
|Rollenverwaltung|Add registered users to device (Registrierte Benutzer zu Gerät hinzufügen)|
|Rollenverwaltung|Löschen des Geräts|
|Rollenverwaltung|Löschen der Gerätekonfiguration|
|Rollenverwaltung|Device no longer compliant (Gerät nicht mehr konform)|
|Rollenverwaltung|Device no longer managed (Gerät nicht mehr verwaltet)|
|Benutzerverwaltung|Registrierten Besitzer aus Gerät entfernen|
|Benutzerverwaltung|Registrierte Benutzer aus Gerät entfernen|
|Benutzerverwaltung|Gerät aktualisieren|
|Benutzerverwaltung|Update device configuration (Gerätekonfiguration aktualisieren)|
|Benutzerverwaltung|Add eligible member to role (Berechtigtes Mitglied zur Rolle hinzufügen)|
|Benutzerverwaltung|Add member to role (Mitglied zu Rolle hinzufügen)|
|Benutzerverwaltung|Add role assignment to role definition (Rollenzuweisung zu Rollendefinition hinzufügen)|
|Benutzerverwaltung|Add role from template (Rolle aus Vorlage hinzufügen)|
|Benutzerverwaltung|Add scoped member to role (Zugeordnetes Mitglied zu Rolle hinzufügen)|
|Benutzerverwaltung|Remove eligible member from role (Berechtigtes Mitglied aus Rolle entfernen)|
|Benutzerverwaltung|Remove member from role (Mitglied aus Rolle entfernen)|
|Benutzerverwaltung|Remove role assignment from role definition (Rollenzuweisung aus Rollendefinition entfernen)|
|Benutzerverwaltung|Remove scoped member from role (Zugeordnetes Mitglied aus Rolle entfernen)|
|Benutzerverwaltung|Rolle aktualisieren|
|Benutzerverwaltung|AccessReview_Review|
|Benutzerverwaltung|AccessReview_Update|
|Benutzerverwaltung|ActivationAborted|
|Benutzerverwaltung|ActivationApproved|
|Benutzerverwaltung|ActivationCanceled|
|Benutzerverwaltung|ActivationRequested|
|Benutzerverwaltung|Hinzugefügt|
|Benutzerverwaltung|Zuweisen|


## <a name="identity-protection"></a>Schutz der Identität (Identity Protection)

|Überwachungskategorie|Aktivität|
|---|---|
|Verzeichnisverwaltung|Erhöhen|
|Verzeichnisverwaltung|Entfernt|
|Verzeichnisverwaltung|Änderungen für Rolleneinstellung|
|Andere|ScanAlertsNow|
|Andere|Signup|
|Andere|Deautorisieren|
|Andere|UpdateAlertSettings|
|Andere|UpdateCurrentState|
|Richtlinienverwaltung|Zugriffsüberprüfung beendet|
|Richtlinienverwaltung|Genehmigende Person zu Anforderungsgenehmigung hinzufügen|
|Richtlinienverwaltung|Reviewer zu Zugriffsüberprüfung hinzufügen|
|Benutzerverwaltung|Zugriffsüberprüfung anwenden|
|Benutzerverwaltung|Zugriffsüberprüfung erstellen|


## <a name="invited-users"></a>Invited Users (Eingeladene Benutzer)

|Überwachungskategorie|Aktivität|
|---|---|
|Andere|Anforderungsgenehmigung erstellen|
|Andere|Zugriffsüberprüfung löschen|
|Benutzerverwaltung|Reviewer aus Zugriffsüberprüfung entfernen|
|Benutzerverwaltung|Anwenden der Überprüfungsergebnisse anfordern|
|Benutzerverwaltung|Überprüfungsbeendigung anfordern|
|Benutzerverwaltung|App-Zuweisung überprüfen|
|Benutzerverwaltung|Gruppenmitgliedschaft überprüfen|
|Benutzerverwaltung|RBAC-Rollenmitgliedschaft überprüfen|


## <a name="microsoft-identity-manager-mim"></a>Microsoft Identity Manager (MIM)

|Überwachungskategorie|Aktivität|
|---|---|
|Gruppenverwaltung|Genehmigungsanforderung überprüfen|
|Gruppenverwaltung|Zugriffsüberprüfung aktualisieren|
|Gruppenverwaltung|Einstellungen für E-Mail-Benachrichtigungen für Zugriffsüberprüfung aktualisieren|
|Gruppenverwaltung|Einstellung für die Wiederholungsanzahl der Zugriffsüberprüfungen aktualisieren|
|Gruppenverwaltung|Einstellung für die Wiederholungsdauer der Zugriffsüberprüfungen in Tagen aktualisieren|
|Benutzerverwaltung|Einstellung für den Wiederholungsendtyp der Zugriffsüberprüfungen aktualisieren|
|Benutzerverwaltung|Einstellung für den Wiederholungstyp der Zugriffsüberprüfungen aktualisieren|



## <a name="privileged-identity-management"></a>Privileged Identity Management

|Überwachungskategorie|Aktivität|
|---|---|
|Rollenverwaltung|Einstellungen für Erinnerungen für Zugriffsüberprüfung aktualisieren|
|Rollenverwaltung|Anforderungsgenehmigung aktualisieren|
|Rollenverwaltung|Add app role assignment grant to user (App-Rollenzuweisungsgewährung zu Benutzer hinzufügen)|
|Rollenverwaltung|Benutzer hinzufügen|
|Rollenverwaltung|Add users strong authentication phone app detail (Phone-App-Details für die strikte Authentifizierung der Benutzer hinzufügen)|
|Rollenverwaltung|Benutzerlizenz ändern|
|Rollenverwaltung|Benutzerkennwort ändern|
|Rollenverwaltung|Convert federated user to managed (Verbundbenutzer in verwalteten Benutzer konvertieren)|
|Rollenverwaltung|Create application password for user (Anwendungskennwort für Benutzer erstellen)|
|Rollenverwaltung|Delete application password for user (Anwendungskennwort für Benutzer löschen)|
|Rollenverwaltung|Benutzer löschen|
|Rollenverwaltung|Disable account (Konto deaktivieren)|
|Rollenverwaltung|Enable Strong Authentication (Strikte Authentifizierung aktivieren)|
|Rollenverwaltung|Hard Delete user (Benutzer endgültig löschen)|
|Rollenverwaltung|Remove app role assignment from user (App-Rollenzuweisung von Benutzer entfernen)|
|Rollenverwaltung|Remove users strong authentication phone app detail (Phone-App-Details für die strenge Authentifizierung der Benutzer entfernen)|



## <a name="self-service-group-management"></a>Self-Service-Gruppenverwaltung

|Überwachungskategorie|Aktivität|
|---|---|
|Gruppenverwaltung|Benutzerkennwort zurücksetzen|
|Gruppenverwaltung|Benutzer wiederherstellen|
|Gruppenverwaltung|Änderung des Benutzerkennworts erzwingen|
|Gruppenverwaltung|Set user manager (Benutzer-Manager festlegen)|
|Gruppenverwaltung|Set users oath token metadata enabled (OATH-Tokenmetadaten der Benutzer auf „Aktiviert“ festlegen)|
|Gruppenverwaltung|Update StsRefreshTokenValidFrom Timestamp (Zeitstempel StsRefreshTokenValidFrom aktualisieren)|
|Gruppenverwaltung|Externe geheime Schlüssel aktualisieren|
|Gruppenverwaltung|Benutzer aktualisieren|
|Gruppenverwaltung|Admin generates a temporary password (Administrator generiert ein temporäres Kennwort.)|


## <a name="self-service-password-management"></a>Self-service password management (Self-Service-Kennwortverwaltung)

|Überwachungskategorie|Aktivität|
|---|---|
|Verzeichnisverwaltung|Admins requires the user to reset their password (Administratoren fordern Benutzer zum Zurücksetzen ihres Kennworts auf.)|
|Verzeichnisverwaltung|Externen Benutzer einer Anwendung zuweisen|
|Benutzerverwaltung|Email not sent, user unsubscribed (E-Mail nicht gesendet, Abonnement des Benutzers gekündigt)|
|Benutzerverwaltung|Externen Benutzer einladen|
|Benutzerverwaltung|Einlösen einer externen Benutzereinladung|
|Benutzerverwaltung|Erstellen eines viralen Mandanten|
|Benutzerverwaltung|Erstellen eines viralen Benutzers|
|Benutzerverwaltung|User Password Registration (Benutzerkennwortregistrierung)|
|Benutzerverwaltung|User Password Reset (Benutzerkennwortzurücksetzung)|
|Benutzerverwaltung|Blocked from self-service password reset (Von Self-Service-Kennwortzurücksetzung ausgeschlossen)|


## <a name="terms-of-use"></a>Nutzungsbedingungen

|Überwachungskategorie|Aktivität|
|---|---|
|Richtlinienverwaltung|Change password (self-service) (Kennwort ändern (Self-Service))|
|Richtlinienverwaltung|Reset password (by admin) (Kennwort zurücksetzen (durch Administrator))|
|Richtlinienverwaltung|Reset password (self-service) (Kennwort zurücksetzen (Self-Service))|
|Richtlinienverwaltung|Self serve password reset flow activity progress (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs)|
|Richtlinienverwaltung|Self-service password reset flow activity progress (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs)|
|Richtlinienverwaltung|Unlock user account (self-service) (Benutzerkonto entsperren (Self-Service))|
|Richtlinienverwaltung|User registered for self-service password reset (Für Self-Service-Kennwortzurücksetzung registrierter Benutzer)|


## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure AD-Berichte](overview-reports.md)
- [Bericht „Überwachungsprotokolle“](concept-audit-logs.md) 
- [Programmgesteuerter Zugriff auf Azure AD-Berichte](concept-reporting-api.md)
