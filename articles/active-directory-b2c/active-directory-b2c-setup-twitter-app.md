---
title: Einrichten der Registrierung und Anmeldung mit einem Twitter-Konto mithilfe von Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Twitter-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: bf5ae39d83fd021775fbd18cf23d2e6b9078e748
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37927895"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Twitter-Konto mithilfe von Azure Active Directory B2C

## <a name="create-a-twitter-application"></a>Erstellen einer Twitter-Anwendung

Um ein Twitter-Konto als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Anwendung in Ihrem Mandanten erstellen, die es darstellt. Wenn Sie noch über kein Twitter-Konto verfügen, können Sie eines unter [https://twitter.com/signup](https://twitter.com/signup) erstellen.

1. Melden Sie sich auf der Website [Twitter Apps](https://apps.twitter.com/) mit Ihren Twitter-Anmeldeinformationen an.
2. Wählen Sie **Create New App** (Neue App erstellen) aus.
3. Geben Sie einen **Namen**, unter **Description** eine Beschreibung und eine **Website** an.
4. Geben Sie `https://login.microsoftonline.com/te/{tenant}/{policyId}/oauth1/authresp` unter **Callback URLs** (Rückruf-URLs) ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. contosob2c.onmicrosoft.com) und **{policyId}** durch Ihre Richtlinien-ID (z.B. b2c_1_policy). Sie sollten für alle Richtlinien, die ein Twitter-Konto verwenden, eine Rückruf-URL hinzufügen. Achten Sie darauf, `b2clogin.com` anstelle von ` login.microsoftonline.com` zu verwenden, wenn Sie die Anmelde-ID in Ihrer Anwendung verwenden.
5. Stimmen Sie dem **Developer Agreement** (Vereinbarung für Entwickler) zu, und wählen Sie **Create your Twitter application** (Ihre Twitter-Anwendung erstellen) aus.
7. Wählen Sie die Registerkarte **Keys and Access Tokens** .
8. Kopieren Sie den Wert von **Consumer Key** und **Consumer Secret**. Sie benötigen beide Angaben, um ein Twitter-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Twitter als Identitätsanbieter in Ihrem Mandanten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie oben rechts im Azure-Portal zu diesem Verzeichnis wechseln. Wählen Sie die Abonnementinformationen aus, und klicken Sie dann auf **Verzeichnis wechseln**. 

    ![Wechseln zu Ihrem Azure AD B2C-Mandanten](./media/active-directory-b2c-setup-twitter-app/switch-directories.png)

    Wählen Sie das Verzeichnis aus, das den Mandanten enthält.

    ![Auswählen des Verzeichnisses](./media/active-directory-b2c-setup-twitter-app/select-directory.png)

3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** an. Geben Sie z.B. *Twitter* ein.
6. Wählen Sie **Identitätsanbietertyp** und dann **Twitter** aus, und klicken Sie auf **OK**.
7. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie den „Consumer Key“ (Kundenschlüssel) als **Client-ID** und das **Consumer Secret** (Kundengeheimnis) als **Clientgeheimnis** ein.
8. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Twitter-Konfiguration zu speichern.