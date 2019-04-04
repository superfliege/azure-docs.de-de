---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: c8e5e4f826d7835a1fd38d1db5bfeab19b679b30
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203725"
---
## <a name="test-your-app"></a>Testen Ihrer App

1. Führen Sie Ihren Code auf Ihrem Gerät/Emulator aus.
2. Melden Sie sich mit einem Azure Active Directory-Konto (Geschäfts-, Schul- oder Unikonto) oder einem Microsoft-Konto (live.com, outlook.com) an. 

    ![Testen Ihrer Anwendung](media/active-directory-develop-guidedsetup-android-test/mainwindow.png)
    <br/><br/>
    ![Eingeben von Benutzername und Kennwort](media/active-directory-develop-guidedsetup-android-test/usernameandpassword.png)

### <a name="consent-to-your-app"></a>Zustimmung zur App

Wenn sich ein Benutzer zum ersten Mal bei Ihrer Anwendung anmeldet, wird er aufgefordert, den von der App benötigten Berechtigungen zuzustimmen, wie hier gezeigt: 

![Zustimmen zum Anwendungszugriff](media/active-directory-develop-guidedsetup-android-test/androidconsent.png)

### <a name="success"></a>Erfolg!

Nach der Anmeldung und Zustimmung zeigt die App die Antwort der Microsoft Graph-API an. Dieser spezifische Aufruf richtet sich an den Endpunkt **/me** und gibt das [Benutzerprofil](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_get) zurück. Eine Liste der anderen Microsoft Graph-Endpunkte finden Sie in der Entwicklerdokumentation mit einer [Übersicht über Microsoft Graph](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="scopes-and-delegated-permissions"></a>Bereiche und delegierte Berechtigungen

Die Microsoft Graph-API benötigt den Bereich *User.Read*, um das Benutzerprofil zu lesen. Dieser Bereich ist automatisch in jeder Anwendung enthalten, die im Anwendungsregistrierungsportal registriert wird. Für andere APIs sind zusätzliche Bereiche erforderlich. Die Microsoft Graph-API benötigt beispielsweise den Bereich *Calendars.Read*, um die Kalender des Benutzers aufzuführen.

Um auf die Kalender des Benutzers zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung *Calendars.Read* hinzufügen. Fügen Sie dann dem Aufruf von `acquireTokenSilent` den Bereich *Calendars.Read* hinzu. 

> [!NOTE]
> Ihre Benutzer werden unter Umständen zu einer weiteren Zustimmung aufgefordert, wenn Sie Ihre App-Registrierung ändern.

<!--end-collapse-->

[!INCLUDE [Help and support](active-directory-develop-help-support-include.md)]
