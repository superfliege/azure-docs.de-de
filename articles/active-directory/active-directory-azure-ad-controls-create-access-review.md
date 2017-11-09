---
title: "Erstellen einer Zugriffsüberprüfung für Mitglieder einer Gruppe oder Benutzer mit Zugriff auf eine Anwendung mit Azure AD | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine Zugriffsüberprüfung für Mitglieder einer Gruppe oder Benutzer mit Zugriff auf eine Anwendung erstellen."
services: active-directory
author: markwahl-msft
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: ae926e8d3a856c9b488d46b481a1c7b5b581d532
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Erstellen einer Zugriffsüberprüfung von Gruppenmitgliedern oder dem Anwendungszugriff mit Azure AD

Zugriffszuweisungen sind „veraltet“, wenn Benutzer über Zugriff verfügen, den sie nicht mehr benötigen. Um das mit veralteten Zugriffszuweisungen verbundene Risiko zu reduzieren, können Administratoren mit Azure Active Directory (Azure AD) eine Zugriffsüberprüfung erstellen. Diese kann für Mitglieder einer Gruppe oder Benutzer, die einer Anwendung zugewiesen sind, durchgeführt werden. Weitere Informationen zu diesen Szenarien finden Sie unter [Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md) und [Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Erstellen einer Zugriffsüberprüfung

1. Als globaler Administrator navigieren Sie zur Seite [Zugriffsüberprüfungen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) und wählen dann **Programme**.

2. Wählen Sie das Programm, das das Zugriffsüberprüfungs-Steuerelement enthält, das Sie erstellen möchten. Das **Standardprogramm** ist immer vorhanden. Sie können aber auch ein anderes Programm erstellen. Sie können z.B. ein Programm für jede Konformitätsinitiative oder für jedes Geschäftsziel verwenden.

3. Wählen Sie innerhalb des Programms **Steuerelemente**, und wählen Sie dann **Hinzufügen**, um ein Steuerelement hinzuzufügen.

4. Benennen Sie jede Zugriffsüberprüfung. Optional können Sie zu jeder Überprüfung eine Beschreibung angeben. Der Name wird den Prüfern angezeigt.

5. Legen Sie Start- und Enddatum fest. Standardmäßig beginnt eine Zugriffsüberprüfung an dem Tag, an dem sie erstellt wird, und endet nach einem Monat. Sie können Start- und Enddatum so ändern, dass der Start der Zugriffsüberprüfung in der Zukunft liegt und sie so lange dauert, wie Sie wünschen.

6. Zugriffsüberprüfungen können für die Mitglieder einer Gruppe oder für Benutzer erfolgen, die einer Anwendung zugewiesen wurden. Sie können die Zugriffsüberprüfung weiter anpassen, um nur die Gastbenutzer zu überprüfen, die Mitglieder (oder der App zugewiesen) sind, anstatt alle Benutzer zu überprüfen, die Mitglieder sind oder Zugriff auf die Anwendung haben.

7. Wählen Sie entweder eine oder mehrere Personen aus, um alle Benutzer im Bereich zu überprüfen. Alternativ können Sie auswählen, dass die Mitglieder ihren eigenen Zugriff überprüfen. Wenn es sich bei der Ressource um eine Gruppe handelt, können Sie die Gruppenbesitzer um die Überprüfung bitten. Sie können auch festlegen, dass die Prüfer einen Grund angeben müssen, wenn sie den Zugriff genehmigen.

8. Wählen Sie abschließend **Starten**.


## <a name="manage-the-access-review"></a>Verwalten der Zugriffsüberprüfung

Standardmäßig sendet Azure AD kurz nach dem Start der Überprüfung eine E-Mail an die Prüfer. Wenn Sie nicht möchten, dass Azure AD die E-Mail sendet, stellen Sie sicher, dass die Prüfer darüber in Kenntnis gesetzt werden, dass sie eine ausstehende Zugriffsüberprüfung abschließen müssen. Sie können ihnen die Anweisungen zum [Überprüfen des Zugriffs mit der Azure AD-Zugriffsüberprüfung](active-directory-azure-ad-controls-perform-access-review.md) anzeigen. Falls Ihre Überprüfung für Gäste gedacht ist, die ihren eigenen Zugriff überprüfen sollen, können Sie ihnen die Anweisungen zum [Überprüfen des eigenen Zugriffs mit der Azure AD-Zugriffsüberprüfung](active-directory-azure-ad-controls-perform-access-review.md) anzeigen.

Wenn es sich bei einigen Prüfern um Gäste handelt: Gäste werden nur dann per E-Mail benachrichtigt, wenn sie die Einladung bereits angenommen haben.


Sie können den Fortschritt der Überprüfungen durch die Prüfer im Azure AD-Dashboard im Abschnitt **Zugriffsüberprüfungen** nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn die [Überprüfung abgeschlossen](active-directory-azure-ad-controls-complete-access-review.md) ist.

## <a name="next-steps"></a>Nächste Schritte

Wenn eine Zugriffsüberprüfung gestartet wurde, sendet Azure AD automatisch eine E-Mail an die Prüfer, in der diese zur Überprüfung des Zugriffs aufgefordert werden. Benutzern, die diese E-Mail nicht erhalten haben, können Sie die Anweisungen unter [Überprüfen des Zugriffs mit der Azure AD-Zugriffsüberprüfung](active-directory-azure-ad-controls-perform-access-review.md) senden. 

Befolgen Sie nach Ablauf des Zugriffsüberprüfungszeitraums oder nachdem der Administrator die Zugriffsüberprüfung angehalten hat die Schritte in [Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD](active-directory-azure-ad-controls-complete-access-review.md), um die Ergebnisse anzuzeigen und anzuwenden.


