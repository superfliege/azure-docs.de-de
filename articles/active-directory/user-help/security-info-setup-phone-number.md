---
title: Einrichten der Sicherheitsinformationen zur Verwendung von Telefonanrufen – Azure Active Directory | Microsoft-Dokumentation
description: Richten Sie Ihre Sicherheitsinformation ein, um Ihre Identität mithilfe eines mobilen Geräts oder einer geschäftlichen Telefonnummer zu bestätigen.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5c8daa50886c583f26a2d931854d7132eb2ece4b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56177205"
---
# <a name="set-up-security-info-to-use-phone-calls-preview"></a>Einrichten der Sicherheitsinformation zur Verwendung von Telefonanrufen (Vorschauversion)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Zum Einrichten Ihrer Sicherheitsinformation müssen Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto anmelden und den Registrierungsprozess abschließen. Wenn Sie Ihre Sicherheitsinformation noch nie eingerichtet haben, werden Sie nun dazu aufgefordert.

## <a name="set-up-phone-calls"></a>Einrichten von Telefonanrufen

Abhängig von den Einstellungen Ihrer Organisation werden Sie beim Anmelden möglicherweise aufgefordert, eine Telefonnummer zur Ihrer Sicherheitsinformation hinzuzufügen. Führen Sie andernfalls zum Einrichten von Telefonanrufen in der Sicherheitsinformation die Schritte unter [Verwalten der Sicherheitsinformation](security-info-manage-settings.md) aus.

>[!Note]
>Für die Sicherheitsinformation werden Nebenstellen nicht unterstützt. Selbst bei der Angabe im richtigen Format (+1 4255551234X12345) werden Nebenstellen vor dem Anruf entfernt.<br>Wird die Telefonoption nicht angezeigt, lässt Ihre Organisation unter Umständen die Nutzung von Telefonanrufen für die Überprüfung nicht zu. In diesem Fall müssen Sie eine andere Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

### <a name="to-use-your-phone-number"></a>So verwenden Sie eine Telefonnummer

1. Wählen Sie die Option **Telefon** aus.

    Der Assistent **Telefon einrichten** wird geöffnet.

    ![Einrichten der Ländervorwahl und der Telefonnummer](media/security-info/security-info-keep-secure-setup-phone.png)

2. Wählen Sie im Dropdownfeld den zutreffenden Eintrag für **Land oder Region** aus, geben Sie im Feld **Telefonnummer** Ihre Telefonnummer (einschließlich Vorwahl) ein, und wählen Sie die Option **Mich anrufen** aus. Klicken Sie anschließend auf **Weiter**.

    Sie erhalten einen Telefonanruf, um sicherzustellen, dass Sie die richtige Telefonnummer eingegeben haben. Sie werden zum Drücken der Rautetaste (#) aufgefordert, um die Einrichtung zu bestätigen und abzuschließen.

    ![Bildschirm zum Bestätigen der Telefonnummer: Der Anruf wurde beantwortet.](media/security-info/security-info-keep-secure-verify-phone-call.png)

    Ihre Sicherheitsinformation wird aktualisiert, sodass bei der zweistufigen Überprüfung oder der Self-Service-Kennwortzurücksetzung Ihre Telefonnummer zur Bestätigung Ihrer Identität verwendet wird.

    >[!Note]
    >Wenn Sie statt eines Anrufs eine SMS auf Ihrem mobilen Gerät erhalten möchten, führen Sie die Schritte im Artikel [Einrichten der Sicherheitsinformation zur Verwendung von SMS](security-info-setup-text-msg.md) aus.

## <a name="additional-security-info-options"></a>Zusätzliche Sicherheitsinformationsoptionen

Basierend auf der Aktion, die Sie ausführen möchten, stehen zusätzliche Möglichkeiten zur Verfügung, wie Ihre Organisation sich mit Ihnen in Verbindung setzen kann, um Ihre Identität zu überprüfen. Die Optionen lauten:

- **Authentifikator-App:** Sie können eine Authentifikator-App herunterladen und verwenden, um entweder eine Genehmigungsbenachrichtigung oder einen nach dem Zufallsprinzip generierten Genehmigungscode für die zweistufige Überprüfung oder die Kennwortzurücksetzung zu erhalten. Ausführliche Anweisungen zum Einrichten und Verwenden der Microsoft Authenticator-App finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung einer Authentifikator-App](security-info-setup-auth-app.md).

- **SMS an ein mobiles Gerät:** Geben Sie die Nummer Ihres mobilen Geräts an, und Sie erhalten eine SMS mit dem Code, den Sie für die zweistufige Überprüfung oder die Kennwortzurücksetzung verwenden. Ausführliche Anweisungen dazu, wie Sie Ihre Identität mit einer Textnachricht (SMS) bestätigen, finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung von SMS](security-info-setup-text-msg.md).

- **E-Mail-Adresse:** Geben Sie Ihre Geschäfts-, Schul- oder Uni-E-Mail-Adresse an, um eine E-Mail für die Kennwortzurücksetzung zu erhalten. Diese Option ist für die zweistufige Überprüfung nicht verfügbar. Ausführliche Anweisungen zum Einrichten Ihrer E-Mail-Adresse finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung einer E-Mail-Adresse](security-info-setup-email.md).

- **Sicherheitsfragen:** Beantworten Sie einige Sicherheitsfragen, die von Ihrem Administrator für Ihre Organisation erstellt wurden. Diese Option ist nur für die Kennwortzurücksetzung verfügbar, nicht für die zweistufige Überprüfung. Ausführliche Anweisungen zum Einrichten der Sicherheitsfragen finden Sie im Artikel [Einrichten der Sicherheitsinformation zur Verwendung von Sicherheitsfragen](security-info-setup-questions.md).
    
    >[!Note]
    >Wenn einige dieser Optionen fehlen, sind die entsprechenden Methoden in Ihrer Organisation wahrscheinlich nicht zugelassen. In diesem Fall müssen Sie eine verfügbare Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie Ihre Sicherheitsinformation aktualisieren müssen, befolgen Sie die Anweisungen im Artikel [Verwalten der Sicherheitsinformationen (Vorschauversion)](security-info-manage-settings.md).

- Setzen Sie Ihr Kennwort zurück, wenn Sie es verloren oder vergessen haben. Verwenden Sie dazu das [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/), oder führen Sie die Schritte im Artikel [Reset your work or school password](user-help-reset-password.md) (Zurücksetzen des Kennworts eines Geschäfts-, Schul- oder Unikontos) aus.

- Der Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) enthält Tipps zur Problembehandlung bei Anmeldeproblemen.
