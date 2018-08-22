---
title: Includedatei
description: Includedatei
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 08/14/2018
ms.author: cynthn;kareni
ms.custom: include file
ms.openlocfilehash: 130cc66831b25621cb022eb19005c624fcd71b9e
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2018
ms.locfileid: "40105505"
---
**Letzte Aktualisierung des Dokuments**: 14. August 2018 10:00 AM PST.

Eine öffentlich gemachte [neue Art von CPU-Sicherheitsrisiken](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002), die als „Seitenkanalangriffe mit spekulativer Ausführung“ bezeichnet wird, hatte Fragen von Kunden zur Folge, die sich mehr Klarheit wünschen.  

Microsoft hat Maßnahmen zur Minderung dieser Risiken für alle Clouddienste bereitgestellt. Die Infrastruktur, in der Azure ausgeführt und Kundenworkloads voneinander isoliert werden, ist geschützt. Dies bedeutet, dass ein potenzieller Angreifer, der dieselbe Infrastruktur nutzt, Ihre Anwendung über diese Sicherheitsrisiken nicht angreifen kann.

In Azure wird nach Möglichkeit die [Wartung mit Speicherbeibehaltung](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates#memory-preserving-maintenance) genutzt, um die Auswirkungen für Kunden möglichst gering zu halten und auf Neustarts verzichten zu können. In Azure werden diese Methoden eingesetzt, wenn systemweite Updates am Host vorgenommen werden, um unsere Kunden zu schützen.

Weitere Informationen dazu, wie die Sicherheit in alle Bereiche von Azure integriert ist, finden Sie auf der Website mit der [Dokumentation zur Azure-Sicherheit](https://docs.microsoft.com/azure/security/). 

> [!NOTE] 
> Seit der ersten Veröffentlichung dieses Dokuments wurden mehrere Varianten dieser Sicherheitsrisikoklasse öffentlich gemacht. Microsoft investiert weiterhin stark in den Schutz seiner Kunden und in die Bereitstellung von Anleitungen. Diese Seite wird aktualisiert, wenn weitere Fehlerbehebungen veröffentlicht werden. 
> 
> Am 14. August 2018 wurde in der Branche ein neues Sicherheitsrisiko vom Typ „Seitenkanalangriffe mit spekulativer Ausführung“ öffentlich gemacht, das als [L1 Terminal Fault](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180018) (L1TF) bezeichnet wird und dem mehrere CVEs ([CVE-2018-3615, CVE-2018-3620 und CVE-2018-3646](https://www.intel.com/content/www/us/en/security-center/advisory/intel-sa-00161.html)) zugewiesen wurden. Dieses Sicherheitsrisiko betrifft Intel® Core®-Prozessoren und Intel® Xeon®-Prozessoren. Microsoft hat Maßnahmen zur Risikominderung für seine gesamten Clouddienste bereitgestellt, um die Isolation zwischen Kunden zu stärken. Unten finden Sie eine weitere Anleitung zum Schutz vor L1TF und früheren Sicherheitsrisiken ([Spectre-Variante 2 CVE-2017-5715 und Meltdown-Variante 3 CVE-2017-5754](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution)).
>  






## <a name="keeping-your-operating-systems-up-to-date"></a>Halten Sie Ihre Betriebssysteme auf dem neuesten Stand

Ein Betriebssystemupdate ist zwar nicht erforderlich, um Ihre in Azure ausgeführten Anwendungen gegenüber anderen Azure-Kunden zu isolieren, aber es gilt immer als bewährte Methode, die Software auf dem neuesten Stand zu halten. Die aktuellen Sicherheitsrollups für Windows enthalten Maßnahmen zur Risikominderung für mehrere Sicherheitsrisiken vom Typ „Seitenkanalangriffe mit spekulativer Ausführung“. Auch für Linux-Distributionen wurde mehrere Updates zu diesen Sicherheitsrisiken veröffentlicht. Hier sind unsere empfohlenen Maßnahmen zur Aktualisierung Ihres Betriebssystems aufgeführt:

| Angebot | Empfohlene Maßnahme  |
|----------|---------------------|
| Azure Cloud Services  | Aktivieren Sie [automatische Updates](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-configure-portal), oder stellen Sie sicher, dass das neueste Gastbetriebssystem ausgeführt wird. |
| Virtuelle Azure Linux-Computer | Installieren Sie Updates vom Anbieter Ihres Betriebssystems. Weitere Informationen finden Sie unter [Linux](#linux) in diesem Dokument. |
| Virtuelle Azure Windows-Computer  | Installieren Sie das aktuelle Sicherheitsrollup.
| Weitere Azure PaaS-Dienste | Für Kunden, die diese Dienste verwenden, sind keine Maßnahmen erforderlich. Azure hält Ihre Betriebssystemversionen automatisch auf dem neuesten Stand. |

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Zusätzliche Anleitungen für den Fall, dass nicht vertrauenswürdiger Code ausgeführt wird 

Für Kunden, die für nicht vertrauenswürdige Benutzer die Ausführung von beliebigem Code zulassen, kann es ratsam sein, für ihre Azure Virtual Machines oder Cloud Services zusätzliche Sicherheitsfunktionen zu implementieren. Diese Features bieten Schutz vor den prozessinternen Offenlegungsvektoren, mit denen mehrere Sicherheitsrisiken vom Typ „Spekulative Ausführung“ beschrieben werden.

Beispielszenarien, für die zusätzliche Sicherheitsfunktionen empfohlen werden:

- Sie lassen zu, dass Code, den Sie nicht als vertrauenswürdig ansehen, auf Ihrer VM ausgeführt wird.  
    - *Beispielsweise lassen Sie für einen Ihrer Kunden das Hochladen einer Binärdatei oder eines Skripts zu, die bzw. das Sie dann in Ihrer Anwendung ausführen*. 
- Sie lassen für Benutzer, die Sie nicht als vertrauenswürdig ansehen, das Anmelden an Ihrer VM mit Konten mit geringen Rechten zu.   
    - *Beispielsweise lassen Sie zu, dass sich ein Benutzer mit geringen Rechten an einer Ihrer VMs per Remotedesktop- oder SSH-Verbindung anmeldet*.  
- Sie lassen für nicht vertrauenswürdige Benutzer den Zugriff auf virtuelle Computer zu, die über die geschachtelte Virtualisierung implementiert werden.  
    - *Beispielsweise steuern Sie den Hyper-V-Host, aber ordnen die VMs nicht vertrauenswürdigen Benutzern zu*. 

Kunden, die kein Szenario mit nicht vertrauenswürdigem Code implementieren, müssen diese zusätzlichen Sicherheitsfunktionen nicht aktivieren. 

## <a name="enabling-additional-security"></a>Aktivieren von zusätzlicher Sicherheit 

Sie können auf Ihrer VM oder in Ihrem Clouddienst zusätzliche Sicherheitsfunktionen aktivieren.

### <a name="windows"></a>Windows 

Ihr Zielbetriebssystem muss aktuell sein, damit Sie diese zusätzlichen Sicherheitsfunktionen aktivieren können. Für „Seitenkanalangriffe mit spekulativer Ausführung“ sind standardmäßig mehrere Maßnahmen zur Risikominderung aktiviert, aber die hier beschriebenen zusätzlichen Features müssen manuell aktiviert werden und können sich auf die Leistung auswirken. 

**Schritt 1**: [Wenden Sie sich an den Azure-Support](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical), um aktualisierte Firmware (Microcode) auf Ihren virtuellen Computern verfügbar zu machen. 

**Schritt 2**: Aktivieren Sie Kernel Virtual Address Shadowing (KVAS) und Branch Target Injection (BTI) zur Unterstützung des Betriebssystems. Befolgen Sie die Anleitung unter [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution), um den Schutz über die `Session Manager`-Registrierungsschlüssel zu aktivieren. Es ist ein Neustart erforderlich. 

**Schritt 3**: Für Bereitstellungen mit Verwendung von [geschachtelter Virtualisierung](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization) (nur D3 und E3): Diese Anleitung gilt auf der VM, die Sie als Hyper-V-Host verwenden. 

1. Befolgen Sie die Anleitung unter [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution), um den Schutz über die `MinVmVersionForCpuBasedMitigations`-Registrierungsschlüssel zu aktivieren.  
 
1. Legen Sie den Typ für den Hypervisorplaner auf **Core** fest, indem Sie die [hier](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) angegebene Anleitung befolgen. 

**Schritt 4**: Befolgen Sie die Anleitung unter [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution), um mit dem [SpeculationControl](https://aka.ms/SpeculationControlPS)-PowerShell-Modul sicherzustellen, dass der Schutz aktiviert ist. 

> [!NOTE]
> Wenn Sie dieses Modul bereits zu einem früheren Zeitpunkt heruntergeladen haben, müssen Sie die neueste Version installieren.
>

Es sollten alle virtuellen Computer angezeigt werden:

```
branch target injection mitigation is enabled: True

kernel VA shadow is enabled: True  

L1TFWindowsSupportEnabled: True
```


### <a name="linux"></a>Linux

<a name="linux"></a>Für die Aktivierung der internen zusätzlichen Sicherheitsfunktionen ist es erforderlich, dass das Zielbetriebssystem auf dem aktuellsten Stand ist. Einige Maßnahmen zur Risikominderung sind standardmäßig aktiviert. Im folgenden Abschnitt werden die Funktionen beschrieben, die standardmäßig deaktiviert sind bzw. für die eine Hardwareunterstützung (Microcode) benötigt wird. Die Aktivierung dieser Funktionen kann zu einer Beeinträchtigung der Leistung führen. Weitere Informationen finden Sie in der Dokumentation des Anbieters Ihres Betriebssystems.
 
**Schritt 1**: [Wenden Sie sich an den Azure-Support](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical), um aktualisierte Firmware (Microcode) auf Ihren virtuellen Computern verfügbar zu machen.
 
**Schritt 2**: Aktivieren Sie Branch Target Injection (BTI) zur Unterstützung des Betriebssystems, um CVE-2017-5715 (Spectre-Variante 2) zu begegnen, indem Sie die Anweisungen in der Dokumentation des Anbieters Ihres Betriebssystems befolgen. 
 
**Schritt 3**: Aktivieren Sie Kernel Page Table Isolation (KPTI), um CVE-2017-5754 (Meltdown-Variante 3) zu begegnen, indem Sie die Anweisungen in der Dokumentation des Anbieters Ihres Betriebssystems befolgen. 
 
Weitere Informationen erhalten Sie beim Anbieter Ihres Betriebssystems:  
 
- [RedHat und CentOS](https://access.redhat.com/security/vulnerabilities/speculativeexecution) 
- [SUSE](https://www.suse.com/support/kb/doc/?id=7022512) 
- [Ubuntu](https://wiki.ubuntu.com/SecurityTeam/KnowledgeBase/SpectreAndMeltdown) 


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Schützen von Azure-Kunden vor CPU-Sicherheitsrisiken](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
