---
title: Sitzungs- und SSO-Konfiguration in Azure Active Directory B2C | Microsoft-Dokumentation
description: Sitzungs- und SSO-Konfiguration in Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: daff37ca803a7eebdc07a78d6d02f5cee36de2f5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703812"
---
# <a name="session-and-single-sign-on-configuration-in-azure-active-directory-b2c"></a>Sitzungs- und SSO-Konfiguration in Azure Active Directory B2C

Diese Funktion ermöglicht Ihnen eine präzisere Steuerung der folgenden Einstellungen auf [Basis eines Benutzerflows](active-directory-b2c-reference-policies.md):

- Lebensdauer von Webanwendungssitzungen, die von Azure AD B2C verwaltet werden.
- SSO-Verhalten über verschiedene Apps und Benutzerflows in Ihrem Azure AD B2C-Mandanten hinweg.

## <a name="session-behavior"></a>Sitzungsverhalten

Azure AD B2C unterstützt das [OpenID Connect-Authentifizierungsprotokoll](active-directory-b2c-reference-oidc.md) zum Aktivieren der sicheren Anmeldung bei Webanwendungen. Sie können die folgenden Eigenschaften zum Verwalten von Webanwendungssitzungen verwenden:

- **Lebensdauer der Web-App-Sitzung (Minuten)**: Die Gültigkeitsdauer von Azure AD B2C-Sitzungscookies, die nach erfolgreicher Authentifizierung des Benutzers im Browser gespeichert werden
    - Standardwert: 1.440 Minuten.
    - Minimum (inklusive): 15 Minuten.
    - Maximum (inklusive): 1.440 Minuten.
- **Timeout für Web-App-Sitzung**: Wenn diese Option auf **Absolut** festgelegt ist, muss der Benutzer sich nach dem in **Lebensdauer der Web-App-Sitzung (Minuten)** angegebenen Zeitraum erneut authentifizieren. Wenn diese Option auf **Rollen** festgelegt ist (Standardeinstellung), bleibt der Benutzer angemeldet, solange er ständig in Ihrer Webanwendung aktiv ist.

Mit diesen Eigenschaften werden die folgenden Anwendungsfälle ermöglicht:

- Erfüllen Sie die Sicherheits- und Complianceanforderungen Ihrer Branche durch Festlegung der entsprechenden Lebensdauer für Webanwendungssitzungen.
- Erzwingen Sie die Authentifizierung nach einem festgelegten Zeitraum während der Interaktion eines Benutzers mit einer Komponente Ihrer Webanwendung, die eine hohe Sicherheit erfordert. 

Diese Einstellungen sind für Benutzerflows zur Kennwortzurücksetzung nicht verfügbar.

## <a name="single-sign-on-sso-configuration"></a>Einmaliges Anmelden (Single Sign-on, SSO) – Konfiguration

Wenn Sie in Ihrem B2C-Mandanten über mehrere Anwendungen und Benutzerflows verfügen, können Sie die Interaktionen der Benutzer mithilfe der Eigenschaft **Konfiguration des einmaligen Anmeldens** übergreifend verwalten. Sie können die Eigenschaft auf eine der folgenden Optionen festlegen:

- **Mandant**: Dies ist die Standardeinstellung. Mit dieser Einstellung können mehrere Anwendungen und Benutzerflows in Ihrem B2C-Mandanten die gleiche Benutzersitzung gemeinsam nutzen. Beispiel: Sobald sich ein Benutzer bei einer Anwendung angemeldet hat, erfolgt die Anmeldung bei einer anderen Anwendung (etwa Contoso Pharmacy) nahtlos beim Zugriff auf die Anwendung.
- **Anwendung**: Mit dieser Einstellung können Sie eine Benutzersitzung ausschließlich für eine Anwendung beibehalten, unabhängig von anderen Anwendungen. Verwenden Sie diese Einstellung beispielsweise, wenn Sie möchten, dass sich der Benutzer (mit denselben Anmeldeinformationen) bei Contoso Pharmacy anmeldet – auch wenn der Benutzer bereits bei Contoso Shopping angemeldet ist, einer anderen Anwendung im selben B2C-Mandanten. 
- **Richtlinie**: Mit dieser Einstellung können Sie eine Benutzersitzung ausschließlich für einen Benutzerflow beibehalten, unabhängig von den Anwendungen, die diesen verwenden. Wenn der Benutzer sich beispielsweise bereits angemeldet und einen Multi-Factor Authentication-Schritt (MFA) abgeschlossen hat, kann er in mehreren Anwendungen Zugriff auf Komponenten mit höherer Sicherheit erhalten, solange die an den Benutzerflow gebundene Sitzung nicht abläuft.
- **Deaktiviert**: Diese Einstellung erzwingt, dass der Benutzer bei jeder Ausführung der Richtlinie den gesamten Benutzerflow durchlaufen muss.

Diese Einstellungen sind für Benutzerflows zur Kennwortzurücksetzung nicht verfügbar. 

