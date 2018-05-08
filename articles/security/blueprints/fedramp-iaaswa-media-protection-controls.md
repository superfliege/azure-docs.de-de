---
title: Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP – Medienschutz
description: Automatisierung von Webanwendungen für FedRAMP – Medienschutz
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: fe86fd92-ef6b-4d17-a4a2-de6796d251d0
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 37812c2f7ee79685f9014a7999b4355e649ca6e1
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
---
# <a name="media-protection-mp"></a>Medienschutz (Media Protection, MP)

> [!NOTE]
> Diese Regelungen werden von NIST und dem U.S. Department of Commerce als Teil der NIST-Sonderveröffentlichung 800-53 Revision 4 definiert. Informationen zu Testverfahren und Anleitungen für jede Regelung finden Sie unter NIST 800-53 Rev. 4.

## <a name="nist-800-53-control-mp-1"></a>NIST 800-53, Regelung MP-1

#### <a name="media-protection-policy-and-procedures"></a>Richtlinie und Verfahren zum Medienschutz

**MP-1** Die Organisation entwickelt, dokumentiert und verteilt an [Zuordnung: von der Organisation definierte Personen oder Rollen] eine Medienschutzrichtlinie, die sich mit Zweck, Umfang, Rollen, Zuständigkeiten, Managementverpflichtung, Koordination zwischen Organisationsentitäten und Konformität befasst, stellt Verfahren zum Ermöglichen der Implementierung der Medienschutzrichtlinie und der zugehörigen Regulierungen für die Medienschutzrichtlinie bereit und überprüft und aktualisiert die aktuelle Medienschutzrichtlinie [Zuordnung: von der Organisation definierte Häufigkeit] sowie die Verfahren für die Medienschutzrichtlinie [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Medienschutzrichtlinie und -verfahren des Kunden auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-mp-2"></a>NIST 800-53, Regelung MP-2

#### <a name="media-access"></a>Medienzugriff

**MP-2** Die Organisation schränkt den Zugriff auf [Zuordnung: von der Organisation definierte Typen von digitalen und/oder nicht digitalen Medien] auf [Zuordnung: von der Organisation definierte Personen oder Rollen] ein.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure hat Medienzugriff über die Implementierung der Microsoft-Sicherheitsrichtlinie implementiert. Logischer Zugriff auf digitale Medien wird über Active Directory-Gruppenrichtlinienobjekte (Active Directory Group Policy Objects, AD GPOs) und Sicherheitsgruppen gesteuert. Physischer Zugriff auf alle Medien wird durch den Prozess des Rechenzentrumszugriffs beschränkt. Der Zugriff ist auf Personen beschränkt, die zu legitimem geschäftlichen Zwecken auf die Daten zugreifen. Weitere Informationen über die implementierten Rechenzentrums-Zugriffssteuerungen finden Sie unter PE-3, Steuerung des physischen Zugriffs. Der Ressourcenschutz-Standard definiert die erforderlichen Schutzvorrichtungen zum Schutz von Vertraulichkeit, Integrität und Verfügbarkeit von Informationsressourcen in Microsoft Azure-Rechenzentren. |


 ## <a name="nist-800-53-control-mp-3a"></a>NIST 800-53, Regelung MP-3.a

#### <a name="media-marking"></a>Medienkennzeichnung

**MP-3.a** Die Organisation kennzeichnet die Informationssystemmedien mit Angabe von Verteilungseinschränkungen, Handhabungsvorbehalten und (ggf.) anwendbaren Sicherheitskennzeichnungen der Informationen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure markiert Ressourcen in Microsoft-Rechenzentren mit einer HBI-, MBI- oder LBI-Kennzeichnung (High, Moderate, Low Business Impact – hohe, mäßige, niedrige Auswirkungen auf das Unternehmen), die verschiedene Ebenen der Sicherheits- und Handhabungsvorsichtsmaßnahmen erfordert. Ressourcenbesitzer müssen ihre Ressourcen klassifizieren, die in einem Microsoft-Rechenzentrum gespeichert sind. Weitere Informationen finden Sie im Ressourcenklassifizierungs- und Ressourcenschutzstandard. |


 ## <a name="nist-800-53-control-mp-3b"></a>NIST 800-53, Regelung MP-3.b

#### <a name="media-marking"></a>Medienkennzeichnung

**MP-3.b** Die Organisation nimmt [Zuordnung: von der Organisation definierte Informationssystem-Medientypen] von der Kennzeichnung aus, solange das Medium innerhalb von [Zuordnung: von der Organisation definierte kontrollierte Bereiche] bleibt.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure setzt voraus, dass Ressourcenbesitzer ihren Ressourcen eine Ressourcenklassifizierung zuweisen, und keine Ressourcen sind von dieser Anforderung ausgenommen. In der Microsoft-Rechenzentrumsumgebung gelten Server, Netzwerkgeräte und Magnetbänder als Ressourcen. Andere digitale Medien wie USB-Flashlaufwerke/-Speichersticks, externe/Wechsel-Festplattenlaufwerke oder CDs/DVDs werden nicht verwendet. Nicht digitale Medien werden im Rechenzentrum nicht verwendet. |


 ## <a name="nist-800-53-control-mp-4a"></a>NIST 800-53, Regelung MP-4.a

#### <a name="media-storage"></a>Medienspeicher

**MP-4.a** Die Organisation steuert physisch und speichert sicher [Zuordnung: von der Organisation definierte Typen von digitalen und/oder nicht digitalen Medien] in [Zuordnung: von der Organisation definierte kontrollierte Bereiche].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Digitale Microsoft Azure-Medienressourcen werden physisch und sicher in Serverhousing-Räumen des Rechenzentrums gespeichert. Microsoft-Rechenzentren weisen mehrere Ebenen von physischen Zugriffssteuerungen (Zugriffsbadge, Biometrie; weitere Informationen zu physischen Zugriffssteuerungen siehe PE-3) und Videoüberwachung auf, um die sichere Speicherung zu gewährleisten. Zu den digitalen Medien gehören Server, Netzwerkgeräte und Magnetbänder, die für die Sicherung verwendet werden. Nicht digitale Medien werden in der Rechenzentrumsumgebung nicht verwendet. |


 ## <a name="nist-800-53-control-mp-4b"></a>NIST 800-53, Regelung MP-4.b

#### <a name="media-storage"></a>Medienspeicher

**MP-4.b** Die Organisation schützt Informationssystemmedien, bis die Medien mit genehmigten Arbeitsgeräten, Techniken und Verfahren zerstört oder bereinigt werden.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Digitale Microsoft Azure-Medien werden in Serverhousing-Räumen des Microsoft-Rechenzentrums durch physische Zugriffssteuerungen (PE-3) und logische Zugriffssteuerungen (IA-2) während Ihrer Lebensdauer geschützt. Microsoft Azure-Ressourcen werden vor der Entsorgung mit Methoden deaktiviert, gelöscht oder zerstört, die NIST SP 800-88 entsprechen. Microsoft Azure setzt zur Ressourcenzerstörung vor Ort entsprechende Dienste ein. |


 ## <a name="nist-800-53-control-mp-5a"></a>NIST 800-53, Regelung MP-5.a

#### <a name="media-transport"></a>Medientransport

**MP-5.a** Die Organisation schützt und steuert [Zuordnung: von der Organisation definierte Informationssystem-Medientypen] während des Transports außerhalb kontrollierter Bereiche mit [Zuordnung: von der Organisation definierte Schutzvorrichtungen].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Digitale Microsoft Azure-Medien in Microsoft-Rechenzentren sind Server, Netzwerkgeräte und magnetische Sicherungsbänder sowie ggf. Discs. Microsoft-Rechenzentren verwenden keine nicht digitalen Medien. Microsoft nutzt drei Methoden zum Schutz von Medien, die außerhalb des Rechenzentrums transportiert werden: 1) sicherer Transport, 2) Verschlüsselung, 3) Bereinigen, Löschen oder Zerstören. |


 ## <a name="nist-800-53-control-mp-5b"></a>NIST 800-53, Regelung MP-5.b

#### <a name="media-transport"></a>Medientransport

**MP-5.b** Die Verantwortlichkeit für Informationssystemmedien liegt während des Transports außerhalb kontrollierter Bereiche bei der Organisation.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure wendet die Anleitung von NIST SP 800-88 an, um die Verantwortlichkeit für Ressourcen wahrzunehmen, die das Rechenzentrum verlassen: konsistentes Bereinigen/Löschen, Zerstörung der Ressource, Verschlüsselung, genaue Bestandsaufnahme, Überwachung und Schutz der Verwahrungskette während des Transports. |


 ## <a name="nist-800-53-control-mp-5c"></a>NIST 800-53, Regelung MP-5.c

#### <a name="media-transport"></a>Medientransport

**MP-5.c** Die Organisation dokumentiert die Aktivitäten im Zusammenhang mit dem Transport von Informationssystemmedien.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure verwaltet die Bestandsaufnahme vor dem Transport, Überwachung und Schutz der Verwahrungskette während des Transports, Bereinigen/Löschen der Ressource, Zerstörung der Ressource, Empfang der Ressourcen und Bestandsüberprüfung nach dem Transport. |


 ## <a name="nist-800-53-control-mp-5d"></a>NIST 800-53, Regelung MP-5.d

#### <a name="media-transport"></a>Medientransport

