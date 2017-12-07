---
title: Bewertungsberechnungen in Azure Migrate | Microsoft-Dokumentation
description: "Bietet eine Übersicht über Bewertungsberechnungen im Azure Migrate-Dienst."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 39a63769-31eb-49f9-9089-4d3e4e88a412
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: f00825ff9a5018e67672ce452f01130f84919e52
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2017
---
# <a name="assessment-calculations"></a>Bewertungsberechnungen

[Azure Migrate](migrate-overview.md) bewertet lokale Workloads für die Migration zu Azure. Dieser Artikel enthält Informationen dazu, wie Bewertungen berechnet werden.



## <a name="overview"></a>Übersicht

Eine Azure Migrate-Bewertung erfolgt in drei Phasen. Die Bewertung beginnt mit einer Eignungsanalyse, gefolgt von leistungsbasierten Größenschätzungen und abschließend einer Schätzung der monatlichen Kosten. Ein Computer gelangt nur in die nächste Phase, wenn er die vorherige besteht. Beispiel: Besteht ein Computer die Azure-Eignungsprüfung nicht, wird er als ungeeignet für Azure markiert, und die Größen- und Kostenschätzungen werden nicht berechnet. 


## <a name="azure-suitability-analysis"></a>Azure-Eignungsanalyse

Computer, die Sie zu Azure migrieren möchten, müssen Azure-Anforderungen und Einschränkungen erfüllen. Wenn ein lokaler virtueller VM-Datenträger beispielsweise größer als 4 TB ist, kann er nicht in Azure gehostet werden. Die Azure-Konformitätsüberprüfungen sind in der folgenden Tabelle zusammengefasst. 

**Überprüfung** | **Details**
--- | ---
**Starttyp** | Der Starttyp des Gastbetriebssystems muss BIOS und nicht UEFI sein.
**Kerne** | Die Anzahl der Kerne in den Computer darf maximal der Anzahl der für einen virtuellen Azure-Computer unterstützten Kerne (32) entsprechen.<br/><br/> Wenn der Leistungsverlauf verfügbar ist, berücksichtigt Azure Migrate die genutzten Kerne beim Vergleich. Wenn in den Bewertungseinstellungen ein Komfortfaktor festgelegt ist, wird die Anzahl der genutzten Kerne mit dem Komfortfaktor multipliziert.<br/><br/> Wenn kein Leistungsverlauf vorhanden ist, verwendet Azure Migrate die zugeordneten Kerne ohne Anwendung des Komfortfaktors.
**Arbeitsspeicher** | Die Größe des Computerarbeitsspeichers darf maximal dem zulässigen Arbeitsspeicher (448 GB) für einen virtuellen Azure-Computer entsprechen. <br/><br/> Wenn der Leistungsverlauf verfügbar ist, berücksichtigt Azure Migrate den genutzten Arbeitsspeicher beim Vergleich. Wenn ein Komfortfaktor festgelegt ist, wird der genutzte Arbeitsspeicher mit dem Komfortfaktor multipliziert.<br/><br/> Wenn kein Verlauf vorhanden ist, wird der zugeordnete Arbeitsspeicher ohne Anwendung des Komfortfaktors verwendet.<br/><br/> 
**Windows Server 2003–2008 R2** | 32-Bit und 64-Bit-Unterstützung.<br/><br/> Azure bietet nur bestmögliche Unterstützung.
**Windows Server 2008 R2 mit allen SPs** | 64-Bit-Unterstützung.<br/><br/> Azure bietet vollständige Unterstützung.
**Windows Server 2012 und alle SPs** | 64-Bit-Unterstützung.<br/><br/> Azure bietet vollständige Unterstützung.
**Windows Server 2012 R2 und alle SPs** | 64-Bit-Unterstützung.<br/><br/> Azure bietet vollständige Unterstützung.
**Windows Server 2016 und alle SPs** | 64-Bit-Unterstützung.<br/><br/> Azure bietet vollständige Unterstützung.
**Windows Client 7 und höher** | 64-Bit-Unterstützung.<br/><br/> Azure bietet nur mit Visual Studio-Abonnement Unterstützung.
**Linux** | 64-Bit-Unterstützung.<br/><br/> Azure bietet vollständige Unterstützung für diese [Betriebssysteme](../virtual-machines/linux/endorsed-distros.md).
**Speicherdatenträger** | Die zugeteilte Größe eines Datenträgers darf höchstens 4 TB (4096 GB) betragen.<br/><br/> An den Computer dürfen einschließlich des Betriebssystem-Datenträgers höchstens 65 Datenträger angefügt sein. 
**Netzwerk** | An einen Computer dürfen höchstens 32 NICs angefügt sein.


## <a name="performance-based-sizing"></a>Leistungsbasierte Größenanpassung

