---
title: Problembehandlung bei dynamischen Mitgliedschaften in Gruppen | Microsoft-Dokumentation
description: Tipps zur Problembehandlung für dynamische Mitgliedschaften in Gruppen in Azure AD.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/01/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 82a5c57ce874e77a7912945a6fca07acee339197
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444486"
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Problembehandlung bei dynamischen Mitgliedschaften für Gruppen

**Ich habe eine Regel für eine Gruppe konfiguriert, die Mitgliedschaften in der Gruppe werden jedoch nicht aktualisiert.**<br/>Überprüfen Sie die Werte für Benutzerattribute in der Regel: Gibt es Benutzer, die die Regel erfüllen? Wenn alles korrekt aussieht, warten Sie einen Augenblick, bis die Gruppe aufgefüllt ist. Je nach Größe Ihres Mandanten kann das erstmalige Auffüllen der Gruppe oder das Auffüllen nach einer Änderung der Regel bis zu 24 Stunden dauern.

**Ich habe eine Regel konfiguriert, aber jetzt werden die vorhandenen Mitglieder der Regel entfernt.**<br/>Dieses Verhalten wird erwartet. Vorhandene Mitglieder der Gruppe werden entfernt, wenn eine Regel aktiviert oder geändert wird. Die nach der Auswertung der Regel verbleibenden Benutzer werden der Gruppe als Mitglieder hinzugefügt.

**Warum werden Änderungen an der Mitgliedschaft nicht sofort angezeigt, wenn ich eine Regel hinzufüge oder ändere?**<br/>Die dedizierte Mitgliedschaftsauswertung erfolgt in regelmäßigen Abständen in einem asynchronen Hintergrundprozess. Die Dauer dieses Prozesses hängt von der Anzahl der Benutzer in Ihrem Verzeichnis und der Größe der Gruppe, die aufgrund der Regel erstellt wird, ab. Bei Verzeichnissen mit einer geringen Benutzeranzahl werden Änderungen der Gruppenmitgliedschaft üblicherweise innerhalb weniger Minuten angezeigt. Bei Verzeichnissen mit einer hohen Benutzeranzahl kann das Auffüllen 30 Minuten oder länger dauern.

**Bei mir ist ein Regelverarbeitungsfehler aufgetreten.**<br/>In der folgenden Tabelle werden häufige Fehler mit Regeln für eine dynamische Mitgliedschaft und deren Behandlung aufgeführt.

| Regelparserfehler | Fehlerverwendung | Korrigierte Verwendung |
| --- | --- | --- |
| Fehler: Das Attribut nicht unterstützt. |(user.invalidProperty -eq "Value") |(user.department -eq "value")<br/><br/>Stellen Sie sicher, dass das Attribut in der [Liste der unterstützten Eigenschaften](groups-dynamic-membership.md#supported-properties) aufgeführt wird. |
| Fehler: Der Operator wird für das Attribut nicht unterstützt. |(user.accountEnabled -contains true) |(user.accountEnabled -eq true)<br/><br/>Der Operator wird für den Eigenschaftentyp nicht unterstützt (in diesem Beispiel kann „-contains“ nicht für den Booleschen Typ verwendet werden). Verwenden Sie die richtige Operatoren für den Eigenschaftentyp. |
| Fehler: Abfragekompilierungsfehler. | 1. (user.department -eq "Sales") (user.department -eq "Marketing")<br>2. (user.userPrincipalName -match "*@domain.ext") | 1. Fehlender Operator. Verwenden Sie „-and“ oder „-or“, um zwei Prädikate zu verknüpfen.<br>(user.department -eq "Sales") -or (user.department -eq "Marketing")<br>2. Fehler im regulären Ausdruck, der mit „-match“ verwendet wird<br>(user.userPrincipalName -match ".*@domain.ext")<br>oder alternativ: (user.userPrincipalName -match "@domain.ext$") |

## <a name="next-steps"></a>Nächste Schritte

Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](../fundamentals/active-directory-manage-groups.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](../active-directory-apps-index.md)
* [Was ist Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](../connect/active-directory-aadconnect.md)
