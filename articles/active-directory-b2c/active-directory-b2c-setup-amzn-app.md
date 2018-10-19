---
title: Einrichten der Registrierung und Anmeldung mit einem Amazon-Konto mithilfe von Azure Active Directory B2C | Microsoft-Dokumentation
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Amazon-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5bc1e0faf01125f498eb339bba841b96f6de9d82
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181803"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Amazon-Konto mithilfe von Azure Active Directory B2C

## <a name="create-an-amazon-application"></a>Erstellen einer Amazon-Anwendung

Um ein Amazon-Konto als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Anwendung in Ihrem Mandanten erstellen, die es darstellt. Wenn Sie noch über kein Amazon-Konto verfügen, können Sie eines unter [http://www.amazon.com/](http://www.amazon.com/) erstellen.

1. Melden Sie sich beim [Amazon Developer Center](https://login.amazon.com/) mit den Anmeldeinformationen für Ihr Amazon-Konto an.
2. Wenn Sie dies nicht bereits getan haben, klicken Sie auf **Sign Up**, führen Sie die Schritte zur Registrierung von Entwicklern aus, und akzeptieren Sie die Richtlinie.
3. Wählen Sie **Register new application** (Neue Anwendung registrieren) aus.
4. Geben Sie einen **Namen**, unter **Description** eine Beschreibung und unter **Privacy Notice URL** eine URL für den Datenschutzhinweis ein, und klicken Sie dann auf **Save** (Speichern). Die Seite „Datenschutzhinweise“ wird von Ihnen verwaltet und bietet Benutzern Informationen zum Datenschutz.
5. Kopieren Sie im Abschnitt **Web Settings** (Webeinstellungen) den Wert für **Client ID** (Client-ID). Wählen Sie **Show Secret** (Geheimnis anzeigen) aus, um den geheimen Clientschlüssel abzurufen, und kopieren Sie ihn dann. Sie benötigen beide Angaben, um ein Amazon-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. **geheime Clientschlüssel** ist eine wichtige Sicherheitsanmeldeinformation.
6. Wählen Sie im Abschnitt **Web Settings** (Webeinstellungen) die Option **Edit** (Bearbeiten) aus, und geben Sie dann `https://your-tenant-name.b2clogin.com` unter **Allowed JavaScript Origins** (Zulässige JavaScript-Quellen) und `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` unter **Allowed Return URLs** (Zulässige Rückgabe-URIs) ein. Ersetzen Sie `your-tenant-name` durch den Namen Ihres Mandanten. Bei der Eingabe Ihres Mandantennamens dürfen Sie nur Kleinbuchstaben verwenden, auch wenn der Mandant in Azure AD B2C Großbuchstaben enthält.
7. Klicken Sie auf **Speichern**.

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Konfigurieren eines Amazon-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** ein. Geben Sie z.B. *Amazon* ein.
6. Wählen Sie **Identitätsanbietertyp** und dann **Amazon** aus, und klicken Sie auf **OK**.
7. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie die zuvor notierte Client-ID als **Client-ID** und das notierte Clientgeheimnis als **Clientgeheimnis** der zuvor erstellten Amazon-Anwendung ein.
8. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Amazon-Konfiguration zu speichern.

