---
title: Berechtigungen der Administratorrolle in Azure Active Directory | Microsoft-Dokumentation
description: Eine Administratorrolle kann zum Hinzufügen von Benutzern, zum Zuweisen von Administratorrollen, zum Zurücksetzen von Benutzerkennwörtern sowie zum Verwalten von Benutzerlizenzen oder Domänen verwendet werden.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 10/26/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 5c880ca15eea6ccf77f93f74c9e1ca41f7fc01ee
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276719"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Berechtigungen der Administratorrolle in Azure Active Directory

Mithilfe von Azure Active Directory (Azure AD) können Sie verschiedene Administratoren bestimmen, um unterschiedliche Funktionen zu erfüllen. Administratoren können im Azure AD-Portal für Aufgaben wie das Hinzufügen oder Ändern von Benutzern, das Zuweisen von Administratorrollen, das Zurücksetzen von Benutzerkennwörtern, das Verwalten von Benutzerlizenzen oder das Verwalten von Domänennamen festgelegt werden.

Der globale Administrator hat Zugriff auf alle administrativen Funktionen. Der Person, die sich für ein Azure-Abonnement registriert, wird standardmäßig die globale Administratorrolle für das Verzeichnis zugewiesen. Nur globale Administratoren und Administratoren für privilegierte Rollen können Administratorrollen delegieren.

## <a name="assign-or-remove-administrator-roles"></a>Zuweisen oder Entfernen von Administratorrollen

Anweisungen zum Zuweisen von Administratorrollen zu einem Benutzer in Azure Active Directory finden Sie unter [Anzeigen und Zuweisen von Administratorrollen in Azure Active Directory](directory-manage-roles-portal.md).

## <a name="available-roles"></a>Verfügbare Rollen

Die folgenden Administratorrollen sind verfügbar:

