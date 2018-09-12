---
title: Verwalten der Sicherheitsinformation – Azure Active Directory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Ihre Sicherheitsinformation verwalten und beispielsweise die Einstellungen der zweistufigen Überprüfung verwenden.
services: active-directory
author: eross-msft
manager: mtillman
ms.reviewer: sahenry
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: f9a77c1f3fbce295a40db366619d7485d44efc4f
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162715"
---
# <a name="manage-your-security-info-preview"></a>Verwalten der Sicherheitsinformation (Vorschauversion)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Sie können Ihre Sicherheitsinformation für die Anmeldung bei Ihrem Geschäfts-, Schul- oder Unikonto und für die Zurücksetzung Ihres Kennworts verwenden.

Abhängig von den Einstellungen Ihrer Organisation wird beim Anmelden unter Umständen das Kontrollkästchen **Die nächsten X Tage nicht erneut fragen** angezeigt. Wenn Sie dieses Kontrollkästchen aktivieren, können Sie bei Ihrem Gerät so lange angemeldet bleiben, wie dies vom Administrator konfiguriert wurde, ohne dass eine erneute Überprüfung erforderlich ist.

## <a name="change-your-info"></a>Ändern der Information
Basierend auf den vom Administrator und von Ihrer Organisation zugelassenen Werten können Sie eine Sicherheitsinformation aktualisieren oder hinzufügen bzw. Ihre Standardangabe ändern.

### <a name="to-change-your-info"></a>So ändern Sie die Information

1. Melden Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto an.

2. Rufen Sie „myapps.microsoft.com“ auf, und klicken Sie oben rechts auf der Seite auf Ihren Namen und dann auf **Profil**.

3. Klicken Sie im Bereich **Konto verwalten** auf **Edit security info** (Sicherheitsinformation bearbeiten).

    ![Bildschirm „Profil“ mit hervorgehobenem Link „Edit security info“ (Sicherheitsinformation bearbeiten)](media/security-info/security-info-profile.png)

4. Verwenden Sie Ihre Standardmethode, um den Zugriff zu genehmigen und die aktuellen Details zur Sicherheitsinformation anzuzeigen, falls Ihr Administrator diese Umgebung für Ihre Organisation eingerichtet hat.

5. Auf der Seite **Keep your account secure** (Konto schützen) stehen folgende Optionen zur Verfügung:

    - Klicken Sie auf **Add security info** (Sicherheitsinformation hinzufügen), um zusätzliche Methoden hinzuzufügen.

    - Klicken Sie auf **Standard ändern**, um die Standardmethode zu ändern.

    - Klicken Sie neben einer vorhandenen Methode auf das **Stiftsymbol**, um Ihre Informationen zu aktualisieren.

    ![Bildschirm „Sicherheitsinformation“ mit vorhandenen, bearbeitbaren Informationen](media/security-info/security-info-edit.png)

6. Nachdem Sie Ihre Änderungen vorgenommen haben, können Sie die Seite verlassen, und Ihre Änderungen werden gespeichert.

Wenn diese Optionen nicht angezeigt werden oder Sie nicht auf die Seite „myapps.microsoft.com“ zugreifen können, verwendet Ihre Organisation möglicherweise benutzerdefinierte Optionen oder eine benutzerdefinierte Seite. Sie müssen sich an den Administrator wenden, um weitere Unterstützung zu erhalten.

## <a name="manage-your-security-info-for-a-lost-or-potentially-compromised-device"></a>Verwalten Ihrer Sicherheitsinformation für ein verlorenes oder möglicherweise kompromittiertes Gerät

Wenn Sie Ihr Gerät verlieren oder Ihr Gerät kompromittiert wird, müssen Sie den Überprüfungsprozess für alle Geräte wiederholen, die zuvor als vertrauenswürdig eingestuft worden sind.

### <a name="to-manage-your-security-info-for-lost-or-potentially-compromised-devices"></a>So verwalten Sie die Sicherheitsinformation für ein verlorenes oder möglicherweise kompromittiertes Gerät

1. Melden Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto an.

2. Rufen Sie „myapps.microsoft.com“ auf, und klicken Sie oben rechts auf der Seite auf Ihren Namen und dann auf **Profil**.

3. Klicken Sie im Bereich **Konto verwalten** auf **Forget MFA on remembered devices** (MFA auf gespeicherten Geräten löschen).
    
    Wenn Sie diese Option auswählen, müssen Sie den Prozess für Multi-Factor Authentication nach der Anmeldung erneut durchlaufen.

    ![Bildschirm „Profil“ mit hervorgehobenem Link zum Löschen](media/security-info/security-info-forget.png)

## <a name="common-problems-and-solutions-with-your-security-info"></a>Allgemeine Probleme und Lösungen in Bezug auf Ihre Sicherheitsinformation

Dieser Artikel unterstützt Sie beim Beheben von Problemen mit der Sicherheitsinformation, u.a. bei Problemen mit der zweistufigen Überprüfung.

