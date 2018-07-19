---
title: Azure-Einrichtungen, Gelände und physische Sicherheit | Microsoft-Dokumentation
description: Der Artikel beschreibt die Azure-Rechenzentren, einschließlich der physischen Infrastruktur, der Sicherheit und des Complianceangebots.
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
ms.openlocfilehash: a6a9b1d6e12dabb09cde684c34481b4b3442c1b8
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101531"
---
# <a name="azure-facilities-premises-and-physical-security"></a>Azure-Einrichtungen, Gelände und physische Sicherheit
Die Microsoft-Cloud besteht aus einer [global verteilten Rechenzentrumsinfrastruktur](https://azure.microsoft.com/global-infrastructure/). die Tausende von Onlinediensten unterstützt und mehr als 100 höchst sichere Einrichtungen weltweit umfasst.

Die Infrastruktur bringt Anwendungen und Benutzer weltweit näher zusammen, stellt dabei die Datenresidenz sicher und bietet den Kunden umfassende Optionen hinsichtlich Compliance und Ausfallsicherheit. Azure hat 52-Regionen weltweit und ist in 140 Ländern verfügbar.

Als Region wird eine Reihe von Rechenzentren bezeichnet, die über ein großes und robustes Netzwerk miteinander verbunden sind. Das Netzwerk umfasst standardmäßig Inhaltsverteilung, Lastenausgleich, Redundanz und Verschlüsselung. Azure verfügt über mehr globale Regionen als jeder andere Cloudanbieter, sodass Kunden ihre Anwendungen flexibler dort bereitstellen können, wo sie diese benötigen.

Azure-Regionen sind in Geografien unterteilt. Eine Azure-Geografie sorgt dafür, dass Anforderungen an Datenresidenz, Datenhoheit, Compliance und Ausfallsicherheit innerhalb geografischer Grenzen erfüllt werden.

In Geografien können Kunden mit spezifischen Anforderungen an Datenresidenz und Compliance ihre Daten und Anwendungen eng zusammenhalten. Geografien sind fehlertolerant und erhalten dank ihrer Verbindung mit unserer dedizierten Netzwerkinfrastruktur mit sehr hohen Kapazitäten die Verfügbarkeit auch beim Ausfall einer ganzen Region.

Verfügbarkeitszonen sind physisch getrennte Standorte in einer Azure-Region. Jede Verfügbarkeitszone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Durch Verfügbarkeitszonen können Kunden unternehmenskritische Anwendungen mit hoher Verfügbarkeit sowie Wiederherstellbarkeit mit geringer Latenz ausführen.

Die folgende Abbildung veranschaulicht, wie die globale Azur-Infrastruktur Regionen und Verfügbarkeitszonen innerhalb der gleichen Datenresidenzgrenze koppelt, um für Hochverfügbarkeit, Notfallwiederherstellung und Sicherungsfunktionen zu sorgen.

![Datenresidenzgenze][1]

Dank der großen geografischen Verteilung von Rechenzentren ist Microsoft in der Nähe des Kunden und kann die Netzwerklatenz reduzieren und die georedundante Sicherung und Failover ermöglichen.

## <a name="physical-security"></a>Physische Sicherheit
Microsoft entwickelt, erstellt und betreibt Rechenzentren so, dass der physische Zugriff auf die Bereiche, in denen Kundendaten gespeichert sind, streng kontrolliert wird. Microsoft kennt die Wichtigkeit des Schutzes von Kundendaten und hat sich den Schutz der Rechenzentren, die Ihre Daten enthalten, auf die Fahnen geschrieben. Eine komplette Abteilung befasst sich bei Microsoft mit der Entwicklung, der Erstellung und dem Betrieb der Einrichtungen, die Azure unterstützen. Dieses Team hat die Aufgabe, die physische Sicherheit auf dem neuesten Stand zu halten.

Microsoft verfolgt einen mehrstufigen Ansatz bei der physischen Sicherheit, um das Risiko zu verringern, dass nicht autorisierte Benutzer physischen Zugriff auf Daten und Rechenzentrumsressourcen erhalten. Von Microsoft verwaltete Rechenzentren haben umfassende Schutzebenen: Zugriffsgenehmigung an der Einrichtungsgrenze, an der Gebäudegrenze, im Gebäude und in der Rechenzentrumsetage. Physische Sicherheitsebenen sind:

- Zugriffsanforderung und -genehmigung: Sie müssen den Zugriff vor dem Eintreffen im Rechenzentrum anfordern. Sie müssen eine triftige geschäftliche Begründung Ihres Besuchs angeben, z.B. Compliance- oder Überwachungszwecke. Alle Anforderungen werden nach Notwendigkeit des Zugangs von Microsoft-Mitarbeitern genehmigt. Dank der „Notwendigkeit des Zugangs“ kann die Anzahl der Personen, die für die Durchführung einer Aufgabe in Rechenzentren erforderlich sind, auf das absolute Minimum reduziert werden. Nachdem die Berechtigung erteilt wurde, hat nur eine Person basierend auf der genehmigten geschäftlichen Begründung Zugang zu dem abgegrenzten Bereich des Rechenzentrums. Berechtigungen sind auf einen bestimmten Zeitraum befristet und laufen nach dem genehmigten Zeitraum ab.

- Einrichtungsgrenze: Wenn Sie am Rechenzentrum eintreffen, müssen Sie durch einen genau definierten Zugangspunkt gehen. In der Regel besteht jeder Zentimeter dieser Eingrenzung aus hohen Zäunen aus Stahl und Beton. Die Rechenzentren sind von Kameras umgeben, und ein Sicherheitsteam überwacht die Videos ganzjährig rund um die Uhr.

- Gebäudeeingang: Der Eingang des Rechenzentrums wird von Sicherheitspersonal überwacht, das eine rigorose Ausbildung durchlaufen hat und gründlichen Zuverlässigkeitsüberprüfungen unterzogen wurde. Das Sicherheitspersonal geht außerdem routinemäßig das Rechenzentrum ab und überwacht ganzjährig rund um die Uhr die Videos der Kameras im Rechenzentrum.

- Im Gebäude: Nachdem Sie das Gebäude betreten haben, unterziehen Sie sich einer zweistufigen Authentifizierung mittels biometrischer Daten, damit Sie sich weiter im Rechenzentrum bewegen können. Wenn Ihre Identität überprüft wurde, können Sie den Teil des Rechenzentrums betreten, für den der Zugang genehmigt wurde. Sie können sich dort nur für die Dauer der genehmigten Zeitspanne aufhalten.

- Etage des Rechenzentrums: Sie dürfen nur die Etage betreten, die genehmigt wurde. Sie müssen eine Personenkontrolle mit Metalldetektor durchlaufen. Um das Risiko zu verringern, dass ohne unser Wissen unberechtigte Daten in das Rechenzentrum gelangen oder dieses verlassen, dürfen nur genehmigte Geräte in die Rechenzentrumsetage mitgenommen werden. Darüber hinaus überwachen Kameras die Vorder- und Rückseite jedes Serverracks. Die Personenkontrolle mit Metalldetektor wird beim Verlassen des Rechenzentrums wiederholt. Bevor Sie das Rechenzentrum verlassen können, müssen Sie sich einer weiteren Sicherheitsüberprüfung unterziehen.

Besucher müssen Ausweise vor dem Verlassen eines Microsoft-Gebäudes abgeben.

## <a name="physical-security-reviews"></a>Physische Sicherheitsprüfungen
Physische Sicherheitsprüfungen der Einrichtungen werden in regelmäßigen Abständen ausgeführt, um sicherzustellen, dass die Rechenzentren die Microsoft Azure-Sicherheitsanforderungen erfüllen. Die Mitarbeiter des Rechenzentrum-Hostinganbieters bietet keine Microsoft Azure-Dienstverwaltung. Mitarbeiter haben keinen Zugriff per Anmeldung auf die Azure-Systeme bzw. keinen physischen Zugriff auf den Azure-Kollokationsraum und Käfige.

## <a name="data-bearing-devices"></a>Geräte mit Daten
Microsoft verwendet bewährte Verfahren und eine Zurücksetzungslösung, die [NIST 800-88-konform](https://csrc.nist.gov/publications/detail/sp/800-88/archive/2006-09-01) ist. Für Festplatten, die nicht zurückgesetzt werden können, wird ein Zerstörungsverfahren verwendet, das die Platte zerstört und die Wiederherstellung der Daten unmöglich macht. Mögliche Zerstörungsverfahren sind Zersetzen, Schreddern, Pulverisieren oder Verbrennen. Das geeignete Entsorgungsverfahren wird durch den Typ der Ressource bestimmt. Die Zerstörung wird dokumentiert, und die Dokumentation wird aufbewahrt.  

## <a name="equipment-disposal"></a>Entsorgung von Ausrüstung
Microsoft Azure implementiert dieses Prinzip im Kundenauftrag. Am Ende der Nutzungsdauer eines Systems befolgen die für den Betrieb zuständigen Microsoft-Mitarbeiter strenge Verfahren zum Umgang mit Daten und zur Entsorgung von Hardware, um sicherzustellen, dass keine Hardware, die eventuell Kundendaten enthält, nicht vertrauenswürdigen Dritten zugänglich gemacht wird. Dabei wird ein Secure Erase-Ansatz befolgt (mithilfe von Festplatten-Firmware) für Festplatten, die ihn unterstützen. Für Festplatten, die nicht zurückgesetzt werden können, wird ein Zerstörungsverfahren verwendet, das die Platte zerstört und die Wiederherstellung der Daten unmöglich macht. Mögliche Zerstörungsverfahren sind Zersetzen, Schreddern, Pulverisieren oder Verbrennen. Das geeignete Entsorgungsverfahren wird durch den Typ der Ressource bestimmt. Die Zerstörung wird dokumentiert, und die Dokumentation wird aufbewahrt. Alle Microsoft Azure-Dienste setzen anerkannte Dienste zur Speicherung auf Medien und Verwaltung der Entsorgung ein.

## <a name="compliance"></a>Compliance
Die Azure-Infrastruktur ist für eine Vielzahl internationaler und branchenspezifischer Compliancestandards wie ISO 27001, HIPAA, FedRAMP, SOC 1 und SOC 2 zu erfüllen, und wird entsprechend verwaltet. Auch länderspezifische Standards werden erfüllt, z.B. Australia IRAP, UK G-Cloud und Singapore MTCS. In rigorosen Audits durch Drittanbieter – zum Beispiel durch das British Standards Institute – wird überprüft, ob Azure die von diesen Standards geforderten strengen Sicherheitskontrollen einhält.

Eine vollständige Liste der von Azure eingehaltenen Compliancestandards finden Sie unter [Complianceangebote](https://www.microsoft.com/trustcenter/compliance/complianceofferings).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen dazu, wie Microsoft die Azure-Infrastruktur schützt, finden Sie in den folgenden Artikeln:

- [Verfügbarkeit der Azure-Infrastruktur](azure-infrastructure-availability.md)
- [Komponenten und Grenzen des Azure-Informationssystems](azure-infrastructure-components.md)
- [Azure-Netzwerkarchitektur](azure-infrastructure-network.md)
- [Azure-Produktionsnetzwerk](azure-production-network.md)
- [Sicherheitsfeatures für Microsoft Azure SQL-Datenbank](azure-infrastructure-sql.md)
- [Produktionsvorgänge und Verwaltung von Azure](azure-infrastructure-operations.md)
- [Überwachen der Azure-Infrastruktur](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure (Integrität der Azure-Infrastruktur)](azure-infrastructure-integrity.md)
- [Schutz von Kundendaten in Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-physical-security/data-residency-boundary.png
