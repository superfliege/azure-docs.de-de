---
title: Die Elemente der Einladungs-E-Mail für die B2B-Zusammenarbeit – Azure Active Directory | Microsoft-Dokumentation
description: Vorlage für die Einladungs-E-Mail von Azure Active Directory B2B-Zusammenarbeit
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: cbb81f62bcea43c3c30abff6b5f76e3a89347813
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985395"
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email---azure-active-directory"></a>Die Elemente der Einladungs-E-Mail für die B2B-Zusammenarbeit – Azure Active Directory

Einladungs-E-Mails sind eine wichtige Komponente zum Onboarding von Partnern als Benutzer von B2B-Zusammenarbeit in Azure AD. Sie können diese verwenden, um die Vertrauenswürdigkeit des Empfängers zu erhöhen. Sie können der E-Mail Rechtmäßigkeit und Glaubwürdigkeit verleihen, um sicherzustellen, dass der Empfänger ohne Bedenken die Schaltfläche **Loslegen** auswählt und die Einladung annimmt. Diese Vertrauensstellung ist eine der wichtigsten Voraussetzungen dafür, Konflikte bei der Freigabe zu reduzieren. Außerdem können Sie die E-Mail damit auch viel ansprechender machen.

![Einladungs-E-Mail für Azure AD B2B](media/invitation-email-elements/invitation-email.png)

## <a name="explaining-the-email"></a>Erläuterungen zur E-Mail
Sehen wir uns einige Elemente der E-Mail an, damit Sie diese Funktionen optimal nutzen können.

### <a name="subject"></a>Antragsteller
Der Betreff der E-Mail folgt dem folgenden Muster: Sie werden zur Organisation &lt;Mandantenname&gt; eingeladen

### <a name="from-address"></a>Absenderadresse
Wir verwenden ein LinkedIn-ähnliches Muster für die Absenderadresse.  Sie sollten klarstellen, wer der einladende Benutzer ist und welchem Unternehmen er angehört. Außerdem soll deutlich werden, dass die E-Mail von einer Microsoft-E-Mail-Adresse stammt. Das Format lautet: &lt;Anzeigename des Einladenden&gt; von &lt;Mandantenname&gt; (über Microsoft) <invites@microsoft.com>.

### <a name="reply-to"></a>Antworten an
Die Antwort-E-Mail-Adresse wird auf die E-Mail-Adresse des einladenden Benutzers festgelegt (falls verfügbar). Wenn eine Antwort auf die E-Mail gesendet wird, gelangt diese zurück an den einladenden Benutzer.

### <a name="branding"></a>Branding
Die Einladungs-E-Mails von Ihrem Mandanten verwenden das Unternehmensbranding, das Sie möglicherweise für Ihren Mandanten eingerichtet haben. Wenn Sie diese Funktion nutzen möchten, finden Sie [hier](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) die Informationen zur Konfiguration. Das Bannerlogo wird in der E-Mail angezeigt. Befolgen Sie die [hier](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) aufgeführten Anweisungen zur Bildgröße und -qualität, um ein optimales Ergebnis zu erzielen. Darüber hinaus wird auch der Unternehmensname in der Handlungsaufforderung angezeigt.

### <a name="call-to-action"></a>Call-to-Action (Handlungsaufforderung)
Die Handlungsaufforderung besteht aus zwei Teilen: der Erläuterung, warum der Empfänger die E-Mail erhalten hat, und der Aktion, die vom Empfänger erwartet wird.
- Der Bereich „Warum“ wird über das folgende Muster abgedeckt: Sie wurden eingeladen, auf Anwendungen in der Organisation &lt;Mandantenname&gt; zuzugreifen

- Der Bereich „Erwartete Aktion“ wird durch die Schaltfläche **Loslegen** dargestellt. Wenn der Empfänger hinzugefügt wurde, ohne dass eine Einladung erforderlich ist, wird diese Schaltfläche nicht angezeigt.

### <a name="inviters-information"></a>Informationen zum einladenden Benutzer
Der Anzeigename des einladenden Benutzers ist in der E-Mail enthalten. Wenn Sie ein Profilbild für Ihr Azure AD-Konto eingerichtet haben, ist auch dieses Bild in der Einladungs-E-Mail enthalten. Beides dient dazu, das Vertrauen Ihres Empfängers in die E-Mail zu erhöhen.

Wenn Sie noch kein Profilbild eingerichtet haben, wird anstelle des Bilds ein Symbol mit den Initialen des einladenden Benutzers gezeigt:

  ![Anzeigen der Initialen des einladenden Benutzers](media/invitation-email-elements/inviters-initials.png)

### <a name="body"></a>Body
Der Text enthält die Nachricht, die der einladende Benutzer erstellt hat bzw. die über die Einladungs-API übergeben wurde. Dies ist ein Textbereich, der aus Sicherheitsgründen keine HTML-Tags verarbeitet.

### <a name="footer-section"></a>Fußzeilenabschnitt
Die Fußzeile enthält die Microsoft-Unternehmensmarke und informiert den Empfänger, wenn die E-Mail von einem nicht überwachten Alias aus gesendet wurde. Sonderfälle:

- Der einladende Benutzer besitzt keine E-Mail-Adresse im einladenden Mandanten.

  ![Der einladende Benutzer besitzt keine E-Mail-Adresse im einladenden Mandanten – Abbildung](media/invitation-email-elements/inviter-no-email.png)


- Der Empfänger muss die Einladung nicht annehmen.

  ![Der Empfänger muss die Einladung nicht annehmen](media/invitation-email-elements/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

- [Was ist Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
- [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](add-users-administrator.md)
- [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](add-users-information-worker.md)
- [B2B-Zusammenarbeit: Einlösen von Einladungen](redemption-experience.md)
- [Hinzufügen von Benutzern der B2B-Zusammenarbeit ohne Einladung](add-user-without-invite.md)
