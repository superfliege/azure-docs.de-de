---
title: Erstellen einer dynamischen Gruppe und Überprüfen des Status in Azure Active Directory | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie im Azure-Portal Regeln zur Gruppenmitgliedschaft erstellen und den Status überprüfen.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/20/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: e418316a74ccf27ec730261957a8b6c64de5d063
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47040600"
---
# <a name="create-a-dynamic-group-and-check-status"></a>Erstellen einer dynamischen Gruppe und Überprüfen des Status

In Azure Active Directory (Azure AD) können Sie Gruppen erstellen, indem Sie eine Regel anwenden, um die Mitgliedschaft basierend auf Benutzer- oder Geräteeigenschaften zu ermitteln. Wenn sich die Attribute eines Benutzers oder Geräts ändern, wertet Azure AD alle Regeln für dynamische Gruppen im Azure AD-Mandanten aus und führt Hinzufügungen oder Entfernungen durch. Wenn ein Benutzer oder ein Gerät eine Regel für eine Gruppe erfüllt, wird er bzw. es als Mitglied hinzugefügt. Sobald die Vorgaben der Regel nicht mehr erfüllt werden, wird das Mitglied entfernt.

In diesem Artikel wird beschrieben, wie Sie im Azure-Portal eine Regel für die dynamische Mitgliedschaft in Sicherheitsgruppen oder Office 365-Gruppen einrichten. Beispiele für die Regelsyntax und eine vollständige Liste mit den unterstützten Eigenschaften, Operatoren und Werten für eine Mitgliedschaftsregel finden Sie unter [Regeln für eine dynamische Mitgliedschaft für Gruppen in Azure Active Directory](groups-dynamic-membership.md).

## <a name="to-create-a-group-membership-rule"></a>So erstellen Sie eine Regel für die Gruppenmitgliedschaft

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) über ein Konto an, das zur Rolle des globalen Administrators, Intune-Dienstadministrators oder Benutzerkontoadministrators in Ihrem Mandanten gehört.
2. Wählen Sie **Gruppen** aus.
3. Wählen Sie **Alle Gruppen** und **Neue Gruppe** aus.

   ![Neue Gruppe hinzufügen](./media/groups-create-rule/new-group-creation.png)

4. Geben Sie auf dem Blatt **Gruppe** einen Namen und eine Beschreibung für die neue Gruppe ein. Wählen Sie als **Mitgliedschaftstyp** entweder **Dynamischer Benutzer** oder **Dynamisches Gerät** aus, je nachdem, ob Sie eine Regel für Benutzer oder Geräte erstellen möchten. Wählen Sie anschließend **Dynamische Abfrage hinzufügen** aus. Mit dem Regel-Generator können Sie eine einfache Regel erstellen oder selbst eine Regel für die Mitgliedschaft schreiben. Dieser Artikel enthält weitere Informationen zu verfügbaren Benutzer- und Geräteattributen sowie Beispiele für Regeln für die Mitgliedschaft.

   ![Hinzufügen einer Regel für eine dynamische Mitgliedschaft](./media/groups-create-rule/add-dynamic-group-rule.png)

5. Wählen Sie nach der Erstellung der Regel unten auf dem Blatt **Abfrage hinzufügen**.
6. Wählen Sie **Erstellen** on the **Erstellen** aus, um die Gruppe zu erstellen.

> [!TIP]
> Die Gruppenerstellung misslingt, wenn die von Ihnen eingegebene Regel falsch gebildet wurde oder nicht gültig ist. Rechts oben im Portal wird eine Benachrichtigung angezeigt, die erläutert, warum die Regel nicht verarbeitet werden konnte. Lesen Sie sie sorgfältig, um zu erfahren, wie die Regel angepasst werden muss, damit sie gültig ist.

## <a name="check-processing-status-for-a-membership-rule"></a>Überprüfen des Verarbeitungsstatus für eine Mitgliedschaftsregel

Auf der Seite **Übersicht** für die Gruppe sehen Sie den Verarbeitungsstatus der Mitgliedschaft und das zuletzt geänderte Datum.
  
  ![Status der dynamischen Gruppe anzeigen](./media/groups-create-rule/group-status.png)

Für **Verarbeitungsstatus der Mitgliedschaft** können die folgenden Statusmeldungen angezeigt werden:

* **Wird ausgewertet**: Die Gruppenänderung wurde empfangen, die Aktualisierungen werden ausgewertet.
* **Wird verarbeitet**: Die Updates werden verarbeitet.
* **Aktualisierung abgeschlossen**: Die Verarbeitung wurde abgeschlossen, alle anwendbaren Aktualisierungen wurden vorgenommen.
* **Verarbeitungsfehler**: Bei der Auswertung der Mitgliedschaftsregel ist ein Fehler aufgetreten, sodass die Verarbeitung nicht abgeschlossen werden konnte.
* **Aktualisierung angehalten**: Aktualisierungen der Regeln für die dynamische Mitgliedschaft wurden vom Administrator angehalten. „MembershipRuleProcessingState“ ist auf „Paused“ festgelegt.

Für **Letzte Aktualisierung der Mitgliedschaft** können die folgenden Statusmeldungen angezeigt werden:

* &lt;**Datum und Uhrzeit**&gt;: Der Zeitpunkt der letzten Aktualisierung der Mitgliedschaft.
* **In Bearbeitung**: Aktualisierungen sind derzeit in Bearbeitung.
* **Unbekannt**: Der Zeitpunkt der letzten Aktualisierung kann nicht abgerufen werden. Das kann daran liegen, dass die Gruppe neu erstellt wurde.

Wenn bei der Verarbeitung der Mitgliedschaftsregel für eine bestimmte Gruppe ein Fehler auftritt, wird oben auf der Seite **Übersicht** der Gruppe eine Warnung angezeigt. Wenn für alle Gruppen innerhalb des Mandanten für mehr als 24 Stunden keine ausstehenden Aktualisierungen der dynamischen Mitgliedschaft verarbeitet werden können, wird oben in **Alle Gruppen** eine Warnung angezeigt.

![Verarbeiten der Fehlermeldung](./media/groups-create-rule/processing-error.png)

Diese Artikel enthalten zusätzliche Informationen zu Gruppen in Azure Active Directory.

* [Anzeigen vorhandener Gruppen](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Erstellen einer neuen Gruppe und Hinzufügen von Mitgliedern](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](groups-dynamic-membership.md)
