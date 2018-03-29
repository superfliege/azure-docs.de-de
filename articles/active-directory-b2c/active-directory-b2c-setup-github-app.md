---
title: GitHub-Identitätsanbieterkonfiguration – Azure AD B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit GitHub-Konten in mit Azure Active Directory B2C gesicherten Anwendungen
services: active-directory-b2c
documentationcenter: ''
author: davidmu
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 02/06/2017
ms.author: davidmu
ms.openlocfilehash: 17ec4ebd8406f56c431666340ca8834c5ccf9670
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-github-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit GitHub-Konten

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase.
> 

In diesem Artikel erfahren Sie, wie Sie Benutzern die Anmeldung mit einem GitHub-Konto ermöglichen.

## <a name="create-a-github-oauth-application"></a>Erstellen einer GitHub-OAuth-Anwendung

Um GitHub als Identitätsanbieter in Azure AD B2C verwenden zu können, müssen Sie eine GitHub-OAuth-App erstellen und die entsprechenden Parameter bereitstellen.

1. Öffnen Sie nach der Anmeldung bei GitHub die [GitHub-Entwicklereinstellungen](https://github.com/settings/developers).
1. Klicken Sie auf **New OAuth App** (Neue OAuth-App).
1. Geben Sie einen **Anwendungsnamen** und Ihre **Homepage-URL** ein.
1. Geben Sie in **Authorization callback URL** (Autorisierungsrückruf-URL) `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Azure AD B2C-Mandanten (z.B. „B2C.onmicrosoft.com“).

    >[!NOTE]
    >Der Wert für „tenant“ muss in der **Sign-on URL** (Anmelde-URL) in Kleinbuchstaben angegeben werden.

1. Klicken Sie zum Registrieren der Anwendung auf **Register application**.
1. Speichern Sie die Werte für **Client ID** (Client-ID) und **Client Secret** (Geheimer Clientschlüssel). Sie benötigen beide Angaben im nächsten Abschnitt.

## <a name="configure-github-as-an-identity-provider-in-your-azure-ad-b2c-tenant"></a>Konfigurieren von GitHub als Identitätsanbieter in Ihrem Azure AD B2C-Mandanten

1. Führen Sie diese Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
1. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
1. Klicken Sie oben auf dem Blatt auf **+Hinzufügen**.
1. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „GitHub“ ein.
1. Klicken Sie auf **Identitätsanbietertyp**, wählen Sie **GitHub** aus, und klicken Sie auf **OK**.
1. Klicken Sie auf **Diesen Identitätsanbieter einrichten**, und geben Sie die Client-ID und den geheimen Clientschlüssel der GitHub-OAuth-Anwendung ein, die Sie zuvor kopiert haben.
1. Klicken Sie auf **OK** und dann auf **Erstellen**, um Ihre GitHub-Konfiguration zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Erstellen oder bearbeiten Sie eine [integrierte Richtlinie](active-directory-b2c-reference-policies.md), und fügen Sie GitHub als Identitätsanbieter hinzu.