---
title: Einrichten der Sicherheitsinformationen zur Verwendung einer Authenticator-App – Azure Active Directory | Microsoft-Dokumentation
description: Richten Sie Ihre Sicherheitsinformationen ein, um Ihre Identität mithilfe der Microsoft Authenticator-App zu bestätigen.
services: active-directory
author: eross-msft
manager: mtillman
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: bd62ff03c3158743337ce958eb1bda7c7bd2a7f6
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39347844"
---
# <a name="set-up-security-info-to-use-an-authenticator-app-preview"></a>Einrichten der Sicherheitsinformationen zur Verwendung einer Authenticator-App (Vorschau)

[!INCLUDE[preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Zum Einrichten Ihrer Sicherheitsinformationen müssen Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto anmelden und den Registrierungsprozess abschließen. Wenn Sie Ihre Sicherheitsinformationen noch nie eingerichtet haben, werden Sie nun dazu aufgefordert.

## <a name="set-up-the-microsoft-authenticator-app"></a>Einrichten der Microsoft Authenticator-App

Abhängig von den Einstellungen Ihrer Organisation werden Sie bei der Anmeldung möglicherweise aufgefordert, die Microsoft Authenticator-App einzurichten. Führen Sie andernfalls die Schritte unter [Verwalten der Sicherheitsinformationen](security-info-manage-settings.md) aus, um mit dem Einrichten der Microsoft Authenticator-App in den Sicherheitsinformationen zu beginnen.

Informationen zum Herunterladen der Microsoft Authenticator-App finden Sie im Artikel [Erste Schritte mit der Microsoft Authenticator-App](microsoft-authenticator-app-how-to.md).

>[!Note]
>Wenn Sie die Microsoft Authenticator-App nicht verwenden können Sie während der Einrichtung eine andere App auswählen. In diesem Artikel wird die Microsoft Authenticator-App verwendet. Wird die Option für eine Authenticator-App nicht angezeigt, lässt Ihre Organisation unter Umständen die Nutzung einer Authenticator-App für die Überprüfung nicht zu. In diesem Fall müssen Sie eine andere Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

### <a name="to-use-the-microsoft-authenticator-app"></a>Verwenden der Microsoft Authenticator-App

1. Wählen Sie die Option **Authenticator-App** aus.

    Der Assistent **App herunterladen** wird geöffnet.

    ![Assistent „App herunterladen“, erster Bildschirm](media/security-info/security-info-auth-app-wizard.png)

    Wenn Sie die Microsoft Authenticator-App nicht verwenden möchten, können Sie im Bildschirm **App herunterladen** auf den Link **Ich möchte eine andere Authentifikator-App verwenden** klicken.

2. Nachdem Sie die Microsoft Authenticator-App installiert haben, wählen Sie **Weiter** aus.

    Wenn Sie dazu aufgefordert werden, lassen Sie Benachrichtigungen zu, fügen ein neues Konto hinzu und wählen dann **Geschäfts-, Schul- oder Unikonto** aus.

3. Klicken Sie auf **Weiter**.

    Der Bildschirm **QR-Code scanne** wird angezeigt.

    ![Scannen des QR-Codes mithilfe der Authenticator-App](media/security-info/security-info-scan-qr.png)

4. Öffnen Sie die Microsoft Authenticator-App, wählen Sie oben rechts das Symbol zum **Anpassen und Steuern** aus, und wählen Sie dann nacheinander **Konto hinzufügen** und **Geschäfts-, Schul- oder Unikonto** aus. 

5. Wenn Sie eine App zum Lesen von QR-Codes besitzen, scannen Sie den bereitgestellten Code. Wenn Sie nicht über eine solche App verfügen, können Sie den Link **Kann der QR-Code nicht gescannt werden?** auswählen und den Code und die URL manuell in die Microsoft Authenticator-App eingeben.

6. Verwenden Sie die Microsoft Authenticator-App, um die Benachrichtigung zum Aktivieren der App zu genehmigen.

    Ihre Sicherheitsinformationen werden aktualisiert, sodass bei der zweistufigen Überprüfung oder der Self-Service-Kennwortzurücksetzung die Microsoft Authenticator-App zur Überprüfung Ihrer Identität verwendet wird.

## <a name="additional-security-info-options"></a>Zusätzliche Sicherheitsinformationsoptionen

Je nachdem, welche Aktivität Sie ausführen möchten, stehen verschiedene Möglichkeiten zur Verfügung, wie Ihre Organisation sich mit Ihnen in Verbindung setzen kann, um Ihre Identität zu überprüfen. Die Optionen lauten:

- **SMS an ein mobiles Gerät**. Geben Sie die Nummer Ihres mobilen Geräts an, und Sie erhalten eine SMS mit dem Code, den Sie für die zweistufige Überprüfung oder die Kennwortzurücksetzung verwenden. Ausführliche Anweisungen dazu, wie Sie Ihre Identität mit einer SMS bestätigen, finden Sie unter [Einrichten der Sicherheitsinformationen zur Verwendung von SMS](security-info-setup-text-msg.md).

- **Anruf bei einem mobilen Gerät oder einer geschäftlichen Telefonnummer**. Geben Sie die Nummer Ihres mobilen Geräts an, und Sie erhalten einen Telefonanruf für die zweistufige Überprüfung oder die Kennwortzurücksetzung. Ausführliche Anweisungen dazu, wie Sie Ihre Identität mit einer Telefonnummer bestätigen, finden Sie unter [Einrichten der Sicherheitsinformationen zur Verwendung von Telefonanrufen](security-info-setup-phone-number.md).

- **E-Mail-Adresse**. Geben Sie die E-Mail-Adresse Ihres Geschäfts-, Schul- oder Unikontos an, um eine E-Mail für die Kennwortzurücksetzung zu erhalten. Diese Option ist für die zweistufige Überprüfung nicht verfügbar. Ausführliche Anweisungen zum Einrichten Ihrer E-Mail-Adresse finden Sie unter [Einrichten der Sicherheitsinformationen zur Verwendung einer E-Mail-Adresse](security-info-setup-email.md).

- **Sicherheitsfragen**. Beantworten Sie Sicherheitsfragen, die von Ihrem Administrator für Ihre Organisation erstellt wurden. Diese Option ist nur für die Kennwortzurücksetzung verfügbar, nicht für die zweistufige Überprüfung. Ausführliche Anweisungen zum Einrichten der Sicherheitsfragen finden Sie im Artikel [Einrichten der Sicherheitsinformationen zur Verwendung von Sicherheitsfragen](security-info-setup-questions.md).
    
    >[!Note]
    >Wenn einige dieser Optionen fehlen, sind die entsprechenden Methoden in Ihrer Organisation wahrscheinlich nicht zugelassen. In diesem Fall müssen Sie eine verfügbare Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie Ihre Sicherheitsinformationen aktualisieren müssen, befolgen Sie die Anweisungen im Artikel [Verwalten der Sicherheitsinformationen](security-info-manage-settings.md).

- Setzen Sie Ihr Kennwort zurück, wenn Sie es verloren oder vergessen haben. Verwenden Sie dazu das [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/), oder führen Sie die Schritte im Artikel [Zurücksetzen des Kennworts eines Geschäfts-, Schul- oder Unikontos](user-help-reset-password.md) aus.

- Informieren Sie sich im Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) über Tipps zur Behandlung von Anmeldeproblemen.