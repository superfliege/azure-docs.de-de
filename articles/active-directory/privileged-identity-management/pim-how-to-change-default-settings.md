---
title: Konfigurieren von Einstellungen für Azure AD-Verzeichnisrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Einstellungen für Azure AD-Verzeichnisrollen in Azure AD Privileged Identity Management (PIM) konfigurieren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 2d7226f18eb922eaba3c8184656560c33202ef56
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665433"
---
# <a name="configure-azure-ad-directory-role-settings-in-pim"></a>Konfigurieren von Einstellungen für Azure AD-Verzeichnisrollen in PIM

Ein Administrator für privilegierte Rollen kann Azure AD Privileged Identity Management (PIM) in der Organisation anpassen und hierbei auch die Art und Weise ändern, in der ein Benutzer eine berechtigte Rollenzuweisung aktiviert.

## <a name="open-role-settings"></a>Öffnen von Rolleneinstellungen

Führen Sie diese Schritte aus, um die Einstellungen für eine Azure AD-Verzeichnisrolle zu öffnen.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure AD-Verzeichnisrollen**.

1. Klicken Sie auf **Einstellungen**.

    ![Azure AD-Verzeichnisrollen – Einstellungen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Klicken Sie auf **Rollen**.

1. Klicken Sie auf die Rolle, deren Einstellungen Sie konfigurieren möchten.

    ![Azure AD-Verzeichnisrollen – Einstellungen: Rollen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    Auf der Einstellungsseite für jede Rolle können Sie verschiedene Einstellungen konfigurieren. Diese Einstellungen wirken sich nur auf Benutzer aus, die über **berechtigte** Zuweisungen verfügen, nicht über **permanente** Zuweisungen.

## <a name="activations"></a>Aktivierungen

Legen Sie mit dem Schieberegler **Aktivierungen** die maximale Zeit in Stunden fest, die eine Rolle aktiv bleibt, bevor sie abläuft. Dieser Wert kann zwischen 1 und 72 Stunden liegen.

## <a name="notifications"></a>Benachrichtigungen

Mit dem Schalter **Benachrichtigungen** können Sie angeben, ob das System E-Mails an Administratoren sendet, um zu bestätigen, dass sie eine Rolle aktiviert haben. Dies kann für die Erkennung von nicht autorisierten oder unzulässigem Aktivierungen nützlich sein.

## <a name="incidentrequest-ticket"></a>Incident/Ticket anfordern

Mit dem Schalter **Incident/Ticket anfordern** können Sie angeben, ob berechtigte Administratoren eine Ticketnummer angeben müssen, wenn sie ihre Rolle aktivieren. Dies kann nützlich sein, wenn Sie Rollenzugriffe überprüfen.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Mit dem Schalter **Multi-Factor Authentication** können Sie angeben, ob Benutzer ihre Identität mit MFA verifizieren müssen, bevor sie ihre Rollen aktivieren können. Sie müssen dies nur einmal pro Sitzung verifizieren, nicht jedes Mal, wenn sie eine Rolle aktivieren. Beachten Sie zwei Tipps, wenn Sie die MFA aktivieren:

* Benutzer, die Microsoft-Konten als E-Mail-Adressen verwenden (in der Regel @outlook.com, aber nicht immer), können sich nicht für Azure MFA registrieren. Wenn Sie Benutzern mit Microsoft-Konten Rollen zuweisen möchten, sollten Sie sie zu permanenten Administratoren machen oder MFA für diese Rolle deaktivieren.
* Sie können MFA für sehr privilegierte Rollen für Azure AD und Office 365 nicht deaktivieren. Dies ist ein Sicherheitsfeature, da diese Rollen sorgfältig geschützt werden sollten:  
  
  * Anwendungsadministrator
  * Serveradministrator des Anwendungsproxys
  * Abrechnungsadministrator  
  * Complianceadministrator  
  * CRM-Dienstadministrator
  * Genehmigende Person für den LockBox-Kundenzugriff
  * Verzeichnisautor  
  * Exchange-Administrator  
  * Globaler Administrator
  * Intune-Dienstadministrator
  * Postfachadministrator  
  * Partnersupport der Ebene 1  
  * Partnersupport der Ebene 2  
  * Administrator für privilegierte Rollen
  * Sicherheitsadministrator  
  * SharePoint-Administrator  
  * Skype for Business-Administrator  
  * Benutzerkontoadministrator  

Weitere Informationen finden Sie unter [Multi-Factor Authentication (MFA) und PIM](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Genehmigung anfordern

Wenn Sie für die Aktivierung einer Rolle eine Genehmigung anfordern möchten, gehen Sie wie folgt vor.

1. Legen Sie den Schalter **Genehmigung anfordern** auf **Aktiviert** fest. Der Bereich wird erweitert und zeigt Optionen zum Auswählen von genehmigenden Personen an.

    ![Azure AD-Verzeichnisrollen – Einstellungen: Genehmigung anfordern](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Wenn Sie **KEINE** genehmigenden Personen angeben, werden Administratoren mit privilegierten Rollen als standardmäßige genehmigende Personen festgelegt. Administratoren mit privilegierten Rollen sind erforderlich, um **ALLE** Aktivierungsanforderungen für diese Rolle zu genehmigen.

1. Um genehmigende Personen anzugeben, klicken Sie auf **Genehmigende Personen auswählen**.

    ![Azure AD-Verzeichnisrollen – Einstellungen: Genehmigung anfordern](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Wählen Sie eine oder mehrere genehmigende Personen aus, und klicken Sie anschließend auf **Auswählen**. Sie können Benutzer oder Gruppen auswählen. Es werden mindestens zwei genehmigende Personen empfohlen. Eine Genehmigung für die eigene Rolle ist nicht zulässig.

    Ihre Auswahl wird in der Liste der ausgewählten genehmigenden Personen angezeigt.

1. Wenn Sie alle gewünschten Rolleneinstellungen angegeben haben, klicken Sie auf **Speichern**, um Ihre Änderungen zu speichern.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure AD-Verzeichnisrollen in PIM](pim-how-to-add-role-to-user.md)
- [Konfigurieren von Sicherheitswarnungen für Azure AD-Verzeichnisrollen in PIM](pim-how-to-configure-security-alerts.md)
