---
title: Debuggen des SAML-basierten einmaligen Anmeldens – Azure Active Directory | Microsoft-Dokumentation
description: Debuggen des SAML-basierten einmaligen Anmeldens bei Anwendungen in Azure Active Directory.
services: active-directory
author: CelesteDG
documentationcenter: na
manager: mtillman
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/15/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin, dastrock, smalser
ms.openlocfilehash: 8bb0df567fbac6e8f8a2e2f64f868b4f219e05ac
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751393"
---
# <a name="debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Debuggen des SAML-basierten einmaligen Anmeldens bei Anwendungen in Azure Active Directory.

Erfahren Sie, wie Sie Probleme beim [einmaligen Anmelden](../manage-apps/what-is-single-sign-on.md) bei Anwendungen in Azure Active Directory (Azure AD), die [Security Assertion Markup Language (SAML) 2.0](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) unterstützen, finden und beheben. 

## <a name="before-you-begin"></a>Voraussetzungen
Sie sollten die [Erweiterung zur sicheren Anmeldung bei „Meine Apps“](../active-directory-saas-access-panel-user-help.md#i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension) installieren. Diese Browsererweiterung erleichter das Erfassen der SAML-Anforderung sowie die SAML-Antwortinformationen, die Sie für die Behebung von Problemen mit einmaligem Anmelden benötigen. Für den Fall, dass Sie die Erweiterung nicht installieren können, zeigt dieser Artikel Ihnen die Behebung von Problemen mit und ohne Erweiterung.

Verwenden Sie zum Herunterladen und Installieren der Erweiterung zur sicheren Anmeldung bei „Meine Apps“ einen der folgenden Links.

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)


## <a name="test-saml-based-single-sign-on"></a>Testen des einmaligen SAML-basierten Anmeldens

So testen Sie das einmalige SAML-basierte Anmelden zwischen AAD und einer Zielanwendung:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler oder sonstiger Administrator an, der autorisiert ist, Anwendungen zu verwalten.
2.  Klicken Sie im linken Blatt auf **Azure Active Directory** und dann auf **Unternehmensanwendungen**. 
3.  Klicken Sie in der Liste der Unternehmensanwendungen auf die Anwendung, für die Sie einmaliges Anmelden testen möchten, und klicken Sie dann in den Optionen links auf **Einmaliges Anmelden**.
4.  Um das Testen des SAML-basierten einmaligen Anmeldens aufzurufen, klicken Sie im Abschnitt **Domänen und URLs** auf **SAML-Einstellungen testen**. Wenn die Schaltfläche „SAML-Einstellungen testen“ abgeblendet ist, müssen Sie zuerst die erforderlichen Attribute ausfüllen und speichern.
5.  Melden Sie sich auf dem Blatt **Einmaliges Anmelden testen** mit Ihren Unternehmensanmeldeinformationen bei der Zielanwendung an. Sie können sich als der aktuelle Benutzer oder anderer Benutzer anmelden. Wenn Sie sich als anderer Benutzer anmelden, werden Sie in einer Eingabeaufforderung aufgefordert, sich zu authentifizieren.

    ![Test-SAML-Seite](media/active-directory-saml-debugging/testing.png)


Wenn Sie sich erfolgreich angemeldet haben, haben Sie den Test bestanden. Azure AD hat in diesem Fall ein SAML-Antworttoken für die Anwendung ausgestellt. Die Anwendung verwendete das SAML-Token, um Sie erfolgreich anzumelden.

Wenn die Unternehmensanmeldeseite oder die Anwendungsseite einen Fehler aufweist, lösen Sie den Fehler in einem der nächsten Abschnitte auf.


## <a name="resolve-a-sign-in-error-on-your-company-sign-in-page"></a>Auflösen eines Anmeldefehlers auf Ihrer Unternehmensanmeldeseite

Wenn Sie versuchen, sich anzumelden, sehen Sie möglicherweise einen Fehler auf Ihrer Unternehmensanmeldeseite. 

![Anmeldefehler](media/active-directory-saml-debugging/error.png)

Um diesen Fehler zu debuggen, benötigen Sie die Fehlermeldung und die SAML-Anforderung. Die Erweiterung zur sicheren Anmeldung bei „Meine Apps“ erfasst automatisch diese Informationen und zeigt eine Auflösungsanleitung in Azure AD an. 

So lösen Sie den Anmeldefehler auf, wenn die Erweiterung zur sicheren Anmeldung bei „Meine Apps“ installiert ist:

1.  Wenn ein Fehler auftritt, leitet die Erweiterung Sie zurück zum Azure AD-Blatt **Einmaliges Anmelden testen**. 
2.  Klicken Sie auf dem Blatt **Einmaliges Anmelden testen** auf **SAML-Anforderung herunterladen**. 
3.  Sie sollten eine spezifische Auflösungsanleitung auf Basis der Fehler und Werte in der SAML-Anforderung sehen. Lesen Sie die Anleitung.

