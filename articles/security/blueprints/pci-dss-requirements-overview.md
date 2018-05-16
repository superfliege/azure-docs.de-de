---
title: 'Blueprint zur Azure-Zahlungsverarbeitung: Allgemeine Übersicht'
description: 'Blueprint zur Azure-Zahlungsverarbeitung: Kundenzuständigkeitsmatrix (Übersicht)'
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 23cf68d8-bebd-4ac4-a194-39e052281c0e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 4c36f50b5c4ceba911003ec7a633dcab8724c6e0
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="pci-dss-requirements---high-level-overview"></a>Anforderungen des PCI-DSS: Allgemeine Übersicht

Der PCI-DSS-Standard (Payment Card Industry Data Security Standard) wurde mit dem Ziel entwickelt, die Sicherheit von Karteninhaberdaten zu fördern und zu verbessern und die breite Akzeptanz konsistenter Datensicherheitsmaßnahmen voranzutreiben. Der PCI-DSS vermittelt eine Grundlage mit technischen und betrieblichen Anforderungen für den Schutz von Kontodaten. Der PCI-DSS gilt für alle Rechtsträger, die mit der Verarbeitung bargeldloser Zahlungsmittel befasst sind, d. h. Händler, Verarbeiter, Ankäufer, Aussteller und Dienstleister. Ferner gilt PCI-DSS für alle weiteren Rechtsträger, die Daten von Karteninhabern und/oder sensible Authentifizierungsdaten speichern, verarbeiten oder übertragen. Nachfolgend finden Sie eine allgemeine Übersicht zu den 12 Anforderungen des PCI-DSS.

> [!NOTE]
> Diese Anforderungen wurden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

|   |   |
|---|---|
| **Erstellen und Verwalten eines <br/>sicheren Netzwerks und sicherer Systeme** | 1. [Installieren und Pflegen einer Firewallkonfiguration zum Schutz der Karteninhaberdaten](pci-dss-requirement-1-firewall.md)<br/><br/> 2. [Ändern von Kennwörtern und anderen Sicherheitseinstellungen nach der Werksauslieferung](pci-dss-requirement-2-password.md) |  
| **Schützen der Karteninhaberdaten** | 3. [Schützen der gespeicherten Karteninhaberdaten](pci-dss-requirement-3-chd.md)<br/><br/> 4. [Verschlüsselte Übertragung der Karteninhaberdaten über offene öffentliche Netzwerke](pci-dss-requirement-4-encryption.md) |
| **Einsetzen eines Programms zur<br/>Verwaltung von Sicherheitslücken** | 5. [Schützen aller Systeme vor Schadsoftware und regelmäßige Aktualisierung von Virenschutzsoftware oder -programmen](pci-dss-requirement-5-malware.md)<br/><br/> 6. [Entwickeln und Warten sicherer Systeme und Anwendungen](pci-dss-requirement-6-secure-system.md) |
| **Implementieren strikter Maßnahmen<br/>zur Zugriffssteuerung** | 7. [Beschränken des Zugriffs auf Karteninhaberdaten auf das geschäftlich erforderliche Maß](pci-dss-requirement-7-access.md)<br/><br/> 8. [Erkennen und Authentifizieren des Zugriffs auf Systemkomponenten](pci-dss-requirement-8-identity.md) <br/><br/> 9. [Beschränken des physischen Zugriffs auf Karteninhaberdaten](pci-dss-requirement-9-physical-access.md) |
| **Regelmäßiges Überwachen und<br/>Testen von Netzwerken** | 10. [Nachverfolgen und Überwachen des gesamten Zugriffs auf Netzwerkressourcen und Karteninhaberdaten](pci-dss-requirement-10-monitoring.md) <br/><br/> 11. [Regelmäßiges Testen der Sicherheitssysteme und -prozesse](pci-dss-requirement-11-testing.md) |
| **Verwalten einer <br/>Datensicherheitsrichtlinie** | 12. [Verwalten einer Richtlinie, die die IT-Sicherheit für alle Mitarbeiter regelt](pci-dss-requirement-12-policy.md) |

