---
title: "Azure AD SSPR über den Windows 10-Anmeldebildschirm | Microsoft-Dokumentation"
description: "Konfigurieren von Windows 10-Anmeldebildschirm, Azure AD-Kennwortzurücksetzung und „PIN vergessen“"
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
ms.topic: get-started-article
ms.date: 11/08/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 9f7fdb97fd121eecf9e7b2f4edc1b568c8114869
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="azure-ad-password-reset-from-the-login-screen"></a>Azure AD-Kennwortzurücksetzung über den Anmeldebildschirm

Sie haben die Azure AD-Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) bereits bereitgestellt, aber die Benutzer wenden sich weiterhin per Telefon an den Helpdesk, wenn sie ihr Kennwort vergessen haben. Sie rufen den Helpdesk an, weil sie keinen Webbrowser zum Zugreifen auf SSPR nutzen können.

Mit dem neuen Windows 10 Fall Creators Update wird Benutzern mit in Azure AD eingebundenen Geräten auf dem Anmeldebildschirm der Link „Kennwort zurücksetzen“ angezeigt. Wenn die Benutzer auf diesen Link klicken, gelangen sie zur vertrauten Benutzeroberfläche für die Self-Service-Kennwortzurücksetzung. 

Die folgenden Voraussetzungen müssen erfüllt sein, damit Benutzer ihr Azure AD-Kennwort über den Windows 10-Anmeldebildschirm zurücksetzen können:

