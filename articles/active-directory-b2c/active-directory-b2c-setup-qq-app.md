---
title: Einrichten der Registrierung und Anmeldung mit einem QQ-Konto mithilfe von Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit QQ-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 11bb5bf132103bed9e154a12c0e628177ca6a57a
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344923"
---
# <a name="set-up-sign-up-and-sign-in-with-a-qq-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem QQ-Konto mithilfe von Azure Active Directory B2C

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase.
> 

## <a name="create-a-qq-application"></a>Erstellen einer QQ-Anwendung

Um ein QQ-Konto als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Anwendung in Ihrem Mandanten erstellen, die es darstellt. Wenn Sie noch über kein QQ-Konto verfügen, können Sie eines unter [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033) erstellen.

### <a name="register-for-the-qq-developer-program"></a>Registrieren beim QQ-Entwicklerprogramm

1. Melden Sie sich beim [QQ-Entwicklerportal](http://open.qq.com) mit den Anmeldeinformationen für Ihr QQ-Konto an.
2. Rufen Sie nach der Anmeldung die Seite [http://open.qq.com/reg](http://open.qq.com/reg) auf, um sich als Entwickler zu registrieren.
3. Wählen Sie **个人** (Einzelner Entwickler) aus.
4. Geben Sie die erforderlichen Informationen ein, und wählen Sie **下一步** (Nächster Schritt) aus.
5. Schließen Sie den E-Mail-Überprüfungsprozess ab. Es dauert einige Tage, bis Ihre Registrierung als Entwickler genehmigt wird. 

### <a name="register-a-qq-application"></a>Registrieren einer QQ-Anwendung

1. Navigieren Sie zu [https://connect.qq.com/index.html](https://connect.qq.com/index.html).
2. Wählen Sie **应用管理** (App-Verwaltung) aus.
5. Wählen Sie **创建应用** (App erstellen) aus, und geben Sie die erforderlichen Informationen ein.
7. Geben Sie `https://{tenant_name}.b2clogin.com/te/{tenant_name}.onmicrosoft.com/oauth2/authresp` unter **授权回调域** (Rückruf-URL) ein. Wenn Ihr `tenant_name` beispielsweise „contoso“ lautet, legen Sie die URL auf `https://contoso.b2clogin.com/te/contoso.onmicrosoft.com/oauth2/authresp` fest.
8. Wählen Sie **创建应用** (App erstellen) aus.
9. Wählen Sie auf der Bestätigungsseite die Option **应用管理** (App-Verwaltung) aus, um zur Seite für die App-Verwaltung zurückzukehren.
10. Wählen Sie neben der erstellten App die Option **查看** (Anzeigen) aus.
11. Wählen Sie **修改** (Bearbeiten) aus.
12. Kopieren Sie die **APP-ID** und den **APP-SCHLÜSSEL**. Sie benötigen beide Werte, um den Identitätsanbieter Ihrem Mandanten hinzuzufügen.

## <a name="configure-qq-as-an-identity-provider"></a>Konfigurieren von QQ als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie oben rechts im Azure-Portal zu diesem Verzeichnis wechseln. Wählen Sie die Abonnementinformationen aus, und klicken Sie dann auf **Verzeichnis wechseln**. 

    ![Wechseln zu Ihrem Azure AD B2C-Mandanten](./media/active-directory-b2c-setup-qq-app/switch-directories.png)

    Wählen Sie das Verzeichnis aus, das den Mandanten enthält.

    ![Auswählen des Verzeichnisses](./media/active-directory-b2c-setup-qq-app/select-directory.png)

3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** an. Geben Sie z.B. *QQ* ein.
6. Wählen Sie **Identitätsanbietertyp** und dann **QQ (Vorschau)** aus, und klicken Sie auf **OK**.
7. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie die zuvor notierte APP-ID als **Client-ID** und den notierten APP-SCHLÜSSEL als **Clientgeheimnis** der zuvor erstellten QQ-Anwendung ein.
8. Klicken Sie auf **OK** und dann auf **Erstellen**, um die QQ-Konfiguration zu speichern.

