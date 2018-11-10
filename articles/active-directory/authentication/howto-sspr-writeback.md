---
title: 'Vorgehensweise: Konfigurieren des Kennwortrückschreibens für Azure AD SSPR'
description: Verwenden von Azure AD und Azure AD Connect zum Rückschreiben von Kennwörtern in ein lokales Verzeichnis
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: e8a09a9fc87bff692b5d5b4c54f87839f2591b63
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086946"
---
# <a name="how-to-configure-password-writeback"></a>Vorgehensweise: Konfigurieren des Kennwortrückschreibens

Sie sollten das automatische Update von [Azure AD Connect](../hybrid/how-to-connect-install-express.md) verwenden, wenn Sie das Kennwortrückschreiben nutzen möchten.

Für die folgenden Schritte wird davon ausgegangen, dass Sie Azure AD Connect bereits in Ihrer Umgebung konfiguriert haben, indem Sie die [Express](../hybrid/how-to-connect-install-express.md)- oder [benutzerdefinierten](../hybrid/how-to-connect-install-custom.md) Einstellungen verwendet haben.

1. Um das Kennwortrückschreiben zu konfigurieren und zu aktivieren, melden Sie sich bei Ihrem Azure AD Connect-Server an, und starten Sie den **Azure AD Connect**-Konfigurations-Assistenten.
2. Wählen Sie auf der Seite **Willkommen** die Option **Konfigurieren** aus.
3. Wählen Sie auf der Seite **Weitere Aufgaben** die Option **Synchronisierungsoptionen anpassen** aus, und wählen Sie dann **Weiter** aus.
4. Geben Sie auf der Seite **Mit Azure AD verbinden** die Anmeldeinformationen eines globalen Administrators ein, und wählen Sie dann **Weiter** aus.
5. Wählen Sie auf den Seiten **Verzeichnisse verbinden** und **Domänen-/OE-Filterung** die Schaltfläche **Weiter** aus.
6. Aktivieren Sie auf der Seite **Optionale Features** das Kontrollkästchen neben **Kennwortrückschreiben**, und wählen Sie **Weiter** aus.
   ![Aktivieren des Kennwortrückschreibens in Azure AD Connect][Writeback]
7. Wählen Sie auf der Seite **Bereit zur Konfiguration** die Option **Konfigurieren** aus, und warten Sie, bis der Vorgang abgeschlossen ist.
8. Wenn Sie sehen, dass die Konfiguration beendet ist, wählen Sie **Beenden** aus.

Informationen zu allgemeinen Aufgaben zur Problembehandlung im Zusammenhang mit dem Kennwortrückschreiben finden Sie im Artikel zur Problembehandlung im Abschnitt [Problembehandlung: Kennwortrückschreiben](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback).

> [!WARNING]
> Das Kennwortrückschreiben funktioniert nicht mehr für Kunden, die Azure AD Connect-Versionen bis 1.0.8641.0 verwenden, wenn der [Azure Access Control Service (ACS) am 7. November 2018 eingestellt wird](../develop/active-directory-acs-migration.md). Azure AD Connect-Versionen bis 1.0.8641.0 lassen ab diesem Zeitpunkt kein Kennwortrückschreiben mehr zu, da sie für diese Funktionalität von ACS abhängen.
>
> Um eine Dienstunterbrechung zu vermeiden, führen Sie bei früheren Versionen von Azure AD Connect ein Upgrade auf eine neuere Version durch. Weitere Informationen finden Sie unter [Azure AD Connect: Aktualisieren von einer früheren Version auf die aktuelle Version](../hybrid/how-to-upgrade-previous-version.md).
>

## <a name="active-directory-permissions"></a>Active Directory-Berechtigungen

Für das in Azure AD Connect angegebene Konto müssen die folgenden Elemente festgelegt sein, wenn SSPR möglich sein soll:

* **Zurücksetzen des Kennworts** 
* **Kennwort ändern** 
* **Schreibberechtigungen** auf `lockoutTime`
* **Schreibberechtigungen** auf `pwdLastSet`
* **Erweiterte Rechte** für eines der folgenden Elemente:
   * Das Stammobjekt von *jeder Domäne* in dieser Gesamtstruktur
   * Der Benutzerorganisationseinheiten, für die SSPR möglich sein soll

Wenn Sie nicht sicher sind, auf welches Konto sich das beschriebene Konto bezieht, öffnen Sie die Azure Active Directory Connect-Konfigurationsoberfläche, und wählen Sie die Option **Aktuelle Konfiguration anzeigen** aus. Das Konto, dem Sie Berechtigungen hinzufügen müssen, wird unter **Synchronisierte Verzeichnisse** angezeigt.

Wenn Sie diese Berechtigungen festlegen, kann das MA-Dienstkonto für jede Gesamtstruktur Kennwörter im Namen der Benutzerkonten innerhalb dieser Gesamtstruktur verwalten. 

> [!IMPORTANT]
> Wenn Sie diese Berechtigungen nicht gewähren, erhalten die Benutzer – obwohl das Zurückschreiben scheinbar ordnungsgemäß konfiguriert ist – Fehler bei dem Versuch, ihre lokalen Kennwörter über die Cloud zu verwalten.
>

> [!NOTE]
> Es kann bis zu einer Stunde oder länger dauern, bis diese Berechtigungen an alle Objekte in Ihrem Verzeichnis repliziert wurden.
>

Um die entsprechenden Berechtigungen für das Kennwortrückschreiben einzurichten, führen Sie die folgenden Schritte aus:

1. Öffnen Sie „Active Directory-Benutzer und -Computer“ mit einem Konto, das geeignete Berechtigungen für die Domänenverwaltung hat.
2. Vergewissern Sie sich, dass im Menü **Ansicht** die Option **Erweiterte Features** aktiviert ist.
3. Klicken Sie im linken Bereich mit der rechten Maustaste auf das Objekt, das den Stamm der Domäne repräsentiert, und wählen Sie **Eigenschaften** > **Sicherheit** > **Erweitert** aus.
4. Wählen Sie auf der Registerkarte **Berechtigungen** die Option **Hinzufügen** aus.
5. Wählen Sie das Konto aus, dem Berechtigungen zugewiesen werden sollen (vom Azure AD Connect-Setup).
6. Wählen Sie in der Dropdownliste **Gilt für** den Eintrag **Nachfolgerbenutzerobjekte** aus.
7. Aktivieren Sie unter **Berechtigungen** die Kontrollkästchen für folgende Optionen:
    * **Kennwort ändern**
    * **Zurücksetzen des Kennworts**
8. Aktivieren Sie unter **Eigenschaften** die Kontrollkästchen für folgende Optionen:
    * **lockoutTime schreiben**
    * **pwdLastSet schreiben**
9. Wählen Sie auf **Übernehmen/OK** aus, um die Änderungen zu übernehmen und alle geöffneten Dialogfelder zu schließen.

## <a name="next-steps"></a>Nächste Schritte

[Was ist die Kennwortrückschreibung?](concept-sspr-writeback.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Aktivieren des Kennwortrückschreibens in Azure AD Connect"
