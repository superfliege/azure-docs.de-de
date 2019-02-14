---
title: Kapazitätsplanung für Azure Stack | Microsoft Docs
description: Informationen zur Kapazitätsplanung für Azure Stack-Bereitstellungen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: prchint
ms.lastreviewed: 09/18/2018
ms.openlocfilehash: b8bd57953845278aa75e8cbdf41ae28300edad58
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56184906"
---
# <a name="azure-stack-capacity-planning"></a>Azure Stack-Kapazitätsplanung
Beim Auswerten einer Azure Stack-Lösung müssen Entscheidungen in Bezug auf die Hardwarekonfiguration getroffen werden, die eine direkte Auswirkung auf die Gesamtkapazität der Azure Stack-Cloud haben. Hierzu gehört die übliche Auswahl von CPU, Arbeitsspeicherdichte, Speicherkonfiguration und Gesamtumfang der Lösung (z.B. Anzahl von Servern). Im Gegensatz zu einer herkömmlichen Virtualisierungslösung gilt die einfache Arithmetik dieser Komponenten zur Ermittlung der nutzbaren Kapazität nicht. Der erste Grund dafür ist, dass Azure Stack so aufgebaut ist, dass die Infrastruktur- bzw. Verwaltungskomponenten in der Lösung selbst gehostet werden. Der zweite Grund ist, dass ein Teil der Lösungskapazität zur Unterstützung der Resilienz reserviert ist. Es geht um die Aktualisierung der Lösungssoftware auf eine Weise, bei der die Beeinträchtigung von Mandantenworkloads verringert wird.

> [!IMPORTANT]
> Die bereitgestellten Informationen zur Kapazitätsplanung und die zugehörige Kalkulationstabelle dienen nur als Leitfaden, um Ihnen bei der Planung und Konfiguration von Azure Stack zu helfen. Diese Informationen sollen aber nicht als Ersatz für Ihre eigenen Untersuchungen und Analysen dienen. 

## <a name="compute-and-storage-capacity-planning"></a>Planen der Compute- und Speicherkapazität
Eine Azure Stack-Lösung ist als hyperkonvergierter Cluster aus Compute, Netzwerk und Speicher aufgebaut. Dies ermöglicht eine effektive Nutzung oder gemeinsame Verwendung der gesamten Hardwarekapazität im Cluster (die als Skalierungseinheit für Azure Stack bezeichnet wird) in einer Weise, die Verfügbarkeit und Skalierbarkeit gewährleistet. Die gesamte Infrastruktursoftware wird in einer Gruppe von virtuellen Computern (VMs) gehostet und verwendet die gleichen physischen Server wie die Mandanten-VMs. Alle virtuellen Computer werden dann von den Windows Server-Clusteringtechnologien der Skalierungseinheit und einzelnen Hyper-V-Instanzen verwaltet. Dieser Ansatz vereinfacht die Beschaffung und Verwaltung einer Azure Stack-Lösung und ermöglicht die Verschiebung und Skalierbarkeit aller Dienste (Mandanten und Infrastruktur) über die gesamte Skalierungseinheit hinweg.

Die einzige physische Ressource, die in einer Azure Stack-Lösung nicht überdimensioniert wird, ist Serverspeicher. Die anderen Ressourcen, CPU-Kerne, Netzwerkbandbreite und Speicherkapazität werden überdimensioniert, um die verfügbaren Ressourcen optimal zu nutzen. Bei der Berechnung der verfügbaren Kapazität für eine Lösung ist der physische Serverspeicher der Hauptfaktor. Die Nutzung anderer Ressourcen bedeutet dann, das Verhältnis zwischen der möglichen Überdimensionierung und dem zu verstehen, was für die beabsichtigte Workload akzeptabel ist.

Rund 28 virtuelle Computer werden für das Hosting der Infrastruktur von Azure Stack verwendet und nutzen insgesamt etwa 208 GB Speicher und 124 virtuelle Kerne.  Der Grund für diese Anzahl von virtuellen Computern besteht darin, die erforderliche Diensttrennung zu erfüllen, um die Anforderungen an Sicherheit, Skalierbarkeit, Wartung und Patches zu gewährleisten. Diese interne Dienststruktur ermöglicht die zukünftige Einführung neuer Infrastrukturdienste im Rahmen ihrer Entwicklung.

