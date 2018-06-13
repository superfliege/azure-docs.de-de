---
title: Vergleichen von B2B-Zusammenarbeit und B2C in Azure Active Directory | Microsoft-Dokumentation
description: Was ist der Unterschied zwischen Azure Active Directory B2B-Zusammenarbeit und Azure AD B2C?
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 03/15/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 0e8781fcf2710b00ba352bff29a370cbb73e0648
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33927882"
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Vergleichen von B2B-Zusammenarbeit und B2C in Azure Active Directory

Sowohl Azure Active Directory (Azure AD) B2B-Zusammenarbeit als auch Azure AD B2C ermöglichen Ihnen die Arbeit mit externen Benutzern in Azure AD. Aber worin bestehen die Unterschiede?


Funktionen von B2B-Zusammenarbeit |     Eigenständiges Azure AD B2C-Angebot
-------- | --------
Vorgesehen für: Organisationen, die Benutzer einer Partnerorganisation unabhängig vom Identitätsanbieter authentifizieren möchten. | Vorgesehen für: Einladungen für Kunden Ihrer mobilen Apps und Web-Apps (Einzelpersonen, Institutionen oder Organisationen) zu Ihrem Azure AD.
Unterstützte Identitäten: Mitarbeiter mit Geschäfts-, Schul- oder Unikonten, Partner mit Geschäfts-, Schul- oder Unikonten oder beliebige E-Mail-Adressen. Direkter Verbund wird in Kürze unterstützt.  | Unterstützte Identitäten: Endbenutzer mit lokalen Anwendungskonten (E-Mail-Adresse oder Benutzername) oder eine beliebige unterstützte soziale Identität mit direktem Verbund.
Verzeichnis für die Partnerbenutzer: Partnerbenutzer aus der externen Organisation werden in demselben Verzeichnis wie Mitarbeiter verwaltet, aber speziell gekennzeichnet. Sie können auf die gleiche Weise wie Mitarbeiter verwaltet werden, denselben Gruppen hinzugefügt werden usw.  | Verzeichnis für die Kundenbenutzerentitäten: Anwendungsverzeichnis. Wird getrennt vom Verzeichnis der Organisationsmitarbeiter und Partner (sofern vorhanden) verwaltet.
Einmaliges Anmelden (SSO) bei allen verbundenen Azure AD-Apps wird unterstützt. Beispielsweise können Sie den Zugriff auf Office 365 oder lokale Apps und auf andere SaaS-Apps wie z.B. Salesforce oder Workday bereitstellen.  |  Einmaliges Anmelden bei Kunden-Apps innerhalb der Azure AD B2C-Mandanten wird unterstützt. Einmaliges Anmelden bei Office 365 oder anderen Microsoft- und Nicht-Microsoft-SaaS-Apps wird nicht unterstützt.
Partnerlebenszyklus: Wird von der Hostorganisation/einladenden Organisation verwaltet.  | Kundenlebenszyklus: Self-Service oder von der Anwendung verwaltet.
Sicherheitsrichtlinie und Konformität: Wird von der Hostorganisation/einladenden Organisation verwaltet.  | Sicherheitsrichtlinie und Konformität: Wird von der Anwendung verwaltet.
Branding: Das Branding der Hostorganisation/einladenden Organisation wird verwendet.  |    Branding: Wird von der Anwendung verwaltet. Normalerweise wird eher das Produktbranding verwendet, und die Organisation tritt in den Hintergrund.
Weitere Informationen: [Blogbeitrag](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [Dokumentation](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  | Weitere Informationen: [Produktseite](https://azure.microsoft.com/services/active-directory-b2c/), [Dokumentation](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Nächste Schritte

- [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Eigenschaften von B2B-Zusammenarbeitsbenutzern](active-directory-b2b-user-properties.md)

