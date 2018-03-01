---
title: "Automatisierung dreischichtiger Webanwendungen für UK-OFFICIAL – Übersicht"
description: "Automatisierung dreischichtiger Webanwendungen für UK-OFFICIAL – Übersicht"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 49652fd9-b8c6-4a88-bc5e-0b58a0260ed6
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 787f504dea6e98911d6ba4716e88aff322c70c5b
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
---
# <a name="national-cyber-security-centre-cloud-security-principles-overview"></a>Übersicht über die Prinzipien zur Sicherheit in der Cloud des National Cyber Security Centers


> [!NOTE]
> Diese Sicherheitsprinzipien werden vom National Cyber Security Center (NCSC) des Vereinigten Königreichs definiert. Informationen zu Testverfahren und Anleitungen zu den einzelnen Sicherheitsprinzipien finden Sie in der [NCSC-Dokumentation](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles).



## <a name="ncsc-cloud-security-principle-1"></a>NCSC-Cloudsicherheitsprinzip 1
### <a name="data-in-transit-protection"></a>Schutz von Daten während der Übertragung
Benutzerdaten, die in Netzwerken übertragen werden, sollten angemessen gegen Manipulationen und Abhören geschützt werden.

Dies sollte durch eine Kombination dieser Maßnahmen erreicht werden:

- Netzwerkschutz – dem Angreifer wird die Möglichkeit zum Abfangen von Daten genommen
- Verschlüsselung – dem Angreifer wird die Möglichkeit genommen, Daten zu lesen


**Zuständigkeiten:** `Shared`


|||
|---|---|
| **Kunde** | Dieser Blueprint konfiguriert Ressourcen so, dass nur sichere Protokolle für die Kommunikation verwendet werden. Die WAF-Komponente von Application Gateway ist dafür konfiguriert, Kommunikation von externen Benutzern über HTTPS/TLS anzunehmen und mit dem Back-End-Pool ausschließlich über HTTPS/TLS zu kommunizieren. Remotedesktopdienste werden für die Verwendung von sicheren Verbindungen konfiguriert. VPN wird zum Schützen von Webverkehr zwischen AppGateway und Azure verwendet. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Azure verwendet das branchenübliche TLS 1.2-Protokoll (Transport Layer Security) mit 2048-Bit RSA/SHA256-Verschlüsselungsschlüsseln, entsprechend der Empfehlung von CESG/NCSC, um Kommunikation zwischen dem Kunden und der Cloud einerseits und zwischen Azure-Systemen und Rechenzentren andererseits zu verschlüsseln. Wenn Administratoren beispielsweise das Microsoft Azure-Portal verwenden, um den Dienst für ihre Organisation zu verwalten, werden die zwischen dem Portal und dem Gerät des Administrators übertragenen Daten über einen verschlüsselten TLS-Kanal gesendet. Wenn ein E-Mail-Benutzer mittels eines Standard-Webbrowsers eine Verbindung mit Outlook.com herstellt, stellt die HTTPS-Verbindung einen sicheren Kanal für das Empfangen und Senden von E-Mail bereit.<br /> <br /> Azure bietet seinen Kunden eine Reihe von Optionen zum Schützen der eigenen Daten und des eigenen Datenverkehrs. Die in Azure integrierten Funktionen zur Zertifikatverwaltung verleihen Administratoren Flexibilität bei der Konfigurierung von Zertifikaten und Verschlüsselungsschlüsseln für Verwaltungssysteme, einzelne Dienste, SSH-Sitzungen (Secure Shell), VPN-Verbindungen (Virtual Private Network), RDP-Verbindungen (Remote Desktop) und weitere Funktionen. <br /><br /> Entwickler können die integrierten Anbieter von Kryptographiediensten (Cryptographic Service Providers, CSPs) in Microsoft .NET Framework verwenden, um auf die Funktionalität von AES-Algorithmen (Advanced Encryption Standard) und SHA-2 (Secure Hash Algorithm) zuzugreifen, um Aufgaben wie z. B. die Überprüfung digitaler Signaturen zu verarbeiten. Azure Key Vault hilft Kunden bei der sicheren Aufbewahrung von kryptografischen Schlüsseln und Geheimnissen durch die Speicherung in Hardwaresicherheitsmodulen (HSM). |


 ## <a name="ncsc-cloud-security-principle-2"></a>NCSC-Cloudsicherheitsprinzip 2
### <a name="asset-protection-and-resilience"></a>Ressourcenschutz und Stabilität
Benutzerdaten und die für ihre Speicherung oder Verarbeitung verwendeten Ressourcen sollten vor physischer Manipulation, Verlust, Beschädigung oder Übernahme geschützt sein.

Diese Gesichtspunkte sind zu berücksichtigen:

1. Physischer Speicherort und Rechtsprechung
2. Sicherheit im Rechenzentrum
3. Schutz von ruhenden Daten
4. Datenbereinigung
5. Entsorgung von Ausrüstung
6. Physische Stabilität und Verfügbarkeit


 ## <a name="ncsc-cloud-security-principle-21"></a>NCSC-Cloudsicherheitsprinzip 2.1
### <a name="physical-location-and-legal-jurisdiction"></a>Physischer Speicherort und Rechtsprechung
Um die rechtlichen Umstände zu verstehen, unter denen auf Ihre Daten ohne Ihre Zustimmung zugegriffen werden kann, müssen Sie die Standorte kennen, an denen sie gespeichert, verarbeitet und verwaltet werden.
Darüber hinaus müssen Sie verstehen, wie Regelungen zum Umgang mit Daten bezogen auf die Rechtsprechung des Vereinigten Königreichs innerhalb des Diensts durchgesetzt werden. Unangemessener Schutz von Benutzerdaten kann zu rechtlichen und behördlichen Strafmaßnahmen sowie zu einer beschädigten Reputation führen.


**Zuständigkeiten:** `Customer`

> [!NOTE]
> Azure-Dienste werden regional bereitgestellt, und Kunden können bestimmte Azure-Dienste so konfigurieren, dass Kundendaten nur in einer einzelnen Region gespeichert werden. Microsoft Azure stellt eine Liste der weltweit verfügbaren Rechenzentren zur Verfügung, um Verfügbarkeit und Zuverlässigkeit im globalen Maßstab zu bieten. Alle Azure-Rechenzentren wurden auf der Grundlage von ISO/IEC 27001:2013 zertifiziert. Der geografische Bereich des Vereinigten Königreichs besteht aus 2 Regionen: UK Süden und UK Westen.

|||
|---|---|
| **Kunde** | Dieser Blueprint fordert den Administrator zur Angabe auf, für welche Region Azure-Ressourcen bereitgestellt werden sollen. Die empfohlenen Regionen für die Bereitstellung sind UK Süden und UK Westen. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Nicht zutreffend |


 ## <a name="ncsc-cloud-security-principle-22"></a>NCSC-Cloudsicherheitsprinzip 2.2
### <a name="datacenter-security"></a>Sicherheit im Rechenzentrum
Die zur Bereitstellung von Clouddiensten verwendeten Standorte müssen physischen Schutz vor unberechtigtem Zutritt, Manipulation, Diebstahl oder Umkonfigurierung von Systemen bieten. Unangemessene Schutzmaßnahmen können zur Offenlegung, zur Veränderung oder zum Verlust von Daten führen.


**Zuständigkeiten:** `Microsoft Azure`


|||
|---|---|
| **Kunde** | Kunden besitzen keinerlei physischen Zugang zu Systemressourcen in Azure-Rechenzentren; Schutzmaßnahmen für die Sicherheit der Rechenzentren werden von Microsoft Azure implementiert und verwaltet. Dieses Prinzip wurde von Microsoft Azure geerbt. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure implementiert dieses Prinzip im Kundenauftrag. Microsoft Azure wird in geografisch verteilten Anlagen von Microsoft ausgeführt und teilt sich Räumlichkeiten und Versorgungsunternehmen mit anderen Microsoft-Onlinediensten. Jede Anlage ist für einen ununterbrochenen Betrieb – 24x7x365 – ausgelegt und setzt verschiedene branchenübliche Methoden ein, um den Betrieb vor Stromausfall, physischem Eindringen und Netzwerkausfällen zu schützen. Diese Rechenzentren entsprechen Branchenstandards (wie etwa ISO 27001) für die physische Sicherheit und Verfügbarkeit. Sie werden von Microsoft-Betriebspersonal verwaltet und überwacht. <br /> <br /> Azure-Kunden können darauf vertrauen, dass in allen Azure-Rechenzentren physische Sicherheitskontrollen wirksam sind, da Azure für alle Rechenzentren über Zertifikate nach der ISO/IEC-Norm 27001:2013 verfügt. Der geografische Bereich des Vereinigten Königreichs besteht aus zwei Regionen: „Vereinigtes Königreich, Süden“ und „Vereinigtes Königreich, Westen“. |


 ## <a name="ncsc-cloud-security-principle-23"></a>NCSC-Cloudsicherheitsprinzip 2.3
### <a name="data-at-rest-protection"></a>Schutz von ruhenden Daten
Um sicherzustellen, dass Daten für Unberechtigte mit physischem Zugang zu Infrastruktur nicht verfügbar sind, sollten die innerhalb des Diensts verwahrten Daten unabhängig vom verwendeten Speichermedium geschützt sein. Wenn keine angemessenen Maßnahmen wirksam sind, können Daten auf aussortierten, verlorenen oder gestohlenen Medien unabsichtlich offengelegt oder verworfen werden.


**Zuständigkeiten:** `Shared`

> [!NOTE]
> Alle von Microsoft Azure für seine Kunden bereitgestellten Verschlüsselungslösungen lassen sich leicht mit Azure Key Vault integrieren, das eine einfache Verwaltung von Verschlüsselungsschlüsseln ermöglicht.

|||
|---|---|
| **Kunde** | Die Vertraulichkeit und Integrität des von dieser Blueprint-Lösung bereitgestellten Blob-Speichers ist ausnahmslos durch die Verwendung von Azure-Speicherdienstverschlüsselung (Storage Service Encryption, SSE) geschützt. Die SSE schützt ruhende Daten innerhalb von Azure-Speicherkonten mithilfe von 256-Bit-AES-Verschlüsselung. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Azure bietet eine große Bandbreite an Verschlüsselungsfunktionen und gibt Kunden so die Flexibilität, die für ihre Bedürfnisse optimal geeignete Lösung zu wählen. Azure Key Vault hilft Kunden, auf einfache und kostengünstige Weise die Kontrolle über die von Cloudanwendungen und -diensten für die Verschlüsselung von Daten verwendeten Schlüssel zu behalten. Mithilfe von Azure Disk Encryption können Kunden virtuelle Computer verschlüsseln. Mithilfe der Azure-Speicherdienstverschlüsselung können alle Daten im Speicherkonto eines Kunden verschlüsselt werden. |


 ## <a name="ncsc-cloud-security-principle-24"></a>NCSC-Cloudsicherheitsprinzip 2.4
### <a name="data-sanitization"></a>Datenbereinigung
Der Vorgang des Bereitstellens, Migrierens und Aufhebens der Bereitstellung von Ressourcen sollte nicht zu einem unberechtigten Zugriff auf die Benutzerdaten führen.

Unzureichende Sanitisierung von Daten kann zu folgenden Problemen führen:

- Benutzerdaten werden vom Dienstanbieter zeitlich unbegrenzt aufbewahrt
- Auf Benutzerdaten kann bei Wiederverwendung von Ressourcen von anderen Benutzern zugegriffen werden
- Benutzerdaten gehen auf aussortierten, verlorenen oder gestohlenen Medien verloren oder werden offengelegt.


**Zuständigkeiten:** `Microsoft Azure`


|||
|---|---|
| **Kunde** | Microsoft Azure bietet vertragliche Zusicherungen hinsichtlich der endgültigen Löschung von Daten in Azure. Dieses Prinzip wurde in dieser Form von Microsoft Azure geerbt. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure entspricht dem NIST SP800-88r1-Entsorgungsprozess mit einer an FIPS-199 Moderate ausgerichteten Datenklassifizierung. NIST sieht für Laufwerke, die ihn unterstützen, den Secure Erase-Ansatz (mithilfe von Festplatten-Firmware) vor. Festplatten, die nicht zurückgesetzt werden können, werden von Microsoft zerstört, und die Wiederherstellung der enthaltenen Informationen wird unmöglich gemacht (z. B. durch Zerlegung, Schreddern, Pulverisieren oder Verbrennen). Das geeignete Entsorgungsverfahren wird durch den Typ der Ressource bestimmt. Die Zerstörung wird dokumentiert, und die Dokumentation wird aufbewahrt. Alle Microsoft Azure-Dienste setzen anerkannte Dienste zur Speicherung auf Medien und Verwaltung der Entsorgung ein. <br />  <br /> Bei Ablauf oder Beendigung eines Dienstabonnements kann sich der Kunde mit Microsoft in Verbindung setzen und Anweisungen zu diesen Optionen erteilen: <br /><br /> (1) Das Konto des Kunden wird deaktiviert, und anschließend werden die Kundendaten gelöscht; oder <br /> (2) Die Daten bleiben im Onlinedienst in einem Konto mit eingeschränkter Funktionalität für wenigstens 90 Tage (den „Aufbewahrungszeitraum“) nach dem Ablauf oder der Beendigung des Abonnements des Kunden gespeichert, sodass der Kunde die Daten extrahieren kann. Im Anschluss an den Ablauf des Aufbewahrungszeitraums deaktiviert Microsoft das Konto des Kunden und löscht alle Daten. Cache- oder Sicherungskopien werden innerhalb von 30 Tagen nach dem Ende des Aufbewahrungszeitraums gelöscht. |


 ## <a name="ncsc-cloud-security-principle-25"></a>NCSC-Cloudsicherheitsprinzip 2.5
### <a name="equipment-disposal"></a>Entsorgung von Ausrüstung
Sobald die für die Bereitstellung eines Diensts verwendete Ausrüstung das Ende ihrer Nutzungsdauer erreicht, sollte sie auf eine Weise entsorgt werden, die keine Beeinträchtigung der Sicherheit des Diensts oder der darin gespeicherten Daten mit sich bringt.


**Zuständigkeiten:** `Microsoft Azure`


|||
|---|---|
| **Kunde** | Kunden besitzen keinerlei physischen Zugang zu Systemressourcen in Azure-Rechenzentren; die Verfahren zur Entsorgung von Ausrüstung werden von Microsoft Azure implementiert und verwaltet. Dieses Prinzip wurde von Microsoft Azure geerbt. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure implementiert dieses Prinzip im Kundenauftrag. Beim Ende der Nutzungsdauer eines Systems befolgen die für den Betrieb zuständigen Microsoft-Mitarbeiter strenge Verfahren zum Umgang mit Daten und zur Entsorgung von Hardware, um sicherzustellen, dass keine Hardware, die eventuell Kundendaten enthält, nicht vertrauenswürdigen Dritten zugänglich gemacht wird. Microsoft Azure entspricht dem NIST SP800-88r1-Entsorgungsprozess mit einer an FIPS-199 Moderate ausgerichteten Datenklassifizierung. NIST sieht für Laufwerke, die ihn unterstützen, den Secure Erase-Ansatz (mithilfe von Festplatten-Firmware) vor. Festplatten, die nicht zurückgesetzt werden können, werden von Microsoft zerstört, und die Wiederherstellung der enthaltenen Informationen wird unmöglich gemacht (z. B. durch Zerlegung, Schreddern, Pulverisieren oder Verbrennen). Das geeignete Entsorgungsverfahren wird durch den Typ der Ressource bestimmt. Die Zerstörung wird dokumentiert, und die Dokumentation wird aufbewahrt. Alle Microsoft Azure-Dienste setzen anerkannte Dienste zur Speicherung auf Medien und Verwaltung der Entsorgung ein. |


 ## <a name="ncsc-cloud-security-principle-26"></a>NCSC-Cloudsicherheitsprinzip 2.6
### <a name="physical-resilience-and-availability"></a>Physische Stabilität und Verfügbarkeit
Dienste weisen verschiedene Stufen von Ausfallsicherheit auf, die sich auf ihre Fähigkeit auswirken, den normalen Betrieb bei Ausfällen, Schadensfällen oder Angriffen aufrecht zu erhalten. Bei einem Dienst ohne garantierte Verfügbarkeit können unter Umständen Ausfälle auftreten, möglicherweise sogar über einen längeren Zeitraum, ohne Rücksicht auf die Auswirkungen auf Ihr Unternehmen.


**Zuständigkeiten:** `Shared`

> [!NOTE]
> Wenn der Kunde Microsoft Azure entsprechend konfiguriert, indem er Azure Site Recovery und eine alternative Speicherung der Daten in einem geografisch anders gelegenen Rechenzentrum aktiviert, kann Microsoft Azure den fortgesetzten Betrieb der vom Kunden bereitgestellten Ressourcen unterstützen.

