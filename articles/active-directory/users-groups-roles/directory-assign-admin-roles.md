---
title: Zuweisen von Administratorrollen in Azure Active Directory | Microsoft Docs
description: Eine Administratorrolle kann zum Hinzufügen von Benutzern, zum Zuweisen von Administratorrollen, zum Zurücksetzen von Benutzerkennwörtern sowie zum Verwalten von Benutzerlizenzen oder Domänen verwendet werden. Ein Benutzer, dem eine Administratorrolle zugewiesen wurde, verfügt für alle Dienste, die Ihre Organisation abonniert hat, über die gleichen Berechtigungen.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/31/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 384afb36f2a63fcbf290fa96ed15db2a1f469f55
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337850"
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Zuweisen von Administratorrollen in Azure Active Directory

Mithilfe von Azure Active Directory (Azure AD) können Sie verschiedene Administratoren bestimmen, um unterschiedliche Funktionen zu erfüllen. Administratoren können im Azure AD-Portal für Aufgaben wie das Hinzufügen oder Ändern von Benutzern, das Zuweisen von Administratorrollen, das Zurücksetzen von Benutzerkennwörtern, das Verwalten von Benutzerlizenzen oder das Verwalten von Domänennamen festgelegt werden.

## <a name="details-about-the-global-administrator-role"></a>Details zur globalen Administratorrolle

Der globale Administrator hat Zugriff auf alle administrativen Funktionen. Standardmäßig wird der Person, die sich für ein Azure-Abonnement registriert, die globale Administratorrolle für das Verzeichnis zugewiesen. Nur globale Administratoren können weitere Administratorrollen zuweisen.

## <a name="assign-or-remove-administrator-roles"></a>Zuweisen oder Entfernen von Administratorrollen

