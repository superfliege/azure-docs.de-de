---
title: "Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine Zugriffsüberprüfung für Mitglieder einer Gruppe oder für Benutzer mit Zugriff auf eine Anwendung in Azure Active Directory durchführen."
services: active-directory
documentationcenter: 
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
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: e83bc42d658c4d6304f98b99d0e0942595bd65d2
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---

# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD

Administratoren können eine Überprüfung von Gruppenmitgliedern oder Benutzern anfordern, die einer Anwendung zugewiesen sind, indem sie [eine Zugriffsüberprüfung erstellen](active-directory-azure-ad-controls-create-access-review.md). Azure AD sendet den Prüfern automatisch eine E-Mail, in der sie aufgefordert werden, den Zugriff zu überprüfen. Benutzern, die diese E-Mail nicht erhalten haben, können Sie die Anweisungen unter [So überprüfen Sie den Zugriff](active-directory-azure-ad-controls-perform-access-review.md) senden.  Folgen Sie den Schritten in diesem Artikel, nachdem der Zeitraum der Zugriffsüberprüfung abgelaufen ist oder der Administrator die Zugriffsüberprüfung beendet hat, um die Ergebnisse anzuzeigen und anzuwenden.

## <a name="viewing-an-access-review-in-the-azure-portal"></a>Anzeigen einer Zugriffsüberprüfung im Azure-Portal

1. Navigieren Sie zur [Zugriffsüberprüfungsseite](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), wechseln Sie zur Registerkarte **Programme**, und wählen Sie das Programm aus, welches das Zugriffsüberprüfungssteuerelement enthält.
2. Klicken Sie auf „Verwalten“, und wählen Sie das Zugriffsüberprüfungs-Steuerelement aus.  Wenn es zu viele Steuerelemente gibt, können Sie nach Steuerelementen eines bestimmten Typs filtern. Dabei können Sie nach deren Status sortieren oder nach dem Namen des Zugriffsüberprüfungs-Steuerelements oder dem Anzeigename des Besitzers suchen, der es erstellt hat. 

## <a name="stopping-a-review-that-has-not-yet-completed"></a>Beenden einer Überprüfung, die noch nicht abgeschlossen ist

Wenn die Überprüfung das geplante Enddatum noch nicht erreicht hat, kann ein Administrator die Überprüfung anhalten, indem er auf die Schaltfläche **Anhalten** klickt, um sie frühzeitig abzuschließen.  Wenn nicht alle Benutzer zu diesem Zeitpunkt überprüft wurden, ist dies nach dem Beenden der Überprüfung nicht mehr möglich. Eine Überprüfung kann nicht neu gestartet werden, nachdem sie beendet wurde.

## <a name="applying-the-changes"></a>Anwenden der Änderungen 

Nachdem eine Überprüfung abgeschlossen wurde, weil das Enddatum erreicht ist oder weil ein Administrator sie manuell angehalten hat, wird mit der Schaltfläche **Übernehmen** das Ergebnis der Überprüfung implementiert, indem die Gruppe oder Anwendung aktualisiert wird. Wenn der Zugriff eines Benutzers in der Überprüfung verweigert wurde, entfernt Azure AD deren Mitgliedschaft oder Anwendungszuweisung, sobald ein Administrator auf diese Schaltfläche klickt. 

Das Klicken auf die Schaltfläche „Übernehmen“ hat keine Auswirkung auf eine Gruppe, die aus einem lokalen Verzeichnis stammen, oder auf eine dynamische Gruppe.  Wenn Sie eine Gruppe ändern möchten, die aus einem lokalen Verzeichnis stammt, laden Sie die Ergebnisse herunter, und wenden Sie diese Änderungen auf die Darstellung der Gruppe in dem Verzeichnis an.

## <a name="downloading-the-results-of-the-review"></a>Herunterladen der Ergebnisse der Überprüfung

Wechseln Sie zur Registerkarte „Genehmigungen“, und klicken Sie auf die Schaltfläche **Herunterladen**, um die Ergebnisse der Überprüfung abzurufen.  Die CSV-Datei mit den Ergebnissen kann in Excel oder in anderen Programmen angezeigt werden, die CSV-Dateien öffnen können.

## <a name="optional-deleting-a-review"></a>Optional: Löschen einer Überprüfung
Wenn Sie an einer Überprüfung nicht weiter interessiert sind, löschen Sie sie. Die Schaltfläche **Löschen** entfernt die Überprüfung aus Azure AD.

> [!IMPORTANT]
> Vor dem Löschen wird keine Warnung angezeigt. Vergewissern Sie sich daher, dass Sie die Überprüfung tatsächlich löschen möchten.
> 
> 

## <a name="next-steps"></a>Nächste Schritte

- [Manage user access with Azure AD access reviews (Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen)](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Manage guest access with Azure AD access reviews (Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen)](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
- [Manage programs and controls for Azure AD access reviews (Verwalten der Programme und Kontrollen für Azure AD-Zugriffsüberprüfungen)](active-directory-azure-ad-controls-manage-programs-controls.md)
- [Erstellen einer Zugriffsüberprüfung für Mitglieder einer Gruppe oder den Zugriff auf eine Anwendung](active-directory-azure-ad-controls-create-access-review.md)
- [Erstellen einer Zugriffsüberprüfung von Benutzern in der Azure AD-Administratorrolle](active-directory-privileged-identity-management-how-to-start-security-review.md)

