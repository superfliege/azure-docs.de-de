---
title: Konfiguration für LinkedIn in Azure Active Directory B2C | Microsoft-Dokumentation
description: Stellen Sie in Ihren durch Azure Active Directory B2C geschützten Anwendungen Registrierungs- und Anmeldefunktionen für Kunden mit LinkedIn-Konten bereit.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6db3832031a1bb960ee40c0e4fb8c3d0591a976c
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711697"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit LinkedIn-Konten
## <a name="create-a-linkedin-application"></a>Erstellen einer LinkedIn-Anwendung
Um LinkedIn als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine LinkedIn-Anwendung erstellen und mit den entsprechenden Parametern bereitstellen. Sie benötigen dazu ein LinkedIn-Konto. Wenn Sie keins besitzen, können Sie unter [https://www.linkedin.com/](https://www.linkedin.com/) eines erstellen.

1. Navigieren Sie zur [LinkedIn-Entwickler-Website](https://www.developer.linkedin.com/), und melden Sie sich mit den Anmeldeinformationen für Ihr LinkedIn-Konto an.
2. Klicken Sie in der oberen Menüleiste auf **My Apps** und dann auf **Create Application**.
   
    ![LinkedIn – Neue App](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. Geben Sie im Formular **Create a New Application** die relevanten Informationen ein (**Firmenname**, **Name**, **Beschreibung**, **Anwendungslogo-URL**, **Anwendungsverwendung**, **Website-URL**, **E-Mail-Adresse (geschäftlich)** und **Telefon (geschäftlich)**).
4. Akzeptieren Sie die **LinkedIn API Terms of Use**, und klicken Sie auf **Submit**.
   
    ![LinkedIn – Registrieren einer App](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. Kopieren Sie die Werte für **Client ID** und **Client Secret**. (Sie finden sie unter **Authentication Keys**.) Sie benötigen beide Angaben, um LinkedIn als Identitätsanbieter in Ihrem Mandanten zu konfigurieren.
   
   > [!NOTE]
   > Das **Clientgeheimnis** ist eine wichtige Sicherheitsanmeldeinformation.
   > 
   > 
6. Geben Sie im Feld **Authorized Redirect URLs** (unter **OAuth 2.0**) den Wert `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contoso.onmicrosoft.com“). Klicken Sie auf **Add** und dann auf **Update**. Der Wert **{tenant}** muss klein geschrieben sein.
   
    ![LinkedIn – Einrichten einer App](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von LinkedIn als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie diese Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen**.
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „LI“ ein.
5. Klicken Sie auf **Typ des Identitätsanbieters**, wählen Sie **LinkedIn** aus, und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**, und geben Sie die Client-ID und das Clientgeheimnis der LinkedIn-Anwendung ein, die Sie zuvor erstellt haben.
7. Klicken Sie auf **OK** und dann auf **Erstellen**, um die LinkedIn-Konfiguration zu speichern.

