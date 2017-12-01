---
title: "Blueprint zur Azure-Zahlungsverarbeitung – CHD-Anforderungen"
description: PCI-DSS-Anforderung 3
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 9736f7c8-c632-4b86-8b8a-6e4f45c1a7aa
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 356599cbe1e4e1948a5ec16d0d504835fa7dcd43
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="chd-requirements-for-pci-dss-compliant-environments"></a>CHD-Anforderungen für PCI-DSS-konforme Umgebungen
## <a name="pci-dss-requirement-3"></a>PCI-DSS-Anforderung 3

**Schützen der gespeicherten Karteninhaberdaten**

> [!NOTE]
> Diese Anforderungen werden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

Schutzmethoden wie Verschlüsselung, Kürzung, Maskierung und Hashing sind wichtige Komponenten für den Datenschutz von Karteninhabern. Wenn ein Angreifer andere Sicherheitsmaßnahmen umgeht und ohne die richtigen kryptografischen Schlüssel Zugriff auf verschlüsselte Daten erhält, sind die Daten für diese Person unlesbar und unbrauchbar. Auch andere wirksame Methoden zum Schutz gespeicherter Daten sollten als potenzielle Möglichkeiten zur Risikominderung in Betracht gezogen werden. Zu den Methoden zur Risikominimierung gehören z. B. der Verzicht auf die Speicherung von Karteninhaberdaten, sofern dies nicht unbedingt erforderlich ist, die Kürzung von Karteninhaberdaten, wenn keine vollständige PAN benötigt wird, und der Verzicht auf das Versenden ungeschützter PANs unter Verwendung von Messagingtechnologien für Endbenutzer wie E-Mail und Chat.
Im Glossar der Begriffe, Abkürzungen und Akronyme für PCI-DSS und PA-DSS finden Sie Definitionen für „starke Kryptografie“ und andere PCI-DSS-Begriffe.

## <a name="pci-dss-requirement-31"></a>PCI-DSS-Anforderung 3.1

**3.1** Minimieren Sie die Speicherung von Karteninhaberdaten, indem Sie Richtlinien, Verfahren und Prozesse zur Datenaufbewahrung und -entsorgung implementieren, die mindestens Folgendes für die Speicherung aller Karteninhaberdaten (Cardholder Data, CHD) einbeziehen:
- Einschränkung der Datenspeicherungsmenge und der Aufbewahrungszeit auf Werte, die für gesetzliche, behördliche und geschäftliche Anforderungen erforderlich sind
- Bestimmte Aufbewahrungsanforderungen für Karteninhaberdaten
- Prozesse zur sicheren Löschung von Daten, wenn diese nicht mehr benötigt werden
- Ein vierteljährlicher Prozess zur Identifizierung und sicheren Löschung gespeicherter Karteninhaberdaten, die die definierte Aufbewahrungsdauer überschreiten

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Azure ist dafür verantwortlich, dass die zur Löschung vorgesehenen Kundendaten mit Hilfe von NIST 800-88-konformen Protokollen, die in den Richtlinien für die sichere Entsorgung festgelegt sind, sicher ausgemustert werden. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore löscht oder zerstört keine gespeicherten Karteninhaberdaten. Allerdings sind alle Daten verschlüsselt und es werden keine Daten zur Hauptkontonummer (PAN) gespeichert.<br /><br />|



## <a name="pci-dss-requirement-32"></a>PCI-DSS-Anforderung 3.2

**3.2** Speichern Sie keine vertraulichen Authentifizierungsdaten nach der Autorisierung (auch nicht verschlüsselt). Wenn vertrauliche Authentifizierungsdaten empfangen werden, sorgen Sie dafür, dass alle Daten nach Abschluss des Autorisierungsprozesses nicht mehr wiederhergestellt werden können. 
- Es gibt eine geschäftliche Begründung und 
- Die Daten werden sicher gespeichert.
Zu den vertraulichen Authentifizierungsdaten gehören die in den folgenden Anforderungen 3.2.1 bis 3.2.3 genannten Daten:


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore löscht oder zerstört keine gespeicherten Karteninhaberdaten. Die Beispieldaten werden nur zu Demozwecken gespeichert. Allerdings sind alle Daten verschlüsselt und es werden keine Daten zur Hauptkontonummer (PAN) gespeichert.<br /><br />|



### <a name="pci-dss-requirement-321"></a>PCI-DSS-Anforderung 3.2.1

**3.2.1** Speichern Sie nicht den gesamten Inhalt einer Spur (vom Magnetstreifen auf der Rückseite einer Karte; vergleichbare Daten sind auf einem Chip oder an anderer Stelle enthalten) nach der Autorisierung. Diese Daten werden alternativ als vollständige Spur, Spur, Spur 1, Spur 2 und Magnetstreifendaten bezeichnet. 

