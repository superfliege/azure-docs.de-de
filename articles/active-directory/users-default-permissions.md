---
title: Vergleich der Standardbenutzerberechtigungen in Azure Active Directory | Microsoft-Dokumentation
description: Vergleich der Berechtigungen für Mitglieder, Gastbenutzer, App- und Gruppenbesitzer
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 01/29/2018
ms.author: curtand
ms.reviewer: vincesm
ms.openlocfilehash: a9bf9748de5f390f95b8b672e0cf77afa5c49581
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="default-user-permissions-in-azure-active-directory"></a>Standardbenutzerberechtigungen in Azure Active Directory

In Azure Active Directory (Azure AD) wird allen Benutzern ein Satz mit Standardberechtigungen gewährt. Der Zugriffsumfang eines Benutzers basiert auf dem Benutzertyp, den zugewiesenen [Rollenmitgliedschaften](https://docs.microsoft.com/azure/active-directory/active-directory-users-assign-role-azure-portal) und dem Besitz einzelner Objekte. In diesem Artikel werden diese Standardberechtigungen beschrieben, und es werden die Standardberechtigungen für Mitglieder und Gastbenutzer miteinander verglichen.

## <a name="member-and-guest-users"></a>Mitglieder und Gastbenutzer
Der gewährte Satz an Standardberechtigungen richtet sich danach, ob der Benutzer ein natives Mitglied des Mandanten ist (Mitgliedsbenutzer), oder ob es sich um einen Gast im Rahmen der B2B-Zusammenarbeit (Gastbenutzer) handelt. Weitere Informationen zur B2B-Zusammenarbeit und zu Gastbenutzern finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md). 
* Mitgliedsbenutzer können Anwendungen registrieren, ihr eigenes Profilfoto und ihre Mobiltelefonnummer verwalten, das eigene Kennwort verwalten und B2B-Gäste einladen. Zusätzlich können die Benutzer (mit einigen Ausnahmen) alle Verzeichnisinformationen lesen. 
* Azure AD-B2B-Gastbenutzer erhalten eingeschränkte Verzeichnisberechtigungen. Beispielsweise können Gastbenutzer über ihre Profilinformationen hinaus keine Informationen im Mandanten durchsuchen. Ein Gastbenutzer kann jedoch Informationen zu einem anderen Benutzer abrufen, indem er den Benutzerprinzipalnamen oder die objectId angibt. Ein Gast kann keine Informationen zu anderen Mandantenobjekten wie z.B. Gruppen und Anwendungen anzeigen.

Die Standardberechtigungen für Gäste sind per Voreinstellung eingeschränkt. Gäste können zu Administratorrollen hinzugefügt werden, wodurch ihnen alle in der Rolle enthaltenen Lese- und Schreibberechtigungen gewährt werden. Es steht eine weitere Einschränkung zur Verfügung, und zwar die Fähigkeit von Gästen, andere Gäste einzuladen. Durch das Festlegen der Einstellung **Gäste können einladen** auf **Nein** werden Gäste daran gehindert, andere Gäste einzuladen. Weitere Informationen finden Sie unter [Delegieren von Einladungen zur Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-delegate-invitations.md). Um Gastbenutzern standardmäßig dieselben Berechtigungen zuzuweisen wie Mitgliedsbenutzern, legen Sie **Berechtigungen für Gastbenutzer sind eingeschränkt** auf **Nein** fest. Mit dieser Einstellung erhalten alle Gastbenutzer standardmäßig die Berechtigungen für Mitgliedsbenutzer, und Gäste können administrativen Rollen hinzugefügt werden.

## <a name="compare-member-and-guest-default-permissions"></a>Vergleich der Standardberechtigungen für Mitglieds- und Gastbenutzer

