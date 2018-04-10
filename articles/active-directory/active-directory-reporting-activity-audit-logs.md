---
title: Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal | Microsoft-Dokumentation
description: Enthält eine Einführung in die Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/31/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 97ea32a1e0f8815accff6201251771ab8c088859
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a>Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal 

Mit der Berichterstellungsfunktion in Azure Active Directory (Azure AD) können Sie alle Informationen abrufen, die Sie zum Ermitteln des Zustands Ihrer Umgebung benötigen.

Die Architektur für die Berichterstellung in Azure AD umfasst die folgenden Komponenten:

- **Aktivität** 
    - **Anmeldeaktivitäten** : Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung
    - **Überwachungsprotokolle:** Systemaktivitätsinformationen zu Benutzern und zur Gruppenverwaltung sowie zu verwalteten Anwendungen und Verzeichnisaktivitäten.
- **Sicherheit** 
    - **Riskante Anmeldungen:** Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. Weitere Informationen finden Sie unter „Riskante Anmeldungen“.
    - **Benutzer mit Risikomarkierung:** Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. Weitere Informationen finden Sie unter „Benutzer mit Risikomarkierung“.

In diesem Thema erhalten Sie einen Überblick über die Überwachungsaktivitäten.
 
## <a name="who-can-access-the-data"></a>Wer kann auf die Daten zugreifen?
* Benutzer mit den Rollen „Sicherheitsadministrator“ oder der Berechtigung „Sicherheit lesen“
* Globale Administratoren
* Einzelne Benutzer (Nicht-Administratoren) können eigene Aktivitäten anzeigen.


## <a name="audit-logs"></a>Überwachungsprotokolle

Die Überwachungsprotokolle in Azure Active Directory enthalten Datensätze mit Systemaktivitäten, die zum Nachweisen der Konformität verwendet werden können.  
Ihr erster Einstiegspunkt für alle Überwachungsdaten ist die Option **Überwachungsprotokolle** im Abschnitt **Aktivität** von **Azure Active Directory**.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/61.png "Überwachungsprotokolle")

Ein Überwachungsprotokoll enthält eine Standardlistenansicht mit folgenden Informationen:

- Datum und Uhrzeit des Auftretens
- Initiator/Akteur einer Aktivität (*Wer*) 
- Aktivität (*Was*) 
- Ziel

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/18.png "Überwachungsprotokolle")

Sie können die Listenansicht anpassen, indem Sie auf der Symbolleiste auf **Spalten** klicken.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/19.png "Überwachungsprotokolle")

So können Sie weitere Felder anzeigen oder bereits angezeigte Felder entfernen.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/21.png "Überwachungsprotokolle")


Wenn Sie in der Listenansicht auf einen Eintrag klicken, werden die dazu verfügbaren Details angezeigt.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/22.png "Überwachungsprotokolle")


## <a name="filtering-audit-logs"></a>Filtern von Überwachungsprotokollen

Sie können die Überwachungsdaten mit den folgenden Feldern filtern, um die gemeldeten Daten gemäß Ihren Bedürfnissen einzugrenzen:

- Datumsbereich
- Initiiert von (Akteur)
- Category (Kategorie)
- Aktivitätsressourcentyp
- Aktivität

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/23.png "Überwachungsprotokolle")


Mit dem Filter **Datumsbereich** können Sie einen Zeitrahmen für die zurückgegebenen Daten festlegen.  
Mögliche Werte:

- 1 Monat
- 7 Tage
- 24 Stunden
- Benutzerdefiniert

Beim Auswählen eines benutzerdefinierten Zeitraums können Sie eine Startzeit und eine Endzeit konfigurieren.

Bei Verwendung des Filters **Initiiert von** können Sie den Namen eines Akteurs oder dessen Benutzerprinzipalnamen (User Principal Name, UPN) definieren.

Bei Verwendung des Filters **Kategorie** können Sie eine der folgenden Filteroptionen auswählen:

- Alle
- Core category (Kernkategorie)
- Core directory (Kernverzeichnis)
- Self-service password management (Self-Service-Kennwortverwaltung)
- Self-Service-Gruppenverwaltung
- Kontobereitstellung: Automated password rollover (Automatisiertes Kennwortrollover)
- Invited Users (Eingeladene Benutzer)
- MIM service (MIM-Dienst)
- Schutz der Identität (Identity Protection)
- B2C

Bei Verwendung des Filters **Aktivitätsressourcentyp** können Sie eine der folgenden Filteroptionen auswählen:

- Alle 
- Gruppe
- Verzeichnis
- Benutzer
- Anwendung
- Richtlinie
- Gerät
- Andere

Wenn Sie als **Aktivitätsressourcentyp** die Option **Gruppe** auswählen, erhalten Sie eine zusätzliche Filterkategorie zur Angabe einer **Quelle**:

- Azure AD
- O365


Der Filter **Aktivität** basiert auf der getroffenen Auswahl für „Kategorie“ und „Aktivitätsressourcentyp“. Sie können entweder eine bestimmte Aktivität verwenden oder alle auswählen. 

Sie können die Liste aller Überwachungsaktivitäten mit der Graph-API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta abrufen. Dabei ist „$tenantdomain“ Ihr Domänenname. Alternativ können Sie die Informationen im Artikel [Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal](active-directory-reporting-audit-events.md) lesen.


## <a name="audit-logs-shortcuts"></a>Verknüpfungen für Überwachungsprotokolle

Neben **Azure Active Directory** bietet das Azure-Portal noch zwei weitere Einstiegspunkte für Überwachungsdaten:

- Benutzer und Gruppen
- Unternehmensanwendungen

### <a name="users-and-groups-audit-logs"></a>Überwachungsprotokolle für Benutzer und Gruppen

Mit Überwachungsberichten, die auf Benutzern und Gruppen basieren, können Sie beispielsweise Antworten auf folgende Fragen erhalten:

- Welche Arten von Updates wurden von den Benutzern angewendet?

- Wie viele Benutzer wurden geändert?

- Wie viele Kennwörter wurden geändert?

- Welche Schritte hat ein Administrator in einem Verzeichnis ausgeführt?

- Welche Gruppen wurden hinzugefügt?

- Sind Gruppen mit Änderungen der Mitgliedschaft vorhanden?

- Haben sich die Besitzer der Gruppe geändert?

- Welche Lizenzen wurden einer Gruppe oder einem Benutzer zugewiesen?

Wenn Sie nur Überwachungsdaten überprüfen möchten, die sich auf Benutzer und Gruppen beziehen, können Sie die gefilterte Ansicht unter **Überwachungsprotokolle** im Abschnitt **Aktivität** der Option **Benutzer und Gruppen** verwenden. Bei diesem Einstiegspunkt ist für **Aktivitätsressourcentyp** bereits vorab die Option **Benutzer und Gruppen** ausgewählt.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/93.png "Überwachungsprotokolle")

### <a name="enterprise-applications-audit-logs"></a>Überwachungsprotokolle für Unternehmensanwendungen

Mit Überwachungsberichten, die auf Anwendungen basieren, können Sie beispielsweise Antworten auf folgende Fragen erhalten:

* Welche Anwendungen wurden hinzugefügt oder aktualisiert?
* Welche Anwendungen wurden entfernt?
* Hat sich ein Dienstprinzip für eine Anwendung geändert?
* Haben sich die Namen von Anwendungen geändert?
* Wer hat die Zustimmung zu einer Anwendung erteilt?

