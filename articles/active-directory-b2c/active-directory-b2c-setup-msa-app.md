---
title: Konfiguration für Microsoft-Konten in Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Microsoft-Konten in mit Azure Active Directory B2C gesicherten Anwendungen
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c788b14a99125a208390cd4f8ead338efed06933
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444166"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit Microsoft-Konten
## <a name="create-a-microsoft-account-application"></a>Erstellen einer Microsoft-Kontoanwendung
Um den Dienst „Microsoft-Konto“ als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Microsoft-Kontoanwendung erstellen und die entsprechenden Parameter bereitstellen. Sie benötigen dazu ein Microsoft-Konto. Wenn Sie keins besitzen, können Sie unter [https://www.live.com/](https://www.live.com/) eines erstellen.

1. Navigieren Sie zum [Microsoft-Portal für die Anwendungsregistrierung](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), und melden Sie sich mit den Anmeldeinformationen für Ihr Microsoft-Konto an.
2. Klicken Sie auf **App hinzufügen**.
   
    ![Microsoft-Konto – Hinzufügen einer neuen App](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. Geben Sie einen **Namen** für Ihre Anwendung ein, und klicken Sie auf **Anwendung erstellen**.
   
    ![Microsoft-Konto – App-Name](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. Kopieren Sie den Wert unter **Anwendungs-ID**. Sie benötigen ihn, um Microsoft-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren.
   
    ![Microsoft-Konto – Anwendungs-ID](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. Klicken Sie auf **Plattform hinzufügen**, und wählen Sie **Web**.
   
    ![Microsoft-Konto – Plattform hinzufügen](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Microsoft-Konto – Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. Geben Sie im Feld `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`Umleitungs-URIs **den Wert** ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“).
   
    ![Microsoft-Konto – Umleitungs-URL](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. Klicken Sie im Abschnitt **Anwendungsgeheimnisse** auf **Neues Kennwort generieren**. Kopieren Sie das neue Kennwort, das auf dem Bildschirm angezeigt wird. Sie benötigen ihn, um Microsoft-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. Dieses Kennwort ist eine wichtige Anmeldeinformation.
   
    ![Microsoft-Konto – Neues Kennwort generieren](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Microsoft-Konto – Neues Kennwort](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. Aktivieren Sie im Abschnitt **Erweiterte Optionen** das Kontrollkästchen **Live SDK-Unterstützung**. Klicken Sie auf **Speichern**.
   
    ![Microsoft-Konto – Live SDK-Unterstützung](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Microsoft-Konto als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie diese Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen**.
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „MSA“ ein.
5. Klicken Sie auf **Typ des Identitätsanbieters**, wählen Sie **Microsoft-Konto** aus, und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**, und geben Sie die Anwendungs-ID und das Kennwort der Microsoft-Kontoanwendung ein, die Sie zuvor erstellt haben.
7. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Konfiguration für das Microsoft-Konto zu speichern.

