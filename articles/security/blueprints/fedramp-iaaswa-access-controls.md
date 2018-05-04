---
title: Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP – Zugriffssteuerung
description: Automatisierung von Webanwendungen für FedRAMP – Zugriffssteuerung
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: f7e6cd8f-b2df-4db6-8332-de97d86c5281
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 3d30f889c4d7ed0dafcf8559e8987090c03f4d5e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="access-control-ac"></a>Zugriffssteuerung (Access Control, AC)

> [!NOTE]
> Diese Regelungen werden von NIST und dem U.S. Department of Commerce als Teil der NIST-Sonderveröffentlichung 800-53 Revision 4 definiert. Informationen zu Testverfahren und Anleitungen für die einzelnen Regelungen finden Sie unter NIST 800-53 Rev. 4.

## <a name="nist-800-53-control-ac-1"></a>NIST 800-53, Regelung AC-1

#### <a name="access-control-policy-and-procedures"></a>Zugriffssteuerungsrichtlinie und -verfahren

**AC-1** Die Organisation entwickelt, dokumentiert und verbreitet an [Festsetzung: von der Organisation definiertes Personal oder Rollen] eine Zugriffssteuerungsrichtlinie, die sich mit Zweck, Umfang, Rollen, Zuständigkeiten, Verwaltungspflichten, Koordination zwischen Organisationsentitäten und Konformität befasst, stellt Verfahren zum Ermöglichen der Implementierung der Zugriffssteuerungsrichtlinie und der damit verbundenen Zugriffssteuerungsmaßnahmen bereit und überprüft und aktualisiert die aktuelle Zugriffssteuerungsrichtlinie [Festsetzung: von der Organisation definierte Häufigkeit] und die Sicherheitsplanungsverfahren [Festsetzung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinien und -verfahren des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-2a"></a>NIST 800-53, Regelung AC-2.a

#### <a name="account-management"></a>Kontoverwaltung

**AC-2.a** Die Organisation benennt die folgenden Arten von Informationssystemkonten zur Unterstützung von Organisationsmissionen/Geschäftsfunktionen und wählt sie aus: [Festsetzung: von der Organisation definierte Informationssystemkonten].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | In diesem Blueprint werden folgende Systemkontotypen zugrunde gelegt und implementiert: Azure Active Directory-Benutzer (dienen zum Bereitstellen der Lösung und zum Verwalten des Zugriffs auf Azure-Ressourcen), Windows-Benutzer (werden von Active Directory verwaltet) und SQL Server-Dienstkonto. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-2b"></a>NIST 800-53, Regelung AC-2.b

#### <a name="account-management"></a>Kontoverwaltung

**AC-2.b** Die Organisation benennt Konto-Manager für Informationssystemkonten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für das Benennen von Managern für die unter AC 02.a benannten Konten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-2c"></a>NIST 800-53, Regelung AC-2.c

#### <a name="account-management"></a>Kontoverwaltung

**AC-2.c** Die Organisation stellt Bedingungen für Gruppen- und Rollenmitgliedschaften auf.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für das Aufstellen von Kriterien für Rollen- und Gruppenmitgliedschaften für kundenseitig gesteuerte Kontotypen (siehe AC-02.a). |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-2d"></a>NIST 800-53, Regelung AC-2.d

#### <a name="account-management"></a>Kontoverwaltung

**AC-2.d** Die Organisation spezifiziert autorisierte Informationssystembenutzer, Gruppen- und Rollenmitgliedschaften und Zugriffsautorisierungen (z.B. Berechtigungen) und ggf. weitere Attribute für jedes Konto.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist berechtigt, einen bewährten Vorgang zur Kontoautorisierung auf Unternehmensebene einzusetzen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-2e"></a>NIST 800-53, Regelung AC-2.e

#### <a name="account-management"></a>Kontoverwaltung

**AC-2.e** Die Organisation verlangt Genehmigungen durch [Festsetzung: von der Organisation definierte Mitarbeiter oder Rollen] für die Beantragung der Erstellung von Informationssystemkonten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist berechtigt, einen bewährten Vorgang zur Kontoautorisierung auf Unternehmensebene einzusetzen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-2f"></a>NIST 800-53, Regelung AC-2.f

#### <a name="account-management"></a>Kontoverwaltung

**AC-2.f** Die Organisation erstellt, aktiviert, ändert, deaktiviert und entfernt Informationssystemkonten entsprechend den [Festsetzung: von der Organisation definierten Verfahren oder Bedingungen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist berechtigt, einen bewährten Vorgang zur Kontoverwaltung auf Unternehmensebene einzusetzen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-2g"></a>NIST 800-53, Regelung AC-2.g

#### <a name="account-management"></a>Kontoverwaltung

**AC-2.g** Die Organisation überwacht die Nutzung der Informationssystemkonten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Diese Blaupause implementiert das Dashboard für Identität und Zugriff aus der Sicherheits- und Überwachungslösung. Dieses Dashboard ermöglicht Konto-Managern das Überwachen der Nutzung von Informationssystemkonten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-2h"></a>NIST 800-53, Regelung AC-2.h

#### <a name="account-management"></a>Kontoverwaltung

**AC-2.h** Die Organisation benachrichtigt Konto-Manager, wenn Konten nicht mehr benötigt werden, Benutzer ausscheiden oder versetzt werden oder die Informationssystemnutzung einzelner oder der Wissensbedarf sich ändern.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsverfahren des Kunden auf Unternehmensebene können einen Vorgang implementieren, bei dem der zuständige Konto-Manager informiert wird, sobald ein Konto nicht mehr benötigt wird. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-2i"></a>NIST 800-53, Regelung AC-2.i

#### <a name="account-management"></a>Kontoverwaltung

**AC-2.i** Die Organisation autorisiert den Zugriff auf das Informationssystem auf Grundlage einer gültigen Zugriffsautorisierung, der vorgesehenen Systemnutzung und weiterer Attribute entsprechend den Anforderungen der Organisation oder der zugehörigen Organisationsmissionen oder Geschäftsfunktionen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsverfahren des Kunden auf Unternehmensebene können einen Vorgang zur Zugriffsautorisierung implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-2j"></a>NIST 800-53, Regelung AC-2.j

#### <a name="account-management"></a>Kontoverwaltung

**AC-2.j** Die Organisation überprüft Konten [Festsetzung: von der Organisation definierte Häufigkeit] auf Einhaltung der Anforderungen zur Kontoverwaltung.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Überprüfung kundenseitig gesteuerter Konten mit der erforderlichen Häufigkeit, um festzustellen, ob Konten alle Anforderungen der Organisation einhalten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-2k"></a>NIST 800-53, Regelung AC-2.k

#### <a name="account-management"></a>Kontoverwaltung

**AC-2.k** Die Organisation implementiert einen Vorgang, durch den für freigegebene oder Gruppenkonten (sofern vorhanden) bei Entfernung von Personen aus der Gruppe neue Anmeldeinformationen ausgestellt werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsverfahren des Kunden auf Unternehmensebene können einen Vorgang zum Verwalten von Anmeldeinformationen für Gruppenkonten implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-1"></a>NIST 800-53, Regelung AC-2 (1)

#### <a name="account-management--automated-system-account-management"></a>Kontoverwaltung | Automatisierte Systemkontoverwaltung

**AC-2 (1)** Die Organisation setzt automatisierte Mechanismen zur Unterstützung der Verwaltung von Informationssystemkonten ein.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Diese Blaupause implementiert das Dashboard für Identität und Zugriff aus der Sicherheits- und Überwachungslösung. Dieses Dashboard ermöglicht Konto-Managern das Überwachen der Nutzung von Informationssystemkonten. Es kann so konfiguriert werden, dass Warnungen gesendet werden, wenn ungewöhnliche Aktivitäten vermutet werden oder andere vordefinierte Ereignisse auftreten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-2"></a>NIST 800-53, Regelung AC-2 (2)

#### <a name="account-management--removal-of-temporary--emergency-accounts"></a>Kontoverwaltung | Entfernen von temporären oder Notfallkonten

**AC-2 (2)** Das Informationssystem [Auswahl: entfernt; deaktiviert] temporäre und Notfallkonten automatisch nach [Festsetzung: von der Organisation definiertem Zeitraum je Kontotyp].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt keine temporären Konten oder Notfallkonten bereit. Sofern keine manuelle Deaktivierung erfolgt, deaktiviert der bereitgestellte Domänencontroller alle inaktiven Konten automatisch nach 35 Tagen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-3"></a>NIST 800-53, Regelung AC-2 (3)

#### <a name="account-management--disable-inactive-accounts"></a>Kontoverwaltung | Inaktive Konten deaktivieren

**AC-2 (3)** Das Informationssystem deaktiviert inaktive Konten automatisch nach [Festsetzung: von der Organisation definierter Zeitraum].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Domänencontroller, der von diesem Blueprint bereitgestellt wird, ist so konfiguriert, dass er alle Benutzerkonten nach 35 Tagen Inaktivität deaktiviert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-4"></a>NIST 800-53, Regelung AC-2 (4)

#### <a name="account-management--automated-audit-actions"></a>Kontoverwaltung | Automatisierte Überwachung von Aktionen

**AC-2 (4)** Das Informationssystem überwacht automatisch alle Aktionen zur Erstellung, Änderung, Aktivierung, Deaktivierung und Entfernung von Konten und benachrichtigt [Festsetzung: von der Organisation definierte Mitarbeiter oder Rollen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint implementiert die folgenden Systemkontotypen: Azure Active Directory-Benutzer, Windows-Benutzer und SQL Server-Dienstkonto. Aktionen bei der Azure Active Directory-Kontoverwaltung generieren ein Ereignis im Azure-Aktivitätsprotokoll; Aktionen bei der Verwaltung von Betriebssystemebenenkonten generieren ein Ereignis im Systemprotokoll. Diese Protokolle werden von Log Analytics gesammelt und im Log Analytics-Arbeitsbereich gespeichert. Log Analytics kann so konfiguriert werden, dass beim Auftreten vordefinierter Ereignisse Warnungen versendet werden.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-5"></a>NIST 800-53, Regelung AC-2 (5)

#### <a name="account-management--inactivity-logout"></a>Kontoverwaltung | Abmeldung nach Inaktivität

**AC-2 (5)** Die Organisation verlangt eine Abmeldung der Benutzer bei [Festsetzung: von der Organisation definierter Zeitraum der erwarteten Inaktivität oder Beschreibung des Abmeldezeitpunkts].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinie des Kunden auf Unternehmensebene kann eine Richtlinie definieren, laut der Benutzer abgemeldet werden, wenn sie voraussichtlich für einen gewissen Zeitraum (oder aus anderen Gründen) inaktiv sind. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-7a"></a>NIST 800-53, Regelung AC-2 (7).a

#### <a name="account-management--role-based-schemes"></a>Kontoverwaltung | Rollenbasierte Schemas

**AC-2 (7).a** Die Organisation erstellt und verwaltet privilegierte Benutzerkonten gemäß einem Zugriffsschema auf Rollenbasis, mit dem der Zugriff auf und die Rechte im Informationssystem mithilfe von Rollen organisiert werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint implementiert die folgenden Systemkontotypen: Azure Active Directory-Benutzer, Windows-Benutzer und SQL Server-Dienstkonto. Azure Active Directory-Kontoberechtigungen werden mithilfe der rollenbasierten Zugriffssteuerung implementiert, wobei Benutzern Rollen zugewiesen werden. Berechtigungen für Active Directory-Konten werden mithilfe der rollenbasierten Zugriffssteuerung implementiert, indem Benutzer Sicherheitsgruppen zugewiesen werden. Diese rollenbasierten Schemas können vom Kunden zur Erfüllung unternehmenswichtiger Anforderungen erweitert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-7b"></a>NIST 800-53, Regelung AC-2 (7).b

#### <a name="account-management--role-based-schemes"></a>Kontoverwaltung | Rollenbasierte Schemas

**AC-2 (7).b** Die Organisation überwacht die Zuweisungen privilegierter Rollen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Diese Blaupause implementiert das Dashboard für Identität und Zugriff aus der Sicherheits- und Überwachungslösung. Dieses Dashboard ermöglicht Konto-Managern das Überwachen der Nutzung von Informationssystemkonten. Diese Lösung kann abgefragt werden, um privilegierte Rollenzuweisungen zu melden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-7c"></a>NIST 800-53, Regelung AC-2 (7).c

#### <a name="account-management--role-based-schemes"></a>Kontoverwaltung | Rollenbasierte Schemas

**AC-2 (7).c** Das Unternehmen ergreift Maßnahmen wie [Festsetzung: von der Organisation definierte Aktionen], wenn die Zuweisungen privilegierter Rollen nicht mehr angemessen ist.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für das Ergreifen von Maßnahmen für kundenseitig gesteuerte Konten, wenn Zuweisungen privilegierter Rollen nicht mehr angemessen sind. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-9"></a>NIST 800-53, Regelung AC-2 (9)

#### <a name="account-management--restrictions-on-use-of-shared--group-accounts"></a>Kontoverwaltung | Einschränkungen bei der Verwendung von freigegebenen oder Gruppenkonten

**AC-2 (9)** Die Organisation gestattet ausschließlich die Verwendung solcher freigegebener oder Gruppenkonten, die [Festsetzung: von der Organisation definierte Bedingungen für die Einrichtung von freigegebenen oder Gruppenkonten] erfüllen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Für Ressourcen, die von diesem Blueprint bereitgestellt werden, werden keine gemeinsam genutzten Konten oder Gruppenkonten aktiviert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-10"></a>NIST 800-53, Regelung AC-2 (10)

#### <a name="account-management--shared--group-account-credential-termination"></a>Kontoverwaltung | Stornierung von Anmeldeinformationen von freigegebenen oder Gruppenkonten

**AC-2 (10)** Das Informationssystem storniert die Anmeldeinformationen für freigegebene oder Gruppenkonten, wenn Mitglieder die Gruppe verlassen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Für Ressourcen, die von diesem Blueprint bereitgestellt werden, werden keine gemeinsam genutzten Konten oder Gruppenkonten aktiviert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-11"></a>NIST 800-53, Regelung AC-2 (11)

#### <a name="account-management--usage-conditions"></a>Kontoverwaltung | Nutzungsbedingungen

**AC-2 (11)** Das Informationssystem erzwingt [Festsetzung: von der Organisation definierte Umstände und/oder die Nutzungsbedingungen] für [Festsetzung: von der Organisation definierte Informationssystemkonten].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt einen Domänencontroller bereit, mit dem alle bereitgestellten virtuellen Computer verknüpft sind. Eine Gruppenrichtlinie kann in Active Directory eingerichtet und so konfiguriert werden, dass tageszeitliche Beschränkungen oder sonstige Bedingungen für die Kontonutzung implementiert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-12a"></a>NIST 800-53, Regelung AC-2 (12).a

#### <a name="account-management--account-monitoring--atypical-usage"></a>Kontoverwaltung | Kontoüberwachung/ungewöhnliche Nutzung

**AC-2 (12).a** Die Organisation überwacht Informationssystemkonten auf [Festsetzung: von der Organisation definierte ungewöhnliche Nutzung].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Diese Blaupause implementiert das Dashboard für Identität und Zugriff aus der Sicherheits- und Überwachungslösung. Dieses Dashboard ermöglicht Konto-Managern die Überwachung von Zugriffsversuchen auf bereitgestellte Ressourcen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-12b"></a>NIST 800-53, Regelung AC-2 (12).b

#### <a name="account-management--account-monitoring--atypical-usage"></a>Kontoverwaltung | Kontoüberwachung/ungewöhnliche Nutzung

**AC-2 (12).b** Die Organisation meldet eine ungewöhnliche Nutzung der Informationssystemkonten an [Festsetzung: von der Organisation definierte Mitarbeiter oder Rollen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Diese Blaupause implementiert das Dashboard für Identität und Zugriff aus der Sicherheits- und Überwachungslösung. Dieses Dashboard ermöglicht Konto-Managern die Überwachung von Zugriffsversuchen auf bereitgestellte Ressourcen. Diese Lösung kann so konfiguriert werden, dass Warnungen gesendet werden, wenn ungewöhnliche Aktivitäten vermutet werden oder andere vordefinierte Ereignisse auftreten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-2-13"></a>NIST 800-53, Regelung AC-2 (13)

#### <a name="account-management--disable-accounts-for-high-risk-individuals"></a>Kontoverwaltung | Deaktivieren von Konten für Hochrisikopersonen

**AC-2 (13)** Die Organisation deaktiviert Konten von Benutzern, von denen ein erhebliches Risiko innerhalb von [Festsetzung: von der Organisation definierter Zeitraum] nach Risikoerkennung ausgeht.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinien und -verfahren des Kunden auf Unternehmensebene können Bedingungen für das Deaktivieren der Konten von Benutzern implementieren, von denen ein erhebliches Risiko für die Organisation ausgeht. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-3"></a>NIST 800-53, Regelung AC-3

#### <a name="access-enforcement"></a>Zugriffserzwingung

**AC-3** Das Informationssystem erzwingt genehmigte Autorisierungen für den logischen Zugriff auf Daten und Systemressourcen entsprechend den gültigen Zugriffssteuerungsrichtlinien.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint erzwingt die Autorisierungen für den logischen Zugriff mithilfe der rollenbasierten Zugriffssteuerung, die von Azure Active Directory durch Zuweisen von Rollen zu Benutzern, von Active Directory durch Zuweisen von Benutzern zu Sicherheitsgruppen sowie durch Maßnahmen auf der Windows-Betriebssystemebene erzwungen werden. Azure Active Directory-Rollen, die Benutzern oder Gruppen zugewiesen sind, steuern den logischen Zugriff auf Ressourcen in Azure auf Ressourcen-, Gruppen- oder Abonnementebene. Active Directory-Sicherheitsgruppen steuern den logischen Zugriff auf Ressourcen und Funktionen auf Betriebssystemebene. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-4"></a>NIST 800-53, Regelung AC-4

#### <a name="information-flow-enforcement"></a>Erzwingung des Datenflusses

**AC-4** Das Informationssystem erzwingt genehmigte Autorisierungen zum Steuern des Informationsflusses in das System und zwischen verbundenen Systemen basierend auf [Festsetzung: von der Organisation definierten Richtlinien zur Informationsflusssteuerung].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint erzwingt Informationsflussbeschränkungen mithilfe von Netzwerksicherheitsgruppen, die auf die Subnetze, in denen Ressourcen bereitgestellt werden, sowie auf Application Gateway und den Lastenausgleich angewendet werden. Netzwerksicherheitsgruppen stellen sicher, dass der Informationsfluss zwischen Ressourcen auf Grundlage genehmigter Regeln gesteuert wird. Application Gateway und der Lastenausgleich routen Datenverkehr auf Grundlage genehmigter Regeln dynamisch an bestimmte Ressourcen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-4-8"></a>NIST 800-53, Regelung AC-4 (8)

#### <a name="information-flow-enforcement--security-policy-filters"></a>Informationsflusserzwingung | Sicherheitsrichtlinienfilter

**AC-4 (8)** Das Informationssystem erzwingt eine Informationsflussregelung mithilfe von [Festsetzung: von der Organisation definierten Sicherheitsrichtlinienfilter] als Grundlage für Flusssteuerungsentscheidungen für [Festsetzung: von der Organisation definierte Informationsflüsse].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Erzwingung der Informationsflusssteuerung zwischen kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-4-21"></a>NIST 800-53, Regelung AC-4 (21)

#### <a name="information-flow-enforcement--physical--logical-separation-of-information-flows"></a>Informationsflusserzwingung| Physische/logische Trennung von Informationsflüssen

**AC-4 (21)** Das Informationssystem trennt Informationsflüsse logisch oder physisch mithilfe von [Festsetzung: von der Organisation definierten Mechanismen und/oder Verfahren], um [Festsetzung: von der Organisation definierte erforderliche Trennungen nach Datentypen] zu erzielen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Trennung der Informationsflüsse zwischen kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-5a"></a>NIST 800-53, Regelung AC-5.a

#### <a name="separation-of-duties"></a>Aufgabentrennung

**AC-5.a** Die Organisation trennt [Festsetzung: von der Organisation definierte Aufgaben einzelner Personen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Trennung der Aufgaben mithilfe kundenseitig gesteuerter Konten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-5b"></a>NIST 800-53, Regelung AC-5.b

#### <a name="separation-of-duties"></a>Aufgabentrennung

**AC-5.b** Die Organisation dokumentiert die Trennung von Aufgaben einzelner Personen voneinander.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Dokumentation der Trennung von Aufgaben einzelner Personen mithilfe kundenseitig gesteuerter Konten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-5c"></a>NIST 800-53, Regelung AC-5.c

#### <a name="separation-of-duties"></a>Aufgabentrennung

**AC-5.c** Die Organisation definiert Autorisierungen für den Informationssystemzugriff zur Unterstützung der Aufgabentrennung.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint implementiert eine rollenbasierte Zugriffssteuerung, mit der die Trennung von Aufgaben entsprechend den Anforderungen der Organisation konfiguriert werden kann. Azure Active Directory-Kontoberechtigungen werden mithilfe der rollenbasierten Zugriffssteuerung implementiert, wobei Benutzern Rollen zugewiesen werden. Berechtigungen für Active Directory-Konten werden mithilfe der rollenbasierten Zugriffssteuerung implementiert, indem Benutzer Sicherheitsgruppen zugewiesen werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-6"></a>NIST 800-53, Regelung AC-6

#### <a name="least-privilege"></a>Ansatz der geringsten Rechte

**AC-6** Die Organisation setzt den Ansatz der geringsten Rechte um, d.h., autorisierter Zugriff wird nur solchen Benutzern (oder Prozessen, die im Namen von Benutzern handeln) gewährt, die benötigt werden, um die zugewiesenen Aufgaben entsprechend den Organisationszielen und Geschäftsfunktionen erfüllen zu können.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint implementiert eine rollenbasierte Zugriffssteuerung, um Benutzer auf explizit zugewiesene Berechtigungen zu beschränken. Azure Active Directory-Kontoberechtigungen werden mithilfe der rollenbasierten Zugriffssteuerung implementiert, wobei Benutzern Rollen zugewiesen werden. Berechtigungen für Active Directory-Konten werden mithilfe der rollenbasierten Zugriffssteuerung implementiert, indem Benutzer Sicherheitsgruppen zugewiesen werden.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-6-1"></a>NIST 800-53, Regelung AC-6 (1)

#### <a name="least-privilege--authorize-access-to-security-functions"></a>Ansatz der geringsten Rechte | Autorisieren des Zugriffs auf Sicherheitsfunktionen

**AC-6 (1)** Die Organisation autorisiert ausdrücklich den Zugriff auf [Festsetzung: von der Organisation definierte Sicherheitsfunktionen (implementiert in Form von Hardware, Software und Firmware) und sicherheitsrelevante Informationen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsverfahren des Kunden auf Unternehmensebene können einen Vorgang zur Zugriffsautorisierung implementieren, der Zugriff auf Sicherheitsfunktionen umfasst. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-6-2"></a>NIST 800-53, Regelung AC-6 (2)

#### <a name="least-privilege--non-privileged-access-for-nonsecurity-functions"></a>Ansatz der geringsten Rechte | Nicht privilegierter Zugriff auf nicht sicherheitsrelevante Funktionen

**AC-6 (2)** Die Organisation verlangt von Benutzern von Informationssystemkonten oder Rollen, die Zugriff auf [Festsetzung: von der Organisation definierte Sicherheitsfunktionen oder sicherheitsrelevante Informationen] haben, die Verwendung nicht privilegierter Konten oder Rollen beim Zugriff auf nicht sicherheitsrelevante Funktionen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinie des Kunden auf Unternehmensebene kann Benutzern beim Zugriff auf nicht sicherheitsrelevante Funktionen die Verwendung nicht privilegierter Konten vorschreiben. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-6-3"></a>NIST 800-53, Regelung AC-6 (3)

#### <a name="least-privilege--network-access-to-privileged-commands"></a>Ansatz der geringsten Rechte | Netzwerkzugriff auf privilegierte Befehle

**AC-6 (3)** Die Organisation autorisiert den Netzwerkzugriff auf [Festsetzung: von der Organisation definierte privilegierte Befehle] nur für [Festsetzung: von der Organisation definierte, zwingend erforderliche geschäftliche Zwecke] und dokumentiert die Gründe für diesen Zugriff im Sicherheitsplan für das Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinie des Kunden auf Unternehmensebene kann privilegierte Befehle definieren, auf die über ein Netzwerk zugegriffen werden darf. Hinweis: Kunden haben keinen physischen Zugriff auf die Azure-Infrastruktur. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-6-5"></a>NIST 800-53, Regelung AC-6 (5)

#### <a name="least-privilege--privileged-accounts"></a>Ansatz der geringsten Rechte | Privilegierte Konten

**AC-6 (5)** Die Organisation beschränkt privilegierte Konten auf dem Informationssystem auf [Festsetzung: von der Organisation definierte Mitarbeiter oder Rollen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinie des Kunden auf Unternehmensebene kann Beschränkungen für die Verwendung privilegierter Konten definieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-6-7a"></a>NIST 800-53, Regelung AC-6 (7).a

#### <a name="least-privilege--review-of-user-privileges"></a>Ansatz der geringsten Rechte | Kontrolle von Benutzerberechtigungen

**AC-6 (7).a** Die Organisation kontrolliert [Festsetzung: von der Organisation definierte Häufigkeit] die [Festsetzung: von der Organisation definierten Rollen oder Klassen von Benutzern] zugewiesenen Berechtigungen, um deren Notwendigkeit zu überprüfen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Kontrolle der Benutzerberechtigungen kundenseitig gesteuerter Konten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-6-7b"></a>NIST 800-53, Regelung AC-6 (7).b

#### <a name="least-privilege--review-of-user-privileges"></a>Ansatz der geringsten Rechte | Kontrolle von Benutzerberechtigungen

**AC-6 (7).b** Die Organisation weist Berechtigungen entsprechend den Organisationszielen bzw. geschäftlichen Anforderungen im notwendigen Maße neu zu oder entfernt sie.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist im Bedarfsfall verantwortlich für die erneute Zuweisung oder Entfernung von Berechtigungen für kundenseitig gesteuerter Konten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-6-8"></a>NIST 800-53, Regelung AC-6 (8)

#### <a name="least-privilege--privilege-levels-for-code-execution"></a>Ansatz der geringsten Rechte | Berechtigungsebenen für die Ausführung von Code

**AC-6 (8)** Das Informationssystem verhindert die Ausführung von [Festsetzung: von der Organisation definierter Software] auf einer höheren Berechtigungsebene als derjenigen des ausführenden Benutzers.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint implementiert eine rollenbasierte Zugriffssteuerung, um Benutzer auf explizit zugewiesene Berechtigungen zu beschränken. Schutzmaßnahmen virtueller Computer auf Betriebssystemebene gestatten keine Ausführung von Software auf einer höheren Berechtigungsebene als der des ausführenden Benutzers. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-6-9"></a>NIST 800-53, Regelung AC-6 (9)

#### <a name="least-privilege--auditing-use-of-privileged-functions"></a>Ansatz der geringsten Rechte | Überwachen der Verwendung privilegierter Funktionen

**AC-6 (9)** Das Informationssystem überwacht die Ausführung privilegierter Funktionen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Diese Blaupause implementiert den Log Analytics-Dienst. Bereitgestellte virtuelle Computer und Azure-Diagnosespeicherkonten sind verbundene Datenquellen für Log Analytics. Auf diese Weise wird sichergestellt, dass die Ausführung privilegierter Funktionen überwacht wird. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-6-10"></a>NIST 800-53, Regelung AC-6 (10)

#### <a name="least-privilege--prohibit-non-privileged-users-from-executing-privileged-functions"></a>Ansatz der geringsten Rechte | Unterbinden der Ausführung privilegierter Funktionen durch unberechtigte Benutzer

**AC-6 (10)** Das Informationssystem verhindert die Ausführung privilegierter Funktionen einschließlich der Deaktivierung, Umgehung oder Modifikation implementierter Sicherheitsvorrichtungen und Gegenmaßnahmen durch unberechtigte Benutzer.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint implementiert eine rollenbasierte Zugriffssteuerung, um Benutzer auf explizit zugewiesene Berechtigungen zu beschränken.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-7a"></a>NIST 800-53, Regelung AC-7.a

#### <a name="unsuccessful-logon-attempts"></a>Fehlgeschlagene Anmeldeversuche

**AC-7.a** Das Informationssystem erzwingt eine Begrenzung von [Festsetzung: von der Organisation definierte Anzahl] aufeinander folgenden ungültigen Anmeldeversuchen desselben Benutzers innerhalb von [Festsetzung: von der Organisation definierter Zeitraum].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das Azure-Portal beschränkt aufeinander folgende ungültige Anmeldeversuche von Benutzern. Auf der Betriebssystemebene wird eine Gruppenrichtlinie auf alle virtuellen Computer angewendet, die von diesem Blueprint bereitgestellt werden. Die Richtlinie beschränkt die Anzahl ungültiger aufeinander folgender Anmeldeversuche von Benutzern auf maximal drei in einem Zeitraum von 15 Minuten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-7b"></a>NIST 800-53, Regelung AC-7.b

#### <a name="unsuccessful-logon-attempts"></a>Fehlgeschlagene Anmeldeversuche

**AC-7.b** Das Informationssystem [Auswahl: sperrt das Konto/den Knoten für [Festsetzung: von der Organisation definierten Zeitraum]; sperrt das Konto/den Knoten bis zur Freigabe durch einen Administrator; verzögert die nächste Anmeldeaufforderung entsprechend [Festsetzung: von der Organisation definiertem Verzögerungsalgorithmus]], wenn die maximale Anzahl fehlerhafter Versuche überschritten wurde.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das Azure-Portal sperrt Konten nach aufeinanderfolgenden ungültigen Anmeldeversuchen von Benutzern. Auf der Betriebssystemebene wird eine Gruppenrichtlinie auf alle virtuellen Computer angewendet, die von diesem Blueprint bereitgestellt werden. Die Richtlinie sperrt Konten nach drei aufeinander folgenden ungültigen Benutzeranmeldeversuchen für drei Stunden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-7-2"></a>NIST 800-53, Regelung AC-7 (2)

#### <a name="unsuccessful-logon-attempts--purge--wipe-mobile-device"></a>Anzahl fehlgeschlagener Anmeldeversuche | Löschen/Zurücksetzen mobiler Geräte

**AC-7 (2)** Das Informationssystem löscht [Festsetzung: von der Organisation definierte Mobilgeräte] entsprechend den [Festsetzung: von der Organisation definierten Lösch- oder Zurücksetzungsanforderungen oder -verfahren] nach [Festsetzung: von der Organisation definierte Anzahl] fehlgeschlagenen aufeinander folgenden Anmeldeversuchen oder führt eine Zurücksetzung aus.

**Zuständigkeiten:** `Not Applicable`

|||
|---|---|
| **Kunde** | Mobile Geräte gehören nicht zum Geltungsbereich von in Azure bereitgestellten Systemen. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure gestattet keine mobilen Geräte innerhalb der Grenzen von Azure. Aufgrund dessen ist diese Regelung auf Microsoft Azure nicht anwendbar. |


 ## <a name="nist-800-53-control-ac-8a"></a>NIST 800-53, Regelung AC-8.a

#### <a name="system-use-notification"></a>Benachrichtigung zur Systemnutzung

**AC-8.a** Das Informationssystem zeigt Benutzern [Aufgabe: eine von der Organisation definierte Nachricht oder ein Banner zur Systemnutzung] an, bevor der Zugriff auf das System gewährt wird, um Datenschutz- und Sicherheitshinweise entsprechend den gültigen US-amerikanischen Bundesgesetzen, Präsidentenverfügungen, Anordnungen, Verordnungen, Standards und Leitlinien anzugeben, und weist außerdem darauf hin, dass Benutzer auf ein Informationssystem der US-Regierung zugreifen, dass die Nutzung des Informationssystems möglicherweise überwacht, aufgezeichnet und überprüft wird, dass die Nutzung des Informationssystems durch Unbefugte untersagt ist und zivil- und strafrechtlich verfolgt werden kann und dass sich der Benutzer durch die Nutzung des Informationssystems mit der Überwachung und Aufzeichnung einverstanden erklärt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt einen Domänencontroller bereit, mit dem alle bereitgestellten virtuellen Computer verknüpft sind. Eine Gruppenrichtlinie implementiert eine Benachrichtigung zur Systemnutzung, die Benutzern vor der Anmeldung angezeigt wird. Hinweis: Dieser Blueprint implementiert eine exemplarische Systemnutzungsbenachrichtigung. Der Kunde muss den Text bearbeiten, um die Anforderungen der Organisation und/oder des Gesetzgebers zu erfüllen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-8b"></a>NIST 800-53, Regelung AC-8.b

#### <a name="system-use-notification"></a>Benachrichtigung zur Systemnutzung

**AC-8.b** Das Informationssystem belässt die Benachrichtigungsmeldung oder das Banner auf dem Bildschirm, bis der Benutzer die Nutzungsbedingungen bestätigt und eindeutige Handlungen durchführt, um sich anzumelden und nachfolgend auf das Informationssystem zuzugreifen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt einen Domänencontroller bereit, mit dem alle bereitgestellten virtuellen Computer verknüpft sind. Eine Gruppenrichtlinie implementiert eine Benachrichtigung zur Systemnutzung, die Benutzern vor der Anmeldung angezeigt wird. Der Benutzer muss die Benachrichtigung bestätigen, um sich anmelden zu können. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-8c"></a>NIST 800-53, Regelung AC-8.c

#### <a name="system-use-notification"></a>Benachrichtigung zur Systemnutzung

**AC-8.c** Das Informationssystem zeigt bei öffentlich zugänglichen Systemen Systemnutzungsinformationen [Festsetzung: von der Organisation definierte Bedingungen] vor Genehmigung des weiteren Zugriffs sowie ggf. Verweise zu Überwachung, Aufzeichnung und Kontrolle an, die den Datenschutzvorgaben für solche Systeme genügen, bei denen derartige Aktivitäten im Allgemeinen verboten sind, und gibt eine Beschreibung der zulässigen Nutzung solcher Systeme.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für das Anzeigen von Benachrichtigungen zur Systemnutzung auf allen öffentlich zugänglichen, kundenseitig bereitgestellten Ressourcen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-10"></a>NIST 800-53, Regelung AC-10

#### <a name="concurrent-session-control"></a>Steuerung gleichzeitiger Sitzungen

**AC-10** Das Informationssystem begrenzt die Anzahl gleichzeitiger Sitzungen für jedes/jeden [Festsetzung: von der Organisation definiertes Konto und/oder Kontotyp] auf [Festsetzung: Organisation definierte Anzahl].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Eine Betriebssystemrichtlinie wird für alle virtuellen Computer implementiert, die durch diesen Blueprint bereitgestellt werden. Die Richtlinie implementiert Beschränkungen der Anzahl gleichzeitiger Sitzungen (zwei Sitzungen). |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-11a"></a>NIST 800-53, Regelung AC-11.a

#### <a name="session-lock"></a>Sitzungssperre

**AC-11.a** Das Informationssystem verhindert einen weiteren Zugriff auf das System durch Initiieren einer Sitzungssperre nach [Festsetzung: von der Organisation definierter Zeitraum] Inaktivität oder dem Empfang einer entsprechenden Benutzeranforderung.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt einen Domänencontroller bereit, mit dem alle bereitgestellten virtuellen Computer verknüpft sind. Eine Gruppenrichtlinie implementiert eine Inaktivitätssperre für RDP-Sitzungen. Benutzer können die Sperre auch manuell initiieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-11b"></a>NIST 800-53, Regelung AC-11.b

#### <a name="session-lock"></a>Sitzungssperre

**AC-11.b** Das Informationssystem erhält die Sitzungssperre aufrecht, bis der Benutzer den Zugriff mithilfe festgelegter Identifizierungs- und Authentifizierungsverfahren erneut herstellt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt einen Domänencontroller bereit, mit dem alle bereitgestellten virtuellen Computer verknüpft sind. Eine Gruppenrichtlinie implementiert eine Inaktivitätssperre für RDP-Sitzungen. Benutzer müssen sich erneut authentifizieren, um die Sitzungssperre aufzuheben.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-11-1"></a>NIST 800-53, Regelung AC-11 (1)

#### <a name="session-lock--pattern-hiding-displays"></a>Sitzungssperre | Unkenntlichmachung der Anzeige

**AC-11 (1)** Das Informationssystem verbirgt über die Sitzungssperre Informationen, die zuvor auf dem Anzeigegerät sichtbar waren, mit einem allgemein sichtbaren Bild.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt einen Domänencontroller bereit, mit dem alle bereitgestellten virtuellen Computer verknüpft sind. Eine Gruppenrichtlinie implementiert eine Inaktivitätssperre für RDP-Sitzungen. Die Sitzungssperre verbirgt Informationen, die zuvor sichtbar waren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-12"></a>NIST 800-53, Regelung AC-12

#### <a name="session-termination"></a>Beendigung der Sitzung

**AC-12** Das Informationssystem beendet eine Benutzersitzung automatisch nach [Festsetzung: von der Organisation definierten Bedingungen oder Auslöseereignissen, die eine Trennung der Sitzung erfordern].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Remotedesktop-Sitzungshostkonfiguration für die virtuellen Windows-Computer, die durch diesen Blueprint bereitgestellt werden, können so konfiguriert werden, dass sie die Anforderungen der Organisation an die Sitzungsbeendigung erfüllen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-12-1a"></a>NIST 800-53, Regelung AC-12 (1).a

#### <a name="session-termination--user-initiated-logouts--message-displays"></a>Beendigung der Sitzung| Benutzerseitig eingeleitete Abmeldungen/Meldungsanzeigen

**AC-12 (1).a** Das Informationssystem stellt eine Abmeldefunktion für benutzerseitig initiierte Kommunikationssitzungen bereit, wenn eine Authentifizierung verwendet wird, um Zugriff auf [Festsetzung: von der Organisation definierte Datenressourcen] zu erhalten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das Azure-Portal und die VM-Betriebssysteme, die durch diesen Blueprint bereitgestellt werden, ermöglichen Benutzern das Einleiten einer Abmeldung. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-12-1b"></a>NIST 800-53, Regelung AC-12 (1).b

#### <a name="session-termination--user-initiated-logouts--message-displays"></a>Beendigung der Sitzung| Benutzerseitig eingeleitete Abmeldungen/Meldungsanzeigen

**AC-12 (1).b** Das Informationssystem zeigt Benutzern eine ausdrückliche Abmeldenachricht an, die die zuverlässige Beendigung authentifizierter Kommunikationssitzungen signalisiert.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das Azure-Portal und die VM-Betriebssysteme, die durch diesen Blueprint bereitgestellt werden, ermöglichen Benutzern das Einleiten einer Abmeldung. Beim Abmeldevorgang wird den Benutzern signalisiert, dass die Sitzung beendet wurde. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-14a"></a>NIST 800-53, Regelung AC-14.a

#### <a name="permitted-actions-without-identification-or-authentication"></a>Zulässige Aktionen ohne Identifizierung oder Authentifizierung

**AC-14.a** Die Organisation benennt [Festsetzung: von der Organisation definierte Benutzeraktionen], die auf dem Informationssystem ohne Identifizierung oder Authentifizierung entsprechend den Organisationszielen oder Geschäftsfunktionen durchgeführt werden können.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Benennung von Aktionen, die für die Kunden bereitgestellte Ressourcen ohne Identifizierung oder Authentifizierung ausgeführt werden können (z. B. das Anzeigen von einer öffentlich zugänglichen Webseite). |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-14b"></a>NIST 800-53, Regelung AC-14.b

#### <a name="permitted-actions-without-identification-or-authentication"></a>Zulässige Aktionen ohne Identifizierung oder Authentifizierung

**AC-14.b** Die Organisation dokumentiert und vermittelt im Sicherheitsplan für das Informationssystem unterstützende Gründe für Benutzeraktionen, die keine Identifizierung oder Authentifizierung erfordern.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für das Bereitstellen von Dokumentation für Benutzeraktionen, für die keine Identifizierung oder Authentifizierung auf kundenseitig bereitgestellten Ressourcen erforderlich ist. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-17a"></a>NIST 800-53, Regelung AC-17.a

#### <a name="remote-access"></a>Remotezugriff

**AC-17.a** Die Organisation definiert und dokumentiert Nutzungseinschränkungen, Konfigurations- und Verbindungsanforderungen und Implementierungsempfehlungen für alle Arten zulässigen Remotezugriffs.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinie des Kunden auf Unternehmensebene kann Beschränkungen für die Verwendung des Remotezugriffs definieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-17b"></a>NIST 800-53, Regelung AC-17.b

#### <a name="remote-access"></a>Remotezugriff

**AC-17.b** Die Organisation autorisiert den Remotezugriff auf das Informationssystem, bevor solche Verbindungen zugelassen werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die auf Unternehmensebene implementierten Verfahren des Kunden zur Zugriffssteuerung können einen Vorgang für die Remotezugriffsautorisierung vorsehen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-17-1"></a>NIST 800-53, Regelung AC-17 (1)

#### <a name="remote-access--automated-monitoring--control"></a>Remotezugriff | Automatisierte Überwachung/Steuerung

**AC-17 (1)** Das Informationssystem überwacht und steuert Remotezugriffsmethoden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint bietet Remotezugriff auf das Informationssystem über das Azure-Portal, über eine Remotedesktopverbindung per Jumpbox oder über eine kundenseitig implementierte Webanwendung. Zugriffe auf das Azure-Portal und Remotedesktopsitzungen werden überwacht und können über Log Analytics eingesehen werden. Der Kunde muss in der Webanwendung erforderlichenfalls Maßnahmen zur Remotezugriffssteuerung implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-17-2"></a>NIST 800-53, Regelung AC-17 (2)

#### <a name="remote-access--protection-of-confidentiality--integrity-using-encryption"></a>Remotezugriff | Schutz der Vertraulichkeit/Integrität mithilfe von Verschlüsselung

**AC-17 (2)** Das Informationssystem implementiert kryptografische Mechanismen, um die Vertraulichkeit und Integrität der Remotezugriffssitzungen zu schützen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Remotezugriff auf Ressourcen, die durch diesen Blueprint bereitgestellt werden (einschließlich Azure-Portal, Remotedesktopverbindung und Webanwendungsgateway), ist durch TLS geschützt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-17-3"></a>NIST 800-53, Regelung AC-17 (3)

#### <a name="remote-access--managed-access-control-points"></a>Remotezugriff | Verwaltete Zugriffssteuerungspunkte

**AC-17 (3)** Das Informationssystem routet alle Remotezugriffe über [Festsetzung: von der Organisation definierte Anzahl] verwaltete Netzwerk-Zugriffssteuerpunkte.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Remotezugriff auf die durch diesen Blueprint bereitgestellte fiktive Webanwendung erfolgt über ein Anwendungsgateway. Remotezugriffe auf alle anderen Ressourcen erfolgen über eine Jumpbox. Es sind keine weiteren öffentlich zugänglichen Endpunkte vorhanden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-17-4a"></a>NIST 800-53, Regelung AC-17 (4).a

#### <a name="remote-access--privileged-commands--access"></a>Remotezugriff | Privilegierte Befehle/Zugriff

**AC-17 (4).a** Die Organisation autorisiert die Ausführung privilegierter Befehle und den Remotezugriff auf sicherheitsrelevante Informationen nur für [Festsetzung: von der Organisation definierte Anforderungen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinie des Kunden auf Unternehmensebene kann privilegierte Befehle, auf die remote zugegriffen werden kann, definieren und dafür eine Begründung enthalten. Hinweis: Kunden haben keinen direkten Netzwerkzugriff auf die Azure-Infrastruktur. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-17-4b"></a>NIST 800-53, Regelung AC-17 (4).b

#### <a name="remote-access--privileged-commands--access"></a>Remotezugriff | Privilegierte Befehle/Zugriff

**AC-17 (4).b** Die Organisation dokumentiert die Gründe für diesen Zugriff im Sicherheitsplan für das Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinie des Kunden auf Unternehmensebene kann privilegierte Befehle, auf die remote zugegriffen werden kann, definieren und dafür eine Begründung enthalten. Hinweis: Kunden haben keinen direkten Netzwerkzugriff auf die Azure-Infrastruktur. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-17-9"></a>NIST 800-53, Regelung AC-17 (9)

#### <a name="remote-access--disconnect--disable-access"></a>Remotezugriff | Trennen/Deaktivieren des Zugriffs

**AC-17 (9)** Die Organisation bietet die Möglichkeit einer schnellen Trennung oder Deaktivierung des Remotezugriffs auf das Informationssystem innerhalb [Festsetzung: von der Organisation definierter Zeitraum].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint bietet Remotezugriff auf das Informationssystem über das Azure-Portal, über eine Remotedesktopverbindung per Jumpbox oder über eine Webanwendung. Wenn ein Azure Active Directory-Konto deaktiviert oder entfernt wird, wird der Zugriff auf das Azure-Portal sofort unterbunden. Ähnlich wird, wenn ein VM-Konto auf Betriebssystem ebene deaktiviert oder entfernt wird, der Remotedesktopzugriff über die Jumpbox sofort getrennt. Kunden müssen eine Trennung des Remotezugriffs für die Webanwendung implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-18a"></a>NIST 800-53, Regelung AC-18.a

#### <a name="wireless-access"></a>Drahtloser Zugriff

**AC-18.a** Die Organisation definiert Nutzungseinschränkungen, Konfigurations- und Verbindungsanforderungen und Implementierungsempfehlungen für den drahtlosen Zugriff.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gibt keinen drahtlosen Zugriff im Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert Nutzungsbeschränkungen, Anforderungen an Konfiguration und Verbindung und Implementierungsempfehlungen für den drahtlosen Zugriff über den Network Security Standard, der die Verwendung drahtloser Geräte und Technologien in der Microsoft Azure-Umgebung ausdrücklich untersagt. |


 ## <a name="nist-800-53-control-ac-18b"></a>NIST 800-53, Regelung AC-18.b

#### <a name="wireless-access"></a>Drahtloser Zugriff

**AC-18.b** Die Organisation autorisiert den drahtlosen Zugriff auf das Informationssystem, bevor solche Verbindungen zugelassen werden.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gibt keinen drahtlosen Zugriff im Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure gestattet keinen drahtlosen Zugriff innerhalb von Microsoft Azure-Rechenzentren. |


 ### <a name="nist-800-53-control-ac-18-1"></a>NIST 800-53, Regelung AC-18 (1)

#### <a name="wireless-access--authentication-and-encryption"></a>Drahtloser Zugriff | Authentifizierung und Verschlüsselung

**AC-18 (1)** Das Informationssystem schützt den drahtlosen Zugriff auf das System mithilfe der Authentifizierung von [Auswahl (mindestens eine): Benutzern, Geräten] und von Verschlüsselung.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gibt keinen drahtlosen Zugriff im Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure gestattet keinen drahtlosen Zugriff innerhalb der Microsoft Azure-Umgebung. |


 ### <a name="nist-800-53-control-ac-18-3"></a>NIST 800-53, Regelung AC-18 (3)

#### <a name="wireless-access--disable-wireless-networking"></a>Drahtloser Zugriff | Deaktivieren von Drahtlosnetzwerken

**AC-18 (3)** Die Organisation deaktiviert, wenn ein drahtloser Zugriff nicht vorgesehen ist, intern die in Komponenten des Informationssystems integrierten drahtlosen Netzwerkfunktionen vor deren Zulassung und Bereitstellung.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gibt keinen drahtlosen Zugriff im Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure gestattet keinen drahtlosen Zugriff innerhalb der Microsoft Azure-Umgebung. |


 ### <a name="nist-800-53-control-ac-18-4"></a>NIST 800-53, Regelung AC-18 (4)

#### <a name="wireless-access--restrict-configurations-by-users"></a>Drahtloser Zugriff | Beschränken von Konfigurationen durch Benutzer

**AC-18 (4)** Die Organisation benennt und autorisiert ausdrücklich solche Benutzer, die berechtigt sind, drahtlose Netzwerkfunktionen unabhängig zu konfigurieren.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gibt keinen drahtlosen Zugriff im Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure gestattet keinen drahtlosen Zugriff innerhalb der Microsoft Azure-Umgebung. |


 ### <a name="nist-800-53-control-ac-18-5"></a>NIST 800-53, Regelung AC-18 (5)

#### <a name="wireless-access--antennas--transmission-power-levels"></a>Drahtloser Zugriff | Antennen/Sendeleistungspegel

**AC-18 (5)** Die Organisation wählt Funkantennen aus und kalibriert die Übertragungsleistungspegel so, dass die Wahrscheinlichkeit eines Empfangs von Nutzsignalen außerhalb der Grenzen des von der Organisation kontrollierten Bereichs möglichst niedrig ist.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gibt keinen drahtlosen Zugriff im Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure gestattet keinen drahtlosen Zugriff innerhalb der Microsoft Azure-Umgebung. |


 ## <a name="nist-800-53-control-ac-19a"></a>NIST 800-53, Regelung AC-19.a

#### <a name="access-control-for-mobile-devices"></a>Zugriffssteuerung für mobile Geräte

**AC-19.a** Die Organisation definiert Nutzungseinschränkungen, Konfigurations- und Verbindungsanforderungen und Implementierungsempfehlungen für von der Organisation kontrollierte mobile Geräte.

**Zuständigkeiten:** `Not Applicable`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten mobilen Geräte zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure gestattet keine mobilen Geräte innerhalb der Grenzen von Azure. Aufgrund dessen ist diese Regelung auf Microsoft Azure nicht anwendbar. |


 ## <a name="nist-800-53-control-ac-19b"></a>NIST 800-53, Regelung AC-19.b

#### <a name="access-control-for-mobile-devices"></a>Zugriffssteuerung für mobile Geräte

**AC-19.b** Die Organisation autorisiert die Verbindung mobiler Geräte mit Informationssystemen der Organisation.

**Zuständigkeiten:** `Not Applicable`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten mobilen Geräte zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure gestattet keine mobilen Geräte innerhalb der Grenzen von Azure. Aufgrund dessen ist diese Regelung auf Microsoft Azure nicht anwendbar. |


 ### <a name="nist-800-53-control-ac-19-5"></a>NIST 800-53, Regelung AC-19 (5)

#### <a name="access-control-for-mobile-devices--full-device--container-based--encryption"></a>Zugriffssteuerung für mobile Geräte | Vollständige Geräte-/containerbasierte Verschlüsselung

**AC-19 (5)** Die Organisation verwendet die [Auswahl: Gerätevollverschlüsselung; Containerverschlüsselung] zum Schutz der Vertraulichkeit und Integrität von Daten auf [Festsetzung: von der Organisation definierten mobilen Geräten].

**Zuständigkeiten:** `Not Applicable`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten mobilen Geräte zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure gestattet keine mobilen Geräte innerhalb der Grenzen von Azure. Aufgrund dessen ist diese Regelung auf Microsoft Azure nicht anwendbar. |


 ## <a name="nist-800-53-control-ac-20a"></a>NIST 800-53, Regelung AC-20.a

#### <a name="use-of-external-information-systems"></a>Verwendung externer Informationssysteme

**AC-20.a** Die Organisation legt Geschäftsbedingungen fest, die konsistent zu mit anderen Organisationen, die externe Informationssysteme besitzen, betreiben und/oder pflegen, hergestellten Vertrauensbeziehungen sind, und ermöglicht so den Zugriff autorisierter Personen auf das Informationssystem über solche externe Informationssysteme.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinie des Kunden auf Unternehmensebene kann eine Vorschrift zur Verwendung von FedRAMP-konformen Clouddienstangeboten enthalten. Azure wurde vom Federal Risk and Authorization Management Program (FedRAMP) Joint Authorization Board (JAB) eine Provisional Authorities to Operate (P-ATO) gewährt, was den Erwerb und die Nutzung von Azure-Clouddiensten durch Regierungsstellen ermöglicht. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-20b"></a>NIST 800-53, Regelung AC-20.b

#### <a name="use-of-external-information-systems"></a>Verwendung externer Informationssysteme

**AC-20.b** Die Organisation legt Geschäftsbedingungen fest, die konsistent zu mit anderen Organisationen, die externe Informationssysteme besitzen, betreiben und/oder pflegen, hergestellten Vertrauensbeziehungen sind, und ermöglicht so Verarbeitung, Speicherung und Übertragung organisationsseitig gesteuerter Informationen mithilfe solcher externer Informationssysteme.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinie des Kunden auf Unternehmensebene kann eine Vorschrift zur Verwendung von FedRAMP-konformen Clouddienstangeboten enthalten. Azure wurde vom Federal Risk and Authorization Management Program (FedRAMP) Joint Authorization Board (JAB) eine Provisional Authorities to Operate (P-ATO) gewährt, was den Erwerb und die Nutzung von Azure-Clouddiensten durch Regierungsstellen ermöglicht. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-20-1"></a>NIST 800-53, Regelung AC-20 (1)

#### <a name="use-of-external-information-systems--limits-on-authorized-use"></a>Verwendung externer Informationssysteme | Beschränkungen der zulässigen Nutzung

**AC-20 (1)** Die Organisation gestattet autorisierten Personen die Verwendung eines externen Informationssystems für den Zugriff auf das Informationssystem und das Verarbeiten, Speichern und Übertragen von durch die Organisation gesteuerten Informationen erst, nachdem sie sich von der Implementierung der erforderlichen Sicherheitsmaßnahmen auf dem externen System entsprechend den Vorgaben der Informationssicherheitsrichtlinie und des Sicherheitsplans der Organisation überzeugt oder genehmigte Vereinbarungen zur Informationssystemverbindung oder -verarbeitung mit dem Rechtsträger derjenigen Organisation geschlossen hat, die das externe Informationssystem hostet.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Informationstechnikgruppe des Kunden auf Unternehmensebene ist berechtigt, die Einhaltung der Informationssicherheitsanforderungen der Organisation durch den Clouddienstanbieter zu überprüfen und eine unternehmensweite Genehmigung zur Verwendung zugehöriger Clouddienstangebote zu gewähren. Azure wurde vom Federal Risk and Authorization Management Program (FedRAMP) Joint Authorization Board (JAB) eine Provisional Authorities to Operate (P-ATO) gewährt. Azure wird durch eine durch das FedRAMP autorisierte externe Bewertungsorganisation (Third-Party Assessment Organisation, 3PAO) bewertet, um die Einhaltung der Anforderungen an Sicherheitsmaßnahmen sowie weiterer Anforderungen nachzuweisen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ac-20-2"></a>NIST 800-53, Regelung AC-20 (2)

#### <a name="use-of-external-information-systems--portable-storage-devices"></a>Verwendung externer Informationssysteme | Tragbare Speichergeräte

**AC-20 (2)** Die Organisation [Auswahl: beschränkt; unterbindet] die Verwendung von durch die Organisation gesteuerten Wechseldatenträgern durch autorisierte Personen auf externen Informationssystemen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft gestattet keine kundenseitig gesteuerten tragbaren Speichergeräte innerhalb der Microsoft Azure-Umgebung. |


 ## <a name="nist-800-53-control-ac-21a"></a>NIST 800-53, Regelung AC-21.a

#### <a name="information-sharing"></a>Gemeinsame Nutzung von Informationen

**AC-21.a** Die Organisation ermöglicht eine Informationsfreigabe, indem es autorisierten Benutzer gestattet wird zu bestimmen, ob dem freigebenden Partner gewährte Zugriffsautorisierungen den für die betreffenden Informationen geltenden Zugriffsbeschränkungen für [Festsetzung: von der Organisation definierte Umstände für eine Informationsfreigabe, unter denen benutzerseitige Diskretion erforderlich ist] entsprechen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Zugriffssteuerungsrichtlinie des Kunden auf Unternehmensebene kann Vorschriften zur Informationsfreigabe enthalten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-21b"></a>NIST 800-53, Regelung AC-21.b

#### <a name="information-sharing"></a>Gemeinsame Nutzung von Informationen

**AC-21.b** Die Organisation verwendet [Festsetzung: von der Organisation definierte automatisierte Mechanismen oder manuelle Prozesse] zur Unterstützung der Benutzer bei Entscheidungen zur Informationsfreigabe/Zusammenarbeit.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist berechtigt, eine Entscheidungsunterstützungsfunktion mit Informationsfreigabe auf Unternehmensebene einzusetzen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-22a"></a>NIST 800-53, Regelung AC-22.a

#### <a name="publicly-accessible-content"></a>Öffentlich zugängliche Inhalte

**AC-22.a** Die Organisation benennt Personen, die berechtigt sind, Informationen auf einem öffentlich zugänglichen Informationssystem einzustellen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die auf Unternehmensebene implementierten Verfahren des Kunden zur Zugriffssteuerung können Personen angeben, die zum Einstellen öffentlich zugänglicher Informationen berechtigt sind. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-22b"></a>NIST 800-53, Regelung AC-22.b

#### <a name="publicly-accessible-content"></a>Öffentlich zugängliche Inhalte

**AC-22.b** Die Organisation schult autorisierte Personen, um sicherzustellen, dass öffentlich zugängliche Informationen keine nicht für die Öffentlichkeit bestimmten Informationen enthalten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist berechtigt, für Personen, die zum Einstellen öffentlich zugänglicher Informationen berechtigt sind, Schulungen auf Unternehmensebene durchzuführen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-22c"></a>NIST 800-53, Regelung AC-22.c

#### <a name="publicly-accessible-content"></a>Öffentlich zugängliche Inhalte

**AC-22.c** Die Organisation prüft vorgeschlagene Inhalte mit Informationen vor der Bereitstellung auf dem öffentlich zugänglichen Informationssystem, um sicherzustellen, dass keine nicht für die Öffentlichkeit bestimmten Informationen enthalten sind.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die auf Unternehmensebene implementierten Verfahren des Kunden zur Zugriffssteuerung können einen Prüfvorgang für Inhalte vorsehen, die für die Einstellung auf öffentlich zugänglichen Systemen vorgeschlagen wurden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ac-22d"></a>NIST 800-53, Regelung AC-22.d

#### <a name="publicly-accessible-content"></a>Öffentlich zugängliche Inhalte

**AC-22.d** Die Organisation überprüft Inhalte auf dem öffentlich zugänglichen Informationssystem [Festsetzung: von der Organisation definierte Häufigkeit] auf nicht für die Öffentlichkeit bestimmte Informationen und entfernt ggf. solche Informationen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die auf Unternehmensebene implementierten Verfahren des Kunden zur Zugriffssteuerung können einen Vorgang zur regelmäßigen Überprüfung von Inhalten vorsehen, die auf öffentlich zugänglichen Systemen eingestellt wurden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |

