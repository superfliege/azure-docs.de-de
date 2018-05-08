---
title: Konfigurieren von Sicherheitswarnungen | Microsoft Docs
description: Erfahren Sie, wie Sie Sicherheitswarnungen für die Erweiterung Azure Privileged Identity Management konfigurieren.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: users-groups-roles
ms.date: 06/06/2017
ms.author: curtand
ms.custom: pim
ms.openlocfilehash: f77567d35d3916ab1660134959d2d26916e08a0a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Konfigurieren von Sicherheitswarnungen in Azure AD Privileged Identity Management
## <a name="security-alerts"></a>Sicherheitswarnungen
Azure Privileged Identity Management (PIM) generiert bei verdächtigen oder nicht sicheren Aktivitäten in Ihrer Umgebung Warnungen. Wenn eine Warnung ausgelöst wird, wird sie im PIM-Dashboard angezeigt. Wählen Sie die Warnung aus, um einen Bericht mit allen Benutzern oder Rollen anzuzeigen, die die Warnung ausgelöst haben.

![Sicherheitswarnungen im PIM-Dashboard – Screenshot][1]

| Warnung | Severity | Trigger | Empfehlung |
| --- | --- | --- | --- |
| **Rollen werden außerhalb von PIM zugewiesen** |Hoch |Einem Benutzer wurde außerhalb der PIM-Benutzeroberfläche dauerhaft eine privilegierte Rolle zugewiesen. |Sehen Sie sich die Benutzer in der Liste an, und heben Sie dafür die Zuweisung zu privilegierten Rollen auf, die außerhalb von PIM zugewiesen wurden. |
| **Rollen werden zu häufig aktiviert** |Mittel |Es sind zu viele erneute Aktivierungen derselben Rolle innerhalb des in den Einstellungen festgelegten Zeitraums erfolgt. |Wenden Sie sich an den Benutzer, um festzustellen, warum die Rolle so häufig aktiviert wurde. Möglicherweise ist der Zeitraum zu kurz, um die Aufgaben abzuschließen, oder Benutzer verwenden Skripts, um eine Rolle automatisch zu aktivieren. Stellen Sie sicher, dass die Aktivierungsdauer für die jeweilige Rolle lang genug gewählt ist, damit diese Aufgaben durchgeführt werden können. |
| **Rollen erfordern für die Aktivierung keine Multi-Factor Authentication** |Mittel |Bei manchen Rollen ist MFA in den Einstellungen nicht aktiviert. |Wir fordern MFA für die meisten sehr privilegierten Rollen an, empfehlen jedoch dringend, dass Sie MFA für die Aktivierung aller Rollen aktivieren. |
| **Benutzer nutzen ihre privilegierten Rollen nicht** |Niedrig |Es gibt berechtigte Administratoren, die ihre Rollen längere Zeit nicht aktiviert haben. |Starten Sie eine Zugriffsüberprüfung, um die Benutzer zu ermitteln, die keinen Zugriff mehr benötigen. |
| **Es gibt zu viele globale Administratoren** |Niedrig |In den Einstellungen sind mehr globale Administratoren festgelegt als empfohlen. |Wenn Sie eine große Anzahl von globalen Administratoren haben, ist es wahrscheinlich, dass Benutzer mehr Berechtigungen als notwendig erhalten. Verlagern Sie Benutzer in weniger privilegierte Rollen, oder legen Sie einige von ihnen als für die Rolle berechtigt fest, statt sie permanent zuzuweisen. |

### <a name="severity"></a>Severity
* **Hoch**: Aufgrund einer Richtlinienverletzung ist eine sofortige Aktion erforderlich. 
* **Mittel**: Es ist keine sofortige Aktion erforderlich, aber es wird eine potenzielle Richtlinienverletzung angezeigt.
* **Niedrig**: Es ist keine sofortige Aktion erforderlich, aber es wird eine Richtlinienänderung vorgeschlagen.

## <a name="configure-security-alert-settings"></a>Konfigurieren der Einstellungen für Sicherheitswarnungen
Sie können einige der Sicherheitswarnungen in PIM so anpassen, dass sie in der Umgebung und mit den Sicherheitszielen genutzt werden können. Gehen Sie wie folgt vor, um das Blatt mit den Einstellungen aufzurufen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und wählen Sie auf dem Dashboard die Kachel **Azure AD Privileged Identity Management** aus.
2. Wählen Sie **Privilegierte Rollen verwalten** > **Einstellungen** > **Warnungseinstellungen** aus.
   
    ![Navigieren zu den Einstellungen für Sicherheitswarnungen][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>Warnung „Rollen werden zu häufig aktiviert“
Diese Warnung wird ausgelöst, wenn ein Benutzer mehrmals innerhalb eines bestimmten Zeitraums dieselbe privilegierte Rolle aktiviert. Sie können den Zeitraum und die Anzahl der Aktivierungen konfigurieren.

* **Zeitrahmen für Aktivierungsverlängerung**: Geben Sie in Tagen, Stunden, Minuten und Sekunden den Zeitraum an, den Sie verwenden möchten, um verdächtige Erneuerungen nachzuverfolgen.
* **Anzahl von Aktivierungsverlängerungen**: Geben Sie für den gewählten Zeitraum eine Anzahl von Aktivierungen zwischen 2 und 100 an, für die Sie eine Warnung ausgeben möchten. Sie können diese Einstellung mithilfe des Schiebereglers oder durch Eingabe einer Zahl in das Textfeld ändern.

### <a name="there-are-too-many-global-administrators-alert"></a>Warnung „Es gibt zu viele globale Administratoren“
In PIM wird diese Warnung ausgelöst, wenn zwei verschiedene Kriterien erfüllt sind, und Sie können beide konfigurieren. Erstens müssen Sie einen bestimmten Schwellenwert an globalen Administratoren erreichen. Zweitens muss ein bestimmter Anteil aller Rollenzuweisungen globale Administratoren betreffen. Wenn nur eines dieser Kriterien erfüllt ist, wird die Warnung nicht angezeigt.  

* **Mindestanzahl von globalen Administratoren**: Geben Sie eine Anzahl von globalen Administratoren zwischen 2 und 100 an, die Sie als eine unsichere Anzahl betrachten.
* **Prozentsatz von globalen Administratoren**: Geben Sie den Prozentsatz von Administratoren, bei denen es sich um globale Administratoren handelt, zwischen 0 % und 100 % an, der in Ihrer Umgebung als unsicher gilt.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Warnung „Administratoren verwenden ihre privilegierten Rollen nicht“
Diese Warnung wird auslöst, wenn ein Benutzer für eine bestimmte Zeit eine Rolle nicht aktiviert hat.

* **Anzahl Tage**: Geben Sie eine Anzahl von Tagen zwischen 0 und 100 an, die ein Benutzer eine Rolle nicht aktivieren muss.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
