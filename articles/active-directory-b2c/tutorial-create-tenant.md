---
title: Tutorial zum Erstellen eines Azure Active Directory B2C-Mandanten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie sich auf die Registrierung Ihrer Anwendungen vorbereiten können, indem Sie einen Azure Active Directory B2C-Mandanten über das Azure-Portal erstellen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 06/19/2018
ms.author: davidmu
ms.openlocfilehash: 04f3dbbe461bfe0f07b6930a92bdd8a721e55098
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296787"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial zum Erstellen eines Azure Active Directory B2C-Mandanten

Bevor Ihre Anwendungen mit Azure Active Directory (Azure AD) B2C interagieren können, müssen sie in einem von Ihnen verwalteten Mandanten registriert werden.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen eines Azure AD B2C-Mandanten
> * Verknüpfen Ihres Mandanten mit Ihrem Abonnement

Im nächsten Tutorial erfahren Sie, wie Sie eine Anwendung registrieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-an-azure-ad-b2c-tenant"></a>Erstellen eines Azure AD B2C-Mandanten

1. Wählen Sie im Azure-Portal oben links die Option **Ressource erstellen** aus.
2. Suchen Sie im Suchfeld oberhalb der Liste der Azure Marketplace-Ressourcen nach **Active Directory B2C**, und klicken Sie auf **Erstellen**.
3. Wählen Sie **Neuen Azure AD B2C-Mandanten erstellen** aus, geben Sie einen Organisationsnamen und einen anfänglichen Domänennamen ein, der im Mandantennamen verwendet wird, wählen Sie das Land aus, und klicken Sie dann auf **Erstellen**. Stellen Sie sicher, dass Sie das richtige Land des Mandanten auswählen, da es später nicht mehr geändert werden kann.

    ![Erstellen eines Mandanten](./media/tutorial-create-tenant/create-tenant.png)

    In diesem Beispiel lautet der Mandantenname „contoso0522Tenant.onmicrosoft.com“.

**Klicken Sie hier** zu Beginn der Verwaltung Ihres neuen Mandanten, wo es heißt **Hier klicken, um Ihr neues Verzeichnis zu verwalten**. Es wird eine Meldung zur **Problembehandlung** angezeigt, die darauf hinweist, dass Sie Ihr Abonnement mit dem neuen Mandanten verknüpfen müssen. 

## <a name="link-your-tenant-to-your-subscription"></a>Verknüpfen Ihres Mandanten mit Ihrem Abonnement

Sie müssen Ihren Azure AD B2C-Mandanten mit Ihrem Azure-Abonnement verknüpfen, um die gesamte Funktionalität zu aktivieren und die Nutzungsgebühren zahlen zu können. Wenn Sie den Mandanten nicht mit Ihrem Abonnement verknüpfen, funktionieren Ihre Anwendungen nicht ordnungsgemäß.

1. Vergewissern Sie sich, dass Sie das Verzeichnis mit dem Abonnement verwenden, mit dem Sie den neuen Mandanten verknüpfen möchten. Wechseln Sie hierzu oben rechts im Azure-Portal zu diesem Verzeichnis.

    ![Wechseln von Verzeichnissen](./media/tutorial-create-tenant/switch-directories.png)

    Wählen Sie dann das Verzeichnis aus, das Ihr Abonnement enthält.

    ![Auswählen des Verzeichnisses](./media/tutorial-create-tenant/select-directory.png)

2. Wählen Sie im Azure-Portal oben links die Option **Ressource erstellen** aus.
3. Suchen Sie im Suchfeld oberhalb der Liste der Azure Marketplace-Ressourcen nach **Active Directory B2C**, und klicken Sie auf **Erstellen**.
4. Klicken Sie auf **Vorhandenen Azure AD B2C-Mandanten mit meinem Azure-Abonnement verknüpfen** aus, wählen Sie den erstellten Mandanten und Ihr Abonnement aus, geben Sie *myContosoTenantRG* als Ressourcengruppennamen ein, bestätigen Sie den Speicherort, und klicken Sie dann auf **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Azure AD B2C-Mandanten
> * Verknüpfen Ihres Mandanten mit Ihrem Abonnement

> [!div class="nextstepaction"]
> [Aktivieren einer Webanwendung für die Authentifizierung mit Konten](active-directory-b2c-tutorials-web-app.md)