---
title: Einrichten der Registrierung und Anmeldung mit einem LinkedIn-Konto mithilfe von Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit LinkedIn-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e1949c32833bb1d5e6603a6f5e36e22dc58e8cec
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336927"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem LinkedIn-Konto mithilfe von Azure Active Directory B2C

## <a name="create-a-linkedin-application"></a>Erstellen einer LinkedIn-Anwendung

Um ein LinkedIn-Konto als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Anwendung in Ihrem Mandanten erstellen, die es darstellt. Wenn Sie noch über kein LinkedIn-Konto verfügen, können Sie eines unter [https://www.linkedin.com/](https://www.linkedin.com/) erstellen.

1. Melden Sie sich auf der [LinkedIn-Entwickler-Website](https://www.developer.linkedin.com/) mit den Anmeldeinformationen für Ihr LinkedIn-Konto an.
2. Wählen Sie **My Apps** (Meine Apps) aus, und klicken Sie dann auf **Create Application** (Anwendung erstellen).
3. Geben Sie Informationen für **Company Name** (Firmenname), **Application Name** (Anwendungsname), **Application Description** (Anwendungsbeschreibung), **Application Logo** (Anwendungslogo), **Application Use** (Anwendungsverwendung), **Website URL** (Website-URL), **Business Email** (E-Mail geschäftlich) und **Business Phone** (Telefon geschäftlich) ein.
4. Akzeptieren Sie die **LinkedIn API Terms of Use**, und klicken Sie auf **Submit**.
5. Kopieren Sie die Werte für **Client ID** und **Client Secret**. Sie finden sie unter **Authentication Keys** (Authentifizierungsschlüssel). Sie benötigen beide Angaben, um LinkedIn als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. **geheime Clientschlüssel** ist eine wichtige Sicherheitsanmeldeinformation.
6. Geben Sie `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` unter **Authorized Redirect URLs** (Autorisierte Umleitungs-URLs) ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contosob2c“). Wählen Sie **Add** (Hinzufügen) aus, und klicken Sie dann auf **Update** (Aktualisieren).

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>Konfigurieren eines LinkedIn-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie oben rechts im Azure-Portal zu diesem Verzeichnis wechseln. Wählen Sie die Abonnementinformationen aus, und klicken Sie dann auf **Verzeichnis wechseln**. 

    ![Wechseln zu Ihrem Azure AD B2C-Mandanten](./media/active-directory-b2c-setup-li-app/switch-directories.png)

    Wählen Sie das Verzeichnis aus, das den Mandanten enthält.

    ![Auswählen des Verzeichnisses](./media/active-directory-b2c-setup-li-app/select-directory.png)

3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** an. Geben Sie z.B. *LinkedIn* ein.
6. Wählen Sie **Identitätsanbietertyp** und dann **LinkedIn** aus, und klicken Sie auf **OK**.
7. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie die zuvor notierte Client-ID als **Client-ID** und das notierte Clientgeheimnis als **Clientgeheimnis** der zuvor erstellten LinkedIn-Kontoanwendung ein.
8. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Konfiguration des LinkedIn-Kontos zu speichern.

