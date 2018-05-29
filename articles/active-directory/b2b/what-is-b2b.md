---
title: Was ist die Azure Active Directory B2B-Zusammenarbeit? | Microsoft-Dokumentation
description: Die Azure Active Directory B2B-Zusammenarbeit ermöglicht Geschäftspartnern den gezielten Zugriff auf Ihre Unternehmensanwendungen und unterstützt so Ihre unternehmensübergreifenden Beziehungen.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/26/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 2bc405e6356113e0423f833868c86890c0c3d5d2
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
ms.locfileid: "34259591"
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>Was ist die Azure AD B2B-Zusammenarbeit?

Mit den Funktionen der Azure AD B2B-Zusammenarbeit (Azure Active Directory Business-to-Business) kann jede Organisation, die Azure AD verwendet, auf sichere Weise mit Benutzern aus jeder anderen Organisation zusammenarbeiten – unabhängig von deren Größe. Diese Organisationen sind nicht von Azure AD abhängig und müssen nicht einmal über eine IT-Abteilung verfügen.

Organisationen, die Azure AD verwenden, können für ihre Partner Zugriff auf Dokumente, Ressourcen und Anwendungen bereitstellen und dabei die vollständige Kontrolle über ihre eigenen Unternehmensdaten behalten. Entwickler können mit den Azure AD B2B-APIs Anwendungen schreiben, die zwei Organisationen sicherer miteinander verbinden. Und auch die Endbenutzernavigation gestaltet sich unkompliziert.

Im folgenden Video wird eine nützliche Übersicht gezeigt.
>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

## <a name="key-benefits-of-azure-ad-b2b-collaboration"></a>Wichtige Vorteile der Azure AD B2B-Zusammenarbeit

### <a name="work-with-any-user-from-any-partner"></a>Zusammenarbeit mit jedem Benutzer bei jedem Partner

- Partner verwenden eigene Anmeldeinformationen
- Partner müssen nicht Azure AD verwenden
- Keine externen Verzeichnisse und keine komplexe Einrichtung erforderlich

### <a name="simple-and-secure-collaboration"></a>Einfache und sichere Zusammenarbeit

- Zugriff auf alle Unternehmens-Apps oder -daten gemäß differenzierten Azure AD-Autorisierungsrichtlinien
- Benutzerfreundlich
- Sicherheit auf Unternehmensniveau für Apps und Daten

### <a name="no-management-overhead"></a>Kein zusätzlicher Verwaltungsaufwand

- Keine externe Konto- oder Kennwortverwaltung
- Keine Synchronisierung oder manuelle Verwaltung des Kontolebenszyklus
- Kein zusätzlicher externer Verwaltungsaufwand

## <a name="easily-add-b2b-collaboration-users"></a>Einfaches Hinzufügen von Benutzern für die B2B-Zusammenarbeit

Als Administrator können Sie im [Azure-Portal](https://portal.azure.com) problemlos (Gast-)Benutzer für die B2B-Zusammenarbeit zu Ihrer Organisation hinzufügen.

![Gastbenutzer hinzufügen](media/what-is-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a>Ermöglichen Sie den Projektmitarbeitern, sich mit der eigenen Identität anzumelden

B2B-Projektmitarbeiter können sich mit einer Identität ihrer Wahl anmelden. Falls der Benutzer über kein Microsoft- oder Azure AD-Konto verfügt, wird zum Zeitpunkt der Einlösung des Angebots nahtlos ein entsprechendes Konto erstellt.

### <a name="delegate-to-application-and-group-owners"></a>Delegation an Anwendungs- und Gruppenbesitzer

Als Anwendungs- oder Gruppenbesitzer können Sie jeder gewünschten Anwendung direkt B2B-Benutzer hinzufügen, und dies muss keine Microsoft-Anwendung sein. Administratoren können die Berechtigung zum Hinzufügen von B2B-Benutzern an Nicht-Administratoren delegieren. Nicht-Administratoren können im Bereich [Azure AD-Anwendungszugriffsbereich](https://myapps.microsoft.com) Anwendungen oder Gruppen B2B-Zusammenarbeitsbenutzer hinzufügen.

![Zugriffsbereich](media/what-is-b2b/access-panel.png)

![Mitglied hinzufügen](media/what-is-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Autorisierungsrichtlinien schützen Ihre Unternehmensinhalte

Richtlinien für bedingten Zugriff (beispielsweise Multi-Factor Authentication) können erzwungen werden:
- Auf Mandantenebene
- Auf Anwendungsebene
- Für bestimmte Benutzer, um Apps und Daten des Unternehmens zu schützen

### <a name="use-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>Mit APIs und einem Beispielcode können Sie einfach Anwendungen erstellen, die integriert werden sollen

Integrieren Sie Ihre externen Partner entsprechend den Anforderungen Ihrer Organisation.

Mithilfe der [Einladungs-APIs der B2B-Zusammenarbeit](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) können Sie Ihre Onboardingprozesse anpassen und unter anderem Self-Service-Registrierungsportale erstellen. Beispielcode für ein Self-Service-Portal finden Sie auf [Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![Registrierungsportal](media/what-is-b2b/sign-up-portal.png)

Mit der Azure AD B2B-Zusammenarbeit können Sie das gesamte Potenzial von Azure AD nutzen, um Ihre Partnerbeziehungen auf eine Weise zu schützen, die Endbenutzer als einfach und intuitiv empfinden.

## <a name="next-steps"></a>Nächste Schritte

- [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](add-users-administrator.md)
- [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](add-users-information-worker.md)
- [B2B-Zusammenarbeit: Einlösen von Einladungen](redemption-experience.md)
- [Lizenzierung der Azure AD B2B-Zusammenarbeit](licensing-guidance.md)
- Und wie immer empfehlen wir, sich über unsere [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b) mit dem Produktteam in Verbindung zu setzen, um Feedback zu geben, Fragen zu erörtern und Vorschläge zu unterbreiten.