* Windows 10, Version 1709, oder neuerer Client, der in die Azure AD-Domäne eingebunden ist.
* Die Azure AD-Self-Service-Kennwortzurücksetzung muss aktiviert sein.
* Führen Sie das Konfigurieren und Bereitstellen der Einstellung zum Aktivieren des Links „Kennwort zurücksetzen“ durch, indem Sie eine der folgenden Methoden verwenden:
   * [Profil für Intune-Gerätekonfiguration](active-directory-passwords-login.md#configure-reset-password-link-using-intune)
   * [Registrierungsschlüssel](active-directory-passwords-login.md#configure-reset-password-link-using-the-registry)

## <a name="configure-reset-password-link-using-intune"></a>Konfigurieren des Links „Kennwort zurücksetzen“ mit Intune

### <a name="create-a-device-configuration-policy-in-intune"></a>Erstellen einer Richtlinie für die Gerätekonfiguration in Intune

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Intune**.
2. Erstellen Sie ein neues Profil für die Gerätekonfiguration, indem Sie zu **Gerätekonfiguration** > **Profile** > **Profil erstellen** navigieren.
   * Geben Sie einen aussagekräftigen Namen für das Profil an.
   * Geben Sie optional eine aussagekräftige Beschreibung des Profils an.
   * Plattform **Windows 10 und höher**
   * Profiltyp **Benutzerdefiniert**

   ![CreateProfile][CreateProfile]

3. Konfigurieren von **Einstellungen**
   * Fügen Sie mit **Hinzufügen** die folgende OMA-URI-Einstellung hinzu, um den Link „Kennwort zurücksetzen“ zu aktivieren.
      * Geben Sie einen aussagekräftigen Namen an, um den Zweck der Einstellung zu verdeutlichen.
      * Geben Sie optional eine aussagekräftige Beschreibung der Einstellung an.
      * Festlegung von **OMA-URI** auf `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset`
      * Festlegung von **Data type** auf **Integer**
      * Festlegung von **Value** auf **1**
      * Klicken Sie auf **OK**
   * Klicken Sie auf **OK**
4. Klicken Sie auf **Erstellen**

### <a name="assign-a-device-configuration-policy-in-intune"></a>Zuweisen einer Richtlinie für die Gerätekonfiguration in Intune

#### <a name="create-a-group-to-apply-device-configuration-policy-to"></a>Erstellen einer Gruppe, auf die die Richtlinie für die Gerätekonfiguration angewendet werden kann

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Azure Active Directory**.
2. Navigieren Sie zu **Benutzer und Gruppen** > **Alle Gruppen** > **Neue Gruppe**.
3. Geben Sie einen Namen für die Gruppe ein, und wählen Sie unter **Mitgliedschaftstyp** die Option **Zugewiesen**. 
   * Wählen Sie unter **Mitglieder** die in Azure AD eingebundenen Windows 10-Geräte aus, auf die Sie die Richtlinie anwenden möchten.
   * Klicken Sie auf **Auswählen**.
4. Klicken Sie auf **Erstellen**

Weitere Informationen zum Erstellen von Gruppen finden Sie im Artikel [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md).

#### <a name="assign-device-configuration-policy-to-device-group"></a>Zuweisen einer Richtlinie für die Gerätekonfiguration zu einer Gerätegruppe

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Intune**.
2. Suchen Sie wie folgt nach dem zuvor erstellten Profil für die Gerätekonfiguration: Navigieren Sie zu **Gerätekonfiguration** > **Profile**, und klicken Sie auf das erstellte Profil.
3. Weisen Sie das Profil einer Gruppe mit Geräten zu. 
   * Klicken Sie unter **Include** > **Select groups to include** (Einschließen > Einzuschließende Gruppen auswählen) auf **Assignments** (Zuweisungen).
   * Wählen Sie die zuvor erstellte Gruppe aus, und klicken Sie auf **Auswählen**.
   * Klicken Sie im Menü „Einstellungen“ auf **Speichern**

   ![Zuweisung][Assignment]

Sie haben jetzt eine Richtlinie für die Gerätekonfiguration erstellt und zugewiesen, um den Link „Kennwort zurücksetzen“ auf dem Anmeldebildschirm mit Intune zu aktivieren.

## <a name="configure-reset-password-link-using-the-registry"></a>Konfigurieren des Links „Kennwort zurücksetzen“ über die Registrierung

Es wird empfohlen, diese Methode nur zum Testen der Einstellungsänderung zu verwenden.

1. Melden Sie sich mit Administratoranmeldeinformationen an dem Gerät an, das in die Azure AD-Domäne eingebunden ist.
2. Führen Sie den Befehl **regedit** als Administrator aus.
3. Legen Sie den folgenden Registrierungsschlüssel fest:
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## <a name="what-do-users-see"></a>Anzeige für Benutzer

Was ändert sich für die Benutzer, nachdem die Richtlinie nun konfiguriert und zugewiesen wurde? Wie erfahren sie, dass sie ihr Kennwort über den Anmeldebildschirm zurücksetzen können?

![LoginScreen][LoginScreen]

Wenn Benutzer versuchen, sich anzumelden, wird jetzt der Link „Kennwort zurücksetzen“ angezeigt. Mit diesem Link wird die Oberfläche für die Self-Service-Kennwortzurücksetzung auf dem Anmeldebildschirm geöffnet. Mit dieser Funktion können Benutzer ihr Kennwort zurücksetzen, ohne ein anderes Gerät für den Zugriff auf einen Webbrowser verwenden zu müssen.

Eine Anleitung für Benutzer zur Verwendung dieses Features befindet sich unter [Ich habe mein Azure AD-Kennwort vergessen. Was nun?](active-directory-passwords-update-your-own-password.md#reset-password-at-login).

## <a name="common-issues"></a>Häufige Probleme

Beim Testen dieser Funktionalität mit Hyper-V wird der Link „Kennwort zurücksetzen“ nicht angezeigt.

* Navigieren Sie zu der VM, die Sie zum Testen verwenden möchten, klicken Sie auf **Ansicht**, und deaktivieren Sie **Erweiterte Sitzung**.

Beim Testen dieser Funktionalität per Remotedesktop wird der Link „Kennwort zurücksetzen“ nicht angezeigt.

* Die Kennwortzurücksetzung wird für Remotedesktop derzeit nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD:

* [Erfolgreicher Rollout der Self-Service-Kennwortzurücksetzung](active-directory-passwords-best-practices.md)
* [Zurücksetzen der Kennung auf Windows-Geräten mit dem integrierten PIN-Zurücksetzungsdienst von Microsoft mithilfe von Intune](https://docs.microsoft.com/intune/device-windows-pin-reset)
* [Policy CSP – Authentication](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-authentication) (Richtlinien-Kryptografiedienstanbieter – Authentifizierung)

[CreateProfile]: ./media/active-directory-passwords-login/create-profile.png "Erstellen eines Intune-Gerätekonfigurationsprofils zum Aktivieren des Links „Kennwort zurücksetzen“ auf dem Windows 10-Anmeldebildschirm"
[Assignment]: ./media/active-directory-passwords-login/profile-assignment.png "Zuweisen der Intune-Gerätekonfigurationsrichtlinie zu einer Gruppe mit Windows 10-Geräten"
[LoginScreen]: ./media/active-directory-passwords-login/logon-reset-password.png "Link „Kennwort zurücksetzen“ auf dem Windows 10-Anmeldebildschirm"