> [!NOTE]
> Im Rahmen der normalen Geschäftstätigkeit können die folgenden Datenelemente des Magnetstreifens erhalten bleiben: 
> - Name des Karteninhabers 
> - Hauptkontonummer (PAN) 
> - Ablaufdatum 
> - Dienstcode 
>
> Um das Risiko zu minimieren, sollten Sie nur diese Datenelemente speichern, die für das Geschäft benötigt werden.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert nicht den gesamten Inhalt der Karteninhaberdaten.|



### <a name="pci-dss-requirement-322"></a>PCI-DSS-Anforderung 3.2.2

**3.2.2** Speichern Sie den Kartenprüfcode oder -wert (dreistellige oder vierstellige Nummer, die auf der Vorder- oder Rückseite einer Zahlungskarte aufgedruckt ist und zur Überprüfung von Transaktionen verwendet wird, bei denen die Karte nicht vorhanden ist) nach der Autorisierung nicht.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verschlüsselt alle Daten einschließlich der CVV-Beispiele.|



### <a name="pci-dss-requirement-323"></a>PCI-DSS-Anforderung 3.2.3

**3.2.3** Speichern Sie nach der Autorisierung nicht die persönliche Identifikationsnummer (PIN) oder den verschlüsselten PIN-Block.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore speichert keine PIN-Informationen.|



## <a name="pci-dss-requirement-33"></a>PCI-DSS-Anforderung 3.3

**3.3** Maskieren Sie die PAN bei der Anzeige (die ersten sechs und die letzten vier Ziffern sind die maximale Anzahl der anzuzeigenden Ziffern), sodass nur Mitarbeiter mit einem berechtigten geschäftlichen Anspruch die vollständige PAN sehen können. 

> [!NOTE]
> Diese Anforderung ersetzt nicht die strengeren Anforderungen an die Anzeige von Karteninhaberdaten, z. B. die gesetzlichen Anforderungen oder die Anforderungen an die Zahlungskartenmarke für POS-Belege (Point-of-Sale).

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore maskiert die Hauptkontonummer (PAN) mit transparenter Datenverschlüsselung, immer verschlüsselten Spalten und dynamischer Datenmaskierung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-34"></a>PCI-DSS-Anforderung 3.4

**3.4** Stellen Sie die PAN überall dort, wo sie gespeichert ist, unlesbar dar (einschließlich tragbarer digitaler Medien, Sicherungsmedien und in Protokollen), indem Sie einen der folgenden Ansätze verwenden:
- Unidirektionale Hashes basierend auf starker Kryptografie (Hash muss gesamte PAN umfassen)
- Kürzung (Hashing kann nicht verwendet werden, um das abgeschnittene Segment der PAN zu ersetzen)
- Indextoken und -blöcke (Blöcke müssen sicher aufbewahrt werden)
- Starke Kryptografie mit zugehörigen Schlüsselverwaltungsprozessen und -verfahren 

> [!NOTE]
> Eine böswillige Person hat relativ wenig Mühe, die ursprünglichen PAN-Daten zu rekonstruieren, wenn sie sowohl auf die abgeschnittene als auch auf die Hashversion einer PAN zugreifen kann. Wenn in der Umgebung einer Entität die Hashversion und die abgeschnittene Version der gleichen PAN vorhanden sind, müssen zusätzliche Kontrollen implementiert werden, um sicherzustellen, dass die Hashversion und die abgeschnittene Version nicht korreliert werden können, um die ursprüngliche PAN zu rekonstruieren.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Im Contoso Webstore werden alle Kreditkartendaten verschlüsselt und wird Azure Key Vault verwendet, um Schlüssel zu verwalten, sodass ein Abrufen von Karteninhaberdaten verhindert wird.<br /><br />Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-341"></a>PCI-DSS-Anforderung 3.4.1

**3.4.1** Wenn die Datenträgerverschlüsselung verwendet wird (anstelle der Datenbankverschlüsselung auf Datei- oder Spaltenebene), muss der logische Zugriff separat und unabhängig von nativen Authentifizierungs- und Zugriffssteuerungsmechanismen des Betriebssystems verwaltet werden (z. B. durch den Verzicht auf lokale Benutzerkontendatenbanken oder allgemeine Netzwerkanmeldeinformationen). Entschlüsselungsschlüssel dürfen nicht zu Benutzerkonten zugeordnet werden. 

> [!NOTE]
> Diese Anforderung gilt zusätzlich zu allen anderen PCI-DSS-Verschlüsselungs- und -Schlüsselverwaltungsanforderungen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verschlüsselt alle gespeicherten Daten und trennt den Datenverkehr, um eine privilegierte Rechteerweiterung für DevOps-Funktionen zu verhindern.<br /><br />Weil die App Service-Umgebung geschützt und gesperrt ist, muss es einen Mechanismus geben, der es erlaubt, alle DevOps-Releases oder -Änderungen zu berücksichtigen, die notwendig sein könnten, so z. B. die Möglichkeit, eine Web-App mit Kudu zu überwachen.<br /><br />Ein virtueller Computer wurde als Jumpbox (Bastionhost) mit den folgenden Konfigurationen eingerichtet:<br /><br /><ul><li>[Antischadsoftware-Erweiterung](/azure/security/azure-security-antimalware)</li><li>[OMS-Überwachungserweiterung](/azure/virtual-machines/virtual-machines-windows-extensions-oms)</li><li>[VM-Diagnoseerweiterung](/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)</li><li>[Bitlocker-verschlüsselter Datenträger](/azure/security/azure-security-disk-encryption)</li></ul>Die Verwendung von Azure Key Vault entspricht den Azure Government-, PCI-DSS- und HIPAA-Anforderungen.|



## <a name="pci-dss-requirement-35"></a>PCI-DSS-Anforderung 3.5

**3.5** Dokumentieren und implementieren Sie Verfahren zum Schutz von Schlüsseln, mit denen gespeicherte Karteninhaberdaten vor Offenlegung und Missbrauch geschützt werden. 

> [!NOTE]
> Diese Anforderung gilt für Schlüssel, die zur Verschlüsselung gespeicherter Karteninhaberdaten verwendet werden, und auch für Schlüssel, die zum Schutz von Datenverschlüsselungsschlüsseln verwendet werden. Solche Schlüssel müssen mindestens so stark sein wie der Datenverschlüsselungsschlüssel.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | **Für Key Vault verwendende Kunden:**<br /><br />Microsoft Azure stellt sicher, dass die Schlüsseltresore der Kunden logisch voneinander isoliert und logisch von der Verwaltungsebene des Key Vault-Diensts isoliert sind. Key Vault ist so konzipiert, dass Microsoft keinen ständigen Zugriff auf den Schlüsseltresor des Kunden hat. <br /><br />Schlüssel werden mit branchenüblichen Algorithmen, Schlüssellängen und Hardwaresicherheitsmodulen (HSMs) von Microsoft Azure geschützt.<br /><br />Ein Schlüssel, der in Microsoft Azure Key Vault gespeichert ist, kann zum Schutz eines anderen Schlüssels verwendet werden. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore bietet eine Dokumentation zur Veranschaulichung und Bereitstellung einer geschützten Schlüssellösung zum Schutz der Karteninhaberdaten der Demo.|



### <a name="pci-dss-requirement-351"></a>PCI-DSS-Anforderung 3.5.1

**3.5.1** *Zusätzliche Anforderung nur für Dienstanbieter:* Führen Sie eine dokumentierte Beschreibung der kryptografischen Architektur, die Folgendes enthält:
- Details zu allen Algorithmen, Protokollen und Schlüsseln, die zum Schutz der Karteninhaberdaten verwendet werden, einschließlich der Schlüsselstärke und des Ablaufdatums.
- Beschreibung der Verwendung für jeden Schlüssel
- Bestandsaufnahme aller HSMs und anderer SCDs, die für die Schlüsselverwaltung verwendet werden 

> [!NOTE]
> Diese Anforderung ist bis zum 31. Januar 2018 eine bewährte Methode, danach stellt sie eine Anforderung dar.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | **Für Key Vault verwendende Kunden:**<br /><br />Microsoft Azure stellt sicher, dass die Schlüsseltresore der Kunden logisch voneinander isoliert und logisch von der Verwaltungsebene des Key Vault-Diensts isoliert sind. Key Vault ist so konzipiert, dass Microsoft keinen ständigen Zugriff auf den Schlüsseltresor des Kunden hat. <br /><br />Schlüssel werden mit branchenüblichen Algorithmen, Schlüssellängen und Hardwaresicherheitsmodulen (HSMs) von Microsoft Azure geschützt.<br /><br />Ein Schlüssel, der in Microsoft Azure Key Vault gespeichert ist, kann zum Schutz eines anderen Schlüssels verwendet werden. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore bietet eine Dokumentation zur Veranschaulichung und Bereitstellung einer geschützten Schlüssellösung zum Schutz der Karteninhaberdaten der Demo.|



