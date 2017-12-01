---
title: "Blueprint zur Azure-Zahlungsverarbeitung – Richtlinienanforderungen"
description: PCI-DSS-Anforderung 12
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: a79d59d8-20e3-4efe-8686-c8f4ed80e220
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 05e9ed7c886d37a024db1eedbc541705b7d8a9a9
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="policy-requirements-for-pci-dss-compliant-environments"></a>Richtlinienanforderungen für PCI-DSS-konforme Umgebungen  
## <a name="pci-dss-requirement-12"></a>PCI-DSS-Anforderung 12

**Verwalten einer Richtlinie, die die IT-Sicherheit für alle Mitarbeiter regelt**

> [!NOTE]
> Diese Anforderungen werden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

Eine strikte Sicherheitsrichtlinie setzt einen Sicherheitsmaßstab für die gesamte Entität und informiert Mitarbeiter, was von ihnen erwartet wird. Alle Mitarbeiter müssen sich der Vertraulichkeit von Daten und ihrer Zuständigkeiten für deren Schutz bewusst sein. In Anforderung 12 bezieht sich „Mitarbeiter“ auf Vollzeit- und Teilzeitmitarbeiter, temporäre Mitarbeiter, Auftragnehmer und Berater, die sich auf dem Gelände der Entität aufhalten oder anderweitig Zugriff auf die Karteninhaberdaten-Umgebung haben.

## <a name="pci-dss-requirement-121"></a>PCI-DSS-Anforderung 12.1

**12.1** Einrichten, Veröffentlichen, Verwalten und Verbreiter einer Sicherheitsrichtlinie

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Einrichten und Verwalten einer Informationssicherheitsrichtlinie verantwortlich.|



### <a name="pci-dss-requirement-1211"></a>PCI-DSS-Anforderung 12.1.1

**12.1.1** Überprüfen Sie die Sicherheitsrichtlinie mindestens einmal jährlich und aktualisieren Sie sie bei einer Änderung der Umgebung.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind dafür verantwortlich, ihre Informationssicherheitsrichtlinie mindestens einmal jährlich oder bei Änderungen an ihrer Karteninhaberdaten-Umgebung (Cardholder Data Environment, CDE) zu aktualisieren.|



## <a name="pci-dss-requirement-122"></a>PCI-DSS-Anforderung 12.2

**12.2** Implementieren Sie einen Risikobewertungsprozess, der:
- Mindestens einmal jährlich und nach erheblichen Änderungen an der Umgebung (z. B. Übernahme, Fusion, Standortwechsel usw.) ausgeführt wird
- Kritische Ressourcen, Bedrohungen und Sicherheitsrisiken identifiziert
- Zu einer formalen, dokumentierten Analyse von Risiken führt.
- > Beispiele für Risikobewertungsmethoden sind u. a. OCTAVE, ISO 27005 und NIST SP 800-30.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Implementierung eines Risikobewertungsprozesses verantwortlich, der alle in Anforderung 12.2 aufgelisteten Bedrohungen berücksichtigt.|



## <a name="pci-dss-requirement-123"></a>PCI-DSS-Anforderung 12.3

**12.3** Entwickeln Sie Nutzungsrichtlinien für wichtige Technologien, und definieren Sie die ordnungsgemäße Verwendung dieser Technologien.

> [!NOTE]
> Beispiele für wichtige Technologien sind u. a. Remotezugriff und Funktechnologien, Laptops, Tablets, elektronische Wechselmedien, E-Mail-Nutzung und Internetzugriff.
Stellen Sie sicher, dass diese Nutzungsrichtlinien Folgendes erforderlich machen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



### <a name="pci-dss-requirement-1231"></a>PCI-DSS-Anforderung 12.3.1

**12.3.1** Explizite Genehmigung durch autorisierte Parteien

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



### <a name="pci-dss-requirement-1232"></a>PCI-DSS-Anforderung 12.3.2

**12.3.2** Authentifizierung für die Verwendung der Technologie

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



### <a name="pci-dss-requirement-1233"></a>PCI-DSS-Anforderung 12.3.3

**12.3.3** Eine Liste solcher Geräte und der Mitarbeiter mit Zugriff darauf

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



### <a name="pci-dss-requirement-1234"></a>PCI-DSS-Anforderung 12.3.4

**12.3.4** Eine Methode zur genauen und problemlosen Ermittlung von Besitzer, Kontaktinformationen und Zweck (z. B. Beschriftung, Codieren und/oder Bestandsaufnahme von Geräten)

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



