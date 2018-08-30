---
title: Was ist Azure Active Directory (Azure AD)? | Microsoft-Dokumentation
description: Verwenden Sie Azure Active Directory, um Ihre vorhandenen lokalen Identitäten in die Cloud zu erweitern oder in Azure AD integrierte Anwendungen zu entwickeln.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/09/2018
ms.custom: it-pro
ms.openlocfilehash: 5087f759d682382bc22b5f95f462fe0f08619cc8
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "39449993"
---
# <a name="what-is-azure-active-directory"></a>Was ist Azure Active Directory?
Azure Active Directory (Azure AD) ist Microsofts mehrinstanzenfähiger cloudbasierter Verzeichnis- und Identitätsverwaltungsdienst, der Kernverzeichnisdienste, Anwendungszugriffsverwaltung und Identitätsschutz in einer einzigen Lösung vereint. Azure AD bietet außerdem eine umfassende, auf Standards basierende Plattform, mit der Entwickler ihre Anwendungen mit einer Zugriffssteuerung ausstatten können, die auf zentralisierten Richtlinien und Regeln basiert.

![Azure AD Connect-Stapel](./media/active-directory-whatis/Azure_Active_Directory.png)

- **Für IT-Administratoren.** Azure AD bietet eine sicherere Lösung für Ihre Organisation durch die Verwendung von stärkerer Identitätsverwaltung und SSO-Zugriff (Single Sign-On, einmaliges Anmelden) auf Tausende [cloudbasierte SaaS-Apps](../saas-apps/tutorial-list.md) und lokale Apps. Durch diese Apps erhalten Sie auch cloudbasierte Anwendungssicherheit, nahtlosen Zugriff, verbesserte Zusammenarbeit und Automatisierung des Identitätslebenszyklus für Ihre Mitarbeiter, wodurch sowohl die Sicherheit als auch die Konformität erhöht werden.

    Darüber hinaus können Sie Azure AD mit nur [vier Klicks](./../connect/active-directory-aadconnect-get-started-express.md) in eine vorhandene Windows Server Active Directory-Instanz integrieren, sodass für Ihre Organisation die Investitionen in vorhandene lokale Identitätsverwaltungslösungen genutzt werden können, um den cloudbasierten SaaS-App-Zugriff zu verwalten.

- **Für App-Entwickler.** Mit Azure AD können Sie sich auf die Entwicklung Ihrer Apps konzentrieren, indem Sie diese in eine Identitätsverwaltungslösung integrieren, die von Millionen Organisationen auf der ganzen Welt genutzt wird.

- **Für Office 365-, Azure- oder Dynamics CRM Online-Kunden.** Sie verwenden bereits Azure AD. Jeder Office 365-, Azure- und Dynamics CRM Online-Mandant ist tatsächlich ein Azure AD-Mandant, sodass Sie sofort damit beginnen können, den Mitarbeiterzugriff auf Ihre integrierten Cloud-Apps zu verwalten.

## <a name="how-reliable-is-azure-ad"></a>Wie zuverlässig ist Azure AD?
Dank der mehrinstanzenfähigen, geografisch verteilten Azure AD-Struktur mit Hochverfügbarkeit können Sie sich bei Ihren wichtigsten Geschäftsanforderungen auf Azure AD verlassen. Azure AD wird in 28 Datencentern auf der ganzen Welt mit automatischen Failover ausgeführt – so können Sie sicher sein, dass Azure AD hochgradig zuverlässig funktioniert. Selbst wenn einmal ein Datencenter ausfällt, befinden sich Kopien Ihrer Verzeichnisdaten in mindestens zwei weiteren regional verteilten Datencentern und stehen für den sofortigen Zugriff zur Verfügung.