**Bereich** | **Berechtigungen für Mitgliedsbenutzer** | **Berechtigungen für Gastbenutzer**
------------ | --------- | ----------
Benutzer und Kontakte | Alle öffentlichen Eigenschaften von Benutzern und Kontakten lesen<br>Gäste einladen<br>Eigenes Kennwort ändern<br>Eigene Mobiltelefonnummer verwalten<br>Eigenes Foto verwalten<br>Eigene Aktualisierungstoken für ungültig erklären | Eigene Eigenschaften lesen<br>Eigenschaften für Anzeigename, E-Mail-Adresse, Anmeldename, Foto, Benutzerprinzipalname und Benutzertyp anderer Benutzer und Kontakte lesen<br>Eigenes Kennwort ändern
Gruppen   | Sicherheitsgruppen erstellen<br>Office 365-Gruppen erstellen<br>Alle Eigenschaften von Gruppen lesen<br>Nicht ausgeblendete Gruppenmitgliedschaften lesen<br>Ausgeblendete Office 365-Gruppenmitgliedschaften für eingebundene Gruppe lesen<br>Eigenschaften, Besitz und Mitgliedschaft von Gruppen im Besitz des Benutzers verwalten<br>Gäste zu Gruppen im Besitz des Benutzers hinzufügen<br>Einstellungen für dynamische Mitgliedschaften verwalten<br>Gruppen im Besitz des Benutzers löschen<br>Office 365-Gruppen im Besitz des Benutzers wiederherstellen | Alle Eigenschaften von Gruppen lesen<br>Nicht ausgeblendete Gruppenmitgliedschaften lesen<br>Ausgeblendete Office 365-Gruppenmitgliedschaften für eingebundene Gruppen lesen<br>Gruppen im Besitz des Benutzers verwalten<br>Gäste zu Gruppen im Besitz des Benutzers hinzufügen (sofern zulässig)<br>Gruppen im Besitz des Benutzers löschen<br>Office 365-Gruppen im Besitz des Benutzers wiederherstellen           
ANWENDUNGEN | Neue Anwendung registrieren (erstellen)<br>Eigenschaften für registrierte Anwendungen und Unternehmensanwendungen lesen<br>Anwendungseigenschaften, Zuweisungen und Anmeldeinformationen für Anwendungen im Besitz des Benutzers verwalten<br>Anwendungskennwort für Benutzer erstellen oder löschen<br>Anwendungen im Besitz des Benutzers löschen<br>Anwendungen im Besitz des Benutzers wiederherstellen | Eigenschaften für registrierte Anwendungen und Unternehmensanwendungen lesen<br>Anwendungseigenschaften, Zuweisungen und Anmeldeinformationen für Anwendungen im Besitz des Benutzers verwalten<br>Anwendungen im Besitz des Benutzers löschen<br>Anwendungen im Besitz des Benutzers wiederherstellen
Geräte | Alle Eigenschaften von Geräten lesen<br>Alle Eigenschaften von Geräten im Besitz des Benutzers verwalten<br> | Keine Berechtigungen<br>Geräte im Besitz des Benutzers löschen<br>
Verzeichnis | Alle Unternehmensinformationen lesen<br>Alle Domänen lesen<br>Alle Partnerverträge lesen | Anzeigenamen und überprüfte Domänen lesen
Rollen und Bereiche | Alle administrativen Rollen und Mitgliedschaften lesen<br>Alle Eigenschaften und Mitgliedschaften von administrativen Einheiten lesen | Keine Berechtigungen              
Abonnements | Alle Abonnements lesen<br>Mitglied für Diensttarif aktivieren | Keine Berechtigungen
Richtlinien | Alle Eigenschaften von Richtlinien lesen<br>Alle Eigenschaften von Richtlinien im Besitz des Benutzers verwalten | Keine Berechtigungen

## <a name="to-restrict-the-default-permissions-for-member-users"></a>Einschränken der Standardberechtigungen für Mitgliedsbenutzer


Die Standardberechtigungen für Mitgliedsbenutzer können auf die folgende Weise eingeschränkt werden. Weitere Informationen finden Sie unter [Apps, Berechtigungen und Zustimmung in Azure Active Directory](active-directory-apps-permissions-consent.md).

