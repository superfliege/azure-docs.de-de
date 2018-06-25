---
title: Google+-Konfiguration in Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Google+-Konten in mit Azure Active Directory B2C gesicherten Anwendungen
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5377e4b56bca09a1785d14bfe4c32de01e6db7d3
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711364"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit Google+-Konten
## <a name="create-a-google-application"></a>Erstellen einer Google+-Anwendung
Um Google+ als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Google+-Anwendung erstellen und die entsprechenden Parameter bereitstellen. Sie benötigen dazu ein Google+-Konto. Wenn Sie keins besitzen, können Sie unter [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp) eines erstellen.

1. Klicken Sie auf die [Google Developers Console](https://console.developers.google.com/), und melden Sie sich mit Ihren Anmeldeinformationen für Google+ an.
2. Klicken Sie auf **Projekt erstellen**, geben Sie unter **Projektname** einen Projektnamen ein, und klicken Sie auf **Erstellen**.
   
    ![Google+ – Erste Schritte](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google+ - Neues Projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. Klicken Sie links im Navigationsbereich auf **API Manager** und dann auf **Zugangsdaten**.
4. Klicken Sie oben auf die Registerkarte **OAuth-Zustimmungsbildschirm**.
   
    ![Google+ – Anmeldeinformationen](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. Wählen Sie unter **E-Mail-Adresse** eine gültige Adresse aus, oder geben Sie sie ein, geben Sie einen Wert für **Produktname** ein, und klicken Sie auf **Speichern**.
   
    ![Google+ – OAuth-Zustimmungsbildschirm](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. Klicken Sie auf **Anmeldeinformationen hinzufügen**, und wählen Sie dann **OAuth-Client-ID** aus.
   
    ![Google+ – OAuth-Zustimmungsbildschirm](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. Wählen Sie unter **Anwendungstyp** die Option **Webanwendung** aus.
   
    ![Google+ – OAuth-Zustimmungsbildschirm](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. Geben Sie einen **Namen** für die Anwendung an, und geben Sie `https://login.microsoftonline.com` im Feld **Autorisierte JavaScript-Quellen** und `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` im Feld **Autorisierte Weiterleitungs-URIs** ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“). Beim Wert für **{tenant}** wird die Groß-/Kleinschreibung beachtet. Klicken Sie auf **Create**.
   
    ![Google+ - Client-ID erstellen](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. Kopieren Sie die Werte für **Client-ID** und **Clientgeheimnis**. Sie benötigen beide Angaben, um Google+ als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. Das **Clientgeheimnis** ist eine wichtige Anmeldeinformation.
   
    ![Google+ – Clientgeheimnis](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Google+ als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie die folgenden Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen**.
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z. B. „G+“ ein.
5. Klicken Sie auf **Typ des Identitätsanbieters**, wählen Sie **Google** aus, und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**, und geben Sie die Client-ID und das Clientgeheimnis der Google+-Anwendung ein, die Sie zuvor erstellt haben.
7. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Konfiguration für Google+ zu speichern.

