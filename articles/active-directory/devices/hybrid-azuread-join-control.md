---
title: Steuern von Azure AD Hybrid Join für Ihre Geräte | Microsoft-Dokumentation
description: Erfahren Sie mehr zum Steuern von Azure AD Hybrid Join für Ihre Geräte in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 52f3859edcc8a524c56f91ad2a5346a69ea534ed
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103962"
---
# <a name="control-the-hybrid-azure-ad-join-of-your-devices"></a>Steuern der Azure AD-Hybrideinbindung für Ihre Geräte

Azure Active Directory (Azure AD) Hybrid Join ist ein Prozess für die automatische Registrierung Ihrer lokalen in die Domäne eingebundenen Geräte bei Azure AD. Es kann auch vorkommen, dass Sie nicht alle Ihre Geräte automatisch registrieren möchten. Dies gilt beispielsweise während des anfänglichen Rollouts, um sicherzustellen, dass alles wie erwartet funktioniert.

Dieser Artikel enthält Anleitungen zum Steuern von Azure AD Hybrid Join für Ihre Geräte. 


## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie mit Folgendem vertraut sind:

-  [Einführung in die Geräteverwaltung in Azure Active Directory](../device-management-introduction.md)
 
-  [Planen der Implementierung von Azure Active Directory (Azure AD) Hybrid Join](hybrid-azuread-join-plan.md)

-  [Konfigurieren von Azure Active Directory Hybrid Join für verwaltete Domänen](hybrid-azuread-join-managed-domains.md) oder [Konfigurieren von Azure Active Directory Hybrid Join für Verbunddomänen](hybrid-azuread-join-federated-domains.md)



## <a name="control-windows-current-devices"></a>Steuern aktueller Windows-Geräte

Für Geräte, auf denen das Windows-Desktopbetriebssystem ausgeführt wird, wird die Version Windows 10 Anniversary Update (Version 1607) oder höher unterstützt. Es wird empfohlen, ein Upgrade auf die aktuelle Version von Windows 10 durchzuführen.

Alle aktuellen Windows-Geräte werden beim Gerätestart oder der Benutzeranmeldung automatisch bei Azure AD registriert. Sie können dieses Verhalten entweder mit einem Gruppenrichtlinienobjekt (GPO) oder System Center Configuration Manager steuern.

Zum Steuern aktueller Windows-Geräte müssen Sie: 


1.  **Für alle Geräte**: Deaktivieren der automatischen Geräteregistrierung.
2.  **Für ausgewählte Geräte**: Aktivieren der automatischen Geräteregistrierung.

Wenn Sie sichergestellt haben, dass alles wie erwartet funktioniert, können Sie die automatische Geräteregistrierung für alle Geräte erneut aktivieren.



### <a name="group-policy-object"></a>Gruppenrichtlinienobjekt 

Sie können das Verhalten der Geräteregistrierung Ihrer Geräte durch das Bereitstellen der folgenden GPO steuern: **In die Domäne eingebundene Computer als Geräte registrieren**.

Festlegen des GPO:

1.  Öffnen Sie den **Server-Manager**, und navigieren Sie zu **Tools** > **Gruppenrichtlinienverwaltung**.

2.  Wechseln Sie zum Domänenknoten, der der Domäne entspricht, in der Sie die automatische Registrierung (de)aktivieren möchten.

3.  Klicken Sie mit der rechten Maustaste auf **Gruppenrichtlinienobjekte**, und wählen Sie dann **Neu** aus.

4.  Geben Sie einen Namen (z.B. **Azure AD Hybrid Join**) für das Gruppenrichtlinienobjekt ein. 

5.  Klicken Sie auf **OK**.

6.  Klicken Sie mit der rechten Maustaste auf Ihr GPO, und wählen Sie dann **Bearbeiten** aus.

7.  Navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Geräteregistrierung**. 

8.  Klicken Sie mit der rechten Maustaste auf **In die Domäne eingebundene Computer als Geräte registrieren**, und wählen Sie dann die Option **Bearbeiten**.

    > [!NOTE] 
    > In früheren Versionen der Gruppenrichtlinien-Verwaltungskonsole hatte diese Gruppenrichtlinienvorlage einen anderen Namen. Wenn Sie eine frühere Version der Konsole verwenden, navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Workplace Join** > **Clientcomputer automatisch in Arbeitsbereich einbinden**. 

9.  Wählen Sie eine der folgenden Einstellungen und dann **Anwenden** aus:

    - **Deaktiviert**: Zum Verhindern der automatischen Geräteregistrierung.
    - **Enabled**: Zum Aktivieren der automatischen Geräteregistrierung.

10. Klicken Sie auf **OK**.

Sie müssen das GPO mit einem Speicherort Ihrer Wahl verknüpfen. Beispiel: Zum Festlegen dieser Richtlinie für alle aktuell in die Domäne eingebundenen Geräten in Ihrer Organisation verknüpfen Sie das GPO mit der Domäne. Für eine gesteuerte Bereitstellung legen Sie diese Richtlinie auf aktuell in die Domäne eingebundene Windows-Geräte fest, die zu einer Organisationseinheit oder Sicherheitsgruppe gehören.

### <a name="configuration-manager-controlled-deployment"></a>Gesteuerte Bereitstellung über den Konfigurations-Manager 

Sie können das Verhalten der Geräteregistrierung Ihrer aktuellen Geräte durch das Konfigurieren der folgenden Clienteinstellung steuern: **Neue, in die Domäne eingebundene Windows 10-Geräte automatisch bei Azure Active Directory registrieren**.

Konfigurieren der Clienteinstellung:

1.  Öffnen Sie den **Konfigurations-Manager**, und navigieren Sie zu **Cloud Services**.

2.  Wählen Sie unter **Geräteeinstellungen** eine der folgenden Einstellungen für **Neue, in die Domäne eingebundene Windows 10-Geräte automatisch bei Azure Active Directory registrieren** aus:

    - **Nein**: Zum Verhindern der automatischen Geräteregistrierung.
    - **Ja**: Zum Aktivieren der automatischen Geräteregistrierung.


3.  Klicken Sie auf **OK**.
    

Sie müssen diese Clienteinstellung mit einem Speicherort Ihrer Wahl verknüpfen. Beispiel: Zum Konfigurieren dieser Clienteinstellung für alle aktuellen Windows-Geräte in Ihrer Organisation verknüpfen Sie die Clienteinstellung mit der Domäne. Für eine gesteuerte Bereitstellung können Sie die Clienteinstellung für aktuell in die Domäne eingebundene Windows-Geräte konfigurieren, die zu einer Organisationseinheit oder Sicherheitsgruppe gehören.

> [!Important]
> Die vorstehende Konfiguration schließt zwar die in vorhandenen in die Domäne eingebundenen Windows 10-Geräte ein, neue Geräte, die in die Domäne eingebunden werden, versuchen möglicherweise trotzdem, aufgrund der etwaigen Verzögerung bei der Anwendung der Gruppenrichtlinie oder Configuration Manager-Einstellungen auf den Geräten ein Azure AD Hybrid Join abzuschließen. 
>
> Um dies zu vermeiden, empfehlen wir Ihnen, ein neues Sysprep-Image zu erstellen (als Beispiel für eine Bereitstellungsmethode). Erstellen Sie es von einem Gerät, für das Azure AD Hybrid Join noch nicht durchgeführt wurde, und beim den die Gruppenrichtlinien- oder Configuration Manager-Clienteinstellung bereits angewendet wurde. Darüber hinaus müssen Sie das neue Image für die Bereitstellung neuer Computer verwenden, die in die Domäne Ihrer Organisation eingebunden werden. 

## <a name="control-windows-down-level-devices"></a>Steuern kompatibler Windows-Geräte

Zum Registrieren von kompatiblen Windows-Geräten müssen Sie dieses Windows Installer-Paket (.msi) über die Seite [Microsoft Workplace Join für Computer mit einem anderen Betriebssystem als Windows 10](https://www.microsoft.com/download/details.aspx?id=53554) aus dem Download Center herunterladen und installieren.

Sie können das Paket mithilfe eines Softwareverteilungssystems wie [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager) bereitstellen. Das Paket unterstützt die Standardoptionen für die Installation im Hintergrund unter Verwendung des quiet-Parameters. Configuration Manager Current Branch bietet zusätzliche Vorteile gegenüber früheren Versionen, z.B. die Möglichkeit zur Nachverfolgung abgeschlossener Registrierungen.

Das Installationsprogramm erstellt einen geplanten Task auf dem System, der im Kontext des Benutzers ausgeführt wird. Der Task wird ausgelöst, wenn sich der Benutzer bei Windows anmeldet. Nach der Authentifizierung durch Azure AD verknüpft die Aufgabe das Gerät unter Verwendung der Benutzeranmeldeinformationen mit Azure AD.

Zum Steuern der Registrierung dürfen Sie das Windows Installer-Paket nur für eine ausgewählte Gruppe von kompatiblen Windows-Geräten bereitstellen. Wenn Sie sichergestellt haben, dass alles wie erwartet funktioniert, können Sie den Rollout des Pakets für alle kompatiblen Geräte durchführen.


## <a name="next-steps"></a>Nächste Schritte

* [Einführung in die Geräteverwaltung in Azure Active Directory](../device-management-introduction.md)



