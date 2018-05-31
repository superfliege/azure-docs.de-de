---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 7c052f30e564700b0a99b76342dc6c34ec5983b7
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33901277"
---
## <a name="register-your-application"></a>Anwendung registrieren

Wenn Sie Ihre Anwendung registrieren und die Anwendungsregistrierungsinformationen Ihrer Projektmappe hinzufügen möchten, haben Sie zwei Möglichkeiten:

### <a name="option-1-express-mode"></a>Option 1: Expressmodus

Gehen Sie zur schnellen Registrierung Ihrer Anwendung wie folgt vor:

1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure).
2.  Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.  Stellen Sie sicher, dass die Option für geführtes Setup aktiviert ist.
4.  Befolgen Sie die Anweisungen, um Ihrer Anwendung eine Umleitungs-URL hinzufügen.

### <a name="option-2-advanced-mode"></a>Option 2: Erweiterter Modus

Wenn Sie Ihre Anwendung registrieren und die Anwendungsregistrierungsinformationen Ihrer Projektmappe hinzufügen möchten, führen Sie folgende Schritte aus:

1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein. 
3. Stellen Sie sicher, dass die Option für geführtes Setup deaktiviert ist.
4. Klicken Sie auf `Add Platform`, und wählen Sie dann `Web` aus.
5. Kehren Sie zurück zu Visual Studio, und wählen Sie im Projektmappen-Explorer das Projekt aus. Untersuchen Sie das Fenster „Eigenschaften“ (wenn es nicht angezeigt wird, drücken Sie F4).
6. Ändern Sie „SSL-aktiviert“ in `True`.
7. Klicken Sie mit der rechten Maustaste in Visual Studio auf das Projekt, und wählen Sie dann **Eigenschaften** und die Registerkarte **Web** aus. Ändern Sie im Abschnitt *Server* die *Projekt-URL* in die SSL-URL.
8. Kopieren Sie die SSL-URL, und fügen Sie diese URL zur Liste der Umleitungs-URLs im Registrierungsportal hinzu:<br/><br/>![Projekteigenschaften](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
9. Fügen Sie im Stammordner der Datei `web.config` im Abschnitt `configuration\appSettings` Folgendes hinzu:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

10. Ersetzen Sie `ClientId` durch die Anwendungs-ID, die Sie gerade registriert haben.
11. Ersetzen Sie `redirectUri` durch die SSL-URL Ihres Projekts.

