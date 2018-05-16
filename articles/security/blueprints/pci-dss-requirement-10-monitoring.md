---
title: Blueprint zur Azure-Zahlungsverarbeitung – Überwachungsanforderungen
description: PCI-DSS-Anforderung 10
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 293a1673-54bc-478c-9400-231074004eee
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: d3d26d31c592c34d1e51c3e2c2d6501edefa8dce
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="monitoring-requirements-for-pci-dss-compliant-environments"></a>Überwachungsanforderungen für PCI-DSS-konforme Umgebungen 
## <a name="pci-dss-requirement-10"></a>PCI-DSS-Anforderung 10

**Nachverfolgen und Überwachen des gesamten Zugriffs auf Netzwerkressourcen und Karteninhaberdaten**

> [!NOTE]
> Diese Anforderungen werden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

Protokollierungsmechanismen sowie die Möglichkeit zum Nachzuverfolgen von Benutzeraktivitäten sind entscheidend, wenn es darum geht, die Auswirkungen einer Kompromittierung von Daten zu verhindern, erkennen oder minimieren. Das Vorhandensein von Protokollen in allen Umgebungen ermöglicht umfassende Überwachung, Warnungen und Analysen, wenn etwas schief geht. Das Ermitteln der Ursache für eine Gefährdung ist ohne Systemaktivitätsprotokolle sehr schwierig, wenn nicht sogar unmöglich.

## <a name="pci-dss-requirement-101"></a>PCI-DSS-Anforderung 10.1

