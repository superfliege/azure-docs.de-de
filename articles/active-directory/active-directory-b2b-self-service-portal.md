---
title: Self-Service-Anmeldungsportal für Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation
description: Die Azure Active Directory B2B-Zusammenarbeit ermöglicht Geschäftspartnern den gezielten Zugriff auf Ihre Unternehmensanwendungen und unterstützt so Ihre unternehmensübergreifenden Beziehungen.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/08/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: b16f7c040212db6d0dbeb7161a18f205cf524090
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33930434"
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Self-Service-Anmeldungsportal für Azure AD B2B-Zusammenarbeit

Kunden können mit den integrierten Features, die über das [Azure-Portal](https://portal.azure.com) und den [Anwendungszugriffsbereich](https://myapps.microsoft.com) für Endbenutzer verfügbar gemacht werden, viele Aufgaben erledigen. Unter Umständen müssen Sie jedoch den Onboarding-Workflow für B2B-Benutzer an die Bedürfnisse ihrer Organisation anpassen. Hierzu können Sie die [Einladungs-API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) verwenden.

Als einladende Organisation wissen Sie möglicherweise im Vorfeld nicht, welche externen Mitarbeiter Zugriff auf ihre Ressourcen benötigen. Benutzer von Partnerunternehmen müssen sich selbstständig registrieren können – mit einer Reihe von Richtlinien, die von der einladenden Organisation (sprich: von Ihnen) gesteuert werden. Dieses Szenario lässt sich über die APIs realisieren. Auf GitHub steht ein [entsprechendes Beispielprojekt](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web) zur Verfügung.

Dieses GitHub-Projekt zeigt, wie Organisationen mithilfe der APIs eine richtlinienbasierte Self-Service-Registrierungsfunktion für ihre vertrauenswürdigen Partner bereitstellen und Regeln definieren, die bestimmen, auf welche Apps die Partner Zugriff haben. Partnerbenutzer können auf Ressourcen zugreifen, wenn Sie sie benötigen. Der Zugriff ist sicher, und es ist kein manuelles Onboarding durch die einladende Organisation erforderlich. Sie können das Projekt ganz einfach in einem Azure-Abonnement Ihrer Wahl bereitstellen.

## <a name="as-is-code"></a>Unveränderter Code

Dieser Code wird als Beispiel zur Veranschaulichung der Verwendung der Einladungs-API von Azure Active Directory B2B zur Verfügung gestellt. Er muss von Ihrem Entwicklungsteam oder von einem Partner angepasst und überprüft werden, bevor Sie ihn in einem Produktionsszenario bereitstellen.

## <a name="next-steps"></a>Nächste Schritte

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Lizenzierung der Azure AD B2B-Zusammenarbeit](active-directory-b2b-licensing.md)
* [Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-faq.md)