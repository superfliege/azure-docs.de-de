---
title: Includedatei
description: Includedatei
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/21/2018
ms.author: cynthn;kareni
ms.custom: include file
ms.openlocfilehash: 49db6b625a9e4fc46fe414eb723dfccd890efd64
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34677358"
---
**Letzte Aktualisierung des Dokuments:** 21. Mai 2018, 15:00 Uhr PST.

Eine vor kurzem öffentlich gemachte [neue Art von CPU-Sicherheitsrisiken](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002), die als „Seitenkanalangriffe mit spekulativer Ausführung“ bezeichnet wird, hatte Fragen von Kunden zur Folge, sich mehr Klarheit wünschen.  

Microsoft hat Maßnahmen zur Minderung dieser Risiken für alle Clouddienste bereitgestellt. Die Infrastruktur, in der Azure ausgeführt und Kundenworkloads voneinander isoliert werden, ist geschützt.  Dies bedeutet, dass andere Kunden in Azure diese Sicherheitsrisiken nicht für Angriffe auf Ihre Anwendung ausnutzen können.

Darüber hinaus erweitert Azure die Nutzung der [Wartung mit Speicherbeibehaltung](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates#memory-preserving-maintenance), wo immer dies möglich ist. Damit wird ein virtueller Computer bis zu 30 Sekunden lang angehalten, während der Host aktualisiert oder der virtuelle Computer auf einen bereits aktualisierten Host verschoben wird.  Die Wartung mit Speicherbeibehaltung minimiert mögliche Auswirkungen für Kunden weiter und macht Neustarts überflüssig.  Azure nutzt diese Methoden bei der Durchführung von systemweiten Updates auf dem Host.

> [!NOTE] 
Am 21. Mai 2018 veröffentlichten Google Projekt Zero und Microsoft eine neue Unterklasse des Seitenkanal-Sicherheitsrisikos durch spekulative Ausführung, das als Speculative Store Bypass bekannt ist. Über die Microsoft-Cloudinfrastruktur wurden zusätzliche tief greifende Gegenmaßnahmen verteilt, die Sicherheitsrisiken durch spekulative Ausführung direkt behandeln. Weitere Informationen sind hier verfügbar: https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180012 
>
> Ende Februar 2018 veröffentlichte die Intel Corporation aktuelle [Microcode-Revisionsrichtlinien](https://newsroom.intel.com/wp-content/uploads/sites/11/2018/03/microcode-update-guidance.pdf) zum Status ihrer Microcodereleases, die die Stabilität verbessern und vor den vor Kurzem durch [Google Project Zero](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html) offengelegten Schwachstellen schützen. Die von Azure am [3. Januar 2018](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) eingerichteten Entschärfungen sind durch das Intel-Microcodeupdate nicht betroffen. Microsoft hat bereits eine umfassende Risikominderung zum Schutz von Azure-Kunden vor anderen virtuellen Azure-Computern eingerichtet.  
>
> Der Microcode von Intel betrifft Spectre-Variante 2 ([CVE-2017-5715](https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=2017-5715) oder Branch Target Injection) zum Schutz vor Angriffen. Dies betrifft Sie nur, wenn Sie freigegebene oder nicht vertrauenswürdige Workloads in Ihren virtuellen Computern in Azure ausführen. Unsere Techniker testen derzeit die Stabilität zur Minimierung von Leistungsauswirkungen auf den Microcode, bevor dieser Azure-Kunden zur Verfügung gestellt wird.  Da nur wenige Kunden nicht vertrauenswürdige Workloads auf ihren virtuellen Computern ausführen, müssen die meisten Kunden diese Funktion nach der Veröffentlichung nicht aktivieren. 
>
> Diese Seite wird aktualisiert, sobald weitere Informationen verfügbar sind.  






## <a name="keeping-your-operating-systems-up-to-date"></a>Halten Sie Ihre Betriebssysteme auf dem neuesten Stand

Ein Betriebssystemupdate ist zwar nicht erforderlich, um Ihre in Azure ausgeführten Anwendungen von anderen Kunden in Azure zu isolieren, es gilt jedoch immer als bewährte Methode, die Versionen von Betriebssystemen auf dem neuesten Stand zu halten. Die [Sicherheitsrollups für Windows](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) von Januar 2018 und später enthalten Maßnahmen zur Minderung dieser Sicherheitsrisiken.

Die folgenden Angebote enthalten unsere empfohlenen Aktionen zur Aktualisierung Ihres Betriebssystems: 

<table>
<tr>
<th>Angebot</th> <th>Empfohlene Maßnahme </th>
</tr>
<tr>
<td>Azure Cloud Services </td>  <td>Aktivieren Sie automatische Updates, oder stellen Sie sicher, dass das neueste Gastbetriebssystem ausgeführt wird.</td>
</tr>
<tr>
<td>Virtuelle Azure Linux-Computer</td> <td>Installieren Sie bei Verfügbarkeit Updates vom Anbieter Ihres Betriebssystems. </td>
</tr>
<tr>
<td>Virtuelle Azure Windows-Computer </td> <td>Stellen Sie vor dem Installieren von Betriebssystemupdates sicher, dass eine unterstützte Antivirenanwendung ausgeführt wird. Holen Sie Informationen zur Kompatibilität vom Anbieter der Antivirensoftware ein.<p> Installieren Sie das [Sicherheitsrollup von Januar](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002). </p></td>
</tr>
<tr>
<td>Weitere Azure PaaS-Dienste</td> <td>Für Kunden, die diese Dienste verwenden, sind keine Maßnahmen erforderlich. Azure hält Ihre Betriebssystemversionen automatisch auf dem neuesten Stand. </td>
</tr>
</table>

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Zusätzliche Anleitungen für den Fall, dass nicht vertrauenswürdiger Code ausgeführt wird 

Weitere Kundenaktionen sind nur erforderlich, wenn nicht vertrauenswürdiger Code ausgeführt wird. Wenn Sie Code zulassen, der nicht vertrauenswürdig ist (z.B. indem Sie einem Ihrer Kunden das Hochladen einer Binärdatei oder eines Codeausschnitts gewähren, die bzw. den Sie dann in der Cloud innerhalb Ihrer Anwendung ausführen), müssen die folgenden zusätzlichen Schritte ausgeführt werden.  


### <a name="windows"></a>Windows 
Wenn Sie Windows verwenden und nicht vertrauenswürdigen Code hosten, müssen Sie auch die Windows-Funktion zum Shadowing der virtuellen Kerneladresse aktivieren, die zusätzlichen Schutz vor Sicherheitsrisiken durch Seitenkanalangriffe mit spekulativer Ausführung bietet (insbesondere für Meltdown der Variante 3, [CVE-2017-5754](https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=2017-5754) oder Rogue Data Cache Load). Diese Funktion ist standardmäßig deaktiviert, da sie im aktivierten Zustand die Leistung beeinträchtigen kann. Führen Sie die Anweisungen in [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) aus, um Schutzmaßnahmen auf dem Server zu aktivieren. Wenn Sie Azure Cloud Services ausführen, stellen Sie sicher, dass WA-GUEST-OS-5.15_201801-01 oder WA-GUEST-OS-4.50_201801-01 (verfügbar ab 10. Januar 2018) ausgeführt wird, und aktivieren Sie den Registrierungsschlüssel über eine Startaufgabe.


### <a name="linux"></a>Linux
Wenn Sie Linux verwenden und nicht vertrauenswürdigen Code hosten, müssen Sie auch Linux auf eine neuere Version aktualisieren, die KPTI (Kernel Page-Table Isolation) implementiert. Dadurch werden die Seitentabellen im Kernelspeicher von denen im Benutzerbereich getrennt. Diese Risikominderungen erfordern ein Update des Linux-Betriebssystems und können bei Verfügbarkeit von Ihrem Distributionsanbieter bezogen werden. Der Anbieter Ihres Betriebssystems kann Auskunft darüber geben, ob Schutzmaßnahmen standardmäßig aktiviert oder deaktiviert sind.



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Schützen von Azure-Kunden vor CPU-Sicherheitsrisiken](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
