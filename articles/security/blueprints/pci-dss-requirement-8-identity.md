---
title: Blueprint zur Azure-Zahlungsverarbeitung – Identitätsanforderungen
description: PCI-DSS-Anforderung 8
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 1a398601-8c48-4f8e-b3d4-eba94edad61c
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: a83040a6b5174307ea73e5473165835458d217f6
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33894916"
---
# <a name="identity-requirements-for-pci-dss-compliant-environments"></a>Identitätsanforderungen für PCI-DSS-konforme Umgebungen 
## <a name="pci-dss-requirement-8"></a>PCI-DSS-Anforderung 8

**Identifizieren und Authentifizieren des Zugriffs auf Systemkomponenten**

> [!NOTE]
> Diese Anforderungen werden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

Durch das Zuweisen einer eindeutigen Identifizierung (ID) an jede Person mit Zugriff wird sichergestellt, dass jeder Benutzer eindeutig für seine Aktionen verantwortlich ist. Wenn eine solche Verantwortlichkeit vorhanden ist, werden Aktionen für wichtige Daten und Systeme von bekannten und autorisierten Benutzern und Prozessen ausgeführt und können auf diese zurückverfolgt werden.
Die Effektivität eines Kennworts hängt größtenteils vom Entwurf und der Implementierung des Authentifizierungssystems ab – insbesondere, wie häufig ein Angreifer Kennwortversuche vornehmen kann, und welche Sicherheitsmethoden die Benutzerkennwörter am Zugangspunkt, während der Übertragung und im Speicher schützen.

> [!NOTE]
> Diese Anforderungen gelten für alle Konten, einschließlich POS-Konten (Point Of Sale) mit Verwaltungsfunktionen und alle Konten, die zum Anzeigen bzw. für den Zugriff auf Daten von Karteninhaber oder auf Systeme mit Karteninhaberdaten verwendet werden. Dazu gehören Konten, die von Anbietern und anderen Drittanbietern (z. B. für Support oder Wartung) verwendet werden. Diese Anforderungen gelten nicht für Konten, die von Endverbrauchern (z. B. Karteninhabern) verwendet werden. Allerdings sind die Anforderungen 8.1.1, 8.2, 8.5, 8.2.3 bis 8.2.5 und 8.1.6 bis 8.1.8 nicht für Benutzerkonten innerhalb einer POS-Zahlungsanwendung vorgesehen, die jeweils nur auf eine Kreditkartennummer Zugriff haben, um eine einzelne Transaktion zu erleichtern (z. B. Kassiererkonten).
 
## <a name="pci-dss-requirement-81"></a>PCI-DSS-Anforderung 8.1

**8.1** Definieren und implementieren Sie wie folgt Richtlinien und Verfahren, um eine ordnungsgemäße Identifizierungsverwaltung für Benutzer (außer Endverbraucher) und Administratoren in allen Systemkomponenten sicherzustellen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt einen Anwendungsfall und eine Beschreibung für die richtige Verwendung von Administratoren für die Beispielbereitstellung bereit.|



### <a name="pci-dss-requirement-811"></a>PCI-DSS-Anforderung 8.1.1

