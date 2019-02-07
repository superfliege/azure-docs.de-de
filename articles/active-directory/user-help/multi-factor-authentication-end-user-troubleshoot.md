---
title: Problembehandlung bei der zweistufigen Überprüfung – Azure Active Directory | Microsoft-Dokumentation
description: Enthält Anweisungen für Benutzer zu den Schritten, die bei einem Problem mit Azure Multi-Factor Authentication und der zweistufigen Überprüfung auszuführen sind.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: lizross
ms.reviewer: kexia
ms.openlocfilehash: 2ede7a172eda1122b0668984cc8e11937b3ae852
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55768182"
---
# <a name="get-help-with-two-step-verification"></a>Hilfe bei der zweistufigen Überprüfung

Die zweistufige Überprüfung ist eine Sicherheitsfunktion, mit der Ihre Organisation Ihre Konten schützt. Die zweistufige Überprüfung ist sicherer als nur ein Kennwort, da sie auf zwei Formen der Authentifizierung basiert: etwas, das Sie wissen, und etwas, das Sie bei sich tragen. Das, was Sie wissen, ist Ihr Kennwort, und das, was Sie bei sich tragen, ist Ihr Smartphone oder ein Gerät. Mithilfe der zweistufigen Überprüfung Sie können auch dann verhindern, dass sich böswillige Hacker als Sie anmelden, wenn diese Ihr Kennwort kennen.

Microsoft bietet die zweistufige Überprüfung, aber Ihre Organisation entscheidet, ob Sie das Feature verwenden. Sie können das Feature nicht deaktivieren, wenn es von Ihrer Organisation vorausgesetzt wird. Auch die Verwendung eines Kennworts zum Schutz Ihres Kontos können Sie nicht deaktivieren.

>[!Note]
>Weitere Informationen zur Verwendung der zweistufigen Überprüfung mit Ihrem persönlichen Microsoft-Konto finden Sie im Artikel [Informationen zur zweistufigen Überprüfung](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="why-do-i-need-to-use-another-verification-method"></a>Warum muss ich eine andere Überprüfungsmethode verwenden?

Es gibt verschiedene Gründe, weshalb Sie möglicherweise eine andere Überprüfungsmethode mit Ihrem Konto verwenden müssen. Beispiel: 

- **Sie haben Ihr Smartphone oder Gerät vergessen.** An manchen Tagen lassen Sie Ihr Smartphone möglicherweise zu Hause, müssen sich aber dennoch bei der Arbeit anmelden. Zunächst sollten Sie versuchen, sich mit einer anderen Methode anzumelden, für die Ihr Smartphone nicht benötigt wird.

- **Sie haben Ihr Smartphone verloren oder haben eine neue Telefonnummer.** Wenn Sie Ihr Smartphone verloren oder eine neue Nummer erhalten haben, können Sie sich mit einer anderen Methode anmelden oder Ihren Administrator bitten, Ihre Einstellungen zu löschen. Es wird dringend empfohlen, dass Sie Ihren Administrator informieren, wenn Sie Ihr Smartphone verloren haben oder wenn es gestohlen wurde, damit Ihr Konto entsprechend aktualisiert werden kann. Nachdem Ihre Einstellungen gelöscht wurden, werden Sie bei der nächsten Anmeldung aufgefordert, sich für die [zweistufige Überprüfung zu registrieren](multi-factor-authentication-end-user-first-time.md).

- **Keinen Authentifizierungstext oder keinen Telefonanruf erhalten.** Es gibt verschiedene Gründe, warum Sie den Text oder den Telefonanruf möglicherweise nicht erhalten. Wenn Sie zuvor bereits erfolgreich SMS-Nachrichten oder Telefonanrufe erhalten haben, liegt das Problem möglicherweise beim Telefonanbieter, nicht bei Ihrem Konto. Wenn Sie häufig Verzögerungen aufgrund eines schlechten Empfangs bemerken, empfiehlt sich die Verwendung der [Microsoft Authenticator-App](user-help-auth-app-download-install.md) auf Ihrem mobilen Gerät. Diese App kann zufällige Sicherheitscodes für die Anmeldung, generieren, ohne ein Mobilfunksignal oder eine Internetverbindung vorauszusetzen.<br><br>Wenn Sie eine SMS erhalten möchten, bitten Sie einem Freund, Ihnen eine Test-SMS zusenden, um sicherzustellen, dass Sie SMS-Nachrichten empfangen können. Wenn Sie mehrere Nachrichten erhalten haben, müssen Sie den Code der neuesten verwenden.

- **Ihre App-Kennwörter funktionieren nicht** App-Kennwörter ersetzen normale Kennwörter für ältere Desktopanwendungen, die die zweistufige Überprüfung nicht unterstützen. Stellen Sie zunächst sicher, dass Sie das Kennwort korrekt eingegeben haben. Wenn das Problem dadurch nicht behoben wird, [erstellen Sie ein neues App-Kennwort](multi-factor-authentication-end-user-app-passwords.md), oder wenden Sie sich an Ihren Administrator, um [Ihre vorhandenen App-Kennwörter zu löschen](../authentication/howto-mfa-userdevicesettings.md), damit Sie ein neues erstellen können.

## <a name="sign-in-using-another-verification-method"></a>Anmelden mit einer anderen Überprüfungsmethode

1. Melden Sie sich wie gewohnt bei Ihrem Konto an, und klicken Sie auf der Seite **Zweistufige Überprüfung** auf den Link **Auf andere Weise anmelden**.

    ![Ändern der Überprüfungsmethode für die Anmeldung](./media/multi-factor-authentication-end-user-troubleshoot/two-factor-auth-signin-another-way.png)

    >[!Note]
    >Wenn Sie den Link **Sign in another way** (Andere Methode für die Anmeldung verwenden) nicht sehen, bedeutet dies, dass Sie keine anderen Überprüfungsmethoden eingerichtet haben. Wenden Sie sich an Ihren Administrator, damit er Ihnen bei der Anmeldung bei Ihrem Konto hilft. Fügen Sie nach der Anmeldung zusätzliche Überprüfungsmethoden hinzu. Weitere Informationen zum Hinzufügen von Überprüfungsmethoden finden Sie im Artikel [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md).<br><br>Wenn Sie zwar den Link, aber keine anderen Überprüfungsmethoden sehen, müssen Sie sich an Ihren Administrator wenden, damit dieser Ihnen bei der Anmeldung bei Ihrem Konto hilft.

2. Wählen Sie Ihre alternative Überprüfungsmethode aus, und fahren Sie mit dem zweistufigen Überprüfungsprozess fort.

3. Wenn Sie wieder zu Ihrem Konto zurückgekehrt sind, können Sie die Überprüfungsmethoden (bei Bedarf) aktualisieren. Weitere Informationen zum Hinzufügen oder Ändern von Methoden finden Sie im Artikel [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md).

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Ich konnte keine Lösung für mein Problem finden

Wenn Sie diese Schritte ausgeführt haben, aber weiterhin Problem auftreten, wenden Sie sich an Ihren Administrator, damit er Ihnen weiterhilft.

## <a name="related-topics"></a>Verwandte Themen

* [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md)

* [Microsoft Authenticator-App – häufig gestellte Fragen](user-help-auth-app-faq.md)
