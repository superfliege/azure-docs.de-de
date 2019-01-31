---
title: Abschließen einer Zugriffsüberprüfung für Azure AD-Verzeichnisrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Zugriffsüberprüfung für Ihre Azure AD-Verzeichnisrollen in Azure AD Privileged Identity Management (PIM) abschließen und die Ergebnisse anzeigen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: fc4dcc22cf22f70fcf441c3c8a54aeda2ffd7588
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55189665"
---
# <a name="complete-an-access-review-for-azure-ad-directory-roles-in-pim"></a>Abschließen einer Zugriffsüberprüfung für Azure AD-Verzeichnisrollen in PIM
Nachdem eine [Zugriffsüberprüfung gestartet wurde](pim-how-to-start-security-review.md), können Administratoren für privilegierte Rollen den privilegierten Zugriff überprüfen. Azure AD Privileged Identity Management (PIM) sendet automatisch eine E-Mail, in der Benutzer aufgefordert werden, ihren Zugriff zu überprüfen. Benutzern, die diese E-Mail nicht erhalten, können Sie die Anweisungen unter [Ausführen einer Zugriffsüberprüfung](pim-how-to-perform-security-review.md) senden.

Wenn der Zeitraum für die Zugriffsüberprüfung abgelaufen ist oder alle Benutzer die Selbstüberprüfung abgeschlossen haben, führen Sie die Schritte in diesem Artikel aus, um die Überprüfung zu verwalten und die Ergebnisse anzuzeigen.

## <a name="manage-access-reviews"></a>Verwalten von Zugriffsüberprüfungen
1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/) , und wählen Sie auf dem Dashboard die Anwendung **Azure AD Privileged Identity Management** aus.
2. Wählen Sie auf dem Dashboard den Abschnitt **Zugriffsüberprüfungen** aus.
3. Wählen Sie die Zugriffsüberprüfung aus, die Sie verwalten möchten.

Auf dem Detailblatt der Zugriffsüberprüfung stehen eine Reihe von Optionen zum Verwalten dieser Überprüfung zur Verfügung.

![Schaltflächen der PIM-Zugriffsüberprüfung – Screenshot](./media/pim-how-to-complete-review/PIM_review_buttons.png)

### <a name="remind"></a>Erinnerung
Wenn eine Zugriffsüberprüfung so eingerichtet ist, dass die Benutzer sich selbst überprüfen, wird über die Schaltfläche **Erinnerung** eine Benachrichtigung gesendet. 

### <a name="stop"></a>Beenden
Alle Zugriffsüberprüfungen weisen ein Enddatum auf, Sie können jedoch die Schaltfläche **Beenden** verwenden, um sie zu einem frühen Zeitpunkt abzuschließen. Wenn nicht alle Benutzer zu diesem Zeitpunkt überprüft wurden, ist dies nach dem Beenden der Überprüfung nicht mehr möglich. Eine Überprüfung kann nicht neu gestartet werden, nachdem sie beendet wurde.

### <a name="apply"></a>Anwenden
Nachdem eine Überprüfung abgeschlossen wurde, weil das Enddatum erreicht ist oder weil sie manuell beendet wurde, wird mit der Schaltfläche **Übernehmen** das Ergebnis der Überprüfung implementiert. Wenn bei der Überprüfung der Zugriff eine Benutzers verweigert wurde, ist dies der Schritt, mit dem die Rollenzuweisung entfernt wird.  

### <a name="export"></a>Export
Wenn Sie die Ergebnisse der Zugriffsüberprüfung manuell anwenden möchten, können Sie die Überprüfung exportieren. Über die Schaltfläche **Export** können Sie den Download einer CSV-Datei starten. Sie können die Ergebnisse in Excel oder einem anderen Programm verwalten, in dem sich CSV-Dateien öffnen lassen.

### <a name="delete"></a>Löschen
Wenn Sie an einer Überprüfung nicht weiter interessiert sind, löschen Sie sie. Über die Schaltfläche **Löschen** entfernen Sie die Überprüfung aus der PIM-Anwendung.

> [!IMPORTANT]
> Vor dem Löschvorgang wird keine Warnung angezeigt. Vergewissern Sie sich daher, dass Sie die Überprüfung tatsächlich löschen möchten. 

## <a name="next-steps"></a>Nächste Schritte

- [Starten einer Zugriffsüberprüfung für Azure AD-Verzeichnisrollen in PIM](pim-how-to-start-security-review.md)
- [Ausführen einer Zugriffsüberprüfung für Azure AD-Verzeichnisrollen in PIM](pim-how-to-perform-security-review.md)
