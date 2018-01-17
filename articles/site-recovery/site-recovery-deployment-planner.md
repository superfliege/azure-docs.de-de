---
title: "Azure Site Recovery Deployment Planner für „VMware zu Azure“ | Microsoft-Dokumentation"
description: Dies ist ein Leitfaden zum Azure Site Recovery Deployment Planner.
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/04/2017
ms.author: nisoneji
ms.openlocfilehash: 2985ed0b4bf5d9525bc2274d71b703922524f5a8
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2018
---
# <a name="azure-site-recovery-deployment-planner-for-vmware-to-azure"></a>Azure Site Recovery-Bereitstellungsplaner für „VMware zu Azure“
Dieser Artikel ist der Leitfaden zum Deployment Planner (Bereitstellungsplaner) von Azure Site Recovery für Bereitstellungen von „VMware zu Azure“ in der Produktion.

## <a name="overview"></a>Übersicht

Bevor Sie mit dem Schützen von virtuellen VMware-Computern (VMs) mit Site Recovery beginnen, ist es ratsam, basierend auf Ihrer täglichen Datenänderungsrate genügend Bandbreite zuzuordnen, um den gewünschten RPO-Wert (Recovery Point Objective) zu erzielen. Achten Sie darauf, dass Sie lokal die richtige Anzahl von Konfigurationsservern und Prozessservern bereitstellen.

Außerdem ist es erforderlich, den richtigen Typ und die richtige Anzahl von Azure-Zielspeicherkonten zu erstellen. Sie erstellen entweder Standard- oder Storage Premium-Konten und berücksichtigen dabei für Ihre Quellproduktionsserver den Zuwachs, der sich im Laufe der Zeit durch eine vermehrte Nutzung ergeben kann. Sie wählen den Speichertyp pro VM basierend auf den Workloadmerkmalen (z.B. Lese/Schreib-E/A-Vorgänge pro Sekunde [IOPS] oder Datenänderung) und den Site Recovery-Grenzwerten aus.

Der Azure Site Recovery-Bereitstellungsplaner ist ein Befehlszeilentool für Notfallwiederherstellungsszenarien für „Hyper-V zu Azure“ und „VMware zu Azure“. Sie können mit diesem Tool per Remoteverbindung ein Profil für Ihre virtuellen VMware-Computer erstellen (ohne jegliche Auswirkung auf die Produktion), um die Anforderungen an die Bandbreite und Azure Storage zu ermitteln und für einen erfolgreichen Ablauf der Replikation und des Testfailovers zu sorgen. Das Tool kann ausgeführt werden, ohne dass lokal Site Recovery-Komponenten installiert werden müssen. Damit Sie ein genaues Ergebnis für den erzielten Durchsatz erhalten, empfehlen wir Ihnen Folgendes: Führen Sie den Planner auf einem Windows Server aus, für den die Mindestanforderungen des Site Recovery-Konfigurationsservers erfüllt werden, den Sie später in einem der ersten Schritte für die Produktion bereitstellen.

Das Tool umfasst die folgenden Details:

**Kompatibilitätsbewertung**

* Bewertung der Eignung von virtuellen Computern basierend auf Datenträgeranzahl, Datenträgergröße, IOPS, Änderungsrate, Starttyp (EFI/BIOS) und Betriebssystemversion
 
**Vergleich von erforderlicher Netzwerkbandbreite und RPO-Bewertung**

* Geschätzte erforderliche Netzwerkbandbreite für die Deltareplikation
* Durchsatz für Site Recovery vom lokalen Standort zu Azure
* Anzahl von VMs für die Batcherstellung basierend auf der geschätzten Bandbreite zur Durchführung der ersten Replikation innerhalb eines bestimmten Zeitraums
* RPO, die für eine bestimmte Bandbreite erzielt werden kann
* Auswirkung auf die gewünschte RPO, wenn eine niedrigere Bandbreite bereitgestellt wird

