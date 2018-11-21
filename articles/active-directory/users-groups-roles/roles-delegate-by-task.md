---
title: Delegieren der am wenigsten privilegierten Rollen nach Aufgabe in Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie mehr über die zu delegierenden Rollen für Identitätsaufgaben in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 11/08/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: b4704e9f078b248ef1835babb3b63c378b011c4e
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2018
ms.locfileid: "51516586"
---
# <a name="administrator-roles-by-identity-task-in-azure-active-directory"></a>Administratorrollen nach Identitätsaufgabe in Azure Active Directory

In diesem Artikel finden Sie die erforderlichen Informationen, um Administratorrechte eines Benutzers einzuschränken, indem Sie die am wenigsten privilegierten Rollen in Azure Active Directory (Azure AD) zuweisen. Es werden Administratoraufgaben, die nach Featurebereichen organisiert sind, die am wenigsten privilegierte Rolle, die für die Ausführung der einzelnen Aufgaben erforderlich ist, sowie zusätzliche nicht globale Administratorrollen beschrieben, die die Aufgabe ausführen können.

## <a name="application-proxy"></a>Anwendungsproxy

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Konfigurieren einer Anwendungsproxy-App | Anwendungsadministrator | 
Konfigurieren von Connectorgruppeneigenschaften | Anwendungsadministrator | 
Erstellen einer Anwendungsregistrierung, wenn die Funktion für alle Benutzer deaktiviert ist | Anwendungsentwickler | Cloudanwendungs-, Anwendungsadministrator
Erstellen einer Connectorgruppe | Anwendungsadministrator | 
Löschen einer Connectorgruppe | Anwendungsadministrator | 
Anwendungsproxy deaktivieren | Anwendungsadministrator | 
Herunterladen eines Connectordiensts | Anwendungsadministrator | 
Lesen aller Konfigurationen | Anwendungsadministrator | 

## <a name="b2c"></a>B2C

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Erstellen von Azure AD B2C-Verzeichnissen | Alle Benutzer außer Gäste (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Erstellen von B2C-Anwendungen | Globaler Administrator | 
Erstellen von Unternehmensanwendungen | Cloudanwendungsadministrator | Anwendungsadministrator
Erstellen, Lesen, Aktualisieren und Löschen von B2C-Richtlinien | Globaler Administrator | 
Erstellen, Lesen, Aktualisieren und Löschen von Identitätsanbietern | Globaler Administrator | 
Erstellen, Lesen, Aktualisieren und Löschen von Benutzerflows zur Kennwortzurücksetzung | Globaler Administrator | 
Erstellen, Lesen, Aktualisieren und Löschen von Benutzerflows zur Profilbearbeitung | Globaler Administrator | 
Erstellen, Lesen, Aktualisieren und Löschen von Benutzerflows zur Anmeldung | Globaler Administrator | 
Erstellen, Lesen, Aktualisieren und Löschen von Benutzerflows zur Registrierung |Globaler Administrator | 
Erstellen, Lesen, Aktualisieren und Löschen von Benutzerattributen | Globaler Administrator | 
Erstellen, Lesen, Aktualisieren und Löschen von Benutzern | Globaler Administrator (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs))
Lesen aller Konfigurationen | Globaler Administrator | 
Lesen von B2C-Überwachungsprotokollen | Globaler Administrator (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs)) | 

