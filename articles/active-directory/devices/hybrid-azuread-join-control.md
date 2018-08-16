---
title: Konfigurieren von in Azure Active Directory eingebundenen Hybridgeräten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure Active Directory eingebundene Hybridgeräte konfigurieren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 9ffc84009adfca60e9ae6b188b65b15e874e7d9c
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622169"
---
# <a name="how-to-control-the-hybrid-azure-ad-join-of-your-devices"></a>Steuern der Azure AD-Hybrideinbindung für Ihre Geräte

Die Azure AD-Hybrideinbindung ist ein Prozess für die automatische Registrierung Ihrer lokalen in die Domäne eingebundenen Geräte bei Azure AD. In einigen Fällen sollten Sie nicht alle Ihre Geräte automatisch registrieren. Dies gilt beispielsweise während des anfänglichen Rollouts, um sicherzustellen, dass alles wie erwartet funktioniert.

Dieser Artikel enthält Anleitungen zum Steuern der Azure AD-Hybrideinbindung Ihrer Geräte. 


## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie mit Folgendem vertraut sind:

-  [Einführung in die Geräteverwaltung in Azure Active Directory](../device-management-introduction.md)
 
-  [Planen der Implementierung einer Azure Active Directory-Hybrideinbindung](hybrid-azuread-join-plan.md)

-  Konfigurieren der Azure Active Directory-Hybrideinbindung für [verwaltete Domänen](hybrid-azuread-join-managed-domains.md) oder [Verbunddomänen](hybrid-azuread-join-federated-domains.md)



## <a name="control-windows-current-devices"></a>Steuern aktueller Windows-Geräte

Für Geräte, auf denen das Windows-Desktopbetriebssystem ausgeführt wird, wird die Version Windows 10 Anniversary Update (Version 1607) oder höher unterstützt. Es wird empfohlen, ein Upgrade auf die aktuelle Version von Windows 10 durchzuführen.

Alle aktuellen Windows-Geräte werden beim Gerätestart oder der Benutzeranmeldung automatisch bei Azure AD registriert. Sie können dieses Verhalten entweder mit einem Gruppenrichtlinienobjekt (GPO) oder System Center Configuration Manager steuern.

Zum Steuern aktueller Windows-Geräte müssen Sie: 

1.  **Auf allen Geräten**: Die automatische Geräteregistrierung deaktivieren.
2.  **Auf ausgewählten Geräten**: Die automatische Geräteregistrierung aktivieren.

Wenn Sie sichergestellt haben, dass alles wie erwartet funktioniert, können Sie die automatische Geräteregistrierung für alle Geräte erneut aktivieren.



## <a name="group-policy-object"></a>Gruppenrichtlinienobjekt 

Sie können das Verhalten der Geräteregistrierung Ihrer Geräte durch das Bereitstellen der folgenden GPO steuern: **In die Domäne eingebundene Computer als Geräte registrieren**

**Festlegen des GPO**:

1.  Öffnen Sie den **Server-Manager**, und navigieren Sie zu **Tools\>Gruppenrichtlinienverwaltung**.

2.  Wechseln Sie zum Domänenknoten, der der Domäne entspricht, in der Sie die automatische Registrierung (de)aktivieren möchten.

3.  Klicken Sie mit der rechten Maustaste auf **Gruppenrichtlinienobjekte**, und wählen Sie dann **Neu** aus.

4.  Geben Sie einen Namen (z.B. *Azure AD-Hybrideinbindung*) für das Gruppenrichtlinienobjekt ein. 

5.  Klicken Sie auf **OK**.

6.  Klicken Sie mit der rechten Maustaste auf Ihr GPO, und wählen Sie dann **Bearbeiten** aus.

7.  Navigieren Sie zu **Computerkonfiguration \> Richtlinien \> Administrative Vorlagen \> Windows-Komponenten \> Geräteregistrierung**. 

8.  Klicken Sie mit der rechten Maustaste auf **In die Domäne eingebundene Computer als Geräte registrieren**, und wählen Sie dann die Option **Bearbeiten**.

    > [!NOTE] 
    > In früheren Versionen der Gruppenrichtlinien-Verwaltungskonsole hatte diese Gruppenrichtlinienvorlage einen anderen Namen. Wenn Sie eine frühere Version der Konsole verwenden, navigieren Sie zu **Computerkonfiguration \> Richtlinien \> Administrative Vorlagen \> Windows-Komponenten \> Arbeitsbereichverknüpfung \> Clientcomputer automatisch in Arbeitsbereich einbinden**. 

9.  Wählen Sie eine der folgenden Einstellungen aus, und klicken Sie dann auf **Anwenden**:

    - **Deaktiviert** – zum Verhindern der automatischen Geräteregistrierung
    - **Aktiviert** – zum Aktivieren der automatischen Geräteregistrierung

10. Klicken Sie auf **OK**.

Sie müssen das GPO mit einem Speicherort Ihrer Wahl verknüpfen. Beispiel: Zum Festlegen dieser Richtlinie für alle aktuell in die Domäne eingebundenen Geräten in Ihrer Organisation verknüpfen Sie das GPO mit der Domäne. Für eine gesteuerte Bereitstellung legen Sie diese Richtlinie auf aktuell in die Domäne eingebundene Windows-Geräte fest, die zu einer Organisationseinheit oder Sicherheitsgruppe gehören.

### <a name="configuration-manager-controlled-deployment"></a>Gesteuerte Bereitstellung über den Konfigurations-Manager 

Sie können das Verhalten der Geräteregistrierung Ihrer aktuellen-Geräten steuern, indem Sie die folgende Clienteinstellung konfigurieren: **Neue, in die Domäne eingebundene Windows 10-Geräte automatisch bei Azure Active Directory registrieren **


**Festlegen der Clienteinstellung**:

1.  Öffnen Sie den **Konfigurations-Manager**, und navigieren Sie zu **Cloud Services**.

2.  Wählen Sie unter **Geräteeinstellungen** eine der folgenden Einstellungen für **Neue, in die Domäne eingebundene Windows 10-Geräte automatisch bei Azure Active Directory registrieren** aus:

    - **Nein** – zum Verhindern der automatischen Geräteregistrierung
    - **Ja** – zum Aktivieren der automatischen Geräteregistrierung


3.  Klicken Sie auf **OK**.
    

Sie müssen diese Clienteinstellung mit einem Speicherort Ihrer Wahl verknüpfen. Beispiel: Zum Konfigurieren dieser Clienteinstellung für alle aktuellen Windows-Geräte in Ihrer Organisation verknüpfen Sie die Clienteinstellung mit der Domäne. Für eine gesteuerte Bereitstellung können Sie die Clienteinstellung für aktuell in die Domäne eingebundene Windows-Geräte konfigurieren, die zu einer Organisationseinheit oder Sicherheitsgruppe gehören.

> [!Important]
> Die oben genannte Konfiguration schließt zwar die in vorhandenen in die Domäne eingebundenen Windows 10-Geräte ein, allerdings besteht weiterhin die Möglichkeit, dass neue Geräte, die in die Domäne eingebunden werden, eine Hybrid-Azure AD-Einbindung aufgrund der etwaigen Verzögerung bei der tatsächlichen Anwendung der Gruppenrichtlinie oder Configuration Manager-Einstellungen für das neue Windows 10-Gerät durchführen, das in die Domäne eingebunden wird. Um dies zu verhindern, wird empfohlen, ein neues sysprep-Image (als Beispiel für eine Bereitstellungsmethode verwendet) über ein Gerät zu erstellen, das bislang noch nie in Hybrid-Azure AD eingebunden war und auf dem bereits die oben genannte Gruppenrichtlinieneinstellung oder Configuration Manager-Clienteinstellung angewendet wurde. Darüber hinaus müssen Sie das neue Image für die Bereitstellung neuer Computer verwenden, die in die Domäne Ihrer Organisation eingebunden werden. 

## <a name="control-windows-down-level-devices"></a>Steuern kompatibler Windows-Geräte

Zum Registrieren von kompatiblen Windows-Geräten müssen Sie dieses Windows Installer-Paket (.msi) über die Seite [Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) (Microsoft Workplace Join für Computer mit einem anderen Betriebssystem als Windows 10) aus dem Download Center herunterladen und installieren.

Sie können das Paket mithilfe eines Softwareverteilungssystems wie System Center Configuration Manager bereitstellen. Das Paket unterstützt die Standardoptionen für die Installation im Hintergrund unter Verwendung des quiet-Parameters. [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager) Current Branch bietet zusätzliche Vorteile gegenüber früheren Versionen, z.B. die Möglichkeit zur Nachverfolgung abgeschlossener Registrierungen.

Das Installationsprogramm erstellt einen geplanten Task auf dem System, der im Kontext des Benutzers ausgeführt wird. Der Task wird ausgelöst, wenn sich der Benutzer bei Windows anmeldet. Nach der Authentifizierung durch Azure AD verknüpft die Aufgabe das Gerät unter Verwendung der Benutzeranmeldeinformationen mit Azure AD.


Zum Steuern der Registrierung dürfen Sie das Windows Installer-Paket nur für eine ausgewählte Gruppe von kompatiblen Windows-Geräten bereitstellen. Wenn Sie sichergestellt haben, dass alles wie erwartet funktioniert, können Sie den Rollout des Pakets für alle kompatiblen Geräte durchführen.


## <a name="next-steps"></a>Nächste Schritte

* [Einführung in die Geräteverwaltung in Azure Active Directory](../device-management-introduction.md)



