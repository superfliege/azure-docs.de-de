

## <a name="what-is-happening"></a>Was passiert?

Am [3. Januar](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html) wurde ein branchenweites hardwarebasiertes Sicherheitsrisiko öffentlich gemacht. Die Sicherheit unserer Kunden steht für uns stets an oberster Stelle. Daher ergreifen wir aktiv Maßnahmen, um sicherzustellen, dass kein Azure-Kunde diesen Sicherheitsrisiken ausgesetzt ist.

Mit der Offenlegung der Sicherheitsrisiken haben wir [die geplante Wartungszeit verkürzt](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) und damit begonnen, die VMs, für die das Update weiterhin erforderlich war, automatisch neustarten zu lassen.


## <a name="how-can-i-see-which-of-my-vms-are-already-updated"></a>Wie kann ich erkennen, welche meiner VMs bereits aktualisiert wurden? 

Sie können in der [VM-Liste im Azure-Portal](https://aka.ms/T08tdc) den Status Ihrer VMs anzeigen und sehen, ob der Neustart durchgeführt wurde. Für Ihre VMs ist entweder „Bereits aktualisiert“ angegeben, wenn das Update durchgeführt wurde, oder „Geplant“, falls das Update noch erforderlich ist. Verwenden Sie [Azure Service Health](https://portal.azure.com/), falls Sie nur die VMs mit dem Status „Scheduled“ anzeigen möchten.

## <a name="can-i-find-out-exactly-when-my-vms-will-be-rebooted"></a>Kann ich in Erfahrung bringen, wann genau meine VMs neu gestartet werden?

Die beste Möglichkeit, eine Warnung vor dem Neustart zu erhalten, ist die Konfiguration von [Geplanten Ereignissen](https://docs.microsoft.com/azure/virtual-machines/windows/scheduled-events). Auf diese Weise erhalten Sie 15 Minuten, bevor die VM zu Wartungszwecken heruntergefahren wird, eine Benachrichtigung.

## <a name="can-i-manually-redeploy-now-to-perform-the-required-maintenance"></a>Kann ich nun manuell eine erneute Bereitstellung für die erforderliche Wartung durchführen? 

Wir können nicht garantieren, dass eine erneut bereitgestellte VM einem aktualisierten Host zugeordnet wird. Das Azure-Fabric ordnet VMs nach Möglichkeit bereits aktualisierten Hosts zu. Es kann vorkommen, dass eine VM bei einer erneuten Bereitstellung einem nicht aktualisierten Host zugeordnet wird, sodass ein zweiter Neustart im Rahmen einer geplanten Wartung erzwungen werden müsste. Deshalb wird von manuellen erneuten Bereitstellungen als Problemumgehung abgeraten.

## <a name="how-long-will-the-reboot-take"></a>Wie lange dauert der Neustart? 

Die meisten Neustarts dauern ungefähr **30 Minuten**.

## <a name="does-the-guest-os-need-to-be-updated"></a>Muss das Gast-BS aktualisiert werden? 

Dieses Azure-Infrastrukturupdate behandelt die offengelegten Sicherheitsrisiken auf Hypervisorebene und erfordert kein Update für Windows- oder Linux-VM-Images. Allerdings sollten Sie in Bezug auf Ihre VM-Images weiterhin bewährte Sicherheitsmethoden anwenden. Entsprechende Updates und Anleitungen erhalten Sie ggf. beim Anbieter Ihrer Betriebssysteme. Es wurde eine Anleitung für Windows Server-VM-Kunden veröffentlicht, auf die Sie [hier](../articles/virtual-machines/windows/mitigate-se.md) zugreifen können.

## <a name="will-there-be-a-performance-impact-as-a-result-of-resolving-this-update"></a>Kommt es aufgrund dieses Updates zu Leistungseinbußen?

Bei den meisten Azure-Kunden sollte dieses Update zu keinerlei merklichen Leistungseinbußen führen. Wir haben den CPU- und Datenträger-E/A-Pfad optimiert und können keine merklichen Leistungsbeeinträchtigungen erkennen, nachdem die Fehlerbehebung durchgeführt wurde. Nur bei einigen Kunden kann es ggf. zu geringen Leistungseinbußen kommen. Dies lässt sich durch den beschleunigten Netzwerkbetrieb (Azure Accelerated Networking) ([Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) oder [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)) beheben. Diese Funktion steht allen Azure-Kunden kostenlos zur Verfügung.

## <a name="i-follow-your-recommendations-for-high-availability-will-my-environment-remain-highly-available-during-the-reboot"></a>Ich folge den Empfehlungen im Hinblick auf Hochverfügbarkeit. Ist meine Umgebung beim Neustart weiterhin hochverfügbar?

Ja, in einer Verfügbarkeitsgruppe oder VM-Skalierungsgruppe bereitgestellte virtuelle Computer werden wie Updatedomänenkonstrukte (UD) behandelt. Bei der Wartung berücksichtigt Azure die UD-Einschränkung und startet virtuelle Computer nicht von einer anderen UD (innerhalb derselben Verfügbarkeitsgruppe) aus neu. Weitere Informationen zu Hochverfügbarkeit finden Sie unter [Verwalten der Verfügbarkeit virtueller Windows-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) oder [Verwalten der Verfügbarkeit virtueller Linux-Computer](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

## <a name="i-have-architected-my-business-continuitydisaster-recovery-plan-using-region-pairs-will-reboots-to-my-vms-occur-in-region-pairs-at-the-same-time"></a>Ich habe meinen Plan für die Geschäftskontinuität und Notfallwiederherstellung mit Regionspaaren erstellt. Werden Neustarts meiner VMs für Regionspaare gleichzeitig durchgeführt?

Normalerweise werden Azure-Ereignisse für die geplante Wartung für Regionspaare einzeln durchgeführt, um die Wahrscheinlichkeit zu verringern, dass es in beiden Regionen zu Beeinträchtigungen kommt. Aufgrund der Dringlichkeit dieses Sicherheitsupdates führen wir das Update aber in allen Regionen gleichzeitig durch.

## <a name="what-about-paas-services-on-azure"></a>Welche Rolle spielen PaaS-Dienste unter Azure?  

Die Sicherheitslücke wurde durch Azure-Plattformdienste behoben, z.B. Web + Mobil, Data Services, IoT- und serverlose Dienste. Für Kunden, die diese Dienste verwenden, sind keine Maßnahmen erforderlich.

## <a name="intel-released-additional-guidance-on-january-22-2018-related-to-the-security-vulnerabilities--will-this-guidance-cause-any-additional-maintenance-activities-by-azure"></a>Intel hat am 22. Januar 2018 im Zusammenhang mit den Sicherheitsrisiken weitere Anweisungen veröffentlicht.  Führen diese Anweisungen zu weiteren Wartungsaktivitäten seitens Azure?  

Die zuvor am 3. Januar 2018 angekündigten Azure-Maßnahmen sind von den [aktualisierten Anweisungen](https://newsroom.intel.com/news/root-cause-of-reboot-issue-identified-updated-guidance-for-customers-and-partners/) von Intel nicht betroffen. Diese neuen Informationen ziehen keine zusätzlichen Wartungsaktivitäten für Kunden-VMs nach sich.
 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Schützen von Azure-Kunden vor CPU-Sicherheitsrisiken](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
