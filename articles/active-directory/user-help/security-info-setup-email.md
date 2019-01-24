---
title: Einrichten der Sicherheitsinformation zur Verwendung einer E-Mail-Adresse – Azure Active Directory | Microsoft-Dokumentation
description: Richten Sie Ihre Sicherheitsinformation ein, um Ihre Identität mithilfe einer Geschäfts-, Schul- oder Uni-E-Mail-Adresse zu bestätigen.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: 33ac67e7ef7313fcf54e0507d8c3d83b1d42ca2d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54816142"
---
# <a name="set-up-security-info-to-use-email-preview"></a>Einrichten der Sicherheitsinformation zur Verwendung einer E-Mail-Adresse (Vorschauversion)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Zum Einrichten Ihrer Sicherheitsinformation müssen Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto anmelden und den Registrierungsprozess abschließen. Wenn Sie Ihre Sicherheitsinformation noch nie eingerichtet haben, werden Sie nun dazu aufgefordert.

## <a name="set-up-email"></a>Einrichten einer E-Mail-Adresse

Abhängig von den Einstellungen Ihrer Organisation werden Sie beim Anmelden möglicherweise aufgefordert, eine E-Mail-Adresse zu Ihrer Sicherheitsinformation hinzuzufügen. Führen Sie andernfalls zum Einrichten der E-Mail-Adresse in der Sicherheitsinformation die Schritte unter [Verwalten der Sicherheitsinformation](security-info-manage-settings.md) aus.

>[!Note]
>Es wird empfohlen, eine E-Mail-Konto zu verwenden, für das kein Netzwerkkennwort erforderlich ist.<br>Wird die E-Mail-Option nicht angezeigt, lässt Ihre Organisation unter Umständen die Nutzung einer E-Mail-Adresse für die Überprüfung nicht zu. In diesem Fall müssen Sie eine andere Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

### <a name="to-use-your-email-address"></a>So verwenden Sie Ihre E-Mail-Adresse

1. Wählen Sie die Option **E-Mail** aus, und geben Sie Ihre E-Mail-Adresse in das Feld ein. Verwenden Sie nicht die E-Mail-Adresse Ihres Geschäfts-, Schul- oder Unikontos.

     ![Seite „Sicherheitsinformation“ mit Eingabefeld für die E-Mail-Adresse](media/security-info/security-info-keep-secure-setup-email.png)

2. Sehen Sie nach, ob Ihre Organisation eine E-Mail von Microsoft erhalten hat, geben Sie den enthaltenen Prüfcode ins Feld **Bestätigen Sie Ihre E-Mail-Adresse** ein, und klicken Sie auf **Fertig**.

     ![Seite „Sicherheitsinformation“ mit dem Eingabefeld für den E-Mail-Prüfcode](media/security-info/security-info-verify-email.png)

    >[!Note]
    >Wenn Ihre Organisation keine E-Mail von Microsoft erhalten hat, vergewissern Sie sich, dass Sie die E-Mail-Adresse richtig eingegeben haben, und überprüfen Sie den Ordner für Junk-E-Mails bzw. den Spam-Ordner.

3. Klicken Sie auf der Seite **Keep your account secure** (Konto schützen) auf **Fertig**.

    Ihre Sicherheitsinformationen werden aktualisiert, sodass bei der Kennwortzurücksetzung Ihre E-Mail-Adresse zur Bestätigung Ihrer Identität verwendet wird.

## <a name="additional-security-info-options"></a>Zusätzliche Sicherheitsinformationsoptionen

Basierend auf der Aktion, die Sie ausführen möchten, stehen verschiedene Möglichkeiten zur Verfügung, wie Ihre Organisation sich mit Ihnen in Verbindung setzen kann, um Ihre Identität zu überprüfen. Die Optionen lauten:

- **Authentifikator-App:** Sie können eine Authentifikator-App herunterladen und verwenden, um entweder eine Genehmigungsbenachrichtigung oder einen nach dem Zufallsprinzip generierten Genehmigungscode für die zweistufige Überprüfung oder die Kennwortzurücksetzung zu erhalten. Ausführliche Anweisungen zum Einrichten und Verwenden der Microsoft Authenticator-App finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung einer Authentifikator-App](security-info-setup-auth-app.md).

- **SMS an ein mobiles Gerät:** Geben Sie die Nummer Ihres mobilen Geräts an, und Sie erhalten eine SMS mit dem Code, den Sie für die zweistufige Überprüfung oder die Kennwortzurücksetzung verwenden. Ausführliche Anweisungen dazu, wie Sie Ihre Identität mit einer Textnachricht (SMS) bestätigen, finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung von SMS](security-info-setup-text-msg.md).

- **Anruf bei einem mobilen Gerät oder einer geschäftlichen Telefonnummer:** Geben Sie die Nummer Ihres mobilen Geräts an, und erhalten Sie einen Telefonanruf für die zweistufige Überprüfung oder die Kennwortzurücksetzung. Ausführliche Anweisungen dazu, wie Sie Ihre Identität mit einer Telefonnummer bestätigen, finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung von Telefonanrufen](security-info-setup-phone-number.md).

- **Sicherheitsfragen:** Beantworten Sie einige Sicherheitsfragen, die von Ihrem Administrator für Ihre Organisation erstellt wurden. Diese Option ist nur für die Kennwortzurücksetzung verfügbar, nicht für die zweistufige Überprüfung. Ausführliche Anweisungen zum Einrichten der Sicherheitsfragen finden Sie im Artikel [Einrichten der Sicherheitsinformation zur Verwendung von Sicherheitsfragen](security-info-setup-questions.md).
    
    >[!Note]
    >Wenn einige dieser Optionen fehlen, sind die entsprechenden Methoden in Ihrer Organisation wahrscheinlich nicht zugelassen. In diesem Fall müssen Sie eine verfügbare Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie Ihre Sicherheitsinformation aktualisieren müssen, befolgen Sie die Anweisungen im Artikel [Verwalten der Sicherheitsinformationen (Vorschauversion)](security-info-manage-settings.md).

- Setzen Sie Ihr Kennwort zurück, wenn Sie es verloren oder vergessen haben. Verwenden Sie dazu das [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/), oder führen Sie die Schritte im Artikel [Reset your work or school password](user-help-reset-password.md) (Zurücksetzen des Kennworts eines Geschäfts-, Schul- oder Unikontos) aus.

- Der Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) enthält Tipps zur Problembehandlung bei Anmeldeproblemen.
