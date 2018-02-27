---
title: "Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP – System- und Informationsintegrität"
description: "Automatisierung von Webanwendungen für FedRAMP – System- und Informationsintegrität"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 2ff2778b-2c37-41b5-a39c-6594b3e3b10b
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 2d744032c2a35fbbedf34397861c3bb03aa39939
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
---
# <a name="system-and-information-integrity-si"></a>Integrität von System und Informationen (SI)

> [!NOTE]
> Diese Regelungen werden von NIST und dem U.S. Department of Commerce als Teil der NIST-Sonderveröffentlichung 800-53 Revision 4 definiert. Informationen zu Testverfahren und Anleitungen für jede Regelung finden Sie unter NIST 800-53 Rev. 4.

## <a name="nist-800-53-control-si-1"></a>NIST 800-53, Regelung SI-1

#### <a name="system-and-information-integrity-policy-and-procedures"></a>Integrität von System und Informationen – Richtlinien und Verfahren

**SI-1** Die Organisation entwickelt, dokumentiert und verteilt an [Zuordnung: von der Organisation definierte Personen oder Rollen] eine Richtlinie für die System- und Informationsintegrität, die sich mit Zweck, Umfang, Rollen, Zuständigkeiten, Managementverpflichtung, Koordination zwischen Organisationsentitäten und Konformität befasst, stellt Verfahren zum Ermöglichen der Implementierung der Richtlinie für die System- und Informationsintegrität und der zugehörigen Regelungen für die System- und Informationsintegrität bereit und überprüft und aktualisiert die aktuelle Richtlinie für die System- und Informationsintegrität [Zuordnung: von der Organisation definierte Häufigkeit] sowie die Verfahren für die System- und Informationsintegrität [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Richtlinien und Verfahren des Kunden für die Integrität von System und Informationen auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-2a"></a>NIST 800-53, Regelung SI-2.a

#### <a name="flaw-remediation"></a>Fehlerbehebung

**SI-2.a** Die Organisation identifiziert, meldet und korrigiert Fehler im Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt die OMS Automation & Control-Lösung bereit, um den Status von Updates für die in dieser Architektur bereitgestellten virtuellen Windows-Computer nachzuverfolgen. Auf dem OMS-Dashboard zeigt die Kachel „Updateverwaltung“ den Fehlerbehebungsstatus für alle bereitgestellten Windows-Server. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-2b"></a>NIST 800-53, Regelung SI-2.b

#### <a name="flaw-remediation"></a>Fehlerbehebung

**SI-2.b** Die Organisation testet Software- und Firmwareupdates in Bezug auf die Fehlerbehebung vor der Installation auf Effektivität und mögliche Nebenwirkungen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, Updates in Bezug auf die Fehlerbehebung vor der Installation auf kundenseitig bereitgestellten Ressourcen auf Effektivität und mögliche Nebenwirkungen zu testen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-2c"></a>NIST 800-53, Regelung SI-2.c

#### <a name="flaw-remediation"></a>Fehlerbehebung

**SI-2.c** Die Organisation installiert sicherheitsrelevante Software-und Firmwareupdates innerhalb von [Zuordnung: von der Organisation definierter Zeitraum] ab Freigabe der Updates.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Virtuelle Windows-Computer, die durch diesen Blueprint bereitgestellt werden, sind standardmäßig so konfiguriert, dass sie automatische Updates vom Windows Update-Dienst erhalten. Diese Lösung stellt auch die OMS Automation & Control-Lösung bereit, mit der Updatebereitstellungen erstellt werden können, um bei Bedarf Patches auf Windows-Servern bereitzustellen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-2d"></a>NIST 800-53, Regelung SI-2.d

#### <a name="flaw-remediation"></a>Fehlerbehebung

**SI-2.d** Die Organisation integriert die Fehlerbehebung in den Konfigurationsverwaltungsprozess der Organisation.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, die Fehlerbehebung in die Konfigurationsverwaltung einzubeziehen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-2-1"></a>NIST 800-53, Regelung SI-2 (1)

#### <a name="flaw-remediation--central-management"></a>Fehlerbehebung | Zentrale Verwaltung

**SI-2 (1)** Die Organisation verwaltet den Fehlerbehebungsprozess zentral.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt die OMS Automation & Control-Lösung bereit, um den Status von Updates für die in dieser Architektur bereitgestellten virtuellen Windows-Computer nachzuverfolgen. Auf dem OMS-Dashboard zeigt die Kachel „Updateverwaltung“ den Fehlerbehebungsstatus für alle bereitgestellten Windows-Server. Updatebereitstellungen können erstellt werden, um bei Bedarf Patches auf Windows-Servern bereitzustellen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-2-2"></a>NIST 800-53, Regelung SI-2 (2)

#### <a name="flaw-remediation--automated-flaw-remediation-status"></a>Fehlerbehebung | Automatisierter Fehlerbehebungsstatus

**SI-2 (2)** Die Organisation setzt [Zuordnung: von der Organisation definierte Häufigkeit] automatisierte Mechanismen ein, um den Zustand der Komponenten des Informationssystems in Bezug auf die Fehlerbehebung zu ermitteln.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt die OMS Automation & Control-Lösung bereit, um den Status von Updates für die in dieser Architektur bereitgestellten virtuellen Windows-Computer nachzuverfolgen. Für jeden verwalteten Windows-Computer wird zweimal pro Tag ein Scanvorgang durchgeführt. Alle 15 Minuten wird die Windows-API aufgerufen, um den letzten Updatezeitpunkt abzufragen und zu ermitteln, ob sich der Status geändert hat. Wenn ja, wird ein Konformitätsscan initiiert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-2-3a"></a>NIST 800-53, Regelung SI-2 (3).a

#### <a name="flaw-remediation--time-to-remediate-flaws--benchmarks-for-corrective-actions"></a>Fehlerbehebung | Zeitraum bis zur Behebung von Fehlern / Benchmarks für Korrekturmaßnahmen

**SI-2 (3).a** Die Organisation misst den Zeitraum zwischen der Erkennung und Behebung von Fehlern.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Behebung von Fehlern in kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-2-3b"></a>NIST 800-53, Regelung SI-2 (3).b

#### <a name="flaw-remediation--time-to-remediate-flaws--benchmarks-for-corrective-actions"></a>Fehlerbehebung | Zeitraum bis zur Behebung von Fehlern / Benchmarks für Korrekturmaßnahmen

**SI-2 (3).b** Die Organisation richtet [Zuordnung: von der Organisation definierte Benchmarks] für das Ergreifen von Korrekturmaßnahmen ein.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde kann für Fehlerbehebungsprozesse Benchmarks auf Unternehmensebene verwenden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-3a"></a>NIST 800-53, Regelung SI-3.a

#### <a name="malicious-code-protection"></a>Schutz vor schädlichem Code

**SI-3.a** Die Organisation setzt an den Ein- und Ausgangspunkten des Informationssystems Mechanismen für den Schutz vor schädlichem Code ein, um schädlichen Code zu erkennen und zu beseitigen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt über die Microsoft Antischadsoftware-Erweiterung für virtuelle Computer hostbasierten Schutz vor Schadsoftware für alle bereitgestellten virtuellen Windows-Computer bereit. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-3b"></a>NIST 800-53, Regelung SI-3.b

#### <a name="malicious-code-protection"></a>Schutz vor schädlichem Code

**SI-3.b** Die Organisation aktualisiert die Mechanismen zum Schutz vor schädlichem Code gemäß den Konfigurationsverwaltungsrichtlinien und -verfahren der Organisation, sobald neue Releases verfügbar sind.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt über die Microsoft Antischadsoftware-Erweiterung für virtuelle Computer hostbasierten Schutz vor Schadsoftware für alle bereitgestellten virtuellen Windows-Computer bereit. Diese Erweiterung ist so konfiguriert, dass die Antischadsoftware-Engine und die Schutzsignaturen automatisch aktualisiert werden, sobald Releases verfügbar sind. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-3c"></a>NIST 800-53, Regelung SI-3.c

#### <a name="malicious-code-protection"></a>Schutz vor schädlichem Code

**SI-3.c** Die Organisation konfiguriert Mechanismen zum Schutz vor schädlichem Code, sodass in Übereinstimmung mit der Sicherheitsrichtlinie der Organisation [Zuordnung: von der Organisation definierte Häufigkeit] regelmäßige Scans des Informationssystems sowie Echtzeitscans von Dateien aus externen Quellen am [Auswahl (mindestens ein Element): Endpunkt, Ein- und Ausgangspunkt des Netzwerks] durchgeführt werden, wenn die Dateien heruntergeladen, geöffnet oder ausgeführt werden; die Organisation konfiguriert folgende Mechanismen als Reaktion auf die Erkennung von schädlichem Code: [Auswahl (mindestens ein Element): Blockieren von schädlichem Code, Isolieren von schädlichem Code, Senden von Benachrichtigungen an einen Administrator, [Zuordnung: von der Organisation definierte Aktion]].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt über die Microsoft Antischadsoftware-Erweiterung für virtuelle Computer hostbasierten Schutz vor Schadsoftware für alle bereitgestellten virtuellen Windows-Computer bereit. Diese Erweiterung ist so konfiguriert, dass sowohl Echtzeitüberprüfungen als auch regelmäßige (wöchentliche) Überprüfungen durchgeführt, die Antischadsoftware-Engine und die Schutzsignaturen automatisch aktualisiert und automatische Korrekturmaßnahmen durchgeführt werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-3d"></a>NIST 800-53, Regelung SI-3.d

#### <a name="malicious-code-protection"></a>Schutz vor schädlichem Code

**SI-3.d** Die Organisation kümmert sich um falsch positive Ergebnisse während der Erkennung und Beseitigung von schädlichem Code und die daraus resultierenden möglichen Auswirkungen auf die Verfügbarkeit des Informationssystems.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Schutz vor schädlichem Code verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-3-1"></a>NIST 800-53, Regelung SI-3 (1)

#### <a name="malicious-code-protection--central-management"></a>Schutz vor schädlichem Code | Zentrale Verwaltung

**SI-3 (1)** Die Organisation verwaltet die Mechanismen zum Schutz vor schädlichem Code zentral.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt über die Microsoft Antischadsoftware-Erweiterung für virtuelle Computer hostbasierten Schutz vor Schadsoftware für alle bereitgestellten virtuellen Windows-Computer bereit. Azure OMS stellt zentrale Funktionen zum Überprüfen des aktuellen Status der Antischadsoftware-Lösung bereit. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-3-2"></a>NIST 800-53, Regelung SI-3 (2)

#### <a name="malicious-code-protection--automatic-updates"></a>Schutz vor schädlichem Code | Automatische Updates

**SI-3 (2)** Das Informationssystem aktualisiert Mechanismen zum Schutz vor schädlichem Code automatisch.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt über die Microsoft Antischadsoftware-Erweiterung für virtuelle Computer hostbasierten Schutz vor Schadsoftware für alle bereitgestellten virtuellen Windows-Computer bereit. Diese Erweiterung ist so konfiguriert, dass die Antischadsoftware-Engine und die Schutzsignaturen automatisch aktualisiert werden, sobald Releases verfügbar sind. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-3-7"></a>NIST 800-53, Regelung SI-3 (7)

#### <a name="malicious-code-protection--nonsignature-based-detection"></a>Schutz vor schädlichem Code | Basierend auf der Erkennung nicht vorhandener Signaturen

**SI-3 (7)** Das Informationssystem implementiert Mechanismen zum Schutz vor schädlichem Code basierend auf der Erkennung nicht vorhandener Signaturen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt über die Microsoft Antischadsoftware-Erweiterung für virtuelle Computer hostbasierten Schutz vor Schadsoftware für alle bereitgestellten virtuellen Windows-Computer bereit. Diese Erweiterung ist für die Ausführung einer heuristischen Erkennung konfiguriert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-4a"></a>NIST 800-53, Regelung SI-4.a

#### <a name="information-system-monitoring"></a>Überwachung des Informationssystems

**SI-4.a** Die Organisation überwacht das Informationssystem, um Angriffe und Anzeichen potenzieller Angriffe gemäß [Zuordnung: von der Organisation definierte Ziele] sowie nicht autorisierte lokale, Netzwerk- und Remoteverbindungen zu erkennen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt die Log Analytics-Lösung und die Sicherheits- und Überwachungslösung von OMS bereit. Diese Lösung bietet eine umfassende Übersicht über die Sicherheitslage sowie über Angriffe und Anzeichen möglicher Angriffe. Das Sicherheits- und Überwachungsdashboard gewährt High-Level-Einblick in den Sicherheitszustand der bereitgestellten Ressourcen anhand von Daten, die in allen bereitgestellten OMS-Lösungen verfügbar sind. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-4b"></a>NIST 800-53, Regelung SI-4.b

#### <a name="information-system-monitoring"></a>Überwachung des Informationssystems

**SI-4.b** Die Organisation identifiziert eine nicht autorisierte Verwendung des Informationssystems durch [Zuordnung: von der Organisation definierte Techniken und Methoden].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt die Sicherheits- und Überwachungslösung von OMS bereit. Die Domäne für Identifizierung und Zugriff stellt ein Dashboard mit einer Übersicht über den Identitätszustand des Informationssystems bereit, einschließlich der Anzahl von nicht erfolgreichen Anmeldeversuchen und der aktuellen Anzahl von angemeldeten Konten. Die in diesem Dashboard verfügbaren Informationen können beim Identifizieren einer potenziell verdächtigen Aktivität helfen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-4c"></a>NIST 800-53, Regelung SI-4.c

#### <a name="information-system-monitoring"></a>Überwachung des Informationssystems

**SI-4.c** Die Organisation stellt Überwachungsgeräte strategisch innerhalb des Informationssystems bereit, um von der Organisation festgelegte wichtige Informationen zu erfassen; Überwachungsgeräte werden zudem an Ad-hoc-Standorten im System bereitgestellt, um bestimmte Arten von Transaktionen nachzuverfolgen, die für die Organisation von Interesse sind.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt die Log Analytics-Lösung und die Sicherheits- und Überwachungslösung von OMS bereit. Das Sicherheits- und Überwachungsdashboard gewährt High-Level-Einblick in den Sicherheitszustand der bereitgestellten Ressourcen anhand von Daten, die in allen bereitgestellten OMS-Lösungen verfügbar sind, und gewährt auch Einblick in Überwachungsdaten der VM-Betriebssysteme. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-4d"></a>NIST 800-53, Regelung SI-4.d

#### <a name="information-system-monitoring"></a>Überwachung des Informationssystems

**SI-4.d** Die Organisation schützt die aus Tools für die Überwachung von Eindringversuchen abgerufenen Informationen vor nicht autorisierten Zugriffen, Bearbeitungen und Löschungen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Logische Zugriffskontrollen werden verwendet, um Überwachungsinformationen in diesem Blueprint vor nicht autorisierten Zugriffen, Änderungen und Löschungen zu schützen. Azure Active Directory erzwingt einen genehmigten logischen Zugriff mithilfe von rollenbasierten Gruppenmitgliedschaften. Die Möglichkeit zum Anzeigen von Überwachungsinformationen und Verwenden von Überwachungstools kann auf Benutzer beschränkt werden, die diese Berechtigungen benötigen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-4e"></a>NIST 800-53, Regelung SI-4.e

#### <a name="information-system-monitoring"></a>Überwachung des Informationssystems

**SI-4.e** Die Organisation stuft die Aktivitäten zur Überwachung des Informationssystems herauf, wenn Informationen aus Strafverfolgungsbehörden, Nachrichtendiensten oder anderen glaubwürdigen Informationsquellen auf ein erhöhtes Risiko für den Betrieb und die Ressourcen der Organisation, für Personen, für andere Organisationen oder für den Staat hinweisen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überwachung von kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-4f"></a>NIST 800-53, Regelung SI-4.f

#### <a name="information-system-monitoring"></a>Überwachung des Informationssystems

**SI-4.f** Die Organisation holt eine rechtliche Beurteilung in Bezug auf Aktivitäten zur Überwachung des Informationssystems in Übereinstimmung mit gültigen Gesetzen, Executive Orders (Verfügungen des US-Präsidenten), Direktiven, Richtlinien oder Bestimmungen ein.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überwachung von kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-4g"></a>NIST 800-53, Regelung SI-4.g

#### <a name="information-system-monitoring"></a>Überwachung des Informationssystems

**SI-4.g** Die Organisation stellt [Zuordnung: von der Organisation definierte Informationen zur Überwachung des Informationssystems] für [Zuordnung: von der Organisation definierte Personen oder Rollen] [Auswahl (mindestens ein Element): nach Bedarf; [Zuordnung: von der Organisation definierte Häufigkeit]] bereit.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überwachung von kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-1"></a>NIST 800-53, Regelung SI-4 (1)

#### <a name="information-system-monitoring--system-wide-intrusion-detection-system"></a>Überwachung des Informationssystems | Systemweites Angriffserkennungssystem

**SI-4 (1)** Die Organisation verbindet individuelle Angriffserkennungstools mit einem im gesamten Informationssystem bereitgestellten Angriffserkennungssystem, und konfiguriert diese.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überwachung von kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-2"></a>NIST 800-53, Regelung SI-4 (2)

#### <a name="information-system-monitoring--automated-tools-for-real-time-analysis"></a>Überwachung des Informationssystems | Automatisierte Tools für die Echtzeitanalyse

**SI-4 (2)** Die Organisation setzt automatisierte Tools ein, um die Analyse von Ereignissen nahezu in Echtzeit zu unterstützen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt die Log Analytics-Lösung und verschiedene OMS-Lösungen bereit (einschließlich der Sicherheits- und Überwachungslösung). Log Analytics bietet eine Analyse von Ereignissen nahezu in Echtzeit über bereitgestellte Ressourcen hinweg. OMS-Lösungen bieten einen umfassenden Überblick über den Sicherheitszustand in allen Lösungsdomänen. OMS gewährt Einblick in den Sicherheitszustand der bereitgestellten Ressourcen anhand von Daten, die in allen bereitgestellten OMS-Lösungen verfügbar sind. OMS kann so konfiguriert werden, dass basierend auf definierten Kriterien Warnungen generiert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-4"></a>NIST 800-53, Regelung SI-4 (4)

#### <a name="information-system-monitoring--inbound-and-outbound-communications-traffic"></a>Überwachung des Informationssystems | Ein- und ausgehender Kommunikationsdatenverkehr

**SI-4 (4)** Das Informationssystem überwacht den ein- und ausgehendem Kommunikationsdatenverkehr [Zuordnung: von der Organisation definierte Häufigkeit] auf ungewöhnliche oder nicht autorisierte Aktivitäten oder Bedingungen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überwachung von kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-5"></a>NIST 800-53, Regelung SI-4 (5)

#### <a name="information-system-monitoring--system-generated-alerts"></a>Überwachung des Informationssystems | Vom System generierte Warnungen

**SI-4 (5)** Das Informationssystem warnt [Zuordnung: von der Organisation definierte Personen oder Rollen], wenn die folgenden Anzeichen einer Kompromittierung oder potenziellen Kompromittierung auftreten: [Zuordnung: von der Organisation definierte Anzeichen für Kompromittierung].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt verschiedene OMS-Lösungen bereit (einschließlich der Sicherheits- und Überwachungslösung). Log Analytics bietet eine Analyse von Ereignissen nahezu in Echtzeit über bereitgestellte Ressourcen hinweg. OMS-Lösungen bieten einen umfassenden Überblick über den Sicherheitszustand in allen Lösungsdomänen. OMS kann so konfiguriert werden, dass basierend auf definierten Kriterien Warnungen generiert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-11"></a>NIST 800-53, Regelung SI-4 (11)

#### <a name="information-system-monitoring--analyze-communications-traffic-anomalies"></a>Überwachung des Informationssystems | Analysieren von Anomalien im Kommunikationsdatenverkehr

**SI-4 (11)** Die Organisation analysiert den ausgehenden Kommunikationsdatenverkehr an der externen Grenze des Informationssystems und ausgewählten [Zuordnung: von der Organisation definierte interne Punkte im System (z.B. Subnetzwerke, Subsysteme)], um Anomalien zu entdecken.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Analyse von Anomalien im Kommunikationsdatenverkehr für die kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-14"></a>NIST 800-53, Regelung SI-4 (14)

#### <a name="information-system-monitoring--wireless-intrusion-detection"></a>Überwachung des Informationssystems | Angriffserkennung in Drahtlosnetzwerken

**SI-4 (14)** Die Organisation setzt ein System zur Erkennung von Angriffen in Drahtlosnetzwerken ein, um nicht autorisierte drahtlose Geräte zu identifizieren und Angriffsversuche und potenzielle Kompromittierungen/Sicherheitsverletzungen im Informationssystem zu erkennen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | In Azure-Rechenzentren ist keine von Kunden gesteuerte Hardware erlaubt, einschließlich drahtloser Geräte. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure führt regelmäßig einmal pro Quartal eine Überwachung auf nicht autorisierte drahtlose Signale durch, wie in AC-18 erläutert. <br /> Microsoft Azure implementiert diese Regelung im Namen von PaaS- und IaaS-Kunden. |


 ### <a name="nist-800-53-control-si-4-16"></a>NIST 800-53, Regelung SI-4 (16)

#### <a name="information-system-monitoring--correlate-monitoring-information"></a>Überwachung des Informationssystems | Korrelieren der Überwachungsinformationen

**SI-4 (16)** Die Organisation korreliert Informationen aus Überwachungstools, die im gesamten Informationssystem eingesetzt werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt die Log Analytics-Lösung und verschiedene OMS-Lösungen bereit (einschließlich der Sicherheits- und Überwachungslösung). OMS gewährt Einblick in den Sicherheitszustand der bereitgestellten Ressourcen anhand von Daten, die in allen bereitgestellten OMS-Lösungen verfügbar sind. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-18"></a>NIST 800-53, Regelung SI-4 (18)

#### <a name="information-system-monitoring--analyze-traffic--covert-exfiltration"></a>Überwachung des Informationssystems | Analyse des Datenverkehrs / verdeckte Ausschleusung

**SI-4 (18)** Die Organisation analysiert den ausgehenden Kommunikationsdatenverkehr an der externen Grenze des Informationssystems (also dem Systemperimeter) und an [Zuordnung: von der Organisation definierte interne Punkte im System (z.B. Subnetzwerke, Subsysteme)], um eine verdeckte Ausschleusung von Informationen zu entdecken.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Analyse des Kommunikationsdatenverkehrs für die kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-19"></a>NIST 800-53, Regelung SI-4 (19)

#### <a name="information-system-monitoring--individuals-posing-greater-risk"></a>Überwachung des Informationssystems | Personen mit erhöhtem Risiko

**SI-4 (19)** Die Organisation implementiert [Zuordnung: von der Organisation definierte zusätzliche Überwachung] von Personen, die gemäß [Zuordnung: von der Organisation definierte Quellen] ein erhöhtes Risiko darstellen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überwachung von Personen verantwortlich, die ein erhöhtes Risiko darstellen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-20"></a>NIST 800-53, Regelung SI-4 (20)

#### <a name="information-system-monitoring--privileged-users"></a>Überwachung des Informationssystems | Privilegierte Benutzer

**SI-4 (20)** Die Organisation implementiert [Zuordnung: von der Organisation definierte zusätzliche Überwachung] privilegierter Benutzer.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überwachung von privilegierten Benutzern verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-22"></a>NIST 800-53, Regelung SI-4 (22)

#### <a name="information-system-monitoring--unauthorized-network-services"></a>Überwachung des Informationssystems | Nicht autorisierte Netzwerkdienste

**SI-4 (22)** Das Informationssystem erkennt Netzwerkdienste, die von [Zuordnung: von der Organisation definierte Autorisierungs- oder Genehmigungsprozesse] und [Auswahl (mindestens ein Element): Überwachungen; Warnungen [Zuordnung: von der Organisation definierte Personen oder Rollen]] nicht autorisiert oder genehmigt wurden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für das Erkennen von nicht autorisierten Netzwerkdiensten verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-23"></a>NIST 800-53, Regelung SI-4 (23)

#### <a name="information-system-monitoring--host-based-devices"></a>Überwachung des Informationssystems | Hostbasierte Geräte

**SI-4 (23)** Die Organisation implementiert [Zuordnung: von der Organisation definierte hostbasierte Überwachungsmechanismen] auf [Zuordnung: von der Organisation definierte Informationssystemkomponenten].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint erfasst Überwachungsdaten aus bereitgestellten Ressourcen. Dazu zählen auch Daten aus hostbasierten Überwachungsfunktionen. Microsoft Monitoring Agent wird auf allen virtuellen Windows-Computern installiert, um Überwachungsdaten zu erfassen, die von Log Analytics und anderen OMS-Lösungen verwendet werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-4-24"></a>NIST 800-53, Regelung SI-4 (24)

#### <a name="information-system-monitoring--indicators-of-compromise"></a>Überwachung des Informationssystems | Anzeichen einer Kompromittierung

**SI-4 (24)** Das Informationssystem ermittelt, erfasst, verteilt und verwendet Anzeichen einer Kompromittierung.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Ermittlung, Erfassung, Verteilung und Verwendung von Anzeichen einer Kompromittierung auf kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-5a"></a>NIST 800-53, Regelung SI-5.a

#### <a name="security-alerts-advisories-and-directives"></a>Sicherheitswarnungen, Empfehlungen und Direktiven

**SI-5.a** Die Organisation erhält fortlaufend von [Zuordnung: von der Organisation definierte externe Organisationen] Sicherheitswarnungen, Empfehlungen und Direktiven für das Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verwaltung von Sicherheitswarnungen, Empfehlungen und Direktiven für die kundenseitig bereitgestellten Ressourcen (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-5b"></a>NIST 800-53, Regelung SI-5.b

#### <a name="security-alerts-advisories-and-directives"></a>Sicherheitswarnungen, Empfehlungen und Direktiven

**SI-5.b** Die Organisation generiert interne Sicherheitswarnungen, Empfehlungen und Direktiven nach eigenem Ermessen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verwaltung von Sicherheitswarnungen, Empfehlungen und Direktiven für die kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-5c"></a>NIST 800-53, Regelung SI-5.c

#### <a name="security-alerts-advisories-and-directives"></a>Sicherheitswarnungen, Empfehlungen und Direktiven

**SI-5.c** Die Organisation verteilt Sicherheitswarnungen, Empfehlungen und Direktiven an: [Auswahl (mindestens ein Element): [Zuordnung: von der Organisation definierte Personen oder Rollen], [Zuordnung: von der Organisation definierte Elemente innerhalb der Organisation], [Zuordnung: von der Organisation definierte externe Organisationen]].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verwaltung von Sicherheitswarnungen, Empfehlungen und Direktiven für die kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-5d"></a>NIST 800-53, Regelung SI-5.d

#### <a name="security-alerts-advisories-and-directives"></a>Sicherheitswarnungen, Empfehlungen und Direktiven

**SI-5.d** Die Organisation implementiert Sicherheitsdirektiven gemäß bestehender Zeitrahmen oder benachrichtigt die ausstellende Organisation über das Maß der Nichteinhaltung.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verwaltung von Sicherheitswarnungen, Empfehlungen und Direktiven für die kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-5-1"></a>NIST 800-53, Regelung SI-5 (1)

#### <a name="security-alerts-advisories-and-directives--automated-alerts-and-advisories"></a>Sicherheitswarnungen, Empfehlungen und Direktiven | Automatisierte Warnungen und Empfehlungen

**SI-5 (1)** Die Organisation setzt automatisierte Mechanismen ein, um Informationen aus Sicherheitswarnungen und Empfehlungen in der gesamten Organisation verfügbar zu machen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verwaltung von Sicherheitswarnungen, Empfehlungen und Direktiven für die kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-6a"></a>NIST 800-53, Regelung SI-6.a

#### <a name="security-function-verification"></a>Überprüfung von Sicherheitsfunktionen

**SI-6.a** Das Informationssystem überprüft den ordnungsgemäßen Betrieb von [Zuordnung: von der Organisation definierte Sicherheitsfunktionen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überprüfung der Sicherheitsfunktionen für kundenseitig bereitgestellte Ressourcen (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-6b"></a>NIST 800-53, Regelung SI-6.b

#### <a name="security-function-verification"></a>Überprüfung von Sicherheitsfunktionen

**SI-6.b** Das Informationssystem führt diese Überprüfung [Auswahl (mindestens ein Element): [Zuordnung: von der Organisation definierte Systemübergangszustände] auf Befehl eines Benutzers mit geeigneter Berechtigung [Zuordnung: von der Organisation definierte Häufigkeit]] aus.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überprüfung der Sicherheitsfunktionen für kundenseitig bereitgestellte Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-6c"></a>NIST 800-53, Regelung SI-6.c

#### <a name="security-function-verification"></a>Überprüfung von Sicherheitsfunktionen

**SI-6.c** Das Informationssystem benachrichtigt [Zuordnung: von der Organisation definierte Personen oder Rollen] bei nicht erfolgreich ausgeführten Sicherheitsüberprüfungstests.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überprüfung der Sicherheitsfunktionen für kundenseitig bereitgestellte Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-6d"></a>NIST 800-53, Regelung SI-6.d

#### <a name="security-function-verification"></a>Überprüfung von Sicherheitsfunktionen

**SI-6.d** Das Informationssystem [Auswahl (mindestens ein Element): fährt das Informationssystem herunter; startet das Informationssystem neu; [Zuordnung: von der Organisation definierte alternative Aktion(en)]], wenn Anomalien entdeckt werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überprüfung der Sicherheitsfunktionen für kundenseitig bereitgestellte Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-7"></a>NIST 800-53, Regelung SI-7

#### <a name="software-firmware-and-information-integrity"></a>Integrität von Software, Firmware und Informationen

**SI-7** Die Organisation setzt Tools zur Überprüfung der Integrität ein, um nicht autorisierte Änderungen an [Zuordnung: von der Organisation definierte Software, Firmware und Informationen] zu erkennen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer führen Windows-Betriebssysteme aus. Windows bietet Echtzeitüberprüfung, -schutz und -wiederherstellung der Dateiintegrität von Systemkerndateien, die als Teil von Windows oder im Rahmen von autorisierten Windows-Systemaktualisierungen über die WRP-Funktion (Windows Resource Protection) installiert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-7-1"></a>NIST 800-53, Regelung SI-7 (1)

#### <a name="software-firmware-and-information-integrity--integrity-checks"></a>Integrität von Software, Firmware und Informationen | Integritätsüberprüfungen

**SI-7 (1)** Das Informationssystem führt [Zuordnung: von der Organisation definierte Häufigkeit] eine Integritätsüberprüfung von [Zuordnung: von der Organisation definierte Software, Firmware und Informationen] [Auswahl (mindestens ein Element): bei Systemstart; bei [Zuordnung: von der Organisation definierte Übergangszustände oder sicherheitsrelevante Ereignisse]] durch.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer führen Windows-Betriebssysteme aus. Windows bietet Echtzeitüberprüfung, -schutz und -wiederherstellung der Dateiintegrität von Systemkerndateien, die als Teil von Windows oder im Rahmen von autorisierten Windows-Systemaktualisierungen über die WRP-Funktion (Windows Resource Protection) installiert werden. WRP ermöglicht eine Integritätsüberprüfung in Echtzeit. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-7-2"></a>NIST 800-53, Regelung SI-7 (2)

#### <a name="software-firmware-and-information-integrity--automated-notifications-of-integrity-violations"></a>Integrität von Software, Firmware und Informationen | Automatisierte Benachrichtigungen bei Integritätsverletzungen

**SI-7 (2)** Die Organisation setzt automatisierte Tools ein, die bei Entdeckung von Diskrepanzen bei der Integritätsüberprüfung Benachrichtigungen an [Zuordnung: von der Organisation definierte Personen oder Rollen] senden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer führen Windows-Betriebssysteme aus. Windows bietet Echtzeitüberprüfung, -schutz und -wiederherstellung der Dateiintegrität von Systemkerndateien, die als Teil von Windows oder im Rahmen von autorisierten Windows-Systemaktualisierungen über die WRP-Funktion (Windows Resource Protection) installiert werden.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-7-5"></a>NIST 800-53, Regelung SI-7 (5)

#### <a name="software-firmware-and-information-integrity--automated-response-to-integrity-violations"></a>Integrität von Software, Firmware und Informationen | Automatisierte Reaktion bei Integritätsverletzungen

**SI-7 (5)** Das Informationssystem [Auswahl (mindestens ein Element): fährt das Informationssystem herunter; startet das Informationssystem neu; implementiert [Zuordnung: von der Organisation definierte Sicherheitsmaßnahmen]], wenn Integritätsverletzungen entdeckt werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die automatische Reaktion bei Integritätsverletzungen in kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-7-7"></a>NIST 800-53, Regelung SI-7 (7)

#### <a name="software-firmware-and-information-integrity--integration-of-detection-and-response"></a>Integrität von Software, Firmware und Informationen | Integration von Erkennung und Reaktion

**SI-7 (7)** Die Organisation integriert die Erkennung von nicht autorisierten [Zuordnung: von der Organisation definierte sicherheitsrelevante Änderungen am Informationssystem] in die Organisationsfunktionen für die Reaktion auf Incidents.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Schutz der Software- und Informationsintegrität für kundenseitig bereitgestellte Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-7-14"></a>NIST 800-53, Regelung SI-7 (14)

#### <a name="software-firmware-and-information-integrity--binary-or-machine-executable-code"></a>Integrität von Software, Firmware und Informationen | Binärcode oder ausführbarer Maschinencode

**SI-7 (14)** Die Organisation verbietet die Verwendung von Binärcode oder ausführbarem Maschinencode von Quellen mit eingeschränkter Garantie oder ohne Garantie und ohne Bereitstellung von Quellcode; sie erteilt zudem Ausnahmen der Quellcodevoraussetzung nur für zwingend notwendige geschäftskritische/betriebliche Anforderungen und mit Genehmigung des autorisierenden Organs.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Verfahren des Kunden für die System- und Informationsintegrität auf Unternehmensebene schreiben möglicherweise vor, dass der Quellcode für Binärcode oder ausführbaren Maschinencode bei einigen Quellen abgerufen werden muss. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-8a"></a>NIST 800-53, Regelung SI-8.a

#### <a name="spam-protection"></a>Spamschutz

**SI-8.a** Die Organisation setzt an den Ein- und Ausgangspunkten des Informationssystems Mechanismen für den Spamschutz ein, um unerwünschte Nachrichten zu erkennen und geeignete Maßnahmen zu ergreifen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Im Rahmen dieses Blueprints werden keine Mailserver bereitgestellt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-8b"></a>NIST 800-53, Regelung SI-8.b

#### <a name="spam-protection"></a>Spamschutz

**SI-8.b** Die Organisation aktualisiert die Spamschutzmechanismen gemäß den Konfigurationsverwaltungsrichtlinien und -verfahren der Organisation, wenn neue Releases verfügbar sind.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Im Rahmen dieses Blueprints werden keine Mailserver bereitgestellt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-8-1"></a>NIST 800-53, Regulierung SI-8 (1)

#### <a name="spam-protection--central-management"></a>Spamschutz | Zentrale Verwaltung

**SI-8 (1)** Die Organisation verwaltet Spamschutzmechanismen zentral.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Im Rahmen dieses Blueprints werden keine Mailserver bereitgestellt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-si-8-2"></a>NIST 800-53, Regulierung SI-8 (2)

#### <a name="spam-protection--automatic-updates"></a>Spamschutz | Automatische Updates

**SI-8 (2)** Das Informationssystem aktualisiert Spamschutzmechanismen automatisch.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Im Rahmen dieses Blueprints werden keine Mailserver bereitgestellt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-10"></a>NIST 800-53, Regelung SI-10

#### <a name="information-input-validation"></a>Überprüfen von Informationseingaben

**SI-10** Das Informationssystem überprüft die Gültigkeit von [Zuordnung: von der Organisation definierte Informationseingaben].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Überprüfung der Informationseingaben für kundenseitig bereitgestellte Ressourcen (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-11a"></a>NIST 800-53, Regelung SI-11.a

#### <a name="error-handling"></a>Fehlerbehandlung

**SI-11.a** Das Informationssystem generiert Fehlermeldungen, die für Korrekturmaßnahmen erforderliche Informationen bereitstellen, ohne Informationen offenzulegen, die von Angreifern ausgenutzt werden könnten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten Ressourcen nutzen kommerzielle Betriebssysteme und Softwareanwendungen. Diese Software verwendet Best Practices der Branche, um sicherzustellen, dass in Fehlermeldungen keine vertraulichen Daten offengelegt werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-11b"></a>NIST 800-53, Regelung SI-11.b

#### <a name="error-handling"></a>Fehlerbehandlung

**SI-11.b** Das Informationssystem legt Fehlermeldungen nur für [Zuordnung: von der Organisation definierte Personen oder Rollen] offen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten Ressourcen nutzen kommerzielle Betriebssysteme und Softwareanwendungen. Diese Software verwendet Best Practices der Branche, um Fehlermeldungen bereitzustellen, die für den Kontext des Benutzers geeignet sind, der die Meldung erhält. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-12"></a>NIST 800-53, Regelung SI-12

#### <a name="information-handling-and-retention"></a>Informationsverarbeitung und -aufbewahrung

**SI-12** Die Organisation verarbeitet und speichert Informationen im Informationssystem sowie die Informationsausgabe aus dem System in Übereinstimmung mit gültigen Gesetzen, Executive Orders (Verfügungen des US-Präsidenten), Direktiven, Richtlinien, Bestimmungen, Standards und betrieblichen Erfordernissen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verarbeitung und Aufbewahrung von Informationen in den kundenseitig bereitgestellten Ressourcen (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) sowie für die Informationsausgabe von diesen Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-si-16"></a>NIST 800-53, Regulierung SI-16

#### <a name="memory-protection"></a>Arbeitsspeicherschutz

**SI-16** Das Informationssystem implementiert [Zuordnung: von der Organisation definierte Sicherheitsmaßnahmen], um den Arbeitsspeicher vor der nicht autorisierten Ausführung von Code zu schützen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer führen Windows-Betriebssysteme aus. In Windows sind Schutzmechanismen implementiert, um die Codeausführung in eingeschränkten Arbeitsspeicherbereichen zu verhindern: No Execute (NX), zufällige Anordnung des Adressraumlayouts (Address Space Layout Randomization, ASLR) und Datenausführungsverhinderung (Data Execution Prevention, DEP). |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |
