---
title: Durchführen einer Zugriffsüberprüfung für Gruppen oder Anwendungen – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Gruppenmitglieder oder den Anwendungszugriff betreffende Zugriffsüberprüfung in Azure Active Directory-Zugriffsüberprüfungen durchführen.
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4265a7e08eab079e55ce91b27142ec3e55b3f3e9
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2019
ms.locfileid: "58579596"
---
# <a name="complete-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Durchführen einer Gruppen oder Anwendungen betreffenden Zugriffsüberprüfung in Azure AD-Zugriffsüberprüfungen

Mit Azure Active Directory (Azure AD) können Administratoren [eine Zugriffsüberprüfung erstellen](create-access-review.md). Diese kann für Mitglieder einer Gruppe oder Benutzer, die einer Anwendung zugewiesen sind, durchgeführt werden. Azure AD sendet Prüfern automatisch eine E-Mail, in der sie aufgefordert werden, den Zugriff zu überprüfen. Benutzern, die diese E-Mail nicht erhalten haben, können Sie die Anweisungen unter [Starten einer Zugriffsüberprüfung mit Azure AD-Zugriffsüberprüfungen](perform-access-review.md) senden. (Beachten Sie, dass Gäste, die als Prüfer zugewiesen wurden, die Einladung aber nicht angenommen haben, keine E-Mail zu Zugriffsüberprüfungen erhalten, da die Einladung zuerst akzeptiert werden muss, bevor Prüfer Überprüfungen vornehmen können.) Folgen Sie den Schritten in diesem Artikel, nachdem der Zeitraum der Zugriffsüberprüfung abgelaufen ist oder ein Administrator die Zugriffsüberprüfung beendet hat, um die Ergebnisse anzuzeigen und anzuwenden.

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
- [Erstellen einer Zugriffsüberprüfung von Gruppen oder Anwendungen](create-access-review.md)
- [Erstellen einer Zugriffsüberprüfung von Benutzern in der Azure AD-Administratorrolle](../privileged-identity-management/pim-how-to-start-security-review.md)
