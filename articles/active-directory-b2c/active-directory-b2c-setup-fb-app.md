---
title: Einrichten der Registrierung und Anmeldung mit einem Facebook-Konto mithilfe von Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Facebook-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 985285b463d66770f97a431705d5b9198b632592
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344605"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Facebook-Konto mithilfe von Azure Active Directory B2C

## <a name="create-a-facebook-application"></a>Erstellen einer Facebook-Anwendung

Um ein Facebook-Konto als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Anwendung in Ihrem Mandanten erstellen, die es darstellt. Wenn Sie noch über kein Facebook-Konto verfügen, können Sie unter [https://www.facebook.com/](https://www.facebook.com/) eines erstellen.

1. Melden Sie sich auf der [Facebook-Entwickler-Website](https://developers.facebook.com/) mit den Anmeldeinformationen für Ihr Facebook-Konto an.
2. Wenn Sie dies noch nicht getan haben, müssen Sie sich als Facebook-Entwickler registrieren. Dazu wählen Sie oben rechts auf der Seite die Option **Register** (Registrieren) aus, akzeptieren die Facebook-Richtlinien und führen die Registrierungsschritte aus.
3. Wählen Sie **My Apps** (Meine Apps) aus, und klicken Sie dann auf **Add New App** (Neue App hinzufügen). 
4. Geben Sie unter **Display Name** einen Anzeigenamen und unter **Contact Email** (Kontakt-E-Mail) eine gültige E-Mail-Adresse ein.
5. Klicken Sie auf **Create App ID** (App-ID erstellen). Hierzu müssen Sie möglicherweise die Richtlinien für die Facebook-Plattform akzeptieren und eine Online-Sicherheitsüberprüfung durchführen.
6. Wählen Sie **Settings** > **Basic** (Einstellungen > Allgemeines) aus.
7. Wählen Sie unten auf der Seite die Option **Add Platform** (Plattform hinzufügen) und dann **Website** aus.
8. Geben Sie `https://{tenantname}.b2clogin.com/` unter **Site URL** (Website-URL) ein. Geben Sie unter **Privacy Policy URL** (Datenrichtlinien-URL) eine URL ein, z.B. `http://www.contoso.com`.
9. Klicken Sie auf **Save changes** (Änderungen speichern).
11. Kopieren Sie im oberen Bereich der Seite den Wert von **App ID** (App-ID). 
12. Klicken Sie auf **Show**, und kopieren Sie den Wert unter **App Secret**. Sie benötigen beide Angaben, um Facebook als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. **App Secret** handelt es sich um eine wichtige Sicherheitsinformation für die Anmeldung.
13. Wählen Sie **Products** (Produkte) und dann **Set up** (Einrichten) unter **Facebook Login** (Facebook-Anmeldung) aus.
14. Wählen Sie unter **Facebook Login** (Facebook-Anmeldung) die Option **Settings** (Einstellungen) aus.
15. Geben Sie `https://{tenantname}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` unter **Valid OAuth redirect URIs** (Gültige OAuth-Umleitungs-URIs) ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contosob2c“). Klicken Sie unten auf der Seite auf **Save Changes**.
16. Um Ihre Facebook-Anwendung in Azure AD B2C verfügbar zu machen, wählen Sie **App Review** (App-Überprüfung) aus, legen Sie **Make My Application public?** (Meine Anwendung öffentlich machen?) auf **YES** (Ja) fest, wählen Sie eine Kategorie aus, z.B. `Business and Pages`, und klicken Sie dann auf **Confirm** (Bestätigen).

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Konfigurieren eines Facebook-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie oben rechts im Azure-Portal zu diesem Verzeichnis wechseln. Wählen Sie die Abonnementinformationen aus, und klicken Sie dann auf **Verzeichnis wechseln**. 

    ![Wechseln zu Ihrem Azure AD B2C-Mandanten](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Wählen Sie das Verzeichnis aus, das den Mandanten enthält.

    ![Auswählen des Verzeichnisses](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** ein. Geben Sie z.B. *Facebook* ein.
6. Wählen Sie **Identitätsanbietertyp** und dann **Facebook** aus, und klicken Sie auf **OK**.
7. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie die zuvor notierte App-ID als **Client-ID** und den notierten App-Geheimcode als **Clientgeheimnis** der zuvor erstellten Facebook-Anwendung ein.
8. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Facebook-Konfiguration zu speichern.