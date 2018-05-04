---
title: Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP – Konfigurationsverwaltung
description: Automatisierung von Webanwendungen für FedRAMP – Konfigurationsverwaltung
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 5b953c0d-236f-4b61-b2c5-df2199490c73
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 356eccac6af2780c02d1cd935d41891b5f89f1a2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="configuration-management-cm"></a>Konfigurationsverwaltung (Configuration Management, CM)

> [!NOTE]
> Diese Regelungen werden von NIST und dem U.S. Department of Commerce als Teil der NIST-Sonderveröffentlichung 800-53 Revision 4 definiert. Informationen zu Testverfahren und Anleitungen für jede Regelung finden Sie unter NIST 800-53 Rev. 4.

## <a name="nist-800-53-control-cm-1"></a>NIST 800-53, Regelung CM-1

#### <a name="configuration-management-policy-and-procedures"></a>Konfigurationsverwaltung – Richtlinien und Verfahren

**CM-1** Die Organisation entwickelt, dokumentiert und verbreitet an [Festsetzung: von der Organisation definiertes Personal oder Rollen] eine Konfigurationsverwaltungsrichtlinie, die sich mit Zweck, Umfang, Rollen, Zuständigkeiten, Verwaltungspflichten, Koordination zwischen Organisationsentitäten und Konformität befasst, Verfahren zum Ermöglichen der Implementierung der Konfigurationsverwaltungsrichtlinie und der damit verbundenen Konfigurationsverwaltungsmaßnahmen bereitstellt und die aktuelle Konfigurationsverwaltungsrichtlinie [Festsetzung: von der Organisation definierte Häufigkeit] und die Konfigurationsverwaltungsverfahren [Festsetzung: von der Organisation definierte Häufigkeit] überprüft und aktualisiert.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Konfigurationsverwaltungsrichtlinien und -verfahren des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-2"></a>NIST 800-53, Regelung CM-2

#### <a name="baseline-configuration"></a>Baselinekonfiguration

**CM-2** Die Organisation entwickelt, dokumentiert und verwaltet mit der Konfigurationssteuerung eine aktuelle Baselinekonfiguration des Informationssystems.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Azure Resource Manager-Vorlagen und begleitenden Ressourcen, aus denen dieser Blueprint besteht, stellen die Baseline einer „Konfiguration als Code“ für die bereitgestellte Architektur dar. Die Lösung wird über GitHub zur Verfügung gestellt, das auch für die Konfigurationssteuerung verwendet werden kann. Im Leistungsumfang der Lösung ist eine DSC-Baseline (Desired State Configuration) für jeden bereitgestellten virtuellen Computer inbegriffen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-2-1a"></a>NIST 800-53, Regelung CM-2 (1).a

#### <a name="baseline-configuration--reviews-and-updates"></a>Baselinekonfiguration | Überprüfungen und Updates

