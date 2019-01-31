---
title: Einrichten der Sicherheitsinformationen zur Verwendung von SMS – Azure Active Directory | Microsoft-Dokumentation
description: Richten Sie Ihre Sicherheitsinformationen ein, um Ihre Identität mithilfe einer SMS zu bestätigen.
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
ms.openlocfilehash: 0f11b8b91f1dad208e8d14e21a20c316fe46074c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191420"
---
# <a name="set-up-security-info-to-use-text-messaging-preview"></a>Einrichten der Sicherheitsinformationen zur Verwendung von SMS (Vorschau)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Zum Einrichten Ihrer Sicherheitsinformationen müssen Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto anmelden und den Registrierungsprozess abschließen. Wenn Sie Ihre Sicherheitsinformation noch nie eingerichtet haben, werden Sie nun dazu aufgefordert.

## <a name="set-up-text-messaging"></a>Einrichten von SMS

Abhängig von den Einstellungen Ihrer Organisation werden Sie beim Anmelden möglicherweise aufgefordert, die SMS-Option zur Ihren Sicherheitsinformationen hinzuzufügen. Führen Sie andernfalls die Schritte unter [Verwalten der Sicherheitsinformationen](security-info-manage-settings.md) aus, um mit dem Einrichten der SMS-Option in den Sicherheitsinformationen zu beginnen.

Die SMS-Option gehört zur Telefonoption, daher erfolgt die Einrichtung auf die gleiche Weise wie bei einer Telefonnummer. Allerdings erhalten Sie einen Telefonanruf von Microsoft, sondern eine SMS. Wird die Telefonoption nicht angezeigt, lässt Ihre Organisation unter Umständen die Nutzung einer Telefonnummer für die Überprüfung nicht zu. In diesem Fall müssen Sie eine andere Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

### <a name="to-use-a-text-message"></a>Verwenden der SMS-Option

1. Wählen Sie die Option **Telefon** aus.

    Der Assistent **Telefon einrichten** wird geöffnet.

    ![Einrichten der Landesvorwahl und Telefonnummer](media/security-info/security-info-keep-secure-setup-text.png)

2. Wählen Sie aus der Dropdownliste den zutreffenden Eintrag für **Land oder Region** aus, geben Sie im Feld **Telefonnummer** Ihre Telefonnummer (einschließlich Vorwahl) ein, wählen Sie die Option **Code per SMS an mich senden** aus, und wählen Sie dann **Weiter**.

    Sie erhalten eine SMS mit einem Code, den Sie auf einer Überprüfungsseite eingeben müssen.

    ![Überprüfungsseite zum Eingeben des Codes aus der SMS](media/security-info/security-info-keep-secure-verify-text-msg.png)

    Ihre Sicherheitsinformationen werden aktualisiert, sodass bei der zweistufigen Überprüfung oder der Self-Service-Kennwortzurücksetzung eine SMS zur Überprüfung Ihrer Identität an Sie gesendet wird.

    >[!Note]
    >Wenn Sie statt einer SMS einen Anruf erhalten möchten, führen Sie die Schritte im Artikel [Einrichten der Sicherheitsinformationen zur Verwendung von Telefonanrufen](security-info-setup-phone-number.md) aus.

## <a name="additional-security-info-options"></a>Zusätzliche Sicherheitsinformationsoptionen

Basierend auf der Aktion, die Sie ausführen möchten, stehen verschiedene Möglichkeiten zur Verfügung, wie Ihre Organisation sich mit Ihnen in Verbindung setzen kann, um Ihre Identität zu überprüfen. Die Optionen lauten:

- **Authentifikator-App:** Sie können eine Authentifikator-App herunterladen und verwenden, um entweder eine Genehmigungsbenachrichtigung oder einen nach dem Zufallsprinzip generierten Genehmigungscode für die zweistufige Überprüfung oder die Kennwortzurücksetzung zu erhalten. Eine Schrittanleitung zum Einrichten und Verwenden der Microsoft Authenticator-App finden Sie unter [Einrichten der Sicherheitsinformationen zur Verwendung einer Authenticator-App](security-info-setup-auth-app.md).

- **Anruf bei einem mobilen Gerät oder einer geschäftlichen Telefonnummer**. Geben Sie die Nummer Ihres mobilen Geräts an, und erhalten Sie einen Telefonanruf für die zweistufige Überprüfung oder die Kennwortzurücksetzung. Ausführliche Anweisungen dazu, wie Sie Ihre Identität mit einer Telefonnummer bestätigen, finden Sie unter [Einrichten der Sicherheitsinformationen zur Verwendung von Telefonanrufen](security-info-setup-phone-number.md).

- **E-Mail-Adresse**. Geben Sie Ihre Geschäfts-, Schul- oder Uni-E-Mail-Adresse an, um eine E-Mail für die Kennwortzurücksetzung zu erhalten. Diese Option ist für die zweistufige Überprüfung nicht verfügbar. Ausführliche Anweisungen zum Einrichten Ihrer E-Mail-Adresse finden Sie unter [Einrichten der Sicherheitsinformationen zur Verwendung einer E-Mail-Adresse](security-info-setup-email.md).
   
    >[!Note]
    >Wenn einige dieser Optionen fehlen, sind die entsprechenden Methoden in Ihrer Organisation wahrscheinlich nicht zugelassen. In diesem Fall müssen Sie eine verfügbare Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

- **Sicherheitsfragen**. Beantworten Sie einige Sicherheitsfragen, die von Ihrem Administrator für Ihre Organisation erstellt wurden. Diese Option ist nur für die Kennwortzurücksetzung verfügbar, nicht für die zweistufige Überprüfung. Ausführliche Anweisungen zum Einrichten der Sicherheitsfragen finden Sie im Artikel [Einrichten der Sicherheitsinformation zur Verwendung von Sicherheitsfragen](security-info-setup-questions.md).

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie Ihre Sicherheitsinformation aktualisieren müssen, befolgen Sie die Anweisungen im Artikel [Verwalten der Sicherheitsinformationen (Vorschauversion)](security-info-manage-settings.md).

- Setzen Sie Ihr Kennwort zurück, wenn Sie es verloren oder vergessen haben. Verwenden Sie dazu das [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/), oder führen Sie die Schritte im Artikel [Reset your work or school password](user-help-reset-password.md) (Zurücksetzen des Kennworts eines Geschäfts-, Schul- oder Unikontos) aus.

- Der Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) enthält Tipps zur Problembehandlung bei Anmeldeproblemen.