Um zu erfahren, wie in Azure Active Directory einem Benutzer Administratorrollen zugewiesen werden, lesen Sie [Zuweisen von Administratorrollen zu einem Benutzer in Azure Active Directory](../fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="available-roles"></a>Verfügbare Rollen

Die folgenden Administratorrollen sind verfügbar:

* **[Anwendungsadministrator:](#application-administrator)** Benutzer mit dieser Rolle können alle Aspekte von Unternehmensanwendungen, Anwendungsregistrierungen und Anwendungsproxyeinstellungen erstellen und verwalten. Diese Rolle ermöglicht auch die Zustimmung zu delegierten Berechtigungen und Anwendungsberechtigungen mit Ausnahme von Microsoft Graph und Azure AD Graph. Mitglieder dieser Rolle werden bei der Erstellung neuer Anwendungsregistrierungen oder Unternehmensanwendungen nicht als Besitzer hinzugefügt.

* **[Anwendungsentwickler:](#application-developer)** Benutzer mit dieser Rolle können Anwendungsregistrierungen erstellen, wenn die Einstellung „Benutzer können Anwendungen registrieren“ auf „Nein“ festgelegt ist. Diese Rolle erlaubt es den Mitgliedern auch, in ihrem eigenen Namen zuzustimmen, wenn die Einstellung „Benutzer können Apps zustimmen, die in ihrem Namen auf Unternehmensdaten zugreifen“ auf „Nein“ festgelegt ist. Mitglieder dieser Rolle werden bei der Erstellung neuer Anwendungsregistrierungen oder Unternehmensanwendungen als Besitzer hinzugefügt.

* **[Rechnungsadministrator:](#billing-administrator)** Erledigt Käufe, verwaltet Abonnements, verwaltet Supporttickets und überwacht die Dienstintegrität.

* **[Cloudanwendungsadministrator:](#cloud-application-administrator)** Benutzer in dieser Rolle haben die gleichen Berechtigungen wie die Rolle des Anwendungsadministrators, mit Ausnahme der Möglichkeit, den Anwendungsproxy zu verwalten. Diese Rolle ermöglicht die Erstellung und Verwaltung aller Aspekte von Unternehmensanwendungen und Anwendungsregistrierungen. Diese Rolle ermöglicht auch die Zustimmung zu delegierten Berechtigungen und Anwendungsberechtigungen mit Ausnahme von Microsoft Graph und Azure AD Graph. Mitglieder dieser Rolle werden bei der Erstellung neuer Anwendungsregistrierungen oder Unternehmensanwendungen nicht als Besitzer hinzugefügt.

* **[Complianceadministrator:](#compliance-administrator)** Benutzer mit dieser Rolle verfügen über Verwaltungsberechtigungen in Office 365 Security & Compliance Center und Exchange Admin Center. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrator für den bedingten Zugriff:](#conditional-access-administrator)** Benutzer mit dieser Rolle haben die Möglichkeit, Azure Active Directory-Einstellungen für den bedingten Zugriff zu verwalten.
  > [!NOTE]
  > Um die Exchange ActiveSync-Richtlinie für bedingten Zugriff in Azure bereitzustellen, muss der Benutzer auch ein globaler Administrator sein.
  
* **[Geräteadministratoren](#device-administrators)**: Diese Rolle kann nur als zusätzlicher lokaler Administrator in den [Geräteeinstellungen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/) zugewiesen werden. Benutzer mit dieser Rolle werden auf allen Windows 10-Geräten, die in Azure Active Directory eingebunden sind, als Administratoren für den lokalen Computer festgelegt. Sie haben nicht die Möglichkeit zum Verwalten von Geräteobjekten in Azure Active Directory. 

* **[Verzeichnis lesen:](#directory-readers)** Dies ist eine Legacyrolle, die Anwendungen ohne Unterstützung für das [Consent Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md) zugewiesen wird. Diese Rolle sollte keinem Benutzer zugewiesen werden.

* **[Konten für die Verzeichnissynchronisierung:](#directory-synchronization-accounts)** Verwenden Sie diese Rolle nicht. Diese Rolle wird automatisch dem Azure AD Connect-Dienst zugewiesen und ist weder für eine andere Verwendung vorgesehen, noch wird eine andere Verwendung unterstützt.

* **[Verzeichnis schreiben:](#directory-writers)** Dies ist eine Legacyrolle, die Anwendungen ohne Unterstützung für das [Consent Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md) zugewiesen wird. Diese Rolle sollte keinem Benutzer zugewiesen werden.

* **[Dynamics 365-Dienstadministrator/CRM-Dienstadministrator:](#dynamics-365-service-administrator)** Benutzer mit dieser Rolle verfügen über globale Berechtigungen in Microsoft Dynamics 365 Online, sofern der Dienst vorhanden ist, und können Supporttickets verwalten und die Dienstintegrität überwachen. Weitere Informationen finden Sie unter [Verwenden der Dienstadministratorrolle zum Verwalten Ihres Mandanten](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).

* **[Exchange-Dienstadministrator:](#exchange-service-administrator)** Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft Exchange Online, wenn der Dienst vorhanden ist. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Globaler Administrator/Unternehmensadministrator:](#company-administrator)** Benutzer mit dieser Rolle haben Zugriff auf alle Verwaltungsfeatures in Azure Active Directory sowie Dienste, die Azure Active Directory-Identitäten nutzen, z.B. Exchange Online, SharePoint Online oder Skype for Business Online. Die Person, die die Anmeldung für den Azure Active Directory-Mandanten vornimmt, wird ein globaler Administrator. Nur globale Administratoren können weitere Administratorrollen zuweisen. In Ihrem Unternehmen können mehrere globale Administratoren vorhanden sein. Globale Administratoren können das Kennwort für alle Benutzer und alle anderen Administratoren zurücksetzen.

  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und in Azure AD PowerShell wird diese Rolle als „Unternehmensadministrator“ identifiziert. Im [Azure-Portal](https://portal.azure.com)ist dies der „globale Administrator“.
  >
  >

* **[Gasteinladender:](#guest-inviter)** Benutzer mit dieser Rolle können Einladungen für Azure Active Directory B2B-Gastbenutzer verwalten, wenn die Benutzereinstellung **Mitglieder können einladen** auf „Nein“ festgelegt ist. Weitere Informationen zur B2B-Zusammenarbeit finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Es sind keine anderen Berechtigungen eingeschlossen.

* **[Information Protection-Administrator:](#information-protection-administrator)** Benutzer mit dieser Rolle verfügen über alle Berechtigungen für den Azure Information Protection-Dienst. Sie können Bezeichnungen für die Azure Information Protection-Richtlinie konfigurieren, Schutzvorlagen verwalten und den Schutz aktivieren. Diese Rolle gewährt keine Berechtigungen für Identity Protection Center, Privileged Identity Management, Monitor Office 365 Service Health oder Office 365 Security & Compliance Center.

* **[Intune-Dienstadministrator:](#intune-service-administrator)** Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft Intune Online, wenn der Dienst vorhanden ist. Darüber hinaus beinhaltet diese Rolle die Möglichkeit, Benutzer und Geräte zum Zuordnen von Richtlinien zu verwalten sowie Gruppen zu erstellen und zu verwalten. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung mit Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control).

* **[Lizenzadministrator:](#license-administrator)** Benutzer mit dieser Rolle können Lizenzzuweisungen für Benutzer und Gruppen (unter Verwendung der gruppenbasierten Lizenzierung) hinzufügen, entfernen und aktualisieren sowie den Verwendungsstandort für Benutzer verwalten. Mit dieser Rolle ist es nicht möglich, Abonnements zu erwerben oder zu verwalten, Gruppen zu erstellen oder zu verwalten oder Benutzer zu erstellen oder zu verwalten (mit Ausnahme des Verwendungsstandorts).

* **[Nachrichtencenter-Leser:](#message-center-reader)** Benutzer mit dieser Rolle können Benachrichtigungen und empfohlene Integritätsupdates für ihre Organisation und die konfigurierten Dienste wie Exchange, Intune und Microsoft Teams im [Office 365-Nachrichtencenter](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) überwachen. Nachrichtencenter-Leser erhalten eine wöchentliche E-Mail-Übersicht der Beiträge und Updates und können Beiträge in Office 365 teilen. In Azure AD haben Benutzer mit dieser Rolle nur schreibgeschützten Zugriff auf Azure AD-Dienste wie Benutzer und Gruppen. 

* **[Partnersupport der Ebene 1:](#partner-tier1-support)** Verwenden Sie diese Rolle nicht. Diese Rolle wurde als veraltet markiert und wird aus Azure AD entfernt. Diese Rolle ist für einige wenige Wiederverkaufspartner von Microsoft und nicht zur allgemeinen Verwendung vorgesehen.

* **[Partnersupport der Ebene 2:](#partner-tier2-support)** Verwenden Sie diese Rolle nicht. Diese Rolle wurde als veraltet markiert und wird aus Azure AD entfernt. Diese Rolle ist für einige wenige Wiederverkaufspartner von Microsoft und nicht zur allgemeinen Verwendung vorgesehen.

* **[Kennwortadministrator/Helpdeskadministrator:](#helpdesk-administrator)** Benutzer mit dieser Rolle können Kennwörter ändern, Dienstanforderungen verwalten und die Integrität des Diensts überwachen. Helpdeskadministratoren können Kennwörter nur für Benutzer und andere Helpdeskadministratoren ändern. 

  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und Azure AD PowerShell wird diese Rolle als „Helpdeskadministrator“ bezeichnet. Im [Azure-Portal](https://portal.azure.com/) lautet diese „Kennwortadministrator“.
  >
  >
  
* **[Power BI-Dienstadministrator:](#power-bi-service-administrator)** Benutzer mit dieser Rolle verfügen über globale Berechtigungen in Microsoft Power BI, wenn der Dienst vorhanden ist, sowie die Möglichkeit, Supporttickets zu verwalten und die Dienstintegrität zu überwachen. Weitere Informationen finden Sie unter [Grundlegendes zur Power BI-Administratorrolle](https://docs.microsoft.com/power-bi/service-admin-role).

* **[Administrator für privilegierte Rollen:](#privileged-role-administrator)** Benutzer mit dieser Rolle können Rollenzuweisungen in Azure Active Directory und in Azure AD Privileged Identity Management vornehmen. Überdies ermöglicht diese Rolle die vollumfängliche Verwaltung von Privileged Identity Management.

* **[Berichtsleser:](#reports-reader)** Benutzer mit dieser Rolle können Verwendungsberichtsdaten und das Berichtsdashboard in Office 365 Admin Center und das Anpassungskontextpaket in Power BI anzeigen. Darüber hinaus stellt die Rolle Zugriff auf Anmeldeberichte und -aktivitäten in Azure AD und von der Microsoft Graph-Berichterstellungs-API zurückgegebene Daten zur Verfügung. Ein Benutzer, dem die Rolle „Meldet Reader“ zugewiesen ist, kann nur auf relevante Nutzungs- und Anpassungsmetriken zugreifen. Sie verfügen nicht über Administratorrechte, um Einstellungen zu konfigurieren oder auf produktspezifische Verwaltungskonsolen wie das Exchange Admin Center zuzugreifen. 

* **[Sicherheitsadministrator:](#security-administrator)** Benutzer mit dieser Rolle verfügen über alle Leseberechtigungen der Rolle „Sicherheit lesen“ sowie die Möglichkeit, die Konfiguration für sicherheitsrelevante Dienste zu verwalten (z.B. Azure Active Directory Identity Protection, Azure Information Protection, Privileged Identity Management oder Office 365 Security & Compliance Center). Weitere Informationen zu Office 365-Berechtigungen finden Sie unter [Berechtigungen im Office 365 Security & Compliance Center](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  | Geben Sie in | Möglich |
  | --- | --- |
  | Identity Protection Center |<ul><li>Alle Berechtigungen der Rolle „Sicherheit lesen“<li>Darüber hinaus die Möglichkeit, alle IPC-Vorgänge außer des Zurücksetzens von Kennwörtern auszuführen. |
  | Privileged Identity Management |<ul><li>Alle Berechtigungen der Rolle „Sicherheit lesen“<li>**nicht** verwalten. |
  | <p>Überwachen der Office 365-Dienstintegrität</p><p>Office 365 Security & Compliance Center |<ul><li>Alle Berechtigungen der Rolle „Sicherheit lesen“<li>Kann alle Einstellungen im Feature „Advanced Threat Protection“ (Schutz vor Malware und Viren, schadhafte URL-Konfiguration, URL-Ablaufverfolgung usw.) konfigurieren. |
  
* **[Sicherheit lesen:](#security-reader)** Benutzer mit dieser Rolle verfügen über einen globalen schreibgeschützten Zugriff, einschließlich aller Informationen in Azure Active Directory, Identity Protection, Privileged Identity Management, sowie die Möglichkeit, Azure Active Directory-Anmeldeberichte und -Überwachungsprotokolle zu lesen. Die Rolle gewährt außerdem Leseberechtigung im Office 365 Security & Compliance Center. Weitere Informationen zu Office 365-Berechtigungen finden Sie unter [Berechtigungen im Office 365 Security & Compliance Center](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  | Geben Sie in | Möglich |
  | --- | --- |
  | Identity Protection Center |Lesen von allen Sicherheitsberichten und Einstellungsinformationen für die Sicherheitsfunktionen<ul><li>Antispam<li>Verschlüsselung<li>Verhindern von Datenverlusten<li>Antischadsoftware<li>Erweiterter Schutz vor Bedrohungen<li>Antiphishing<li>Nachrichtenflussregeln |
  | Privileged Identity Management |<p>Verfügt über schreibgeschützten Zugriff auf alle eingeblendeten Informationen in Azure AD PIM: Richtlinien und Berichte für Azure AD-Rollenzuweisungen, Sicherheitsüberprüfungen und in Zukunft Lesezugriff auf Richtliniendaten und Berichte für Szenarios zusätzlich zur Azure AD-Rollenzuweisung.<p>**Kann sich nicht** für Azure AD PIM registrieren oder Änderungen durchführen. Im PIM-Portal oder über PowerShell können Personen mit dieser Rolle zusätzliche Rollen (z.B. globaler Administrator oder Administrator für privilegierte Rollen) aktivieren, wenn der Benutzer für sie geeignet ist. |
  | <p>Überwachen der Office 365-Dienstintegrität</p><p>Office 365 Security & Compliance Center</p> |<ul><li>Lesen und Verwalten von Warnungen<li>Lesen von Sicherheitsrichtlinien<li>Lesen von Bedrohungsanalysen, Cloud App Discovery und Quarantäne in Suchen und Untersuchungen<li>Lesen aller Berichte |

* **[Dienstunterstützungsadministrator:](#service-support-administrator)** Benutzer mit dieser Rolle können bei Microsoft Supportanfragen für Azure- und Office 365-Dienste öffnen sowie das Service-Dashboard und das Nachrichtencenter im Azure-Portal und im Office 365-Verwaltungsportal einsehen. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[SharePoint-Dienstadministrator:](#sharepoint-service-administrator)** Benutzer mit dieser Rolle verfügen über globale Berechtigungen in Microsoft SharePoint Online, wenn der Dienst vorhanden ist, sowie die Möglichkeit, Supporttickets zu verwalten und die Dienstintegrität zu überwachen. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Skype for Business-/Lync-Dienstadministrator:](#lync-service-administrator)** Benutzer mit dieser Rolle verfügen über globale Berechtigungen in Microsoft Skype for Business, wenn der Dienst vorhanden ist, sowie die Berechtigung zur Verwaltung von Skype-spezifischen Benutzerattributen in Azure Active Directory. Darüber hinaus bietet diese Rolle die Möglichkeit, Supporttickets zu verwalten und die Dienstintegrität zu überwachen. Weitere Informationen finden Sie unter [Informationen zur Skype for Business-Administratorrolle](https://support.office.com/en-us/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5).

  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und Azure AD PowerShell wird diese Rolle als „Lync-Dienstadministrator“ bezeichnet. Im [Azure-Portal](https://portal.azure.com/) lautet diese „Skype for Business-Dienstadministrator“.
  >
  >

* **[Benutzerkontoadministrator:](#user-account-administrator)** Benutzer mit dieser Rolle können sämtliche Benutzer und Gruppen erstellen und verwalten. Darüber hinaus beinhaltet diese Rolle die Möglichkeit, Supporttickets zu verwalten und die Dienstintegrität zu überwachen. Es gelten einige Einschränkungen. So lässt diese Rolle beispielsweise nicht das Löschen eines globalen Administrators zu. Benutzerkontoadministratoren können Kennwörter für Benutzer, Helpdeskadministratoren und andere Benutzerkontoadministratoren ändern.

| Möglich | Nicht möglich |
| --- | --- |
| <p>Anzeigen von Unternehmens- und Benutzerinformationen</p><p>Verwalten von Office-Supporttickets</p><p>Ändern von Kennwörtern nur für Benutzer, Helpdeskadministratoren und andere Benutzerkontoadministratoren durch Benutzerkontoadministratoren</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Erstellen, Bearbeiten und Löschen von Benutzern und Gruppen, Verwalten von Benutzerlizenzen (mit Einschränkungen). Er oder Sie kann keinen globalen Administrator löschen oder andere Administratoren erstellen.</p> |<p>Durchführen von Abrechnungs- und Kaufvorgängen für Office-Produkte</p><p>Verwalten von Domänen</p><p>Verwalten von Unternehmensinformationen</p><p>Delegieren von Administratorrollen an andere Benutzer</p><p>Verwenden der Verzeichnissynchronisierung</p><p>Aktivieren und Deaktivieren der Multi-Factor Authentication</p><p>Anzeigen von Überwachungsprotokollen</p> |

## <a name="deprecated-roles"></a>Veraltete Rollen

Die folgenden Rollen sollten nicht verwendet werden. Sie wurden als veraltet markiert und werden aus Azure AD entfernt.

* Ad-hoc-Lizenzadministrator
* Geräteeinbindung
* Geräte-Manager
* Gerätebenutzer
* Benutzererstellung mit E-Mail-Überprüfung
* Postfachadministrator
* Geräteeinbindung am Arbeitsplatz

## <a name="detailed-azure-active-directory-permissions"></a>Ausführliche Azure Active Directory-Berechtigungen
In den folgenden Tabellen werden die spezifischen Berechtigungen der einzelnen Rollen in Azure Active Directory beschrieben. Einige Rollen, z.B. der globale Administrator, können zusätzliche Berechtigungen in Microsoft-Diensten außerhalb von Azure Active Directory aufweisen.

### <a name="adhoc-license-administrator"></a>Ad-hoc-Lizenzadministrator
Kann alle Aspekte von App-Registrierungen und Enterprise-Apps erstellen und verwalten.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/domains/default/read | Lesen der Basiseigenschaften für Domänen in Azure Active Directory |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lesen der groups.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/default/read | Lesen der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/memberOf/read | Lesen der groups.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/members/read | Lesen der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/read | Lesen der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/settings/read | Lesen der groups.settings-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/oAuth2PermissionGrants/default/read | Lesen der Basiseigenschaften für oAuth2PermissionGrants in Azure Active Directory |
| microsoft.aad.directory/oAuth2PermissionGrants/update | Aktualisieren von oAuth2PermissionGrants in Azure Active Directory |
| microsoft.aad.directory/organization/default/read | Lesen der Basiseigenschaften für die Organisation in Azure Active Directory |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Lesen der organizations.trustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/read | Lesen der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/default/read | Lesen der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/directReports/read | Lesen der users.directReports-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/invitedBy/read | Lesen der users.invitedBy-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/invitedUsers/read | Lesen der users.invitedUsers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/manager/read | Lesen der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/memberOf/read | Lesen der users.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lesen der users.oAuth2PermissionGrants-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedDevices/read | Lesen der users.ownedDevices-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedObjects/read | Lesen der users.ownedObjects-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/registeredDevices/read | Lesen der users.registeredDevices-Eigenschaft in Azure Active Directory |

### <a name="application-administrator"></a>Anwendungsadministrator
Kann alle Aspekte von App-Registrierungen und Enterprise-Apps erstellen und verwalten.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/applications/create | Erstellen von Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/default/update | Aktualisieren der Basiseigenschaften für Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/delete | Löschen von Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/owners/update | Aktualisieren der applications.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/policies/update | Aktualisieren der applications.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/create | Erstellen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/read | Lesen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/update | Aktualisieren von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/delete | Löschen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/create | Erstellen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | Aktualisieren der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Löschen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Aktualisieren der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/create | Erstellen von servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/default/update | Aktualisieren der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/delete | Löschen von servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Aktualisieren der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/update | Aktualisieren der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualisieren der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.reports/allEntities/read | Lesen von Azure AD-Berichten. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="application-developer"></a>Anwendungsentwickler
Erstellen von Anwendungsregistrierungen unabhängig von der Einstellung „Benutzer können Anwendungen registrieren“

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Erstellen von Anwendungen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Erstellen von appRoleAssignments in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Erstellen von oAuth2PermissionGrants in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Erstellen von servicePrincipals in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |

### <a name="billing-administrator"></a>Abrechnungsadministrator
Ausführen von allgemeinen Abrechnungsaufgaben wie der Aktualisierung der Zahlungsinformationen.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/organization/default/update | Aktualisieren der Basiseigenschaften für die Organisation in Azure Active Directory |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Aktualisieren der organizations.trustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.commerce.billing/allEntities/allTasks | Verwalten sämtlicher Aspekte der Office 365-Abrechnung. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="cloud-application-administrator"></a>Cloudanwendungsadministrator
Erstellen und Verwalten sämtlicher Aspekte von App-Registrierungen und Enterprise-Apps außer App-Proxy.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/applications/create | Erstellen von Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/default/update | Aktualisieren der Basiseigenschaften für Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/delete | Löschen von Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/owners/update | Aktualisieren der applications.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/policies/update | Aktualisieren der applications.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/create | Erstellen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/update | Aktualisieren von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/delete | Löschen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/create | Erstellen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | Aktualisieren der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Löschen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Aktualisieren der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Aktualisieren der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/create | Erstellen von servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/default/update | Aktualisieren der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/delete | Löschen von servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/update | Aktualisieren der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualisieren der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.reports/allEntities/read | Lesen von Azure AD-Berichten. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="company-administrator"></a>Unternehmensadministrator
Verwalten sämtlicher Aspekte von Azure AD und Microsoft-Diensten, die Azure AD-Identitäten verwenden.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Erstellen und Löschen von administrativeUnits und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/applications/allProperties/allTasks | Erstellen und Löschen von Anwendungen und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Erstellen und Löschen von appRoleAssignments und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/contacts/allProperties/allTasks | Erstellen und Löschen von Kontakten und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/contracts/allProperties/allTasks | Erstellen und Löschen von Verträgen und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/devices/allProperties/allTasks | Erstellen und Löschen von Geräten und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | Erstellen und Löschen von directoryRoles und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | Erstellen und Löschen von directoryRoleTemplates und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/domains/allProperties/allTasks | Erstellen und Löschen von Domänen und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/groups/allProperties/allTasks | Erstellen und Löschen von Gruppen und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | Erstellen und Löschen von groupSettings und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | Erstellen und Löschen von groupSettingTemplates und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | Erstellen und Löschen von loginTenantBranding und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | Erstellen und Löschen von oAuth2PermissionGrants und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/organization/allProperties/allTasks | Erstellen und Löschen der Organisationen und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/policies/allProperties/allTasks | Erstellen und Löschen von Richtlinien und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Erstellen und Löschen von roleAssignments und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Erstellen und Löschen von roleDefinitions und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | Erstellen und Löschen von scopedRoleMemberships und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/serviceAction/activateService | Ausführen der Dienstaktion Activateservice in Azure Active Directory |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | Ausführen der Dienstaktion DisableDirectoryFeature in Azure Active Directory |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | Ausführen der Dienstaktion Enabledirectoryfeature in Azure Active Directory |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | Ausführen der Dienstaktion Getavailableextentionproperties in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | Erstellen und Löschen von servicePrincipals und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Erstellen und Löschen von subscribedSkus und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/users/allProperties/allTasks | Erstellen und Löschen von Benutzern und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directorySync/allEntities/allTasks | Ausführen sämtlicher Aktionen in Azure AD Connect. |
| microsoft.aad.identityProtection/allEntities/allTasks | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren von Standardeigenschaften in microsoft.identityProtection |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lesen aller Ressourcen in microsoft.aad.privilegedIdentityManagement |
| microsoft.aad.reports/allEntities/allTasks | Lesen und Konfigurieren von Azure AD-Berichten. |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.informationProtection/allEntities/allTasks | Verwalten sämtlicher Aspekte von Azure Information Protection |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.commerce.billing/allEntities/allTasks | Verwalten sämtlicher Aspekte der Office 365-Abrechnung. |
| microsoft.office365.complianceManager/allEntities/allTasks | Verwalten sämtlicher Aspekte von Office 365 Compliance-Manager |
| microsoft.office365.exchange/allEntities/allTasks | Verwalten sämtlicher Aspekte von Exchange Online. |
| microsoft.intune/allEntities/allTasks | Verwalten sämtlicher Aspekte von Intune. |
| microsoft.office365.lockbox/allEntities/allTasks | Verwalten sämtlicher Aspekte der Office 365 Kunden-Lockbox |
| microsoft.powerApps.powerBI/allEntities/allTasks | Verwalten sämtlicher Aspekte von Power BI. |
| microsoft.office365.protectionCenter/allEntities/allTasks | Verwalten sämtlicher Aspekte von Office 365 Protection Center |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.sharepoint/allEntities/allTasks | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren von Standardeigenschaften in microsoft.office365.sharepoint |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Verwalten sämtlicher Aspekte von Skype for Business Online |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Verwalten sämtlicher Aspekte von Dynamics 365. |

### <a name="compliance-administrator"></a>Complianceadministrator
Lesen und Verwalten der Konformitätskonfiguration und der zugehörigen Berichte in Azure AD und Office 365.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.complianceManager/allEntities/allTasks | Verwalten sämtlicher Aspekte von Office 365 Compliance-Manager |
| microsoft.office365.exchange/allEntities/allTasks | Verwalten sämtlicher Aspekte von Exchange Online. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.sharepoint/allEntities/allTasks | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren von Standardeigenschaften in microsoft.office365.sharepoint |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Verwalten sämtlicher Aspekte von Skype for Business Online |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="conditional-access-administrator"></a>Administrator für den bedingten Zugriff
Verwalten von Funktionen zum bedingten Zugriff.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/create | Erstellen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/default/read | Lesen der policies.conditionalAccess-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/default/update | Aktualisieren der policies.conditionalAccess-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/delete | Löschen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Lesen der policies.conditionalAccess-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Aktualisieren der policies.conditionalAccess-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Lesen der policies.conditionalAccess-Eigenschaft in Azure Active Directory |

### <a name="crm-service-administrator"></a>CRM-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Dynamics 365.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Verwalten sämtlicher Aspekte von Dynamics 365. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="customer-lockbox-access-approver"></a>Genehmigende Person für den LockBox-Kundenzugriff
Kann Microsoft-Supportanfragen zum Zugriff auf Benutzerorganisationsdaten genehmigen.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.office365.lockbox/allEntities/allTasks | Verwalten sämtlicher Aspekte der Office 365 Kunden-Lockbox |

### <a name="device-administrators"></a>Geräteadministratoren
Mitglieder dieser Rolle werden der lokalen Administratorgruppe auf in Azure AD eingebundenen Geräten hinzugefügt.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/groupSettings/default/read | Lesen der Basiseigenschaften für groupSettings in Azure Active Directory |
| microsoft.aad.directory/groupSettingTemplates/default/read | Lesen der Basiseigenschaften für groupSettingTemplates in Azure Active Directory |

### <a name="device-managers"></a>Geräte-Manager
Kann Microsoft-Supportanfragen zum Zugriff auf Benutzerorganisationsdaten genehmigen.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/devices/default/read | Lesen der Basiseigenschaften für Geräte in Azure Active Directory |
| microsoft.aad.directory/devices/default/update | Aktualisieren der Basiseigenschaften für Geräte in Azure Active Directory |
| microsoft.aad.directory/devices/memberOf/read | Lesen der devices.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/devices/registeredOwners/read | Lesen der devices.registeredOwners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/devices/registeredOwners/update | Aktualisieren der devices.registeredOwners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/devices/registeredUsers/read | Lesen der devices.registeredUsers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/devices/registeredUsers/update | Aktualisieren der devices.registeredUsers-Eigenschaft in Azure Active Directory |

### <a name="directory-readers"></a>Rolle „Verzeichnis lesen“
Lesen von grundlegenden Verzeichnisinformationen. Zum Gewähren des Zugriffs auf Anwendungen

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/default/read | Lesen der Basiseigenschaften für administrativeUnits in Azure Active Directory |
| microsoft.aad.directory/administrativeUnits/members/read | Lesen der administrativeUnits.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/default/read | Lesen der Basiseigenschaften für Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/owners/read | Lesen der applications.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/contacts/default/read | Lesen der Basiseigenschaften für Kontakte in Azure Active Directory |
| microsoft.aad.directory/contacts/memberOf/read | Lesen der contacts.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/contracts/default/read | Lesen der Basiseigenschaften für Verträge in Azure Active Directory |
| microsoft.aad.directory/devices/default/read | Lesen der Basiseigenschaften für Geräte in Azure Active Directory |
| microsoft.aad.directory/devices/memberOf/read | Lesen der devices.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/devices/registeredOwners/read | Lesen der devices.registeredOwners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/devices/registeredUsers/read | Lesen der devices.registeredUsers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/directoryRoles/default/read | Lesen der Basiseigenschaften für directoryRoles in Azure Active Directory |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Lesen der directoryRoles.eligibleMembers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/directoryRoles/members/read | Lesen der directoryRoles.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/domains/default/read | Lesen der Basiseigenschaften für Domänen in Azure Active Directory |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lesen der groups.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/default/read | Lesen der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/memberOf/read | Lesen der groups.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/members/read | Lesen der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/read | Lesen der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/settings/read | Lesen der groups.settings-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groupSettings/default/read | Lesen der Basiseigenschaften für groupSettings in Azure Active Directory |
| microsoft.aad.directory/groupSettingTemplates/default/read | Lesen der Basiseigenschaften für groupSettingTemplates in Azure Active Directory |
| microsoft.aad.directory/oAuth2PermissionGrants/default/read | Lesen der Basiseigenschaften für oAuth2PermissionGrants in Azure Active Directory |
| microsoft.aad.directory/organization/default/read | Lesen der Basiseigenschaften für die Organisation in Azure Active Directory |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Lesen der organizations.trustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lesen der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lesen der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/default/read | Lesen der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lesen der servicePrincipals.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Lesen der servicePrincipals.oAuth2PermissionGrants-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lesen der servicePrincipals.ownedObjects-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/read | Lesen der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/read | Lesen der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/subscribedSkus/default/read | Lesen der Basiseigenschaften für subscribedSkus in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/read | Lesen der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/default/read | Lesen der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/directReports/read | Lesen der users.directReports-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/invitedBy/read | Lesen der users.invitedBy-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/invitedUsers/read | Lesen der users.invitedUsers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/manager/read | Lesen der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/memberOf/read | Lesen der users.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lesen der users.oAuth2PermissionGrants-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedDevices/read | Lesen der users.ownedDevices-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedObjects/read | Lesen der users.ownedObjects-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/registeredDevices/read | Lesen der users.registeredDevices-Eigenschaft in Azure Active Directory |

### <a name="directory-synchronization-accounts"></a>Konten zur Verzeichnissynchronisierung
Nur vom Azure AD Connect-Dienst verwendet.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Aktualisieren der organization.dirSync-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/create | Erstellen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/delete | Löschen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/default/read | Lesen der Basiseigenschaften für Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/default/update | Aktualisieren der Basiseigenschaften für Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/owners/read | Lesen der policies.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/owners/update | Aktualisieren der policies.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Lesen der policies.policiesAppliedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lesen der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lesen der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Aktualisieren der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/create | Erstellen von servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/default/read | Lesen der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/default/update | Aktualisieren der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lesen der servicePrincipals.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Lesen der servicePrincipals.oAuth2PermissionGrants-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/read | Lesen der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/update | Aktualisieren der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lesen der servicePrincipals.ownedObjects-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/read | Lesen der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualisieren der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directorySync/allEntities/allTasks | Ausführen sämtlicher Aktionen in Azure AD Connect. |

### <a name="directory-writers"></a>Verzeichnis schreiben
Lesen und Schreiben von grundlegenden Verzeichnisinformationen. Zum Gewähren des Zugriffs auf Anwendungen

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/groups/create | Erstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/createAsOwner | Erstellen von Gruppen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Aktualisieren der groups.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/default/update | Aktualisieren der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/members/update | Aktualisieren der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/update | Aktualisieren der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/settings/update | Aktualisieren der groups.settings-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groupSettings/create | Erstellen der groupSettings in Azure Active Directory |
| microsoft.aad.directory/groupSettings/default/update | Aktualisieren der Basiseigenschaften für groupSettings in Azure Active Directory |
| microsoft.aad.directory/groupSettings/delete | Löschen von groupSettings in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualisieren der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/default/update | Aktualisieren der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/users/manager/update | Aktualisieren der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/userPrincipalName/update | Aktualisieren der users.userPrincipalName-Eigenschaft in Azure Active Directory |

### <a name="exchange-service-administrator"></a>Exchange-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Exchange.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.exchange/allEntities/allTasks | Verwalten sämtlicher Aspekte von Exchange Online. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="guest"></a>Gast
Die Standardrolle für Gastbenutzer. Lesen einer begrenzten Anzahl von Verzeichnisinformationen.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/applications/default/read | Lesen der Basiseigenschaften für Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/owners/read | Lesen der applications.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/domains/default/read | Lesen der Basiseigenschaften für Domänen in Azure Active Directory |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lesen der groups.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/default/read | Lesen der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/memberOf/read | Lesen der groups.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/members/read | Lesen der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/read | Lesen der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/settings/read | Lesen der groups.settings-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/organization/basicProfile/read | Lesen von Basisinformationen zum Organisationsprofil in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lesen der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lesen der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/default/read | Lesen der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lesen der servicePrincipals.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/members/read | Lesen der servicePrincipals.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Lesen der servicePrincipals.oAuth2PermissionGrants-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/read | Lesen der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lesen der servicePrincipals.ownedObjects-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/read | Lesen der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/basicProfile/read | Lesen der users.basicProfile-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/read | Lesen der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/default/read | Lesen der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/directReports/read | Lesen der users.directReports-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/eligibleMemberOf/read | Lesen der users.eligibleMemberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/invitedBy/read | Lesen der users.invitedBy-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/invitedUsers/read | Lesen der users.invitedUsers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/manager/read | Lesen der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/memberOf/read | Lesen der users.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lesen der users.oAuth2PermissionGrants-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedDevices/read | Lesen der users.ownedDevices-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedObjects/read | Lesen der users.ownedObjects-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/password/update | Aktualisieren der Kennwörter für alle Benutzer in Azure Active Directory. Weitere Informationen finden Sie in der Onlinedokumentation. |
| microsoft.aad.directory/users/pendingMemberOf/read | Lesen der users.pendingMemberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/registeredDevices/read | Lesen der users.registeredDevices-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/scopedAdministratorOf/read | Lesen der users.scopedAdministratorOf-Eigenschaft in Azure Active Directory |

### <a name="guest-inviter"></a>Gasteinladender
Einladen von Gastbenutzern unabhängig von der Einstellung „Mitglieder können Gäste einladen“.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Lesen der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/default/read | Lesen der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/directReports/read | Lesen der users.directReports-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/invitedBy/read | Lesen der users.invitedBy-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/inviteGuest | Einladen von Gastbenutzern in Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Lesen der users.invitedUsers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/manager/read | Lesen der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/memberOf/read | Lesen der users.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lesen der users.oAuth2PermissionGrants-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedDevices/read | Lesen der users.ownedDevices-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedObjects/read | Lesen der users.ownedObjects-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/registeredDevices/read | Lesen der users.registeredDevices-Eigenschaft in Azure Active Directory |

### <a name="helpdesk-administrator"></a>Helpdeskadministrator
Zurücksetzen von Kennwörtern für Nicht-Administratoren und Helpdeskadministratoren.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/users/password/update | Aktualisieren der Kennwörter für alle Benutzer in Azure Active Directory. Weitere Informationen finden Sie in der Onlinedokumentation. |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="information-protection-administrator"></a>Information Protection-Administrator
Verwalten sämtlicher Aspekte des Produkts Azure Information Protection.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Verwalten sämtlicher Aspekte von Azure Information Protection |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="intune-service-administrator"></a>Intune-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Intune.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/contacts/create | Erstellen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/contacts/default/update | Aktualisieren der Basiseigenschaften für Kontakte in Azure Active Directory |
| microsoft.aad.directory/contacts/delete | Löschen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/devices/create | Erstellen von Geräten in Azure Active Directory |
| microsoft.aad.directory/devices/default/update | Aktualisieren der Basiseigenschaften für Geräte in Azure Active Directory |
| microsoft.aad.directory/devices/delete | Löschen von Geräten in Azure Active Directory |
| microsoft.aad.directory/devices/registeredOwners/update | Aktualisieren der devices.registeredOwners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/devices/registeredUsers/update | Aktualisieren der devices.registeredUsers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/appRoleAssignments/update | Aktualisieren der groups.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/create | Erstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/createAsOwner | Erstellen von Gruppen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/groups/default/update | Aktualisieren der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/delete | Löschen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/hiddenMembers/read | Lesen der groups.hiddenMembers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/members/update | Aktualisieren der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/update | Aktualisieren der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/restore | Wiederherstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/settings/update | Aktualisieren der groups.settings-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualisieren der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/default/update | Aktualisieren der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/manager/update | Aktualisieren der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.intune/allEntities/allTasks | Verwalten sämtlicher Aspekte von Intune. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="license-administrator"></a>Lizenzadministrator
Kann Produktlizenzen für Benutzer und Gruppen verwalten.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/usageLocation/update | Aktualisieren der users.usageLocation-Eigenschaft in Azure Active Directory |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |

### <a name="lync-service-administrator"></a>Lync-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Skype for Business.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Verwalten sämtlicher Aspekte von Skype for Business Online |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="message-center-reader"></a>Nachrichtencenter-Leser
Lesen von Nachrichten und Updates für die Organisation ausschließlich im Office 365-Nachrichtencenter. 

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.accessmessagecenter/allEntities/allTasks | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren der Standardeigenschaften im Nachrichtencenter. |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |

### <a name="partner-tier1-support"></a>Partnersupport der Ebene 1
Verwenden Sie diese Rolle nicht – sie ist nicht zur allgemeinen Verwendung vorgesehen.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/contacts/create | Erstellen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/contacts/default/update | Aktualisieren der Basiseigenschaften für Kontakte in Azure Active Directory |
| microsoft.aad.directory/contacts/delete | Löschen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/groups/create | Erstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/createAsOwner | Erstellen von Gruppen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/groups/members/update | Aktualisieren der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/update | Aktualisieren der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualisieren der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/default/update | Aktualisieren der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/delete | Löschen von Benutzern in Azure Active Directory |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/users/manager/update | Aktualisieren der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/password/update | Aktualisieren der Kennwörter für alle Benutzer in Azure Active Directory. Weitere Informationen finden Sie in der Onlinedokumentation. |
| microsoft.aad.directory/users/restore | Wiederherstellen gelöschter Benutzerkonten in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Aktualisieren der users.userPrincipalName-Eigenschaft in Azure Active Directory |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="partner-tier2-support"></a>Partnersupport der Ebene 2
Verwenden Sie diese Rolle nicht – sie ist nicht zur allgemeinen Verwendung vorgesehen.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/contacts/create | Erstellen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/contacts/default/update | Aktualisieren der Basiseigenschaften für Kontakte in Azure Active Directory |
| microsoft.aad.directory/contacts/delete | Löschen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/domains/allTasks | Erstellen und Löschen von Domänen und Lesen und Aktualisieren der Standardeigenschaften in Azure Active Directory |
| microsoft.aad.directory/groups/create | Erstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/delete | Löschen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/members/update | Aktualisieren der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/restore | Wiederherstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/organization/default/update | Aktualisieren der Basiseigenschaften für die Organisation in Azure Active Directory |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Aktualisieren der organizations.trustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualisieren der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/default/update | Aktualisieren der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/delete | Löschen von Benutzern in Azure Active Directory |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/users/manager/update | Aktualisieren der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/password/update | Aktualisieren der Kennwörter für alle Benutzer in Azure Active Directory. Weitere Informationen finden Sie in der Onlinedokumentation. |
| microsoft.aad.directory/users/restore | Wiederherstellen gelöschter Benutzerkonten in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Aktualisieren der users.userPrincipalName-Eigenschaft in Azure Active Directory |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="power-bi-service-administrator"></a>Power BI-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Power BI.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.powerApps.powerBI/allEntities/allTasks | Verwalten sämtlicher Aspekte von Power BI. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="privileged-role-administrator"></a>Administrator für privilegierte Rollen
Verwalten von Rollenzuweisungen in Azure AD.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | Aktualisieren von directoryRoles in Azure Active Directory |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren von Standardeigenschaften in microsoft.aad.privilegedIdentityManagement |

### <a name="reports-reader"></a>Meldet Reader
Lesen von Anmeldungs- und Überwachungsberichten.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.reports/allEntities/read | Lesen von Azure AD-Berichten. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.usageReports/allEntities/read | Lesen von Office 365-Nutzungsberichten. |

### <a name="security-administrator"></a>Sicherheitsadministrator
Lesen von Sicherheitsinformationen und Berichten

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/applications/policies/update | Aktualisieren der applications.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/create | Erstellen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/default/update | Aktualisieren der Basiseigenschaften für Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/delete | Löschen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/owners/update | Aktualisieren der policies.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualisieren der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.identityProtection/allEntities/read | Lesen aller Ressourcen in microsoft.aad.identityProtection |
| microsoft.aad.identityProtection/allEntities/update | Aktualisieren aller Ressourcen in microsoft.aad.identityProtection |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lesen aller Ressourcen in microsoft.aad.privilegedIdentityManagement |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.office365.protectionCenter/allEntities/read | Lesen sämtlicher Aspekte von Office 365 Protection Center. |
| microsoft.office365.protectionCenter/allEntities/update | Aktualisieren aller Ressourcen in microsoft.office365.protectionCenter |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |

### <a name="security-reader"></a>Sicherheit lesen
Lesen von Sicherheitsinformationen und Berichten in Azure AD und Office 365.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.identityProtection/allEntities/read | Lesen aller Ressourcen in microsoft.aad.identityProtection |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lesen aller Ressourcen in microsoft.aad.privilegedIdentityManagement |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.office365.protectionCenter/allEntities/read | Lesen sämtlicher Aspekte von Office 365 Protection Center. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |

### <a name="service-support-administrator"></a>Dienstunterstützungsadministrator
Lesen von Service Health-Informationen und Verwalten von Supporttickets.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="sharepoint-service-administrator"></a>SharePoint-Dienstadministrator
Verwalten sämtlicher Aspekte des SharePoint-Diensts.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.sharepoint/allEntities/allTasks | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren von Standardeigenschaften in microsoft.office365.sharepoint |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="user-account-administrator"></a>Benutzerkontoadministrator
Verwalten sämtlicher Aspekte von Benutzern und Gruppen

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Erstellen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/delete | Löschen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/update | Aktualisieren von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/contacts/create | Erstellen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/contacts/default/update | Aktualisieren der Basiseigenschaften für Kontakte in Azure Active Directory |
| microsoft.aad.directory/contacts/delete | Löschen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/groups/appRoleAssignments/update | Aktualisieren der groups.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/create | Erstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/createAsOwner | Erstellen von Gruppen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/groups/default/update | Aktualisieren der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/delete | Löschen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/hiddenMembers/read | Lesen der groups.hiddenMembers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/members/update | Aktualisieren der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/update | Aktualisieren der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/restore | Wiederherstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/settings/update | Aktualisieren der groups.settings-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualisieren der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/create | Erstellen von Benutzern in Azure Active Directory |
| microsoft.aad.directory/users/default/update | Aktualisieren der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/delete | Löschen von Benutzern in Azure Active Directory |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/users/manager/update | Aktualisieren der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/password/update | Aktualisieren der Kennwörter für alle Benutzer in Azure Active Directory. Weitere Informationen finden Sie in der Onlinedokumentation. |
| microsoft.aad.directory/users/restore | Wiederherstellen gelöschter Benutzerkonten in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Aktualisieren der users.userPrincipalName-Eigenschaft in Azure Active Directory |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="user"></a>Benutzer
Die Standardrolle für Mitgliedsbenutzer. Lesen aller und Schreiben einer begrenzten Anzahl von Verzeichnisinformationen.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Rolle „Verzeichnis lesen“.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Erstellen von Anwendungen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/groups/createAsOwner | Erstellen von Gruppen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/groups/default/read | Lesen der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/oAuth2PermissionGrants/create | Erstellen von oAuth2PermissionGrants in Azure Active Directory |
| microsoft.aad.directory/oAuth2PermissionGrants/delete | Löschen von oAuth2PermissionGrants in Azure Active Directory |
| microsoft.aad.directory/oAuth2PermissionGrants/update | Aktualisieren von oAuth2PermissionGrants in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Erstellen von servicePrincipals in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/users/activateServicePlan | Aktivieren von Serviceplanbenutzern in Azure Active Directory |
| microsoft.aad.directory/users/inviteGuest | Einladen von Gastbenutzern in Azure Active Directory. |
| microsoft.aad.directory/applications/delete | Löschen von Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/restore | Wiederherstellen von Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/default/update | Aktualisieren der Basiseigenschaften für Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/owners/update | Aktualisieren der applications.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/policies/update | Aktualisieren der applications.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/devices/disable | Deaktivieren von Geräten in Azure Active Directory |
| microsoft.aad.directory/groups/appRoleAssignments/update | Aktualisieren der groups.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/delete | Löschen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/default/update | Aktualisieren der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/dynamicMembershipRule/update | Aktualisieren der groups.dynamicMembershipRule-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/members/update | Aktualisieren der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/update | Aktualisieren der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/restore | Wiederherstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/settings/update | Aktualisieren der groups.settings-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/delete | Löschen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/default/update | Aktualisieren der Basiseigenschaften für Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/owners/update | Aktualisieren der policies.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Aktualisieren der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/default/update | Aktualisieren der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/delete | Löschen von servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/update | Aktualisieren der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualisieren der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/changePassword | Ändern der Kennwörter für alle Benutzer in Azure Active Directory Weitere Informationen finden Sie in der Onlinedokumentation. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/users/basicProfile/update | Aktualisieren der users.basicProfile-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/mobile/update | Aktualisieren der users.mobile-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/searchableDeviceKey/update | Aktualisieren der users.searchableDeviceKey-Eigenschaft in Azure Active Directory |

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Ändern von Administratoren für ein Azure-Abonnement finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen](../../billing/billing-add-change-azure-subscription-administrator.md)
* Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Weitere Informationen zur Beziehung zwischen Azure Active Directory und Ihrem Azure-Abonnement finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
