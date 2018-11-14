---
title: Konfigurieren von Sicherheitswarnungen für Azure AD-Verzeichnisrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Sicherheitswarnungen für Azure AD-Verzeichnisrollen in Azure AD Privileged Identity Management (PIM) konfigurieren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 11/01/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e7204c223681b9a33c439b0d9fc653167422384a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51011696"
---
# <a name="configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Konfigurieren von Sicherheitswarnungen für Azure AD-Verzeichnisrollen in PIM

Azure AD Privileged Identity Management (PIM) generiert bei verdächtigen oder nicht sicheren Aktivitäten in Ihrer Umgebung Warnungen. Wenn eine Warnung ausgelöst wird, wird sie im PIM-Dashboard angezeigt. Wählen Sie die Warnung aus, um einen Bericht mit allen Benutzern oder Rollen anzuzeigen, die die Warnung ausgelöst haben.

![PIM-Sicherheitswarnungen – Screenshot](./media/pim-how-to-configure-security-alerts/pim-directory-alerts.png)

## <a name="security-alerts"></a>Sicherheitswarnungen

Dieser Abschnitt enthält alle Sicherheitswarnungen für Verzeichnisrollen, sowie Informationen zur Behebung und Vermeidung. Der Schweregrad hat folgende Bedeutung:

* **Hoch**: Aufgrund einer Richtlinienverletzung ist eine sofortige Aktion erforderlich.
* **Mittel**: Es ist keine sofortige Aktion erforderlich, aber es wird eine potenzielle Richtlinienverletzung angezeigt.
* **Niedrig**: Es ist keine sofortige Aktion erforderlich, aber es wird eine Richtlinienänderung vorgeschlagen.

### <a name="roles-are-being-assigned-outside-of-pim"></a>Rollen werden außerhalb von PIM zugewiesen

| | |
| --- | --- |
| **Severity** | Hoch |
| **Warum erhalte ich diese Warnung?** | Privilegierte Rollenzuweisungen außerhalb von PIM werden nicht ordnungsgemäß überwacht und können auf einen aktiven Angriff hinweisen. |
| **Wie behebe ich das Problem?** | Überprüfen Sie die Benutzer in der Liste, und entfernen Sie die Zuweisung zu privilegierten Rollen, die außerhalb von PIM zugewiesen wurden. |
| **Prävention** | Untersuchen Sie, wo Benutzern außerhalb des PIM privilegierte Rollen zugewiesen werden, und verbieten Sie zukünftige Zuweisungen, die von dort stammen. |
| **Aktionen zur Risikominderung im Portal** | Entfernt das Konto aus der privilegierte Rolle. |

### <a name="potential-stale-accounts-in-a-privileged-role"></a>Potenzielle veraltete Konten in einer privilegierten Rolle

| | |
| --- | --- |
| **Severity** | Mittel |
| **Warum erhalte ich diese Warnung?** | Konten, die ihr Kennwort in letzter Zeit nicht geändert haben, können Dienst- oder freigegebene Konten sein, die nicht gepflegt werden. Diese Konten in privilegierten Rollen sind anfällig für Angriffe. |
| **Wie behebe ich das Problem?** | Überprüfen Sie die Konten in der Liste. Wenn kein Zugriff darauf mehr erforderlich ist, entfernen sie aus ihren privilegierten Rollen. |
| **Prävention** | Stellen Sie sicher, dass die starken Kennwörter für freigegebenen Konten rotiert werden, wenn sich die Benutzer ändern, die das Kennwort kennen. </br>Überprüfen Sie regelmäßig Konten mit privilegierten Rollen mithilfe von Zugriffsprüfungen und entfernen Sie nicht mehr benötigte Rollenzuweisungen. |
| **Aktionen zur Risikominderung im Portal** | Entfernt das Konto aus der privilegierte Rolle. |

### <a name="users-arent-using-their-privileged-roles"></a>Benutzer nutzen ihre privilegierten Rollen nicht

| | |
| --- | --- |
| **Severity** | Niedrig |
| **Warum erhalte ich diese Warnung?** | Benutzer, denen nicht benötigte privilegierte Rollen zugewiesen wurden, erhöhen die Möglichkeit eines Angriffs. Es ist auch einfacher für Angreifer, auf Konten, die nicht aktiv genutzt werden, unbemerkt zu bleiben. |
| **Wie behebe ich das Problem?** | Überprüfen Sie die Benutzer in der Liste, und entfernen Sie die Zuweisung zu privilegierten Rollen, die sie nicht benötigen. |
| **Prävention** | Weisen Sie privilegierte Rollen Benutzern zu, die eine geschäftliche Rechtfertigung haben. </br>Planen Sie regelmäßige Zugriffsüberprüfungen, um sicherzustellen, dass die Benutzer ihren Zugriff weiterhin benötigen. |
| **Aktionen zur Risikominderung im Portal** | Entfernt das Konto aus der privilegierte Rolle. |
| **Trigger** | Wird auslöst, wenn ein Benutzer für eine bestimmte Zeit eine Rolle nicht aktiviert hat. |
| **Anzahl von Tagen** | Diese Einstellung gibt die Anzahl von Tagen zwischen 0 und 100 an, die ein Benutzer eine Rolle nicht aktivieren muss.|

### <a name="there-are-too-many-global-administrators"></a>Es gibt zu viele globale Administratoren

