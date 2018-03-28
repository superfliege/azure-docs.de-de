---
title: Plug-In zum einmaligen Anmelden mit Microsoft Azure Active Directory – Administratorhandbuch | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und Microsoft Azure Active Directory single sign-on for JIRA konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: jeedes
ms.openlocfilehash: af949d1db8af37a534a16364f9f0763479c436e4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="microsoft-azure-active-directory-single-sign-on-plugin-admin-guide"></a>Plug-In zum einmaligen Anmelden mit Microsoft Azure Active Directory – Administratorhandbuch

## <a name="table-of-contents"></a>Inhaltsverzeichnis

1. **[ÜBERSICHT](#overview)**
2. **[FUNKTIONSWEISE](#how-it-works)**
3. **[ZIELGRUPPE](#audience)**
4. **[ANNAHMEN](#assumptions)**
5. **[VORAUSSETZUNGEN](#prerequisites)**
6. **[UNTERSTÜTZTE VERSIONEN VON JIRA UND CONFLUENCE](#supported-versions-of-jira-and-confluence)**
7. **[INSTALLATION](#installation)**
8. **[PLUG-IN-KONFIGURATION](#plugin-configuration)**
9. **[FELDERLÄUTERUNG ZUM ADD-ON-KONFIGURATIONSBILDSCHIRM:](#field-explanation-for-add---on-configuration-screen:)**
10. **[PROBLEMBEHANDLUNG](#troubleshooting)**

## <a name="overview"></a>Übersicht

Diese Add-Ons ermöglichen Microsoft Azure AD-Kunden, sich mit ihrem Organisationsbenutzernamen und -kennwort bei Produkten auf Atlassian Jira- und Confluence-Basis anzumelden. Es wird SSO auf SAML 2.0-Basis implementiert.

## <a name="how-it-works"></a>So funktioniert's

Wenn Benutzer sich bei einer Atlassian Jira- oder Confluence-Anwendung anmelden möchten, sehen sie die Schaltfläche **Anmelden mit Azure AD** auf der Anmeldeseite. Wenn sie darauf klicken, werden sie aufgefordert, sich mit der Azure AD-Organisationsanmeldeseite anzumelden.

Sobald die Benutzer authentifiziert sind, sollten sie sich bei der Anwendung anmelden können. Wenn sie bereits mit Organisations-ID und Kennwort authentifiziert sind, melden sie sich direkt bei der Anwendung an. Beachten Sie auch, dass diese Anmeldung in JIRA und Confluence funktioniert. Wenn Benutzer bei einer JIRA-Anwendung angemeldet sind und Confluence im selben Browserfenster geöffnet ist, müssen sie sich einmal anmelden und die Anmeldeinformationen nicht erneut für eine andere App bereitstellen. Die Benutzer können auch unter dem Azure-Konto über MyApps direkt zu dem Atlassian-Produkt gelangen und sollten angemeldet werden, ohne zur Eingabe der Anmeldeinformationen aufgefordert zu werden.

> [!NOTE]
> Die Benutzerbereitstellung erfolgt nicht mithilfe dieses Add-Ons.

## <a name="audience"></a>Zielgruppe

JIRA- und Confluence-Administratoren, die dieses Plug-In zum Aktivieren von SSO mithilfe von Azure AD verwenden möchten.

## <a name="assumptions"></a>Annahmen

* JIRA-/Confluence-Instanz ist HTTPS-tauglich.
* Benutzer sind bereits in JIRA/Confluence erstellt.
* Benutzern sind Rollen in JIRA/Confluence zugewiesen.
* Administratoren haben Zugriff auf Informationen, die zum Konfigurieren des Plug-Ins erforderlich sind.
* JIRA/Confluence ist auch außerhalb des Unternehmensnetzwerks verfügbar.
* Das Add-On funktioniert nur mit der lokalen Version von JIRA und Confluence.

## <a name="prerequisites"></a>Voraussetzungen

Beachten Sie folgende Voraussetzungen, bevor Sie mit der Add-On-Installation fortfahren:

* JIRA/Confluence sind auf einer 64-Bit-Version von Windows installiert.
* Die Versionen von JIRA/Confluence sind HTTPS-tauglich.
* Beachten Sie die unterstützte Version für das Plug-In im Abschnitt „Unterstützte Versionen“ weiter unten.
* JIRA/Confluence ist im Internet verfügbar.
* Administratoranmeldeinformationen für JIRA/Confluence
* Anmeldeinformationen für Azure AD
* WebSudo sollte in JIRA und Confluence deaktiviert werden.

## <a name="supported-versions-of-jira-and-confluence"></a>Unterstützte Versionen von JIRA und Confluence

Zum aktuellen Zeitpunkt werden die folgenden Versionen von JIRA und Confluence unterstützt:

* JIRA Core und Software: 6.0 bis 7.2.0
* JIRA Service Desk: 3.0 bis 3.2
* Confluence: 5.0 bis 5.10

## <a name="installation"></a>Installation

Der Administrator sollte das Plug-In mit folgenden Schritten installieren:

1. Melden Sie sich mit Ihrer JIRA/Confluence-Instanz als Administrator an.
    
2. Wechseln Sie zu „JIRA/Confluence-Verwaltung“, und klicken Sie auf „Add-Ons“.
    
3. Suchen Sie vom Atlassian Marketplace aus nach **Microsoft SAML-SSO-Plug-In**.
 
4. Die geeignete Version des Add-Ons wird in der Suche angezeigt.
 
5. Wählen Sie das Plug-In aus, und UPM installiert dasselbe.
 
6. Sobald das Plug-In installiert ist, wird es im Abschnitt „Add-Ons verwalten“ unter „Vom Benutzer installiert“ angezeigt.
 
7. Sie müssen das Plug-In konfigurieren, bevor Sie es verwenden.
 
8. Wenn Sie auf das Plug-In klicken, wird eine Konfigurationsschaltfläche angezeigt.
 
9. Klicken Sie darauf, um Konfigurationseingaben bereitzustellen.
    
## <a name="plugin-configuration"></a>Plug-In-Konfiguration

Die folgende Abbildung zeigt den Bildschirm zur Add-On-Konfiguration in JIRA und Confluence.
    
![Add-On-Konfiguration](./media/ms-confluence-jira-plugin-adminguide/jira.png)

### <a name="field-explanation-for-add-on-configuration-screen"></a>Felderläuterung zum Add-On-Konfigurationsbildschirm:

1.   Metadaten-URL: Die URL zum Abrufen der Verbundmetadaten von Azure AD
 
2.   Bezeichner: Wird von Azure AD zum Überprüfen der Quelle der Anforderung verwendet. Dies wird dem Bezeichnerelement in Azure AD zugeordnet. Dies wird vom Plug-In automatisch als „https://<Domäne:Port>/“ abgeleitet.
 
3.   Antwort-URL: Verwenden Sie die Antwort-URL in Ihrer IdP zum Initiieren der SAML-Anmeldung. Dies wird dem Antwort-URL-Element in Azure AD zugeordnet. Dies wird vom Plug-In automatisch als „https://<Domäne:Port>//plugins/servlet/saml/auth“ abgeleitet.
 
4.   Anmelde-URL: Verwenden Sie die Anmelde-URL in Ihrer IdP zum Initiieren der SAML-Anmeldung. Dies wird dem Anmelde-URL-Element in Azure AD zugeordnet. Dies wird vom Plug-In automatisch als „https://<Domäne:Port>//plugins/servlet/saml/auth“ abgeleitet.
 
5.   IdP-Entitäts-ID: Die Entitäts-ID, die Ihr IdP verwendet. Dies wird gefüllt, wenn die Metadaten-URL aufgelöst ist.
 
6.   Anmelde-URL: Die Anmelde-URL aus Ihrem IdP. Dies wird aus Azure AD gefüllt, wenn die Metadaten-URL aufgelöst ist.
 
7.   Abmelde-URL: Die Abmelde-URL aus Ihrem IdP. Dies wird aus Azure AD gefüllt, wenn die Metadaten-URL aufgelöst ist.
 
8.   X. 509-Zertifikat: Das X.509-Zertifikat Ihres IdP. Dies wird aus Azure AD gefüllt, wenn die Metadaten-URL aufgelöst ist.
 
9.   Anmeldeschaltflächen-Name: Benennen Sie die Anmeldeschaltfläche, die Ihre Organisation anzeigen möchte. Dieser Text wird Benutzern auf dem Anmeldebildschirm auf der Anmeldeschaltfläche angezeigt.
 
10.   SAML-Benutzer-ID-Speicherorte: Wo die Benutzer-ID in der SAML-Antwort erwartet wird. Dies kann entweder in NameID oder in einem benutzerdefinierten Attributnamen sein. Diese ID muss die JIRA/Confluence-Benutzer-ID sein.
 
11.   Attributname: Name des Attributs, in dem die Benutzer-ID erwartet werden kann.
 
12.   Startbereichsermittlung aktivieren: Aktivieren Sie dieses Flag, wenn das Unternehmen die ADFS-basierte Anmeldung verwendet.
 
13.   Domänenname: Geben Sie hier im Fall einer auf ADFS basierenden Anmeldung den Domänennamen ein.
 
14.   Einmaliges Abmelden aktivieren: Aktivieren Sie diese Option, wenn Sie sich bei Azure AD abmelden möchten, wenn ein Benutzer sich bei JIRA/Confluence abmeldet.

### <a name="troubleshooting"></a>Problembehandlung

* Fehlermeldung, dass mehrere Zertifikate vorliegen
    
    * Melden Sie sich bei Azure AD an, und entfernen Sie mehrere Zertifikate, die für die App verfügbar sind. Stellen Sie sicher, dass nur ein Zertifikat vorhanden ist.

* Das Zertifikat läuft bald in Azure AD ab.
    
    * Add-Ons tragen Sorge für den automatischen Rollover des Zertifikats. Wenn ein Zertifikat in Kürze abläuft, sollte das neue Zertifikat als aktiv gekennzeichnet werden, und das nicht verwendete Zertifikat sollte gelöscht werden. Wenn ein Benutzer versucht, sich in diesem Szenario bei JIRA anzumelden, ruft das Add-On das neue Zertifikat ab und speichert es im Plug-In.

* Gewusst wie: Deaktivieren von WebSudo (Deaktivieren der sicheren Administratorsitzung)
    
    * JIRA: Sichere Administratorsitzungen (d.h. Bestätigung des Kennworts vor dem Zugriff auf die Verwaltungsfunktionen) sind standardmäßig aktiviert. Wenn Sie dies in Ihrer JIRA-Instanz deaktivieren möchten, können Sie dieses Feature mittels folgender Zeile in der Datei „jira-config.properties“ deaktivieren: „ira.websudo.is.disabled = true“.
    
    * Confluence: Gehen Sie wie in der folgenden URL gezeigt vor: https://confluence.atlassian.com/doc/configuring-secure-administrator-sessions-218269595.html.

* Felder, die von der Metadaten-URL aufgefüllt werden sollten, werden nicht aufgefüllt
    
    * Überprüfen Sie, ob die URL richtig ist. Überprüfen Sie, ob Sie den richtigen Mandanten und die richtige App-ID zugeordnet haben.
    
    * Rufen Sie die URL im Browser auf, und stellen Sie fest, ob Sie die Verbundmetadaten-XML empfangen.

* Interner Serverfehler:
    
    * Sehen Sie die Protokolle in den Protokollverzeichnissen der Installation durch. Falls der Fehler gemeldet wird, wenn der Benutzer versucht, sich mittels Azure AD-SSO anzumelden, können Sie die Protokolle gemeinsam mit dem Support auswerten, dessen Link Sie weiter unten in diesem Dokument finden.

* Benutzer-ID nicht gefunden, wenn der Benutzer versucht, sich anzumelden
    
    * Der Benutzer ist nicht in JIRA/Confluence erstellt, also erstellen Sie ihn.

* App wurde in Azure AD nicht gefunden
    
    * Stellen Sie fest, ob die entsprechende URL in Azure AD der App zugeordnet ist.

* Supportinformationen: Sie erreichen uns unter: [Azure AD-SSO-Integrationsteam](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). Wir antworten innerhalb von 24 bis 48 Geschäftsstunden.
    
    * Sie können auch bei Microsoft ein Supportticket über den Azure-Portalkanal erstellen.