---
title: Azure Active Directory Identity Protection-Benachrichtigungen | Microsoft Docs
description: "Erfahren Sie, wie Benachrichtigungen Ihre Untersuchungsaktivitäten unterstützen."
services: active-directory
keywords: Azure Active Directory Identity Protection, Cloud App Discovery, Verwalten von Anwendungen, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: bea21439afef4fda453732edffc84c62667dfe38
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Identity Protection Benachrichtigungen

Von Azure AD Identity Protection werden zwei Arten von automatisierten Benachrichtigungs-E-Mails gesendet, die der Verwaltung von Benutzerrisiko- und Risikoereignissen dienen:

- E-Mail für erkannte gefährdete Benutzer
- Wöchentliche E-Mail mit Übersicht

Dieser Artikel gibt Ihnen eine Übersicht zu beiden Benachrichtigungs-E-Mails.


## <a name="users-at-risk-detected-email"></a>E-Mail für erkannte gefährdete Benutzer

Als Reaktion auf ein erkanntes gefährdetes Konto erstellt Azure Active Directory Identity Protection eine E-Mail-Warnung mit dem Betreff **Gefährdete Benutzer erkannt**. Die E-Mail enthält einen Link zum Bericht vom Typ **[Benutzer mit Risikokennzeichnung](active-directory-reporting-security-user-at-risk.md)**. Als bewährte Methode sollten Sie die gefährdeten Benutzer sofort untersuchen.

![E-Mail für erkannte gefährdete Benutzer](./media/active-directory-identityprotection-notifications/01.png)


### <a name="configuration"></a>Konfiguration

Als Administrator können Sie Folgendes festlegen:

- **Die Risikostufe, bei der die Erstellung dieser E-Mail ausgelöst wird**: Standardmäßig ist die Risikostufe auf „Hoch“ festgelegt.
- **Die Empfänger dieser E-Mail**: Standardmäßig umfassen die Empfänger alle globalen Administratoren. Globale Administratoren können außerdem weitere globale Administratoren, Sicherheitsadministratoren und Benutzer mit Leseberechtigung für Sicherheitsfunktionen hinzufügen.  


Um das zugehörige Dialogfeld zu öffnen, klicken auf der Seite **Identity Protection** im Abschnitt **Einstellungen** auf **Warnungen**.

![E-Mail für erkannte gefährdete Benutzer](./media/active-directory-identityprotection-notifications/05.png)


## <a name="weekly-digest-email"></a>Wöchentliche E-Mail mit Übersicht

Die wöchentliche E-Mail mit einer Übersicht enthält eine Zusammenfassung der neuen Risikoereignisse.  
Sie hat folgenden Inhalt:

- Gefährdete Benutzer

- Verdächtige Aktivitäten

- Erkannte Sicherheitsrisiken

- Links zu verwandten Berichten in Identity Protection

    ![Wiederherstellung](./media/active-directory-identityprotection-notifications/400.png "Wiederherstellung")

### <a name="configuration"></a>Konfiguration

Als Administrator können Sie das Senden einer wöchentlichen Übersichts-E-Mail deaktivieren.

![Benutzerrisiken](./media/active-directory-identityprotection-notifications/62.png "Benutzerrisiken")

Um das zugehörige Dialogfeld zu öffnen, klicken auf der Seite **Identity Protection** im Abschnitt **Einstellungen** auf **Wöchentliche Übersicht**.

![E-Mail für erkannte gefährdete Benutzer](./media/active-directory-identityprotection-notifications/04.png)


## <a name="see-also"></a>Weitere Informationen

- [Azure Active Directory Identity Protection](active-directory-identityprotection.md)
