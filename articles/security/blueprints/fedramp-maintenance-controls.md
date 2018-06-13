---
title: Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP – Wartung
description: Automatisierung von Webanwendungen für FedRAMP – Wartung
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 53acae01-bea9-4570-a9bc-734ff65262ba
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: de7dd5b4651f7f74d90d9d026af71cd676c720e6
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
ms.locfileid: "29150918"
---
# <a name="maintenance-ma"></a>Wartung (Maintenance, MA)

> [!NOTE]
> Diese Regelungen werden von NIST und dem U.S. Department of Commerce als Teil der NIST-Sonderveröffentlichung 800-53 Revision 4 definiert. Informationen zu Testverfahren und Anleitungen für jede Regelung finden Sie unter NIST 800-53 Rev. 4.

## <a name="nist-800-53-control-ma-1"></a>NIST 800-53, Regelung MA-1

#### <a name="system-maintenance-policy-and-procedures"></a>Systemwartung – Richtlinien und Verfahren

**MA-1** Die Organisation entwickelt, dokumentiert und verteilt an [Zuordnung: von der Organisation definierte Personen oder Rollen] eine Systemwartungsrichtlinie, die sich mit Zweck, Umfang, Rollen, Zuständigkeiten, Managementverpflichtung, Koordination zwischen Organisationsentitäten und Konformität befasst, stellt Verfahren zum Ermöglichen der Implementierung der Systemwartungsrichtlinie und der damit verbundenen Systemwartungsregelungen bereit und überprüft und aktualisiert die aktuelle Systemwartungsrichtlinie [Zuordnung: von der Organisation definierte Häufigkeit] und die Systemwartungsverfahren [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Systemwartungsrichtlinien und -verfahren des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-2a"></a>NIST 800-53, Regelung MA-2.a

#### <a name="controlled-maintenance"></a>Kontrollierte Wartung

**MA-2.a** In Übereinstimmung mit den Hersteller- oder Lieferantenspezifikationen und/oder Anforderungen der Organisation plant und dokumentiert die Organisation Wartungs- und Reparaturaktivitäten an Informationssystemkomponenten bzw. führt diese Aktivitäten durch und prüft die zugehörigen Datensätze.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für eine kontrollierte Wartung verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-2b"></a>NIST 800-53, Regelung MA-2.b

#### <a name="controlled-maintenance"></a>Kontrollierte Wartung

**MA-2.b** Die Organisation genehmigt und überwacht alle vor Ort oder remote ausgeführten Wartungsaktivitäten, unabhängig davon, ob die Arbeitsgeräte vor Ort oder an einem anderen Standort gewartet werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für eine kontrollierte Wartung verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-2c"></a>NIST 800-53, Regelung MA-2.c

#### <a name="controlled-maintenance"></a>Kontrollierte Wartung

**MA-2.c** Die Organisation verlangt, dass [Zuordnung: von der Organisation definierte Personen oder Rollen] das Entfernen des Informationssystems oder der Systemkomponenten aus den Einrichtungen der Organisation für externe Wartungs- oder Reparaturaktivitäten ausdrücklich genehmigen müssen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure verlangt, dass für Vermögenswerte (z.B. Netzwerkgeräte oder Server), die an einen externen Standort transportiert werden müssen, eine explizite Genehmigung seitens des Besitzers der jeweiligen Anlage vorliegt. |


 ## <a name="nist-800-53-control-ma-2d"></a>NIST 800-53, Regelung MA-2.d

#### <a name="controlled-maintenance"></a>Kontrollierte Wartung

**MA-2.d** Die Organisation bereinigt Arbeitsgeräte insofern, dass alle Informationen von den zugehörigen Medien entfernt werden, bevor diese für externe Wartungs- oder Reparaturaktivitäten aus den Einrichtungen der Organisation transportiert werden.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Der Microsoft Azure-Standard zum Schutz von Vermögenswerten definiert die Vorsichtsmaßnahmen für den Umgang mit Vermögenswerten, die beim Transport von Vermögenswerten an einen externen Standort anzuwenden sind. Der Standard zum Schutz von Vermögenswerten schreibt vor, dass Datenspeicheranlagen gemäß NIST SP 800-88, Guidelines for Media Sanitization, vor dem Transport aus dem Rechenzentrum bereinigt bzw. gelöscht werden müssen. |


 ## <a name="nist-800-53-control-ma-2e"></a>NIST 800-53, Regelung MA-2.e

#### <a name="controlled-maintenance"></a>Kontrollierte Wartung

**MA-2.e** Die Organisation überprüft alle potenziell betroffenen Sicherheitsmaßnahmen, um sicherzustellen, dass die Maßnahmen auch nach Wartungs- oder Reparaturaktivitäten weiterhin wirksam sind.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für eine kontrollierte Wartung verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-2f"></a>NIST 800-53, Regelung MA-2.f

#### <a name="controlled-maintenance"></a>Kontrollierte Wartung

**MA-2.f** Die Organisation erfasst [Zuordnung: von der Organisation definierte wartungsbezogene Informationen] in organisatorischen Wartungsdatensätzen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für eine kontrollierte Wartung verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ma-2-2a"></a>NIST 800-53, Regelung MA-2 (2).a

#### <a name="controlled-maintenance--automated-maintenance-activities"></a>Kontrollierte Wartung | Automatisierte Wartungsaktivitäten

**MA-2 (2).a** Die Organisation setzt automatisierte Mechanismen zum Planen, Durchführen und Dokumentieren von Wartungen und Reparaturen ein.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Automatisierung von Wartungsaktivitäten verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ma-2-2b"></a>NIST 800-53, Regelung MA-2 (2).b

#### <a name="controlled-maintenance--automated-maintenance-activities"></a>Kontrollierte Wartung | Automatisierte Wartungsaktivitäten

**MA-2 (2).b** Die Organisation erstellt aktuelle, genaue und vollständige Datensätze über alle angeforderten, geplanten, in Bearbeitung befindlichen und abgeschlossenen Wartungs- und Reparaturaktivitäten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Automatisierung von Wartungsaktivitäten verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-3"></a>NIST 800-53, Regelung MA-3

#### <a name="maintenance-tools"></a>Wartungswerkzeuge

**MA-3** Die Organisation genehmigt, kontrolliert und überwacht Wartungswerkzeuge für Informationssysteme.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure verwendet für die Durchführung der Wartung mehrere Tools bzw. Werkzeuge. Softwarewartungstools werden im Rahmen des Microsoft Azure-Änderungs- und Freigabeprozesses genehmigt, kontrolliert und verwaltet. <br /> Das für Standortdienste zuständige Team führt einen Bestand an genehmigten Wartungswerkzeugen für den Einsatz im Rechenzentrum (siehe MA-3). Wartungsmitarbeiter werden dazu angewiesen, die bereitgestellten Wartungswerkzeuge zu verwenden. Für die Verwendung von Werkzeugen, die nicht vom Rechenzentrum zur Verfügung gestellt werden, ist eine Genehmigung der Rechenzentrumsverwaltung erforderlich. Physische Handwerkzeuge (z.B. Schraubendreher, Schraubenschlüssel) sind von dieser Regelung ausgeschlossen. <br /> Jeder Raum verfügt über ein beschränktes physisches Schließfach oder einen zugangskontrollierten Raum für die Aufbewahrung von speziellen Wartungswerkzeugen, wie z.B. Oszilloskopen für Ethernettests und Fiber-Channel-Testgeräte von Fluke sowie Ethernet-Tonsignaltestern. Das für Standortdienste zuständige Team führt routinemäßige Bestandskontrollen durch, um den Zustand aller Werkzeuge zu überprüfen. Der Zugang zum Schließfach oder Wartungslagerraum wird in Zutrittsleserprotokollen, die im Falle einer Untersuchung zur Verfügung stehen, nachverfolgt. |


 ### <a name="nist-800-53-control-ma-3-1"></a>NIST 800-53, Regelung MA-3 (1)

#### <a name="maintenance-tools--inspect-tools"></a>Wartungswerkzeuge | Überprüfen der Werkzeuge

**MA-3 (1)** Die Organisation prüft die von Wartungsmitarbeitern in einen Raum eingeführten Wartungswerkzeuge auf unsachgemäße oder unbefugte Modifikationen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Das für Standortdienste zuständige Microsoft Azure-Team führt einen Bestand an genehmigten Wartungswerkzeugen für den Einsatz im Rechenzentrum (weitere Informationen finden Sie unter MA-3). Wartungsmitarbeiter werden dazu angewiesen, die bereitgestellten Wartungswerkzeuge zu verwenden. Für die Verwendung von Werkzeugen, die nicht vom Rechenzentrum zur Verfügung gestellt werden, ist eine Genehmigung der Rechenzentrumsverwaltung erforderlich. |


 ### <a name="nist-800-53-control-ma-3-2"></a>NIST 800-53, Regelung MA-3 (2)

#### <a name="maintenance-tools--inspect-media"></a>Wartungswerkzeuge | Überprüfen von Medien

**MA-3 (2)** Die Organisation prüft Medien, die Diagnose- und Testprogramme enthalten, auf Schadsoftware, bevor die Medien im Informationssystem verwendet werden.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure verbietet den Einsatz von mobilen Geräten oder Speichermedien in der Produktionsumgebung von Microsoft Azure-Rechenzentren, wenn keine Genehmigung seitens der Rechenzentrumsverwaltung vorliegt. Die Verwendung von persönlichen Medien in der Produktionsumgebung von Microsoft Azure-Rechenzentren ist untersagt. <br /> Microsoft Azure hat einen Prozess implementiert, um Laptops vor dem Einsatz in der Produktionsumgebung von Microsoft Azure-Rechenzentren zu überprüfen. Sicherheitsbeauftragte werden darin ausgebildet, Mitarbeiter, die Laptops in der Produktionsumgebung verwenden, zur Prüfung aufzufordern, ob die Laptops einer Überprüfung unterzogen wurden und diese bestanden haben. |


 ### <a name="nist-800-53-control-ma-3-3"></a>NIST 800-53, Regelung MA-3 (3)

#### <a name="maintenance-tools--prevent-unauthorized-removal"></a>Wartungswerkzeuge | Vermeidung einer unbefugten Entfernung

**MA-3 (3)** Die Organisation verhindert die unbefugte Entfernung von Wartungsgeräten, die organisatorische Informationen enthalten, indem sie sicherstellt, dass keine organisatorischen Informationen auf den Geräten enthalten sind; die Geräte bereinigt oder zerstört; die Geräte in einem Raum lagert; oder für [Zuordnung: von der Organisation definierte Personen oder Rollen] eine Befreiung bezüglich der Verpflichtung zur ausdrücklichen Genehmigung für die Entfernung des Geräts aus dem Raum einholt.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure setzt rechenzentrumsspezifische Wartungswerkzeuge ein, die innerhalb der Einrichtung gelagert und nicht entfernt werden. Jeder Raum verfügt über ein beschränktes physisches Schließfach oder einen Lagerraum für die Aufbewahrung von Wartungswerkzeugen, wie z.B. Oszilloskopen für Ethernettests und Fiber-Channel-Testgeräten von Fluke sowie Ethernet-Tonsignaltestern. Der Zugang zum Schließfach oder Lagerraum der Rechenzentrumsbeschaffung wird kontrolliert, um einen unbefugten Zugang zu den Wartungswerkzeugen zu verhindern. <br /> Organisatorische Informationen werden während der Wartung durch die unter MA-4 genannten Regelungen geschützt. Um auf organisatorische Informationen zugreifen zu können, muss der Benutzer über privilegierte Konten und Authentifikatoren verfügen. |


 ## <a name="nist-800-53-control-ma-4a"></a>NIST 800-53, Regelung MA-4.a

#### <a name="nonlocal-maintenance"></a>Nichtlokale Wartung

**MA-4.a** Die Organisation genehmigt und überwacht nichtlokale Wartungs- und Diagnoseaktivitäten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Durchführung von nichtlokalen Wartungsarbeiten an kundenseitig bereitgestellten Betriebssystemen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-4b"></a>NIST 800-53, Regelung MA-4.b

#### <a name="nonlocal-maintenance"></a>Nichtlokale Wartung

**MA-4.b** Die Organisation erlaubt die Verwendung von nichtlokalen Wartungs- und Diagnosewerkzeugen nur gemäß den organisatorischen Richtlinien und dokumentiert dies im Sicherheitsplan für das Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Durchführung von nichtlokalen Wartungsarbeiten an kundenseitig bereitgestellten Betriebssystemen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-4c"></a>NIST 800-53, Regelung MA-4.c

#### <a name="nonlocal-maintenance"></a>Nichtlokale Wartung

**MA-4.c** Die Organisation setzt leistungsstarke Authentifikatoren bei der Einrichtung von nichtlokalen Wartungs- und Diagnosesitzungen ein.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Durchführung von nichtlokalen Wartungsarbeiten an kundenseitig bereitgestellten Betriebssystemen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-4d"></a>NIST 800-53, Regelung MA-4.d

#### <a name="nonlocal-maintenance"></a>Nichtlokale Wartung

**MA-4.a** Die Organisation führt Aufzeichnungen über nichtlokale Wartungs- und Diagnoseaktivitäten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Durchführung von nichtlokalen Wartungsarbeiten an kundenseitig bereitgestellten Betriebssystemen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-4e"></a>NIST 800-53, Regelung MA-4.e

#### <a name="nonlocal-maintenance"></a>Nichtlokale Wartung

**MA-4.e** Die Organisation beendet Sitzungs- und Netzwerkverbindungen, wenn die nichtlokale Wartung abgeschlossen ist.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Durchführung von nichtlokalen Wartungsarbeiten an kundenseitig bereitgestellten Betriebssystemen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ma-4-2"></a>NIST 800-53, Regelung MA-4 (2)

#### <a name="nonlocal-maintenance--document-nonlocal-maintenance"></a>Nichtlokale Wartung | Dokumentieren von nichtlokalen Wartungsaktivitäten

**MA-4 (2)** Die Organisation dokumentiert im Sicherheitsplan für das Informationssystem die Richtlinien und Verfahren für die Einrichtung und Nutzung von nichtlokalen Wartungs- und Diagnoseverbindungen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Dokumentation von nichtlokalen Wartungsarbeiten im Sicherheitsplan für kundenseitig bereitgestellte Betriebssysteme verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ma-4-3"></a>NIST 800-53, Regelung MA-4 (3)

#### <a name="nonlocal-maintenance--comparable-security--sanitization"></a>Nichtlokale Wartung | Vergleichbares Sicherheits- bzw. Bereinigungssystem

**MA-4 (3)** Die Organisation verlangt, dass nichtlokale Wartungs- und Diagnosedienste von einem Informationssystem durchgeführt werden müssen, das eine vergleichbare Sicherheitsfunktion wie die in dem zu wartenden System implementierte Funktion implementiert; alternativ entfernt sie die zu wartende Komponente vor der Durchführung der nichtlokalen Wartungs- oder Diagnosedienste aus dem Informationssystem, bereinigt die Komponente (in Bezug auf organisatorische Informationen) vor der Entfernung aus den Räumen der Organisation und nach der Durchführung des Dienstes, und überprüft und bereinigt die Komponente (in Bezug auf potenzielle Schadsoftware), bevor die Komponente wieder mit dem Informationssystem verbunden wird.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, alle nichtlokalen Wartungsarbeiten an kundenseitig bereitgestellten Betriebssystemen von einem Informationssystem mit einem vergleichbaren Sicherheitsniveau durchführen zu lassen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ma-4-6"></a>NIST 800-53, Regelung MA-4 (6)

#### <a name="nonlocal-maintenance--cryptographic-protection"></a>Nichtlokale Wartung | Kryptografischer Schutz

**MA-4 (6)** Das Informationssystem implementiert kryptographische Mechanismen, um die Integrität und Vertraulichkeit der nichtlokalen Wartungs- und Diagnosekommunikation zu schützen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, kryptographische Mechanismen bei der Durchführung von nichtlokalen Wartungen und Diagnosen von kundenseitig bereitgestellten Betriebssystemen zu implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-5a"></a>NIST 800-53, Regelung MA-5.a

#### <a name="maintenance-personnel"></a>Wartungsmitarbeiter

**MA-5.a** Die Organisation legt einen Prozess für die Autorisierung von Wartungsmitarbeitern fest und führt eine Liste der autorisierten Wartungsorganisationen oder -mitarbeiter.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Prozess zur Autorisierung und Begleitung von Systemwartungsmitarbeitern des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-5b"></a>NIST 800-53, Regelung MA-5.b

#### <a name="maintenance-personnel"></a>Wartungsmitarbeiter

**MA-5.b** Die Organisation stellt sicher, dass Mitarbeiter ohne Begleitung, die die Wartung des Informationssystems durchführen, über die erforderlichen Zugangsberechtigungen verfügen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Prozess zur Autorisierung und Begleitung von Systemwartungsmitarbeitern des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-5c"></a>NIST 800-53, Regelung MA-5.c

#### <a name="maintenance-personnel"></a>Wartungsmitarbeiter

**MA-5.c** Die Organisation benennt Mitarbeiter der Organisation mit den erforderlichen Zugangsberechtigungen und der technischen Kompetenz, um die Wartungsaktivitäten von Mitarbeitern zu überwachen, die nicht über die erforderlichen Zugangsberechtigungen verfügen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Prozess zur Autorisierung und Begleitung von Systemwartungsmitarbeitern des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ma-5-1a"></a>NIST 800-53, Regelung MA-5 (1).a

#### <a name="maintenance-personnel--individuals-without-appropriate-access"></a>Wartungsmitarbeiter | Mitarbeiter ohne entsprechende Zugangsberechtigungen

**MA-5 (1).a** Die Organisation implementiert Verfahren für den Einsatz von Wartungsmitarbeitern, die nicht über angemessene Sicherheitsfreigaben verfügen oder nicht in den USA ansässig sind, für die u.a. folgende Anforderungen gelten: Wartungsmitarbeiter, die nicht über die erforderlichen Zugangsberechtigungen, Freigaben oder formalen Zugangsgenehmigungen verfügen, müssen während der Durchführung von Wartungs- und Diagnoseaktivitäten am Informationssystem von zugelassenem Mitarbeitern der Organisation, die über eine vollständige Freigabe, entsprechende Zugangsberechtigungen und die technischen Qualifikationen verfügen, begleitet und beaufsichtigt werden; vor der Einleitung von Wartungs- oder Diagnoseaktivitäten durch Mitarbeiter, die nicht über die erforderlichen Zugangsberechtigungen, Freigaben oder formalen Zugangsgenehmigungen verfügen, werden alle Komponenten von flüchtigen Informationsspeichern innerhalb des Informationssystems bereinigt und alle permanenten Speichermedien entfernt oder physisch vom System getrennt und gesichert.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Prozess zur Autorisierung und Begleitung von Systemwartungsmitarbeitern des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ma-5-1b"></a>NIST 800-53, Regelung MA-5 (1).b

#### <a name="maintenance-personnel--individuals-without-appropriate-access"></a>Wartungsmitarbeiter | Mitarbeiter ohne entsprechende Zugangsberechtigungen

**MA-5 (1) .b** Die Organisation entwickelt und implementiert alternative Sicherheitsvorkehrungen für den Fall, dass eine Komponente des Informationssystems nicht bereinigt, entfernt oder vom System getrennt werden kann.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Prozess zur Autorisierung und Begleitung von Systemwartungsmitarbeitern des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ma-6"></a>NIST 800-53, Regelung MA-6

#### <a name="timely-maintenance"></a>Rechtzeitige Wartung

**MA-6** Die Organisation erhält innerhalb von [Zuordnung: von der Organisation definierter Zeitraum] nach einem Ausfall Wartungssupport und/oder Ersatzteile für [Zuordnung: von der Organisation definierte Informationssystemkomponenten].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure-Rechenzentren verfügen über ein festes Wartungspersonal, das Support für kritische Infrastruktursysteme für Rechenzentren und den Betrieb von Rechenzentren bereitstellt. Die Teams haben kritische Sicherheits- und Technologiesystemkomponenten identifiziert, für die sie Ersatzteile vor Ort im Bestand führen. Kritische Systeme werden in N+1-Konfigurationen entworfen, und Dienste sind auf Resilienz ausgelegt. Dies ermöglicht es dem Verwaltungsteam des Rechenzentrums, die Wiederherstellungsziele im Falle einer Dienstunterbrechung oder bei Anwendung eines Notfallplans zu erreichen. Kritische Informationssystemdienste werden von mehr als einem Rechenzentrum bereitgestellt, um eine Unterbrechung der Dienste aufgrund eines Vorfalls in einem der Rechenzentren zu verhindern. Kundenanwendungen sind für die Bereitstellung in mehreren Rechenzentren verantwortlich, um Redundanz und Ausfallsicherheit zu gewährleisten. |
