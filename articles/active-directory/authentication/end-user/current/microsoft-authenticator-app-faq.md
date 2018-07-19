---
title: Hilfe für die Microsoft Authenticator-App – Azure AD | Microsoft-Dokumentation
description: Enthält eine Liste mit häufig gestellten Fragen und Antworten im Zusammenhang mit Microsoft Authenticator-App und Azure Multi-Factor Authentication (MFA).
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/08/2018
ms.author: lizross
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: 9027b09c186dfb7661fc63f200f4d2e5da96a70a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130100"
---
# <a name="microsoft-authenticator-app-faq"></a>Microsoft Authenticator-App – häufig gestellte Fragen

In diesem Artikel werden häufig gestellte Fragen zur Microsoft Authenticator-App beantwortet. Wenn Sie keine Antwort auf Ihre Frage finden, wechseln Sie zum [Forum zur Microsoft Authenticator-App](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Sie können sich auch einen weiteren Artikel mit häufig gestellten Fragen ([Anmelden per Smartphone anstelle Ihres Kennworts](microsoft-authenticator-app-phone-signin-faq.md)) ansehen.

Die Microsoft Authenticator-App ersetzt die Azure Authenticator-App und ist die empfohlene App, wenn Sie Azure Multi-Factor Authentication verwenden. Die Microsoft Authenticator-App ist für [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) und [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071) verfügbar.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="what-data-does-the-authenticator-store-on-my-behalf-and-how-can-i-delete-it"></a>Was speichert Authenticator in meinem Namen, und wie kann ich diese Daten löschen?

Microsoft Authenticator speichert die Kontoinformationen, die Sie beim Hinzufügen eines Kontos erstellen. Bei Verwendung von Authenticator wird ein Diagnoseprotokoll für Debugzwecke erstellt, und es werden nützliche Daten gespeichert, die bei der Microsoft-Diagnose als Hilfe bei unvorhersehbaren Problemen dienen. Sie können auf die Protokolldaten zugreifen, indem Sie **Hilfe** > **Protokolle senden** > **Protokolle anzeigen** öffnen.

Sie können die Daten löschen, indem Sie die Kontokachel löschen. Hierbei werden alle Kontoinformationen gelöscht, die von der Anwendung verwendet werden, einschließlich der Protokolle. 

Weitere Informationen zur Verwendung Ihrer Daten durch Microsoft finden Sie unter: https://servicetrust.microsoft.com/ViewPage/PrivacyGettingStarted.

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>Wozu dienen die Codes in der App? Warum wird die Zahl rückwärts gezählt?

Wenn Sie die Microsoft Authenticator-App öffnen, werden Ihnen die hinzugefügten Konten sowie jeweils eine sechs- oder achtstellige Zahl angezeigt. Möglicherweise sehen Sie einen 30-sekündigen Timer, der abläuft.

Diese Codes werden verwendet, wenn Sie sich bei Ihrem Konto anmelden. Nachdem Sie Ihren Benutzernamen und Ihr Kennwort eingegeben haben, werden Sie möglicherweise zur Eingabe eines Prüfcodes aufgefordert. Öffnen Sie die Microsoft Authenticator-App, und kopieren Sie den aktuell angezeigten Code. Geben Sie diesen Code auf der Anmeldeseite ein, um den Vorgang abzuschließen.

Die Codes ändern sich alle 30 Sekunden, damit Sie den gleichen Code nie zweimal verwenden. Es handelt sich nicht um ein Kennwort, das Sie sich merken sollen. Die Idee dabei ist, dass nur eine Person mit Zugriff auf Ihr Telefon Ihren Prüfcode kennen kann.

Für die Codes sind weder Internet noch Daten erforderlich, daher müssen Sie sich bei keinem Telefondienst anmelden. Wenn Sie die App schließen, wird sie nicht länger im Hintergrund ausgeführt, sodass auch der Akku nicht verbraucht wird. Sie können die App schließen und ignorieren, bis Sie sich das nächste Mal anmelden.  

### <a name="i-only-get-notifications-when-i-have-the-app-open-if-the-app-isnt-open-i-dont-get-any-notifications"></a>Ich erhalte Benachrichtigungen nur, wenn die App geöffnet ist. Wenn die App nicht geöffnet ist, erhalte ich keine Benachrichtigungen.

Wenn Sie Benachrichtigungen erhalten, aber kein Signalton ausgegeben wird und das Gerät nicht vibriert, obwohl es nicht stummgeschaltet ist, sollten Sie zunächst die App-Einstellungen überprüfen. Aktivieren Sie die Verwendung von Tönen und Vibration bei Benachrichtigungen für die App.

Wenn Sie überhaupt keine Benachrichtigungen erhalten, überprüfen Sie Folgendes:

- Ist Ihr Telefon im Nicht-stören-Modus oder stummgeschaltet? In diesem Modus können Apps am Senden von Benachrichtigungen gehindert werden.
- Erhalten Sie Benachrichtigungen von anderen Apps? Wenn dies nicht der Fall ist, liegt möglicherweise ein Problem mit den Netzwerkverbindungen auf Ihrem Mobiltelefon oder mit dem Benachrichtigungskanal von Android oder Apple vor. Im ersten Fall können Sie das Problem über die Einstellungen des Mobiltelefons beheben, im zweiten Fall müssen Sie sich jedoch möglicherweise mit Ihrem Mobilfunkanbieter in Verbindung setzen, um Unterstützung zu erhalten.
- Können Sie Benachrichtigungen nur für bestimmte Konten in der App erhalten? In diesem Fall entfernen Sie die Konten aus der App, die Probleme bereiten, und fügen sie erneut hinzu, um Pushbenachrichtigungen zu aktivieren.

Sie können uns Ihre Protokolle zur Diagnose senden, wenn Sie diese Vorschläge zur Problembehandlung befolgt haben, die Probleme jedoch weiterhin bestehen. Rufen Sie die App-Einstellungen auf. Wählen Sie anschließend **Hilfe und Feedback** und **Protokolle senden** aus. Wechseln Sie anschließend zum [Internetforum zur Microsoft Authenticator-App](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp), und informieren Sie uns über Ihr Problem und darüber, welche Schritte Sie bisher ausgeführt haben.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Ich verwende für Sicherheitscodes bereits die Microsoft Authenticator-App. Wie wechsle ich zu One-Click-Pushbenachrichtigungen?
Die Bestätigung einer Anmeldung mittels Pushbenachrichtigungen steht nur für persönliche Microsoft-Konten oder für Geschäfts-, Schul- oder Unikonten von Microsoft zur Verfügung, nicht für Konten von Drittanbietern wie Google oder Facebook. Wenn Sie über ein Geschäfts-, Schul- oder Unikonto von Microsoft verfügen, kann Ihre Organisation diese Option deaktivieren.

Wenn Sie ein Microsoft-Konto als persönliches Konto verwenden, und zu Pushbenachrichtigungen wechseln möchten, müssen Sie Ihr Konto wieder hinzufügen. Registrieren Sie das Gerät erneut mit Ihrem Konto, und richten Sie Pushbenachrichtigungen ein.  

Wenn Sie Microsoft Authenticator für Ihr Geschäfts- oder Schulkonto verwenden, entscheidet Ihre Organisation, ob sie One-Click-Benachrichtigungen zulässt.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Funktionieren One-Click-Pushbenachrichtigungen für Nicht-Microsoft-Konten?
Nein, Pushbenachrichtigungen funktionieren nur mit Microsoft-Konten und Azure Active Directory-Konten. Wenn Ihre Organisation oder Schule Azure AD-Konten verwendet, kann sie diese Funktion deaktivieren.  

### <a name="i-got-a-new-device-or-restored-my-device-from-a-backup-how-do-i-set-up-my-accounts-in-the-microsoft-authenticator-app-again"></a>Ich habe ein neues Gerät oder habe mein Gerät auf der Grundlage einer Sicherung wiederhergestellt. Wie richte ich meine Konten in der Microsoft Authenticator-App wieder ein?
Wenn Sie ein iOS-Gerät verwenden und die Option **Auto Backup** (Automatische Sicherung) aktiviert sowie eine Sicherung Ihrer Konten auf Ihrem alten Gerät erstellt haben, können Sie mithilfe dieser Sicherung Ihre Kontoanmeldeinformationen auf dem neuen Gerät wiederherstellen. Weitere Informationen finden Sie im Artikel [Sichern und Wiederherstellen von Kontoanmeldeinformationen mit der Microsoft Authenticator-App](../../../../multi-factor-authentication/end-user/microsoft-authenticator-app-backup-and-recovery.md). 

### <a name="i-lost-my-device-or-moved-on-to-a-new-device-how-do-i-make-sure-notifications-dont-continue-to-go-to-my-old-device"></a>Ich habe mein Gerät verloren oder verwende nun ein neues Gerät. Wie stelle ich sicher, dass Benachrichtigungen nicht mehr an mein altes Gerät gesendet werden?  
Wenn Sie die Microsoft Authenticator-App zu Ihrem neuen iOS-Gerät hinzufügen, wird die App nicht automatisch auf dem alten Gerät entfernt. Es reicht auch nicht aus, die App auf dem alten Gerät zu löschen. Sie müssen die App auf dem alten Gerät löschen und Microsoft oder Ihrer Organisation mitteilen, dass die Registrierung des alten Geräts in Ihrem Konto aufgehoben werden soll.
- **Entfernen der App auf einem Gerät mit einem persönlichen Microsoft-Konto:** Navigieren Sie auf der Seite [Kontosicherheit](https://account.microsoft.com/security)  zum Bereich für die zweistufige Überprüfung, und deaktivieren Sie die Überprüfung für Ihr altes Gerät.  
- **Entfernen der App auf einem Gerät mit einem Geschäfts-, Schul- oder Unikonto von Microsoft:** Navigieren Sie auf der Seite [MyApps](https://myapps.microsoft.com/) zum Bereich für die zweistufige Überprüfung bzw. zum benutzerdefinierten Portal Ihrer Organisation, und deaktivieren Sie die Überprüfung für Ihr altes Gerät. 



### <a name="how-do-i-remove-an-account-from-the-app"></a>Wie entferne ich ein Konto aus der App?
* iOS: Wischen Sie im Hauptbildschirm nach links zu einer Kontokachel. Klicken Sie auf **Löschen**.
* Windows Phone: Wählen Sie im Hauptbildschirm die Menütaste und anschließend **Konten bearbeiten** aus. Tippen Sie auf das **X** neben dem Kontonamen.
* Android: Wählen Sie im Hauptbildschirm die Menütaste und anschließend **Konten bearbeiten** aus. Tippen Sie auf das **X** neben dem Kontonamen.

Wenn Sie ein Gerät besitzen, das bei Ihrer Organisation registriert ist, müssen Sie möglicherweise einen weiteren Schritt befolgen, um Ihr Konto zu entfernen. Auf diesen Geräten wird die Microsoft Authenticator-App automatisch als Geräteadministrator registriert. Wenn Sie die App vollständig deinstallieren möchten, müssen Sie die Registrierung der App zuerst in den App-Einstellungen aufheben.

### <a name="why-does-the-app-request-so-many-permissions"></a>Warum fordert die App so viele Berechtigungen an?
Hier finden Sie eine vollständige Liste der Berechtigungen, die möglicherweise erforderlich sind, und Hinweise zu deren Verwendung in der App. Die jeweils angezeigten Berechtigungen hängen vom Typ des Telefons ab.

* **Kamera**: Wir verwenden Ihre Kamera, um QR-Codes zu scannen, wenn Sie ein Geschäfts-, Schul- oder Unikonto oder ein nicht von Microsoft stammendes Konto hinzufügen.
* **Kontakte und Telefon**: Wird verwendet, um den Vorgang zu vereinfachen, indem auf Ihrem Telefon nach bereits vorhandenen Konten gesucht wird, wenn Sie sich mit Ihrem persönlichen Microsoft-Konto anmelden.
* **SMS**: Hiermit wird sichergestellt, dass Ihre Telefonnummer mit der hinterlegten Nummer übereinstimmt. Bei der ersten Anmeldung mit Ihrem persönlichen Microsoft-Konto  erhalten Sie eine Textnachricht mit einem sechs- bis achtstelligen Prüfcode auf dem Telefon, auf dem Sie die App heruntergeladen haben. Sie werden nicht aufgefordert, diesen Code zu suchen und in der App einzugeben, sondern der Code wird in der Textnachricht für Sie gesucht.
* **Andere Apps in den Hintergrund rücken**: Die Benachrichtigung an Sie, mit der Ihre Identität überprüft wird, wird auch in allen anderen ausgeführten Apps angezeigt.
* **Daten aus dem Internet empfangen**: Diese Berechtigung ist für das Senden von Benachrichtigungen erforderlich.
* **Standby-Modus verhindern**: Wenn Sie Ihr Gerät bei Ihrer Organisation registrieren, kann Ihre Organisation diese Richtlinie auf Ihrem Telefon ändern.
* **Vibrationsfunktion steuern**: Sie können auswählen, ob Ihr Telefon vibrieren soll, wenn Sie eine Benachrichtigung erhalten, dass Sie Ihre Identität bestätigen müssen.
* **Verwendung der Fingerprint-Hardware**: Bei einigen Geschäfts- und Schulkonten wird eine zusätzliche PIN benötigt, wenn Sie Ihre Identität bestätigen. Sie können Ihren Fingerabdruck verwenden, um den Prozess zu vereinfachen, anstatt die PIN einzugeben.
* **Anzeigen von Netzwerkverbindungen**: Wenn Sie über ein Microsoft-Konto verfügen, benötigt die App eine Netzwerk-/Internetverbindung.
* **Lesen Ihres Speichers**: Diese Berechtigung ist nur notwendig, wenn Sie ein technisches Problem über die App-Einstellungen melden. Einige Informationen aus Ihrem Speicherkonto werden gesammelt, um das Problem zu diagnostizieren.
* **Vollständiger Netzwerkzugriff**: Diese Berechtigung ist erforderlich, um Benachrichtigungen senden zu können, mit denen Sie Ihre Identität bestätigen.
* **Bei Start automatisch starten**: Wenn Sie Ihr Telefon neu starten, stellt diese Berechtigung sicher, dass Sie weiterhin Benachrichtigungen erhalten, um Ihre Identität zu bestätigen.

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>Warum ermöglicht die Microsoft Authenticator-App die Genehmigung einer Anforderung ohne Entsperrung des Geräts?

Sie müssen Ihr Gerät nicht entsperren, um Überprüfungsanforderungen zu genehmigen. Das Einzige, was Sie nachweisen können müssen, ist, dass Sie jederzeit über Ihr Mobiltelefon erreichbar sind. Bei der zweistufigen Überprüfung werden zwei Dinge überprüft: etwas, das Sie wissen (das Kennwort), und etwas, das Sie haben (Ihr Smartphone; eingerichtet mit der Microsoft Authenticator-App und registriert als MFA-Nachweis.) Somit sind die Kriterien des zweiten Authentifizierungsschritts erfüllt, wenn Sie über das Smartphone verfügen und die Anforderung bestätigen.

### <a name="what-does-the-lock-icon-in-the-account-list-mean"></a>Was bedeutet das Schlosssymbol in der Kontoliste?

Das Schlosssymbol bedeutet, dass das Gerät in Azure AD und für das Konto registriert ist. Die Geräteregistrierung für iOS erfolgt während der Microsoft Intune-Registrierung.

## <a name="next-steps"></a>Nächste Schritte

### <a name="contact-us"></a>Kontakt
Falls Ihre Frage hier nicht beantwortet wurde, lassen Sie es uns wissen. Posten Sie Ihre Frage im [Forum der Microsoft Authenticator-App](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp), um Hilfe aus der Community zu erhalten, oder hinterlassen Sie einen Kommentar auf dieser Seite.


### <a name="related-topics"></a>Verwandte Themen
* [Zweistufige Überprüfung](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) für Microsoft-Konten
* [Probleme beim Einrichten der zweistufigen Überprüfung](multi-factor-authentication-end-user-troubleshoot.md) für Ihr Geschäfts-, Schul- oder Unikonto?
* [Verwenden von Microsoft Authenticator für die Anmeldung über Ihr Telefon](microsoft-authenticator-app-phone-signin-faq.md)
