---
title: Was ist Azure Active Directory (Azure AD)? | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Azure Active Directory Ihre vorhandenen lokalen Identitäten in die Cloud erweitern und in Azure AD integrierte Apps entwickeln.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: overview
ms.date: 09/13/2018
ms.custom: it-pro
ms.openlocfilehash: 755c301651e7c49faa8b05b2b443c5cce1a03c90
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364052"
---
# <a name="what-is-azure-active-directory"></a>Was ist Azure Active Directory?
Azure Active Directory (Azure AD) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Azure AD kombiniert Kernverzeichnisdienste, Anwendungszugriffsverwaltung und Identitätsschutz in einer einzigen Lösung und bietet eine standardbasierte Plattform, die Entwickler dabei unterstützt, die Zugriffssteuerung für ihre Apps auf Grundlage zentralisierter Richtlinien und Regeln bereitzustellen.

![Azure AD Connect-Stapel](./media/active-directory-whatis/Azure_Active_Directory.png)

## <a name="benefits-of-azure-ad"></a>Vorteile von Azure AD
Azure AD ermöglicht Ihnen Folgendes:

-   Erstellen und Verwalten einer einzelnen Identität für jeden Benutzer in Ihrer gesamten Unternehmensumgebung, wobei Benutzer, Gruppen und Geräte mit [Azure AD Connect](../connect/active-directory-aadconnect.md) synchronisiert werden

-   Bereitstellen des Zugriffs auf Ihre Apps mit einmaligem Anmelden, einschließlich Tausender vorab integrierter SaaS-Apps, und Bereitstellen von noch sichererem Remotezugriff auf lokale SaaS-Anwendungen mithilfe des [Azure AD-Anwendungsproxys](../manage-apps/application-proxy.md)

-   Verwenden eines sicheren Anwendungszugriffs durch Erzwingen der regelbasierten Richtlinien für die [Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md) (MFA) für lokale und Cloud-Apps

-   Verbessern der Benutzerproduktivität durch [Self-Service-Kennwortzurücksetzung](../user-help/user-help-reset-password.md) sowie Gruppen- und Anwendungszugriffsanforderungen mithilfe des [MyApps-Portals](../user-help/active-directory-saas-access-panel-introduction.md)

