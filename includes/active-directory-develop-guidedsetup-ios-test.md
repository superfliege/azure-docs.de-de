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
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: 918e4016f638555bfe2dbaeaa849e963e352c78e
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203479"
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Testen Sie das Abfragen der Microsoft Graph-API von Ihrer iOS-Anwendung aus

Um den Code im Simulator auszuführen, drücken Sie **STRG** + **R**.

![Testen Ihrer Anwendung im Simulator](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

Wenn Sie Tests durchführen möchten, wählen Sie **Microsoft Graph-API aufrufen** aus. Geben Sie Ihren Benutzernamen und Ihr Kennwort ein, wenn Sie dazu aufgefordert werden.

### <a name="provide-consent-for-application-access"></a>Zustimmen zum Anwendungszugriff

Bei der ersten Anmeldung bei Ihrer Anwendung werden Sie aufgefordert, Ihre Zustimmung zu geben, dass die Anwendung auf Ihr Profil zugreifen und Sie anmelden darf:

![Zustimmen zum Anwendungszugriff](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Anzeigen von Anwendungsergebnissen
Nach der Anmeldung sollten Benutzerprofilinformationen angezeigt werden, die vom Microsoft Graph-API-Aufruf im Abschnitt **Protokollierung** zurückgegeben werden. 

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich **user.read**, um das Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die im Registrierungsportal registriert ist. Andere Microsoft Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern unter Umständen zusätzliche Bereiche. Die Microsoft Graph-API benötigt den Bereich **Calendars.Read**, um die Kalender des Benutzers aufzuführen.

Um auf die Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung **Calendars.Read** hinzufügen. Fügen Sie dann dem Aufruf von **acquireTokenSilent** den Bereich **Calendars.Read** hinzu.

>[!NOTE]
>Wenn Sie die Anzahl der Bereiche erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
