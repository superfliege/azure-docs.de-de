---
title: Privileged Identity Management für Azure-Ressourcen – Abschließen einer Zugriffsüberprüfung für Azure-Ressourcen | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Zugriffsüberprüfung für Azure-Ressourcen durchführen.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 5ce02c2d27ec3de87fe44e9c904f19409600e5c5
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---finish-access-review"></a>Privileged Identity Management – Ressourcenrolle – Abschließen der Zugriffsüberprüfung
Nachdem eine [Sicherheitsüberprüfung gestartet wurde](pim-resource-roles-start-access-review.md), können Administratoren für privilegierte Rollen den privilegierten Zugriff überprüfen. Privileged Identity Management (PIM) für Azure-Ressourcen sendet automatisch eine E-Mail, in der Benutzer aufgefordert werden, ihren Zugriff zu überprüfen. Benutzern, die diese E-Mail nicht erhalten, können Sie die Anweisungen unter [Ausführen einer Sicherheitsüberprüfung](pim-resource-roles-perform-access-review.md)senden.

Wenn der Zeitraum für die Sicherheitsüberprüfung abgelaufen ist oder alle Benutzer die Selbstüberprüfung abgeschlossen haben, führen Sie die Schritte in diesem Artikel aus, um die Überprüfung zu verwalten und die Ergebnisse anzuzeigen.

## <a name="manage-security-reviews"></a>Verwalten von Sicherheitsüberprüfungen
1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/), und wählen Sie in Ihrem Dashboard die Anwendung **Azure-Ressourcen** aus.
2. Wählen Sie Ihre Ressource aus.
3. Wählen Sie auf dem Dashboard den Abschnitt **Zugriffsüberprüfungen** aus.
![](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)
4. Wählen Sie die Zugriffsüberprüfung aus, die Sie verwalten möchten.

Auf dem Detailblatt der Zugriffsüberprüfung stehen eine Reihe von Optionen zum Verwalten dieser Überprüfung zur Verfügung.
![](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>Beenden
Alle Zugriffsüberprüfungen weisen ein Enddatum auf, Sie können jedoch die Schaltfläche **Beenden** verwenden, um sie zu einem frühen Zeitpunkt abzuschließen. Wenn nicht alle Benutzer zu diesem Zeitpunkt überprüft wurden, ist dies nach dem Beenden der Überprüfung nicht mehr möglich. Eine Überprüfung kann nicht neu gestartet werden, nachdem sie beendet wurde.

### <a name="reset"></a>Zurücksetzen
Sie können eine Zugriffsüberprüfung zurücksetzen, um alle dafür getroffenen Entscheidungen zu entfernen. Nachdem Sie eine Zugriffsüberprüfung zurückgesetzt haben, werden alle Benutzer als „Nicht überprüft“ gekennzeichnet. 

### <a name="apply"></a>Anwenden
Nachdem eine Überprüfung abgeschlossen wurde, weil das Enddatum erreicht ist oder weil sie manuell beendet wurde, wird mit der Schaltfläche **Übernehmen** das Ergebnis der Überprüfung implementiert. Wenn bei der Überprüfung der Zugriff eine Benutzers verweigert wurde, ist dies der Schritt, mit dem die Rollenzuweisung entfernt wird.  

### <a name="delete"></a>Löschen
Wenn Sie an einer Überprüfung nicht weiter interessiert sind, löschen Sie sie. Über die Schaltfläche **Löschen** entfernen Sie die Überprüfung aus der PIM-Anwendung.

## <a name="results"></a>Ergebnisse
Auf der Registerkarte „Ergebnisse“ können Sie eine Liste mit Ihren Ergebnissen der Überprüfung anzeigen und herunterladen. ![](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>Prüfer
Zeigen Sie Prüfer für Ihre vorhandene Zugriffsüberprüfung an, und fügen Sie sie hinzu. Erinnern Sie Prüfer daran, ihre Überprüfungen durchzuführen.
![](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)



