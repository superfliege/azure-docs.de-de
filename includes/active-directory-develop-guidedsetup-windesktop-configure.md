---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 220723988f349bf015d2de7633af78782bc03bac
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203220"
---
## <a name="register-your-application"></a>Anwendung registrieren

Sie können Ihre Anwendung auf zwei Arten registrieren.

### <a name="option-1-express-mode"></a>Option 1: Expressmodi

Gehen Sie zur schnellen Registrierung Ihrer Anwendung wie folgt vor:
1. Navigieren Sie zum [Anwendungsregistrierungsportal von Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure).

2. Klicken Sie auf **App hinzufügen**.

3. Geben Sie im Feld **Anwendungsname** einen Namen für Ihre Anwendung ein.

4. Vergewissern Sie sich, dass das Kontrollkästchen **Guided Setup** (Geführtes Setup) aktiviert ist, und klicken Sie anschließend auf **Erstellen**.

5. Befolgen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen Sie sie in Ihren Code ein.

### <a name="option-2-advanced-mode"></a>Option 2: Erweiterter Modus

Wenn Sie Ihre Anwendung registrieren und die Anwendungsregistrierungsinformationen Ihrer Projektmappe hinzufügen möchten, führen Sie folgende Schritte aus:
1. Falls Sie Ihre Anwendung noch nicht registriert haben, navigieren Sie zum [Anwendungsregistrierungsportal von Microsoft](https://apps.dev.microsoft.com/portal/register-app).

2. Klicken Sie auf **App hinzufügen**.

3. Geben Sie im Feld **Anwendungsname** einen Namen für Ihre Anwendung ein.

4. Vergewissern Sie sich, dass das Kontrollkästchen **Guided Setup** (Geführtes Setup) deaktiviert ist, und klicken Sie anschließend auf **Erstellen**.

5. Klicken Sie auf **Plattform hinzufügen** > **Native Anwendung** und anschließend auf **Speichern**.

6. Kopieren Sie die GUID im Feld **Anwendungs-ID**.

7. Öffnen Sie in Visual Studio die Datei *App.xaml.cs*, und ersetzen Sie `your_client_id_here` durch die Anwendungs-ID, die Sie soeben registriert und kopiert haben.

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```