### <a name="pci-dss-requirement-1235"></a>PCI-DSS-Anforderung 12.3.5

**12.3.5** Akzeptable Anwendungszwecke der Technologie

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



### <a name="pci-dss-requirement-1236"></a>PCI-DSS-Anforderung 12.3.6

**12.3.6** Akzeptable Netzwerkorte für die Technologien

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Ermitteln geeigneter Netzwerkadressen für cloudbasierte virtuelle Computer, Speicher- und Unterstützungsdienste verantwortlich.|



### <a name="pci-dss-requirement-1237"></a>PCI-DSS-Anforderung 12.3.7

**12.3.7** Liste der vom Unternehmen genehmigten Produkte

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Ermitteln geeigneter Netzwerkadressen für cloudbasierte virtuelle Computer, Speicher- und Unterstützungsdienste verantwortlich.|



### <a name="pci-dss-requirement-1238"></a>PCI-DSS-Anforderung 12.3.8

**12.3.8** Automatische Trennung von Sitzungen für Remotezugriffstechnologien nach einer gewissen Zeit der Inaktivität

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure nutzt die Funktion für das Sperren von AD-Sitzungen von Microsoft für Unternehmen, die nach einer gewissen Zeit der Inaktivität Sitzungssperrungen erzwingt. Netzwerkverbindungen werden nach 30 Minuten der Inaktivität beendet. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



### <a name="pci-dss-requirement-1239"></a>PCI-DSS-Anforderung 12.3.9

**12.3.9** Aktivierung von Remotezugriffstechnologien für Anbieter und Geschäftspartner nur bei Bedarf seitens der Lieferanten und Geschäftspartner und sofortige Deaktivierung nach der Verwendung

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



### <a name="pci-dss-requirement-12310"></a>PCI-DSS-Anforderung 12.3.10

**12.3.10** Verhindern Sie, dass Mitarbeiter, die über Remotezugriffstechnologien auf Daten von Karteninhabern zugreifen, diese Daten auf lokale Festplatten und elektronische Wechselmedien kopieren oder darauf speichern, sofern dies nicht für eine definierte geschäftliche Anforderung explizit autorisiert ist.
Liegt eine autorisierte geschäftliche Anforderung vor, müssen die Nutzungsrichtlinien erfordern, dass die Daten in Übereinstimmung mit allen geltenden PCI-DSS-Anforderungen geschützt werden.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind dafür verantwortlich, zu verhindern, dass Mitarbeiter, die über Remotezugriffstechnologien auf Daten von Karteninhabern zugreifen, diese Daten auf lokale Festplatten und elektronische Wechselmedien kopieren oder darauf speichern, sofern dies nicht für eine definierte geschäftliche Anforderung explizit autorisiert ist.|



## <a name="pci-dss-requirement-124"></a>PCI-DSS-Anforderung 12.4

**12.4** Stellen Sie sicher, dass die Sicherheitsrichtlinie und die Prozeduren die Informationssicherheitsaufgaben für alle Mitarbeiter klar definieren.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



### <a name="pci-dss-requirement-1241"></a>PCI-DSS-Anforderung 12.4.1

**12.4.1** Zusätzliche Anforderung nur für Dienstanbieter: Die Geschäftsleitung muss die Zuständigkeit für den Schutz von Daten von Karteninhabern begründen und ein PCI-DSS-Konformitätsprogramm implementieren, das Folgendes umfasst:
- Allgemeine Verantwortlichkeit für die Aufrechterhaltung der PCI-DSS-Konformität
- Definieren einer Charta für ein PCI-DSS-Konformitätsprogramm und Information der Geschäftsführung 

> [!NOTE]
> Diese Anforderung ist bis zum 31 Januar 2018 eine bewährte Methode, danach ist Sie eine Anforderung.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden, bei denen es sich um Dienstanbieter handelt, sind für das Dokumentieren ihres PCI-Konformitätsprogramms verantwortlich.|



## <a name="pci-dss-requirement-125"></a>PCI-DSS-Anforderung 12.5

**12.5** Weisen Sie Einzelperson oder einem Team die folgenden Aufgaben zur Verwaltung der Informationssicherheit zu.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Definieren und Zuweisen von Informationssicherheitsaufgaben an ihre Mitarbeiter verantwortlich.|



### <a name="pci-dss-requirement-1251"></a>PCI-DSS-Anforderung 12.5.1

**12.5.1** Einrichten, Dokumentieren und Verteilen von Sicherheitsrichtlinien und Prozeduren

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Definieren und Zuweisen von Informationssicherheitsaufgaben an ihre Mitarbeiter verantwortlich.|



### <a name="pci-dss-requirement-1252"></a>PCI-DSS-Anforderung 12.5.2

**12.5.2** Überwachen Sie Sicherheitsbenachrichtigungen und -informationen, und verteilen Sie sie an relevante Mitarbeiter.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Definieren und Zuweisen von Informationssicherheitsaufgaben an ihre Mitarbeiter verantwortlich.|



### <a name="pci-dss-requirement-1253"></a>PCI-DSS-Anforderung 12.5.3

**12.5.3** Einrichten, Dokumentieren und Verteilen von Prozeduren für die Reaktion auf Sicherheitsvorfälle und deren Eskalation, um eine zeitgerechte und effektive Handhabung aller Situationen sicherzustellen

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



### <a name="pci-dss-requirement-1254"></a>PCI-DSS-Anforderung 12.5.4

**12.5.4** Verwalten Sie Benutzerkonten, einschließlich Hinzufügungen, Löschungen und Änderungen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



### <a name="pci-dss-requirement-1255"></a>PCI-DSS-Anforderung 12.5.5

**12.5.5** Überwachen und steuern Sie den gesamten Zugriff auf Daten.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien mit Vorgaben für die ordnungsgemäße Verwendung, Implementierung und Authentifizierung wichtiger Technologien innerhalb ihrer CDE verantwortlich.|



## <a name="pci-dss-requirement-126"></a>PCI-DSS-Anforderung 12.6

**12.6** Implementieren Sie ein formales Programm zum Sicherheitsbewusstsein, um allen Mitarbeitern die Bedeutung von Sicherheitsrichtlinien und -verfahren für Karteninhaberdaten bewusst zu machen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Erstellen und Verwalten von Richtlinien rund um das Sicherheitsbewusstsein für Mitarbeiter mit Zugriff auf die CDE verantwortlich.|



### <a name="pci-dss-requirement-1261"></a>PCI-DSS-Anforderung 12.6.1

**12.6.1** Schulen Sie Mitarbeiter nach der Einstellung und mindestens einmal jährlich. 

> [!NOTE]
> Die Methoden können je nach Rolle und Zugriffsebene auf die Daten von Karteninhabern des Mitarbeiters variieren.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind verantwortlich für das Sicherstellen, dass sind Mitarbeiter mindestens einmal jährlich Informationssicherheits- und PCI-DSS-Bewusstseinsschulungen angeboten bekommen und wahrnehmen.|



### <a name="pci-dss-requirement-1262"></a>PCI-DSS-Anforderung 12.6.2

**12.6.2** Legen Sie fest, dass Mitarbeiter mindestens einmal jährlich bestätigen müssen, dass sie die Sicherheitsrichtlinie und -verfahren gelesen und verstanden haben.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind verantwortlich für das Sicherstellen, dass sind Mitarbeiter mindestens einmal jährlich Informationssicherheits- und PCI-DSS-Bewusstseinsschulungen angeboten bekommen und wahrnehmen.|



## <a name="pci-dss-requirement-127"></a>PCI-DSS-Anforderung 12.7

**12.7** Überprüfen Sie potenzielle Mitarbeiter vor der Einstellen, um das Risiko von Angriffen aus internen Quellen zu minimieren. (Beispiele für Hintergrundprüfungen sind Beschäftigungsverlauf, Strafregistereinträge, Kredithistorie und Referenzprüfungen.) 

> [!NOTE]
> Diese Anforderung ist lediglich eine Empfehlung für potenzielle Mitarbeiter in bestimmten Positionen, z. B. Kassenpersonal, das jeweils nur auf eine Kreditkartennummer Zugriff hat, um eine Transaktion zu vereinfachen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind dafür verantwortlich, gründliche Hintergrundprüfungen von Mitarbeitern mit Zugriff auf die CDE sicherzustellen.|



## <a name="pci-dss-requirement-128"></a>PCI-DSS-Anforderung 12.8

