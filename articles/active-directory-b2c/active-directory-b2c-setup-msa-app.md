---
title: Einrichten der Registrierung und Anmeldung mit einem Microsoft-Konto mithilfe von Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Microsoft-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 338c2a197cb50091c3b272e0ce590341ffda1d7f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341082"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Microsoft-Konto mithilfe von Azure Active Directory B2C

## <a name="create-a-microsoft-account-application"></a>Erstellen einer Microsoft-Kontoanwendung

Um ein Microsoft-Konto als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Anwendung in Ihrem Mandanten erstellen, die es darstellt. Wenn Sie noch über kein Microsoft-Konto verfügen, können Sie eines unter [https://www.live.com/](https://www.live.com/) erstellen.

1. Melden Sie sich beim [Microsoft App-Registrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) mit den Anmeldeinformationen für Ihr Microsoft-Konto an.
2. Wählen Sie oben rechts **App hinzufügen** aus.
3. Geben Sie einen **Namen** für Ihre Anwendung an, und klicken Sie auf **Erstellen**.
4. Kopieren Sie auf der Registrierungsseite den Wert für **Anwendungs-ID**. Sie verwenden ihn, um das Microsoft-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren.
5. Wählen Sie **Plattform hinzufügen** und dann **Web** aus.
6. Geben Sie `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` unter **Umleitungs-URLs** ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z. B. „contosob2c“).
7. Wählen Sie unter **Anwendungsgeheimnisse** die Option **Neues Kennwort generieren** aus. Kopieren Sie das neue Kennwort, das auf dem Bildschirm angezeigt wird. Sie benötigen es, um das Microsoft-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. Dieses Kennwort ist eine wichtige Anmeldeinformation.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Konfigurieren eines Microsoft-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie oben rechts im Azure-Portal zu diesem Verzeichnis wechseln. Wählen Sie die Abonnementinformationen aus, und klicken Sie dann auf **Verzeichnis wechseln**. 

    ![Wechseln zu Ihrem Azure AD B2C-Mandanten](./media/active-directory-b2c-setup-msa-app/switch-directories.png)

    Wählen Sie das Verzeichnis aus, das den Mandanten enthält.

    ![Auswählen des Verzeichnisses](./media/active-directory-b2c-setup-msa-app/select-directory.png)

3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** an. Geben Sie z.B. *MSA* ein.
6. Wählen Sie **Identitätsanbietertyp** und dann **Microsoft-Konto** aus, und klicken Sie auf **OK**.
7. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie die zuvor notierte Anwendungs-ID als **Client-ID** und das notierte Kennwort als **Clientgeheimnis** der zuvor erstellten Microsoft-Anwendung ein.
8. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Konfiguration für das Microsoft-Konto zu speichern.

