---
title: Einrichten der Registrierung und Anmeldung mit einem Google-Konto mithilfe von Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Google-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: dd0bf50d73b70e37195e8e5e45336b68e4e883e7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915638"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Google-Konto mithilfe von Azure Active Directory B2C

## <a name="create-a-google-application"></a>Erstellen einer Google-Anwendung

Um ein Google-Konto als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Anwendung in Ihrem Mandanten erstellen, die es darstellt. Wenn Sie noch über kein Google-Konto verfügen, können Sie eines unter [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp) erstellen.

1. Melden Sie sich bei der [Google Developers Console](https://console.developers.google.com/) mit den Anmeldeinformationen für Ihr Google-Konto an.
2. Wählen Sie **Create project** (Projekt erstellen) aus, und klicken Sie dann auf **Create** (Erstellen). Wenn Sie zuvor bereits Projekte erstellt haben, wählen Sie die Projektliste und dann **New Project** (Neues Projekt) aus.
3. Geben Sie einen **Projektnamen** ein, und klicken Sie auf **Erstellen**.
3. Wählen Sie im linken Menü die Option **Credentials** (Anmeldeinformationen) und anschließend **Create credentials** > **Oauth client ID** (Anmeldeinformationen erstellen > OAuth-Client-ID) aus.
4. Wählen Sie **Configure consent screen** (Genehmigungsbildschirm konfigurieren) aus.
5. Wählen Sie unter **Email address** (E-Mail-Adresse) eine gültige Adresse aus, oder geben Sie sie an, geben Sie einen Wert unter **Product name shown to users** (Für Benutzer angezeigter Produktname) ein, und klicken Sie auf **Save** (Speichern).
6. Wählen Sie unter **Anwendungstyp** die Option **Webanwendung** aus.
7. Geben Sie einen **Namen** für die Anwendung an, und geben Sie `https://login.microsoftonline.com` unter **Authorized JavaScript origins** (Autorisierte JavaScript-Quellen) und `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` unter **Authorized redirect URIs** (Autorisierte Umleitungs-URIs) ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“).
8. Klicken Sie auf **Create**.
9. Kopieren Sie die Werte für **Client-ID** und **Clientgeheimnis**. Sie benötigen beide Angaben, um Google als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. Das **Clientgeheimnis** ist eine wichtige Anmeldeinformation.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Konfigurieren eines Google-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie oben rechts im Azure-Portal zu diesem Verzeichnis wechseln. Wählen Sie die Abonnementinformationen aus, und klicken Sie dann auf **Verzeichnis wechseln**. 

    ![Wechseln zu Ihrem Azure AD B2C-Mandanten](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Wählen Sie das Verzeichnis aus, das den Mandanten enthält.

    ![Auswählen des Verzeichnisses](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** ein. Geben Sie z.B. *Google* ein.
6. Wählen Sie **Identitätsanbietertyp** und dann **Google** aus, und klicken Sie auf **OK**.
7. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie die zuvor notierte Client-ID als **Client-ID** und das notierte Clientgeheimnis als **Clientgeheimnis** der zuvor erstellten Google-Anwendung ein.
8. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Google-Konfiguration zu speichern.