Wenn Sie nur Überwachungsdaten überprüfen möchten, die sich auf Ihre Anwendungen beziehen, können Sie die gefilterte Ansicht unter **Überwachungsprotokolle** im Abschnitt **Aktivität** des Blatts **Unternehmensanwendungen** verwenden. Bei diesem Einstiegspunkt ist für **Aktivitätsressourcentyp** bereits vorab die Option **Unternehmensanwendungen** ausgewählt.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/134.png "Überwachungsprotokolle")

Dieser Ansicht kann weiter nach **Gruppen** oder **Benutzer** gefiltert werden.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/25.png "Überwachungsprotokolle")



## <a name="azure-ad-audit-activity-list"></a>Liste Azure AD-Überwachungsaktivitäten

Dieser Abschnitt enthält eine Liste aller Aktivitäten, die protokolliert werden können. 


|Service Name|Überwachungskategorie|Aktivitätsressourcentyp|Aktivität|
|---|---|---|---|
|Kontobereitstellung|Anwendungsverwaltung|Anwendung|Verwaltung|
|Kontobereitstellung|Anwendungsverwaltung|Anwendung|Directory Operation (Verzeichnisvorgang)|
|Kontobereitstellung|Anwendungsverwaltung|Anwendung|Export|
|Kontobereitstellung|Anwendungsverwaltung|Anwendung|Importieren|
|Kontobereitstellung|Anwendungsverwaltung|Anwendung|Andere|
|Kontobereitstellung|Anwendungsverwaltung|Anwendung|Process Escrow (Hinterlegung verarbeiten)|
|Kontobereitstellung|Anwendungsverwaltung|Anwendung|Synchronization Rule Action (Synchronisierungsregelaktion)|
|Anwendungsproxy|Anwendungsverwaltung|Anwendung|Anwendung hinzufügen|
|Anwendungsproxy|Ressource|Ressource|Add application SSL certificate (Anwendungs-SSL-Zertifikat hinzufügen)|
|Anwendungsproxy|Ressource|Ressource|Delete SSL binding (SSL-Bindung löschen)|
|Anwendungsproxy|Anwendungsverwaltung|Anwendung|Löschen der Anwendung|
|Anwendungsproxy|Verzeichnisverwaltung|Verzeichnis|Disable Desktop Sso (Desktop-SSO deaktivieren)|
|Anwendungsproxy|Verzeichnisverwaltung|Verzeichnis|Disable Desktop Sso for a specific domain (Desktop-SSO für eine bestimmte Domäne deaktivieren)|
|Anwendungsproxy|Verzeichnisverwaltung|Verzeichnis|Anwendungsproxy deaktivieren|
|Anwendungsproxy|Verzeichnisverwaltung|Verzeichnis|Passthrough-Authentifizierung deaktivieren|
|Anwendungsproxy|Verzeichnisverwaltung|Verzeichnis|Enable Desktop Sso (Desktop-SSO aktivieren)|
|Anwendungsproxy|Verzeichnisverwaltung|Verzeichnis|Enable Desktop Sso for a specific domain (Desktop-SSO für eine bestimmte Domäne aktivieren)|
|Anwendungsproxy|Verzeichnisverwaltung|Verzeichnis|Anwendungsproxy aktivieren|
|Anwendungsproxy|Verzeichnisverwaltung|Verzeichnis|Passthrough-Authentifizierung aktivieren|
|Anwendungsproxy|Ressource|Ressource|Register connector (Connector registrieren)|
|Anwendungsproxy|Anwendungsverwaltung|Anwendung|Aktualisieren der Anwendung|
|Anwendungsproxy|Anwendungsverwaltung|Anwendung|Update application Single Sign-On Mode (SSO-Modus der Anwendung aktualisieren)|
|Automatisiertes Kennwortrollover|Anwendungsverwaltung|Anwendung|Automatisiertes Kennwortrollover|
|B2C|Anwendungsverwaltung|Anwendung|Add V2 application permissions (V2-Anwendungsberechtigungen hinzufügen)|
|B2C|Autorisierung|Autorisierung|Add V2 application permissions (V2-Anwendungsberechtigungen hinzufügen)|
|B2C|Schlüssel|Schlüssel|Add a key based on ASCII secret to a CPIM key container (Schlüssel basierend auf einem ASCII-Geheimnis zu einem CPIM-Schlüsselcontainer hinzufügen)|
|B2C|Autorisierung|Autorisierung|Add a key based on ASCII secret to a CPIM key container (Schlüssel basierend auf einem ASCII-Geheimnis zu einem CPIM-Schlüsselcontainer hinzufügen)|
|B2C|Schlüssel|Schlüssel|Add a key to a CPIM key container (Schlüssel zu einem CPIM-Schlüsselcontainer hinzufügen)|
|B2C|Autorisierung|Autorisierung|Add a key to a CPIM key container (Schlüssel zu einem CPIM-Schlüsselcontainer hinzufügen)|
|B2C|Ressource|Ressource|AdminPolicyDatas-RemoveResources|
|B2C|Autorisierung|Autorisierung|AdminPolicyDatas-SetResources|
|B2C|Ressource|Ressource|AdminPolicyDatas-SetResources|
|B2C|Ressource|Ressource|AdminUserJourneys-GetResources|
|B2C|Autorisierung|Autorisierung|AdminUserJourneys-GetResources|
|B2C|Autorisierung|Autorisierung|AdminUserJourneys-RemoveResources|
|B2C|Ressource|Ressource|AdminUserJourneys-RemoveResources|
|B2C|Ressource|Ressource|AdminUserJourneys-SetResources|
|B2C|Autorisierung|Autorisierung|AdminUserJourneys-SetResources|
|B2C|Autorisierung|Autorisierung|Create IdentityProvider (IdentityProvider-Element erstellen)|
|B2C|Ressource|Ressource|Create IdentityProvider (IdentityProvider-Element erstellen)|
|B2C|Autorisierung|Autorisierung|Create V1 application (V1-Anwendung erstellen)|
|B2C|Anwendungsverwaltung|Anwendung|Create V1 application (V1-Anwendung erstellen)|
|B2C|Anwendungsverwaltung|Anwendung|Create V2 application (V2-Anwendung erstellen)|
|B2C|Autorisierung|Autorisierung|Create V2 application (V2-Anwendung erstellen)|
|B2C|Verzeichnisverwaltung|Verzeichnis|Create a custom domains in the tenant (Benutzerdefinierte Domäne im Mandanten erstellen)|
|B2C|Autorisierung|Autorisierung|Create a custom domains in the tenant (Benutzerdefinierte Domäne im Mandanten erstellen)|
|B2C|Autorisierung|Autorisierung|Create a new AdminUserJourney (Neues AdminUserJourney-Element erstellen)|
|B2C|Ressource|Ressource|Create a new AdminUserJourney (Neues AdminUserJourney-Element erstellen)|
|B2C|Ressource|Ressource|Create localized resource json (Lokalisierten JSON-Ressourcencode erstellen)|
|B2C|Autorisierung|Autorisierung|Create localized resource json (Lokalisierten JSON-Ressourcencode erstellen)|
|B2C|Autorisierung|Autorisierung|Create new Custom IDP (Neuen benutzerdefinierten IDP erstellen)|
|B2C|Ressource|Ressource|Create new Custom IDP (Neuen benutzerdefinierten IDP erstellen)|
|B2C|Ressource|Ressource|Create new IDP (Neuen IDP erstellen)|
|B2C|Autorisierung|Autorisierung|Create new IDP (Neuen IDP erstellen)|
|B2C|Autorisierung|Autorisierung|Create or update a B2C directory resource (B2C-Verzeichnisressourcen erstellen oder aktualisieren)|
|B2C|Ressource|Ressource|Create or update a B2C directory resource (B2C-Verzeichnisressourcen erstellen oder aktualisieren)|
|B2C|Ressource|Ressource|Richtlinie erstellen|
|B2C|Autorisierung|Autorisierung|Richtlinie erstellen|
|B2C|Autorisierung|Autorisierung|Create trustFramework policy (Vertrauensframework-Richtlinie erstellen)|
|B2C|Ressource|Ressource|Create trustFramework policy (Vertrauensframework-Richtlinie erstellen)|
|B2C|Autorisierung|Autorisierung|Create trustFramework policy with configurable prefix (Vertrauensframework-Richtlinie mit konfigurierbarem Präfix erstellen)|
|B2C|Ressource|Ressource|Create trustFramework policy with configurable prefix (Vertrauensframework-Richtlinie mit konfigurierbarem Präfix erstellen)|
|B2C|Ressource|Ressource|Create user attribute (Benutzerattribut erstellen)|
|B2C|Autorisierung|Autorisierung|Create user attribute (Benutzerattribut erstellen)|
|B2C|Autorisierung|Autorisierung|CreateTrustFrameworkPolicy|
|B2C|Ressource|Ressource|CreateTrustFrameworkPolicy|
|B2C|Autorisierung|Autorisierung|Creates or Update an new AdminUserJourney (Neues AdminUserJourney-Element erstellen oder aktualisieren)|
|B2C|Ressource|Ressource|Identitätsanbieter löschen|
|B2C|Autorisierung|Autorisierung|Identitätsanbieter löschen|
|B2C|Ressource|Ressource|Delete IdentityProvider (IdentityProvider-Element löschen)|
|B2C|Autorisierung|Autorisierung|Delete IdentityProvider (IdentityProvider-Element löschen)|
|B2C|Anwendungsverwaltung|Anwendung|Delete V1 application (V1-Anwendung löschen)|
|B2C|Autorisierung|Autorisierung|Delete V1 application (V1-Anwendung löschen)|
|B2C|Anwendungsverwaltung|Anwendung|Delete V2 application (V2-Anwendung löschen)|
|B2C|Autorisierung|Autorisierung|Delete V2 application (V2-Anwendung löschen)|
|B2C|Autorisierung|Autorisierung|Delete V2 application permission grant (Berechtigungszuweisung für V2-Anwendung löschen)|
|B2C|Anwendungsverwaltung|Anwendung|Delete V2 application permission grant (Berechtigungszuweisung für V2-Anwendung löschen)|
|B2C|Ressource|Ressource|Delete a B2C directory resource (B2C-Verzeichnisressource löschen)|
|B2C|Autorisierung|Autorisierung|Delete a B2C directory resource (B2C-Verzeichnisressource löschen)|
|B2C|Schlüssel|Schlüssel|Delete a CPIM key container (CPIM-Schlüsselcontainer löschen)|
|B2C|Autorisierung|Autorisierung|Delete a CPIM key container (CPIM-Schlüsselcontainer löschen)|
|B2C|Schlüssel|Schlüssel|Delete key container (Schlüsselcontainer löschen)|
|B2C|Ressource|Ressource|Delete trustFramework policy (Vertrauensframework-Richtlinie löschen)|
|B2C|Autorisierung|Autorisierung|Delete trustFramework policy (Vertrauensframework-Richtlinie löschen)|
|B2C|Ressource|Ressource|Benutzerattribut löschen|
|B2C|Autorisierung|Autorisierung|Benutzerattribut löschen|
|B2C|Autorisierung|Autorisierung|Enable B2C feature (B2C-Feature aktivieren)|
|B2C|Verzeichnisverwaltung|Verzeichnis|Enable B2C feature (B2C-Feature aktivieren)|
|B2C|Ressource|Ressource|Get B2C directory resources in a resource group (B2C-Verzeichnisressourcen in einer Ressourcengruppe abrufen)|
|B2C|Ressource|Ressource|Get B2C directory resources in a subscription (B2C-Verzeichnisressourcen in einem Abonnement abrufen)|
|B2C|Autorisierung|Autorisierung|Get B2C directory resources in a subscription (B2C-Verzeichnisressourcen in einem Abonnement abrufen)|
|B2C|Autorisierung|Autorisierung|Get Custom IDP (Benutzerdefinierten IDP abrufen)|
|B2C|Ressource|Ressource|Get Custom IDP (Benutzerdefinierten IDP abrufen)|
|B2C|Ressource|Ressource|Get IDP (IDP abrufen)|
|B2C|Autorisierung|Autorisierung|Get IDP (IDP abrufen)|
|B2C|Autorisierung|Autorisierung|Get V1 and V2 applications (V1- und V2-Anwendungen abrufen)|
|B2C|Anwendungsverwaltung|Anwendung|Get V1 and V2 applications (V1- und V2-Anwendungen abrufen)|
|B2C|Autorisierung|Autorisierung|Get V1 application (V1-Anwendung abrufen)|
|B2C|Anwendungsverwaltung|Anwendung|Get V1 application (V1-Anwendung abrufen)|
|B2C|Autorisierung|Autorisierung|Get V1 applications (V1-Anwendungen abrufen)|
|B2C|Anwendungsverwaltung|Anwendung|Get V1 applications (V1-Anwendungen abrufen)|
|B2C|Anwendungsverwaltung|Anwendung|Get V2 application (V2-Anwendung abrufen)|
|B2C|Autorisierung|Autorisierung|Get V2 application (V2-Anwendung abrufen)|
|B2C|Anwendungsverwaltung|Anwendung|Get V2 applications (V2-Anwendungen abrufen)|
|B2C|Autorisierung|Autorisierung|Get V2 applications (V2-Anwendungen abrufen)|
|B2C|Ressource|Ressource|Get a B2C directory resource (B2C-Verzeichnisressource abrufen)|
|B2C|Autorisierung|Autorisierung|Get a B2C directory resource (B2C-Verzeichnisressource abrufen)|
|B2C|Autorisierung|Autorisierung|Get a list of custom domains in the tenant (Liste der benutzerdefinierten Domänen im Mandanten abrufen)|
|B2C|Verzeichnisverwaltung|Verzeichnis|Get a list of custom domains in the tenant (Liste der benutzerdefinierten Domänen im Mandanten abrufen)|
|B2C|Autorisierung|Autorisierung|Get a user journey (User Journey abrufen)|
|B2C|Ressource|Ressource|Get a user journey (User Journey abrufen)|
|B2C|Ressource|Ressource|Get allowed application claims for user journey (Zulässige Anwendungsansprüche für User Journey abrufen)|
|B2C|Autorisierung|Autorisierung|Get allowed application claims for user journey (Zulässige Anwendungsansprüche für User Journey abrufen)|
|B2C|Ressource|Ressource|Get allowed self-asserted claims for user journey (Zulässige selbstbestätigte Ansprüche für User Journey abrufen)|
|B2C|Autorisierung|Autorisierung|Get allowed self-asserted claims for user journey (Zulässige selbstbestätigte Ansprüche für User Journey abrufen)|
|B2C|Ressource|Ressource|Get allowed self-asserted claims of policy (Zulässige selbstbestätigte Ansprüche der Richtlinie abrufen)|
|B2C|Autorisierung|Autorisierung|Get allowed self-asserted claims of policy (Zulässige selbstbestätigte Ansprüche der Richtlinie abrufen)|
|B2C|Ressource|Ressource|Get available output claims list (Liste der verfügbaren Ausgabeansprüche abrufen)|
|B2C|Autorisierung|Autorisierung|Get available output claims list (Liste der verfügbaren Ausgabeansprüche abrufen)|
|B2C|Ressource|Ressource|Get content definitions for user journey (Inhaltsdefinitionen für User Journey abrufen)|
|B2C|Autorisierung|Autorisierung|Get content definitions for user journey (Inhaltsdefinitionen für User Journey abrufen)|
|B2C|Autorisierung|Autorisierung|Get idps for a specific admin flow (IDPs für einen bestimmten Verwaltungsflow abrufen)|
|B2C|Ressource|Ressource|Get idps for a specific admin flow (IDPs für einen bestimmten Verwaltungsflow abrufen)|
|B2C|Schlüssel|Schlüssel|Get key container active key metadata in JWK (Schlüsselcontainermetadaten zu aktiven Schlüsseln abrufen)|
|B2C|Autorisierung|Autorisierung|Get key container active key metadata in JWK (Schlüsselcontainermetadaten zu aktiven Schlüsseln abrufen)|
|B2C|Schlüssel|Schlüssel|Get key container metadata (Metadaten des Schlüsselcontainers abrufen)|
|B2C|Ressource|Ressource|Get list of all admin flows (Liste aller Verwaltungsflows abrufen)|
|B2C|Autorisierung|Autorisierung|Get list of all admin flows (Liste aller Verwaltungsflows abrufen)|
|B2C|Autorisierung|Autorisierung|Get list of tags for all admin flows for all users (Liste der Tags für alle Verwaltungsflows für alle Benutzer abrufen)|
|B2C|Ressource|Ressource|Get list of tags for all admin flows for all users (Liste der Tags für alle Verwaltungsflows für alle Benutzer abrufen)|
|B2C|Ressource|Ressource|Get list of tenants for a user (Liste der Mandanten für einen Benutzer abrufen)|
|B2C|Autorisierung|Autorisierung|Get list of tenants for a user (Liste der Mandanten für einen Benutzer abrufen)|
|B2C|Ressource|Ressource|Get local accounts' self-asserted claims (Selbstbestätigte Ansprüche des lokalen Kontos abrufen)|
|B2C|Autorisierung|Autorisierung|Get local accounts' self-asserted claims (Selbstbestätigte Ansprüche des lokalen Kontos abrufen)|
|B2C|Ressource|Ressource|Get localized resource json (Lokalisierten JSON-Ressourcencode abrufen)|
|B2C|Autorisierung|Autorisierung|Get localized resource json (Lokalisierten JSON-Ressourcencode abrufen)|
|B2C|Autorisierung|Autorisierung|Get operations of Microsoft.AzureActiveDirectory resource provider (Vorgänge des Microsoft.AzureActiveDirectory-Ressourcenanbieters abrufen)|
|B2C|Ressource|Ressource|Get operations of Microsoft.AzureActiveDirectory resource provider (Vorgänge des Microsoft.AzureActiveDirectory-Ressourcenanbieters abrufen)|
|B2C|Ressource|Ressource|Get policies (Richtlinien abrufen)|
|B2C|Autorisierung|Autorisierung|Get policies (Richtlinien abrufen)|
|B2C|Ressource|Ressource|Get policy (Richtlinie abrufen)|
|B2C|Autorisierung|Autorisierung|Get policy (Richtlinie abrufen)|
|B2C|Verzeichnisverwaltung|Verzeichnis|Get resource properties of a tenant (Ressourceneigenschaften eines Mandanten abrufen)|
|B2C|Autorisierung|Autorisierung|Get resource properties of a tenant (Ressourceneigenschaften eines Mandanten abrufen)|
|B2C|Ressource|Ressource|Get supported IDP list (Liste der unterstützten IDPs abrufen)|
|B2C|Autorisierung|Autorisierung|Get supported IDP list (Liste der unterstützten IDPs abrufen)|
|B2C|Ressource|Ressource|Get supported IDP list of the user journey (Liste der unterstützten IDPs der User Journey abrufen)|
|B2C|Autorisierung|Autorisierung|Get supported IDP list of the user journey (Liste der unterstützten IDPs der User Journey abrufen)|
|B2C|Verzeichnisverwaltung|Verzeichnis|Get tenant Info (Abrufen der Mandanteninformationen)|
|B2C|Autorisierung|Autorisierung|Get tenant Info (Abrufen der Mandanteninformationen)|
|B2C|Verzeichnisverwaltung|Verzeichnis|Get tenant allowed features (Vom Mandanten zugelassene Features abrufen)|
|B2C|Autorisierung|Autorisierung|Get tenant allowed features (Vom Mandanten zugelassene Features abrufen)|
|B2C|Autorisierung|Autorisierung|Get tenant defined Custom IDP list (Vom Mandanten festgelegte Liste der benutzerdefinierten IDPs abrufen)|
|B2C|Ressource|Ressource|Get tenant defined Custom IDP list (Vom Mandanten festgelegte Liste der benutzerdefinierten IDPs abrufen)|
|B2C|Ressource|Ressource|Get tenant defined IDP list (Vom Mandanten festgelegte IDP-Liste)|
|B2C|Autorisierung|Autorisierung|Get tenant defined IDP list (Vom Mandanten festgelegte IDP-Liste)|
|B2C|Ressource|Ressource|Get tenant defined local IDP list (Vom Mandanten festgelegte Liste der lokalen IDPs)|
|B2C|Autorisierung|Autorisierung|Get tenant defined local IDP list (Vom Mandanten festgelegte Liste der lokalen IDPs)|
|B2C|Ressource|Ressource|Get tenant details for a user for resource creation (Mandantendetails für einen Benutzer zur Ressourcenerstellung abrufen)|
|B2C|Autorisierung|Autorisierung|Get tenant details for a user for resource creation (Mandantendetails für einen Benutzer zur Ressourcenerstellung abrufen)|
|B2C|Autorisierung|Autorisierung|Get tenant list (Mandantenliste abrufen)|
|B2C|Autorisierung|Autorisierung|Get tenantDomains (tenantDomains-Element abrufen)|
|B2C|Verzeichnisverwaltung|Verzeichnis|Get tenantDomains (tenantDomains-Element abrufen)|
|B2C|Ressource|Ressource|Get the default supported culture for CPIM (Standardmäßige unterstützte Kultur für CPIM abrufen)|
|B2C|Autorisierung|Autorisierung|Get the default supported culture for CPIM (Standardmäßige unterstützte Kultur für CPIM abrufen)|
|B2C|Ressource|Ressource|Get the details of an admin flow (Details eines Verwaltungsflows abrufen)|
|B2C|Autorisierung|Autorisierung|Get the details of an admin flow (Details eines Verwaltungsflows abrufen)|
|B2C|Autorisierung|Autorisierung|Get the list of UserJourneys for this tenant (UserJourneys-Liste für diesen Mandanten abrufen)|
|B2C|Ressource|Ressource|Get the list of UserJourneys for this tenant (UserJourneys-Liste für diesen Mandanten abrufen)|
|B2C|Autorisierung|Autorisierung|Get the set of available supported cultures for CPIM (Satz der verfügbaren unterstützten Kulturen für CPIM abrufen)|
|B2C|Ressource|Ressource|Get the set of available supported cultures for CPIM (Satz der verfügbaren unterstützten Kulturen für CPIM abrufen)|
|B2C|Autorisierung|Autorisierung|Get trustFramework policy (Vertrauensframework-Richtlinie abrufen)|
|B2C|Ressource|Ressource|Get trustFramework policy (Vertrauensframework-Richtlinie abrufen)|
|B2C|Autorisierung|Autorisierung|Get trustFramework policy as xml (Vertrauensframework-Richtlinie als XML abrufen)|
|B2C|Ressource|Ressource|Get trustFramework policy as xml (Vertrauensframework-Richtlinie als XML abrufen)|
|B2C|Ressource|Ressource|Get user attribute (Benutzerattribut abrufen)|
|B2C|Autorisierung|Autorisierung|Get user attribute (Benutzerattribut abrufen)|
|B2C|Autorisierung|Autorisierung|Get user attributes (Benutzerattribute abrufen)|
|B2C|Ressource|Ressource|Get user attributes (Benutzerattribute abrufen)|
|B2C|Autorisierung|Autorisierung|Get user journey list (User Journey-Liste abrufen)|
|B2C|Ressource|Ressource|Get user journey list (User Journey-Liste abrufen)|
|B2C|Autorisierung|Autorisierung|GetIEFPolicies|
|B2C|Ressource|Ressource|GetIEFPolicies|
|B2C|Autorisierung|Autorisierung|GetIdentityProviders|
|B2C|Ressource|Ressource|GetIdentityProviders|
|B2C|Ressource|Ressource|GetTrustFrameworkPolicy|
|B2C|Autorisierung|Autorisierung|GetTrustFrameworkPolicy|
|B2C|Schlüssel|Schlüssel|Gets a CPIM key container in jwk format (CPIM-Schlüsselcontainer im JWK-Format abrufen)|
|B2C|Autorisierung|Autorisierung|Gets a CPIM key container in jwk format (CPIM-Schlüsselcontainer im JWK-Format abrufen)|
|B2C|Schlüssel|Schlüssel|Gets list of key containers in the tenant (Liste der Schlüsselcontainer im Mandanten abrufen)|
|B2C|Autorisierung|Autorisierung|Gets list of key containers in the tenant (Liste der Schlüsselcontainer im Mandanten abrufen)|
|B2C|Autorisierung|Autorisierung|Gets the type of tenant (Mandantentyp abrufen)|
|B2C|Verzeichnisverwaltung|Verzeichnis|Gets the type of tenant (Mandantentyp abrufen)|
|B2C|Authentifizierung|Authentifizierung|Issue an access token to the application (Zugriffstoken für die Anwendung ausstellen)|
|B2C|Authentifizierung|Authentifizierung|Issue an authorization code to the application (Autorisierungscode für die Anwendung ausstellen)|
|B2C|Andere|Andere|Issue an authorization code to the application (Autorisierungscode für die Anwendung ausstellen)|
|B2C|Authentifizierung|Authentifizierung|Issue an id_token to the application (ID-Token für die Anwendung ausstellen)|
|B2C|Andere|Andere|Issue an id_token to the application (ID-Token für die Anwendung ausstellen)|
|B2C|Autorisierung|Autorisierung|MigrateTenantMetadata|
|B2C|Ressource|Ressource|MigrateTenantMetadata|
|B2C|Ressource|Ressource|Verschieben von Ressourcen|
|B2C|Autorisierung|Autorisierung|Patch IdentityProvider („IdentityProvider“ patchen)|
|B2C|Ressource|Ressource|Patch IdentityProvider („IdentityProvider“ patchen)|
|B2C|Ressource|Ressource|PutTrustFrameworkPolicy|
|B2C|Autorisierung|Autorisierung|PutTrustFrameworkPolicy|
|B2C|Autorisierung|Autorisierung|PutTrustFrameworkpolicy|
|B2C|Ressource|Ressource|PutTrustFrameworkpolicy|
|B2C|Ressource|Ressource|User Journey entfernen|
|B2C|Autorisierung|Autorisierung|User Journey entfernen|
|B2C|Autorisierung|Autorisierung|Restore a CPIM key container backup (Sicherung eines CPIM-Schlüsselcontainers wiederherstellen)|
|B2C|Schlüssel|Schlüssel|Restore a CPIM key container backup (Sicherung eines CPIM-Schlüsselcontainers wiederherstellen)|
|B2C|Anwendungsverwaltung|Anwendung|Retrieve V2 application permissions grants (Berechtigungszuweisungen für V2-Anwendung abrufen)|
|B2C|Autorisierung|Autorisierung|Retrieve V2 application permissions grants (Berechtigungszuweisungen für V2-Anwendung abrufen)|
|B2C|Anwendungsverwaltung|Anwendung|Retrieve V2 application service principals in the current tenant (Dienstprinzipale der V2-Anwendung im aktuellen Mandanten abrufen)|
|B2C|Autorisierung|Autorisierung|Retrieve V2 application service principals in the current tenant (Dienstprinzipale der V2-Anwendung im aktuellen Mandanten abrufen)|
|B2C|Schlüssel|Schlüssel|Save key container (Schlüsselcontainer speichern)|
|B2C|Autorisierung|Autorisierung|Update Custom IDP (Benutzerdefinierten IDP aktualisieren)|
|B2C|Ressource|Ressource|Update Custom IDP (Benutzerdefinierten IDP aktualisieren)|
|B2C|Ressource|Ressource|Update IDP (IDP aktualisieren)|
|B2C|Autorisierung|Autorisierung|Update IDP (IDP aktualisieren)|
|B2C|Ressource|Ressource|Lokalen IDP aktualisieren|
|B2C|Autorisierung|Autorisierung|Lokalen IDP aktualisieren|
|B2C|Anwendungsverwaltung|Anwendung|Update V1 application (V1-Anwendung aktualisieren)|
|B2C|Autorisierung|Autorisierung|Update V1 application (V1-Anwendung aktualisieren)|
|B2C|Anwendungsverwaltung|Anwendung|Update V2 application (V2-Anwendung aktualisieren)|
|B2C|Autorisierung|Autorisierung|Update V2 application (V2-Anwendung aktualisieren)|
|B2C|Anwendungsverwaltung|Anwendung|Update V2 application permission grant (Berechtigungszuweisung für V2-Anwendung aktualisieren)|
|B2C|Autorisierung|Autorisierung|Update V2 application permission grant (Berechtigungszuweisung für V2-Anwendung aktualisieren)|
|B2C|Ressource|Ressource|Update a B2C directory resource (B2C-Verzeichnisressource aktualisieren)|
|B2C|Ressource|Ressource|Aktualisieren von Richtlinien|
|B2C|Autorisierung|Autorisierung|Aktualisieren von Richtlinien|
|B2C|Ressource|Ressource|Update subscription status (Abonnementstatus aktualisieren)|
|B2C|Ressource|Ressource|Update user attribute (Benutzerattribut aktualisieren)|
|B2C|Autorisierung|Autorisierung|Update user attribute (Benutzerattribut aktualisieren)|
|B2C|Schlüssel|Schlüssel|Upload a CPIM encrypted key (Verschlüsselten CPIM-Schlüssel hochladen)|
|B2C|Autorisierung|Autorisierung|Upload a CPIM encrypted key (Verschlüsselten CPIM-Schlüssel hochladen)|
|B2C|Autorisierung|Autorisierung|User Authorization: API is disabled for tenant featureset (Benutzerautorisierung: Die API ist für die Mandantenfeatures deaktiviert.)|
|B2C|Autorisierung|Autorisierung|User Authorization: User granted access as 'Tenant Admin' (Benutzerautorisierung: Der Benutzer hat Zugriff als „Mandantenadministrator“ gewährt.)|
|B2C|Autorisierung|Autorisierung|User Authorization: User was granted 'Authenticated Users' access rights (Benutzerautorisierung: Dem Benutzer wurden Zugriffsrechte vom Typ „Authentifizierte Benutzer“ gewährt.)|
|B2C|Authentifizierung|Authentifizierung|Validate local account credentials (Anmeldeinformationen des lokalen Kontos überprüfen)|
|B2C|Ressource|Ressource|Validate move resources (Verschiebung von Ressourcen überprüfen)|
|B2C|Authentifizierung|Authentifizierung|Validate user authentication (Benutzerauthentifizierung überprüfen)|
|B2C|Verzeichnisverwaltung|Verzeichnis|Verify if B2C feature is enabled (Überprüfen, ob das B2C-Feature aktiviert ist)|
|B2C|Autorisierung|Autorisierung|Verify if B2C feature is enabled (Überprüfen, ob das B2C-Feature aktiviert ist)|
|B2C|Autorisierung|Autorisierung|Verify if feature is enabled (Überprüfen, ob das Feature aktiviert ist)|
|B2C|Verzeichnisverwaltung|Verzeichnis|Verify if feature is enabled (Überprüfen, ob das Feature aktiviert ist)|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Add OAuth2PermissionGrant (OAuth2PermissionGrant hinzufügen)|
|Kernverzeichnis|Verwaltung administrativer Einheiten|AdministrativeUnit|Verwaltungseinheit hinzufügen|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Add app role assignment grant to user (App-Rollenzuweisungsgewährung zu Benutzer hinzufügen)|
|Kernverzeichnis|Gruppenverwaltung|Group|Add app role assignment to group (App-Rollenzuweisung zu Gruppe hinzufügen)|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Add app role assignment to service principal (App-Rollenzuweisung zu Dienstprinzipal hinzufügen)|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Anwendung hinzufügen|
|Kernverzeichnis|Ressource|Ressource|Gerät hinzufügen|
|Kernverzeichnis|Ressource|Ressource|Add device configuration (Gerätekonfiguration hinzufügen)|
|Kernverzeichnis|Rollenverwaltung|Rolle|Add eligible member to role (Berechtigtes Mitglied zur Rolle hinzufügen)|
|Kernverzeichnis|Gruppenverwaltung|Group|Gruppe hinzufügen|
|Kernverzeichnis|Verwaltung administrativer Einheiten|AdministrativeUnit|Mitglied der Verwaltungseinheit hinzufügen|
|Kernverzeichnis|Gruppenverwaltung|Group|Mitglied zur Gruppe hinzufügen|
|Kernverzeichnis|Rollenverwaltung|Rolle|Add member to role (Mitglied zu Rolle hinzufügen)|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Besitzer der Anwendung hinzufügen|
|Kernverzeichnis|Gruppenverwaltung|Group|Add owner to group (Besitzer zu Gruppe hinzufügen)|
|Kernverzeichnis|Richtlinienverwaltung|Richtlinie|Add owner to policy (Besitzer zu Richtlinie hinzufügen)|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Add owner to service principal (Besitzer zu Dienstprinzipal hinzufügen)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Partner zu Unternehmen hinzufügen|
|Kernverzeichnis|Richtlinienverwaltung|Richtlinie|Richtlinie hinzufügen|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Richtlinie dem Dienstprinzipal hinzufügen|
|Kernverzeichnis|Ressource|Ressource|Add registered owner to device (Registrierten Besitzer zu Gerät hinzufügen)|
|Kernverzeichnis|Ressource|Ressource|Add registered users to device (Registrierte Benutzer zu Gerät hinzufügen)|
|Kernverzeichnis|Rollenverwaltung|Rolle|Add role assignment to role definition (Rollenzuweisung zu Rollendefinition hinzufügen)|
|Kernverzeichnis|Rollenverwaltung|Rolle|Add role from template (Rolle aus Vorlage hinzufügen)|
|Kernverzeichnis|Rollenverwaltung|Rolle|Add scoped member to role (Zugeordnetes Mitglied zu Rolle hinzufügen)|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Dienstprinzipal hinzufügen|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Anmeldeinformationen für Dienstprinzipal hinzufügen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Add inverified domain (Nicht überprüfte Domäne hinzufügen)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Benutzer hinzufügen|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Add users strong authentication phone app detail (Phone-App-Details für die strenge Authentifizierung der Benutzer hinzufügen)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Add verified domain (Überprüfte Domäne hinzufügen)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Benutzerlizenz ändern|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Benutzerkennwort ändern|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Consent to application (Anwendung zustimmen)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Convert federated user to managed (Verbundbenutzer in verwalteten Benutzer konvertieren)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Create application password for user (Anwendungskennwort für Benutzer erstellen)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Unternehmen erstellen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Unternehmenseinstellungen erstellen|
|Kernverzeichnis|Gruppenverwaltung|Group|Create group settings (Gruppeneinstellungen erstellen)|
|Kernverzeichnis|Verwaltung administrativer Einheiten|AdministrativeUnit|Verwaltungseinheit löschen|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Löschen der Anwendung|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Delete application password for user (Anwendungskennwort für Benutzer löschen)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Unternehmenseinstellungen löschen|
|Kernverzeichnis|Ressource|Ressource|Löschen des Geräts|
|Kernverzeichnis|Ressource|Ressource|Löschen der Gerätekonfiguration|
|Kernverzeichnis|Gruppenverwaltung|Group|Gruppe löschen|
|Kernverzeichnis|Gruppenverwaltung|Group|Delete group settings (Gruppeneinstellungen löschen)|
|Kernverzeichnis|Richtlinienverwaltung|Richtlinie|Löschen von Richtlinien|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Benutzer löschen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Partner tiefer stufen|
|Kernverzeichnis|Ressource|Ressource|Device no longer compliant (Gerät nicht mehr konform)|
|Kernverzeichnis|Ressource|Ressource|Device no longer managed (Gerät nicht mehr verwaltet)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Directory deleted (Verzeichnis gelöscht)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Directory deleted permanently (Verzeichnis unwiderruflich gelöscht)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Directory scheduled for deletion (Löschen des Verzeichnisses geplant)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Disable account (Konto deaktivieren)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Enable Strong Authentication (Strenge Authentifizierung aktivieren)|
|Kernverzeichnis|Gruppenverwaltung|Group|Finish applying group based license to users (Gruppenbasierte Lizenzzuweisung zu Benutzern fertig stellen)|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Hard Delete application (Anwendung endgültig löschen)|
|Kernverzeichnis|Gruppenverwaltung|Group|Hard Delete group (Gruppe endgültig löschen)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Hard Delete user (Benutzer endgültig löschen)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Promote company to partner (Unternehmen zu Partner heraufstufen)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Rights Management-Eigenschaften bereinigen|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Remove OAuth2PermissionGrant (OAuth2PermissionGrant entfernen)|
|Kernverzeichnis|Gruppenverwaltung|Group|Remove app role assignment from group (App-Rollenzuweisung von Gruppe entfernen)|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Remove app role assignment from service principal (App-Rollenzuweisung von Dienstprinzipal entfernen)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Remove app role assignment from user (App-Rollenzuweisung von Benutzer entfernen)|
|Kernverzeichnis|Rollenverwaltung|Rolle|Remove eligible member from role (Berechtigtes Mitglied aus Rolle entfernen)|
|Kernverzeichnis|Verwaltung administrativer Einheiten|AdministrativeUnit|Mitglied aus Verwaltungseinheit entfernen|
|Kernverzeichnis|Gruppenverwaltung|Group|Mitglied aus Gruppe entfernen|
|Kernverzeichnis|Rollenverwaltung|Rolle|Remove member from role (Mitglied aus Rolle entfernen)|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Besitzer aus Anwendung entfernen|
|Kernverzeichnis|Gruppenverwaltung|Group|Remove owner from group (Besitzer aus Gruppe entfernen)|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Remove owner from service principal (Besitzer aus Dienstprinzipal entfernen)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Partner aus Unternehmen entfernen|
|Kernverzeichnis|Richtlinienverwaltung|Richtlinie|Richtlinien-Anmeldeinformationen entfernen|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Richtlinie aus Dienstprinzipal entfernen|
|Kernverzeichnis|Ressource|Ressource|Registrierten Besitzer aus Gerät entfernen|
|Kernverzeichnis|Ressource|Ressource|Registrierte Benutzer aus Gerät entfernen|
|Kernverzeichnis|Rollenverwaltung|Rolle|Remove role assignment from role definition (Rollenzuweisung aus Rollendefinition entfernen)|
|Kernverzeichnis|Rollenverwaltung|Rolle|Remove scoped member from role (Zugeordnetes Mitglied aus Rolle entfernen)|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Dienstprinzipal entfernen|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Anmeldeinformationen für Dienstprinzipal entfernen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Remove unverified domain (Nicht überprüfte Domäne entfernen)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Remove users strong authentication phone app detail (Phone-App-Details für die strenge Authentifizierung der Benutzer entfernen)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Remove verified domain (Überprüfte Domäne entfernen)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Benutzerkennwort zurücksetzen|
|Kernverzeichnis|Gruppenverwaltung|Group|Gruppe wiederherstellen|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Anwendung wiederherstellen|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Benutzer wiederherstellen|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Zustimmung widerrufen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Unternehmensinformationen festlegen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|DirSync-Funktion festlegen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|DirSyncEnabled-Flag festlegen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Set Partnership (Partnerschaft festlegen)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Schwellenwert für versehentliches Löschen festlegen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Zulässigen Datenspeicherort für Unternehmen festlegen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Funktion für multinationales Unternehmen auf „Aktiviert“ festlegen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Verzeichnisfunktion für Mandanten festlegen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Domänenauthentifizierung festlegen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Verbundeinstellungen für Domäne festlegen|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Änderung des Benutzerkennworts erzwingen|
|Kernverzeichnis|Gruppenverwaltung|Group|Set group license (Gruppenlizenz festlegen)|
|Kernverzeichnis|Gruppenverwaltung|Group|Gruppe für Verwaltung durch Benutzer festgelegt|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Kennwortrichtlinie festlegen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Eigenschaften für Rights Management festlegen|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Set user manager (Benutzer-Manager festlegen)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Set users oath token metadata enabled (OATH-Tokenmetadaten der Benutzer auf „Aktiviert“ festlegen)|
|Kernverzeichnis|Gruppenverwaltung|Group|Start applying group based license to users (Starten der gruppenbasierten Lizenzzuweisung zu Benutzern)|
|Kernverzeichnis|Gruppenverwaltung|Group|Trigger group license recalculation (Neuberechnung der Gruppenlizenzen auslösen)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Update StsRefreshTokenValidFrom Timestamp (Zeitstempel „StsRefreshTokenValidFrom“ aktualisieren)|
|Kernverzeichnis|Verwaltung administrativer Einheiten|AdministrativeUnit|Verwaltungseinheit aktualisieren|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Aktualisieren der Anwendung|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Update company (Unternehmen aktualisieren)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Unternehmenseinstellungen aktualisieren|
|Kernverzeichnis|Ressource|Ressource|Gerät aktualisieren|
|Kernverzeichnis|Ressource|Ressource|Update device configuration (Gerätekonfiguration aktualisieren)|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Domäne aktualisieren|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Externe geheime Schlüssel aktualisieren|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Externe geheime Schlüssel aktualisieren|
|Kernverzeichnis|Gruppenverwaltung|Group|Gruppe aktualisieren|
|Kernverzeichnis|Gruppenverwaltung|Group|Update group settings (Gruppeneinstellungen aktualisieren)|
|Kernverzeichnis|Richtlinienverwaltung|Richtlinie|Aktualisieren von Richtlinien|
|Kernverzeichnis|Rollenverwaltung|Rolle|Rolle aktualisieren|
|Kernverzeichnis|Anwendungsverwaltung|Anwendung|Update service principal (Dienstprinzipal aktualisieren)|
|Kernverzeichnis|Benutzerverwaltung|Benutzer|Benutzer aktualisieren|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Domäne überprüfen|
|Kernverzeichnis|Verzeichnisverwaltung|Verzeichnis|Per E-Mail verifizierte Domäne überprüfen|
|Schutz der Identität (Identity Protection)|Benutzerverwaltung|Benutzer|Admin generates a temporary password (Administrator generiert ein temporäres Kennwort.)|
|Schutz der Identität (Identity Protection)|Benutzerverwaltung|Benutzer|Admins requires the user to reset their password (Administratoren fordern Benutzer zum Zurücksetzen ihres Kennworts auf.)|
|Schutz der Identität (Identity Protection)|Andere|Andere|Download a single risk event type (Einzelnen Risikoereignistyp herunterladen)|
|Schutz der Identität (Identity Protection)|Andere|Andere|Download admins and status of weekly digest opt-in (Administratoren und Status der Aktivierung der wöchentlichen Übersicht herunterladen)|
|Schutz der Identität (Identity Protection)|Andere|Andere|Download all risk event types (Alle Risikoereignistypen herunterladen)|
|Schutz der Identität (Identity Protection)|Andere|Andere|Download free user risk events (Kostenlose Benutzerrisikoereignisse herunterladen)|
|Schutz der Identität (Identity Protection)|Andere|Andere|Download users flagged for risk (Benutzer herunterladen, für die ein Risiko angezeigt wird)|
|Schutz der Identität (Identity Protection)|Verzeichnisverwaltung|Verzeichnis|Onboarding|
|Schutz der Identität (Identity Protection)|Richtlinienverwaltung|Richtlinie|Set MFA registration policy (MFA-Registrierungsrichtlinie festlegen)|
|Schutz der Identität (Identity Protection)|Richtlinienverwaltung|Richtlinie|Set sign-in risk policy (Richtlinie zum Anmelderisiko festlegen)|
|Schutz der Identität (Identity Protection)|Richtlinienverwaltung|Richtlinie|Set user risk policy (Richtlinie zum Benutzerrisiko festlegen)|
|Schutz der Identität (Identity Protection)|Verzeichnisverwaltung|Verzeichnis|Warnungseinstellungen aktualisieren|
|Schutz der Identität (Identity Protection)|Verzeichnisverwaltung|Verzeichnis|Update weekly digest settings (Einstellungen für wöchentliche Übersicht aktualisieren)|
|Invited Users (Eingeladene Benutzer)|Benutzerverwaltung|Benutzer|Externen Benutzer einer Anwendung zuweisen|
|Invited Users (Eingeladene Benutzer)|Andere|Andere|Verarbeitete Batch-Einladungen|
|Invited Users (Eingeladene Benutzer)|Andere|Andere|Batch-Einladungen hochgeladen|
|Invited Users (Eingeladene Benutzer)|Benutzerverwaltung|Benutzer|Email not sent, user unsubscribed (E-Mail nicht gesendet, Abonnement des Benutzers gekündigt)|
|Invited Users (Eingeladene Benutzer)|Benutzerverwaltung|Benutzer|Externen Benutzer einladen|
|Invited Users (Eingeladene Benutzer)|Benutzerverwaltung|Benutzer|Einlösen einer externen Benutzereinladung|
|Invited Users (Eingeladene Benutzer)|Benutzerverwaltung|Benutzer|Erstellen eines viralen Mandanten|
|Invited Users (Eingeladene Benutzer)|Benutzerverwaltung|Benutzer|Erstellen eines viralen Benutzers|
|Microsoft Identity Manager (MIM)|Gruppenverwaltung|Group|Mitglied hinzufügen|
|Microsoft Identity Manager (MIM)|Gruppenverwaltung|Group|Erstellen einer Gruppe|
|Microsoft Identity Manager (MIM)|Gruppenverwaltung|Group|Gruppe löschen|
|Microsoft Identity Manager (MIM)|Gruppenverwaltung|Group|Mitglied entfernen|
|Microsoft Identity Manager (MIM)|Gruppenverwaltung|Group|Gruppe aktualisieren|
|Microsoft Identity Manager (MIM)|Benutzerverwaltung|Benutzer|User Password Registration (Benutzerkennwortregistrierung)|
|Microsoft Identity Manager (MIM)|Benutzerverwaltung|Benutzer|User Password Reset (Benutzerkennwortzurücksetzung)|
|Privileged Identity Management|Rollenverwaltung|Rolle|AccessReview_Review|
|Privileged Identity Management|Rollenverwaltung|Rolle|AccessReview_Update|
|Privileged Identity Management|Rollenverwaltung|Rolle|ActivationAborted|
|Privileged Identity Management|Rollenverwaltung|Rolle|ActivationApproved|
|Privileged Identity Management|Rollenverwaltung|Rolle|ActivationCanceled|
|Privileged Identity Management|Rollenverwaltung|Rolle|ActivationRequested|
|Privileged Identity Management|Rollenverwaltung|Rolle|Hinzugefügt|
|Privileged Identity Management|Rollenverwaltung|Rolle|Zuweisen|
|Privileged Identity Management|Rollenverwaltung|Rolle|Erhöhen|
|Privileged Identity Management|Rollenverwaltung|Rolle|Entfernt|
|Privileged Identity Management|Rollenverwaltung|Rolle|Änderungen für Rolleneinstellung|
|Privileged Identity Management|Rollenverwaltung|Rolle|ScanAlertsNow|
|Privileged Identity Management|Rollenverwaltung|Rolle|Signup|
|Privileged Identity Management|Rollenverwaltung|Rolle|Deautorisieren|
|Privileged Identity Management|Rollenverwaltung|Rolle|UpdateAlertSettings|
|Privileged Identity Management|Rollenverwaltung|Rolle|UpdateCurrentState|
|Self-Service-Gruppenverwaltung|Gruppenverwaltung|Group|Approve a pending request to join a group (Ausstehende Gruppenbeitrittsanforderung genehmigen)|
|Self-Service-Gruppenverwaltung|Gruppenverwaltung|Group|Cancel a pending request to join a group (Ausstehende Gruppenbeitrittsanforderung abbrechen)|
|Self-Service-Gruppenverwaltung|Gruppenverwaltung|Group|Create lifecycle management policy (Richtlinie für Lebenszyklusverwaltung erstellen)|
|Self-Service-Gruppenverwaltung|Gruppenverwaltung|Group|Delete a pending request to join a group (Ausstehende Gruppenbeitrittsanforderung löschen)|
|Self-Service-Gruppenverwaltung|Gruppenverwaltung|Group|Reject a pending request to join a group (Ausstehende Gruppenbeitrittsanforderung ablehnen)|
|Self-Service-Gruppenverwaltung|Gruppenverwaltung|Group|Gruppe verlängern|
|Self-Service-Gruppenverwaltung|Gruppenverwaltung|Group|Request to join a group (Gruppenbeitritt anfordern)|
|Self-Service-Gruppenverwaltung|Gruppenverwaltung|Group|Set dynamic group properties (Dynamische Gruppeneigenschaften festlegen)|
|Self-Service-Gruppenverwaltung|Gruppenverwaltung|Group|Update lifecycle management policy (Richtlinie für Lebenszyklusverwaltung aktualisieren)|
|Self-Service-Kennwortverwaltung|Benutzerverwaltung|Benutzer|Blocked from self-service password reset (Von Self-Service-Kennwortzurücksetzung ausgeschlossen)|
|Self-Service-Kennwortverwaltung|Benutzerverwaltung|Benutzer|Change password (self-service) (Kennwort ändern (Self-Service))|
|Self-Service-Kennwortverwaltung|Verzeichnisverwaltung|Verzeichnis|Disable password writeback for directory (Kennwortrückschreiben für Verzeichnis deaktivieren)|
|Self-Service-Kennwortverwaltung|Verzeichnisverwaltung|Verzeichnis|Enable password writeback for directory (Kennwortrückschreiben für Verzeichnis deaktivieren)|
|Self-Service-Kennwortverwaltung|Benutzerverwaltung|Benutzer|Reset password (by admin) (Kennwort zurücksetzen (durch Administrator))|
|Self-Service-Kennwortverwaltung|Benutzerverwaltung|Benutzer|Reset password (self-service) (Kennwort zurücksetzen (Self-Service))|
|Self-Service-Kennwortverwaltung|Benutzerverwaltung|Benutzer|Self serve password reset flow activity progress (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs)|
|Self-Service-Kennwortverwaltung|Benutzerverwaltung|Benutzer|Self-service password reset flow activity progress (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs)|
|Self-Service-Kennwortverwaltung|Benutzerverwaltung|Benutzer|Unlock user account (self-service) (Benutzerkonto entsperren (Self-Service))|
|Self-Service-Kennwortverwaltung|Benutzerverwaltung|Benutzer|User registered for self-service password reset (Für Self-Service-Kennwortzurücksetzung registrierter Benutzer)|
|Nutzungsbedingungen|Richtlinienverwaltung|Richtlinie|Accept Terms Of Use (Nutzungsbedingungen akzeptieren)|
|Nutzungsbedingungen|Richtlinienverwaltung|Richtlinie|Create Terms Of Use (Nutzungsbedingungen erstellen)|
|Nutzungsbedingungen|Richtlinienverwaltung|Richtlinie|Decline Terms Of Use (Nutzungsbedingungen ablehnen)|
|Nutzungsbedingungen|Richtlinienverwaltung|Richtlinie|Delete Terms Of Use (Nutzungsbedingungen löschen)|
|Nutzungsbedingungen|Richtlinienverwaltung|Richtlinie|Edit Terms Of Use (Nutzungsbedingungen bearbeiten)|
|Nutzungsbedingungen|Richtlinienverwaltung|Richtlinie|Publish Terms Of Use (Nutzungsbedingungen veröffentlichen)|
|Nutzungsbedingungen|Richtlinienverwaltung|Richtlinie|Unpublish Terms Of Use (Veröffentlichung der Nutzungsbedingungen aufheben)|




## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über die Berichterstellung finden Sie unter [Azure Active Directory-Berichterstellung](active-directory-reporting-azure-portal.md).