|||
|---|---|
| **Kunde** | Der Kunde ist für die Einrichtung eines alternativen Speicherorts verantwortlich. In der Kundenanweisung zur Regelungsimplementierung sollte die Fähigkeit des Kunden zum Betrieb im Schadensfall behandelt werden. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure verfügt über vom NCSC (UK National Cyber Security Center, Nationales Zentrum für Cybersicherheit des Vereinigten Königreichs) genehmigte Rechenzentren an verschiedenen geografischen Standorten (Vereinigtes Königreich, Süden und Vereinigtes Königreich, Westen), um Stabilität und Verfügbarkeit bereitzustellen. Es liegt in der Verantwortung des Kunden, mithilfe des Azure Site Recovery-Diensts Kapazität in einer alternativen Region zu reservieren. Nachdem Azure Site Recovery konfiguriert wurde, startet und beendet Azure die Dienste des Kunden mit nahtlosem Übergang zum alternativen Verarbeitungsstandort. |


 ## <a name="ncsc-cloud-security-principle-3"></a>NCSC-Cloudsicherheitsprinzip 3
### <a name="separation-between-users"></a>Trennung von Benutzern
Ein böswilliger oder sicherheitsgefährdeter Benutzer des Diensts sollte nicht imstande sein, Einfluss auf den Dienst oder die Daten eines anderen zu erlangen.

Zu den Faktoren, die sich auf die Trennung von Benutzern auswirken, gehören:

- die Stelle, an der die Regelungen zur Trennung implementiert sind – dies wird stark durch das Dienstmodell (z.B. IaaS, PaaS, SaaS) beeinflusst
- mit wem Sie den Dienst teilen – dies wird durch das Bereitstellungsmodell (z.B. öffentliche, private oder Communitycloud) vorgeschrieben
- das Maß an Gewissheit, das in der Implementierung der Trennungsregelungen verfügbar ist

> [!NOTE]
> Bei einem IaaS-Dienst sollten Sie die durch Compute-, Speicher- und Netzwerkkomponenten bereitgestellte Trennung betrachten. Darüber hinaus können auch auf IaaS beruhende SaaS- und PaaS-Dienste möglicherweise einige der Trennungseigenschaften der zugrundeliegenden IaaS-Infrastruktur erben.

**Zuständigkeiten:** `Microsoft Azure`