**12.8** Verwalten und implementieren Sie wie folgt Richtlinien und Prozeduren zur Verwaltung von Dienstanbietern, für die Daten von Karteninhabern freigegeben werden, oder die Auswirkungen auf die Sicherheit von Karteninhaberdaten haben könnten.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Überwachung der PCI-Konformität für Dienstanbieter verantwortlich, für die Daten von Karteninhabern freigegeben werden, oder die die Sicherheit der CDE beeinträchtigen könnten. Kunden müssen eine Liste mit allen innerhalb ihrer CDE verwendeten Diensten führen.|



### <a name="pci-dss-requirement-1281"></a>PCI-DSS-Anforderung 12.8.1

**12.8.1** Führen Sie eine Liste von Dienstanbietern, einschließlich einer Beschreibung des bereitgestellten Diensts.


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Überwachung der PCI-Konformität für Dienstanbieter verantwortlich, für die Daten von Karteninhabern freigegeben werden, oder die die Sicherheit der CDE beeinträchtigen könnten. Kunden müssen eine Liste mit allen innerhalb ihrer CDE verwendeten Diensten führen.|



### <a name="pci-dss-requirement-1282"></a>PCI-DSS-Anforderung 12.8.2

**12.8.2** Treffen Sie eine schriftliche Vereinbarung, in der bestätigt wird, dass die Dienstanbieter für die Sicherheit der Daten von Karteninhabern verantwortlich ist, die der Dienstanbieter besitzt oder im Auftrag des Kunden anderweitig speichert, verarbeitet oder überträgt, oder insoweit er Einfluss auf die Sicherheit der Karteninhaberdaten-Umgebung des Kunden hat. 

> [!NOTE]
> Der genaue Wortlaut einer Bestätigung hängt von der Vereinbarung zwischen den beiden Parteien, den Details des bereitgestellten Diensts und den Zuständigkeiten ab, die jeder Partei zugewiesen werden. Die Bestätigung muss nicht den genauen Wortlaut in dieser Anforderung enthalten.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für das Treffen von schriftlichen Vereinbarungen mit Dienstanbietern verantwortlich, in denen die Zuständigkeit für die Aufrechterhaltung der Sicherheit der Daten von Karteninhabern bestätigt wird.|



### <a name="pci-dss-requirement-1283"></a>PCI-DSS-Anforderung 12.8.3

**12.8.3** Stellen Sie sicher, dass ein Prozess für die Zusammenarbeit mit Dienstanbietern etabliert ist, der eine entsprechende Due-Diligence-Prüfung vor der Zusammenarbeit umfasst.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Sicherstellung verantwortlich, dass ein Prozess für die Zusammenarbeit mit Dienstanbietern etabliert ist, der eine entsprechende Due-Diligence-Prüfung vor der Zusammenarbeit umfasst.|



### <a name="pci-dss-requirement-1284"></a>PCI-DSS-Anforderung 12.8.4

**12.8.4** Verwalten Sie ein Programm zur mindestens jährlichen Überwachung des PCI-DSS-Konformitätsstatus von Dienstanbietern.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Verwaltung eines Programms zur mindestens jährlichen Überwachung des PCI-DSS-Konformitätsstatus von Dienstanbietern verantwortlich.|



### <a name="pci-dss-requirement-1285"></a>PCI-DSS-Anforderung 12.8.5