**MP-5.d** Die Organisation beschränkt die Aktivitäten im Zusammenhang mit dem Transport von Informationssystemmedien auf autorisierte Personen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure beschränkt die Aktivitäten des Ressourcentransports durch den Schutz der Verwahrungskette auf entsprechend berechtigte Mitarbeiter. Die Verwendung von Sperren, manipulationssichere Siegel und die obligatorische Überprüfung des Ressourcenbestandes stellen sicher, dass nur autorisierte Personen am Ressourcentransport beteiligt sind. |


 ### <a name="nist-800-53-control-mp-5-4"></a>NIST 800-53, Regelung MP-5 (4)

#### <a name="media-transport--cryptographic-protection"></a>Medientransport | Kryptografischer Schutz

**MP-5 (4)** Das Informationssystem implementiert kryptografische Mechanismen, um Vertraulichkeit und Integrität der Informationen, die auf digitalen Medien gespeichert sind, während des Transports außerhalb kontrollierter Bereiche zu schützen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure verwendet den Data Protection Service (DPS), um kryptografische Schlüssel mit einem FIPS 140-2 Level 3-validierten Verschlüsselungsmodul (Zert. 1694) und HSM (Zert. 1178) zu verwalten, um Daten mit AES 256-Bit-Verschlüsselung auf den Magnetbändern zu sichern. |


 ## <a name="nist-800-53-control-mp-6a"></a>NIST 800-53, Regelung MP-6.a

#### <a name="media-sanitization"></a>Medienbereinigung

**MP-6.a** Die Organisation bereinigt [Zuordnung: von der Organisation definierte Informationssystemmedien] vor der Entsorgung, Freigabe aus der Kontrolle der Organisation oder Freigabe zur Wiederverwendung mit [Zuordnung: von der Organisation definierte Bereinigungstechniken und -verfahren] in Übereinstimmung mit den entsprechenden Standards und Richtlinien von Rechtsprechung und Organisation.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure setzt voraus, dass digitale Medien in der Microsoft Azure-Rechenzentrumsumgebung mit von Microsoft Azure genehmigten Tools und in Übereinstimmung mit NIST SP 800-88, Richtlinien für die Medienbereinigung, vor der Wiederverwendung oder Entsorgung bereinigt/gelöscht werden. Nicht digitale Medien werden in der Microsoft Azure-Rechenzentrumsumgebung nicht verwendet. |


 ## <a name="nist-800-53-control-mp-6b"></a>NIST 800-53, Regelung MP-6.b

#### <a name="media-sanitization"></a>Medienbereinigung

**MP-6.b** Die Organisation wendet Bereinigungsmechanismen mit der Stärke und Integrität an, die der Sicherheitskategorie oder -klassifizierung der Informationen angemessen ist.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure verwendet Datenlöscheinheiten und -prozesse, um Daten in Übereinstimmung mit NIST SP 800-88 und der Microsoft Azure-Ressourcenklassifizierung der Ressource zu bereinigen/löschen. Für Ressourcen, die zerstört werden müssen, setzt Microsoft Azure vor Ort entsprechende Dienste ein. |


 ### <a name="nist-800-53-control-mp-6-1"></a>NIST 800-53, Regelung MP-6 (1)

#### <a name="media-sanitization--review--approve--track--document--verify"></a>Medienbereinigung | Überprüfen / Genehmigen / Überwachen / Dokumentieren / Verifizieren

**MP-6 (1)** Die Organisation überprüft, genehmigt, überwacht, dokumentiert und verifiziert die Medienbereinigungs- und Entsorgungsaktionen.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | In Microsoft Azure sind gemäß der Anleitung in NIST SP 800-88 für Ressourcenklassifizierungs- und Ressourcenschutzstandard Verfahren für die Medienbereinigung implementiert. Alle magnetischen oder elektronischen Medien werden gemäß der NIST SP 800-88-Spezifikationen in Übereinstimmung mit der Azure-Ressourcenklassifizierung bereinigt/gelöscht. Azure nutzt Datenlöscheinheiten aus Extreme Protocol Solutions (EPS). EPS-Software unterstützt die Anforderungen von NIST SP 800-88 für Bereinigung und Löschen/sicheres Löschen. |


 ### <a name="nist-800-53-control-mp-6-2"></a>NIST 800-53, Regelung MP-6 (2)

#### <a name="media-sanitization--equipment-testing"></a>Medienbereinigung | Testen von Arbeitsgeräten

