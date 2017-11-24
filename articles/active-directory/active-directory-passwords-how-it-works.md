---
title: Funktionsweise von Azure AD SSPR | Microsoft-Dokumentation
description: "Ausführliche Informationen zur Self-Service-Kennwortzurücksetzung in Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 618c5908-5bf6-4f0d-bf88-5168dfb28a88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 56ddd5742b63851b9477bae0705ebd24e30ff185
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Ausführliche Informationen zur Self-Service-Kennwortzurücksetzung in Azure AD

Wie funktioniert die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR)? Was bedeutet diese Option in der Benutzeroberfläche? Im weiteren Verlauf erfahren Sie mehr über SSRP in Azure Active Directory (Azure AD).

## <a name="how-does-the-password-reset-portal-work"></a>Wie funktioniert das Portal für die Kennwortzurücksetzung?

Wenn ein Benutzer zum Kennwortzurücksetzungsportal navigiert, wird ein Workflow gestartet, um Folgendes zu bestimmen:

   * Wie soll die Seite lokalisiert werden?
   * Ist das Benutzerkonto gültig?
   * Zu welcher Organisation gehört der Benutzer?
   * Wo wird das Kennwort des Benutzers verwaltet?
   * Ist der Benutzer zur Verwendung des Features lizenziert?

In den folgenden Schritten wird beschrieben, welche Logik hinter der Seite zur Kennwortzurücksetzung steckt:

