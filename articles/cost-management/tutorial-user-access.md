---
title: Zuweisen des Zugriffs in der Azure-Kostenverwaltung | Microsoft-Dokumentation
description: "Sie weisen den Zugriff für Kostenverwaltungsdaten mit Benutzerkonten zu, die Ebenen des Zugriffs auf Entitäten definieren."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 09/19/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 32e5c2c7c058267c7a2c43d5264922c8a4cab83a
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---


# <a name="assign-access-to-cost-management-data"></a>Zuweisen des Zugriffs auf Kostenverwaltungsdaten

Der Zugriff auf Kostenverwaltungsdaten wird über die Benutzer- oder Entitätsverwaltung bereitgestellt. Cloudyn-Benutzerkonten steuern den Zugriff auf *Entitäten* und administrative Funktionen. Es gibt zwei Arten des Zugriffs: Administrator und Benutzer. Sofern keine benutzerspezifische Änderung erfolgt, ermöglicht der Administratorzugriff eine unbeschränkte Nutzung sämtlicher Funktionen im Cloudyn-Portal, einschließlich Benutzerverwaltung, Empfängerlistenverwaltung und des Zugriffs auf alle Entitätsdaten auf Stammentitätsebene. Der Benutzerzugriff ist für Endbenutzer ausgelegt, die über ihren Zugriff auf Entitätsdaten Berichte anzeigen oder erstellen.

Entitäten geben die hierarchische Struktur Ihrer Geschäftsorganisation wieder. Sie stehen für Abteilungen, Geschäftsbereiche und Teams in Ihrer Organisation in Cloudyn. Die Entitätshierarchie hilft Ihnen dabei, die Ausgaben nach Entitäten genau nachzuverfolgen.

Bei der Registrierung Ihrer Azure-Vereinbarung oder Ihres Azure-Kontos wurde ein Konto mit Administratorberechtigung in Cloudyn erstellt, sodass Sie alle Schritte in diesem Tutorial ausführen können. Dieses Tutorial behandelt den Zugriff auf Kostenverwaltungsdaten, einschließlich Benutzerverwaltung und Entitätsverwaltung. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines Benutzers mit Administratorzugriff
> * Erstellen eines Benutzers mit Benutzerzugriff
> * Erstellen von Entitäten



## <a name="create-a-user-with-admin-access"></a>Erstellen eines Benutzers mit Administratorzugriff

Obwohl Sie bereits über Administratorzugriff verfügen, benötigen Kollegen in Ihrer Organisation möglicherweise ebenfalls Administratorzugriff. Klicken Sie im Cloudyn-Portal auf das Zahnradsymbol in der rechten oberen Ecke, und wählen Sie **User Management** (Benutzerverwaltung) aus. Klicken Sie auf **Add New User** (Neuen Benutzer hinzufügen), um einen neuen Benutzer hinzuzufügen.

Geben Sie die erforderlichen Informationen zum Benutzer ein. Sie können das Kennwortfeld leer lassen, damit der Benutzer bei der ersten Anmeldung ein neues Kennwort festlegen kann. Ein Link mit Anmeldeinformationen wird von Cloudyn per E-Mail an den Benutzer gesendet, wenn Sie **Notify user by email** (Benutzer per E-Mail benachrichtigen) aktivieren. Wählen Sie die erforderlichen Berechtigungen für die Benutzerverwaltung aus, damit der Benutzer andere Benutzer erstellen und ändern kann. Aktivieren Sie die Empfängerlistenverwaltung, damit der Benutzer Empfängerlisten bearbeiten kann.

Unter **User has admin access** (Benutzer verfügt über Administratorzugriff) ist die Stammentität Ihrer Organisation ausgewählt. Lassen Sie den Stamm ausgewählt, und speichern Sie die Benutzerinformationen. Durch das Auswählen der Stammentität verfügt der Benutzer nicht nur für die Stammentität in der Struktur über Administratorberechtigungen, sondern auch für alle Entitäten, die sich darunter befinden.  
  ![Neuen Benutzer mit Administratorzugriff hinzufügen](.\media\tutorial-user-access\new-admin-access.png)

## <a name="create-a-user-with-user-access"></a>Erstellen eines Benutzers mit Benutzerzugriff
Typische Benutzer, die Zugriff auf Kostenverwaltungsdaten wie Dashboards oder Berichte benötigen, sollten über den entsprechenden Benutzerzugriff zum Anzeigen verfügen. Erstellen Sie einen neuen Benutzer mit Benutzerzugriff ähnlich dem, den Sie mit Administratorzugriff erstellt haben, jedoch mit folgenden Unterschieden:

- Deaktivieren Sie **Allow User Management** (Benutzerverwaltung zulassen) und **Allow Recipient lists Management** (Empfängerlistenverwaltung zulassen), und deaktivieren Sie alle Elemente in der Liste **User has admin access** (Benutzer verfügt über Administratorzugriff).
- Aktivieren Sie in der Liste **User has user access** (Benutzer verfügt über Benutzerzugriff) die Entitäten, für die der Benutzer Zugriff benötigt.
- Sie können auch nach Bedarf Administratorrechte für den Zugriff auf bestimmte Entitäten zulassen.

![Neuen Benutzer mit Benutzerzugriff erstellen](.\media\tutorial-user-access\new-user-access.png)

## <a name="create-entities"></a>Erstellen von Entitäten

Wenn Sie Ihre Kostenentitätshierarchie definieren, besteht eine bewährte Methode darin, die Struktur Ihrer Organisation zu identifizieren.

Berücksichtigen Sie beim Erstellen der Struktur, wie die Kosten nach Geschäftseinheiten, Kostenstellen, Umgebungen und Vertriebsabteilungen unterteilt werden sollen oder müssen. Die Entitätsstruktur in Cloudyn ist aufgrund der Entitätsvererbung flexibel. Einzelne Abonnements für Ihre Cloudkonten sind mit bestimmten Entitäten verknüpft. Entitäten sind daher mehrinstanzenfähig. Sie können mit Entitäten bestimmten Benutzern den Zugriff auf nur ihre Segmente des Unternehmens zuweisen. Auf diese Weise bleiben die Daten isoliert, selbst bei großen Bereichen eines Unternehmens wie Niederlassungen. Datenisolierung trägt zudem zu Governance bei.  

Bei der Registrierung Ihrer Azure-Vereinbarung oder Ihres Azure-Kontos bei Cloudyn wurden Ihre Azure-Ressourcendaten, einschließlich Auslastung, Leistung, Abrechnung und Tagdaten von Ihren Abonnements in Ihr Cloudyn-Konto kopiert. Ihre Entitätsstruktur müssen Sie jedoch manuell erstellen. Wenn Sie die Azure Resource Manager-Registrierung übersprungen haben, stehen nur Abrechnungsdaten und einige Ressourcenberichte im Cloudyn-Portal zur Verfügung.

Klicken Sie im Cloudyn-Portal in der rechten oberen Ecke auf **Settings** (Einstellungen), und wählen Sie **Cloud Accounts** (Cloudkonten) aus. Sie beginnen mit einer einzigen Entität (Stamm) und erstellen Ihre Entitätsstruktur unter dem Stamm. Hier sehen Sie ein Beispiel für eine Entitätshierarchie, die vielen IT-Organisationen nach Fertigstellung der Struktur ähnelt:

![Entitätsstruktur](.\media\tutorial-user-access\entity-tree.png)

Klicken Sie neben **Entities** (Entitäten) auf **Add Entity** (Entität hinzufügen). Geben Sie Informationen zu der Person oder Abteilung ein, die Sie hinzufügen möchten. Die Eingaben in den Feldern **Full Name** (Vollständiger Name) und **Email** müssen keinen vorhandenen Benutzern entsprechen. Wenn Sie eine Liste von Zugriffsebenen anzeigen möchten, suchen Sie in der Hilfe nach *Adding an entity* (Hinzufügen einer Entität).

![Entität hinzufügen](.\media\tutorial-user-access\add-entity.png)

**Speichern** Sie nach Abschluss die Entität.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Benutzers mit Administratorzugriff
> * Erstellen eines Benutzers mit Benutzerzugriff
> * Erstellen von Entitäten

Lesen Sie in der Cloudyn-Dokumentation nach, um mehr über die ersten Schritte mit Cloudyn und über die Verwendung der Features von Cloudyn zu erfahren.

> [!div class="nextstepaction"]
> [Cloudyn-Dokumentation](https://support.cloudyn.com/hc/)

