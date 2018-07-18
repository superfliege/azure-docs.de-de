---
title: Grundlegendes zum Rabatt für reservierte Azure-Instanzen | Microsoft Docs
description: Hier erfahren Sie, wie der Rabatt für reservierte Azure-VM-Instanzen auf ausgeführte virtuelle Computer angewendet wird.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: yashar
ms.openlocfilehash: d1229a49a5bb3bf3198c91a748ed37b7a626c506
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064320"
---
# <a name="understand-how-the-reserved-instance-discount-is-applied"></a>Grundlegendes zur Anwendung des Rabatts für reservierte Instanzen
Nachdem Sie eine reservierte Azure-VM-Instanz erworben haben, wird der Rabatt für reservierte Instanzen automatisch auf die virtuellen Computer angewendet, die den Attributen und der Menge der reservierten Instanz entsprechen. Eine reservierte Instanz deckt die Infrastrukturkosten Ihrer virtuellen Computer ab. In der folgenden Tabelle sind die Kosten für virtuelle Computer nach dem Kauf einer reservierten Instanz veranschaulicht. In jedem Fall werden Ihnen die regulären Gebühren für Speicher und Netzwerke berechnet.

| Typ des virtuellen Computers  | Gebühren bei einer reservierten Instanz |    
|-----------------------|--------------------------------------------|
|Virtuelle Linux-Computer ohne zusätzliche Software | Die reservierte Instanz deckt die Kosten Ihrer VM-Infrastruktur ab.|
|Virtuelle Linux-Computer mit Softwaregebühren (z.B. Red Hat) | Die reservierte Instanz deckt die Infrastrukturkosten ab. Zusätzliche Software wird in Rechnung gestellt.|
|Virtuelle Windows-Computer ohne zusätzliche Software |Die reservierte Instanz deckt die Infrastrukturkosten ab. Windows-Software wird in Rechnung gestellt.|
|Virtuelle Windows-Computer mit zusätzlicher Software (z.B. SQL Server) | Die reservierte Instanz deckt die Infrastrukturkosten ab. Windows-Software und zusätzliche Software werden in Rechnung gestellt.|
|Virtuelle Windows-Computer mit [Azure-Hybridvorteil](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Die reservierte Instanz deckt die Infrastrukturkosten ab. Die Kosten für die Windows-Software werden durch den Azure-Hybridvorteil abgedeckt. Zusätzliche Software wird separat in Rechnung gestellt.| 

## <a name="application-of-reserved-instance-discount-to-non-windows-vms"></a>Anwendung des Rabatts für reservierte Instanzen auf virtuelle Nicht-Windows-Computer
 Der Rabatt für reservierte Azure-Instanzen wird auf Stundenbasis auf ausgeführte VM-Instanzen angewendet. Die von Ihnen erworbenen reservierten Instanzen werden zur Anwendung des Rabatts für reservierte Instanzen auf die von den ausgeführten virtuellen Computern ausgegebene Nutzung abgestimmt. Bei virtuellen Computern, die keine volle Stunde lang ausgeführt werden, wird die reservierte Instanz von anderen virtuellen Computern aufgefüllt, die keine reservierte Instanz verwenden. Dazu gehören auch gleichzeitig ausgeführte virtuelle Computer. Am Ende der Stunde wird die Anwendung der reservierten Instanz für virtuelle Computer für diese Stunde gesperrt. Wenn ein virtueller Computer nicht eine ganze Stunde lang ausgeführt wird oder gleichzeitige virtuelle Computer diese Stunde der reservierten Instanz nicht füllen, ist die reservierte Instanz für diese Stunde nicht vollständig ausgelastet. Im folgenden Diagramm wird die Anwendung einer reservierten Instanz auf die abrechenbare VM-Nutzung veranschaulicht. Die Abbildung basiert auf dem Kauf von einer reservierten Instanz und zwei entsprechenden VM-Instanzen.

![Screenshot einer angewendeten reservierten Instanzen und zweier entsprechender VM-Instanzen](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1.  Jegliche Nutzung, die über der Grenze für die reservierte Instanz liegt, wird zu den regulären Preisen für nutzungsbasierte Zahlung in Rechnung gestellt. Die Nutzung unterhalb der Grenze für die reservierte Instanz wird nicht in Rechnung gestellt, da diese Nutzung bereits im Rahmen des Kaufs der reservierten Instanz bezahlt wurde.
2.  In der ersten Stunde wird Instanz 1 für 0,75 Stunden und Instanz 2 für 0,5 Stunden ausgeführt. Die Gesamtnutzung für die erste Stunde beträgt 1,25 Stunden. Ihnen wird der Preis für die nutzungsbasierte Zahlung für die verbleibenden 0,25 Stunden in Rechnung gestellt.
3.  In der zweiten und dritten Stunde wurden beide Instanzen jeweils eine Stunde lang ausgeführt. Eine Instanz wird durch die reservierte Instanz abgedeckt, die andere wird zu den Preisen für die nutzungsbasierte Zahlung in Rechnung gestellt.
4.  In der vierten Stunde wird Instanz 1 für 0,5 Stunden und Instanz 2 für 1 Stunde ausgeführt. Instanz 1 wird vollständig durch die reservierte Instanz abgedeckt. Außerdem sind 0,5 Stunden von Instanz 2 abgedeckt. Ihnen wird der Preis für die nutzungsbasierte Zahlung für die verbleibenden 0,5 Stunden in Rechnung gestellt.

Wenn Sie grundlegende Informationen wünschen und die Anwendung Ihrer reservierten Azure-Instanzen in Abrechnungsnutzungsberichten anzeigen möchten, lesen Sie [Grundlegendes zur Nutzung reservierter Instanzen](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reserved-instance-discount-to-windows-vms"></a>Anwendung des Rabatts für reservierte Instanzen auf virtuelle Windows-Computer
Wenn Sie Windows-VM-Instanzen ausführen, wird die reservierte Instanz angewendet, um die Infrastrukturkosten abzudecken. Die Anwendung der reservierten Instanz auf die VM-Infrastrukturkosten ist bei virtuellen Windows-Computern und virtuellen Nicht-Windows-Computern identisch. Windows-Software wird separat basierend auf den vCPUs in Rechnung gestellt. Informationen finden Sie unter [Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten](https://go.microsoft.com/fwlink/?linkid=862756). Sie können Ihre Windows-Lizenzierungskosten mit dem [Azure-Hybridvorteil für Windows Server] (https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) decken.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu reservierten Instanzen finden Sie in den folgenden Artikeln:

- [Was ist Azure Reserved VM Instances?](billing-save-compute-costs-reservations.md)
- [Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Verwalten von reservierten Instanzen in Azure](billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Anwendung des Rabatts für reservierte Instanzen](billing-understand-vm-reservation-charges.md)
- [Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung](billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung reservierter Instanzen für die Enterprise-Registrierung](billing-understand-reserved-instance-usage-ea.md)
- [Grundlegendes zur Nutzung reservierter Instanzen für CSP-Abonnements](https://docs.microsoft.com/partner-center/azure-reservations)
- [Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)


## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