|||
|---|---|
| **Kunde** | Microsoft Azure stellt Isolation für jeden Benutzer sicher, um eine Beeinflussung von Dienst oder Daten eines anderen durch einen böswilligen oder sicherheitsgefährdeten Benutzer zu verhindern. Dieses Prinzip wurde in dieser Form von Microsoft Azure geerbt. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Da Cloudserver von Kunden virtuell sind, ist das Paradigma der physischen Trennung nicht mehr zutreffend. Microsoft Azure wurde dafür entwickelt, die in einer Umgebung mit mehreren Mandanten inhärenten Risiken zu erkennen und ihnen entgegenzuwirken. Die Speicherung und Verarbeitung von Daten ist zwischen Benutzern von Azure logisch mithilfe von Active Directory und speziell für Dienste mit mehreren Mandanten entwickelte Funktionalität getrennt, mit dem Ziel, sicherzustellen, dass auf die in gemeinsam verwendeten Azure-Rechenzentren gespeicherten Benutzerdaten kein Zugriff durch andere Organisationen möglich ist. <br /> <br /> Für jede gemeinsam verwendete Cloudarchitektur ist die für jeden Kunden bereitgestellte Isolation fundamental, um eine Beeinflussung von Dienst oder Daten eines anderen durch einen böswilligen oder sicherheitsgefährdeten Benutzer zu verhindern. <br /> <br /> Weitere Informationen zur Microsoft-Mandantentrennung finden Sie in der umfassenden Beschreibung unter [Azure Security and Compliance Blueprint – NCSC-Cloudsicherheitsprinzipien – Kundenzuständigkeitsmatrix](https://aka.ms/blueprintuk-gcrm). |


 ## <a name="ncsc-cloud-security-principle-4"></a>NCSC-Cloudsicherheitsprinzip 4
### <a name="governance-framework"></a>Governance-Framework
Der Dienstanbieter sollte über ein Governance-Framework zur Sicherheit verfügen, das zur Koordinierung und Steuerung seiner Verwaltung des Diensts und der darin enthaltenen Informationen verwendet wird. Alle technischen Regelungen, die außerhalb dieses Frameworks bereitgestellt werden, werden dadurch grundsätzlich unterminiert.
Die Verwendung eines effektiven Governance-Frameworks stellt sicher, dass Verfahrens-, Mitarbeiter-, physische und technische Kontrollen während der gesamten Lebensdauer eines Diensts funktionieren. Es sollte außerdem auf Änderungen am Dienst, technische Entwicklungen und das Auftreten neuer Bedrohungen reagieren.


**Zuständigkeiten:** `Microsoft Azure`


|||
|---|---|
| **Kunde** | Microsoft Azure unterhält ein dokumentiertes Governance-Framework zur Sicherheit für Azure-Dienste. Dieses Prinzip wurde in dieser Form von Microsoft Azure geerbt. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Das Microsoft-Compliance-Framework umfasst eine Standardmethodik zur Definition von Compliancedomänen, zur Bestimmung der für ein bestimmtes Team oder eine bestimmte Ressource geltenden Ziele und zur Erfassung, wie die Ziele der Domänenkontrolle für ihre Anwendung auf einen bestimmten Satz Branchenstandards, Bestimmungen oder Geschäftsanforderungen hinreichend detailliert behandelt werden. Das Framework bildet Regelungen auf mehrere regulatorische Standards ab, was es Microsoft ermöglicht, Dienste unter Verwendung einer gemeinsamen Menge an Regelungen zu entwerfen und zu erstellen, wodurch die Compliance übergreifend über einen Bereich von heute geltenden und sich zukünftig entwickelnden Vorschriften optimiert wird. <br /> <br /> Die Microsoft-Complianceprozesse machen es Kunden darüber hinaus leichter, Compliance übergreifend über mehrere Dienste zu erreichen und deren sich wandelnde Anforderungen effizient zu erfüllen. In der Kombination haben es sicherheitssteigernde Technologien und wirksame Complianceprozesse Microsoft ermöglicht, eine umfangreiche Sammlung von Zertifizierungen von Drittanbietern auf- und weiter auszubauen. Mit diesen Zertifizierungen können Kunden ihren Kunden, Auditoren und Regulierungsbehörden leichter ihre Compliancebereitschaft vor Augen führen. <br /> <br />  Azure steht mit einer umfassenden Sammlung internationaler sowie regionaler und branchenspezifischer Compliancenormen im Einklang, wie etwa ISO 27001, FedRAMP, SOC 1 und SOC 2. Die Compliance mit den in diesen Normen enthaltenen strengen Sicherheitsregulierungen wird durch gründliche Audits von Drittanbietern überprüft, die aufzeigen, dass Azure-Dienste mit Branchenstandards, Zertifizierungen, Attestierungen und Autorisierungen auf Weltniveau funktionieren und diesen entsprechen. <br /> <br /> Azure wurde mit einer Compliancestrategie entwickelt, die Kunden hilft, Geschäftsziele ebenso wie Branchenstandards und behördliche Vorschriften zu berücksichtigen. Das Compliance-Framework zur Sicherheit beinhaltet Test- und Auditphasen, Sicherheitsanalysen, optimale Verfahren zum Risikomanagement und Benchmarkanalysen der Sicherheit, um Zertifikate und Attestierungen zu erhalten. <br /> <br /> Weitere Informationen zur Einhaltung von Compliance-Frameworks durch Microsoft finden Sie in der umfassenden Beschreibung in der [Azure Security and Compliance Blueprint – NCSC-Cloudsicherheitsprinzipien – Kundenzuständigkeitsmatrix](https://aka.ms/blueprintuk-gcrm).|


 ## <a name="ncsc-cloud-security-principle-5"></a>NCSC-Cloudsicherheitsprinzip 5
### <a name="operational-security"></a>Betriebssicherheit
Der Dienst muss sicher betrieben und verwaltet werden, um Angriffe erschweren, erkennen oder verhindern zu können. Gute Betriebssicherheit sollte keine komplexen, bürokratischen, zeitraubenden oder teuren Prozesse erfordern.

Vier Elemente sind zu berücksichtigen:

- Konfigurationsverwaltung und Change Management: Sie sollten sicherstellen, dass Änderungen am System ordnungsgemäß getestet und genehmigt wurden. Änderungen sollten nicht zu unerwarteten Änderungen an Sicherheitseigenschaften führen
- Management von Sicherheitsrisiken: Sie sollten Sicherheitsprobleme in beteiligten Komponenten bestimmen und entschärfen
- Vorbeugende Überwachung: Sie sollten Verfahren implementieren, mit denen Angriffe und unautorisierte Aktivitäten im Dienst erkannt werden können
- Incident Management: Stellen Sie sicher, dass Sie auf Incidents reagieren und die Sicherheit und Verfügbarkeit Ihres Diensts wiederherstellen können




 ## <a name="ncsc-cloud-security-principle-51"></a>NCSC-Cloudsicherheitsprinzip 5.1
### <a name="configuration-and-change-management"></a>Konfigurationsverwaltung und Change Management
Sie sollten ein genaues Bild von den Ressourcen haben, aus denen der Dienst besteht, zusammen mit ihrer Konfiguration und ihren Abhängigkeiten.
Änderungen, die Auswirkungen auf die Sicherheit des Diensts haben könnten, sollten bestimmt und verwaltet werden. Unbefugte Änderungen müssen erkannt werden.
Wo Änderungen nicht wirksam verwaltet werden, können unwillentlich Sicherheitsrisiken in einen Dienst eingeführt werden. Und selbst wenn ein Bewusstsein für ein Risiko vorhanden ist, wird es möglicherweise nicht vollständig entschärft.


**Zuständigkeiten:** `Shared`


|||
|---|---|
| **Kunde** | Die Azure Resource Manager-Vorlagen und begleitenden Ressourcen, aus denen dieser Blueprint besteht, stellen die Baseline einer „Konfiguration als Code“ für die bereitgestellte Architektur dar. Die Lösung wird über GitHub zur Verfügung gestellt, das auch für die Konfigurationssteuerung verwendet werden kann. <br /> <br /> Kontoberechtigungen von Azure Active Directory werden mithilfe einer rollenbasierten Zugriffssteuerung implementiert, bei der Benutzer Rollen zugewiesen werden, die streng kontrollieren, welche Benutzer bereitgestellte Ressourcen anzeigen und steuern können. Active Directory-Kontoberechtigungen werden mithilfe einer rollenbasierten Zugriffssteuerung implementiert, indem Benutzer Sicherheitsgruppen zugewiesen werden. Diese Sicherheitsgruppen steuern die Aktionen, die Benutzer hinsichtlich der Betriebssystemkonfiguration ausführen können. Diese rollenbasierten Schemas können vom Kunden erweitert werden, um unternehmenswichtige Anforderungen zu erfüllen. <br /> <br /> Um mit diesem Prinzip kompatibel zu sein, muss der Kunde für den Produktionseinsatz weitere Konfiguration vornehmen. Diese Konfigurationen müssen daher Bestandteil des Change Management-Prozesses des Kunden sein. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure überprüft und aktualisiert daher im Jahresabstand die Konfigurationseinstellungen und Basislinienkonfigurationen von Hardware, Software und Netzwerkgeräten. Änderungen werden vor der Einführung in die Produktionsumgebung in einer Entwicklungs- und/oder Testumgebung entwickelt, getestet und genehmigt. <br /> <br />Microsoft Azure wendet Basislinienkonfigurationen mithilfe des Änderungs- und Veröffentlichungsprozesses für Microsoft Azure-Softwarekomponenten (z.B. OS, Fabric, RDFE, XStore usw.) sowie des Bootstrap-Konfigurationsprozesses für Hardware- und Netzwerkgerätekomponenten, die in die Microsoft Azure-Produktionsumgebung eingeführt werden, an, wie unten kurz dargestellt. <br /> <br /> Die erforderlichen Basislinienkonfigurationen für Azure-basierte Dienste werden vom Azure-Sicherheits- und Complianceteam und von Serviceteams im Rahmen der Tests vor der Bereitstellung ihres Produktionsdiensts überprüft. |


 ## <a name="ncsc-cloud-security-principle-52"></a>NCSC-Cloudsicherheitsprinzip 5.2
### <a name="vulnerability-management"></a>Verwaltung von Sicherheitsrisiken
Bei Dienstanbietern sollte ein Verwaltungsprozess implementiert sein, um Sicherheitsrisiken zu bestimmen, selektieren und zu entschärfen. Dienste, bei denen das nicht der Fall ist, werden schnell anfällig für Angriffe mithilfe öffentlich bekannter Methoden und Werkzeuge.


**Zuständigkeiten:** `Customer`


|||
|---|---|
| **Kunde** | Der Kunde ist für die Überprüfung auf Sicherheitsrisiken auf vom Kunden bereitgestellten Ressourcen (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) verantwortlich. In der Kundenanweisung zur Implementierung sollten die für die Durchführung der Überprüfungen auf Sicherheitsrisiken verwendeten Tools erwähnt werden. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Die Verwaltung von Sicherheitsupdates hilft beim Schützen von Systemen vor bekannten Sicherheitsrisiken. Azure verwendet integrierte Bereitstellungssysteme zur Verwaltung der Verteilung und Installation von Sicherheitsaktualisierungen für Microsoft-Software. Azure kann darüber hinaus die Ressourcen des Microsoft Security Response Center (MSRC) einbeziehen, das Sicherheitsvorfälle und Sicherheitsrisiken in der Cloud täglich rund um die Uhr bestimmt, überwacht, darauf reagiert und beseitigt. |


 ## <a name="ncsc-cloud-security-principle-53"></a>NCSC-Cloudsicherheitsprinzip 5.3
### <a name="protective-monitoring"></a>Vorbeugende Überwachung
Ein Dienst, der nicht wirksam auf Angriffe, missbräuchliche Verwendung und Fehlfunktionen überprüft wird, wird Angriffe (seien sie erfolgreich oder nicht) wahrscheinlich nicht erkennen. Daher ist er auch nicht imstande, auf potenzielle Gefährdungen Ihrer Umgebung und Ihrer Daten schnell zu reagieren.


**Zuständigkeiten:** `Customer`


|||
|---|---|
| **Kunde** | Der Kunde ist für die Überwachung der vom Kunden bereitgestellten Ressourcen (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) verantwortlich. In der Kundenanweisung zur Regulierungsimplementierung sollten die zu Mechanismen zum Überwachen, Erkennen und Antworten auf Angriffe, missbräuchliche Verwendung und Fehlfunktion angesprochen werden. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure Security weist definierte Anforderungen für die aktive Überwachung auf. Dienstteams konfigurieren die Tools für die aktive Überwachung in Übereinstimmung mit diesen Anforderungen. Zu den Tools für die aktive Überwachung gehören der Monitoring Agent (MA) und der System Center Operations Manager (SCOM), die dafür konfiguriert sind, Warnungen für Mitarbeiter von Microsoft Azure Security in Situationen bereitzustellen, die sofortiges Eingreifen erfordern. |


 ## <a name="ncsc-cloud-security-principle-54"></a>NCSC-Cloudsicherheitsprinzip 5.4
### <a name="incident-management"></a>Incident Management
Wenn keine sorgfältig im Voraus geplanten Prozesse für das Incident Management vorhanden sind, kann es beim Eintreten von Incidents zu Fehlentscheidungen kommen, was die Gesamtauswirkungen auf die Anwender noch verschärfen kann.
Diese Prozesse müssen nicht komplex sein oder große Mengen an Beschreibungen erfordern, aber gutes Incident Management minimiert die Auswirkungen von Sicherheits-, Zuverlässigkeits- und Umweltproblemen auf die Benutzer eines Dienstes.


**Zuständigkeiten:** `Shared`

> [!NOTE]
> In Fällen, in denen Sicherheitsvorfälle beim Kunden den Sicherheitsstatus von Microsoft Azure beeinträchtigen kann, ist der Kunde für die Benachrichtigung von Microsoft Azure verantwortlich.

|||
|---|---|
| **Kunde** | Der Kunde ist für die Einrichtung eines Prozesses für das Incident Management für die vom Kunden bereitgestellten Ressourcen (bestehend aus Anwendungen, Betriebssystemen, Datenbanken und Software) verantwortlich. Die Kundenanweisung zur Implementierung sollte das Berichten von Incidents und Warnungen, die Unterstützung von zeitnahen Reaktionen auf Incidents und das Weiterleiten von Informationen an PGA und andere HMG-Organisationen in sinnvollem Umfang ansprechen. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Microsoft hat einen Prozess zum Verwalten von Sicherheitsvorfällen implementiert, um eine koordinierte Antwort auf Incidents zu erleichtern, sollte einer eintreten. <br /> <br /> Wenn Microsoft unberechtigte Zugriffe auf auf seiner Ausrüstung oder in seinen Einrichtungen gespeicherte Kundendaten bekannt wird, oder unberechtigte Zugriffe auf solche Ausrüstungen und Einrichtungen, die zu Verlust, Offenlegung oder Veränderung von Kundendaten führen, hat Microsoft sich zur Durchführung dieser Aktionen bekannt: <br /> <br />   – Schnelle Benachrichtigung des Kunden vom Sicherheitsvorfall; <br /> – Schnelle Untersuchung des Sicherheitsvorfalls und Bereitstellen von detaillierten Informationen über den Sicherheitsvorfall für den Kunden; und <br /> – Unmittelbares Ergreifen angemessener Schritte zum Abmildern der Wirkungen und zum Minimieren der aus dem Sicherheitsvorfall resultierenden Schäden.  <br />  <br /> Es wurde ein Incident-Management-Framework mit definierten Rollen und zugewiesenen Zuständigkeiten eingerichtet. Das WASIM-Team (Windows Azure Security Incident Management) ist für das Verwalten von Sicherheitsvorfällen zuständig, einschließlich Eskalation und der Hinzuziehung von Spezialistenteams, sollte dies erforderlich sein. Azure-Betriebsmanager sind zusammen mit Unterstützung durch andere Funktionen für das Überwachen der Untersuchung und Behebung von Sicherheits- und Datenschutzvorfällen zuständig. <br /> <br /> Weitere Informationen zu den Microsoft-Prozessen für die Reaktion auf Incidents finden Sie in der vollständigen Beschreibung unter [Azure Security and Compliance Blueprint – NCSC-Cloudsicherheitsprinzipien – Kundenzuständigkeitsmatrix](https://aka.ms/blueprintuk-gcrm). |


 ## <a name="ncsc-cloud-security-principle-6"></a>NCSC-Cloudsicherheitsprinzip 6
### <a name="personnel-security"></a>Personalsicherheit
Wenn Mitarbeiter des Dienstanbieters Zugriff auf Ihre Daten und Ihre Systeme haben, brauchen Sie sehr viel Vertrauen in deren Vertrauenswürdigkeit. Gründliche Überprüfung, unterstützt durch angemessene Schulung, verringert die Wahrscheinlichkeit von Vorfällen oder böswilliger Kompromittierung durch Mitarbeiter des Dienstanbieters.
Der Dienstanbieter sollte seine Mitarbeiter Sicherheitsüberprüfungen und regelmäßigen Sicherheitsschulungen unterziehen. Mitarbeiter in diesen Rollen sollten sich ihrer Verantwortung bewusst sein. Die Anbieter sollten darlegen, wie sie Mitarbeiter in privilegierten Rollen überwachen und verwalten.


**Zuständigkeiten:** `Shared`


|||
|---|---|
| **Kunde** | Der Kunde ist für die Überprüfung von Personen und für das Bereitstellen regelmäßiger Sicherheitsschulungen für Personen mit Zugriff auf vom Kunden bereitgestellte Ressourcen verantwortlich. Die Kundenimplementierungsanweisung sollte die Überprüfungskriterien für Rollen und die Häufigkeit von Sicherheitsschulungen ansprechen. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure-Mitarbeiter, die Azure-Dienste betreiben und Kundensupport bereitstellen (oder Microsoft-Vertragspartner, die unterstützend in Plattformbetrieb, Problembehandlung und technischem Support tätig sind) durchlaufen eine Microsoft-Standardüberprüfung (oder eine gleichwertige Überprüfung), um Ausbildung, Erwerbsverlauf und Vorstrafenregister zu bewerten. Die Hintergrundüberprüfungen entsprechen weitgehend den Anforderungen von BPSS/BS7858 der UK-Regierung. Sie umfassen jedoch keine förmliche Identitätsüberprüfung.  <br /> <br /> Die Verträge von Mitarbeitern und Vertragspartnern mit Microsoft enthalten Geheimhaltungsbestimmungen. Alle geeigneten Microsoft-Mitarbeiter und -Vertragspartner nehmen an einem von Microsoft Azure bezahlten Sicherheitsschulungsprogramm teil, das die Mitarbeiter über ihre Verantwortung für die Informationssicherheit informiert. <br /> <br /> Mitarbeiter oder Vertragspartner von Microsoft Azure-Diensten, die im Verdacht stehen, Sicherheitsverletzungen zu begehen und/oder gegen die Richtlinie zur Informationssicherheit zu verstoßen, werden einem Untersuchungsverfahren und geeigneten Disziplinarmaßnahmen ausgesetzt, die bis zur Kündigung reichen können. Wenn es durch die Umstände gedeckt ist, kann Microsoft die Angelegenheit zur Verfolgung an eine Strafverfolgungsbehörde übergeben. <br /> <br /> Um dieses System aus Hintergrundüberprüfungen und Sicherheitserziehung zu ergänzen, setzt Microsoft Kombinationen aus vorbeugenden, abwehrenden und reaktiven Regelungen ein, um den Schutz vor unberechtigten Entwickler- und/oder Administratoraktivitäten zu stärken; dazu gehören die folgenden Mechanismen: <br />  <br /> – Strenge Zugriffssteuerung für vertrauliche Daten, einschließlich der Anforderung von SmartCard-basierter, zweistufiger Authentifizierung für die Ausführung sensibler Vorgänge. <br /> – Kombinationen von Regelungen, die die unabhängige Erkennung von böswilligen Aktivitäten verbessern. <br /> – Mehrstufige Überwachung, Protokollierung und Berichterstellung. |


 ## <a name="ncsc-cloud-security-principle-7"></a>NCSC-Cloudsicherheitsprinzip 7
### <a name="secure-development"></a>Sichere Entwicklung
Dienste sollten so ausgelegt und entwickelt werden, dass Bedrohungen ihrer Sicherheit erkannt und entschärft werden. Dienste, auf die das nicht zutrifft, sind möglicherweise anfällig für Sicherheitsprobleme, die Ihre Daten kompromittieren, zum Ausfall des Diensts oder zu anderen schädlichen Aktivitäten führen können.


**Zuständigkeiten:** `Shared`


|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer führen Windows-Betriebssysteme aus. Windows bietet die Echtzeitüberprüfung von Dateiintegrität, Schutz und Wiederherstellung von Dateien des Systemkerns, die als Teil von Windows oder im Rahmen von autorisierten Windows-Systemaktualisierungen über die WRP-Funktion (Windows Resource Protection) installiert werden, die Integritätsprüfung in Echtzeit ermöglicht. <br /> <br /> In Windows sind Schutzmechanismen implementiert, um die Codeausführung in eingeschränkten Arbeitsspeicherbereichen zu verhindern: No Execute (NX), zufällige Anordnung des Adressraumlayouts (Address Space Layout Randomization, ASLR) und Datenausführungsverhinderung (Data Execution Prevention, DEP). <br /> <br /> Dieser Blueprint stellt in Form von Microsoft Windows Defender hostbasierten Virenschutz für alle bereitgestellten virtuellen Windows-Computer bereit. Windows Defender ist für die umgehende automatische Aktualisierung der Antischadsoftware-Engine und der Schutzsignaturen bei Verfügbarkeit von Aktualisierungen konfiguriert. <br /> <br /> Um mit diesem Prinzip kompatibel zu sein, muss der Kunde für den Produktionseinsatz weitere Konfiguration vornehmen. So müssen diese Konfigurationen etwa Teil des sicheren Entwicklungsprozesses des Kunden sein. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Der Microsoft Security Development Lifecycle (SDL) stellt einen wirksamen Prozess zur Modellierung von Bedrohungen dar, der dazu dient, Bedrohungen und Sicherheitsrisiken in Software und Diensten zu erkennen. Bedrohungsmodellierung stellt eine Teamübung dar, die den Operations Manager, Programm-/Projektleiter, Entwickler und Tester umfasst und eine wichtige Aufgabe der Sicherheitsanalyse darstellt, die im Rahmen der Lösungsentwicklung durchgeführt wird. Teammitglieder verwenden das SDL-Tool zur Bedrohungsmodellierung, um alle Dienste und Projekte sowohl bei deren Erstellung als auch bei der Aktualisierung mit neuen Features und Funktionen zu modellieren. Bedrohungsmodelle umfassen den gesamten Code, der auf der Angriffsfläche zugänglich ist, und sämtlichen Code, der von einem Drittanbieter erstellt oder in Lizenz erworben wurde, und berücksichtigen alle Vertrauensstellungsgrenzen. Das STRIDE-System (Spoofing, Tampering, Repudiation, Information disclosure, Denial of service und Elevation of privilege – Spoofing, Manipulation, Ablehnung, Offenlegung von Informationen, Denial-of-Service und Berechtigungserweiterung) wird verwendet, um die frühe Erkennung und Behebung von Sicherheitsbedrohungen im Entwicklungsprozess zu unterstützen, bevor sie sich auf Kunden auswirken können. |


 ## <a name="ncsc-cloud-security-principle-8"></a>NCSC-Cloudsicherheitsprinzip 8
### <a name="supply-chain-security"></a>Sicherheit der Lieferkette
Der Dienstanbieter sollte sicherstellen, dass seine Lieferkette alle Sicherheitsprinzipien, deren Implementierung der Dienst für sich in Anspruch nimmt, in zufriedenstellender Weise unterstützt.
Clouddienste bauen häufig auf Produkten und Diensten von Drittanbietern auf. Wenn dieses Prinzip nicht implementiert wird, können daher folglich Kompromittierungen der Lieferkette die Sicherheit des Diensts unterminieren und sich auf die Implementierung anderer Sicherheitsprinzipien auswirken.


**Zuständigkeiten:** `Shared`


|||
|---|---|
| **Kunde** | Es liegt in der Verantwortung des Kunden, die Dokumentation der sicheren Lieferkette für jede von Drittanbietern erworbene Software und die Betriebssysteme bereitzustellen, die innerhalb seines Azure-Abonnements eingesetzt werden. In der Kundenimplementierungsanweisung sollte die Ausnahme angesprochen werden, damit die in dieser Dokumentation der Lieferkette benannten Prozesse eingehalten werden können. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Die MCSC-Gruppe (Microsoft Cloud Supply Chain) besteht aus sechs eigenständigen Teams, von denen jedes zum Schutz von Azure vor Bedrohungen der Lieferkette beiträgt.  <br />  <br /> – Beschaffung <br /> – Betrieb beim Kunden <br /> – Qualität der Bereitstellung <br /> – Verwaltung der Lieferantenbeziehungen <br /> – Ersatz <br />  <br /> Weitere Informationen zur Microsoft MCSC-Gruppe finden Sie in der vollständigen Beschreibung unter [Azure Security and Compliance Blueprint – NCSC-Cloudsicherheitsprinzipien – Kundenzuständigkeitsmatrix](https://aka.ms/blueprintuk-gcrm). |


 ## <a name="ncsc-cloud-security-principle-9"></a>NCSC-Cloudsicherheitsprinzip 9
### <a name="secure-user-management"></a>Sichere Benutzerverwaltung
Ihr Anbieter sollte die Tools für die sichere Verwaltung Ihrer Dienstnutzung zur Verfügung stellen. Verwaltungsoberflächen und -verfahren stellen einen grundlegenden Bestandteil der Sicherheitsbarriere dar, die den unberechtigten Zugriff auf und die unberechtigte Änderung Ihrer Ressourcen, Anwendungen und Daten verhindert.

Diese Gesichtspunkte sind zu berücksichtigen:

- Authentifizierung der Benutzer bei Verwaltungsoberflächen und Supportkanälen
- Trennung und Zugriffskontrolle innerhalb von Verwaltungsoberflächen



 ## <a name="ncsc-cloud-security-principle-91"></a>NCSC-Cloudsicherheitsprinzip 9.1
### <a name="authentication-of-users-to-management-interfaces-and-within-support-channels"></a>Authentifizierung der Benutzer bei Verwaltungsoberflächen und innerhalb von Supportkanälen
Um einen sicheren Dienst aufrecht zu erhalten, müssen die Benutzer ordnungsgemäß authentifiziert sein, bevor ihnen erlaubt wird, Verwaltungsaktivitäten auszuführen, Fehler zu melden oder Änderungen am Dienst anzufordern.
Diese Aktivitäten können über ein Webportal zur Dienstverwaltung oder über andere Kanäle, wie etwa ein Telefon oder E-Mail geleitet werden. Zu ihnen gehören wahrscheinlich solche Funktionen wie die Bereitstellung neuer Dienstelemente, das Verwalten von Benutzerkonten und das Verwalten von Benutzerdaten.
Dienstanbieter müssen sicherstellen, dass alle Verwaltungsanfragen, die möglicherweise Beeinträchtigungen der Sicherheit bewirken können, über sichere und authentifizierte Kanäle erfolgen. Wenn die Authentifizierung der Benutzer nicht stark ist, ist ein Eindringling möglicherweise imstande, privilegierte Aktionen auszuführen, die die Sicherheit von Dienst oder Daten unterminieren.


**Zuständigkeiten:** `Shared`


|||
|---|---|
| **Kunde** | Wenn Administratoren auf das Microsoft Azure Portal zugreifen, um Azure-Ressourcen für ihre Organisation zu verwalten, werden die zwischen dem Portal und dem Gerät des Administrators übermittelten Daten über einen verschlüsselten TLS-Kanal (Transport Layer Security) gesendet, der 2048-Bit-RSA/SHA256-Verschlüsselungsschlüssel verwendet, wie von CESG/NCSC empfohlen.  <br /> <br /> Dieser Blueprint setzt Windows-Authentifizierung, Remotedesktop und BitLocker ein. Diese Komponenten können für die Nutzung von gemäß FIPS 140 überprüften Kryptografiemodulen konfiguriert werden. <br /> <br /> Dieser Blueprint erzwingt die Autorisierungen für den logischen Zugriff mithilfe der rollenbasierten Zugriffssteuerung, die von Azure Active Directory durch Zuweisen von Rollen zu Benutzern, von Active Directory durch Zuweisen von Benutzern zu Sicherheitsgruppen sowie durch Maßnahmen auf der Windows-Betriebssystemebene erzwungen werden. Azure Active Directory-Rollen, die Benutzern oder Gruppen zugewiesen sind, steuern den logischen Zugriff auf Ressourcen in Azure auf Ressourcen-, Gruppen- oder Abonnementebene. Active Directory-Sicherheitsgruppen steuern den logischen Zugriff auf Ressourcen und Funktionen auf Betriebssystemebene. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Kunden verwalten ihre Azure-Ressourcen über das Azure-Portal, das Zugriff auf alle virtuellen Computer, Datenbanken, Clouddienste und sonstigen für das Kundenkonto konfigurierten Ressourcen bietet. Der Webzugriff auf das Azure-Portal ist gemäß CESG/NCSC-Empfehlung mit branchenüblichen TLS 1.2-Verbindungen (Transport Layer Security) geschützt, die 2048-Bit-RSA/SHA256-Verschlüsselungsschlüssel nutzen. Es stehen rollenbasierte Zugriffssteuerelemente zur Verfügung, die es Kunden erlauben, für bestimmte Benutzer und Gruppen eingeschränkten Zugriff auf Azure-Verwaltungsressourcen bereitzustellen. |


 ## <a name="ncsc-cloud-security-principle-92"></a>NCSC-Cloudsicherheitsprinzip 9.2
### <a name="separation-and-access-control-within-management-interfaces"></a>Trennung und Zugriffskontrolle innerhalb von Verwaltungsoberflächen
Viele Clouddienste werden mithilfe von Webanwendungen oder APIs verwaltet. Diese Schnittstellen stellen einen Schlüsselbestandteil der Sicherheit des Diensts dar. Wenn Benutzer auf den Verwaltungsoberflächen nicht angemessen voneinander getrennt werden, ist ein Benutzer möglicherweise in der Lage, den Dienst eines anderen Benutzers zu beeinträchtigen oder dessen Daten zu ändern.
Ihre berechtigten Verwaltungskonten verfügen wahrscheinlich über Zugriff auf große Datenmengen. Das Einschränken der Berechtigungen einzelner Benutzer auf die unbedingt benötigten kann dabei helfen, den durch böswillige Benutzer, gefährdete Anmeldeinformationen oder gefährdete Geräte verursachten Schaden zu begrenzen.
Die rollenbasierte Zugriffssteuerung bietet einen Mechanismus, dies zu erreichen, und stellt für Benutzer, die größere Bereitstellungen verwalten, wohl eine besonders wichtige Funktion dar.
Das Verfügbarmachen von Verwaltungsoberflächen für weniger leicht zugängliche Netzwerke (z.B. Communitynetze anstelle öffentlicher Netze) erschwert Angreifern Zugang und Angriff, da sie zuerst Zugriff auf eins dieser Netzwerke erwerben müssen. Hilfestellung beim Bewerten der Risiken der Verfügbarmachung von Oberflächen für verschiedene Arten von Netzwerken finden Sie unter Prinzip 11.


**Zuständigkeiten:** `Shared`


|||
|---|---|
| **Kunde** | Dieser Blueprint stellt eine Application Gateway-Instanz und einen Lastenausgleich bereit und konfiguriert Regeln für Netzwerksicherheitsgruppen, um das Pendeln über die Außengrenzen und zwischen den Subnetzen im Inneren zu steuern. Die Benutzerfunktionalität ist von den Funktionen zur Systemverwaltung mithilfe von Durchsetzung logischer Zugriffskontrollen und der Systemarchitektur getrennt. Oberflächen für Funktionen der Systemverwaltung sind von den Benutzeroberflächen getrennt. Alle Verwaltungsverbindungen erfolgen über einen als Bollwerk fungierenden sicheren Host (Jumpbox), der sich in einem verwalteten Subnetz befindet und dessen Regeln für die Netzwerksicherheitsgruppen so gestaltet sind, dass der Zugriff auf Produktionsressourcen in geeigneter Weise eingeschränkt wird. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Wie bereits im Abschnitt zur Trennung erwähnt, stellt die Trennung zwischen Benutzern einen Kernbestandteil von Azure dar. Azure Active Directory (Azure AD oder AAD) kann verwendet werden, um jedem Benutzer, der sich beim Azure-Portal authentifiziert, Zugriff auf genau nur die Ressourcen zu erteilen, die er anzuzeigen und zu verwalten berechtigt ist. Im Ergebnis sind verschiedene Benutzerkonten bei der Verwaltung über das gemeinsame Azure-Portal streng voneinander getrennt. |


 ## <a name="ncsc-cloud-security-principle-10"></a>NCSC-Cloudsicherheitsprinzip 10
### <a name="identity-and-authentication"></a>Identität und Authentifizierung
Jeglicher Zugriff auf Dienstschnittstellen sollte auf authentifizierte und autorisierte Personen beschränkt sein.
Schwache Authentifizierung bei diesen Schnittstellen kann unter Umständen den unberechtigten Zugriff auf Ihre Systeme ermöglichen, was zu Diebstahl oder Veränderung Ihrer Daten, Änderungen an Ihrem Dienst oder zur Dienstverweigerung führen kann.
Daher ist wichtig, dass die Authentifizierung über sichere Kanäle erfolgt. E-Mail, HTTP oder Telefon sind anfällig für Angriffe durch Abfangen und Social Engineering.


**Zuständigkeiten:** `Shared`


|||
|---|---|
| **Kunde** | Dieser Blueprint nutzt Active Directory für die Kontoverwaltung. Der Zugriff auf die von diesem Blueprint bereitgestellten Ressourcen ist über die integrierte Kerberos-Funktionalität von Azure Active Directory, Active Directory und dem Windows-Betriebssystem vor Replay-Angriffen geschützt. Bei der Kerberos-Authentifizierung enthält der vom Client gesendete Authentifikator zusätzliche Daten, z.B. eine verschlüsselte IP-Adressenliste, die Clientzeitstempel und die Ticketlebensdauer. Wenn ein Paket erneut verwendet wird, erfolgt eine Überprüfung des Zeitstempels. Ist der Zeitstempel älter als oder genauso alt wie der vorherige Authentifikator, wird das Paket zurückgewiesen. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Azure bietet Dienste zur Erfassung der Identität sowie zu deren Integration in Identitätsspeicher, die möglicherweise bereits verwendet werden. Azure AD ist ein umfassender Dienst zur Identitäts- und Zugriffsverwaltung für die Cloud, der hilft, den Zugriff auf Daten in lokalen und Cloudanwendungen zu schützen. Azure AD vereinfacht die Verwaltung von Benutzern und Gruppen außerdem durch die Kombination von Verzeichniskerndiensten, erweiterter Identitätskontrolle, Sicherheit und Anwendungszugriffsverwaltung. |


 ## <a name="ncsc-cloud-security-principle-11"></a>NCSC-Cloudsicherheitsprinzip 11
### <a name="external-interface-protection"></a>Schutz von externen Schnittstellen
Alle externen oder weniger vertrauenswürdigen Schnittstellen des Diensts sollten erkannt und angemessen verteidigt werden.
Wenn einige der verfügbar gemachten Schnittstellen privat sind (wie etwa Verwaltungsoberflächen), kann der Einfluss einer Gefährdung noch folgenreicher sein.
Sie können verschiedene Methoden verwenden, um Verbindungen mit Clouddiensten herzustellen, die Ihre Unternehmenssysteme unterschiedlich hohen Risikostufen aussetzen.


**Zuständigkeiten:** `Shared`


|||
|---|---|
| **Kunde** | Dieser Blueprint stellt Ressourcen in einer Architektur mit einem separaten Websubnetz, Datenbanksubnetz, Active Directory-Subnetz und Verwaltungssubnetz bereit. Subnetze werden logisch durch Regeln für Netzwerksicherheitsgruppen getrennt, die auf die einzelnen Subnetze angewendet werden, um den Datenverkehr zwischen den Subnetzen auf das für die System- und Verwaltungsfunktionen erforderliche Maß einzuschränken (sodass beispielsweise externer Datenverkehr keinen Zugriff auf die Datenbank-, Verwaltungs- oder Active Directory-Subnetze hat).  <br /> <br /> Es wird ein Application Gateway bereitgestellt, um externe Verbindungen mit einer vom Kunden bereitgestellten Webanwendung zu verwalten. Externe Verbindungen für den Verwaltungszugriff sind auf eine Jumpbox (Bollwerkhost) eingeschränkt, die in einem Verwaltungssubnetz bereitgestellt ist, dessen Netzwerksicherheitsregeln externe Verbindungen auf berechtigte IP-Adressen beschränken. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Microsoft verwendet ein Verfahren, das als "Red Teaming" bezeichnet wird, um die Azure-Sicherheitsregulierungen und Prozesse durch Standard-Penetrationstests zu verbessern. Das Red Team ist eine Gruppe von Vollzeitmitarbeitern von Microsoft, die zielgerichtete und hartnäckige Angriffe auf Microsoft-Infrastruktur, -Plattformen und -Anwendungen, nicht aber auf Anwendungen oder Daten von Endkunden durchführt. <br /> <br /> Weitere Informationen zu den Microsoft Red Teams sowie eine Beschreibung der Initiativen von Blue Teams finden Sie in der vollständigen Beschreibung unter [Azure Security and Compliance Blueprint – NCSC-Cloudsicherheitsprinzipien – Kundenzuständigkeitsmatrix](https://aka.ms/blueprintuk-gcrm).  |


 ## <a name="ncsc-cloud-security-principle-12"></a>NCSC-Cloudsicherheitsprinzip 12
### <a name="secure-service-administration"></a>Sichere Dienstverwaltung
Systeme, die für die Verwaltung eines Clouddiensts verwendet werden, verfügen über einen mit hohen Berechtigungen ausgestatteten Zugriff auf diesen Dienst. Ihre Gefährdung würde erhebliche Auswirkungen haben; dies schließt auch Mittel zum Umgehen der Sicherheitskontrollen und zum Stehlen oder Verändern großer Datenmengen ein.
Entwurf, Implementierung und Verwaltung von Verwaltungssystemen sollten sich an für Unternehmen bewährten Methoden orientieren und sich ihres hohen Werts für Angreifer bewusst sein.


**Zuständigkeiten:** `Shared`


|||
|---|---|
| **Kunde** | Es liegt in der Verantwortung des Kunden, eine sichere Arbeitsstation für die Verwaltung seines Azure-Abonnements und der vom Kunden bereitgestellten Ressourcen (die Anwendungen, Betriebssysteme, Datenbanken und Software umfassen) bereitzustellen. In der Kundenimplementierungsanweisung sollten die zum Entschärfen der Ausnutzungsgefahr (exploitation) der vom Kunden bereitgestellten Ressourcen verwendeten Mechanismen angesprochen werden. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Für Betriebspersonal von Microsoft Azure ist die Verwendung von sicheren Administratorarbeitsstationen (Secure Admin Workstation, SAW; auch als Arbeitsstationen mit privilegiertem Zugriff (Privileged Access Workstation) oder PAW bezeichnet). Der SAW-Ansatz ist eine Erweiterung der gut bewährten und empfohlenen Methode, bei den Administratoren das Admin- und das Benutzerkonto voneinander zu trennen. Bei dieser Methode wird ein persönlich zugewiesenes Administratorkonto verwendet, das von dem Standardbenutzerkonto des Benutzers vollständig getrennt ist. SAW baut auf dieser Kontotrennungsmethode auf, indem für solche sensiblen Konten eine vertrauenswürdige Arbeitsstation bereitgestellt wird. |


 ## <a name="ncsc-cloud-security-principle-13"></a>NCSC-Cloudsicherheitsprinzip 13
### <a name="audit-information-for-users"></a>Überwachungsinformationen für Benutzer
Ihnen sollten die Überwachungsdatensätze zur Verfügung gestellt werden, die Sie benötigen, um den Zugriff auf den Dienst und auf die darin enthaltenen Daten zu überwachen. Der Typ der Überwachungsinformationen, die Ihnen zur Verfügung stehen, hat einen direkten Einfluss auf Ihre Möglichkeit, unangemessene oder bösartige Aktivitäten innerhalb eines sinnvollen Zeitraums zu erkennen und darauf zu reagieren.


**Zuständigkeiten:** `Shared`


|||
|---|---|
| **Kunde** | Die von diesem Blueprint überwachten Ereignisse beinhalten diejenigen, die von Azure-Aktivitätsprotokollen für bereitgestellte Ressourcen, Protokollen auf Betriebssystemebene und Active Directory-Protokollen erfasst werden. Diese Ereignisprotokolle beinhalten ausreichende Informationen, um den Zeitpunkt des Eintretens von Ereignissen, das Ergebnis des Ereignisses und andere ausführliche Informationen zu bestimmen, die zur Untersuchung von Sicherheitsvorfällen dienen. Kunden können außerdem weitere Ereignisse für die Überwachung auswählen, um Unternehmensanforderungen zu entsprechen. Für alle Azure-Ressourcen sind Überwachungsprotokolle im Azure-Portal verfügbar. |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Azure Log Analytics erfasst Datensätze der Ereignisse, die innerhalb der Systeme und Netzwerke einer Organisation eintreten, unmittelbar beim Eintreten, bevor sie von anderen manipuliert werden können, und ermöglicht verschiedene Arten von Analyse durch übergreifendes Zuordnen von Daten über mehrere Computer hinweg. Mit Azure können Kunden mithilfe von Azure IaaS- und PaaS-Rollen Sicherheitsereignisse generieren und im zentralen Speicher ihrer Abonnements sammeln. Diese gesammelten Ereignisse können zur kontinuierlichen Überwachung in lokale SIEM-Systeme (Security Information and Event Management) exportiert werden. Nach der Übertragung der Daten zur Speicherung bestehen viele Möglichkeiten zum Anzeigen der Diagnosedaten. <br /> <br /> Die integrierten Azure-Diagnosefunktionen können das Debuggen unterstützen. Für Anwendungen, die in Azure bereitgestellt werden, ist standardmäßig eine Reihe von Betriebssystem-Sicherheitsereignissen aktiviert. Kunden können Ereignisse für die Überwachung hinzufügen, entfernen oder ändern, indem sie die Betriebssystem-Überwachungsrichtlinie anpassen. <br /> <br /> Der Einstieg in das Erfassen allgemeiner Protokolle mithilfe von Windows-Ereignisweiterleitung (Windows Event Forwarding, WEF) oder der höher entwickelten Azure-Diagnose ist ziemlich einfach, wenn Windows-basierte virtuelle Computer mithilfe von IaaS in Azure bereitgestellt werden. Darüber hinaus kann die Azure-Diagnose für das Erfassen von Protokollen und Ereignissen aus Instanzen der PaaS-Rolle konfiguriert werden. Bei der Verwendung von IaaS-basierten virtuellen Computern konfiguriert und aktiviert ein Kunde einfach die gewünschten Sicherheitsereignisse, in der gleichen Weise, wie er Windows Servern das Erfassen von Überwachungsprotokollen in einem lokalen Rechenzentrum ermöglichen würde. Für Webanwendungen kann darüber hinaus IIS-Protokollierung aktiviert werden, wenn es sich dabei um die Hauptanwendung und -bereitstellung in Azure handelt. Kunden haben immer die Option, Sicherheitsdaten in Speicherkonten an unterstützten geografischen Standorten ihrer Wahl zu speichern, um die Anforderungen an Datensouveränität zu erfüllen. |


 ## <a name="ncsc-cloud-security-principle-14"></a>NCSC-Cloudsicherheitsprinzip 14
### <a name="secure-use-of-the-service"></a>Sichere Verwendung des Diensts
Die Sicherheit von Clouddiensten und der darin gespeicherten Daten kann unterminiert werden, wenn der Dienst mangelhaft verwendet wird. Folglich haben Sie bei der Verwendung des Diensts bestimmte Verantwortlichkeiten, damit Ihre Daten angemessen geschützt sind.
Das Ausmaß Ihrer Verantwortung unterscheidet sich in den verschiedenen Bereitstellungsmodellen des Clouddiensts und den Szenarien, in denen Sie den Dienst verwenden möchten. Bestimmte Funktionen einzelner Dienste können ebenfalls einen Einfluss haben. Beispielsweise stellt die Weise, in der ein Content Delivery Network Ihren privaten Schlüssel schützt oder ein Cloudzahlungsanbieter betrügerische Transaktionen erkennt, jenseits der und noch vor den von den Cloudsicherheitsprinzipien abgedeckten allgemeinen Überlegungen eine wichtige Sicherheitsüberlegung dar.
Bei IaaS- und PaaS-Angeboten sind Sie für wichtige Aspekte der Sicherheit Ihrer Daten und Workloads zuständig. Wenn Sie z.B. eine IaaS-Computeinstanz bereitstellen, liegt es normalerweise in Ihrer Verantwortung, ein modernes Betriebssystem zu installieren, das Betriebssystem sicher zu konfigurieren, alle Anwendungen in sicherer Weise bereitzustellen und die Instanz darüber hinaus durch Anwenden von Patches oder Durchführen der erforderlichen Wartungen zu warten.


**Zuständigkeiten:** `Customer`

> [!NOTE]
> Der Kunde kann das Azure Security Center verwenden, um Bedrohungen mit mehr Transparenz und Kontrolle über die Sicherheit von Azure-Ressourcen zu verhindern, zu ermitteln und zu behandeln. Das Azure Security Center bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

|||
|---|---|
| **Kunde** | Die Vorlagen für Azure Resource Manager und die sie begleitenden Ressourcen, aus denen dieser Blueprint besteht, folgen einem tiefgreifenden Sicherheitsansatz. Um mit diesem Prinzip kompatibel zu sein, muss der Kunde für den Produktionseinsatz weitere Konfiguration vornehmen (beispielsweise bei DBMS-Software und der Bereitstellung von Webanwendungen). |
| **Anbieter&nbsp;(Microsoft&nbsp;Azure)** | Nicht zutreffend |

## <a name="disclaimer"></a>Haftungsausschluss

 - Dieses Dokument dient nur zu Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHEN, STILLSCHWEIGENDEN ODER GESETZLICH VORGEGEBENEN GARANTIEN HINSICHTLICH DER INFORMATIONEN IN DIESEM DOKUMENT. Dieses Dokument wird so bereitgestellt, wie es ist. Informationen und Stellungnahmen in diesem Dokument, einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne vorherige Ankündigung geändert werden. Kunden, die dieses Dokument lesen, tragen das Risiko, es zu verwenden.
 - Dieses Dokument gibt Kunden keine Rechte an geistigem Eigentum an Microsoft-Produkten oder -Lösungen.
 - Kunden dürfen dieses Dokument zu internen Referenzzwecken kopieren und verwenden.
 - Bestimmte Empfehlungen in diesem Dokument können zu einer erhöhten Nutzung von Daten, Netzwerken oder Computeressourcen in Azure sowie zu höheren Azure-Lizenz- oder -Abonnementkosten für den Kunden führen.
 - Diese Architektur soll Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollte nicht unverändert in einer Produktionsumgebung verwendet werden.
 - Dieses Dokument wurde als Referenz entwickelt und sollte nicht dazu verwendet werden, alle Mittel zu definieren, mit denen ein Kunde bestimmte Konformitätsanforderungen und -vorschriften erfüllen kann. Kunden sollten bei genehmigten Kundenimplementierungen rechtliche Unterstützung von ihrer Organisation einholen.