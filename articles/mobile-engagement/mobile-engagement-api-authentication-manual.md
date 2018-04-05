---
title: 'Authentifizieren mit Mobile Engagement-REST-APIs: Manuelle Einrichtung'
description: Beschreibt, wie Sie die Authentifizierung für Mobile Engagement-REST-APIs manuell einrichten
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0d71908b34ddf8313aa45014420c9e63a00078c9
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Authentifizieren bei Mobile Engagement-REST-APIs – manuelles Setup
> [!IMPORTANT]
> Azure Mobile Engagement wird am 31.3.2018 außer Kraft gesetzt. Diese Seite wird kurz danach gelöscht.
> 

Dies ist eine Zusatzdokumentation zu [Authentifizieren mit Mobile Engagement-REST-APIs](mobile-engagement-api-authentication.md). Lesen Sie zuerst diesen Artikel, um den Kontext zu verstehen. Hier wird eine alternative Möglichkeit zum einmaligen Einrichten Ihrer Authentifizierung für die Mobile Engagement-REST-APIs mithilfe des Azure-Portals beschrieben.

> [!NOTE]
> Die folgenden Anweisungen basieren auf [diesem Active Directory-Leitfaden](../azure-resource-manager/resource-group-create-service-principal-portal.md). Sie sind an die Authentifizierungsanforderungen für Mobile Engagement-APIs angepasst. Konsultieren Sie diesen Leitfaden, wenn Sie die folgenden Schritte im Detail verstehen möchten.

1. Melden Sie sich über das [Azure-Portal](https://portal.azure.com/) bei Ihrem Azure-Konto an.
2. Wählen Sie im linken Bereich **Active Directory** aus.

   ![Active Directory auswählen][1]

3. Klicken Sie auf **App-Registrierungen**, um die Anwendungen in Ihrem Verzeichnis anzuzeigen.

   ![Anwendungen anzeigen][3]

4. Wählen Sie **Registrierung einer neuen Anwendung** aus.

   ![Anwendung hinzufügen][4]

5. Geben Sie den Namen der Anwendung ein. Wählen Sie als Typ der Anwendung **Web-App/API** aus, und klicken Sie dann auf die Schaltfläche **Weiter**. Sie können für **ANMELDE-URL** beliebige Dummy-URLs angeben. Sie werden in diesem Szenario nicht verwendet, und die URLs selbst werden nicht überprüft.

   Sobald Sie fertig sind, haben Sie eine Azure Active Directory-App (Azure AD) mit dem Namen, den Sie angegeben haben. Dies ist Ihr **AD\_-APP\_-NAME**, den Sie unbedingt notieren sollten.

   ![App-Name][8]

7. Wählen Sie den App-Namen aus.

8. Notieren Sie sich die **Anwendungs-ID**. Dies ist die Client-ID, die für Ihre API-Aufrufe als **CLIENT\_ID** verwendet wird.

   ![Anwendungs-ID suchen][10]

9. Wechseln Sie rechts zum Abschnitt **Schlüssel**.

   ![Abschnitt „Schlüssel“][11]

10. Erstellen Sie einen neuen Schlüssel, und kopieren Sie ihn sofort. Er wird nicht erneut angezeigt.

    ![Bereich „Schlüssel“ mit Details zu Schlüsseln][12]

    > [!IMPORTANT]
    > Dieser Schlüssel läuft am Ende der Dauer ab, die Sie angegeben haben. Stellen Sie sicher, dass Sie ihn zu gegebener Zeit erneuern, da sonst Ihre API-Authentifizierung nicht mehr funktioniert. Wenn Sie vermuten, dass die Sicherheit dieses Schlüssels gefährdet ist, können Sie ihn löschen und neu erstellen.
    >
    
11. Klicken Sie oben auf der Seite auf die Schaltfläche **Endpunkte**. Kopieren Sie dann den **OAUTH 2.0-TOKENENDPUNKT**.

    ![Endpunkt kopieren][14]

16. Dieser Endpunkt liegt im folgenden Format vor, wobei die GUID in der URL Ihre **MANDANTEN-ID** darstellt: `https://login.microsoftonline.com/<GUID>/oauth2/token`

17. Als Nächstes konfigurieren Sie die Berechtigungen für diese App. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), um diesen Vorgang zu starten.

18. Klicken Sie auf **Ressourcengruppen**, und suchen dann Sie die Ressourcengruppe **Mobile Engagement**.

    ![MobileEngagement suchen][15]

19. Wählen Sie die Ressourcengruppe **MobileEngagement** und dann **Alle Einstellungen** aus.

    ![Zu Einstellungen für MobileEngagement navigieren][16]

20. Wählen Sie im Abschnitt **Einstellungen** die Option **Benutzer** aus. Klicken Sie dann zum Hinzufügen eines Benutzers auf **Hinzufügen**.

    ![Hinzufügen eines Benutzers][17]

21. Klicken Sie auf **Rolle auswählen**.

    ![Auswählen einer Rolle][18]

22. Wählen Sie **Besitzer** aus.

    ![„Besitzer“ als Rolle auswählen][19]

23. Suchen Sie im Suchfeld den Namen Ihrer Anwendung, **AD\_APP\_NAME**. Dieser Name wird hier nicht standardmäßig angezeigt. Nachdem Sie ihn gefunden haben, wählen Sie ihn aus. Klicken Sie unten im Abschnitt auf **Auswählen**.

    ![Den Namen auswählen][20]

24. Im Abschnitt **Zugriff hinzufügen** wird die Anwendung mit **1 Benutzer, 0 Gruppen** angezeigt. Klicken Sie auf **OK**, um die Änderung zu bestätigen.

    ![Hinzugefügten Benutzer bestätigen][21]

Sie haben nun die erforderliche Azure AD-Konfiguration abgeschlossen und sind zum Aufrufen der APIs bereit.

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client-secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