**12.8.5** Pflegen Sie Informationen dazu, welche PCI-DSS-Anforderungen von den einzelnen Dienstanbietern und von der Entität verwaltet werden.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind dafür verantwortlich, eine Kopie der [Matrix der Verantwortungsübersicht](https://aka.ms/pciblueprintcrm32) zu erhalten. In dieser Matrix sind die PCI-DSS-Anforderungen, die in der Verantwortung des Kunden, und die Anforderungen skizziert, die in der Verantwortung von Microsoft Azure liegen.|



## <a name="pci-dss-requirement-129"></a>PCI-DSS-Anforderung 12.9

**12.9** Zusätzliche Anforderung nur für Dienstanbieter: Dienstanbieter bestätigen Kunden schriftlich, dass sie für die Sicherheit der Daten von Karteninhabern verantwortlich sind, die der Dienstanbieter besitzt oder im Auftrag des Kunden anderweitig speichert, verarbeitet oder überträgt, oder insoweit er Einfluss auf die Sicherheit der Karteninhaberdaten-Umgebung des Kunden hat. 

> [!NOTE]
> Der genaue Wortlaut einer Bestätigung hängt von der Vereinbarung zwischen den beiden Parteien, den Details des bereitgestellten Diensts und den Zuständigkeiten ab, die jeder Partei zugewiesen werden. Die Bestätigung muss nicht den genauen Wortlaut in dieser Anforderung enthalten.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden, bei denen es sich um Dienstanbieter handelt, sind dafür verantwortlich, ihre Zuständigkeiten bei der Verwaltung der PCI-Konformität anzuerkennen. |



## <a name="pci-dss-requirement-1210"></a>PCI-DSS-Anforderung 12.10

**12.10** Implementieren Sie einen Plan für Reaktionen auf Vorfälle. Seien Sie darauf vorbereitet, umgehend auf eine Systemverletzung zu reagieren.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Entwicklung von IR-Plänen und Tests verantwortlich, in denen alle Kundenkontrollen im Zusammenhang mit gemeinsamen genutzten Kontaktpunkten und Kundenanwendungen, die die Infrastruktur von Azure nutzen, berücksichtigt werden. Es liegt im Verantwortungsbereich des Kunden, Azure die richtigen Kontaktinformationen bereitzustellen, für den Fall, dass ihnen ein Vorfall gemeldet werden muss, der u. U. Auswirkungen auf ihre Anwendung oder Daten hat.|



### <a name="pci-dss-requirement-12101"></a>PCI-DSS-Anforderung 12.10.1

**12.10.1** Erstellen Sie den Plan für Reaktionen auf Vorfälle, der im Fall einer Systemverletzung implementiert wird. Stellen Sie sicher, dass der Plan mindestens Folgendes berücksichtigt:
- Rollen und Zuständigkeiten sowie Kommunikations- und Kontaktstrategien im Fall einer Gefährdung, mindestens einschließlich der Benachrichtigung der Zahlungsmarken
- Bestimmte Verfahren für die Reaktion auf Vorfälle
- Verfahren für die Geschäftswiederherstellung und -kontinuität
- Datensicherungsprozesse
- Analyse der gesetzlichen Anforderungen für das Melden von Gefährdungen
- Abdeckung und Reaktionen aller wichtigen Systemkomponenten
- Verweis auf oder Einschluss von Verfahren der Zahlungsmarken für die Reaktion auf Vorfälle

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Entwicklung von IR-Plänen und Tests verantwortlich, in denen alle Kundenkontrollen im Zusammenhang mit gemeinsamen genutzten Kontaktpunkten und Kundenanwendungen, die die Infrastruktur von Azure nutzen, berücksichtigt werden. Es liegt im Verantwortungsbereich des Kunden, Azure die richtigen Kontaktinformationen bereitzustellen, für den Fall, dass ihnen ein Vorfall gemeldet werden muss, der u. U. Auswirkungen auf ihre Anwendung oder Daten hat.|



### <a name="pci-dss-requirement-12102"></a>PCI-DSS-Anforderung 12.10.2

**12.10.2** Überprüfen und testen Sie den Plan einschließlich aller Elemente in Anforderung 12.10.1 mindestens einmal jährlich.


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Entwicklung von IR-Plänen und Tests verantwortlich, in denen alle Kundenkontrollen im Zusammenhang mit gemeinsamen genutzten Kontaktpunkten und Kundenanwendungen, die die Infrastruktur von Azure nutzen, berücksichtigt werden. Es liegt im Verantwortungsbereich des Kunden, Azure die richtigen Kontaktinformationen bereitzustellen, für den Fall, dass ihnen ein Vorfall gemeldet werden muss, der u. U. Auswirkungen auf ihre Anwendung oder Daten hat.|



### <a name="pci-dss-requirement-12103"></a>PCI-DSS-Anforderung 12.10.3

**12.10.3** Benennen Sie bestimmte Mitarbeiter, die sieben Tage pro Woche rund um die Uhr verfügbar sind, um auf Warnungen zu reagieren.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Entwicklung von IR-Plänen und Tests verantwortlich, in denen alle Kundenkontrollen im Zusammenhang mit gemeinsamen genutzten Kontaktpunkten und Kundenanwendungen, die die Infrastruktur von Azure nutzen, berücksichtigt werden. Es liegt im Verantwortungsbereich des Kunden, Azure die richtigen Kontaktinformationen bereitzustellen, für den Fall, dass ihnen ein Vorfall gemeldet werden muss, der u. U. Auswirkungen auf ihre Anwendung oder Daten hat.|



### <a name="pci-dss-requirement-12104"></a>PCI-DSS-Anforderung 12.10.4

**12.10.4** Stellen Sie Mitarbeitern mit Aufgaben in der Reaktion auf Sicherheitsverletzungen entsprechende Schulungen bereit.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Entwicklung von IR-Plänen und Tests verantwortlich, in denen alle Kundenkontrollen im Zusammenhang mit gemeinsamen genutzten Kontaktpunkten und Kundenanwendungen, die die Infrastruktur von Azure nutzen, berücksichtigt werden. Es liegt im Verantwortungsbereich des Kunden, Azure die richtigen Kontaktinformationen bereitzustellen, für den Fall, dass ihnen ein Vorfall gemeldet werden muss, der u. U. Auswirkungen auf ihre Anwendung oder Daten hat.|



### <a name="pci-dss-requirement-12105"></a>PCI-DSS-Anforderung 12.10.5

**12.10.5** Schließen Sie Warnungen von Sicherheitsüberwachungsystemen ein, einschließlich, aber nicht beschränkt auf Angrifferkennungs-/Angriffschutzsystemen, Firewalls und Systemen zur Überwachung der Datenintegrität.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Entwicklung von IR-Plänen und Tests verantwortlich, in denen alle Kundenkontrollen im Zusammenhang mit gemeinsamen genutzten Kontaktpunkten und Kundenanwendungen, die die Infrastruktur von Azure nutzen, berücksichtigt werden. Es liegt im Verantwortungsbereich des Kunden, Azure die richtigen Kontaktinformationen bereitzustellen, für den Fall, dass ihnen ein Vorfall gemeldet werden muss, der u. U. Auswirkungen auf ihre Anwendung oder Daten hat.|



### <a name="pci-dss-requirement-12106"></a>PCI-DSS-Anforderung 12.10.6

**12.10.6** Entwickeln Sie einen Prozess für das Ändern und Weiterentwickeln des Plans für Reaktionen auf Vorfälle, um gewonnene Erkenntnisse und Branchenentwicklungen einzubeziehen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Entwicklung von IR-Plänen und Tests verantwortlich, in denen alle Kundenkontrollen im Zusammenhang mit gemeinsamen genutzten Kontaktpunkten und Kundenanwendungen, die die Infrastruktur von Azure nutzen, berücksichtigt werden. Es liegt im Verantwortungsbereich des Kunden, Azure die richtigen Kontaktinformationen bereitzustellen, für den Fall, dass ihnen ein Vorfall gemeldet werden muss, der u. U. Auswirkungen auf ihre Anwendung oder Daten hat.|



## <a name="pci-dss-requirement-1211"></a>PCI-DSS-Anforderung 12.11

**12.11** **Zusätzliche Anforderung nur für Dienstanbieter:** Führen Sie mindestens vierteljährliche Überprüfungen durch, um sicherzustellen, dass Mitarbeiter die Sicherheitsrichtlinien und betrieblichen Verfahren einhalten.
Überprüfungen müssen die folgenden Prozesse umfassen:
- Tägliche Protokollüberprüfungen
- Überprüfungen von Firewall-Regelsätzen
- Anwenden von Konfigurationsstandards auf neue Systeme
- Reagieren auf Sicherheitswarnungen
- Change Management-Prozesse 

> [!NOTE]
> Diese Anforderung ist bis zum 31 Januar 2018 eine bewährte Methode, danach ist Sie eine Anforderung.


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden, bei denen es sich um Dienstanbieter handelt, sind dafür verantwortlich, ihre Überprüfungen von Prozessen zu dokumentieren, um die Leistung der PCI-Konformitätskontrolle zu bestätigen.|



### <a name="pci-dss-requirement-12111"></a>PCI-DSS-Anforderung 12.11.1

**12.11.1** Zusätzliche Anforderung nur für Dienstanbieter: Pflegen Sie eine Dokumentation des vierteljährlichen Überprüfungsprozesses, einschließlich:
- Dokumentieren der Ergebnisse der Überprüfung
- Überprüfen und Genehmigen der Ergebnisse von Mitarbeitern, die für das PCI-DSS-Konformitätsprogramm zuständig sind 

> [!NOTE]
> Diese Anforderung ist bis zum 31 Januar 2018 eine bewährte Methode, danach ist Sie eine Anforderung.


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden, bei denen es sich um Dienstanbieter handelt, sind dafür verantwortlich, ihre Überprüfungen von Prozessen zu dokumentieren, um die Leistung der PCI-Konformitätskontrolle zu bestätigen.|




