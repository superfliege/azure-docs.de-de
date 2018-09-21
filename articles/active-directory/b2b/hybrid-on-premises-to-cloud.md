---
title: Gewähren des Zugriffs auf Cloudressourcen für lokal verwaltete Partnerkonten als Azure AD B2B-Benutzer | Microsoft-Dokumentation
description: Ermöglichen Sie es lokal verwalteten Partnern mithilfe von Azure AD B2B-Zusammenarbeit, mit denselben Anmeldeinformationen auf lokale und cloudbasierte Ressourcen zuzugreifen.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 722ab51d0f591b8f16924d1d5661385267b7f0e6
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295154"
---
# <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources-using-azure-ad-b2b-collaboration"></a>Gewähren des Zugriffs auf Cloudressourcen für lokal verwaltete Partnerkonten mit Azure AD B2B-Zusammenarbeit

Vor Azure Active Directory (Azure AD) haben Unternehmen mit lokalen Identitätssystemen Partnerkonten herkömmlicherweise in ihrem lokalen Verzeichnis verwaltet. Wenn Sie in einem solchen Unternehmen damit beginnen, Apps zu Azure AD zu migrieren, möchten Sie sicherstellen, dass Ihre Partner auf die benötigten Ressourcen zugreifen können. Dabei sollte es keine Rolle spielen, ob die Ressourcen lokal oder in der Cloud gespeichert sind. Zudem sollen Ihre Partnerbenutzer in der Lage sein, für lokale und Azure AD-Ressourcen dieselben Anmeldeinformationen zu verwenden. 

Wenn Sie Konten für externe Partner in Ihrem lokalen Verzeichnis erstellen (beispielsweise ein Konto mit dem Anmeldenamen „mkrause“ für eine externe Benutzerin namens Marie Krause in Ihrer Domäne „partners.contoso.com“), können Sie diese Konten jetzt mit der Cloud synchronisieren. Insbesondere können Sie mithilfe von Azure AD Connect die Partnerkonten als Azure AD B2B-Benutzer (also als Benutzer mit „UserType = Guest“) mit der Cloud synchronisieren. Dadurch können Ihre Partner mit denselben Anmeldeinformationen wie für ihre lokalen Konten auf Cloudressourcen zugreifen, ohne mehr Zugriff als nötig zu erhalten. 

## <a name="identify-unique-attributes-for-usertype"></a>Identifizieren von eindeutigen Attributen für UserType

Vor dem Aktivieren der Synchronisierung des UserType-Attributs müssen Sie entscheiden, wie das UserType-Attribut vom lokalen Active Directory abgeleitet werden soll. Das heißt, welche Parameter in Ihrer lokalen Umgebung sind für Ihre externen Mitarbeiter eindeutig? Bestimmen Sie einen Parameter, der diese externen Mitarbeiter von Mitgliedern Ihrer eigenen Organisation unterscheidet.

Hierbei gibt es zwei gängige Vorgehensweisen:

- Sie legen ein nicht verwendetes lokales Active Directory-Attribut (z.B. extensionAttribute1) als Quellattribut fest. 
- Oder Sie leiten den Wert für das UserType-Attribut aus anderen Eigenschaften ab. Angenommen, Sie möchten alle Benutzer als „Guest“ synchronisieren, wenn ihr lokales Active Directory-Attribut UserPrincipalName mit dem Domänenteil *@partners.contoso.com* endet.
 
Detaillierte Attributanforderungen finden Sie unter [Synchronisierung des Benutzertyps aktivieren](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Konfigurieren von Azure AD Connect für die Synchronisierung von Benutzern mit der Cloud

Nachdem Sie das eindeutige Attribut identifiziert haben, können Sie Azure AD Connect so konfigurieren, dass diese Benutzer mit der Cloud als Azure AD B2B-Benutzer (d.h. Benutzer mit UserType = Guest) synchronisiert werden. Aus Sicht der Autorisierung sind diese Benutzer nicht von B2B-Benutzern zu unterscheiden, die durch den Einladungsprozess von Azure AD B2B-Zusammenarbeit erstellt wurden.

Eine Anleitung zur Implementierung finden Sie unter [Synchronisierung des Benutzertyps aktivieren](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Nächste Schritte

- [Azure Active Directory B2B-Zusammenarbeit für Hybridorganisationen](hybrid-organizations.md)
- [Gewähren des Zugriffs auf lokale Anwendungen für B2B-Benutzer in Azure AD](hybrid-cloud-to-on-premises.md)
- Eine Übersicht zu Azure AD Connect finden Sie unter [Integrieren Ihrer lokalen Verzeichnisse in Azure Active Directory](../hybrid/whatis-hybrid-identity.md).

