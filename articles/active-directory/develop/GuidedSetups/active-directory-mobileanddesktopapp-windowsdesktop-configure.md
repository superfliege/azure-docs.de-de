---
title: Azure AD v2 Windows Desktop erste Schritte - Konfiguration | Microsoft Docs
description: "Wie kann eine Windows Desktop .NET (XAML)-Anwendung ein Zugriffstoken abrufen und Aufrufen eine API von Azure Active Directory-v2-Endpunkt geschützt. | Microsoft Azure | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 1dfaa7ade664e43dcb9aa788b0197ca17e6ec4cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
## <a name="create-an-application-express"></a>Erstellen einer Anwendung (Express)
Nun Sie zum Registrieren Ihrer Anwendung in müssen der *Microsoft App-Registrierungsportal*:
1. Registrieren Sie Ihre Anwendung über die [Microsoft App-Registrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)
2.  Geben Sie einen Namen für die Anwendung und Ihre e-Mail-Adresse
3.  Stellen Sie sicher, dass die Option für das Setup geführtes aktiviert ist
4.  Führen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen ihn in Ihren code

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Fügen Sie Ihre Anwendung Registrierungsinformationen zur Projektmappe (Erweitert)
Nun Sie zum Registrieren Ihrer Anwendung in müssen der *Microsoft App-Registrierungsportal*:
1. Wechseln Sie zu der [Microsoft App-Registrierungsportal](https://apps.dev.microsoft.com/portal/register-app) zum Registrieren einer Anwendung
2. Geben Sie einen Namen für die Anwendung und Ihre e-Mail-Adresse 
3. Stellen Sie sicher, dass die Option für das Setup geführtes deaktiviert ist.
4. Klicken Sie auf `Add Platform`, und wählen Sie dann `Native Application` und klicken Sie auf Speichern
5. Kopieren Sie die GUID im Anwendungs-ID, wechseln Sie zurück zu Visual Studio, öffnen `App.xaml.cs` , und Ersetzen Sie `your_client_id_here` mit Anwendungs-ID, die Sie gerade registriert haben:

```csharp
private static string ClientId = "your_application_id_here";
```