## <a name="company-branding"></a>Unternehmensbranding

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Konfigurieren des Unternehmensbrandings | Globaler Administrator | 
Lesen aller Konfigurationen | Rolle „Verzeichnis lesen“ | Standardbenutzerrolle (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="company-properties"></a>Unternehmenseigenschaften

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Konfigurieren von Unternehmenseigenschaften | Globaler Administrator | 

## <a name="connect"></a>Verbinden

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Pass-Through-Authentifizierung | Globaler Administrator | 
Lesen aller Konfigurationen | Globaler Administrator | 
Nahtloses einmaliges Anmelden | Globaler Administrator | 

## <a name="connect-health"></a>Connect Health

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Hinzufügen oder Löschen von Diensten | Besitzer (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Anwenden von Fehlerbehebungen zum Synchronisieren von Fehlern | Mitwirkender (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Owner (Besitzer)
Konfigurieren von Benachrichtigungen | Mitwirkender (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Owner (Besitzer)
Konfigurieren von Einstellungen | Besitzer (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Konfigurieren von Synchronisierungsbenachrichtigungen | Mitwirkender (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Owner (Besitzer)
Lesen von AD FS-Sicherheitsberichten | Sicherheit lesen | Mitwirkender, Besitzer
Lesen aller Konfigurationen | Leser (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Mitwirkender, Besitzer
Lesen von Synchronisierungsfehlern | Leser (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Mitwirkender, Besitzer
Lesen von Synchronisierungsdiensten | Leser (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Mitwirkender, Besitzer
Anzeigen von Metriken und Warnungen | Leser (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Mitwirkender, Besitzer
Anzeigen von Metriken und Warnungen | Leser (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Mitwirkender, Besitzer
Anzeigen von Metriken und Warnungen zum Synchronisierungsdienst | Leser (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Mitwirkender, Besitzer


## <a name="custom-domain-names"></a>Benutzerdefinierte Domänennamen

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Verwalten von Domänen | Globaler Administrator | 
Lesen aller Konfigurationen | Rolle „Verzeichnis lesen“ | Standardbenutzerrolle (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="domain-services"></a>Domänendienste

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Erstellen einer Azure AD Domain Services-Instanz | Globaler Administrator | 
Ausführen aller Azure AD Domain Services-Tasks | Administratorengruppe für Azure AD-Domänencontroller (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-administer-domain#administrative-tasks-you-can-perform-on-a-managed-domain)) | 
Lesen aller Konfigurationen | Leser für Azure-Abonnements, die den AD DS-Dienst umfassen | 

## <a name="devices"></a>Geräte

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Deaktivieren eines Geräts | Cloudgeräteadministrator | 
Aktivieren eines Geräts | Cloudgeräteadministrator | 
Lesen einer Standardkonfiguration | Standardbenutzerrolle (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Lesen von BitLocker-Schlüsseln | Sicherheit lesen | Kennwort-, Sicherheitsadministrator

## <a name="enterprise-applications"></a>Unternehmensanwendungen

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Erteilen der Zustimmung zu delegierten Berechtigungen | Cloudanwendungsadministrator | Anwendungsadministrator
Erteilen der Zustimmung zu Anwendungsberechtigungen, jedoch nicht für Microsoft Graph oder Azure AD Graph | Cloudanwendungsadministrator | Anwendungsadministrator
Erteilen der Zustimmung zu Anwendungsberechtigungen für Microsoft Graph oder Azure AD Graph | Globaler Administrator | 
Erteilen der Zustimmung für Anwendungen, die auf eigene Daten zugreifen | Standardbenutzerrolle (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Erstellen einer Unternehmensanwendung | Cloudanwendungsadministrator | Anwendungsadministrator
Verwalten eines Anwendungsproxys | Anwendungsadministrator | 
Verwalten von Benutzereinstellungen | Globaler Administrator | 
Überprüfung des Lesezugriffs einer Gruppe oder einer App | Sicherheit lesen | Sicherheits-, Benutzeradministrator
Lesen aller Konfigurationen | Standardbenutzerrolle (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Aktualisieren von Enterprise-Anwendungszuweisungen | Besitzer einer Enterprise-Anwendung (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Cloudanwendungs-, Anwendungsadministrator
Aktualisieren von Enterprise-Anwendungsbesitzern | Besitzer einer Enterprise-Anwendung (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Cloudanwendungs-, Anwendungsadministrator
Aktualisieren von Enterprise-Anwendungseigenschaften | Besitzer einer Enterprise-Anwendung (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Cloudanwendungs-, Anwendungsadministrator
Aktualisieren der Enterprise-Anwendungsbereitstellung | Besitzer einer Enterprise-Anwendung (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Cloudanwendungs-, Anwendungsadministrator
Aktualisieren des Self-Service-Zugriffs auf Enterprise-Anwendungen | Besitzer einer Enterprise-Anwendung (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Cloudanwendungs-, Anwendungsadministrator
Aktualisieren von Eigenschaften für das einmalige Anmelden | Besitzer einer Enterprise-Anwendung (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Cloudanwendungs-, Anwendungsadministrator

## <a name="groups"></a>Gruppen

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Lizenz zuweisen | Benutzerkontoadministrator | 
Erstellen einer Gruppe | Benutzerkontoadministrator | 
Erstellen, Aktualisieren oder Löschen der Zugriffsüberprüfung einer Gruppe oder einer App | Benutzerkontoadministrator | 
Verwalten des Gruppenablaufs | Benutzerkontoadministrator | 
Verwalten von Gruppeneinstellungen | Benutzerkontoadministrator | 
Lesen der gesamten Konfiguration (mit Ausnahme der ausgeblendeten Mitgliedschaft) | Rolle „Verzeichnis lesen“ | Standardbenutzerrolle (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Lesen der ausgeblendeten Mitgliedschaft | Gruppenmitglied | Gruppenbesitzer, Kennwortadministrator, Exchange-Administrator, SharePoint-Administrator, Teams-Administrator, Benutzerkontoadministrator
Lesen der Mitgliedschaft von Gruppen mit ausgeblendeter Mitgliedschaft | Helpdeskadministrator | Benutzerkonto-, Teams-Administrator
Widerrufen von Lizenzen | Lizenzadministrator | Benutzerkontoadministrator
Aktualisieren der Gruppenmitgliedschaft | Gruppenbesitzer (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Benutzerkontoadministrator
Aktualisieren von Gruppenbesitzern | Gruppenbesitzer (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Benutzerkontoadministrator
Aktualisieren von Gruppeneigenschaften | Gruppenbesitzer (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Benutzerkontoadministrator

## <a name="identity-protection"></a>Schutz der Identität (Identity Protection)

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Konfigurieren von Warnungsbenachrichtigungen| Sicherheitsadministrator | 
Konfigurieren und Aktivieren oder Deaktivieren einer MFA-Richtlinie| Sicherheitsadministrator | 
Konfigurieren und Aktivieren oder Deaktivieren einer Richtlinie zum Anmelderisiko| Sicherheitsadministrator | 
Konfigurieren und Aktivieren oder Deaktivieren einer Richtlinie zum Benutzerrisiko | Sicherheitsadministrator | 
Konfigurieren von wöchentlichen Digests | Sicherheitsadministrator| 
Alle Risikoereignisse schließen | Sicherheitsadministrator | 
Beheben oder Ausschließen von Sicherheitsrisiken | Sicherheitsadministrator | 
Lesen aller Konfigurationen | Sicherheit lesen | 
Lesen aller Risikoereignisse | Sicherheit lesen | 
Lesen von Sicherheitsrisiken | Sicherheit lesen | 

## <a name="licenses"></a>Lizenzen

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Lizenz zuweisen | Lizenzadministrator | Benutzerkontoadministrator
Lesen aller Konfigurationen | Rolle „Verzeichnis lesen“ | Standardbenutzerrolle (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Widerrufen von Lizenzen | Lizenzadministrator | Benutzerkontoadministrator
Testen oder Erwerben von Abonnements | Rechnungsadministrator | 


## <a name="monitoring---audit-logs"></a>Überwachung – Überwachungsprotokolle

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Lesen von Überwachungsprotokollen | Berichtsleser | Benutzer mit Leseberechtigung für Sicherheitsfunktionen, Sicherheitsadministrator

## <a name="monitoring---sign-ins"></a>Überwachung – Anmeldungen

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Lesen von Anmeldeprotokollen | Berichtsleser | Benutzer mit Leseberechtigung für Sicherheitsfunktionen, Sicherheitsadministrator

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Löschen aller vorhandenen App-Kennwörter, die von den ausgewählten Benutzern erstellt wurden | Globaler Administrator | 
Deaktivieren der MFA | Globaler Administrator | 
MFA aktivieren | Globaler Administrator | 
Verwalten von MFA-Diensteinstellungen | Globaler Administrator | 
Ausgewählte Benutzer müssen Kontaktmethoden erneut bereitstellen. | Globaler Administrator | 
Wiederherstellen der mehrstufigen Authentifizierung für alle gespeicherten Geräte  | Globaler Administrator | 

## <a name="mfa-server"></a>MFA-Server

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Benutzer sperren/zulassen | Globaler Administrator | 
Konfigurieren der Kontosperrung | Globaler Administrator | 
Konfigurieren von Cacheregeln | Globaler Administrator | 
Konfigurieren der Betrugswarnung | Globaler Administrator
Konfigurieren von Benachrichtigungen | Globaler Administrator | 
Konfigurieren einer Einmalumgehung | Globaler Administrator | 
Konfigurieren von Einstellungen für Telefonanrufe | Globaler Administrator | 
Konfigurieren von Anbietern | Globaler Administrator | 
Konfigurieren der Servereinstellungen | Globaler Administrator | 
Lesen eines Aktivitätsberichts | Globaler Administrator | 
Lesen aller Konfigurationen | Globaler Administrator | 
Lesen eines Serverstatus | Globaler Administrator |  

## <a name="organizational-relationships"></a>Organisationsbeziehungen

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Verwalten von Identitätsanbietern | Globaler Administrator | 
Verwalten von Einstellungen | Globaler Administrator | 
Verwalten von Nutzungsbedingungen | Globaler Administrator | 
Lesen aller Konfigurationen | Globaler Administrator | 

## <a name="password-reset"></a>Zurücksetzen des Kennworts

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Konfigurieren von Authentifizierungsmethoden | Globaler Administrator | 
Konfigurieren der Anpassung | Globaler Administrator | 
Konfigurieren einer Benachrichtigung | Globaler Administrator | 
Konfigurieren einer lokalen Integration | Globaler Administrator | 
Konfigurieren der Eigenschaften der Kennwortzurücksetzung | Globaler Administrator | 
Konfigurieren der Registrierung | Globaler Administrator | 
Lesen aller Konfigurationen | Sicherheits-, Benutzeradministrator | 

## <a name="privileged-identity-management"></a>Privileged Identity Management

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Zuweisen von Benutzern zu Rollen | Administrator für privilegierte Rollen | 
Konfigurieren von Rolleneinstellungen | Administrator für privilegierte Rollen | 
Anzeigen der Überwachungsaktivität | Benutzer mit Leseberechtigung für Sicherheitsfunktionen | 
Anzeigen von Rollenmitgliedschaften | Benutzer mit Leseberechtigung für Sicherheitsfunktionen | 

## <a name="roles-and-administrators"></a>Rollen und Administratoren

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Verwalten von Rollenzuweisungen | Administrator für privilegierte Rollen | 
Überprüfung des Lesezugriffs einer Azure AD-Rolle  | Sicherheit lesen | Sicherheitsadministrator, Administrator für privilegierte Rollen
Lesen aller Konfigurationen | Standardbenutzerrolle (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 

## <a name="security---authentication-methods"></a>Sicherheit – Authentifizierungsmethoden

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Konfigurieren von Authentifizierungsmethoden | Globaler Administrator | 
Lesen aller Konfigurationen | Globaler Administrator | 

## <a name="security---conditional-access"></a>Sicherheit – Bedingter Zugriff

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Konfigurieren von durch MFA bestätigten IP-Adressen | Administrator für bedingten Zugriff | 
Erstellen von benutzerdefinierten Steuerelementen | Administrator für bedingten Zugriff | Sicherheitsadministrator
Erstellen von benannten Standorten | Administrator für bedingten Zugriff | Sicherheitsadministrator
Erstellen von Richtlinien | Administrator für bedingten Zugriff | Sicherheitsadministrator
Erstellen von Nutzungsbedingungen | Administrator für bedingten Zugriff | Sicherheitsadministrator
Erstellen von VPN-Konnektivitätszertifikaten | Administrator für bedingten Zugriff | Sicherheitsadministrator
Löschen einer klassischen Richtlinie | Administrator für bedingten Zugriff | Sicherheitsadministrator
Löschen von Nutzungsbedingungen | Administrator für bedingten Zugriff | Sicherheitsadministrator
Löschen eines VPN-Konnektivitätszertifikats | Administrator für bedingten Zugriff | Sicherheitsadministrator
Deaktivieren einer klassischen Richtlinie | Administrator für bedingten Zugriff | Sicherheitsadministrator
Verwalten von benutzerdefinierten Steuerelementen | Administrator für bedingten Zugriff | Sicherheitsadministrator
Verwalten von benannten Standorten | Administrator für bedingten Zugriff | Sicherheitsadministrator
Verwalten von Nutzungsbedingungen | Administrator für bedingten Zugriff | Sicherheitsadministrator
Lesen aller Konfigurationen | Benutzer mit Leseberechtigung für Sicherheitsfunktionen | Sicherheitsadministrator
Lesen von benannten Standorten | Benutzer mit Leseberechtigung für Sicherheitsfunktionen | Administrator für bedingten Zugriff, Sicherheitsadministrator

## <a name="security---identity-security-score"></a>Sicherheit – Identity Security Score

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen | 
---- | --------------------- | ----------------
Lesen aller Konfigurationen | Benutzer mit Leseberechtigung für Sicherheitsfunktionen | Sicherheitsadministrator
Lesen des Security Score | Benutzer mit Leseberechtigung für Sicherheitsfunktionen | Sicherheitsadministrator
Aktualisieren des Ereignisstatus | Sicherheitsadministrator | 

## <a name="security---risky-sign-ins"></a>Sicherheit – Riskante Anmeldungen

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Lesen aller Konfigurationen | Sicherheit lesen | 
Lesen riskanter Anmeldevorgänge | Sicherheit lesen | 

## <a name="security---users-flagged-for-risk"></a>Sicherheit – Benutzer mit Risikokennzeichnung

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Verwerfen aller Ereignisse | Sicherheitsadministrator | 
Lesen aller Konfigurationen | Sicherheit lesen | 
Lesen von Benutzern mit Risikokennzeichnung | Sicherheit lesen | 

## <a name="users"></a>Benutzer

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Hinzufügen von Benutzern zur Verzeichnisrolle | Administrator für privilegierte Rollen | 
Hinzufügen von Benutzern zur Gruppe | Benutzerkontoadministrator | 
Lizenz zuweisen | Lizenzadministrator | Benutzerkontoadministrator
Erstellen eines Gastbenutzers | Gasteinladender | Benutzerkontoadministrator
Benutzer erstellen | Benutzerkontoadministrator | 
Löschen von Benutzern | Benutzerkontoadministrator | 
Ungültige Aktualisierungstoken von Administratoren mit eingeschränkten Berechtigungen (siehe Dokumentation) | Benutzerkontoadministrator | 
Ungültige Aktualisierungstoken von Benutzern ohne Administratorrechte (siehe Dokumentation) | Kennwortadministrator | Benutzerkontoadministrator
Ungültige Aktualisierungstoken von privilegierten Administratorrollen (siehe Dokumentation) | Globaler Administrator | 
Lesen einer Standardkonfiguration | Standardbenutzerrolle (siehe [Dokumentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Zurücksetzen des Kennworts für Administratoren mit eingeschränkten Berechtigungen (siehe Dokumentation) | Benutzerkontoadministrator | 
Zurücksetzen des Kennworts für Benutzer ohne Administratorrechte (siehe Dokumentation) | Kennwortadministrator | Benutzerkontoadministrator
Zurücksetzen des Kennworts von privilegierten Administratoren | Globaler Administrator | 
Widerrufen von Lizenzen | Lizenzadministrator | Benutzerkontoadministrator
Verwalten aller Eigenschaften mit Ausnahme des Benutzerprinzipalnamens | Benutzerkontoadministrator | 
Aktualisieren des Benutzerprinzipalnamens für Administratoren mit eingeschränkten Berechtigungen (siehe Dokumentation) | Benutzerkontoadministrator | 
Aktualisieren der Benutzerprinzipalnamens-Eigenschaft für Administratoren mit eingeschränkten Berechtigungen (siehe Dokumentation) | Globaler Administrator | 
Aktualisieren von Benutzereinstellungen | Globaler Administrator | 


## <a name="support"></a>Support

Aufgabe | Am wenigsten privilegierte Rolle | Zusätzliche Rollen
---- | --------------------- | ----------------
Einreichen eines Supporttickets | Dienstadministrator | Anwendungsadministrator, Rechnungsadministrator, Cloudanwendungsadministrator, Complianceadministrator, Dynamics 365-Administrator, Desktop Analytics-Administrator, Exchange-Administrator, Kennwortadministrator, Information Protection-Administrator, Intune-Administrator, Skype for Business-Administrator, Power BI-Administrator, Administrator für die privilegierte Authentifizierung, SharePoint-Administrator, Teams Communications-Administrator, Teams-Administrator, Benutzeradministrator, Workplace Analytics-Administrator

## <a name="next-steps"></a>Nächste Schritte

* [Anzeigen und Zuweisen von Administratorrollen in Azure Active Directory](directory-manage-roles-portal.md)
* [Berechtigungen von Administratorrollen in Azure Active Directory](directory-assign-admin-roles.md)