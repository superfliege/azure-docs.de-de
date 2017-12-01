---
title: "FedRAMP Azure Blueprint Automation – Überwachung und Verantwortlichkeit"
description: "Webanwendungen für FedRAMP – Überwachung und Verantwortlichkeit"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: c5858e07-ca74-4526-b31f-e6b4e55bb594
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 83ef9cbb7652bf128d7758237a8e6fbeed6c6565
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2017
---
# <a name="audit-and-accountability-au"></a>Überwachung und Verantwortlichkeit (AU)

> [!NOTE]
> Diese Regelungen werden von NIST und dem U.S. Department of Commerce als Teil der NIST-Sonderveröffentlichung 800-53 Revision 4 definiert. Informationen zu Testverfahren und Anleitungen für jede Regelung finden Sie unter NIST 800-53 Rev. 4.

## <a name="nist-800-53-control-au-1"></a>NIST 800-53, Regelung AU-1

#### <a name="audit-and-accountability-policy-and-procedures"></a>Überwachung und Verantwortlichkeit – Richtlinien und Verfahren

**AU-1** Die Organisation entwickelt, dokumentiert und verteilt an [Zuordnung: von der Organisation definierte Personen oder Rollen] eine Überwachungs- und Verantwortlichkeitsrichtlinie, die sich mit Zweck, Umfang, Rollen, Zuständigkeiten, Managementverpflichtung, Koordination zwischen Organisationsentitäten und Compliance befasst, stellt Verfahren zum Ermöglichen der Implementierung der Überwachungs- und Verantwortlichkeitsrichtlinie und der damit verbundenen Überwachungs- und Verantwortlichkeitsregelungen bereit; und überprüft und aktualisiert die aktuelle Überwachungs- und Verantwortlichkeitsrichtlinie [Zuordnung: von der Organisation definierte Häufigkeit] sowie die Überwachungs- und Verantwortlichkeitsverfahren [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Überwachungs- und Verantwortlichkeitsrichtlinien und -verfahren des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-2a"></a>NIST 800-53, Regelung AU-2.a

#### <a name="audit-events"></a>Überwachen von Ereignissen

**AU-2.a** Die Organisation bestimmt, dass das Informationssystem zur Überwachung der folgenden Ereignisse fähig ist: [Zuordnung: von der Organisation definierte überwachbare Ereignisse].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Überwachungsfunktion für diesen Azure-Blueprint wird vom Azure Monitor- und Log Analytics-Dienst in der OMS bereitgestellt. Azure Monitor stellt detaillierte Überwachungsprotokolle zu Aktivitäten im Zusammenhang mit den bereitgestellten Ressourcen zur Verfügung. Diese Protokolle und Protokolle auf Betriebssystemebene werden von Log Analytics gesammelt und im OMS-Repository gespeichert. Log Analytics korreliert Überwachungsdaten für die von dieser Lösung bereitgestellten Ressourcen und kann auf die kundenseitig bereitgestellte Webanwendung erweitert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-2b"></a>NIST 800-53, Regelung AU-2.b

#### <a name="audit-events"></a>Überwachen von Ereignissen

**AU-2.b** Die Organisation koordiniert die Sicherheitsüberwachungsfunktion mit anderen Organisationsentitäten, die zur besseren gegenseitigen Unterstützung und Auswahl der überwachbaren Ereignisse überwachungsbezogene Informationen benötigen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde kann einen etablierten Prozess auf Unternehmensebene zur Ermittlung von überwachbaren Ereignissen anwenden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-2c"></a>NIST 800-53, Regelung AU-2.c

#### <a name="audit-events"></a>Überwachen von Ereignissen

**AU-2.c** Die Organisation stellt eine Begründung bereit, warum sich die überwachbaren Ereignisse für nachträgliche Untersuchungen von Sicherheitsvorfällen eignen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Azure-Blueprint überwachten Ereignisse beinhalten ausreichende Informationen, um den Zeitpunkt des Eintretens von Ereignissen, die Quelle des Ereignisses, das Ergebnis des Ereignisses und andere ausführliche Informationen zu bestimmen, die zur Untersuchung von Sicherheitsvorfällen dienen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-2d"></a>NIST 800-53, Regelung AU-2.d

#### <a name="audit-events"></a>Überwachen von Ereignissen

**AU-2.d** Die Organisation bestimmt, dass die folgenden Ereignisse innerhalb des Informationssystems zu überwachen sind: [Zuordnung: von der Organisation definierte Ereignisse (die Teilmenge der in AU-2.a definierten überwachbaren Ereignisse) sowie die Häufigkeit der Überwachung (oder die eine Überwachung erfordernde Situation) für jedes identifizierte Ereignis].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Azure-Blueprint überwachten Ereignisse beinhalten diejenigen, die von Azure-Aktivitätsprotokollen für bereitgestellte Ressourcen, Protokollen auf Betriebssystemebene, Active Directory-Protokollen und SQL Server-Protokollen erfasst werden. Kunden können außerdem weitere Ereignisse für die Überwachung auswählen, um Unternehmensanforderungen zu entsprechen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-2-3"></a>NIST 800-53, Regelung AU-2 (3)

#### <a name="audit-events--reviews-and-updates"></a>Überwachen von Ereignissen | Prüfungen und Updates

**AU-2 (3)** Die Organisation prüft und aktualisiert die überwachten Ereignisse [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde kann eine regelmäßige Prüfung auf Unternehmensebene und einen entsprechenden Updateprozess für die definierte Gruppe von überwachten Ereignissen einrichten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-3"></a>NIST 800-53, Regelung AU-3

#### <a name="content-of-audit-records"></a>Inhalt der Überwachungsdatensätze

**AU-3** Das Informationssystem generiert Überwachungsdatensätze, die folgende Informationen enthalten: Art des aufgetretenen Ereignisses, Zeitpunkt des Auftretens des Ereignisses, Ort des Auftretens des Ereignisses, Quelle des Ereignisses, Ergebnis des Ereignisses und Identität von Personen im Zusammenhang mit dem Ereignis.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint basiert auf integrierten Überwachungsfunktionen von Azure, Windows Server und SQL Server. Diese Überwachungslösungen erfassen Überwachungsdatensätze mit ausreichender Detailgenauigkeit, um die Anforderungen für diese Regelung zu erfüllen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-3-1"></a>NIST 800-53, Regelung AU-3 (1)

#### <a name="content-of-audit-records--additional-audit-information"></a>Inhalt der Überwachungsdatensätze | Zusätzliche Überwachungsinformationen

**AU-3 (1)** Das Informationssystem generiert Überwachungsdatensätze, die folgende zusätzliche Informationen enthalten: [Zuordnung: von der Organisation definierte zusätzliche, ausführlichere Informationen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Azure-Aktivitätsprotokollereignisse verwenden ein ausführliches Schema, das Felder für mehr als 20 Typen von Überwachungsinformationen enthält. Neben Aktivitätsprotokollen stellt dieser Azure-Blueprint die Log Analytics-Lösung in der OMS bereit, die unterschiedliche Datenquellen wie Windows-, Linux- und Azure-Diagnoseprotokolle sowie kundenspezifische Protokolle unterstützt.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-3-2"></a>NIST 800-53, Regelung AU-3 (2)

#### <a name="content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>Inhalt der Überwachungsdatensätze | Zentrale Verwaltung von Inhalten geplanter Überwachungsdatensätze

**AU-3 (2)** Das Informationssystem ermöglicht eine zentrale Verwaltung und Konfiguration der zu erfassenden Inhalte in Überwachungsdatensätzen, die von [Zuordnung: von der Organisation definierte Informationssystemkomponenten] generiert werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Alle von diesem Azure-Blueprint bereitgestellten virtuellen Computer werden in die bereitgestellte Active Directory-Domäne eingebunden. Alle in Domänen eingebundenen virtuellen Computer implementieren eine Gruppenrichtlinie, die für die zentrale Verwaltung der Konfiguration des Überwachungssystems auf Betriebssystemebene konfiguriert werden kann. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-4"></a>NIST 800-53, Regelung AU-4

#### <a name="audit-storage-capacity"></a>Speicherkapazität für Überwachungsdatensätze

**AU-4** Die Organisation ordnet Speicherkapazitäten für Überwachungsdatensätze in Übereinstimmung mit [Zuordnung: von der Organisation definierte Aufbewahrungsanforderungen für Überwachungsdatensätze] zu.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint ordnet ausreichende Speicherkapazitäten zu, um Überwachungsdatensätze für einen Zeitraum von einem Jahr aufzubewahren. Alle Überwachungsdatensätze werden vom Log Analytics-Dienst gesammelt, der für einen Aufbewahrungszeitraum von einem Jahr konfiguriert ist. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-5a"></a>NIST 800-53, Regelung AU-5.a

#### <a name="response-to-audit-processing-failures"></a>Reaktion auf Verarbeitungsfehler bei Überwachungsinformationen

**AU-5.a** Das Informationssystem benachrichtigt [Zuordnung: von der Organisation definiertes Personal oder Rollen] im Falle eines Verarbeitungsfehlers bei Überwachungsinformationen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Dienststatus für Azure Monitor und Log Analytics ist auf der Website mit dem Azure-Status und im Azure-Portal auf dem Blatt „Dienstintegrität“ einsehbar. Über Log Analytics kann die Bereitstellung von Benachrichtigungen für andere Typen von Verarbeitungsfehlern bei Überwachungsinformationen konfiguriert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-5b"></a>NIST 800-53, Regelung AU-5.b

#### <a name="response-to-audit-processing-failures"></a>Reaktion auf Verarbeitungsfehler bei Überwachungsinformationen

**AU-5.b** Das Informationssystem führt die folgenden zusätzlichen Aktionen durch: [Zuordnung: von der Organisation definierte durchzuführende Aktionen (z.B. Herunterfahren des Informationssystems, Überschreiben der ältesten Überwachungsdatensätze, Beenden der Generierung von Überwachungsdatensätzen)].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Alle Überwachungsdatensätze, die von den durch diesen Azure-Blueprint generierten Ressourcen bereitgestellt werden, werden von Log Analytics gesammelt und für einen Zeitraum von einem Jahr aufbewahrt. Die Speicherbelegung für die Aufbewahrung dieser Überwachungsdatensätze wird dynamisch zugeordnet, um sicherzustellen, dass genügend Kapazitäten vorhanden sind. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-5-1"></a>NIST 800-53, Regelung AU-5 (1)

#### <a name="response-to-audit-processing-failures--audit-storage-capacity"></a>Reaktion auf Verarbeitungsfehler bei Überwachungsinformationen | Speicherkapazitäten für Überwachungsdatensätze

**AU-5 (1)** Das Informationssystem gibt eine Benachrichtigung für [Zuordnung: von der Organisation definierte Mitarbeiter, Rollen und/oder Speicherorte] innerhalb von [Zuordnung: von der Organisation definierter Zeitraum] aus, wenn das zugeordnete Speichervolume für Überwachungsdatensätze [Zuordnung: von der Organisation definierter Prozentsatz] der maximalen Repositoryspeicherkapazität für Überwachungsdatensätze erreicht.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Alle Überwachungsdatensätze, die von den durch diesen Azure-Blueprint generierten Ressourcen bereitgestellt werden, werden von Log Analytics gesammelt und für einen Zeitraum von einem Jahr aufbewahrt. Die Speicherbelegung für die Aufbewahrung dieser Überwachungsdatensätze wird dynamisch zugeordnet, um sicherzustellen, dass genügend Kapazitäten vorhanden sind. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-5-2"></a>NIST 800-53, Regelung AU-5 (2)

#### <a name="response-to-audit-processing-failures--real-time-alerts"></a>Reaktion auf Verarbeitungsfehler bei Überwachungsinformationen | Benachrichtigungen in Echtzeit

**AU-5 (2)** Das Informationssystem generiert innerhalb von [Zuordnung: von der Organisation definierter realer Zeitraum] eine Benachrichtigung für [Zuordnung: von der Organisation definierte Mitarbeiter, Rollen und/oder Speicherorte], wenn folgende Überwachungsfehlerereignisse auftreten: [Zuordnung: von der Organisation definierte Überwachungsfehlerereignisse, die Benachrichtigungen in Echtzeit erfordern].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Dienststatus für Azure ist im Azure-Portal auf dem Blatt „Dienstintegrität“ einsehbar. Über Log Analytics kann die Bereitstellung von Benachrichtigungen für andere Typen von Verarbeitungsfehlern bei Überwachungsinformationen konfiguriert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-6a"></a>NIST 800-53, Regelung AU-6.a

#### <a name="audit-review-analysis-and-reporting"></a>Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung

**AU-6.a** Die Organisation prüft und analysiert die Überwachungsdatensätze des Informationssystems [Zuordnung: von der Organisation definierte Häufigkeit] auf Hinweise auf [Zuordnung: von der Organisation definierte unangemessene oder ungewöhnliche Aktivitäten].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Prüfung und Analyse der kundenseitig bereitgestellten Ressourcen (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-6b"></a>NIST 800-53, Regelung AU-6.b

#### <a name="audit-review-analysis-and-reporting"></a>Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung

**AU-6.b** Die Organisation meldet die Ergebnisse [Zuordnung: von der Organisation definierte Mitarbeiter oder Rollen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Meldung von Ergebnissen bezüglich unangemessener oder ungewöhnlicher Aktivitäten (definiert in AU-06.a) zu den kundenseitig bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-6-1"></a>NIST 800-53, Regelung AU-6 (1)

#### <a name="audit-review-analysis-and-reporting--process-integration"></a>Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung | Prozessintegration

**AU-6 (1)** Die Organisation wendet automatisierte Mechanismen zur Integration von Prüf-, Analyse- und Berichterstellungsprozessen für die Überwachung an, um Organisationsprozesse zur Untersuchung und Behandlung verdächtiger Aktivitäten zu unterstützen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde kann zentrale Prüf-, Analyse- und Berichterstellungsfunktionen für die Überwachung auf Unternehmensebene anwenden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-6-3"></a>NIST 800-53, Regelung AU-6 (3)

#### <a name="audit-review-analysis-and-reporting--correlate-audit-repositories"></a>Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung | Korrelieren von Überwachungsrepositorys

**AU-6 (3)** Die Organisation analysiert und korreliert Überwachungsdatensätze für verschiedene Repositorys, um Informationen zur organisationsweiten Situation zu erhalten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint implementiert die Log Analytics-Lösung in der OMS, um Überwachungsdaten für bereitgestellte Ressourcen für Informationen zur organisationsweiten Situation zu zentralisieren. Kunden können Log Analytics bei Bedarf in andere Systeme integrieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-6-4"></a>NIST 800-53, Regelung AU-6 (4)

#### <a name="audit-review-analysis-and-reporting--central-review-and-analysis"></a>Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung | Zentrale Überprüfung und Analyse

**AU-6 (4)** Das Informationssystem bietet die Möglichkeit, Überwachungsdatensätze aus verschiedenen Komponenten innerhalb des Systems zentral zu überprüfen und zu analysieren.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint implementiert die Log Analytics-Lösung in der OMS, um Überwachungsdaten von bereitgestellten Ressourcen für eine zentrale Prüfung, Analyse und Berichterstellung zu zentralisieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-6-5"></a>NIST 800-53, Regelung AU-6 (5)

#### <a name="audit-review-analysis-and-reporting--integration--scanning-and-monitoring-capabilities"></a>Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung | Integrations-, Scan- und Überwachungsfunktionen

**AU-6 (5)** Die Organisation integriert die Analyse der Überwachungsdatensätze in die Analyse von [Auswahl (mindestens ein Element): Informationen zur Überprüfung der Sicherheitsrisiken; Leistungsdaten; Überwachungsinformationen des Informationssystems; [Zuordnung: von der Organisation definierte Daten/Informationen aus anderen Quellen]], um die Möglichkeit zur Identifizierung unangemessener oder ungewöhnlicher Aktivitäten zu verbessern.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint stellt die Sicherheits- und Überwachungslösung OMS bereit. Diese Lösung bietet einen umfassenden Überblick über den Sicherheitszustand. Das Sicherheits- und Überwachungsdashboard gewährt Einblick in den Sicherheitszustand der bereitgestellten Ressourcen. Hierfür werden die in den bereitgestellten OMS-Lösungen verfügbaren Daten herangezogen, die Protokolldaten und Daten zu den Sicherheitsrisiken aus der Bewertung anhand von Baselines und Patches integrieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-6-6"></a>NIST 800-53, Regelung AU-6 (6)

#### <a name="audit-review-analysis-and-reporting--correlation-with-physical-monitoring"></a>Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung | Korrelation mit der physischen Überwachung

**AU-6 (6)** Die Organisation korreliert Informationen aus Überwachungsdatensätzen mit Informationen aus der Überwachung des physischen Zugriffs, um die Erkennung von verdächtigen, unangemessenen, ungewöhnlichen oder böswilligen Aktivitäten weiter zu verbessern.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Das Microsoft Azure-SIM-Team verwendet die jeweiligen physischen Überwachungsdaten und korreliert diese mit Überwachungsdatensätzen, um festzustellen, ob bei der Erkennung physischer Vorfälle damit im Zusammenhang stehende logische Verletzungen oder verdächtiges Verhalten stattgefunden haben. |


 ### <a name="nist-800-53-control-au-6-7"></a>NIST 800-53, Regelung AU-6 (7)

#### <a name="audit-review-analysis-and-reporting--permitted-actions"></a>Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung | Zulässige Aktionen

**AU-6 (7)** Die Organisation gibt die zulässigen Aktionen für die einzelnen [Auswahl (mindestens ein Element): Prozesse des Informationssystems; Rollen; Benutzer] im Zusammenhang mit der Überprüfung und Analyse von Überwachungsinformationen sowie der Berichterstellung an.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die von diesem Azure-Blueprint bereitgestellten virtuellen Windows-Computer implementieren Berechtigungen auf Betriebssystemebene, die die für einen Benutzer in Bezug auf Überwachungsinformationen verfügbaren Aktionen beschränken. In Azure können Benutzern oder Benutzergruppen Rollen (z.B. Besitzer, Mitwirkender, Leser oder eine benutzerdefinierte Rolle) zugewiesen werden, um die verfügbaren Aktionen in Bezug auf alle Ressourcen oder bereitgestellten Lösungen wie Log Analytics zu beschränken.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-6-10"></a>NIST 800-53, Regelung AU-6 (10)

#### <a name="audit-review-analysis-and-reporting--audit-level-adjustment"></a>Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung | Anpassung der Überwachungsebene

**AU-6 (10)** Die Organisation passt die Ebene der Prüfung, Analyse und Berichterstellung für die Überwachung innerhalb des Informationssystems an, wenn sich basierend auf Informationen von Justizbehörden, Geheimdiensten oder anderen glaubwürdigen Quellen eine Änderung am Risiko ergibt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Anpassung der Ebene der Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung für die kundenseitig bereitgestellten Ressourcen (bestehend aus Anwendungen, Betriebssysteme, Datenbanken und Software) verantwortlich, wenn sich eine Änderung am Risiko auf Basis der Informationen, die von Justizbehörden, Geheimdiensten oder anderen glaubwürdigen Quellen, ergibt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-7a"></a>NIST 800-53, Regelung AU-7.a

#### <a name="audit-reduction-and-report-generation"></a>Überwachungsreduzierung und Berichterstellung

**AU-7.a** Das Informationssystem bietet eine Überwachungsreduzierung und Berichterstellungsfunktion, die Anforderungen in Bezug auf die bedarfsgesteuerte Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung erfüllt sowie nachträgliche Untersuchungen von Sicherheitsvorfällen unterstützt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint implementiert die Log Analytics-Lösung in der OMS. Log Analytics sammelt Daten von verwalteten Ressourcen in einem zentralen Repository und stellt dadurch Überwachungsdienste für die OMS bereit. Die gesammelten Daten können für Warnungen und Analysen genutzt und exportiert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-7b"></a>NIST 800-53, Regelung AU-7.b

#### <a name="audit-reduction-and-report-generation"></a>Überwachungsreduzierung und Berichterstellung

**AU-7.b** Das Informationssystem stellt Funktionen zur Überwachungsreduzierung und Berichterstellung bereit, die nicht den ursprünglichen Inhalt oder die zeitliche Reihenfolge der Überwachungsdatensätze ändern.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint implementiert die Log Analytics-Lösung in der OMS. Log Analytics sammelt Daten von verwalteten Ressourcen in einem zentralen Repository und stellt dadurch Überwachungsdienste für die OMS bereit. Der Inhalt und die zeitliche Reihenfolge der Überwachungsdatensätze werden bei der Sammlung durch Log Analytics nicht geändert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-7-1"></a>NIST 800-53, Regelung AU-7 (1)

#### <a name="audit-reduction-and-report-generation--automatic-processing"></a>Überwachungsreduzierung und Berichterstellung | Automatische Verarbeitung

**AU-7 (1)** Das Informationssystem bietet die Möglichkeit, Überwachungsdatensätze für relevante Ereignisse basierend auf [Zuordnung: von der Organisation definierte Überwachungsfelder in Überwachungsdatensätzen] zu verarbeiten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint implementiert die Log Analytics-Lösung in der OMS. Log Analytics sammelt Daten von verwalteten Ressourcen in einem zentralen Repository und stellt dadurch Überwachungsdienste für die OMS bereit. Die gesammelten Daten können für Warnungen und Analysen genutzt und exportiert werden. Log Analytics verfügt über eine leistungsstarke Abfragesprache, um im Repository gespeicherte Daten zu extrahieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-8a"></a>NIST 800-53, Regelung AU-8.a

#### <a name="time-stamps"></a>Zeitstempel

**AU-8.a** Das Informationssystem verwendet interne Systemuhren zum Generieren von Zeitstempeln für Überwachungsdatensätze.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Bei den Ressourcen, die von diesem Azure-Blueprint bereitgestellt werden, werden zum Generieren von Zeitstempeln für Überwachungsdatensätze interne Systemuhren verwendet. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-8b"></a>NIST 800-53, Regelung AU-8.b

#### <a name="time-stamps"></a>Zeitstempel

**AU-8.b** Das Informationssystem erfasst Zeitstempel für Überwachungsdatensätze, die sich der koordinierten Weltzeit (UTC) oder Greenwich Mean Time (GMT) zuordnen lassen und in [Zuordnung: von der Organisation definierten Granularität der Zeitmessung] angezeigt werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Bei den Ressourcen, die von diesem Azure-Blueprint bereitgestellt werden, werden zum Generieren von Zeitstempeln für Überwachungsdatensätze interne Systemuhren verwendet. Zeitstempel werden in UTC erfasst. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-8-1a"></a>NIST 800-53, Regelung AU-8 (1).a

#### <a name="time-stamps--synchronization-with-authoritative-time-source"></a>Zeitstempel | Synchronisierung mit einer verbindlichen Zeitquelle

**AU-8 (1).a** Das Informationssystem gleicht die internen Uhren des Informationssystems [Zuordnung: von der Organisation definierte Häufigkeit] mit [Zuordnung: von der Organisation definierte verbindliche Zeitquelle] ab.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Bei den Ressourcen, die von diesem Azure-Blueprint bereitgestellt werden, werden zum Generieren von Zeitstempeln für Überwachungsdatensätze interne Systemuhren verwendet. Interne Systemuhren sind für die Synchronisierung mit einer verbindlichen Zeitquelle konfiguriert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-8-1b"></a>NIST 800-53, Regelung AU-8 (1).b

#### <a name="time-stamps--synchronization-with-authoritative-time-source"></a>Zeitstempel | Synchronisierung mit einer verbindlichen Zeitquelle

**AU-8 (1).b** Das Informationssystem synchronisiert die internen Systemuhren mit der verbindlichen Zeitquelle, wenn der Zeitunterschied größer ist als [Zuordnung: von der Organisation definierter Zeitraum].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Bei den Ressourcen, die von diesem Azure-Blueprint bereitgestellt werden, werden zum Generieren von Zeitstempeln für Überwachungsdatensätze interne Systemuhren verwendet. Interne Systemuhren sind für die Synchronisierung mit einer verbindlichen Zeitquelle konfiguriert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-9"></a>NIST 800-53, Regelung AU-9

#### <a name="protection-of-audit-information"></a>Schutz von Überwachungsinformationen

**AU-9** Das Informationssystem schützt Überwachungsinformationen und -tools vor nicht autorisierten Zugriffen, Änderungen und Löschungen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Logische Zugriffskontrollen dienen dem Schutz von Überwachungsinformationen und -tools in diesem Azure-Blueprint vor nicht autorisiertem Zugriff, Änderungen und Löschungen. Azure Active Directory erzwingt einen genehmigten logischen Zugriff mithilfe von rollenbasierten Gruppenmitgliedschaften. Die Möglichkeit zum Anzeigen von Überwachungsinformationen und Verwenden von Überwachungstools kann auf Benutzer beschränkt werden, die diese Berechtigungen benötigen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-9-2"></a>NIST 800-53, Regelung AU-9 (2)

#### <a name="protection-of-audit-information--audit-backup-on-separate-physical-systems--components"></a>Schutz von Überwachungsinformationen | Sicherung von Überwachungsinformationen auf getrennten physischen Systemen/Komponenten

**AU-9 (2)** Das Informationssystem sichert Überwachungsdatensätze [Zuordnung: von der Organisation definierte Häufigkeit] auf einem System oder einer Systemkomponente, die sich physisch an einem anderen Standort als das überwachte System oder die überwachte Komponente befindet.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint implementiert den Log Analytics-Dienst in der OMS. Bei den bereitgestellten VMs und Azure Diagnosespeicherkonten handelt es sich um Quellen, die mit Log Analytics verbunden sind und separat von ihrem Ursprung aufbewahrt werden. Daten werden nahezu in Echtzeit von der OMS erfasst. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-9-3"></a>NIST 800-53, Regelung AU-9 (3)

#### <a name="protection-of-audit-information--cryptographic-protection"></a>Schutz von Überwachungsinformationen | Kryptografischer Schutz

**AC-9 (3)** Das Informationssystem implementiert kryptografische Mechanismen, um die Integrität der Überwachungsinformationen und -tools zu schützen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint implementiert den Log Analytics-Dienst in der OMS. Log Analytics stellt sicher, dass eingehende Daten aus einer vertrauenswürdigen Quelle stammen, indem Zertifikate und die Integrität der Daten mittels Azure-Authentifizierung überprüft werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-9-4"></a>NIST 800-53, Regelung AU-9 (4)

#### <a name="protection-of-audit-information--access-by-subset-of-privileged-users"></a>Schutz von Überwachungsinformationen | Zugriff durch eine Teilmenge berechtigter Benutzer

**AU-9 (4)** Die Organisation autorisiert den Zugriff auf die Verwaltung von Überwachungsfunktionalität nur für [Zuordnung: von der Organisation definierte Teilmenge von berechtigten Benutzern].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Logische Zugriffskontrollen dienen dem Schutz von Überwachungsinformationen und -tools in diesem Azure-Blueprint vor nicht autorisierten Zugriffen, Änderungen und Löschungen. Azure Active Directory erzwingt einen genehmigten logischen Zugriff mithilfe von rollenbasierten Gruppenmitgliedschaften. Die Möglichkeit zum Anzeigen von Überwachungsinformationen und Verwenden von Überwachungstools kann auf Benutzer beschränkt werden, die diese Berechtigungen benötigen.
 |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-10"></a>NIST 800-53, Regelung AU-10

#### <a name="non-repudiation"></a>Unleugbarkeit

**AU-10** Das Informationssystem bietet Schutz vor Personen (oder von einer Person ausgeführte Prozesse), deren Ausführung fälschlicherweise abgelehnt wurde [Zuordnung: von der Organisation definierte Aktionen, für die Unleugbarkeit gelten muss].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Überwachungsfunktion für diesen Azure-Blueprint wird vom Azure Monitor- und Log Analytics-Dienst in der OMS bereitgestellt. Azure Monitor stellt detaillierte Überwachungsprotokolle zu Aktivitäten im Zusammenhang mit den bereitgestellten Ressourcen zur Verfügung. Diese Protokolle und Protokolle auf Betriebssystemebene werden von Log Analytics gesammelt und im OMS-Repository gespeichert. Diese Protokolle enthalten ausführliche Datensätze zu Ereignissen von Informationssystemen und können zum Schutz vor Unleugbarkeit beitragen. Darüber hinaus ist der Zugriff auf Daten durch rollenbasierte Zugriffssteuerung beschränkt, um eine nicht autorisierte Änderung oder Löschung von Protokolldaten zu verhindern. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-11"></a>NIST 800-53, Regelung AU-11

#### <a name="audit-record-retention"></a>Aufbewahrung von Überwachungsdatensätzen

**AU-11** Die Organisation bewahrt Überwachungsdatensätze für einen Zeitraum von [Zuordnung: von der Organisation definierter Zeitraum in Übereinstimmung mit der Richtlinie zur Aufbewahrung von Datensätzen] auf, um Unterstützung für nachträgliche Untersuchungen von Sicherheitsvorfällen bereitzustellen und gesetzliche sowie organisatorische Anforderungen hinsichtlich der Informationsaufbewahrung zu erfüllen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint implementiert den Log Analytics-Dienst in der OMS. Log Analytics sammelt Daten von verwalteten Ressourcen in einem zentralen Repository und stellt dadurch Überwachungsdienste für die OMS bereit. Gesammelte Daten werden gemäß Log Analytics-Konfiguration ein Jahr lang aufbewahrt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-12a"></a>NIST 800-53, Regelung AU-12.a

#### <a name="audit-generation"></a>Generierung von Überwachungsdatensätzen

**AU-12.a** Das Informationssystem stellt eine Funktion zur Generierung der Überwachungsdatensätze für die überwachbaren Ereignisse bereit, die unter AU-2.a zu [Zuordnung: von der Organisation definierte Informationssystemkomponenten] definiert werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Azure-Blueprint überwachten Ereignisse beinhalten diejenigen, die von Azure-Aktivitätsprotokollen für bereitgestellte Ressourcen, Protokollen auf Betriebssystemebene, Active Directory-Protokollen und SQL Server-Protokollen erfasst werden. Kunden können außerdem weitere Ereignisse für die Überwachung auswählen, um Unternehmensanforderungen zu entsprechen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-12b"></a>NIST 800-53, Regelung AU-12.b

#### <a name="audit-generation"></a>Generierung von Überwachungsdatensätzen

**AU-12.b** Das Informationssystem bietet [Zuordnung: von der Organisation definierten Mitarbeitern oder Rollen] die Möglichkeit, die durch bestimmte Komponenten des Informationssystems zu überwachenden Ereignisse auszuwählen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Zugriff auf Überwachungsfunktionen wird durch rollenbasierte Zugriffssteuerung in Azure und auf Betriebssystemebene der virtuellen Computer beschränkt. Die Konfiguration der Ereignisse, die zur Überwachung der von diesem Azure-Blueprint bereitgestellten Ressourcen ausgewählt wurden, können von Benutzern mit entsprechender rollenbasierter Autorisierung konfiguriert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-au-12c"></a>NIST 800-53, Regelung AU-12.c

#### <a name="audit-generation"></a>Generierung von Überwachungsdatensätzen

**AU-12.c** Das Informationssystem generiert Überwachungsdatensätze für die Ereignisse, die unter AU-2.d mit dem unter AU-3 definierten Inhalt definiert werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Azure-Blueprint überwachten Ereignisse beinhalten diejenigen, die von Azure-Aktivitätsprotokollen für bereitgestellte Ressourcen, Protokollen auf Betriebssystemebene, Active Directory-Protokollen und SQL Server-Protokollen erfasst werden. Kunden können außerdem weitere Ereignisse für die Überwachung auswählen, um Unternehmensanforderungen zu entsprechen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-12-1"></a>NIST 800-53, Regelung AU-12 (1)

#### <a name="audit-generation--system-wide--time-correlated-audit-trail"></a>Generierung von Überwachungsdatensätzen | Systemweiter bzw. zeitlich korrelierter Audit-Trail

**AU-12 (1)** Das Informationssystem kompiliert Überwachungsdatensätze aus [Zuordnung: von der Organisation definierte Informationssystemkomponenten] in einem systemweiten (logischen oder physischen) Audit-Trail, der zeitlich in [Zuordnung: von der Organisation definierter Toleranzgrad für das Verhältnis zwischen den Zeitstempeln der einzelnen Datensätze im Audit-Trail] korreliert wird.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Azure-Blueprint implementiert den Log Analytics-Dienst in der OMS. Log Analytics sammelt Daten von verwalteten Ressourcen in einem zentralen Repository und stellt dadurch Überwachungsdienste für die OMS bereit. Da Zeitstempel von Überwachungsdatensätzen nicht geändert werden, wird der Audit-Trail zeitlich korreliert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-au-12-3"></a>NIST 800-53, Regelung AU-12 (3)

#### <a name="audit-generation--changes-by-authorized-individuals"></a>Generierung von Überwachungsdatensätzen | Änderungen durch autorisierte Personen

**AU-12 (3)** Das Informationssystem bietet [Zuordnung: von der Organisation definierten Personen oder Rollen] die Möglichkeit, die für [Zuordnung: von der Organisation definierte Informationssystemkomponenten] auszuführende Überwachung basierend auf [Zuordnung: von der Organisation definierten auswählbaren Ereigniskriterien] innerhalb von [Zuordnung: von der Organisation definierten Zeitschwellenwerten] zu ändern.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Zugriff auf Überwachungsfunktionen wird durch rollenbasierte Zugriffssteuerung in Azure und auf Betriebssystemebene der virtuellen Computer beschränkt. Die Konfiguration der Ereignisse, die zur Überwachung der von diesem Azure-Blueprint bereitgestellten Ressourcen ausgewählt wurden, können von Benutzern mit entsprechender rollenbasierter Autorisierung konfiguriert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |
