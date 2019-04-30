---
title: Konfigurieren von Token – Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Lebensdauer der Token und die Kompatibilitätseinstellungen in Azure Active Directory B2C konfigurieren.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 2455b8c12ed042af3a06a158a4e5c60a0aee748b
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59686136"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Konfigurieren von Token in Azure Active Directory B2C

In diesem Artikel erfahren Sie, wie Sie die [Lebensdauer und Kompatibilität eines Token](active-directory-b2c-reference-tokens.md) in Azure Active Directory (Azure AD) B2C konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie einen Benutzerflow](tutorial-create-user-flows.md), damit sich Benutzer bei Ihrer Anwendung registrieren und anmelden können.

## <a name="configure-token-lifetime"></a>Konfigurieren der Tokenlebensdauer

Sie können die Lebensdauer des Tokens für den beliebigen Benutzerflow konfigurieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält. Wählen Sie im Hauptmenü den **Verzeichnis- und Abonnementfilter** aus, und wählen Sie das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
3. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
4. Wählen Sie **Benutzerflows (Richtlinien)** aus.
5. Öffnen Sie den Benutzerflow, den Sie zuvor erstellt haben. 
6. Wählen Sie **Eigenschaften** aus.
7. Passen Sie unter **Tokenlebensdauer** die folgenden Eigenschaften an die Anforderungen Ihrer Anwendung an:

    ![Konfigurieren der Tokenlebensdauer](./media/configure-tokens/token-lifetime.png)

8. Klicken Sie auf **Speichern**.

## <a name="configure-token-compatibility"></a>Konfigurieren der Tokenkompatibilität

1. Wählen Sie **Benutzerflows (Richtlinien)** aus.
2. Öffnen Sie den Benutzerflow, den Sie zuvor erstellt haben. 
3. Wählen Sie **Eigenschaften** aus.
4. Passen Sie unter **Tokenkompatibilitätseinstellungen** die folgenden Eigenschaften an die Anforderungen Ihrer Anwendung an:

    ![Konfigurieren der Tokenkompatibilität](./media/configure-tokens/token-compatibility.png)

5. Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Verwenden von Zugriffstoken](active-directory-b2c-access-tokens.md).



