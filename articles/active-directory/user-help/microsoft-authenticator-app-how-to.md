---
title: Erste Schritte mit der Microsoft Authenticator-App – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie für Microsoft Authenticator ein Upgrade auf die neueste Version durchführen.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.reviewer: librown
ms.openlocfilehash: d5ea6f18fb288711bae67b6840ddb5b8bb9942c2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245442"
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a>Erste Schritte mit der Microsoft Authenticator-App

Die Microsoft Authenticator-App bietet eine zusätzliche Sicherheitsstufe für Ihr Geschäfts-, Schul- oder Unikonto (z.B. alain@contoso.com) oder Ihr persönliches Microsoft-Konto (z.B. alain@outlook.com) und unterstützt Sie dabei, nicht autorisierte Zugriffe auf Konten sowie betrügerische Transaktionen zu unterbinden.

Wenn Sie die App für die zweistufige Überprüfung verwenden, gibt es zwei Möglichkeiten:

- **Benachrichtigung**. Die App sendet eine Benachrichtigung an Ihr Gerät. Stellen Sie sicher, dass die Informationen in der Benachrichtigung richtig sind, und wählen Sie **Überprüfen** aus. Wenn Sie die Benachrichtigung nicht kennen, wählen Sie **Ablehnen** aus. Nachdem Sie auf **Ablehnen** geklickt haben, können Sie die Anforderung auch als betrügerisch markieren.

- **Prüfcode**. Nachdem Sie Ihren Benutzernamen und Ihr Kennwort eingegeben haben, können Sie die App öffnen und den auf dem Bildschirm **Konten** angezeigten Prüfcode in den Anmeldebildschirm kopieren. Der Prüfcode fungiert als zweite Authentifizierungsmethode.

## <a name="opt-in-for-two-step-verification"></a>Abonnieren der zweistufigen Überprüfung

Ihre Organisation entscheidet, ob Sie die zweistufige Überprüfung für Ihr Geschäfts-, Schul- oder Unikonto verwenden. Ihr Administrator informiert Sie darüber, welche Überprüfungsmethoden eingerichtet und verwendet werden müssen. Weitere Informationen finden Sie unter [Welchen Zweck erfüllen App-Kennwörter bei Azure Multi-Factor Authentication?](multi-factor-authentication-end-user.md).

Für Ihr persönliches Microsoft-Konto können Sie die zweistufige Überprüfung selbst einrichten. Detaillierte Anweisungen und Informationen finden Sie unter [Informationen zur Überprüfung in zwei Schritten](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

Sie können die Microsoft Authenticator-App auch mit Nicht-Microsoft-Konten verwenden. Die Funktion heißt in diesen Konten möglicherweise nicht „zweistufige Überprüfung“, aber Sie sollten sie bei den Sicherheits- oder Anmeldeeinstellungen finden. Weitere Informationen zum Einrichten von Nicht-Microsoft-Konten finden Sie in den [Videos des Microsoft-Kundendiensts](https://www.youtube.com/playlist?list=PLyhj1WZ29G65QdD9NxTOAm8HwOS-OBUrX).

## <a name="install-the-app"></a>Installieren der App

Die Microsoft Authenticator-App ist für [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) und [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071) verfügbar. Um ein optimales Benutzererlebnis zu erzielen, sollten Sie Benachrichtigungen für die App aktivieren, wenn Sie danach gefragt werden. 

## <a name="add-accounts-to-the-app"></a>Hinzufügen von Konten zur App

Sie können Geschäfts-, Schul- oder Unikonten oder persönliche Konten zur Microsoft Authenticator-App hinzufügen. 

### <a name="add-a-personal-microsoft-account"></a>Hinzufügen eines persönlichen Microsoft-Kontos

Für ein persönliches Microsoft-Konto (das Sie zur Anmeldung bei Outlook.com, Xbox, Skype usw. verwenden) müssen Sie sich lediglich in der Microsoft Authenticator-App bei Ihrem Konto anmelden.

### <a name="add-a-work-or-school-account"></a>Hinzufügen eines Geschäfts-, Schul- oder Unikontos

1. Wenn möglich, öffnen Sie auf einem anderen Computer oder Gerät den Bildschirm [Zusätzliche Sicherheitsüberprüfung](https://aka.ms/mfasetup). Informationen dazu, wie Sie auf diesen Bildschirm gelangen, finden Sie unter [Ändern der Sicherheitseinstellungen](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page). Alternativ dazu können Sie sich auch an Ihren Administrator wenden.

    >[!Note]
    >Wenn der Administrator die Vorschauversion der Benutzeroberfläche für Sicherheitsinformationen aktiviert hat, können Sie die Anweisungen im Abschnitt [Einrichten der Sicherheitsinformationen zur Verwendung einer Authenticator-App](security-info-setup-auth-app.md) befolgen.

2. Aktivieren Sie das Kontrollkästchen neben **Authenticator-App**, und wählen Sie dann **Konfigurieren**.

    ![Schaltfläche „Konfigurieren“ auf der Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung](./media/microsoft-authenticator-app-how-to/auth-app-configure.png)

    Der Bildschirm **Mobile App konfigurieren** wird geöffnet und zeigt einen QR-Code an, den Sie mit der Authentifizierungs-App scannen können.

    ![Bildschirm, der den QR-Code enthält](./media/microsoft-authenticator-app-how-to/auth-app-barcode.png)

3. Öffnen Sie die Microsoft Authenticator-App. Wählen Sie auf dem Bildschirm **Konten** nacheinander die Optionen **Konto hinzufügen** und **Geschäfts-, Schul- oder Unikonto**.

4. Verwenden Sie die Kamera Ihres Geräts, um den QR-Code zu scannen, und wählen Sie **Fertig**, um den Bildschirm mit dem QR-Code zu schließen.

    >[!Note]
    >Wenn die Kamera nicht ordnungsgemäß funktioniert, können Sie den [QR-Code und die URL manuell eingeben](#add-an-account-to-the-app-manually).

    Der Bildschirm **Konten** der App zeigt den Namen Ihres Kontos und einen sechsstelligen Prüfcode an. Um die Sicherheit zu erhöhen, wird der Prüfcode alle 30 Sekunden geändert, um zu verhindern, dass Sie denselben Code zweimal verwenden.  

    ![Bildschirm „Konten“](./media/microsoft-authenticator-app-how-to/auth-app-accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Manuelles Hinzufügen eines Kontos zur App

1. Wechseln Sie zum Bildschirm **Zusätzliche Sicherheitsüberprüfung**. Informationen dazu, wie Sie auf diese Seite gelangen, finden Sie unter [Ändern der Sicherheitseinstellungen](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).

2. Aktivieren Sie das Kontrollkästchen neben **Authenticator-App**, und wählen Sie dann **Konfigurieren**.

    Der Bildschirm **Mobile App konfigurieren** wird angezeigt.

3. Kopieren Sie den Code und die URL aus dem Bildschirm **Mobile App konfigurieren**, sodass Sie diese Informationen manuell in den QE-Scanner eingeben können.

4. Öffnen Sie die Microsoft Authenticator-App. Wählen Sie auf dem Bildschirm **Konten** nacheinander die Optionen **Konto hinzufügen** und **Geschäfts-, Schul- oder Unikonto**.

5. Wählen Sie auf dem Bildschirm des QR-Scanners die Option **Code manuell eingeben** aus.

    ![Bildschirm zum Scannen eines QR-Codes](./media/microsoft-authenticator-app-how-to/auth-app-manual-code.png)
   
6. Geben Sie den Code und die URL aus dem Bildschirm mit dem QR-Code in den Bildschirm **Konto hinzufügen** ein, und wählen Sie dann **Fertig stellen** aus.

    ![Bildschirm für die Eingabe von Code und URL](./media/microsoft-authenticator-app-how-to/auth-app-code-url.png)

    Der Bildschirm **Konten** der App zeigt den Namen Ihres Kontos und einen sechsstelligen Prüfcode an. Um die Sicherheit zu erhöhen, wird der Prüfcode alle 30 Sekunden geändert, um zu verhindern, dass Sie denselben Code zweimal verwenden.

### <a name="using-your-devices-fingerprint-or-facial-recognition-capabilities"></a>Verwenden der Fingerabdruck- oder Gesichtserkennungsfunktionen Ihres Geräts

Möglicherweise ist in Ihrem Unternehmen eine PIN erforderlich, um die Identitätsüberprüfung abzuschließen. Sie können die Microsoft Authenticator-App so einrichten, dass sie statt einer PIN die Fingerabdruck- oder Gesichtserkennungsfunktionen Ihres Geräts verwendet. Sie können diese Funktionalität einrichten, wenn Sie die Authenticator-App zum ersten Mal zum Überprüfen Ihres Kontos verwenden. Wählen Sie dabei die Option zum Verwenden der Biometriefunktionen Ihres Geräts anstelle Ihrer PIN für die Identifizierung aus.

## <a name="use-the-app-when-you-sign-in"></a>Verwenden der App bei der Anmeldung

Nachdem Sie Ihre Konten zur App hinzugefügt haben, können Sie die App verwenden, um sich bei Ihren Konten anzumelden.

Wenn Sie die Verwendung von Prüfcodes in der App aktiviert haben, werden diese auf der Seite **Konten** angezeigt. Die Codes ändern sich alle 30 Sekunden, sodass Sie stets über einen neuen Code verfügen, wenn Sie einen benötigen. Sie müssen sie jedoch erst beachten, wenn Sie sich anmelden und zur Eingabe eines Überprüfungscodes aufgefordert werden.

## <a name="next-steps"></a>Nächste Schritte

- Antworten auf allgemeinere Fragen zur App finden Sie unter [Microsoft Authenticator-App – häufig gestellte Fragen](microsoft-authenticator-app-faq.md).

- Weitere Informationen zur zweistufigen Überprüfung finden Sie unter [Einrichten meines Kontos für die Überprüfung in zwei Schritten](multi-factor-authentication-end-user-first-time.md).

- Weitere Informationen zu Sicherheitsinformationen finden Sie unter [Verwalten Ihrer Sicherheitsinformationen](security-info-manage-settings.md).
