---
title: Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Zugriffsüberprüfung für Mitglieder einer Gruppe oder für Benutzer mit Zugriff auf eine Anwendung in Azure Active Directory durchführen.
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
ms.date: 05/02/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: ce791e2b230360a04a3051a8964e39c6d5a457ab
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191362"
---
# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD

Mit Azure Active Directory (Azure AD) können Administratoren [eine Zugriffsüberprüfung erstellen](create-access-review.md). Diese kann für Mitglieder einer Gruppe oder Benutzer, die einer Anwendung zugewiesen sind, durchgeführt werden. Azure AD sendet Prüfern automatisch eine E-Mail, in der sie aufgefordert werden, den Zugriff zu überprüfen. Benutzern, die diese E-Mail nicht erhalten haben, können Sie die Anweisungen unter [So überprüfen Sie den Zugriff](perform-access-review.md) senden. (Beachten Sie, dass Gäste, die als Prüfer zugewiesen wurden, die Einladung aber nicht angenommen haben, keine E-Mail zu Zugriffsüberprüfungen erhalten, da die Einladung zuerst akzeptiert werden muss, bevor Prüfer Überprüfungen vornehmen können.) Folgen Sie den Schritten in diesem Artikel, nachdem der Zeitraum der Zugriffsüberprüfung abgelaufen ist oder ein Administrator die Zugriffsüberprüfung beendet hat, um die Ergebnisse anzuzeigen und anzuwenden.

## <a name="view-an-access-review-in-the-azure-portal"></a>Anzeigen einer Zugriffsüberprüfung im Azure-Portal

1. Navigieren Sie zur [Zugriffsüberprüfungsseite](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), klicken Sie auf **Programme**, und wählen Sie das Programm aus, welches das Zugriffsüberprüfungssteuerelement enthält.

2. Klicken Sie auf **Verwalten**, und wählen Sie das Zugriffsüberprüfungs-Steuerelement aus. Wenn das Programm viele Steuerelemente enthält, können Sie nach Steuerelementen eines bestimmten Typs filtern und diese nach ihrem Status sortieren. Darüber hinaus können Sie nach dem Namen des Zugriffsüberprüfung-Steuerelements oder dem Anzeigename des Besitzers, der das Steuerelement erstellt hat, suchen. 

## <a name="stop-a-review-that-hasnt-finished"></a>Beenden einer nicht abgeschlossenen Überprüfung

Wenn die Überprüfung noch nicht am geplanten Enddatum angelangt ist, kann ein Administrator auf **Stop** (Beenden) klicken, um die Überprüfung frühzeitig zu beenden. Nachdem die Überprüfung beendet wurde, können Benutzer nicht weiter überprüft werden. Eine Überprüfung kann nicht neu gestartet werden, nachdem sie beendet wurde.

## <a name="apply-the-changes"></a>Übernehmen von Änderungen 

Nachdem eine Zugriffsüberprüfung abgeschlossen wurde, weil sie entweder das Enddatum erreicht hat oder von einem Administrator manuell angehalten wurde, und wenn für die Überprüfung keine automatische Anwendung konfiguriert wurde, können Sie **Anwenden** auswählen, um die Änderungen manuell anzuwenden. Das Ergebnis der Überprüfung wird durch das Aktualisieren der Gruppe oder der Anwendung implementiert. Wenn der Zugriff eines Benutzers in der Überprüfung verweigert wurde, entfernt Azure AD deren Mitgliedschaft oder Anwendungszuweisung, wenn ein Administrator diese Option auswählt. 

Nachdem eine Zugriffsüberprüfung abgeschlossen wurde und wenn eine automatische Anwendung konfiguriert wurde, ändert sich der Status der Überprüfung von „Abgeschlossen“ zu verschiedenen Zwischenstadien und letztendlich in den Status „Angewendet“. Erwartungsgemäß werden abgelehnte Benutzer, sofern vorhanden, innerhalb weniger Minuten aus der Ressourcengruppen oder App-Zuweisung entfernt.

Die konfigurierte automatische Anwendung einer Überprüfung oder die Auswahl von **Anwenden** haben keine Auswirkung auf eine Gruppe, die aus einem lokalen Verzeichnis stammt, oder auf eine dynamische Gruppe. Wenn Sie eine Gruppe ändern möchten, die aus einem lokalen Verzeichnis stammt, laden Sie die Ergebnisse herunter, und wenden Sie diese Änderungen auf die Darstellung der Gruppe im Verzeichnis an.

## <a name="download-the-results-of-the-review"></a>Herunterladen der Ergebnisse der Überprüfung

Klicken Sie auf **Approvals** (Genehmigungen), um die Ergebnisse der Überprüfung abzurufen, und klicken Sie anschließend auf **Herunterladen**. Die CSV-Datei mit den Ergebnissen kann in Excel oder in anderen Programmen angezeigt werden, die mit UTF-8 codierte CSV-Dateien öffnen können.

## <a name="optional-delete-a-review"></a>Optional: Rezension löschen
Wenn Sie eine Überprüfung nicht mehr benötigen, können Sie diese löschen. Klicken Sie auf **Löschen**, um eine Überprüfung aus Azure AD zu entfernen.

> [!IMPORTANT]
> Vor dem Löschen wird keine Warnung angezeigt. Vergewissern Sie sich daher, dass Sie die Überprüfung tatsächlich löschen möchten.
> 
> 

## <a name="next-steps"></a>Nächste Schritte

- [Manage user access with Azure AD access reviews (Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen)](manage-user-access-with-access-reviews.md)
- [Manage guest access with Azure AD access reviews](manage-guest-access-with-access-reviews.md) (Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen)
- [Manage programs and controls for Azure AD access reviews](manage-programs-controls.md) (Verwalten der Programme und Kontrollen für Azure AD-Zugriffsüberprüfungen)
- [Erstellen einer Zugriffsüberprüfung für Mitglieder einer Gruppe oder den Zugriff auf eine Anwendung](create-access-review.md)
- [Erstellen einer Zugriffsüberprüfung von Benutzern in der Azure AD-Administratorrolle](../privileged-identity-management/pim-how-to-start-security-review.md)
