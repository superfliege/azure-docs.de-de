---
title: Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP – Identifizierung und Authentifizierung
description: Automatisierung von Webanwendungen für FedRAMP – Identifizierung und Authentifizierung
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 1033f63f-daf0-4174-a7f6-7b0f569020e2
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 21b5c453716f99be26c8dd6400bb3489477b4956
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33206548"
---
# <a name="identification-and-authentication-ia"></a>Identifikation und Authentifizierung (IA)

## <a name="nist-800-53-control-ia-1"></a>NIST 800-53, Regelung IA-1

> [!NOTE]
> Diese Regelungen werden von NIST und dem U.S. Department of Commerce als Teil der NIST-Sonderveröffentlichung 800-53 Revision 4 definiert. Informationen zu Testverfahren und Anleitungen für jede Regelung finden Sie unter NIST 800-53 Rev. 4.

#### <a name="identification-and-authentication-policy-and-procedures"></a>Identifikation und Authentifizierung – Richtlinie und Verfahren

**IA-1** Die Organisation entwickelt, dokumentiert und verteilt an [Zuordnung: von der Organisation definierte Personen oder Rollen] eine Identifikations- und Authentifizierungsrichtlinie, die sich mit Zweck, Umfang, Rollen, Zuständigkeiten, Managementverpflichtung, Koordination zwischen Organisationsentitäten und Konformität befasst, stellt Verfahren zum Ermöglichen der Implementierung der Identifikations- und Authentifizierungsrichtlinie und der damit verbundenen Identifikations- und Authentifizierungsregelungen bereit und überprüft und aktualisiert die aktuelle Identifikations- und Authentifizierungsrichtlinie [Zuordnung: von der Organisation definierte Häufigkeit] sowie die Identifikations- und Authentifizierungsverfahren [Zuordnung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die kundenseitigen Unternehmensrichtlinien und -verfahren für Identifikation und Authentifizierung können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-2"></a>NIST 800-53, Regelung IA-2

#### <a name="identification-and-authentication-organizational-users"></a>Identifikation und Authentifizierung (Organisationsbenutzer)

**IA-2** Das Informationssystem sorgt für eine eindeutige Identifikation und Authentifizierung der Organisationsbenutzer (oder Prozesse, die im Namen der Organisationsbenutzer agieren).

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Durch diesen Blueprint erstellte Konten verfügen über eindeutige Bezeichner. Integrierte Konten ohne eindeutige Bezeichner werden deaktiviert oder entfernt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-2-1"></a>NIST 800-53, Regelung IA-2 (1)

#### <a name="identification-and-authentication-organizational-users--network-access-to-privileged-accounts"></a>Identifikation und Authentifizierung (Organisationsbenutzer) | Netzwerkzugriff auf privilegierte Konten

**IA-2 (1)** Das Informationssystem implementiert eine mehrstufige Authentifizierung für den Netzwerkzugriff auf privilegierte Konten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Implementierung einer mehrstufigen Authentifizierung für den Netzwerkzugriff auf privilegierte Konten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-2-2"></a>NIST 800-53, Regelung IA-2 (2)

#### <a name="identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts"></a>Identifikation und Authentifizierung (Organisationsbenutzer) | Netzwerkzugriff auf nicht privilegierte Konten

**IA-2 (2)** Das Informationssystem implementiert eine mehrstufige Authentifizierung für den Netzwerkzugriff auf nicht privilegierte Konten.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Implementierung einer mehrstufigen Authentifizierung für den Netzwerkzugriff auf nicht privilegierte Konten. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-2-3"></a>NIST 800-53, Regelung IA-2 (3)

#### <a name="identification-and-authentication-organizational-users--local-access-to-privileged-accounts"></a>Identifikation und Authentifizierung (Organisationsbenutzer) | Lokaler Zugriff auf privilegierte Konten

**IA-2 (3)** Das Informationssystem implementiert eine mehrstufige Authentifizierung für den lokalen Zugriff auf privilegierte Konten.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen lokalen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure lässt lokalen Zugriff nur zu, wenn ein physischer Zugriff erforderlich ist. Der lokale Administratorzugriff darf nur zur Problembehandlung verwendet werden, wenn auf dem Mitgliedsserver Netzwerkprobleme auftreten und die Domänenauthentifizierung nicht funktioniert. <br /> Azure implementiert eine mehrstufige Authentifizierung für den lokalen Zugriff über Zugriffssteuerungsmechanismen, die für den physischen Zugriff auf die Umgebung erforderlich sind. Räume in Azure-Rechenzentren, die alle Azure-Infrastruktursysteme innerhalb der Systemgrenzen enthalten, sind durch verschiedene physische Sicherheitsmechanismen eingeschränkt, einschließlich der Anforderung des Zugriffs über Unternehmenssmartcards und biometrische Geräte. Beide Formen der Authentifizierung sind für den physischen Zugriff am Zugangspunkt zu Azure-Rechenzentren erforderlich. |


 ### <a name="nist-800-53-control-ia-2-4"></a>NIST 800-53, Regelung IA-2 (4)

#### <a name="identification-and-authentication-organizational-users--local-access-to-non-privileged-accounts"></a>Identifikation und Authentifizierung (Organisationsbenutzer) | Lokaler Zugriff auf nicht privilegierte Konten

**IA-2 (4)** Das Informationssystem implementiert eine mehrstufige Authentifizierung für den lokalen Zugriff auf nicht privilegierte Konten.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Kunden haben keinen lokalen Zugriff auf Systemressourcen in Azure-Rechenzentren. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure stuft alle Microsoft Azure Government-Konten, die von Microsoft Azure Government-Personal genutzt werden, als privilegierte Konten ein. Die mehrstufige Authentifizierung wird für alle Microsoft Azure Government-Personalkonten implementiert, die Smartcards und PINs verwenden, was einen lokalen Zugriff einschließt. |


 ### <a name="nist-800-53-control-ia-2-5"></a>NIST 800-53, Regelung IA-2 (5)

#### <a name="identification-and-authentication-organizational-users--group-authentication"></a>Identifikation und Authentifizierung (Organisationsbenutzer)| Gruppenauthentifizierung

**IA-2 (5)** Die Organisation fordert, dass die Authentifizierung von Einzelpersonen über einen individuellen Authentifikator erfolgt, wenn ein Gruppenauthentifikator verwendet wird.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Für Ressourcen, die von diesem Blueprint bereitgestellt werden, werden keine gemeinsam genutzten Konten oder Gruppenkonten aktiviert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-2-8"></a>NIST 800-53, Regelung IA-2 (8)

#### <a name="identification-and-authentication-organizational-users--network-access-to-privileged-accounts---replay-resistant"></a>Identifikation und Authentifizierung (Organisationsbenutzer) | Netzwerkzugriff auf privilegierte Konten – Schutz vor Replay-Angriffen

**IA-2 (8)** Das Informationssystem implementiert einen Authentifizierungsmechanismus für den Netzwerkzugriff auf privilegierte Konten, der Schutz vor Replay-Angriffen bietet.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Zugriff auf die durch diesen Blueprint bereitgestellten Ressourcen ist durch die integrierte Kerberos-Funktion von Azure Active Directory, Active Directory und dem Windows-Betriebssystem vor Replay-Angriffen geschützt. Bei der Kerberos-Authentifizierung enthält der vom Client gesendete Authentifikator zusätzliche Daten, z.B. eine verschlüsselte IP-Adressenliste, die Clientzeitstempel und die Ticketlebensdauer. Wenn ein Paket erneut verwendet wird, erfolgt eine Überprüfung des Zeitstempels. Ist der Zeitstempel älter als oder genauso alt wie der vorherige Authentifikator, wird das Paket zurückgewiesen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-2-9"></a>NIST 800-53, Regelung IA-2 (9)

#### <a name="identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts---replay-resistant"></a>Identifikation und Authentifizierung (Organisationsbenutzer) | Netzwerkzugriff auf nicht privilegierte Konten – Schutz vor Replay-Angriffen

**IA-2 (9)** Das Informationssystem implementiert einen Authentifizierungsmechanismus für den Netzwerkzugriff auf nicht privilegierte Konten, der Schutz vor Replay-Angriffen bietet.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Zugriff auf die durch diesen Blueprint bereitgestellten Ressourcen ist durch die integrierte Kerberos-Funktion von Azure Active Directory, Active Directory und dem Windows-Betriebssystem vor Replay-Angriffen geschützt. Bei der Kerberos-Authentifizierung enthält der vom Client gesendete Authentifikator zusätzliche Daten, z.B. eine verschlüsselte IP-Adressenliste, die Clientzeitstempel und die Ticketlebensdauer. Wenn ein Paket erneut verwendet wird, erfolgt eine Überprüfung des Zeitstempels. Ist der Zeitstempel älter als oder genauso alt wie der vorherige Authentifikator, wird das Paket zurückgewiesen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-2-11"></a>NIST 800-53, Regelung IA-2 (11)

#### <a name="identification-and-authentication-organizational-users--remote-access----separate-device"></a>Identifikation und Authentifizierung (Organisationsbenutzer)| Remotezugriff – separates Gerät

**IA-2 (11)** Das Informationssystem implementiert eine mehrstufige Authentifizierung für den Remotezugriff auf privilegierte und nicht privilegierte Konten, sodass einer der Faktoren über ein Gerät bereitgestellt wird, das von dem System getrennt ist, auf das zugegriffen werden soll, und das [Zuordnung: von der Organisation definierte Anforderungen zur Stärke des Mechanismus] erfüllt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Implementierung einer mehrstufigen Authentifizierung für den Remotezugriff auf kundenseitig bereitgestellte Ressourcen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-2-12"></a>NIST 800-53, Regelung IA-2 (12)

#### <a name="identification-and-authentication-organizational-users--acceptance-of-piv-credentials"></a>Identifikation und Authentifizierung (Organisationsbenutzer)| Akzeptieren von PIV-Anmeldeinformationen

**IA-2 (12)** Das Informationssystem akzeptiert PIV-Anmeldeinformationen (Personal Identity Verification) und führt eine elektronische Überprüfung der Anmeldeinformationen durch.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für das Akzeptieren und Überprüfen von PIV-Anmeldeinformationen (Personal Identity Verification) verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-3"></a>NIST 800-53, Regelung IA-3

#### <a name="device-identification-and-authentication"></a>Identifikation und Authentifizierung von Geräten

**IA-3** Das Informationssystem führt eine eindeutige Identifikation und Authentifizierung für [Festlegung: von der Organisation definierten Geräte oder Gerätetypen] durch, bevor eine [Auswahl (eine oder mehrere Optionen): lokale, Remote-, Netzwerk-] Verbindung hergestellt wird.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für das Implementieren der Geräteidentifikation und -authentifizierung verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-4a"></a>NIST 800-53, Regelung IA-4.a

#### <a name="identifier-management"></a>Bezeichnerverwaltung

**IA-4.a** Die Organisation verwaltet Informationssystembezeichner durch die Autorisierung von [Zuordnung: von der Organisation definierte Personen oder Rollen] zur Zuweisung eines Personen-, Gruppen-, Rollen- oder Gerätebezeichners.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für das Verwalten von Bezeichnern (z.B. Einzelpersonen, Gruppen, Rollen und Geräte) für Kundenressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-4b"></a>NIST 800-53, Regelung IA-4.b

#### <a name="identifier-management"></a>Bezeichnerverwaltung

**IA-4.b** Die Organisation verwaltet Informationssystembezeichner durch Auswahl eines Bezeichners zur Identifikation einer Einzelperson, Gruppe, Rolle oder eines Geräts.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint fordert während der Bereitstellung zur Eingabe kundenspezifischer Bezeichner für individuelle Konten auf.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-4c"></a>NIST 800-53, Regulierung IA-4.c

#### <a name="identifier-management"></a>Bezeichnerverwaltung

**IA-4.b** Die Organisation verwaltet Informationssystembezeichner durch das Zuweisen des Bezeichners zu einer vorgesehenen Einzelperson, Gruppe, Rolle oder einem Gerät.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für das Verwalten von Bezeichnern (z.B. Einzelpersonen, Gruppen, Rollen und Geräte) für Kundenressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-4d"></a>NIST 800-53, Regelung IA-4.d

#### <a name="identifier-management"></a>Bezeichnerverwaltung

**IA-4.d** Die Organisation verwaltet Informationssystembezeichner, indem die Wiederverwendung von Bezeichnern für [Zuordnung: von der Organisation definierter Zeitraum] verhindert wird.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Active Directory- und lokalen Windows-Betriebssystemkonten wird eine eindeutige Sicherheits-ID (SID) zugewiesen. Azure Active Directory-Konten wird eine global eindeutige Objekt-ID zugewiesen. Diese eindeutigen IDs können nicht wiederverwendet werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-4e"></a>NIST 800-53, Regelung IA-4.e

#### <a name="identifier-management"></a>Bezeichnerverwaltung

**IA-4.e** Die Organisation verwaltet Informationssystembezeichner, indem der Bezeichner nach [Zuordnung: von der Organisation definierter Zeitraum] deaktiviert wird.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint implementiert eine geplante Aufgabe für Active Directory, sodass Konten nach 35 Tagen Inaktivität automatisch deaktiviert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-4-4"></a>NIST 800-53, Regelung IA-4 (4)

#### <a name="identifier-management--identify-user-status"></a>Bezeichnerverwaltung | Identifizieren des Benutzerstatus

**IA-4 (4)** Die Organisation verwaltet einzelne Bezeichner durch das eindeutige Identifizieren jeder Einzelperson als [Zuordnung: von der Organisation definiertes Merkmal zum Identifizieren des Benutzerstatus].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Azure Active Directory und Active Directory unterstützen die Kennzeichnung von Auftragnehmern, Lieferanten und anderen Benutzertypen mithilfe der Anwendung von Namenskonventionen auf die Bezeichner. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-5a"></a>NIST 800-53, Regelung IA-5.a

#### <a name="authenticator-management"></a>Authentifikatorverwaltung

**IA-5.a** Die Organisation verwaltet Authentifikatoren des Informationssystems, indem sie im Rahmen der ersten Verteilung von Authentifikatoren die Identität der Person, Gruppe, Rolle oder des Geräts überprüft, die den Authentifikator erhält.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für das Verwalten der Authentifikatoren verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-5b"></a>NIST 800-53, Regelung IA-5.b

#### <a name="authenticator-management"></a>Authentifikatorverwaltung

**IA-5.b** Die Organisation verwaltet Authentifikatoren des Informationssystems, indem sie den anfänglichen Authentifikatorinhalt für die von der Organisation definierten Authentifikatoren festlegt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der gesamte anfängliche Authentifikatorinhalt für Konten, die durch diesen Blueprint erstellt werden, erfüllt die in IA-5 (1) dargelegten Anforderungen, die bei Angabe durch den Kunden während der Bereitstellung überprüft werden.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-5c"></a>NIST 800-53, Regelung IA-5.c

#### <a name="authenticator-management"></a>Authentifikatorverwaltung

**IA-5.c** Die Organisation verwaltet Systeminformationsauthentifikatoren, indem sie sicherstellt, dass die Authentifikatoren über eine ausreichende Stärke des Mechanismus für die beabsichtigte Verwendung verfügen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Von diesem Blueprint verwendete Authentifikatoren erfüllen die FedRAMP-spezifischen Sicherheitsanforderungen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-5d"></a>NIST 800-53, Regelung IA-5.d

#### <a name="authenticator-management"></a>Authentifikatorverwaltung

**IA-5.d** Die Organisation verwaltet Authentifikatoren des Informationssystems, indem sie administrative Verfahren für die Erstverteilung von Authentifikatoren, für verlorene, kompromittierte oder beschädigte Authentifikatoren und für das Sperren von Authentifikatoren einführt und implementiert.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für das Verwalten der Authentifikatoren verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-5e"></a>NIST 800-53, Regelung IA-5.e

#### <a name="authenticator-management"></a>Authentifikatorverwaltung

**IA-5.e** Die Organisation verwaltet Authentifikatoren des Informationssystems, indem sie den Standardinhalt der Authentifikatoren vor der Installation des Informationssystems ändert.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Bei allen Authentifikatoren für Komponenten dieses Blueprints wurden die Standardeinstellungen geändert. Authentifikatoren werden während der Bereitstellung dieser Lösung kundenseitig angegeben. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-5f"></a>NIST 800-53, Regelung IA-5.f

#### <a name="authenticator-management"></a>Authentifikatorverwaltung

**IA-5.f** Die Organisation verwaltet Authentifikatoren des Informationssystems, indem sie Mindest- und Höchstwerte für die Lebensdauer und Wiederverwendungsbedingungen für Authentifikatoren festlegt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für das Verwalten der Authentifikatoren verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-5g"></a>NIST 800-53, Regelung IA-5.g

#### <a name="authenticator-management"></a>Authentifikatorverwaltung

**IA-5.g** Die Organisation verwaltet Authentifikatoren des Informationssystems, indem sie die Authentifikatoren ändert oder aktualisiert [Zuordnung: von der Organisation definierter Zeitraum nach Authentifikatortyp].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt einen Domänencontroller bereit, mit dem alle bereitgestellten virtuellen Computer verknüpft sind. Es wird eine Gruppenrichtlinie erstellt und konfiguriert, um Beschränkungen der Kennwortgültigkeitsdauer (60 Tage) zu implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-5h"></a>NIST 800-53, Regelung IA-5.h

#### <a name="authenticator-management"></a>Authentifikatorverwaltung

**IA-5.h** Die Organisation verwaltet Authentifikatoren des Informationssystems, indem sie den Inhalt der Authentifikatoren vor unbefugter Offenlegung und Änderung schützt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint implementiert Key Vault, um Authentifikatorinhalte vor unbefugter Offenlegung und Änderung zu schützen. In Key Vault werden folgende Authentifikatoren gespeichert: das Azure-Kennwort für das bereitgestellte Konto, das Administratorkennwort für den virtuellen Computer und das Kennwort für das SQL Server-Dienstkonto. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-5i"></a>NIST 800-53, Regelung IA-5.i

#### <a name="authenticator-management"></a>Authentifikatorverwaltung

**IA-5.i** Die Organisation verwaltet Authentifikatoren des Informationssystems, indem sie von Einzelpersonen verlangt, dass bestimmte Sicherheitsvorkehrungen zum Schutz von Authentifikatoren ergriffen und auf Geräten implementiert werden müssen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint implementiert Key Vault, um Authentifikatorinhalte vor unbefugter Offenlegung und Änderung zu schützen. In Key Vault werden folgende Authentifikatoren gespeichert: das Azure-Kennwort für das bereitgestellte Konto, das Administratorkennwort für den virtuellen Computer und das Kennwort für das SQL Server-Dienstkonto. Key Vault verschlüsselt Schlüssel und Geheimnisse (beispielsweise Authentifizierungsschlüssel, Schlüssel für Speicherkonten, Datenverschlüsselungsschlüssel und Kennwörter) durch das Verwenden von Schlüsseln, die durch Hardwaresicherheitsmodule (HSMs) geschützt werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-5j"></a>NIST 800-53, Regelung IA-5.j

#### <a name="authenticator-management"></a>Authentifikatorverwaltung

**IA-5.j** Die Organisation verwaltet Authentifikatoren des Informationssystems, indem sie die Authentifikatoren für Gruppen-/Rollenkonten ändert, wenn sich die Mitgliedschaft für diese Konten ändert.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Für Ressourcen, die von diesem Blueprint bereitgestellt werden, werden keine gemeinsam genutzten Konten oder Gruppenkonten aktiviert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-1a"></a>NIST 800-53, Regelung IA-5 (1).a

#### <a name="authenticator-management--password-based-authentication"></a>Authentifikatorverwaltung |Kennwortbasierte Authentifizierung

**IA-5 (1).a** Das Informationssystem für die kennwortbasierte Authentifizierung erzwingt die minimale Kennwortkomplexität von [Zuordnung: von der Organisation definierte Anforderungen an Groß-/Kleinschreibung, Anzahl von Zeichen, Kombination aus Groß- und Kleinbuchstaben, Ziffern und Sonderzeichen, einschließlich Mindestanforderungen für jeden Typ].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt einen Domänencontroller bereit, mit dem alle bereitgestellten virtuellen Computer verknüpft sind. Es wird eine Gruppenrichtlinie erstellt und konfiguriert, um die Anforderungen an die Kennwortkomplexität für lokale VM-Konten und AD-Konten durchzusetzen.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-1b"></a>NIST 800-53, Regelung IA-5 (1).b

#### <a name="authenticator-management--password-based-authentication"></a>Authentifikatorverwaltung |Kennwortbasierte Authentifizierung

**IA-5 (1).b** Das Informationssystem für die kennwortbasierte Authentifizierung erzwingt beim Erstellen neuer Kennwörter mindestens die folgende Anzahl geänderter Zeichen: [Zuordnung: von der Organisation definierte Anzahl].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verwendung einer kennwortbasierten Authentifizierung innerhalb der vom Kunden bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-1c"></a>NIST 800-53, Regelung IA-5 (1).c

#### <a name="authenticator-management--password-based-authentication"></a>Authentifikatorverwaltung |Kennwortbasierte Authentifizierung

**IA-5 (1).c** Das Informationssystem für die kennwortbasierte Authentifizierung speichert und überträgt ausschließlich kryptographisch geschützte Kennwörter.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Azure Active Directory stellt sicher, dass alle Kennwörter während der Speicherung und Übertragung kryptografisch geschützt werden. Kennwörter, die von Active Directory und lokal auf bereitgestellten Windows-VMs gespeichert werden, werden im Rahmen der integrierten Sicherheitsfunktionalität automatisch mit einem Hash versehen. Remotedesktop-Authentifizierungssitzungen werden mit TLS gesichert, um sicherzustellen, dass Authentifikatoren bei der Übertragung geschützt sind. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-1d"></a>NIST 800-53, Regelung IA-5 (1).d

#### <a name="authenticator-management--password-based-authentication"></a>Authentifikatorverwaltung |Kennwortbasierte Authentifizierung

**IA-5 (1).d** Das Informationssystem für die kennwortbasierte Authentifizierung erzwingt Mindest- und Höchstwerte zur Einschränkung der Kennwortgültigkeitsdauer von [Zuordnung: von der Organisation definierte Mindest- und Höchstwerte für die Gültigkeitsdauer].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt einen Domänencontroller bereit, mit dem alle bereitgestellten virtuellen Computer verknüpft sind. Es wird eine Gruppenrichtlinie erstellt und konfiguriert, um Beschränkungen für Kennwörter durchzusetzen, die den Mindestwert (1 Tag) und den Höchstwert (60 Tage) für die Gültigkeitsdauer von lokalen Konten und AD-Konten erzwingen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-1e"></a>NIST 800-53, Regelung IA-5 (1).e

#### <a name="authenticator-management--password-based-authentication"></a>Authentifikatorverwaltung |Kennwortbasierte Authentifizierung

**IA-5 (1).e** Das Informationssystem für die kennwortbasierte Authentifizierung verbietet die Wiederverwendung von Kennwörtern für [Zuordnung: von der Organisation definierte Anzahl] Generationen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt einen Domänencontroller bereit, mit dem alle bereitgestellten virtuellen Computer verknüpft sind. Es wird eine Gruppenrichtlinie eingerichtet und konfiguriert, um Einschränkungen zur Wiederverwendung (24 Kennwörter) für lokale Konten und AD-Konten zu erzwingen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-1f"></a>NIST 800-53, Regelung IA-5 (1).f

#### <a name="authenticator-management--password-based-authentication"></a>Authentifikatorverwaltung |Kennwortbasierte Authentifizierung

**IA-5 (1).f** Das Informationssystem für die kennwortbasierte Authentifizierung ermöglicht die Verwendung eines temporären Kennworts für Systemanmeldungen mit sofortiger Änderung in ein dauerhaftes Kennwort.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Azure Active Directory dient zum Verwalten der Zugriffskontrolle auf das Informationssystem. Wenn ein Konto neu erstellt oder ein temporäres Kennwort generiert wird, erzwingt Azure Active Directory, dass der Benutzer sein Kennwort bei der nächsten Anmeldung ändert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-2a"></a>NIST 800-53, Regelung IA-5 (2).a

#### <a name="authenticator-management--pki-based-authentication"></a>Authentifikatorverwaltung |Kennwortbasierte Authentifizierung

**IA-5 (2).a** Das Informationssystem für die PKI-basierte Authentifizierung verifiziert Zertifizierungen, indem ein Zertifizierungspfad zu einem akzeptierten Vertrauensanker erstellt und überprüft wird, einschließlich einer Überprüfung der Informationen zum Zertifikatstatus.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verwendung der PKI-basierten Authentifizierung innerhalb der vom Kunden bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-2b"></a>NIST 800-53, Regelung IA-5 (2).b

#### <a name="authenticator-management--pki-based-authentication"></a>Authentifikatorverwaltung |Kennwortbasierte Authentifizierung

**IA-5 (2).b** Das Informationssystem für die PKI-basierte Authentifizierung erzwingt den autorisierten Zugriff auf den zugehörigen privaten Schlüssel.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verwendung der PKI-basierten Authentifizierung innerhalb der vom Kunden bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-2c"></a>NIST 800-53, Regelung IA-5 (2).c

#### <a name="authenticator-management--pki-based-authentication"></a>Authentifikatorverwaltung |Kennwortbasierte Authentifizierung

**IA-5 (2).c** Das Informationssystem für die PKI-basierte Authentifizierung ordnet die authentifizierte Identität dem Konto der Einzelperson oder Gruppe zu.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verwendung der PKI-basierten Authentifizierung innerhalb der vom Kunden bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-2d"></a>NIST 800-53, Regelung IA-5 (2).d

#### <a name="authenticator-management--pki-based-authentication"></a>Authentifikatorverwaltung |Kennwortbasierte Authentifizierung

**IA-5 (2).d** Das Informationssystem für die PKI-basierte Authentifizierung implementiert einen lokalen Cache mit Sperrdaten, um die Pfaderkennung und -überprüfung zu unterstützen, falls ein Zugriff auf die Sperrinformationen über das Netzwerk nicht möglich ist.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Verwendung der PKI-basierten Authentifizierung innerhalb der vom Kunden bereitgestellten Ressourcen verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-3"></a>NIST 800-53, Regelung IA-5 (3)

#### <a name="authenticator-management--in-person-or-trusted-third-party-registration"></a>Authentifikatorverwaltung | Persönliche Registrierung oder Registrierung über vertrauenswürdige Drittanbieter

**IA-5 (3)** Die Organisation fordert, dass der Registrierungsvorgang zum Erhalt von [Zuordnung: von der Organisation definierte Authentifikatortypen und/oder spezifische Authentifikatoren] [Auswahl: persönlich; durch einen vertrauenswürdigen Drittanbieter] bei [Zuordnung: von der Organisation definierte Registrierungsstelle] mit Autorisierung durch [Zuordnung: von der Organisation definierte Personen oder Rollen] durchgeführt werden muss.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für das Registrieren der Authentifikatoren verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-4"></a>NIST 800-53, Regelung IA-5 (4)

#### <a name="authenticator-management--automated-support--for-password-strength-determination"></a>Authentifikatorverwaltung | Automatisierte Unterstützung zum Bestimmen der Kennwortsicherheit

**IA-5 (4)** Die Organisation setzt automatisierte Tools ein, um festzustellen, ob die Kennwortauthentifikatoren sicher genug sind, um die [Zuordnung: von der Organisation definierte Anforderungen] zu erfüllen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Zu den mit diesem Blueprint bereitgestellten Benutzerkonten gehören AD-Konten und lokale Benutzerkonten. Beide bieten Mechanismen, um die Einhaltung festgelegter Kennwortanforderungen zum Erstellen eines anfänglichen Kennworts und bei Kennwortänderungen zu erzwingen. Azure Active Directory dient als automatisiertes Tool zur Ermittlung, ob Kennwortauthentifikatoren sicher genug sind, um die in IA-5 (1) dargelegten Anforderungen in Bezug auf Kennwortlänge, Komplexität, Rotation und Gültigkeitsdauer zu erfüllen. Azure Active Directory stellt sicher, dass die Kennwortauthentifikatorstärke bei der Erstellung diese Standards erfüllt. Kundenseitig angegebene Kennwörter zur Bereitstellung dieser Lösung werden überprüft, damit sie die Anforderungen an die Kennwortsicherheit erfüllen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-6"></a>NIST 800-53, Regelung IA-5 (6)

#### <a name="authenticator-management--protection-of-authenticators"></a>Authentifikatorverwaltung | Schutz von Authentifikatoren

**IA-5 (6)** Die Organisation schützt die Authentifikatoren entsprechend der Sicherheitskategorie der Informationen, auf die der Authentifikator Zugriff erlaubt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Schutz der Authentifikatoren verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-7"></a>NIST 800-53, Regelung IA-5 (7)

#### <a name="authenticator-management--no-embedded-unencrypted-static-authenticators"></a>Authentifikatorverwaltung | Keine Einbettung unverschlüsselter statischer Authentifikatoren

**IA-5 (7)** Die Organisation stellt sicher, dass unverschlüsselte statische Authentifikatoren nicht in Anwendungen oder Zugriffsskripts eingebettet oder über Funktionstasten gespeichert werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Es werden keine unverschlüsselten statischen Authentifikatoren in Anwendungen, Zugriffsskripts oder Funktionsschlüssel eingebettet, die durch diesen Blueprint bereitgestellt werden. Skripts oder Anwendungen, die einen Authentifikator verwendet, rufen vor jeder Verwendung einen Azure Key Vault-Container auf. Der Zugriff auf Azure Key Vault-Container wird überwacht. Damit können Verstöße gegen dieses Verbot erkannt werden, wenn für den Zugriff auf ein System ein Dienstkonto ohne einen zugehörigen Aufruf an den Azure Key Vault-Container verwendet wird. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-8"></a>NIST 800-53, Regelung IA-5 (8)

#### <a name="authenticator-management--multiple-information-system-accounts"></a>Authentifikatorverwaltung | Konten auf mehreren Informationssystemen

**IA-5 (8)** Die Organisation implementiert [Zuordnung: von der Organisation definierte Sicherheitsmaßnahmen], um dem Risiko einer Kompromittierung durch Einzelpersonen entgegenzuwirken, die über Konten auf mehreren Informationssystemen verfügen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde kann Sicherheitsmaßnahmen auf Unternehmensebene einsetzen, um Risiken durch Einzelpersonen entgegenzuwirken, die über Konten auf mehreren Informationssystemen verfügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-11"></a>NIST 800-53, Regelung IA-5 (11)

#### <a name="authenticator-management--hardware-token-based-authentication"></a>Authentifikatorverwaltung | Auf Hardwaretoken basierende Authentifizierung

**IA-5 (11)** Das Informationssystem für die auf Hardwaretoken basierende Authentifizierung nutzt Mechanismen, die den [Zuordnung: von der Organisation definierte Qualitätsanforderungen für Token] entsprechen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, Mechanismen einzusetzen, mit denen die Qualitätsanforderungen an die auf Hardwaretoken basierende Authentifizierung erfüllt werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-5-13"></a>NIST 800-53, Regelung IA-5 (13)

#### <a name="authenticator-management--expiration-of-cached-authenticators"></a>Authentifikatorverwaltung | Ablauf zwischengespeicherter Authentifikatoren

**IA-5 (13)** Das Informationssystem verbietet die Verwendung von zwischengespeicherten Authentifikatoren nach [Zuordnung: von der Organisation definierter Zeitraum].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten Ressourcen sind so konfiguriert, dass keine zwischengespeicherten Authentifikatoren verwendet werden können. Für die Authentifizierung bei bereitgestellten virtuellen Computern ist es erforderlich, dass zum Zeitpunkt der Authentifizierung ein Authentifikator eingegeben wird. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-6"></a>NIST 800-53, Regelung IA-6

#### <a name="authenticator-feedback"></a>Authentifikatorrückmeldung

**IA-6** Das Informationssystem verhindert die Rückmeldung von Authentifizierungsinformationen während des Authentifizierungsprozesses, um die Informationen vor einer möglichen Ausnutzung durch Unbefugte zu schützen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Zugriff auf Ressourcen, die durch diesen Blueprint bereitgestellt werden, erfolgt über Remotedesktop und basiert auf der Windows-Authentifizierung. Das Standardverhalten von Windows-Authentifizierungssitzungen maskiert Kennwörter bei der Eingabe während einer Authentifizierungssitzung.  |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-7"></a>NIST 800-53, Regelung IA-7

#### <a name="cryptographic-module-authentication"></a>Kryptografiemodulauthentifizierung

**IA-7** Das Informationssystem implementiert Authentifizierungsmechanismen in einem Kryptografiemodul, die den Anforderungen geltender Bundesgesetze, Durchführungsverordnungen, Direktiven, Richtlinien, Bestimmungen, Standards und Vorschriften für eine solche Authentifizierung entsprechen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Von diesem Blueprint werden Windows-Authentifizierung, Remotedesktopdienste und BitLocker verwendet. Diese Komponenten können für die Nutzung von gemäß FIPS 140 überprüften Kryptografiemodulen konfiguriert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-ia-8"></a>NIST 800-53, Regelung IA-8

#### <a name="identification-and-authentication-non-organizational-users"></a>Identifikation und Authentifizierung (Nicht-Organisationsbenutzer)

**IA-8** Das Informationssystem sorgt für eine eindeutige Identifikation und Authentifizierung der Nicht-Organisationsbenutzer (oder Prozesse, die im Namen der Nicht-Organisationsbenutzer agieren).

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist verantwortlich für die Identifikation und Authentifizierung von Nicht-Organisationsbenutzern, die auf kundenseitig bereitgestellte Ressourcen zugreifen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-8-1"></a>NIST 800-53, Regelung IA-8 (1)

#### <a name="identification-and-authentication-non-organizational-users--acceptance-of-piv-credentials-from-other-agencies"></a>Identifikation und Authentifizierung (Nicht-Organisationsbenutzer)| Akzeptieren von PIV-Anmeldeinformationen von anderen Behörden

**IA-8 (1)** Das Informationssystem akzeptiert PIV-Anmeldeinformationen (Personal Identity Verification) von anderen Bundesbehörden und führt eine elektronische Überprüfung der Anmeldeinformationen durch.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für das Akzeptieren und Überprüfen von PIV-Anmeldeinformationen (Personal Identity Verification) verantwortlich, die von anderen Bundesbehörden ausgegeben wurden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-8-2"></a>NIST 800-53, Regelung IA-8 (2)

#### <a name="identification-and-authentication-non-organizational-users--acceptance-of-third-party-credentials"></a>Identifikation und Authentifizierung (Organisationsbenutzer)| Akzeptieren von Anmeldeinformationen von Drittanbietern

**IA-8 (2)** Das Informationssystem akzeptiert nur FICAM-genehmigte Anmeldeinformationen von Drittanbietern.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, dass nur von der Federal Identity, Credential, and Access Management (FICAM) Trust Framework Solutions Initiative genehmigte Anmeldeinformationen von Drittanbietern akzeptiert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-8-3"></a>NIST 800-53, Regelung IA-8 (3)

#### <a name="identification-and-authentication-non-organizational-users--use-of-ficam-approved-products"></a>Identifikation und Authentifizierung (Nicht-Organisationsbenutzer) | Verwendung von FICAM-genehmigten Produkten

**IA-8 (3)** Die Organisation verwendet nur FICAM- genehmigte Informationssystemkomponenten in [Zuordnung: von der Organisation definierte Informationssysteme], um Anmeldeinformationen von Drittanbietern zu akzeptieren.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, dass er nur die von der Federal Identity, Credential, and Access Management (FICAM) Trust Framework Solutions Initiative genehmigten Ressourcen für das Akzeptieren von Anmeldeinformationen von Drittanbietern einsetzt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-ia-8-4"></a>NIST 800-53, Regelung IA-8 (4)

#### <a name="identification-and-authentication-non-organizational-users--use-of-ficam-issued-profiles"></a>Identifikation und Authentifizierung (Nicht-Organisationsbenutzer) | Verwendung von über FICAM ausgegebenen Profilen

**IA-8 (4)** Das Informationssystem entspricht den von der FICAM herausgegebenen Profilen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Einhaltung der von der Federal Identity, Credential, and Access Management (FICAM) Trust Framework Solutions Initiative ausgegebenen Profile verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |
