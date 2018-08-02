---
title: Aktivieren einer Testversion im Azure Marketplace mithilfe von Azure Active Directory | Azure
description: Aktivieren des Listingtyps „Testversion“ mithilfe von Azure Active Directory im Azure Marketplace und in AppSource für Herausgeber von Apps und Diensten.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/04/2018
ms.author: ellacroi
ms.openlocfilehash: c5b7b4967c1acef733d366e651d50706db42aace
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160466"
---
# <a name="enable-a-trial-listing-by-using-azure-active-directory"></a>Aktivieren einer Testversion mithilfe von Azure Active Directory

Azure Active Directory (Azure AD) ist ein Cloudidentitätsdienst, der die Authentifizierung mit einem Geschäfts-, Schul- oder Unikonto von Microsoft mithilfe der branchenüblichen Standardframeworks ermöglicht. Azure AD unterstützt OAuth- und OpenID Connect-Authentifizierung. Der [Azure Marketplace](https://azuremarketplace.microsoft.com) verwendet Azure AD, um sie und Ihre Kunden zu authentifizieren.

Weitere Informationen zu Azure AD finden Sie in [Azure Active Directory](https://azure.microsoft.com/services/active-directory).

Wenn ein Kunde im Marketplace Ihr Listing vom Typ „Testversion“ ausgewählt hat, wird der Kunde an Ihre Testumgebung weitergeleitet. In Ihrer Testumgebung können Sie Ihren Kunden direkt und ohne weitere Anmeldeschritte einrichten. Ihre App oder Ihr Angebot empfängt während der Authentifizierung ein Token von Azure AD. Das Token enthält wichtige Benutzerinformationen, die für die Erstellung eines Benutzerkontos in Ihrer App oder Ihrem Angebot verwendet wird. Sie können die Einrichtung der Kunden automatisieren und die Wahrscheinlichkeit einer Konversion erhöhen.

Weitere Informationen zum Token, das während der Authentifizierung von Azure AD gesendet wird, finden Sie in [Beispieltoken](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Verwenden Sie Azure AD, um in Ihrer App oder Testversion die Authentifizierung mit nur einem Klick zu ermöglichen. Mit Azure AD bietet Ihnen die folgenden Vorteile: 
*   Optimieren Sie das Benutzererlebnis zwischen Marketplace und Testumgebung.
*   Auch wenn der Benutzer vom Marketplace an Ihre Domäne oder Testumgebung umgeleitet wird, hat er weiterhin das Gefühl, sich innerhalb des gleichen Produkts zu befinden.
*   Da keine weiteren Anmeldeschritte erforderlich sind, sinkt die Wahrscheinlichkeit, dass der Kunde Ihr Angebot wieder verlässt.
*   Senken Sie Bereitstellungshürden für die große Anzahl von Azure AD-Benutzern.

## <a name="verify-your-azure-ad-integration-in-the-marketplace-multitenant-apps"></a>Überprüfen Ihrer Azure AD-Integration im Marketplace: mehrinstanzenfähige Apps
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

## <a name="verify-your-azure-ad-integration-in-the-marketplace-single-tenant-apps"></a>Überprüfen Sie Ihre Azure AD-Integration im Marketplace: Einzelmandanten-Apps
Mit Azure AD können Sie die folgenden Optionen für Ihre Einzelmandantenlösung unterstützen: 
*   Fügen Sie Benutzern mithilfe von Azure Active Directory B2B (Azure AD B2B) als Gastbenutzer zu Ihrem Verzeichnis hinzu.
    
    Weitere Informationen zu Azure AD B2B finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Sie können auch manuell Testversionen für Kunden einrichten, indem Sie die Veröffentlichungsoption „Kontaktformular“ auswählen.
*   Entwickeln einer Testversion pro Kunde
*   Erstellen einer mehrinstanzenfähigen Beispiel-Demo-App, die SSO verwendet.

## <a name="next-steps"></a>Nächste Schritte
*   Lesen Sie den [Veröffentlichungsleitfaden für Azure Marketplace und AppSource](./marketplace-publishers-guide.md).
