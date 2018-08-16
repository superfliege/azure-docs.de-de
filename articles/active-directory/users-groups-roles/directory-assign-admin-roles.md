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
ms.date: 07/25/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: c6c388bb98d189d91703c0ce82971b3ec4da4150
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39505510"
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

* **[Anwendungsentwickler:](#application-developer)** Benutzer mit dieser Rolle können Anwendungsregistrierungen erstellen, wenn die Einstellung „Benutzer können Anwendungen registrieren“ auf „Nein“ festgelegt ist. Diese Rolle erlaubt es den Mitgliedern auch, in ihrem eigenen Namen zuzustimmen, wenn die Einstellung „Benutzer können Apps zustimmen, die in ihrem Namen auf Unternehmensdaten zugreifen“ auf „Nein“ gesetzt ist. Mitglieder dieser Rolle werden bei der Erstellung neuer Anwendungsregistrierungen oder Unternehmensanwendungen als Besitzer hinzugefügt.

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

* **[Dynamics 365-Dienstadministrator/CRM-Dienstadministrator:](#dynamics-365-service-administrator)** Benutzer mit dieser Rolle verfügen über globale Berechtigungen in Microsoft Dynamics 365 Online, sofern der Dienst vorhanden ist, und können Supporttickets verwalten und die Dienstintegrität überwachen. Weitere Informationen finden Sie unter [Verwenden der Dienstadministratorrolle zum Verwalten Ihres Mandanten](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).

* **[Exchange-Dienstadministrator:](#exchange-service-administrator)** Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft Exchange Online, wenn der Dienst vorhanden ist. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Globaler Administrator/Unternehmensadministrator:](#company-administrator)** Benutzer mit dieser Rolle haben Zugriff auf alle Verwaltungsfeatures in Azure Active Directory sowie Dienste, die Azure Active Directory-Identitäten nutzen, z.B. Exchange Online, SharePoint Online oder Skype for Business Online. Die Person, die die Anmeldung für den Azure Active Directory-Mandanten vornimmt, wird ein globaler Administrator. Nur globale Administratoren können weitere Administratorrollen zuweisen. In Ihrem Unternehmen können mehrere globale Administratoren vorhanden sein. Globale Administratoren können das Kennwort für alle Benutzer und alle anderen Administratoren zurücksetzen.

  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und in Azure AD PowerShell wird diese Rolle als „Unternehmensadministrator“ identifiziert. Im [Azure-Portal](https://portal.azure.com)ist dies der „globale Administrator“.
  >
  >

* **[Gasteinladender:](#guest-inviter)** Benutzer mit dieser Rolle können Einladungen für Azure Active Directory B2B-Gastbenutzer verwalten, wenn die Benutzereinstellung **Mitglieder können einladen** auf „Nein“ festgelegt ist. Weitere Informationen zur B2B-Zusammenarbeit finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Es sind keine anderen Berechtigungen eingeschlossen.

* **[Information Protection-Administrator:](#information-protection-administrator)** Benutzer mit dieser Rolle verfügen über alle Berechtigungen für den Azure Information Protection-Dienst. Sie können Bezeichnungen für die Azure Information Protection-Richtlinie konfigurieren, Schutzvorlagen verwalten und den Schutz aktivieren. Diese Rolle gewährt keine Berechtigungen für Identity Protection Center, Privileged Identity Management, Monitor Office 365 Service Health oder Office 365 Security & Compliance Center.

* **[Intune-Dienstadministrator:](#intune-service-administrator)** Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft Intune Online, wenn der Dienst vorhanden ist. Darüber hinaus beinhaltet diese Rolle die Möglichkeit, Benutzer und Geräte zum Zuordnen von Richtlinien zu verwalten sowie Gruppen zu erstellen und zu verwalten. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung mit Microsoft Intune](https://docs.microsoft.com/en-us/intune/role-based-access-control).

* **[Nachrichtencenter-Leser:](#message-center-reader)** Benutzer mit dieser Rolle können Benachrichtigungen und empfohlene Integritätsupdates für ihre Organisation und die konfigurierten Dienste wie Exchange, Intune und Microsoft Teams im [Office 365-Nachrichtencenter](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) überwachen. Nachrichtencenter-Leser erhalten eine wöchentliche E-Mail-Übersicht der Beiträge und Updates und können Beiträge in Office 365 teilen. In Azure AD haben Benutzer mit dieser Rolle nur schreibgeschützten Zugriff auf Azure AD-Dienste wie Benutzer und Gruppen. 

* **[Partnersupport der Ebene 1:](#partner-tier1-support)** Verwenden Sie diese Rolle nicht. Diese Rolle wurde als veraltet markiert und wird aus Azure AD entfernt. Diese Rolle ist für einige wenige Wiederverkaufspartner von Microsoft und nicht zur allgemeinen Verwendung vorgesehen.

* **[Partnersupport der Ebene 2:](#partner-tier2-support)** Verwenden Sie diese Rolle nicht. Diese Rolle wurde als veraltet markiert und wird aus Azure AD entfernt. Diese Rolle ist für einige wenige Wiederverkaufspartner von Microsoft und nicht zur allgemeinen Verwendung vorgesehen.

* **[Kennwortadministrator/Helpdeskadministrator:](#helpdesk-administrator)** Benutzer mit dieser Rolle können Kennwörter ändern, Dienstanforderungen verwalten und die Integrität des Diensts überwachen. Helpdeskadministratoren können Kennwörter nur für Benutzer und andere Helpdeskadministratoren ändern. 

  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und Azure AD PowerShell wird diese Rolle als „Helpdeskadministrator“ bezeichnet. Im [Azure-Portal](https://portal.azure.com/) lautet diese „Kennwortadministrator“.
  >
  >
  
* **[Power BI-Dienstadministrator:](#power-bi-service-administrator)** Benutzer mit dieser Rolle verfügen über globale Berechtigungen in Microsoft Power BI, wenn der Dienst vorhanden ist, sowie die Möglichkeit, Supporttickets zu verwalten und die Dienstintegrität zu überwachen. Weitere Informationen finden Sie unter [Grundlegendes zur Power BI-Administratorrolle](https://docs.microsoft.com/en-us/power-bi/service-admin-role).

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


### <a name="to-add-a-colleague-as-a-global-administrator"></a>Hinzufügen eines Kollegen als globalen Administrator

1. Melden Sie sich beim [Azure Active Directory Admin Center](https://aad.portal.azure.com) über ein Konto an, das als globaler Administrator oder Administrator für privilegierte Rollen für das Mandantenverzeichnis konfiguriert ist.

   ![Öffnen des Azure AD Admin Center](./media/directory-assign-admin-roles/active-directory-admin-center.png)

2. Wählen Sie **Benutzer** aus.

3. Suchen Sie den Benutzer, der als globaler Administrator festgelegt werden soll, und öffnen Sie das Blatt für diesen Benutzer.

4. Wählen Sie auf dem Blatt „Benutzer“ die Option **Verzeichnisrolle** aus.
 
5. Wählen Sie auf dem Blatt „Verzeichnisrolle“ die Rolle **Globaler Administrator** aus, und speichern Sie sie.

## <a name="detailed-azure-active-directory-permissions"></a>Ausführliche Azure Active Directory-Berechtigungen
In den folgenden Tabellen werden die spezifischen Berechtigungen der einzelnen Rollen in Azure Active Directory beschrieben. Einige Rollen, z.B. der globale Administrator, können zusätzliche Berechtigungen in Microsoft-Diensten außerhalb von Azure Active Directory aufweisen.


### <a name="application-administrator"></a>Anwendungsadministrator
Kann alle Aspekte von App-Registrierungen und Enterprise-Apps erstellen und verwalten.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Application/Create | Erstellen von Anwendungen in Azure Active Directory. |
| microsoft.aad.directory/Application/Delete | Löschen von Anwendungen in Azure Active Directory. |
| microsoft.aad.directory/Application/Update | Aktualisieren der Standardeigenschaften für Anwendungen in Azure Active Directory. |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Aktualisieren der Applications.DefaultPolicy-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Application/Update/Owners | Aktualisieren der Applications.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Create | Erstellen von AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Löschen von AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Aktualisieren der Standardeigenschaften für AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Erstellen von Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Löschen von Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Aktualisieren der Standardeigenschaften für Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Aktualisieren der Policies.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllNoDirectory | Zustimmungserteilung im Namen aller Benutzer für sämtliche Ressourcen mit Ausnahme von Azure Active Directory (Azure AD Graph und Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/Create | Erstellen von ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Delete | Löschen von ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Aktualisieren der Standardeigenschaften für ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Aktualisieren der ServicePrincipals.AppRoleAssignedTo-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Aktualisieren der ServicePrincipals.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Aktualisieren der ServicePrincipals.DefaultPolicy-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Aktualisieren der ServicePrincipals.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory. |
| microsoft.aad.reports/AllEntities/Read | Lesen von Azure AD-Berichten. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="application-developer"></a>Anwendungsentwickler
Erstellen von Anwendungsregistrierungen unabhängig von der Einstellung **Benutzer können Anwendungen registrieren**.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Application/CreateAsOwner | Erstellen von Anwendungen in Azure Active Directory. Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/AppRoleAssignment/CreateAsOwner | Erstellen von AppRoleAssignments in Azure Active Directory. Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/OAuth2PermissionGrant/CreateAsOwner | Erstellen von OAuth2PermissionGrants in Azure Active Directory. Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/ServicePrincipal/CreateAsOwner | Erstellen von ServicePrincipals in Azure Active Directory. Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |

### <a name="billing-administrator"></a>Abrechnungsadministrator
Ausführen von allgemeinen Abrechnungsaufgaben wie der Aktualisierung der Zahlungsinformationen.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Organization/Update | Aktualisieren der Standardeigenschaften für Organisationen in Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/TrustedCAsForPasswordlessAuth | Aktualisieren der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.billing/AllEntities/AllActions | Verwalten sämtlicher Aspekte der Office 365-Abrechnung. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="cloud-application-administrator"></a>Cloudanwendungsadministrator
Erstellen und Verwalten sämtlicher Aspekte von App-Registrierungen und Enterprise-Apps außer App-Proxy.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Application/Create | Erstellen von Anwendungen in Azure Active Directory. |
| microsoft.aad.directory/Application/Delete | Löschen von Anwendungen in Azure Active Directory. |
| microsoft.aad.directory/Application/Update | Aktualisieren der Standardeigenschaften für Anwendungen in Azure Active Directory. |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Aktualisieren der Applications.DefaultPolicy-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Application/Update/Owners | Aktualisieren der Applications.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Create | Erstellen von AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Löschen von AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Aktualisieren der Standardeigenschaften für AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Erstellen von Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Löschen von Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Aktualisieren der Standardeigenschaften für Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Aktualisieren der Policies.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllNoDirectory | Zustimmungserteilung im Namen aller Benutzer für sämtliche Ressourcen mit Ausnahme von Azure Active Directory (Azure AD Graph und Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/Create | Erstellen von ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Delete | Löschen von ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Aktualisieren der Standardeigenschaften für ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Aktualisieren der ServicePrincipals.AppRoleAssignedTo-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Aktualisieren der ServicePrincipals.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Aktualisieren der ServicePrincipals.DefaultPolicy-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Aktualisieren der ServicePrincipals.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory. |
| microsoft.aad.reports/AllEntities/Read | Lesen von Azure AD-Berichten. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="company-administrator"></a>Unternehmensadministrator
Verwalten sämtlicher Aspekte von Azure AD und Microsoft-Diensten, die Azure AD-Identitäten verwenden. In der Microsoft Graph-API, der Azure AD Graph-API und in Azure AD PowerShell wird diese Rolle als „Unternehmensadministrator“ identifiziert. Im [Azure-Portal](https://portal.azure.com)ist dies der „globale Administrator“.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/AdministrativeUnit/AllActions/AllProperties | Erstellen und Löschen von AdministrativeUnits und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/Application/AllActions/AllProperties | Erstellen und Löschen von Anwendungen und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/AllActions/AllProperties | Erstellen und Löschen von AppRoleAssignments und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/CollaborationSpace/AllActions/AllProperties | Erstellen und Löschen von CollaborationSpaces und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/Contact/AllActions/AllProperties | Erstellen und Löschen von Kontakten und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory |
| microsoft.aad.directory/Device/AllActions/AllProperties | Erstellen und Löschen von Geräten und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/AllActions/AllProperties | Erstellen und Löschen von DirectoryRoles und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/DirectoryRoleTemplate/AllActions/AllProperties | Erstellen und Löschen von DirectoryRoleTemplates und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/AllActions/AllProperties | Erstellen und Löschen von DirectorySettings und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/DirectorySettingTemplate/AllActions/AllProperties | Erstellen und Löschen von DirectorySettingTemplates und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/Domain/AllActions/AllProperties | Erstellen und Löschen von Domänen und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/Group/AllActions/AllProperties | Erstellen und Löschen von Gruppen und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/LoginTenantBranding/AllActions/AllProperties | Erstellen und Löschen von LoginTenantBrandings und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/OAuth2PermissionGrant/AllActions/AllProperties | Erstellen und Löschen von OAuth2PermissionGrants und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/Policy/AllActions/AllProperties | Erstellen und Löschen von Richtlinien und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllWithDirectory | Zustimmungserteilung im Namen aller Benutzer für sämtliche Ressourcen einschließlich Azure Active Directory (Azure AD Graph und Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/AllActions/AllProperties | Erstellen und Löschen von ServicePrincipals und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/Organization/AllActions/AllProperties | Erstellen und Löschen von Organisationen und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.directory/User/AllActions/AllProperties | Erstellen und Löschen von Benutzern und Lesen und Aktualisieren aller Eigenschaften in Azure Active Directory. |
| microsoft.aad.aadconnect/AllEntities/AllActions | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren von Standardeigenschaften in microsoft.aad.aadconnect. |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.billing/AllEntities/AllActions | Verwalten sämtlicher Aspekte der Office 365-Abrechnung. |
| microsoft.aad.compliance/AllEntities/AllActions | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren von Standardeigenschaften im Compliance Center. |
| microsoft.aad.directorysync/AllEntities/AllActions | Ausführen sämtlicher Aktionen in Azure AD Connect. |
| microsoft.aad.lockbox/AllEntities/AllActions | Verwalten sämtlicher Aspekte des Lockbox-Diensts. |
| microsoft.aad.privilegedrolemanagement/AllEntities/AllActions | Verwalten sämtlicher Aspekte des Verwaltungsdiensts für privilegierte Rollen. |
| microsoft.aad.reports/AllEntities/AllActions | Lesen und Konfigurieren von Azure AD-Berichten. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.crm/AllEntities/AllActions | Verwalten sämtlicher Aspekte von Dynamics 365. |
| microsoft.exchange/AllEntities/AllActions | Verwalten sämtlicher Aspekte von Exchange Online. |
| microsoft.aad.informationprotection/AllEntities/AllActions | Verwalten sämtlicher Aspekte von Information Protection. |
| microsoft.intune/AllEntities/AllActions | Verwalten sämtlicher Aspekte von Intune. |
| microsoft.powerbi/AllEntities/AllActions | Verwalten sämtlicher Aspekte von Power BI. |
| microsoft.protectioncenter/AllEntities/AllActions | Verwalten von Office 365 Protection Center. |
| microsoft.sharepoint/AllEntities/AllActions | Verwalten von SharePoint Online. |
| microsoft.skypeforbusiness/AllEntities/AllActions | Verwalten von Skype for Business Online. |

### <a name="compliance-administrator"></a>Complianceadministrator
Lesen und Verwalten der Konformitätskonfiguration und der zugehörigen Berichte in Azure AD und Office 365.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.compliance/AllEntities/AllActions | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren von Standardeigenschaften im Compliance Center. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.exchange/Compliance/AllActions | Verwalten der Konformität in Exchange Online. |
| microsoft.sharepoint/Compliance/AllActions | Verwalten der Konformität in SharePoint Online. |
| microsoft.skypeforbusiness/Compliance/AllActions | Verwalten der Konformität in Skype for Business Online. |

### <a name="conditional-access-administrator"></a>Administrator für den bedingten Zugriff
Verwalten von Funktionen zum bedingten Zugriff.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/ConditionalAccessPolicy/Create | Erstellen von ConditionalAccessPolicys in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Delete | Löschen von ConditionalAccessPolicys in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read | Lesen der Standardeigenschaften für ConditionalAccessPolicys in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read/Owners | Lesen der ConditionalAccessPolicys.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read/PolicyAppliedTo | Lesen der ConditionalAccessPolicys.PolicyAppliedTo-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Update | Aktualisieren der Standardeigenschaften für ConditionalAccessPolicys in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Update/Owners | Aktualisieren der ConditionalAccessPolicys.Owners-Eigenschaft in Azure Active Directory. |

### <a name="device-administrators"></a>Geräteadministratoren

Benutzer mit dieser Rolle werden auf allen Windows 10-Geräten, die in Azure Active Directory eingebunden sind, als Administratoren für den lokalen Computer festgelegt. Sie können keine Geräteobjekte in Azure Active Directory verwalten.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

### <a name="directory-readers"></a>Rolle „Verzeichnis lesen“
Lesen von grundlegenden Verzeichnisinformationen. Zum Gewähren des Zugriffs auf Anwendungen.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/AdministrativeUnit/Read | Lesen der Standardeigenschaften für AdministrativeUnits in Azure Active Directory. |
| microsoft.aad.directory/AdministrativeUnit/Read/Members | Lesen der AdministrativeUnits.Members-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Application/Read | Lesen der Standardeigenschaften für Anwendungen in Azure Active Directory. |
| microsoft.aad.directory/Application/Read/Owners | Lesen der Applications.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/CollaborationSpace/Read | Lesen der Standardeigenschaften für CollaborationSpaces in Azure Active Directory. |
| microsoft.aad.directory/CollaborationSpace/Read/Owners | Lesen der CollaborationSpaces.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Contact/Read | Lesen der Standardeigenschaften für Kontakte in Azure Active Directory. |
| microsoft.aad.directory/Contact/Read/MemberOf | Lesen der Contacts.MemberOf-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Device/Read | Lesen der Standardeigenschaften für Geräte in Azure Active Directory. |
| microsoft.aad.directory/Device/Read/MemberOf | Lesen der Devices.MemberOf-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Device/Read/RegisteredOwners | Lesen der Devices.RegisteredOwners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Device/Read/RegisteredUsers | Lesen der Devices.RegisteredUsers-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read | Lesen der Standardeigenschaften für DirectoryRoles in Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read/EligibleMembers | Lesen der DirectoryRoles.EligibleMembers-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read/Members | Lesen der DirectoryRoles.Members-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Read | Lesen der Standardeigenschaften für DirectorySettings in Azure Active Directory. |
| microsoft.aad.directory/DirectorySettingTemplate/Read | Lesen der Standardeigenschaften für DirectorySettingTemplates in Azure Active Directory. |
| microsoft.aad.directory/Domain/Read | Lesen der Standardeigenschaften für Domänen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lesen der Standardeigenschaften für Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/AppRoleAssignments | Lesen der Groups.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/MemberOf | Lesen der Groups.MemberOf-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Members | Lesen der Groups.Members-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Owners | Lesen der Groups.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Settings | Lesen der Groups.Settings-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/OAuth2PermissionGrant/Read | Lesen der Standardeigenschaften für OAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read | Lesen der Standardeigenschaften für ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignedTo | Lesen der ServicePrincipals.AppRoleAssignedTo-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignments | Lesen der ServicePrincipals.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/DefaultPolicy | Lesen der ServicePrincipals.DefaultPolicy-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/MemberOf | Lesen der ServicePrincipals.MemberOf-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OAuth2PermissionGrants | Lesen der ServicePrincipals.OAuth2PermissionGrants-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/Owners | Lesen der ServicePrincipals.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OwnedObjects | Lesen der ServicePrincipals.OwnedObjects-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read | Lesen der Standardeigenschaften für Organisationen in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read | Lesen der Standardeigenschaften für Benutzer in Azure Active Directory. |
| microsoft.aad.directory/User/Read/AppRoleAssignments | Lesen der Users.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/DirectReports | Lesen der Users.DirectReports-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedBy | Lesen der Users.InvitedBy-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedUsers | Lesen der Users.InvitedUsers-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/Manager | Lesen der Users.Manager-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/MemberOf | Lesen der Users.MemberOf-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OAuth2PermissionGrants | Lesen der Users.OAuth2PermissionGrants-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedDevices | Lesen der Users.OwnedDevices-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedObjects | Lesen der Users.OwnedObjects-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/RegisteredDevices | Lesen der Users.RegisteredDevices-Eigenschaft in Azure Active Directory. |

### <a name="directory-synchronization-accounts"></a>Konten zur Verzeichnissynchronisierung
Nur vom Azure AD Connect-Dienst verwendet.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Policy/Create | Erstellen von Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Löschen von Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Read | Lesen der Standardeigenschaften für Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Read/Owners | Lesen der Policies.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Policy/Read/PolicyAppliedTo | Lesen der Policies.PolicyAppliedTo-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Aktualisieren der Standardeigenschaften für Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Aktualisieren der Policies.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Create | Erstellen von ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read | Lesen der Standardeigenschaften für ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignedTo | Lesen der ServicePrincipals.AppRoleAssignedTo-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignments | Lesen der ServicePrincipals.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/DefaultPolicy | Lesen der ServicePrincipals.DefaultPolicy-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/MemberOf | Lesen der ServicePrincipals.MemberOf-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OAuth2PermissionGrants | Lesen der ServicePrincipals.OAuth2PermissionGrants-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/Owners | Lesen der ServicePrincipals.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OwnedObjects | Lesen der ServicePrincipals.OwnedObjects-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Aktualisieren der Standardeigenschaften für ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Aktualisieren der ServicePrincipals.AppRoleAssignedTo-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Aktualisieren der ServicePrincipals.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Aktualisieren der ServicePrincipals.DefaultPolicy-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Aktualisieren der ServicePrincipals.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/DirSync | Aktualisieren der Organizations.DirSync-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directorysync/AllEntities/AllActions | Ausführen sämtlicher Aktionen in Azure AD Connect. |

### <a name="directory-writer"></a>Verzeichnis schreiben
Lesen und Schreiben von grundlegenden Verzeichnisinformationen. Zum Gewähren des Zugriffs auf Anwendungen

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/DirectorySetting/Create | Erstellen von DirectorySettings in Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Delete | Löschen von DirectorySettings in Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Update | Aktualisieren der Standardeigenschaften für DirectorySettings in Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Erstellen von Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Erstellen von Gruppen in Azure Active Directory. Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/Group/Read | Lesen der Standardeigenschaften für Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Aktualisieren der Standardeigenschaften für Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Aktualisieren der Groups.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Aktualisieren der Groups.Members-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Aktualisieren der Groups.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Aktualisieren der Groups.Settings-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/User/Update | Aktualisieren der Standardeigenschaften für Benutzer in Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Aktualisieren der Users.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Aktualisieren der Users.Manager-Eigenschaft in Azure Active Directory. |

### <a name="dynamics-365-service-administrator"></a>Dynamics 365-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Dynamics 365.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.crm/AllEntities/AllActions | Verwalten sämtlicher Aspekte von Dynamics 365. |

### <a name="exchange-service-administrator"></a>Exchange-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Exchange.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.exchange/AllEntities/AllActions | Verwalten sämtlicher Aspekte von Exchange Online. |

### <a name="guest-inviter"></a>Gasteinladender
Einladen von Gastbenutzern unabhängig von der Einstellung **Mitglieder können Gäste einladen**.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der Gastrolle.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/User/InviteGuest | Einladen von Gastbenutzern in Azure Active Directory. |
| microsoft.aad.directory/User/Read | Lesen der Standardeigenschaften für Benutzer in Azure Active Directory. |
| microsoft.aad.directory/User/Read/AppRoleAssignments | Lesen der Users.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/DirectReports | Lesen der Users.DirectReports-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedBy | Lesen der Users.InvitedBy-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedUsers | Lesen der Users.InvitedUsers-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/Manager | Lesen der Users.Manager-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/MemberOf | Lesen der Users.MemberOf-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OAuth2PermissionGrants | Lesen der Users.OAuth2PermissionGrants-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedDevices | Lesen der Users.OwnedDevices-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedObjects | Lesen der Users.OwnedObjects-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Read/RegisteredDevices | Lesen der Users.RegisteredDevices-Eigenschaft in Azure Active Directory. |

### <a name="helpdesk-administrator"></a>Helpdeskadministrator
Zurücksetzen von Kennwörtern für Nicht-Administratoren und Helpdeskadministratoren.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/User/Update/PasswordHelpdeskScope | Aktualisieren von Kennwörtern für eingeschränkte Administratoren und andere Helpdesk-Administratoren in Azure Active Directory. Weitere Informationen finden Sie in der Onlinedokumentation. |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |

### <a name="information-protection-administrator"></a>Information Protection-Administrator
Verwalten sämtlicher Aspekte des Produkts Azure Information Protection.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Group/Read | Lesen der Standardeigenschaften für Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.informationprotection/AllEntities/AllActions | Verwalten sämtlicher Aspekte von Information Protection. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="intune-service-administrator"></a>Intune-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Intune.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Contact/Create | Erstellen von Kontakten in Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Löschen von Kontakten in Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Aktualisieren der Standardeigenschaften für Kontakte in Azure Active Directory. |
| microsoft.aad.directory/Device/Create | Erstellen von Geräten in Azure Active Directory. |
| microsoft.aad.directory/Device/Delete | Löschen von Geräten in Azure Active Directory. |
| microsoft.aad.directory/Device/Update | Aktualisieren der Standardeigenschaften für Geräte in Azure Active Directory. |
| microsoft.aad.directory/Device/Update/RegisteredOwners | Aktualisieren der Devices.RegisteredOwners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Device/Update/RegisteredUsers | Aktualisieren der Devices.RegisteredUsers-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Erstellen von Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Erstellen von Gruppen in Azure Active Directory. Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/Group/Delete | Löschen von Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lesen der Standardeigenschaften für Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/HiddenMembers | Lesen der Groups.HiddenMembers-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Wiederherstellen von Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Aktualisieren der Standardeigenschaften für Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Aktualisieren der Groups.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Aktualisieren der Groups.Members-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Aktualisieren der Groups.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Aktualisieren der Groups.Settings-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Update | Aktualisieren der Standardeigenschaften für Benutzer in Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Aktualisieren der Users.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Aktualisieren der Users.Manager-Eigenschaft in Azure Active Directory. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.intune/AllEntities/AllActions | Verwalten sämtlicher Aspekte von Intune. |

### <a name="lync-service-administrator"></a>Lync-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Skype for Business.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.skypeforbusiness/AllEntities/AllActions | Verwalten von Skype for Business Online. |

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
| microsoft.aad.directory/Group/Read | Lesen der Standardeigenschaften für Gruppen in Azure Active Directory. |
| microsoft.aad.accessmessagecenter/AllEntities/AllActions | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren der Standardeigenschaften im Nachrichtencenter. |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |

### <a name="partner-tier1-support"></a>Partnersupport der Ebene 1
Verwenden Sie diese Rolle nicht – sie ist nicht zur allgemeinen Verwendung vorgesehen.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.directory/Contact/Create | Erstellen von Kontakten in Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Löschen von Kontakten in Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Aktualisieren der Standardeigenschaften für Kontakte in Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Erstellen von Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Erstellen von Gruppen in Azure Active Directory. Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/Group/Read | Lesen der Standardeigenschaften für Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Aktualisieren der Groups.Members-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Aktualisieren der Groups.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Löschen von Benutzern in Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/User/Restore | Wiederherstellen gelöschter Benutzerkonten in Azure Active Directory. |
| microsoft.aad.directory/User/Update | Aktualisieren der Standardeigenschaften für Benutzer in Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Aktualisieren der Users.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Aktualisieren der Users.Manager-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Update/PasswordUserScope | Aktualisieren von Kennwörtern für Nicht-Administratoren in Azure Active Directory. Weitere Informationen finden Sie in der Onlinedokumentation. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="partner-tier2-support"></a>Partnersupport der Ebene 2
Verwenden Sie diese Rolle nicht – sie ist nicht zur allgemeinen Verwendung vorgesehen.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.directory/Contact/Create | Erstellen von Kontakten in Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Löschen von Kontakten in Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Aktualisieren der Standardeigenschaften für Kontakte in Azure Active Directory. |
| microsoft.aad.directory/Domain/AllActions | Erstellen und Löschen von Domänen und Lesen und Aktualisieren der Standardeigenschaften in Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Erstellen von Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Delete | Löschen von Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lesen der Standardeigenschaften für Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Wiederherstellen von Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Aktualisieren der Groups.Members-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Organization/Update | Aktualisieren der Standardeigenschaften für Organisationen in Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/TrustedCAsForPasswordlessAuth | Aktualisieren der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Löschen von Benutzern in Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/User/Restore | Wiederherstellen gelöschter Benutzerkonten in Azure Active Directory. |
| microsoft.aad.directory/User/Update | Aktualisieren der Standardeigenschaften für Benutzer in Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Aktualisieren der Users.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Aktualisieren der Users.Manager-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Password | Aktualisieren der Kennwörter für alle Benutzer in Azure Active Directory. Weitere Informationen finden Sie in der Onlinedokumentation. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="power-bi-service-administrator"></a>Power BI-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Power BI.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.powerbi/AllEntities/AllActions | Verwalten sämtlicher Aspekte von Power BI. |

### <a name="privileged-role-administrator"></a>Administrator für privilegierte Rollen
Verwalten von Rollenzuweisungen in Azure AD.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/DirectoryRole/Update | Aktualisieren der Standardeigenschaften für DirectoryRoles in Azure Active Directory. |
| microsoft.aad.privilegedrolemanagement/AllEntities/AllActions | Verwalten sämtlicher Aspekte des Verwaltungsdiensts für privilegierte Rollen. |

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
| microsoft.aad.reports/AllEntities/Read | Lesen von Azure AD-Berichten. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.usagereports/AllEntities/Read | Lesen von Office 365-Nutzungsberichten. |

### <a name="security-administrator"></a>Sicherheitsadministrator
Lesen von Sicherheitsinformationen und Berichten

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Aktualisieren der Applications.DefaultPolicy-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Erstellen von Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Löschen von Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Aktualisieren der Standardeigenschaften für Richtlinien in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Aktualisieren der Policies.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Aktualisieren der ServicePrincipals.DefaultPolicy-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.privilegedrolemanagement/AllEntities/Read | Lesen sämtlicher Aspekte von Privileged Identity Management. |
| microsoft.protectioncenter/AllEntities/Read | Lesen sämtlicher Aspekte von Office 365 Protection Center. |
| microsoft.protectioncenter/AllEntities/Update | Verwalten von Office 365 Protection Center. |

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
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.privilegedidentitymanagement/AllEntities/Read | Lesen sämtlicher Aspekte von Privileged Identity Management. |
| microsoft.protectioncenter/AllEntities/Read | Lesen sämtlicher Aspekte von Office 365 Protection Center. |

### <a name="service-support-administrator"></a>Dienstunterstützungsadministrator
Lesen von Service Health-Informationen und Verwalten von Supporttickets.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |

### <a name="sharepoint-service-administrator"></a>SharePoint-Dienstadministrator
Verwalten sämtlicher Aspekte des SharePoint-Diensts.

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung weiter oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.sharepoint/AllEntities/AllActions | Verwalten von SharePoint Online. |

### <a name="user-account-administrator"></a>Benutzerkontoadministrator
Verwalten sämtlicher Aspekte von Benutzern und Gruppen

  > [!NOTE]
  > Diese Rolle erbt zusätzliche Berechtigungen von der [Benutzerrolle](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/AppRoleAssignment/Create | Erstellen von AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Löschen von AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Aktualisieren der Standardeigenschaften für AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/Contact/Create | Erstellen von Kontakten in Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Löschen von Kontakten in Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Aktualisieren der Standardeigenschaften für Kontakte in Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Erstellen von Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Erstellen von Gruppen in Azure Active Directory. Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/Group/Delete | Löschen von Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lesen der Standardeigenschaften für Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/HiddenMembers | Lesen der Groups.HiddenMembers-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Wiederherstellen von Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Aktualisieren der Standardeigenschaften für Gruppen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Aktualisieren der Groups.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Aktualisieren der Groups.Members-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Aktualisieren der Groups.Owners-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Aktualisieren der Groups.Settings-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lesen der Organizations.TrustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory. |
| microsoft.aad.directory/User/Create | Erstellen von Benutzern in Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Löschen von Benutzern in Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/User/Restore | Wiederherstellen gelöschter Benutzerkonten in Azure Active Directory. |
| microsoft.aad.directory/User/Update | Aktualisieren der Standardeigenschaften für Benutzer in Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Aktualisieren der Users.AppRoleAssignments-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Aktualisieren der Users.Manager-Eigenschaft in Azure Active Directory. |
| microsoft.aad.directory/User/Update/PasswordUserAcctAdminScope | Aktualisieren von Kennwörtern für eingeschränkte Administratoren, Helpdesk-Administratoren und andere Benutzerkontoadministratoren in Azure Active Directory. Weitere Informationen finden Sie in der Onlinedokumentation. |
| microsoft.aad.accessservice/AllEntities/AllActions | Erstellen und Löschen sämtlicher Ressourcen und Lesen und Aktualisieren der Standardeigenschaften in Azure Access Control. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lesen und Konfigurieren des Office 365-Dienststatus. |

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Ändern von Administratoren für ein Azure-Abonnement finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen](../../billing/billing-add-change-azure-subscription-administrator.md)
* Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Weitere Informationen zur Beziehung zwischen Azure Active Directory und Ihrem Azure-Abonnement finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
