---
title: Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP – Physischer Schutz und Schutz der Umgebung
description: Automatisierung von Webanwendungen für FedRAMP – Physischer Schutz und Schutz der Umgebung
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 0bf8349b-450d-413c-a535-6f7b80b82781
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 792b9da0f4e5ec73c39f56a6e4805cf3c37133c4
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33206561"
---
# <a name="physical-and-environmental-protection-pe"></a>Physischer Schutz und Schutz der Umgebung (Physical and Environmental Protection, PE)

> [!NOTE]
> Diese Regelungen werden von NIST und dem U.S. Department of Commerce als Teil der NIST-Sonderveröffentlichung 800-53 Revision 4 definiert. Informationen zu Testverfahren und Anleitungen für jede Regelung finden Sie unter NIST 800-53 Rev. 4.

## <a name="nist-800-53-control-pe-1"></a>NIST 800-53, Regelung PE-1

#### <a name="physical-and-environmental-protection-policy-and-procedures"></a>Verfahren für physischen Schutz und Schutz der Umgebung

**PE-1** Die Organisation entwickelt, dokumentiert und verteilt an [Zuordnung: von der Organisation definierte Personen oder Rollen] eine Richtlinie für den physischen Schutz und Schutz der Umgebung, die sich mit Zweck, Umfang, Rollen, Zuständigkeiten, Managementverpflichtung, Koordination zwischen Organisationsentitäten und Konformität befasst, stellt Verfahren zum Ermöglichen der Implementierung der Richtlinie für den physischen Schutz und Schutz der Umgebung und der zugehörigen Regulierungen für den physischen Schutz und Schutz der Umgebung bereit und überprüft und aktualisiert die aktuelle Richtlinie für den physischen Schutz und Schutz der Umgebung [Zuordnung: von der Organisation definierte Häufigkeit] sowie die Verfahren für den physischen Schutz und Schutz der Umgebung [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Richtlinie und Verfahren des Kunden für den physischen Schutz und Schutz der Umgebung auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pe-2a"></a>NIST 800-53, Regulierung PE-2.a

#### <a name="physical-access-authorizations"></a>Autorisierungen für physischen Zugriff

**PE-2.a** Die Organisation entwickelt, genehmigt und verwaltet eine Liste der Personen mit autorisiertem Zugang zu dem Gebäude, wo sich das Informationssystem befindet.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Physischer Zugriff auf ein Microsoft-Rechenzentrum muss vom Rechenzentrumsverwaltungs-Team (Datacenter Management, DCM) mithilfe des Tools für Zugriff auf das Rechenzentrum genehmigt werden. Zugriffszuordnungen erfordern ein Enddatum, nach dessen Verstreichen der Zugriff automatisch entzogen wird und neu genehmigt werden muss. Wenn kein Zugriff mehr erforderlich ist, fordern Sicherheitsbeauftragte oder Verwaltung des Rechenzentrums darüber hinaus manuell die Beendigung des Zugriffs an. |


 ## <a name="nist-800-53-control-pe-2b"></a>NIST 800-53, Regelung PE-2.b

#### <a name="physical-access-authorizations"></a>Autorisierungen für physischen Zugriff

**PE-2.b** Die Organisation gibt Anmeldeinformationen für die Autorisierung für den Zugang zum Gebäude aus.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Das Tool für Zugriff auf das Rechenzentrum ist die maßgebliche Quelle, die alle Personen auflistet, die für den Zugriff auf ein bestimmtes Rechenzentrum autorisiert sind. Das Tool ist mit den Zugriffssteuerungsmitteln für die physische Sicherheit des Rechenzentrums verknüpft und autorisiert den Zugriff auf Grundlage von Zugriffsebenen, die vom DCM-Team genehmigt werden. Zugriffsebenen werden im Tool entweder dem von Microsoft ausgestellten Badge eines Benutzers oder einem Badge für temporären Zugriff zugewiesen, das im Rechenzentrum vom Kontrollraumsupervisor (Control Room Supervisor, CRS) zugewiesen wird. Zugriffsebenen werden vom DCM-Team genehmigt. Zusätzlich zu den Anmeldeinformationen, die physischen Badges zugewiesen werden, erfordern einige Bereiche des Rechenzentrums die Registrierung der biometrischen Daten des Benutzers (Handgeometrie oder Fingerabdruck). |


 ## <a name="nist-800-53-control-pe-2c"></a>NIST 800-53, Regelung PE-2.c

#### <a name="physical-access-authorizations"></a>Autorisierungen für physischen Zugriff

**PE-2.c** Die Organisation überprüft die Zugriffsliste, in der die Autorisierung von Personen für den Zugang zum Gebäude ausführlich festgehalten ist, [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Zusätzlich zu der in Teil a beschriebenen Sperrung des Zugriffs prüft Microsoft Azure Listen über autorisierten Zugriff für Azure-Rechenzentren alle 90 Tage, um individuellen Zugriff nach Bedarf aufzuheben/zu aktualisieren. |


 ## <a name="nist-800-53-control-pe-2d"></a>NIST 800-53, Regelung PE-2.d

#### <a name="physical-access-authorizations"></a>Autorisierungen für physischen Zugriff

**PE-2.d** Die Organisation entfernt Personen von der Liste für den Zugang zum Gebäude, wenn der Zugriff nicht mehr erforderlich ist.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure hebt den Zugriff automatisch auf, wenn das Enddatum der Zugriffszuweisung erreicht ist. Wenn kein Zugriff mehr erforderlich ist, fordern Sicherheitsbeauftragte oder Verwaltung des Rechenzentrums manuell die Beendigung des Zugriffs im Zugriffstool des Rechenzentrums an. Darüber hinaus hebt Microsoft Azure alle nicht benötigten Zugriffsautorisierungen auf, die im Rahmen der in Teil c beschriebenen Überprüfung der Zugriffsliste ermittelt werden. |


 ## <a name="nist-800-53-control-pe-3a"></a>NIST 800-53, Regelung PE-3.a

#### <a name="physical-access-control"></a>Steuerung des physischen Zugriffs

**PE-3.a** Die Organisation erzwingt Autorisierungen des physischen Zugriffs bei [Zuordnung: von der Organisation definierte Eingangs-/Ausgangspunkte des Gebäudes, wo sich das Informationssystem befindet] durch Überprüfen der individuellen Zugriffsautorisierungen, bevor der Zugang zu dem Gebäude gewährt wird, sowie Kontrolle des Eingangs/Ausgangs des Gebäudes mit [Auswahl (mindestens): [Zuordnung: von der Organisation definierte Systeme/Mittel zur Steuerung des physischen Zugriffs]; Wächter].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure erzwingt Autorisierungen des physischen Zugriffs für alle physischen Zugangspunkte von Azure-Rechenzentren 24 Stunden täglich mit Personal, Alarmen, Videoüberwachung, mehrstufiger Authentifizierung und Personenschleusen. |


 ## <a name="nist-800-53-control-pe-3b"></a>NIST 800-53, Regelung PE-3.b

#### <a name="physical-access-control"></a>Steuerung des physischen Zugriffs

**PE-3.b** Die Organisation verwaltet Überwachungsprotokolle für physischen Zugriff [Zuordnung: von der Organisation definierte Eingangs-/Ausgangspunkte].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Alle Zugänge zu den Gebäuden von Azure-Rechenzentren werden protokolliert und überwacht. |


 ## <a name="nist-800-53-control-pe-3c"></a>NIST 800-53, Regelung PE-3.c

#### <a name="physical-access-control"></a>Steuerung des physischen Zugriffs

**PE-3.c** Die Organisation stellt [Zuordnung: von der Organisation definierte Schutzvorrichtungen] zum Kontrollieren des Zugangs zu Bereichen innerhalb des Gebäudes, die offiziell als öffentlich zugänglich definiert sind, bereit.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Azure-Rechenzentren enthalten keine Bereiche, die als öffentlich zugänglich definiert sind. |


 ## <a name="nist-800-53-control-pe-3d"></a>NIST 800-53, Regelung PE-3.d

#### <a name="physical-access-control"></a>Steuerung des physischen Zugriffs

**PE-3.d** Die Organisation sorgt für die Begleitung von Besuchern und Überwachung ihrer Aktivitäten [Zuordnung: von der Organisation definierte Umstände, die Begleitung und Überwachung von Besuchern erfordern].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Alle Besucher, denen der Zugang zum Rechenzentrum genehmigt wird (siehe PE-2), tragen Badges mit dem Hinweis „Nur in Begleitung“ oder andere visuelle Hinweise (z.B. farbige Badges) und müssen jederzeit in Begleitung sein. Begleiteten Besucher werden keine Zugriffsebenen gewährt, und sie sind auf den Zugang ihrer Begleiter beschränkt. Begleiter überwachen alle Aktivitäten ihrer Besucher im Rechenzentrum. |


 ## <a name="nist-800-53-control-pe-3e"></a>NIST 800-53, Regelung PE-3.e

#### <a name="physical-access-control"></a>Steuerung des physischen Zugriffs

**PE-3.e** Die Organisation sichert Schlüssel, Kombinationen und andere Mittel des physischen Zugriffs.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Physische Schlüssel und Badges für temporären Zugriff werden im Sicherheitsvorgangszentrum (Security Operations Center, SOC) sicher aufbewahrt. Sicherheitsbeauftragte sind 24 Stunden täglich vor Ort. Schlüssel werden an bestimmte Mitarbeiter ausgegeben, wenn das Zugangsbadge der Person mit dem physischen Schlüssel übereinstimmt. Während jeder Schicht werden SchlüsseIinventuren durchgeführt, und Schlüssel dürfen nicht außerhalb des Standorts mitgenommen werden. |


 ## <a name="nist-800-53-control-pe-3f"></a>NIST 800-53, Regelung PE-3.f

#### <a name="physical-access-control"></a>Steuerung des physischen Zugriffs

**PE-3.f** Die Organisation inventarisiert [Zuordnung: von der Organisation definierte Mittel für den physischen Zugriff] alle [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Mittel für den physischen Zugriff in Azure-Rechenzentren werden mindestens einmal jährlich inventarisiert. Schlüssel und Badges für temporären Zugriff werden mehrmals täglich während jeder Schicht inventarisiert. Lesegeräte für Zugangsbadges und ähnliche Zugangsmittel sind mit dem physischen Sicherheitssystem verknüpft, wo der Status fortlaufend dargestellt wird. |


 ## <a name="nist-800-53-control-pe-3g"></a>NIST 800-53, Regelung PE-3.g

#### <a name="physical-access-control"></a>Steuerung des physischen Zugriffs

**PE-3.g** Die Organisation ändert Kombinationen und Schlüssel [Zuordnung: von der Organisation definierte Häufigkeit] und/oder wenn die Schlüssel verloren gegangen sind, Kombinationen kompromittiert wurden, oder Personen versetzt werden bzw. ausscheiden.  

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure-Rechenzentren implementieren bestimmte Verfahren in Fällen, wo ein Badge oder Schlüssel für den Zugriff verloren gegangen ist, oder eine Person versetzt wird oder ausscheidet. Im Falle des Versetzens oder Ausscheidens wird der Zugriff der Person sofort im System aufgehoben und ihr Zugangsbadge entfernt. |


 ### <a name="nist-800-53-control-pe-3-1"></a>NIST 800-53, NIST 800-53, Regelung PE-3 (1)

#### <a name="physical-access-control--information-system-access"></a>Steuerung des physischen Zugriffs | Zugriff auf das Informationssystem

**PE-3 (1)** Die Organisation erzwingt Autorisierungen für den physischen Zugriff auf das Informationssystem zusätzlich zu den Kontrollen des physischen Zugangs zu dem Gebäude in [Zuordnung: von der Organisation definierte physische Bereiche, die mindestens eine Komponente des Informationssystems enthalten].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure schränkt weiterhin den Zugang zu Bereichen in Microsoft-Rechenzentren, die wichtige Systeme enthalten (z.B. Serverhousings, kritische Umgebungen, MDF-Räume usw.) mit verschiedenen Sicherheitsmechanismen ein, z.B. elektronischer Zugangskontrolle, biometrischen Geräten und Antipassbackkontrollen. Zugriff auf Azure-Serverhousings wird als separate, höhere Ebene des DCAT-Zugriffs als andere Zugriffsbereiche des Rechenzentrums gewährt. Darüber hinaus müssen sich alle Azure-Vollzeitmitarbeiter und Lieferanten, die Zugriff auf die Azure Government-Serverhousings haben, formell dem Cloud Screening von Microsoft und der Überprüfung ihrer US-Staatsbürgerschaft unterziehen, bevor sie für den Zugriff auf die Umgebung autorisiert sind. Weitere Informationen zum Cloud Screening für die Azure Government-Serverhousings finden Sie im Abschnitt PS-03. |


 ## <a name="nist-800-53-control-pe-4"></a>NIST 800-53, Regelung PE-4

#### <a name="access-control-for-transmission-medium"></a>Zugriffssteuerung für das Übertragungsmedium

**PE-4** Die Organisation steuert den physischen Zugriff auf [Zuordnung: von der Organisation definierte Verteilungs- und Übertragungsleitungen des Informationssystems] in Gebäuden der Organisation mit [Zuordnung: von der Organisation definierte Schutzvorrichtungen].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure hat die Steuerung des Zugriffs auf das Übertragungsmedium über den Entwurf und den Aufbau der Hauptverteilerräume (Main Distribution Frame, MDF) und Serverhousings implementiert, um Verteilungs- und Übertragungsleitungen des Informationssystems vor versehentlicher Beschädigung, Unterbrechung und physischer Manipulation zu schützen. Für den Zugang zu MDF-Räumen und Serverhousings ist die zweistufige Authentifizierung erforderlich (Zugangsbadge und Biometrie). Dadurch wird sichergestellt, dass der Zugriff entsprechend berechtigten Mitarbeitern (siehe PE-2, PE-3) vorbehalten ist. Innerhalb der MDF-Räume werden Verteilungs- und Übertragungsleitungen durch die Verwendung von Metallrohrkabeln, gesperrten Racks, Käfigen oder Kabelschächten vor versehentlicher Beschädigung, Unterbrechung und physischer Manipulation geschützt. |


 ## <a name="nist-800-53-control-pe-5"></a>NIST 800-53, Regelung PE-5

#### <a name="access-control-for-output-devices"></a>Steuerung des Zugriffs auf Ausgabegeräte

**PE-5** Die Organisation steuert den physischen Zugriff auf Ausgabegeräte des Informationssystems, um zu verhindern, dass nicht autorisierte Personen die Ausgabe erhalten.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure-Rechenzentren verfügen nicht über Ausgabegeräte (Monitore, Drucker, Audiogeräte usw.), die dauerhaft mit Azure-Ressourcen oder freigegebenen Azure-Ressourcen verbunden sind. Außerdem führen Sicherheitsbeauftragte mehrmals pro Schicht Kontrollgänge im Gebäude durch und prüfen dabei z.B., ob Türen verschlossen und Racks gesichert sind. Der Zugang zum Rechenzentrum ist auf Personen mit Zugriffsgenehmigung beschränkt. Für den Zugang zu Serverhousings ist die zweistufige Authentifizierung erforderlich (Zugangsbadge und Biometrie). |


 ## <a name="nist-800-53-control-pe-6a"></a>NIST 800-53, Regelung PE-6.a

#### <a name="monitoring-physical-access"></a>Überwachen des physischen Zugriffs

**PE-6.a** Die Organisation überwacht den physischen Zugang zu dem Gebäude, in dem sich das Informationssystem befindet, um physische Sicherheitsvorfälle zu erkennen und darauf zu reagieren.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Der physische Zugriff wird durch Implementieren von Sicherheitsgeräten und -prozessen in den Rechenzentren überwacht. Beispiele hierfür sind 24 Stunden täglich elektronische Überwachung der Zugriffskontrolle, Alarm- und Videosysteme sowie Kontrollgänge im Gebäude und auf dem Grundstück. Ein Kontrollraumsupervisor befindet sich jederzeit zur Überwachung des physischen Zugriffs im Rechenzentrum im SOC. |


 ## <a name="nist-800-53-control-pe-6b"></a>NIST 800-53, Regelung PE-6.b

#### <a name="monitoring-physical-access"></a>Überwachen des physischen Zugriffs

**PE-6.b** Die Organisation überprüft Protokolle des physischen Zugriffs [Zuordnung: von der Organisation definierte Häufigkeit] und beim Eintreten von [Zuordnung: von der Organisation definierte Ereignisse oder potenzielle Anzeichen für Ereignisse].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Protokolle des physischen Zugriffs werden fortlaufend überprüft und für nachfolgende investigative Überprüfungen beibehalten. |


 ## <a name="nist-800-53-control-pe-6c"></a>NIST 800-53, Regelung PE-6.c

#### <a name="monitoring-physical-access"></a>Überwachen des physischen Zugriffs

**PE-6.c** Die Organisation koordiniert die Ergebnisse der Überprüfungen und Ermittlungen mithilfe der Organisationsfunktion für die Reaktion auf Vorfälle. 

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Sicherheitsereignisse, die innerhalb des Rechenzentrums auftreten, werden vom Sicherheitsteam dokumentiert. Das Sicherheitsteam erstellt Berichte, in denen die Details eines Sicherheitsereignisses nach seinem Auftreten erfasst werden. <br /> Bei Vorfällen, die eine Benachrichtigung einer Behörde erfordern, koordiniert das Microsoft Azure-Sicherheitsteam mit dem Hauptanwendungsanbieter (z.B. O365) die Benachrichtigung von Genehmigungsbehörde, US CERT und FedRAMP im Rahmen der US-CERT-Richtlinien (siehe IR-6). |


 ### <a name="nist-800-53-control-pe-6-1"></a>NIST 800-53, Regelung PE-6 (1)

#### <a name="monitoring-physical-access--intrusion-alarms--surveillance-equipment"></a>Überwachen des physischen Zugriffs | Eindringalarme / Überwachungsgeräte

**PE-6 (1)** Die Organisation überwacht Alarme bei physischem Eindringen und Überwachungsgeräte.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Zusätzlich zu der Sicherheit vor Ort 24 Stunden täglich nutzen (geleaste und vollständig verwaltete) Microsoft Azure-Rechenzentren auch Alarmüberwachungssysteme und Überwachungskameras. Der 24 Stunden täglich im SOC anwesende Kontrollraumsupervisor überwacht Alarme und reagiert darauf. In einer Reaktionssituation nutzt der Kontrollraumsupervisor Kameras in dem Bereich, wo der untersuchte Vorfall aufgetreten ist, um dem Reagierenden Echtzeitinformationen zu liefern. |


 ### <a name="nist-800-53-control-pe-6-4"></a>NIST 800-53, Regelung PE-6 (4)

#### <a name="monitoring-physical-access--monitoring-physical-access-to-information-systems"></a>Überwachen des physischen Zugriffs | Überwachen des physischen Zugriffs auf Informationssysteme

**PE-6 (4)** Die Organisation überwacht den physischen Zugriff auf das Informationssystem zusätzlich zur Überwachung des physischen Zugangs zum Gebäude in [Zuordnung: von der Organisation definierte physische Bereiche, die mindestens eine Komponente des Informationssystems enthalten].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure überwacht sowohl den physischen Zugang zum Gebäude als auch die Informationssysteme im Rechenzentrum. Alle Geräte der Onlinedienste von Microsoft befinden sich in den Bereichen von Rechenzentren, die auf physischen Zugriff überwacht werden. Alle Serverhousings und MDF-Räume werden durch Zugriffssteuerung, Alarme und Video geschützt. |


 ## <a name="nist-800-53-control-pe-8a"></a>NIST 800-53, Regelung PE-8.a

#### <a name="visitor-access-records"></a>Datensätze von Besucherzugriffen

**PE-8.a** Die Organisation bewahrt Datensätze von Besucherzugängen in das Gebäude, wo sich das Informationssystem befindet, für [Zuordnung: von der Organisation definierter Zeitraum] auf.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Datensätze von Rechenzentrumszugriffen werden im Rechenzentrumszugriffs-Tool in Form genehmigter Anforderungen verwaltet. Wie in PE-3 beschrieben, müssen Besucher jederzeit begleitet werden. Der Zugriff des Begleiters innerhalb des Rechenzentrums wird protokolliert und kann ggf. zur späteren Überprüfung mit dem Besucher korreliert werden. |


 ## <a name="nist-800-53-control-pe-8b"></a>NIST 800-53, Regelung PE-8.b

#### <a name="visitor-access-records"></a>Datensätze von Besucherzugriffen

**PS-8.b** Die Organisation überprüft die Datensätze von Besucherzugriffen [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Die Datensätze der Zugriffe von Besuchern mit genehmigter Zugriffsanforderung werden überprüft, wenn ihre Identifizierung anhand einer von einer Behörde ausgegebenen ID oder des von Microsoft ausgestellten Badges verifiziert wird. Wie in PE-3 beschrieben, müssen Besucher während ihres Aufenthalts im Rechenzentrum jederzeit begleitet werden. |


 ### <a name="nist-800-53-control-pe-8-1"></a>NIST 800-53, Regelung PE-8 (1)

#### <a name="visitor-access-records--automated-records-maintenance--review"></a>Datensätze von Besucherzugriffen | Automatische Verwaltung von Datensätzen / Überprüfen

**PE-8 (1)** Die Organisation wendet automatisierte Mechanismen zur Vereinfachung der Verwaltung und Überprüfung von Datensätzen von Besucherzugriffen an.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure verwaltet Datensätze von Rechenzentrumszugriffen in DCAT in Form genehmigter DCAT-Anforderungen. DCAT-Anforderungen können nur vom DCM-Team genehmigt werden. Zugriffsebenen innerhalb des Rechenzentrums werden in DCAT zugewiesen und verwaltet. Der Rechenzentrumszugriff wird vierteljährlich überprüft. Alle Zugriffe auf Azure-Rechenzentren werden in DCAT aufgezeichnet und sind für mögliche zukünftige Ermittlungen verfügbar. Besucher müssen jederzeit begleitet werden. Der Zugriff des Begleiters innerhalb des Rechenzentrums wird im Alarmüberwachungssystem protokolliert und kann ggf. zur späteren Überprüfung mit dem Besucher korreliert werden. Der Besucherzugriff wird fortlaufend durch den zugewiesenen Begleiter und den Kontrollraumsupervisor mittels Überwachungskamera und Alarmüberwachungssystem überprüft. Besuchern wird kein Zugriff gestattet, und sie müssen jederzeit begleitet werden. |


 ## <a name="nist-800-53-control-pe-9"></a>NIST 800-53, Regelung PE-9

#### <a name="power-equipment-and-cabling"></a>Stromversorgungsgeräte und -verkabelung

**PE-9** Die Organisation schützt Stromversorgungsgeräte und -verkabelung für das Informationssystem vor Beschädigung und Zerstörung.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure bietet Schutzbereiche und entsprechende Kennzeichnung für Kabel. Die Geräte der Infrastruktur von Microsoft Azure wie Kabel, elektrische Leitungen und Notstromaggregate müssen sich in Bereichen befinden, die so konzipiert wurden, dass sie vor Umgebungsrisiken wie Diebstahl, Feuer, Explosivstoffen, Rauch, Wasser, Staub, Vibrationen, Erdbeben, schädlichen Chemikalien, elektrischen Störungen, Stromausfällen und elektrischen Störgrößen (Spitzen) geschützt sind. Alle tragbaren Ressourcen der Onlinedienste (z.B. Racks, Server, Netzwerkgeräte) müssen an ihrem Standort verriegelt oder befestigt werden, um vor Diebstahl oder Schäden durch Bewegung geschützt zu sein. Stromversorgungs- und Informationssystemkabel in Microsoft Azure-Umgebungen sind entsprechend gekennzeichnet und gegen Abhörmaßnahmen oder Beschädigung geschützt. Stromversorgungs- und Informationssystemkabel werden an allen Punkten in einer Umgebung voneinander getrennt, um Störungen zu vermeiden. |


 ## <a name="nist-800-53-control-pe-10a"></a>NIST 800-53, Regelung PE-10.a

#### <a name="emergency-shutoff"></a>Notabschaltung

**PE-10.a** Die Organisation bietet die Möglichkeit, die Stromversorgung des Informationssystems oder einzelner Systemkomponenten im Notfall abzuschalten.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure hat gemäß der lokalen Brandschutzbestimmungen Notabschaltungs-Schalter (Emergency Power Off, EPO) innerhalb des Rechenzentrums installiert. In einigen von Microsoft Azure verwalteten Rechenzentren setzt der Rechenzentrumsentwurf keine EPO-Schalter mehr voraus. |


 ## <a name="nist-800-53-control-pe-10b"></a>NIST 800-53, Regelung PE-10.b

#### <a name="emergency-shutoff"></a>Notabschaltung

**PE-10.b** Die Organisation platziert Notabschaltungsschalter oder -geräte in [Zuordnung: von der Organisation definierte Position gemäß Informationssystem oder Systemkomponente], um sicheren und einfachen Zugriff durch Mitarbeiter zu ermöglichen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. EPO-Schalter werden strategisch sinnvoll platziert, damit sie im Notfall mühelos betätigt werden können. EPO-Schalter können in Serverhousings, mit Personal besetzten Facilities Operation Centers (FOCs) oder nach Vorgabe der lokalen Brandschutzbestimmungen platziert werden. |


 ## <a name="nist-800-53-control-pe-10c"></a>NIST 800-53, Regelung PE-10.c

#### <a name="emergency-shutoff"></a>Notabschaltung

**PE-10.c** Die Organisation schützt die Notabschaltung der Stromversorgung vor nicht autorisierter Aktivierung.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Um die versehentliche Aktivierung zu verhindern, können EPO-Schalter mit einem Schutzgehäuse versehen werden, duale Aktivierung erfordern oder vor der Aktivierung einen akustischen Alarm als Warnung ausgeben. Darüber hinaus stehen EPO-Schalter unter Videoüberwachung. |


 ## <a name="nist-800-53-control-pe-11"></a>NIST 800-53, Regelung PE-11

#### <a name="emergency-power"></a>Notstromversorgung

**PE-11** Die Organisation bietet eine kurzfristige unterbrechungsfreie Stromversorgung, um [Auswahl (mindestens): ein ordnungsgemäßes Herunterfahren des Informationssystems; den Übergang des Informationssystems zu langfristiger alternativer Stromversorgung] zu erleichtern, wenn eine primäre Stromquelle ausfällt.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure hat durch den Schutz der Geräte und Schaltkreise im Rechenzentrum mit einem unterbrechungsfreien Stromversorgungssystem (USV) eine kurzfristige Notstromversorgung zur Überbrückung, bis Notstromaggregate online geschaltet werden können, implementiert. |


 ### <a name="nist-800-53-control-pe-11-1"></a>NIST 800-53, Regelung PE-11 (1)

#### <a name="emergency-power--long-term-alternate-power-supply---minimal-operational-capability"></a>Notstromversorgung | Langfristige alternative Stromversorgung – minimale Betriebsfähigkeit

**PE-11 (1)** Die Organisation bietet eine langfristige alternative Stromversorgung für das Informationssystem, die die minimal erforderliche Betriebsfähigkeit bei einem erweiterten Ausfall der primären Stromquelle aufrechterhalten kann.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure hat eine langfristige alternative Stromversorgung für das Informationssystem implementiert, die die minimal erforderliche Betriebsfähigkeit bei einem erweiterten Ausfall der primären Stromquelle aufrechterhalten kann. Wenn die Stromversorgung ausfällt oder auf einen nicht akzeptablen Spannungspegel sinkt, übernehmen unterbrechungsfreie Stromversorgungssysteme (USVs) sofort die Stromversorgung. Diese Stromversorgung reicht aus, um die Server zu betreiben, bis die Notstromaggregate starten. Notstromaggregate sichern die Stromversorgung bei längeren Ausfallzeiten und geplanter Wartung und können bei Naturkatastrophen dank lokaler Kraftstoffreserven den Betrieb des Rechenzentrums gewährleisten. Azure betreibt Dieselgeneratoren in vielen seiner Rechenzentren. Notstromaggregate werden verwendet, um die Stabilität der Netzstromversorgung zu unterstützen, oder in außergewöhnlichen Reparatur- und Wartungssituationen, die erfordern, dass die Rechenzentren vom Stromnetz getrennt werden müssen. |


 ## <a name="nist-800-53-control-pe-12"></a>NIST 800-53, Regelung PE-12

#### <a name="emergency-lighting"></a>Notbeleuchtung

**PE-12** Die Organisation verwendet und wartet eine automatische Notbeleuchtung für das Informationssystem, die bei einem Ausfalls oder einer Unterbrechung der Stromversorgung aktiviert wird und Notausgänge und Evakuierungswege im Gebäude sichert.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. (Geleaste und vollständig verwaltete) Microsoft Azure-Rechenzentren implementieren die Notbeleuchtung in Form einer an der Decke angebrachten Notbeleuchtung in dedizierten, durch USV und Generatorsysteme gesicherten Schaltkreisen (siehe PE-11). Automatische Notbeleuchtung wird entlang aller Evakuierungswege, bei allen Notausgängen und innerhalb der Serverhousings gemäß des National Fire and Protection Association (NFPA) Life Safety Code implementiert. Falls die Netzstromversorgung ausfällt, schaltet die Notbeleuchtung automatisch zur Notstromversorgung durch USV und Generatoren um. Die Notbeleuchtungssysteme in Microsoft Azure-Rechenzentren unterliegen Routinewartungsmaßnahmen, um sicherzustellen, dass sie stets in funktionsfähigem Zustand sind. |


 ## <a name="nist-800-53-control-pe-13"></a>NIST 800-53, Regelung PE-13

#### <a name="fire-protection"></a>Brandschutz

**PE-13** Die Organisation nutzt und wartet Geräte/Systeme zur Bekämpfung und Erkennung von Bränden für das Informationssystem, die von einer unabhängigen Energiequelle versorgt werden.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure hat den Brandschutz durch die Installation von Systemen zur Bekämpfung und Erkennung von Bränden in den Microsoft Azure-Rechenzentren implementiert. <br /> Microsoft Azure-Rechenzentren implementieren stabile Branderkennungsmechanismen. Der Brandschutzansatz von Microsoft Azure umfasst die Verwendung fotoelektrischer, unter dem Boden und an der Decke installierter Rauchmelder, die in das Sprinkleranlagensystem integriert sind. Darüber hinaus befinden sich Xtralis VESDA-Systeme (Very Early Smoke Detection Apparatus – Gerät für sehr frühe Raucherkennung) in jedem Serverhousing, die die Luft überwachen. VESDA-Einheiten sind hochempfindliche Systeme zur Luftprobenentnahme, die in mehreren besonders wichtigen Bereichen installiert sind. VESDA-Einheiten ermöglichen eine Untersuchung vor einem tatsächlichen Feuererkennungsalarm. <br /> Feuermelder sind im gesamten Rechenzentrum für manuellen Feueralarm installiert. Feuerlöscher sind über das gesamte Rechenzentrum verteilt und werden jährlich ordnungsgemäß geprüft, gewartet und gekennzeichnet. Das Sicherheitspersonal kontrolliert alle Gebäudebereiche mehrmals pro Schicht. Personal des Rechenzentrums prüft mit einem täglichen Kontrollgang, ob alle Brandschutzanforderungen erfüllt sind. <br /> Bereiche mit empfindlichen elektrischen Geräten (Serverhousings, MDFs, usw.) werden durch doppelt gesicherte, vorgesteuerte Sprinklersysteme (Trockenanlagen) geschützt. Trockenanlagensprinkler sind zweistufige vorgesteuerte Systeme, die sowohl eine (hitzebedingte) Aktivierung des Sprinklerkopfes als auch Raucherkennung zur Wasserabgabe erfordern. Bei Aktivierung des Sprinklerkopfes wird der Luftdruck in den Rohren reduziert, sodass diese mit Wasser gefüllt werden können. Wasser wird abgegeben, wenn ein Rauch- oder Hitzemelder ebenfalls aktiviert wird. <br /> Branderkennungs-/Brandbekämpfungs- und Notbeleuchtungssysteme sind mit den USV- und Generatorsystemen des Rechenzentrums verbunden, die für eine redundante Stromversorgung sorgen. |


 ### <a name="nist-800-53-control-pe-13-1"></a>NIST 800-53, Regelung PE-13 (1)

#### <a name="fire-protection--detection-devices--systems"></a>Brandschutz | Erkennungsgeräte / -systeme

**PE-13 (1)** Die Organisation verwendet Branderkennungsgeräte/-systeme für das Informationssystem, die bei einem Brand automatisch aktiviert werden und [Zuordnung: von der Organisation definierte Personen oder Rollen] und [Zuordnung: von der Organisation definierte Notfallresponder] benachrichtigen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure verwendet Branderkennungsgeräte/-systeme für das Informationssystem, die bei einem Brand automatisch aktiviert werden und Personal des Rechenzentrums sowie Notfallresponder benachrichtigen. Wenn ein Branderkennungsmechanismus in einem Serverhousing aktiviert wird, wird die örtliche Feuerwache automatisch über das Feueralarmsystem benachrichtigt. Darüber hinaus sind die Brandschutz- und Branderkennungssysteme in das Sicherheitssystem eingebunden, das Mitarbeiter und Sicherheitspersonal des lokalen Standorts benachrichtigt. |


 ### <a name="nist-800-53-control-pe-13-2"></a>NIST 800-53, Regelung PE-13 (2)

#### <a name="fire-protection--suppression-devices--systems"></a>Brandschutz | Bekämpfungsgeräte / -systeme

**PE-13 (2)** Die Organisation verwendet Brandbekämpfungsgeräte/-systeme für das Informationssystem, die bei einer Aktivierung automatisch [Zuordnung: von der Organisation definierte Personen oder Rollen] und [Zuordnung: von der Organisation definierte Notfallresponder] benachrichtigen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Wenn ein Brandbekämpfungssystem in einem Rechenzentrum aktiviert wird, wird die örtliche Feuerwache automatisch über das Feueralarmsystem benachrichtigt. Darüber hinaus sind die Brandschutz- und Branderkennungssysteme in das Sicherheitssystem eingebunden, das Mitarbeiter und Sicherheitspersonal des lokalen Standorts benachrichtigt. |


 ### <a name="nist-800-53-control-pe-13-3"></a>NIST 800-53, Regelung PE-13 (3)

#### <a name="fire-protection--automatic-fire-suppression"></a>Brandschutz | Automatische Brandbekämpfung

**PE-13 (3)** Die Organisation verwendet eine automatische Brandbekämpfungsfunktion für das Informationssystem, wenn in dem Gebäude nicht kontinuierlich Personal anwesend ist.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. In Microsoft Azure-Rechenzentren ist 24 Stunden täglich Personal anwesend. Brandbekämpfungssysteme werden automatisch ohne manuellen Eingriff aktiviert, wenn ein Feueralarm ausgelöst wird. |


 ## <a name="nist-800-53-control-pe-14a"></a>NIST 800-53, Regelung PE-14.a

#### <a name="temperature-and-humidity-controls"></a>Temperatur- und Luftfeuchtigkeitssteuerung

**PE-14.a** Die Organisation sorgt dafür, dass die Temperatur- und Luftfeuchtigkeitswerte in dem Gebäude, wo sich das Informationssystem befindet, bei [Zuordnung: von der Organisation definierte akzeptable Werte] liegen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure hält die Temperatur- und Luftfeuchtigkeitswerte in Übereinstimmung mit den Richtlinien der American Society of Heating, Refrigerating and Air-conditioning Engineers (ASHRAE – amerikanische Gesellschaft für Heizungs-, Kühlanlagen- und Klimaanlageningenieure) ein. Die Temperatur- und Luftfeuchtigkeitswerte werden durch das Building Management System (BMS) des Rechenzentrums kontinuierlich überwacht. |


 ## <a name="nist-800-53-control-pe-14b"></a>NIST 800-53, Regelung PE-14.b

#### <a name="temperature-and-humidity-controls"></a>Temperatur- und Luftfeuchtigkeitssteuerung

**PE-14.c** Die Organisation überwacht die Temperatur- und Luftfeuchtigkeitswerte [Festsetzung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. In Microsoft Azure-Rechenzentren werden die Temperatur- und Luftfeuchtigkeitswerte kontinuierlich durch das Building Management System (BMS) überwacht. Mitarbeiter des Rechenzentrums überwachen das BMS über das Facilities Operations Center (FOC), damit sie Temperatur und Luftfeuchtigkeit im Rechenzentrum regeln können, bevor Alarmpunkte überschritten werden. |


 ### <a name="nist-800-53-control-pe-14-2"></a>NIST 800-53, Regelung PE-14 (2)

#### <a name="temperature-and-humidity-controls--monitoring-with-alarms--notifications"></a>Temperatur- und Luftfeuchtigkeitssteuerung | Überwachung mit Alarmen / Benachrichtigungen

**PE-14 (2)** Die Organisation setzt eine Temperatur- und Luftfeuchtigkeitsüberwachung ein, die bei Änderungen, die für Personal oder Geräte potenziell schädlich sein können, einen Alarm oder eine Benachrichtigung ausgibt.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. In Microsoft Azure-Rechenzentren werden die Temperatur- und Luftfeuchtigkeitswerte kontinuierlich durch das Building Management System (BMS) überwacht. Mitarbeiter des Rechenzentrums überwachen das BMS über das Facilities Operations Center (FOC), damit sie Temperatur und Luftfeuchtigkeit im Rechenzentrum regeln können, bevor Alarmpunkte überschritten werden. |


 ## <a name="nist-800-53-control-pe-15"></a>NIST 800-53, Regelung PE-15

#### <a name="water-damage-protection"></a>Wasserschadenschutz

**PE-15** Die Organisation schützt das Informationssystem durch eine Hauptabschaltung oder zugängliche, ordnungsgemäß funktionierende und den verantwortlichen Mitarbeitern bekannte Absperrventile vor Wasserschäden.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure bietet eine Wasser-/Leckerkennung in Bereichen mit hohem Wasserleckagenrisiko (z.B. Luftversorgungseinheiten). Brandbekämpfungssysteme können auch beim Auftreten von Lecks Alarme auslösen. Das Wasser-/Leckerkennungssystem ist in das Alarm- und Benachrichtigungssystem des Gebäudes integriert. Die Sprinklersysteme in den Rechenzentren sind in Zonen eingeteilt. Das Personal des Rechenzentrums ist mit Notfallverfahren vertraut, kennt die Position der Absperrventile und weiß, wie sie betätigt werden. Die Sprinklersteigleitungen können einzeln oder als Gruppe über Absperrventile geschlossen werden. Alle Sprinkler im kritischen Bereich sind doppelt gesicherte, vorgesteuerte Sprinklersysteme, die zwei Formen der Aktivierung erfordern, bevor Wasser abgegeben wird. Der Druck des Sprinklersystems wird überwacht, und bei Wasserleckagen wird ein Alarm ausgegeben. |


 ### <a name="nist-800-53-control-pe-15-1"></a>NIST 800-53, Regelung PE-15 (1)

#### <a name="water-damage-protection--automation-support"></a>Wasserschadenschutz | Automatisierungsunterstützung

**PE-15 (1)** Die Organisation wendet automatisierte Mechanismen an, um zu erkennen, ob in der Nähe des Informationssystems Wasser ausgetreten ist, und warnt [Zuordnung: von der Organisation definierte Personen oder Rollen].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure wendet automatisierte Mechanismen an, um zu erkennen, ob Wasser im Rechenzentrum ausgetreten ist, und warnt das Personal des Rechenzentrums. Azure bietet eine Wasser-/Leckerkennung in Bereichen mit hohem Wasserleckagenrisiko (z.B. Luftversorgungseinheiten). Brandbekämpfungssysteme können auch beim Auftreten von Lecks Alarme auslösen. Das Wasser-/Leckerkennungssystem ist in das Alarm- und Benachrichtigungssystem des Gebäudes integriert. Der Druck des Sprinklersystems wird überwacht, und bei Wasserleckagen wird ein Alarm ausgegeben. |


 ## <a name="nist-800-53-control-pe-16"></a>NIST 800-53, Regelung PE-16

#### <a name="delivery-and-removal"></a>Lieferung und Entfernung

**PE-16** Die Organisation autorisiert, überwacht und steuert [Zuordnung: von der Organisation definierte Typen von Informationssystemkomponenten], die in das Gebäude eingebracht und daraus entfernt werden, und verwaltet Datensätze dieser Elemente.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure implementiert diese Anforderung im Interesse der Kunden. Microsoft Azure implementiert die strenge Durchsetzung der Vorschriften, die bestimmen, welche Gegenstände in das Rechenzentrum eingebracht und daraus entfernt werden dürfen. Alle Systemkomponenten/Objekte werden in der Asset Management-Tool-Datenbank nachverfolgt. |


 ## <a name="nist-800-53-control-pe-17a"></a>NIST 800-53, Regelung PE-17.a

#### <a name="alternate-work-site"></a>Alternativer Arbeitsort

**PE-17.a** Die Organisation verwendet [Zuordnung: von der Organisation definierte Sicherheitssteuerungen] an alternativen Arbeitsorten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Bereitstellungen alternativer Arbeitsorte des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung gerecht zu werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pe-17b"></a>NIST 800-53, Regelung PE-17.b

#### <a name="alternate-work-site"></a>Alternativer Arbeitsort

**PE-17.b** Die Organisation bewertet so gut wie möglich die Wirksamkeit von Sicherheitssteuerungen an alternativen Arbeitsorten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Bereitstellungen alternativer Arbeitsorte des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung gerecht zu werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pe-17c"></a>NIST 800-53, Regelung PE-17.c

#### <a name="alternate-work-site"></a>Alternativer Arbeitsort

**PE-17.c** Die Organisation bietet Mitarbeitern die Möglichkeit, bei Sicherheitsvorfällen oder Problemen mit Informationssicherheitspersonal zu kommunizieren.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Bereitstellungen alternativer Arbeitsorte des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung gerecht zu werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-pe-18"></a>NIST 800-53, Regelung PE-18

#### <a name="location-of-information-system-components"></a>Position der Informationssystemkomponenten

**PE-18** Die Organisation positioniert Informationssystemkomponenten innerhalb des Gebäudes, um potenzielle Schäden durch [Zuordnung: von der Organisation definierte physische und Umgebungsgefahren] und die Möglichkeit des nicht autorisierten Zugriffs zu minimieren.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen physischen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Azure implementiert einen strategischen Rechenzentrumsentwurfsansatz, um der Regelung für die Position der Informationssystemkomponenten gerecht zu werden. Alle Geräte der Onlinedienste von Microsoft befinden sich in Bereichen, die so konzipiert wurden, dass sie vor Umgebungsrisiken wie Diebstahl, Feuer, Explosivstoffen, Rauch, Wasser, Staub, Vibrationen, Erdbeben, schädlichen Chemikalien, elektrischen Störungen, Stromausfällen, elektrischen Störgrößen (Spitzen) und Strahlung geschützt sind. In Gebäude und Infrastruktur sind Absteifungen als Erdbebenschutz implementiert. Alle Serverhousings und MDF-Räume werden durch Zugriffssteuerung, Alarme und Video geschützt. Das Gebäude wird auch 24 Stunden täglich von Sicherheitspersonal kontrolliert. Alle tragbaren Azure-Ressourcen sind zum Schutz vor Diebstahl oder Schäden durch Bewegung an ihrem Standort verriegelt oder befestigt. |


