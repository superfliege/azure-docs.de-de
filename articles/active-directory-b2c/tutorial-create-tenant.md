---
title: Tutorial zum Erstellen eines Azure Active Directory B2C-Mandanten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie sich auf die Registrierung Ihrer Anwendungen vorbereiten können, indem Sie einen Azure Active Directory B2C-Mandanten über das Azure-Portal erstellen.
services: B2C
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7571e5f4d95320ab92fa3b69b0ea1f05ff9c771f
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408401"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial zum Erstellen eines Azure Active Directory B2C-Mandanten

Bevor Ihre Anwendungen mit Azure Active Directory (Azure AD) B2C interagieren können, müssen sie in einem von Ihnen verwalteten Mandanten registriert werden.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen eines Azure AD B2C-Mandanten
> * Verknüpfen Ihres Mandanten mit Ihrem Abonnement

Im nächsten Tutorial erfahren Sie, wie Sie eine Anwendung registrieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-azure-ad-b2c-tenant"></a>Erstellen eines Azure AD B2C-Mandanten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihr Abonnement enthält, indem Sie im Hauptmenü auf den **Verzeichnis- und Abonnementfilter** klicken und das Verzeichnis auswählen, das es enthält. Dabei handelt es sich um ein anderes Verzeichnis als das, in dem Ihr Azure AD B2C-Mandant enthalten sein wird.

    ![Wechseln zum Abonnementverzeichnis](./media/tutorial-create-tenant/switch-directory-subscription.png)

3. Wählen Sie im Azure-Portal oben links die Option **Ressource erstellen** aus.
4. Suchen Sie nach **Active Directory B2C**, wählen Sie dies aus, und klicken Sie dann auf **Erstellen**.
5. Wählen Sie **Neuen Azure AD B2C-Mandanten erstellen** aus, geben Sie einen Organisationsnamen und einen anfänglichen Domänennamen ein, der im Mandantennamen verwendet wird, wählen Sie das Land aus (dieses kann später nicht mehr geändert werden), und klicken Sie dann auf **Erstellen**.

    ![Erstellen eines Mandanten](./media/tutorial-create-tenant/create-tenant.png)

    In diesem Beispiel lautet der Mandantenname „contoso0926Tenant.onmicrosoft.com“.

6. Wählen Sie auf der Seite **Neuen B2C-Mandanten erstellen oder mit vorhandenem Mandanten verknüpfen** die Option **Vorhandenen Azure AD B2C-Mandanten mit meinem Azure-Abonnement verknüpfen** aus. Wählen Sie dann den Mandanten, den Sie erstellt haben, und Ihr Abonnement aus, und klicken Sie auf **Neu erstellen**. Geben Sie einen Namen für die Ressourcengruppe ein, die den Mandanten enthalten wird, wählen Sie den Speicherort aus, und klicken Sie dann auf **Erstellen**.
7. Stellen Sie zum Verwenden Ihres neuen Mandanten sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das Verzeichnis auswählen, das ihn enthält.

    ![Wechseln zum Mandantenverzeichnis](./media/tutorial-create-tenant/switch-directories.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Azure AD B2C-Mandanten
> * Verknüpfen Ihres Mandanten mit Ihrem Abonnement

> [!div class="nextstepaction"]
> [Aktivieren einer Webanwendung für die Authentifizierung mit Konten](active-directory-b2c-tutorials-web-app.md)