Weitere Informationen über Vereinbarungen zum Servicelevel finden Sie unter [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Auswählen einer Edition
Alle Microsoft Online-Unternehmensdienste nutzen für Anmelde- und andere Identitätsanforderungen Azure Active Directory (Azure AD). Wenn Sie einen der Microsoft Online-Unternehmensdienste (z.B. Office 365 oder Microsoft Azure) abonnieren, erhalten Sie Azure AD mit Zugriff auf alle Free-Features. Mit der Free-Edition von Azure Active Directory können Sie Benutzer und Gruppen verwalten, eine Synchronisierung mit lokalen Verzeichnissen ausführen, sich mittels einmaligem Anmelden (SSO) bei Azure, Office 365 und tausenden gängigen SaaS-Anwendungen wie Salesforce, Workday, Concur, DocuSign, Google Apps, Box, ServiceNow, Dropbox und mehr anmelden. 

Sie können Ihre Azure Active Directory-Anwendung erweitern, indem Sie mit den Basic-, Premium P1- und Premium P2-Editionen von Azure Active Directory kostenpflichtige Funktionen hinzufügen. Die kostenpflichtigen Editionen von Azure Active Directory ergänzen Ihr kostenloses Verzeichnis. Sie bieten leistungsstarke Funktionen wie etwa Self-Service, optimierte Überwachung, Sicherheitsberichterstellung, Multi-Factor Authentication (MFA) und sicheren Zugriff für Ihre mobilen Mitarbeiter.

> [!NOTE]
> Informationen zu den Preisoptionen für diese Editionen finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium P1, Premium P2 und Azure Active Directory Basic werden derzeit in China nicht unterstützt. Wenn Sie weitere Informationen zur Azure AD-Preisgestaltung wünschen, können Sie sich an das Azure Active Directory-Forum wenden.
>

* **Azure Active Directory Basic** – Diese Edition wurde für Taskworker entwickelt, die in erster Linie cloudbasiert arbeiten. Sie bietet einen cloudzentrierten Anwendungszugriff sowie Self-Service-Identitätsverwaltungslösungen. Mit der Basic Edition von Azure Active Directory erhalten Sie Funktionen zur Produktivitätssteigerung und Kostensenkung wie gruppenbasierte Zugriffsverwaltung, Self-Service-Kennwortzurücksetzung für Cloud-Anwendungen und den Azure Active Directory-Anwendungsproxy (zur Veröffentlichung lokaler Webanwendungen mit Azure Active Directory). Diese Features basieren auf einer SLA auf Unternehmensebene mit einer Verfügbarkeitsgarantie von 99,9 Prozent.
* **Azure Active Directory Premium P1** – Die Azure Active Directory Premium-Edition wurde für Organisationen mit anspruchsvolleren Anforderungen im Hinblick auf die Identitäts- und Zugriffsverwaltung entwickelt und bietet zusätzliche Identitätsverwaltungsfunktionen. Außerdem können Hybridbenutzer mit dieser Edition nahtlos auf lokale und cloudbasierte Funktionen zugreifen. Diese Edition enthält alles, was Sie für Information-Worker und Identitätsadministratoren in Hybridumgebungen für Anwendungszugriff, Self-Service-Identitäts- und Zugriffsverwaltung (Identity and Access Management, IAM), Identitätsschutz und Sicherheit in der Cloud benötigen. Sie unterstützt erweiterte Verwaltungs- und Delegierungsressourcen wie dynamische Gruppen und Self-Service-Gruppenverwaltung. Die Edition enthält Microsoft Identity Manager (eine lokale Identitäts- und Zugriffsverwaltungs-Suite) und bietet Cloudfunktionen für das Zurückschreiben. Dadurch werden Lösungen wie die Self-Service-Kennwortzurücksetzung für Ihre lokalen Benutzer ermöglicht.
* **Azure Active Directory Premium P2** – Diese Edition bietet erweiterten Schutz für alle Benutzer und Administratoren und umfasst nicht nur sämtliche Funktionen von Azure AD Premium P1, sondern auch Identity Protection und Privileged Identity Management. Azure Active Directory Identity Protection wertet Milliarden von Signalen aus, um basierend auf der Risikostufe bedingten Zugriff auf Ihre Anwendungen und kritischen Unternehmensdaten bereitzustellen. Darüber hinaus unterstützen wir Sie mithilfe von Azure Active Directory Privileged Identity Management bei Verwaltung und Schutz privilegierter Konten. So können Sie Administratoren sowie deren Zugriff ermitteln, einschränken und überwachen und bei Bedarf Just-in-Time-Zugriff gewähren.  

> [!NOTE]
> Eine Reihe von Azure Active Directory-Funktionen sind als Editionen mit nutzungsbasierter Bezahlung verfügbar:
>
> * Active Directory B2C ist die Identitäts- und Zugriffsverwaltungslösung für Ihre kundenorientierten Anwendungen. Weitere Informationen finden Sie unter [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).
> * Azure Multi-Factor Authentication kann über benutzer- oder authentifizierungsspezifische Anbieter genutzt werden. Weitere Informationen finden Sie unter [Was ist Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md).
>

## <a name="how-can-i-get-started"></a>Wie fange ich an?

**Wenn Sie IT-Administrator sind:**

* [Erste Schritte mit einem Azure-Abonnement:](https://azure.microsoft.com/trial/get-started-active-directory/) Registrieren Sie sich noch heute über diesen Link für eine kostenlose 30-Tage-Testversion, und stellen Sie Ihre erste Cloud-Lösung in weniger als fünf Minuten bereit.

* Lesen Sie den Artikel [Erste Schritte mit Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) mit Tipps und Tricks zum schnellen Einrichten und Ausführen eines Azure AD-Mandanten.

**Wenn Sie Entwickler sind:**
 
* Sehen Sie sich die [Anleitung für Entwickler](../develop/azure-ad-developers-guide.md) für Azure Active Directory an.

* [Starten Sie eine Testversion.](https://azure.microsoft.com/trial/get-started-active-directory/) Registrieren Sie sich noch heute für eine kostenlose 30-Tage-Testversion, und beginnen Sie damit, Ihre Apps in Azure AD zu integrieren.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zu den Grundlagen der Azure-Identitätsverwaltung](https://docs.microsoft.com/azure/active-directory/identity-fundamentals)
