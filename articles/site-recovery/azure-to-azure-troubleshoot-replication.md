---
title: Azure Site Recovery-Problembehandlung für Azure-zu-Azure-Replikationsprobleme und -fehler| Microsoft-Dokumentation
description: Problembehandlungsfehler und -probleme beim Replizieren von virtuellen Azure-Computern für die Notfallwiederherstellung
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/30/2018
ms.author: asgang
ms.openlocfilehash: 22ea3d955fe2910dc99ab4015165008da899d48e
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52312849"
---
# <a name="troubleshoot-azure-to-azure-vm-ongoing-replication-issues"></a>Behandlung von Problemen mit laufenden VM-Replikationen mit Azure als Quelle und Ziel

In diesem Artikel werden die Probleme beschrieben, die in Azure Site Recovery beim Replizieren und Wiederherstellen von virtuellen Azure-Computern aus einer Region in einer anderen Region häufiger auftreten können, und es wird erklärt, wie die Problembehandlung durchgeführt werden kann. Weitere Informationen zu unterstützten Konfigurationen finden Sie unter [Azure Site Recovery support matrix for replicating from Azure to Azure](site-recovery-support-matrix-azure-to-azure.md) (Azure Site Recovery-Supportmatrix zum Replizieren aus Azure in Azure).


## <a name="recovery-points-not-getting-generated"></a>Wiederherstellungspunkte nicht generiert

FEHLERMELDUNG: Für die VM sind keine absturzkonsistente Wiederherstellungspunkte für die VM in den letzten 60 Minuten verfügbar.</br>
FEHLER-ID: 153007 </br>

Azure Site Recovery repliziert Daten konsistent von der Quellregion zur Notfallwiederherstellungsregion und erstellt alle 5 Minuten einen absturzkonsistenten Wiederherstellungspunkt. Wenn Site Recovery 60 Minuten lang keine Wiederherstellungspunkte erstellen kann, wird der Benutzer benachrichtigt. Im Folgenden finden Sie die Ursachen, die zu diesem Fehler führen können:

