---
title: Übersicht über die Microsoft Authenticator-App – Azure Active Directory | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, was die Microsoft Authenticator-App ist, wie Sie funktioniert und welche Informationen sie enthält.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 01/24/2019
ms.author: lizross
ms.openlocfilehash: 2379f1ff4fb4385015cc6077cb923cab998d1d11
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175212"
---
# <a name="what-is-the-microsoft-authenticator-app"></a>Wozu dient die Microsoft Authenticator-App?
Die Microsoft Authenticator-App unterstützt Sie bei der Kontoanmeldung, wenn Sie die zweistufige Überprüfung verwenden. Mit der zweistufigen Überprüfung können Sie den Zugriff auf Ihre Konten besser schützen, insbesondere, wenn Sie vertrauliche Informationen anzeigen. Kennwörter können vergessen, gestohlen oder kompromittiert werden. Die zweistufige Überprüfung ist eine zusätzliche Sicherheitsmaßnahme, die zum Schutz Ihres Kontos beiträgt, indem sie den Zugriff durch Unbefugte erschwert.

Die Verwendung der Microsoft Authenticator-App kann auf verschiedene Arten erfolgen:

- Lassen Sie sich nach der Anmeldung mit Ihrem Benutzernamen und Kennwort zu einer zweiten Überprüfungsmethode auffordern.

- Stellen Sie die Anmeldung ohne Kennwort bereit, indem Sie Ihren Benutzernamen und Ihr mobiles Gerät mit Ihrem Fingerabdruck, der Gesichtserkennung oder einer PIN verwenden.

