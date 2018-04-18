---
title: Zuordnen von Benutzeransprüchen für die B2B-Zusammenarbeit in Azure Active Directory | Microsoft-Dokumentation
description: Passen Sie die Benutzeransprüche an, die im SAML-Token für Azure Active Directory B2B-Benutzer ausgegeben werden.
services: active-directory
documentationcenter: ''
author: twooley
manager: mtillman
editor: ''
tags: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/06/2018
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 8f5e471d4e7102300cd5581976b45c9fa8cc57bc
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Zuordnen von Benutzeransprüchen für die B2B-Zusammenarbeit in Azure Active Directory

Azure Active Directory (Azure AD) unterstützt die Anpassung der Ansprüche, die im SAML-Token für B2B-Zusammenarbeitsbenutzer ausgegeben werden. Wenn sich ein Benutzer bei der Anwendung authentifiziert, stellt Azure AD ein SAML-Token für die App aus, das Informationen (so genannte „Ansprüche“) über den Benutzer enthält, die ihn eindeutig identifizieren. Dazu gehören normalerweise der Benutzername, die E-Mail-Adresse, der Vorname und der Nachname des Benutzers.

Sie können die im SAML-Token an die Anwendung übermittelten Ansprüche im [Azure-Portal](https://portal.azure.com) anzeigen oder bearbeiten. Klicken Sie zum Aufrufen der Einstellungen auf **Azure Active Directory** > **Unternehmensanwendungen** > die für einmaliges Anmelden konfigurierte Anwendung > **Einmaliges Anmelden**. Sehen Sie sich die Einstellungen für das SAML-Token im Abschnitt **Benutzerattribute** an.

![SAML-Tokenattribute auf der Benutzeroberfläche](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

Es gibt zwei Gründe dafür, dass Sie die im SAML-Token ausgegebenen Ansprüche möglicherweise bearbeiten müssen:

1. Die Anwendung wurde erfordert einen anderen Satz an Anspruchs-URIs oder Anspruchswerten.

2. Die Anwendung erfordert, dass es sich beim NameIdentifier-Anspruch nicht um den in Azure AD gespeicherten Benutzerprinzipalnamen (User Principal Name, UPN) handelt.

Informationen zum Hinzufügen und Bearbeiten von Ansprüchen finden Sie unter [Anpassen ausgestellter Ansprüche im SAML-Token für Unternehmensanwendungen in Azure Active Directory](develop/active-directory-saml-claims-customization.md).

Aus Sicherheitsgründen ist eine mandantenübergreifende Zuordnung von NameID und UPN für Benutzer der B2B-Zusammenarbeit nicht möglich.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den Eigenschaften von B2B-Zusammenarbeitsbenutzern finden Sie unter [Eigenschaften eines Azure Active Directory B2B-Zusammenarbeitsbenutzers](active-directory-b2b-user-properties.md).
- Informationen zu Benutzertoken für B2B-Zusammenarbeitsbenutzer finden Sie unter [Informationen zu Benutzertoken für die Azure AD B2B-Zusammenarbeit](active-directory-b2b-user-token.md).