1. Der Benutzer klickt auf den Link **Sie können nicht auf Ihr Konto zugreifen?** oder wechselt direkt zu [https://aka.ms/sspr](https://passwordreset.microsoftonline.com).
   * Die Benutzeroberfläche wird basierend auf dem Browsergebietsschema in der entsprechenden Sprache wiedergegeben. Die Benutzeroberfläche für das Zurücksetzen des Kennworts wird in alle Sprachen lokalisiert, die Office 365 unterstützt.
2. Der Benutzer gibt eine Benutzer-ID ein und durchläuft erfolgreich die Captchaprüfung.
3. Azure AD prüft folgendermaßen, ob der Benutzer diese Funktion verwenden darf:
   * Es wird geprüft, ob die Funktion für diesen Benutzer aktiviert ist und ob eine Azure AD-Lizenz zugewiesen ist.
     * Wenn diese Funktion für den Benutzer nicht aktiviert ist oder keine Lizenz vorliegt, wird der Benutzer aufgefordert, sich zum Zurücksetzen des Kennworts an den Administrator zu wenden.
   * Es wird überprüft, ob der Benutzer in seinem Konto Daten für die Überprüfung in mehreren Schritten definiert hat, die der Administratorrichtlinie entsprechen.
     * Wenn die Richtlinie nur eine Überprüfung in einem Schritt vorsieht, wird sichergestellt, dass der Benutzer für mindestens eine der durch die Administratorrichtlinie aktivierten Überprüfungen geeignete Daten definiert hat.
       * Sind keine entsprechenden Daten konfiguriert, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
     * Wenn die Richtlinie eine Überprüfung in zwei Schritten vorsieht, wird sichergestellt, dass der Benutzer für mindestens zwei der durch die Administratorrichtlinie aktivierten Überprüfungen geeignete Daten definiert hat.
       * Sind keine entsprechenden Daten konfiguriert, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
   * Es wird überprüft, ob das Benutzerkennwort lokal verwaltet wird oder nicht (im Verbund oder mit Kennworthashsynchronisierung).
     * Wenn das Rückschreiben von Kennwörtern konfiguriert ist und das Benutzerkennwort lokal verwaltet wird, kann der Benutzer mit der Authentifizierung fortfahren und sein Kennwort zurücksetzen.
     * Wenn das Rückschreiben von Kennwörtern nicht konfiguriert ist und das Benutzerkennwort lokal verwaltet wird, wird der Benutzer aufgefordert, sich zum Zurückzusetzen des Kennworts an den Administrator zu wenden.
4. Wenn festgestellt wird, dass der Benutzer sein Kennwort zurücksetzen darf, wird er durch den Vorgang für die Kennwortzurücksetzung geleitet.

## <a name="authentication-methods"></a>Authentifizierungsmethoden

Wenn SSPR aktiviert ist, müssen Sie mindestens eine der folgenden Optionen als Authentifizierungsmethode auswählen. Manchmal werden diese Optionen als Gates bezeichnet. Für mehr Flexibilität für Ihre Benutzer wird dringend empfohlen, mindestens zwei Authentifizierungsmethoden auszuwählen.

* E-Mail
* Mobiltelefon
* Bürotelefon
* Sicherheitsfragen

![Authentifizierung][Authentication]

### <a name="what-fields-are-used-in-the-directory-for-the-authentication-data"></a>Welche Felder werden im Verzeichnis für die Authentifizierungsdaten verwendet?

* **Bürotelefon** entspricht der geschäftlichen Telefonnummer.
    * Benutzer können dieses Feld nicht selbst festlegen. Es muss von einem Administrator definiert werden.
* **Mobiltelefon** entspricht entweder dem Authentifizierungstelefon (nicht öffentlich sichtbar) oder dem Mobiltelefon (öffentlich sichtbar).
    * Der Dienst sucht zuerst nach dem Authentifizierungstelefon und verwendet dann das Mobiltelefon, wenn das Authentifizierungstelefon nicht vorhanden ist.
* **Alternative E-Mail-Adresse** entspricht entweder der E-Mail für Authentifizierung (nicht öffentlich sichtbar) oder der alternativen E-Mail-Adresse.
    * Der Dienst sucht zuerst nach der E-Mail für die Authentifizierung und greift dann auf die alternative E-Mail-Adresse zurück.

Standardmäßig werden nur die Cloudattribute „Bürotelefon“ und „Mobiltelefon“ mit Ihrem Cloudverzeichnis aus dem lokalen Verzeichnis für Authentifizierungsdaten synchronisiert.

Benutzer können ihr Kennwort nur dann zurücksetzen, wenn für sie Daten in den Authentifizierungsmethoden vorliegen, die der Administrator aktiviert und als erforderlich festgelegt hat.

Wenn ein Benutzer nicht möchte, dass seine Mobiltelefonnummer im Verzeichnis sichtbar ist, sie aber dennoch für die Kennwortzurücksetzung verwenden möchte, sollten Administratoren die Daten nicht in das Verzeichnis übernehmen. Der Benutzer sollte sein **Authentifizierungstelefon**-Attribut dann über das [Registrierungsportal für die Kennwortzurücksetzung](http://aka.ms/ssprsetup) auffüllen. Administratoren können diese Informationen im Profil des Benutzers anzeigen, sie werden jedoch nicht an anderer Stelle veröffentlicht.

### <a name="the-number-of-authentication-methods-required"></a>Anzahl erforderlicher Authentifizierungsmethoden

Diese Option bestimmt die Mindestanzahl der verfügbaren Authentifizierungsmethoden oder Gates, die ein Benutzer zum Zurücksetzen oder Entsperren des Kennworts durchlaufen muss. Sie kann auf ein oder zwei festgelegt werden.

Benutzer können wählen, weitere Authentifizierungsmethoden bereitzustellen, wenn diese Authentifizierungsmethode vom Administrator aktiviert wird.

Sind für einen Benutzer nicht die mindestens erforderlichen Methoden registriert, wird eine Fehlerseite angezeigt, die ihn auffordert, einen Administrator um die Kennwortzurücksetzung zu bitten.

#### <a name="change-authentication-methods"></a>Ändern der Authentifizierungsmethoden

Was passiert, wenn Sie mit einer Richtlinie beginnen, bei der zum Zurücksetzen oder Entsperren der Registrierung nur eine einzelne Authentifizierungsmethode erforderlich ist, und Sie dies in zwei Authentifizierungsmethoden ändern?

| Anzahl der registrierten Methoden | Anzahl der erforderlichen Methoden | Ergebnis |
| :---: | :---: | :---: |
| Mindestens 1 | 1 | **Kann** zurücksetzen oder entsperren |
| 1 | 2 | **Kann nicht** zurücksetzen oder entsperren |
| 2 oder mehr | 2 | **Kann** zurücksetzen oder entsperren |

Wenn Sie die den Benutzern zur Verfügung stehenden Arten von Authentifizierungsmethoden ändern, können Benutzer unter Umständen SSPR nicht mehr verwenden, wenn ihnen nicht genügend Daten zur Verfügung stehen.

Beispiel: 
1. Die ursprüngliche Richtlinie wird mit zwei erforderlichen Authentifizierungsmethoden konfiguriert. Sie verwendet nur die Bürotelefonnummer und die Sicherheitsfragen. 
2. Der Administrator ändert die Richtlinie so, dass anstelle von Sicherheitsfragen ein Mobiltelefon und eine alternative E-Mail-Adresse verwendet werden.
3. Benutzer, für die die Felder für Mobiltelefon und alternative E-Mail-Adresse nicht aufgefüllt werden, können ihre Kennwörter nicht zurücksetzen.

### <a name="how-secure-are-my-security-questions"></a>Wie sicher sind meine Sicherheitsfragen?

Wenn Sie Sicherheitsfragen verwenden, wird empfohlen, diese in Verbindung mit einer anderen Methode zu verwenden. Sicherheitsfragen können weniger sicher als andere Methoden sein, da einige Personen unter Umständen die Antworten auf die Fragen eines anderen Benutzers kennen.

> [!NOTE] 
> Sicherheitsfragen werden privat und sicher in einem Benutzerobjekt im Verzeichnis gespeichert und können nur während der Registrierung von Benutzern beantwortet werden. Es gibt keine Möglichkeit für einen Administrator, die Fragen oder Antworten eines Benutzers zu lesen oder zu ändern.
>

### <a name="security-question-localization"></a>Lokalisierung der Sicherheitsfragen

Sämtliche folgenden vordefinierten Fragen werden in alle Office 365-Sprachen lokalisiert und basieren auf dem Gebietsschema des Browsers des Benutzers:

* In welcher Stadt haben Sie Ihren ersten Partner bzw. Ihre erste Partnerin kennengelernt?
* In welcher Stadt haben sich Ihre Eltern kennengelernt?
* In welcher Stadt lebt Ihr Bruder/Ihre Schwester?
* Wie heißt die Geburtsstadt Ihres Vaters?
* In welcher Stadt haben Sie Ihre erste Arbeitsstelle angetreten?
* Wie heißt die Geburtsstadt Ihrer Mutter?
* In welcher Stadt haben Sie den Neujahrstag 2000 verbracht?
* Wie hieß Ihr Lieblingslehrer bzw. Ihre Lieblingslehrerin in der Grundschule mit Nachnamen?
* Bei welcher Universität haben Sie sich beworben, dort jedoch nicht studiert?
* An welchem Ort fand Ihre Hochzeitsfeier statt?
* Wie lautet der 2. Vorname Ihres Vaters?
* Was ist Ihr Lieblingsgericht?
* Wie lautet der Vor- und Nachname Ihrer Großmutter mütterlicherseits?
* Wie lautet der 2. Vorname Ihrer Mutter?
* In welchem Monat und Jahr ist Ihre älteste Schwester bzw. Ihr ältester Bruder geboren? (z.B. November 1985)
* Wie lautet der 2. Vorname Ihrer ältesten Schwester bzw. Ihres ältesten Bruders?
* Wie lautet der Vor- und Nachname Ihres Großvaters väterlicherseits?
* Wie lautet der 2. Vorname Ihrer jüngsten Schwester bzw. Ihres jüngsten Bruders?
* Auf welche Schule sind Sie in der sechsten Klasse gegangen?
* Wie lautet der Vor- und Nachname Ihres besten Freundes bzw. Ihrer besten Freundin in Ihrer Kindheit?
* Wie lautet der Vor- und Nachname Ihres ersten Partners bzw. Ihrer ersten Partnerin?
* Wie lautete der Nachname des Lehrers bzw. der Lehrerin Ihres Lieblingsfachs?
* Welche Marke und welches Modell hatte Ihr erstes Auto oder Motorrad?
* Wie heißt die erste Schule, die Sie besucht haben?
* In welchem Krankenhaus wurden Sie geboren?
* In welcher Straße haben Sie als Kind zuerst gewohnt?
* Wer war der Held Ihrer Kindheit?
* Wie hieß Ihr Lieblingskuscheltier?
* Wie hieß Ihr erstes Haustier?
* Welchen Spitznamen hatten Sie als Kind?
* Was war in der Schulzeit Ihre Lieblingssportart?
* Was war Ihr erster Job?
* Wie lauten die letzten vier Ziffern der Telefonnummer in Ihrer Kindheit?
* Was wollten Sie als Kind später einmal werden?
* Welches ist die berühmteste Person, die Sie kennen gelernt haben?

### <a name="custom-security-questions"></a>Benutzerdefinierte Sicherheitsfragen

Benutzerdefinierte Sicherheitsfragen sind nicht für verschiedene Gebietsschemas lokalisiert. Alle benutzerdefinierten Fragen werden auch bei abweichendem Gebietsschema im Browser in der Sprache angezeigt, in der sie in der administrativen Benutzeroberfläche eingegeben wurden. Wenn Sie lokalisierte Fragen benötigen, verwenden Sie die vordefinierten Fragen.

Die Länge von benutzerdefinierten Sicherheitsfragen ist auf 200 Zeichen begrenzt.

### <a name="security-question-requirements"></a>Anforderungen an Sicherheitsfragen

* Die Mindestanzahl von Zeichen für Antworten ist drei Zeichen.
* Die maximale Zeichenanzahl für Antworten ist 40 Zeichen.
* Benutzer können dieselbe Frage nicht mehrmals beantworten.
* Benutzer können nicht dieselbe Antwort für mehrere Fragen angeben.
* Bei der Definition von Fragen und Antworten kann ein beliebiger Zeichensatz verwendet werden (einschließlich Unicode).
* Die Anzahl der definierten Fragen muss größer als oder gleich der Anzahl der für die Registrierung erforderlichen Fragen sein.

## <a name="registration"></a>Registrierung

### <a name="require-users-to-register-when-they-sign-in"></a>Erzwingen der Registrierung für Benutzer bei der Anmeldung

Zum Aktivieren dieser Option müssen Benutzer, für die die Kennwortzurücksetzung aktiviert ist, die Registrierung für die Kennwortzurücksetzung abschließen, wenn sie sich mit Azure AD bei Anwendungen anmelden. Dies umfasst Folgendes:

* Office 365
* Azure-Portal
* Anpassung des Zugriffsbereichs
* Verbundanwendungen
* Benutzerdefinierte Anwendungen unter Verwendung von Azure AD

Wenn die Erzwingung der Registrierung deaktiviert ist, können Benutzer ihre Kontaktinformationen dennoch manuell registrieren. Sie können entweder [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) aufrufen oder im Zugriffsbereich auf der Registerkarte **Profil** auf den Link **Für das Zurücksetzen des Kennworts registrieren** klicken.

> [!NOTE]
> Benutzer können das Registrierungsportal für die Kennwortzurücksetzung durch Klicken auf **Abbrechen** oder durch Schließen des Fensters schließen. Sie werden aber bis zum Abschluss der Registrierung bei jeder Anmeldung erneut zur Registrierung aufgefordert.
>
> Dadurch wird die Verbindung des Benutzers nicht unterbrochen, wenn er bereits angemeldet ist.

### <a name="set-the-number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Festlegen der Anzahl von Tagen, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen

Diese Option bestimmt den Zeitraum zwischen dem Festlegen und dem erneuten Bestätigen der Authentifizierungsinformationen und ist nur verfügbar, wenn die Option **Registrierung von Benutzern bei der Anmeldung verlangen** aktiviert ist.

Gültige Werte sind 0 bis 730 Tage, wobei „0“ bedeutet, dass Benutzer nie zum erneuten Bestätigen ihrer Authentifizierungsinformationen aufgefordert werden.

## <a name="notifications"></a>Benachrichtigungen

### <a name="notify-users-on-password-resets"></a>Benutzer über Kennwortzurücksetzungen benachrichtigen?

Ist diese Option auf **Ja** festgelegt, erhält der Benutzer, der sein Kennwort zurücksetzt, eine E-Mail, in der er darüber informiert wird, dass sein Kennwort geändert wurde. Die E-Mail wird über das SSPR-Portal an die in Azure AD hinterlegte primäre und alternative E-Mail-Adresse gesendet. Niemand sonst wird über das Zurücksetzen informiert.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Sollen alle Administratoren benachrichtigt werden, wenn andere Administratoren ihr Kennwort zurücksetzen?

Ist diese Option auf **Ja** festgelegt, erhalten *alle Administratoren* eine E-Mail an ihre in Azure AD erfasste primäre E-Mail-Adresse. In dieser E-Mail werden sie darüber informiert, dass ein anderer Administrator ihr Kennwort mithilfe von SSPR geändert hat.

Beispiel: Es gibt vier Administratoren in einer Umgebung. Administrator A setzt das Kennwort mithilfe von SSPR zurück. Administratoren B, C und D erhalten eine E-Mail-Nachricht, in der sie über die Kennwortzurücksetzung informiert werden.

## <a name="on-premises-integration"></a>Lokale Integration

Wenn Sie Azure AD Connect installieren, konfigurieren und aktivieren, stehen folgende zusätzliche Optionen für lokale Integrationen zur Verfügung. Wenn diese Optionen abgeblendet sind, wurde das Rückschreiben nicht ordnungsgemäß konfiguriert. Weitere Informationen finden Sie unter [Konfigurieren des Kennwortrückschreibens](active-directory-passwords-writeback.md#configuring-password-writeback).

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Kennwörter in Ihr lokales Verzeichnis zurückschreiben?

Dieses Steuerelement bestimmt, ob das Rückschreiben von Kennwörtern für dieses Verzeichnis aktiviert ist. Wenn das Rückschreiben aktiviert ist, gibt es den Status des lokalen Diensts für das Rückschreiben an. Dies ist nützlich, wenn Sie das Rückschreiben von Kennwörtern vorübergehend deaktivieren möchten, ohne Azure AD Connect erneut zu konfigurieren.

* Wenn die Option auf **Ja** gesetzt ist, wird das Rückschreiben aktiviert, und Verbundbenutzer und Benutzer mit Kennworthashsynchronisierung können ihre Kennwörter zurücksetzen.
* Wenn die Option auf **Nein** gesetzt ist, wird das Rückschreiben deaktiviert, und Verbundbenutzer und Benutzer mit Kennworthashsynchronisierung können ihre Kennwörter nicht zurücksetzen.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Benutzern das Entsperren von Konten ohne Zurücksetzen des Kennworts erlauben

Dieses Steuerelement legt fest, ob Benutzer, die das Kennwortzurücksetzungsportal aufrufen, die Option zum Entsperren ihrer lokalen Active Directory-Konten ohne Zurücksetzen ihres Kennworts erhalten sollen. Standardmäßig werden bei einer Kennwortzurücksetzung Konten von Azure AD entsperrt. Mit dieser Einstellung können Sie diese beiden Vorgänge trennen. 

* Bei der Einstellung **Ja** erhalten Benutzer die Option zum Zurücksetzen ihres Kennworts und Entsperren ihres Kontos oder die Option zum Entsperren des Kontos, ohne dass das Kennwort zurückgesetzt werden muss.
* Bei der Einstellung **Nein** können Benutzer das Entsperren des Kontos nur in Kombination mit dem Zurücksetzen des Kennworts vornehmen.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Funktionsweise der Kennwortzurücksetzung für B2B-Benutzer
Das Zurücksetzen und Ändern von Kennwörtern wird von allen B2B-Konfigurationen (Business-to-Business) uneingeschränkt unterstützt. Das Zurücksetzen von B2B-Benutzerkennwörtern wird in den folgenden drei Fällen unterstützt:

   * **Benutzer aus einer Partnerorganisation mit einem vorhandenen Azure AD-Mandanten:** Falls die Organisation, mit der Sie eine Partnerschaft eingegangen sind, über einen Azure AD-Mandanten verfügt, *respektieren wir die in diesem Mandanten aktivierten Kennwortzurücksetzungsrichtlinien*. Damit die Kennwortzurücksetzung funktioniert, muss die Partnerorganisation nur sicherstellen, dass Azure AD SSPR aktiviert ist. Für Office 365-Kunden fallen keine zusätzlichen Gebühren an, und Azure AD SSPR kann anhand der Schritte unter [Schnelle Bereitstellung der Self-Service-Kennwortzurücksetzung in Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) aktiviert werden.
   * **Benutzer, die sich mit der Self-Service-Registrierung registriert haben:** Wenn die Organisation, mit der Sie eine Partnerschaft eingegangen sind, das [Self-Service-Registrierungsfeature](active-directory-self-service-signup.md) verwendet, um auf einen Mandanten zuzugreifen, steht eine Zurücksetzung des Kennworts unter Verwendung der E-Mail-Adresse zur Verfügung, mit der die Registrierung erfolgt ist.
   * **B2B-Benutzer:** Alle neuen B2B-Benutzer, die mithilfe der neuen [Azure AD-B2B-Funktionen](active-directory-b2b-what-is-azure-ad-b2b.md) erstellt werden, können ihre Kennwörter unter Verwendung der E-Mail-Adresse zurücksetzen, mit der sie sich im Rahmen des Einladungsprozesses registriert haben.

Navigieren Sie zum Testen dieses Szenarios mit einem dieser Partnerbenutzer zu „http://passwordreset.microsoftonline.com“. Sofern eine alternative E-Mail-Adresse oder eine E-Mail-Adresse für die Authentifizierung definiert ist, funktioniert die Kennwortzurücksetzung wie erwartet.

> [!NOTE]
> Microsoft-Konten (etwa Hotmail.com, Outlook.com oder eine andere persönliche E-Mail-Adresse), denen der Gastzugriff auf Ihren Azure AD-Mandanten gewährt wurde, können Azure AD SSPR nicht nutzen. Für diese Konten muss das Kennwort anhand der Informationen im Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) zurückgesetzt werden.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD:

* [Erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung](active-directory-passwords-best-practices.md)
* [Zurücksetzen oder Ändern des Kennworts](active-directory-passwords-update-your-own-password.md)
* [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md)
* [Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung](active-directory-passwords-licensing.md)
* [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung](active-directory-passwords-data.md)
* [Authentifizierungsmethoden](active-directory-passwords-how-it-works.md#authentication-methods)
* [Kennwortrichtlinien und -einschränkungen in Azure Active Directory](active-directory-passwords-policy.md)
* [Übersicht über die Kennwortrückschreibung](active-directory-passwords-writeback.md)
* [Berichterstellungsoptionen für die Kennwortverwaltung von Azure AD](active-directory-passwords-reporting.md)
* [Ausführliche Informationen zur Self-Service-Kennwortzurücksetzung in Azure AD](active-directory-passwords-how-it-works.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)

[Authentication]: ./media/active-directory-passwords-how-it-works/sspr-authentication-methods.png "Verfügbare Azure AD-Authentifizierungsmethoden und erforderliche Menge"
