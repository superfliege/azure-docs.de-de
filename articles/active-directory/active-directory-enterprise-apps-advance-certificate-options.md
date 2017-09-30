---
title: "Erweiterte Optionen für die Zertifikatsignatur im SAML-Token für bereits in Azure Active Directory integrierte Apps | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie erweiterte Optionen für die Zertifikatsignatur im SAML-Token für bereits in Azure Active Directory integrierte Apps verwenden."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: jeedes
ms.custom: aaddev
ms.translationtype: HT
ms.sourcegitcommit: 1868e5fd0427a5e1b1eeed244c80a570a39eb6a9
ms.openlocfilehash: 70e495965287a0edcb31493b69311fe28e04f6dc
ms.contentlocale: de-de
ms.lasthandoff: 09/19/2017

---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Erweiterte Optionen für die Zertifikatsignatur im SAML-Token für Katalog-Apps in Azure Active Directory
Azure Active Directory unterstützt heute Tausende vorab integrierte Anwendungen aus dem Azure AD-Anwendungskatalog – darunter über 500, die einmaliges Anmelden mithilfe des SAML 2.0-Protokolls unterstützen. Wenn sich ein Benutzer mithilfe von SAML über Azure AD bei einer Anwendung authentifiziert, sendet Azure AD ein Token an die Anwendung (über eine HTTP POST-Anfrage). Die Anwendung überprüft und verwendet dann das Token, um den Benutzer anzumelden, anstatt den Benutzernamen und das Kennwort anzufordern. Diese SAML-Token werden mit dem eindeutigen Zertifikat signiert, das in Azure AD generiert wird. Dieses SAML-Token wird mit den spezifischen Standardalgorithmen signiert.

Azure Active Directory verwendet einige der Standardeinstellungen für die Kataloganwendungen. Die Standardwerte werden auf der Grundlage der Anwendungsanforderung eingerichtet.

Azure Active Directory unterstützt erweiterte Einstellungen für die Zertifikatsignatur. Aktivieren Sie zum Auswählen dieser Optionen zunächst das Kontrollkästchen **Erweiterte Einstellungen für die Zertifikatsignatur**:

![Optionen für die Zertifikatsignatur][1]

Wenn dieses Kontrollkästchen aktiviert ist, können Sie die **Optionen für die Zertifikatsignatur** und den **Zertifikatsignaturalgorithmus** einrichten.

## <a name="certificate-signing-options"></a>Optionen für die Zertifikatsignatur

Azure AD unterstützt die drei folgenden Arten von Optionen für die Zertifikatsignatur:

1. **SAML-Assertion signieren**: Die für die meisten Kataloganwendungen festgelegte Standardoption. Bei Verwendung dieser Option signiert Azure AD die SAML-Assertionen und das Zertifikat als IDP mit dem X509-Zertifikat der Anwendung. Außerdem wird der Signaturalgorithmus verwendet, der in der Dropdownliste darunter ausgewählt ist.

2. **SAML-Antwort signieren**: Bei Verwendung dieser Option signiert Azure AD die SAML-Antwort als IDP mit dem X509-Zertifikat der Anwendung. Außerdem wird der Signaturalgorithmus verwendet, der in der Dropdownliste darunter ausgewählt ist.

3. **SAML-Antwort und-Assertion signieren**: Bei Verwendung dieser Option signiert Azure AD das gesamte SAML-Token als IDP mit dem X509-Zertifikat der Anwendung. Außerdem wird der Signaturalgorithmus verwendet, der in der Dropdownliste darunter ausgewählt ist.

    ![Optionen für die Zertifikatsignatur][4]

## <a name="certificate-signing-algorithm"></a>Zertifikatsignaturalgorithmus

Azure Active Directory unterstützt zwei Arten von Signaturalgorithmen zum Signieren der SAML-Antwort:

1. SHA256: Der Standardalgorithmus, den Azure Active Directory zum Signieren der SAML-Antwort verwendet. Hierbei handelt es sich um den neuesten Algorithmus, und er gilt als sicherer als SHA1. Die meisten der Anwendungen unterstützen den SHA256-Algorithmus. Falls die Anwendung nur SHA1 als Signaturalgorithmus unterstützt, können Sie den Algorithmus ändern. Andernfalls empfiehlt es sich, die SAML-Antwort mit dem SHA256-Algorithmus zu signieren.

    ![SHA256-Zertifikatsignaturalgorithmus][3]

2. SHA1: Der ältere, weniger sichere Algorithmus. Falls die Anwendung nur diesen Signaturalgorithmus unterstützt, können Sie diese Option in der Dropdownliste auswählen. Dadurch signiert Azure AD die SAML-Antwort unter Verwendung des SHA1-Algorithmus.

    ![SHA1-Zertifikatsignaturalgorithmus][2]

## <a name="next-steps"></a>Nächste Schritte
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](active-directory-saas-custom-apps.md)
* [Problembehandlung bei SAML-basiertem einmaligem Anmelden](develop/active-directory-saml-debugging.md)

<!--Image references-->

[1]: ./media/active-directory-enterprise-apps-advance-certificate-options/saml-advance-certificate.png
[2]: ./media/active-directory-enterprise-apps-advance-certificate-options/saml-signing-algo-sha1.png
[3]: ./media/active-directory-enterprise-apps-advance-certificate-options/saml-signing-algo-sha256.png
[4]: ./media/active-directory-enterprise-apps-advance-certificate-options/saml-signing-options.png
