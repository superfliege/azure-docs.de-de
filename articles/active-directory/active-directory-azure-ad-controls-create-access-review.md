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
ms.openlocfilehash: c2c5aee3db11255be5a6fe405424c2cbb92c5b95
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Erstellen einer Zugriffsüberprüfung von Gruppenmitgliedern oder dem Anwendungszugriff mit Azure AD

Zugriffszuweisungen sind „veraltet“, wenn Benutzer über Zugriff verfügen, den sie nicht mehr benötigen.  Um das Risiko im Zusammenhang mit Statuszugriffszuweisungen zu reduzieren, können Administratoren eine Überprüfung von Gruppenmitgliedern oder Benutzern anfordern, die einer Anwendung zugewiesen sind, indem sie eine Zugriffsüberprüfung erstellen. In den Handbüchern zum [Verwalten des Benutzerzugriffs](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md) und [Verwalten des Gastzugriffs](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md) finden Sie weitere Informationen zu diesen Szenarios.  

## <a name="how-to-create-an-access-review"></a>Erstellen einer Zugriffsüberprüfung


1. Als globaler Administrator navigieren Sie zur Seite [Zugriffsüberprüfung](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) und wechseln dann auf die Registerkarte **Programme**.
2. Klicken Sie auf das Programm, in dem sich das Steuerelement für die Zugriffsüberprüfung befinden wird, das Sie erstellen möchten.  Ein Programm, das „Standardprogramm“ ist immer vorhanden. Sie können aber auch ein anderes Programm erstellen.  Sie können z.B. ein Programm für jede Konformitätsinitiative oder für jedes Geschäftsziel verwenden.
3. Klicken Sie im Programm auf „Steuerelemente“, und klicken Sie dann auf **Hinzufügen**, um ein Steuerelement hinzuzufügen.
4. Jede Zugriffsüberprüfung muss über einen Namen und kann optional auch über eine Beschreibung verfügen.  Der Name wird den Prüfern angezeigt.  
5. Standardmäßig beginnt eine Zugriffsüberprüfung am dem Tag, an dem sie erstellt wird, und endet planmäßig nach einem Monat.  Sie können die Start- und Enddaten so ändern, dass der Start der Zugriffsüberprüfung in der Zukunft liegt und so lange dauert, wie Sie wünschen.
6. Zugriffsüberprüfungen können für die Mitglieder einer Gruppe oder für Benutzer erfolgen, die einer Anwendung zugewiesen wurden.  Sie können die Zugriffsüberprüfung weiter anpassen, um nur die Gastbenutzer zu überprüfen, die Mitglieder (oder der App zugewiesen) sind, anstatt alle Benutzer zu überprüfen, die Mitglieder sind oder Zugriff auf die Anwendung haben.
7. Sie können eine oder mehrere Personen auswählen, die alle betroffenen Benutzer überprüfen, oder Sie können auswählen, dass die Mitglieder selbst ihren eigenen Zugriff überprüfen.  Wenn es sich bei der Ressource um eine Gruppe handelt, können Sie die Gruppenbesitzer um die Überprüfung bitten.  Sie können auch festlegen, dass die Prüfer einen Grund angeben müssen, wenn sie den Zugriff genehmigen.
8. Klicken Sie abschließend auf **Start** (Starten).


## <a name="managing-the-access-review"></a>Verwalten der Zugriffsüberprüfung

Standardmäßig sendet Azure AD kurz nach dem Start der Überprüfung eine E-Mail an die Prüfer.  Wenn Sie nicht möchten, dass Azure AD die E-Mail sendet, stellen Sie sicher, dass die Prüfer darüber in Kenntnis gesetzt werden, dass sie eine ausstehende Zugriffsüberprüfung abschließen müssen.  Sie können ihnen die Anweisungen zum [Überprüfen des Zugriffs](active-directory-azure-ad-controls-perform-access-review.md) oder, falls Ihre Überprüfung für Gäste gedacht ist, die ihren eigenen Zugriff überprüfen sollen, die Anweisungen zum [Überprüfen des eigenen Zugriffs](active-directory-azure-ad-controls-perform-access-review.md) zeigen.

Beachten Sie, wenn es sich bei einigen Prüfern um Gäste handelt, dass diese nur dann per E-Mail benachrichtigt werden, wenn sie die Einladung bereits angenommen haben.


Sie können den Fortschritt der Überprüfungen durch die Prüfer im Azure AD-Dashboard im Abschnitt mit den Zugriffsüberprüfungen nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn die [Überprüfung abgeschlossen](active-directory-azure-ad-controls-complete-access-review.md)ist.

## <a name="next-steps"></a>Nächste Schritte

Wenn eine Zugriffsüberprüfung gestartet wurde, sendet Azure AD automatisch eine E-Mail an die Prüfer, in der diese zur Überprüfung des Zugriffs aufgefordert werden. Benutzern, die diese E-Mail nicht erhalten haben, können Sie die Anweisungen unter [So überprüfen Sie den Zugriff](active-directory-azure-ad-controls-perform-access-review.md) senden.  

Befolgen Sie nach Ablauf des Zugriffsüberprüfungszeitraums oder nachdem der Administrator die Zugriffsüberprüfung angehalten hat, die Schritte zum [Abschließen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-complete-access-review.md), um die Ergebnisse anzuzeigen und anzuwenden.


