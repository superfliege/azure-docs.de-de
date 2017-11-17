---
title: 'Berichterstellung: Azure AD SSPR | Microsoft-Dokumentation'
description: "Berichterstellung zu Ereignissen der Self-Service-Kennwortzurücksetzung von Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: e4524704c6db0d21388ea407870c65d4f69a6323
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Berichterstellungsoptionen für die Kennwortverwaltung von Azure AD

Nach der Bereitstellung möchten viele Organisationen wissen, wie oder ob SSPR tatsächlich verwendet wird. Azure AD stellt Berichtsfunktionen bereit, mit denen Sie Fragen mithilfe vordefinierter Berichte beantworten, die ordnungsgemäße Lizenzierung ermitteln und benutzerdefinierte Abfragen erstellen können.

![Berichterstellung][Reporting]

Die folgenden Fragen können mit Berichten beantwortet werden, die im [Azure-Portal] (https://portal.azure.com/) vorhanden sind.

> [!NOTE]
> Sie müssen [ein globaler Administrator](active-directory-assign-admin-roles-azure-portal.md) sein und müssen aktivieren, dass diese Daten im Auftrag Ihres Unternehmens erfasst werden, indem Sie die Registerkarte „Berichterstellung“ oder Überwachungsprotokolle mindestens einmal besuchen. Bis dahin werden für Ihre Organisation keine Daten gesammelt.

* Wie viele Personen haben sich für die Kennwortzurücksetzung registriert?
* Wer hat sich für das Zurücksetzen von Kennwörtern registriert?
* Welche Daten registrieren die Benutzer?
* Wie viele Personen haben ihre Kennwörter in den letzten sieben Tagen zurückgesetzt?
* Welche Methoden werden von Benutzern und Administratoren am häufigsten zum Zurücksetzen von Kennwörtern eingesetzt?
* Welche Probleme treten häufig für Benutzer oder Administratoren bei dem Versuch auf, das Kennwort zurückzusetzen?
* Welche Administratoren setzen häufig ihre eigenen Kennwörter zurück?
* Gibt es verdächtige Aktivitäten beim Zurücksetzen des Kennworts?

## <a name="power-bi-content-pack"></a>Power BI-Inhaltspaket

Als Power BI-Benutzer können Sie ein Inhaltspaket für Azure AD nutzen, dass eine benutzerfreundliche Berichterstellung für SSPR ermöglicht. Weitere Informationen zum Verwenden und Bereitstellen des Inhaltspakets finden Sie im Artikel [Verwenden des Azure Active Directory-Power BI-Inhaltspakets](active-directory-reporting-power-bi-content-pack-how-to.md). Sie können dann Ihre eigenen Dashboards erstellen und diese mit anderen Personen in Ihrer Organisation gemeinsam nutzen.

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Anzeigen von Kennwortverwaltungsberichten im Azure-Portal

Im Azure-Portal können Kennwortzurücksetzungen sowie Aktivitäten zur Registrierung für die Kennwortzurücksetzung besser angezeigt werden.  Gehen Sie wie folgt vor, um die Ereignisse zur Kennwortzurücksetzung und zur Registrierung für die Kennwortzurücksetzung anzuzeigen:

1. Navigieren Sie zu [**portal.azure.com**](https://portal.azure.com).
2. Klicken Sie auf der linken Seite im Hauptnavigationsbereich des Azure-Portals auf das Menü **Weitere Dienste**.
3. Suchen Sie in der Liste mit den Diensten nach **Azure Active Directory**, und wählen Sie die entsprechende Option aus.
4. Klicken Sie im Azure Active Directory-Navigationsmenü auf **Benutzer und Gruppen**.
5. Klicken Sie im Navigationsmenü für „Benutzer und Gruppen“ auf das Navigationselement **Überwachungsprotokolle**. Daraufhin werden sämtliche Überwachungsereignisse für alle Benutzer in Ihrem Verzeichnis angezeigt. Diese Ansicht kann auch gefiltert werden, um alle kennwortbezogenen Ereignisse anzuzeigen.
6. Wenn Sie diese Ansicht nach den Ereignissen filtern möchten, die mit der Kennwortzurücksetzung zusammenhängen, klicken Sie im oberen Bereich des Blatts auf die Schaltfläche **Filter**.
7. Wählen Sie im Menü **Filter** die Dropdownliste **Kategorie** und anschließend den Kategorietyp **Self-Service-Kennwortverwaltung** aus.
8. Grenzen Sie die Liste optional weiter ein, indem Sie die spezifische **Aktivität** auswählen, für die Sie sich interessieren.

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Abrufen von Kennwortverwaltungsereignissen von der API für Azure AD-Berichte und -Ereignisse

Die API für Azure AD-Berichte und -Ereignisse unterstützt das Abrufen aller Informationen, die in den Berichten „Kennwortzurücksetzung“ und „Registrierung für Zurücksetzen des Kennworts“ enthalten sind. Mithilfe dieser API können Sie einzelne Ereignisse zur Kennwortzurücksetzung und zur Registrierung für die Kennwortzurücksetzung herunterladen und in die Berichtstechnologie Ihrer Wahl integrieren.

### <a name="how-to-get-started-with-the-reporting-api"></a>Erste Schritte mit der Reporting-API

Zum Zugreifen auf diese Daten müssen Sie eine kleine App oder ein Skript schreiben, um sie von unseren Servern abzurufen. [Hier erfahren Sie, wie Sie die ersten Schritte mit der Azure AD Reporting-API ausführen](active-directory-reporting-api-getting-started.md).

Nachdem Sie über ein funktionierendes Skript verfügen, sollten Sie sich als Nächstes mit den Kennwortzurücksetzungs- und Registrierungsereignissen befassen, die Sie zum Erfüllen Ihrer Szenarien abrufen können.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): Listet die Spalten auf, die für Ereignisse zum Zurücksetzen des Kennworts verfügbar sind.
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): Listet die Spalten auf, die für Ereignisse zum Registrieren der Kennwortzurücksetzung verfügbar sind.

### <a name="reporting-api-data-retrieval-limitations"></a>Datenabrufeinschränkungen der Reporting-API

Derzeit ruft die API für Azure AD-Berichte und -Ereignisse bis zu **75.000 Einzelereignisse** vom Typ [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) und [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) aus den **letzten 30 Tagen** ab.

Wenn Sie weiter zurückliegende Daten abrufen oder speichern möchten, sollten Sie sie in einer externen Datenbank speichern und mithilfe der API die resultierenden Deltas abfragen. Es empfiehlt sich, mit dem Abrufen der Daten zu beginnen, wenn in Ihrer Organisation die SSPR-Verwendung gestartet wird. Speichern Sie die Daten extern, und verfolgen Sie ab diesem Punkt die Deltas nach.

## <a name="description-of-report-columns-in-azure-portal"></a>Beschreibung der Berichtsspalten im Azure-Portal

In der folgende Liste werden alle Berichtsspalten im Detail beschrieben:

* **Benutzer** – Der Benutzer, der versucht hat, sich für die Kennwortzurücksetzung zu registrieren.
* **Rolle** – Die Rolle des Benutzers im Verzeichnis.
* **Datum und Uhrzeit** – Datum und Uhrzeit des Versuchs.
* **Registrierte Daten** – Die Authentifizierungsdaten, die vom Benutzer während der Registrierung für die Kennwortzurücksetzung bereitgestellt wurden.

## <a name="description-of-report-values-in-azure-portal"></a>Beschreibung der Berichtswerte im Azure-Portal

Die folgende Tabelle beschreibt die verschiedenen Werte, die für die einzelnen Spalten zulässig sind:

| Column | Zulässige Werte und ihre Bedeutung |
| --- | --- |
| Registrierte Daten |**Alternative E-Mail-Adresse** – Benutzer hat für die Authentifizierung eine alternative E-Mail-Adresse oder eine E-Mail-Adresse zur Authentifizierung verwendet.<p><p>**Bürotelefon** – Benutzer hat seine Bürotelefonnummer zur Authentifizierung verwendet.<p>**Mobiltelefon** – Benutzer hat sein Mobiltelefon oder sein Authentifizierungstelefon zum Authentifizieren verwendet.<p>**Sicherheitsfragen** – Benutzer hat Sicherheitsfragen zur Authentifizierung verwendet.<p>**Eine beliebige Kombination der oben genannten Daten (z.B. alternative E-Mail-Adresse und Mobiltelefon)** – Tritt auf, wenn eine Richtlinie für die zweistufige Überprüfung angegeben ist, und zeigt, welche beiden Methoden der Benutzer zur Authentifizierung seiner Anforderung zum Zurücksetzen des Kennworts verwendet hat. |

## <a name="self-service-password-management-activity-types"></a>Aktivitätstypen für die Self-Service-Kennwortverwaltung

In der Überwachungsereigniskategorie **Self-Service-Kennwortverwaltung** werden folgende Aktivitätstypen angezeigt.  Danach ist jeweils eine Beschreibung angegeben.

