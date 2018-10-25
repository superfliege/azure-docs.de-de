---
title: Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration | Microsoft-Dokumentation
description: Für die Azure Active Directory B2B-Kollaboration sind keine kostenpflichtigen Azure AD-Lizenzen erforderlich, aber es sind auch kostenpflichtige Features für B2B-Gastbenutzer erhältlich.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: mal
ms.openlocfilehash: d80794511f334cd6dc5af418e24fc774b7d8728f
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867509"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration

Mit der Azure Active Directory Business-to-Business-Kollaboration (Azure AD B2B-Kollaboration) können Sie externe Benutzer (oder „Gastbenutzer“) zur Verwendung Ihrer kostenpflichtigen Azure AD-Dienste einladen. Für jede kostenpflichtige Azure AD-Lizenz, die Sie einem Benutzer zuweisen, können Sie unter dem Kontingent für externe Benutzer bis zu fünf Gastbenutzer einladen.

Ein Gastbenutzer ist eine Person, die kein Mitglied Ihrer Organisation oder eines verbundenen Unternehmens Ihrer Organisation ist. Gastbenutzer sind durch ihre Beziehung zu Ihrer Organisation und nicht durch die Anmeldeinformationen definiert, mit denen sie sich anmelden. Tatsächlich kann sich ein Gastbenutzer entweder mit einer externen Identität oder mit Anmeldeinformationen Ihrer Organisation anmelden.

Folgende Personen sind *keine* Gastbenutzer:
- Ihre Mitarbeiter, Auftragnehmer vor Ort oder Vertreter vor Ort
- Mitarbeiter, Auftragnehmer oder Vertreter Ihrer Partner vor Ort

Die Lizenzierung von B2B-Gastbenutzern wird basierend auf dem Verhältnis von 1:5 automatisch berechnet und gemeldet. Derzeit ist es nicht möglich, Lizenzen für B2B-Gastbenutzer direkt Gastbenutzern zuzuweisen.

In einigen Fällen wird ein Gastbenutzer nicht über das Kontingent für externe Benutzer mit dem Verhältnis von 1:5 gemeldet. Wenn ein Gastbenutzer bereits über eine kostenpflichtige Azure AD-Lizenz in seiner Organisation verfügt, verwendet er keine Ihrer B2B-Lizenzen für Gastbenutzer. Darüber hinaus können Gastbenutzer kostenlose Azure AD-Features ohne zusätzliche Lizenzanforderungen nutzen. Gastbenutzer haben Zugriff auf kostenlose Azure AD-Features, auch wenn sie über keine kostenpflichtigen Azure AD-Lizenzen verfügen. 

## <a name="examples-calculating-guest-user-licenses"></a>Beispiele: Berechnen der Lizenzen für Gastbenutzer
Nachdem Sie bestimmt haben, wie viele Gastbenutzer Zugriff auf Ihre kostenpflichtigen Azure AD-Dienste benötigen, müssen Sie sicherstellen, dass Sie über genügend kostenpflichtige Azure AD-Lizenzen verfügen, um Gastbenutzer im erforderlichen Verhältnis von 1:5 abzudecken. Hier einige Beispiele:

- Sie möchten 100 Gastbenutzer für Ihre Azure AD-Anwendungen oder Azure AD-Dienste einladen, und Sie möchten allen Gastbenutzern die Zugriffsverwaltung und Bereitstellung zuweisen. Sie möchten zudem für 50 dieser Gastbenutzer die mehrstufige Authentifizierung (MFA) und bedingten Zugriff festlegen. Für diese Kombination benötigen Sie 10 Azure AD Basic-Lizenzen und 10 Azure AD Premium P1-Lizenzen. Wenn Sie Identity Protection-Features für Ihre Gastbenutzer verwenden möchten, benötigen Sie Azure AD Premium P2-Lizenzen im gleichen Verhältnis von 1:5 für die Gastbenutzer.
- Sie möchten 60 Gastbenutzer einladen, für die alle MFA erforderlich ist. Daher müssen Sie über mindestens 12 Azure AD Premium P1-Lizenzen verfügen. Sie haben 10 Mitarbeiter mit Azure AD Premium P1-Lizenzen, sodass entsprechend dem Lizenzierungsverhältnis von 1:5 bis zu 50 Gastbenutzer zulässig sind. Sie müssen zwei zusätzliche Premium P1-Lizenzen erwerben, um 10 zusätzliche Gastbenutzer abzudecken.

## <a name="using-the-b2b-collaboration-api-to-invite-users-from-your-affiliates"></a>Verwenden der B2B-Kollaboration-API zum Einladen von Benutzern von Ihren Partnern

Per Definition ist ein B2B-Gastbenutzer ein externer Benutzer, den Sie zur Verwendung Ihrer kostenpflichtigen Azure AD-Anwendungen und Azure AD-Dienste einladen. Mitarbeiter, Auftragnehmer vor Ort oder Vertreter vor Ort Ihres Unternehmens oder eines Ihrer verbundenen Unternehmen sind nicht für die B2B-Kollaboration qualifiziert, auch wenn B2B-Features verwendet werden. Hier einige Beispiele: 
- Sie möchten externe Anmeldeinformationen (z.B. die Identität eines sozialen Netzwerks) verwenden, um einen Benutzer einzuladen, der Mitarbeiter Ihrer Organisation ist. Dieses Szenario entspricht nicht den Lizenzanforderungen und ist nicht zulässig. Durch Verwendung externer Anmeldeinformationen wird aus einem Mitarbeiter kein externer Benutzer.  
- Sie möchten B2B-APIs verwenden, um einen Benutzer aus einem der verbundenen Unternehmen Ihrer Organisation einzuladen. Obwohl der Benutzer in diesem Szenario über B2B-APIs eingeladen wird, gilt dies nicht als B2B-Zusammenarbeit. Das Szenario entspricht nicht den Lizenzanforderungen, da ein Benutzer eines verbundenen Unternehmens kein externer Benutzer ist. 

In beiden Szenarien ist die bessere Lösung, die Benutzer über die B2B-API als Mitglieder (invitedUserType =-Member) einzuladen und ihnen einzeln eine Azure AD-Lizenz zuzuweisen. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

* [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
* [Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit](faq.md)
