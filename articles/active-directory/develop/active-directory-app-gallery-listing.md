---
title: Ihre Anwendung im Azure Active Directory-Anwendungskatalog auflisten
description: "Auflisten einer Anwendung, die einmaliges Anmelden unterstützt, im Azure Active Directory-Katalog | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/09/2018
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: feb09aa8f8e22ad6fbda6a490d251c500bedf3ee
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Ihre Anwendung im Azure Active Directory-Anwendungskatalog auflisten


##  <a name="what-is-azure-ad-app-gallery"></a>Was ist der Azure AD-App-Katalog?

Azure AD ist ein cloudbasierter Identitätsdienst. [Azure AD-App-Katalog](https://azure.microsoft.com/marketplace/active-directory/all/) ist ein allgemeiner Store, in dem alle Anwendungsconnectors für einmaliges Anmelden und Benutzerbereitstellung veröffentlicht werden. Unsere gemeinsamen Kunden, die Azure AD als Identitätsanbieter einsetzen, suchen nach verschiedenen SaaS-Anwendungsconnectors, die hier veröffentlicht werden. IT-Administratoren fügen Connectors aus dem App-Katalog hinzu, konfigurieren diese und verwenden sie für einmaliges Anmelden und Bereitstellungen. Dabei unterstützt Azure AD alle wichtigen Verbundprotokolle wie SAML 2.0, OpenID Connect, OAuth und WS-Fed für einmaliges Anmelden. 

## <a name="what-are-the-benefits-of-listing-the-application-in-the-gallery"></a>Welche Vorteile bietet eine Auflistung der Anwendung im Katalog?

*  Bestmögliches Kundenerlebnis beim einmaligen Anmelden

*  Einfache und minimale Konfiguration der Anwendung

*  Leichte Auffindbarkeit der Anwendung im Katalog durch den Kunden 

*  Die Nutzung dieser Integration ist unabhängig von der vorhandenen Azure AD-SKU (Free, Basic oder Premium).

*  Tutorials zur schrittweisen Konfiguration für die gemeinsamen Kunden

*  Möglichkeit der Benutzerbereitstellung für dieselbe App bei Verwendung von SCIM


##  <a name="what-are-the-pre-requisites"></a>Was sind die Voraussetzungen?

Damit eine Anwendung in der Azure AD-Galerie aufgelistet werden kann, muss diese zunächst eines der von Azure AD unterstützten Verbundprotokolle implementieren. Lesen Sie die Geschäftsbedingungen des Azure AD-App-Katalogs. Je nachdem, welches Verbundprotokoll Sie verwenden, gehen Sie wie folgt vor: 

*   **OpenID Connect** – Erstellen Sie die mehrinstanzenfähige Anwendung in Azure AD, und implementieren Sie das [Azure AD-Genehmigungsframework](active-directory-integrating-applications.md#overview-of-the-consent-framework) für Ihre Anwendung. Senden Sie die Anmeldeanforderung an einen gemeinsamen Endpunkt, damit jeder Kunde der Anwendung zustimmen kann. Sie können den Benutzerzugriff des Kunden anhand der Mandanten-ID und der im Token erhaltenen UPN des Benutzers steuern. Befolgen Sie zum Integrieren Ihrer Anwendung in Azure AD diese [Anweisungen für Entwickler](active-directory-authentication-scenarios.md).

*   **SAML 2.0 oder WS-Fed** – Ihre Anwendung sollte über eine Funktion zur SAML/WS-Fed-SSO-Integration im SP- oder IDP-Modus verfügen. Jede App, die SAML 2.0 unterstützt, kann direkt in einen Azure AD-Mandanten integriert werden. Dazu befolgen Sie diese [Anweisungen zum Hinzufügen einer benutzerdefinierten Anwendung](../active-directory-saas-custom-apps.md).

*   **Kennwortbasiertes SSO** – Erstellen Sie eine Webanwendung mit HTML-Anmeldeseite, um das [kennwortbasierte einmalige Anmelden](../active-directory-appssoaccess-whatis.md) zu konfigurieren. Das kennwortbasierte einmalige Anmelden (auch als „Password Vaulting“ oder „Kennworttresor“ bezeichnet) ermöglicht es Ihnen, den Benutzerzugriff und die Kennwörter für Webanwendungen zu verwalten, die keinen Identitätsverbund unterstützen. Es ist auch für Szenarien nützlich, in denen mehrere Benutzer ein Konto gemeinsam verwenden müssen, wie z. B. bei den App-Konten für die sozialen Medien Ihrer Organisation. 

## <a name="process-for-submitting-the-request-in-the-portal"></a>Prozess für die Übermittlung der Anforderung im Portal

Nachdem Sie getestet haben, dass Ihre Anwendungsintegration mit Azure AD funktioniert, müssen Sie Ihre Anforderung für den Zugriff auf unser [Anwendungsnetzwerkportal](https://microsoft.sharepoint.com/teams/apponboarding/Apps) übermitteln. Wenn Sie über ein Office 365-Konto verfügen, können Sie sich damit bei diesem Portal anmelden. Verwenden Sie andernfalls Ihre Microsoft-ID (Live ID, Outlook, Hotmail usw.), um sich anzumelden. Auf der folgenden Seite können Sie Ihren Zugriff anfordern. Geben Sie in das Textfeld eine geschäftliche Begründung ein, und klicken Sie auf **Zugriff anfordern**. Unser Team wird alle Details prüfen und Ihnen den Zugriff entsprechend gewähren. Danach können Sie sich beim Portal anmelden und Ihre detaillierte Anforderung für die Anwendung übermitteln.

Wenn Sie Probleme mit dem Zugriff haben, wenden Sie sich an das [Azure AD-SSO-Integrationsteam](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![Zugriffsanforderung im SharePoint-Portal](./media/active-directory-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Zeitpläne
    
*   Prozess der Auflistung einer SAML 2.0- oder WS-Fed-Anwendung in der Galerie: **7 bis 10 Werktage**

   ![Zeitachse für Auflistung einer SAML-Anwendung in der Galerie](./media/active-directory-app-gallery-listing/timeline.png)

*   Prozess der Auflistung einer OpenID Connect-Anwendung in der Galerie: **2 bis 5 Werktage**

   ![Zeitachse für Auflistung einer SAML-Anwendung in der Galerie](./media/active-directory-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Eskalationen

Bei Eskalationen wenden Sie sich per E-Mail an das [Azure AD SSO-Integrationsteam](<mailto:SaaSApplicationIntegrations@service.microsoft.com>), das sich umgehend bei Ihnen meldet.

