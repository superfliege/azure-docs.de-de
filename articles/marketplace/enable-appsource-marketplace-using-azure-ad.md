---
title: Aktivieren eines Microsoft AppSource- und Azure Marketplace-Eintrags mithilfe von Azure Active Directory | Azure
description: Aktivieren des Listingtyps mithilfe von Azure Active Directory im Azure Marketplace und in AppSource für Herausgeber von Apps und Diensten.
services: Azure, AppSource, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: qianw211
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/12/2018
ms.author: qianw211
ms.openlocfilehash: d7fd09928c0a687755d216e7f10f7eac23677c63
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45987341"
---
# <a name="enable-a-microsoft-appsource-and-azure-marketplace-listing-by-using-azure-active-directory"></a>Aktivieren eines Microsoft AppSource- und Azure Marketplace-Eintrags mithilfe von Azure Active Directory

Microsoft Azure Active Directory (Azure AD) ist ein Cloudidentitätsdienst, der die Authentifizierung mit einem Microsoft-Konto mithilfe der branchenüblichen Standardframeworks ermöglicht.  Weitere Informationen zu Azure AD finden Sie in [Azure Active Directory](https://azure.microsoft.com/services/active-directory).

## <a name="benefits-of-using-azure-active-directory"></a>Vorteile von Azure Active Directory

Microsoft AppSource- und Azure Marketplace-Kunden nutzen die produktinternen Umgebungen, um in den Listingkatalogen zu suchen. Dazu müssen sie sich beim Produkt anmelden.  Durch die Integration Ihrer Anwendung mit Azure AD können Sie die Kundenbindung beschleunigen und die Kundenzufriedenheit optimieren. Azure AD:

- Ermöglicht Single Sign-On (SSO) für Millionen von Unternehmen.
- Ermöglicht eine konsistente Umgebung für die Benutzeranmeldung über Anwendungen verschiedener Partnern hinweg.
- Bietet skalierbare, plattformübergreifende Authentifizierung für Ihre mobilen und Cloud-Apps.

Wie im folgenden Abschnitt beschrieben, sind bestimmte Angebote erforderlich, um Azure AD für die Veröffentlichung auf dem Marktplatz zu implementieren.

## <a name="azure-active-directory-requirements"></a>Azure Active Directory-Anforderungen

Es gibt verschiedene [Listingoptionen und Angebotstypen](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) für Microsoft AppSource und den Azure Marketplace.  Azure AD-Anforderungen für diese Listingoptionen und Angebotstypen werden unten angezeigt:

| **Angebotstyp**    | **AAD-SSO erforderlich?**  |  |   |  |
| :------------------- | :-------------------|:-------------------|:-------------------|:-------------------|
|  | Kontaktformular | Testversion | Testversion | Transaktion |
| Virtual Machine | N/V | Nein  | Nein  | Nein  |
| Azure-Apps (Lösungsvorlage)  | N/V | N/V | N/V | N/V |
| Verwaltete Apps  | N/V | N/V | N/V | Nein  |
| SaaS  | Nein  | Ja | Ja | JA |
| Container  | N/V | N/V | N/V | Nein  |
| Beratungsdienste  | Nein  | N/V | N/V | N/V |

Weitere Informationen zu den technischen Anforderungen für SaaS finden Sie unter [SaaS-Anwendungen: Leitfaden für die Veröffentlichung von Angeboten](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

## <a name="integration-with-azure-active-directory"></a>Integration in Azure Active Directory

Informationen zur Integration in Azure AD zur Aktivierung von SSO finden Sie unter https://aka.ms/aaddev.

Weitere Informationen zum SSO mit Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="enable-a-trial-listing-by-using-azure-active-directory"></a>Aktivieren einer Testversion mithilfe von Azure Active Directory

Wenn ein Kunde im Marketplace Ihr Listing vom Typ „Testversion“ ausgewählt hat, wird der Kunde an Ihre Testumgebung weitergeleitet. In Ihrer Testumgebung können Sie Ihren Kunden direkt und ohne weitere Anmeldeschritte einrichten. Ihre App oder Ihr Angebot empfängt während der Authentifizierung ein Token von Azure AD. Das Token enthält wichtige Benutzerinformationen, die für die Erstellung eines Benutzerkontos in Ihrer App oder Ihrem Angebot verwendet wird. Sie können die Einrichtung der Kunden automatisieren und die Wahrscheinlichkeit einer Konversion erhöhen.

Weitere Informationen zum Token, das während der Authentifizierung von Azure AD gesendet wird, finden Sie in [Beispieltoken](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Verwenden Sie Azure AD, um in Ihrer App oder Testversion die Authentifizierung mit nur einem Klick zu ermöglichen. Mit Azure AD bietet Ihnen die folgenden Vorteile: 
*   Optimieren Sie das Benutzererlebnis zwischen Marketplace und Testumgebung.
*   Auch wenn der Benutzer vom Marketplace an Ihre Domäne oder Testumgebung umgeleitet wird, hat er weiterhin das Gefühl, sich innerhalb des gleichen Produkts zu befinden.
*   Da keine weiteren Anmeldeschritte erforderlich sind, sinkt die Wahrscheinlichkeit, dass der Kunde Ihr Angebot wieder verlässt.
*   Senken Sie Bereitstellungshürden für die große Anzahl von Azure AD-Benutzern.

### <a name="verify-your-azure-ad-integration-in-the-marketplace-multitenant-apps"></a>Überprüfen Ihrer Azure AD-Integration im Marketplace: mehrinstanzenfähige Apps
Mit Azure AD können Sie die folgenden Optionen für Ihre Lösung unterstützen:
*   Registrieren Sie Ihre App in den Storefronts im Marketplace.
*   Aktivieren Sie das Feature für die Unterstützung der Mehrinstanzenfähigkeit in Azure AD, um für die Testversion die Nutzung per einfachem Klick zu ermöglichen.

Weitere Informationen zur App-Registrierung finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

Wenn Sie das einmalige Anmelden (Single Sign-On, SSO) im Azure AD-Verbund bisher noch nicht verwendet haben, führen Sie diese Schritte aus:
1.  Registrieren Sie Ihre App im Marketplace. 
2.  Richten Sie das einmalige Anmelden mit Azure AD mithilfe von OAuth 2.0 oder OpenID Connect ein.
    *   Weitere Informationen zu OAuth 2.0 finden Sie unter [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
    *   Weitere Informationen zu Open ID Connect finden Sie unter [OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).
3.  Aktivieren Sie das Feature für die Unterstützung der Mehrinstanzenfähigkeit in Azure AD, um für Ihre Testversion die Nutzung per einfachem Klick zu ermöglichen.
    
    Weitere Informationen zu AppSource-Zertifizierung finden Sie unter [AppSource-Zertifizierung](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified). 

### <a name="verify-your-azure-ad-integration-in-the-marketplace-single-tenant-apps"></a>Überprüfen Sie Ihre Azure AD-Integration im Marketplace: Einzelmandanten-Apps
Mit Azure AD können Sie die folgenden Optionen für Ihre Einzelmandantenlösung unterstützen: 
*   Fügen Sie Benutzern mithilfe von Azure Active Directory B2B (Azure AD B2B) als Gastbenutzer zu Ihrem Verzeichnis hinzu. Weitere Informationen zu Azure AD B2B finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Sie können auch manuell Testversionen für Kunden einrichten, indem Sie die Veröffentlichungsoption „Kontaktformular“ auswählen.
*   Entwickeln einer Testversion pro Kunde
*   Erstellen einer mehrinstanzenfähigen Beispieldemo-App, die SSO verwendet.

## <a name="next-steps"></a>Nächste Schritte

Falls Sie dies noch nicht getan haben, 
- [Registrieren Sie](https://azuremarketplace.microsoft.com/sell) beim Marketplace.

Wenn Sie registriert sind und ein neues Angebot erstellen oder an einem vorhandenen arbeiten,
- [melden Sie sich beim Cloud-Partnerportal an](https://cloudpartner.azure.com/), um Ihr Angebot zu erstellen oder zu vervollständigen.