**8.1.1** Weisen Sie allen Benutzern eine eindeutige ID zu, bevor Sie auf Systemkomponenten oder Daten von Karteninhabern zugreifen können.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore implementiert Azure Active Directory und die rollenbasierte Zugriffssteuerung in Azure Active Directory, um sicherzustellen, dass alle Benutzer über eine eindeutige ID verfügen. Weitere Informationen finden Sie in den [PCI-Anleitungen für Identitätsverwaltung](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-812"></a>PCI-DSS-Anforderung 8.1.2

**8.1.2** Kontrollieren Sie das Hinzufügen, Löschen und Ändern von Benutzer-IDs, Anmeldeinformationen und anderen Bezeichnerobjekten.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore implementiert Azure Active Directory und die rollenbasierte Zugriffssteuerung in Azure Active Directory, um sicherzustellen, dass alle Benutzer über eine eindeutige ID verfügen. Weitere Informationen finden Sie in den [PCI-Anleitungen für Identitätsverwaltung](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-813"></a>PCI-DSS-Anforderung 8.1.3

**8.1.3** Widerrufen Sie den Zugriff für ausgeschiedene Benutzer umgehend.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore nutzt Azure Active Directory für die Benutzerverwaltung. Die Sperrung von Benutzern kann in Active Directory vorgenommen werden.|



### <a name="pci-dss-requirement-814"></a>PCI-DSS-Anforderung 8.1.4

**8.1.4** Entfernen oder deaktivieren Sie inaktive Benutzerkonten innerhalb von 90 Tagen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore nutzt Azure Active Directory für die Benutzerverwaltung. Die Option `-enableADDomainPasswordPolicy` kann so festgelegt werden, dass Kennwörter nach 90 Tagen ablaufen.|



### <a name="pci-dss-requirement-815"></a>PCI-DSS-Anforderung 8.1.5

**8.1.5** Verwalten Sie wie folgt IDs, die von Drittanbietern zum Zugriff auf bzw. zu Support- oder Wartungsaktivitäten für Systemkomponenten über Remotezugriff verwendet werden:
- Nur während des erforderlichen Zeitraums aktiviert und bei Nichtverwendung deaktiviert.
- Während der Verwendung überwacht.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure hat anwendbare geschäftliche und organisatorische Sicherheitsrichtlinien übernommen, darunter eine Richtlinie zur Informationssicherheit. Die Richtlinien wurden genehmigt, veröffentlicht und an Microsoft Azure übermittelt. Die Richtlinie zur Informationssicherheit erfordert, dass der Zugriff auf Microsoft Azure-Objekte basierend auf einer geschäftlichen Begründung mit der Autorisierung des Objektbesitzers gewährt wird und basierend auf den „Need-to-know“- und „Least-Privilege“-Prinzipien eingeschränkt ist. Außerdem betrifft die Richtlinie auch Anforderungen für den Zugriffsverwaltungs-Lebenszyklus, einschließlich Zugriffsbereitstellung, Authentifizierung und Zugriffsautorisierung, Widerruf von Zugriffsrechten und regelmäßige Zugriffsüberprüfungen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | In der Contoso Webstore-Demo ist Azure Active Directory und die rollenbasierte Zugriffssteuerung in Azure Active Directory implementiert, um den Benutzerzugriff auf die Installation zu verwalten. Weitere Informationen finden Sie in den [PCI-Anleitungen für Identitätsverwaltung](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-816"></a>PCI-DSS-Anforderung 8.1.6

**8.1.6** Begrenzen Sie wiederholte Zugriffsversuche, indem Sie die Benutzer-ID nach höchstens sechs Versuchen sperren.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Contoso Webstore hat eine klare Aufgabentrennung (Separation of Duties, SOD) für alle Benutzer der Demo implementiert. Weitere Informationen finden Sie unter „Azure Active Directory Identity Protection“ in den [ PCI-Anleitungen für Identitätsverwaltung](payment-processing-blueprint.md#identity-management).|



### <a name="pci-dss-requirement-817"></a>PCI-DSS-Anforderung 8.1.7

**8.1.7** Legen Sie die Sperrdauer auf mindestens 30 Minuten oder bis zur Aktivierung der Benutzer-ID durch einen Administrator fest.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Kunden sind für das Erstellen, Durchsetzen und Überwachen einer Kennwortrichtlinie verantwortlich, die PCI-DSS-Anforderungen erfüllt.|



### <a name="pci-dss-requirement-818"></a>PCI-DSS-Anforderung 8.1.8

**8.1.8** Legen Sie fest, dass sich der Benutzer erneut authentifizieren muss, um das Terminal oder die Sitzung erneut zu aktivieren, wenn eine Sitzung mehr als 15 Minuten inaktiv ist.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Kunden sind für das Erstellen, Durchsetzen und Überwachen einer Kennwortrichtlinie verantwortlich, die PCI-DSS-Anforderungen erfüllt.|



## <a name="pci-dss-requirement-82"></a>PCI-DSS-Anforderung 8.2

**8.2** Sorgen Sie neben der Zuweisung einer eindeutigen ID für eine ordnungsgemäße Authentifizierungsverwaltung für Benutzer (außer Endverbraucher) und Administratoren in allen Systemkomponenten, indem Sie mindestens eine der folgenden Methoden zum Authentifizieren aller Benutzer einsetzen:
- Etwas, dass man weiß, z. B. ein Kennwort oder eine Passphrase
- Etwas, das man hat, z. B. ein Token-Gerät oder eine Smartcard
- Etwas, das man hat, z. B. ein biometrisches Merkmal

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Implementierung für mehrstufige Authentifizierung wurde deaktiviert, um die Verwendung der Demo möglichst einfach zu machen. Mehrstufige Authentifizierung kann mithilfe von [Azure Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) implementiert werden.|



### <a name="pci-dss-requirement-821"></a>PCI-DSS-Anforderung 8.2.1

**8.2.1** Verwenden Sie starke Kryptografie, um alle Authentifizierungsinformationen (z. B. Kennwörter/Passphrasen) während der Übertragung und Speicherung auf allen Systemkomponenten unlesbar zu machen.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure hat zentrale Verfahren zur Verwaltung von kryptografischen Schlüsseln während des gesamten Lebenszyklus (z. B. Erstellung, Verteilung, Widerruf) eingeführt. Microsoft Azure verwendet die PKI-Unternehmensinfrastruktur von Microsoft. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erzwingt gemäß der Dokumentation im Bereitstellungshandbuch sichere Kennwörter. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).<br /><br />|



### <a name="pci-dss-requirement-822"></a>PCI-DSS-Anforderung 8.2.2

**8.2.2** Überprüfen Sie die Benutzeridentität, bevor Sie Änderungen an Anmeldeinformationen für die Authentifizierung vornehmen, z. B. Kennwörter zurücksetzen, neue Token bereitstellen oder neue Schlüssel generieren.


**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure hat zentrale Verfahren zur Verwaltung von kryptografischen Schlüsseln während des gesamten Lebenszyklus (z. B. Erstellung, Verteilung, Widerruf) eingeführt. Microsoft Azure verwendet die PKI-Unternehmensinfrastruktur von Microsoft. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erzwingt gemäß der Dokumentation im Bereitstellungshandbuch sichere Kennwörter. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-823"></a>PCI-DSS-Anforderung 8.2.3

**8.2.3** Kennwörter/Passphrasen müssen folgenden Anforderungen erfüllen:
- Sie bestehen aus mindestens sieben Zeichen.
- Sie enthalten sowohl numerische als auch alphabetische Zeichen.
Alternativ können die Kennwörter/Passphrasen mindestens eine Komplexität und Sicherheit haben, die den oben angegebenen Parametern entspricht.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erzwingt gemäß der Dokumentation im Bereitstellungshandbuch sichere Kennwörter.|



### <a name="pci-dss-requirement-824"></a>PCI-DSS-Anforderung 8.2.4

**8.2.4** Ändern Sie Benutzerkennwörter/-passphrasen mindestens alle 90 Tage.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore nutzt Azure Active Directory für die Benutzerverwaltung. Die Option `-enableADDomainPasswordPolicy` kann so festgelegt werden, dass Kennwörter nach höchstens 90 Tagen ablaufen.|



### <a name="pci-dss-requirement-825"></a>PCI-DSS-Anforderung 8.2.5

**8.2.5** Lassen Sie nicht zu, dass ein Benutzer neue Kennwörter/Passphrasen übermittelt, die mit einem/einer der vier zuletzt verwendeten Kennwörter/Passphrasen identisch sind.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erzwingt gemäß der Dokumentation im Bereitstellungshandbuch sichere Kennwörter. Weitere Informationen finden Sie in den [PCI-Anleitungen für Identitätsverwaltung](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-826"></a>PCI-DSS-Anforderung 8.2.6

**8.2.6** Legen Sie Kennwörter/Passphrasen zur erstmaligen Verwendung und nach dem Zurücksetzen auf einen eindeutigen Wert für jeden Benutzer fest, und ändern Sie diese unmittelbar nach der ersten Verwendung.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erzwingt gemäß der Dokumentation im Bereitstellungshandbuch sichere Kennwörter. Weitere Informationen finden Sie in den [PCI-Anleitungen für Identitätsverwaltung](payment-processing-blueprint.md#identity-management).<br /><br />|



## <a name="pci-dss-requirement-83"></a>PCI-DSS-Anforderung 8.3

**8.3** Sichern Sie den gesamten einzelnen Administratorzugriff, der nicht über die Konsole erfolgt, sowie den gesamten Remotezugriff auf die Karteninhaberdaten-Umgebung (Cardholder Data Environment, CDE) mit mehrstufiger Authentifizierung.

> [!NOTE]
> Mehrstufige Authentifizierung erfordert, dass mindestens zwei der drei Authentifizierungsmethoden (siehe Beschreibungen der Authentifizierungsmethoden in Anforderung 8.2 ) für die Authentifizierung verwendet werden. Die zweimalige Verwendung eines Faktors (z. B. mit zwei separaten Kennwörtern) gilt nicht als mehrstufige Authentifizierung.


**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Azure-Administratoren müssen die mehrstufige Authentifizierung verwenden, um für Wartungs- und Verwaltungsaufgaben auf Azure-Systeme und -Server zuzugreifen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erstellt während der Bereitstellung drei Konten: „admin“, „sqladmin“ und „edna“ (der während der Demoausführung bei der Web-App angemeldete Standardbenutzer). Die mehrstufige Authentifizierung ist für die Demo nicht implementiert.|



### <a name="pci-dss-requirement-831"></a>PCI-DSS-Anforderung 8.3.1

**8.3.1** Binden Sie die mehrstufige Authentifizierung für Zugriff, der nicht über die Konsole erfolgt, für Mitarbeiter mit Administratorzugriff in die CDE ein.

> [!NOTE]
> Diese Anforderung ist bis zum 31. Januar 2018 eine bewährte Methode, danach stellt sie eine Anforderung dar.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Azure-Administratoren müssen die mehrstufige Authentifizierung verwenden, um für Wartungs- und Verwaltungsaufgaben auf Azure-Systeme und -Server zuzugreifen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erstellt während der Bereitstellung drei Konten: „admin“, „sqladmin“ und „edna“ (der während der Demoausführung bei der Web-App angemeldete Standardbenutzer). Die mehrstufige Authentifizierung ist für die Demo nicht implementiert.|



### <a name="pci-dss-requirement-832"></a>PCI-DSS-Anforderung 8.3.2

**8.3.2** Binden Sie die mehrstufige Authentifizierung für alle Remotezugriffe auf das Netzwerk (sowohl durch Benutzer und Administratoren als auch durch Drittanbieter für Support- oder Wartungsaktivitäten) ein, deren Ursprung außerhalb des Netzwerks der Entität liegt.


**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Azure-Administratoren müssen die mehrstufige Authentifizierung verwenden, um für Wartungs- und Verwaltungsaufgaben auf Azure-Systeme und -Server zuzugreifen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erstellt während der Bereitstellung drei Konten: „admin“, „sqladmin“ und „edna“ (der während der Demoausführung bei der Web-App angemeldete Standardbenutzer). Die mehrstufige Authentifizierung ist für die Demo nicht implementiert.|



## <a name="pci-dss-requirement-84"></a>PCI-DSS-Anforderung 8.4

**8.4** Dokumentieren Sie Authentifizierungsrichtlinien und -verfahren, und teilen Sie sie allen Benutzern mit, einschließlich:
- Anleitungen für die Auswahl sicherer Authentifizierungsanmeldeinformationen
- Anleitungen dazu, wie Benutzer ihre Anmeldeinformationen für die Authentifizierung schützen müssen
- Die Anweisung, Kennwörter nicht wiederzuverwenden
- Die Anweisung, Kennwörter zu ändern, wenn ein Verdacht besteht, dass sie gefährdet sind

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Kunden sind dafür verantwortlich, Anleitungen zu befolgen und Authentifizierungsverfahren und -richtlinien zu dokumentieren und allen Benutzern mitzuteilen.|



## <a name="pci-dss-requirement-85"></a>PCI-DSS-Anforderung 8.5

**8.5** Verwenden Sie IDs, Kennwörter oder andere Authentifizierungsmethoden, die für Gruppen bestimmt, freigegeben oder generisch sind nicht wie folgt:
- Generische Benutzer-IDs werden deaktiviert oder entfernt.
- Für die Systemverwaltung und andere wichtige Funktionen sind keine freigegebenen Benutzer-IDs vorhanden.
- Zur Verwaltung von Systemkomponenten werden keine freigegebenen und generischen Benutzer-IDs verwendet.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erstellt während der Bereitstellung drei Konten: „admin“, „sqladmin“ und „edna“ (der während der Demoausführung bei der Web-App angemeldete Standardbenutzer). Die mehrstufige Authentifizierung ist für die Demo nicht implementiert.|



### <a name="pci-dss-requirement-851"></a>PCI-DSS-Anforderung 8.5.1

**8.5.1**  **Zusätzliche Anforderung nur für Dienstanbieter:** Dienstanbieter mit Remotezugriff auf Kundenstandorte (z. B. für den Support für POS-Systeme oder Server) müssen für jeden Kunden eindeutige Anmeldeinformationen für die Authentifizierung (z. B. ein Kennwort/eine Passphrase) verwenden. 

> [!NOTE]
> Diese Anforderung soll nicht für gemeinsam verwendete Hostinganbieter gelten, die auf ihre eigenen Hostingumgebungen zugreifen, in denen mehrere Kundenumgebungen gehostet werden.

**Zuständigkeiten:&nbsp;&nbsp;`Not Applicable`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Gilt nicht für Microsoft Azure-Kunden. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Gilt nicht für Microsoft Azure-Kunden.|



## <a name="pci-dss-requirement-86"></a>PCI-DSS-Anforderung 8.6

**8.6** Wenn andere Authentifizierungsmechanismen (z. B. physische oder logische Sicherheitstoken, Smartcards, Zertifikate usw.) verwendet werden, muss die Verwendung dieser Mechanismen wie folgt zugewiesen werden:
- Authentifizierungsmechanismen müssen einen einzelnen Konto zugewiesen sein und dürfen nicht für mehrere Konten gemeinsam verwendet werden.
- Physische und/oder logische Maßnahmen müssen angewendet sein, um sicherzustellen, dass nur das gewünschte Konto diesen Mechanismus verwenden kann, um Zugriff zu erhalten.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erstellt während der Bereitstellung drei Konten: „admin“, „sqladmin“ und „edna“ (der während der Demoausführung bei der Web-App angemeldete Standardbenutzer). Die mehrstufige Authentifizierung ist für die Demo nicht implementiert. Der gesamte Zugriff wird über [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) verwaltet. Diese Lösung unterstützt Sie dabei, kryptografische Schlüssel und geheime Schlüssel zu schützen, die von Cloudanwendungen und -diensten verwendet werden. |



## <a name="pci-dss-requirement-87"></a>PCI-DSS-Anforderung 8.7

**8.7** Der gesamte Zugriff auf eine Datenbank, die Daten von Karteninhabern enthält (einschließlich des Zugriffs durch Anwendungen, Administratoren und alle anderen Benutzer) ist wie folgt eingeschränkt:
- Der gesamte Benutzerzugriff auf, Benutzerabfragen von und Benutzeraktionen für Datenbanken erfolgen über programmgesteuerte Methoden.
- Nur Datenbankadministratoren haben die Möglichkeit, direkt auf Datenbanken zuzugreifen oder diese abzufragen.
- Anwendungs-IDs für Datenbankanwendungen können nur von den Anwendungen (und nicht von einzelnen Benutzer oder andere Prozesse außerhalb der Anwendung) verwendet werden.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore schützt alle Daten von Karteninhabern mit Azure Key Vault, und die Verschlüsselung von Datensätzen ist in der Bereitstellungsdokumentation beschrieben. Weitere Informationen finden Sie in den [PCI-Anleitungen für Verschlüsselung](payment-processing-blueprint.md#encryption-and-secrets-management).<br /><br />|



## <a name="pci-dss-requirement-88"></a>PCI-DSS-Anforderung 8.8

**8.8** Stellen Sie sicher, dass Sicherheitsrichtlinien und betriebliche Verfahren für Identifizierung und Authentifizierung dokumentiert und verwendet werden sowie allen beteiligten Parteien bekannt sind.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Kunden sind dafür verantwortlich, sicherzustellen, dass Sicherheitsrichtlinien und betriebliche Verfahren für Identifizierung und Authentifizierung dokumentiert und verwendet werden sowie allen beteiligten Parteien bekannt sind.|




