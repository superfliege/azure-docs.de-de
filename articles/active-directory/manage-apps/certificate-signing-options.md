---
title: Erweiterte Optionen für die Zertifikatsignatur im SAML-Token für bereits in Azure Active Directory integrierte Apps | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie erweiterte Optionen für die Zertifikatsignatur im SAML-Token für bereits in Azure Active Directory integrierte Apps verwenden.
services: active-directory
documentationcenter: ''
author: jeevansd
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: 16cb3e034bd928bbae8ae1f2e8d6b94b9ec699e4
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35302540"
---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Erweiterte Optionen für die Zertifikatsignatur im SAML-Token für Katalog-Apps in Azure Active Directory
Azure Active Directory (Azure AD) unterstützt im Anwendungskatalog von Azure Active Directory mittlerweile Tausende von vorab integrierten Anwendungen. Hierin sind auch mehr als 500 Anwendungen enthalten, die einmaliges Anmelden mit dem SAML 2.0-Protokoll unterstützen. Wenn sich ein Benutzer mithilfe von SAML über Azure AD bei einer Anwendung authentifiziert, sendet Azure AD ein Token an die Anwendung (per HTTP POST). Die Anwendung überprüft und verwendet dann das Token, um den Benutzer anzumelden, anstatt den Benutzernamen und das Kennwort anzufordern. Diese SAML-Token werden mit dem eindeutigen Zertifikat, das in Azure AD generiert wird, und mit spezifischen Standardalgorithmen signiert.

Azure AD verwendet einige der Standardeinstellungen für die Kataloganwendungen. Die Standardwerte werden auf der Grundlage der Anwendungsanforderungen eingerichtet.

Azure AD unterstützt erweiterte Einstellungen für die Zertifikatsignatur. Aktivieren Sie zuerst das Kontrollkästchen **Erweiterte Einstellungen für die Zertifikatsignatur**, um diese Optionen auszuwählen:

![Erweiterte Einstellungen für die Zertifikatsignatur](./media/certificate-signing-options/saml-advance-certificate.png)

Nachdem Sie dieses Kontrollkästchen aktiviert haben, können Sie die Zertifikatsignaturoptionen und den Zertifikatsignaturalgorithmus einrichten.

## <a name="certificate-signing-options"></a>Optionen für die Zertifikatsignatur

Azure AD unterstützt drei Optionen für die Zertifikatsignatur:

* **SAML-Assertion signieren**: Diese Standardoption ist für die meisten Kataloganwendungen festgelegt. Bei Verwendung dieser Option signiert Azure AD die SAML-Assertionen und das Zertifikat als IdP mit dem X509-Zertifikat der Anwendung. Außerdem wird der Signaturalgorithmus verwendet, der in der Dropdownliste **Signaturalgorithmus** darunter ausgewählt ist.

* **SAML-Antwort signieren**: Bei Verwendung dieser Option signiert Azure AD die SAML-Antwort als IdP mit dem X509-Zertifikat der Anwendung. Außerdem wird der Signaturalgorithmus verwendet, der in der Dropdownliste **Signaturalgorithmus** ausgewählt ist.

* **SAML-Antwort und -Assertion signieren**: Bei Verwendung dieser Option signiert Azure AD das gesamte SAML-Token als IdP mit dem X509-Zertifikat der Anwendung. Außerdem wird der Signaturalgorithmus verwendet, der in der Dropdownliste **Signaturalgorithmus** ausgewählt ist.

    ![Optionen für die Zertifikatsignatur](./media/certificate-signing-options/saml-signing-options.png)

## <a name="certificate-signing-algorithms"></a>Zertifikatsignaturalgorithmen

Azure AD unterstützt zwei Signaturalgorithmen zum Signieren der SAML-Antwort:

* **SHA-256**: Azure AD verwendet diesen Standardalgorithmus zum Signieren der SAML-Antwort. Hierbei handelt es sich um den neuesten Algorithmus, der als sicherer als SHA-1 gilt. Die meisten Anwendungen unterstützen den SHA-256-Algorithmus. Wenn eine Anwendung nur SHA-1 als Signaturalgorithmus unterstützt, können Sie dies ändern. Andernfalls wird empfohlen, den SHA-256-Algorithmus zum Signieren der SAML-Antwort zu verwenden.

    ![SHA-256-Zertifikatsignaturalgorithmus](./media/certificate-signing-options/saml-signing-algo-sha256.png)

* **SHA-1**: Dies ist der ältere Algorithmus, der als weniger sicher als SH-256 gilt. Wenn eine Anwendung nur diesen Signaturalgorithmus unterstützt, können Sie diese Option in der Dropdownliste **Signaturalgorithmus** auswählen. Azure AD signiert die SAML-Antwort dann mit dem SHA-1-Algorithmus.

    ![SHA-1-Zertifikatsignaturalgorithmus](./media/certificate-signing-options/saml-signing-algo-sha1.png)

## <a name="next-steps"></a>Nächste Schritte
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Problembehandlung bei SAML-basiertem einmaligem Anmelden](../develop/active-directory-saml-debugging.md)


