---
title: Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Benutzerzugriff als Mitgliedschaft in einer Gruppe oder als Zuweisung zu einer Anwendung mit Azure Active Directory-Zugriffsüberprüfungen verwalten.
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
ms.subservice: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: b53fd8b53b85525a3105cfee3594cd284e3838a8
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182152"
---
# <a name="manage-user-access-with-azure-ad-access-reviews"></a>Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen

Mit Azure Active Directory (Azure AD) können Sie ganz einfach den entsprechenden Zugriff von Benutzern sicherstellen. Hierzu können Sie die Benutzer selbst oder einen Entscheidungsträger bitten, an einer Zugriffsüberprüfung teilzunehmen und den Zugriff des Benutzers erneut zu zertifizieren (oder zu „attestieren“). Basierend auf Vorschlägen von Azure AD können die Prüfer die Notwendigkeit des weiteren Zugriffs der einzelnen Benutzer abwägen. Nach Abschluss einer Zugriffsüberprüfung können Sie dann Änderungen vornehmen und Zugriffsrechte für Benutzer entfernen, die diese nicht mehr benötigen.

> [!NOTE]
> Wenn Sie nur den Zugriff von Gastbenutzern und nicht aller Benutzertypen überprüfen möchten, finden Sie entsprechende Informationen unter [Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen](manage-guest-access-with-access-reviews.md). Wenn Sie die Benutzermitgliedschaft in Administratorrollen wie „Globaler Administrator“ überprüfen möchten, finden Sie entsprechende Informationen unter [Starten einer Zugriffsüberprüfung in Azure AD Privileged Identity Management](../privileged-identity-management/pim-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Voraussetzungen 


Zugriffsüberprüfungen sind mit der Premium P2-Edition von Azure AD (in Microsoft Enterprise Mobility + Security E5 enthalten) verfügbar. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](../fundamentals/active-directory-whatis.md). Jeder Benutzer, der mit diesem Feature interagiert (einschließlich Erstellen oder Ausfüllen einer Überprüfung bzw. Bestätigen des Zugriffs), benötigt eine Lizenz. 

## <a name="create-and-perform-an-access-review"></a>Erstellen und Durchführen einer Zugriffsüberprüfung

Sie können einen oder mehrere Benutzer als Prüfer einer Zugriffsüberprüfung festlegen.  

1. Wählen Sie eine Gruppe in Azure AD mit mindestens einem Mitglied aus. Oder wählen Sie eine mit Azure AD verbundene Anwendung aus, der mindestens ein Benutzer zugewiesen ist. 

2. Legen Sie fest, ob jeder Benutzer seinen eigenen Zugriff oder ob ein oder mehrere Benutzer den Zugriff aller Benutzer überprüfen soll/sollen.

3. Als globaler Administrator oder Benutzerkontoadministrator navigieren Sie zur Seite [Zugriffsüberprüfungen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

4. Erstellen Sie die Zugriffsüberprüfung. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung von Gruppenmitgliedern oder dem Anwendungszugriff mit Azure AD](create-access-review.md).

5. Bitten Sie die Prüfer zu Beginn der Zugriffsüberprüfung, ihre Einschätzung abzugeben. Standardmäßig erhalten alle Prüfer eine E-Mail von Azure AD mit einem Link zu dem Zugriffsbereich, in dem sie [die Zugriffsüberprüfung durchführen](perform-access-review.md).

6. Wenn die Prüfer keine Angaben gemacht haben, kann Azure AD ihnen eine Erinnerung senden. Standardmäßig sendet Azure AD automatisch eine Erinnerung, wenn die Prüfer nach der Hälfte der Zeit noch nicht reagiert haben.

7. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden und die Änderungen anwenden. Weitere Informationen finden Sie unter [Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD](complete-access-review.md).


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Zugriffsüberprüfung für Mitglieder einer Gruppe oder den Zugriff auf eine Anwendung](create-access-review.md)