* **[Anwendungsadministrator:](#application-administrator)** Benutzer mit dieser Rolle können alle Aspekte von Unternehmensanwendungen, Anwendungsregistrierungen und Anwendungsproxyeinstellungen erstellen und verwalten. Diese Rolle ermöglicht auch die Zustimmung zu delegierten Berechtigungen und Anwendungsberechtigungen mit Ausnahme von Microsoft Graph und Azure AD Graph. Mitglieder dieser Rolle werden bei der Erstellung neuer Anwendungsregistrierungen oder Unternehmensanwendungen nicht als Besitzer hinzugefügt.

  <b>Wichtig</b>: Diese Rolle ermöglicht die Verwaltung von Anmeldeinformationen für Anwendungen. Benutzer, die dieser Rolle zugewiesen sind, können einer Anwendung Anmeldeinformationen hinzufügen und diese Anmeldeinformationen verwenden, um die Anwendung zu imitieren. Wenn der Identität der Anwendung der Zugriff auf Azure Active Directory gewährt wurde, z.B. die Berechtigung, Benutzer oder andere Objekte zu erstellen oder zu aktualisieren, kann ein dieser Rolle zugewiesener Benutzer diese Aktionen ausführen, während er die Identität der Anwendung annimmt. Diese Fähigkeit, die Identität der Anwendung anzunehmen, bedeutet ggf. eine Rechteerweiterung im Vergleich zu den Rollenzuweisungen des Benutzers in Azure AD. Beachten Sie, dass das Zuweisen eines Benutzers zur Anwendungsadministratorrolle ihm die Möglichkeit gibt, die Identität einer Anwendung anzunehmen.

* **[Anwendungsentwickler:](#application-developer)** Benutzer mit dieser Rolle können Anwendungsregistrierungen erstellen, wenn die Einstellung „Benutzer können Anwendungen registrieren“ auf „Nein“ festgelegt ist. Diese Rolle erlaubt es den Mitgliedern auch, in ihrem eigenen Namen zuzustimmen, wenn die Einstellung „Benutzer können Apps zustimmen, die in ihrem Namen auf Unternehmensdaten zugreifen“ auf „Nein“ festgelegt ist. Mitglieder dieser Rolle werden bei der Erstellung neuer Anwendungsregistrierungen oder Unternehmensanwendungen als Besitzer hinzugefügt.

* **[Rechnungsadministrator:](#billing-administrator)** Erledigt Käufe, verwaltet Abonnements, verwaltet Supporttickets und überwacht die Dienstintegrität.

* **[Cloudanwendungsadministrator:](#cloud-application-administrator)** Benutzer in dieser Rolle haben die gleichen Berechtigungen wie die Rolle des Anwendungsadministrators, mit Ausnahme der Möglichkeit, den Anwendungsproxy zu verwalten. Diese Rolle ermöglicht die Erstellung und Verwaltung aller Aspekte von Unternehmensanwendungen und Anwendungsregistrierungen. Diese Rolle ermöglicht auch die Zustimmung zu delegierten Berechtigungen und Anwendungsberechtigungen mit Ausnahme von Microsoft Graph und Azure AD Graph. Mitglieder dieser Rolle werden bei der Erstellung neuer Anwendungsregistrierungen oder Unternehmensanwendungen nicht als Besitzer hinzugefügt.

  <b>Wichtig</b>: Diese Rolle ermöglicht die Verwaltung von Anmeldeinformationen für Anwendungen. Benutzer, die dieser Rolle zugewiesen sind, können einer Anwendung Anmeldeinformationen hinzufügen und diese Anmeldeinformationen verwenden, um die Anwendung zu imitieren. Wenn der Identität der Anwendung der Zugriff auf Azure Active Directory gewährt wurde, z.B. die Berechtigung, Benutzer oder andere Objekte zu erstellen oder zu aktualisieren, kann ein dieser Rolle zugewiesener Benutzer diese Aktionen ausführen, während er die Identität der Anwendung annimmt. Diese Fähigkeit, die Identität der Anwendung anzunehmen, bedeutet ggf. eine Rechteerweiterung im Vergleich zu den Rollenzuweisungen des Benutzers in Azure AD. Beachten Sie, dass das Zuweisen eines Benutzers zur Cloudanwendungsadministrator-Rolle ihm die Möglichkeit gibt, die Identität einer Anwendung anzunehmen.

* **[Cloudgeräteadministrator](#cloud-device-administrator)**: Benutzer in dieser Rolle können Geräte in Azure AD aktivieren, deaktivieren und löschen sowie Windows 10-BitLocker-Schlüssel (falls vorhanden) im Azure-Portal lesen. Die Rolle gewährt keine Berechtigungen für die Verwaltung anderer Eigenschaften auf dem Gerät.

* **[Complianceadministrator:](#compliance-administrator)** Benutzer mit dieser Rolle verfügen über Verwaltungsberechtigungen in Office 365 Security & Compliance Center und Exchange Admin Center. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrator für den bedingten Zugriff:](#conditional-access-administrator)** Benutzer mit dieser Rolle haben die Möglichkeit, Azure Active Directory-Einstellungen für den bedingten Zugriff zu verwalten.
  > [!NOTE]
  > Um die Exchange ActiveSync-Richtlinie für bedingten Zugriff in Azure bereitzustellen, muss der Benutzer auch ein globaler Administrator sein.
  
* **[Geräteadministratoren](#device-administrators)**: Diese Rolle kann nur als zusätzlicher lokaler Administrator in den [Geräteeinstellungen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/) zugewiesen werden. Benutzer mit dieser Rolle werden auf allen Windows 10-Geräten, die in Azure Active Directory eingebunden sind, als Administratoren für den lokalen Computer festgelegt. Sie haben nicht die Möglichkeit zum Verwalten von Geräteobjekten in Azure Active Directory. 

* **[Verzeichnis lesen:](#directory-readers)** Dies ist eine Legacyrolle, die Anwendungen ohne Unterstützung für das [Consent Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md) zugewiesen wird. Diese Rolle sollte keinem Benutzer zugewiesen werden.

* **[Konten für die Verzeichnissynchronisierung:](#directory-synchronization-accounts)** Verwenden Sie diese Rolle nicht. Diese Rolle wird automatisch dem Azure AD Connect-Dienst zugewiesen und ist weder für eine andere Verwendung vorgesehen, noch wird eine andere Verwendung unterstützt.

* **[Verzeichnis schreiben:](#directory-writers)** Dies ist eine Legacyrolle, die Anwendungen ohne Unterstützung für das [Consent Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md) zugewiesen wird. Diese Rolle sollte keinem Benutzer zugewiesen werden.

* **[Dynamics 365-Administrator/CRM-Administrator:](#dynamics-365-administrator)** Benutzer mit dieser Rolle verfügen über globale Berechtigungen in Microsoft Dynamics 365 Online, sofern der Dienst vorhanden ist, und können Supporttickets verwalten und die Dienstintegrität überwachen. Weitere Informationen finden Sie unter [Verwenden der Dienstadministratorrolle zum Verwalten Ihres Mandanten](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).
  > [!NOTE] 
  > In der Microsoft Graph-API, der Azure AD Graph-API und in Azure AD PowerShell wird diese Rolle als „Dynamics 365-Dienstadministrator“ bezeichnet. Im Azure-Portal lautet sie „Dynamics 365-Administrator“.

* **[Exchange-Administrator:](#exchange-administrator)** Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft Exchange Online, wenn der Dienst vorhanden ist. Außerdem haben sie die Möglichkeit, alle Office 365-Gruppen zu erstellen und zu verwalten, Supporttickets zu verwalten und die Dienstintegrität zu überwachen. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und in Azure AD PowerShell wird diese Rolle als „Exchange-Dienstadministrator“ bezeichnet. Im Azure-Portal lautet sie „Exchange-Administrator“.

* **[Globaler Administrator/Unternehmensadministrator:](#company-administrator)** Benutzer mit dieser Rolle haben Zugriff auf alle Verwaltungsfeatures in Azure Active Directory sowie Dienste, die Azure Active Directory-Identitäten nutzen, z.B. Exchange Online, SharePoint Online oder Skype for Business Online. Die Person, die die Anmeldung für den Azure Active Directory-Mandanten vornimmt, wird ein globaler Administrator. Nur globale Administratoren können weitere Administratorrollen zuweisen. In Ihrem Unternehmen können mehrere globale Administratoren vorhanden sein. Globale Administratoren können das Kennwort für alle Benutzer und alle anderen Administratoren zurücksetzen.

  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und in Azure AD PowerShell wird diese Rolle als „Unternehmensadministrator“ identifiziert. Im [Azure-Portal](https://portal.azure.com)ist dies der „globale Administrator“.
  >
  >

* **[Gasteinladender:](#guest-inviter)** Benutzer mit dieser Rolle können Einladungen für Azure Active Directory B2B-Gastbenutzer verwalten, wenn die Benutzereinstellung **Mitglieder können einladen** auf „Nein“ festgelegt ist. Weitere Informationen zur B2B-Zusammenarbeit finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Es sind keine anderen Berechtigungen eingeschlossen.

* **[Information Protection-Administrator:](#information-protection-administrator)** Benutzer mit dieser Rolle verfügen über alle Berechtigungen für den Azure Information Protection-Dienst. Sie können Bezeichnungen für die Azure Information Protection-Richtlinie konfigurieren, Schutzvorlagen verwalten und den Schutz aktivieren. Diese Rolle gewährt keine Berechtigungen für Identity Protection Center, Privileged Identity Management, Monitor Office 365 Service Health oder Office 365 Security & Compliance Center.

* **[Intune-Administrator:](#intune-administrator)** Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft Intune Online, wenn der Dienst vorhanden ist. Darüber hinaus beinhaltet diese Rolle die Möglichkeit, Benutzer und Geräte zum Zuordnen von Richtlinien zu verwalten sowie Gruppen zu erstellen und zu verwalten. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung mit Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control).
  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und in Azure AD PowerShell wird diese Rolle als „Intune-Dienstadministrator“ bezeichnet. Im Azure-Portal lautet sie „Intune-Administrator“.

* **[Lizenzadministrator:](#license-administrator)** Benutzer mit dieser Rolle können Lizenzzuweisungen für Benutzer und Gruppen (unter Verwendung der gruppenbasierten Lizenzierung) hinzufügen, entfernen und aktualisieren sowie den Verwendungsstandort für Benutzer verwalten. Mit dieser Rolle ist es nicht möglich, Abonnements zu erwerben oder zu verwalten, Gruppen zu erstellen oder zu verwalten oder Benutzer zu erstellen oder zu verwalten (mit Ausnahme des Verwendungsstandorts).

* **[Nachrichtencenter-Leser:](#message-center-reader)** Benutzer mit dieser Rolle können Benachrichtigungen und empfohlene Integritätsupdates für ihre Organisation und die konfigurierten Dienste wie Exchange, Intune und Microsoft Teams im [Office 365-Nachrichtencenter](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) überwachen. Nachrichtencenter-Leser erhalten eine wöchentliche E-Mail-Übersicht der Beiträge und Updates und können Beiträge in Office 365 teilen. In Azure AD haben Benutzer mit dieser Rolle nur schreibgeschützten Zugriff auf Azure AD-Dienste wie Benutzer und Gruppen. 

* **[Partnersupport der Ebene 1:](#partner-tier1-support)** Verwenden Sie diese Rolle nicht. Diese Rolle wurde als veraltet markiert und wird aus Azure AD entfernt. Diese Rolle ist für einige wenige Wiederverkaufspartner von Microsoft und nicht zur allgemeinen Verwendung vorgesehen.

* **[Partnersupport der Ebene 2:](#partner-tier2-support)** Verwenden Sie diese Rolle nicht. Diese Rolle wurde als veraltet markiert und wird aus Azure AD entfernt. Diese Rolle ist für einige wenige Wiederverkaufspartner von Microsoft und nicht zur allgemeinen Verwendung vorgesehen.

* **[Kennwortadministrator/Helpdeskadministrator:](#helpdesk-administrator)** Benutzer mit dieser Rolle können Kennwörter ändern, Aktualisierungstoken für ungültig erklären, Dienstanforderungen verwalten und die Integrität des Diensts überwachen. Wenn ein Aktualisierungstoken für ungültig erklärt wird, muss sich der Benutzer erneut anmelden. Helpdeskadministratoren können Kennwörter zurücksetzen und Token anderer Benutzer annullieren, die keine Administratoren oder nur Mitglieder der folgenden Rollen sind:
  * Rolle „Verzeichnis lesen“
  * Gasteinladender
  * Helpdeskadministrator
  * Nachrichtencenter-Leser
  * Meldet Reader
  
  <b>Wichtig</b>: Benutzer mit dieser Rolle können Kennwörter für Benutzer ändern, die Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen innerhalb und außerhalb von Azure Active Directory haben. Benutzer, die Kennwörter ändern können, können ggf. auch die Identität und die Berechtigungen des betreffenden Benutzers annehmen. Beispiel: 
  * Besitzer von Anwendungsregistrierungen und Unternehmensanwendungen, die Anmeldeinformationen von Apps verwalten können, die sie besitzen. Diese Apps können über höhere Berechtigungen in Azure AD und in anderen Diensten verfügen, die Helpdeskadministratoren nicht gewährt werden. So kann ein Helpdeskadministrator die Identität eines Anwendungsbesitzers annehmen und dann die Identität einer privilegierten Anwendung durch Aktualisieren der Anmeldeinformationen für die Anwendung annehmen.
  * Besitzer von Azure-Abonnements, die ggf. auf vertrauliche oder private Informationen bzw. kritische Konfigurationen in Azure zugreifen können.
  * Besitzer von Sicherheitsgruppen und Office 365-Gruppen, die die Gruppenmitgliedschaft verwalten können. Diese Gruppen können Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen in Azure AD und in anderen Diensten gewähren.
  * Administratoren in anderen Diensten außerhalb von Azure AD wie Exchange Online, Office Security and Compliance Center und Personalwesen.
  * Nichtadministratoren wie Führungskräfte, Rechtsberater und Mitarbeiter der Personalabteilung mit Zugriff auf vertrauliche oder private Informationen.

  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und Azure AD PowerShell wird diese Rolle als „Helpdeskadministrator“ bezeichnet. Im [Azure-Portal](https://portal.azure.com/) lautet diese „Kennwortadministrator“.
  >
  
* **[Power BI-Administrator:](#power-bi-administrator)** Benutzer mit dieser Rolle verfügen über globale Berechtigungen in Microsoft Power BI, wenn der Dienst vorhanden ist, sowie die Möglichkeit, Supporttickets zu verwalten und die Dienstintegrität zu überwachen. Weitere Informationen finden Sie unter [Grundlegendes zur Power BI-Administratorrolle](https://docs.microsoft.com/power-bi/service-admin-role).
  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und Azure AD PowerShell wird diese Rolle als „Power BI-Dienstadministrator“ bezeichnet. Im Azure-Portal lautet sie „Power BI-Administrator“.

* **[Administrator für privilegierte Rollen:](#privileged-role-administrator)** Benutzer mit dieser Rolle können Rollenzuweisungen in Azure Active Directory und in Azure AD Privileged Identity Management vornehmen. Überdies ermöglicht diese Rolle die vollumfängliche Verwaltung von Privileged Identity Management.

  <b>Wichtig</b>: Diese Rolle ermöglicht die Verwaltung der Mitgliedschaft in allen Azure AD-Rollen, einschließlich der globalen Administratorrolle. Diese Rolle umfasst keine anderen privilegierten Funktionen in Azure AD wie das Erstellen oder Aktualisieren von Benutzern. Benutzer, die dieser Rolle zugewiesen sind, können sich selbst oder anderen jedoch zusätzliche Berechtigungen gewähren, indem sie zusätzliche Rollen zuweisen.

* **[Berichtsleser:](#reports-reader)** Benutzer mit dieser Rolle können Verwendungsberichtsdaten und das Berichtsdashboard in Office 365 Admin Center sowie das Anpassungskontextpaket in Power BI anzeigen. Darüber hinaus stellt die Rolle Zugriff auf Anmeldeberichte und -aktivitäten in Azure AD und von der Microsoft Graph-Berichterstellungs-API zurückgegebene Daten zur Verfügung. Ein Benutzer, dem die Rolle „Meldet Reader“ zugewiesen ist, kann nur auf relevante Nutzungs- und Anpassungsmetriken zugreifen. Sie verfügen nicht über Administratorrechte, um Einstellungen zu konfigurieren oder auf produktspezifische Verwaltungskonsolen wie das Exchange Admin Center zuzugreifen. 

* **[Sicherheitsadministrator:](#security-administrator)** Benutzer mit dieser Rolle verfügen über alle Leseberechtigungen der Rolle „Benutzer mit Leseberechtigung für Sicherheitsfunktionen“ sowie die Möglichkeit, die Konfiguration für sicherheitsrelevante Dienste zu verwalten (z.B. Azure Active Directory Identity Protection, Azure Information Protection oder Office 365 Security & Compliance Center). Weitere Informationen zu Office 365-Berechtigungen finden Sie unter [Berechtigungen im Office 365 Security & Compliance Center](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
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

* **[SharePoint-Administrator:](#sharepoint-administrator)** Benutzer mit dieser Rolle verfügen über globale Berechtigungen in Microsoft SharePoint Online, wenn der Dienst vorhanden ist, sowie die Möglichkeit, Office 365-Gruppen zu erstellen und zu verwalten, Supporttickets zu verwalten und die Dienstintegrität zu überwachen. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und Azure AD PowerShell wird diese Rolle als „SharePoint-Dienstadministrator“ bezeichnet. Im Azure-Portal lautet sie „SharePoint-Administrator“.

* **[Skype for Business-/Lync-Administrator:](#skype-for-business-administrator)** Benutzer mit dieser Rolle verfügen über globale Berechtigungen in Microsoft Skype for Business, wenn der Dienst vorhanden ist, sowie die Berechtigung zur Verwaltung von Skype-spezifischen Benutzerattributen in Azure Active Directory. Darüber hinaus bietet diese Rolle die Möglichkeit, Supporttickets zu verwalten und die Dienstintegrität zu überwachen, sowie auf Teams und Skype for Business Admin Center zuzugreifen. Das Konto muss auch für Teams lizenziert sein, andernfalls kann es keine PowerShell-Cmdlets von Teams ausführen. Weitere Informationen dazu finden Sie unter [Skype for Business Online-Administrator](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) und Informationen zur Teams-Lizenzierung unter [Add-On-Lizenzierung für Skype for Business und Microsoft Teams](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing).

  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und Azure AD PowerShell wird diese Rolle als „Lync-Dienstadministrator“ bezeichnet. Im [Azure-Portal](https://portal.azure.com/) lautet sie „Skype for Business-Administrator“.

* **[Teams-Kommunikationsadministrator](#teams-communications-administrator)**: Benutzer in dieser Rolle können Aspekte der Microsoft Teams-Workload im Zusammenhang mit Sprache und Telefonie verwalten. Dazu gehören die Verwaltungstools für die Telefonnummernzuweisung, Sprach- und Besprechungsrichtlinien und der uneingeschränkte Zugriff auf das Toolset zur Anrufanalyse.

* **[Teams-Kommunikationssupporttechniker](#teams-communications-support-engineer)**: Benutzer in dieser Rolle können Kommunikationsprobleme in Microsoft Teams und Skype for Business mithilfe der Tools zur Problembehandlung für Benutzeraufrufe im Admin Center von Microsoft Teams und Skype for Business behandeln. Benutzer in dieser Rolle können vollständige Anrufdatensatzinformationen für alle Teilnehmer anzeigen.

* **[Teams-Kommunikationssupportspezialist](#teams-communications-support-specialist)**: Benutzer in dieser Rolle können Kommunikationsprobleme in Microsoft Teams und Skype for Business mithilfe der Tools zur Problembehandlung für Benutzeraufrufe im Admin Center von Microsoft Teams und Skype for Business behandeln. Benutzer in dieser Rolle können Benutzerdetails im Anruf nur für den bestimmten Benutzer anzeigen, nach dem sie gesucht haben.

* **[Teams-Administrator:](#teams-administrator)** Benutzer mit dieser Rolle können alle Aspekte der Microsoft Teams-Workload über das Admin Center von Microsoft Teams und Skype for Business und die entsprechenden PowerShell-Module verwalten. Dazu zählen unter anderem alle Verwaltungstools im Zusammenhang mit Telefonie, Messaging, Besprechungen und den Teams selbst. Außerdem bietet diese Rolle die Möglichkeit, alle Office 365-Gruppen zu erstellen und zu verwalten, Supporttickets zu verwalten und die Dienstintegrität zu überwachen.
  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und Azure AD PowerShell wird diese Rolle als „Teams-Dienstadministrator“ bezeichnet. Im Azure-Portal lautet sie „Teams-Administrator“.

* **[Benutzerkontoadministrator:](#user-account-administrator)** Benutzer mit dieser Rolle können Benutzer erstellen und verwalten. Dabei unterliegen sie einigen Einschränkungen (s. unten). Außerdem können Benutzer mit dieser Rolle Gruppen erstellen und verwalten. Zudem haben sie die Möglichkeit, Benutzeransichten und Supporttickets zu verwalten sowie die Dienstintegrität zu überwachen.

  | | |
  | --- | --- |
  |Allgemeine Berechtigungen|<p>Erstellen von Benutzern und Gruppen</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Verwalten von Office-Supporttickets|
  |<p>Für alle Benutzer einschließlich aller Administratoren</p>|<p>Verwalten von Lizenzen</p><p>Verwalten aller Benutzereigenschaften mit Ausnahme des Benutzerprinzipalnamens</p>
  |Nur für Benutzer, die keine Administratoren sind und keine der folgenden eingeschränkten Administratorrollen haben:<ul><li>Rolle „Verzeichnis lesen“<li>Gasteinladender<li>Helpdeskadministrator<li>Nachrichtencenter-Leser<li>Meldet Reader<li>Benutzerkontoadministrator|<p>Löschen und Wiederherstellen</p><p>Deaktivieren und Aktivieren</p><p>Annullieren von Aktualisierungstoken</p><p>Verwalten aller Benutzereigenschaften einschließlich des Benutzerprinzipalnamens</p><p>Kennwort zurücksetzen</p><p>Aktualisieren von Geräteschlüsseln (FIDO)</p>
  
  <b>Wichtig</b>: Benutzer mit dieser Rolle können Kennwörter für Benutzer ändern, die Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen innerhalb und außerhalb von Azure Active Directory haben. Benutzer, die Kennwörter ändern können, können ggf. auch die Identität und die Berechtigungen des betreffenden Benutzers annehmen. Beispiel: 
  * Besitzer von Anwendungsregistrierungen und Unternehmensanwendungen, die Anmeldeinformationen von Apps verwalten können, die sie besitzen. Diese Apps können über höhere Berechtigungen in Azure AD und in anderen Diensten verfügen, die Benutzeradministratoren nicht gewährt werden. So kann ein Benutzeradministrator die Identität eines Anwendungsbesitzers annehmen und dann die Identität einer privilegierten Anwendung durch Aktualisieren der Anmeldeinformationen für die Anwendung annehmen.
  * Besitzer von Azure-Abonnements, die ggf. auf vertrauliche oder private Informationen bzw. kritische Konfigurationen in Azure zugreifen können.
  * Besitzer von Sicherheitsgruppen und Office 365-Gruppen, die die Gruppenmitgliedschaft verwalten können. Diese Gruppen können Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen in Azure AD und in anderen Diensten gewähren.
  * Administratoren in anderen Diensten außerhalb von Azure AD wie Exchange Online, Office Security and Compliance Center und Personalwesen.
  * Nichtadministratoren wie Führungskräfte, Rechtsberater und Mitarbeiter der Personalabteilung mit Zugriff auf vertrauliche oder private Informationen.

## <a name="role-permissions"></a>Rollenberechtigungen
In den folgenden Tabellen werden die spezifischen Berechtigungen der einzelnen Rollen in Azure Active Directory beschrieben. Einige Rollen können zusätzliche Berechtigungen in Microsoft-Diensten außerhalb von Azure Active Directory aufweisen.

### <a name="application-administrator"></a>Anwendungsadministrator
Kann alle Aspekte von App-Registrierungen und Enterprise-Apps erstellen und verwalten.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | Aktualisieren der applications.audience-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/authentication/update | Aktualisieren der applications.authentication-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/basic/update | Aktualisieren der Basiseigenschaften für Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/create | Erstellen von Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/credentials/update | Aktualisieren der applications.credentials-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/delete | Löschen von Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/owners/update | Aktualisieren der applications.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/permissions/update | Aktualisieren der applications.permissions-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/policies/update | Aktualisieren der applications.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/create | Erstellen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/read | Lesen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/update | Aktualisieren von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/delete | Löschen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Aktualisieren der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/create | Erstellen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Löschen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Aktualisieren der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/basic/update | Aktualisieren der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/create | Erstellen von servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/delete | Löschen von servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Aktualisieren der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/update | Aktualisieren der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualisieren der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.reports/applicationAuditLogs/read | Lesen von applicationAuditLogs in Azure AD-Berichten |
| microsoft.aad.reports/applicationSignInReports/read | Lesen von applicationSignInReports in Azure AD-Berichten |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="application-developer"></a>Anwendungsentwickler
Erstellen von Anwendungsregistrierungen unabhängig von der Einstellung „Benutzer können Anwendungen registrieren“.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Erstellen von Anwendungen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Erstellen von appRoleAssignments in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Erstellen von oAuth2PermissionGrants in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Erstellen von servicePrincipals in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |

### <a name="billing-administrator"></a>Abrechnungsadministrator
Ausführen von allgemeinen Abrechnungsaufgaben wie der Aktualisierung der Zahlungsinformationen.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/organization/basic/update | Aktualisieren der Basiseigenschaften für die Organisation in Azure Active Directory |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Aktualisieren der organizations.trustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.commerce.billing/allEntities/allTasks | Verwalten sämtlicher Aspekte der Office 365-Abrechnung. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="desktop-analytics-administrator"></a>Desktop Analytics-Administrator
Dieser Administrator kann auf Desktop Analytics-Verwaltungstools und -Dienste einschließlich Intune zugreifen und diese verwalten.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Verwalten aller Aspekte von Desktop Analytics. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="cloud-application-administrator"></a>Cloudanwendungsadministrator
Erstellen und Verwalten sämtlicher Aspekte von App-Registrierungen und Enterprise-Apps außer App-Proxy.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | Aktualisieren der applications.audience-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/authentication/update | Aktualisieren der applications.authentication-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/basic/update | Aktualisieren der Basiseigenschaften für Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/create | Erstellen von Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/credentials/update | Aktualisieren der applications.credentials-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/delete | Löschen von Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/owners/update | Aktualisieren der applications.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/permissions/update | Aktualisieren der applications.permissions-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/policies/update | Aktualisieren der applications.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/create | Erstellen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/update | Aktualisieren von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/delete | Löschen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/create | Erstellen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Aktualisieren der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Löschen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Aktualisieren der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lesen der policies.applicationConfiguration-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Aktualisieren der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/basic/update | Aktualisieren der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/create | Erstellen von servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/delete | Löschen von servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/update | Aktualisieren der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualisieren der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.reports/applicationAuditLogs/read | Lesen von applicationAuditLogs in Azure AD-Berichten |
| microsoft.aad.reports/applicationSignInReports/read | Lesen von applicationSignInReports in Azure AD-Berichten |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="cloud-device-administrator"></a>Cloudgeräteadministrator
Vollzugriff zum Verwalten von Geräten in Azure AD.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/devices/delete | Löschen von Geräten in Azure Active Directory |
| microsoft.aad.directory/devices/disable | Deaktivieren von Geräten in Azure Active Directory |
| microsoft.aad.directory/devices/enable | Aktivieren von Geräten in Azure Active Directory |
| microsoft.aad.reports/applicationAuditLogs/read | Lesen von applicationAuditLogs in Azure AD-Berichten |
| microsoft.aad.reports/applicationSignInReports/read | Lesen von applicationSignInReports in Azure AD-Berichten |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |

### <a name="company-administrator"></a>Unternehmensadministrator
Verwalten sämtlicher Aspekte von Azure AD und Microsoft-Diensten, die Azure AD-Identitäten verwenden.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
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
| microsoft.aad.reports/applicationAuditLogs/read | Lesen von applicationAuditLogs in Azure AD-Berichten |
| microsoft.aad.reports/applicationSignInReports/read | Lesen von applicationSignInReports in Azure AD-Berichten |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.informationProtection/allEntities/allTasks | Verwalten sämtlicher Aspekte von Azure Information Protection |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.commerce.billing/allEntities/allTasks | Verwalten sämtlicher Aspekte der Office 365-Abrechnung. |
| microsoft.intune/allEntities/allTasks | Verwalten sämtlicher Aspekte von Intune. |
| microsoft.office365.complianceManager/allEntities/allTasks | Verwalten sämtlicher Aspekte von Office 365 Compliance-Manager |
| microsoft.office365.exchange/allEntities/allTasks | Verwalten sämtlicher Aspekte von Exchange Online. |
| microsoft.office365.lockbox/allEntities/allTasks | Verwalten sämtlicher Aspekte der Office 365 Kunden-Lockbox |
| microsoft.office365.messageCenter/messages/read | Lesen von Nachrichten in microsoft.office365.messageCenter |
| microsoft.office365.messageCenter/securityMessages/read | Lesen von securityMessages in microsoft.office365.messageCenter |
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
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
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

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/basic/read | Lesen der policies.conditionalAccess-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/basic/update | Aktualisieren der policies.conditionalAccess-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/create | Erstellen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/delete | Löschen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Lesen der policies.conditionalAccess-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Aktualisieren der policies.conditionalAccess-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Lesen der policies.conditionalAccess-Eigenschaft in Azure Active Directory |

### <a name="crm-service-administrator"></a>CRM-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Dynamics 365.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
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
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.office365.lockbox/allEntities/allTasks | Verwalten sämtlicher Aspekte der Office 365 Kunden-Lockbox |

### <a name="device-administrators"></a>Geräteadministratoren
Mitglieder dieser Rolle werden der lokalen Administratorgruppe auf in Azure AD eingebundenen Geräten hinzugefügt.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | Lesen der Basiseigenschaften für groupSettings in Azure Active Directory |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Lesen der Basiseigenschaften für groupSettingTemplates in Azure Active Directory |

### <a name="directory-readers"></a>Rolle „Verzeichnis lesen“
Lesen von grundlegenden Verzeichnisinformationen. Die Rolle gewährt Zugriff auf Anwendungen und ist nicht für Benutzer vorgesehen.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/basic/read | Lesen der Basiseigenschaften für administrativeUnits in Azure Active Directory |
| microsoft.aad.directory/administrativeUnits/members/read | Lesen der administrativeUnits.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/audience/read | Lesen der applications.audience-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/authentication/read | Lesen der applications.authentication-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/basic/read | Lesen der Basiseigenschaften für Anwendungen in Azure Active Directory |
| microsoft.aad.directory/applications/credentials/read | Lesen der applications.credentials-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/owners/read | Lesen der applications.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/permissions/read | Lesen der applications.permissions-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/applications/policies/read | Lesen der applications.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/contacts/basic/read | Lesen der Basiseigenschaften für Kontakte in Azure Active Directory |
| microsoft.aad.directory/contacts/memberOf/read | Lesen der contacts.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/contracts/basic/read | Lesen der Basiseigenschaften für Verträge in Azure Active Directory |
| microsoft.aad.directory/devices/basic/read | Lesen der Basiseigenschaften für Geräte in Azure Active Directory |
| microsoft.aad.directory/devices/memberOf/read | Lesen der devices.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/devices/registeredOwners/read | Lesen der devices.registeredOwners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/devices/registeredUsers/read | Lesen der devices.registeredUsers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/directoryRoles/basic/read | Lesen der Basiseigenschaften für directoryRoles in Azure Active Directory |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Lesen der directoryRoles.eligibleMembers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/directoryRoles/members/read | Lesen der directoryRoles.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/domains/basic/read | Lesen der Basiseigenschaften für Domänen in Azure Active Directory |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lesen der groups.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/basic/read | Lesen der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/memberOf/read | Lesen der groups.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/members/read | Lesen der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/read | Lesen der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/settings/read | Lesen der groups.settings-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groupSettings/basic/read | Lesen der Basiseigenschaften für groupSettings in Azure Active Directory |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Lesen der Basiseigenschaften für groupSettingTemplates in Azure Active Directory |
| microsoft.aad.directory/oAuth2PermissionGrants/basic/read | Lesen der Basiseigenschaften für oAuth2PermissionGrants in Azure Active Directory |
| microsoft.aad.directory/organization/basic/read | Lesen der Basiseigenschaften für die Organisation in Azure Active Directory |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Lesen der organizations.trustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/roleAssignments/basic/read | Lesen der Basiseigenschaften für roleAssignments in Azure Active Directory |
| microsoft.aad.directory/roleDefinitions/basic/read | Lesen der Basiseigenschaften für roleDefinitions in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lesen der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lesen der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/basic/read | Lesen der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lesen der servicePrincipals.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Lesen der servicePrincipals.oAuth2PermissionGrants-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lesen der servicePrincipals.ownedObjects-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/read | Lesen der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/read | Lesen der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/subscribedSkus/basic/read | Lesen der Basiseigenschaften für subscribedSkus in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/read | Lesen der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/basic/read | Lesen der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/directReports/read | Lesen der users.directReports-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/invitedBy/read | Lesen der users.invitedBy-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/invitedUsers/read | Lesen der users.invitedUsers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/manager/read | Lesen der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/memberOf/read | Lesen der users.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Lesen der users.oAuth2PermissionGrants-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedDevices/read | Lesen der users.ownedDevices-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedObjects/read | Lesen der users.ownedObjects-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/registeredDevices/read | Lesen der users.registeredDevices-Eigenschaft in Azure Active Directory |

### <a name="directory-synchronization-accounts"></a>Konten zur Verzeichnissynchronisierung
Nur vom Azure AD Connect-Dienst verwendet.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Aktualisieren der organization.dirSync-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/create | Erstellen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/delete | Löschen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/basic/read | Lesen der Basiseigenschaften für Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/basic/update | Aktualisieren der Basiseigenschaften für Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/owners/read | Lesen der policies.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/owners/update | Aktualisieren der policies.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Lesen der policies.policiesAppliedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lesen der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren der servicePrincipals.appRoleAssignedTo-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lesen der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Aktualisieren der servicePrincipals.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/basic/read | Lesen der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/basic/update | Aktualisieren der Basiseigenschaften für servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/create | Erstellen von servicePrincipals in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lesen der servicePrincipals.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Lesen der servicePrincipals.oAuth2PermissionGrants-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/read | Lesen der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/owners/update | Aktualisieren der servicePrincipals.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lesen der servicePrincipals.ownedObjects-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/read | Lesen der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualisieren der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directorySync/allEntities/allTasks | Ausführen sämtlicher Aktionen in Azure AD Connect. |

### <a name="directory-writers"></a>Verzeichnis schreiben
Lesen und Schreiben von grundlegenden Verzeichnisinformationen. Die Rolle gewährt Zugriff auf Anwendungen und ist nicht für Benutzer vorgesehen.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/groups/create | Erstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/createAsOwner | Erstellen von Gruppen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Aktualisieren der groups.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/basic/update | Aktualisieren der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/members/update | Aktualisieren der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/update | Aktualisieren der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/settings/update | Aktualisieren der groups.settings-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groupSettings/basic/update | Aktualisieren der Basiseigenschaften für groupSettings in Azure Active Directory |
| microsoft.aad.directory/groupSettings/create | Erstellen der groupSettings in Azure Active Directory |
| microsoft.aad.directory/groupSettings/delete | Löschen von groupSettings in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualisieren der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/basic/update | Aktualisieren der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Kennzeichnen sämtlicher Aktualisierungstoken für Benutzer in Azure Active Directory als ungültig. |
| microsoft.aad.directory/users/manager/update | Aktualisieren der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/userPrincipalName/update | Aktualisieren der users.userPrincipalName-Eigenschaft in Azure Active Directory |

### <a name="exchange-service-administrator"></a>Exchange-Dienstadministrator
Verwalten sämtlicher Aspekte des Produkts Exchange.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Aktualisieren der groups.unified-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/unified/basic/update | Aktualisieren der Basiseigenschaften von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/create | Erstellen von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/delete | Löschen von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/members/update | Aktualisieren der Mitgliedschaft von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/owners/update | Aktualisieren des Besitzers von Office 365-Gruppen |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.exchange/allEntities/allTasks | Verwalten sämtlicher Aspekte von Exchange Online. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="guest-inviter"></a>Gasteinladender
Diese Rolle kann Gastbenutzer einladen, unabhängig von der Einstellung „Mitglieder können Gäste einladen“.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Lesen der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/basic/read | Lesen der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/directReports/read | Lesen der users.directReports-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/invitedBy/read | Lesen der users.invitedBy-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/inviteGuest | Einladen von Gastbenutzern in Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Lesen der users.invitedUsers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/manager/read | Lesen der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/memberOf/read | Lesen der users.memberOf-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Lesen der users.oAuth2PermissionGrants-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedDevices/read | Lesen der users.ownedDevices-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/ownedObjects/read | Lesen der users.ownedObjects-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/registeredDevices/read | Lesen der users.registeredDevices-Eigenschaft in Azure Active Directory |

### <a name="helpdesk-administrator"></a>Helpdeskadministrator
Zurücksetzen von Kennwörtern für Nicht-Administratoren und Helpdeskadministratoren.

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
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
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
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Aktualisieren der Basiseigenschaften für Kontakte in Azure Active Directory |
| microsoft.aad.directory/contacts/create | Erstellen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/contacts/delete | Löschen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/devices/basic/update | Aktualisieren der Basiseigenschaften für Geräte in Azure Active Directory |
| microsoft.aad.directory/devices/create | Erstellen von Geräten in Azure Active Directory |
| microsoft.aad.directory/devices/delete | Löschen von Geräten in Azure Active Directory |
| microsoft.aad.directory/devices/registeredOwners/update | Aktualisieren der devices.registeredOwners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/devices/registeredUsers/update | Aktualisieren der devices.registeredUsers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/appRoleAssignments/update | Aktualisieren der groups.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/basic/update | Aktualisieren der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/create | Erstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/createAsOwner | Erstellen von Gruppen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/groups/delete | Löschen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/hiddenMembers/read | Lesen der groups.hiddenMembers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/members/update | Aktualisieren der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/update | Aktualisieren der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/restore | Wiederherstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/settings/update | Aktualisieren der groups.settings-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualisieren der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/basic/update | Aktualisieren der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/manager/update | Aktualisieren der users.manager-Eigenschaft in Azure Active Directory |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.intune/allEntities/allTasks | Verwalten sämtlicher Aspekte von Intune. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="license-administrator"></a>Lizenzadministrator
Kann Produktlizenzen für Benutzer und Gruppen verwalten.

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
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
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
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.office365.messageCenter/messages/read | Lesen von Nachrichten in microsoft.office365.messageCenter |

### <a name="partner-tier1-support"></a>Partnersupport der Ebene 1
Verwenden Sie diese Rolle nicht – sie ist nicht zur allgemeinen Verwendung vorgesehen.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Aktualisieren der Basiseigenschaften für Kontakte in Azure Active Directory |
| microsoft.aad.directory/contacts/create | Erstellen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/contacts/delete | Löschen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/groups/create | Erstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/createAsOwner | Erstellen von Gruppen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/groups/members/update | Aktualisieren der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/update | Aktualisieren der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualisieren der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/basic/update | Aktualisieren der Basiseigenschaften für Benutzer in Azure Active Directory |
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
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Aktualisieren der Basiseigenschaften für Kontakte in Azure Active Directory |
| microsoft.aad.directory/contacts/create | Erstellen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/contacts/delete | Löschen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/domains/allTasks | Erstellen und Löschen von Domänen und Lesen und Aktualisieren der Standardeigenschaften in Azure Active Directory |
| microsoft.aad.directory/groups/create | Erstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/delete | Löschen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/members/update | Aktualisieren der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/restore | Wiederherstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/organization/basic/update | Aktualisieren der Basiseigenschaften für die Organisation in Azure Active Directory |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Aktualisieren der organizations.trustedCAsForPasswordlessAuth-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualisieren der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/basic/update | Aktualisieren der Basiseigenschaften für Benutzer in Azure Active Directory |
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
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
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
Dieser Administrator kann Rollenzuweisungen in Azure AD und alle Aspekte von Privileged Identity Management verwalten.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | Aktualisieren von directoryRoles in Azure Active Directory |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren von Standardeigenschaften in microsoft.aad.privilegedIdentityManagement |

### <a name="reports-reader"></a>Meldet Reader
Lesen von Anmeldungs- und Überwachungsberichten.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.reports/applicationAuditLogs/read | Lesen von applicationAuditLogs in Azure AD-Berichten |
| microsoft.aad.reports/applicationSignInReports/read | Lesen von applicationSignInReports in Azure AD-Berichten |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.usageReports/allEntities/read | Lesen von Office 365-Nutzungsberichten. |

### <a name="security-administrator"></a>Sicherheitsadministrator
Dieser Administrator kann Sicherheitsinformationen und -berichte lesen und die Konfiguration in Azure AD und Office 365 verwalten.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/applications/policies/update | Aktualisieren der applications.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/policies/basic/update | Aktualisieren der Basiseigenschaften für Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/create | Erstellen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/delete | Löschen von Richtlinien in Azure Active Directory |
| microsoft.aad.directory/policies/owners/update | Aktualisieren der policies.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualisieren der servicePrincipals.policies-Eigenschaft in Azure Active Directory |
| microsoft.aad.identityProtection/allEntities/read | Lesen aller Ressourcen in microsoft.aad.identityProtection |
| microsoft.aad.identityProtection/allEntities/update | Aktualisieren aller Ressourcen in microsoft.aad.identityProtection |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lesen aller Ressourcen in microsoft.aad.privilegedIdentityManagement |
| microsoft.aad.reports/applicationAuditLogs/read | Lesen von applicationAuditLogs in Azure AD-Berichten |
| microsoft.aad.reports/applicationSignInReports/read | Lesen von applicationSignInReports in Azure AD-Berichten |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.office365.protectionCenter/allEntities/read | Lesen sämtlicher Aspekte von Office 365 Protection Center. |
| microsoft.office365.protectionCenter/allEntities/update | Aktualisieren aller Ressourcen in microsoft.office365.protectionCenter |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |

### <a name="security-reader"></a>Sicherheit lesen
Lesen von Sicherheitsinformationen und Berichten in Azure AD und Office 365.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.identityProtection/allEntities/read | Lesen aller Ressourcen in microsoft.aad.identityProtection |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lesen aller Ressourcen in microsoft.aad.privilegedIdentityManagement |
| microsoft.aad.reports/applicationAuditLogs/read | Lesen von applicationAuditLogs in Azure AD-Berichten |
| microsoft.aad.reports/applicationSignInReports/read | Lesen von applicationSignInReports in Azure AD-Berichten |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.office365.protectionCenter/allEntities/read | Lesen sämtlicher Aspekte von Office 365 Protection Center. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |

### <a name="service-support-administrator"></a>Dienstunterstützungsadministrator
Lesen von Service Health-Informationen und Verwalten von Supporttickets.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
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
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Aktualisieren der groups.unified-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/unified/basic/update | Aktualisieren der Basiseigenschaften von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/create | Erstellen von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/delete | Löschen von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/members/update | Aktualisieren der Mitgliedschaft von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/owners/update | Aktualisieren des Besitzers von Office 365-Gruppen |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.sharepoint/allEntities/allTasks | Erstellen und Löschen aller Ressourcen und Lesen und Aktualisieren von Standardeigenschaften in microsoft.office365.sharepoint |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |

### <a name="teams-communications-administrator"></a>Teams-Kommunikationsadministrator
Kann Anruf- und Besprechungsfunktionen im Microsoft Teams-Dienst verwalten.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/policies/basic/read | Lesen der Basiseigenschaften für Richtlinien in Azure Active Directory |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.office365.usageReports/allEntities/read | Lesen von Office 365-Nutzungsberichten. |

### <a name="teams-communications-support-engineer"></a>Teams-Kommunikationssupporttechniker
Kann Kommunikationsprobleme in Teams mithilfe von erweiterten Tools behandeln.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/policies/basic/read | Lesen der Basiseigenschaften für Richtlinien in Azure Active Directory |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |

### <a name="teams-communications-support-specialist"></a>Teams-Kommunikationssupportspezialist
Kann Kommunikationsprobleme in Teams mithilfe von allgemeinen Tools behandeln.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/policies/basic/read | Lesen der Basiseigenschaften für Richtlinien in Azure Active Directory |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |

### <a name="teams-service-administrator"></a>Teams-Dienstadministrator
Kann den Microsoft Teams-Dienst verwalten.

  > [!NOTE]
  > Diese Rolle verfügt über zusätzliche Berechtigungen außerhalb von Azure Active Directory. Weitere Informationen finden Sie in der Rollenbeschreibung oben.
  >
  >

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Lesen der groups.hiddenMembers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Aktualisieren der groups.unified-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/unified/basic/update | Aktualisieren der Basiseigenschaften von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/create | Erstellen von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/delete | Löschen von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/members/update | Aktualisieren der Mitgliedschaft von Office 365-Gruppen |
| microsoft.aad.directory/groups/unified/owners/update | Aktualisieren des Besitzers von Office 365-Gruppen |
| microsoft.aad.directory/policies/basic/read | Lesen der Basiseigenschaften für Richtlinien in Azure Active Directory |
| microsoft.azure.accessService/allEntities/allTasks | Verwalten sämtlicher Aspekte des Azure-Zugriffsdiensts |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
| microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren des Office 365-Dienststatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Office 365-Supporttickets. |
| microsoft.office365.usageReports/allEntities/read | Lesen von Office 365-Nutzungsberichten. |

### <a name="user-account-administrator"></a>Benutzerkontoadministrator
Dieser Administrator kann alle Aspekte von Benutzern und Gruppen verwalten, einschließlich der Kennwortzurücksetzung für eingeschränkte Administratoren.

| **Aktionen** | **Beschreibung** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Erstellen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/delete | Löschen von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/appRoleAssignments/update | Aktualisieren von appRoleAssignments in Azure Active Directory |
| microsoft.aad.directory/contacts/basic/update | Aktualisieren der Basiseigenschaften für Kontakte in Azure Active Directory |
| microsoft.aad.directory/contacts/create | Erstellen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/contacts/delete | Löschen von Kontakten in Azure Active Directory |
| microsoft.aad.directory/groups/appRoleAssignments/update | Aktualisieren der groups.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/basic/update | Aktualisieren der Basiseigenschaften für Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/create | Erstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/createAsOwner | Erstellen von Gruppen in Azure Active Directory Der Ersteller wird als erster Besitzer hinzugefügt, und das erstellte Objekt wird auf das Kontingent von 250 erstellten Objekten angerechnet, das für den Ersteller gilt. |
| microsoft.aad.directory/groups/delete | Löschen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/hiddenMembers/read | Lesen der groups.hiddenMembers-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/members/update | Aktualisieren der groups.members-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/owners/update | Aktualisieren der groups.owners-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/groups/restore | Wiederherstellen von Gruppen in Azure Active Directory |
| microsoft.aad.directory/groups/settings/update | Aktualisieren der groups.settings-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualisieren der users.appRoleAssignments-Eigenschaft in Azure Active Directory |
| microsoft.aad.directory/users/assignLicense | Verwalten der Lizenzen für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/basic/update | Aktualisieren der Basiseigenschaften für Benutzer in Azure Active Directory |
| microsoft.aad.directory/users/create | Erstellen von Benutzern in Azure Active Directory |
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

## <a name="deprecated-roles"></a>Veraltete Rollen

Die folgenden Rollen sollten nicht verwendet werden. Sie sind veraltet und werden aus Azure AD entfernt.

* Ad-hoc-Lizenzadministrator
* Geräteeinbindung
* Geräte-Manager
* Gerätebenutzer
* Benutzererstellung mit E-Mail-Überprüfung
* Postfachadministrator
* Geräteeinbindung am Arbeitsplatz

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Zuweisen eines Benutzers als Administrator eines Azure-Abonnements finden Sie unter [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](../../role-based-access-control/role-assignments-portal.md).
* Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Weitere Informationen zur Beziehung zwischen Azure Active Directory und Ihrem Azure-Abonnement finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
