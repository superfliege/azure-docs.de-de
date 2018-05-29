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
ms.date: 05/09/2018
ms.author: pabutler
ms.openlocfilehash: 48b0b4177dad6262105bf30be2b8714f6ea1228f
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076705"
---
# <a name="saas-applications-technical-publishing-guide"></a>Technischer Leitfaden für die Veröffentlichung von SaaS-Anwendungen

Willkommen beim technischen Leitfaden für die Veröffentlichung von Azure Marketplace-SaaS-Anwendungen. Dieser Leitfaden soll zukünftige und bestehende Herausgeber dabei unterstützen, ihre Anwendungen und Dienste mithilfe des Angebots für SaaS-Anwendungen im Azure Marketplace zu listen. 

Dieser Leitfaden ist zum besseren Verständnis der Vorgehensweise zum Veröffentlichen eines SaaS-Angebots in folgende Abschnitte unterteilt:
* Angebotsübersicht
* Geschäftliche Anforderungen
* Technische Anforderungen
* Veröffentlichungsvorgang
* Verwenden von Azure Active Directory, um Tests zu ermöglichen
* Zertifizieren Ihrer Azure AD-Integration für Marketplace

## <a name="offer-overview"></a>Angebotsübersicht  

SaaS-Anwendungen stehen in beiden Azure-Storefronts zur Verfügung. In der folgenden Tabelle werden die derzeit verfügbaren Optionen beschrieben:

| Storefront-Option | Listing (Eintrag) | Trial/Transact (Testversion/Transaktion) |  
| --- | --- | --- |  
| AppSource | Ja (Kontaktaufnahme) | Ja (Power BI/Dynamics) |
| Azure Marketplace | Nein  | Ja (SaaS-Apps) |   

**Listing** (Eintrag): Diese Veröffentlichungsoption besteht aus einem Angebot vom Typ „Contact Me“ (Kontaktaufnahme) und wird verwendet, wenn eine Teilnahme auf Testversions- oder Transaktionsebene nicht möglich ist. Dieser Ansatz hat den Vorteil, dass Herausgeber, die eine Lösung auf dem Markt anbieten, sofort Leads gewinnen können, die zu Abschlüssen führen und zum Wachstum Ihres Geschäfts beitragen.  
**Trial/Transact** (Testversion/Transaktion): Der Kunde hat die Möglichkeit, Ihre Lösung direkt zu kaufen oder eine Testversion für Ihre Lösung anzufordern. Die Bereitstellung einer Testversion führt zu einer stärkeren Einbindung von Kunden und ermöglicht es ihnen, Ihre Lösung vor dem Kauf in Augenschein zu nehmen. Eine Testversion steigert Ihre Verkaufschancen in den Storefronts, und die Interaktion mit den Kunden führt in der Regel zu weiteren und lukrativeren Leads. Testversionen müssen mindestens für die Dauer des Testzeitraums kostenlosen Support enthalten.  

| SaaS-Apps-Angebot | Geschäftliche Anforderungen | Technische Anforderungen |  
| --- | --- | --- |  
| **Kontaktaufnahme** | Ja | Nein  |  
| **Power BI/Dynamics** | Ja | Ja (Azure AD-Integration) |  
| **SaaS-Apps**| Ja | Ja (Azure AD-Integration) |     

