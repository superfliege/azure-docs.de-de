---
title: Blueprint zur Azure-Zahlungsverarbeitung – Anforderungen für den physischen Zugriff
description: PCI-DSS-Anforderung 9
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 91595a69-e9ce-4f9c-8388-10224165d9c0
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: b702f3d3a06b1d47c2853ab51ccf4872441da03d
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="physical-access-requirements-for-pci-dss-compliant-environments"></a>Anforderungen für den physischen Zugriff in PCI-DSS-konformen Umgebungen 
## <a name="pci-dss-requirement-9"></a>PCI-DSS-Anforderung 9

**Beschränken des physischen Zugriffs auf Daten von Karteninhabern**

> [!NOTE]
> Diese Anforderungen werden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

Jeder physische Zugriff auf Daten oder Systemen, die Daten von Karteninhabern enthalten, bieten Personen die Möglichkeit, auf Geräte oder Daten zuzugreifen und Systeme oder Druckfassungen zu entfernen, und sollte entsprechend eingeschränkt sein. In Anforderung 9 bezieht sich „Mitarbeiter vor Ort“ auf Vollzeit- und Teilzeitmitarbeiter, temporäre Mitarbeiter, Auftragnehmer und Berater, die sich auf dem Gelände der Entität aufhalten. „Besucher“ bezieht sich auf Anbieter, Gäste von Mitarbeitern vor Ort, Servicetechnikers oder Personen, die das Gelände für kurze Zeit betreten, in der Regel nicht mehr als einen Tag. „Medien“ bezieht sich auf alle Papierdokumente und elektronischen Medien, die Daten von Karteninhabern enthalten.

## <a name="pci-dss-requirement-91"></a>PCI-DSS-Anforderung 9.1

**9.1** Verwenden Sie geeignete Gebäudezugangskontrollen, um den physischen Zugriff auf Systeme in der Karteninhaberdaten-Umgebung zu beschränken und zu überwachen.

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure ist für das Implementieren, Durchsetzen und Überwachen von physischer Zugriffssicherheit für Rechenzentren verantwortlich. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



### <a name="pci-dss-requirement-911"></a>PCI-DSS-Anforderung 9.1.1

**9.1.1** Verwenden Sie entweder Videokameras oder Zutrittskontrollmechanismen (oder beides) zum Überwachen des individuellen physischen Zutritts zu sensiblen Bereichen. Überprüfen Sie gesammelte Daten, und bringen Sie sie in Korrelation mit anderen Einträgen. Speichern Sie Daten mindestens drei Monate lang, falls keine anderweitige gesetzliche Beschränkung gilt.

> [!NOTE]
> „Sensible Bereiche“ bezieht sich auf Rechenzentren, Serverräume oder andere Bereiche, in denen sich Systeme befinden, die Daten von Karteninhabern speichern, verarbeiten oder übertragen. Dies umfasst keine öffentlichen Bereiche, in denen nur POS-Terminals (Point-of-Sale) vorhanden sind, z. B. die Kassenbereiche in einem Einzelhandelsgeschäft.

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure ist für das Implementieren, Durchsetzen und Überwachen von Zutrittskontrollmechanismen mit CCTV und Biometriedaten verantwortlich. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



### <a name="pci-dss-requirement-912"></a>PCI-DSS-Anforderung 9.1.2

**9.1.2** Implementieren Sie physische und/oder logische Maßnahmen, um den Zugriff auf öffentlich zugängliche Netzwerkanschlüsse einzuschränken. 

Beispiel: Netzwerkanschlüsse in öffentlichen Bereichen und Bereichen, die für Besucher zugänglich sind, können deaktiviert und nur dann aktiviert werden, wenn der Netzwerkzugriff ausdrücklich autorisiert ist. Alternativ können Prozesse implementiert werden, um sicherzustellen, dass Besucher Bereichen mit aktiven Netzwerkanschlüssen nur in Begleitung betreten.

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Es gibt keine öffentlich zugänglichen Netzwerkanschlüsse auf der Microsoft Azure-Plattform. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



### <a name="pci-dss-requirement-913"></a>PCI-DSS-Anforderung 9.1.3

**9.1.3** Beschränken Sie den physischen Zugriff auf Funkzugriffspunkte, Gateways, Handgeräte, Netzwerk-/Kommunikationshardware und Telekommunikationsleitungen.

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Der physische Zugriff auf Microsoft Azure-Netzwerkhardware ist durch Zugriffslisten, mehrere Formen der Authentifizierung, physische Zugangsbarrieren und die erforderliche Genehmigung einer geschäftlichen Anforderung für den Zugang zu Geräten stark kontrolliert. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



## <a name="pci-dss-requirement-92"></a>PCI-DSS-Anforderung 9.2

**9.2** Entwickeln Sie Prozeduren zur problemlosen Unterscheidung zwischen Mitarbeitern vor Ort und Besuchern, die Folgendes umfassen:
- Identifizieren von Mitarbeitern vor Ort und Besuchern (z. B. durch Ausgabe von Ausweisen)
- Änderungen an Zugriffsanforderungen
- Aufheben oder Ungültigmachen der Identifizierung von Mitarbeitern vor Ort und abgelaufener Besucheridentifizierungen (z. B. ID-Ausweise).

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure ist für das Implementieren, Durchsetzen und Überwachen von physischer Zugriffssicherheit und Identifizierung von Mitarbeitern und Auftragnehmern verantwortlich, wenn diese Rechenzentren besuchen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



## <a name="pci-dss-requirement-93"></a>PCI-DSS-Anforderung 9.3

**9.3** Kontrollieren Sie den physischen Zutritt zu sensiblen Bereichen für Mitarbeiter vor Ort wie folgt:
- Der Zutritt muss autorisiert sein und auf der jeweiligen Position basieren.
- Der Zutritt wird nach Beendigung des Mitarbeiterverhältnisses sofort widerrufen, und alle physischen Zutrittsmechanismen wie Schlüssel, Zugangskarten usw. werden zurückgegeben oder deaktiviert.

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Zutrittsberechtigungen für Microsoft-Rechenzentren werden durch eine vom Rechenzentrumsteam genehmigte autorisierte Zutrittsliste basierend auf dem Prinzip der geringsten Rechte gesteuert. Die Zutrittssteuerungsliste wird vierteljährlich überprüft und aktualisiert.<br /><br />Microsoft Azure-Rechenzentren nutzen Geräte für physischen Zutritt wie Umzäunungstore, Lesegeräte für elektronische Zutrittsausweise, biometrische Lesegeräte, Schleusen/Portale und Geräte zur Doppelbenutzungskontrolle. Zutrittsausweisgeräte werden kontinuierlich überwacht. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



## <a name="pci-dss-requirement-94"></a>PCI-DSS-Anforderung 9.4

**9.4** Implementieren Sie Prozeduren zur Identifizierung und Autorisierung von Besuchern. Diese Prozeduren müssen Folgendes umfassen.

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure ist verantwortlich, dafür zu sorgen, dass vorab genehmigte Lieferungen in einer sicheren Ladebucht angenommen werden, die physisch von Datenverarbeitungseinrichtungen isoliert ist und von entsprechend berechtigten Mitarbeiter überwacht wird. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



### <a name="pci-dss-requirement-941"></a>PCI-DSS-Anforderung 9.4.1

**9.4.1** Besucher werden vor dem Zutritt zu Bereichen, in denen Daten von Karteninhabern verarbeitet oder verwaltet werden, autorisiert und halten sich in diesen Bereichen nur in Begleitung auf.


**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure ist verantwortlich, dafür zu sorgen, dass vorab genehmigte Lieferungen in einer sicheren Ladebucht angenommen werden, die physisch von Datenverarbeitungseinrichtungen isoliert ist und von entsprechend berechtigten Mitarbeiter überwacht wird. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



### <a name="pci-dss-requirement-942"></a>PCI-DSS-Anforderung 9.4.2

**9.4.2** Besucher werden identifiziert und erhalten Ausweise oder andere Identifizierungskennzeichnungen, die ablaufen und die Besucher offensichtlich von Mitarbeitern vor Ort unterscheiden.

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Der Zutritt zu Microsoft-Rechenzentren muss vorab genehmigt werden, und autorisierte Besucher müssen bei der Ankunft mit physischer Sicherheit einchecken und zuvor einen gültigen Identitätsnachweis vorlegen. Ausweise kennzeichnen Mitarbeiter deutlich. Auftragnehmer erhalten temporäre Ausweise, die beim Verlassen des Gebäudes abgegeben werden müssen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



### <a name="pci-dss-requirement-943"></a>PCI-DSS-Anforderung 9.4.3

**9.4.3** Besucher werden vor dem Verlassen des Gebäudes oder am Ablaufdatum zur Abgabe des Ausweises oder der Identifizierungskennzeichnung aufgefordert.

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Besucher müssen Ausweise vor dem Verlassen eines Microsoft-Gebäudes abgeben. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



### <a name="pci-dss-requirement-944"></a>PCI-DSS-Anforderung 9.4.4

