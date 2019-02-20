---
title: Verwalten der Authentifizierung in Azure Maps | Microsoft-Dokumentation
description: Sie können über das Azure-Portal die Authentifizierung in Azure Maps verwalten.
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 68c3c8ac39f5803e01ee1038ec85ddb96ac80b30
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56242684"
---
# <a name="manage-authentication-in-azure-maps"></a>Verwalten der Authentifizierung in Azure Maps

Nachdem Sie ein Azure Maps-Konto erstellt haben, werden Client-ID und Schlüssel erstellt, um entweder die Authentifizierung über Azure Active Directory (Azure AD) oder Shared Key zu unterstützen.

Sie finden Ihre Authentifizierungsdetails durch Navigieren zur Seite **Authentifizierung** unter **Einstellungen** im Azure-Portal. Navigieren Sie zu Ihrem Konto. Wählen Sie dann im Menü die Option **Authentifizierung** aus.


## <a name="view-authentication-details"></a>Anzeigen von Authentifizierungsdetails

Um Ihre Authentifizierungsdetails anzuzeigen, navigieren Sie zu der Option **Authentifizierung** im Menü „Einstellungen“.

![Anzeigen der Authentifizierung](./media/how-to-manage-authentication/how-to-view-auth.png)

 Weitere Informationen zu Authentifizierung und Azure Maps finden Sie unter [Authentifizierung mit Azure Maps](https://aka.ms/amauth).


## <a name="configure-azure-ad-app-registration"></a>Konfigurieren der Azure AD-App-Registrierung

Sobald ein Azure Map-Konto erstellt ist, ist eine Verknüpfung zwischen Ihrem Azure AD-Mandanten und der Azure Maps-Azure-Ressource erforderlich. 

1. Wechseln Sie zum Azure AD-Blatt, und erstellen Sie eine App-Registrierung mit Namen und Anmelde-URL der Startseite der Web-App/API, z.B. „https://localhost/“. Wenn Sie bereits über eine registrierte App verfügen, fahren Sie mit Schritt 2 fort.

    ![App-Registrierung](./media/how-to-manage-authentication/app-registration.png)

    ![App-Registrierung](./media/how-to-manage-authentication/app-create.png)

2. Weisen Sie Azure Maps delegierte API-Berechtigungen durch Navigieren zu einer Anwendung unter App-Registrierungen zu, und klicken Sie auf **Einstellungen**.  Wählen Sie **Erforderliche Berechtigungen** und dann **Hinzufügen** aus. Suchen Sie Azure Maps unter **API auswählen**, und klicken Sie auf **Auswählen**.

    ![App-API-Berechtigungen](./media/how-to-manage-authentication/app-permissions.png)

3. Abschließend wählen Sie unter „Berechtigungen auswählen“ „Auf Azure Maps zugreifen“ aus, und klicken Sie auf „Auswählen“.

    ![Auswählen von App-API-Berechtigungen](./media/how-to-manage-authentication/select-app-permissions.png)

4. Führen Sie je nach Authentifizierungsimplementierung Schritt „a“ oder „b“ aus.

    1. Wenn die Anwendung die Benutzertokenauthentifizierung mit unserem Azure Maps-Web-SDK verwendet, müssen Sie den `oauthEnableImplicitFlow` durch Festlegung auf „true“ im Abschnitt „Manifest“ der Detailseite der App-Registrierung aktivieren.
    
       ![App-Manifest](./media/how-to-manage-authentication/app-manifest.png)

    2. Wenn Ihre Anwendung Server/Anwendung-Authentifizierung verwendet, wechseln Sie innerhalb der App-Registrierung zum Blatt **Schlüssel**, und erstellen Sie entweder ein Kennwort, oder laden Sie ein öffentliches Schlüsselzertifikat in die App-Registrierung hoch. Wenn Sie ein Kennwort erstellen und **Speichern**, kopieren Sie es zur späteren Verwendung, und speichern Sie es sicher, denn Sie werden es benötigen, um Token aus Azure AD abzurufen.

       ![App-Schlüssel](./media/how-to-manage-authentication/app-keys.png)


## <a name="grant-rbac-to-azure-maps"></a>Gewähren von RBAC in Azure Maps

Sobald ein Azure Maps-Konto mit Ihrem Azure AD-Mandanten verknüpft ist, kann die Zugriffssteuerung durch Zuweisen des Benutzers oder der Anwendung zu verfügbaren Azure Maps-Zugriffssteuerungsrollen gewährt werden.

1. Navigieren Sie zur Option **Zugriffssteuerung**, klicken Sie auf **Rollenzuweisungen** und **Rollenzuweisung hinzufügen**.

    ![Gewähren von RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. Wählen Sie im Popoutfenster „Rollenzuweisung“ **Rolle** „Azure Maps Date Reader (Vorschauversion)“, **Zugriff zuweisen zu** aus: Azure AD-Benutzer, Gruppe oder Dienstprinzipal, Benutzer oder Anwendung in Dropdownliste **Auswählen** und **Speichern**.

    ![Rollenzuweisung hinzufügen](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Anzeigen verfügbarer Azure Maps-RBAC-Rollen

Navigieren Sie zum Anzeigen von Rollen für die rollenbasierte Zugriffssteuerung für Azure Maps, denen Zugriff gewährt werden kann, zu **Zugriffssteuerung (IAM)**, klicken Sie auf **Rollen**, und suchen Sie nach Rollen, die mit **Azure Maps** beginnen.

![Anzeigen verfügbarer Rollen](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-role-based-access-control-rbac"></a>Anzeigen der rollenbasierten Zugriffssteuerung in Azure Maps (RBAC)

Azure AD ermöglicht eine präzise Zugriffssteuerung über rollenbasierte Zugriffssteuerung (RBAC). 

Zum Anzeigen von Benutzern oder Apps, denen die rollenbasierte Zugriffssteuerung für Azure Maps gewährt wurden, navigieren Sie zu **Zugriffssteuerung (IAM)**, wählen **Rollenzuweisungen** aus und filtern nach **Azure Maps**. Alle verfügbaren Rollen werden unten angezeigt.

![Anzeigen RBAC](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Anfordern von Token für Azure Maps

Sobald Ihre App registriert und mit Azure Maps verknüpft ist, können Sie Zugriffstoken anfordern.

* Wenn die Anwendung Benutzertokenauthentifizierung mit unserem Azure Maps-Webdienst-SDK (Web) verwendet, müssen Sie Ihre HTML-Seite mit der Azure Maps-Client-ID und der Azure AD-App-ID konfigurieren.

* Fordern Sie für Anwendungen, die Server/Anwendung-Authentifizierung verwenden, ein Token vom Azure AD-Anmeldungsendpunkt `https://login.microsoftonline.com` mit Azure AD-Ressourcen-ID `https://atlas.microsoft.com/`, Azure Maps-Client-ID, Azure AD-App-ID und Azure AD-App-Registrierungskennwort oder -Zertifikat an.

Weitere Informationen zum Anfordern von Zugriffstoken aus Azure AD für Benutzer und Dienstprinzipale finden Sie unter [Was ist Authentifizierung?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Azure AD-Authentifizierung und Azure Maps-Web-SDK finden Sie unter [Verwenden des Azure Maps-Kartensteuerelements](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).