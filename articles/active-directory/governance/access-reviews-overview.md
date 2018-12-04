---
title: Was sind Azure AD-Zugriffsüberprüfungen? | Microsoft-Dokumentation
description: Mithilfe von Azure Active Directory-Zugriffsüberprüfungen können Sie Gruppenmitgliedschaften und den Anwendungszugriff steuern, um die Anforderungen von Governance-, Risikomanagement- und Konformitätsinitiativen in Ihrer Organisation zu erfüllen.
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
ms.component: compliance
ms.date: 11/19/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: fe51419106f1164f9a9b5993261c61bad63333b5
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52262957"
---
# <a name="what-are-azure-ad-access-reviews"></a>Was sind Azure AD-Zugriffsüberprüfungen?

Mithilfe von Azure Active Directory-Zugriffsüberprüfungen (Azure AD-Zugriffsüberprüfungen) können Unternehmen Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen und Rollenzuweisungen effizient verwalten. Der Benutzerzugriff kann regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Personen weiterhin Zugriff haben.

Das folgende Video bietet einen kurzen Überblick über Zugriffsüberprüfungen:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>Warum sind Zugriffsüberprüfungen wichtig?

Azure AD ermöglicht Ihnen die interne Zusammenarbeit mit Mitarbeitern in Ihrer Organisation sowie die Zusammenarbeit mit Benutzern aus externen Organisationen, z. B. mit Partnern. Benutzer können Gruppen beitreten, Gäste einladen, Verbindungen mit Cloud-Apps herstellen und mit ihren Firmen- oder persönlichen Geräten remote arbeiten. Die Annehmlichkeit, die Möglichkeiten von Self-Service nutzen zu können, hat zur Notwendigkeit besserer Zugriffsverwaltungsfunktionen geführt.

- Wie stellen Sie sicher, dass neue Mitarbeiter die richtigen Zugriffsrechte erhalten, damit sie produktiv arbeiten können?
- Wie stellen Sie sicher, dass die alten Zugriffsrechte von Personen entfernt werden, die Teams wechseln oder das Unternehmen verlassen, insbesondere wenn es sich um Gäste handelt?
- Zu viele Zugriffsrechte können zu Auditergebnissen und Sicherheitsrisiken führen, weil sie auf eine mangelnde Kontrolle über den Zugriff hinweisen.
- Sie müssen sich proaktiv mit Ressourcenbesitzern austauschen, um sicherzustellen, dass sie regelmäßig überprüfen, wer Zugriff auf ihre Ressourcen hat.

## <a name="when-to-use-access-reviews"></a>In welchen Fällen sollten Zugriffsüberprüfungen verwendet werden?

- **Zu viele Benutzer in privilegierten Rollen:** Sie sollten überprüfen, wie viele Benutzer über Administratorzugriff verfügen, wie viele dieser Benutzer globale Administratoren sind und ob es eingeladene Gäste oder Partner gibt, die nicht entfernt wurden, nachdem ihnen eine administrative Aufgabe zugewiesen wurde. Sie können die Rollenzuweisung für Benutzer in [Azure AD-Verzeichnisrollen](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) wie globale Administratoren oder in [Azure-Ressourcenrollen](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) wie Benutzerzugriffsadministratoren in [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) erneut bestätigen.
- **Wenn eine Automatisierung nicht möglich ist:** Sie können Regeln für dynamische Mitgliedschaften für Sicherheits- oder Office 365-Gruppen erstellen, aber was geschieht, wenn die Personaldaten nicht in Azure AD gespeichert sind oder die Benutzer nach dem Verlassen der Gruppe weiterhin Zugriff benötigen, um ihre Nachfolger einzuarbeiten? Dann können Sie eine Überprüfung für diese Gruppe erstellen, um sicherzustellen, dass die Benutzer, die weiterhin Zugriff benötigen, auch weiterhin Zugriff haben.
- **Wenn eine Gruppe zu einem neuen Zweck verwendet wird:** Wenn Sie über eine Gruppe verfügen, die mit Azure AD synchronisiert wird, oder wenn Sie die Salesforce-Anwendung für alle Benutzer in der Gruppe „Vertriebsteam“ aktivieren möchten, wäre es sinnvoll, den Besitzer der Gruppe zu bitten, die Gruppenmitgliedschaft zu überprüfen, bevor die Gruppe in einem anderen Risikoinhalt verwendet wird.
- **Unternehmenskritischer Datenzugriff:** Bei bestimmten Ressourcen kann es zu Überwachungszwecken erforderlich sein, die Mitarbeiter außerhalb der IT-Abteilung zu bitten, sich regelmäßig abzumelden und zu begründen, warum sie Zugriff benötigen.
- **Zum Verwalten einer Richtlinienausnahmeliste:** Im Idealfall halten sich alle Benutzer an die Zugriffsrichtlinien, um den Zugriff auf die Ressourcen Ihrer Organisation zu schützen. Es kann aber auch Geschäftsszenarien geben, in denen Ausnahmen erforderlich sind. Als IT-Administrator können Sie diese Aufgabe verwalten, das Übersehen von Richtlinienausnahmen vermeiden und Prüfern den Nachweis erbringen, dass diese Ausnahmen regelmäßig überprüft werden.
- **Auffordern von Gruppenbesitzern zum Bestätigen, dass sie weiterhin Gäste in ihren Gruppen benötigen:** Der Mitarbeiterzugriff kann mit lokalem Identity & Access Management (IAM) automatisiert werden. Dies trifft jedoch nicht auf die Zugriffsrechte eingeladener Gäste zu. Wenn eine Gruppe Gästen Zugriff auf vertrauliche Unternehmensinhalte gewährt, muss der Besitzer der Gruppe bestätigen, dass für die Gäste immer noch eine berechtigte geschäftliche Notwendigkeit des Zugriffs besteht.
- **Regelmäßige Durchführung von Überprüfungen:** Sie können die Häufigkeit der regelmäßigen Durchführung von Zugriffsüberprüfungen für Benutzer (z. B. wöchentlich, monatlich, vierteljährlich oder jährlich) einrichten. Die Prüfer werden zu Beginn jeder Überprüfung benachrichtigt. Prüfer können den Zugriff über eine benutzerfreundliche Oberfläche und mithilfe intelligenter Empfehlungen genehmigen oder verweigern.

## <a name="where-do-you-create-reviews"></a>Wo erstellen Sie Überprüfungen?

Je nachdem, was Sie überprüfen möchten, erstellen Sie Ihre Zugriffsüberprüfung in Azure AD Access Reviews (Azure AD-Zugriffsüberprüfungen), Azure AD-Unternehmens-Apps (in der Vorschauversion) oder Azure AD PIM.

| Zugriffsrechte von Benutzern | Prüfer | Überprüfung erstellt in | Prüferoberfläche |
| --- | --- | --- | --- |
| Mitglieder von Sicherheitsgruppen</br>Mitglieder von Office-Gruppen | Angegebene Prüfer</br>Gruppenbesitzer</br>Selbstüberprüfung | Azure AD-Zugriffsüberprüfungen</br>Azure AD-Gruppen | Zugriffsbereich |
| Einer verbundenen App zugewiesen | Angegebene Prüfer</br>Selbstüberprüfung | Azure AD-Zugriffsüberprüfungen</br>Azure AD-Unternehmens-Apps (in der Vorschauversion) | Zugriffsbereich |
| Azure AD-Verzeichnisrolle | Angegebene Prüfer</br>Selbstüberprüfung | Azure AD PIM | Azure-Portal |
| Azure-Ressourcenrolle | Angegebene Prüfer</br>Selbstüberprüfung | Azure AD PIM | Azure-Portal |

## <a name="prerequisites"></a>Voraussetzungen

Um Zugriffsüberprüfungen verwenden zu können, müssen Sie über eine der folgenden Lizenzen verfügen:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5-Lizenz

Weitere Informationen finden Sie unter [Gewusst wie: Registrieren für Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) oder [Enterprise Mobility + Security E5-Testversion](http://aka.ms/emse5trial).

## <a name="get-started-with-access-reviews"></a>Erste Schritte mit Zugriffsüberprüfungen

Schauen Sie sich das folgende kurze Demovideo an, um mehr über das Erstellen und Durchführen von Zugriffsüberprüfungen zu erfahren:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Wenn Sie Zugriffsüberprüfungen in Ihrer Organisation bereitstellen möchten, folgen Sie den im Video gezeigten Schritten, um Zugriffsüberprüfungen zu integrieren, Ihre Administratoren zu schulen und Ihre erste Zugriffsüberprüfung zu erstellen!

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="enable-access-reviews"></a>Aktivieren von Zugriffsüberprüfungen

Führen Sie die folgenden Schritte aus, um Zugriffsüberprüfungen zu aktivieren.

1. Melden Sie sich als globaler Administrator oder Benutzerkontoadministrator beim [Azure-Portal](https://portal.azure.com) an, in dem Sie Zugriffsüberprüfungen verwenden möchten.

1. Klicken Sie auf **Alle Dienste**, und suchen Sie nach dem Dienst für Zugriffsüberprüfungen.

    ![Alle Dienste – Zugriffsüberprüfungen](./media/access-reviews-overview/all-services-access-reviews.png)

1. Klicken Sie auf **Zugriffsüberprüfungen**.

    ![Zugriffsüberprüfungen – Integrieren](./media/access-reviews-overview/onboard-button.png)

1. Klicken Sie in der Navigationsliste auf **Integrieren**, um die Seite **Zugriffsüberprüfungen integrieren** zu öffnen.

    ![Zugriffsüberprüfungen integrieren](./media/access-reviews-overview/onboard-access-reviews.png)

1. Klicken Sie auf **Erstellen**, um Zugriffsüberprüfungen im aktuellen Verzeichnis zu aktivieren. Beim nächsten Starten von Zugriffsüberprüfungen sind die entsprechenden Optionen aktiviert.

    ![Aktivierte Zugriffsüberprüfungen](./media/access-reviews-overview/access-reviews-enabled.png)

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Zugriffsüberprüfung für Mitglieder einer Gruppe oder den Zugriff auf eine Anwendung](create-access-review.md)
- [Erstellen einer Zugriffsüberprüfung von Benutzern in der Azure AD-Administratorrolle](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Durchführen einer Zugriffsüberprüfung mit Azure AD-Zugriffsüberprüfungen](perform-access-review.md)
- [Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung in Azure AD](complete-access-review.md)
