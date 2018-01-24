---
title: "Azure Active Directory B2B-Zusammenarbeit für Hybridorganisationen | Microsoft-Dokumentation"
description: "Partnern mit Azure AD B2B-Zusammenarbeit Zugriff auf lokale Ressourcen und Cloudressourcen gewähren"
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 12/15/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 2e690eeea6a9f7e1cc10830a913774daa3c66689
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2017
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Azure Active Directory B2B-Zusammenarbeit für Hybridorganisationen

In einer Hybridorganisation, in der Sie sowohl über lokale Ressourcen als auch über Cloudressourcen verfügen, können Sie externen Partnern Zugriff auf Cloud- und lokale Ressourcen gewähren. Für den Zugriff auf lokale Ressourcen lassen sich Partnerkonten lokal in Ihrer lokalen Active Directory-Umgebung verwalten. Partner erhalten so die Möglichkeit, sich mit ihren Partner-Kontoanmeldeinformationen anzumelden und auf die Ressourcen Ihrer Organisation zuzugreifen. Um Partnern den Zugriff auf Cloudressourcen mit denselben Anmeldeinformationen zu gewähren, können Sie jetzt mithilfe von Azure Active Directory (Azure AD) Connect die Partnerkonten als Azure AD B2B-Benutzer (d.h. Benutzer mit UserType = Guest) mit der Cloud synchronisieren.

## <a name="identify-unique-attributes-for-usertype"></a>Identifizieren von eindeutigen Attributen für UserType

Vor dem Aktivieren der Synchronisierung des UserType-Attributs müssen Sie entscheiden, wie das UserType-Attribut vom lokalen Active Directory abgeleitet werden soll. Das heißt, welche Parameter in Ihrer lokalen Umgebung sind für Ihre externen Mitarbeiter eindeutig? Bestimmen Sie einen Parameter, der diese externen Mitarbeiter von Mitgliedern Ihrer eigenen Organisation unterscheidet.

Hierbei gibt es zwei gängige Vorgehensweisen:

- Sie legen ein nicht verwendetes lokales Active Directory-Attribut (z.B. extensionAttribute1) als Quellattribut fest. 
- Oder Sie leiten den Wert für das UserType-Attribut aus anderen Eigenschaften ab. Angenommen, Sie möchten alle Benutzer als „Guest“ synchronisieren, wenn ihr lokales Active Directory-Attribut UserPrincipalName mit dem Domänenteil *@partners.fabrikam123.org* endet.
 
Detaillierte Attributanforderungen finden Sie unter [Synchronisierung des Benutzertyps aktivieren](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Konfigurieren von Azure AD Connect für die Synchronisierung von Benutzern mit der Cloud

Nachdem Sie das eindeutige Attribut identifiziert haben, können Sie Azure AD Connect so konfigurieren, dass diese Benutzer mit der Cloud als Azure AD B2B-Benutzer (d.h. Benutzer mit UserType = Guest) synchronisiert werden. Aus Sicht der Autorisierung sind diese Benutzer nicht von B2B-Benutzern zu unterscheiden, die durch den Einladungsprozess von Azure AD B2B-Zusammenarbeit erstellt wurden.

Eine Anleitung zur Implementierung finden Sie unter [Synchronisierung des Benutzertyps aktivieren](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht zu Azure AD B2B-Zusammenarbeit finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
- Eine Übersicht zu Azure AD Connect finden Sie unter [Integrieren Ihrer lokalen Verzeichnisse in Azure Active Directory](connect/active-directory-aadconnect.md).

