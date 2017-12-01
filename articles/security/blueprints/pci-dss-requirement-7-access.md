---
title: "Blueprint zur Azure-Zahlungsverarbeitung – Anforderungen für den Zugriff"
description: PCI-DSS-Anforderung 7
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: ac3afee9-0471-465d-a115-67488a1635a6
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 5a3c9eac552fb96309cfa791a2e72a7102662e60
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="access-requirements-for-pci-dss-compliant-environments"></a>Zugriffsanforderungen für PCI-DSS-konforme Umgebungen 
## <a name="pci-dss-requirement-7"></a>PCI-DSS-Anforderung 7

**Beschränken des Zugriffs auf Karteninhaberdaten auf das geschäftlich erforderliche Maß**

> [!NOTE]
> Diese Anforderungen werden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

Um sicherzustellen, dass kritische Daten nur von autorisiertem Personal abgerufen werden können, müssen Systeme und Prozesse vorhanden sein, die den Zugriff je nach erforderlichem Maß und Verantwortlichkeiten einschränken.

Ein „erforderliches Maß“ liegt vor, wenn Zugriffsrechte nur für die geringste Menge an Daten und für den Ansatz der geringsten Rechte gewährt werden, die für die Ausführung eines Auftrags erforderlich sind.

## <a name="pci-dss-requirement-71"></a>PCI-DSS-Anforderung 7.1

**7.1** Beschränken Sie den Zugriff auf Systemkomponenten und Karteninhaberdaten auf diejenigen Personen, deren Funktion einen solchen Zugriff erfordert.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Azure erzwingt bestehende ISMS-Richtlinien in Bezug auf den Zugriff des Azure-Personals auf Azure-Systemkomponenten, die Überprüfung der Wirksamkeit der Zugriffssteuerung, die Bereitstellung von Just-In-Time-Administratorzugriff, den Widerruf des Zugriffs, wenn er nicht mehr benötigt wird, und die Sicherstellung, dass Mitarbeiter, die auf die Azure-Plattform zugreifen, geschäftliche Anforderungen haben. Der Azure-Zugriff auf Kundenumgebungen ist stark eingeschränkt und nur mit Zustimmung des Kunden gestattet.<br /><br />Es wurden Verfahren eingeführt, um den physischen Zugriff auf das Rechenzentrum auf autorisierte Mitarbeiter, Hersteller, Auftragnehmer und Besucher zu beschränken. Die Sicherheitsüberprüfung und das Einchecken sind für Mitarbeiter erforderlich, die vorübergehend Zugriff auf das Innere des Rechenzentrums benötigen. Physische Zugriffsprotokolle werden vierteljährlich von Azure-Teams überprüft. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Kunden Sie für die Beschränkung des Zugriffs auf Systemkomponenten und Karteninhaberdaten auf diejenigen Personen zuständig, deren Funktion einen solchen Zugriff erfordert. Dazu gehören das Einschränken und Beschränken des Zugriffs auf das Azure-Verwaltungsportal sowie das Festlegen von Konten oder Rollen mit der Berechtigung, PaaS-Dienste zu erstellen, zu ändern oder zu löschen.|



### <a name="pci-dss-requirement-711"></a>PCI-DSS-Anforderung 7.1.1

**7.1.1** Definieren Sie die Zugriffsanforderungen für die einzelnen Rollen, einschließlich:
- Systemkomponenten und Datenressourcen, auf die jede Rolle für ihre Funktion zugreifen muss
- Erforderliche Berechtigungsstufe (z. B. Benutzer, Administrator usw.) für den Zugriff auf Ressourcen

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Kunden sind verantwortlich für die Definition und Dokumentation eines Genehmigungsprozesses für Benutzer-IDs, die Definition des Ansatzes der geringsten Rechte, die Beschränkung des Zugriffs auf Karteninhaberdaten, die Verwendung eindeutiger IDs, die Trennung von Aufgaben und den Widerruf des Benutzerzugriffs, wenn dieser nicht mehr erforderlich ist.|



### <a name="pci-dss-requirement-712"></a>PCI-DSS-Anforderung 7.1.2

**7.1.2** Beschränken Sie den Zugriff auf privilegierte Benutzer-IDs auf die geringsten Rechte, die für die Ausführung von Aufgaben erforderlich sind.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure hat anwendbare geschäftliche und organisatorische Sicherheitsrichtlinien übernommen, darunter eine Richtlinie zur Informationssicherheit. Die Richtlinien wurden genehmigt, veröffentlicht und an Microsoft Azure übermittelt. Die Richtlinie zur Informationssicherheit von Microsoft Azure erfordert, dass der Zugriff auf Microsoft Azure-Objekte basierend auf einer geschäftlichen Begründung mit der Autorisierung des Objektbesitzers gewährt wird und basierend auf den Prinzipien zum „erforderlichen Maß“ und den „geringsten Rechten“ eingeschränkt ist. Die Richtlinie betrifft auch Anforderungen für den Zugriffsverwaltungs-Lebenszyklus, einschließlich Zugriffsbereitstellung, Zugriffsautorisierung, Widerruf der Autorisierung von Zugriffsrechten und regelmäßige Zugriffsüberprüfungen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erstellt während der Bereitstellung drei Konten: „admin“, „sqladmin“ und „edna“ (der während der Demoausführung bei der Web-App angemeldete Standardbenutzer). Benutzerrollen sind auf die Aufgaben beschränkt, die auf dem dokumentierten Demoszenario basieren.|



### <a name="pci-dss-requirement-713"></a>PCI-DSS-Anforderung 7.1.3

**7.1.3** Weisen Sie den Zugriff auf der Grundlage der Stellenklassifizierung und der Funktion der einzelnen Mitarbeiter zu.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erstellt während der Bereitstellung drei Konten: „admin“, „sqladmin“ und „edna“ (der während der Demoausführung bei der Web-App angemeldete Standardbenutzer). Benutzerrollen sind auf die Aufgaben beschränkt, die auf dem dokumentierten Demoszenario basieren.|



### <a name="pci-dss-requirement-714"></a>PCI-DSS-Anforderung 7.1.4

**7.1.4** Fordern Sie eine dokumentierte Genehmigung durch autorisierte Stellen an, die die erforderlichen Berechtigungen festlegen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Kunden Sie für die Beschränkung des Zugriffs auf Systemkomponenten und Karteninhaberdaten auf diejenigen Personen zuständig, deren Funktion einen solchen Zugriff erfordert. Dazu gehören das Einschränken und Beschränken des Zugriffs auf das Azure-Verwaltungsportal sowie das Festlegen von Konten oder Rollen mit der Berechtigung, PaaS-Dienste zu erstellen, zu ändern oder zu löschen.|



## <a name="pci-dss-requirement-72"></a>PCI-DSS-Anforderung 7.2

**7.2** Richten Sie ein Zugriffssteuerungssystem für Systemkomponenten ein, das den Zugriff je nach erforderlichem Maß eines Benutzers einschränkt und auf „Alles verweigern“ eingestellt ist, sofern dies nicht ausdrücklich erlaubt ist.
Dieses Zugriffssteuerungssystem muss Folgendes umfassen:
- 7.2.1 Abdeckung aller Systemkomponenten
- 7.2.2 Zuweisung von Berechtigungen an Einzelpersonen auf der Grundlage von Stellenklassifizierung und Funktion
- 7.2.3 Standardeinstellung „Alles verweigern“

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet Azure Active Directory, um den Zugriff auf bestimmte Benutzer zu beschränken. Weitere Informationen finden Sie in den [PCI-Anleitungen für Identitätsverwaltung](payment-processing-blueprint.md#identity-management).|



## <a name="pci-dss-requirement-73"></a>PCI-DSS-Anforderung 7.3

**7.3** Stellen Sie sicher, dass Sicherheitsrichtlinien und betriebliche Verfahren für das Einschränken des Zugriffs auf Daten von Karteninhabern dokumentiert und verwendet werden sowie allen beteiligten Parteien bekannt sind.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Dokumentation enthält einen Anwendungsfall und eine Beschreibung, wer CHD verwendet und wie CHD verwendet wird.|




