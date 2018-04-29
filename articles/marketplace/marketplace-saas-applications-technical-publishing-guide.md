---
title: Technischer Leitfaden für die Veröffentlichung von Azure Marketplace-SaaS-Anwendungen
description: Schrittanleitung und Prüflisten für die Veröffentlichung von SaaS-Anwendungen im Azure Marketplace
services: Marketplace, Compute, Storage, Networking, Blockchain, Security, SaaS
documentationcenter: ''
author: BrentL-Collabera
manager: ''
editor: BrentL-Collabera
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/28/2018
ms.author: pabutler
ms.openlocfilehash: eb6db45ca0fcb6879aeaeaaf70715691cac438b0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="saas-applications-technical-publishing-guide"></a>Technischer Leitfaden für die Veröffentlichung von SaaS-Anwendungen

Willkommen beim technischen Leitfaden für die Veröffentlichung von Azure Marketplace-SaaS-Anwendungen. Dieser Leitfaden soll zukünftige und bestehende Herausgeber dabei unterstützen, ihre Anwendungen und Dienste mithilfe des Angebots für SaaS-Anwendungen im Azure Marketplace zu listen.  
Das Angebot für SaaS-Anwendungen empfiehlt sich, wenn Ihre Lösung in Ihrem eigenen Azure-Abonnement bereitgestellt wird und Kunden sich über eine Schnittstelle anmelden, die Sie entwerfen und verwalten, um die Anwendung zu testen. In diesem Angebot wird [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) verwendet, um Ihre vorhandene Testumgebung zu nutzen. Anders gesagt: Es handelt sich um eine kostenlose Testversion, die vom Kunden gesteuert und kontrolliert und von einem Partner gehostet wird. Es ist von entscheidender Bedeutung, dass Ihre Lösung auf eine Weise bereitgestellt wird, die es Käufern von Cloudlösungen ermöglicht, Ihre Lösung kostenlos und unabhängig auszuprobieren. Das Testerlebnis eines solchen Angebots kommt der Art und Weise entgegen, in der Kunden nach Cloudlösungen suchen.  

Eine Übersicht über alle Marketplace-Angebote finden Sie im [Marketplace-Leitfaden für Herausgeber](https://aka.ms/sellerguide).

## <a name="saas-application-technical-guidance"></a>Technischer Leitfaden für SaaS-Anwendungen
Die technischen Voraussetzungen für SaaS-Anwendungen sind einfach. Herausgeber müssen nur für die Integration in Azure AD sorgen, um eine Veröffentlichung zu ermöglichen.  Die Integration von Anwendungen in Azure AD ist umfassend dokumentiert, und Microsoft bietet verschiedene SDKs und Ressourcen für diesen Zweck.  

Als Erstes empfiehlt es sich, ein Abonnement speziell für die Veröffentlichung im Azure Marketplace einzurichten, sodass Sie diese Arbeit von anderen Initiativen trennen können. Darüber hinaus empfehlen wir die Installation der folgenden Tools in Ihrer Entwicklungsumgebung (falls die Tools nicht bereits vorhanden sind): 
- [Azure-CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)  
- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-5.0.0)  
- [Azure-Entwicklungstools (Informationen zu den verfügbaren Tools)](https://azure.microsoft.com/tools/)  
- [Visual Studio Code](https://code.visualstudio.com/)  

### <a name="resources"></a>angeben
Die folgenden Listen enthalten Links zu den besten Azure AD-Ressourcen für den Einstieg: 

**Documentation**

- [Entwicklerhandbuch zu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)

- [Integration in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate)

- [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

- [Azure Roadmap – Sicherheit und Identität](https://azure.microsoft.com/roadmap/?category=security-identity)

**Videos**

- [Azure Active Directory Authentication with Vittorio Bertocci](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration) (Authentifizierung über Azure Active Directory mit Vittorio Bertocci)

- [Azure Active Directory Identity Technical Briefing - Part 1 of 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration) (Technische Anleitung zu Azure Active Directory-Identitäten – Teil 1 von 2)

- [Azure Active Directory Identity Technical Briefing - Part 2 of 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration) (Technische Anleitung zu Azure Active Directory-Identitäten – Teil 2 von 2)

- [Building Apps with Microsoft Azure Active Directory](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration) (Erstellen von Apps mit Microsoft Azure Active Directory)

- [Verschiedene Microsoft Azure-Videos rund um Active Directory](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

**Schulung**  
- [Microsoft Azure for IT Pros Content Series: Azure Active Directory](https://mva.microsoft.com/en-US/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965) (Themenreihe zu Microsoft Azure für IT-Experten: Azure Active Directory)

**Updates des Azure Active Directory-Diensts**  
- [Azure AD-Dienstupdates](https://azure.microsoft.com/updates/?product=active-directory)

Wenn Sie Unterstützung benötigen, nutzen Sie die folgenden Ressourcen:
- [MSDN-Foren](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
- [StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="the-azure-active-directory-gallery"></a>Der Azure Active Directory-Katalog
Sie können Ihre Anwendung nicht nur im Azure Marketplace bzw. in AppSource, sondern auch im Azure AD-Anwendungskatalog listen, der zum Azure Marketplace AppStore gehört. Kunden, die Azure AD als Identitätsanbieter verwenden, finden hier die verschiedenen veröffentlichten SaaS-Anwendungsconnectors. IT-Administratoren können Connectors aus dem App-Katalog hinzufügen und diese dann für das einmalige Anmelden (Single Sign-On, SSO) und die Bereitstellung konfigurieren und verwenden. Azure AD unterstützt alle wichtigen Verbundprotokolle für SSO, einschließlich SAML 2.0, OpenID Connect, OAuth und WS-Fed.  

Nachdem Sie mit einem Test sichergestellt haben, dass die Integration Ihrer Anwendung in Azure AD funktioniert, übermitteln Sie Ihre Anforderung für den Zugriff auf unser Anwendungsnetzwerkportal. Wenn Sie über ein Office 365-Konto verfügen, verwenden Sie dieses für die Anmeldung beim Portal. Wenn Sie kein Office 365-Konto besitzen, können Sie Ihr Microsoft-Konto (z.B. Outlook oder Hotmail) zur Anmeldung verwenden.

## <a name="program-benefits"></a>Programmvorteile
- SSO sorgt für ein optimales Kundenerlebnis.
- Die Konfiguration der Anwendung ist einfach und nur in sehr geringem Maß notwendig.
- Kunden können nach der Anwendung suchen und finden sie leicht im Katalog.
- Jeder Kunde kann diese Integration nutzen, unabhängig von der vorhandenen Azure AD-SKU (Free, Basic oder Premium).
- Wir bieten ein Tutorial mit Schrittanleitungen zur Konfiguration für unsere gemeinsamen Kunden.
- Das Programm ermöglicht die Benutzerbereitstellung für dieselbe App bei Verwendung von SCIM.

## <a name="prerequisites"></a>Voraussetzungen
Damit eine Anwendung im Azure AD-Katalog gelistet werden kann, muss zunächst eines der von Azure AD unterstützten Verbundprotokolle in der Anwendung implementiert werden. Lesen Sie die Nutzungsbedingungen für den Azure AD-Anwendungskatalog, die Sie unter [Rechtliche Hinweise zu Microsoft Azure](https://azure.microsoft.com/support/legal/) finden.  

Im Folgenden werden einige weitere Voraussetzungen beschrieben, die je nach dem von Ihnen verwendeten Protokoll ebenfalls erfüllt sein müssen:

**OpenID Connect**: Erstellen Sie die mehrinstanzenfähige Anwendung in Azure AD, und implementieren Sie das Zustimmungsframework für Ihre Anwendung. Senden Sie die Anmeldeanforderung an den gemeinsamen Endpunkt, sodass jeder Kunde der Anwendung zustimmen kann. Sie können den Benutzerzugriff des Kunden anhand der Mandanten-ID und der im Token erhaltenen UPN des Benutzers steuern.  
**SAML 2.0 oder WS-Fed**: Ihre Anwendung sollte über eine Funktion zur SSO-Integration in SAML oder WS-Fed im SP- oder IDP-Modus verfügen.