**MP-6 (2)** Die Organisation testet Arbeitsgeräte zur Bereinigung und Verfahren [Zuordnung: von der Organisation definierte Häufigkeit], um sicherzustellen, dass die beabsichtigte Bereinigung erzielt wird.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure nutzt Datenlöscheinheiten und Prozesse zum Bereinigen/Löschen von Daten in Übereinstimmung mit NIST SP 800-88. Alle 180 Tage testet „DCS operations“ die Microsoft Azure-Datenlöscheinheiten und den Löschprozess. Im Test überprüft „DCS operations“ durch eine forensische Analyse der getesteten Festplatten, ob die vorgesehene Bereinigung erreicht wurde, um sicherzustellen, dass die Daten von den Datenlöscheinheiten bereinigt wurden. |


 ### <a name="nist-800-53-control-mp-6-3"></a>NIST 800-53, Regelung MP-6 (3)

#### <a name="media-sanitization--nondestructive-techniques"></a>Medienbereinigung | Nicht zerstörerische Techniken

**MP-6 (3)** Die Organisation wendet unter den folgenden Umständen nicht zerstörerische Bereinigungstechniken auf tragbare Speichergeräte an, bevor eine Verbindung solcher Geräte mit dem Informationssystem hergestellt wird: [Zuordnung: durch die Organisation definierte Umstände, die eine Bereinigung tragbarer Speichergeräte erfordern].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure stellt sicher, dass Azure-Rechenzentren das Sicherheitsverfahren für Tools und Wechselmedien im Data Center Services Run Book ausführen, um die Infektion der Behördenumgebung durch Schadsoftware auf tragbaren Speichergeräten zu verhindern. Das Verfahren gibt an, dass an USB-Laufwerken vor der Verwendung in einer Behördenumgebung die folgenden Aktionen ausgeführt werden: <br /> (1) Formatieren der USB-Laufwerke direkt nach Erwerb der Laufwerke vom Hersteller oder Händler, vor der ersten Verwendung oder bei Wiederverwendung für ein anderes Tool. <br /> (2) Überprüfen aller USB-Laufwerke auf Schadsoftware, die in einer Behördenumgebung verwendet werden sollen, bevor das Laufwerk in den Bereich eingebracht wird. <br /> (3) Formatieren eines Laufwerks nach Verwendung in einer Behördenumgebung, bevor es den Bereich verlässt. <br /> Das Sicherheitsverfahren für Tools und Wechselmedien setzt auch voraus, dass alle verloren gegangenen, verworfenen, gestohlenen oder falsch platzierten USB-Speichersticks in keinem Fall wieder in Azure-Rechenzentren eingeführt, sondern katalogisiert und zerstört werden. |


 ## <a name="nist-800-53-control-mp-7"></a>NIST 800-53, Regelung MP-7

#### <a name="media-use"></a>Verwenden von Medien

**MP-7** Die Organisation [Auswahl: beschränkt; verhindert] die Verwendung von [Zuordnung: von der Organisation definierte Informationssystem-Medientypen] auf [Zuordnung: von der Organisation definierte Informationssysteme oder Systemkomponenten] mit [Zuordnung: von der Organisation definierte Schutzvorrichtungen].

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure setzt voraus, dass Ressourcenbesitzer ihren Ressourcen eine Ressourcenklassifizierung zuweisen, und keine Ressourcen sind von dieser Anforderung ausgenommen. In der Microsoft Azure-Rechenzentrumsumgebung gelten Server und Netzwerkgeräte als Ressourcen. Die Verwaltung anderer digitaler Medien wie USB-Flashlaufwerke/-Speichersticks erfolgt gemäß bestimmter Richtlinien und Verfahren. CDs/DVDs werden nicht verwendet. Nicht digitale Medien werden im Rechenzentrum nicht verwendet. Die Verwendung von digitalen Medien in Microsoft Azure-Rechenzentren wird 24 Stunden täglich mit Kameras überwacht. Weitere Informationen siehe PE-06. |


 ### <a name="nist-800-53-control-mp-7-1"></a>NIST 800-53, Regelung MP-7 (1)

#### <a name="media-use--prohibit-use-without-owner"></a>Verwenden der Medien | Verhindern der Verwendung ohne Besitzer

**MP-7 (1)** Die Organisation verhindert die Verwendung von tragbaren Speichergeräten in Organisationsinformationssystemen, wenn für solche Geräte kein Besitzer identifizierbar ist.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Es gehören keine kundenseitig gesteuerten Medien zum Geltungsbereich der in Azure bereitgestellten Systeme. |
| **Anbieter (Microsoft Azure)** | Microsoft beschränkt die Verwendung von beschreibbaren Wechselmedien auf Medien, die explizit von der Verwaltung des Rechenzentrums mithilfe des DCS-Verfahrens für Tools und Wechselmedien genehmigt wurden. Wie im Dokument der Arbeitsregeln und -bestimmungen für Microsoft-Rechenzentren festgelegt, sind Medien, die sich in persönlichem Besitz befinden, oder für die kein Besitzer identifizierbar ist, in keinem Produktionsbereich zugelassen. |
