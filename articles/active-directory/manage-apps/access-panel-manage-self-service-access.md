---
title: Verwenden des Self-Service-Anwendungszugriffs | Microsoft-Dokumentation
description: Aktivieren des Self-Service-Anwendungszugriffs, um Benutzern die Suche ihrer eigenen Anwendungen zu ermöglichen
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: japere,asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 55da8731855c8afda496edff33f3fbb7982cd44b
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2019
ms.locfileid: "65784552"
---
# <a name="how-to-use-self-service-application-access"></a>Verwenden des Self-Service-Anwendungszugriffs

Damit Ihre Benutzer über ihren Zugriffsbereich Anwendungen selbst ermitteln können, müssen Sie den **Self-Service-Anwendungszugriff** auf alle Anwendungen aktivieren, die Benutzer selbst ermitteln und für die sie den Zugriff anfordern können sollen.

Diese Funktion ermöglicht Ihnen als IT-Abteilung, Zeit und Geld zu sparen, und empfiehlt sich unbedingt als Bestandteil einer modernen Anwendungsbereitstellung mit Azure Active Directory.

Mit dieser Funktion können Sie die folgenden Schritte ausführen:

-   Benutzern ermöglichen, Anwendungen über den [Anwendungszugriffsbereich](https://myapps.microsoft.com/) ohne Zutun der IT-Abteilung selbst zu ermitteln

-   Diese Benutzer einer vorkonfigurierten Gruppe hinzufügen, sodass Sie sehen können, welcher Benutzer den Zugriff angefordert hat, sowie den Zugriff aufheben und die den Benutzern zugewiesenen Rollen verwalten können

-   Optional festlegen, dass eine genehmigende Person des Unternehmens Anforderungen für den Anwendungszugriff genehmigt, sodass dies nicht durch die IT-Abteilung erfolgen muss

-   Optional bis zu 10 Personen konfigurieren, die den Zugriff auf eine Anwendung genehmigen können

-   Optional festlegen, dass eine genehmigende Person des Unternehmens direkt über den [Anwendungszugriffsbereich](https://myapps.microsoft.com/) die Kennwörter angeben kann, die die Benutzer für die Anmeldung bei der Anwendung verwenden können

-   Optional Benutzer mit Self-Service-Zuweisung automatisch direkt einer Anwendungsrolle zuweisen

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>Aktivieren des Self-Service-Anwendungszugriffs, um Benutzern die Suche ihrer eigenen Anwendungen zu ermöglichen

Der Self-Service-Anwendungszugriff bietet die Möglichkeit, dass Benutzer Anwendungen selbst ermitteln können und dass die entsprechende Geschäftseinheit den Zugriff auf diese Anwendungen optional genehmigen kann. Sie können festlegen, dass die Mitarbeiter der Geschäftseinheit direkt über ihren Zugriffsbereich die den Benutzern zugewiesenen Anmeldeinformationen für Anwendungen mit einmaligem Anmelden per Kennwort verwalten können.

Führen Sie die folgenden Schritte aus, um den Self-Service-Anwendungszugriff auf eine Anwendung zu aktivieren:

1. Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2. Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie oben im Hauptnavigationsmenü auf der linken Seite auf **Alle Dienste** klicken.

3. Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4. Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5. Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6. Wählen Sie in der Liste die Anwendung aus, für die Sie den Self-Service-Zugriff aktivieren möchten.

7. Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Self-Service**.

8. Um den Self-Service-Anwendungszugriff auf die Anwendung zu aktivieren, legen Sie **Benutzern das Anfordern des Zugriffs auf diese Anwendung erlauben?** auf **Ja** fest.

9. Um dann die Gruppe auszuwählen, zu der Benutzer, die Zugriff auf diese Anwendung anfordern, hinzugefügt werden sollen, klicken Sie auf das Auswahlfeld neben **Welcher Gruppe sollen zugewiesene Benutzer hinzugefügt werden?**, und wählen Sie eine Gruppe aus.

10. **Optional**: Wenn eine Genehmigung des Unternehmens erforderlich sein soll, damit Benutzer Zugriff erhalten, legen Sie **Genehmigung anfordern, bevor Zugriff auf diese Anwendung gewährt wird?** auf **Ja** fest.

11. **Optional: nur für Anwendungen mit einmaligem Anmelden per Kennwort:** Wenn Sie möchten, dass die genehmigenden Personen des Unternehmens die für genehmigte Benutzer an die Anwendung gesendeten Kennwörter angeben können, legen Sie **Genehmigenden Personen das Festlegen von Benutzerkennwörtern für diese Anwendung gestatten?** auf **Ja** fest.

12. **Optional**: Um die genehmigenden Personen des Unternehmens anzugeben, die den Zugriff auf die Anwendung genehmigen können, klicken Sie auf die Auswahl neben **Wer darf den Zugriff auf diese Anwendung genehmigen?**. Hier können Sie bis zu 10 genehmigende Personen auswählen.

    * Gruppen werden nicht unterstützt.

13. **Optional**: Wenn Sie **für Anwendungen, die Rollen verfügbar machen**, den für den Self-Service genehmigten Benutzern eine Rolle zuweisen möchten, klicken Sie auf die Auswahl neben **Welcher Rolle sollen Benutzer in dieser Anwendung zugewiesen werden?**, und wählen Sie die Rolle aus, die den Benutzern zugewiesen werden soll.

14. Klicken Sie abschließend oben auf dem Blatt auf die Schaltfläche **Speichern**.

Nachdem Sie die Self-Service-Anwendungskonfiguration abgeschlossen haben, können Benutzer in ihrem [Anwendungszugriffsbereich](https://myapps.microsoft.com/) auf die Schaltfläche **+Hinzufügen** klicken und die Apps suchen, für die Sie den Self-Service-Zugriff aktiviert haben. Den genehmigenden Personen des Unternehmens wird im [Zugriffsbereich](https://myapps.microsoft.com/) zudem eine Benachrichtigung angezeigt. Sie können festlegen, dass sie in einer E-Mail darüber benachrichtigt werden, dass ein Benutzer den Zugriff auf eine Anwendung angefordert hat, der zu genehmigen ist. 

Diese Genehmigungen unterstützen nur Workflows mit einzelnen Genehmigungen. Das bedeutet, dass bei der Angabe mehrerer genehmigender Personen jede einzelne genehmigende Person den Zugriff auf die Anwendung genehmigen kann.

## <a name="next-steps"></a>Nächste Schritte
[Einrichten von Azure Active Directory zur Self-Service-Gruppenverwaltung](../users-groups-roles/groups-self-service-management.md)