Nachdem ein Computer als geeignet für Azure markiert wurde, wird er von Azure Migrate anhand der folgenden Kriterien einer VM-Größe in Azure zugeordnet:

- **Speicherüberprüfung**: Azure Migrate versucht, jeden an den Computer angefügten Datenträger einem Datenträger in Azure zuzuordnen: – Azure Migrate multipliziert die E/A-Vorgänge pro Sekunde (IOPS) mit dem Komfortfaktor. Außerdem wird der Durchsatz (in Mbit/s) der einzelnen Datenträger mit dem Komfortfaktor multipliziert. So wird für einen effektiven Datenträger-IOPS und -Durchsatz gesorgt. Auf dieser Grundlage ordnet Azure Migrate den Datenträger einem Standard- oder Premium-Datenträger in Azure zu.
    - Wenn der Dienst keinen Datenträger mit dem erforderlichen IOPS und Durchsatz finden kann, wird der Computer als ungeeignet für Azure gekennzeichnet.
    - Wenn mehrere geeignete Datenträger gefunden werden, wählt Azure Migrate diejenigen aus, die die Speicherredundanzmethode sowie den in den Bewertungseinstellungen festgelegten Speicherort unterstützen.
    - Falls mehrere geeignete Datenträger vorhanden sind, werden die mit den geringsten Kosten ausgewählt.
- **Durchsatz des Speicherdatenträgers**: [Erfahren Sie mehr](../azure-subscription-service-limits.md#storage-limits) über Azure-Grenzwerte pro Datenträger und virtuellen Computer.
- **Datenträgertyp**: Azure Migrate unterstützt nur verwaltete Datenträger.
- **Netzwerküberprüfung**: Azure Migrate versucht, einen virtuellen Azure-Computer zu finden, der die Anzahl von NICs auf dem lokalen Computer unterstützen kann.
    - Dazu werden die vom Computer pro Sekunde (Mbit/s) übertragenen Daten (Netzwerk ausgehend) für alle NICs aggregiert und der Komfortfaktor auf die Gesamtzahl angewendet. Mithilfe dieser Zahl wird ein virtueller Azure-Computer gesucht, der die erforderliche Netzwerkleistung unterstützen kann.
    - Azure Migrate verwendet die Netzwerkeinstellungen des virtuellen Computers und nimmt an, dass es sich um ein Netzwerk außerhalb des Rechenzentrums handelt.
    - Wenn keine Netzwerkleistungsdaten verfügbar sind, wird bei der Größenanpassung für virtuelle Computer nur die NIC-Anzahl berücksichtigt.
- **Computeüberprüfung**: Nach der Berechnung der Speicher- und Netzwerkanforderungen prüft Azure Migrate die Computeanforderungen:
    - Wenn die Leistungsdaten für den virtuellen Computer verfügbar sind, wird der Komfortfaktor auf die genutzten Kerne und den Arbeitsspeicher angewendet. Auf Grundlage dieser Zahl wird versucht, eine geeignete VM-Größe in Azure zu finden.
    - Wenn keine geeignete Größe gefunden wird, wird der Computer als ungeeignet für Azure gekennzeichnet.
    - Wenn eine geeignete Größe gefunden wird, wendet Azure Migrate die Speicher -und Netzwerkberechnungen an. Für die endgültige VM-Größenempfehlung werden dann noch Speicherort- und Tarifeinstellungen angewendet.


## <a name="monthly-cost-estimation"></a>Schätzung der monatlichen Kosten

Nach Abschluss der Größenempfehlungen berechnet Azure Migrate die Computer- und Speicherkosten nach der Migration.

- **Computekosten**: Azure Migrate berechnet anhand der empfohlenen Azure-VM-Größe mithilfe der Abrechnungs-API die monatlichen Kosten für den virtuellen Computer. Bei der Berechnung werden Betriebssystem-, Software Assurance-, Standort- und Währungseinstellungen berücksichtigt. Die Kosten aller Computer werden zusammengefasst, um die monatlichen Gesamtcomputekosten zu berechnen.
- **Speicherkosten**: Die monatlichen Speicherkosten für einen Computer werden berechnet, indem die monatlichen Kosten aller an den Computer angefügten Datenträger zusammengefasst werden. Azure Migrate berechnet die gesamten monatlichen Speicherkosten durch Aggregieren der Speicherkosten aller Computer. Derzeit werden bei der Berechnung keine Angebote berücksichtigt, die in den Bewertungseinstellungen angegeben sind.

Kosten werden in der Währung angezeigt, die in den Bewertungseinstellungen festgelegt ist. 


## <a name="next-steps"></a>Nächste Schritte

[Einrichten einer Bewertung für lokale virtuelle VMware-Computer](tutorial-assessment-vmware.md)
