---
title: Registrieren einer Anwendung beim Azure AD v2.0-Endpunkt mithilfe des Portals | Microsoft-Dokumentation
description: Registrieren einer App bei Microsoft für die Anmeldung und den Zugriff auf Microsoft-Dienste mit dem v2.0-Endpunkt
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 8ab4e6b5b2813a216b6dd6f0fc108a09239ca9a6
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39507611"
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Registrieren einer App mit dem v2.0-Endpunkt
Um eine App zu erstellen, die die Anmeldung sowohl für ein persönliches Microsoft-Konto (MSA) als auch für ein Geschäfts-, Schul- oder Unikonto (Azure AD) zulässt, müssen Sie zunächst eine Anwendung bei Microsoft registrieren. Derzeit können Sie keine in Azure AD oder MSA vorhandenen Apps nutzen, stattdessen müssen Sie eine neue App erstellen.

> [!NOTE]
> Nicht alle Szenarien und Funktionen von Azure Active Directory werden vom v2.0-Endpunkt unterstützt. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um zu bestimmen, ob Sie den v2.0-Endpunkt verwenden sollten.


## <a name="visit-the-microsoft-app-registration-portal"></a>Aufrufen des Microsoft App-Registrierungsportals
Navigieren Sie als Erstes zum Microsoft App-Registrierungsportal unter [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). 

Melden Sie sich entweder mit einem persönlichen Microsoft-Konto oder einem Geschäfts- oder Schulkonto an. Wenn Sie noch kein Konto besitzen, melden Sie sich für ein neues persönliches Konto an.

Fertig? Jetzt sollten Sie Ihre Liste der Microsoft-Apps sehen, die wahrscheinlich noch leer ist. Ändern wir das.

Klicken Sie auf **App hinzufügen**, und geben Sie einen Namen für die App ein. Das Portal weist der App eine global eindeutige Anwendungs-ID zu, die Sie später in Ihrem Code verwenden. Wenn Ihre App eine serverseitige Komponente enthält, die Zugriffstoken zum Aufrufen von APIs benötigt (denken Sie an Office, Azure oder Ihre eigene Web-API), sollten Sie auch hier einen **geheimen Anwendungsschlüssel** erstellen.

Fügen Sie dann die **Plattformen** hinzu, auf denen Ihre App verwendet wird.

* Geben Sie bei webbasierten Apps einen **Umleitungs-URI** an, an den die Anmeldenachrichten gesendet werden können.
* Notieren Sie sich den standardmäßigen Umleitungs-URI für mobile Apps, der automatisch erstellt wurde.
* Bei Web-APIs wird für Sie automatisch ein Standardbereich für den Zugriff auf die Web-API erstellt. Mithilfe der Schaltfläche **Bereich hinzufügen** können Sie zusätzliche Bereiche hinzufügen. Über das Formular **Vorautorisierte Anwendungen** können Sie auch Anwendungen hinzufügen, die für die Verwendung der Web-API vorautorisiert sind. 

Optional können Sie das Aussehen und Verhalten der Anmeldeseite im Abschnitt **Profil** anpassen. Achten Sie darauf, vor dem Fortfahren auf **Speichern** zu klicken.

> [!NOTE]
> Beim Erstellen einer Anwendung über [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) wird die Anwendung bei dem Home-Mandanten des Kontos registriert, das Sie für die Anmeldung im Portal verwenden. Das bedeutet, dass Sie eine Anwendung nicht mit einem persönlichen Microsoft-Konto bei Ihrem Azure AD-Mandanten registrieren können. Wenn Sie eine Anwendung ausdrücklich bei einem bestimmten Mandanten registrieren möchten, müssen Sie sich mit einem Konto anmelden, das ursprünglich in diesem Mandanten erstellt wurde.


## <a name="build-a-quickstart-app"></a>Erstellen einer Schnellstart-App
Nachdem Sie eine Microsoft-App erstellt haben, können Sie eines der Schnellstarttutorials zum v2.0-Endpunkt absolvieren. Hier sind einige Vorschläge:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