| | |
| --- | --- |
| **Severity** | Niedrig |
| **Warum erhalte ich diese Warnung?** | Globaler Administrator ist die höchste privilegierte Rolle. Wenn ein globaler Administrator gefährdet ist, erhält der Angreifer Zugriff auf alle seine Berechtigungen, sodass Ihr gesamtes System gefährdet ist. |
| **Wie behebe ich das Problem?** | Überprüfen Sie die Benutzer in der Liste, und entfernen Sie alle, die nicht unbedingt die Rolle eines globalen Administrators benötigen. </br>Weisen Sie diesen Benutzern weniger privilegierte Rollen zu. |
| **Prävention** | Weisen Sie Benutzern die am wenigsten privilegierte Rolle zu, die sie benötigen. |
| **Aktionen zur Risikominderung im Portal** | Entfernt das Konto aus der privilegierte Rolle. |
| **Trigger** | Wird ausgelöst, wenn zwei verschiedene Kriterien erfüllt sind, und Sie können beide konfigurieren. Erstens müssen Sie einen bestimmten Schwellenwert an globalen Administratoren erreichen. Zweitens muss ein bestimmter Anteil aller Rollenzuweisungen globale Administratoren betreffen. Wenn nur eines dieser Kriterien erfüllt ist, wird die Warnung nicht angezeigt. |
| **Mindestanzahl globaler Administratoren** | Diese Einstellung gibt die Anzahl von globalen Administratoren zwischen 2 und 100 an, die Sie als eine unsichere Anzahl betrachten. |
| **Prozentsatz von globalen Administratoren** | Diese Einstellung gibt den Prozentsatz von Administratoren an, bei denen es sich um globale Administratoren handelt, zwischen 0 % und 100 % an, der in Ihrer Umgebung als unsicher gilt. |

### <a name="roles-are-being-activated-too-frequently"></a>Rollen werden zu häufig aktiviert

| | |
| --- | --- |
| **Severity** | Niedrig |
| **Warum erhalte ich diese Warnung?** | Mehrere Aktivierungen für dieselbe privilegierte Rolle durch denselben Benutzer sind ein Zeichen für einen Angriff. |
| **Wie behebe ich das Problem?** | Überprüfen Sie die Benutzer in der Liste, und stellen Sie sicher, dass die [Aktivierungsdauer](pim-how-to-change-default-settings.md) für die privilegierte Rolle lang genug gewählt ist, damit diese Aufgaben durchgeführt werden können. |
| **Prävention** | Stellen Sie sicher, dass die [Aktivierungsdauer](pim-how-to-change-default-settings.md) für privilegierte Rollen lang genug gewählt ist, damit Benutzer diese Aufgaben durchführen können.</br>[Erzwingen Sie MFA](pim-how-to-change-default-settings.md) für privilegierte Rollen, deren Konten von mehreren Administratoren gemeinsam verwendet werden. |
| **Aktionen zur Risikominderung im Portal** | N/V |
| **Trigger** | Wird ausgelöst, wenn ein Benutzer mehrmals innerhalb eines bestimmten Zeitraums dieselbe privilegierte Rolle aktiviert. Sie können den Zeitraum und die Anzahl der Aktivierungen konfigurieren. |
| **Zeitrahmen für Aktivierungsverlängerung** | Diese Einstellung gibt den Zeitraum in Tagen, Stunden, Minuten und Sekunden an, den Sie verwenden möchten, um verdächtige Verlängerungen nachzuverfolgen. |
| **Anzahl von Aktivierungsverlängerungen** | Diese Einstellung gibt im gewählten Zeitraum die Anzahl von Aktivierungen zwischen 2 und 100 an, für die Sie eine Warnung ausgeben möchten. Sie können diese Einstellung mithilfe des Schiebereglers oder durch Eingabe einer Zahl in das Textfeld ändern. |

### <a name="roles-dont-require-mfa-for-activation"></a>Für Rollen ist zur Aktivierung keine MFA erforderlich

| | |
| --- | --- |
| **Severity** | Niedrig |
| **Warum erhalte ich diese Warnung?** | Ohne MFA können kompromittierte Benutzer privilegierte Rollen aktivieren. |
| **Wie behebe ich das Problem?** | Überprüfen Sie die Liste der Rollen, und [fordern Sie MFA für jede Rolle an](pim-how-to-change-default-settings.md). |
| **Prävention** | [Fordern Sie MFA](pim-how-to-change-default-settings.md) für jede Rolle an.  |
| **Aktionen zur Risikominderung im Portal** | Damit ist MFA für die Aktivierung der privilegierten Rolle erforderlich. |

## <a name="configure-security-alert-settings"></a>Konfigurieren der Einstellungen für Sicherheitswarnungen

Sie können einige der Sicherheitswarnungen in PIM so anpassen, dass sie in der Umgebung und mit den Sicherheitszielen genutzt werden können. Gehen Sie wie folgt vor, um Einstellungen für Sicherheitswarnungen zu öffnen:

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure AD-Rollen**.

1. Klicken Sie auf **Einstellungen** und dann auf **Warnung**.

    ![Navigieren zu den Einstellungen für Sicherheitswarnungen](./media/pim-how-to-configure-security-alerts/settings-alerts.png)

1. Klicken Sie auf den Name einer Warnung, um die Einstellung für die Warnung zu konfigurieren.

    ![Einstellungen zu Sicherheitswarnungen](./media/pim-how-to-configure-security-alerts/security-alert-settings.png)

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Einstellungen für Azure AD-Verzeichnisrollen in PIM](pim-how-to-change-default-settings.md)