**9.4.4** Ein Besucherprotokoll wird verwendet, um ein physisches Überwachungsprotokoll der Besucheraktivität im Gebäude sowie in Computerräumen und Rechenzentren zu führen, in denen Daten von Karteninhabern gespeichert oder übertragen.
Dokumentieren Sie den Namen des Besuchers, das vertretene Unternehmen sowie den Mitarbeiter vor Ort, der den physischen Zutritt autorisiert, in diesem Protokoll.
Bewahren Sie dieses Protokoll mindestens drei Monate lang auf, falls keine anderweitige gesetzliche Beschränkung gilt.

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure ist dafür verantwortlich, ein Besucherprotokoll als physisches Überwachungsprotokoll der Besucheraktivität im Gebäude sowie in Computerräumen und Rechenzentren zu führen, in denen Daten von Karteninhabern gespeichert oder übertragen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



## <a name="pci-dss-requirement-95"></a>PCI-DSS-Anforderung 9.5

**9.5** Sichern Sie alle Medien physisch.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert alle Daten in Azure SQL-Datenbank. Eine PaaS-SQL-Datenbank-Instanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-951"></a>PCI-DSS-Anforderung 9.5.1

**9.5.1** Lagern Sie Sicherungsmedien an einem sicheren Ort, vorzugsweise einem externen Standort wie einem alternativen oder Notfallstandort oder in einem gewerblichen Lager. Überprüfen Sie den Sicherheit dieses Standorts mindestens einmal jährlich.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert alle Daten in Azure SQL-Datenbank. Eine PaaS-SQL-Datenbank-Instanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-96"></a>PCI-DSS-Anforderung 9.6

**9.6** Halten Sie die interne oder externe Verteilung jeglicher Art von Medien einschließlich der Folgenden unter strenger Kontrolle.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert alle Daten in Azure SQL-Datenbank. Eine PaaS-SQL-Datenbank-Instanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-961"></a>PCI-DSS-Anforderung 9.6.1

**9.6.1** Klassifizieren Sie Medien, damit die Vertraulichkeit der Daten bestimmt werden kann.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert alle Daten in Azure SQL-Datenbank. Eine PaaS-SQL-Datenbank-Instanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-962"></a>PCI-DSS-Anforderung 9.6.2

**9.6.2** Senden Sie die Medien über einen gesicherten Courier-Sender oder eine andere Übermittlungsmethode, die genau verfolgt werden kann.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert alle Daten in Azure SQL-Datenbank. Eine PaaS-SQL-Datenbank-Instanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-963"></a>PCI-DSS-Anforderung 9.6.3

**9.6.3** Stellen Sie sicher, dass die Geschäftsleitung alle Medien genehmigt, die aus einem gesicherten Bereich entfernt werden (einschließlich Medien, die an Benutzer verteilt werden).

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert alle Daten in Azure SQL-Datenbank. Eine PaaS-SQL-Datenbank-Instanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-97"></a>PCI-DSS-Anforderung 9.7

**9.7** Halten Sie die Speicherung und den Zugriff auf Medien unter strenger Kontrolle.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert alle Daten in Azure SQL-Datenbank. Eine PaaS-SQL-Datenbank-Instanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-971"></a>PCI-DSS-Anforderung 9.7.1

**9.7.1** Führen Sie ordnungsgemäße Bestandsprotokolle für alle Medien, und führen mindestens einmal jährlich Medieninventuren durch.


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert alle Daten in Azure SQL-Datenbank. Eine PaaS-SQL-Datenbank-Instanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-98"></a>PCI-DSS-Anforderung 9.8

**9.8** Vernichten Sie wie folgt Medien, die aus geschäftlichen oder rechtlichen Gründen nicht mehr benötigt werden.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert alle Daten in Azure SQL-Datenbank. Eine PaaS-SQL-Datenbank-Instanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-981"></a>PCI-DSS-Anforderung 9.8.1

**9.8.1** Vernichten Sie physisches Material durch Schreddern, Verbrennen oder Einstampfen, damit Daten von Karteninhabern nicht wiederhergestellt werden können. Für Materialien, die zerstört werden sollen, müssen sichere Lagerbehälter verwendet werden.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert alle Daten in Azure SQL-Datenbank. Eine PaaS-SQL-Datenbank-Instanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-982"></a>PCI-DSS-Anforderung 9.8.2

