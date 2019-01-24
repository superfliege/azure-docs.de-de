---
title: 'Einrichten der Sicherheitsinformation zur Verwendung von Sicherheitsfragen: Azure Active Directory | Microsoft-Dokumentation'
description: Richten Sie Ihre Sicherheitsinformation ein, um Ihre Identität mithilfe vordefinierter Sicherheitsfragen zu bestätigen.
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
ms.openlocfilehash: 0ee486c41193608887763b611ed2a879cb5e0ef4
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54814782"
---
# <a name="set-up-security-info-to-use-pre-defined-security-questions-preview"></a>Einrichten der Sicherheitsinformation zur Verwendung von Sicherheitsfragen (Vorschau)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Zum Einrichten Ihrer Sicherheitsinformation müssen Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto anmelden und den Registrierungsprozess abschließen. Wenn Sie Ihre Sicherheitsinformation noch nie eingerichtet haben, werden Sie nun dazu aufgefordert.

## <a name="set-up-security-questions"></a>Einrichten von Sicherheitsfragen

Abhängig von den Einstellungen Ihrer Organisation werden Sie beim Anmelden möglicherweise aufgefordert, Sicherheitsfragen zur Ihrer Sicherheitsinformation hinzuzufügen. Führen Sie andernfalls zum Einrichten von Sicherheitsfragen in der Sicherheitsinformation die Schritte unter [Verwalten der Sicherheitsinformation](security-info-manage-settings.md) aus.

Wenn Sie Sicherheitsfragen verwenden, wird empfohlen, diese in Verbindung mit einer anderen Methode zu verwenden. Sicherheitsfragen können weniger sicher als andere Methoden sein, da einige Personen unter Umständen die Antworten auf die Fragen einer anderen Person kennen.

>[!Note]
>Sicherheitsfragen werden privat und sicher in einem Benutzerobjekt im Verzeichnis gespeichert und können nur während der Registrierung von Ihnen beantwortet werden. Es gibt keine Möglichkeit für Ihren Administrator, Ihre Fragen oder Antworten zu lesen oder zu ändern.<br>Wird die Option für Sicherheitsfragen nicht angezeigt, lässt Ihre Organisation unter Umständen die Nutzung von Sicherheitsfragen für die Überprüfung nicht zu. In diesem Fall müssen Sie eine andere Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

### <a name="to-choose-and-answer-your-security-questions"></a>So wählen Sie Sicherheitsfragen aus und beantworten sie

1. Klicken Sie auf **Sicherheitsfragen**, und wählen Sie dann aus, welche Sicherheitsfragen Sie beantworten möchten. 

    Die Anzahl der auszuwählenden Sicherheitsfragen wird von Ihrem Administrator bestimmt.

    ![Seite „Sicherheitsinformation“, Auswählen der Sicherheitsfragen](media/security-info/security-info-keep-secure-setup-pick-questions.png)

2. Geben Sie Antworten für Ihre ausgewählten Fragen ein, und klicken Sie dann auf **Fertig**.

## <a name="additional-security-info-options"></a>Zusätzliche Sicherheitsinformationsoptionen

Basierend auf der Aktion, die Sie ausführen möchten, stehen verschiedene Möglichkeiten zur Verfügung, wie Ihre Organisation sich mit Ihnen in Verbindung setzen kann, um Ihre Identität zu überprüfen. Die Optionen lauten:

- **Authentifikator-App:** Sie können eine Authentifikator-App herunterladen und verwenden, um entweder eine Genehmigungsbenachrichtigung oder einen nach dem Zufallsprinzip generierten Genehmigungscode für die zweistufige Überprüfung oder die Kennwortzurücksetzung zu erhalten. Ausführliche Anweisungen zum Einrichten und Verwenden der Microsoft Authenticator-App finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung einer Authentifikator-App](security-info-setup-auth-app.md).

- **SMS an ein mobiles Gerät:** Geben Sie die Nummer Ihres mobilen Geräts an, und Sie erhalten eine SMS mit dem Code, den Sie für die zweistufige Überprüfung oder die Kennwortzurücksetzung verwenden. Ausführliche Anweisungen dazu, wie Sie Ihre Identität mit einer Textnachricht (SMS) bestätigen, finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung von SMS](security-info-setup-text-msg.md).

- **Anruf bei einem mobilen Gerät oder einer geschäftlichen Telefonnummer:** Geben Sie die Nummer Ihres mobilen Geräts an, und erhalten Sie einen Telefonanruf für die zweistufige Überprüfung oder die Kennwortzurücksetzung. Ausführliche Anweisungen dazu, wie Sie Ihre Identität mit einer Telefonnummer bestätigen, finden Sie unter [Einrichten der Sicherheitsinformationen zur Verwendung von Telefonanrufen](security-info-setup-phone-number.md).

- **E-Mail-Adresse**. Geben Sie Ihre Geschäfts-, Schul- oder Uni-E-Mail-Adresse an, um eine E-Mail für die Kennwortzurücksetzung zu erhalten. Diese Option ist für die zweistufige Überprüfung nicht verfügbar. Ausführliche Anweisungen zum Einrichten Ihrer E-Mail-Adresse finden Sie unter [Einrichten der Sicherheitsinformationen zur Verwendung einer E-Mail-Adresse](security-info-setup-email.md).
   
    >[!Note]
    >Wenn einige dieser Optionen fehlen, sind die entsprechenden Methoden in Ihrer Organisation wahrscheinlich nicht zugelassen. In diesem Fall müssen Sie eine verfügbare Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie Ihre Sicherheitsinformation aktualisieren müssen, befolgen Sie die Anweisungen im Artikel [Verwalten der Sicherheitsinformationen (Vorschauversion)](security-info-manage-settings.md).

- Setzen Sie Ihr Kennwort zurück, wenn Sie es verloren oder vergessen haben. Verwenden Sie dazu das [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/), oder führen Sie die Schritte im Artikel [Reset your work or school password](user-help-reset-password.md) (Zurücksetzen des Kennworts eines Geschäfts-, Schul- oder Unikontos) aus.

- Der Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) enthält Tipps zur Problembehandlung bei Anmeldeproblemen.