Um das automatisierte Update aller physischen Server- und Infrastruktursoftwarekomponenten oder Patchen und Update zu unterstützen, nutzen Infrastruktur- und Benutzer-VM-Platzierungen nicht alle Speicherressourcen der Skalierungseinheit. Ein Teil des Gesamtspeichers über alle Server einer Skalierungseinheit hinweg wird nicht zugeordnet, um die Anforderungen der Lösung an Resilienz zu erfüllen. Wenn beispielsweise das Windows Server-Image des physischen Servers aktualisiert wird, werden die auf dem Server gehosteten virtuellen Computer an einen anderen Speicherort innerhalb der Skalierungseinheit verschoben, während die Windows Server-Images des Servers aktualisiert werden. Wenn das Update abgeschlossen ist, wird der Server neu gestartet und verarbeitet erneut Workloads. Das Ziel von Patch und Update einer Azure Stack-Lösung ist es, die Notwendigkeit zu vermeiden, gehostete virtuelle Computer beenden zu müssen. Um dieses Ziel zu erreichen, ist ein absolutes Minimum an Speicherkapazität eines Servers nicht zugeordnet, um das Verschieben von virtuellen Computern innerhalb der Skalierungseinheit zu ermöglichen. Diese Platzierung und Verschiebung gilt sowohl für Infrastruktur-VMs als auch für virtuelle Computer, die im Auftrag des Benutzers oder Mandanten der Azure Stack-Lösung erstellt wurden. Die endgültigen Implementierungsergebnisse sind so, dass die für die Unterstützung der erforderlichen VM-Verschiebung reservierte Speichermenge viel mehr als die Kapazität eines einzelnen Servers betragen kann, da Platzierungsprobleme bei virtuellen Computern mit unterschiedlichem Speicherbedarf auftreten können. Ein zusätzlicher Mehraufwand in der Kategorie der Speichernutzung ist der Mehraufwand der Windows Server-Instanz selbst. Die Basis-Betriebssysteminstanz für jeden Server nutzt Speicher für das Betriebssystem und seine virtuellen Auslagerungstabellen sowie den Speicher, der von Hyper-V für die Verwaltung der einzelnen gehosteten virtuellen Computer verwendet wird.

Eine detaillierte Beschreibung der Komplexität von Kapazitätsberechnungen finden Sie weiter unten in diesem Abschnitt. In dieser Einführung werden die folgenden Beispiele vorgestellt, um das Verständnis der verfügbaren Kapazität unterschiedlicher Lösungsgrößen zu erleichtern. Dabei handelt es sich um Schätzungen, die Annahmen über die Nutzung des VM-Speichers des Mandanten enthalten, die für Produktionsinstallationen möglicherweise nicht zutreffen. Für diese Tabelle wird die Azure-VM-Standardgröße D2 verwendet. Azure-Standard-VMs der Größe D2werden mit 2 virtuellen CPUs und 7 GB Arbeitsspeicher definiert.

|     |Kapazität pro Server|| Kapazität der Skalierungseinheit|  |  |||
|-----|-----|-----|-----|-----|-----|-----|-----|
|     | Arbeitsspeicher | CPU-Kerne | Anzahl von Servern | Arbeitsspeicher | CPU-Kerne | Mandanten-VMs<sup>1</sup>     | Kernverhältnis<sup>2</sup>    |
|Beispiel 1|256 GB|28|4|1024 GB| 112 | 54 |4:3|
|Beispiel 2|512 GB|28|4|2.024 GB|112|144|4:1|
|Beispiel 3|384 GB|28|12|4608 GB|336|432|3:1|
|     |     |     |     |     |     |     |     |

> <sup>1</sup> Standard-VMs D2.

> <sup>2</sup> Verhältnis von virtuellen Kernen zu physischen Kernen.

Wie bereits weiter oben erwähnt, wird die VM-Kapazität durch den verfügbaren Speicher bestimmt. Das Verhältnis von virtuellen Kernen zu physischen Kernen veranschaulicht, wie sich die VM-Dichte auf die verfügbare CPU-Kapazität auswirkt, wenn die Lösung nicht mit einer größeren Anzahl von physischen Kernen ausgestattet (eine andere CPU ausgewählt) wird. Dasselbe gilt für die Speicherkapazität und Speichercachekapazität.

Die oben genannten Beispiele für die VM-Dichte dienen nur der Erläuterung. Auf die Berechnungen zur Unterstützung trifft weitere Komplexität zu. Der Kontakt mit Microsoft oder einem Lösungspartner ist erforderlich, um die Entscheidungen zur Kapazitätsplanung und die sich daraus ergebenden verfügbaren Kapazitäten besser zu verstehen.

Der Rest dieses Abschnitts beschreibt die Anforderungen an die Bereitstellung von Azure Stack für Compute und Speicher. Verwenden Sie diese Informationen, um ein grundlegendes Verständnis dafür zu erlangen, welche Komponenten erforderlich sind und welche Mindestkonfigurationswerte diese aufweisen. Darüber hinaus werden zusätzliche Informationen bereitgestellt, um zu beschreiben, wie die Lösung im Hinblick auf die verfügbare Kapazität und mögliche Grenzen des Systems in Bezug auf Mandantenkapazität und Leistungsfähigkeit konfiguriert ist.

> [!NOTE]
> Die Kapazitätsplanungsanforderungen für Netzwerke sind minimal, da nur die Größe der öffentlichen virtuellen IP-Adresse (VIP) konfigurierbar ist. Informationen zur Vorgehensweise beim Hinzufügen weiterer öffentlicher IP-Adresse zu Azure Stack finden Sie unter [Hinzufügen öffentlicher IP-Adressen](azure-stack-add-ips.md).


## <a name="next-steps"></a>Nächste Schritte
[Planen der Computekapazität](capacity-planning-compute.md)