**Ursache 1: [Hohe Datenänderungsrate auf dem virtuellen Quellcomputer](#high-data-change-rate-on-the-source-virtal-machine)**    
**Ursache 2: [Netzwerkkonnektivitätsproblem](#Network-connectivity-issue)**

## <a name="causes-and-solutions"></a>Ursachen und Lösungen

### <a name="high-data-change-rate-on-the-source-virtal-machine"></a>Hohe Datenänderungsrate auf dem virtuellen Quellcomputer
Azure Site Recovery löst ein Ereignis aus, wenn die Datenänderungsrate auf dem virtuellen Quellcomputer die unterstützten Grenzwerte übersteigt. Um festzustellen, ob das Problem auf eine hohe Änderungsrate zurückzuführen ist, navigieren Sie zu „Replizierte Elemente“ > „VM“, und klicken Sie auf „Ereignisse – letzte 72 Stunden“.
Das Ereignis „Die Datenänderungsrate überschreitet die unterstützten Grenzwerte.“ sollte angezeigt werden, wie im folgenden Screenshot gezeigt wird.

![data_change_rate_high](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event.png)

Wenn Sie auf das Ereignis klicken, sehen Sie die genauen Datenträgerinformationen, wie im folgenden Screenshot gezeigt wird.

![data_change_rate_event](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event2.png)


#### <a name="azure-site-recovery-limits"></a>Azure Site Recovery-Grenzwerte
Die folgende Tabelle enthält die Azure Site Recovery-Grenzwerte. Diese Grenzwerte basieren auf unseren Tests, können aber nicht alle möglichen E/A-Kombinationen für Anwendungen abdecken. Die tatsächlichen Ergebnisse können je nach Ihrer E/A-Mischung für die Anwendungen variieren. Außerdem sollte erwähnt werden, dass es zwei Grenzwerte zu beachten gilt, nämlich jene für Datenänderungen bei Datenträgern und jene für Datenänderungen bei virtuellen Computern.
Bei dem Premium P20-Datenträger beispielsweise in der folgenden Tabelle können Sie sehen, dass Site Recovery Datenänderungen pro Datenträger von 5 MB/s mit maximal fünf solcher Datenträger pro VM verarbeiten kann. Denn der Grenzwert für die Gesamtänderungsrate liegt bei 25 MB/s pro VM.

**Replikationsspeicherziel** | **Durchschnittliche E/A-Größe des Quelldatenträgers** |**Durchschnittliche Datenänderungsrate des Quelldatenträgers** | **Gesamte Datenänderungsrate des Quelldatenträgers pro Tag**
---|---|---|---
Standardspeicher | 8 KB | 2 MB/s | 168 GB pro Datenträger
Premium-Datenträger – P10 oder P15 | 8 KB  | 2 MB/s | 168 GB pro Datenträger
Premium-Datenträger – P10 oder P15 | 16 KB | 4 MB/s |  336 GB pro Datenträger
Premium-Datenträger – P10 oder P15 | 32 KB oder höher | 8 MB/s | 672 GB pro Datenträger
Premium-Datenträger – P20, P30, P40 oder P50 | 8 KB    | 5 MB/s | 421 GB pro Datenträger
Premium-Datenträger – P20, P30, P40 oder P50 | 16 KB oder höher |10 MB/s | 842 GB pro Datenträger

### <a name="solution"></a>Lösung
Hierbei gilt es zu beachten, dass für Azure Site Recovery Grenzwerte für die Datenänderungsrate basierend auf dem Typ des Datenträger gelten. Um herauszufinden, ob sich dieses Problem wiederholt oder ob es vorübergehend auftritt, ist es wichtig, das Muster der Datenänderungsrate des betroffenen virtuellen Computers zu ermitteln.
Um die Datenänderungsrate des betroffenen virtuellen Computers zu ermitteln, navigieren Sie unter „Überwachung“ zu dem virtuellen Quellcomputer und dann zu „Metriken“. Fügen Sie anschließend die Metriken hinzu, wie unten dargestellt.

![high_data_change_rate](./media/site-recovery-azure-to-azure-troubleshoot/churn.png)

1. Klicken Sie auf „Metrik hinzufügen“, und fügen Sie „Auf den Betriebssystemdatenträger geschriebene Bytes/s“ und „Auf den Datenträger geschriebene Bytes/s“ hinzu.
2. Überwachen Sie die im Screenshot dargestellte Spitze.
3. Darauf wird die Gesamtanzahl an Schreibvorgängen gezeigt, die auf Betriebssystemdatenträgern und allen Datenträgern in Kombination auftreten. Diese Metriken liefern zwar keine Informationen auf Datenträgerebene, stellen jedoch einen guten Indikator für das Muster der gesamten Datenänderungen dar.

Wenn wie in den oben genannten Fällen nur gelegentlich eine große Datenmenge anfällt und die Datenänderungsrate zeitweise über 10 MBit/s (bei Premium) und 2 MBit/s (bei Standard) liegt und dann zurückgeht, kann die Replikation folgen. Wenn die Änderung in den meisten Fällen jedoch weit über dem unterstützten Grenzwert liegt, sollten Sie wenn möglich eine der folgenden Optionen berücksichtigen:

**Option 1**: Schließen Sie den Datenträger aus, der eine hohe Datenänderungsrate verursacht: </br>
Derzeit können Sie den Datenträger mit [Site Recovery PowerShell](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine) ausschließen.

**Option 2:** Ändern Sie den Speicherdatenträgertarif für die Notfallwiederherstellung: </br>
Diese Option ist nur möglich, wenn die Datenänderungsrate weniger als 10 MB/s beträgt. Nehmen wir an, eine VM mit einem P10-Datenträger weist eine Datenänderungsrate zwischen 8 MB/s und 10 MB/s auf. Wenn ein Kunde den P30-Datenträger bei bestehendem Schutz als Zielspeicher verwenden kann, kann das Problem gelöst werden.

### <a name="Network-connectivity-issue"></a>Netzwerkkonnektivitätsproblem

#### <a name="network-latency-to-cache-storage-account-"></a>Netzwerklatenz im Cachespeicherkonto:
 Site Recovery sendet replizierte Daten an das Cachespeicherkonto. Das Problem kann auftreten, wenn die Daten vom virtuellen Computer langsamer als 4 MB/3 s in das Cachespeicherkonto hochgeladen werden. Verwenden Sie den Befehl [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) zum Hochladen von Daten vom virtuellen Computer in das Cachespeicherkonto, um zu überprüfen, ob ein Latenzproblem vorliegt.<br>
Wenn die Latenz hoch ist, überprüfen Sie, ob Sie ein virtuelles Netzwerkgerät zum Steuern des ausgehenden Netzwerkdatenverkehrs von virtuellen Computern verwenden. Das virtuelle Netzwerkgerät wird möglicherweise gedrosselt, wenn der gesamte Replikationsdatenverkehr durch das virtuelle Netzwerkgerät läuft. Es wird empfohlen, einen Netzwerk-Dienstendpunkt in Ihrem virtuellen Netzwerk für „Storage“ zu erstellen, damit der Replikationsdatenverkehr nicht an die virtuelle Netzwerkappliance geleitet wird. Informationen dazu finden Sie unter [Konfiguration der virtuellen Netzwerkappliance](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-about-networking#network-virtual-appliance-configuration).

#### <a name="network-connectivity"></a>Netzwerkverbindung
Damit die Site Recovery-Replikation funktioniert, ist für die VM die ausgehende Konnektivität zu bestimmten URLs oder IP-Bereichen erforderlich. Wenn sich Ihr virtueller Computer hinter einer Firewall befindet oder Netzwerksicherheitsgruppen-Regeln (NSG-Regeln) zum Steuern der ausgehenden Konnektivität verwendet werden, wird ggf. eine dieser Fehlermeldungen angezeigt.</br>
Informationen zum Sicherstellen, dass alle URLs verbunden sind, finden Sie unter [Ausgehende Konnektivität für Site Recovery-URLs](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges). 