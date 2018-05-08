---
title: Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP – Planung
description: Automatisierung von Webanwendungen für FedRAMP – Planung
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 55032e87-763a-452d-bb22-9c28936d5bb4
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: d5e5666db71ac9f5a136c9acb448fb13bf39a853
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
---
# <a name="planning-pl"></a>Planung (PL)

> [!NOTE]
> Diese Regulierungen werden von NIST und dem U.S. Department of Commerce als Teil der NIST-Sonderveröffentlichung 800-53 Revision 4 definiert. Informationen zu Testverfahren und Anleitungen für jede Regelung finden Sie unter NIST 800-53 Rev. 4.

## <a name="nist-800-53-control-pl-1"></a>NIST 800-53, Regulierung PL-1

#### <a name="security-planning-policy-and-procedures"></a>Sicherheitsplanungsrichtlinie und Verfahren

**PS-1** Die Organisation entwickelt, dokumentiert und verbreitet an [Zuordnung: von der Organisation definiertes Personal oder Rollen] eine Sicherheitsplanungsrichtlinie, die sich mit Zweck, Umfang, Rollen, Zuständigkeiten, Managementverpflichtung, Koordination zwischen Organisationsentitäten und Konformität befasst, und stellt Verfahren zum Ermöglichen der Implementierung der Sicherheitsplanungsrichtlinie und der damit verbundenen Sicherheitsplanungsregulierungen bereit und überprüft und aktualisiert die aktuelle Sicherheitsplanungsrichtlinie [Zuordnung: von der Organisation definierte Häufigkeit] und die Sicherheitsplanungsverfahren [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Sicherheitsplanungsrichtlinien und -verfahren des Kunden auf Unternehmensebene können ausreichen, um dieser Regulierung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-2a"></a>NIST 800-53, Regulierung PL-2.a

#### <a name="system-security-plan"></a>Systemsicherheitsplan

**PL-2.a** Die Organisation entwickelt einen Sicherheitsplan für das Informationssystem, der mit der Unternehmensarchitektur der Organisation konsistent ist, definiert explizit die Autorisierungsgrenze für das System, beschreibt den betrieblichen Kontext des Informationssystems in Bezug auf Missionen und Geschäftsprozesse, stellt die Sicherheitskategorisierung des Informationssystems einschließlich unterstützender Begründung zur Verfügung, beschreibt die Betriebsumgebung für das Informationssystem und die Beziehungen zu oder Verbindungen mit anderen Informationssystemen, gibt einen Überblick über die Sicherheitsanforderungen für das System, identifiziert alle relevanten Überlagerungen (falls zutreffend), beschreibt die Sicherheitsregulierungen, die zur Erfüllung dieser Anforderungen vorhanden oder geplant sind (einschließlich einer Begründung für die gezielten Entscheidungen) und wird vor der Implementierung des Plans von der bevollmächtigenden Amtsperson oder dem designierten Vertreter überprüft und genehmigt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, einen Systemsicherheitsplan (SSP) zu entwickeln, der die Kriterien der Zielautorisierung (z.B. FedRAMP) erfüllt. Kunden können sich auf die NIST-Sonderpublikation 800-18 R1 (Guide for Developing Security Plans for Federal Information Systems, Leitfaden für die Entwicklung von Sicherheitsplänen für Informationssysteme auf Bundesebene) beziehen. Der Kunden-SSP sollte Regulierungen berücksichtigen, die von Microsoft Azure geerbt wurden, und sich bezüglich der Details zur Implementierung am Microsoft Azure-SSP orientieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-2b"></a>NIST 800-53, Regulierung PL-2.b

#### <a name="system-security-plan"></a>Systemsicherheitsplan

**PL-2.b** Die Organisation verteilt Kopien des Sicherheitsplans und teilt nachträgliche Änderungen des Plans [Zuordnung: von der Organisation definiertes Personal oder Rollen] mit.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verteilung des Systemsicherheitsplans verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-2c"></a>NIST 800-53, Regulierung PL-2.c

#### <a name="system-security-plan"></a>Systemsicherheitsplan

**PL-2.c** Die Organisation überprüft den Sicherheitsplan für das Informationssystem [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überprüfung des Systemsicherheitsplans verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-2d"></a>NIST 800-53, Regulierung PL-2.d

#### <a name="system-security-plan"></a>Systemsicherheitsplan

**PL-2.d** Die Organisation aktualisiert den Plan, um Änderungen des Informationssystems/der Betriebsumgebung oder Probleme, die bei der Implementierung des Plans oder bei der Beurteilung der Sicherheitsregulierung festgestellt wurden, zu berücksichtigen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Aktualisierung des Systemsicherheitsplans verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-2e"></a>NIST 800-53, Regulierung PL-2.e

#### <a name="system-security-plan"></a>Systemsicherheitsplan

**PL-2.e** Die Organisation schützt den Sicherheitsplan vor nicht autorisierter Offenlegung und Änderung.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Schutz des Systemsicherheitsplans verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-pl-2-3"></a>NIST 800-53, Regulierung PL-2 (3)

#### <a name="system-security-plan--plan--coordinate-with-other-organizational-entities"></a>Systemsicherheitsplan | Planen/Koordieren mit anderen Organisationsentitäten

**PL-2 (3)** Die Organisation plant und koordiniert sicherheitsrelevante Aktivitäten, die das Informationssystem betreffen, vor der Durchführung solcher Aktivitäten mit [Zuordnung: von der Organisation definierte Einzelpersonen oder Gruppen] mit dem Ziel, die Auswirkungen auf andere Organisationsentitäten zu verringern.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Planung und Koordination sicherheitsbezogener Aktivitäten, um die Auswirkungen auf andere Organisationsentitäten zu verringern. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-4a"></a>NIST 800-53, Regulierung PL-4.a

#### <a name="rules-of-behavior"></a>Verhaltensregeln

**PL-4.a** Die Organisation erstellt Regeln und stellt diese Personen zur Verfügung, die Zugang zum Informationssystem benötigen, die ihre Verantwortlichkeiten und das erwartete Verhalten in Bezug auf die Nutzung von Informationen und Informationssystemen beschreiben.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Verhaltensregeln des Kunden auf Unternehmensebene können ausreichen, um dieser Regulierung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-4b"></a>NIST 800-53, Regulierung PL-4.b

#### <a name="rules-of-behavior"></a>Verhaltensregeln

**PL-4.b** Die Organisation erhält von solchen Personen eine unterschriebene Bestätigung, dass sie die Verhaltensregeln gelesen, verstanden und ihnen zugestimmt haben, bevor sie den Zugriff auf Informationen und das Informationssystem autorisiert.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Verhaltensregeln des Kunden auf Unternehmensebene können ausreichen, um dieser Regulierung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-4c"></a>NIST 800-53, Regulierung PL-4.c

#### <a name="rules-of-behavior"></a>Verhaltensregeln

**PS-4.c** Die Organisation überprüft und aktualisiert die Verhaltensregeln [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Verhaltensregeln des Kunden auf Unternehmensebene können ausreichen, um dieser Regulierung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-4d"></a>NIST 800-53, Regulierung PL-4.d

#### <a name="rules-of-behavior"></a>Verhaltensregeln

**PL-4.d** Die Organisation verlangt von Personen, die eine frühere Version der Verhaltensregeln unterzeichnet haben, dass sie diese lesen und erneut unterzeichnen, wenn die Verhaltensregeln überarbeitet/aktualisiert werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Verhaltensregeln des Kunden auf Unternehmensebene können ausreichen, um dieser Regulierung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-pl-4-1"></a>NIST 800-53, Regulierung PL-4 (1)

#### <a name="rules-of-behavior--social-media-and-networking-restrictions"></a>Verhaltensregeln | Einschränkungen für soziale Medien und Networking

**PL-4 (1)** Die Organisation schließt in den Verhaltensregeln explizite Einschränkungen für die Nutzung von sozialen Medien und Websites für Networking sowie die Veröffentlichung von Organisationsinformationen auf öffentlichen Webseiten ein.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Verhaltensregeln des Kunden auf Unternehmensebene können Einschränkungen für soziale Medien und Websites für Networking enthalten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-8a"></a>NIST 800-53, Regulierung PL-8.a

#### <a name="information-security-architecture"></a>Informationssicherheitsarchitektur

**PL-8.a** Die Organisation entwickelt eine Informationssicherheitsarchitektur für das Informationssystem, die die allgemeine Philosophie, die Anforderungen und den Ansatz beschreibt, die hinsichtlich des Schutzes der Vertraulichkeit, Integrität und Verfügbarkeit von Organisationsinformationen zu verfolgen sind, sie beschreibt, wie die Informationssicherheitsarchitektur in die Unternehmensarchitektur integriert ist und diese unterstützt, und sie beschreibt alle Annahmen zur Informationssicherheit externer Dienste sowie Abhängigkeiten von dieser.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Entwicklung einer Informationssicherheitsarchitektur für vom Kunden bereitgestellte Ressourcen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-8b"></a>NIST 800-53, Regulierung PL-8.b

#### <a name="information-security-architecture"></a>Informationssicherheitsarchitektur

**PL-8.b** Die Organisation überprüft und aktualisiert die Informationssicherheitsarchitektur [Zuordnung: von der Organisation definierte Häufigkeit], um Aktualisierungen in der Unternehmensarchitektur widerzuspiegeln.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für das Überprüfen und Aktualisieren der Informationssicherheitsarchitektur. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pl-8c"></a>NIST 800-53, Regulierung PL-8.c

#### <a name="information-security-architecture"></a>Informationssicherheitsarchitektur

**PL-8.c** Die Organisation stellt sicher, dass geplante Änderungen der Informationssicherheitsarchitektur im Sicherheitsplan, im Betriebssicherheitskonzept (CONOPS) und in den Organisationsbeschaffungen/-akquisitionen berücksichtigt werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Berücksichtigung geplanter Änderungen an der Informationssicherheitsarchitektur verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |
