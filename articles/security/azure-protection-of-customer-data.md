---
title: Schutz von Kundendaten in Azure
description: Dieser Artikel beschreibt, wie Azure die Kundendaten schützt.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 9a3b00e39f78f65b05b7d730447440d481979539
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101590"
---
# <a name="protection-of-customer-data-in-azure"></a>Schutz von Kundendaten in Azure   
Der Zugriff auf Kundendaten durch Microsoft-Betriebs- und -Supportpersonal wird standardmäßig verweigert. Wenn Zugriff auf Kundendaten gewährt wird, ist eine Genehmigung durch Führungskräfte erforderlich, und der Zugriff wird sorgfältig verwaltet und protokolliert. Die Zugriffssteuerungsanforderungen werden in der folgenden Microsoft Azure-Sicherheitsrichtlinie definiert:

- Kein standardmäßiger Zugriff auf Kundendaten
- Keine Benutzer- oder Administratorkonten auf Kunden-VMs
- Erteilen des geringsten Rechts, das zum Abschluss der Aufgabe erforderlich ist; Überwachen und Protokollieren der Zugriffsanforderungen

Den Supportmitarbeitern von Microsoft Azure wurden eindeutige Corporate-AD-Konten von Microsoft zugewiesen. Microsoft Azure überlässt die Steuerung des Zugriffs auf wichtige Informationssysteme dem von MSIT verwalteten unternehmenseigenen Active Directory von Microsoft. Die mehrstufige Authentifizierung ist erforderlich, und der Zugriff wird nur über sichere Konsolen erteilt.

Alle Zugriffsversuche werden überwacht und können über einen grundlegenden Satz von Berichten angezeigt werden.

## <a name="data-protection"></a>Datenschutz
Azure bietet Kunden hohe Datensicherheitsoptionen sowohl standardmäßig als auch als Kundenoptionen.

**Trennung von Daten**: Microsoft Azure ist ein mehrinstanzenfähiger Dienst, d.h. die Bereitstellungen und virtuellen Computer mehrerer Kunden werden auf derselben physischen Hardware gespeichert. Mithilfe einer logischen Isolierung trennt Azure die Daten der Kunden voneinander. Die Trennung bietet die Skalierung und die wirtschaftlichen Vorteile von mehrinstanzenfähigen Diensten und verhindert gleichzeitig rigoros, dass Kunden auf die Daten anderer Kunden zugreifen.

**Schutz von Daten im Ruhezustand**:-Kunden sind dafür verantwortlich sicherzustellen, dass in Azure gespeicherte Daten in Übereinstimmung mit ihren Standards verschlüsselt werden. Azure bietet eine große Bandbreite an Verschlüsselungsfunktionen und gibt Kunden so die Flexibilität, die für ihre Bedürfnisse optimal geeignete Lösung zu wählen. Azure Key Vault hilft Kunden, auf einfache Weise die Kontrolle über die von Cloudanwendungen und -diensten für die Verschlüsselung von Daten verwendeten Schlüssel zu pflegen. Mithilfe von Azure Disk Encryption können Kunden virtuelle Computer verschlüsseln. Mithilfe der Azure-Speicherdienstverschlüsselung können alle Daten im Speicherkonto eines Kunden verschlüsselt werden.

**Datenschutz bei der Übertragung**: Kunden können die Verschlüsselung für den Datenverkehr zwischen ihren eigenen VMs und den Endbenutzern ermöglichen. Azure schützt Daten während der Übertragung zu oder von externen Komponenten und intern, z.B. zwischen zwei virtuellen Netzwerken, übertragene Daten. Azure verwendet das branchenübliche TLS 1.2-Protokoll (Transport Layer Security) oder höher mit 2048-Bit RSA/SHA256-Verschlüsselungsschlüsseln, entsprechend der Empfehlung von CESG/NCSC, um die Kommunikation zu verschlüsseln zwischen:

- dem Kunden und der Cloud
- intern zwischen Azure-Systemen und Rechenzentren

**Verschlüsselung**: Die Verschlüsselung von Daten im Speicher und während der Übertragung kann von Kunden als bewährte Methode zum Sicherstellen der Vertraulichkeit und Integrität der Daten bereitgestellt werden. Kunden können auf einfache Weise ihre Azure-Cloud-Dienste so konfigurieren, dass sie mit SSL die Kommunikation über das Internet und sogar zwischen ihren von Azure gehosteten virtuellen Computern schützen.

**Datenredundanz**: Microsoft stellt sicher, dass Daten im Falle einer Cyberattacke oder einer physischen Beschädigung eines Rechenzentrums geschützt werden. Kunden können sich für Folgendes entscheiden:

- Aufbewahrung im Land aus Compliance- und Latenzgründen
- Aufbewahrung außerhalb des Lands aus Sicherheitsgründen und zwecks Notfallwiederherstellung

Daten können in einem ausgewählten geografischen Bereich aus Redundanzgründen repliziert werden, werden jedoch nicht außerhalb dieses Bereichs übertragen. Kunden haben mehrere Optionen für die Replikation von Daten, einschließlich der Anzahl von Kopien und der Anzahl und des Standorts der Replikationsrechenzentren.

Wenn Sie ein Speicherkonto erstellen, müssen Sie eine der folgenden Replikationsoptionen auswählen:

- Lokal redundanter Speicher (LRS). Lokal redundanter Speicher verwaltet drei Kopien Ihrer Daten. LRS wird innerhalb eines einzelnen Standorts dreimal in einer einzelnen Region repliziert. LRS schützt Ihre Daten vor normalen Hardware-Ausfällen, jedoch nicht vor dem Ausfall eines einzelnen Standorts.
- Zonenredundanter Speicher (ZRS). Zonenredundanter Speicher verwaltet drei Kopien Ihrer Daten. ZRS wird dreimal an zwei bis drei Standorten repliziert und bietet dann eine höhere Stabilität als LRS. Die Replikation erfolgt in einer einzelnen Region oder in zwei Regionen. ZRS gewährleistet die Dauerhaftigkeit Ihrer Daten innerhalb einer einzelnen Region.
- Georedundanter Speicher (GRS). Nach der Erstellung Ihres Speicherkontos ist geografisch redundanter Speicher standardmäßig aktiviert. Mit GRS werden Ihre Daten dreimal innerhalb der primären Region und dreimal in einer sekundären Region repliziert, die hunderte Kilometer von der primären Region entfernt ist. Mit GRS werden Ihre Daten dreimal in der primären Region repliziert. Ihre Daten werden auch dreimal in einer sekundären Region hunderte von Kilometern von der primären Region entfernt repliziert, wodurch höchste Stabilität erreicht wird. Im Falle eines Ausfalls in der primären Region wird für Azure Storage ein Failover auf die sekundäre Region ausgeführt. Durch GRS wird die Dauerhaftigkeit Ihrer Daten in zwei separaten Regionen gewährleistet.

**Datenvernichtung**: Wenn Kunden löschen oder Azure beenden, befolgt Microsoft strenge Standards für das Überschreiben von Speicherressourcen vor der Wiederverwendung sowie die physische Zerstörung der außer Betrieb gesetzten Hardware. Microsoft führt eine vollständige Löschung von Daten auf Wunsch der Kunden und beim Beenden des Vertrags durch.

## <a name="customer-data-ownership"></a>Dateneigentümerschaft liegt beim Kunden
Microsoft untersucht, genehmigt oder überwacht die Anwendungen nicht, die Kunden in Azure bereitstellen. Microsoft weiß außerdem nicht, welche Art von Daten die Kunden in Azure speichern. Microsoft beansprucht nicht die Dateneigentümerschaft über die in Azure eingegebenen Kundeninformationen.

## <a name="records-management"></a>Datensatzverwaltung
Azure verfügt über interne Datensatzaufbewahrungsanforderungen für Back-End-Daten. Die Kunden sind verantwortlich für die Identifizierung ihrer eigenen Datensatzaufbewahrungsanforderungen. Bei in Azure gespeicherten Datensätzen ist der Kunde dafür verantwortlich, dass ihre Daten extrahiert und Inhalte außerhalb von Azure für einen vom Kunden festgelegten Zeitraum aufbewahrt werden.

Azure bietet Kunden die Möglichkeit zum Exportieren von Daten und Überprüfen von Berichten aus dem Produkt. Die Exporte werden lokal gespeichert, um die Informationen während eines vom Kunden definierten Aufbewahrungszeitraums aufzubewahren.

## <a name="electronic-discovery-e-discovery"></a>eDiscovery
Azure-Kunden sind verantwortlich für die Einhaltung von eDiscovery-Anforderungen bei der Verwendung von Azure-Diensten. Wenn ein Azure-Kunde seine Kundendaten aufbewahren muss, kann er die Daten exportieren und lokal speichern. Darüber hinaus können Kunden Exporte ihrer Daten beim Azure-Kundensupport anfordern. Azure ermöglicht Kunden nicht nur, ihre Daten zu exportieren, sondern führt auch eine umfassende interne Protokollierung und Überwachung durch.

## <a name="next-steps"></a>Nächste Schritte
Weitere Information dazu, wie Microsoft die Azure-Infrastruktur schützt, finden Sie in den folgenden Artikeln:

- [Azure-Einrichtungen, Gelände und physische Sicherheit](azure-physical-security.md)
- [Verfügbarkeit der Azure-Infrastruktur](azure-infrastructure-availability.md)
- [Komponenten und Grenzen des Azure-Informationssystems](azure-infrastructure-components.md)
- [Azure-Netzwerkarchitektur](azure-infrastructure-network.md)
- [Azure-Produktionsnetzwerk](azure-production-network.md)
- [Sicherheitsfeatures für Microsoft Azure SQL-Datenbank](azure-infrastructure-sql.md)
- [Produktionsvorgänge und Verwaltung von Azure](azure-infrastructure-operations.md)
- [Überwachen der Azure-Infrastruktur](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure (Integrität der Azure-Infrastruktur)](azure-infrastructure-integrity.md)