**9.8.2** Sorgen Sie dafür, dass Daten von Karteninhabern auf elektronischen Medien nicht mehr wiederherstellbar sind, damit keine Daten von Karteninhabern rekonstruiert werden können.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Datenvernichtungstechniken hängen von der Art des zu vernichtenden Datenobjekts ab, z. B. Abonnements, Speicher, virtuelle Computer oder Datenbanken. In der mehrinstanzenfähigen Microsoft Azure-Umgebung wird sorgfältig darauf geachtet, dass die Daten eines Kunden nicht in die Daten eines anderen Kunden gelangen, oder dass beim Löschen seiner Daten durch einen Kunden keine anderen Kunden (in den meisten Fällen einschließlich des Kunden, der zuvor im Besitz der Daten war) auf die gelöschten Daten zugreifen können.<br /><br />Microsoft Azure hält den NIST 800-88-Standard für Medienbereinigung ein; dabei wird in erster Linien sichergestellt, dass Daten nicht versehentlich freigegeben werden. Diese Richtlinien umfassen sowohl physische als auch elektronische Bereinigung. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore lässt sich vollständig löschen, indem die während der Bereitstellung verwendete Ressourcengruppe gelöscht wird.|



## <a name="pci-dss-requirement-99"></a>PCI-DSS-Anforderung 9.9

**9.9** Schützen Sie Geräte, die Zahlungskarten über direkte physische Interaktion mit der Karte erfassen, vor Manipulation und Austausch.

> [!NOTE]
> Diese Anforderungen gelten für Kartenlesegeräte, die bei Präsenztransaktionen (d. h. Durchziehen oder Auflegen der Karte) an der Kasse verwendet werden. Diese Anforderung soll nicht für Komponente mit manueller Tasteneingabe wie Computer- oder Kassentastaturen gelten. 

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore nutzt Azure zur Protokollierung aller Systemänderungen.<br /><br />Log Analytics ermöglicht die umfassende Protokollierung von Änderungen. Änderungen können auf Ihre Richtigkeit hin überprüft werden. Genauere Anweisungen finden Sie unter [Azure Security and Compliance Blueprint – PCI-DSS-konforme Umgebungen für die Zahlungsverarbeitung](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-991"></a>PCI-DSS-Anforderung 9.9.1

**9.9.1** Pflegen Sie eine aktuelle Liste der Geräte. Diese Liste muss folgende Informationen enthalten:
- Fabrikat, Modell des Geräts
- Standort des Geräts (z. B. die Adresse des Standorts oder Gebäudes, an dem oder in dem sich das Gerät befindet)
- Seriennummer des Geräts oder andere eindeutige Methode der Identifizierung

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore bietet eine Referenzarchitektur und eine Liste aller Dienste, die in der Bereitstellungsdokumentation verwendet werden.|



### <a name="pci-dss-requirement-992"></a>PCI-DSS-Anforderung 9.9.2

**9.9.2** Überprüfen Sie in regelmäßigen Abständen Geräteoberflächen auf Manipulation (z. B. Kartenskimmer) oder Austausch (z. B. durch Überprüfen der Seriennummer oder anderer Gerätemerkmale, um sicherzustellen, dass es nicht durch eine betrügerische Vorrichtung ausgetauscht wurde).

> [!NOTE]
> Beispiele für Anzeichen, dass ein Gerät manipuliert oder ausgetauscht wurde sind unerwartete an das Gerät angeschlossene Zusatzteile oder Kabel, fehlende oder geänderte Sicherheitsetiketten, beschädigte oder andersfarbige Gehäuse und Änderungen an der Seriennummer oder anderen äußeren Kennzeichen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



### <a name="pci-dss-requirement-993"></a>PCI-DSS-Anforderung 9.9.3

**9.9.3** Stellen Sie Schulungen für Mitarbeiter zur Erkennung von versuchter Manipulation oder Austausch von Geräten bereit. Schulungen sollten folgende Anweisungen umfassen:
- Überprüfen Sie die Identität aller Drittpersonen, die sich als Reparatur- oder Wartungspersonal, bevor Sie ihnen Zugriff gewähren, um Änderungen oder Problembehandlungen an Geräten vorzunehmen.
- Führen Sie immer eine Überprüfung durch, bevor Sie Geräte installieren, ersetzen oder zurückgeben.
- Achten Sie auf verdächtiges Verhalten in der Nähe von Geräte (z. B. unbekannte Personen, die versuchen, Geräte abzustecken oder zu öffnen).
- Melden Sie verdächtiges Verhalten und Anzeichen für Gerätemanipulation oder -Austausch dem zuständigen Personal (z. B. einem Vorgesetzten oder einem Sicherheitsbeauftragten).

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



## <a name="pci-dss-requirement-910"></a>PCI-DSS-Anforderung 9.10

**9.10** Stellen Sie sicher, dass Sicherheitsrichtlinien und betriebliche Verfahren für das Einschränken des physischen Zugriffs auf Daten von Karteninhabern dokumentiert und verwendet werden sowie allen beteiligten Parteien bekannt sind.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|




