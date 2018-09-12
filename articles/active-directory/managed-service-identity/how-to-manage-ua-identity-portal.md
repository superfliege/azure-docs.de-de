---
title: Verwalten einer vom Benutzer zugewiesenen verwalteten Identität über das Azure-Portal
description: Ausführliche Anweisungen zum Erstellen, Auflisten und Löschen einer vom Benutzer zugewiesenen verwalteten Identität
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 529daff80f8cfa0f9e4c9e64efd5648ae57e6e66
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345019"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-the-azure-portal"></a>Erstellen, Auflisten oder Löschen einer vom Benutzer zugewiesenen verwalteten Identität über das Azure-Portal

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität verwenden, um sich bei Diensten mit Unterstützung für die Azure AD-Authentifizierung zu authentifizieren, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

In diesem Artikel erfahren Sie, wie Sie im Azure-Portal eine vom Benutzer zugewiesene verwaltete Identität erstellen, auflisten und löschen.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel auszuführen, benötigt Ihr Konto die folgenden Rollenzuweisungen:
    - [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor), um eine vom Benutzer zugewiesene verwaltete Identität zu erstellen, zu lesen (aufzulisten), zu aktualisieren und zu löschen
    - [Operator für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-operator), um die Eigenschaften einer vom Benutzer zugewiesenen verwalteten Identität zu lesen (auflisten)

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer vom Benutzer zugewiesenen verwalteten Identität

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, um die vom Benutzer zugewiesene verwaltete Identität zu erstellen.
2. Geben Sie *Verwaltete Identitäten* im Suchfeld ein, und klicken Sie unter **Dienste** auf **Verwaltete Identitäten**.
3. Klicken Sie auf **Hinzufügen**, und geben Sie im Bereich **Vom Benutzer zugewiesene verwaltete Identität erstellen** in den folgenden Feldern Werte ein:
   - **Ressourcenname:** Dies ist der Name für die vom Benutzer zugewiesene verwaltete Identität, z.B. UAI1.
   - **Abonnement:** Wählen Sie das Abonnement aus, in dem die vom Benutzer zugewiesene verwaltete Identität erstellt werden soll.
   - **Ressourcengruppe:** Erstellen Sie für die vom Benutzer zugewiesene Identität eine neue Ressourcengruppe, oder wählen Sie **Vorhandene verwenden** aus, um die vom Benutzer zugewiesene verwaltete Identität in einer vorhandenen Ressourcengruppe zu erstellen.
   - **Standort:** Wählen Sie einen Standort zum Bereitstellen der vom Benutzer zugewiesenen verwalteten Identität aus, z.B. **USA, Westen**.
4. Klicken Sie auf **Create**.

![Erstellen einer vom Benutzer zugewiesenen verwalteten Identität](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-managed-identities"></a>Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, um die vom Benutzer zugewiesenen verwalteten Identitäten aufzulisten.
2. Geben Sie *Verwaltete Identitäten* im Suchfeld ein, und klicken Sie unter „Dienste“ auf **Verwaltete Identitäten**.
3. Eine Liste der vom Benutzer zugewiesenen verwalteten Identitäten für Ihr Abonnement wird zurückgegeben.  Durch Klicken auf den entsprechenden Namen können Sie die Details der vom Benutzer zugewiesenen verwalteten Identität anzeigen.

![Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Löschen einer vom Benutzer zugewiesenen verwalteten Identität

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, um eine vom Benutzer zugewiesene verwaltete Identität zu löschen.
2. Wählen Sie die vom Benutzer zugewiesene verwaltete Identität aus, und klicken Sie auf **Löschen**.
3. Wählen Sie im Bestätigungsfeld **Ja** aus.

![Löschen einer vom Benutzer zugewiesenen verwalteten Identität](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)