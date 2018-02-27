---
title: "Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP – Sicherheitsbewertung und -autorisierung"
description: "Automatisierung von Webanwendungen für FedRAMP – Sicherheitsbewertung und -autorisierung"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 47c5914d-0d76-498a-9298-b3d9040791f8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: d1c9846589adaef0baeeaf16077c8e74bef1cd9f
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
---
# <a name="security-assessment-and-authorization-ca"></a>Sicherheitsbewertung und -autorisierung (CA)

> [!NOTE]
> Diese Regelungen werden von NIST und dem U.S. Department of Commerce als Teil der NIST-Sonderveröffentlichung 800-53 Revision 4 definiert. Informationen zu Testverfahren und Anleitungen für jede Regelung finden Sie unter NIST 800-53 Rev. 4.

## <a name="nist-800-53-control-ca-1"></a>NIST 800-53, Regelung CA-1

#### <a name="security-assessment-and-authorization-policy-and-procedures"></a>Sicherheitsbewertungs- und -autorisierungsrichtlinie und -verfahren

**CA-1** Die Organisation entwickelt, dokumentiert und verbreitet an [Zuordnung: von der Organisation definiertes Personal oder Rollen] eine Sicherheitsbewertungs- und -autorisierungsrichtlinie, die sich mit Zweck, Umfang, Rollen, Zuständigkeiten, Managementverpflichtung, Koordination zwischen Organisationsentitäten und Konformität befasst, und stellt Verfahren zum Ermöglichen der Implementierung der Sicherheitsbewertungs- und -autorisierungsrichtlinie und der damit verbundenen Sicherheitsbewertungs- und -autorisierungsregelungen bereit und überprüft und aktualisiert die aktuelle Sicherheitsbewertungs- und -autorisierungsrichtlinie [Festlegung: von der Organisation definierte Häufigkeit] und die Sicherheitsbewertungs- und -autorisierungverfahren [Festlegung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Sicherheitsbewertungs- und -autorisierungsrichtlinien und -verfahren des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-2a"></a>NIST 800-53, Regelung CA-2.a

#### <a name="security-assessments"></a>Sicherheitsbewertungen

**CA-2.a** Die Organisation entwickelt einen Sicherheitsbewertungsplan, der den Gültigkeitsbereich der Bewertung einschließlich Sicherheitsmaßnahmen und Verbesserungen der geprüften Maßnahmen, zur Bestimmung der Effektivität von Sicherheitsmaßnahmen zu verwendende Bewertungsverfahren und die Bewertungsumgebung, das Gutachterteam und die Bewertungsrollen und -zuständigkeiten beschreibt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Entwicklung eines Sicherheitsbewertungsplans für das kundenseitig bereitgestellte System. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-2b"></a>NIST 800-53, Regelung CA-2.b

#### <a name="security-assessments"></a>Sicherheitsbewertungen

**CA-2.b** Die Organisation bewertet die Sicherheitsmaßnahmen im Informationssystem und in dessen Betriebsumgebung [Festsetzung: von der Organisation definierte Häufigkeit], um zu ermitteln, in welchem Umfang die Maßnahmen korrekt implementiert wurden, wie vorgesehen funktionieren und das bezogen auf die Erfüllung der vorgesehenen Sicherheitsanforderungen gewünschte Ergebnis erzeugen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Bewertung der in CA-02.a definierten Sicherheitsmaßnahmen für vom Kunden bereitgestellte Ressourcen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-2c"></a>NIST 800-53, Regelung CA-2.c

#### <a name="security-assessments"></a>Sicherheitsbewertungen

**CA-2.c** Die Organisation erzeugt einen Bericht zur Sicherheitsbewertung, der die Ergebnisse der Bewertung dokumentiert .

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Erstellung eines Sicherheitsbewertungsberichts. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-2d"></a>NIST 800-53, Regelung CA-2.d

#### <a name="security-assessments"></a>Sicherheitsbewertungen

**CA-2.d** Die Organisation stellt [Festsetzung: von der Organisation definierten Personen oder Rollen] die Ergebnisse der Sicherheitsmaßnahmenbewertung zur Verfügung.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Übermittlung der Sicherheitsbewertungsergebnisse an die erforderlichen Personen/Rollen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ca-2-1"></a>NIST 800-53, Regelung CA-2 (1)

#### <a name="security-assessments--independent-assessors"></a>Sicherheitsbewertungen | Unabhängige Gutachter

**CA-2 (1)** Die Organisation beschäftigt Gutachter oder Gutachterteams mit [Festsetzung: von der Organisation definiertem Unabhängigkeitsgrad] zur Durchführung von Bewertungen der Sicherheitsmaßnahmen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für den Einsatz unabhängiger Gutachter oder Gutachterteams zur Durchführung der Bewertung von Sicherheitsmaßnahmen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ca-2-2"></a>NIST 800-53, Regelung CA-2 (2)

#### <a name="security-assessments--specialized-assessments"></a>Sicherheitsbewertungen | Spezialisierte Bewertungen

**CA-2 (2)** Die Organisation führt im Zuge der Bewertungen von Sicherheitsmaßnahmen [Festsetzung: von der Organisation definierte Häufigkeit] [Auswahl: angekündigt; unangekündigt], [Auswahl (mindestens eine): ausführliche Überwachung; Sicherheitsrisikoüberprüfungen; Prüfung auf böswillige Benutzer; Bewertung interner Bedrohungen; Leistungs-/Auslastungstests; [Festsetzung: sonstige von der Organisation definierte Formen der Sicherheitsbewertung]] durch.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Auswahl zusätzlicher Tests, die in die Bewertung der Sicherheitsmaßnahmen eingebunden werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ca-2-3"></a>NIST 800-53, Regelung CA-2 (3)

#### <a name="security-assessments--external-organizations"></a>Sicherheitsbewertungen | Externe Organisationen

**CA-2 (3)** Die Organisation akzeptiert die Ergebnisse der Bewertung des [Festsetzung: von der Organisation definierten Informationssystems], die von [Festsetzung: von der Organisation definierte externe Organisation] durchgeführt wurde, sobald die Bewertung folgende Anforderungen erfüllt: [Festsetzung: von der Organisation definierte Anforderungen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Sicherheitsbewertungs- und -autorisierungsverfahren des Kunden auf Unternehmensebene können die Akzeptanz von mit Clouddienstangeboten (wie Azure) erstellten Bewertungsergebnissen zum Gegenstand haben, die von einer externen Organisation (z. B. einer externen Bewertungsorganisation (Third-Party Assessment Organisation, 3PAO) oder eine anderen Agentur FedRAMP-konform durchgeführt wurde. Azure wird durch eine durch das FedRAMP autorisierte externe Bewertungsorganisation (Third-Party Assessment Organisation, 3PAO) bewertet, um die Einhaltung der Anforderungen an Sicherheitsmaßnahmen sowie weiterer Anforderungen nachzuweisen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-3a"></a>NIST 800-53, Regelung CA-3.a

#### <a name="system-interconnections"></a>Systemverbindungen

**CA-3.a** Die Organisation autorisiert Verbindungen aus dem Informationssystem mit anderen Informationssystemen mithilfe von Interconnection Security Agreements.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Sicherheitsbewertungs- und -autorisierungsverfahren des Kunden auf Unternehmensebene können sich auf die Autorisierung von Systemverbindungen erstrecken. Hinweis: FedRAMP verlangt keine ISAs zwischen einem CSP und einer US-amerikanischen Bundesagentur. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-3b"></a>NIST 800-53, Regelung CA-3.b

#### <a name="system-interconnections"></a>Systemverbindungen

**CA-3.b** Die Organisation dokumentiert für jede Verbindung die Schnittstelleneigenschaften, die Sicherheitsanforderungen und die Art der übermittelten Informationen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Sicherheitsbewertungs- und -autorisierungsverfahren des Kunden auf Unternehmensebene können Anforderungen für die Herstellung von Systemverbindungen definieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-3c"></a>NIST 800-53, Regelung CA-3.c

#### <a name="system-interconnections"></a>Systemverbindungen

**PS-3.c** Die Organisation überprüft und aktualisiert die Interconnection Security Agreements [Festsetzung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Sicherheitsbewertungs- und -autorisierungsverfahren des Kunden auf Unternehmensebene können ISA-Prüf- und -Aktualisierungsprozesse definieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ca-3-3"></a>NIST 800-53, Regelung CA-3 (3)

#### <a name="system-interconnections--unclassified-non-national-security-system-connections"></a>Systemverbindungen | Nicht geheime, nicht für die nationale Sicherheit relevante Systemverbindungen

**CA-3 (3)** Die Organisation untersagt die direkte Verbindung eines [Festsetzung: von der Organisation definierten nicht geheimen und für die nationale Sicherheit nicht relevanten Systems] mit einem externen Netzwerk ohne Verwendung von [Festsetzung; von der Organisation definiertes Begrenzungsschutzgerät].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Sicherheitsbewertungs- und -autorisierungsverfahren des Kunden auf Unternehmensebene können Begrenzungsschutzanforderungen für Systemverbindungen definieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ca-3-5"></a>NIST 800-53, Regelung CA-3 (5)

#### <a name="system-interconnections--restrictions-on-external-system-connections"></a>Systemverbindungen | Einschränkungen für Verbindungen mit externen System

**CA-3 (5)** Die Organisation gestattet das Herstellen einer Verbindung von [Festsetzung: von der Organisation definierten Informationssystemen] mit externen Informationssystemen nach dem Prinzip „[Auswahl: Alles erlauben; Durch Ausnahmen verweigern; Alles verweigern; Durch Ausnahme erlauben]“.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Azure Application Gateway und Netzwerksicherheitsgruppen werden bereitgestellt, um die externe Konnektivität auf Ressourcen zu beschränken, die durch diesen Blueprint bereitgestellt werden. Auf Netzwerksicherheitsgruppen angewendete Regelsätze werden mit einem Schema konfiguriert, das den Zugriff standardmäßig verweigert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-5a"></a>NIST 800-53, Regelung CA-5.a

#### <a name="plan-of-action-and-milestones"></a>Aktions- und Meilensteinplan

**CA-5.a** Die Organisation entwickelt einen Aktions- und Meilensteinplan für das Informationssystem, um die geplanten Abhilfemaßnahmen der Organisation zur Beseitigung von Schwachstellen oder Mängeln zu dokumentieren, die bei der Bewertung von Sicherheitsmaßnahmen erkannt wurden, und bekannte Sicherheitsrisiken im System zu verringern oder ganz zu beseitigen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Entwicklung eines Aktions- und Meilensteinplans (POA&M) für die vom Kunden bereitgestellten Ressourcen (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) . |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-5b"></a>NIST 800-53, Regelung CA-5.b

#### <a name="plan-of-action-and-milestones"></a>Aktions- und Meilensteinplan

**CA-5.b** Die Organisation aktualisiert den bestehenden Aktions- und Meilensteinplan [Festsetzung: von der Organisation definierte Häufigkeit] auf der Grundlage der Ergebnisse von Sicherheitsmaßnahmenbewertungen, Sicherheitswirkungsanalyse und kontinuierlichen Überwachungsaktivitäten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Aktualisierung der POA&M-Elemente gemäß Definition in CA-05.a. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-6a"></a>NIST 800-53, Regelung CA-6.a

#### <a name="security-authorization"></a>Sicherheitsautorisierung

**CA-6.a** Die Organisation benennt eine Führungskraft oder einen Manager als Autorisierungsbefugten für das Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist berechtigt, einen Prozess auf Unternehmensebene zu implementieren, bei dem Autorisierungsbefugte benannt werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-6b"></a>NIST 800-53, Regelung CA-6.b

#### <a name="security-authorization"></a>Sicherheitsautorisierung

**CA-6.b** Die Organisation stellt sicher, dass der Autorisierungsbefugte das Informationssystem zur Verarbeitung autorisiert, bevor der Betrieb begonnen wird.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist berechtigt, einen Systemautorisierungsprozess auf Unternehmensebene zu implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-6c"></a>NIST 800-53, Regelung CA-6.c

#### <a name="security-authorization"></a>Sicherheitsautorisierung

**CA-6.c** Die Organisation aktualisiert Sicherheitsautorisierung [Festsetzung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist berechtigt, einen Systemautorisierungsprozess auf Unternehmensebene zu implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-7a"></a>NIST 800-53, Regelung CA-7.a

#### <a name="continuous-monitoring"></a>Kontinuierliche Überwachung

**CA-7.a** Die Organisation entwickelt eine Strategie für die kontinuierliche Überwachung und implementiert ein kontinuierliches Überwachungsprogramm, das die Festlegung zu überwachender [Festsetzung: von der Organisation definierte Metriken] vorsieht.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das kontinuierliche Überwachungsprogramm des Kunden auf Unternehmensebene kann ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-7b"></a>NIST 800-53, Regelung CA-7.b

#### <a name="continuous-monitoring"></a>Kontinuierliche Überwachung

**CA-7.b** Die Organisation entwickelt eine Strategie für die kontinuierliche Überwachung und implementiert ein kontinuierliches Überwachungsprogramm, das [Festsetzung: von der Organisation definierte Häufigkeiten] für die Überwachung und [Festsetzung: von der Organisation definierte Häufigkeiten] für die diese Überwachung unterstützende Bewertung spezifiziert.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das kontinuierliche Überwachungsprogramm des Kunden auf Unternehmensebene kann ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-7c"></a>NIST 800-53, Regelung CA-7.c

#### <a name="continuous-monitoring"></a>Kontinuierliche Überwachung

**CA-7.c** Die Organisation entwickelt eine Strategie für die kontinuierliche Überwachung und implementiert ein kontinuierliches Überwachungsprogramm, das fortlaufend Bewertungen der Sicherheitsmaßnahmen entsprechend der Organisationsstrategie für die kontinuierliche Überwachung vornimmt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das kontinuierliche Überwachungsprogramm des Kunden auf Unternehmensebene kann ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-7d"></a>NIST 800-53, Regelung CA-7.d

#### <a name="continuous-monitoring"></a>Kontinuierliche Überwachung

**CA-7.d** Die Organisation entwickelt eine Strategie für die kontinuierliche Überwachung und implementiert ein kontinuierliches Überwachungsprogramm, das eine fortlaufende Überwachung des Sicherheitsstatus organisationsseitig definierter Metriken entsprechend der Organisationsstrategie für die kontinuierliche Überwachung vornimmt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das kontinuierliche Überwachungsprogramm des Kunden auf Unternehmensebene kann ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-7e"></a>NIST 800-53, Regelung CA-7.e

#### <a name="continuous-monitoring"></a>Kontinuierliche Überwachung

**CA-7.e** Die Organisation entwickelt eine Strategie für die kontinuierliche Überwachung und implementiert ein kontinuierliches Überwachungsprogramm, das Korrelation und Analyse von sicherheitsrelevanten Daten umfasst, die von Beurteilungen und Überwachung erzeugt wurden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das kontinuierliche Überwachungsprogramm des Kunden auf Unternehmensebene kann ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-7f"></a>NIST 800-53, Regelung CA-7.f

#### <a name="continuous-monitoring"></a>Kontinuierliche Überwachung

**CA-7.f** Die Organisation entwickelt eine Strategie für die kontinuierliche Überwachung und implementiert ein kontinuierliches Überwachungsprogramm, das Reaktionsmaßnahmen umfasst, die die Ergebnisse der Analyse sicherheitsrelevanter Daten berücksichtigen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das kontinuierliche Überwachungsprogramm des Kunden auf Unternehmensebene kann ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-7g"></a>NIST 800-53, Regelung CA-7.g

#### <a name="continuous-monitoring"></a>Kontinuierliche Überwachung

**CA-7.g** Die Organisation entwickelt eine Strategie für die kontinuierliche Überwachung und implementiert ein kontinuierliches Überwachungsprogramm, das den Sicherheitsstatus von Organisation und Informationssystem [Festsetzung: von der Organisation definierte Häufigkeit] an [Festsetzung: von der Organisation definierte Mitarbeiter oder Rollen] meldet.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das kontinuierliche Überwachungsprogramm des Kunden auf Unternehmensebene kann ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ca-7-1"></a>NIST 800-53, Regelung CA-7 (1)

#### <a name="continuous-monitoring--independent-assessment"></a>Kontinuierliche Überwachung | Unabhängige Bewertung

**CA-7 (1)** Die Organisation nimmt Gutachter oder Gutachterteams mit [Festsetzung: von der Organisation definiertem Unabhängigkeitsgrad] zur laufenden Überwachung der Sicherheitsmaßnahmen im Informationssystem in Anspruch.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das kontinuierliche Überwachungsprogramm des Kunden auf Unternehmensebene kann einen Prozess zur unabhängigen Bewertung von Sicherheitsmaßnahmen vorsehen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ca-7-3"></a>NIST 800-53, Regelung CA-7 (3)

#### <a name="continuous-monitoring--trend-analyses"></a>Kontinuierliche Überwachung | Trendanalysen

**CA-7 (3)** Die Organisation ermittelt anhand von Trendanalysen, ob die Implementierung von Sicherheitsmaßnahmen, die Häufigkeit der kontinuierlichen Überwachungsaktivitäten und/oder die Arten von Aktivitäten, die bei der kontinuierlichen Überwachung verwendet werden, auf der Grundlage empirischer Daten geändert werden müssen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das kontinuierliche Überwachungsprogramm des Kunden auf Unternehmensebene kann einen Prozess für Trendanalysen vorsehen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-8"></a>NIST 800-53, Regelung CA-8

#### <a name="penetration-testing"></a>Penetrationstests

**CA-8** Die Organisation führt [Festsetzung: von der Organisation definierte Häufigkeit] Penetrationstests an [Festsetzung: von der Organisation definierten Informationssystemen oder Systemkomponenten] durch.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist berechtigt, eine Penetrationtestbewertung auf Unternehmensebene durchführen zu lassen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ca-8-1"></a>NIST 800-53, Regelung CA-8 (1)

#### <a name="penetration-testing--independent-penetration-agent-or-team"></a>Penetrationstests | Unabhängiger Dienstleister oder Team für Penetrationstests

**CA-8 (1)** Die Organisation nimmt einen unabhängigen Dienstleister oder ein Team für die Durchführung von Penetrationstests des Informationssystems oder der Systemkomponenten in Anspruch.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist berechtigt, eine Penetrationstestbewertung auf Unternehmensebene von einem unabhängigen Prüfer oder einem Team durchführen zu lassen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-9a"></a>NIST 800-53, Regelung CA-9.a

#### <a name="internal-system-connections"></a>Interne Systemverbindungen

**CA-9.a** Die Organisation autorisiert interne Verbindungen von [Festsetzung: von der Organisation definierte Informationssystemkomponenten oder -komponentenklassen] mit dem Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Autorisierung interner Verbindungen zwischen kundenseitig bereitgestellten Ressourcen (z.B. Systemverbindungen mit virtuellen Computern). |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ca-9b"></a>NIST 800-53, Regelung CA-9.b

#### <a name="internal-system-connections"></a>Interne Systemverbindungen

**CA-9.b** Die Organisation dokumentiert für jede interne Verbindung die Schnittstellenmerkmale, Sicherheitsanforderungen und die Art der übermittelten Informationen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Dokumentation der Details aller internen Verbindungen zwischen den in CA-09.a definierten Klassen/Ressourcen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |
