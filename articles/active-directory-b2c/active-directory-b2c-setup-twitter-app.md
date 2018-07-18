---
title: Twitter-Konfiguration für Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Twitter-Konten in mit Azure Active Directory B2C geschützten Anwendungen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 4/17/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 68b9e9edbacda08bc98b6b7af5d944d3df9edca1
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34709579"
---
# <a name="provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts-using-azure-ad-b2c"></a>Bereitstellen von Registrierung und Anmeldung für Kunden mit Twitter-Konten mithilfe von Azure AD B2C

## <a name="create-a-twitter-application"></a>Erstellen einer Twitter-Anwendung
Um Twitter als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Twitter-Anwendung erstellen und die entsprechenden Parameter bereitstellen. Sie benötigen dazu ein Twitter-Konto. Wenn Sie keins besitzen, können Sie unter [https://twitter.com/signup](https://twitter.com/signup) eines erstellen.

1. Wechseln Sie zur Website [Twitter Apps](https://apps.twitter.com/), und melden Sie sich mit Ihren Anmeldeinformationen an.
2. Klicken Sie auf **Create New App**. 
3. Geben Sie im Formular einen Wert für **Name**, **Description** und **Website** ein.
4. Geben Sie `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in **Callback URL** ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. contosob2c.onmicrosoft.com).
5. Aktivieren Sie das Kontrollkästchen, um **Developer Agreement** zuzustimmen, und klicken Sie auf **Create your Twitter application**.
6. Nachdem die App erstellt wurde, wählen Sie sie in der Liste aus, und klicken Sie dann auf die Registerkarte **Einstellungen**.
7. Deaktivieren Sie das Feld **Enable Callback Locking** (Rückrufsperren aktivieren), und klicken Sie dann auf **Update settings** (Einstellungen aktualisieren).
8. Wählen Sie die Registerkarte **Keys and Access Tokens** .
9. Kopieren Sie den Wert von **Consumer Key** und **Consumer Secret**. Sie benötigen beide Angaben, um Twitter als Identitätsanbieter in Ihrem Mandanten zu konfigurieren.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Twitter als Identitätsanbieter in Ihrem Mandanten
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator des Azure AD B2C-Mandanten an. 
2. Um zu Ihrem Azure AD B2C-Mandanten zu wechseln, wählen Sie das Azure AD B2C-Verzeichnis rechts oben im Portal aus.
3. Klicken Sie auf **Alle Dienste**, und wählen Sie dann **Azure AD B2C** in der Liste der Dienste unter **Sicherheit + Identität** aus.
4. Klicken Sie auf **Identitätsanbieter**.
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „Twitter“ ein.
5. Klicken Sie auf **Identitätsanbietertyp**, wählen Sie **Twitter (Vorschauversion)** aus, und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**, und geben Sie den **Consumer Key** von Twitter als **Client-ID** und das **Consumer Secret** von Twitter als **Clientschlüssel** ein.
7. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Twitter-Konfiguration zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Erstellen oder bearbeiten Sie eine [integrierte Richtlinie](active-directory-b2c-reference-policies.md), und fügen Sie Twitter als Identitätsanbieter hinzu.