Berechtigung | Erläuterung der Einstellung
---------- | ------------
Fähigkeit zum Erstellen von Sicherheitsgruppen | Durch das Festlegen dieser Einstellung auf „Nein“ werden Benutzer daran gehindert, Sicherheitsgruppen zu erstellen. Globale Administratoren und Benutzerkontoadministratoren können weiterhin Sicherheitsgruppen erstellen. Informationen zur Vorgehensweise finden Sie unter [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md).
Fähigkeit zum Erstellen von Office 365-Gruppen | Durch das Festlegen dieser Einstellung auf „Nein“ werden Benutzer daran gehindert, Office 365-Gruppen zu erstellen. Durch das Festlegen dieser Option auf „Einige“ wird einem ausgewählten Benutzersatz das Erstellen von Office 365-Gruppen ermöglicht. Globale Administratoren und Benutzerkontoadministratoren können weiterhin Office 365-Gruppen erstellen. Informationen zur Vorgehensweise finden Sie unter [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md).
Fähigkeit zur Genehmigung von Anwendungen | Durch das Festlegen dieser Einstellung auf „Nein“ werden Benutzer daran gehindert, Anwendungen zu genehmigen. Globale Administratoren können weiterhin Anwendungen genehmigen. Weitere Informationen finden Sie unter [Apps, Berechtigungen und Zustimmung in Azure Active Directory](active-directory-apps-permissions-consent.md).
Fähigkeit zum Hinzufügen von Katalog-Apps zum Zugriffspanel | Durch das Festlegen dieser Option auf „Nein“ werden Benutzer daran gehindert, Katalog-Apps zu ihrem Zugriffspanel hinzuzufügen.
Fähigkeit zum Registrieren (Erstellen) von Anwendungen | Durch das Festlegen dieser Einstellung auf „Nein“ werden Nicht-Administratorbenutzer daran gehindert, Anwendungen zu erstellen. Globale Administratoren können weiterhin Anwendungen erstellen. Weitere Informationen finden Sie unter [Apps, Berechtigungen und Zustimmung in Azure Active Directory](active-directory-apps-permissions-consent.md).
Administratoren und Benutzer in der Rolle „Gasteinladender“ dürfen Gäste einladen | Das Festlegen dieser Option auf „Nein“ hindert alle Benutzer daran, Gäste einzuladen. Siehe „Konfigurieren von Standardberechtigungen für Mitgliedsbenutzer“. Weitere Informationen finden Sie unter [Apps, Berechtigungen und Zustimmung in Azure Active Directory](active-directory-apps-permissions-consent.md).
Mitglieder können Gäste einladen | Das Festlegen dieser Einstellung auf „Nein“ hindert alle Benutzer daran, Gäste einzuladen. Globale Administratoren, Benutzerkontoadministratoren und Gasteinladende können weiterhin Gäste einladen. Weitere Informationen finden Sie unter [Apps, Berechtigungen und Zustimmung in Azure Active Directory](active-directory-apps-permissions-consent.md).
Zugriff auf Azure AD-Verwaltungsportal einschränken | Das Festlegen dieser Option auf „Nein“ hindert Benutzer daran, auf das Azure Active Directory-Portal zuzugreifen.
Fähigkeit zum Lesen anderer Benutzer | Diese Einstellung ist nur in PowerShell verfügbar. Wenn diese Einstellung auf $false festgelegt ist, wird verhindert, dass alle Nicht-Administratoren Benutzerinformationen aus dem Verzeichnis lesen. Nicht verhindert wird jedoch, dass Benutzerinformationen in anderen Microsoft-Diensten wie Exchange Online gelesen werden können. Diese Einstellung ist für besondere Umstände bestimmt; die Einstellung auf $false wird nicht empfohlen.

## <a name="object-ownership"></a>Objektbesitz

### <a name="application-registration-owner-permissions"></a>Berechtigungen als Anwendungsregistrierungsbesitzer
Wenn ein Benutzer eine Anwendung registriert, wird er automatisch als Besitzer für die Anwendung hinzugefügt. Als Besitzer kann der Benutzer die Metadaten der Anwendung verwalten, beispielsweise den Namen und Berechtigungen, die von der App anfordert werden. Darüber hinaus kann der Benutzer auch die mandantenspezifische Konfiguration der Anwendung verwalten, z.B. die SSO-Konfiguration und Benutzerzuweisungen. Ein Besitzer kann außerdem andere Besitzer hinzufügen oder entfernen. Im Gegensatz zu globalen Administratoren können Besitzer nur Anwendungen verwalten, deren Besitzer sie sind. Informationen zum Zuweisen eines Anwendungsregistrierungsbesitzers finden Sie unter [Azure Active Directory-App-Registrierung](active-directory-app-registration.md).

<!-- ### Enterprise application owner permissions

When a user adds a new enterprise application, they are automatically added as an owner for the tenant-specific configuration of the application. As an owner, they can manage the tenant-specific configuration of the application, such as the SSO configuration, provisioning, and user assignments. An owner can also add or remove other owners. Unlike Global Administrators, owners can manage only the applications they own. <!--To assign an enterprise application owner, see *Assigning Owners for an Application*.-->

### <a name="group-owner-permissions"></a>Berechtigungen als Gruppenbesitzer

Wenn ein Benutzer eine Gruppe erstellt, wird er automatisch als Besitzer für diese Gruppe hinzugefügt. Als Besitzer kann der Benutzer die Eigenschaften der Gruppe verwalten, beispielsweise den Namen sowie die Gruppenmitgliedschaft. Ein Besitzer kann außerdem andere Besitzer hinzufügen oder entfernen. Im Gegensatz zu globalen Administratoren und Benutzerkontoadministratoren können Besitzer nur Gruppen verwalten, deren Besitzer sie sind. Informationen zum Zuweisen eines Gruppenbesitzers finden Sie unter [Verwalten von Besitzern für eine Gruppe](active-directory-accessmanagement-managing-group-owners.md).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Ändern von Administratoren für ein Azure-Abonnement finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen](../billing-add-change-azure-subscription-administrator.md)
* Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md)
* Weitere Informationen zur Beziehung zwischen Azure Active Directory und Ihrem Azure-Abonnement finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Verwalten von Benutzern](active-directory-create-users.md)
