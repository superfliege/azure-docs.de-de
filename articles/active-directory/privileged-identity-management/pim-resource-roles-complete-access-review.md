---
title: Ausführen einer Zugriffsüberprüfung für Azure-Ressourcen mit Privileged Identity Management | Microsoft Docs
description: Beschreibt, wie Sie eine Zugriffsüberprüfung für Azure-Ressourcen ausführen.
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
ms.openlocfilehash: ae64d9ebbca80f6c21b8c7f352022a0878518e65
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="complete-an-access-review-for-azure-resources-by-using-privileged-identity-management"></a>Ausführen einer Zugriffsüberprüfung für Azure-Ressourcen mit Privileged Identity Management
Nachdem eine [Sicherheitsüberprüfung gestartet wurde](pim-resource-roles-start-access-review.md), können Administratoren für privilegierte Rollen den privilegierten Zugriff überprüfen. Privileged Identity Management (PIM) für Azure-Ressourcen sendet automatisch eine E-Mail, in der Benutzer aufgefordert werden, ihren Zugriff zu überprüfen. Wenn ein Benutzer diese E-Mail nicht erhalten hat, können Sie ihm die Anweisungen unter [Ausführen einer Sicherheitsüberprüfung](pim-resource-roles-perform-access-review.md) zusenden.

Wenn der Zeitraum für die Sicherheitsüberprüfung abgelaufen ist oder alle Benutzer die Selbstüberprüfung abgeschlossen haben, führen Sie die Schritte in diesem Artikel aus, um die Überprüfung zu verwalten und die Ergebnisse anzuzeigen.

## <a name="manage-security-reviews"></a>Verwalten von Sicherheitsüberprüfungen
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/). Wählen Sie dann im Dashboard die Anwendung **Azure-Ressourcen** aus.

2. Wählen Sie Ihre Ressource aus.

3. Wählen Sie auf dem Dashboard den Abschnitt **Zugriffsüberprüfungen** aus.
![Zugriffsüberprüfungen](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)

4. Wählen Sie die Zugriffsüberprüfung aus, die Sie verwalten möchten.

Auf dem Detailblatt der Zugriffsüberprüfung stehen eine Reihe von Optionen zum Verwalten dieser Überprüfung zur Verfügung. Die folgenden Optionen sind verfügbar:

![Optionen zum Verwalten einer Überprüfung](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>Beenden
Alle Zugriffsüberprüfungen weisen ein Enddatum auf, Sie können jedoch die Schaltfläche **Beenden** verwenden, um sie zu einem frühen Zeitpunkt abzuschließen. Alle Benutzer, die ihre Überprüfung bis zu diesem Zeitpunkt nicht abgeschlossen haben, können sie nicht mehr abschließen, nachdem Sie die Überprüfung beendet haben. Eine Überprüfung kann nicht neu gestartet werden, nachdem sie beendet wurde.

### <a name="reset"></a>Reset
Sie können eine Zugriffsüberprüfung zurücksetzen, um alle dafür getroffenen Entscheidungen zu entfernen. Nachdem Sie eine Zugriffsüberprüfung zurückgesetzt haben, werden alle Benutzer als „Nicht überprüft“ gekennzeichnet. 

### <a name="apply"></a>Anwenden
Nachdem eine Zugriffsüberprüfung abgeschlossen wurde, verwenden Sie die Schaltfläche **Übernehmen**, um das Ergebnis der Überprüfung zu implementieren. Wenn bei der Überprüfung der Zugriff eines Benutzers verweigert wurde, wird mit diesem Schritt seine Rollenzuweisung entfernt.  

### <a name="delete"></a>Löschen
Wenn Sie an der Überprüfung nicht weiter interessiert sind, löschen Sie sie. Über die Schaltfläche **Löschen** entfernen Sie die Überprüfung aus der PIM-Anwendung.

## <a name="results"></a>Ergebnisse
Auf der Registerkarte **Ergebnisse** können Sie eine Liste mit Ihren Ergebnissen der Überprüfung anzeigen und herunterladen. 
![Registerkarte „Ergebnisse“](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>Prüfer
Zeigen Sie Prüfer für Ihre vorhandene Zugriffsüberprüfung an, und fügen Sie sie hinzu. Erinnern Sie Prüfer daran, ihre Überprüfungen durchzuführen.
![Prüfer hinzufügen](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)



