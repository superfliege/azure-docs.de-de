---
title: Anzeigen des Überwachungsverlaufs für Azure AD-Verzeichnisrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Überwachungsverlauf für Azure AD-Verzeichnisrollen in Azure AD Privileged Identity Management (PIM) anzeigen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d9a60544f01048cd90605306e64d750982bf7a4
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56200546"
---
# <a name="view-audit-history-for-azure-ad-directory-roles-in-pim"></a>Anzeigen des Überwachungsverlaufs für Azure AD-Verzeichnisrollen in PIM
Im Privileged Identity Management-Überwachungsverlauf können Sie alle Benutzerzuweisungen und -aktivierungen für alle Rollen in einem bestimmten Zeitraum sehen. Wenn Sie den vollständigen Überwachungsverlauf zur Aktivität in Ihrem Mandanten anzeigen möchten – Administratoren, Endbenutzer und Synchronisierungsaktivität eingeschlossen –, können Sie hierzu die [Azure Active Directory-Zugriffs- und Nutzungsberichte](../reports-monitoring/overview-reports.md)

## <a name="navigate-to-audit-history"></a>Navigieren zum Überwachungsverlauf
Wählen Sie auf dem Dashboard im [Azure-Portal](https://portal.azure.com) die App **Azure AD Privileged Identity Management** aus. Dort können Sie auf den Überwachungsverlauf zugreifen, indem Sie im PIM-Dashboard auf **Privilegierte Rollen verwalten** > **Überwachungsverlauf** klicken.

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Sie können die Daten nach der „Aktion“ sortieren und nach „Aktivierung genehmigt“ suchen.


## <a name="audit-history-graph"></a>Diagramm des Überwachungsverlaufs
Sie können mithilfe des Überwachungsverlaufs die Gesamtanzahl von Aktivierungen, die maximale Anzahl von Aktivierungen pro Tag und die durchschnittliche Anzahl von Aktivierungen pro Tag in einem Liniendiagramm anzeigen.  Sie können auch die Daten nach Rolle filtern, wenn der Überwachungsverlauf mehrere Rollen enthält.

Sortieren Sie den Verlauf mithilfe der Schaltflächen **Zeit**, **Aktion** und **Rolle**.

## <a name="audit-history-list"></a>Überwachungsverlaufsliste
Die Überwachungsverlaufsliste enthält die folgenden Spalten:

* **Anforderer** : Gibt den Benutzer an, der die Rollenaktivierung oder -änderung angefordert hat.  Wenn der Wert „Azure System“ lautet, suchen Sie im Azure-Überwachungsverlauf nach weiteren Informationen.
* **Benutzer** : Gibt den Benutzer an, der eine Rolle aktiviert oder einer Rolle zugewiesen ist.
* **Rolle** : Gibt die Rolle an, die dem Benutzer zugewiesen ist oder die durch den Benutzer aktiviert wurde.
* **Aktion** : Gibt die vom Anforderer ausgeführten Aktionen an. Mögliche Aktionen: Zuweisung, Aufhebung einer Zuweisung, Aktivierung oder Deaktivierung.
* **Zeit** : Dies ist der Zeitpunkt, zu dem die Aktion erfolgt ist.
* **Erläuterung** : Wenn während der Aktivierung Text in das Feld für die Begründung eingegeben wurde, wird er hier angezeigt.
* **Ablauf** : Ist nur für die Aktivierung von Rollen relevant.

## <a name="filter-audit-history"></a>Filtern des Überwachungsverlaufs
Sie können die im Überwachungsverlauf angezeigten Informationen filtern, indem Sie auf die Schaltfläche **Filter** klicken.  Das Blatt **Diagrammparameter aktualisieren** wird angezeigt.

Klicken Sie nach dem Einrichten der Filter auf **Aktualisieren**, um die Daten im Verlauf zu filtern.  Wenn die Daten nicht sofort angezeigt werden, aktualisieren Sie die Seite.

### <a name="change-the-date-range"></a>Ändern des Datumsbereichs
Verwenden Sie die Schaltflächen **Heute**, **Letzte Woche**, **Letzter Monat** oder **Benutzerdefiniert**, um den Zeitbereich des Überwachungsverlaufs zu ändern.

Wenn Sie auf **Benutzerdefiniert** klicken, werden die Datumsfelder **Von** und **Bis** angezeigt, um den Datumsbereich für den Verlauf anzugeben.  Sie können die Datumsangaben im Format MM/TT/JJJJ eingeben oder auf das Symbol **Kalender** klicken und das Datum im Kalender auswählen.

### <a name="change-the-roles-included-in-the-history"></a>Ändern der im Verlauf enthaltenen Rollen
Aktivieren oder deaktivieren Sie das Kontrollkästchen **Rolle** neben jeder Rolle, die im Verlauf enthalten bzw. nicht enthalten sein soll.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

- [Anzeigen des Aktivitäts- und Überwachungsverlaufs für Azure-Ressourcenrollen in PIM](azure-pim-resource-rbac.md)