### <a name="pci-dss-requirement-352"></a>PCI-DSS-Anforderung 3.5.2

**3.5.2** Beschränken Sie den Zugriff auf kryptografische Schlüssel auf die geringste Anzahl von Verwaltungsberechtigten.


**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | **Für Key Vault verwendende Kunden:**<br /><br />Key Vault unterstützt differenzierte Zugriffsrichtlinien, die es dem Key Vault-Besitzer ermöglichen, Zugriff auf bestimmte Funktionen zu gewähren, um bestimmte Operationen für bestimmte Entitäten durchzuführen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Schlüsselverwaltung ist auf ein Benutzerkonto eingegrenzt (admin##@contosowebstore.com).|



### <a name="pci-dss-requirement-353"></a>PCI-DSS-Anforderung 3.5.3

**3.5.3** Speichern Sie Geheimnis und private Schlüssel, die zum Verschlüsseln/Entschlüsseln von Karteninhaberdaten verwendet werden, jederzeit in einer (oder mehreren) der folgenden Formen:
- Verschlüsselt mit einem Schlüssel für die Schlüsselverschlüsselung, der mindestens so stark ist wie der Datenverschlüsselungsschlüssel, und der getrennt vom Datenverschlüsselungsschlüssel gespeichert wird.
- Innerhalb eines sicheren kryptografischen Geräts (z. B. eines Hardwaresicherheitsmoduls (HSM) oder eines PTS-zugelassenen Interaktionspunktgeräts)
- Als mindestens zwei vollständige Schlüsselkomponenten oder Schlüsselfreigaben gemäß einer branchenüblichen Methode 

> [!NOTE]
> Es ist nicht erforderlich, dass öffentliche Schlüssel in einer dieser Formen gespeichert werden.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | **Für Key Vault verwendende Kunden:**<br /><br />Die Schlüssel werden in den spezifischen, vom Kunden identifizierten Schlüsseltresoren gespeichert.<br /><br />Auf den Schlüsseltresor kann von mehreren Anwendungen gleichzeitig und global zugegriffen werden, wodurch die Notwendigkeit reduziert wird, einen Schlüssel zu kopieren und an mehreren Standorten zu speichern. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Key Vault für die gesamte Schlüsselverwaltung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-354"></a>PCI-DSS-Anforderung 3.5.4

**3.5.4** Speichern Sie kryptografische Schlüssel an möglichst wenigen Stellen.


**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | **Für Key Vault verwendende Kunden:**<br /><br />Die Schlüssel werden in den spezifischen, vom Kunden identifizierten Schlüsseltresoren gespeichert. <br /><br />Auf den Schlüsseltresor kann von mehreren Anwendungen gleichzeitig und global zugegriffen werden, wodurch die Notwendigkeit reduziert wird, einen Schlüssel zu kopieren und an mehreren Standorten zu speichern. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Key Vault für die gesamte Schlüsselverwaltung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



## <a name="pci-dss-requirement-36"></a>PCI-DSS-Anforderung 3.6

**3.6** Dokumentieren und implementieren Sie alle Schlüsselverwaltungsprozesse und -verfahren für kryptografische Schlüssel, die zur Verschlüsselung von Karteninhaberdaten verwendet werden, einschließlich der folgenden. 

> [!NOTE]
> Zahlreiche Branchenstandards für die Schlüsselverwaltung sind aus verschiedenen Ressourcen verfügbar, einschließlich NIST, das unter „http://csrc.nist.gov“ verfügbar ist.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Key Vault für die gesamte Schlüsselverwaltung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-361"></a>PCI-DSS-Anforderung 3.6.1

**3.6.1** Generierung starker kryptografischer Schlüssel

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | **Für Key Vault verwendende Kunden:** <br /><br />Bei der Generierung von Schlüsseln in Key Vault ist Azure für die Generierung von Schlüsseln gemäß den Spezifikationen des Kunden verantwortlich. Schlüssel werden mit einem HSM generiert. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Key Vault für die gesamte Schlüsselverwaltung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-362"></a>PCI-DSS-Anforderung 3.6.2

**3.6.2** Sichere kryptografische Schlüsselverteilung

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | **Für Key Vault verwendende Kunden:**<br /><br />Das BYOK-Tool (Bring Your Own Key) kapselt den Kundenschlüssel und ist auf einen bestimmten Sicherheitstresor ausgerichtet, der an ein bestimmtes Azure-Abonnement gebunden ist. Der Schlüssel kann nur in den Schlüsseltresor des definierten Abonnements in der angegebenen Region importiert werden. Dieser Prozess verwendet die Verschlüsselungsverfahren des Hardwareherstellers. Kunden erhalten eine Benachrichtigung, dass die Übertragung erfolgreich war. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Key Vault für die gesamte Schlüsselverwaltung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-363"></a>PCI-DSS-Anforderung 3.6.3

**3.6.3** Sicherer kryptografischer Schlüsselspeicher

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | **Für Key Vault verwendende Kunden:**<br /><br />Schlüssel werden in den HSMs gespeichert und mit der kryptografischen Sicherheit des Hardwareherstellers gesichert. Die Metadaten der Schlüssel werden in Azure Storage in einem verschlüsselten Zustand gespeichert, der für jeden Schlüsseltresor eindeutig ist. <br /><br /> |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Key Vault für die gesamte Schlüsselverwaltung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-364"></a>PCI-DSS-Anforderung 3.6.4

**3.6.4** Änderungen des kryptografischen Schlüssels für Schlüssel, die das Ende ihrer Kryptoperiode erreicht haben (z. B. nach Ablauf einer bestimmten Zeitspanne und/oder nachdem eine bestimmte Menge an Verschlüsselungstext durch einen bestimmten Schlüssel erzeugt wurde), die vom zugehörigen Anwendungsanbieter oder Schlüsselbesitzer definiert wurde und auf branchenüblichen bewährten Methoden und Richtlinien (z. B. NIST Special Publication 800-57) basiert.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | **Für Key Vault verwendende Kunden:**<br /><br />Key Vault unterstützt Funktionen zum Aktualisieren oder Zurücksetzen von Schlüsseln, die vom Kunden definiert werden. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Key Vault für die gesamte Schlüsselverwaltung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-365"></a>PCI-DSS-Anforderung 3.6.5

**3.6.5 bei einer** Zurückziehen oder Ersetzen (z. B. Archivierung, Zerstörung und/oder Widerruf) von Schlüsseln, wenn die Integrität des Schlüssels geschwächt ist (z. B. Ausscheiden eines Mitarbeiters mit Kenntnis einer Klartext-Schlüsselkomponente) oder der Verdacht besteht, dass Schlüssel gefährdet sind. 

> [!NOTE]
> Wenn zurückgezogene oder ersetzte kryptografische Schlüssel aufbewahrt werden sollen, müssen diese sicher archiviert werden (z. B. durch Verwendung eines Schlüssels für die Schlüsselverschlüsselung). Archivierte kryptografische Schlüssel sollten nur zu Entschlüsselungs- und Verifizierungszwecken verwendet werden.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | **Für Key Vault verwendende Kunden:**<br /><br />Key Vault unterstützt Funktionen zum Zurückziehen oder Ersetzen von Schlüsseln, die vom Kunden definiert werden. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Key Vault für die gesamte Schlüsselverwaltung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-366"></a>PCI-DSS-Anforderung 3.6.6

**3.6.6** Werden manuelle Verwaltungsoperationen für kryptografische Klartextschlüssel verwendet, so müssen diese Operationen mit geteiltem Wissen und doppelter Kontrolle verwaltet werden. 

> [!NOTE]
> Beispiele für manuelle Schlüsselverwaltungsoperationen sind unter anderem: Schlüsselgenerierung, Übertragung, Ladevorgang, Speicherung und Zerstörung.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Key Vault für die gesamte Schlüsselverwaltung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-367"></a>PCI-DSS-Anforderung 3.6.7

**3.6.7 durch** Verhinderung der unbefugten Ersetzung von kryptografischen Schlüsseln.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | **Für Key Vault verwendende Kunden:**<br /><br />Schlüsseltresore sind logisch getrennt und unterstützen keine verzeichnisübergreifende Autorisierung. Dadurch wird eine unbefugte Ersetzung verhindert. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Key Vault für die gesamte Schlüsselverwaltung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-368"></a>PCI-DSS-Anforderung 3.6.8

**3.6.8 in** Anforderung an Verwaltungsberechtigte für kryptografische Schlüssel zur formellen Anerkennung, dass sie ihre Verantwortung als Schlüsselverwaltungsberechtigte verstehen und akzeptieren.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Schlüsselverwaltung ist auf ein Benutzerkonto eingegrenzt (admin##@contosowebstore.com).|



## <a name="pci-dss-requirement-37"></a>PCI-DSS-Anforderung 3.7

**3.7** Stellen Sie sicher, dass Sicherheitsrichtlinien und betriebliche Verfahren zum Schützen gespeicherter Daten von Karteninhabern dokumentiert und verwendet werden sowie allen beteiligten Parteien bekannt sind.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Key Vault für die gesamte Schlüsselverwaltung. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|




