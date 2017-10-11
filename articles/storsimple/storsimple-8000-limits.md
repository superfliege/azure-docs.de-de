---
title: "StorSimple 8000-Serie systemeinschränkungen | Microsoft Docs"
description: "Beschreibt systemeinschränkungen und empfohlene Größen für die Komponenten der StorSimple 8000-Serie und Verbindungen."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c7392678-0924-46c6-9c59-1665cb9b6586
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/28/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cc3c0ad193af7625c8c4c1c2e82b6bdc8be33310
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="what-are-storsimple-8000-series-system-limits"></a>Was sind die StorSimple 8000-Serie systemeinschränkungen?

## <a name="overview"></a>Übersicht

StorSimple bietet skalierbare und flexible Speicher für Ihr Datencenter. Es gibt jedoch einige Einschränkungen, die Sie bedenken sollten Sie planen, bereitstellen und Ihrer StorSimple-Lösung betreiben. In der folgenden Tabelle werden diese Grenzwerte beschrieben und enthält einige Empfehlungen, damit Sie das beste aus Ihrer StorSimple-Lösung abrufen können.

| Limit-Bezeichner | Grenzwert | Kommentare |
| --- | --- | --- |
| Maximale Anzahl von Anmeldeinformationsobjekten |64 | |
| Maximale Anzahl von volumecontainern |64 | |
| Maximale Anzahl von volumes |255 | |
| Maximale Anzahl von lokalen volumes |32 | |
| Maximale Anzahl von Zeitplänen pro bandbreitenvorlage |168 |Einen Zeitplan für jede Stunde, jeden Tag der Woche (24 * 7). |
| Maximale Größe eines mehrstufigen Volumes auf physischen Geräten |64 TB für 8100 und 8600 |8100 und 8600 sind physische Geräte. |
| Maximale Größe eines mehrstufigen Volumes auf virtuellen Geräten in Azure |30 TB für 8010 <br></br> 64 TB für 8020 |8010 und 8020 sind virtuelle Geräte in Azure, die Standard und Premium-Speicher bzw. verwenden. |
| Maximale Größe für ein lokales Volume auf physischen Geräten |8.5 TB für 8100 <br></br> 22,5 TB für 8600 |8100 und 8600 sind physische Geräte. |
| Maximale Anzahl von iSCSI-Verbindungen |512 | |
| Maximale Anzahl von iSCSI-Verbindungen von Initiatoren |512 | |
| Maximale Anzahl von zugriffssteuerungsdatensätzen pro Gerät |64 | |
| Maximale Anzahl von Volumes pro Sicherungsrichtlinie |20 | |
| Maximale Anzahl von Sicherungen, die gemäß Zeitplan (in einer Sicherungsrichtlinie) beibehalten |64 | |
| Maximale Anzahl von Zeitplänen pro Sicherungsrichtlinie |10 | |
| Maximale Anzahl von Momentaufnahmen eines beliebigen Typs, die pro Volume beibehalten werden können |256 |Diese Zahl umfasst lokale Momentaufnahmen und cloud-Momentaufnahmen. |
| Maximale Anzahl von Momentaufnahmen, die in einem beliebigen Gerät vorhanden sein können |10,000 | |
| Maximale Anzahl von Volumes, die parallel für Sicherung, Wiederherstellung verarbeitet werden können, oder Klonen |16 |<ul><li>Wenn mehr als 16 Volumes vorhanden sind, werden sie nacheinander verarbeitet, wenn verarbeitungsslots verfügbar werden.</li><li>Neue Sicherungen eines geklonten oder wiederhergestellten mehrstufigen Volumes kann nicht erfolgen, bis der Vorgang abgeschlossen ist. Allerdings sind für ein lokales Volume Sicherungen zulässig, nachdem das Volume online ist.</li></ul> |
| Wiederherstellen und Klonen von Wiederherstellungszeit bei mehrstufigen volumes |< 2 Minuten |<ul><li>Das Volume wird innerhalb von zwei Minuten nach Wiederherstellungs- oder Klonvorgang Vorgang unabhängig von der Größe des Volumes zur Verfügung gestellt.</li><li>Das Volume Leistung anfänglich ist möglicherweise langsamer als normal, da die meisten Daten und Metadaten noch in der Cloud befinden. Als Datenflüsse aus der Cloud zum StorSimple-Gerät kann die Leistung erhöhen.</li><li>Die Gesamtzeit zum Herunterladen der Metadaten hängt von der zugewiesenen Volumegröße ab. Metadaten wird automatisch in das Gerät im Hintergrund mit einer Rate von 5 Minuten pro TB zugewiesener Volumedaten übertragen. Dieser Wert kann durch die Internetbandbreite zur Cloud beeinflusst werden.</li><li>Der Wiederherstellungs- oder Klonvorgang ist abgeschlossen, wenn alle Metadaten auf dem Gerät ist.</li><li>Sicherungsvorgänge können nicht ausgeführt werden, bis die Wiederherstellung oder Klonen vollständig abgeschlossen werden. |
| Stellen Sie die Wiederherstellungszeit bei lokalen Volumes wieder her. |< 2 Minuten |<ul><li>Das Volume wird innerhalb von zwei Minuten des Wiederherstellungsvorgangs, unabhängig von der Größe des Volumes zur Verfügung gestellt.</li><li>Das Volume Leistung anfänglich ist möglicherweise langsamer als normal, da die meisten Daten und Metadaten noch in der Cloud befinden. Als Datenflüsse aus der Cloud zum StorSimple-Gerät kann die Leistung erhöhen.</li><li>Die Gesamtzeit zum Herunterladen der Metadaten hängt von der zugewiesenen Volumegröße ab. Metadaten wird automatisch in das Gerät im Hintergrund mit einer Rate von 5 Minuten pro TB zugewiesener Volumedaten übertragen. Dieser Wert kann durch die Internetbandbreite zur Cloud beeinflusst werden.</li><li>Im Gegensatz zur mehrstufigen Volumes bei lokalen Volumes werden der Volumedaten auch lokal auf dem Gerät heruntergeladen. Der Restore-Vorgang ist abgeschlossen, wenn alle Daten des Datenträgers wurde auf dem Gerät versetzt.</li><li>Die Wiederherstellungsvorgänge möglicherweise lange dauern. Die Gesamtzeit zum Abschließen des Wiederherstellungsvorgangs hängt von der Größe des bereitgestellten lokalen Volumes, die Internet-Bandbreite und die vorhandenen Daten auf dem Gerät. Sicherungsvorgänge für das lokale Volume sind zulässig, während der Wiederherstellungsvorgang ausgeführt wird. |
| Verarbeitungsrate für Cloud-Momentaufnahmen |15 Minuten/TB |<ul><li>Minimale Zeit, zu der Cloud "snapshot" bereit für den Upload pro TB zugewiesener Volumedaten in der Sicherung. </li><li> Insgesamt Cloud momentaufnahmezeit wird berechnet, indem Sie dieses Mal an die momentaufnahmezeit hochladen, die durch die Internetbandbreite zur cloud beeinträchtigt wird. |
| Maximaler Client Lese-/Schreibzugriff Durchsatz (wenn von der SSD-Ebene aus bereitgestellt) * |920/720 MB/s mit einer einzelnen 10-GbE-Netzwerkschnittstelle |Bis zu 2 X mit MPIO und zwei Netzwerkschnittstellen. |
| Maximaler Client Lese-/Schreibzugriff Durchsatz (wenn von HDD-Ebene aus bereitgestellt) * |120/250 MB/s | |
| Maximaler Client Lese-/Schreibzugriff Durchsatz (wenn von der cloudebene aus bereitgestellt) * für Update 3 oder höher. ** |40/60 MB/s für Ebenen Volumes<br><br>60/80 MB/s für Ebenen Volumes mit Archivierungsfunktion während der Volumeerstellung aktiviert |Lesedurchsatz hängt von Clients generieren und verwalten müssen genügend e/a-Warteschlangentiefe ab. <br><br>Geschwindigkeit erreicht, hängt von der Geschwindigkeit des zugrunde liegenden Storage-Kontos verwendet. |

&#42; Der maximale Durchsatz pro e/a-Typ wurde mit 100 %-Lese- und 100 %-schreibszenarien gemessen. Tatsächliche Durchsatz kann geringer sein und hängt von e/a-Downmix und netzwerkbedingungen.

&#42; &#42; LEISTUNGSANGABEN vor Update 3 können geringer sein.

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die [StorSimple-Systemanforderungen](storsimple-8000-system-requirements.md).

