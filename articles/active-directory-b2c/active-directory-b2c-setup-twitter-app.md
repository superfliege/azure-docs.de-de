---
title: Einrichten der Registrierung und Anmeldung mit einem Twitter-Konto mithilfe von Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Twitter-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: a07f138461759ff0871df0c677ef36e0fb36ae34
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703966"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Twitter-Konto mithilfe von Azure Active Directory B2C

## <a name="create-an-application"></a>Erstellen einer Anwendung

Um Twitter als Identitätsanbieter in Azure AD B2C zu nutzen, müssen Sie eine Twitter-Anwendung erstellen. Wenn Sie noch über kein Twitter-Konto verfügen, können Sie eines unter [https://twitter.com/signup](https://twitter.com/signup) erstellen.

1. Melden Sie sich auf der Website für [Twitter-Entwickler](https://developer.twitter.com/en/apps) mit den Anmeldeinformationen für Ihr Twitter-Konto an.
2. Wählen Sie **Create an app** (App erstellen) aus.
3. Geben Sie in **App name** einen App-Namen und in **Application description** eine Anwendungsbeschreibung ein.
4. Gegen Sie unter der **Website-URL** `https://your-tenant.b2clogin.com` ein. Ersetzen Sie `your-tenant` durch den Namen Ihres Mandanten. Beispiel: https://contosob2c.b2clogin.com.
5. Geben Sie `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/your-user-flow-Id/oauth1/authresp` in **Callback URL** ein. Ersetzen Sie `your-tenant` durch den Namen Ihres Mandanten und `your-user-flow-Id` durch den Bezeichner Ihres Benutzerflows. Beispiel: `b2c_1A_signup_signin_twitter`. Bei der Eingabe Ihres Mandantennamens dürfen Sie nur Kleinbuchstaben verwenden, auch wenn der Mandant in Azure AD B2C Großbuchstaben enthält.
6. Lesen und akzeptieren Sie die Nutzungsbedingungen am Ende der Seite, und wählen Sie dann **Create** (Erstellen) aus.
7. Wählen Sie auf der Seite **App details** (Anwendungsdetails) **Edit > Edit details** (Bearbeiten > Details bearbeiten), aktivieren Sie das Kontrollkästchen für **Enable Sign in with Twitter** (Anmeldung bei Twitter aktivieren), und wählen Sie dann **Save** (Speichern).
8. Wählen Sie **Keys and tokens** (Schlüssel und Token) aus, und notieren Sie die Werte für **Consumer API Key** (Consumer-API-Schlüssel) und **Consumer API secret key** (Geheimer Consumer-API-Schlüssel) für die spätere Verwendung.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Twitter als Identitätsanbieter in Ihrem Mandanten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** an. Geben Sie z.B. *Twitter* ein.
6. Wählen Sie **Identitätsanbietertyp** und dann **Twitter** aus, und klicken Sie auf **OK**.
7. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie den API-Schlüssel als **Client-ID** und den geheimen API-Schlüssel als **Geheimer Clientschlüssel** ein.
8. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Twitter-Konfiguration zu speichern.
