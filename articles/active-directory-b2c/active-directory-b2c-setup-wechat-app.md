---
title: Einrichten der Registrierung und Anmeldung mit einem WeChat-Konto mithilfe von Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit WeChat-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e88187c5035abc28ca9deecaf8517e8a21e38d1d
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952333"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem WeChat-Konto mithilfe von Azure Active Directory B2C

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase.
> 

## <a name="create-a-wechat-application"></a>Erstellen einer WeChat-Anwendung

Um ein WeChat-Konto als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Anwendung in Ihrem Mandanten erstellen, die es darstellt. Wenn Sie noch über kein WeChat-Konto verfügen, können Sie unter [http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html) eines erstellen.

### <a name="register-a-wechat-application"></a>Registrieren einer WeChat-Anwendung

1. Melden Sie sich unter [https://open.weixin.qq.com/](https://open.weixin.qq.com/) mit Ihren WeChat-Anmeldeinformationen an.
2. Wählen Sie **管理中心** (Verwaltungscenter) aus.
3. Führen Sie die Schritte zum Registrieren einer neuen Anwendung aus.
4. Geben Sie `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` unter **授权回调域** (Rückruf-URL) ein. Wenn Ihr `tenant_name` beispielsweise „contoso.onmicrosoft.com“ lautet, setzen Sie die URL auf `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Kopieren Sie die **APP-ID** und den **APP-SCHLÜSSEL**. Sie benötigen diese Angaben, um den Identitätsanbieter Ihrem Mandanten hinzuzufügen.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von WeChat als Identitätsanbieter in Ihrem Mandanten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie oben rechts im Azure-Portal zu diesem Verzeichnis wechseln. Wählen Sie die Abonnementinformationen aus, und klicken Sie dann auf **Verzeichnis wechseln**. 

    ![Wechseln zu Ihrem Azure AD B2C-Mandanten](./media/active-directory-b2c-setup-wechat-app/switch-directories.png)

    Wählen Sie das Verzeichnis aus, das den Mandanten enthält.

    ![Auswählen des Verzeichnisses](./media/active-directory-b2c-setup-wechat-app/select-directory.png)

3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** an. Geben Sie z.B. *WeChat* ein.
6. Wählen Sie **Identitätsanbietertyp** und dann **WeChat (Vorschau)** aus, und klicken Sie auf **OK**.
7. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie die zuvor notierte APP-ID als **Client-ID** und den notierten APP-SCHLÜSSEL als **Clientgeheimnis** der zuvor erstellten WeChat-Anwendung ein.
8. Klicken Sie auf **OK** und dann auf **Erstellen**, um die WeChat-Konfiguration zu speichern.

