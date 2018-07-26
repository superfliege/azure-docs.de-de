---
title: Einrichten der Registrierung und Anmeldung mit einem GitHub-Konto mithilfe von Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit GitHub-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 88fffd28319101c112f848eebc6e8ee27f7f863e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952017"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem GitHub-Konto mithilfe von Azure Active Directory B2C

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase.
> 

Um ein GitHub-Konto als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Anwendung in Ihrem Mandanten erstellen, die es darstellt. Wenn Sie noch über kein GitHub-Konto verfügen, können Sie eines unter [https://www.github.com/](https://www.github.com/) erstellen.

## <a name="create-a-github-oauth-application"></a>Erstellen einer GitHub-OAuth-Anwendung

1. Melden Sie sich auf der [GitHub-Entwickler-Website](https://github.com/settings/developers) mit Ihren GitHub-Anmeldeinformationen an.
2. Wählen Sie **OAuth Apps** (OAuth-Apps) und dann **Register a new application** (Neue Anwendung registrieren) aus.
3. Geben Sie einen **Anwendungsnamen** und Ihre **Homepage-URL** ein.
4. Geben Sie `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` unter **Authorization callback URL** (Autorisierungsrückruf-URL) ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Azure AD B2C-Mandanten (z.B. „B2C.onmicrosoft.com“).
5. Klicken Sie zum Registrieren der Anwendung auf **Register application**.
6. Kopieren Sie die Werte für **Client ID** und **Client Secret**. Sie benötigen beide Angaben, um den Identitätsanbieter Ihrem Mandanten hinzuzufügen.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Konfigurieren eines GitHub-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie oben rechts im Azure-Portal zu diesem Verzeichnis wechseln. Wählen Sie die Abonnementinformationen aus, und klicken Sie dann auf **Verzeichnis wechseln**. 

    ![Wechseln zu Ihrem Azure AD B2C-Mandanten](./media/active-directory-b2c-setup-github-app/switch-directories.png)

    Wählen Sie das Verzeichnis aus, das den Mandanten enthält.

    ![Auswählen des Verzeichnisses](./media/active-directory-b2c-setup-github-app/select-directory.png)

3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** an. Geben Sie z.B. *GitHub* ein.
6. Wählen Sie **Identitätsanbietertyp** und dann **GitHub (Vorschau)** aus, und klicken Sie auf **OK**.
7. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie die zuvor notierte Client-ID als **Client-ID** und das notierte Clientgeheimnis als **Clientgeheimnis** der zuvor erstellten GitHub-Kontoanwendung ein.
8. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Konfiguration des GitHub-Kontos zu speichern.