-   Nutzen der [hohen Verfügbarkeit und Zuverlässigkeit](https://docs.microsoft.com/azure/architecture/checklist/availability) einer Lösung für die Identitäts- und Zugriffsverwaltung, die weltweit verfügbar, für Großunternehmen geeignet und cloudbasiert ist

## <a name="who-uses-azure-ad"></a>Azure AD-Zielgruppe
Azure AD wurde für IT-Administratoren, App-Entwickler und Benutzer von Office 365, Azure und Dynamics CRM Online konzipiert.

- **IT-Administratoren.** Azure AD bietet eine sicherere Lösung für Ihre Organisation durch die Verwendung von stärkerer Identitätsverwaltung und SSO-Zugriff (Single Sign-On, einmaliges Anmelden) auf Tausende [cloudbasierte SaaS-Apps](../saas-apps/tutorial-list.md) und lokale Apps. Diese Apps bieten Ihnen auch cloudbasierte Anwendungssicherheit, nahtlosen Zugriff, verbesserte Zusammenarbeit und Automatisierung des Identitätslebenszyklus für Ihre Benutzer, wodurch sowohl die Sicherheit als auch die Compliance verbessert werden.

    Darüber hinaus können Sie mit [Azure AD Connect](../connect/active-directory-aadconnect-get-started-express.md) Azure AD in eine vorhandene Windows Server Active Directory-Instanz integrieren, damit Ihre Organisation mit vorhandenen lokalen Identitätsverwaltungslösungen den cloudbasierten SaaS-App-Zugriff verwalten kann.

- **Für App-Entwickler.** Mit Azure AD können Sie sich auf die Entwicklung Ihrer Apps konzentrieren. Der Dienst ermöglicht Ihnen die Integration in eine Identitätsverwaltungslösung, die von Millionen Organisationen weltweit genutzt wird.

- **Für Office 365-, Azure- oder Dynamics CRM Online-Kunden.** Sie verwenden bereits Azure AD. Jeder Office 365-, Azure- und Dynamics CRM Online-Mandant ist tatsächlich ein Azure AD-Mandant, sodass Sie sofort damit beginnen können, den Benutzerzugriff auf Ihre integrierten Cloud-Apps zu verwalten.

## <a name="how-reliable-is-azure-ad"></a>Wie zuverlässig ist Azure AD?
Dank der mehrinstanzenfähigen, geografisch verteilten Azure AD-Struktur mit Hochverfügbarkeit können Sie sich bei Ihren wichtigsten Geschäftsanforderungen auf Azure AD verlassen. Azure AD wird in 28 Rechenzentren weltweit mit automatisiertem Failover ausgeführt. Das bedeutet, dass selbst bei Ausfall eines Rechenzentrums Kopien Ihrer Verzeichnisdaten in mindestens zwei weiteren regional verteilten Rechenzentren gespeichert und für den sofortigen Zugriff verfügbar sind.

Weitere Informationen über Vereinbarungen zum Servicelevel finden Sie unter [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Auswählen einer Edition
Alle Microsoft Online-Unternehmensdienste nutzen für Anmelde- und andere Identitätsanforderungen Azure AD. Wenn Sie einen Microsoft Online-Unternehmensdienst (z.B. Office 365 oder Microsoft Azure) abonnieren, erhalten Sie automatisch Azure AD mit Zugriff auf alle Azure AD Free-Features. Mit Azure Active Directory Free können Sie Benutzer und Gruppen verwalten, Synchronisierungen mit lokalen Verzeichnissen ausführen, sich mittels einmaligem Anmelden (SSO) bei Azure, Office 365 und zahlreichen gängigen SaaS-Apps wie Salesforce, Workday, Concur, DocuSign, Google Apps, Box, ServiceNow, Dropbox anmelden. 

Um Ihre Azure AD-Implementierung zu verbessern, können Sie auch kostenpflichtige Funktionen hinzufügen, indem Sie ein Upgrade auf Azure Active Directory Basic, Premium P1 oder Premium P2 ausführen. Die kostenpflichtigen Azure Ad-Editionen ergänzen Ihr kostenloses Verzeichnis. Sie bieten Funktionen für Unternehmen wie Self-Service, erweiterte Überwachung, Sicherheitsberichterstellung, Multi-Factor Authentication (MFA) und sicheren Zugriff für Ihre mobilen Mitarbeiter.

> [!NOTE]
> Informationen zu den Preisoptionen für diese Editionen finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium P1, Premium P2 und Azure Active Directory Basic werden derzeit in China nicht unterstützt. Wenn Sie weitere Informationen zur Azure AD-Preisgestaltung wünschen, können Sie sich an das Azure Active Directory-Forum wenden.

- **Azure Active Directory Basic:** Diese Edition wurde für das primär cloudbasierte Arbeiten zum Bearbeiten von Aufgaben entwickelt und bietet einen cloudzentrierten Anwendungszugriff sowie Self-Service-Identitätsverwaltungslösungen. Mit der Basic-Edition erhalten Sie Funktionen zur Produktivitätssteigerung und Kostensenkung wie gruppenbasierte Zugriffsverwaltung, Self-Service-Kennwortzurücksetzung für Cloud-Apps und den Azure Active Directory-Anwendungsproxy zum Veröffentlichen lokaler Web-Apps mit Azure AD. Diese Features basieren auf einer SLA auf Unternehmensniveau mit einer Verfügbarkeitsgarantie von 99,9 Prozent.

- **Azure Active Directory Premium P1:** Diese Edition wurde für Organisationen mit anspruchsvolleren Anforderungen in Hinblick auf die Identitäts- und Zugriffsverwaltung entwickelt und bietet zusätzliche Identitätsverwaltungsfunktionen für Unternehmen. Außerdem können Hybridbenutzer mit dieser Edition nahtlos auf lokale und cloudbasierte Funktionen zugreifen. Diese Edition enthält alles, was Sie für Information-Worker und Identitätsadministratoren in Hybridumgebungen für Anwendungszugriff, Self-Service-Identitäts- und Zugriffsverwaltung (Identity and Access Management, IAM), Identitätsschutz und Sicherheit in der Cloud benötigen. Sie unterstützt erweiterte Verwaltungs- und Delegierungsressourcen wie dynamische Gruppen und Self-Service-Gruppenverwaltung. Die Edition enthält Microsoft Identity Manager (eine lokale Identitäts- und Zugriffsverwaltungs-Suite) und bietet Cloudfunktionen für das Zurückschreiben. Dadurch werden Lösungen wie die Self-Service-Kennwortzurücksetzung für Ihre lokalen Benutzer ermöglicht.

- **Azure Active Directory Premium P2:** Diese Edition bietet erweiterten Schutz für Ihre Benutzer und Administratoren und umfasst nicht nur sämtliche Funktionen von Azure AD Premium P1, sondern auch Identity Protection und Privileged Identity Management. Azure Active Directory Identity Protection wertet Milliarden von Signalen aus, um basierend auf den Risiken bedingten Zugriff auf Ihre Apps und kritische Unternehmensdaten bereitzustellen. Darüber hinaus unterstützen wir Sie mithilfe von Azure Active Directory Privileged Identity Management bei Verwaltung und Schutz privilegierter Konten. So können Sie Administratoren sowie deren Zugriff ermitteln, einschränken und überwachen und bei Bedarf Just-in-Time-Zugriff gewähren.  

> [!NOTE]
> Eine Reihe von Azure Active Directory-Funktionen sind auch als Editionen mit nutzungsbasierter Bezahlung verfügbar:<ul><li>**Azure Active Directory B2C:** Identitäts- und Zugriffsverwaltungslösung für verbraucherorientierte Apps. Weitere Informationen finden Sie unter [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).</li><li>**Azure Multi-Factor Authentication:** Verwendung pro Benutzer oder pro Authentifizierungsanbieter. Weitere Informationen finden Sie unter [Was ist Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md).

## <a name="as-an-admin-how-do-i-get-started"></a>Erste Schritte als Administrator
Registrieren Sie sich für eine kostenlose 30-tägige Testversion, und stellen Sie Ihre erste Cloudlösung bereit. Weitere Informationen dazu finden Sie unter [Azure Active Directory Premium-Testversion](https://azure.microsoft.com/trial/get-started-active-directory/).

## <a name="as-a-developer-how-do-i-get-started"></a>Erste Schritte als Entwickler
Registrieren Sie sich für eine kostenlose 30-tägige Testversion, und beginnen Sie mit der Integration Ihrer Apps in Azure AD. Weitere Informationen dazu finden Sie unter [Azure Active Directory Premium-Testversion](https://azure.microsoft.com/trial/get-started-active-directory/). Hilfreiche Tipps enthält auch das [Entwicklerhandbuch](../develop/azure-ad-developers-guide.md) für Azure Active Directory.

## <a name="next-steps"></a>Nächste Schritte
- [Grundlagen der Azure-Identitäts- und Zugriffsverwaltung](identity-fundamentals.md)

- [Integrieren von Azure AD in Windows Server Active Directory](../hybrid/how-to-connect-install-express.md)