>[!Important]
>Dieser Inhalt richtet sich an Benutzer. Administratoren finden in der [Azure Active Directory-Dokumentation](https://docs.microsoft.com/azure/active-directory) weitere Informationen zum Einrichten und Verwalten der Azure Active Directory-Umgebung (Azure AD).<br><br>Wenn Sie Probleme bei der Anmeldung bei Ihrem Konto haben, lesen Sie dem Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429).  Erhalten Sie weitere Informationen zur Meldung [Dieses Microsoft-Konto ist nicht vorhanden.](https://support.microsoft.com/help/13811), wenn Sie sich bei Ihrem Microsoft-Konto anmelden.

## <a name="terminology"></a>Begriff
|Begriff|BESCHREIBUNG|
|----|-----------|
|Zweistufige Überprüfung |Bei dieser Überprüfung benötigen Sie in der Regel nur zwei Angaben zur Überprüfung, etwa ein Kennwort und eine PIN. Die Microsoft Authenticator-App unterstützt sowohl die standardmäßige zweistufige Überprüfung als auch die Anmeldung ohne Kennwort.|
|Multi-Factor Authentication (MFA)|Jede zweistufige Überprüfung stellt eine mehrstufige Authentifizierung dar. Dabei müssen Sie je nach den Anforderungen Ihrer Organisation *mindestens* zwei Angaben zur Überprüfung machen.|
|Microsoft-Konto (auch MSA genannt)|Sie erstellen Ihre persönlichen Microsoft-Konten, um Zugriff auf Ihre verbraucherorientierten Microsoft-Produkte und -Clouddienste wie Outlook, OneDrive, MSN, Xbox LIVE oder Office 365 zu erhalten. Ihr Microsoft-Konto wird im von Microsoft betriebenen Microsoft-Kontosystem für Endbenutzeridentitäten erstellt und gespeichert.|
|Geschäfts- oder Schulkonto|Ihre Organisation erstellt Ihr Geschäfts-, Schul- oder Unikonto (z.B. alain@contoso.com), über das Sie Zugriff auf interne und möglicherweise eingeschränkte Ressourcen wie Microsoft Azure, Windows Intune und Office 365 erhalten.|

## <a name="how-two-factor-verification-works-with-the-app"></a>Funktionsweise der zweistufigen Überprüfung mit der App
Die zweistufige Überprüfung mit der Microsoft Authenticator-App funktioniert wie folgt:

- **Benachrichtigung**. Geben Sie Ihren Benutzernamen und Ihr Kennwort auf dem Gerät ein, auf dem Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto bzw. Ihrem persönlichen Microsoft-Konto anmelden. Die Microsoft Authenticator-App sendet eine Benachrichtigung mit der Frage **Anmeldung genehmigen?**. Wählen Sie **Genehmigen** aus, wenn Sie den Anmeldeversuch anerkennen. Andernfalls wählen Sie **Verweigern** aus. Wenn Sie **Ablehnen** wählen, können Sie die Anforderung auch als betrügerisch markieren.

- **Prüfcode**. Geben Sie Ihren Benutzernamen und Ihr Kennwort auf dem Gerät ein, auf dem Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto bzw. Ihrem persönlichen Microsoft-Konto anmelden. Fügen Sie dann den zugehörigen Prüfcode vom Bildschirm **Konten** der Microsoft Authenticator-App ein.

- **Anmeldung ohne Kennwort.** Geben Sie Ihren Benutzernamen und Ihr Kennwort auf dem Gerät ein, auf dem Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto bzw. Ihrem persönlichen Microsoft-Konto anmelden. Bestätigen Sie dann auf Ihrem mobilen Gerät per Fingerabdruck, Gesichtserkennung oder PIN Ihre Identität. Bei dieser Methode müssen Sie kein Kennwort eingeben.

### <a name="whether-to-use-your-devices-biometric-capabilities"></a>Verwenden der Biometriefunktionen Ihres Geräts
Wenn Sie eine PIN verwenden, um die Authentifizierung abzuschließen, können Sie in der Microsoft Authenticator-App die Biometriefunktionen Ihres Geräts (Fingerabdruck oder Gesichtserkennung) aktivieren. Sie können diese Funktionalität einrichten, wenn Sie die Authenticator-App zum ersten Mal zum Überprüfen Ihres Kontos verwenden. Wählen Sie dabei die Option zum Verwenden der Biometriefunktionen Ihres Geräts anstelle Ihrer PIN für die Identifizierung aus.

## <a name="who-decides-if-you-use-this-feature"></a>Wer entscheidet über die Verwendung dieser Funktion?
Je nach Kontotyp entscheidet Ihre Organisation, ob Sie die zweistufige Überprüfung verwenden müssen. Möglicherweise können Sie diese Entscheidung aber auch selbst treffen.

- **Geschäfts-, Schul- oder Unikonto.** Wenn Sie ein Geschäfts-, Schul- oder Uni-Konto verwenden (z. B. alain@contoso.com), entscheidet Ihre Organisation, ob Sie die zweistufige Überprüfung verwenden müssen. Sie gibt zudem vor, welche Überprüfungsmethode anzuwenden ist. Weitere Informationen zur mehrstufigen Authentifizierung finden Sie unter [Was ist Azure Multi-Factor Authentication?](multi-factor-authentication-end-user.md). Weitere Informationen zum Einrichten der Sicherheitsinformationen zum Verwenden der Microsoft Authenticator-App finden Sie unter [Einrichten der Sicherheitsinformationen zur Verwendung einer Authenticator-App (Vorschau)](security-info-setup-auth-app.md).

- **Persönliches Microsoft-Konto.** Sie können die zweistufige Überprüfung auch für Ihre persönlichen Microsoft-Konten einrichten (z.B. alain@outlook.com).

- **Persönliches, nicht von Microsoft bereitgestelltes Konto.** Sie können die zweistufige Überprüfung auch für Ihre persönlichen, nicht von Microsoft bereitgestellten Konten einrichten (z.B. alain@gmail.com). Ihre nicht von Microsoft bereitgestellten Konten verwenden möglicherweise nicht den Begriff „zweistufige Überprüfung“. Sie finden die Funktion jedoch in der Regel in den Einstellungen für die **Sicherheit** oder die **Anmeldung**.

## <a name="in-this-section"></a>Inhalt dieses Abschnitts

|Artikel |BESCHREIBUNG |
|------|------------|
|[Herunterladen und Installieren der App](microsoft-authenticator-app-how-to.md)|Hier erfahren Sie, wo und wie Sie die Microsoft Authenticator-App für Geräte mit Android, iOS und Windows Phone herunterladen und installieren.|
|[Add your work or school accounts](microsoft-authenticator-app-add-work-account.md) (Hinzufügen von Geschäfts-, Schul- oder Unikonten)|Hier erfahren Sie, wie Sie Geschäfts-, Schul- oder Unikonten bzw. persönliche Konten zur Microsoft Authenticator-App hinzufügen.|
|[Add your personal accounts](microsoft-authenticator-app-add-personal-account.md) (Hinzufügen persönlicher Konten)|Hier erfahren Sie, wie Sie persönliche Microsoft-Konten sowie nicht von Microsoft bereitgestellte Konten zur Microsoft Authenticator-App hinzufügen.|
|[Manually add your accounts](microsoft-authenticator-app-add-account-manual.md) (Manuelles Hinzufügen von Konten)|Hier erfahren Sie, wie Sie der Microsoft Authenticator-App Ihre Konten manuell hinzufügen, wenn der bereitgestellte QR-Code nicht gescannt werden kann.|
|[Sign-in using the app](microsoft-authenticator-app-phone-signin-faq.md) (Anmelden mit der App)|Hier erfahren Sie, wie Sie sich mit der Microsoft Authenticator-App bei verschiedenen Konten anmelden.|
|[Sichern und Wiederherstellen von Kontoanmeldeinformationen](microsoft-authenticator-app-backup-and-recovery.md)| Enthält Informationen zum Sichern und Wiederherstellen Ihrer Kontoanmeldeinformationen mithilfe der Microsoft Authenticator-App.|
|[Microsoft Authenticator-App – häufig gestellte Fragen](microsoft-authenticator-app-faq.md)|Enthält Antworten auf häufig gestellte Fragen zur App.|