**Azure-Infrastrukturanforderungen**

* Speichertypanforderung (Standard- oder Storage Premium-Konto) für jede VM
* Gesamtzahl von Standard- und Storage Premium-Konten zur Einrichtung für die Replikation
* Vorschläge für die Benennung von Speicherkonten gemäß Azure Storage-Vorgabe
* Anordnung von Speicherkonten für alle VMs
* Anzahl von einzurichtenden Azure-Kernen vor dem Testfailover oder Failover für das Abonnement
* Empfohlene Azure-VM-Größe für jede lokale VM

**Anforderungen an die lokale Infrastruktur**
* Erforderliche Anzahl von Konfigurationsservern und Prozessservern für die lokale Bereitstellung

**Geschätzte Kosten für Notfallwiederherstellung in Azure**
* Geschätzte Gesamtkosten für Notfallwiederherstellung in Azure: Compute-, Speicher-, Netzwerk- und Azure Site Recovery-Lizenzkosten
* Ausführliche Kostenanalyse pro VM


>[!IMPORTANT]
>
>Da die Nutzung m Laufe der Zeit voraussichtlich zunehmen wird, werden alle vorherigen Toolberechnungen mit einem Zuwachsfaktor von 30 Prozent in Bezug auf die Workloadmerkmale durchgeführt, und für alle Profilerstellungsmetriken (Lese/Schreib-IOPS, Datenänderung usw.) wird das 95. Perzentil verwendet. Beide Elemente (für die Berechnung des Zuwachsfaktors und des Perzentils) sind konfigurierbar. Weitere Informationen zum Zuwachsfaktor finden Sie im Abschnitt „Informationen zum Zuwachsfaktor“. Weitere Informationen zum Perzentilwert finden Sie im Abschnitt „Für die Berechnung verwendeter Perzentilwert“.
>

## <a name="support-matrix"></a>Unterstützungsmatrix

| | **VMware zu Azure** |**Hyper-V in Azure**|**Azure zu Azure**|**Hyper-V zum sekundären Standort**|**VMware zum sekundären Standort**
--|--|--|--|--|--
Unterstützte Szenarien |Ja|Ja|Nein |Ja*|Nein 
Unterstützte Version | vCenter 6.5, 6.0 oder 5.5| Windows Server 2016, Windows Server 2012 R2 | Nicht verfügbar |Windows Server 2016, Windows Server 2012 R2|Nicht verfügbar
Unterstützte Konfiguration|vCenter, ESXi| Hyper-V-Cluster, Hyper-V-Host|Nicht verfügbar|Hyper-V-Cluster, Hyper-V-Host|Nicht verfügbar|
Anzahl von Servern, für die pro ausgeführter Azure Site Recovery-Bereitstellungsplaner-Instanz die Profilerstellung durchgeführt werden kann |Einzeln (für VMs, die zu einem vCenter Server oder einem ESXi-Server gehören, kann die Profilerstellung auf einmal durchgeführt werden)|Mehrere (für VMs mehrerer Hosts oder Hostcluster kann die Profilerstellung auf einmal durchgeführt werden)| Nicht verfügbar |Mehrere (für VMs mehrerer Hosts oder Hostcluster kann die Profilerstellung auf einmal durchgeführt werden)| Nicht verfügbar

*Das Tool ist hauptsächlich für das Notfallwiederherstellungsszenario „Hyper-V zu Azure“ bestimmt. Für die Notfallwiederherstellung vom Typ „Hyper-V zum sekundären Standort“ kann es nur zum besseren Verständnis von quellseitigen Empfehlungen eingesetzt werden, z.B. erforderliche Netzwerkbandbreite, erforderlicher freier Speicherplatz auf den einzelnen Hyper-V-Quellservern und Batchverarbeitungszahlen und Batchdefinitionen der ersten Replikation.  Ignorieren Sie die Azure-Empfehlungen und Kosten im Bericht. Außerdem gilt der Vorgang „Durchsatzberechnung“ nicht für das Notfallwiederherstellungsszenario „Hyper-V zu sekundärem Standort“.