**10.1** Implementieren Sie Überwachungspfade, um alle Zugriffe auf Systemkomponenten mit den einzelnen Benutzern zu verknüpfen.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure beschränkt den Zugriff auf Verwaltungs- und Diagnosetools auf autorisierte Mitarbeiter mit relevanten Verantwortungsbereichen. Microsoft Azure beschränkt den privilegierten Zugriff auf in der Produktionsumgebung verwendeten Tools basierend auf Prinzipien der geringsten Rechte. Microsoft Azure erstellt und pflegt ein Protokoll des gesamten individuellen Benutzerzugriffs auf Microsoft Azure-Systemkomponenten in der Plattformumgebung.<br /><br />Komponenten der Microsoft Azure-Plattform (einschließlich Betriebssystem, CloudNet, Fabric usw.) sind für das Protokollieren und Erfassen von Sicherheitsereignissen konfiguriert. Administrator-Aktivität auf der Microsoft Azure-Plattform wird protokolliert. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verfügt über eine umfangreiche Protokollierung aller System- und Benutzeraktivitäten (einschließlich CHD-Protokollierung). Weitere Informationen finden Sie in den [PCI-Anleitungen zum Protokollieren](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-102"></a>PCI-DSS-Anforderung 10.2

**10.2** Implementieren Sie automatisierte Überwachungspfade für alle Komponenten, um die folgenden Ereignisse nachvollziehen:
- **10.2.1** Alle Zugriffe auf Daten von Karteninhabern durch Einzelbenutzer
- **10.2.2** Alle Aktionen, die von einer Person mit Stamm- oder Administratorrechten ausgeführt werden
- **10.2.3** Zugriff auf alle Überwachungspfade
- **10.2.4** Ungültige logische Zugriffsversuche
- **10.2.5** Die Verwendung von und Änderungen an Identifizierungs- und Authentifizierungsmechanismen – einschließlich aber nicht beschränkt auf die Erstellung von neuen Konten und Rechteerweiterungen – und alle Änderungen, Hinzufügungen oder Löschungen von Konten mit Stamm- oder Administratorberechtigungen
- **10.2.6** Initialisieren, Beenden oder Anhalten der Überwachungsprotokolle
- **10.2.7** Erstellen und Löschen von Objekten auf Systemebene

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure beschränkt den Zugriff auf Verwaltungs- und Diagnosetools auf autorisierte Mitarbeiter mit relevanten Verantwortungsbereichen. Microsoft Azure beschränkt den privilegierten Zugriff auf in der Produktionsumgebung verwendeten Tools basierend auf Prinzipien der geringsten Rechte. Microsoft Azure erstellt und pflegt ein Protokoll des gesamten individuellen Benutzerzugriffs auf Microsoft Azure-Systemkomponenten in der Plattformumgebung.<br /><br />Komponenten der Microsoft Azure-Plattform (einschließlich Betriebssystem, CloudNet, Fabric usw.) sind für das Protokollieren und Erfassen von Sicherheitsereignissen konfiguriert. Administrator-Aktivität auf der Microsoft Azure-Plattform wird protokolliert. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verfügt über eine umfangreiche Protokollierung aller System- und Benutzeraktivitäten, einschließlich CHD-Protokollierung. Weitere Informationen finden Sie in den [PCI-Anleitungen zum Protokollieren](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-103"></a>PCI-DSS-Anforderung 10.3

**10.3** Erfassen Sie mindestens die folgenden Überwachungsprotokolleinträge für alle Systemkomponenten für die einzelnen Ereignisse:
- **10.3.1** Benutzererkennung
- **10.3.2** Ereignistyp
- **10.3.3** Datum und Uhrzeit
- **10.3.4** Angabe einer erfolgreichen oder fehlgeschlagenen Aktion
- **10.3.5** Ursprung des Ereignisses
- **10.3.6** Identität oder Name von betroffenen Daten, Systemkomponenten oder Ressourcen

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure hat Verfahren zum Synchronisieren von Servern und Netzwerkgeräten in der Microsoft Azure-Umgebung eingeführt, bei denen NTP Stratum 1-Zeitserver mit GPS (Global Positioning System)-Satelliten synchronisiert werden. Die Synchronisierung wird automatisch alle fünf Minuten ausgeführt. Microsoft Azure ist dafür verantwortlich, die ordnungsgemäße Zeitsynchronisierung durch Diensthosts sicherzustellen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erfasst gemäß der Anforderung 10.3 die Benutzer-ID, den Ereignistyp, Datums-/Zeitstempel, erfolgreiche oder fehlgeschlagene Ereignisse, die Quelle des Ereignisses und den Namen der Ressource.|



## <a name="pci-dss-requirement-104"></a>PCI-DSS-Anforderung 10.4

**10.4** Synchronisieren Sie mithilfe der Zeitsynchronisierung alle kritischen Systemuhren und -zeiten, und stellen Sie sicher, dass folgende Funktionen für das Abrufen, Verteilen und Speichern der Zeit implementiert sind. 
> [!NOTE]
> Network Time Protocol (NTP) ist ein Beispiel für Zeitsynchronisierungstechnologie.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure hat Verfahren zum Synchronisieren von Servern und Netzwerkgeräten in der Microsoft Azure-Umgebung eingeführt, bei denen NTP Stratum 1-Zeitserver mit GPS (Global Positioning System)-Satelliten synchronisiert werden. Die Synchronisierung wird automatisch alle fünf Minuten ausgeführt. Microsoft Azure ist dafür verantwortlich, die ordnungsgemäße Zeitsynchronisierung durch Diensthosts sicherzustellen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Zeitsynchronisierung für den PaaS-Dienst wird von Azure ausgeführt.|



### <a name="pci-dss-requirement-1041"></a>PCI-DSS-Anforderung 10.4.1

**10.4.1** Wichtige Systeme weisen die richtige und konsistente Uhrzeit auf.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 10.4](#pci-dss-requirement-10-4). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Zeitsynchronisierung für den PaaS-Dienst wird von Azure ausgeführt.|



### <a name="pci-dss-requirement-1042"></a>PCI-DSS-Anforderung 10.4.2

**10.4.2** Zeitdaten sind geschützt.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 10.4](#pci-dss-requirement-10-4). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Zeitsynchronisierung für den PaaS-Dienst wird von Azure ausgeführt.|



### <a name="pci-dss-requirement-1043"></a>PCI-DSS-Anforderung 10.4.3

**10.4.3** Uhrzeiteinstellungen werden von branchenweit anerkannten Zeitquellen empfangen.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 10.4](#pci-dss-requirement-10-4). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Zeitsynchronisierung für den PaaS-Dienst wird von Azure ausgeführt.|



## <a name="pci-dss-requirement-105"></a>PCI-DSS-Anforderung 10.5

**10.5** Sichern Sie Überwachungsprotokolle, damit sie nicht geändert werden können.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | In der Microsoft Azure-Umgebung werden FIM- und IDS-Tools implementiert. Microsoft Azure verwendet EWS, um die Echtzeitanalyse von Ereignissen innerhalb der Betriebsumgebung zu unterstützen. MAs und AIMS generieren nahezu in Echtzeit Benachrichtigungen zu Ereignissen, die möglicherweise das System gefährden könnten. <br /><br />Die Protokollierung von Dienst-, Benutzer- und Sicherheitsereignissen (Webserverprotokolle, FTP-Serverprotokolle usw.) ist aktiviert und wird zentral beibehalten. Azure beschränkt den Zugriff auf Überwachungsprotokolle auf autorisierte Mitarbeiter basierend auf dem jeweiligen Verantwortungsbereich. Ereignisprotokolle werden in der sicheren Archivinfrastruktur von Azure archiviert und 180 Tage lang beibehalten. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt die Überwachung aller Elemente für Azure bereit. Die Sicherung in einer externen Quelle kann von [Azure Backup](https://azure.microsoft.com/services/backup/) durchgeführt werden.|



### <a name="pci-dss-requirement-1051"></a>PCI-DSS-Anforderung 10.5.1

**10.5.1** Beschränken Sie die Anzeige von Überwachungsprotokollen auf Personen mit Aufgaben, die dies erfordern.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 10.5](#pci-dss-requirement-10-5). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt die Überwachung aller Elemente für Azure bereit. Die Sicherung in einer externen Quelle kann von [Azure Backup](https://azure.microsoft.com/services/backup/) durchgeführt werden.|



### <a name="pci-dss-requirement-1052"></a>PCI-DSS-Anforderung 10.5.2

**10.5.2** Schützen Sie Überwachungsprotokolldateien vor nicht autorisierten Änderungen.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 10.5](#pci-dss-requirement-10-5). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt die Überwachung aller Elemente für Azure bereit. Die Sicherung in einer externen Quelle kann von [Azure Backup](https://azure.microsoft.com/services/backup/) durchgeführt werden.|



### <a name="pci-dss-requirement-1053"></a>PCI-DSS-Anforderung 10.5.3

**10.5.3** Sichern Sie Überwachungsprotokolldateien direkt auf einem zentralen Protokollserver oder Medien, die schwer zu ändern sind.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 10.5](#pci-dss-requirement-10-5). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt die Überwachung aller Elemente für Azure bereit. Die Sicherung in einer externen Quelle kann von [Azure Backup](https://azure.microsoft.com/services/backup/) durchgeführt werden.|



### <a name="pci-dss-requirement-1054"></a>PCI-DSS-Anforderung 10.5.4

**10.5.4** Schreiben Sie Protokolle für Technologien mit externem Zugriff auf einen sicheren, zentralen, internen Protokollserver oder ein Mediengerät.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 10.5](#pci-dss-requirement-10-5). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt die Überwachung aller Elemente für Azure bereit. Die Sicherung in einer externen Quelle kann von [Azure Backup](https://azure.microsoft.com/services/backup/) durchgeführt werden.|



### <a name="pci-dss-requirement-1055"></a>PCI-DSS-Anforderung 10.5.5

**10.5.5** Verwenden Sie Software zur Datenintegritätsüberwachung oder Änderungserkennung für Protokolle, um sicherzustellen, dass bei einer Änderung an vorhandenen Protokolldaten Warnungen generiert werden (neu hinzugefügte Daten sollten jedoch keine Warnung verursachen).

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 10.5](#pci-dss-requirement-10-5). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt die Überwachung aller Elemente für Azure bereit. Die Sicherung in einer externen Quelle kann von [Azure Backup](https://azure.microsoft.com/services/backup/) durchgeführt werden.|



## <a name="pci-dss-requirement-106"></a>PCI-DSS-Anforderung 10.6

**10.6** Überprüfen Sie Protokolle und Sicherheitsereignisse für alle Systemkomponenten, um Anomalien oder verdächtige Aktivitäten zu identifizieren.
 
> [!NOTE]
> Für Protokolle können Tools zum Sammeln von Informationen, Analysieren und Benachrichtigen verwendet werden, um diese Anforderung zu erfüllen.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | In der Microsoft Azure-Umgebung werden FIM- und IDS-Tools implementiert. Microsoft Azure verwendet EWS, um die Echtzeitanalyse von Ereignissen innerhalb der Betriebsumgebung zu unterstützen. MAs und AIMS generieren nahezu in Echtzeit Benachrichtigungen zu Ereignissen, die möglicherweise das System gefährden könnten. <br /><br />Die Protokollierung von Dienst-, Benutzer- und Sicherheitsereignissen (Webserverprotokolle, FTP-Serverprotokolle usw.) ist aktiviert und wird zentral beibehalten. Azure beschränkt den Zugriff auf Überwachungsprotokolle auf autorisierte Mitarbeiter basierend auf dem jeweiligen Verantwortungsbereich. Ereignisprotokolle werden in der sicheren Archivinfrastruktur von Azure archiviert und 180 Tage lang beibehalten. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet [Azure Security Center](https://azure.microsoft.com/services/security-center/) zum Überwachen, Melden und Verhindern von Anomalien. [Azure Advisor](/azure/advisor/advisor-security-recommendations) bietet eine einheitliche, konsolidierte Ansicht der Empfehlungen für alle Ihre Azure-Ressourcen.|



### <a name="pci-dss-requirement-1061"></a>PCI-DSS-Anforderung 10.6.1

**10.6.1** Überprüfen Sie Folgendes mindestens täglich:
- Alle Sicherheitsereignisse
- Protokolle aller Systemkomponenten, die CHD und/oder SAD speichern, verarbeiten oder übertragen
- Protokolle aller kritischen Systemkomponenten
- Protokolle aller Server und Systemkomponenten, die Sicherheitsfunktionen (z. B. Firewalls, Angrifferkennungssysteme /Angriffschutzsysteme, Authentifizierungsserver, E-Commerce-Umleitungsserver usw.) ausführen

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 10.6](#pci-dss-requirement-10-6). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet [Azure Security Center](https://azure.microsoft.com/services/security-center/) zum Überwachen, Melden und Verhindern von Anomalien. [Azure Advisor](/azure/advisor/advisor-security-recommendations) bietet eine einheitliche, konsolidierte Ansicht der Empfehlungen für alle Ihre Azure-Ressourcen.|



### <a name="pci-dss-requirement-1062"></a>PCI-DSS-Anforderung 10.6.2

**10.6.2** Überprüfen Sie die Protokolle aller anderen Systemkomponenten regelmäßig basierend auf den Richtlinien und der Risikomanagementstrategie der Organisation gemäß der jährlichen Risikobewertung der Organisation.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 10.6](#pci-dss-requirement-10-6). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet [Azure Security Center](https://azure.microsoft.com/services/security-center/) zum Überwachen, Melden und Verhindern von Anomalien. [Azure Advisor](/azure/advisor/advisor-security-recommendations) bietet eine einheitliche, konsolidierte Ansicht der Empfehlungen für alle Ihre Azure-Ressourcen.|



### <a name="pci-dss-requirement-1063"></a>PCI-DSS-Anforderung 10.6.3

**10.6.3** Verfolgen Sie während des Überprüfungsvorgangs erkannte Ausnahmen und Anomalien nach.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 10.6](#pci-dss-requirement-10-6). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet [Azure Security Center](https://azure.microsoft.com/services/security-center/) zum Überwachen, Melden und Verhindern von Anomalien. [Azure Advisor](/azure/advisor/advisor-security-recommendations) bietet eine einheitliche, konsolidierte Ansicht der Empfehlungen für alle Ihre Azure-Ressourcen.|



## <a name="pci-dss-requirement-107"></a>PCI-DSS-Anforderung 10.7

**10.7** Bewahren Sie den Überwachungsprotokollverlauf mindestens ein Jahr lang auf, wobei die Daten der letzten drei Monate sofort für die Analyse verfügbar sind (z. B. online, archiviert oder aus einer Sicherung wiederherstellbar).

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure bewahrt Überwachungsprotokolle ein Jahr lang auf, wobei die Daten der letzten drei Monate über das interne Portal sofort verfügbar sind. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verfügt über eine umfangreiche Protokollierung aller System- und Benutzeraktivitäten (einschließlich CHD-Protokollierung). Weitere Informationen finden Sie in den [PCI-Anleitungen zum Protokollieren](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-108"></a>PCI-DSS-Anforderung 10.8

**10.8** **Zusätzliche Anforderung nur für Dienstanbieter:** Implementieren Sie einen Prozess für die rechtzeitige Erkennung und Meldung von Fehlern bei sicherheitsrelevanten Verwaltungssystemen, einschließlich, aber nicht beschränkt Fehler bei:
- Firewalls
- IDS/IPS
- FIM
- Virenschutz
- Physische Zugriffssteuerung
- Logische Zugriffssteuerung
- Überwachungsprotokollmechanismen
- Segmentierungsteuerelemente (sofern verwendet) 

> [!NOTE]
> Diese Anforderung ist bis zum 31. Januar 2018 eine bewährte Methode, danach stellt sie eine Anforderung dar.



**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure verwendet EWS, um die Echtzeitanalyse von Ereignissen innerhalb der Betriebsumgebung zu unterstützen. MAs und AIMS generieren nahezu in Echtzeit Benachrichtigungen zu Ereignissen, die möglicherweise das System gefährden könnten. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verfügt über eine umfangreiche Protokollierung aller System- und Benutzeraktivitäten (einschließlich CHD-Protokollierung). Weitere Informationen finden Sie in den [PCI-Anleitungen zum Protokollieren](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-1081"></a>PCI-DSS-Anforderung 10.8.1

**10.8.1** **Zusätzliche Anforderung nur für Dienstanbieter:** Reagieren Sie rechtzeitig auf Fehler bei wichtigen Sicherheitskontrollen. Prozesse für das Reagieren auf Fehler in Sicherheitskontrollen müssen Folgendes umfassen:
- Wiederherstellen von Sicherheitsfunktionen
- Identifizieren und Dokumentieren der Dauer (Datum und Uhrzeit von Beginn bis Ende) des Sicherheitsfehlers
- Identifizieren und Dokumentieren von Fehlerursachen, einschließlich der Grundursache, und Dokumentieren der erforderlichen Problembehandlung für die Grundursache
- Identifizieren und Behandeln von Sicherheitsrisiken, die während des Fehlers entstanden sind
- Ausführen einer Risikoanalyse, um zu bestimmen, ob als Ergebnis des Sicherheitsfehlers weitere Aktionen erforderlich sind
- Implementieren von Kontrollen zum Verhindern, dass der Fehler erneut auftritt – Wiederaufnahme der Überwachung von Sicherheitskontrollen 

> [!NOTE]
> Diese Anforderung ist bis zum 31. Januar 2018 eine bewährte Methode, danach stellt sie eine Anforderung dar.


**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure verwendet EWS, um die Echtzeitanalyse von Ereignissen innerhalb der Betriebsumgebung zu unterstützen. MAs und AIMS generieren nahezu in Echtzeit Benachrichtigungen zu Ereignissen, die möglicherweise das System gefährden könnten. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verfügt über eine umfangreiche Protokollierung aller System- und Benutzeraktivitäten (einschließlich CHD-Protokollierung). Weitere Informationen finden Sie in den [PCI-Anleitungen zum Protokollieren](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-109"></a>PCI-DSS-Anforderung 10.9

**10.9** Stellen Sie sicher, dass Sicherheitsrichtlinien und betriebliche Verfahren für die Überwachung des gesamten Zugriffs auf Netzwerkressourcen und Daten von Karteninhabern dokumentiert und verwendet werden sowie allen beteiligten Parteien bekannt sind.


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt einen Anwendungsfall und eine Beschreibung für die Verwaltung und den Schutz der CHD bereit.|