|Problem|Lösung|
|-------|--------|
|Ich habe mein Smartphone nicht bei mir.|Möglicherweise haben Sie Ihr Smartphone nicht immer dabei, möchten sich aber dennoch bei Ihrem Geschäfts-, Schul- oder Unikonto anmelden. Um dieses Problem zu beheben, können Sie sich mit einer anderen Authentifizierungsmethode anmelden, für die Ihr Smartphone nicht erforderlich ist, beispielsweise mit Ihrer E-Mail-Adresse oder der Bürotelefonnummer. Um weitere Methoden zur Sicherheitsinformation hinzuzufügen, können Sie die Schritte im Abschnitt [Ändern der Information](#change-your-info) ausführen.|
|Ich habe mein Telefon verloren, oder es wurde mir gestohlen|Leider kann Ihr Telefon verloren gehen oder gestohlen werden. In diesem Fall wird dringend empfohlen, Ihre Organisation zu informieren, damit das IT-Team Ihre App-Kennwörter zurücksetzen und alle gespeicherten Geräte aus der Liste der vertrauenswürdigen Geräte löschen kann. Sie können auch Ihre eigenen vertrauenswürdigen Geräte löschen, indem Sie die Schritte im Abschnitt [Verwalten Ihrer Sicherheitsinformation für ein verlorenes oder möglicherweise kompromittiertes Gerät](#manage-your-security-info-for-a-lost-or-potentially-compromised-device) ausführen.|
|Ich habe eine neue Telefonnummer.|Es gibt zwei Möglichkeiten, dieses Problem zu beheben. Sie können sich mit einer anderen Authentifizierungsmethode anmelden, für die Ihre Telefonnummer nicht erforderlich ist (etwa mit einer E-Mail-Adresse). Ist dies nicht möglich, können Sie das IT-Team Ihrer Organisation bitten, Ihre Einstellungen zu löschen. Um weitere Methoden zur Sicherheitsinformation hinzuzufügen, können Sie die Schritte im Abschnitt [Ändern der Information](#change-your-info) ausführen.|
|Meine Standardmethode ist falsch.|Sie können die Standardmethode in den Sicherheitsoptionen aktualisieren. Spezifische Details finden Sie im Abschnitt [Ändern der Information](#change-your-info).|
|Ich empfange keine SMS und keinen Anruf auf meinem mobilen Gerät.|Wenn Sie zuvor bereits erfolgreich SMS oder Telefonanrufe auf Ihrem mobilen Gerät erhalten haben, liegt das Problem wahrscheinlich beim Telefonanbieter, nicht bei Ihrem Konto. Stellen Sie sicher, dass der Empfang gut ist und Sie SMS und Telefonanrufe empfangen können. Bitten Sie einen Freund, Sie probehalber anzurufen oder Ihnen eine SMS zu schicken.<br><br>Wenn Sie SMS und Telefonanrufe empfangen können, die Benachrichtigung aber immer noch nicht erhalten haben, probieren Sie eine andere Methode aus. Sie können weitere Methoden zur Sicherheitsinformation hinzufügen, indem Sie die Schritte im Abschnitt [Ändern der Information](#change-your-info) ausführen. Wenn Sie über keine andere Methode zum Hinzufügen verfügen, bitten Sie den Support Ihres Unternehmens, Ihre Einstellungen zu löschen, sodass Sie Ihre Methoden bei der nächsten Anmeldung einrichten können.<br><br>Wenn häufig Probleme aufgrund eines schlechten Empfangs auftreten, empfiehlt sich die Verwendung der Microsoft Authenticator-App auf Ihrem mobilen Gerät. Die App kann zufällige Sicherheitscodes generieren, die Sie zum Anmelden verwenden. Diese Codes erfordern weder mobilen Empfang noch eine Internetverbindung. Weitere Informationen zur Microsoft Authenticator-App finden Sie im Artikel [Erste Schritte mit der Microsoft Authenticator-App](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).|
|Mit den Optionen in dieser Tabelle konnte mein Problem nicht gelöst werden.|Wenn Sie diese Schritte zur Problembehandlung ausgeführt haben, das Problem aber weiterhin besteht, wenden Sie sich an den Unternehmenssupport.|

## <a name="next-steps"></a>Nächste Schritte

- Unter [Übersicht über die Sicherheitsinformation (Vorschau)](user-help-security-info-overview.md) finden Sie weitere Informationen zur Sicherheitsinformation.

- Informationen zur zweistufigen Überprüfung finden Sie im Artikel [Two-step verification overview](user-help-two-step-verification-overview.md) (Übersicht über die zweistufige Überprüfung). 

- In den folgenden Artikeln finden Sie Anleitungen zum Einrichten Ihrer Geräte im Bereich „Sicherheitsinformation“:

    - [Einrichten der Sicherheitsinformation zur Verwendung einer Authentifikator-App](security-info-setup-auth-app.md)

    - [Einrichten der Sicherheitsinformation zur Verwendung von Telefonanrufen](security-info-setup-phone-number.md)

    - [Einrichten der Sicherheitsinformation zur Verwendung von SMS](security-info-setup-text-msg.md)

    - [Einrichten der Sicherheitsinformation zur Verwendung einer E-Mail-Adresse](security-info-setup-email.md)

    - [Einrichten der Sicherheitsinformation zur Verwendung von Sicherheitsfragen](security-info-setup-questions.md)

- Setzen Sie Ihr Kennwort zurück, wenn Sie es verloren oder vergessen haben. Verwenden Sie dazu das [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/), oder führen Sie die Schritte im Artikel [Reset your work or school password](user-help-reset-password.md) (Zurücksetzen des Kennworts eines Geschäfts-, Schul- oder Unikontos) aus.

- Der Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) enthält Tipps zur Problembehandlung bei Anmeldeproblemen.