## <a name="prerequisites"></a>Voraussetzungen
Das Tool verfügt über zwei Hauptphasen: die Profilerstellung und die Berichterstellung. Es gibt auch noch eine dritte Option, mit der nur der Durchsatz berechnet werden kann. Die Anforderungen für den Server, über den die Profilerstellung und Durchsatzmessung initiiert werden, sind in der folgenden Tabelle aufgeführt:

| Serveranforderung | BESCHREIBUNG|
|---|---|
|Profilerstellung und Messung des Durchsatzes| <ul><li>Betriebssystem: Microsoft Windows Server 2016 oder Microsoft Windows Server 2012 R2<br>(idealerweise mindestens basierend auf [Empfohlene Größen für den Konfigurationsserver](https://aka.ms/asr-v2a-on-prem-components))</li><li>Computerkonfiguration: 8 vCPUs, 16 GB RAM, 300 GB HDD</li><li>[Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)</li><li>[VMware vSphere PowerCLI 6.0 R3](https://aka.ms/download_powercli)</li><li>[Microsoft Visual C++ Redistributable für Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)</li><li>Zugriff auf Azure über das Internet von diesem Server</li><li>Azure-Speicherkonto</li><li>Administratorzugriff auf dem Server</li><li>Mindestens 100 GB freier Speicherplatz (bei 1.000 VMs mit durchschnittlich drei Datenträgern, Profilerstellung über 30 Tage)</li><li>Die Einstellungen für die VMware vCenter-Statistikebene sollten auf „2“ oder „Höchste Stufe“ festgelegt werden.</li><li>Port 443 zulassen: Der ASR-Bereitstellungsplaner verwendet diesen Port zum Verbinden mit vCenter Server/ESXi-Host.</ul></ul>|
| Berichterstellung | Windows-PC oder Windows-Server mit Microsoft Excel 2013 oder höher |
| Benutzerberechtigungen | Leseberechtigung für das Benutzerkonto, das zum Zugreifen auf den VMware vCenter-Server/VMware vSphere ESXi-Host während der Profilerstellung verwendet wird |

> [!NOTE]
>
>Mit dem Tool können nur Profile für VMs mit VMDK- und RDM-Datenträgern erstellt werden. Die Profilerstellung für VMs mit iSCSI- oder NFS-Datenträgern ist nicht möglich. Site Recovery unterstützt zwar iSCSI- und NFS-Datenträger für VMware-Server, aber da sich der Deployment Planner nicht auf dem Gast befindet und die Profilerstellung nur mithilfe von vCenter-Leistungsindikatoren durchgeführt wird, hat das Tool keinen Einblick in diese Datenträgertypen.
>

## <a name="download-and-extract-the-deployment-planner-tool"></a>Herunterladen und Extrahieren des Bereitstellungsplaner-Tools
1. [Laden Sie die aktuelle Version des Azure Site Recovery Deployment Planner herunter.](https://aka.ms/asr-deployment-planner)  
Das Tool ist in einem ZIP-Ordner enthalten. Die aktuelle Version des Tools unterstützt nur das Szenario „VMware zu Azure“.

2. Kopieren Sie den ZIP-Ordner auf den Windows-Server, auf dem Sie das Tool ausführen möchten.  
Sie können das Tool unter Windows Server 2012 R2 ausführen, wenn der Server Netzwerkzugriff hat, um eine Verbindung mit dem vCenter-Server/vSphere ESXi-Host herzustellen, der die VMs für die Profilerstellung enthält. Wir empfehlen Ihnen aber, das Tool auf einem Server auszuführen, dessen Hardwarekonfiguration die Anforderungen der [Größenrichtlinie für Konfigurationsserver](https://aka.ms/asr-v2a-on-prem-components) erfüllt. Führen Sie das Tool auf dem Konfigurationsserver aus, falls Sie Site Recovery-Komponenten bereits lokal bereitgestellt haben.

 Es wird empfohlen, auf dem Server, auf dem Sie das Tool ausführen, die gleiche Hardwarekonfiguration wie auf dem Konfigurationsserver (der über einen integrierten Prozessserver verfügt) zu verwenden. Mit dieser Konfiguration wird sichergestellt, das der vom Tool gemeldete erzielte Durchsatz mit dem tatsächlichen Durchsatz übereinstimmt, den Site Recovery während der Replikation erreichen kann. Die Berechnung des Durchsatzes richtet sich nach der verfügbaren Netzwerkbandbreite auf dem Server und der Hardwarekonfiguration (CPU, Speicher usw.) des Servers. Wenn Sie das Tool auf einem anderen Server ausführen, wird der Durchsatz von diesem Server zu Microsoft Azure berechnet. Da sich die Hardwarekonfiguration des Servers von der des Konfigurationsservers unterscheiden kann, ist es auch möglich, dass der vom Tool gemeldete Durchsatz ungenau ist.

3. Extrahieren Sie den ZIP-Ordner.  
Der Ordner enthält mehrere Dateien und Unterordner. Die ausführbare Datei ist die Datei „ASRDeploymentPlanner.exe“ im übergeordneten Ordner.

    Beispiel:  
    Kopieren Sie die ZIP-Datei auf das Laufwerk „E:\“, und extrahieren Sie sie.
   E:\ASR Deployment Planner_v2.1zip

    E:\ASR Deployment Planner_v2.1\ASRDeploymentPlanner.exe

### <a name="updating-to-the-latest-version-of-deployment-planner"></a>Aktualisieren auf die neueste Version des Bereitstellungsplaners
Wählen Sie eine der folgenden Vorgehensweisen, wenn Sie über eine vorherige Version des Bereitstellungsplaners verfügen:
 * Falls die aktuelle Version keine Fehlerbehebung für die Profilerstellung enthält und die Profilerstellung bereits mit der aktuellen Planner-Version ausgeführt wird, ist es ratsam, die Profilerstellung fortzusetzen.
 * Wenn die aktuelle Version eine Fehlerbehebung für die Profilerstellung enthält, empfehlen wir Ihnen, die Profilerstellung für die aktuelle Version zu beenden und mit der neuen Version neu zu starten.


 >[!NOTE]
 >
 >Übergeben Sie beim Starten der Profilerstellung mit der neuen Version den gleichen Pfad des Ausgabeverzeichnisses, damit die Profildaten vom Tool an die vorhandenen Dateien angefügt werden. Zum Erstellen des Berichts wird ein vollständiger Satz von Profilerstellungsdaten verwendet. Wenn Sie ein anderes Ausgabeverzeichnis übergeben, werden neue Dateien erstellt und alte Profilerstellungsdaten nicht zum Erstellen des Berichts verwendet.
 >
 >Jeder neue Deployment Planner ist ein kumulatives Update der ZIP-Datei. Es ist nicht erforderlich, die neuesten Dateien in den vorherigen Ordner zu kopieren. Sie können einen neuen Ordner erstellen und verwenden.


## <a name="version-history"></a>Versionsverlauf
Die aktuelle Version des Tools ASR-Bereitstellungsplaner ist 2.1.
Auf der Seite [ASR Deployment Planner Version History](https://social.technet.microsoft.com/wiki/contents/articles/51049.asr-deployment-planner-version-history.aspx) (ASR-Bereitstellungsplaner – Versionsverlauf) finden Sie Informationen zu den Fehlerbehebungen, die in den einzelnen Updates hinzugefügt werden.

## <a name="next-steps"></a>Nächste Schritte
* [Ausführen des Bereitstellungsplaners](site-recovery-vmware-deployment-planner-run.md).