**CM-2 (1).a** Die Organisation überprüft und aktualisiert die Baselinekonfiguration des Informationssystems [Festsetzung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Prüfung und Aktualisierung der kundenseitig bereitgestellten Baselinekonfiguration (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-2-1b"></a>NIST 800-53, Regelung CM-2 (1).b

#### <a name="baseline-configuration--reviews-and-updates"></a>Baselinekonfiguration | Überprüfungen und Updates

**CM-2 (1).b** Die Organisation überprüft und aktualisiert die Baselinekonfiguration des Informationssystems, wenn dies aufgrund von [Festsetzung: von der Organisation definierte Umstände] erforderlich ist.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist ggf. für die Prüfung und Aktualisierung der kundenseitig bereitgestellten Baselinekonfiguration verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-2-1c"></a>NIST 800-53, Regelung CM-2 (1).c

#### <a name="baseline-configuration--reviews-and-updates"></a>Baselinekonfiguration | Überprüfungen und Updates

**CM-2 (1).c** Die Organisation überprüft und aktualisiert die Baselinekonfiguration des Informationssystems als wesentlichen Bestandteil von Neuinstallationen und Upgrades für Komponenten des Informationssystems.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist ggf. für die Prüfung und Aktualisierung der kundenseitig bereitgestellten Baselinekonfiguration verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-2-2"></a>NIST 800-53, Regelung CM-2 (2)

#### <a name="baseline-configuration--automation-support-for-accuracy--currency"></a>Baselinekonfiguration | Automatisierungsunterstützung zur Steigerung der Genauigkeit/Währung

**CM-2 (2)** Die Organisation setzt automatisierte Mechanismen ein, um eine aktuelle, vollständige, genaue und unmittelbar verfügbare Baselinekonfiguration des Informationssystems zu verwalten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Azure Resource Manager-Vorlagen und begleitenden Ressourcen, aus denen dieser Blueprint besteht, stellen die Baseline einer „Konfiguration als Code“ für die bereitgestellte Architektur dar. Die Lösung wird über GitHub zur Verfügung gestellt, das auch für die Konfigurationssteuerung verwendet werden kann. Im Azure-Portal steht ein Automatisierungsskript für alle bereitgestellten Ressourcen zur Verfügung, das eine stets aktuelle Darstellung dieser Ressourcen bietet.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-2-3"></a>NIST 800-53, Regelung CM-2 (3)

#### <a name="baseline-configuration--retention-of-previous-configurations"></a>Baselinekonfiguration | Beibehaltung von vorherigen Konfigurationen

**CM-2 (3)** Die Organisation behält [Festsetzung: von der Organisation definierte Vorgängerversionen von Baselinekonfigurationen des Informationssystems] bei, um Unterstützung für Rollbacks zu bieten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Beibehaltung von vorherigen Baselinekonfigurationsversionen für kundenseitig bereitgestellte Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-2-7a"></a>NIST 800-53, Regelung CM-2 (7).a

#### <a name="baseline-configuration--configure-systems-components-or-devices-for-high-risk-areas"></a>Baselinekonfiguration | Konfigurieren von Systemen, Komponenten und Geräten für Gebiete mit hohem Risiko

**CM-2 (7).a** Die Organisation verteilt [Festsetzung: von der Organisation definierte Informationssysteme, Systemkomponenten oder Geräte] mit [Festsetzung: von der Organisation definierten Konfigurationen] an Personen, die an von der Organisation mit bedeutendem Risiko eingestufte Standorte reisen.

**Zuständigkeiten:** `Not Applicable`

|||
|---|---|
| **Kunde** | Zum Geltungsbereich der in Azure bereitgestellten Systeme gehören keine kundenseitig gesteuerten physischen Geräte. |
| **Anbieter (Microsoft Azure)** | Inhalte von Microsoft Azure-Kunden werden niemals außerhalb des Microsoft Azure-Diensts gespeichert, der sich physisch auf dem Kontinent der USA befindet. Microsoft Azure-Mitarbeitern ist es nicht gestattet, zum Microsoft Azure-Bestand gehörende Geräte auf Reisen mitzunehmen. Aufgrund dessen ist diese Regelung auf Microsoft Azure nicht anwendbar. |


 ### <a name="nist-800-53-control-cm-2-7b"></a>NIST 800-53, Regelung CM-2 (7).b

#### <a name="baseline-configuration--configure-systems-components-or-devices-for-high-risk-areas"></a>Baselinekonfiguration | Konfigurieren von Systemen, Komponenten und Geräten für Gebiete mit hohem Risiko

**CM-2 (7).b** Die Organisation trifft bei der Geräterückgabe durch die jeweiligen Personen [Festsetzung: von der Organisation definierte Sicherheitsvorkehrungen].

**Zuständigkeiten:** `Not Applicable`

|||
|---|---|
| **Kunde** | Zum Geltungsbereich der in Azure bereitgestellten Systeme gehören keine kundenseitig gesteuerten physischen Geräte. |
| **Anbieter (Microsoft Azure)** | Da Inhalte von Microsoft Azure-Kunden niemals außerhalb von Microsoft Azure gespeichert werden und Microsoft Azure-Mitarbeiter zum Microsoft Azure-Bestand gehörende Geräte nicht auf Reisen mitnehmen, ist diese Regelung nicht anwendbar. |


 ## <a name="nist-800-53-control-cm-3a"></a>NIST 800-53, Regelung CM-3.a

#### <a name="configuration-change-control"></a>Steuerung von Konfigurationsänderungen

**CM-3.a** Die Organisation bestimmt, welche Arten von Änderungen am Informationssystem von der Konfiguration gesteuert werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, die konfigurationsgesteuerten Arten von Änderungen an kundenseitig bereitgestellten Ressourcen (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) zu bestimmen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-3b"></a>NIST 800-53, Regelung CM-3.b

#### <a name="configuration-change-control"></a>Steuerung von Konfigurationsänderungen

**CM-3.b** Die Organisation überprüft Vorschläge für konfigurationsgesteuerte Änderungen am Informationssystem und genehmigt derartige Änderungen unter expliziter Berücksichtigung der Analyse der Auswirkungen auf die Sicherheit, bzw. lehnt diese ab.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, Vorschläge für konfigurationsgesteuerte Änderungen an kundenseitig bereitgestellten Ressourcen zu überprüfen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-3c"></a>NIST 800-53, Regelung CM-3.c

#### <a name="configuration-change-control"></a>Steuerung von Konfigurationsänderungen

**CM-3.c** Die Organisation dokumentiert Entscheidungen zu Konfigurationsänderungen im Zusammenhang mit dem Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, konfigurationsgesteuerte Änderungen an kundenseitig bereitgestellten Ressourcen (siehe CM-03.b) zu dokumentieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-3d"></a>NIST 800-53, Regelung CM-3.d

#### <a name="configuration-change-control"></a>Steuerung von Konfigurationsänderungen

**CM-3.d** Die Organisation implementiert genehmigte konfigurationsgesteuerte Änderungen am Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Implementierung der unter CM-03.b genehmigten, konfigurationsgesteuerten Änderungen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-3e"></a>NIST 800-53, Regelung CM-3.e

#### <a name="configuration-change-control"></a>Steuerung von Konfigurationsänderungen

**CM-3.e** Die Organisation bewahrt Datensätze zu konfigurationsgesteuerten Änderungen am Informationssystem für einen Zeitraum von [Festsetzung: von der Organisation definierter Zeitraum] auf.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, einen Datensatz zu konfigurationsgesteuerten Änderungen an kundenseitig bereitgestellten Ressourcen beizubehalten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-3f"></a>NIST 800-53, Regelung CM-3.f

#### <a name="configuration-change-control"></a>Steuerung von Konfigurationsänderungen

**CM-3.f** Die Organisation überwacht und überprüft Aktivitäten im Zusammenhang mit konfigurationsgesteuerten Änderungen am Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überwachung und Überprüfung von Konfigurationsänderungen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-3g"></a>NIST 800-53, Regelung CM-3.g

#### <a name="configuration-change-control"></a>Steuerung von Konfigurationsänderungen

**CM-3.g** Die Organisation koordiniert und beaufsichtigt Aktivitäten zur Steuerung von Konfigurationsänderungen durch [Festsetzung: von der Organisation definierte Entitäten für die Steuerung von Konfigurationsänderungen (z.B. Komitee, Board)], der/die/das [Auswahl (mindestens ein Element): [Festsetzung: von der Organisation definierte Häufigkeit]; [Festsetzung: von der Organisation definierte Konfigurationsänderungsbedingungen]] einberufen wird.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Koordination und Beaufsichtigung von Aktivitäten zur Steuerung von Konfigurationsänderungen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-3-1a"></a>NIST 800-53, Regelung CM-3 (1).a

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Steuerung von Konfigurationsänderungen | Automatisierte Mechanismen zu Dokumentation, Benachrichtigungen und Änderungsverboten

**CM-3 (1).a** Die Organisation setzt automatisierte Mechanismen ein, um Änderungsvorschläge für das Informationssystem zu dokumentieren.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, automatisierte Mechanismen zur Dokumentation von Änderungsvorschlägen einzusetzen (siehe CM-03.b). |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-3-1b"></a>NIST 800-53, Regelung CM-3 (1).b

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Steuerung von Konfigurationsänderungen | Automatisierte Mechanismen zu Dokumentation, Benachrichtigungen und Änderungsverboten

**CM-3 (1).b** Die Organisation setzt automatisierte Mechanismen ein, um [Festsetzung: von der Organisation definierte Genehmigungsstellen] über Änderungsvorschläge für das Informationssystem in Kenntnis zu setzen und Genehmigungen für Änderungen anzufordern.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Einsatz eines automatisierten Mechanismus verantwortlich, um Genehmigungen für Änderungsvorschläge für kundenseitig bereitgestellte Ressourcen weiterzuleiten und anzufordern. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-3-1c"></a>NIST 800-53, Regelung CM-3 (1).c

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Steuerung von Konfigurationsänderungen | Automatisierte Mechanismen zu Dokumentation, Benachrichtigungen und Änderungsverboten

**CM-3 (1).c** Die Organisation setzt automatisierte Mechanismen ein, um nicht genehmigte oder abgelehnte Änderungsvorschläge für das Informationssystem für einen Zeitraum von [Festsetzung: von der Organisation definierter Zeitraum] hervorzuheben.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Einsatz eines automatisierten Mechanismus verantwortlich, um nicht überprüfte Änderungsvorschläge hervorzuheben. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-3-1d"></a>NIST 800-53, Regelung CM-3 (1).d

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Steuerung von Konfigurationsänderungen | Automatisierte Mechanismen zu Dokumentation, Benachrichtigungen und Änderungsverboten

**CM-3 (1).d** Die Organisation setzt automatisierte Mechanismen ein, um Änderungen am Informationssystem solange zu unterbinden, bis die bezeichneten Genehmigungen erteilt wurden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Einsatz eines automatisierten Mechanismus verantwortlich, um die Implementierung von nicht genehmigten Änderungen an kundenseitig bereitgestellten Ressourcen zu unterbinden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-3-1e"></a>NIST 800-53, Regelung CM-3 (1).e

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Steuerung von Konfigurationsänderungen | Automatisierte Mechanismen zu Dokumentation, Benachrichtigungen und Änderungsverboten

**CM-3 (1).e** Die Organisation setzt automatisierte Mechanismen ein, um alle Änderungen am Informationssystem zu dokumentieren.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Einsatz eines automatisierten Mechanismus verantwortlich, um alle implementierten Änderungen an kundenseitig bereitgestellten Ressourcen zu dokumentieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-3-1f"></a>NIST 800-53, Regelung CM-3 (1).f

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Steuerung von Konfigurationsänderungen | Automatisierte Mechanismen zu Dokumentation, Benachrichtigungen und Änderungsverboten

**CM-3 (1).f** Die Organisation setzt automatisierte Mechanismen ein, um [Festsetzung: von der Organisation definierte Mitarbeiter] darüber in Kenntnis zu setzen, wenn genehmigte Änderungen am Informationssystem vorgenommen wurden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Einsatz eines automatisierten Mechanismus verantwortlich, um Benachrichtigungen zu senden, wenn genehmigte Änderungen an kundenseitig bereitgestellten Ressourcen vorgenommen wurden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-3-2"></a>NIST 800-53, Regelung CM-3 (2)

#### <a name="configuration-change-control--test--validate--document-changes"></a>Steuerung von Konfigurationsänderungen | Testen, Überprüfen und Dokumentieren von Änderungen

**CM-3 (2)** Vor der Implementierung von Änderungen im Betriebssystem testet, überprüft und dokumentiert die Organisation diese Änderungen am Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, Änderungen an kundenseitig bereitgestellten Ressourcen vor ihrer Implementierung zu testen, zu überprüfen und zu dokumentieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-3-4"></a>NIST 800-53, Regelung CM-3 (4)

#### <a name="configuration-change-control--security-representative"></a>Steuerung von Konfigurationsänderungen | Sicherheitsbeauftragter

**CM-3 (4)** Die Organisation verlangt, dass ein Informationssicherheitsbeauftragter Mitglied von [Festsetzung: von der Organisation definierte Entitäten für die Steuerung von Konfigurationsänderungen] sein muss.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, ein Mitglied der unter CM 03.g definierten Entität für die Steuerung von Änderungen als Informationssicherheitsbeauftragten zu ernennen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-3-6"></a>NIST 800-53, Regelung CM-3 (6)

#### <a name="configuration-change-control--cryptography-management"></a>Steuerung von Konfigurationsänderungen | Verwaltung von kryptographischen Mechanismen

**CM-3 (6)** Die Organisation stellt sicher, dass die für [Festsetzung: von der Organisation definierten Sicherheitsvorkehrungen] vorgesehenen eingesetzten kryptographischen Mechanismen von der Konfigurationsverwaltung gesteuert werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, die ordnungsgemäße Steuerung der kryptographischen Mechanismen durch die Konfigurationsverwaltung sicherzustellen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-4"></a>NIST 800-53, Regelung CM-4

#### <a name="security-impact-analysis"></a>Analyse der Auswirkungen auf die Sicherheit

**CM-4** Die Organisation analysiert Änderungen am Informationssystem, um vor der Implementierung von Änderungen die möglichen Auswirkungen auf die Sicherheit zu ermitteln.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Analyse von Änderungsvorschlägen für kundenseitig bereitgestellte Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-4-1"></a>NIST 800-53, Regelung CM-4 (1)

#### <a name="security-impact-analysis--separate-test-environments"></a>Analyse der Auswirkungen auf die Sicherheit | Trennen von Testumgebungen

**CM-4 (1)** Bevor die Organisation Änderungen am Informationssystem in einer Betriebsumgebung implementiert, analysiert sie diese in einer separaten Testumgebung, indem sie Beeinträchtigungen der Sicherheit aufgrund von Schwachstellen, Inkompatibilität oder vorsätzlichen Verletzungen ermittelt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, Änderungsvorschläge für kundenseitig bereitgestellte Ressourcen in einer Testumgebung zu analysieren, bevor diese in einer Betriebsumgebung implementiert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-5"></a>NIST 800-53, Regelung CM-5

#### <a name="access-restrictions-for-change"></a>Zugriffseinschränkungen für Änderungen

**CM-5** Die Organisation definiert, dokumentiert, genehmigt und erzwingt Einschränkungen für den physischen und logischen Zugriff im Zusammenhang mit Änderungen am Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Kontoberechtigungen von Azure Active Directory werden mithilfe einer rollenbasierten Zugriffssteuerung implementiert, bei der Benutzer Rollen zugewiesen werden, die streng kontrollieren, welche Benutzer bereitgestellte Ressourcen anzeigen und steuern können. Active Directory-Kontoberechtigungen werden mithilfe einer rollenbasierten Zugriffssteuerung implementiert, indem Benutzer Sicherheitsgruppen zugewiesen werden. Diese Sicherheitsgruppen steuern die Aktionen, die Benutzer hinsichtlich der Betriebssystemkonfiguration ausführen können. Diese rollenbasierten Schemas können vom Kunden erweitert werden, um unternehmenskritische Anforderungen zu erfüllen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-5-1"></a>NIST 800-53, Regelung CM-5 (1)

#### <a name="access-restrictions-for-change--automated-access-enforcement--auditing"></a>Zugriffseinschränkungen für Änderungen | Automatisierte Zugriffserzwingung und Überwachung

**CM-5 (1)** Das Informationssystem erzwingt Zugriffseinschränkungen und unterstützt die Überwachung von Erzwingungsaktionen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Kontoberechtigungen von Azure Active Directory werden mithilfe einer rollenbasierten Zugriffssteuerung implementiert, bei der Benutzer Rollen zugewiesen werden, die streng kontrollieren, welche Benutzer bereitgestellte Ressourcen anzeigen und steuern können. Active Directory-Kontoberechtigungen werden mithilfe einer rollenbasierten Zugriffssteuerung implementiert, indem Benutzer Sicherheitsgruppen zugewiesen werden. Diese Sicherheitsgruppen steuern die Aktionen, die Benutzer hinsichtlich der Betriebssystemkonfiguration ausführen können. Alle Zugriffe und Zugriffsversuche werden überwacht. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-5-2"></a>NIST 800-53, Regelung CM-5 (2)

#### <a name="access-restrictions-for-change--review-system-changes"></a>Zugriffseinschränkungen für Änderungen | Überprüfen von Systemänderungen

**CM-5 (2)** Die Organisation überprüft [Festsetzung: von der Organisation definierte Häufigkeit] Änderungen am Informationssystem sowie [Festsetzung: von der Organisation definierte Umstände], um zu ermitteln, ob nicht autorisierte Änderungen vorgenommen wurden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überprüfung von Änderungen an kundenseitig bereitgestellten Ressourcen verantwortlich, um festzustellen, ob nicht autorisierte Änderungen vorgenommen wurden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-5-3"></a>NIST 800-53, Regelung CM-5 (3)

#### <a name="access-restrictions-for-change--signed-components"></a>Zugriffseinschränkungen für Änderungen | Signierte Komponenten

**CM-5 (3)** Das Informationssystem verhindert die Installation von [Festsetzung: von der Organisation definierten Software- und Firmwarekomponenten], wenn nicht überprüft wurde, ob die Komponente mit einem von der Organisation anerkannten und genehmigten Zertifikat digital signiert wurde.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer implementieren Windows AppLocker, um anzugeben, welche Benutzer bestimmte Anwendungen installieren und/oder ausführen können. Darüber hinaus sind alle Windows-Betriebssystemupdates digital signiert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-5-5a"></a>NIST 800-53, Regelung CM-5 (5).a

#### <a name="access-restrictions-for-change--limit-production--operational-privileges"></a>Zugriffseinschränkungen für Änderungen | Beschränken von Berechtigungen in Bezug auf Produktion/Betrieb

**CM-5 (5).a** Die Organisation beschränkt Berechtigungen zum Ändern von Informationssystemkomponenten und systembezogenen Informationen in einer Produktions- oder Betriebsumgebung.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, Berechtigungen zum Durchführen von Änderungen in kundenseitig bereitgestellten Produktions- oder Betriebsumgebungen zu beschränken. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-5-5b"></a>NIST 800-53, Regelung CM-5 (5).b

#### <a name="access-restrictions-for-change--limit-production--operational-privileges"></a>Zugriffseinschränkungen für Änderungen | Beschränken von Berechtigungen in Bezug auf Produktion/Betrieb

**CM-5 (5).b** Die Organisation überprüft die Berechtigungen [Festsetzung: von der Organisation definierte Häufigkeit], und wertet diese erneut aus.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, die unter CM-05 (05) definierten Berechtigungen zu überprüfen und einer erneuten Auswertung zu unterziehen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-6a"></a>NIST 800-53, Regelung CM-6.a

#### <a name="configuration-settings"></a>Konfigurationseinstellungen

**CM-6.a** Mithilfe von [Festsetzung: von der Organisation definierte Sicherheitskonfigurationsprüflisten], die gemäß den Betriebsanforderungen die restriktivsten Sicherheitseinstellungen darstellen, legt die Organisation Konfigurationseinstellungen für die im Informationssystem eingesetzten IT-Produkte fest bzw. dokumentiert diese.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint enthält eine DSC-Baseline (Desired State Configuration) für jeden bereitgestellten virtuellen Computer. Diese deklarativen PowerShell-Skripts definieren und konfigurieren die Ressourcen, auf die diese angewendet werden. Die DSC-Baseline für die von dieser Lösung bereitgestellten Ressourcen kann vom Kunden erweitert werden, um unternehmenskritische Anforderungen zu erfüllen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-6b"></a>NIST 800-53, Regelung CM-6.b

#### <a name="configuration-settings"></a>Konfigurationseinstellungen

**CM-6.b** Die Organisation implementiert die Konfigurationseinstellungen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint enthält eine DSC-Baseline (Desired State Configuration) für jeden bereitgestellten virtuellen Computer. Die Baselines werden bei der Bereitstellung automatisch durch benutzerdefinierte Skript-VM-Erweiterungen auf virtuelle Computer angewendet. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-6c"></a>NIST 800-53, Regelung CM-6.c

#### <a name="configuration-settings"></a>Konfigurationseinstellungen

**CM-6.c** Die Organisation identifiziert, dokumentiert und genehmigt basierend auf [Festsetzung: von der Organisation definierten Betriebsanforderungen] alle Abweichungen von festgelegten Konfigurationseinstellungen für [Festsetzung: von der Organisation definierte Komponenten des Informationssystems].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, Abweichungen von festgelegten Konfigurationseinstellungen für kundenseitig bereitgestellte Ressourcen zu identifizieren, zu dokumentieren und zu genehmigen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-6d"></a>NIST 800-53, Regelung CM-6.d

#### <a name="configuration-settings"></a>Konfigurationseinstellungen

**CM-6.d** Die Organisation überwacht und steuert Änderungen an den Konfigurationseinstellungen in Übereinstimmung mit den organisatorischen Richtlinien und Verfahren.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt Automation DSC bereit. Automation DSC stimmt Computerkonfigurationen mit einer bestimmten von der Organisation definierten Konfiguration ab. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-6-1"></a>NIST 800-53, Regelung CM-6 (1)

#### <a name="configuration-settings--automated-central-management--application--verification"></a>Konfigurationseinstellungen | Automatisierte zentrale Verwaltung, Anwendung und Überprüfung

**CM-6 (1)** Die Organisation setzt automatisierte Mechanismen ein, um Konfigurationseinstellungen für [Festsetzung: von der Organisation definierte Informationssystemkomponenten] zentral zu verwalten, anzuwenden und zu überprüfen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt Azure Automation DSC bereit. Automation DSC stimmt Computerkonfigurationen mit einer bestimmten von der Organisation definierten Konfiguration ab und überwacht außerdem fortlaufend Änderungen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-6-2"></a>NIST 800-53, Regelung CM-6 (2)

#### <a name="configuration-settings--respond-to-unauthorized-changes"></a>Konfigurationseinstellungen | Reaktion auf nicht autorisierte Änderungen

**CM-6 (2)** Die Organisation setzt [Festsetzung: von der Organisation definierte Sicherheitsmaßnahmen] ein, um auf nicht autorisierte Änderungen an [Festsetzung: von der Organisation definierten Konfigurationseinstellungen] zu reagieren.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt Azure Automation DSC bereit. Automation DSC kann so konfiguriert werden, dass bei Feststellung von Fehlkonfigurationen eine Benachrichtigung generiert wird oder diese behoben werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-7a"></a>NIST 800-53, Regelung CM-7.a

#### <a name="least-functionality"></a>Mindestfunktionalität

**CM-7.a** Die Organisation konfiguriert das Informationssystem so, dass ausschließlich wesentliche Funktionen bereitgestellt werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten Ressourcen sind so konfiguriert, dass die Mindestfunktionen für den jeweiligen vorgesehenen Einsatzzweck bereitgestellt werden. Im Leistungsumfang eines jeden bereitgestellten virtuellen Computers ist eine DSC-Baseline (Desired State Configuration) inbegriffen. Diese deklarativen PowerShell-Skripts definieren und konfigurieren die Ressourcen, auf die diese angewendet werden. Die DSC-Baseline für die von dieser Lösung bereitgestellten Ressourcen kann vom Kunden erweitert werden, um die Funktionalität weiter zu beschränken und so unternehmenskritische Anforderungen erfüllen zu können. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-7b"></a>NIST 800-53, Regelung CM-7.b

#### <a name="least-functionality"></a>Mindestfunktionalität

**CM-7.b** Die Organisation untersagt oder beschränkt die Verwendung der folgenden Funktionen, Ports, Protokolle und/oder Dienste: [Festsetzung: von der Organisation definierte verbotene oder eingeschränkte Funktionen, Ports, Protokolle und/oder Dienste].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt Azure Application Gateway und Netzwerksicherheitsgruppen bereit, mit denen die Nutzung auf die erforderlichen Ports und Protokolle beschränkt werden kann. Kunden können für Application Gateway, Netzwerksicherheitsgruppen und DSC-Baselines für virtuelle Computer weitere Konfigurationen vornehmen, um die Verwendung von Funktionen, Ports, Protokollen und Diensten ausschließlich auf die Bereitstellung der vorgesehenen Funktionalität zu beschränken. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-7-1a"></a>NIST 800-53, Regelung CM-7 (1).a

#### <a name="least-functionality--periodic-review"></a>Mindestfunktionalität | Regelmäßige Überprüfung

**CM-7 (1).a** Die Organisation überprüft das Informationssystem [Festsetzung: von der Organisation definierte Häufigkeit], um unnötige und/oder unsichere Funktionen, Ports, Protokolle und Dienste zu identifizieren.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Prüfung und Analyse der kundenseitig bereitgestellten Ressourcen (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) verantwortlich, um unnötige und/oder unsichere Funktionen, Ports, Protokolle und Dienste zu identifizieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-7-1b"></a>NIST 800-53, Regelung CM-7 (1).b

#### <a name="least-functionality--periodic-review"></a>Mindestfunktionalität | Regelmäßige Überprüfung

**CM-7 (1).b** Die Organisation deaktiviert [Festsetzung: von der Organisation definierte Funktionen, Ports, Protokolle und Dienste innerhalb des Informationssystems, die als unnötig und/oder unsicher eingestuft werden].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, als unnötig oder unsicher eingestufte Funktionen, Ports, Protokolle und Dienste zu deaktivieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-7-2"></a>NIST 800-53, Regelung CM-7 (2)

#### <a name="least-functionality--prevent-program-execution"></a>Mindestfunktionalität | Unterbinden der Programmausführung

**CM-7 (2)** Das Informationssystem verhindert die Programmausführung in Übereinstimmung mit [Auswahl (mindestens ein Element): [Festsetzung: von der Organisation definierten Richtlinien für die Nutzung und Einschränkungen von Softwareprogrammen]; Regeln, die die Geschäftsbedingungen für die Nutzung von Softwareprogrammen festlegen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, die Programmausführung gemäß den vom Kunden definierten Nutzungsrichtlinien für Softwareprogramme zu unterbinden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-7-5a"></a>NIST 800-53, Regelung CM-7 (5).a

#### <a name="least-functionality--authorized-software--whitelisting"></a>Mindestfunktionalität | Autorisierte Software und Whitelists

**CM-7 (5).a** Die Organisation identifiziert [Festsetzung: von der Organisation definierte Softwareprogramme, die zur Ausführung im Informationssystem berechtigt sind].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Identifizierung von autorisierten Softwareprogrammen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-7-5b"></a>NIST 800-53, Regelung CM-7 (5).b

#### <a name="least-functionality--authorized-software--whitelisting"></a>Mindestfunktionalität | Autorisierte Software und Whitelists

**CM-7 (5).b** Die Organisation setzt eine Richtlinie ein, bei der alle Programme abgelehnt und Programme in Form von Ausnahmen hinzugefügt werden, um die Ausführung von autorisierten Softwareprogrammen im Informationssystem zu ermöglichen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Einsatz einer Richtlinie verantwortlich, bei der alle Programme abgelehnt und Programme in Form von Ausnahmen hinzugefügt werden, um die Ausführung von autorisierten Softwareprogrammen auf kundenseitig bereitgestellten Ressourcen zu ermöglichen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-7-5c"></a>NIST 800-53, Regelung CM-7 (5).c

#### <a name="least-functionality--authorized-software--whitelisting"></a>Mindestfunktionalität | Autorisierte Software und Whitelists

**CM-7 (5).c** Die Organisation überprüft und aktualisiert die Liste der autorisierten Softwareprogramme [Festsetzung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, die Liste der autorisierten Softwareprogramme zu überprüfen und zu aktualisieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-8a"></a>NIST 800-53, Regelung CM-8.a

#### <a name="information-system-component-inventory"></a>Komponentenbestand des Informationssystems

**CM-8.a** Die Organisation entwickelt und dokumentiert einen Komponentenbestand für das Informationssystem, der folgende Eigenschaften erfüllt: entspricht exakt dem aktuellen Informationssystem; umfasst alle Komponenten innerhalb der Autorisierungsgrenze des Informationssystems; befindet sich auf der für die Nachverfolgung und Berichterstellung als notwendig erachteten Granularitätsebene; und enthält [Festsetzung: von der Organisation definierte Informationen, die zur Ermöglichung einer effektiven Verantwortlichkeit für Informationssystemkomponenten als notwendig erachtet werden].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt alle Ressourcen in einer Azure Resource Manager-Ressourcengruppe bereit. Azure Resource Manager bietet eine stets aktuelle Liste der bereitgestellten Ressourcen und kann zur Kennzeichnung und Gruppierung von Ressourcen für die Bestandsverwaltung angepasst werden. Die von dieser Lösung bereitgestellten Ressourcen erhalten ein bestimmtes Ressourcentag, das mit der Systemgrenze verknüpft werden kann. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-8b"></a>NIST 800-53, Regelung CM-8.b

#### <a name="information-system-component-inventory"></a>Komponentenbestand des Informationssystems

**CM-8.b** Die Organisation überprüft und aktualisiert den Komponentenbestand des Informationssystems [Festsetzung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt alle Ressourcen in einer Azure Resource Manager-Ressourcengruppe bereit. Azure Resource Manager bietet eine stets aktuelle Liste der bereitgestellten Ressourcen, die im Azure-Portal zur Einsicht zur Verfügung stehen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-8-1"></a>NIST 800-53, Regelung CM-8 (1)

#### <a name="information-system-component-inventory--updates-during-installations--removals"></a>Komponentenbestand des Informationssystems | Updates bei Installationen und Entfernungen

**CM-8 (1)** Die Organisation aktualisiert den Komponentenbestand des Informationssystems als integralen Bestandteil von Komponenteninstallationen, Entfernungen und Aktualisierungen des Informationssystems.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt alle Ressourcen in einer Azure Resource Manager-Ressourcengruppe bereit. Auf dem Ressourcenblatt im Azure-Portal werden alle bereitgestellten Ressourcen aufgelistet, sodass beim Bereitstellen und Entfernen von Ressourcen stets der aktuelle Bestand dargestellt wird. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-8-2"></a>NIST 800-53, Regelung CM-8 (2)

#### <a name="information-system-component-inventory--automated-maintenance"></a>Komponentenbestand des Informationssystems | Automatisierte Wartung

**CM-8 (2)** Die Organisation setzt automatisierte Mechanismen ein, um einen aktuellen, vollständigen, genauen und unmittelbar verfügbaren Komponentenbestand des Informationssystems zu verwalten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt alle Ressourcen in einer Azure Resource Manager-Ressourcengruppe bereit. Auf dem Ressourcenblatt im Azure-Portal werden alle bereitgestellten Ressourcen aufgelistet, sodass beim Bereitstellen und Entfernen von Ressourcen stets der aktuelle Bestand dargestellt wird. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-8-3a"></a>NIST 800-53, Regelung CM-8 (3).a

#### <a name="information-system-component-inventory--automated-unauthorized-component-detection"></a>Komponentenbestand des Informationssystems | Automatisierte Erkennung von nicht autorisierten Komponenten

**CM-8 (3).a** Die Organisation setzt [Festsetzung: von der Organisation definierte Häufigkeit] automatisierte Mechanismen ein, um das Vorhandensein nicht autorisierter Hardware-, Software- und Firmwarekomponenten innerhalb des Informationssystems zu erkennen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Einsatz von automatisierten Mechanismen verantwortlich, um das Vorhandensein von nicht autorisierter Software in kundenseitig bereitgestellten Ressourcen zu erkennen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-8-3b"></a>NIST 800-53, Regelung CM-8 (3).b

#### <a name="information-system-component-inventory--automated-unauthorized-component-detection"></a>Komponentenbestand des Informationssystems | Automatisierte Erkennung von nicht autorisierten Komponenten

**CM-8 (3).b** Die Organisation ergreift die folgenden Maßnahmen, wenn nicht autorisierte Komponenten erkannt werden: [Auswahl (mindestens ein Element): Deaktivieren des Netzwerkzugriffs durch solche Komponenten; Isolieren der Komponenten; Benachrichtigen von [Festsetzung: von der Organisation definiertes Personal oder Rollen]].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Ergreifung von Maßnahmen verantwortlich, wenn nicht autorisierte Software erkannt wird. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-8-4"></a>NIST 800-53, Regelung CM-8 (4)

#### <a name="information-system-component-inventory--accountability-information"></a>Komponentenbestand des Informationssystems | Informationen zur Verantwortlichkeit

**CM-8 (4)** Die Organisation gibt in den Informationen zum Komponentenbestand des Informationssystems eine durch [Auswahl (mindestens ein Element): Name; Position; Rolle] festzulegende Methode an, um die verantwortlichen Personen für die Verwaltung dieser Komponenten zu bestimmen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt alle Ressourcen in einer Azure Resource Manager-Ressourcengruppe bereit. Bei Azure-Ressourcentags handelt es sich um Schlüssel-Wert-Paare, die zur Kategorisierung von Ressourcen für Verantwortlichkeits- und/oder Verwaltungszwecke verwendet werden können. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-8-5"></a>NIST 800-53, Regelung CM-8 (5)

#### <a name="information-system-component-inventory--no-duplicate-accounting-of-components"></a>Komponentenbestand des Informationssystems | Vermeidung einer doppelten Kontoverwaltung für Komponenten

**CM-8 (5)** Die Organisation stellt sicher, dass alle Komponenten innerhalb der Autorisierungsgrenze des Informationssystems nicht in anderen Komponentenbeständen des Informationssystems doppelt vorkommen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt alle Ressourcen in einer Azure Resource Manager-Ressourcengruppe bereit. Azure Resource Manager bietet eine stets aktuelle Liste der bereitgestellten Ressourcen. Die von dieser Lösung bereitgestellten Ressourcen erhalten ein bestimmtes Ressourcentag, das mit der Systemgrenze verknüpft werden kann. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-9a"></a>NIST 800-53, Regelung CM-9.a

#### <a name="configuration-management-plan"></a>Konfigurationsverwaltungsplan

**CM-9.a** Die Organisation entwickelt, dokumentiert und implementiert einen Konfigurationsverwaltungsplan für das Informationssystem, der Rollen, Zuständigkeiten und Konfigurationsverwaltungsprozesse sowie -verfahren festlegt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, einen Konfigurationsverwaltungsplan für kundenseitig bereitgestellte Ressourcen zu entwickeln, zu dokumentieren und zu implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-9b"></a>NIST 800-53, Regelung CM-9.b

#### <a name="configuration-management-plan"></a>Konfigurationsverwaltungsplan

**CM-9.b** Die Organisation entwickelt, dokumentiert und implementiert einen Konfigurationsverwaltungsplan für das Informationssystem, der einen Prozess zur Identifizierung von Konfigurationselementen für den Systementwicklungszyklus und zur Konfigurationsverwaltung von Konfigurationselementen festlegt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, einen Konfigurationsverwaltungsplan für kundenseitig bereitgestellte Ressourcen zu entwickeln, zu dokumentieren und zu implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-9c"></a>NIST 800-53, Regelung CM-9.c

#### <a name="configuration-management-plan"></a>Konfigurationsverwaltungsplan

**CM-9.c** Die Organisation entwickelt, dokumentiert und implementiert einen Konfigurationsverwaltungsplan für das Informationssystem, der die Konfigurationselemente für das Informationssystem definiert und die Konfigurationselemente der Konfigurationsverwaltung unterstellt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, einen Konfigurationsverwaltungsplan für kundenseitig bereitgestellte Ressourcen zu entwickeln, zu dokumentieren und zu implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-9d"></a>NIST 800-53, Regelung CM-9.d

#### <a name="configuration-management-plan"></a>Konfigurationsverwaltungsplan

**CM-9.d** Die Organisation entwickelt, dokumentiert und implementiert einen Konfigurationsverwaltungsplan für das Informationssystem, der vor einer nicht autorisierten Offenlegung und Änderung geschützt ist.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, einen Konfigurationsverwaltungsplan für kundenseitig bereitgestellte Ressourcen zu entwickeln, zu dokumentieren und zu implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-10a"></a>NIST 800-53, Regelung CM-10.a

#### <a name="software-usage-restrictions"></a>Einschränkungen für die Softwarenutzung

**CM-10.a** Die Organisation verwendet Software und die dazugehörige Dokumentation im Einklang mit vertraglichen Vereinbarungen und Urheberrechtsgesetzen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Für die durch diesen Blueprint bereitgestellten Ressourcen sind Windows- und SQL Server-Lizenzen inbegriffen. Dies ist ein integriertes Feature von Azure. Organisationen mit bestehenden Softwarelizenzbedingungen können die Bereitstellung alternativer Lizenzmodelle in Erwägung ziehen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-10b"></a>NIST 800-53, Regelung CM-10.b

#### <a name="software-usage-restrictions"></a>Einschränkungen für die Softwarenutzung

**CM-10.b** Die Organisation verfolgt die Nutzung von Software und der zugehörigen Dokumentation nach, die durch Mengenlizenzen geschützt sind, um die Erstellung von Kopien und Verteilung zu kontrollieren.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Für die durch diesen Blueprint bereitgestellten Ressourcen sind Windows- und SQL Server-Lizenzen inbegriffen. Der Benutzer ist nicht verpflichtet, die Nutzung der Lizenzen gesondert nachzuverfolgen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-10c"></a>NIST 800-53, Regelung CM-10.c

#### <a name="software-usage-restrictions"></a>Einschränkungen für die Softwarenutzung

**CM-10.c** Die Organisation kontrolliert und dokumentiert die Verwendung der Peer-to-Peer-Technologie für Dateifreigaben, um sicherzustellen, dass diese Funktion nicht für die unautorisierte Verbreitung, Darstellung, Wiedergabe oder Vervielfältigung von urheberrechtlich geschützten Werken verwendet wird.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Durch diesen Blueprint werden keine Peer-to-Peer-Dateifreigabefunktionen bereitgestellt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-10-1"></a>NIST 800-53, Regelung CM-10 (1)

#### <a name="software-usage-restrictions--open-source-software"></a>Einschränkungen für die Softwarenutzung | Open Source-Software

**CM-10 (1)** Die Organisation legt die folgenden Einschränkungen für die Verwendung von Open Source-Software fest: [Festsetzung: von der Organisation definierte Einschränkungen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Richtlinie des Kunden für die Konfigurationsverwaltung auf Unternehmensebene kann Einschränkungen für die Nutzung von Open Source-Software umfassen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-11a"></a>NIST 800-53, Regelung CM-11.a

#### <a name="user-installed-software"></a>Von Benutzern installierte Software

**CM-11.a** Die Organisation legt [Festsetzung: von der Organisation definierte Richtlinien] fest, die die Installation von Software durch Benutzer regeln.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, eine Richtlinie für die Installation von Software auf den kundenseitig bereitgestellten Ressourcen durch Benutzer zu erstellen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-11b"></a>NIST 800-53, Regelung CM-11.b

#### <a name="user-installed-software"></a>Von Benutzern installierte Software

**CM-11.b** Die Organisation erzwingt Softwareinstallationsrichtlinien durch [Festsetzung: von der Organisation definierte Methoden].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Erzwingung von Softwareinstallationsrichtlinien verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-cm-11c"></a>NIST 800-53, Regelung CM-11.c

#### <a name="user-installed-software"></a>Von Benutzern installierte Software

**CM-11.c** Die Organisation überwacht die Einhaltung der Richtlinien [Festsetzung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überwachung der Compliance für kundenseitig bereitgestellte Ressourcen mit den unter CM-11.a. festgelegten Richtlinien verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-cm-11-1"></a>NIST 800-53, Regelung CM-11 (1)

#### <a name="user-installed-software--alerts-for-unauthorized-installations"></a>Vom Benutzer installierte Software | Benachrichtigungen zu nicht autorisierten Installationen

**CM-11 (1)** Das Informationssystem setzt [Festsetzung: von der Organisatorisch definiertes Personal oder Rollen] darüber in Kenntnis, wenn eine nicht autorisierte Softwareinstallation erkannt wird.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, Benachrichtigungen zu senden, wenn eine nicht autorisierte Softwareinstallation festgestellt wird. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |

