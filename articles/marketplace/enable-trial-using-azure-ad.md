---
title: Aktivieren des Typs „Testversion“ mithilfe von Azure AD | Azure
description: Aktivieren des Listingtyps „Testversion“ mithilfe von Azure Active Directory (Azure AD) im Azure Marketplace und in AppSource für Herausgeber von Apps und Diensten
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
ms.openlocfilehash: 4140ba98c0c65c22674c61dc7266818af904e777
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34826061"
---
# <a name="enable-trial-using-azure-ad"></a>Aktivieren des Typs „Testversion“ mithilfe von Azure AD  
Azure Active Directory (Azure AD) ist ein Cloudidentitätsdienst, der die Authentifizierung mit einem Geschäfts-, Schul- oder Unikonto von Microsoft mithilfe der branchenüblichen Standardframeworks „OAuth“ und „OpenID Connect“ ermöglicht.  
*   Weitere Informationen zu Azure AD finden Sie auf der Azure Active Directory-Seite unter [azure.microsoft.com/services/active-directory](https://azure.microsoft.com/services/active-directory).  

Sie und Ihre Kunden werden im Marketplace mithilfe von Azure AD authentifiziert. Wenn Ihr Kunde im Marketplace Ihr Listing vom Typ „Testversion“ ausgewählt hat, wird der Kunde an Ihre Testumgebung weitergeleitet.  In Ihrer Testumgebung können Sie Ihren Kunden direkt und ohne weitere Anmeldeschritte einrichten. Ihre App oder Ihr Angebot erhält während der Authentifizierung ein Token von Azure AD, das wertvolle Benutzerinformationen zum Erstellen eines Benutzerkontos in Ihrer App oder Ihrem Angebot enthält. Sie können die Einrichtung automatisieren und die Wahrscheinlichkeit einer Konversion erhöhen.  
*   Weitere Informationen zu dem Token, das während der Authentifizierung von Azure AD gesendet wird, finden Sie im Abschnitt „Beispieltoken“ unter [docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Verwenden Sie Azure AD, um in Ihrer App oder Testversion die Authentifizierung mit nur einem Klick zu ermöglichen.  
*   Optimieren Sie das Benutzererlebnis zwischen Marketplace und Testumgebung.  
*   Auch wenn der Benutzer vom Marketplace an Ihre Domäne oder Testumgebung umgeleitet wird, hat er weiterhin das Gefühl, sich innerhalb des gleichen Produkts zu befinden.  
*   Da keine weiteren Anmeldeschritte erforderlich sind, sinkt die Wahrscheinlichkeit, dass der Kunde Ihr Angebot wieder verlässt.  
*   Senken Sie Bereitstellungshürden für die große Anzahl von Azure AD-Benutzern.  

## <a name="verify-your-azure-ad-integration-on-the-marketplace-multitenant-apps"></a>Überprüfen Ihrer Azure AD-Integration im Marketplace: mehrinstanzenfähige Anwendungen  
Unterstützen Sie die folgenden Optionen für Ihre Lösung mit Azure AD.  
*   Registrieren Sie Ihre App in den Storefronts im Marketplace.  
*   Aktivieren Sie das Feature für die Unterstützung der Mehrinstanzenfähigkeit in Azure AD, um für die Testversion die Nutzung per einfachem Klick zu ermöglichen.  
    *   Weitere Informationen zur App-Registrierung finden Sie auf der Seite „Integrieren von Anwendungen in Azure Active Directory“ unter [docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  

Wenn Sie das einmalige Anmelden (Single Sign-On, SSO) im Azure AD-Verbund bisher noch nicht verwendet haben, führen Sie folgende Schritte aus.  
1.  Registrieren Sie Ihre App im Marketplace. 
2.  Richten Sie das einmalige Anmelden mit Azure AD mithilfe von OAuth 2.0 oder OpenID Connect ein.  
    *   Weitere Informationen zu OAuth 2.0 finden Sie auf der OAuth 2.0-Seite unter [docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).  
    *   Weitere Informationen zu Open ID Connect, finden Sie auf der OpenID Connect-Seite unter [docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).  
3.  Aktivieren Sie das Feature für die Unterstützung der Mehrinstanzenfähigkeit in Azure AD, um für Ihre Testversion die Nutzung per einfachem Klick zu ermöglichen.  
    *   Weitere Informationen zur AppSource-Zertifizierung finden Sie auf der Seite „Zertifizieren von AppSource für Azure Active Directory“ unter [docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified). 

## <a name="verify-your-azure-ad-integration-on-the-marketplace-single-tenant-apps"></a>Überprüfen Sie Ihre Azure AD-Integration im Marketplace: Einzelmandanten-Apps  
Unterstützen Sie eine der folgenden Optionen für Ihre Einzelmandantenlösung.  
*   Fügen Sie Benutzern mithilfe von Azure AD B2B als Gastbenutzer zu Ihrem Verzeichnis hinzu.  
    *   Weitere Informationen zu Azure AD B2B finden Sie auf der Seite „Was ist die Azure AD B2B-Zusammenarbeit?“ unter [docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Richten Sie unter Verwendung der Kontaktaufnahme manuell Testversionen für Kunden ein.  
*   Entwickeln Sie eine Testversion pro Kunde.  
*   Erstellen Sie eine mehrinstanzenfähigen Beispiel-Demo-App mit SSO.  

## <a name="next-steps"></a>Nächste Schritte
*   Sehen Sie sich den [Herausgeberleitfaden für Azure Marketplace und AppSource](./marketplace-publishers-guide.md) an.  
 
---  

