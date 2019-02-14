---
title: Problembehandlung für dynamische Mitgliedschaften in Gruppen – Azure Active Directory | Microsoft-Dokumentation
description: Tipps zur Problembehandlung für dynamische Mitgliedschaften in Gruppen in Azure AD.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a1fef19c555b9d2e52d4734a8f7bc5e39183e684
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56169308"
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Problembehandlung bei dynamischen Mitgliedschaften für Gruppen

**Ich habe eine Regel für eine Gruppe konfiguriert, die Mitgliedschaften in der Gruppe werden jedoch nicht aktualisiert.**<br/>Überprüfen Sie die Werte für Benutzerattribute in der Regel: Gibt es Benutzer, die die Regel erfüllen? Wenn alles korrekt aussieht, warten Sie einen Augenblick, bis die Gruppe aufgefüllt ist. Je nach Größe Ihres Mandanten kann das erstmalige Auffüllen der Gruppe oder das Auffüllen nach einer Änderung der Regel bis zu 24 Stunden dauern.

**Ich habe eine Regel konfiguriert, aber jetzt werden die vorhandenen Mitglieder der Regel entfernt.**<br/>Dieses Verhalten wird erwartet. Vorhandene Mitglieder der Gruppe werden entfernt, wenn eine Regel aktiviert oder geändert wird. Die nach der Auswertung der Regel verbleibenden Benutzer werden der Gruppe als Mitglieder hinzugefügt.

**Warum werden Änderungen an der Mitgliedschaft nicht sofort angezeigt, wenn ich eine Regel hinzufüge oder ändere?**<br/> Die dedizierte Mitgliedschaftsauswertung erfolgt in regelmäßigen Abständen in einem asynchronen Hintergrundprozess. Die Dauer dieses Prozesses hängt von der Anzahl der Benutzer in Ihrem Verzeichnis und der Größe der Gruppe, die aufgrund der Regel erstellt wird, ab. Bei Verzeichnissen mit einer geringen Benutzeranzahl werden Änderungen der Gruppenmitgliedschaft üblicherweise innerhalb weniger Minuten angezeigt. Bei Verzeichnissen mit einer hohen Benutzeranzahl kann das Auffüllen 30 Minuten oder länger dauern.

**Bei mir ist ein Regelverarbeitungsfehler aufgetreten.**<br/>In der folgenden Tabelle werden häufige Fehler mit Regeln für eine dynamische Mitgliedschaft und deren Behandlung aufgeführt.

| Regelparserfehler | Fehlerverwendung | Korrigierte Verwendung |
| --- | --- | --- |
| Fehler Das Attribut wird nicht unterstützt. |(user.invalidProperty -eq "Value") |(user.department -eq "value")<br/><br/>Stellen Sie sicher, dass das Attribut in der [Liste der unterstützten Eigenschaften](groups-dynamic-membership.md#supported-properties) aufgeführt wird. |
| Fehler Der Operator wird für das Attribut nicht unterstützt. |(user.accountEnabled -contains true) |(user.accountEnabled -eq true)<br/><br/>Der Operator wird für den Eigenschaftentyp nicht unterstützt (in diesem Beispiel kann „-contains“ nicht für den Booleschen Typ verwendet werden). Verwenden Sie die richtige Operatoren für den Eigenschaftentyp. |
| Fehler Abfragekompilierungsfehler. | 1. (user.department -eq "Sales") (user.department -eq "Marketing")<br>2. (user.userPrincipalName -match "*@domain.ext") | 1. Fehlender Operator. Verwenden Sie „-and“ oder „-or“, um zwei Prädikate zu verknüpfen.<br>(user.department -eq "Sales") -or (user.department -eq "Marketing")<br>2. Fehler im regulären Ausdruck, der mit „-match“ verwendet wird<br>(user.userPrincipalName -match ".*@domain.ext")<br>oder alternativ: (user.userPrincipalName -match "@domain.ext$") |

## <a name="next-steps"></a>Nächste Schritte

Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](../fundamentals/active-directory-manage-groups.md)
* [Anwendungsverwaltung in Azure Active Directory](../manage-apps/what-is-application-management.md)
* [Was ist Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