Weitere Informationen zu den Marketplace-Storefronts sowie eine Beschreibung der einzelnen Veröffentlichungsoptionen finden Sie im [Marketplace-Herausgeberleitfaden](https://aka.ms/sellerguide) sowie in den [Veröffentlichungsoptionen](https://docs.microsoft.com/en-us/azure/marketplace/marketplace-publishers-guide#select-a-publishing-option).

## <a name="business-requirements"></a>Geschäftliche Anforderungen
Die geschäftlichen Anforderungen für SaaS-Angebote können parallel zu den technischen Anforderungen angegeben werden. Die meisten der geschäftlichen Anforderungen und Informationen werden bei der Erstellung des SaaS-Angebots im Cloud Partner-Portal erfasst. Die geschäftlichen Anforderungen lauten wie folgt: 
* Akzeptierung der Teilnahmerichtlinien
* Microsoft-Integration 
* Angabe der Zielgruppe für das Angebot
* Definition und Bestimmung der zu verwendenden Leadverwaltung
* Einrichtung von Datenschutz- und Nutzungsrichtlinien
* Definition der Supportkontakte  

Weitere Informationen finden Sie im Thema [Voraussetzungen für die Veröffentlichung im Marketplace](https://docs.microsoft.com/en-us/azure/marketplace/marketplace-publishers-guide#prerequisites-for-marketplace-publishing).

## <a name="technical-requirements"></a>Technische Anforderungen

Die technischen Voraussetzungen für SaaS-Anwendungen sind einfach. Herausgeber müssen nur für die Integration in Azure Active Directory (Azure AD) sorgen, um eine Veröffentlichung zu ermöglichen. Die Integration von Anwendungen in Azure AD ist umfassend dokumentiert, und Microsoft bietet verschiedene SDKs und Ressourcen für diesen Zweck.  

Als Erstes empfiehlt es sich, ein Abonnement speziell für die Veröffentlichung im Azure Marketplace einzurichten, sodass Sie diese Arbeit von anderen Initiativen trennen können. Danach können Sie Ihre SaaS-Anwendung in diesem Abonnement bereitstellen und mit der Entwicklungsarbeit beginnen.  

Die beste Azure Active Directory-Dokumentation sowie entsprechende Beispiele und Anleitungen finden Sie auf den folgenden Websites: 

* [Entwicklerhandbuch zu Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide)

* [Integration in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-how-to-integrate)

* [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

* [Azure Roadmap – Sicherheit und Identität](https://azure.microsoft.com/roadmap/?category=security-identity)

Videotutorials finden Sie hier:

* [Azure Active Directory Authentication with Vittorio Bertocci](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration) (Authentifizierung über Azure Active Directory mit Vittorio Bertocci)

* [Azure Active Directory Identity Technical Briefing - Part 1 of 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration) (Technische Anleitung zu Azure Active Directory-Identitäten – Teil 1 von 2)

* [Azure Active Directory Identity Technical Briefing - Part 2 of 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration) (Technische Anleitung zu Azure Active Directory-Identitäten – Teil 2 von 2)

* [Building Apps with Microsoft Azure Active Directory](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration) (Erstellen von Apps mit Microsoft Azure Active Directory)

* [Verschiedene Microsoft Azure-Videos rund um Active Directory](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

Eine kostenlose Azure Active Directory-Schulung finden Sie hier:  
* [Microsoft Azure for IT Pros Content Series: Azure Active Directory](https://mva.microsoft.com/en-US/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965) (Themenreihe zu Microsoft Azure für IT-Experten: Azure Active Directory)

Darüber hinaus bietet Azure Active Directory eine Website für die Suche nach Dienstupdates:   
* [Azure AD-Dienstupdates](https://azure.microsoft.com/updates/?product=active-directory)

Wenn Sie Unterstützung benötigen, nutzen Sie die folgenden Ressourcen:
* [MSDN-Foren](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
* [StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="publishing-process"></a>Veröffentlichungsvorgang

Der SaaS-Veröffentlichungsprozess umfasst sowohl technische als auch geschäftliche Schritte.  Die meisten Aufgaben für die Azure Active Directory-Entwicklung und -Integration können parallel zu den Aufgaben erledigt werden, die zum Erfüllen der geschäftlichen Anforderungen des Angebots erforderlich sind. Der Großteil der geschäftlichen Anforderungen ist Teil der Konfiguration des SaaS-App-Angebots im Cloud Partner-Portal.  
Das folgende Diagramm zeigt die wichtigsten Veröffentlichungsschritte für das Angebot vom Typ „Trial/Transact“ (Testversion/Transaktion):  

![SaaS-Veröffentlichungsschritte](./media/marketplace-saas-applications-technical-publishing-guide/saaspublishingsteps.png)  

In der folgenden Tabelle werden die einzelnen Veröffentlichungsschritte beschrieben:  

| Veröffentlichungsschritt | Beschreibung |   
| --- | --- |  
| **Erstellen der SaaS-Anwendung** | Melden Sie sich beim Cloud Partner-Portal an, klicken Sie auf **Neu**, und wählen Sie das Angebot **SaaS-Apps** aus. |  
| **Erstellen der Azure AD-Integration** | Gehen Sie gemäß den Informationen zu den technischen Anforderungen aus dem vorherigen Abschnitt vor, um Ihre SaaS-Lösung in Azure AD zu integrieren. |  
| **Festlegen der Angebotseinstellungen**| Geben Sie alle anfänglichen Informationen für das SaaS-Angebot ein: die gewünschte Angebots-ID und den gewünschten Angebotsnamen. |     
| **Festlegen der technischen Informationen** | Geben Sie die technischen Informationen für das Angebot ein. Für SaaS-Anwendungen sind der URI der Lösung und die Art der Kaufschaltfläche des Angebots (kostenlos, Testversion oder Kontaktaufnahme) erforderlich. |  
| **Testversion (optional)** | Hierbei handelt es sich um eine optionale Art von Testversion, die vor allem für andere Arten von Marketplace-Angeboten erforderlich ist. Sie ermöglicht die Bereitstellung der Testversion in den Abonnements des Herausgebers (anstatt in den Abonnements des Endkunden). |  
| **Festlegen des Storefront-Materials für das Angebot**| In diesem Abschnitt verlinkt der Herausgeber Logos, Marketingmaterial und juristisch relevante Dokumente bzw. lädt sie hoch und konfiguriert das Leadverwaltungssystem. |
| **Festlegen der Angebotskontakte** | Geben Sie Technik- und Supportkontaktinformationen für das SaaS-Angebot ein. |  
| **Überprüfen der Azure AD-Integration der SaaS-App** | Vergewissern Sie sich, dass Ihre SaaS-App in Azure AD integriert ist, bevor Sie die App zur Veröffentlichung übermitteln. |  
| **Veröffentlichen des Angebots**| Nach Fertigstellung der Angebotsressourcen und der technischen Ressourcen kann das Angebot übermittelt werden. Im Rahmen des daraufhin beginnenden Veröffentlichungsprozesses wird die Lösungsvorlage getestet, überprüft, zertifiziert und zur Veröffentlichung freigegeben. |

## <a name="using-azure-active-directory-to-enable-trials"></a>Verwenden von Azure Active Directory, um Tests zu ermöglichen  

Microsoft authentifiziert alle Marketplace-Benutzer mit Azure AD. Wenn sich also ein authentifizierter Benutzer in Marketplace durch Ihr Testversionsangebot klickt und zu Ihrer Testumgebung umgeleitet wird, können Sie dem Benutzer ohne zusätzlichen Anmeldeschritt direkt eine Testversion bereitstellen. Das Token, das Ihre App im Rahmen der Authentifizierung von Azure AD erhält, enthält wichtige Benutzerinformationen, mit denen Sie ein Benutzerkonto in Ihrer App erstellen können, um die Bereitstellung zu automatisieren und die Wahrscheinlichkeit eines Abschlusses zu erhöhen. Weitere Informationen zu dem Token finden Sie in der [Azure AD-Tokenreferenz](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims).

Die Verwendung der durch Azure AD ermöglichten Ein-Klick-Authentifizierung bei Ihrer App oder Testversion hat folgende Vorteile:  
* Der Prozess zwischen Marketplace und Testumgebung wird optimiert.  
* Das Gefühl einer „produktinternen Erfahrung“ bleibt selbst dann erhalten, wenn der Benutzer vom Marketplace zu Ihrer Domäne bzw. Testumgebung umgeleitet wird.  
* Die Wahrscheinlichkeit eines Abbruchs bei der Umleitung sinkt, da keine zusätzliche Anmeldung erforderlich ist.  
* Weniger Bereitstellungshindernisse für den Großteil der Azure AD-Benutzer.  

## <a name="certifying-your-azure-ad-integration-for-marketplace"></a>Zertifizieren Ihrer Azure AD-Integration für Marketplace  

Die Zertifizierung der Azure AD-Integration ist auf mehrere Arten möglich – je nachdem, ob es sich um eine Anwendung mit nur einem Mandanten oder um eine mehrinstanzenfähige Anwendung handelt und ob Sie bereits die einmalige Verbundanmeldung von Azure AD unterstützen.  

**Vorgehensweise für mehrinstanzenfähige Anwendungen:**  

Wenn Sie Azure AD bereits unterstützt, gehen Sie wie folgt vor:
1.  Registrieren Sie Ihre Anwendung im Azure-Portal
2.  Aktivieren Sie das Feature für die Unterstützung der Mehrinstanzenfähigkeit in Azure AD, um eine Testversion mit Ein-Klick-Feature zu erhalten. Ausführlichere Informationen finden Sie [hier](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications).  

Wenn die einmalige Azure AD-Verbundanmeldung für Sie neu ist, gehen Sie wie folgt vor: 
1.  Registrieren Sie Ihre Anwendung im Azure-Portal
2.  Entwickeln Sie SSO mit Azure AD mithilfe von [OpenID Connect](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-openid-connect-code) oder [OAuth 2.0](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-oauth-code).
3.  Aktivieren Sie das Feature für die Unterstützung der Mehrinstanzenfähigkeit in AAD, um eine Testversion mit Ein-Klick-Feature zu erhalten. Ausführlichere Informationen finden Sie [hier](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-devhowto-appsource-certified).  

**Optionen für Anwendungen mit nur einem Mandanten:**  
* Fügen Sie Ihrem Verzeichnis mithilfe von [Azure B2B](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) Benutzer als Gastbenutzer hinzu.
* Stellen Sie Testversionen für Kunden unter Verwendung von „Contact Me“ (Kontaktaufnahme) bereit.
* Entwickeln Sie eine kundenspezifische Testversion.
* Erstellen Sie eine mehrinstanzenfähige Beispiel-Demo-App mit SSO.