* [**Blocked from self-service password reset**](#activity-type-blocked-from-self-service-password-reset) (Von Self-Service-Kennwortzurücksetzung ausgeschlossen): Gibt an, dass ein Benutzer in den letzten 24 Stunden mehr als fünfmal versucht hat, ein Kennwort zurückzusetzen, ein bestimmtes Gate zu verwenden oder eine Telefonnummer zu bestätigen.
* [**Change password (self-service)**](#activity-type-change-password-self-service) (Kennwort ändern (Self-Service)): Gibt an, dass ein Benutzer ein Kennwort geändert hat (entweder freiwillig oder weil das Kennwort abgelaufen ist).
* [**Reset password (by admin)**](#activity-type-reset-password-by-admin) (Kennwort zurücksetzen (durch Administrator)): Gibt an, dass ein Administrator im Auftrag eines Benutzers ein Kennwort über das Azure-Portal zurückgesetzt hat.
* [**Reset password (self-service)**](#activity-type-reset-password-self-service) (Kennwort zurücksetzen (Self-Service)): Gibt an, dass ein Benutzer sein Kennwort über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) zurückgesetzt hat.
* [**Self serve password reset flow activity progress**](#activity-type-self-serve-password-reset-flow-activity-progress) (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs): Gibt die einzelnen Schritte an, die ein Benutzer im Rahmen des Kennwortzurücksetzungsprozesses durchläuft (beispielsweise ein bestimmtes Authentifizierungsgate für die Kennwortzurücksetzung).
* [**Unlock user account (self-service)**](#activity-type-unlock-user-account-self-service) (Benutzerkonto entsperren (Self-Service)): Gibt an, dass ein Benutzer sein Active Directory-Konto über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) mithilfe des Features zur AD-Kontoentsperrung ohne Zurücksetzen entsperrt hat, ohne das Kennwort zurückzusetzen.
* [**User registered for self-service password reset**](#activity-type-user-registered-for-self-service-password-reset) (Für Self-Service-Kennwortzurücksetzung registrierter Benutzer): Gibt an, dass ein Benutzer alle erforderlichen Informationen registriert hat, um sein Kennwort gemäß der aktuellen Mandanten-Kennwortzurücksetzungsrichtlinie zurücksetzen zu können.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Aktivitätstyp: „Blocked from self-service password reset“ (Von Self-Service-Kennwortzurücksetzung ausgeschlossen)

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer in den letzten 24 Stunden mehr als fünfmal versucht hat, ein Kennwort zurückzusetzen, ein bestimmtes Gate zu verwenden oder eine Telefonnummer zu bestätigen.
* **Actor der Aktivität:** Der Benutzer, dessen weitere Zurücksetzungsvorgänge gedrosselt wurden. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, dessen weitere Zurücksetzungsvorgänge gedrosselt wurden. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Benutzer gedrosselt wurde, sodass er in den nächsten 24 Stunden keine weiteren Zurücksetzungsvorgänge mehr ausführen, keine weiteren Authentifizierungsmethoden mehr verwenden und keine zusätzlichen Telefonnummern mehr bestätigen kann.
* **Ursache für Aktivitätsstatusfehler:** nicht zutreffend

### <a name="activity-type-change-password-self-service"></a>Aktivitätstyp: „Change password (self-service)“ (Kennwort ändern (Self-Service))

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer ein Kennwort geändert hat (entweder freiwillig oder weil das Kennwort abgelaufen ist).
* **Akteur der Aktivität:** Der Benutzer, der sein Kennwort geändert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sein Kennwort geändert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Benutzer sein Kennwort geändert hat.
  * _Fehler:_ Gibt an, dass ein Benutzer sein Kennwort nicht ändern konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.
* **Ursache für Aktivitätsstatusfehler** - 
  * _FuzzyPolicyViolationInvalidPassword:_ Das vom Benutzer gewählte Kennwort wurde durch die Erkennung nicht zulässiger Kennwörter von Microsoft automatisch blockiert, da es zu allgemein oder besonders unsicher war.

### <a name="activity-type-reset-password-by-admin"></a>Aktivitätstyp: „Reset password (by admin)“ (Kennwort zurücksetzen (durch Administrator))

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Administrator im Auftrag eines Benutzers ein Kennwort über das Azure-Portal zurückgesetzt hat.
* **Akteur der Aktivität:** Der Administrator, der das Kennwort im Auftrag eines anderen Benutzers oder Administrators zurückgesetzt hat. Hierbei muss es sich um einen globalen Administrator, um einen Kennwortadministrator, um einen Benutzeradministrator oder um einen Helpdeskadministrator handeln.
* **Ziel der Aktivität:** Der Benutzer, dessen Kennwort zurückgesetzt wurde. Hierbei kann es sich um einen Endbenutzer oder um einen anderen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Administrator das Kennwort eines Benutzers zurückgesetzt hat.
  * _Fehler:_ Gibt an, dass ein Administrator das Kennwort eines Benutzers nicht ändern konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.

### <a name="activity-type-reset-password-self-service"></a>Aktivitätstyp: „Reset password (self-service)“ (Kennwort zurücksetzen (Self-Service))

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer sein Kennwort über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) zurückgesetzt hat.
* **Akteur der Aktivität:** Der Benutzer, der sein Kennwort zurückgesetzt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sein Kennwort zurückgesetzt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Benutzer sein eigenes Kennwort zurückgesetzt hat.
  * _Fehler:_ Gibt an, dass ein Benutzer sein eigenes Kennwort nicht zurücksetzen konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.
* **Ursache für Aktivitätsstatusfehler** -
  * _FuzzyPolicyViolationInvalidPassword:_ Das vom Administrator gewählte Kennwort wurde durch die Erkennung nicht zulässiger Kennwörter von Microsoft automatisch blockiert, da es zu allgemein oder besonders unsicher war.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Aktivitätstyp: „Self serve password reset flow activity progress“ (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs)

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt die einzelnen Schritte an, die ein Benutzer im Rahmen des Kennwortzurücksetzungsprozesses durchläuft (beispielsweise ein bestimmtes Authentifizierungsgate für die Kennwortzurücksetzung).
* **Actor der Aktivität:** Der Benutzer, der einen Teil des Kennwortzurücksetzungsablaufs ausgeführt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der einen Teil des Kennwortzurücksetzungsablaufs ausgeführt hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Benutzer einen bestimmten Schritt des Kennwortzurücksetzungsablaufs ausgeführt hat.
  * _Fehler:_ Gibt an, dass ein bestimmter Schritt des Kennwortzurücksetzungsablaufs nicht erfolgreich war. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.
* **Zulässige Ursachen für Aktivitätsstatus**
  * Eine Tabelle mit allen zulässigen Statusgründen für die Zurücksetzungsaktivität finden Sie [weiter unten](#allowed-values-for-details-column).

### <a name="activity-type-unlock-user-account-self-service"></a>Aktivitätstyp: „Unlock user account (self-service)“ (Benutzerkonto entsperren (Self-Service))

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:** Gibt an, dass ein Benutzer sein Active Directory-Konto über das [Azure AD-Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com) mithilfe des Features zur AD-Kontoentsperrung ohne Zurücksetzen entsperrt hat, ohne das Kennwort zurückzusetzen.
* **Akteur der Aktivität:** Der Benutzer, der sein Konto entsperrt hat, ohne das Kennwort zurückzusetzen. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sein Konto entsperrt hat, ohne das Kennwort zurückzusetzen. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass ein Benutzer sein eigenes Konto entsperrt hat.
  * _Fehler:_ Gibt an, dass ein Benutzer sein Konto nicht entsperren konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Aktivitätstyp: „User registered for self-service password reset“ (Für Self-Service-Kennwortzurücksetzung registrierter Benutzer)

In der folgenden Liste werden die Details dieser Aktivität erläutert:

* **Beschreibung der Aktivität:**: Gibt an, dass ein Benutzer alle erforderlichen Informationen registriert hat, um sein Kennwort gemäß der aktuellen Mandanten-Kennwortzurücksetzungsrichtlinie zurücksetzen zu können. 
* **Actor der Aktivität:** Der Benutzer, der sich für die Kennwortzurücksetzung registriert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Ziel der Aktivität:** Der Benutzer, der sich für die Kennwortzurücksetzung registriert hat. Hierbei kann es sich um einen Endbenutzer oder um einen Administrator handeln.
* **Zulässige Statuswerte der Aktivität**
  * _Erfolgreich:_ Gibt an, dass sich ein Benutzer gemäß aktueller Richtlinie für die Kennwortzurücksetzung registriert hat. 
  * _Fehler:_ Gibt an, dass sich ein Benutzer nicht für die Kennwortzurücksetzung registrieren konnte. Durch Klicken auf die Zeile können Sie die Kategorie **Activity Status Reason** (Grund für den Aktivitätsstatus) anzeigen, um mehr über die Fehlerursache zu erfahren. Das bedeutet nicht, dass ein Benutzer sein eigenes Kennwort nicht zurücksetzen kann, sondern lediglich, dass er den Registrierungsprozess nicht abgeschlossen hat. Wenn das Konto des Benutzers korrekte Daten (beispielsweise eine noch nicht bestätigte Telefonnummer) enthält, können diese trotzdem zum Zurücksetzen des Kennworts verwendet werden, auch wenn die Telefonnummer noch nicht bestätigt wurde. Weitere Informationen finden Sie unter [Was geschieht bei Registrierung eines Benutzers?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers).

## <a name="next-steps"></a>Nächste Schritte

* [Erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung](active-directory-passwords-best-practices.md)
* [Ich habe mein Azure AD-Kennwort vergessen. Was nun?](active-directory-passwords-update-your-own-password.md)
* [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md)
* [Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung](active-directory-passwords-licensing.md)
* [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung](active-directory-passwords-data.md)
* [Authentifizierungsmethoden](active-directory-passwords-how-it-works.md#authentication-methods)
* [Kennwortrichtlinien und -einschränkungen in Azure Active Directory](active-directory-passwords-policy.md)
* [Worum handelt es sich beim Rückschreiben von Kennwörtern, und warum sollte ich mir Gedanken darüber machen?](active-directory-passwords-writeback.md)
* [Welche Optionen sind für SSPR verfügbar, und was bedeuten sie?](active-directory-passwords-how-it-works.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)

[Reporting]: ./media/active-directory-passwords-reporting/sspr-reporting.png "Beispiel für Protokolle zur Überwachung der SSPR-Aktivität in Azure AD"