So lösen Sie den Fehler auf, wenn die Erweiterung zur sicheren Anmeldung bei „Meine Apps“ nicht installiert ist:

1. Kopieren Sie die Fehlermeldung rechts unten auf der Seite. Die Fehlermeldung enthält:
    - Eine Korrelations-ID und einen Zeitstempel. Diese Werte werden bei der Erstellung einer Supportanfrage an Microsoft benötigt, um Ihr Problem zu identifizieren und einen Lösungsvorschlag zu unterbreiten.
    - Eine Anweisung, die die Ursache des Problems identifiziert.
2.  Wechseln Sie zurück zu Azure AD, und suchen Sie das Blatt **Einmaliges Anmelden testen**.
3.  Fügen Sie die Fehlermeldung oben in das Textfeld **Leitfaden zur Problemlösung abrufen** ein.
3.  Klicken Sie auf **Leitfaden zur Problemlösung abrufen**, um Schritte zur Lösung des Problems anzuzeigen. Der Leitfaden könnte Informationen aus der SAML-Anforderung bzw. -Antwort benötigen. Wenn Sie die Erweiterung zur sicheren Anmeldung bei „Meine Apps“ nicht verwenden, könnten Sie mit einem Tool wie [Fiddler](http://www.telerik.com/fiddler) die SAML-Anforderung und -Antwort abrufen.
4.  Vergewissern Sie sich, dass das Ziel in der SAML-Anforderung der URL des SAML-Diensts für einmaliges Anmelden aus Azure Active Directory entspricht.
5.  Vergewissern Sie sich, dass der Aussteller in der SAML-Anforderung dem Bezeichner entspricht, den Sie in Azure Active Directory für die Anwendung konfiguriert haben. Azure AD sucht anhand des Ausstellers in Ihrem Verzeichnis nach einer Anwendung.
6.  Vergewissern Sie sich, dass AssertionConsumerServiceURL dort ist, wo die Anwendung das SAML-Token aus Azure Active Directory erwartet. Sie können diesen Wert in Azure Active Directory konfigurieren, er ist jedoch nicht zwingend erforderlich, wenn er Teil der SAML-Anforderung ist.


## <a name="resolve-a-sign-in-error-on-the-application-page"></a>Auflösen eines Anmeldefehlers auf der Anwendungsseite

Sie können sich vielleicht erfolgreich anmelden und sehen dann die Anzeige einer Fehlermeldung auf der Anwendungsseite. Dies geschieht, wenn Azure AD ein Token für die Anwendung ausgestellt hat, aber die Anwendung die Antwort nicht akzeptiert.   

So beheben Sie den Fehler

1. Wenn sich die Anwendung im Azure AD-Katalog befindet, vergewissern Sie sich, dass Sie alle Schritte zur Integration der Anwendung in Azure AD ausgeführt haben. Die Integrationsanweisungen für Ihre Anwendung finden Sie in der [Liste der Tutorials zur Integration von SaaS-Anwendungen](../saas-apps/tutorial-list.md).
2. Rufen Sie die SAML-Antwort ab.
    - Wenn die Erweiterung zur sicheren Anmeldung bei „Meine Apps“ installiert ist, klicken Sie im Blatt **Einmaliges Anmelden testen** auf **SAML-Antwort herunterladen**.
    - Wenn die Erweiterung nicht installiert ist, verwenden Sie ein Tool wie [Fiddler](http://www.telerik.com/fiddler) zum Abrufen der SAML-Antwort. 
3. Beachten Sie diese Elemente im SAML-Antworttoken:
    - Für den Benutzer eindeutiger Bezeichner von Wert und Format der NameID
    - Im Token ausgestellte Ansprüche
    - Zum Signieren des Tokens verwendetes Zertifikat. Informationen zum Überprüfen der SAML-Antwort finden Sie unter [SAML-Protokoll für einmaliges Anmelden](active-directory-single-sign-on-protocol-reference.md).
4. Weitere Informationen zur SAML-Antwort finden Sie unter [SAML-Protokoll für einmaliges Anmelden](active-directory-single-sign-on-protocol-reference.md).
5. Da Sie nun die SAML-Antwort überprüft haben, suchen Sie unter [Fehler auf der Seite einer Anwendung nach dem Anmelden](../application-sign-in-problem-application-error.md) nach Anleitung zum Auflösen des Problems. 
6. Wenn Sie sich immer noch nicht erfolgreich anmelden können, fragen Sie den Anwendungsanbieter, was in der SAML-Antwort fehlt.


## <a name="next-steps"></a>Nächste Schritte
Da nun einmaliges Anmelden für Ihre Anwendung funktioniert, können Sie mit [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../active-directory-saas-app-provisioning.md) oder [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](../active-directory-conditional-access-azure-portal-get-started.md) fortfahren.


