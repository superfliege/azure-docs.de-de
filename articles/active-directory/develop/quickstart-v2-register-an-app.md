---
title: Registrieren einer App mit dem Azure AD v2.0-Endpunkt | Microsoft Docs
description: Erfahren Sie, wie eine App bei Microsoft für die Anmeldung und den Zugriff auf Microsoft-Dienste mit dem v2.0-Endpunkt registriert wird.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/02/2018
ms.author: celested
ms.reviewer: lenalepa
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0e2980b48a5d9b1f2e9d75e59a27bfceb0d1180
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56195582"
---
# <a name="quickstart-register-an-app-with-the-azure-active-directory-v20-endpoint"></a>Schnellstart: Registrieren einer App beim Azure Active Directory v2.0-Endpunkt

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Um eine App zu erstellen, die die Anmeldung sowohl für ein persönliches Microsoft-Konto (MSA) als auch für ein Geschäfts-, Schul- oder Unikonto (Azure AD) zulässt, müssen Sie zunächst eine App mit dem Azure Active Directory (Azure AD) v2.0-Endpunkt registrieren. Derzeit können Sie keine in Azure AD oder MSA vorhandenen Apps nutzen, stattdessen müssen Sie eine neue App erstellen.

Nicht alle Szenarien und Features von Azure AD werden vom v2.0-Endpunkt unterstützt. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um zu bestimmen, ob Sie den v2.0-Endpunkt verwenden sollten.

> [!NOTE]
> Sie registrieren eine neue App? Probieren Sie die neue Benutzeroberfläche für **App-Registrierungen (Vorschauversion)** im Azure-Portal aus. Informationen zu den ersten Schritten finden Sie unter [Schnellstart: Registrieren einer Anwendung bei der Microsoft Identity Platform (Vorschauversion)](quickstart-register-app.md).

## <a name="step-1-sign-in-to-the-microsoft-application-registration-portal"></a>Schritt 1: Anmelden beim Anwendungsregistrierungsportal von Microsoft

1. Navigieren Sie zum Microsoft App-Registrierungsportal unter [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).
1. Melden Sie sich entweder mit einem persönlichen Microsoft-Konto oder einem Geschäfts- oder Schulkonto an. Wenn Sie noch kein Konto besitzen, melden Sie sich für ein neues persönliches Konto an.
1. Fertig? Jetzt sollten Sie Ihre Liste der Microsoft-Apps sehen, die wahrscheinlich noch leer ist. Ändern wir das.

## <a name="step-2-register-an-app"></a>Schritt 2: Registrieren einer App

1. Wählen Sie **App hinzufügen** aus, und geben Sie einen Namen für die App ein.
    Das Portal weist der App eine global eindeutige Anwendungs-ID zu, die Sie später in Ihrem Code verwenden. Wenn Ihre App eine serverseitige Komponente enthält, die Zugriffstoken zum Aufrufen von APIs benötigt (denken Sie an Office, Azure oder Ihre eigene Web-API), sollten Sie auch hier ein **Anwendungsgeheimnis** erstellen.
1. Fügen Sie dann die **Plattformen** hinzu, auf denen Ihre App verwendet wird.
    * Geben Sie bei webbasierten Apps einen **Umleitungs-URI** an, an den die Anmeldenachrichten gesendet werden können.
    * Notieren Sie sich den standardmäßigen Umleitungs-URI für mobile Apps, der automatisch erstellt wurde.
    * Bei Web-APIs wird für Sie automatisch ein Standardbereich für den Zugriff auf die Web-API erstellt.
        Mithilfe der Schaltfläche **Bereich hinzufügen** können Sie zusätzliche Bereiche hinzufügen. Über das Formular **Vorautorisierte Anwendungen** können Sie auch Anwendungen hinzufügen, die für die Verwendung der Web-API vorautorisiert sind.
1. Optional können Sie das Aussehen und Verhalten der Anmeldeseite im Abschnitt **Profil** anpassen. 
1. **Speichern Sie** Ihre Änderungen, bevor Sie fortfahren.

> [!NOTE]
> Beim Registrieren einer Anwendung über [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) wird die Anwendung bei dem Basismandanten des Kontos registriert, das Sie für die Anmeldung im Portal verwenden. Das bedeutet, dass Sie eine Anwendung nicht mit einem persönlichen Microsoft-Konto bei Ihrem Azure AD-Mandanten registrieren können. Wenn Sie eine Anwendung ausdrücklich bei einem bestimmten Mandanten registrieren möchten, müssen Sie sich mit einem Konto anmelden, das ursprünglich in diesem Mandanten erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun über eine Microsoft-App verfügen, können Sie einen der v2.0-Schnellstarts abschließen. Hier sind einige Vorschläge:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]
