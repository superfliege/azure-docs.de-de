---
title: "Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie den Benutzerzugriff, z.B. die Mitgliedschaft in einer Gruppe oder die Zuweisung zu einer Anwendung, mit Azure Active Directory-Zugriffsüberprüfungen verwalten."
services: active-directory
documentationcenter: 
author: markwahl-msft
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 0459eb5cc71939202c8491f6b2714e28bd8e202d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-user-access-with-azure-ad-access-reviews"></a>Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen

Mit Azure Active Directory können Sie problemlos sicherstellen, dass Benutzer über benötigte Zugriffsrechte verfügen, indem Sie die Benutzer selbst oder einen Entscheidungsträger bitten, eine Zugriffsüberprüfung durchzuführen und den Zugriff zu bestätigen.  Basierend auf Vorschlägen von Azure AD können die Prüfer die Notwendigkeit des weiteren Zugriffs der einzelnen Benutzer abwägen. Nach Abschluss einer Zugriffsüberprüfung können Sie dann Änderungen vornehmen und Zugriffsrechte für Benutzer entfernen, die diese nicht mehr benötigen.

> [!NOTE]
> Wenn Sie nur den Zugriff von Gastbenutzern und nicht aller Benutzertypen überprüfen möchten, finden Sie entsprechende Informationen unter [Verwalten des Gastbenutzerzugriffs mit Zugriffsüberprüfungen](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md).  Wenn Sie die Benutzermitgliedschaft in Administratorrollen wie „Globaler Administrator“ überprüfen möchten, finden Sie entsprechende Informationen unter [Starten einer Zugriffsüberprüfung in Azure AD PIM](active-directory-privileged-identity-management-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Voraussetzungen 

Zugriffsüberprüfungen sind mit der Premium P2-Edition von Azure Active Directory (in EMS E5 enthalten) verfügbar. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).  Jeder Benutzer, der dieses Feature zum Erstellen, Durchführen oder Anwenden einer Zugriffsüberprüfung verwendet, benötigt eine Lizenz.


## <a name="creating-and-performing-an-access-review"></a>Erstellen und Durchführen einer Zugriffsüberprüfung

Sie können einen oder mehrere Benutzer als Prüfer einer Zugriffsüberprüfung festlegen.  

1. Wählen Sie eine Gruppe in Azure Active Directory mit einem oder mehreren Mitgliedern oder eine mit Azure Active Directory verbundene Anwendung aus, der ein oder mehrere Benutzer zugewiesen sind. 
2. Legen Sie fest, ob jeder Benutzer seinen eigenen Zugriff oder ob ein oder mehrere Benutzer den Zugriff aller Benutzer überprüfen sollen.
3. Aktivieren Sie die Zugriffsüberprüfungen, sodass sie in den Zugriffsbereichen der Prüfer angezeigt werden.  Navigieren Sie als globaler Administrator zur Seite [Zugriffsüberprüfungen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/). 
4. Starten Sie die Zugriffsüberprüfung. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-create-access-review.md).
5. Bitten Sie die Prüfer, ihre Einschätzung abzugeben. Standardmäßig erhalten alle Prüfer eine E-Mail von Azure AD mit einem Link zu dem Zugriffsbereich, in dem sie [die Zugriffsüberprüfung durchführen](active-directory-azure-ad-controls-perform-access-review.md).
6. Wenn die Prüfer keine Angaben gemacht haben, kann Azure AD ihnen eine Erinnerung senden.  Standardmäßig sendet Azure AD automatisch eine Erinnerung, wenn die Prüfer nach der Hälfte der Zeit noch nicht reagiert haben.
7. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden und die Änderungen anwenden. Weitere Informationen finden Sie unter [Abschließen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-complete-access-review.md).


## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Zugriffsüberprüfung für Mitglieder einer Gruppe oder den Zugriff auf eine Anwendung](active-directory-azure-ad-controls-create-access-review.md)




