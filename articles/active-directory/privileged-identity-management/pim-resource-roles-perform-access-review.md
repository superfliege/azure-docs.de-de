---
title: Ausführen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Zugriffsüberprüfung für Ihre Azure-Ressourcenrollen in Azure AD Privileged Identity Management (PIM) durchführen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 3b9c998351432a591ece0a61d60256aea89a6f8f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180890"
---
# <a name="perform-an-access-review-of-my-azure-resource-roles-in-pim"></a>Ausführen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM
Privileged Identity Management (PIM) für Azure-Ressourcen vereinfacht für Unternehmen die Verwaltung von Zugriffsberechtigungen für Ressourcen in Azure. 

Wenn Ihnen eine Administratorrolle zugewiesen wurde, werden Sie vom Administrator für privilegierte Rollen Ihrer Organisation möglicherweise gebeten, regelmäßig zu bestätigen, dass Sie diese Rolle für Ihre Aufgaben benötigen. Sie erhalten möglicherweise eine E-Mail mit einem Link, oder Sie können direkt zum [Azure-Portal](https://portal.azure.com)wechseln. Führen Sie die Schritte in diesem Artikel aus, um die Ihnen zugewiesenen Rollen selbst zu überprüfen.

Wenn Sie Administrator für privilegierte Rollen sind und sich für die Zugriffsüberprüfungen interessieren, erhalten Sie weitere Informationen unter [Starten einer Zugriffsüberprüfung](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Hinzufügen der Anwendung Privileged Identity Management
Sie können die Anwendung Azure Active Directory PIM (Azure AD) im [Azure-Portal](https://portal.azure.com/) verwenden, um die Überprüfung durchzuführen. Wenn die Anwendung in Ihrem Portal nicht vorhanden ist, führen Sie folgende Schritte aus, um zu beginnen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie in der oberen rechten Ecke des Azure-Portals Ihren Benutzernamen, und wählen Sie das Verzeichnis aus, in dem Sie arbeiten möchten.
3. Wählen Sie **Alle Dienste** aus, und verwenden Sie das Textfeld **Filter**, um nach *Azure AD Privileged Identity Management* zu suchen.
4. Aktivieren Sie **An Dashboard anheften**, und wählen Sie dann **Erstellen** aus. Die PIM-Anwendung wird geöffnet.

## <a name="approve-or-deny-access"></a>Genehmigen oder Verweigern des Zugriffs
Wenn Sie Zugriff genehmigen oder verweigern, teilen Sie dem Prüfer lediglich mit, ob Sie diese Rolle weiterhin verwenden. Wählen Sie **Genehmigen** aus, wenn Sie in der Rolle bleiben möchten, oder **Verweigern**, wenn Sie den Zugriff nicht mehr benötigen. Ihr Status ändert sich nur, wenn der Prüfer die Ergebnisse anwendet.

Gehen Sie wie folgt vor, um die Zugriffsüberprüfung zu finden und abzuschließen:
1. Navigieren Sie zur Azure AD PIM-Anwendung.
2. Wählen Sie das Blatt **Zugriff überprüfen** aus.

   ![Screenshot der PIM-Anwendung mit ausgewähltem Blatt „Zugriff überprüfen“](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Wählen Sie die Überprüfung aus, die Sie abschließen möchten. 
4. Wählen Sie entweder **Genehmigen** oder **Verweigern** aus. Möglicherweise müssen Sie im Feld **Grund angeben** einen Grund für Ihre Entscheidung eingeben.

   ![Screenshot der Seite „Details überprüfen“](media/azure-pim-resource-rbac/rbac-access-review-choice.png)

## <a name="next-steps"></a>Nächste Schritte

- [Ausführen einer Zugriffsüberprüfung für Azure AD-Verzeichnisrollen in PIM](pim-how-to-perform-security-review.md)
