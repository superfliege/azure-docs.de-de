---
title: 'Azure Active Directory B2C: Konfiguration für Twitter | Microsoft-Dokumentation'
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Twitter-Konten in mit Azure Active Directory B2C geschützten Anwendungen.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 4/06/2017
ms.author: davidmu
ms.openlocfilehash: ee2d82f8c90b88a898428973a1febaa21034a14f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit Twitter-Konten

## <a name="create-a-twitter-application"></a>Erstellen einer Twitter-Anwendung
Um Twitter als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Twitter-Anwendung erstellen und die entsprechenden Parameter bereitstellen. Sie benötigen dazu ein Twitter-Entwicklerkonto. Wenn Sie keins besitzen, können Sie unter [https://dev.twitter.com/](https://dev.twitter.com/) eines erstellen.

1. Wechseln Sie zur [Website für Twitter-Entwickler](https://dev.twitter.com/), und melden Sie sich mit Ihren Anmeldeinformationen an.
2. Klicken Sie unter **Tools & Support** auf **My Apps** und dann auf **Create New App**. 
3. Geben Sie im Formular einen Wert für **Name**, **Description** und **Website** ein.
4. Geben Sie `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in **Callback URL** ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. contosob2c.onmicrosoft.com).
5. Aktivieren Sie das Kontrollkästchen, um **Developer Agreement** zuzustimmen, und klicken Sie auf **Create your Twitter application**.
6. Nachdem die App erstellt wurde, klicken Sie auf die Registerkarte **Keys and Access Tokens**.
7. Kopieren Sie den Wert von **Consumer Key** und **Consumer Secret**. Sie benötigen beide Angaben, um Twitter als Identitätsanbieter in Ihrem Mandanten zu konfigurieren.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Twitter als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie die folgenden Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) .
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen**.
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „Twitter“ ein.
5. Klicken Sie auf **Identitätsanbietertyp**, wählen Sie **Twitter** aus, und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**, und geben Sie den **Consumer Key** von Twitter als **Client-ID** und das **Consumer Secret** von Twitter als **Clientschlüssel** ein.
7. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Twitter-Konfiguration zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Erstellen oder bearbeiten Sie eine [integrierte Richtlinie](active-directory-b2c-reference-policies.md), und fügen Sie Twitter als Identitätsanbieter hinzu.