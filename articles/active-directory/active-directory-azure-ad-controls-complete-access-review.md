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
ms.openlocfilehash: b301ff06c01d51c02f7d7393801b35cd8965403c
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD

Mit Azure Active Directory (Azure AD) können Administratoren [eine Zugriffsüberprüfung erstellen](active-directory-azure-ad-controls-create-access-review.md). Diese kann für Mitglieder einer Gruppe oder Benutzer, die einer Anwendung zugewiesen sind, durchgeführt werden. Azure AD sendet Prüfern automatisch eine E-Mail, in der sie aufgefordert werden, den Zugriff zu überprüfen. Benutzern, die diese E-Mail nicht erhalten haben, können Sie die Anweisungen unter [So überprüfen Sie den Zugriff](active-directory-azure-ad-controls-perform-access-review.md) senden. Folgen Sie den Schritten in diesem Artikel, nachdem der Zeitraum der Zugriffsüberprüfung abgelaufen ist oder ein Administrator die Zugriffsüberprüfung beendet hat, um die Ergebnisse anzuzeigen und anzuwenden.

## <a name="view-an-access-review-in-the-azure-portal"></a>Anzeigen einer Zugriffsüberprüfung im Azure-Portal

1. Navigieren Sie zur [Zugriffsüberprüfungsseite](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), klicken Sie auf **Programme**, und wählen Sie das Programm aus, welches das Zugriffsüberprüfungssteuerelement enthält.

2. Klicken Sie auf **Verwalten**, und wählen Sie das Zugriffsüberprüfungs-Steuerelement aus. Wenn das Programm viele Steuerelemente enthält, können Sie nach Steuerelementen eines bestimmten Typs filtern und diese nach ihrem Status sortieren. Darüber hinaus können Sie nach dem Namen des Zugriffsüberprüfung-Steuerelements oder dem Anzeigename des Besitzers, der das Steuerelement erstellt hat, suchen. 

## <a name="stop-a-review-that-hasnt-finished"></a>Beenden einer nicht abgeschlossenen Überprüfung

Wenn die Überprüfung noch nicht am geplanten Enddatum angelangt ist, kann ein Administrator auf **Stop** (Beenden) klicken, um die Überprüfung frühzeitig zu beenden. Nachdem die Überprüfung beendet wurde, können Benutzer nicht weiter überprüft werden. Eine Überprüfung kann nicht neu gestartet werden, nachdem sie beendet wurde.

## <a name="apply-the-changes"></a>Übernehmen von Änderungen 

Klicken Sie auf **Apply** (Übernehmen), nachdem eine Zugriffsüberprüfung beendet wurde, entweder weil das Enddatum erreicht wurde oder weil sie von einem Administrator manuell beendet wurde. Das Ergebnis der Überprüfung wird durch das Aktualisieren der Gruppe oder der Anwendung implementiert. Wenn der Zugriff eines Benutzers in der Überprüfung verweigert wurde, entfernt Azure AD deren Mitgliedschaft oder Anwendungszuweisung, wenn ein Administrator diese Option auswählt. 

Das Klicken auf **Übernehmen** hat keine Auswirkung auf eine Gruppe, die aus einem lokalen Verzeichnis stammt, oder auf eine dynamische Gruppe. Wenn Sie eine Gruppe ändern möchten, die aus einem lokalen Verzeichnis stammt, laden Sie die Ergebnisse herunter, und wenden Sie diese Änderungen auf die Darstellung der Gruppe im Verzeichnis an.

## <a name="download-the-results-of-the-review"></a>Herunterladen der Ergebnisse der Überprüfung

Klicken Sie auf **Approvals** (Genehmigungen), um die Ergebnisse der Überprüfung abzurufen, und klicken Sie anschließend auf **Herunterladen**. Die CSV-Datei mit den Ergebnissen kann in Excel oder in anderen Programmen angezeigt werden, die CSV-Dateien öffnen können.

## <a name="optional-delete-a-review"></a>Optional: Löschen einer Überprüfung
Wenn Sie eine Überprüfung nicht mehr benötigen, können Sie diese löschen. Klicken Sie auf **Löschen**, um eine Überprüfung aus Azure AD zu entfernen.

> [!IMPORTANT]
> Vor dem Löschen wird keine Warnung angezeigt. Vergewissern Sie sich daher, dass Sie die Überprüfung tatsächlich löschen möchten.
> 
> 

## <a name="next-steps"></a>Nächste Schritte

- [Manage user access with Azure AD access reviews (Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen)](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Manage guest access with Azure AD access reviews](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md) (Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen)
- [Manage programs and controls for Azure AD access reviews](active-directory-azure-ad-controls-manage-programs-controls.md) (Verwalten der Programme und Kontrollen für Azure AD-Zugriffsüberprüfungen)
- [Erstellen einer Zugriffsüberprüfung für Mitglieder einer Gruppe oder den Zugriff auf eine Anwendung](active-directory-azure-ad-controls-create-access-review.md)
- [Erstellen einer Zugriffsüberprüfung von Benutzern in der Azure AD-Administratorrolle](active-directory-privileged-identity-management-how-to-start-security-review.md)
