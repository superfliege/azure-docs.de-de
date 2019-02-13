---
title: Hinzufügen einer Webanwendung – Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihrem Active Directory B2C-Mandanten eine Webanwendung hinzufügen.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: c20f455a0a325dadd3eeeb77dea7026de4834c56
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55757322"
---
# <a name="add-a-web-api-application-to-your-azure-active-directory-b2c-tenant"></a>Hinzufügen einer Web-API-Anwendung zu Ihrem Azure Active Directory B2C-Mandanten

Web-API-Ressourcen müssen bei Ihrem Mandanten registriert werden, damit sie geschützte Ressourcenanforderungen von Clientanwendungen, die ein Zugriffstoken bereitstellen, akzeptieren und darauf reagieren können.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
3. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
4. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
5. Geben Sie einen Namen für die Anwendung ein. Beispiel: *webapi1*.
6. Wählen Sie für **Web-App/Web-API einschließen** und n**Impliziten Ablauf zulassen** die Option **Ja**.
7. Geben Sie für **Antwort-URLs** einen Endpunkt ein, an den Azure AD B2C von Ihrer App angeforderte Token zurückgibt. Das Beispiel in diesem Tutorial wird lokal ausgeführt und lauscht an `https://localhost:44332`.
8. Geben Sie für **App-ID-URI** den für Ihre Web-API verwendeten Bezeichner ein. Der vollständige Bezeichner-URI (einschließlich der Domäne) wird für Sie generiert. Beispiel: `https://contosotenant.onmicrosoft.com/api`.
9. Klicken Sie auf **Create**.
10. Notieren Sie sich auf der Eigenschaftenseite die Anwendungs-ID, die Sie beim Konfigurieren der Webanwendung verwenden.

## <a name="configure-scopes"></a>Konfigurieren von Bereichen

Bereiche ermöglichen die Steuerung des Zugriffs auf geschützte Ressourcen. Bereiche werden von der Web-API verwendet, um eine bereichsbasierte Zugriffssteuerung zu implementieren. Benutzer der Web-API können beispielsweise über Lese- und Schreibzugriff oder nur über Lesezugriff verfügen. In diesem Tutorial verwenden Sie Bereiche zum Definieren von Lese- und Schreibberechtigungen für die Web-API.

1. Wählen Sie **Anwendungen** und dann *webapi1* aus.
2. Wählen Sie **Veröffentlichte Bereiche** aus.
3. Geben Sie unter **Bereich** den Bereich `Hello.Read` und als Beschreibung `Read access to hello` ein.
4. Geben Sie unter **Bereich** den Bereich `Hello.Write` und als Beschreibung `Write access to hello` ein.
5. Klicken Sie auf **Speichern**.

Mit den veröffentlichten Bereichen können einer Clientanwendung Berechtigungen für die Web-API gewährt werden.

## <a name="grant-permissions"></a>Erteilen von Berechtigungen

Wenn Sie über eine Anwendung eine geschützte Web-API aufrufen möchten, müssen Sie Ihrer Anwendung Berechtigungen für die API erteilen. Im vorbereitenden Tutorial haben Sie in Azure AD B2C eine Webanwendung namens *webapp1* erstellt. Sie verwenden diese Anwendung für den Aufruf der Web-API.

1. Wählen Sie **Anwendungen** und anschließend Ihre Webanwendung aus.
2. Wählen Sie **API-Zugriff** und dann **Hinzufügen** aus.
3. Wählen Sie in der Dropdownliste **API auswählen** die Web-API *webapi1* aus.
4. Wählen Sie in der Dropdownliste **Bereiche auswählen** die Bereiche **Hello.Read** und **Hello.Write** aus, die Sie zuvor festgelegt haben.
5. Klicken Sie auf **OK**.

Ihre Anwendung ist für den Aufruf der geschützten Web-API registriert. Ein Benutzer authentifiziert sich mit Azure AD B2C, um die Anwendung zu verwenden. Die Anwendung bezieht eine Autorisierungsgewährung von Azure AD B2C, um auf die geschützte Web-API zuzugreifen.