---
title: 'Einrichten der Registrierung und Anmeldung mit einem GitHub-Konto: Azure Active Directory B2C | Microsoft-Dokumentation'
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit GitHub-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: f1c1ac91c08fe27445f4b9631500543d1d0287bd
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56427274"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem GitHub-Konto mithilfe von Azure Active Directory B2C

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase.
> 

Um ein GitHub-Konto als [Identitätsanbieter](active-directory-b2c-reference-oauth-code.md) in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Anwendung in Ihrem Mandanten erstellen, die es darstellt. Wenn Sie noch über kein GitHub-Konto verfügen, können Sie eines unter [https://www.github.com/](https://www.github.com/) erstellen.

## <a name="create-a-github-oauth-application"></a>Erstellen einer GitHub-OAuth-Anwendung

1. Melden Sie sich auf der [GitHub-Entwickler-Website](https://github.com/settings/developers) mit Ihren GitHub-Anmeldeinformationen an.
2. Klicken Sie auf **OAuth Apps** (OAuth-Apps) und anschließend auf **New OAuth App** (Neue OAuth-App).
3. Geben Sie einen **Anwendungsnamen** und Ihre **Homepage-URL** ein.
4. Geben Sie `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` unter **Authorization callback URL** (Autorisierungsrückruf-URL) ein. Ersetzen Sie `your-tenant-name` durch den Namen des Azure AD B2C-Mandanten. Verwenden Sie bei der Eingabe Ihres Mandantennamens nur Kleinbuchstaben, auch wenn der Mandant in Azure AD B2C Großbuchstaben enthält.
5. Klicken Sie zum Registrieren der Anwendung auf **Register application**.
6. Kopieren Sie die Werte für **Client ID** und **Client Secret**. Sie benötigen beide Angaben, um den Identitätsanbieter Ihrem Mandanten hinzuzufügen.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Konfigurieren eines GitHub-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** an. Geben Sie z.B. *GitHub* ein.
6. Wählen Sie **Identitätsanbietertyp** und dann **GitHub (Vorschau)** aus, und klicken Sie auf **OK**.
7. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie die zuvor notierte Client-ID als **Client-ID** und den notierten geheimen Clientschlüssel als **geheimen Clientschlüssel** der zuvor erstellten GitHub-Kontoanwendung ein.
8. Klicken Sie auf **OK**, und klicken Sie dann auf **Erstellen**, um die Konfiguration des GitHub-Kontos zu speichern.
